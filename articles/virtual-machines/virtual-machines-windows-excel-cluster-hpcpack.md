<properties
 pageTitle="Cluster HPC Pack voor Excel en SOA | Microsoft Azure"
 description="Aan de slag grootschalige Excel en SOA werkbelasting uitgevoerd op een HPC Pack cluster in Azure wordt aangegeven"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Aan de slag met Excel en SOA werkbelasting op een HPC Pack cluster in Azure wordt aangegeven

In dit artikel leest u hoe u een Microsoft HPC Pack cluster op Azure virtuele machines met behulp van een sjabloon Azure quickstart of (optioneel) een implementatiescript Azure PowerShell. Azure Marketplace VM afbeeldingen die zijn ontworpen voor Microsoft Excel of service gerichte architectuur (SOA) werkbelasting HPC Pack een cluster wordt gebruikt. U kunt het cluster eenvoudige Excel HPC en SOA services vanaf een lokale clientcomputer uitvoeren. De services Excel HPC opnemen offloading van Excel-werkmap en de gebruiker gedefinieerde functies in Excel of UDF's.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Op hoog niveau ziet in het volgende diagram u het cluster HPC Pack dat u maakt.

![HPC cluster met knooppunten met Excel-werkbelastingen][scenario]

## <a name="prerequisites"></a>Vereisten voor

*   **Clientcomputer** - moet u een Windows-clientcomputer om in te dienen steekproef Excel en SOA taken aan het cluster. Moet u ook een Windows-computer om uit te voeren de implementatiescript Azure PowerShell cluster (als u ervoor deze implementatiemethode kiest) en

*   **Azure abonnement** - als u geen Azure-abonnement hebt, kunt u een [account vrij te geven](https://azure.microsoft.com/pricing/free-trial/) in een paar minuten.

*   **Quotum voor cores** - moet u mogelijk Verhoog het quotum van cores, met name als u meerdere knooppunten met multicore VM grootten implementeert. Als u een sjabloon Azure quickstart gebruikt, is het quotum voor cores in resourcemanager per Azure regio. Mogelijk moet u in dat geval het quotum voor in een specifiek gebied vergroten. Zie [Azure abonnementen, quota, en voorwaarden](../azure-subscription-service-limits.md). Een quotum, [opent u een verzoek voor ondersteuning van online-klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) gratis verhogen.

*   **Microsoft Office-licentie** - als u implementeert berekeningscluster knooppunten gebruik van de afbeelding van een Marketplace HPC Pack VM met Microsoft Excel, een evaluatieversie van 30 dagen van Microsoft Excel Professional Plus 2013 is geïnstalleerd. Achter de punt evaluatie moet u een geldige licentie voor Microsoft Office Excel als u wilt doorgaan om uit te voeren werkbelasting activeren bieden. Zie [Excel activering](#excel-activation) verderop in dit artikel. 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Stap 1. Een HPC Pack cluster in Azure instellen

We twee opties voor het instellen van het cluster weergeven: te beginnen met een sjabloon Azure quickstart en de Azure-portal; en tweede, met een implementatiescript Azure PowerShell.


### <a name="option-1-use-a-quickstart-template"></a>Optie 1. Een sjabloon quickstart gebruiken
Gebruik een Azure quickstart-sjabloon snel en eenvoudig een HPC Pack cluster in de portal van Azure implementeren. Als u de sjabloon in de portal opent, krijgt u een eenvoudige UI waar u de instellingen voor uw cluster invoeren. Dit zijn de stappen. 

>[AZURE.TIP]Desgewenst kunt u een [sjabloon van Azure Marketplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) die Hiermee maakt u een soortgelijke cluster specifiek voor Excel-werkbelastingen gebruiken. De stappen verschillen enigszins van de volgende handelingen uit.

1.  Ga naar de [sjabloonpagina HPC-Cluster maken op GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Als u wilt, raadpleegt u informatie over de sjabloon en de broncode.

2.  Klik op **Deploy naar Azure** als u wilt een implementatie starten met de sjabloon in de portal van Azure.

    ![Sjabloon implementeren naar Azure][github]

3.  Volg deze stappen voor het invoeren van de parameters voor de HPC cluster sjabloon in de portal.

    een. Klik op de pagina **Parameters** invoeren of wijzigen van de waarden voor de sjabloonparameters. (Klik op het pictogram naast elk van de instellingen voor help-informatie.) Voorbeeldwaarden worden weergegeven in het volgende scherm. In dit voorbeeld wordt een cluster met de naam *hpc01* in de *hpc.local* domein bestaande uit een hoofd knooppunt gemaakt en 2 knooppunten berekenen. De knooppunten berekeningscluster worden gemaakt van een afbeelding HPC Pack VM met Microsoft Excel.

    ![Parameters invoeren][parameters]

    >[AZURE.NOTE]Het hoofd knooppunt VM wordt automatisch uit de [meest recente Marketplace-afbeelding](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) van HPC Pack 2012 R2 op Windows Server 2012 R2 gemaakt. De afbeelding is momenteel gebaseerd op HPC Pack 2012 R2 Update 3.
    >
    >Berekent het dat knooppunt VMs zijn gemaakt op basis van de meest recente afbeelding van de geselecteerde berekeningscluster knooppunt familie. Selecteer de optie **ComputeNodeWithExcel** voor de meest recente HPC Pack berekeningscluster knooppunt afbeelding met een evaluatieversie van Microsoft Excel Professional Plus 2013. Als u wilt een cluster implementeren voor algemene SOA-sessies of voor Excel UDF offloading, kiest u de optie **ComputeNode** (zonder Excel is geïnstalleerd).

    b. Kies het abonnement.

    c. Een resourcegroep voor het cluster, zoals *hpc01RG*maken.

    d. Kies een locatie voor de resourcegroep, zoals Central US.

    e. Lees de voorwaarden op de pagina **juridische voorwaarden** . Als u akkoord gaat, klikt u op **aanschaffen**. Wanneer u klaar bent met de waarden voor de sjabloon instellen, klik vervolgens op **maken**.

4.  Wanneer de implementatie is voltooid (het duurt meestal ongeveer 30 minuten), het cluster certificaat-bestand uit het hoofd knooppunt exporteren. In een latere stap importeert u dit openbare certificaat op de clientcomputer voor de verificatie van de servers voor secure HTTP binding.

    een. Verbinding maken met het hoofd knooppunt door extern bureaublad van de Azure-portal.

     ![Verbinding maken met het hoofd knooppunt][connect]

    b. Standaardprocedures in Certificaatbeheer exporteren van het certificaat hoofd knooppunt (bevindt zich onder certificaat: \LocalMachine\My) zonder de persoonlijke sleutel gebruiken. In dit voorbeeld exporteren *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Het certificaat exporteren][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Optie 2. Gebruik het HPC Pack IaaS implementatie-script

De implementatiescript HPC Pack IaaS biedt een andere veelzijdige manier om een cluster HPC Pack implementeren. Er wordt een cluster gemaakt in het implementatiemodel klassieke, dat de sjabloon wordt gebruikt voor het implementatiemodel resourcemanager Azure. Het script is ook compatibel met een abonnement in de globale Azure of China Azure-service.

**Aanvullende vereisten**

* **Azure PowerShell** - [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) (versie 0.8.10 of hoger) op de clientcomputer.

* **HPC Pack IaaS implementatiescript** - downloaden en uitpakken van de nieuwste versie van het script in het [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=44949). Controleren welke versie van het script door te voeren `New-HPCIaaSCluster.ps1 –Version`. In dit artikel is gebaseerd op versie 4.5.0 of hoger van het script.

**Het configuratiebestand maken**

 De implementatiescript HPC Pack IaaS wordt een XML-configuratiebestand gebruikt als invoer waarmee de infrastructuur van het cluster HPC beschreven. Als u wilt implementeren een cluster bestaande uit een hoofd knooppunt en 18 berekeningscluster knooppunten gemaakt op basis van de afbeelding van berekeningscluster knooppunt dat Microsoft Excel bevat, vervangt door de waarden voor uw omgeving in het volgende voorbeeld-configuratiebestand. Zie voor meer informatie over het configuratiebestand, het bestand Manual.rtf in de scriptmap en [een HPC cluster met het implementatiescript HPC Pack IaaS maken](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Opmerkingen over het configuratiebestand**

* De **VMName** van het hoofd knooppunt **moet** hetzelfde zijn als de **servicenaam**of SOA-taken niet uitgevoerd.

* Zorg ervoor dat u **EnableWebPortal** opgeven, zodat het certificaat hoofd knooppunt is gegenereerd en geëxporteerd.

* Een post-configuratietaken PowerShell-script PostConfig.ps1 die wordt uitgevoerd op het hoofd knooppunt Hiermee geeft u het bestand. Het volgende voorbeeldscript de verbindingsreeks van Azure opslag configureert, verwijdert de rol van berekeningscluster knooppunt uit het hoofd knooppunt en levert met alle knooppunten online wanneer ze worden gebruikt. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Het script uitvoeren**

1.  Open de PowerShell-console op de clientcomputer als beheerder.

2.  De map wijzigen naar de scriptmap (E:\IaaSClusterScript in dit voorbeeld).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Als u wilt het cluster HPC Pack implementeert, kunt u de volgende opdracht uitvoeren. In dit voorbeeld wordt ervan uitgegaan dat de configuratiebestand in E:\HPCDemoConfig.xml bevindt zich.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

De implementatiescript HPC Pack kan enige tijd worden uitgevoerd. Wat het script is is wilt exporteren en het certificaat cluster downloaden en sla deze in de map documenten van de huidige gebruiker op de clientcomputer. Het script genereert een de volgende strekking weergegeven. In een volgende stap importeert u het certificaat in het juiste certificaatarchief.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Stap 2. Excel-werkmappen offload en UDF's uitvoeren vanuit een on-premises implementatie-client

### <a name="excel-activation"></a>Activering van Excel

Wanneer u de afbeelding ComputeNodeWithExcel VM voor productie-werkbelastingen, moet u een geldige Microsoft Office-licentie productcode om te activeren van Excel op de knooppunten berekeningscluster bieden. Anders de evaluatieversie van Excel verloopt na 30 dagen en Excel-werkmappen uitgevoerd, mislukt met de COMException (0x800AC472). 

Opnieuw Excel kunt u een andere 30 dagen van de evaluatietijd: Meld u aan bij het hoofd knooppunt en clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` berekenen op alle Excel knooppunten via HPC Cluster Manager. U opnieuw kunt maximaal twee keer. Daarna moet u een geldige licentie Office-productcode opgeven.

Office Professional Plus 2013 hebben geïnstalleerd op de afbeelding VM is een volume-editie met een algemene Volume licentie sleutel (GVLK). Kan worden geactiveerd via KMS Key Management Service () / Active Directory-Based activering (AD-BA) of toets MAK (Multiple Activation). 

    * Als u wilt gebruiken KMS/AD-BA, een bestaande KMS-server gebruiken of een nieuwe record met behulp van het Microsoft Office 2013 Volume licentie Pack instelt. (Als u wilt de server instellen op het hoofd knooppunt.) Kies vervolgens de KMS-host-code via Internet of telefoon activeren. Vervolgens clusrun `ospp.vbs` in te stellen de KMS-server en poort Office activeren op alle de Excel knooppunten berekenen. 
    
    * Gebruik van MAK, eerste clusrun `ospp.vbs` de sleutel kunnen invoeren en vervolgens activeren alle de Excel knooppunten via Internet of telefoon berekenen. 

>[AZURE.NOTE]Detailhandel-productcodes voor Office Professional Plus 2013 kunnen niet worden gebruikt met deze VM-afbeelding. Als u geldige sleutels en installatiemedium voor Office of Excel-versies dan deze Office Professional Plus 2013 volume-editie hebt, kunt u ze in plaats daarvan gebruiken. Eerst dit volume-editie verwijderen en installeer de versie die u hebt. Het opnieuw geïnstalleerde Excel berekeningscluster knooppunt kan worden vastgelegd als de afbeelding van een aangepaste VM gebruiken in een implementatie bij het op schaal.

### <a name="offload-excel-workbooks"></a>Excel-werkmappen offload

Volg deze stappen om een Excel-werkmap offload zodat deze wordt uitgevoerd op het cluster HPC Pack in Azure wordt aangegeven. Hiervoor moet u Excel 2010 of 2013 al geïnstalleerd op de clientcomputer.

1. Gebruik een van de opties in stap 1 om te implementeren van een HPC Pack cluster met het Excel berekenen knooppunt afbeelding. Aanvragen cluster-certificaatbestand (.cer) en cluster gebruikersnaam en wachtwoord.

2. Importeer het certificaat cluster onder certificaat: \CurrentUser\Root op de clientcomputer.

3. Zorg ervoor dat Excel is geïnstalleerd. Maak een Excel.exe.config-bestand met de volgende inhoud in dezelfde map als Excel.exe op de clientcomputer. Dit zorgt ervoor dat de HPC Pack 2012 R2 Excel COM-invoegtoepassing geladen.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  De client taken kunnen verzenden naar het cluster HPC Pack ingesteld. Eén optie is de volledige [installatie HPC Pack 2012 R2 Update 3](http://www.microsoft.com/download/details.aspx?id=49922) downloaden en installeren van de client HPC Pack. U kunt ook download en installeer de [hulpprogramma's HPC Pack 2012 R2 Update 3-client](https://www.microsoft.com/download/details.aspx?id=49923) en de juiste visuele C++ 2010 distribueren pakket voor uw computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  In dit voorbeeld gebruiken we een steekproef Excel-werkmap met de naam ConvertiblePricing_Complete.xlsb. U kunt deze downloaden [hier](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  De Excel-werkmap naar een werkmap zoals D:\Excel\Run kopiëren.

7.  Open de Excel-werkmap. Klik op het lint **ontwikkelen** klikt u op **COM-invoegtoepassingen** en Bevestig dat de HPC Pack Excel COM-invoegtoepassing is geladen.

    ![Excel-invoegtoepassing voor HPC Pack][addin]

8.  De VBA-macro HPCControlMacros in Excel door te wijzigen van de regels met opmerkingen, zoals wordt weergegeven in het volgende script bewerken. Vervangt door passende waarden voor uw omgeving.

    ![Excel-macro voor HPC Pack][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  Kopieer de Excel-werkmap naar een Uploadmap zoals D:\Excel\Upload. Deze map is opgegeven in de constante HPC_DependsFiles in de VBA-macro.

10. De werkmap op het cluster in Azure uitgevoerd, klikt u op de knop **Cluster** op het werkblad.

### <a name="run-excel-udfs"></a>Excel UDF's uitvoeren

Als u wilt uitvoeren in Excel UDF's, de voorgaande stappen 1 en 3 voor het instellen van de clientcomputer. Voor Excel UDF's moet u niet de Excel-hebt geïnstalleerd op berekeningscluster knooppunten. Ja, wanneer u uw cluster maakt berekenen knooppunten, kunt u een gewone berekenen knooppunt afbeelding in plaats van de afbeelding van het knooppunt berekenen met Excel.

>[AZURE.NOTE] Er is een 34 tekenlimiet in de Excel 2010 en 2013 cluster verbindingslijn dialoogvenster. Dit dialoogvenster kunt u opgeven van het cluster die wordt uitgevoerd de UDF's. Als de naam van de volledige cluster langer is (bijvoorbeeld hpcexcelhn01.southeastasia.cloudapp.azure.com), het past niet in het dialoogvenster. De tijdelijke oplossing er is een variabele alle computers zoals *CCP_IAASHN* met de waarde van de naam van de lange cluster instellen. Voer vervolgens *% CCP_IAASHN %* in het dialoogvenster als de naam van het cluster hoofd knooppunt. 

Nadat het cluster met succes wordt geïmplementeerd, gaat u verder met de volgende stappen voor het uitvoeren van een steekproef ingebouwde Excel UDF. Zie voor aangepaste UDF's van Excel, deze [resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) voor de XLL's bouwen en implementeren ze op het cluster IaaS.

1.  Open een nieuw Excel-werkmap. Klik op **Invoegtoepassingen**op het lint **ontwikkelen** . Klik in het dialoogvenster, klikt u op **Bladeren**, Ga naar de map %CCP_HOME%Bin\XLL32 en selecteer de steekproef ClusterUDF32.xll. Als de ClusterUDF32 niet op de clientcomputer bestaat, kunt u deze uit de map %CCP_HOME%Bin\XLL32 op het hoofd knooppunt kopiëren.

    ![Selecteer de UDF][udf]

2.  Klik op **bestand** > **Opties** > **Geavanceerde**. Schakel onder **formules**, het selectievakje **toestaan gebruiker gedefinieerde functies om uit te voeren een berekeningscluster XLL**. Vervolgens klikt u op **Opties** en voer de clusternaam van de volledige in **de naam van de kop knooppunt Cluster**. (Zoals aangegeven eerder dit invoervak is beperkt tot 34 tekens op, zodat de naam van een lang cluster mogelijk niet passen. U kunt een variabele van alle computers hier voor de naam van een lang cluster.)

    ![De UDF configureren][options]

3.  De berekening UDF op het cluster uitgevoerd, klikt u op de cel met de waarde =XllGetComputerNameC() en druk op Enter. De functie haalt gewoon de naam van het berekeningscluster knooppunt waarvoor de UDF wordt uitgevoerd. Voor het eerst wordt uitgevoerd, wordt het dialoogvenster referenties gevraagd voor de gebruikersnaam en wachtwoord verbinding maken met de cluster IaaS.

    ![UDF uitvoeren][run]

    Wanneer er veel cellen om te berekenen, drukt u op Alt-Shift-Ctrl + F9 om de berekening uitgevoerd op alle cellen.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Stap 3. Een werkbelasting SOA uitvoeren vanuit een on-premises implementatie-client

Algemene SOA-toepassingen op het cluster HPC Pack IaaS uitgevoerd, eerst via een van de methoden in stap 1 het cluster implementeren. Geef een algemene afbeelding van het knooppunt in dit geval berekenen omdat u niet Excel op de knooppunten berekeningscluster hoeft. Daarna volgt u deze stappen.

1. Na het certificaat cluster ophalen, op de clientcomputer onder certificaat: \CurrentUser\Root importeren.

2. Installeer de [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) en [hulpprogramma's HPC Pack 2012 R2 Update 3-client](https://www.microsoft.com/download/details.aspx?id=49923). Deze hulpprogramma's kunnen u ontwikkelen en SOA-clienttoepassingen uitvoeren.

3. Download de HelloWorldR2- [code van de steekproef](https://www.microsoft.com/download/details.aspx?id=41633). Open de HelloWorldR2.sln in Visual Studio 2010 of 2012.

4. Het project EchoService eerst maken. De service aan het cluster IaaS vervolgens implementeren op dezelfde manier die u dashboard naar een cluster on-premises implementatie implementeren. Zie de Readme.doc in HelloWordR2 voor gedetailleerde stappen. De HellWorldR2 en andere projecten zoals is beschreven in de volgende sectie om te genereren van de SOA-clienttoepassingen die worden uitgevoerd op een cluster Azure IaaS maken en wijzigen.

### <a name="use-http-binding-with-azure-storage-queue"></a>Http-binding gebruiken met Azure opslag wachtrij

Als u wilt gebruiken Http binding met een wachtrij Azure opslag, kunt u een paar wijzigingen aanbrengen door de voorbeeldcode.

* Naam van het cluster bijwerken.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* (Optioneel) Gebruik de standaardwaarde TransportScheme in SessionStartInfo of stel deze expliciet in op Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Standaard binding voor de BrokerClient gebruiken.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Of set expliciet met de basicHttpBinding.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* U kunt ook de vlag UseAzureQueue ingesteld op waar in SessionStartInfo. Als niet is ingesteld, zal worden ingesteld op waar al dan niet standaard wanneer de naam van de cluster Azure domeinachtervoegsels bevat, waardoor de TransportScheme Http is.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>Http-binding zonder Azure opslag wachtrij gebruiken

Als u wilt gebruiken Http binding zonder een wachtrij Azure opslag, moet u expliciet de vlag UseAzureQueue ingesteld op ONWAAR in de SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Gebruik NetTcp-binding

Als u wilt gebruiken NetTcp-binding, zijn de configuratie is vergelijkbaar met het verbinding maken met een on-premises implementatie cluster. U moet een paar eindpunten op het hoofd knooppunt VM openen. Als u de implementatiescript HPC Pack IaaS gebruikt bij het maken van het cluster, bijvoorbeeld de eindpunten in de portal van Azure klassieke als volgt instellen.


1. De VM stoppen.

2. Toevoegen de TCP-poorten 9090, 9087, 9091, 9094 voor de sessie Broker, werknemer en gegevensservices, respectievelijk Broker

    ![Eindpunten configureren][endpoint]

3. Start de VM.

De clienttoepassing SOA vereist geen wijzigingen, behalve de naam van de kop naar de volledige naam van het IaaS cluster wijzigen.

## <a name="next-steps"></a>Volgende stappen

* Zie de [volgende informatiebronnen](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) voor meer informatie over het uitvoeren van Excel werkbelasting met HPC Pack.

* Zie [SOA-Services beheren in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) voor meer informatie over de implementatie en het beheer van SOA-services met HPC Pack.

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png
