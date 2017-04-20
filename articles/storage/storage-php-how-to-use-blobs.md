<properties
    pageTitle="Het gebruik van blobopslag (object opslag) uit PHP | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure-blobopslag (object opslag)."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>Het gebruik van PHP-blobopslag

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Azure-blobopslag is een service die ongestructureerde gegevens worden opgeslagen in de cloud als objecten/BLOB's. Blobopslag kunt elk gewenst type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstaller opslaan. Blobopslag is ook object opslag genoemd.

Deze handleiding ziet u hoe u veelvoorkomende scenario's met de service Azure blob uitvoert. In de voorbeelden in PHP zijn geschreven en gebruiken van de [Azure SDK voor PHP] [download]. De scenario's waarvoor zijn **geüpload**, **vermelding**, **downloaden**en BLOB's **verwijderen** . Zie voor meer informatie over BLOB's, de sectie van de [volgende stappen](#next-steps) .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken

De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de service Azure blob is de verwijzing naar klassen in de SDK Azure voor PHP uit uw code. U kunt elke extra ontwikkeling maken uw toepassing, zoals Kladblok.

In deze handleiding gebruikt u de service-functies die kunnen worden aangeroepen binnen een PHP-toepassing lokaal of in code die wordt uitgevoerd vanuit een Azure Webrol, werknemer rol of website.

## <a name="get-the-azure-client-libraries"></a>Ophalen van de Azure clientbibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Uw toepassing voor toegang tot de blob-service configureren

Als u wilt de service Azure blob API's gebruikt, moet u:

1. Een verwijzing naar de autoloader-bestand met de instructie [require_once] en
2. Overzicht van alle klassen die u kunt gebruiken.

Het volgende voorbeeld ziet u hoe u het bestand autoloader nemen en om te verwijzen naar de klasse **ServicesBuilder** .

> [AZURE.NOTE] In dit voorbeeld (en andere voorbeelden in dit artikel) wordt ervan uitgegaan dat u de bibliotheken van de Client PHP voor Azure via Composer hebt geïnstalleerd. Als u de bibliotheken handmatig hebt geïnstalleerd, moet u verwijzen naar de `WindowsAzure.php` autoloader-bestand.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


In de onderstaande voorbeelden de `require_once` instructie altijd worden weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uitvoeren wordt verwezen.

## <a name="set-up-an-azure-storage-connection"></a>Een verbinding Azure opslag instellen

Als u wilt een Azure blob service-client wordt gestart, moet u eerst een geldige verbindingsreeks hebben. De notatie voor de verbindingsreeks van de blob-service is:

Voor toegang tot een live service:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Voor toegang tot de emulator opslag:

    UseDevelopmentStorage=true


Als u wilt maken van een serviceclient Azure-, moet u de klasse **ServicesBuilder** gebruiken. U kunt:

* Doorgeven de verbindingsreeks rechtstreeks of
* Gebruik de **CloudConfigurationManager (CCM)** om te controleren van meerdere externe bronnen voor de verbindingsreeks:
    * Standaard wordt deze geleverd met ondersteuning voor één externe bron - milieu variabelen.
    * U kunt nieuwe bronnen toevoegen door de klasse **ConnectionStringSource** uitbreiden.

Voor de voorbeelden hier beschreven, wordt de verbindingsreeks rechtstreeks worden doorgegeven.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>Een container maken

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Een object **BlobRestProxy** kunt u een container blob maken met de methode **createContainer** . Wanneer u een container maakt, kunt u opties op de container instellen, maar in orde is niet vereist. (In het onderstaande voorbeeld ziet u het instellen van de container toegangsbeheerlijst () en de container metagegevens.)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Bellen **setPublicAccess (PublicAccessType::CONTAINER\_en\_BLOB's)** zorgt ervoor dat de gegevens van de container en blob toegankelijk zijn via anonieme aanvragen. Bellen **setPublicAccess(PublicAccessType::BLOBS_ONLY)** zorgt ervoor alleen blob-gegevens toegankelijk zijn via anonieme aanvragen. Zie voor meer informatie over de container ACL's, [Set container ACL (REST API)][container-acl].

Zie voor meer informatie over foutcodes Blob-service, [Blob Service foutcodes][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Als u wilt een bestand als een blob uploaden, gebruikt u de methode **BlobRestProxy -> createBlockBlob** . Hiermee maakt u de blob als deze niet bestaat, of het overschreven als dat zo is. In het onderstaande voorbeeld wordt ervan uitgegaan dat de container al is gemaakt en gebruikt [fopen] [ fopen] het bestand als een gegevensstroom te openen.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Houd er rekening mee dat het vorige voorbeeld een blob als gegevensstroom uploadt. Echter een blob kan ook worden geüpload als een tekenreeks met, bijvoorbeeld de [bestand\_krijgen\_inhoud] [ file_get_contents] functie. Klik hiertoe met behulp van het vorige voorbeeld wijzigen `$content = fopen("c:\myfile.txt", "r");` naar `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

U kunt de BLOB's in een container, gebruikt u de methode **BlobRestProxy -> listBlobs** met een **foreach** lus tot en met het resultaat. De volgende code de naam van elke blob weergegeven als u in een container en worden de URI weergegeven naar de browser.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Een blob downloaden

Als u wilt downloaden van een blob, de methode **BlobRestProxy -> getBlob** bellen en klik vervolgens de methode **getContentStream** bellen op het resulterende **GetBlobResult** -object.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Houd er rekening mee dat het bovenstaande voorbeeld een blob als een resource stream (de standaardinstelling wordt). U kunt echter de [stream\_krijgen\_inhoud] [ stream-get-contents] functie om de resulterende stream omzetten in een tekenreeks.

## <a name="delete-a-blob"></a>Een blob verwijderen

Als u wilt verwijderen van een blob, geeft u de containernaam en de naam van de blob aan **BlobRestProxy -> deleteBlob**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Een container blob verwijderen

Ten slotte, als u wilt verwijderen in een container blob, geeft u de containernaam van de aan **BlobRestProxy -> deleteContainer**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van de Azure blob-service hebt geleerd, volgt u deze koppelingen voor meer informatie over opslagtaken voor meer complexe.

- Ga naar de [opslag van Azure-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- Zie het [PHP blok blob voorbeeld](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
- Zie het [PHP pagina blob voorbeeld](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [Gegevens met het hulpprogramma AzCopy-opdrachtregel overbrengen](storage-use-azcopy.md)
 
Zie ook het [PHP Developer Center](/develop/php/)voor meer informatie.


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
