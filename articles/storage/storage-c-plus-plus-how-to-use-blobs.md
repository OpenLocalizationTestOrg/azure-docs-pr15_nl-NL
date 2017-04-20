<properties
    pageTitle="Het gebruik van blobopslag (object opslag) vanuit C++ | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure-blobopslag (object opslag)."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-blob-storage-from-c"></a>Het gebruik van C++-blobopslag  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Azure-blobopslag is een service die ongestructureerde gegevens worden opgeslagen in de cloud als objecten/BLOB's. Blobopslag kunt elk gewenst type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstaller opslaan. Blobopslag is ook object opslag genoemd.

Deze handleiding laat zien hoe u veelvoorkomende scenario's voor het gebruik van de Azure Blob storage-service uitvoert. In de voorbeelden in C++ zijn geschreven en gebruikt u de [Bibliotheek van Azure opslag Client voor C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). De scenario's waarvoor zijn **geüpload**, **vermelding**, **downloaden**en BLOB's **verwijderen** .  

>[AZURE.NOTE] Deze handleiding is bedoeld voor de bibliotheek van Azure opslag-Client voor C++ versie 1.0.0 en hoger. De aanbevolen versie is opslag clientbibliotheek 2.2.0, verkrijgbaar via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp).

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Een C++-toepassing maken
In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een C++-toepassing.  

Hiervoor moet u de bibliotheek van Azure opslag-Client voor C++ installeren en een account Azure opslagruimte in uw Azure abonnement maken.   

Als u wilt de bibliotheek van Azure opslag-Client voor C++ hebt geïnstalleerd, kunt u de volgende methoden:

-   **Linux:** Volg de instructies in de [Azure opslag Client voor C++ Leesmij](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) bibliotheekpagina.  
-   **Windows:** Klik in Visual Studio, op **Hulpmiddelen > NuGet Package Manager > Package Manager-Console**. Typ de volgende opdracht in de [beheerconsole van NuGet pakket](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) en druk op **ENTER**.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Uw toepassing voor toegang tot Blob Storage configureren  
Toevoegen aan dat de volgende beweringen naar het begin van het C++-bestand waarop u gebruiken van de Azure opslag API's wilt voor toegang tot BLOB's opnemen:  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag instellen
Een Azure opslag-client gebruikt een verbindingsreeks opslag voor de opslag van de eindpunten en referenties voor toegang tot data management-services. Wanneer u zich in een clienttoepassing, moet u de verbindingsreeks van opslag in de volgende indeling opgeven met behulp van de naam van uw opslag-account en de opslag access-toets voor de opslag-account wordt vermeld in de [Portal van Azure](https://portal.azure.com) voor de waarden *accountnaam* en *AccountKey* aan te geven. Zie voor informatie over de opslag accounts en toegangstoetsen, [Over Azure opslag Accounts](storage-create-storage-account.md). In dit voorbeeld ziet u hoe u een statische veld houdt u de verbindingsreeks kunt declareren:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Als u wilt testen van de toepassing in uw lokale Windows-computer, kunt u de Microsoft Azure- [opslag emulator](storage-use-emulator.md) die is geïnstalleerd met de [SDK van Azure](https://azure.microsoft.com/downloads/). De emulator opslag is een hulpprogramma waarmee de Blob, wachtrij en tabel services die beschikbaar zijn in Azure op uw computer plaatselijke ontwikkeling simuleert. Het volgende voorbeeld ziet hoe u een statische veld houdt u de verbindingsreeks naar uw lokale opslag emulator kunt declareren:

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Als u wilt de emulator Azure opslag, selecteert u de knop **Start** of druk op de **Windows** -toets. Begin te typen **Azure opslag Emulator**en selecteer **Microsoft Azure opslag Emulator** in de lijst met toepassingen.  

In de volgende voorbeelden wordt ervan uitgegaan dat u een van de volgende twee manieren hebt gebruikt om de opslag-verbindingsreeks.  

## <a name="retrieve-your-connection-string"></a>De verbindingsreeks ophalen
U kunt de klasse **cloud_storage_account** om aan te geven van uw accountgegevens opslag. Als u wilt uw accountgegevens opslag opgehaald uit de verbindingsreeks opslag, kunt u de methode **parseren** gebruiken.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Vervolgens krijgt u een verwijzing naar een klasse **cloud_blob_client** omdat u objecten die staan voor containers en BLOB's die zijn opgeslagen in de Blob Storage-Service ophalen. De volgende code maakt een **cloud_blob_client** -object met behulp van de opslagruimte account-object die wordt opgehaald hierboven:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>Hoe u: een container maken

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

In dit voorbeeld ziet u hoe u kunt een container maken, als deze nog niet bestaat:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Standaard de nieuwe container privé is en moet u uw opslagruimte toegangstoets wilt BLOB's downloaden van deze container. Als u de bestanden (BLOB's) in de container beschikbaar maken voor iedereen wilt, kunt u de container als public met de volgende code instellen:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Iedereen op Internet BLOB's in een openbare container kunt zien, maar u kunt wijzigen of ze alleen verwijderen als u de juiste toegangstoets hebt.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Hoe u: een blob uploaden naar een container
Azure-blobopslag ondersteunt blok BLOB's en pagina BLOB's. In de meeste gevallen is het blok blob het aanbevolen dat u wilt gebruiken.  

Als u wilt een bestand uploaden naar een blob blokkeren, een overzicht van de container ophalen en deze gebruiken om een overzicht van de blob blokkeren. Nadat u een verwijzing blob hebt, kunt u een reeks gegevens uploaden naar deze door de methode **upload_from_stream** roepen. Deze bewerking maakt de blob als deze niet eerder bestaat of overschrijven als deze nog bestaat. Het volgende voorbeeld ziet u hoe u een blob uploaden naar een container en wordt ervan uitgegaan dat de container al is gemaakt.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

U kunt ook kunt u de methode **upload_from_file** naar een bestand uploaden naar een blob blokkeren.

## <a name="how-to-list-the-blobs-in-a-container"></a>Hoe u: een lijst met de BLOB's in een container
U kunt de BLOB's in een container, moet u eerst een overzicht van de container krijgen. U kunt vervolgens van de container **list_blobs** methode gebruiken om op te halen de BLOB's en/of mappen erin. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde **list_blob_item**, moet u de methode **list_blob_item.as_blob** om een **cloud_blob** -object of de methode **list_blob.as_directory** om een object cloud_blob_directory bellen. De volgende code wordt getoond hoe ophalen en uitvoer van de URI van elk item in de container **Mijn-steekproef-container** :

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

Zie voor meer informatie over het weergeven van bewerkingen [Lijst Azure opslag Resources in C++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Hoe u: BLOB's downloaden
BLOB's downloaden, eerst een verwijzing blob ophalen en vervolgens de methode **download_to_stream** aanroepen. Het volgende voorbeeld wordt de methode **download_to_stream** aan de inhoud blob overbrengen naar een stream-object waarvan u kunt vervolgens naar een lokaal bestand.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

U kunt ook kunt u de methode **download_to_file** aan de inhoud van een blob naar een bestand downloaden.
Bovendien kunt u ook de methode **download_text** gebruiken om te downloaden van de inhoud van een blob als een tekenreeks.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>Hoe u: BLOB's verwijderen
U verwijdert een blob, eerst u blob verwijst en vervolgens de methode **delete_blob** bellen op is geïnstalleerd.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Volgende stappen
U kunt de basisbeginselen van blobopslag hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure opslag.  

-   [Het gebruik van de wachtrij opslag van C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Het gebruik van Table Storage vanuit C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Lijst met Azure opslag Resources in C++](storage-c-plus-plus-enumeration.md)
-   [Opslag clientbibliotheek voor C++ verwijzing](http://azure.github.io/azure-storage-cpp)
-   [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
- [Gegevens met het hulpprogramma voor de opdrachtregel AzCopy overbrengen](storage-use-azcopy.md)
