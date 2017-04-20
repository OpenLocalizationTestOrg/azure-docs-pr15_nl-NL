<properties
    pageTitle="Aan de slag met blob storage en Visual Studio verbonden services (cloudservices) | Microsoft Azure"
    description="Hoe u aan de slag met Azure-blobopslag in een project cloud-service in Visual Studio nadat de verbinding met een opslag-account gebruik van Visual Studio services verbonden"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Aan de slag met Azure-blobopslag en Visual Studio verbonden services (cloud services projecten)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe aan de slag met Azure-blobopslag nadat u hebt gemaakt of een account Azure Storage waarnaar wordt verwezen met behulp van het dialoogvenster Visual Studio, **Verbonden Services toevoegen** in een Visual Studio cloud services-project. Leert u hoe u toegang tot en blob containers maken en hoe u algemene taken zoals uploaden, weergeven en downloaden BLOB's uitvoeren. In de voorbeelden zijn geschreven in C\# en gebruikt u de [Bibliotheek van Microsoft Azure opslag Client voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Azure-blobopslag is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens die zijn toegankelijk vanaf een willekeurige plaats in de wereld via HTTP of HTTPS. Een enkel blob kan elke grootte zijn. BLOB's kunnen items, zoals afbeeldingen, audio-en videobestanden, onbewerkte gegevens en documentbestanden zijn.

Net zoals bestanden live in mappen, live-opslag BLOB's in containers. Nadat u een opslagruimte hebt gemaakt, kunt u een of meer containers maken in de opslagruimte. Bijvoorbeeld in een opslag 'Knipselmap' genoemd, kunt u containers maken in de opslagruimte genoemd 'afbeeldingen' om op te slaan, afbeeldingen en andere "audio" om op te slaan audiobestanden genoemd. Nadat u de containers hebt gemaakt, kunt u afzonderlijke blob bestanden uploaden naar deze.

- Zie [aan de slag met Azure-blobopslag met .NET](storage-dotnet-how-to-use-blobs.md)voor meer informatie over het bewerken van programmacode BLOB's.
- Zie [opslagruimte documentatie](https://azure.microsoft.com/documentation/services/storage/)voor algemene informatie over Azure opslag.
- Zie de [documentatie Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/)voor algemene informatie over Azure Cloud Services.
- Zie voor meer informatie over het programmeren ASP.NET-toepassingen, [ASP.NET](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Access blob containers in code

Voor toegang tot via programmacode BLOB's in de cloud serviceprojecten, moet u de volgende items toevoegen als ze niet al aanwezig.

1. De volgende code naamruimtedeclaraties toevoegen aan het begin van een C#-bestand waarin u wilt via programmacode toegang hebben tot Azure opslag.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag-account. De volgende code gebruiken om de uw opslagruimte verbindingsreeks en de accountgegevens opslag van de Azure-service te configureren.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));

3. Een object **CloudBlobClient** om te verwijzen naar een bestaande container in uw account opslag krijgen.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

4. Een object **CloudBlobContainer** om te verwijzen naar een specifieke blob container krijgen.

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Alle de code die wordt weergegeven in de vorige procedure vóór de code die wordt weergegeven in de volgende secties gebruiken.

## <a name="create-a-container-in-code"></a>Een container maken in code

> [AZURE.NOTE] Sommige API's die oproepen af met Azure Storage in ASP.NET uitvoert zijn asynchroon. Zie [asynchroon asynchrone en Await programmeren](http://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De code in het volgende voorbeeld wordt ervan uitgegaan dat u asynchrone programming methoden worden gebruikt.

Als u wilt een container maken in uw account opslag, enige u moet doen, is een oproep toevoegen aan **CreateIfNotExistsAsync** zoals in de volgende code:

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


Als u wilt de bestanden in de container beschikbaar maken voor iedereen, kunt u de container als public met behulp van de volgende code instellen.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Iedereen op Internet BLOB's in een openbare container kunt zien, maar u kunt wijzigen of ze alleen verwijderen als u de juiste toegangstoets hebt.

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Azure opslag ondersteunt blok BLOB's en pagina BLOB's. In de meeste gevallen is het blok blob het aanbevolen dat u wilt gebruiken.

Als u wilt een bestand uploaden naar een blob blokkeren, een overzicht van de container ophalen en deze gebruiken om een overzicht van de blob blokkeren. Nadat u een verwijzing blob hebt, kunt u een reeks gegevens uploaden naar deze door de methode **UploadFromStream** roepen. Hiermee maakt u de blob als deze niet eerder voorkomt, of het overschreven als deze nog bestaat. Het volgende voorbeeld ziet u hoe u een blob uploaden naar een container en wordt ervan uitgegaan dat de container al is gemaakt.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

U kunt de BLOB's in een container, moet u eerst een overzicht van de container krijgen. U kunt vervolgens van de container **ListBlobs** methode gebruiken om op te halen de BLOB's en/of mappen erin. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde **IListBlobItem**, moet u deze naar een object **CloudBlockBlob**, **CloudPageBlob**of **CloudBlobDirectory** geconverteerd. Als het type onbekend is, kunt u een selectievakje type om te bepalen welke deze naar naar. De volgende code wordt getoond hoe ophalen en de URI van elk item in de container **foto's** weergeven:

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

Zoals u ziet in het vorige voorbeeld, heeft de service blob het concept van mappen binnen containers, ook. Dit is zodat u uw BLOB's in een meer map-achtige structuur kunt indelen. Houd rekening met de volgende set blok BLOB's in een container met de naam **foto's**, bijvoorbeeld:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Wanneer u belt **ListBlobs** op de container (bijvoorbeeld het vorige voorbeeld), bevat de geretourneerde collectie **CloudBlobDirectory** en **CloudBlockBlob** objecten die de mappen en BLOB's opgenomen op het hoogste niveau. Hier ziet u de resulterende uitvoer:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Desgewenst kunt u de parameter **UseFlatBlobListing** van van de methode **ListBlobs** instellen op **true**. Hierdoor elke blob wordt geretourneerd als een **CloudBlockBlob**, ongeacht de adreslijst. Hier ziet de oproep door naar **ListBlobs**:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

en hier vindt u de resultaten:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Zie [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx)voor meer informatie.

## <a name="download-blobs"></a>BLOB's downloaden

BLOB's downloaden, eerst een verwijzing blob ophalen en vervolgens de methode **DownloadToStream** aanroepen. Het volgende voorbeeld wordt de methode **DownloadToStream** aan de inhoud blob overbrengen naar een stream-object waarvan u kunt vervolgens naar een lokaal bestand.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

U kunt ook de **DownloadToStream** -methode gebruiken om te downloaden van de inhoud van een blob als een tekenreeks.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>BLOB's verwijderen

Als u wilt een blob verwijderen, eerst u blob verwijst en klikt u vervolgens de methode **verwijderen** .

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asynchroon BLOB's op pagina's weergeven

Als u een groot aantal BLOB's zijn vermelding of om te bepalen van het aantal resultaten die u hiernaar één vermelding betrekking heeft terugkeert, kunt u BLOB's op pagina's met resultaten weergeven. In dit voorbeeld ziet u hoe om resultaten te retourneren op pagina's asynchroon, zodat de uitvoering niet is geblokkeerd terwijl u wacht op een grote verzameling resultaten retourneren.

In dit voorbeeld ziet u een platte blob vermelding, maar u kunt ook een hiërarchische lijst uitvoeren door de **useFlatBlobListing** -parameter van de methode **ListBlobsSegmentedAsync** in **Onwaar**.

Omdat de methode voorbeeld een asynchrone methode roept, deze moet worden voorafgegaan door het trefwoord **asynchrone** en deze een **taak** moet retourneren. Het trefwoord await is opgegeven voor de **ListBlobsSegmentedAsync** de uitvoering van de steekproef methode onderbreekt totdat de aanbieding taak is voltooid.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
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

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
