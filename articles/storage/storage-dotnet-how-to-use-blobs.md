<properties
    pageTitle="Aan de slag met Azure-blobopslag (object opslag) met behulp van .NET | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure-blobopslag (object opslag)."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Aan de slag met Azure-blobopslag met .NET

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Azure-blobopslag is een service die ongestructureerde gegevens worden opgeslagen in de cloud als objecten/BLOB's. -Blobopslag kunt elk gewenst type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstaller opslaan. Blobopslag is ook object opslag genoemd.

### <a name="about-this-tutorial"></a>Over deze zelfstudie

Deze zelfstudie wordt getoond hoe u schrijft .NET-code voor enkele veelvoorkomende scenario's met Azure-blobopslag. Scenario's waarvoor zijn geüpload, vermelding, downloaden en BLOB's verwijderen.

**Geschatte tijd in beslag:** 45 minuten

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure opslag clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Configuration Manager voor .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Een [account van Azure opslag](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Meer voorbeelden

Zie [Aan de slag met Azure-blobopslag in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)voor aanvullende voorbeelden met blobopslag. U kunt downloaden van de steekproef-toepassing en worden uitgevoerd, of de code op GitHub bladeren.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen

Voeg de volgende `using` instructies naar het begin van de `program.cs` bestand:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>Parseren van de verbindingsreeks

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>De client van de service Blob maken

De klasse **CloudBlobClient** kunt ophalen containers en BLOB's die zijn opgeslagen in blobopslag. Hier volgt een manier om u te maken van de serviceclient:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

U bent nu klaar om te schrijven code die gegevens ophaalt uit en schrijft gegevens naar Blob storage.

## <a name="create-a-container"></a>Een container maken

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

In dit voorbeeld ziet u hoe u kunt een container maken, als deze nog niet bestaat:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Standaard is de nieuwe container privé, wat betekent dat u uw opslagruimte toegangstoets wilt BLOB's downloaden van deze container moet opgeven. Als u de bestanden in de container beschikbaar maken voor iedereen wilt, kunt u de container als public met de volgende code instellen:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Iedereen op Internet BLOB's in een openbare container kunt zien, maar u kunt wijzigen of ze alleen verwijderen als u het juiste account toegangstoets of een handtekening gedeelde toegang hebt.

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Azure-blobopslag ondersteunt blok BLOB's en pagina BLOB's.  In de meeste gevallen is blok blob het aanbevolen dat u wilt gebruiken.

Als u wilt een bestand uploaden naar een blob blokkeren, een overzicht van de container ophalen en deze gebruiken om een overzicht van de blob blokkeren. Nadat u een verwijzing blob hebt, kunt u een reeks gegevens uploaden naar deze door de methode **UploadFromStream** roepen. Deze bewerking maakt de blob als deze niet eerder bestaat of overschrijven als deze nog bestaat.

Het volgende voorbeeld ziet u hoe u een blob uploaden naar een container en wordt ervan uitgegaan dat de container al is gemaakt.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

U kunt de BLOB's in een container, moet u eerst een overzicht van de container krijgen. U kunt vervolgens van de container **ListBlobs** methode gebruiken om op te halen de BLOB's en/of mappen erin. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde **IListBlobItem**, moet u deze naar een object **CloudBlockBlob**, **CloudPageBlob**of **CloudBlobDirectory** geconverteerd.  Als het type onbekend is, kunt u een selectievakje type om te bepalen welke deze naar naar.  De volgende code ziet kunt ophalen en uitvoer van de URI van elk item in de `photos` container:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Zoals hierboven, kunt u de naam van BLOB's met informatie over het pad in hun namen toewijzen. Hiermee maakt u de structuur van een virtuele map die u kunt organiseren en bladeren, zoals u zou doen met een traditionele bestandssysteem. Houd er rekening mee dat de mapstructuur virtual alleen is-de alleen beschikbaar in blobopslag resources containers en BLOB's zijn. De bibliotheek van de client opslagruimte biedt echter een object **CloudBlobDirectory** om te verwijzen naar een virtuele map en het proces van het werken met BLOB's die zijn gerangschikt op deze manier vereenvoudigen.

Bekijk bijvoorbeeld de volgende set blok BLOB's in een container met de naam `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Wanneer u belt **ListBlobs** op de container 'foto's ' (bijvoorbeeld het bovenstaande voorbeeld), wordt een hiërarchische lijst als resultaat gegeven. Deze bevat zowel **CloudBlobDirectory** als **CloudBlockBlob** objecten, de mappen en BLOB's in de container, respectievelijk. De resulterende uitvoer ziet er zo:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Desgewenst kunt u de parameter **UseFlatBlobListing** van van de methode **ListBlobs** instellen op **true**. In dit geval elke blob in de container geretourneerd als een object **CloudBlockBlob** . De oproep door naar **ListBlobs** om terug te keren een platte vermelding ziet er zo uit:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

en de resultaten er als volgt uit:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>BLOB's downloaden

BLOB's downloaden, eerst een verwijzing blob ophalen en vervolgens de methode **DownloadToStream** aanroepen. Het volgende voorbeeld wordt de methode **DownloadToStream** aan de inhoud blob overbrengen naar een stream-object waarvan u kunt vervolgens naar een lokaal bestand.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

U kunt ook de **DownloadToStream** -methode gebruiken om te downloaden van de inhoud van een blob als een tekenreeks.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>BLOB's verwijderen

U verwijdert een blob, eerst u blob verwijst en vervolgens de methode **Delete** bellen op is geïnstalleerd.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asynchroon BLOB's op pagina's weergeven

Als u een groot aantal BLOB's zijn vermelding of om te bepalen van het aantal resultaten die u hiernaar één vermelding betrekking heeft terugkeert, kunt u BLOB's op pagina's met resultaten weergeven. In dit voorbeeld ziet u hoe om resultaten te retourneren op pagina's asynchroon, zodat de uitvoering niet is geblokkeerd terwijl u wacht op een grote verzameling resultaten retourneren.

In dit voorbeeld ziet u een platte blob vermelding, maar u kunt ook een hiërarchische lijst uitvoeren door in te stellen de `useFlatBlobListing` parameter van de methode **ListBlobsSegmentedAsync** `false`.

Omdat de methode voorbeeld een asynchrone methode roept, moet u deze voorafgegaan door de `async` trefwoord, waarna ze moet retourneren een **taak** . Het trefwoord await is opgegeven voor de **ListBlobsSegmentedAsync** de uitvoering van de steekproef methode onderbreekt totdat de aanbieding taak is voltooid.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="writing-to-an-append-blob"></a>Schrijven naar een blob toevoegen

Een toevoegquery blob is een nieuw type blob, kennis met versie 5.x van de bibliotheek van de client Azure opslag voor .NET. Een toevoegquery blob is geoptimaliseerd voor het toevoegen van gegevens, zoals logboekregistratie. Een toevoegquery blob bestaat uit blokken zoals een blob blok, maar wanneer u een nieuw blok aan een toevoegquery blob toevoegt, altijd wordt toegevoegd aan het einde van de blob. U kunt geen bijwerken of verwijderen van een bestaand notitieblok in een blob toevoegen. De blokkering van id's voor een toevoegquery blob niet beschikbaar zijn als voor een blob blokkeren.

Elk blok in een toevoegquery blob kan een andere grootte, tot maximaal 4 MB aan en een toevoegquery blob maximaal 50.000 blokken kunt opnemen. De maximale grootte van een toevoegquery blob daarom iets meer dan 195 GB (4 MB X 50.000 geblokkeerd).

In het onderstaande voorbeeld Hiermee maakt u een nieuwe toevoegen blob en sommige gegevens worden toegevoegd aan, die zijn simuleren van een eenvoudige logboekregistratie-bewerking.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Zie [lidmaatschap blok BLOB's, pagina BLOB's, en BLOB's toevoegen](https://msdn.microsoft.com/library/azure/ee691964.aspx) voor meer informatie over de verschillen tussen de drie typen BLOB's.

## <a name="managing-security-for-blobs"></a>Beveiliging voor BLOB's beheren

Standaard beveiligt Azure Storage uw gegevens door het beperken van toegang tot de accounteigenaar, die in het bezit van de toegangstoetsen account. Wanneer u delen blob-gegevens in uw account opslag moet, is het is belangrijk dat te doen zonder de beveiliging van uw account toegangstoetsen. Bovendien kunt u blob-gegevens om ervoor te zorgen dat deze is beveiligd gaan via het netwerk en in Azure Storage coderen.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Toegang tot blob-gegevens beheren

Standaard is de blob-gegevens in uw account opslag toegankelijk alleen voor accounteigenaar opslag. Verificatie aanvragen tegen blobopslag, is de toegangstoets account al dan niet standaard vereist. Echter, kunt u bepaalde blob-gegevens beschikbaar stellen aan andere gebruikers. U hebt twee opties:

- **Anonieme toegang:** U kunt een container of de BLOB's openbaar beschikbaar maken voor anonieme toegang. Zie [anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md) voor meer informatie.
- **Gedeeld access handtekeningen:** U kunt clients voorzien van een gedeelde access-handtekening (SA's), waarmee gedelegeerd toegang tot een bron in uw opslag-account met machtigingen die u opgeeft en na verloop van een interval dat u opgeeft. Zie [Gebruik van gedeelde Access handtekeningen (SA's)](storage-dotnet-shared-access-signature-part-1.md) voor meer informatie.

### <a name="encrypting-blob-data"></a>Coderen van blob-gegevens

Azure opslag ondersteunt coderen blob-gegevens op de client en op de server:

- **Aan de clientzijde versleuteling:** De bibliotheek van de Client opslag voor .NET ondersteunt coderen van gegevens in clienttoepassingen voordat uploaden naar Azure Storage en ontsleutelen van gegevens bij het downloaden van de client. De bibliotheek ondersteunt ook integratie met Azure-toets kluis voor belangrijke opslagbeheer-account. Zie [Aan de clientzijde codering met .NET voor Microsoft Azure opslag](storage-client-side-encryption.md) voor meer informatie. Zie ook [Zelfstudie: versleutelen en ontsleutelen BLOB's in Microsoft Azure Storage met Azure-toets kluis](storage-encrypt-decrypt-blobs-key-vault.md).
- **Aan de clientzijde versleuteling**: Azure opslag ondersteunt nu aan de clientzijde versleuteling. Zie [Azure opslag Service codering voor gegevens in rust (Preview)](storage-service-encryption.md).

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Blob storage hebt geleerd, volgt u deze koppelingen voor meer informatie.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure opslag Explorer
- [Microsoft Azure opslag Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige-app van Microsoft waarmee u kunt visueel werken met gegevens van de Azure-opslag in Windows, OS X en Linux.

### <a name="blob-storage-samples"></a>Voorbeelden van BLOB storage

- [Aan de slag met Azure-blobopslag in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>BLOB storage verwijzing

- [Opslag clientbibliotheek voor .NET-verwijzing](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [Overzicht van de REST API](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Conceptuele hulplijnen

- [Gegevens met het hulpprogramma voor de opdrachtregel AzCopy overbrengen](storage-use-azcopy.md)
- [Aan de slag met bestandsopslag voor .NET](storage-dotnet-how-to-use-files.md)
- [Hoe u met de WebJobs SDK Azure-blobopslag](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
