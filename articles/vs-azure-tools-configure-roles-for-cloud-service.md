<properties
   pageTitle="De rollen configureren voor een Azure Cloudservice met Visual Studio | Microsoft Azure"
   description="Informatie over het instellen en configureren van rollen van Azure cloudservices met behulp van Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>De rollen configureren voor een Azure Cloudservice met Visual Studio

Een Azure cloudservice kan hebben een of meer werknemer of web rollen. Voor elke rol moet u bepalen hoe die rol is ingesteld en ook configureren hoe die rol wordt uitgevoerd. Meer informatie over rollen in cloudservices, raadpleegt u de video [Inleiding tot Azure-Cloudservices](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). De gegevens voor uw cloudservice is opgeslagen in de volgende bestanden:

- **ServiceDefinition.csdef**

    De definitie servicebestand Hiermee definieert u de runtime-instellingen voor uw cloudservice inclusief welke functies zijn vereist, eindpunten en VM grootte. Geen van de gegevens die zijn opgeslagen in dit bestand kunnen worden gewijzigd wanneer uw rol wordt uitgevoerd.

- **ServiceConfiguration.cscfg**

    Het configuratiebestand service Hiermee configureert u hoeveel exemplaren van een rol worden uitgevoerd en de waarden van de instellingen voor een rol. De gegevens die zijn opgeslagen in dit bestand kunnen worden gewijzigd terwijl uw rol wordt uitgevoerd.

Als u wilt kunnen verschillende waarden voor deze instellingen opslaan voor hoe uw rol wordt uitgevoerd, kunt u meerdere configuraties hebben. U kunt een andere service te configureren voor elke implementatieomgeving. U kunt bijvoorbeeld instellen dat de verbindingsreeks van uw opslag-account om de lokale Azure opslag emulator gebruiken in een lokale service te configureren en een andere service te configureren voor het gebruik van de Azure opslag in de cloud te maken.

Wanneer u een nieuwe Azure cloudservice in Visual Studio maakt, worden standaard twee configuraties gemaakt. Deze configuraties worden toegevoegd aan uw project Azure. De configuraties heten:

- ServiceConfiguration.Cloud.cscfg

- ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Een Azure cloudservice configureren

U kunt een Azure cloudservice in Solution Explorer configureren in Visual Studio, zoals wordt weergegeven in de volgende afbeelding.

![Cloudservice configureren](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>Een Azure cloudservice configureren

1. Als u wilt configureren elke rol in uw project Azure in **Solution Explorer**, opent u het snelmenu voor de rol in het Azure project en kies vervolgens **Eigenschappen**.

    Een pagina met de naam van de rol wordt weergegeven in de Visual Studio-editor. De pagina bevat de velden voor het tabblad **configuratie** .

1. Kies de naam van de configuratie van de service die u wilt bewerken in de lijst **Service te configureren** .

    Als u wijzigingen aanbrengen op alle van de serviceconfiguraties voor deze rol wilt, kunt u **Alle configuraties**kiezen.

    >[AZURE.IMPORTANT] Als u een specifieke serviceconfiguratie kiest, worden bepaalde eigenschappen zijn uitgeschakeld omdat ze alleen kunnen worden ingesteld voor alle configuraties. Als u wilt deze eigenschappen bewerken, moet u alle configuraties.

    U kunt nu een tabblad om een ingeschakelde eigenschappen in deze weergave te werken.

## <a name="change-the-number-of-role-instances"></a>Het aantal exemplaren van de rol wijzigen

Ter verbetering van de prestaties van uw cloudservice, kunt u het aantal exemplaren van een rol die worden uitgevoerd, op basis van het aantal gebruikers of het selectievakje laden verwacht voor een bepaalde rol wijzigen. Wanneer de cloudservice wordt uitgevoerd in Azure wordt aangegeven, wordt een afzonderlijke virtuele machine gemaakt voor elk exemplaar van een rol. Dit geldt voor de facturering voor de implementatie van deze cloudservice. Zie voor meer informatie over facturering, [informatie over uw factuur voor Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>Het aantal exemplaren voor een rol wijzigen

1. Kies het tabblad **configuratie** .

1. Kies de configuratie van de service die u wilt bijwerken in de lijst **Service te configureren** .

    >[AZURE.NOTE] U kunt het aantal exemplaar voor een specifieke service te configureren of voor alle configuraties instellen.

1. Voer in het tekstvak **exemplaar aantal** het aantal exemplaren die u voor deze rol wilt starten.

    >[AZURE.NOTE] Elk exemplaar wordt uitgevoerd op een aparte virtuele machine wanneer u uw cloudservice naar Azure publiceren.

1. Kies de knop **Opslaan** op de werkbalk om deze wijzigingen opslaan in het configuratiebestand service.

## <a name="manage-connection-strings-for-storage-accounts"></a>Verbindingstekenreeksen met de voor opslag-accounts beheren

U kunt toevoegen, verwijderen of wijzigen van verbindingstekenreeksen met de voor uw serviceconfiguraties. U kunt verschillende verbindingstekenreeksen voor verschillende configuraties. Bijvoorbeeld: u kunt een lokale verbindingsreeks voor een lokale service-configuratie met een waarde van `UseDevelopmentStorage=true`. U kunt ook voor het configureren van de configuratie van een cloud-service die gebruikmaakt van een account opslagruimte in Azure wordt aangegeven.

>[AZURE.WARNING] Wanneer u de belangrijkste accountgegevens Azure opslagruimte voor een verbindingsreeks van opslag account invoert, wordt deze informatie lokaal opgeslagen in het configuratiebestand service. Deze informatie is echter momenteel niet opgeslagen als versleutelde tekst.

Met behulp van een andere waarde voor elke service te configureren, er geen andere verbindingstekenreeksen gebruiken in uw cloudservice of wijzigen van uw code, wanneer u uw cloudservice naar Azure publiceren. Kunt u dezelfde naam voor de verbindingsreeks in uw code en wordt de waarde verschillende, op basis van de configuratie van de service die u selecteert als u uw cloudservice samenstelt of wanneer u de afbeelding publiceert.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Voor het beheren van verbindingstekenreeksen met de voor opslag-accounts

1. Kies het tabblad **Instellingen** .

1. Kies de configuratie van de service die u wilt bijwerken in de lijst **Service te configureren** .

    >[AZURE.NOTE] U kunt bijwerken verbindingstekenreeksen met de voor een specifieke service te configureren, maar als u wilt toevoegen of verwijderen van een verbindingsreeks moet u alle configuraties.

1. Als u wilt een verbindingsreeks hebt toegevoegd, klikt u op de knop **Instelling toevoegen** . Een nieuwe vermelding wordt toegevoegd aan de lijst.

1. Typ in het tekstvak **naam** de naam die u wilt gebruiken voor de verbindingsreeks.

1. Kies in de vervolgkeuzelijst **Type** **Verbindingsreeks**.

1. De waarde voor de verbindingsreeks wilt wijzigen, klikt u op de knop weglatingsteken (...). Het dialoogvenster **Opslag verbindingsreeks maken** wordt weergegeven.

1. Als u wilt gebruiken in de lokale opslag account emulator, kies de knop van de optie **Microsoft Azure opslag emulator** en kies vervolgens de knop **OK** .

1. Als u een account opslagruimte in Azure wordt aangegeven, klikt u op de knop van de optie **uw abonnement** en selecteer het gewenste opslag-account.

1. Aangepaste referenties kiest u de knop opties **handmatig ingevoerd referenties** . Voer de naam van het opslag-account en de toets primaire of tweede. Raadpleeg [voorbereiden om te publiceren of een Azure-toepassing van Visual Studio implementeren](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md)voor informatie over het maken van een opslag-account en hoe u de gegevens voor de opslag-account in het dialoogvenster **Maken opslag verbindingsreeks** invoeren.

1. U verwijdert een verbindingsreeks, selecteert u de verbindingsreeks en kies vervolgens de knop **Instelling verwijderen** .

1. Kies het pictogram **Opslaan** op de werkbalk om deze wijzigingen opslaan in het configuratiebestand service.

1. Voor toegang tot de verbindingsreeks in het configuratiebestand service, moet u de waarde van de configuratie-instelling. De volgende code toont een voorbeeld waar blobopslag wordt gemaakt en de gegevens die zijn geüpload, met een verbindingsreeks `MyConnectionString` uit het configuratiebestand service wanneer een gebruiker **Button1** op de pagina Default.aspx in de Webrol voor een Azure-cloudservice kiest. Voeg de volgende beweringen naar Default.aspx.cs gebruiken:

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Open Default.aspx.cs in de ontwerpweergave en een knop toevoegen vanuit de werkset. Voeg de volgende code aan de `Button1_Click` methode. In deze code wordt `GetConfigurationSettingValue` aan de waarde van het configuratiebestand service voor de verbindingsreeks. Wordt een blob gemaakt in de opslagruimte account waarnaar wordt verwezen in de verbindingsreeks `MyConnectionString` en ten slotte het programma tekst toegevoegd aan de blob.

    ```
    protected void Button1_Click(object sender, EventArgs e)
    {
        // Setup the connection to Azure Storage
        var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
        var blobClient = storageAccount.CreateCloudBlobClient();
        // Get and create the container
        var blobContainer = blobClient.GetContainerReference("quicklap");
        blobContainer.CreateIfNotExists();
        // upload a text blob
        var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
        blob.UploadText("Hello Azure");

    }
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Aangepaste instellingen gebruiken in uw Azure-cloudservice toevoegen

Aangepaste instellingen in het configuratiebestand service kunnen u een naam en een waarde voor een tekenreeks voor een specifieke service te configureren. U kunt deze instelling gebruiken voor het configureren van een functie in uw cloudservice door het lezen van de instelling en het gebruik van deze waarde om te bepalen de logica in uw code. U kunt deze service configuratiewaarden wijzigen zonder dat u moet opnieuw opbouwen uw-pakket of wanneer uw cloudservice wordt uitgevoerd. Uw code kunt controleren voor meldingen van wanneer een instelling wordt gewijzigd. Zie [RoleEnvironment.Changing gebeurtenis](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

U kunt toevoegen, verwijderen of aangepaste instellingen wijzigen voor uw serviceconfiguraties. U kunt verschillende waarden voor deze tekenreeksen voor verschillende configuraties.

Met behulp van een andere waarde voor elke service te configureren, er geen andere tekenreeksen in de cloudservice gebruiken of wijzigen van uw code, wanneer u uw cloudservice naar Azure publiceren. Kunt u dezelfde naam voor de tekenreeks in uw code en wordt de waarde verschillende, op basis van de configuratie van de service die u selecteert als u uw cloudservice samenstelt of wanneer u de afbeelding publiceert.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Aangepaste instellingen gebruiken in uw Azure-cloudservice toevoegen

1. Kies het tabblad **Instellingen** .

1. Kies de configuratie van de service die u wilt bijwerken in de lijst **Service te configureren** .

    >[AZURE.NOTE] U kunt bijwerken tekenreeksen voor een specifieke service te configureren, maar als u wilt toevoegen of verwijderen van een tekenreeks, moet u **Alle configuraties**selecteren.

1. Als u wilt toevoegen van een tekenreeks, klikt u op de knop **Instelling toevoegen** . Een nieuwe vermelding wordt toegevoegd aan de lijst.

1. Typ in het tekstvak **naam** de naam die u wilt gebruiken voor de tekenreeks.

1. Kies in de vervolgkeuzelijst **Type** **tekenreeks**.

1. Typ als u wilt toevoegen of wijzigen van de waarde voor de tekenreeks, in het vak **waarde** de nieuwe waarde.

1. Als u wilt verwijderen van een tekenreeks, selecteert u de tekenreeks en kies vervolgens de knop **Instellingen verwijderen** .

1. Kies de knop **Opslaan** op de werkbalk om deze wijzigingen opslaan in het configuratiebestand service.

1. Voor toegang tot de tekenreeks in een bestand voor de configuratie van de service, moet u de waarde van de configuratie-instelling.

    U nodig hebt om ervoor te zorgen dat de volgende via overzichten zijn al toegevoegd aan Default.aspx.cs net zoals u in de vorige procedure hebt gedaan.

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Voeg de volgende code aan de `Button1_Click` methode voor toegang tot deze tekenreeks op dezelfde manier dat u toegang een verbindingsreeks tot. Uw code kan uitvoeren enkele specifieke code op basis van de waarde van de tekenreeks instellingen voor de service-configuratiebestand die wordt gebruikt.

    ```
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
    if (settingValue == “ThisValue”)
    {
    // Perform these lines of code
    }
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Lokale opslag voor elk exemplaar van de rol beheren

U kunt lokale bestandsopslag systeem voor elk exemplaar van een rol toevoegen. U kunt lokale gegevens hier die niet hoeft te worden gebruikt door andere rollen opslaan. Alle gegevens die u niet nodig hebt om op te slaan in de tabel, blob of opslag van de SQL-Database kunnen worden opgeslagen hier. U kunt bijvoorbeeld deze lokale opslag naar cachegegevens die mogelijk moeten opnieuw worden gebruikt. Deze opgeslagen gegevens kan niet worden gebruikt door andere exemplaren van een rol. 

Instellingen voor lokale opslag gelden voor alle configuraties. U kunt alleen toevoegen, verwijderen of wijzigen van de lokale opslag voor alle configuraties.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Voor het beheren van lokale opslag voor elk exemplaar van de rol

1. Kies het tabblad **Lokale opslag** .

1. Kies **Alle configuraties**in de lijst **Service te configureren** .

1. Als u wilt toevoegen van de invoer in een lokale opslag, klikt u op de knop **Lokale opslag toevoegen** . Een nieuwe vermelding wordt toegevoegd aan de lijst.

1. Typ in het tekstvak **naam** de naam die u wilt gebruiken voor deze lokale opslag.

1. Typ in het tekstvak **grootte** , de grootte in MB die u nodig voor dit lokale opslag hebt.

1. Als u wilt verwijderen van de gegevens in deze lokale opslag wanneer de virtuele machine voor deze functie herhaald wordt, selecteert u het selectievakje **opschonen op rol Prullenbak** .

1. Als u wilt bewerken in een bestaande vermelding in de lokale opslag, kiest u de rij die u wilt bijwerken. Vervolgens kunt u de velden, bewerken, zoals is beschreven in de vorige stappen.

1. Als u wilt verwijderen van de invoer in een lokale opslag, kiest u het fragment opslag in de lijst en kies vervolgens de knop **Lokale opslag verwijderen** .

1. Als u wilt deze wijzigingen in de service-configuratie-bestanden opslaan, kiest u het pictogram **Opslaan** op de werkbalk.

1. Voor toegang tot de lokale opslag die u in het configuratiebestand service hebt toegevoegd, moet u de waarde van de lokale bron configuratie-instelling. Gebruik de volgende programmacoderegels toegang tot deze waarde en maken van een bestand met de naam van **MyStorageTest.txt** en een regel testgegevens naar dat bestand schrijven. U kunt deze code in toevoegen de `Button_Click` methode die u in de bovenstaande procedures gebruikt:

1. U nodig hebt om ervoor te zorgen dat de volgende via overzichten worden toegevoegd aan Default.aspx.cs:

    ```
    using System.IO;
    using System.Text;
    ```

1. Voeg de volgende code aan de `Button1_Click` methode. Hiermee wordt het bestand in de lokale opslag en schrijft testgegevens naar dat bestand.

    ```
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");

    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
    String filePath = Path.Combine(paths);

    using (FileStream writeStream = File.Create(filePath))
    {
          Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
          writeStream.Write(textToWrite, 0, textToWrite.Length);
    }
    ```

1. (Optioneel) Als wilt bekijken in dit bestand dat u hebt gemaakt wanneer u uw cloudservice lokaal uitvoert, gebruikt u de volgende stappen uit:

  1. De Webrol uitgevoerd en selecteer **Button1** om ervoor te zorgen dat de code in `Button1_Click` wordt genoemd.

  1. In het systeemvak, opent u het snelmenu voor het Azure pictogram en kies **Berekenen Emulator-gebruikersinterface weergeven**. Het dialoogvenster **Azure berekenen Emulator** wordt weergegeven.

  1. Selecteer de Webrol.

  1. Kies op de menubalk, **hulpmiddelen**, **Open lokale archief**. Een Windows Verkenner-venster wordt weergegeven.

  1. Klik op de menubalk **MyStorageTest.txt** invoeren in het tekstvak **Zoeken** en kiest u op **Enter** om te beginnen met zoeken.

    Het bestand wordt weergegeven in de lijst met zoekresultaten.

  1. De inhoud van het bestand bekijken, opent u het snelmenu voor het bestand en kies **openen**.

## <a name="collect-cloud-service-diagnostics"></a>Cloud service diagnostische gegevens verzamelen

U kunt naar diagnostische gegevens verzamelen voor uw Azure-cloudservice. Deze gegevens wordt toegevoegd aan een account opslag. U kunt verschillende verbindingstekenreeksen voor verschillende configuraties. Bijvoorbeeld: u kunt een lokale opslag-account voor een lokale service-configuratie met een waarde van UseDevelopmentStorage = true. U kunt ook voor het configureren van de configuratie van een cloud-service die gebruikmaakt van een account opslagruimte in Azure wordt aangegeven. Zie voor meer informatie over Azure diagnostische gegevens vastleggen gegevens door gebruik van Azure diagnostische gegevens verzamelen.

>[AZURE.NOTE] Configuratie van de lokale service is al geconfigureerd voor gebruik van lokale bronnen. Als u de configuratie van de cloud-service te publiceren van uw Azure cloudservice gebruikt, wordt de verbindingsreeks die u opgeeft wanneer u publiceert ook voor de verbindingsreeks diagnostische hulpprogramma's gebruikt, tenzij u een verbindingsreeks hebt opgegeven. Als u uw cloudservice gebruik van Visual Studio inpakken, wordt de verbindingsreeks in de serviceconfiguratie niet gewijzigd.

### <a name="to-collect-cloud-service-diagnostics"></a>Voor het verzamelen van cloud service diagnostische gegevens

1. Kies het tabblad **configuratie** .

1. Kies de configuratie van de service die u wilt bijwerken of **Alle configuraties**te kiezen in de lijst **Service te configureren** .

    >[AZURE.NOTE] U kunt het account opslagruimte voor een specifieke serviceconfiguratie bijwerken, maar als u wilt in- of uitschakelen van diagnostische gegevens moet u alle configuraties.

1. Als u diagnostische gegevens, selecteert u het selectievakje **Diagnostische hulpprogramma's inschakelen** .

1. De waarde voor de opslag-account wilt wijzigen, klikt u op de knop weglatingsteken (...).

    Het dialoogvenster **Opslag verbindingsreeks maken** wordt weergegeven.

1. Als u wilt gebruiken in een lokale verbindingsreeks, kies Azure opslag emulator optie en kies vervolgens de knop **OK** .

1. Als u wilt gebruiken een opslag-account dat is gekoppeld aan uw Azure-abonnement, kiest u de optie **uw abonnement** .

1. Een account opslag voor de lokale verbindingsreeks kiest u de optie **handmatig ingevoerd referenties** .

    Zie voor meer informatie over het maken van een opslag-account en hoe u de gegevens voor de opslag-account in het dialoogvenster **Maken opslag verbindingsreeks** invoeren [voorbereiden om te publiceren of een Azure-toepassing van Visual Studio implementeren](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Kies het opslag-account dat u wilt gebruiken in de **naam van het Account**.

    Als u handmatig invoeren van de referenties van uw opslag-account, kopiëren of typt u de primaire sleutel in de **accountsleutel**. Deze toets kan worden gekopieerd van de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885). Deze toets, deze stappen te volgen vanuit de weergave **Opslag-Accounts** in de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)kopiëren:
    
  1. Selecteer het opslag-account dat u wilt gebruiken voor uw cloudservice.

  1. Kies de knop **Toegangstoetsen beheren** aan de onderkant van het scherm. Het dialoogvenster **Toegangstoetsen beheren** wordt weergegeven.

  1. Als u wilt kopiëren de access-toets, kies de knop **naar Klembord kopiëren** . U kunt nu deze toets in het veld **accountsleutel** plakken.

1. Gebruik de opslag-account dat u, als de verbindingsreeks opgeeft voor diagnostische gegevens (en caching van) wanneer u uw cloudservice naar Azure publiceren, schakel het selectievakje **Update ontwikkeling opslag verbindingstekenreeksen voor diagnostisch hulpprogramma en cache met Azure opslagmedia domeinaccountreferenties wanneer publiceren naar Azure** .

1. Kies de knop **Opslaan** op de werkbalk om deze wijzigingen opslaan in het configuratiebestand service.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>De grootte van de virtuele machine gebruikt voor elke rol wijzigen

U kunt de grootte VM voor elke rol instellen. U kunt alleen deze grootte voor alle serviceconfiguraties instellen. Als u een kleinere machine selecteert, is klikt u vervolgens minder CPU cores, geheugen en lokale schijf opslag toegewezen. De toegewezen bandbreedte is ook kleiner. Zie voor meer informatie over deze formaten en de toegewezen resources [grootten voor Cloudservices](cloud-services/cloud-services-sizes-specs.md).

De vereiste bronnen voor elke virtuele machine in Azure wordt aangegeven van invloed is op de kosten van het uitvoeren van uw cloudservice in Azure wordt aangegeven. Zie voor meer informatie over Azure facturering, [informatie over uw factuur voor Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>De grootte van de virtuele machine wijzigen

1. Kies het tabblad **configuratie** .

1. Kies **Alle configuraties**in de lijst **Service te configureren** .

1. Als u wilt selecteren de grootte voor de virtuele machine voor deze rol, kiest u de juiste grootte in de lijst **VM grootte** .

1. Kies de knop **Opslaan** op de werkbalk om deze wijzigingen opslaan in het configuratiebestand service.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Eindpunten en certificaten voor uw rollen beheren

Netwerken eindpunten configureert u door het opgeven van het protocol, het poortnummer, en, voor HTTPS, de gegevens van het SSL-certificaat. Releases vóór juni 2012 ondersteunen HTTP, HTTPS en TCP. De juni 2012-versie kunt u deze protocollen en UDP. U kunt UDP niet gebruiken voor invoer eindpunten in de emulator berekeningscluster. Alleen voor interne eindpunten kunt u dat protocol.

Ter verbetering van de beveiliging van uw Azure-cloudservice, kunt u eindpunten die gebruikmaken van het HTTPS-protocol. Bijvoorbeeld, hebt u een cloudservice die wordt gebruikt door klanten aan inkooporders, gewenste om ervoor te zorgen dat hun gegevens met behulp van SSL is beveiligd.

U kunt ook de eindpunten die intern kunnen worden gebruikt of extern toevoegen. Externe eindpunten heten invoer eindpunten. Een eindpunt invoer kunt een ander toegangspunt aan gebruikers in uw cloudservice. Als u een WCF-service hebt, wilt u mogelijk een interne eindpunt voor een Webrol gebruiken voor toegang tot deze service weer.

>[AZURE.IMPORTANT] U kunt alleen eindpunten voor alle serviceconfiguraties bijwerken.

Als u de eindpunten HTTPS toevoegt, moet u een SSL-certificaat te gebruiken. Hiervoor kunt u certificaten koppelen aan uw rol voor alle serviceconfiguraties en gebruik deze voor uw eindpunten.

>[AZURE.IMPORTANT] Deze certificaten worden niet geleverd met uw service. U moet uw certificaten afzonderlijk naar Azure via de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)uploaden.

Een management certificaten die u aan uw configuraties koppelen alleen van toepassing als uw cloudservice wordt uitgevoerd in Azure wordt aangegeven. Wanneer uw cloudservice wordt uitgevoerd in de lokale ontwikkelomgeving, wordt een standaard-certificaat dat wordt beheerd door de emulator Azure berekeningscluster gebruikt.

### <a name="to-add-a-certificate-to-a-role"></a>Een certificaat toevoegen aan een rol

1. Kies het tabblad **certificaten** .

1. Kies **Alle configuraties**in de lijst **Service te configureren** .

    >[AZURE.NOTE] Als u wilt toevoegen of verwijderen van certificaten, moet u alle configuraties. U kunt de naam en de vingerafdruk voor een specifieke serviceconfiguratie bijwerken als dat nodig is.

1. Als u wilt een certificaat voor deze rol hebt toegevoegd, klikt u op de knop **Certificaat toevoegen** . Een nieuwe vermelding wordt toegevoegd aan de lijst.

1. Voer in het tekstvak **naam** de naam voor het certificaat.

1. Kies de locatie voor het certificaat dat u wilt toevoegen in de lijst **Archieflocatie** .

1. Kies de store die u wilt gebruiken om het certificaat te selecteren in de lijst **Opslaan** .

1. Als u wilt toevoegen op het certificaat, klikt u op de knop weglatingsteken (...). Het dialoogvenster **Beveiliging van Windows** wordt weergegeven.

1. Kies het certificaat dat u wilt gebruiken in de lijst en kies vervolgens de knop **OK** .

    >[AZURE.NOTE] Wanneer u een certificaat in de store certificaat toevoegt, worden alle tussenliggende certificaten automatisch toegevoegd aan de configuratie-instellingen voor u. Deze tussenliggende certificaten moeten ook worden geüpload naar Azure om te kunnen uw service voor SSL correct configureren.

1. Een certificaat verwijderen, kiest u het certificaat en kies vervolgens de knop **Certificaat verwijderen** .

1. Kies het pictogram **Opslaan** op de werkbalk deze wijzigingen opslaan in de service-configuratie-bestanden.

### <a name="to-manage-endpoints-for-a-role"></a>Voor het beheren van de eindpunten voor een rol

1. Kies het tabblad **eindpunten** .

1. Kies **Alle configuraties**in de lijst **Service te configureren** .

1. Als u wilt een eindpunt hebt toegevoegd, klikt u op de knop **Eindpunt toevoegen** . Een nieuwe vermelding wordt toegevoegd aan de lijst.

1. Typ in het tekstvak **naam** de naam die u wilt gebruiken voor dit eindpunt.

1. Kies het type van het eindpunt dat u nodig hebt in de lijst **Type** .

1. Kies het protocol voor het eindpunt dat u nodig hebt in de lijst **Protocol** .

1. Als er een input eindpunt, in het tekstvak **Openbare poort** , voert u de openbare poort gebruiken.

1. Typ in het tekstvak **Privé poort** de privé poort gebruiken.

1. Als het eindpunt vereist is voor het https-protocol, kiest u in de lijst met **De naam van de SSL-certificaat** een certificaat wilt gebruiken.

    >[AZURE.NOTE] Deze lijst ziet u de certificaten die u hebt toegevoegd voor deze rol in het tabblad **certificaten** .

1. Kies de knop **Opslaan** op de werkbalk deze wijzigingen in de service-configuratie-bestanden wilt opslaan.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over Azure projecten in Visual Studio door het lezen van [een Project Azure configureren](vs-azure-tools-configuring-an-azure-project.md). Meer informatie over het schema van de service cloud door te lezen [Schemaverwijzing](https://msdn.microsoft.com/library/azure/dd179398).
