<properties 
    pageTitle="Gebruik van de Emulator Azure opslag voor ontwikkelen en testen | Microsoft Azure" 
    description="De emulator Azure opslagruimte biedt een gratis lokale ontwikkelomgeving voor ontwikkelen en testen ten opzichte van Azure-opslag. Meer informatie over de opslag emulator, inclusief hoe aanvragen worden geverifieerd, hoe u verbinding maken met de emulator vanuit uw toepassing en het gebruik van het hulpprogramma voor." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Gebruik de Emulator Azure opslag voor ontwikkelen en testen

## <a name="overview"></a>Overzicht

De Microsoft Azure opslag emulator biedt een lokale omgeving waarin de services Azure Blob, wachtrij en tabel voor de software te ontwikkelen emuleert. De emulator opslag gebruikt, kunt u uw toepassing ten opzichte van de van de opslagservices lokaal testen zonder te maken van een Azure-abonnement of dat de kosten. Wanneer u tevreden bent over hoe uw toepassing in de emulator werkt, kunt u overschakelen naar de opslag van Azure-account gebruikt in de cloud.

> [AZURE.NOTE] De emulator opslag is beschikbaar als onderdeel van de [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). U kunt ook de opslagruimte emulator met het [installatieprogramma van de zelfstandige](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409)installeren. Als u wilt configureren de emulator opslag, moet u beheerdersbevoegdheden hebt op de computer.
> 
> De opslagruimte emulator momenteel alleen op Windows wordt uitgevoerd.
>  
> Opmerking: gegevens die zijn gemaakt in één versie van de emulator opslagruimte niet is gegarandeerd toegankelijk wanneer u een andere versie gebruikt. Als u nodig hebt om uw gegevens voor de lange termijn, het raadzaam deze gegevens op te slaan in een Azure opslag-account, in plaats van in de emulator opslag.

## <a name="how-the-storage-emulator-works"></a>De werking van de emulator opslag
 
De opslagruimte emulator maakt gebruik van een lokale Microsoft SQL Server-instantie en het lokale bestandssysteem emuleert van de Azure storage-services. De opslagruimte emulator standaard in een database in Microsoft SQL Server 2012 Express LocalDB.  U kunt kiezen voor het configureren van de emulator opslag voor toegang tot een lokaal exemplaar van SQL Server in plaats van het exemplaar LocalDB. Zie [de begin- en initialisatie de emulator opslag](#start-and-initialize-the-storage-emulator) hieronder voor meer informatie.

SQL Server Management Studio Express om de installatie van uw LocalDB te beheren, kunt u installeren. De opslagruimte emulator verbindt met SQL Server- of LocalDB met Windows-verificatie. 

Er zijn enkele verschillen in functionaliteit aanwezig tussen de opslagruimte emulator en Azure storage-services. Zie [verschillen tussen de opslagruimte emulator en Azure-opslag](#differences-between-the-storage-emulator-and-azure-storage)voor meer informatie over deze verschillen.

## <a name="authenticating-requests-against-the-storage-emulator"></a>Verificatie van aanvragen tegen de emulator opslag

Net als met Azure-opslag in de cloud, moet elke aanvraag die u ten opzichte van de emulator opslag aanbrengt worden geverifieerd, tenzij een anonieme aanvraag is. U kunt verifiëren aanvragen ten opzichte van de opslagruimte emulator met gedeelde sleutel verificatie of met een gedeelde access-handtekening (SA's).

### <a name="authentication-with-shared-key-credentials"></a>Verificatie met Shared Key referenties

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Zie voor meer informatie over verbindingstekenreeksen, [Verbindingsreeks van de Azure-opslag configureren](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Verificatie met een gedeelde access-handtekening 

Sommige Azure opslag client-bibliotheken, zoals de library Xamarin ondersteunen alleen verificatie met een gedeelde toegangstoken handtekening (SA's). U moet deze SA's token met een hulpmiddel of de toepassing die ondersteuning biedt voor verificatie Shared Key maken. Een eenvoudige manier om de token SA's te genereren is via Azure PowerShell:

1. Azure PowerShell installeren als u nog niet is gedaan. Gebruik van de nieuwste versie van de Azure PowerShell-cmdlets wordt aanbevolen. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md#Install) voor installatie-instructies.

2. Open Azure PowerShell en voer de volgende opdrachten. Vervang *accountnaam* en *ACCOUNT_KEY ==* met uw eigen referenties. *CONTAINER_NAME* vervangen door een naam van uw keuze.

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

De resulterende gedeelde toegang handtekening URI voor de nieuwe container moet er ongeveer als volgt te werk:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

De gedeelde access-handtekening hebt gemaakt met dit voorbeeld geldt voor één dag. De handtekening krijgen volledige toegang (dat wil zeggen lezen, schrijven, verwijderen, lijst) tot BLOB's in de container.

Zie voor meer informatie over gedeelde toegang handtekeningen, [Gebruik van gedeelde Access handtekeningen (SA's)](storage-dotnet-shared-access-signature-part-1.md).


## <a name="start-and-initialize-the-storage-emulator"></a>Start en de opslag emulator geïnitialiseerd

Als u wilt de emulator Azure opslag, selecteer de knop Start of druk op de Windows-toets. Begin te typen **Azure opslag Emulator**en selecteer de emulator in de lijst met toepassingen. 

Als de emulator actief is, ziet u een pictogram in het systeemvak van Windows.

Wanneer de opslagruimte emulator wordt gestart, wordt een opdrachtregel venster wordt weergegeven. U kunt deze opdrachtregel venster gebruiken om te starten en stoppen de emulator opslag, evenals gegevens wissen, huidige status, en de emulator geïnitialiseerd. Zie [Opslagruimte Emulator-opdrachtregel in het hulpmiddel](#storage-emulator-command-line-tool-reference)voor meer informatie.

Wanneer de opdrachtregel-venster wordt gesloten, blijft de emulator opslag uitvoeren. Als u wilt weer de opdrachtregel opnieuw, voer de stappen uit als wanneer de opslagruimte emulator starten.

De eerste keer dat u de emulator opslag uitvoert wordt de lokale opslag-omgeving geïnitialiseerd voor u. Het initialisatieproces maakt u een database in LocalDB en behoudt HTTP-poorten voor elke service lokale opslag. 

De opslagruimte emulator is al dan niet standaard naar de map C:\Program Files (x86) \Microsoft SDKs\Azure\Storage Emulator geïnstalleerd. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Initialisatie van de emulator opslag als u wilt gebruiken, een andere SQL-database

Initialisatie van de opslagruimte emulator zodat deze verwijzen naar een SQL-database-instantie dan de LocalDB standaardexemplaar kunt u opslagruimte emulator hulpprogramma voor de opdrachtregel. Houd er rekening mee dat u moet worden uitgevoerd hulpprogramma voor de opdrachtregel met beheerdersbevoegdheden initialisatie van de back-enddatabase voor de opslag-emulator:

1. Klik op de knop **Start** of druk op de **Windows** -toets. Begin te typen `Azure Storage Emulator` en selecteer deze wanneer dit wordt weergegeven om het hulpprogramma opslag emulator voor weer te geven.
2. Typ in het venster opdrachtprompt de volgende opdracht uit, waar `<SQLServerInstance>` is de naam van de SQL Server-instantie. U kunt gebruiken LocalDb `(localdb)\v11.0` als de SQL Server-instantie.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    U kunt ook de volgende opdracht uit, die ervoor zorgt de emulator dat gebruik van de standaard SQL Server-instantie gebruiken:

        AzureStorageEmulator init /server .\\ 

    Of u de volgende opdracht, die wordt opnieuw geïnitialiseerd de database met het LocalDB standaardexemplaar kunt gebruiken:

        AzureStorageEmulator init /forceCreate 

Zie [Opslagruimte Emulator-opdrachtregel in het hulpmiddel](#storage-emulator-command-line-tool-reference)voor meer informatie over deze opdrachten.

## <a name="addressing-resources-in-the-storage-emulator"></a>Adressering van resources in de emulator opslag

De eindpunten van de service voor de opslag emulator zijn verschilt van een Azure opslag-account. Het verschil is vanwege het feit dat de lokale computer geen domeinnamen omzetten, voert zodat de eindpunten van de emulator opslag een lokaal adres in plaats van een domeinnaam vereist.

Wanneer u adres van een resource in een Azure opslag-account, kunt u de volgende kleurenschema, waar de accountnaam maakt deel uit van de hostnaam URI en de resource wordt behandeld maakt deel uit van het pad URI gebruiken:

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

De volgende URI is bijvoorbeeld een geldig adres voor een blob in een Azure opslag-account:

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

Omdat de lokale computer geen domein naamresolutie voert, maakt de accountnaam in de emulator opslag deel uit van het pad URI in plaats van de hostnaam. U de volgende kleurenschema voor een resource die wordt uitgevoerd in de opslagruimte emulator gebruiken:

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

Zo mogelijk het volgende adres worden gebruikt voor toegang tot een blob in de opslagruimte emulator:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

De eindpunten van de service voor de opslag emulator zijn:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Adressering van het account secundaire met AB-GRS

Vanaf versie 3.1 is ondersteunt het account van de emulator opslag leestoegang geografische-redundante herhaling (AB-GRS). Opslag resources in de cloud zowel in de lokale emulator, u kunt toegang tot de secundaire locatie door toevoegen - secundaire naar de accountnaam van het. Zo mogelijk het volgende adres worden gebruikt voor toegang tot een blob met de secundaire alleen-lezen in de opslagruimte emulator:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] Aangeroepen aan de secundaire met de emulator opslag, gebruikt u de bibliotheek van de Client opslag voor .NET 3,2 of hoger. Zie de [Bibliotheek van Microsoft Azure opslag Client voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) voor meer informatie.

## <a name="storage-emulator-command-line-tool-reference"></a>Opslag emulator hulpprogramma voor de opdrachtregel verwijzing

Starten in versie 3.0, wanneer u de Emulator opslag starten, ziet u een opdrachtregel venster pop-upvenster. Gebruik de opdrachtregel venster starten en stoppen als u ook de emulator query voor status en andere bewerkingen uitvoeren.

> [AZURE.NOTE] Als u de Microsoft Azure berekenen emulator is geïnstalleerd, wordt een pictogram op de taakbalk wordt weergegeven wanneer u de Emulator opslag starten. Klik met de rechtermuisknop op het pictogram om een menu, dat een grafische manier om te starten en stoppen de Emulator opslagruimte biedt zichtbaar te maken.

### <a name="command-line-syntax"></a>De syntaxis van de opdrachtregel

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Opties

Als u wilt weergeven in de lijst met opties, typt u `/help` bij de opdrachtprompt.

| Optie | Beschrijving                                                    | Opdracht                                                                                                 | Argumenten                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Starten**  | De opslagruimte emulator wordt gestart.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-inprocess*: de emulator in het huidige proces in plaats van het maken van een nieuw proces te starten.                          |
| **Stoppen**   | De opslagruimte emulator stopt.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Status** | De status van de opslagruimte emulator afdrukken                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Wissen**  | Hiermee wist u de gegevens in alle services op de opdrachtregel worden opgegeven. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *BLOB*: wissen blob-gegevens. <br/>*wachtrij*: Hiermee wist wachtrijgegevens. <br/>*tabel*: Hiermee wist u gegevens in een tabel. <br/>*alle*: Hiermee wist u alle gegevens in alle services. |
| **Initialisatie**   | Hiermee kunt u eenmalige initialisatie voor het instellen van de emulator uitvoeren.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-server Servernaam\exemplaarnaam*: Hiermee geeft u op de server waarop het exemplaar van SQL. <br/>*-sqlinstance exemplaarnaam*: Hiermee geeft u de naam van de SQL-exemplaar moet worden gebruikt in de standaard-server-instantie. <br/>*-forcecreate*: forceert u het maken van de SQL-database, zelfs als deze al bestaat. <br/>*-inprocess*: initialisatie in het huidige proces in plaats van een nieuwe aanmaakproces worden uitgevoerd. U moet de huidige proces met een verhoogde machtigingen om te kunnen uitvoeren initialisatie starten.          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Verschillen tussen de opslagruimte emulator en Azure Storage

Omdat de emulator opslag een geëmuleerde omgeving in een lokale SQL-exemplaar uitgevoerd is, zijn er enkele functionele verschillen tussen de emulator en een account Azure opslag in de cloud:

- De opslagruimte emulator ondersteunt alleen een enkele vaste account en een bekende verificatiesleutel.

- De emulator opslagruimte is niet een scalable storage-service en biedt geen ondersteuning voor een groot aantal gelijktijdige clients.

- Zoals is beschreven in [adressering resources in de opslagruimte emulator](#addressing-resources-in-the-storage-emulator), worden anders resources besproken in de opslagruimte emulator versus een Azure opslag-account. Dit verschil is vanwege het feit dat domein naamresolutie beschikbaar is in de cloud, maar niet op de lokale computer.

- Vanaf versie 3.1 is ondersteunt het account van de emulator opslag leestoegang geografische-redundante herhaling (AB-GRS). Alle accounts hebt AB-GRS ingeschakeld in de emulator, en er is nooit een vertraging tussen de primaire en secundaire replica's. De bewerkingen ophalen Blob Service Stat, krijgen wachtrij Service stat en krijgen tabel Service stat worden ondersteund op de secundaire account en retourneert altijd de waarde van de `LastSyncTime` antwoord element als de huidige tijd op basis van de onderliggende SQL-database.

    Aangeroepen aan de secundaire met de emulator opslag, gebruikt u de bibliotheek van de Client opslag voor .NET 3,2 of hoger. Zie de [Bibliotheek van Microsoft Azure opslag Client voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) voor meer informatie.

- Het bestand service en het SMB-protocol service eindpunten worden momenteel niet ondersteund in de emulator opslag.

- De opslagruimte emulator retourneert een fout VersionNotSupportedByEmulator (HTTP-statuscode 400 - Ongeldige aanvragen) als u een versie van de storage-services die nog niet wordt ondersteund door de versie van de emulator die u gebruikt.

### <a name="differences-for-blob-storage"></a>Verschillen voor Blob storage 

De volgende verschillen zijn van toepassing op blobopslag in de emulator:

- De label voor opslag emulator ondersteunt alleen groter maximaal 2 GB.

- Een Blob plaatsen-bewerking mogelijk mislukt ten opzichte van een blob die bestaat in de opslagruimte emulator en heeft een actieve lease, zelfs als de ID van de lease niet is opgegeven als onderdeel van de aanvraag kunt invullen. 

- Blob bewerkingen worden niet ondersteund door de emulator toevoegen. U probeert een bewerking op een toevoegquery blob geeft als resultaat een fout FeatureNotSupportedByEmulator (HTTP-statuscode 400 - Ongeldige aanvragen).

### <a name="differences-for-table-storage"></a>Verschillen voor Table storage 

De volgende verschillen zijn van toepassing op Table storage in de emulator:

- Datumeigenschappen in de tabel-service in de opslagruimte emulator ondersteunen alleen het bereik worden ondersteund door SQL Server 2005 (*dat wil zeggen*, ze zijn vereist moet na 1 januari 1753). Alle datums voor 1 januari 1753 worden gewijzigd in deze waarde. De precisie van datums is beperkt tot de precisie van SQL Server 2005, wat betekent dat datums nauwkeurig 1 zijn/300th van een tweede.

- De opslagruimte emulator ondersteunt partition sleutel en rij belangrijke eigenschapswaarden van minder dan 512 bytes elke. De totale grootte van de naam van het account, de tabelnaam van de en belangrijke eigenschapnamen samen niet bovendien langer zijn dan 900 bytes.

- De totale grootte van een rij in een tabel in de opslagruimte emulator is beperkt tot minder dan 1 MB.

- Typ in de emulator opslag eigenschappen van gegevens `Edm.Guid` of `Edm.Binary` ondersteunt alleen het `Equal (eq)` en `NotEqual (ne)` vergelijkingsoperatoren in query filteren tekenreeksen.

### <a name="differences-for-queue-storage"></a>Verschillen voor wachtrij opslag

Er zijn geen verschillen specifiek voor wachtrij opslag in de emulator.

## <a name="storage-emulator-release-notes"></a>Releaseopmerkingen voor opslag-emulator

### <a name="version-45"></a>Versie 4.5

- Vaste een fout waardoor initialisatie en de installatie van de opslagruimte emulator mislukt wanneer de naam van de back-database is gewijzigd.

### <a name="version-44"></a>Versie 4.4

- De opslagruimte emulator ondersteunt nu versie 2015-12-11 van de services opslagruimte op Blob, wachtrij en tabel service-eindpunten.

- De opslag-emulator ongewenste verzameling blob-gegevens is nu efficiënter gesteld over grote aantallen BLOB's.

- Vaste een fout waardoor container ACL XML in iets anders uit hoe de storage-service moet worden gevalideerd.

- Een fout die soms veroorzaakt max en min DateTime-waarden moet worden gerapporteerd in de verkeerde tijdzone opgelost.

### <a name="version-43"></a>Versie 4.3

- De opslagruimte emulator ondersteunt nu 2015-07-08-versie van de services opslagruimte op Blob, wachtrij en tabel service-eindpunten.

### <a name="version-42"></a>Versie 4.2

- De opslagruimte emulator ondersteunt nu 2015-04-05-versie van de services opslagruimte op Blob, wachtrij en tabel service-eindpunten.

### <a name="version-41"></a>Versie 4.1

- De opslagruimte emulator ondersteunt nu versie 21-02-2015 van de services opslagruimte op service-eindpunten, Blob, wachtrij en tabel, met uitzondering van de nieuwe functies voor Blob toevoegen. 

- De opslagruimte emulator retourneert nu een zinvolle foutbericht wordt weergegeven als u een versie van de storage-services die nog niet wordt ondersteund door deze versie van de emulator gebruikt. We raden u aan de nieuwste versie van de emulator gebruiken. Download de nieuwste versie van de emulator opslag als u een fout VersionNotSupportedByEmulator (HTTP-statuscode 400 - Ongeldige aanvragen).

- Een fout vaste waarin een raceauto veroorzaakt entiteit tabelgegevens als u wilt niet juist tijdens gelijktijdige samenvoegbewerkingen.

### <a name="version-40"></a>Versie 4.0

- De opslag van uitvoerbare emulator is gewijzigd in *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Versie 3,2

- De opslagruimte emulator ondersteunt nu 2014-02-14-versie van de services opslagruimte op Blob, wachtrij en tabel service-eindpunten. Houd er rekening mee dat bestand service eindpunten worden momenteel niet ondersteund in de emulator opslag. Zie [versiebeheer voor de opslag Azure Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) voor meer informatie over versie 2014-02-14.

### <a name="version-31"></a>Versie 3.1

- Leestoegang geografische-redundante opslag (AB-GRS) wordt nu ondersteund in de emulator opslag. De Blob Service stat ophalen, krijgen wachtrij Service stat en krijgen tabel stat API's worden ondersteund voor de secundaire account en wordt de waarde van het element van de reactie LastSyncTime altijd geretourneerd als de huidige tijd op basis van de onderliggende SQL-database. Aangeroepen aan de secundaire met de emulator opslag, gebruikt u de bibliotheek van de Client opslag voor .NET 3,2 of hoger. Zie de bibliotheek met Microsoft Azure opslag-Client voor .NET-verwijzing voor meer informatie.

### <a name="version-30"></a>Versie 3.0

- De emulator Azure opslagruimte is niet meer in hetzelfde pakket als de emulator berekeningscluster verzonden.

- De grafische gebruikersinterface van de opslag-emulator is vervangen door een aanpasbare opdracht lijn interface. Zie voor meer informatie over de opdracht lijn-interface, opslag Emulator opdrachtregel hulpmiddel. De grafische gebruikersinterface blijft aanwezig zijn in versie 3.0, maar kunnen alleen worden geopend wanneer de Emulator berekenen wordt geïnstalleerd door met de rechtermuisknop op het pictogram op de taakbalk en opslag Emulator-gebruikersinterface weergeven te selecteren.

- Versie 2013-08-15 van de Azure storage-services wordt nu volledig ondersteund. (Deze versie is eerder alleen ondersteund door opslag Emulator versie 2.2.1 Preview.)
