<properties
    pageTitle="Aan de clientzijde versleuteling met .NET voor Microsoft Azure-opslag | Microsoft Azure"
    description="De bibliotheek Azure opslag-Client voor .NET ondersteunt aan de clientzijde versleuteling en integratie met Azure-toets kluis voor maximale beveiliging voor de opslag van Azure-toepassingen."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Aan de clientzijde versleuteling en Azure belangrijke kluis voor Microsoft Azure-opslag

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Overzicht

De [Bibliotheek van Azure opslag Client voor .NET Nuget pakket](https://www.nuget.org/packages/WindowsAzure.Storage) ondersteunt coderen van gegevens in clienttoepassingen voordat uploaden naar Azure Storage en ontsleutelen van gegevens bij het downloaden van de client. De bibliotheek ondersteunt ook integratie met [Azure toets kluis](https://azure.microsoft.com/services/key-vault/) voor belangrijke opslagbeheer-account.

Zie voor een stapsgewijze zelfstudie waarin u bij het proces helpt voor het coderen van BLOB's met aan de clientzijde versleuteling en Azure-toets kluis, [versleutelen en ontsleutelen BLOB's in Microsoft Azure Storage met Azure toets kluis](storage-encrypt-decrypt-blobs-key-vault.md).

Zie voor aan de clientzijde versleuteling met Java, [Aan de clientzijde versleuteling met Java voor Microsoft Azure opslag](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Versleutelen en ontsleutelen via de envelop-methode

De processen van coderen en decoderen volgt u de methode envelop.

### <a name="encryption-via-the-envelope-technique"></a>Versleuteling via de envelop-methode

Versleuteling via de methode envelop werkt in de volgende manier:

1. De bibliotheek van de client Azure opslag genereert een inhoud versleutelingssleutel (CEK), dat wil een symmetric sleutel van een eenmalige gebruik zeggen.
2. Gebruikersgegevens is versleuteld met deze CEK.
3. De CEK wordt vervolgens verpakt (versleuteld) met de toets codering (KEK). De KEK kunt wordt aangegeven door een sleutel-id en worden van een combinatie van asymmetrische sleutel of een symmetric sleutel en kunnen worden beheerd lokaal of opgeslagen in Azure-toets kluizen.

    De bibliotheek van de opslag-client zelf heeft nooit toegang tot KEK. De bibliotheek Hiermee wordt de algoritme van de belangrijkste tekstterugloop die is verstrekt door toets kluis. Gebruikers kunt gebruiken om aangepaste voor belangrijke tekstterugloop/uitpakken desgewenst.

4. De versleutelde gegevens is vervolgens naar de opslag van Azure-service geüpload. De tekstterugloop toets samen met de metagegevens van enkele extra versleuteling wordt opgeslagen als metagegevens (op een blob) of geïnterpoleerd met de versleutelde gegevens (berichten in wachtrij plaatsen en tabel entiteiten).

### <a name="decryption-via-the-envelope-technique"></a>Decoderen via de envelop-methode

Decoderen via de methode envelop werkt in de volgende manier:

1. De clientbibliotheek wordt ervan uitgegaan dat de gebruiker wordt beheerd met de belangrijkste sleutel (KEK) lokaal of in Azure-toets kluizen. De gebruiker hoeft niet te weten de specifieke toets die is gebruikt voor versleuteling. In plaats daarvan kan een belangrijke resolvercache waarin belangrijke identificaties worden omgezet in toetsen en worden ingesteld gebruikt.
2. De clientbibliotheek downloads de versleutelde gegevens samen met versleuteling materiaal die is opgeslagen op de service.
3. De toets teruggelopen inhoud versleuteling (CEK) is en vervolgens omwikkelde (ontsleuteld) met de belangrijkste codering belangrijke (KEK). Hier nogmaals heeft de clientbibliotheek geen toegang tot KEK. Gewoon Hiermee wordt de aangepaste of uitpakken algoritme van de provider van de sleutel kluis.
4. De inhoud sleutel (CEK) worden vervolgens de versleutelde gebruikersgegevens ontsleutelen.

## <a name="encryption-mechanism"></a>Coderingsmethode

De bibliotheek van de client opslag maakt gebruik van [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) om te versleutelen gebruikersgegevens. Name [Gecodeerde blok koppelen (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) -modus met AES. Elke service-werkt enigszins anders, zodat we elk van deze hier wordt besproken.

### <a name="blobs"></a>BLOB 's

De clientbibliotheek ondersteunt momenteel codering van hele BLOB's. Name versleuteling wordt ondersteund wanneer gebruikers met de **UploadFrom** * methoden of de * *OpenWrite** methode. Voor downloads, beide voltooid en bereik downloads worden ondersteund.

Tijdens het coderen, wordt de clientbibliotheek een willekeurig initialisatie Vector (IV) van 16 bytes, samen met een willekeurige inhoud versleutelingssleutel (CEK) 32 bytes genereren en envelop versleuteling van de blob-gegevens van deze gegevens uitvoeren. De tekstterugloop CEK en enkele extra codering-metagegevens worden vervolgens opgeslagen als blob-metagegevens samen met het versleutelde blob op de service.

> [AZURE.WARNING] Als u bewerkt of uw eigen metagegevens voor de blob uploaden, moet u ervoor zorgen dat deze metagegevens behouden blijft. Als u bestanden uploadt nieuwe metagegevens zonder deze metagegevens, de tekstterugloop CEK IV en andere metagegevens niet verloren en de inhoud blob nooit meer opvraagbare zal zijn.

Downloaden van een versleutelde blob heeft betrekking op het ophalen van de inhoud van de hele blob met de **DownloadTo***/**BlobReadStream** gemak methoden. De tekstterugloop CEK is al en samen de IV (opgeslagen als blob metagegevens in dit geval) gebruikt om terug te keren de ontsleuteld gegevens aan de gebruikers.

Downloaden van een willekeurige bereik (**DownloadRange*** methoden) in de versleutelde blob heeft betrekking op het aanpassen van het bereik die door gebruikers om te krijgen een kleine hoeveelheid aanvullende gegevens die kunnen worden gebruikt om het succes ontsleutelen de gevraagde bereik.

Alle typen blob (BLOB's blokkeren, pagina's BLOB's en BLOB's toevoegen) kunnen worden versleuteld/ontsleuteld met dit schema.

### <a name="queues"></a>Wachtrijen

Aangezien berichten van een indeling worden kunnen, definieert de clientbibliotheek een aangepaste indeling met daarin de initialisatie Vector (IV) en de toets versleutelde inhoud versleuteling (CEK) in de berichttekst.

Tijdens het coderen, de clientbibliotheek genereert een willekeurig IV van 16 bytes samen met een willekeurig CEK 32 bytes en envelop versleuteling van de tekst van het bericht wachtrij van deze gegevens uitvoeren. De tekstterugloop CEK en enkele extra codering metagegevens worden toegevoegd aan het bericht versleutelde wachtrij. Deze gewijzigde bericht (Zie hieronder) is opgeslagen op de service.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Tijdens het ontsleutelen, is de tekstterugloop toets opgehaald uit het bericht wachtrij en al. De IV is ook opgehaald uit het bericht wachtrij en gebruikt samen met de omwikkelde toets naar de wachtrij berichtgegevens ontsleutelen. Houd er rekening mee dat de metagegevens versleuteling kleine (onder 500 bytes), is dus terwijl deze worden meegeteld voor de 64KB de limiet voor een bericht wachtrij, het effect beheerbare moet.

### <a name="tables"></a>Tabellen

De clientbibliotheek ondersteunt codering van entiteitseigenschappen voor het invoegen en vervang bewerkingen.

>[AZURE.NOTE] Samenvoegen is momenteel niet ondersteund. Aangezien een subset van eigenschappen mogelijk zijn versleuteld eerder met een andere sleutel, treden gewoon samenvoegen van de nieuwe eigenschappen en bijwerken van de metagegevens verlies van gegevens. Gesprekken voeren extra service te lezen van de vooraf bestaande entiteit van de service samenvoegen hetzij vereist of met een nieuwe sleutel per eigenschap, die beide niet geschikt zijn voor betere prestaties.

Tabel gegevensversleuteling werkt als volgt:  

1. Gebruikers Geef de eigenschappen worden versleuteld.
2. De clientbibliotheek genereert een willekeurig initialisatie Vector (IV) van 16 bytes samen met een willekeurige inhoud versleutelingssleutel (CEK) 32 bytes voor elke entiteit en envelop versleuteling uitvoeren met de afzonderlijke eigenschappen worden versleuteld door een nieuwe IV per eigenschap af te leiden. De versleutelde eigenschap wordt opgeslagen als binaire gegevens.
3. De tekstterugloop CEK en enkele extra codering-metagegevens worden vervolgens opgeslagen als twee extra gereserveerde eigenschappen. De eerste gereserveerde eigenschap (_ClientEncryptionMetadata1) is de tekenreekseigenschap van een die de informatie over IV, versie en tekstterugloop-toets bevat. De tweede gereserveerde eigenschap (_ClientEncryptionMetadata2) is een binaire eigenschap met de informatie over de eigenschappen die zijn versleuteld. De informatie in deze tweede eigenschap (_ClientEncryptionMetadata2) is versleuteld.
4. Vanwege deze extra gereserveerde eigenschappen vereist voor versleuteling, kunnen gebruikers alleen 250 aangepaste eigenschappen in plaats van 252 er nog. De totale grootte van de entiteit moet minder dan 1 MB.

Houd er rekening mee dat alleen tekenreekseigenschappen kunnen worden gecodeerd. Als andere soorten eigenschappen moeten worden versleuteld, moeten ze worden geconverteerd naar tekenreeksen. De versleutelde tekenreeksen zijn opgeslagen op de service als binaire eigenschappen en ze worden geconverteerd naar tekenreeksen na ontsleutelen.

Gebruikers moeten de eigenschappen worden versleuteld opgeven voor tabellen, naast het beleid versleuteling. Dit kan worden uitgevoerd door een kenmerk [EncryptProperty] (voor POCO entiteiten die zijn afgeleid van TableEntity) of een resolvercache versleuteling in verzoek opties. Een oplossing versleuteling is voor een gemachtigde die ervoor zorgt dat een partitiesleutel, rij-toets en naam van eigenschap geeft een Booleaanse waarde waarmee wordt aangegeven of die eigenschap moet worden versleuteld. Tijdens het coderen, de clientbibliotheek deze informatie wordt gebruikt om te bepalen of een eigenschap moet worden gecodeerd tijdens het schrijven naar het netwerk. De gemachtigde bovendien de mogelijkheid van logica rond hoe eigenschappen worden gecodeerd. (Bijvoorbeeld als X, typt u vervolgens versleutelen eigenschap A; anders versleutelen eigenschappen A en b te drukken.) Houd er rekening mee dat dit niet nodig is bij het lezen of query's uitvoeren entiteiten gegevens op te geven.

### <a name="batch-operations"></a>Batchbewerkingen

In de batch bewerkingen uitvoeren, worden de dezelfde KEK gebruikt in alle rijen in de desbetreffende batchbewerking omdat de clientbibliotheek slechts staat één opties object (en dus één beleid/KEK) per batchbewerking. Echter de clientbibliotheek intern genereert een nieuwe willekeurige IV en willekeurige CEK per rij in de batch. Gebruikers kunnen ook verschillende eigenschappen voor elke bewerking in de batch versleutelen door het definiëren van dit probleem in de resolvercache versleuteling kiezen.

### <a name="queries"></a>Query 's

Als u wilt querybewerkingen uitvoert, moet u een belangrijke resolvercache die kan alle toetsen in de resultatenset omzetten. Als een entiteit opgenomen in het queryresultaat niet worden omgezet in een provider, wordt de clientbibliotheek een fout genereren. Voor een query die aan de clientzijde prognoses uitvoert, wordt de clientbibliotheek de eigenschappen van de metagegevens speciale versleuteling (_ClientEncryptionMetadata1 en _ClientEncryptionMetadata2) al dan niet standaard toevoegen aan de geselecteerde kolommen.

## <a name="azure-key-vault"></a>Azure belangrijke kluis

Azure-toets kluis helpt beschermen cryptografische sleutels en geheimen die worden gebruikt door de cloud-toepassingen en -services. Met behulp van Azure-toets kluis, kunnen gebruikers coderen toetsen en geheimen (zoals verificatiesleutels, opslag account toetsen, gegevens versleuteling toetsaanslagen. PFX-bestanden en wachtwoorden) met behulp van de toetsen die zijn beveiligd met hardware beveiligingsmodules (HSM's). Zie voor meer informatie [Wat Azure-toets kluis is?](../key-vault/key-vault-whatis.md).

De bibliotheek van de client opslag gebruikt de sleutel kluis core-bibliotheek alleen worden aangeboden als een gemeenschappelijk kader via Azure voor het beheren van toetsen. De extra voordeel van het gebruik van de sleutel kluis extensies bibliotheek ook toegevoegd aan andere gebruikers. De bibliotheek extensies bevat nuttige functies om eenvoudig en naadloze Symmetric/RSA lokale en cloud belangrijke providers en met aggregatie en caching van.

### <a name="interface-and-dependencies"></a>Interface en afhankelijkheden

Er zijn drie toets kluis-pakketten:

- Microsoft.Azure.KeyVault.Core bevat de IKey en IKeyResolver. Dit is een kleine pakket met geen afhankelijkheden. De bibliotheek van de client opslag voor .NET gedefinieerd als een afhankelijkheid.
- Microsoft.Azure.KeyVault bevat de sleutel kluis REST-client.
- Microsoft.Azure.KeyVault.Extensions bevat extensie-code die implementaties van cryptografische algoritmen en een RSAKey en een SymmetricKey bevat. Dit is afhankelijk van de naamruimten Core en KeyVault en biedt functionaliteit om een statistische resolvercache (wanneer gebruikers gebruiken om meerdere key wilt) en een in de cache belangrijke resolvercache te definiëren. Hoewel de bibliotheek van de client opslagruimte is niet rechtstreeks afhankelijk van dit pakket, als gebruikers Azure-toets kluis wilt om hun sleutels of met de toets kluis extensies gebruiken van de lokale en cloud cryptographic providers gebruiken, moet ze dit pakket.

Toets kluis is bedoeld voor hoogwaardige outmodel toetsen en bandbreedteregeling limieten per toets kluis zijn ontworpen met dit mee. Wanneer u aan de clientzijde versleuteling met toets kluis uitvoert, is het gewenste model symmetric outmodel toetsen opgeslagen als geheimen in sleutel kluis en opgeslagen in de cache lokaal gebruiken. Gebruikers, moeten het volgende doen:

1. Maak een geheim offline en uploadt dit naar de toets kluis.
2. Basis id van het geheim als een parameter gebruiken voor het oplossen van de huidige versie van het geheim voor versleuteling en deze informatie lokaal in cache. CachingKeyResolver gebruiken voor het in cache opslaan; gebruikers niet naar verwachting implementeren hun eigen caching van logica.
3. Gebruik de cache resolvercache als invoer bij het maken van het beleid versleuteling.

Meer informatie over het gebruik van de sleutel kluis vindt u in de [voorbeelden van de code versleuteling](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

## <a name="best-practices"></a>Aanbevolen procedures

Ondersteuning voor codering is alleen beschikbaar in de bibliotheek van de client opslag voor .NET. Windows Phone en Windows Runtime ondersteunen momenteel geen versleuteling.

>[AZURE.IMPORTANT] Wanneer u aan de clientzijde versleuteling gebruikt, worden op de hoogte van de volgende belangrijke punten:
>
>- Bij het lezen van of naar een versleutelde blob schrijven, hele blob upload opdrachten en bereik/geheel blob downloaden opdrachten gebruiken. Schrijf niet naar een versleutelde blob protocol bewerkingen zoals plaatsen blokkeren, bloklijst plaatsen, pagina's schrijven, wissen pagina's of blok toevoegen; u mogelijk anders de versleutelde blob beschadigde en maak het gelezen.
>- Klik voor tabellen bestaat een soortgelijke beperking. Zorg ervoor dat versleutelde eigenschappen niet bijwerken zonder de metagegevens versleuteling bijwerken.
>- Als u metagegevens op de versleutelde blob instelt, kunt u mogelijk de metagegevens voor versleuteling vereist voor ontsleutelen sinds het instellen van metagegevens niet toegevoegde is overschrijven. Dit geldt ook voor momentopnamen; Gebruik geen metagegevens tijdens het maken van een momentopname van een versleutelde blob. Metagegevens moet zijn ingesteld, zorg ervoor dat u de methode **FetchAttributes** eerst gebruikt om toegang te krijgen van de huidige versleuteling-metagegevens aan te roepen als gelijktijdige schrijft voorkomen terwijl metagegevens wordt ingesteld.
>- De eigenschap **RequireEncryption** in de standaardopties voor de aanvraag voor gebruikers die zich alleen met versleutelde gegevens werken moeten inschakelen. Zie hieronder voor meer informatie.


## <a name="client-api--interface"></a>Client-API / Interface

Tijdens het maken van een object EncryptionPolicy, gebruikers alleen een sleutel (implementeren IKey), kunnen leveren alleen een resolvercache (implementeren IKeyResolver) of beide. IKey is het eenvoudige key type die wordt aangeduid met een sleutel-id en waarmee de logica voor het uitpakken van tekstterugloop /. IKeyResolver wordt gebruikt voor het oplossen van een sleutel tijdens het ontsleutelen. Hiermee definieert u een ResolveKey methode die resulteert in een IKey krijgen een belangrijke id. Hiermee kan gebruikers de mogelijkheid om te kiezen tussen meerdere toetsen die worden beheerd op meerdere locaties.

- De sleutel altijd wordt gebruikt voor codering, en de afwezigheid van een sleutel resulteert in een fout.
- Voor decoderen:
    - De belangrijkste resolvercache wordt aangeroepen als u de sleutel is opgegeven. Als de resolvercache is opgegeven, maar geen een toewijzing voor de sleutel-id, wordt een fout gegenereerd.
    - Als resolvercache niet is opgegeven, maar een sleutel is opgegeven, wordt de sleutel wordt gebruikt als de id overeenkomt met de vereiste sleutel-id. Als de id niet overeenkomen, wordt een fout gegenereerd.

De [versleuteling voorbeelden](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) laten zien een meer gedetailleerde end-to-end-scenario voor BLOB's, wachtrijen en tabellen, samen met de toets kluis-integratie.

### <a name="requireencryption-mode"></a>RequireEncryption-modus

Gebruikers kunnen (optioneel) een bewerkingsmodus waar alle uploads en downloads worden gecodeerd inschakelen. In deze modus gegevens zonder een beleid versleuteling uploaden of downloaden van gegevens die niet op de service is versleuteld, niet op de client. De eigenschap **RequireEncryption** van het object request opties besturingselementen voor dit probleem. Als uw toepassing, worden alle objecten die zijn opgeslagen in de opslagruimte van Azure gecodeerd, kunt u de eigenschap **RequireEncryption** instellen op de standaardopties voor de aanvraag voor de service-client-object. Bijvoorbeeld **CloudBlobClient.DefaultRequestOptions.RequireEncryption** instellen op **waar** versleuteling vereisen voor alle blob bewerkingen uitgevoerd via dat clientobject.

### <a name="blob-service-encryption"></a>BLOB service-versleuteling

Een object **BlobEncryptionPolicy** maken en stel deze in de opties van de aanvraag (per API of op een clientniveau met behulp van **DefaultRequestOptions**). Rest wordt verwerkt door de clientbibliotheek intern.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Upload the encrypted contents to the blob.
    blob.UploadFromStream(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    MemoryStream outputStream = new MemoryStream();
    blob.DownloadToStream(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Wachtrij service-versleuteling

Een object **QueueEncryptionPolicy** maken en stel deze in de opties van de aanvraag (per API of op een clientniveau met behulp van **DefaultRequestOptions**). Rest wordt verwerkt door de clientbibliotheek intern.


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
    queue.AddMessage(message, null, null, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);

### <a name="table-service-encryption"></a>Tabel service-versleuteling

Naast het maken van een beleid versleuteling en ingesteld op Opties voor de aanvraag, moet u een **EncryptionResolver** in **TableRequestOptions**opgeven of het kenmerk [EncryptProperty] ingesteld voor de entiteit.

#### <a name="using-the-resolver"></a>Gebruik van de resolvercache


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    {
        EncryptionResolver = (pk, rk, propName) =>
        {
            if (propName == "foo")
            {
                return true;
            }
            return false;
        },
        EncryptionPolicy = policy
    };

    // Insert Entity
    currentTable.Execute(TableOperation.Insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    {
        EncryptionPolicy = policy
    };

    TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
    TableResult result = currentTable.Execute(operation, retrieveOptions, null);

#### <a name="using-attributes"></a>Gebruik van kenmerken

Bovengenoemde, als de entiteit TableEntity implementeert, kunnen de eigenschappen worden versierd met het kenmerk [EncryptProperty] in plaats van de **EncryptionResolver**opgeven.

    [EncryptProperty]
    public string EncryptedProperty1 { get; set; }

## <a name="encryption-and-performance"></a>Versleuteling en prestaties

Houd er rekening mee dat uw opslagruimte Gegevensresultaten in meer belast versleutelt. De inhoud sleutel en IV moet worden gegenereerd, de inhoud zelf moet worden gecodeerd en aanvullende metagegevens moet worden opgemaakt en geüpload. Deze realiseren varieert afhankelijk van de hoeveelheid gegevens worden gecodeerd. Het is raadzaam dat klanten altijd het programma voor de prestaties tijdens de ontwikkeling testen.

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Versleutelen en ontsleutelen BLOB's in Microsoft Azure Storage met Azure-toets kluis](storage-encrypt-decrypt-blobs-key-vault.md)
- De [Bibliotheek van Azure opslag Client voor .NET NuGet pakket](https://www.nuget.org/packages/WindowsAzure.Storage) downloaden
- Downloaden van de Azure-toets kluis NuGet [Core](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [Client](http://www.nuget.org/packages/Microsoft.Azure.KeyVault/)en [uitbreidingen](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) -pakketten  
- Ga naar de [Azure-toets kluis documentatie](../key-vault/key-vault-whatis.md)

