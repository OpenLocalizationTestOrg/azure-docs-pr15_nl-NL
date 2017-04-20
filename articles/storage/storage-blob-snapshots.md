<properties
    pageTitle="Een alleen-lezen momentopname maken van een blob | Microsoft Azure"
    description="Informatie over het maken van een momentopname van een blob back-up blob-gegevens op een bepaald moment. Meer informatie over hoe momentopnamen zijn gefactureerd en hoe ze worden gebruikt om te minimaliseren capaciteit kosten."
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
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="create-a-blob-snapshot"></a>Een momentopname blob maken

## <a name="overview"></a>Overzicht

Een momentopname is een alleen-lezen-versie van een blob dat wordt uitgevoerd op een punt in tijd. Momentopnamen zijn handig om een back-up BLOB's. Nadat u een momentopname hebt gemaakt, kunt u lezen, kopiëren of verwijderen, maar u kunt deze niet wijzigen.

Een momentopname van een blob is gelijk aan de basis blob, behalve dat de blob URI heeft een **DateTime** -waarde die is toegevoegd aan de blob URI om aan te geven van de tijd waarop de momentopname is gemaakt. Als u een pagina blob-URI is bijvoorbeeld `http://storagesample.core.blob.windows.net/mydrives/myvhd`, de momentopname URI vergelijkbaar met is `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`. 

> [AZURE.NOTE] Alle momentopnamen delen van het grondtal blob URI. Het enige verschil tussen de basis blob en de momentopname is de toegevoegde **DateTime** -waarde.

Een blob kan een willekeurig aantal momentopnamen hebben. Momentopnamen persistent totdat ze expliciet worden verwijderd. Een momentopname kan niet de basis blob outlive. U kunt de momentopnamen die is gekoppeld aan de basis label voor het bijhouden van uw huidige momentopnamen opsommen.

Wanneer u een momentopname van een blob maakt, wordt de Systeemeigenschappen van de blob worden gekopieerd naar de momentopname met dezelfde waarde. De basis blob metagegevens wordt ook gekopieerd naar de momentopname, tenzij u afzonderlijke metagegevens voor de momentopname opgeven wanneer u deze hebt gemaakt.

Een lease die is gekoppeld aan de basis blob hebben geen invloed op de momentopname. U kunt geen een lease op een momentopname aanschaffen.

## <a name="create-a-snapshot"></a>Een momentopname maken

Het volgende voorbeeld ziet u hoe u een momentopname maakt in .NET. In dit voorbeeld geeft afzonderlijk metagegevens voor de momentopname wanneer deze is gemaakt.

    private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
    {
        // Create a new block blob in the container.
        CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

        // Add blob metadata.
        baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

        try
        {
            // Upload the blob to create it, with its metadata.
            await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

            // Sleep 5 seconds.
            System.Threading.Thread.Sleep(5000);

            // Create a snapshot of the base blob.
            // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
            // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
            Dictionary<string, string> metadata = new Dictionary<string, string>();
            metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
            await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        }
        catch (StorageException e)
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
 

## <a name="copy-snapshots"></a>Momentopnamen kopiëren

Kopieerbewerkingen die betrekking hebben op personen BLOB's en momentopnamen Volg deze regels:

- U kunt een momentopname via de basis blob kopiëren. Door een momentopname tot de positie van het grondtal blob promoveren, kunt u een eerdere versie van een blob terugzetten. De momentopname blijft, maar het grondtal blob wordt overschreven door een schrijfbare kopie van de opname.

- U kunt een momentopname kopiëren naar een blob bestemming onder een andere naam. De resulterende waarde van doel-blob is een schrijfbare blob en niet een momentopname.

- Als u een bron-blob kopieert, een momentopnamen van de bron-blob niet worden gekopieerd naar de bestemming. Wanneer een blob bestemming wordt overschreven door een kopie, wordt de momentopnamen die is gekoppeld aan de oorspronkelijke bestemming blob behouden blijven.

- Wanneer u een momentopname van een blok blob maakt, wordt ook van de blob vastgelegde bloklijst gekopieerd naar de momentopname. Een niet-doorgevoerde blokken worden niet gekopieerd.

## <a name="specify-an-access-condition"></a>Een access-voorwaarde opgeven

Zodat de momentopname wordt gemaakt alleen als een voorwaarde is voldaan, kunt u een access-voorwaarde opgeven. Om op te geven in een access-voorwaarde, gebruikt u de eigenschap **AccessCondition** . Als de opgegeven voorwaarde is voldaan, de momentopname is niet gemaakt en de Blob-service retourneert statuscode HTTPStatusCode.PreconditionFailed.

## <a name="delete-snapshots"></a>Momentopnamen verwijderen

U kunt een blob met momentopnamen niet verwijderen, tenzij de momentopnamen worden ook verwijderd. U kunt een momentopname afzonderlijk verwijderen of opgeven dat alle momentopnamen worden verwijderd wanneer de bron-blob wordt verwijderd. Als u probeert te verwijderen van een blob nog steeds met momentopnamen, wordt een fout weergegeven.

Het volgende voorbeeld ziet u hoe u verwijdert een blob en de momentopnamen in .NET waar `blockBlob` is een variabele van het type **CloudBlockBlob**:

    await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);

## <a name="snapshots-with-azure-premium-storage"></a>Momentopnamen met Azure Premium-opslag

Gebruik van momentopnamen met Premium opslag volgen deze regels:

- Het maximum aantal momentopnamen per pagina blob in een premium-opslag-account is 100. Als deze limiet is overschreden, geeft de bewerking momentopname Blob foutcode 409 (**SnapshotCountExceeded**) als resultaat.

- U kunt een momentopname van een pagina-blob uitvoeren in een account van de opslagruimte premium eenmaal 10 minuten. Als dit percentage wordt overschreden, geeft de bewerking momentopname Blob foutcode 409 (**SnaphotOperationRateExceeded**) als resultaat.

- U kunt Blob krijgen als u wilt lezen van een momentopname van een pagina blob in een premium-opslag-account niet kunt bellen. Aanroepen van Blob ophalen voor een momentopname in een account van de opslagruimte premium retourneert foutcode 400 (**InvalidOperation**). U kunt echter Blob eigenschappen en krijgen Blob metagegevens bellen ten opzichte van een momentopname in een premium-opslag-account.

- Als u wilt een momentopname lezen, kunt u de bewerking kopie Blob een momentopname kopiëren naar een andere pagina blob in het account. De blob bestemming voor de kopieerbewerking mag geen eventuele bestaande momentopnamen. Als de waarde van doel-blob wel momentopnamen, retourneert de bewerking kopie Blob foutcode 409 (**SnapshotsPresent**).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>De absolute URI terug naar een momentopname

In dit voorbeeld C#-code Hiermee maakt u een momentopname en schrijft de absolute URI voor de primaire locatie.

    //Create the blob service client object.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();

    //Get a reference to a blob.
    CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
    blob.UploadText("This is a blob.");

    //Create a snapshot of the blob and write out its primary URI.
    CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
    Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);

## <a name="understand-how-snapshots-accrue-charges"></a>Meer informatie over hoe momentopnamen kosten toenemen

Maken van een momentopname, dat wil een alleen-lezen kopie van een blob zeggen, kan resulteren in extra opslagruimte voor datakosten bij uw account. Bij het ontwerpen van uw toepassing, is het belangrijk te weten hoe deze kosten mogelijk toenemen zodat u kunt onnodige kosten minimaliseren.

### <a name="important-billing-considerations"></a>Belangrijke aandachtspunten voor de facturering

De volgende lijst bevat belangrijke punten u rekening moet houden bij het maken van een momentopname.

- U opslag account bijhoudt kosten voor unieke blokken of pagina's, ongeacht of ze in de blob of in de momentopname zijn. Uw account wordt niet in rekening gebracht extra kosten voor momentopnamen die is gekoppeld aan een blob totdat u de blob waarop ze zijn gebaseerd bijwerken. Nadat u de basis blob bijwerkt, wordt dit afwijkt van de momentopnamen. Als dit gebeurt, wordt geheven voor de unieke blokken of pagina's in elke blob of een momentopname.

- Wanneer u een tekstblok binnen een blok blob vervangt, wordt dat blok vervolgens BTW geheven als een unieke tekstblok. Dit geldt ook als de blokkering heeft dezelfde blok-ID en dezelfde gegevens, als er in de momentopname. Nadat u de blokkering is erop gebrand nogmaals dit afwijkt van de tegenhanger in een momentopname en u moet betalen voor de gegevens. Hetzelfde geldt voor een pagina in een pagina-blob die automatisch wordt bijgewerkt met identieke gegevens.

- Hiermee vervangt u een blok blob vervangen door de methode **UploadFile**, **UploadText**, **UploadStream**of **UploadByteArray** roepen alle blokken in de blob. Als er een momentopname die is gekoppeld aan die blob, alle blokken in de basis blob en een momentopname nu luidsprekers en brengt voor alle van de blokken in beide BLOB's. Dit geldt ook als de gegevens in de basis blob en de momentopname identiek.

- De service Azure Blob heeft geen een manier om te bepalen of twee blokken identieke gegevens bevatten. Elk blok dat wordt geüpload en vastgelegde wordt behandeld als unieke, zelfs als er al dezelfde gegevens en dezelfde blok-ID. Omdat de kosten toenemen voor unieke blokken, is het is belangrijk dat het bijwerken van een blob met een momentopname resultaten in blokken met extra unieke en extra kosten oplossingen doornemen.

> [AZURE.NOTE] Aanbevolen procedures dicteren dat u beheert momentopnamen zorgvuldig om extra kosten voorkomen. Het is raadzaam dat u momentopnamen in de volgende wijze beheren:

> - Verwijderen en opnieuw maken die is gekoppeld aan een blob telkens wanneer u de blob bijwerkt, zelfs als u met identieke gegevens, bijwerken wilt tenzij het toepassingsontwerp van uw is vereist dat u voor het behoud van momentopnamen momentopnamen. Door te verwijderen en opnieuw te maken van de blob momentopnamen, kunt u ervoor zorgen dat de blob en momentopnamen niet luidsprekers.

> - Als u momentopnamen voor een blob, kunt u geen **UploadFile**, **UploadText**, **UploadStream**of **UploadByteArray** als u wilt bijwerken van de blob bellen. Deze methoden Vervang alle van de blokken in de blob, zodat uw grondtal blob en momentopnamen luidsprekers aanzienlijk zijn. In plaats daarvan het kleinste aantal blokken met behulp van de methoden **PutBlock** en **PutBlockList** bijwerken


### <a name="snapshot-billing-scenarios"></a>Momentopname facturering scenario 's


De volgende scenario's demonstreren hoe kosten toenemen voor een blob blokkeren en de momentopnamen.

In scenario 1 is de basis blob niet bijgewerkt nadat de momentopname is gemaakt, zodat kosten alleen unieke blokken 1, 2 en 3 verbonden zijn.

![Azure opslag resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

In scenario 2 het grondtal blob is bijgewerkt, maar de momentopname niet heeft. Blok 3 werd bijgewerkt en hoewel dit dezelfde gegevens en dezelfde-ID bevat, is niet hetzelfde als 3 in de momentopname blokkeren. Het account wordt daardoor vier blokken belast.

![Azure opslag resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

In scenario 3, het grondtal blob is bijgewerkt, maar de momentopname niet heeft. Blok 3 is vervangen door blok 4 in de basis blob, maar de momentopname weerspiegelt nog steeds blok 3. Het account wordt daardoor vier blokken belast.

![Azure opslag resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

In scenario 4, is de basistoewijzing blob volledig is bijgewerkt en bevat geen van de oorspronkelijke blokken. Het account is daardoor btw geheven voor alle acht unieke blokken. Dit scenario kan optreden als u een update-methode zoals **UploadFile**, **UploadText**, **UploadFromStream**of **UploadByteArray**, omdat deze methoden alles van de inhoud van een blob vervangen.

![Azure opslag resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer voorbeelden blobopslag met [Voorbeelden van Azure-Code](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). U kunt downloaden van een toepassing voor de steekproef en worden uitgevoerd, of de code op GitHub bladeren. 
