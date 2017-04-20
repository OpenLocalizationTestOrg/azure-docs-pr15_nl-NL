<properties
    pageTitle="Actie ontwikkeling met HDInsight script | Microsoft Azure"
    description="Informatie over het aanpassen van Hadoop clusters met scriptactie. Scriptactie kan worden gebruikt voor het installeren van extra software op een cluster Hadoop of de configuratie van toepassingen zijn geïnstalleerd op een cluster wijzigen."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Scriptactie scripts voor HDInsight Windows gebaseerde clusters ontwikkelen

Leer hoe u het scriptactie scripts schrijven voor HDInsight. Zie voor informatie over het gebruik van de actie Script scripts [aanpassen HDInsight clusters met de Script-actie](hdinsight-hadoop-customize-cluster.md). Zie voor hetzelfde artikel geschreven voor HDInsight Linux gebaseerde clusters, [ontwikkelen scriptactie scripts voor HDInsight](hdinsight-hadoop-script-actions-linux.md).

> [AZURE.NOTE] De informatie in dit document hoort bij op basis van Windows HDInsight clusters. Zie voor informatie over het gebruik van scriptacties met clusters op basis van Windows [Script actie ontwikkeling met HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).


Scriptactie kan worden gebruikt voor het installeren van extra software op een cluster Hadoop of de configuratie van toepassingen zijn geïnstalleerd op een cluster wijzigen. Scriptacties zijn scripts waarop knooppunten wordt uitgevoerd wanneer HDInsight clusters zijn geïmplementeerd en worden uitgevoerd nadat knooppunten in het cluster HDInsight configuratie voltooid. Een scriptactie wordt uitgevoerd onder Systeem account beheerdersbevoegdheden en biedt volledige toegangsrechten voor de knooppunten. Elk cluster kan worden geleverd met een lijst met scriptacties worden uitgevoerd in de volgorde waarin ze zijn opgegeven.

> [AZURE.NOTE] Als u het volgende foutbericht weergegeven ondervindt:
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> Komt dat doordat u niet de methoden hebt opgenomen.  Zie [methoden voor aangepaste scripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).

## <a name="sample-scripts"></a>Voorbeeldscripts

Voor het maken van HDInsight clusters op Windows-besturingssysteem, wordt de actie Script Azure PowerShell-script. Hieronder ziet u een steekproef script voor de bestanden van de configuratie van de site configureren:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Het script gaat vier parameters, de bestandsnaam van de configuratie, de eigenschap die u wijzigen wilt, de waarde die u instellen wilt, en een beschrijving. Bijvoorbeeld:

    hive-site.xml hive.metastore.client.socket.timeout 90

Deze parameters wordt de waarde hive.metastore.client.socket.timeout ingesteld op 90 in het onderdeel-site.xml-bestand.  De standaardwaarde is 60 seconden.

Voorbeeldscript kan ook worden gevonden op [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

HDInsight bevat verschillende scripts om extra onderdelen op HDInsight clusters installeren:

Naam | Script
----- | -----
**Elektrische installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Zie [installeren en gebruiken, dus op HDInsight clusters][hdinsight-install-spark].
**R installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Zie [installeren en gebruiken R op HDInsight clusters][hdinsight-r-scripts].
**Solr installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Zie [installeren en gebruiken Solr op HDInsight](hdinsight-hadoop-solr-install.md).
- **Giraph installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Zie [installeren en gebruiken Giraph op HDInsight](hdinsight-hadoop-giraph-install.md).

Scriptactie kan worden geïmplementeerd vanaf de Azure Azure PowerShell-portal of met behulp van de HDInsight .NET SDK.  Zie voor meer informatie, [HDInsight aanpassen clusters met de actie Script][hdinsight-cluster-customize].

> [AZURE.NOTE] De voorbeeldscripts werken alleen met HDInsight cluster versie 3.1 of hoger. Zie voor meer informatie over HDInsight cluster versies, [HDInsight cluster versies](hdinsight-component-versioning.md).





## <a name="helper-methods-for-custom-scripts"></a>Methoden voor aangepaste scripts

Script actie methoden zijn hulpprogramma's die u gebruiken kunt bij het schrijven van aangepaste scripts. Deze zijn gedefinieerd in de [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)en kunnen worden opgenomen in uw scripts met het volgende:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Hier volgen de methoden die worden verstrekt door dit script:

Helpmethode | Beschrijving
-------------- | -----------
**Opslaan-HDIFile** | Een bestand uit de opgegeven id URI (Uniform Resource) naar een locatie op de lokale schijf die is gekoppeld aan het knooppunt van de Azure VM is toegewezen aan de cluster downloaden.
**Vouw HDIZippedFile** | Pak een ZIP-bestand.
**Roepen HDICmdScript** | Een script uitvoeren vanaf cmd.exe.
**Schrijven-HDILog** | Uitvoer van de aangepast script gebruikt voor de scriptactie voor een schrijven.
**Get-Services** | Haal een lijst met services worden uitgevoerd op de computer waarop het script wordt uitgevoerd.
**Get-Service** | Met de specifieke servicenaam als invoer, krijgt u gedetailleerde informatie voor een specifieke service (servicenaam, proces-ID, status, enzovoort) op de computer waarop het script wordt uitgevoerd.
**Get-HDIServices** | Haal een lijst met HDInsight services worden uitgevoerd op de computer waarop het script wordt uitgevoerd.
**Get-HDIService** | Met de specifieke HDInsight servicenaam als invoer, krijgt u gedetailleerde informatie voor een specifieke service (servicenaam, proces-ID, status, enzovoort) op de computer waarop het script wordt uitgevoerd.
**Get-ServicesRunning** | Haal een lijst met services die worden uitgevoerd op de computer waarop het script wordt uitgevoerd.
**Get-ServiceRunning** | Controleer of een specifieke service (op naam) is gestart op de computer waarop het script wordt uitgevoerd.
**Get-HDIServicesRunning** | Haal een lijst met HDInsight services worden uitgevoerd op de computer waarop het script wordt uitgevoerd.
**Get-HDIServiceRunning** | Controleer of een specifieke HDInsight-service (op naam) is gestart op de computer waarop het script wordt uitgevoerd.
**Get-HDIHadoopVersion** | Krijgen de versie van Hadoop geïnstalleerd op de computer waarop het script wordt uitgevoerd.
**Test-IsHDIHeadNode** | Controleer of de computer waarop het script wordt uitgevoerd hoofd knooppunt is.
**Test-IsActiveHDIHeadNode** | Controleer of de computer waarop het script wordt uitgevoerd een actief hoofd knooppunt is.
**Test-IsHDIDataNode** | Controleer of de computer waarop het script wordt uitgevoerd gegevensknooppunt is.
**Bewerken-HDIConfigFile** | Bewerk de zoekconfiguratie bestanden component-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml of garens-site.xml.


## <a name="best-practices-for-script-development"></a>Aanbevolen procedures voor het ontwikkelen van scripts

Wanneer u een aangepast script voor een cluster HDInsight ontwikkelt, zijn er enkele aanbevolen procedures in gedachten moet houden:

- Controleren op de Hadoop-versie

    Alleen HDInsight versie 3.1 (Hadoop 2,4) en hoger ondersteuning voor aangepaste onderdelen installeren op een cluster met scriptactie. In uw aangepast script, moet u de **Get-HDIHadoopVersion** helpmethode gebruiken om te controleren welke versie Hadoop voordat u verder gaat met het uitvoeren van andere taken in het script.


- Bieden stabiele koppelingen naar script resources

    Gebruikers Zorg ervoor dat alle de scripts en andere onderdelen die wordt gebruikt in de aanpassing van een cluster beschikbaar gedurende de levensduur van het cluster blijven en de versies van deze bestanden beter niet wijzigen voor de duur. Deze resources zijn vereist als de opnieuw imaging van knooppunten in het cluster vereist is. De beste manier is om te downloaden en alles in een opslag-account dat de gebruiker bepaalt gearchiveerd. Dit is het standaardaccount voor de opslag of een van de extra opslagruimte-accounts op het moment van implementatie voor een aangepaste cluster opgegeven.
    In de elektrische en R aangepast cluster voorbeelden voorwaarde in de documentatie, bijvoorbeeld we een lokale kopie van de bronnen voor dit account opslag aangebracht hebben: https://hdiconfigactions.blob.core.windows.net/.


- Ervoor zorgen dat het cluster aanpassing script idempotency is ingeschakeld is

    U moet verwacht dat de knooppunten van een cluster HDInsight tijdens de levensduur cluster opnieuw afbeelding zal zijn. Het script van de aanpassing cluster wordt uitgevoerd wanneer een cluster opnieuw afbeelding is. Dit script moet zijn ontworpen om te worden idempotency is ingeschakeld in de zin dat na het opnieuw imaging, het script ervoor zorgen moet dat het cluster wordt geretourneerd naar hetzelfde aangepast status waarin deze verkeerde in vlak achter het script is uitgevoerd voor de eerste keer waarop het cluster in eerste instantie is gemaakt. Bijvoorbeeld als een aangepast script een toepassing op D:\AppLocation geïnstalleerd op de eerste uitvoeren en klik op elke volgende uitvoeren, na het opnieuw imaging, het script moet controleren of de toepassing aanwezig is op de locatie D:\AppLocation voordat u verdergaat met de overige stappen voor het script.


- Aangepaste onderdelen installeren op de optimale locatie

    Wanneer knooppunten opnieuw afbeelding, de C:\ resource station en D:\ systeemstation kunnen worden opnieuw opgemaakte, wat resulteert in de verlies van gegevens en toepassingen die op deze stations was geïnstalleerd. Dit kan ook gebeuren als een knooppunt Azure virtuele machine (VM) die deel uitmaakt van het cluster uitvalt en wordt vervangen door een nieuw knooppunt. U kunt onderdelen installeren op de schijf D:\ of op de locatie C:\apps op het cluster. Alle andere locaties op de schijf C:\ worden gereserveerd. Geef de locatie waar toepassingen of bibliotheken zijn in het cluster aanpassing script zijn geïnstalleerd.


- Betere beschikbaarheid van de architectuur cluster zorgen

    HDInsight heeft een actieve-passieve architectuur voor hoge beschikbaarheid, waarin een hoofd knooppunt in actieve modus is (waar de HDInsight-services worden uitgevoerd) en de andere hoofd knooppunt in de wachtstand is (in welke HDInsight services niet worden uitgevoerd). De knooppunten veranderen actieve en passieve modi als HDInsight services worden gestoord. Als de scriptactie voor een wordt gebruikt voor het installeren van services op beide hoofd knooppunten beschikbaarheid, houd er rekening mee dat het HDInsight failover om niet kunnen worden automatisch uitgevoerd deze gebruiker geïnstalleerde services. Dus door de gebruiker geïnstalleerde services op HDInsight hoofd knooppunten die naar verwachting wordt ten zeerste beschikbaar moeten hun eigen om failover als u in de actieve-passieve modus hebt of zich in de actieve modus.

    Een opdracht HDInsight scriptactie wordt uitgevoerd op beide hoofd knooppunten wanneer de rol van hoofd-knooppunt is opgegeven als een waarde in de parameter *ClusterRoleCollection* . Wanneer u een aangepast script ontwerpt, Controleer dus ervoor dat uw script op de hoogte van deze instelling is. Problemen waar de dezelfde services zijn geïnstalleerd en gestart op beide van de kop knooppunten en ze met elkaar dat bij het moet u niet uitvoeren. Let op dat gegevens verloren die tijdens het opnieuw imaging zijn, zodat de software hebt geïnstalleerd via een scriptactie moet worden robuuste op gebeurtenissen. Toepassingen moeten zijn ontworpen voor gebruik met zeer beschikbare gegevens die is verdeeld over zoveel knooppunten. Houd er rekening mee dat maximaal 1/5 van de knooppunten in een cluster opnieuw kunt replicatie op hetzelfde moment.


- De aangepaste onderdelen als u wilt gebruiken van Azure-blobopslag configureren

    De aangepaste onderdelen die u op knooppunten installeert mogelijk een standaard-configuratie Hadoop Distributed bestand System (HDFS) opslag gebruiken. U moet de configuratie Azure-blobopslag in plaats daarvan wijzigen. Klik op een cluster opnieuw afbeelding, het bestandssysteem HDFS wordt opgemaakt en verliest u gegevens die er is opgeslagen. Azure-blobopslag in plaats daarvan zorgt ervoor dat uw gegevens blijven behouden.

## <a name="common-usage-patterns"></a>Algemene gebruikspatronen

In deze sectie bevat richtlijnen over het implementeren van enkele van de algemene gebruikspatronen die mogelijk optreden kunnen bij het schrijven van uw eigen aangepast script.

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

In de script actie ontwikkelingsfase bevindt, wordt u vaak dat u voor het instellen van de omgevingsvariabelen erop. Een meest waarschijnlijke geval is, bijvoorbeeld wanneer u een binair getal uit een externe site downloaden, op het cluster installeren en voeg de locatie van het waarop deze aan uw omgevingsvariabele 'Pad' is geïnstalleerd. Het codefragment van de volgende ziet u hoe u de omgevingsvariabelen in de aangepast script instellen.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Deze instructie de omgevingsvariabele **MDS_RUNNER_CUSTOM_CLUSTER** wordt ingesteld als de waarde 'waar' en stelt ook het bereik van deze variabele moeten alle computers. Soms is het belangrijk dat omgevingsvariabelen op het gewenste bereik – computer of gebruiker zijn ingesteld. Raadpleeg [hier] [ 1] voor meer informatie over het instellen van de omgevingsvariabelen.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Toegang tot locaties waar de aangepaste scripts worden opgeslagen

Scripts die worden gebruikt voor het aanpassen van een cluster moeten naar een zich in het standaardaccount voor de opslag voor het cluster of in een openbare container met alleen-lezen op een andere opslag-account. Als uw script toegang heeft tot bronnen elders deze moeten worden in een openbaar (ten minste openbare alleen-lezen). U wilt bijvoorbeeld een bestand openen en opslaan met de opdracht SaveFile-HDI.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

In dit voorbeeld, moet u ervoor zorgen dat de container 'somecontainer' in de opslagruimte account 'somestorageaccount' openbaar toegankelijk is. Het script kunnen anders wordt een uitzondering 'Niet gevonden' en mislukt.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Parameters voor de cmdlet toevoegen-AzureRmHDInsightScriptAction doorgeven

Als u wilt meerdere parameters doorgeven aan de cmdlet toevoegen-AzureRmHDInsightScriptAction, moet u de tekenreekswaarde aanduiding van alle parameters voor het script opmaken. Bijvoorbeeld:

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

of

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Uitzondering voor mislukte cluster-implementatie

Als u nauwkeurig een melding ontvangen wilt van het feit dat cluster aanpassing kan niet zoals verwacht, is het belangrijk om een uitzondering en het maken van het cluster mislukt. U wilt bijvoorbeeld een bestand verwerkt als deze bestaat en afhandelen van de fout hoofdletters/kleine letters waar het bestand niet bestaat. Dit zou ervoor zorgen dat het script zonder problemen afgesloten en de status van de cluster correct bekend is. Het codefragment van de volgende geeft een voorbeeld van hoe u dit:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

In dit fragment, als het bestand niet bestaat, leiden deze naar een status waarin het script daadwerkelijk afgesloten zonder problemen na het afdrukken van het foutbericht wordt weergegeven, en het cluster actief, ervan uitgaande dat dit "" voltooid cluster aanpassing bereikt. Als u wilt geïnformeerd overeenkomt met het feit dat aanpassing in principe cluster kan niet zoals verwacht vanwege een ontbrekende bestand, kunt u meer geschikt zijn voor een uitzondering en de stap van de aanpassing cluster mislukt. Hiervoor moet u het volgende voorbeeld-codefragment in plaats daarvan gebruiken.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Controlelijst voor het implementeren van de scriptactie voor een
Hier volgen de stappen die we hebt gemaakt, bij het implementeren van deze scripts voorbereiden:

1. Zet de bestanden met de aangepaste scripts in een locatie die toegankelijk is voor de knooppunten tijdens de implementatie. Dit is een van de standaard- of extra opslagruimte accounts op het moment van cluster implementatie of een andere openbaar opslag container opgegeven.
2. Controles in scripts om ervoor te zorgen dat ze idempotently uitvoeren, zodat het script kan meerdere keren worden uitgevoerd op hetzelfde knooppunt toevoegen.
3. Gebruik de **Schrijven-uitvoer** Azure PowerShell-cmdlet af te drukken STDOUT, evenals STDERR. Gebruik geen **Write-Host**.
4. Gebruik een tijdelijk bestand-map, zoals $env: TEMP, om het gedownloade bestand die worden gebruikt door de scripts en klik vervolgens opschonen van nadat scripts hebt uitgevoerd.
5. Aangepaste software alleen op D:\ of C:\apps installeren. Andere locaties op station C: mag niet worden gebruikt als ze gereserveerd worden. Houd er rekening mee dat installeren bestanden op het station C: buiten de map C:\apps leiden setup-fouten tijdens het opnieuw afbeeldingen van het knooppunt tot kan.
6. In het geval dat OS niveau instellingen of Hadoop-service configuratiebestanden zijn gewijzigd, wilt u mogelijk opnieuw opstarten HDInsight services zodat ze een OS niveau-instellingen, zoals de omgevingsvariabelen instellen in de scripts kunnen verdergaan.

## <a name="debug-custom-scripts"></a>Fouten opsporen in aangepaste scripts

De script-foutenlogboeken zijn opgeslagen, samen met andere uitvoer, in het standaardaccount voor opslag die u hebt opgegeven voor het cluster bij het is gemaakt. De logboeken zijn opgeslagen in een tabel met de naam *u < \cluster-name-fragment >< \time-stamp > bestand*. Hierna ziet u geaggregeerde logboeken met records uit alle knooppunten (hoofd knooppunt en werknemer knooppunten) waarop het script in het cluster wordt uitgevoerd.
Een eenvoudige manier om te controleren van de logboeken is via HDInsight Tools voor Visual Studio. Zie [aan de slag met Hadoop voor Visual Studio tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio) voor het installeren van de hulpmiddelen voor

**Het gebruik van Visual Studio doorzoeken**

1. Open Visual Studio.
2. Klik op **Beeld**en klik vervolgens op **Server Verkenner**.
3. Met de rechtermuisknop op 'Azure', klik op verbinding maken met **Microsoft Azure-abonnementen**en voer uw referenties.
4. Vouw van **opslag**, het Azure opslag-account gebruikt als het standaardbestandssysteem uitvouwen, uit **tabellen**en dubbelklik vervolgens op de naam van de tabel.


U kunt ook externe in knooppunten om te zien van de beide STDOUT en STDERR voor aangepaste scripts. De logboeken op elk knooppunt gelden alleen voor dat knooppunt en bent aangemeld bij **C:\HDInsightLogs\DeploymentAgent.log**. Deze logboekbestanden record alle uitvoer van de aangepast script. Een voorbeeld log fragment voor een actie in een script ziet er zo uit:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


In dit logboek is duidelijk dat de actie een script op de naam van HEADNODE0 VM is uitgevoerd en dat er geen uitzonderingen tijdens de uitvoering veroorzaakt.

In het geval dat een uitvoering van een fout optreedt, wordt de uitvoer met een beschrijving van dit ook worden opgenomen in dit bestand. De informatie in deze logboeken moet nuttig zijn bij het oplossen van script problemen die zich kunnen voordoen.


## <a name="see-also"></a>Zie ook

- [HDInsight clusters met de actie Script aanpassen][hdinsight-cluster-customize]
- [Installeren en gebruiken van een op HDInsight clusters][hdinsight-install-spark]
- [Installeren en gebruiken van R op HDInsight clusters][hdinsight-r-scripts]
- [Installeren en gebruiken Solr op HDInsight clusters](hdinsight-hadoop-solr-install.md).
- [Installeren en gebruiken Giraph op HDInsight clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
