<properties
    pageTitle="Aan de slag met blob storage en Visual Studio verbonden services (ASP.NET-5) | Microsoft Azure"
    description="Hoe u aan de slag met Azure-blobopslag in een project Visual Studio ASP.NET 5 nadat u een opslag-account met behulp van Visual Studio verbonden services hebt gemaakt"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Aan de slag met Azure Blob storage en Visual Studio verbonden services (ASP.NET-5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe aan de slag met Azure-blobopslag in Visual Studio nadat u hebt gemaakt of een account Azure opslag in een ASP.NET-5-project waarnaar wordt verwezen in het dialoogvenster Visual Studio toevoegen verbonden Services.

Azure-blobopslag is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens die zijn toegankelijk vanaf een willekeurige plaats in de wereld via HTTP of HTTPS. Een enkel blob kan elke grootte zijn. BLOB's kunnen items, zoals afbeeldingen, audio-en videobestanden, onbewerkte gegevens en documentbestanden zijn. In dit artikel wordt beschreven hoe u kunt beginnen met blobopslag nadat u een Azure opslag-account hebt gemaakt met behulp van het dialoogvenster Visual Studio, **Verbonden Services toevoegen** in een ASP.NET-5-project.

Net zoals bestanden live in mappen, live-opslag BLOB's in containers. Nadat u een opslagruimte hebt gemaakt, kunt u een of meer containers maken in de opslagruimte. Bijvoorbeeld in een opslag 'Knipselmap' genoemd, kunt u containers maken in de opslagruimte genoemd 'afbeeldingen' om op te slaan, afbeeldingen en andere "audio" om op te slaan audiobestanden genoemd. Nadat u de containers hebt gemaakt, kunt u afzonderlijke blob bestanden uploaden naar deze. Zie [aan de slag met Azure-blobopslag met .NET](storage-dotnet-how-to-use-blobs.md) voor meer informatie over het bewerken van programmacode BLOB's.

##<a name="access-blob-containers-in-code"></a>Access blob containers in code

Voor toegang tot via programmacode BLOB's in ASP.NET-5-projecten, moet u de volgende items toevoegen als ze niet al aanwezig.

1. De volgende code naamruimtedeclaraties naar het begin van een C#-bestand waarin u via programmacode toegang hebben tot Azure opslag wilt toevoegen.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag-account. De volgende code gebruiken om de uw opslagruimte verbindingsreeks en de accountgegevens opslag van de Azure-service te configureren.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **NOTITIE:** Gebruik alle bovenstaande code vóór de code in de volgende secties.


3. Een object **CloudBlobClient** gebruiken om een **CloudBlobContainer** verwijzing naar een bestaande container in uw account opslag.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>Een container maken in code

U kunt ook de **CloudBlobClient** gebruiken om te maken van een container in uw account opslag. U hoeft te is een oproep toevoegen aan **CreateIfNotExistsAsync** zoals in de volgende code:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**NOTITIE:** De API's die het uitvoeren van oproepen naar Azure opslagruimte in ASP.NET-5 zijn asynchroon. Zie [asynchroon asynchrone en Await programmeren](http://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De onderstaande code wordt ervan uitgegaan asynchrone programming methoden worden gebruikt.

Als u wilt de bestanden in de container beschikbaar maken voor iedereen, kunt u de container als public met behulp van de volgende code instellen.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Als u wilt een blob-bestand uploaden naar een container, krijgen een overzicht van de container en ermee blob verwijst. Nadat u een verwijzing blob hebt, kunt u een reeks gegevens uploaden naar deze door de methode **UploadFromStreamAsync** roepen. Hiermee maakt u de blob als deze nog niet aanwezig, of het overschreven als deze nog bestaat. Het volgende voorbeeld ziet u hoe u een blob uploaden naar een container en wordt ervan uitgegaan dat de container al is gemaakt.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container
U kunt de BLOB's in een container, moet u eerst een overzicht van de container krijgen. U kunt vervolgens van de container **ListBlobsSegmentedAsync** methode voor het ophalen van de BLOB's en/of mappen erin bellen. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde **IListBlobItem**, moet u deze naar een object **CloudBlockBlob**, **CloudPageBlob**of **CloudBlobDirectory** geconverteerd. Als u het type blob niet weet, kunt u een selectievakje type gebruiken om te bepalen welke deze naar naar. De volgende code ziet kunt ophalen en uitvoer van de URI van elk item in een container.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

Er zijn andere manieren voor een overzicht van de inhoud van een container blob. Zie [aan de slag met Azure-blobopslag met .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) voor meer informatie.

##<a name="download-a-blob"></a>Een blob downloaden
Als u wilt downloaden van een blob, bent u eerst een verwijzing naar de blob en vervolgens de methode **DownloadToStreamAsync** aanroepen. Het volgende voorbeeld wordt de methode **DownloadToStreamAsync** aan de inhoud blob overbrengen naar een stream-object dat u kunt vervolgens opslaan als een lokaal bestand.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Er zijn andere manieren BLOB's opslaan als bestanden. Zie [aan de slag met Azure-blobopslag met .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) voor meer informatie.

##<a name="delete-a-blob"></a>Een blob verwijderen
Als u wilt verwijderen van een blob, bent u eerst een verwijzing naar de blob en vervolgens de methode **DeleteAsync** bellen op is geïnstalleerd.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
