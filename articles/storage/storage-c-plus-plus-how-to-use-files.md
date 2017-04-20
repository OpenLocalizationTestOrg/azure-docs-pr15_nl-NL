<properties
    pageTitle="Het gebruik van bestandsopslag vanuit C++ | Microsoft Azure"
    description="Gegevens uit een bestand opslaan in de cloud met Azure bestandsopslag."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>Het gebruik van bestandsopslag vanuit C++

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>Over deze zelfstudie

In deze zelfstudie leert u hoe u eenvoudige bewerkingen uitvoeren op de storage-service van Microsoft Azure-bestand. Via steekproeven geschreven in C++, leert u hoe u de waarden voor aandelen en mappen maken, uploaden, weergeven en bestanden verwijderen. Als u nog niet eerder met Microsoft Azure van bestand Storage-service, zijn via de concepten in de volgende gedeelten heel handig zijn de voorbeelden begrijpen.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Een C++-toepassing maken

Als u wilt maken in de voorbeelden, moet u bij het installeren van de Azure opslag clientbibliotheek 2.4.0 voor C++. U moet ook een account hebt gemaakt Azure opslag.

Als u wilt de Azure-opslag-Client 2.4.0 voor C++ hebt geïnstalleerd, kunt u een van de volgende methoden gebruiken:

-   **Linux:** Volg de instructies in de [Azure opslag Client voor C++ Leesmij](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) bibliotheekpagina.

-   **Windows:** Klik in Visual Studio, op **Hulpmiddelen voor &gt; NuGet Package Manager &gt; Package Manager Console**. Typ de volgende opdracht in de [beheerconsole van NuGet pakket](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) en druk op **ENTER**.

    Installatie-pakket wastorage

## <a name="set-up-your-application-to-use-file-storage"></a>Uw toepassing bestandsopslag instellen

Toevoegen aan dat de volgende beweringen naar het begin van het C++-bestand waarop u gebruiken van de Azure opslag API's wilt voor toegang tot bestanden opnemen:

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag instellen

Als u wilt gebruiken bestandsopslag, moet u verbinding maken met uw account Azure opslag. De eerste stap is voor het configureren van een verbindingsreeks die we gebruiken om verbinding met uw account opslag. Laten we definiëren een statische variabele om dat te doen.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Verbinding maken met een account Azure opslag

U kunt de klasse **cloud_storage_account** om aan te geven van uw accountgegevens opslag. Als u wilt uw accountgegevens opslag opgehaald uit de verbindingsreeks opslag, kunt u de methode **parseren** gebruiken.

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>Hoe u: een gedeelde map maken

Alle bestanden en mappen in bestandsopslag bevinden zich in een zogenaamd een **delen**. Uw account opslag kunt zo veel waarden voor aandelen als uw account capaciteit kan hebben. Als u toegang tot een delen en de inhoud ervan, moet u een bestand opslag-client gebruiken.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

Met de client van de opslagruimte bestand, kunt u vervolgens verkrijgen een verwijzing naar een delen.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

Als u wilt maken de optie voor delen, gebruikt u de methode **create_if_not_exists** van het object **cloud_file_share** .

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

Op dit moment bevat **delen** een verwijzing naar een delen met de naam **Mijn-steekproef-delen**.

## <a name="how-to-upload-a-file"></a>Hoe u: een bestand uploaden

Op ieder geval bevat een opslag van Azure bestandsshare een hoofdmap waar de bestanden kunnen zich bevinden. In dit gedeelte leert u hoe u een bestand vanaf een lokale opslag naar de hoofdmap van een delen uploaden.

De eerste stap bij het uploaden van een bestand is verkrijgen van een verwijzing naar de map waarin deze zich moet bevinden. U doen dit door de ondersteuning voor de methode **get_root_directory_reference** van het object delen.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Nu dat u een verwijzing naar de hoofdmap van de optie voor delen hebt, kunt u een bestand op deze kunt uploaden. In dit voorbeeld uploadt uit een bestand, van tekst en vanuit een stroom.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>Hoe u: een map maken

U kunt ook opslag indelen door te plaatsen van bestanden in submappen in plaats van dat alle labels in de hoofdmap. De Azure bestand storage-service kunt u zoveel mappen maken als uw account wordt toestaan. De onderstaande code wordt de map **Mijn-steekproef-directory** onder de hoofdmap, evenals de naam van **Mijn-steekproef-submap**een submap maken.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>Hoe u: een lijst met bestanden en mappen in een delen

Het verkrijgen van een lijst met bestanden en mappen binnen een delen is eenvoudig klaar door de ondersteuning voor **list_files_and_directories** in een verwijzing **cloud_file_directory** . Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde **list_file_and_directory_item**, moet u de methode **list_file_and_directory_item.as_file** om een **cloud_file** -object of de methode **list_file_and_directory_item.as_directory** om een object **cloud_file_directory** bellen.

De volgende code ziet kunt ophalen en uitvoer van de URI van elk item in de hoofdmap van de optie voor delen.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Hoe u: een bestand downloaden

Downloaden van bestanden, eerst ophalen van een verwijzing naar een bestand en vervolgens de methode **download_to_stream** om de inhoud van het bestand overbrengen naar een stream-object die u vervolgens naar een lokaal bestand kunt overblijven aanroepen. U kunt ook de methode **download_to_file** gebruiken om te downloaden van de inhoud van een bestand naar een lokaal bestand. U kunt de methode **download_text** gebruiken om te downloaden van de inhoud van een bestand als een tekenreeks.

Het volgende voorbeeld wordt de methoden **download_to_stream** en **download_text** om te laten zien downloaden van de bestanden die zijn gemaakt in vorige gedeelten.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>Hoe u: een bestand verwijderen

Een andere algemene opslagbewerking voor het bestand is bestand verwijderen. De volgende code Hiermee verwijdert u een bestand met de naam Mijn-steekproef-bestand-3 opgeslagen onder de hoofdmap.

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>Hoe u: een map verwijderen

Als u een map is een eenvoudige taak, hoewel het dat u een map waarin nog steeds bestanden of andere mappen niet verwijderen.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>Hoe u: een Share verwijderen

Verwijderen van een delen wordt uitgevoerd door de methode **delete_if_exists** bellen op een object cloud_file_share. Hier ziet u een voorbeeld van een code die die doet.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>De maximale grootte voor een bestand delen

U kunt het quotum (of de maximale grootte) instellen voor een bestandsshare in gigabytes. U kunt ook controleren om te zien hoeveel gegevens momenteel is opgeslagen op de optie voor delen.

Het quotum voor een delen instelt, kunt u de totale grootte van de bestanden die zijn opgeslagen op de optie voor delen beperken. Als de totale grootte van bestanden op de optie voor delen groter is dan het quotum instellen op de optie voor delen, klikt u vervolgens kunnen clients geen vergroot het formaat van bestaande bestanden of maak nieuwe bestanden, tenzij deze bestanden leeg zijn.

Het volgende voorbeeld ziet u het controleren van de huidige gebruik voor een delen en het instellen van het quotum voor de optie voor delen.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Een handtekening gedeelde toegang voor een bestand of bestandsshare genereren

U kunt een gedeelde toegang handtekening (SA's) voor een bestandsshare of voor een afzonderlijk bestand genereren. U kunt ook een gedeelde-beleid maken op een bestandsshare voor het beheren van gedeelde toegang handtekeningen. Een gedeelde-beleid maken wordt aanbevolen, omdat dit kunt u de SA's intrekken als dit niet moet worden betrouwbaar.

Het volgende voorbeeld wordt gemaakt van een gedeelde-beleid op een delen en vervolgens wordt gebruikt dat beleid op te geven van de beperkingen voor een SA's op een bestand in de optie voor delen.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Zie [Gebruik van gedeelde Access handtekeningen (SA's)](storage-dotnet-shared-access-signature-part-1.md)voor meer informatie over het maken en handtekeningen voor gedeelde access gebruiken.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de opslag van Azure, bekijk het volgende materiaal:

-   [Bibliotheek voor opslag-Client voor C++](https://github.com/Azure/azure-storage-cpp)

-   [Azure opslag Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
