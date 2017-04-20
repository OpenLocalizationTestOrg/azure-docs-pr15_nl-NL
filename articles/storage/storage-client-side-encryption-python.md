<properties
    pageTitle="Aan de clientzijde versleuteling met Python voor Microsoft Azure-opslag | Microsoft Azure"
    description="De bibliotheek Azure opslag-Client voor Python ondersteunt codering van aan de clientzijde voor maximale beveiliging voor de opslag van Azure-toepassingen."
    services="storage"
    documentationCenter="python"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Aan de clientzijde versleuteling met Python voor Microsoft Azure-opslag

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Overzicht

De [Bibliotheek van Azure opslag Client voor Python](https://pypi.python.org/pypi/azure-storage) ondersteunt coderen van gegevens in clienttoepassingen voordat uploaden naar Azure Storage en ontsleutelen van gegevens bij het downloaden van de client.

>[AZURE.NOTE] De bibliotheek Azure opslag Python is in de proefversie.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Versleutelen en ontsleutelen via de envelop-methode
Het proces van versleuteling en decoderen volgt u de methode envelop.

### <a name="encryption-via-the-envelope-technique"></a>Versleuteling via de envelop-methode
Versleuteling via de methode envelop werkt in de volgende manier:

1.  De bibliotheek van de client Azure opslag genereert een inhoud versleutelingssleutel (CEK), dat wil de symmetric sleutel van een een-permanente-gebruiken zeggen.

2.  Gebruikersgegevens is versleuteld met deze CEK.

3.  De CEK wordt vervolgens verpakt (versleuteld) met de toets codering (KEK). De KEK wordt aangegeven door een sleutel-id en kan zowel een combinatie van asymmetrische sleutel symmetric sleutel, die lokaal wordt beheerd.
De bibliotheek van de opslag-client zelf heeft nooit toegang tot KEK. De bibliotheek Hiermee wordt de algoritme van de belangrijkste tekstterugloop die is verstrekt door de KEK. Gebruikers kunt gebruiken om aangepaste voor belangrijke tekstterugloop/uitpakken desgewenst.

4.  De versleutelde gegevens is vervolgens naar de opslag van Azure-service geüpload. De tekstterugloop toets samen met de metagegevens van enkele extra versleuteling wordt opgeslagen als metagegevens (op een blob) of geïnterpoleerd met de versleutelde gegevens (berichten in wachtrij plaatsen en tabel entiteiten).

### <a name="decryption-via-the-envelope-technique"></a>Decoderen via de envelop-methode
Decoderen via de methode envelop werkt in de volgende manier:

1.  De clientbibliotheek wordt ervan uitgegaan dat de gebruiker de belangrijkste sleutel (KEK) lokaal wordt beheerd. De gebruiker hoeft niet te weten de specifieke toets die is gebruikt voor versleuteling. In plaats daarvan kan een belangrijke resolvercache, waarin verschillende sleutel-id's worden omgezet in toetsen, en worden ingesteld gebruikt.

2.  De clientbibliotheek downloads de versleutelde gegevens samen met versleuteling materiaal die is opgeslagen op de service.

3.  De toets teruggelopen inhoud versleuteling (CEK) is en vervolgens omwikkelde (ontsleuteld) met de belangrijkste codering belangrijke (KEK). Hier nogmaals heeft de clientbibliotheek geen toegang tot KEK. Deze gewoon roept uitpakken algoritme van de aangepaste provider.

4.  De toets inhoud versleuteling (CEK) worden vervolgens de versleutelde gebruikersgegevens ontsleutelen.

## <a name="encryption-mechanism"></a>Coderingsmethode  
De bibliotheek van de client opslag maakt gebruik van [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) om te versleutelen gebruikersgegevens. Name [Gecodeerde blok koppelen (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) -modus met AES. Elke service-werkt enigszins anders, zodat we elk van deze hier wordt besproken.

### <a name="blobs"></a>BLOB 's
De clientbibliotheek ondersteunt momenteel codering van hele BLOB's. Name versleuteling wordt ondersteund wanneer gebruikers de **maken gebruiken*** methoden. Gedurende downloads, beide voltooid en bereik downloads worden ondersteund en parallelization van beide uploaden en downloaden is beschikbaar.

Tijdens het coderen, wordt de clientbibliotheek een willekeurig initialisatie Vector (IV) van 16 bytes, samen met een willekeurige inhoud versleutelingssleutel (CEK) 32 bytes genereren en envelop versleuteling van de blob-gegevens van deze gegevens uitvoeren. De tekstterugloop CEK en enkele extra codering-metagegevens worden vervolgens opgeslagen als blob-metagegevens samen met het versleutelde blob op de service.

>[AZURE.WARNING] Als u bewerkt of uw eigen metagegevens voor de blob uploaden, moet u ervoor zorgen dat deze metagegevens behouden blijft. Als u bestanden nieuwe metagegevens zonder deze metagegevens uploadt, de tekstterugloop CEK, IV en andere metagegevens niet verloren en de inhoud blob nooit meer opvraagbare zal zijn.

Downloaden van een versleutelde blob heeft betrekking op het ophalen van de inhoud van de hele blob met de **krijgen*** gemak methoden. De tekstterugloop CEK is al en samen de IV (opgeslagen als blob metagegevens in dit geval) gebruikt om terug te keren de ontsleuteld gegevens aan de gebruikers.

Downloaden van een willekeurige bereik (**krijgen*** methoden met bereik parameters doorgegeven) in de versleutelde blob heeft betrekking op het aanpassen van het bereik die door gebruikers om te krijgen een kleine hoeveelheid aanvullende gegevens die kunnen worden gebruikt om het succes ontsleutelen de gevraagde bereik.

Blok BLOB's en pagina BLOB's kunnen alleen worden versleuteld/ontsleuteld met dit schema. Er is momenteel geen ondersteuning voor het coderen BLOB's toevoegen.

### <a name="queues"></a>Wachtrijen
Aangezien berichten van een indeling worden kunnen, definieert de clientbibliotheek een aangepaste indeling met daarin de initialisatie Vector (IV) en de toets versleutelde inhoud versleuteling (CEK) in de berichttekst.

Tijdens het coderen, de clientbibliotheek genereert een willekeurig IV van 16 bytes samen met een willekeurig CEK 32 bytes en envelop versleuteling van de tekst van het bericht wachtrij van deze gegevens uitvoeren. De tekstterugloop CEK en enkele extra codering metagegevens worden toegevoegd aan het bericht versleutelde wachtrij. Deze gewijzigde bericht (Zie hieronder) is opgeslagen op de service.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Tijdens het ontsleutelen, is de tekstterugloop toets opgehaald uit het bericht wachtrij en al. De IV is ook opgehaald uit het bericht wachtrij en gebruikt samen met de omwikkelde toets naar de wachtrij berichtgegevens ontsleutelen. Houd er rekening mee dat de metagegevens versleuteling kleine (onder 500 bytes), is dus terwijl deze worden meegeteld voor de 64KB de limiet voor een bericht wachtrij, het effect beheerbare moet.

### <a name="tables"></a>Tabellen
De clientbibliotheek ondersteunt codering van entiteitseigenschappen voor het invoegen en vervang bewerkingen.

>[AZURE.NOTE] Samenvoegen is momenteel niet ondersteund. Aangezien een subset van eigenschappen mogelijk zijn versleuteld eerder met een andere sleutel, treden gewoon samenvoegen van de nieuwe eigenschappen en bijwerken van de metagegevens verlies van gegevens. Gesprekken voeren extra service te lezen van de vooraf bestaande entiteit van de service samenvoegen hetzij vereist of met een nieuwe sleutel per eigenschap, die beide niet geschikt zijn voor betere prestaties.

Tabel gegevensversleuteling werkt als volgt:

1.  Gebruikers Geef de eigenschappen worden versleuteld.

2.  De clientbibliotheek genereert een willekeurig initialisatie Vector (IV) van 16 bytes samen met een willekeurige inhoud versleutelingssleutel (CEK) 32 bytes voor elke entiteit en envelop versleuteling uitvoeren met de afzonderlijke eigenschappen moeten worden gecodeerd door een nieuwe IV per eigenschap af te leiden. De versleutelde eigenschap wordt opgeslagen als binaire gegevens.

3.  De tekstterugloop CEK en enkele extra codering-metagegevens worden vervolgens opgeslagen als twee extra gereserveerde eigenschappen. De eerste gereserveerd eigenschap (\_ClientEncryptionMetadata1) de tekenreekseigenschap van een die de informatie over IV, versie en tekstterugloop sleutel bevat is. De tweede gereserveerd eigenschap (\_ClientEncryptionMetadata2) is een binaire eigenschap die bevat de informatie over de eigenschappen die zijn versleuteld. De informatie in deze tweede eigenschap (\_ClientEncryptionMetadata2) zelf versleuteld.

4.  Vanwege deze extra gereserveerde eigenschappen vereist voor versleuteling, kunnen gebruikers alleen 250 aangepaste eigenschappen in plaats van 252 er nog. De totale grootte van de entiteit moet minder dan 1MB.

    Houd er rekening mee dat alleen tekenreekseigenschappen kunnen worden gecodeerd. Als andere soorten eigenschappen moeten worden versleuteld, moeten ze worden geconverteerd naar tekenreeksen. De versleutelde tekenreeksen zijn opgeslagen op de service als binaire eigenschappen en ze worden geconverteerd naar tekenreeksen (onbewerkte tekenreeksen, niet EntityProperties met type EdmType.STRING) na ontsleutelen.

    Gebruikers moeten de eigenschappen worden versleuteld opgeven voor tabellen, naast het beleid versleuteling. Hiermee kan worden uitgevoerd door op te slaan deze eigenschappen in TableEntity objecten met het type is ingesteld op EdmType.STRING en ingesteld op waar of instellen van de encryption_resolver_function op het object tableservice versleutelen. Een oplossing versleuteling is een functie die ervoor zorgt dat een partitiesleutel, rij-toets en naam van eigenschap geeft een Booleaanse waarde waarmee wordt aangegeven of die eigenschap moet worden versleuteld. Tijdens het coderen, de clientbibliotheek deze informatie wordt gebruikt om te bepalen of een eigenschap moet worden gecodeerd tijdens het schrijven naar het netwerk. De gemachtigde bovendien de mogelijkheid van logica rond hoe eigenschappen worden gecodeerd. (Bijvoorbeeld als X, typt u vervolgens versleutelen eigenschap A; anders versleutelen eigenschappen A en b te drukken.) Houd er rekening mee dat dit niet nodig is bij het lezen of query's uitvoeren entiteiten gegevens op te geven.

### <a name="batch-operations"></a>Batchbewerkingen
Eén versleuteling beleid geldt voor alle rijen in de batch. De clientbibliotheek intern genereert een nieuwe willekeurige IV en willekeurige CEK per rij in de batch. Gebruikers kunnen ook verschillende eigenschappen voor elke bewerking in de batch versleutelen door het definiëren van dit probleem in de resolvercache versleuteling kiezen.
Als een batch de beheerder van een context via de tableservice batch()-methode gemaakt is, is de functie van de tableservice versleuteling beleid wordt automatisch toegepast op de batch. Als een batch expliciet gemaakt is door de ondersteuning voor de constructor, moet u het beleid versleuteling doorgegeven als een parameter en links niet gewijzigd voor de levensduur van de batch.
Houd er rekening mee dat entiteiten worden gecodeerd als ze worden ingevoegd in de batch met behulp van de batch versleuteling beleid (entiteiten zijn niet op het moment van de batch met behulp van de tableservice versleuteling beleid doorvoeren gecodeerd).

### <a name="queries"></a>Query 's
Als u wilt querybewerkingen uitvoert, moet u een belangrijke resolvercache die kan alle toetsen in de resultatenset omzetten. Als een entiteit opgenomen in het queryresultaat niet worden omgezet in een provider, wordt de clientbibliotheek een fout genereren. Voor een query die server kant prognoses uitvoert, de clientbibliotheek de eigenschappen van de metagegevens speciale versleuteling wordt toegevoegd (\_ClientEncryptionMetadata1 en \_ClientEncryptionMetadata2) al dan niet standaard naar de geselecteerde kolommen.

>[AZURE.IMPORTANT] Wanneer u aan de clientzijde versleuteling gebruikt, worden op de hoogte van de volgende belangrijke punten:
>
>- Bij het lezen van of naar een versleutelde blob schrijven, hele blob upload opdrachten en bereik/geheel blob downloaden opdrachten gebruiken. Schrijf niet naar een versleutelde blob protocol bewerkingen zoals plaatsen blokkeren, bloklijst plaatsen, pagina's schrijven of wissen pagina's. u mogelijk anders de versleutelde blob beschadigde en maak het gelezen.
>
>- Klik voor tabellen bestaat een soortgelijke beperking. Zorg ervoor dat versleutelde eigenschappen niet bijwerken zonder de metagegevens versleuteling bijwerken.
>
>- Als u metagegevens op de versleutelde blob instelt, kunt u mogelijk de metagegevens voor versleuteling vereist voor ontsleutelen sinds het instellen van metagegevens niet toegevoegde is overschrijven. Dit geldt ook voor momentopnamen; Gebruik geen metagegevens tijdens het maken van een momentopname van een versleutelde blob. Metagegevens moet zijn ingesteld, zorg ervoor dat u de methode **get_blob_metadata** eerst gebruikt om toegang te krijgen van de huidige versleuteling-metagegevens aan te roepen als gelijktijdige schrijft voorkomen terwijl metagegevens wordt ingesteld.
>
>- De vlag **require_encryption** op het serviceobject voor gebruikers die zich alleen met versleutelde gegevens werken moet inschakelen. Zie hieronder voor meer informatie.

De bibliotheek van de client opslag verwacht de meegeleverde KEK en belangrijke resolvercache implementeren van de volgende interface. [Azure toets kluis](https://azure.microsoft.com/services/key-vault/) ondersteuning voor Python KEK management in behandeling is en worden geïntegreerd in deze bibliotheek als voltooid.


## <a name="client-api--interface"></a>Client-API / Interface
Nadat een opslag-object (dat wil zeggen blockblobservice) is gemaakt, de gebruiker waarden kan toewijzen aan de velden die een beleid versleuteling vormen: key_encryption_key, key_resolver_function, en require_encryption. Gebruikers kunnen alleen een KEK invoeren alleen een resolvercache of beide. key_encryption_key is het eenvoudige key type die wordt aangeduid met een sleutel-id en waarmee de logica voor het uitpakken van tekstterugloop /. key_resolver_function wordt gebruikt voor het oplossen van een sleutel tijdens het ontsleutelen. Een geldig KEK krijgen een belangrijke id geretourneerd. Hiermee kan gebruikers de mogelijkheid om te kiezen tussen meerdere toetsen die worden beheerd op meerdere locaties.

De KEK moet de volgende methoden voor het coderen met succes gegevens implementeren:
- wrap_key(cek): terugloopt van de opgegeven CEK (bytes) met behulp van een algoritme van de keuze van de gebruiker. Geeft als resultaat de tekstterugloop-toets.
- get_key_wrap_algorithm(): geeft als resultaat de algoritme gebruikt om te laten teruglopen toetsen.
- get_kid(): geeft als resultaat de id van de tekenreeks voor deze KEK.
De KEK moet de volgende methoden is om gegevens te decoderen implementeren:
- unwrap_key (cek, algoritme): geeft als resultaat de omwikkelde vorm van de opgegeven CEK met de algoritme van de tekenreeks opgegeven.
- get_kid(): geeft als resultaat een tekenreeks met belangrijke-id voor deze KEK.

De belangrijkste resolvercache moet ten minste een methode die, gegeven een sleutel-id, geeft als resultaat de bijbehorende KEK implementeren van de bovenstaande interface implementeren. Alleen deze methode is om te worden toegewezen aan de eigenschap key_resolver_function op het serviceobject.

- De sleutel altijd wordt gebruikt voor codering, en de afwezigheid van een sleutel resulteert in een fout.
- Voor decoderen:
    - De belangrijkste resolvercache wordt aangeroepen als u de sleutel is opgegeven. Als de resolvercache is opgegeven, maar geen een toewijzing voor de sleutel-id, wordt een fout gegenereerd.
    - Als resolvercache niet is opgegeven, maar een sleutel is opgegeven, wordt de sleutel wordt gebruikt als de id overeenkomt met de vereiste sleutel-id. Als de id niet overeenkomen, wordt een fout gegenereerd.

      De voorbeelden versleuteling in azure.storage.samples <fix URL>illustreren een meer gedetailleerde end-to-end-scenario voor BLOB's, wachtrijen en tabellen.
        Voorbeeldimplementaties van de KEK en belangrijke resolvercache beschikbaar in de voorbeeldbestanden als KeyWrapper en KeyResolver respectievelijk.

### <a name="requireencryption-mode"></a>RequireEncryption-modus
Gebruikers kunnen (optioneel) een bewerkingsmodus waar alle uploads en downloads worden gecodeerd inschakelen. In deze modus gegevens zonder een beleid versleuteling uploaden of downloaden van gegevens die niet op de service is versleuteld, niet op de client. De vlag **require_encryption** op het serviceobject Hiermee stelt u dit probleem.

### <a name="blob-service-encryption"></a>BLOB service-versleuteling
Stel de versleuteling velden voor het beleid op het object blockblobservice. Rest wordt verwerkt door de clientbibliotheek intern.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_block_blob_service.key_encryption_key = kek
    my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

    # Upload the encrypted contents to the blob.
    my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

    # Download and decrypt the encrypted contents from the blob.
    blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)

### <a name="queue-service-encryption"></a>Wachtrij service-versleuteling
Stel de versleuteling velden voor het beleid op het object queueservice. Rest wordt verwerkt door de clientbibliotheek intern.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_queue_service.key_encryption_key = kek
    my_queue_service.key_resolver_funcion = key_resolver.resolve_key

    # Add message
    my_queue_service.put_message(queue_name, content)

    # Retrieve message
    retrieved_message_list = my_queue_service.get_messages(queue_name)

### <a name="table-service-encryption"></a>Tabel service-versleuteling
Naast het maken van een beleid versleuteling en ingesteld op Opties voor de aanvraag, moet u een **encryption_resolver_function** op de **tableservice**opgeven of het kenmerk versleutelen ingesteld voor de EntityProperty.

### <a name="using-the-resolver"></a>Gebruik van de resolvercache

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Define the encryption resolver_function.
    def my_encryption_resolver(pk, rk, property_name):
            if property_name == 'foo':
                    return True
            return False

    # Set the KEK and key resolver on the service object.
    my_table_service.key_encryption_key = kek
    my_table_service.key_resolver_funcion = key_resolver.resolve_key
    my_table_service.encryption_resolver_function = my_encryption_resolver

    # Insert Entity
    my_table_service.insert_entity(table_name, entity)

    # Retrieve Entity
    # Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
    my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])

### <a name="using-attributes"></a>Gebruik van kenmerken
Bovengenoemde, kan een eigenschap worden gemarkeerd voor versleuteling door deze in een object EntityProperty opslaan en het instellen van het veld versleutelen.

    encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)

## <a name="encryption-and-performance"></a>Versleuteling en prestaties
Houd er rekening mee dat uw opslagruimte Gegevensresultaten in meer belast versleutelt. De inhoud sleutel en IV moet worden gegenereerd, de inhoud zelf moet worden gecodeerd en aanvullende metagegevens moet worden opgemaakt en geüpload. Deze realiseren varieert afhankelijk van de hoeveelheid gegevens worden gecodeerd. Het is raadzaam dat klanten altijd het programma voor de prestaties tijdens de ontwikkeling testen.

## <a name="next-steps"></a>Volgende stappen

- De [Bibliotheek van Azure opslag Client voor Java PyPi pakket](https://pypi.python.org/pypi/azure-storage) downloaden
- De [bibliotheek van Azure opslag-Client voor Python broncode uit GitHub](https://github.com/Azure/azure-storage-python) downloaden
