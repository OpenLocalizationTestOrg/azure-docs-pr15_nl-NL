<properties
    pageTitle="Het gebruik van bestandsopslag uit Java | Microsoft Azure"
    description="Informatie over het gebruik van de service Azure bestand uploaden, downloaden, lijst, en bestanden verwijderen. Voorbeelden geschreven in Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Het gebruik van bestandsopslag van Java

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Overzicht

In deze handleiding leert u hoe u eenvoudige bewerkingen uitvoeren op de storage-service van Microsoft Azure-bestand. Tot en met voorbeelden geschreven in Java leert u hoe u de waarden voor aandelen en mappen maken, uploaden, weergeven en bestanden verwijderen. Als u nog niet eerder met Microsoft Azure van bestand Storage-service, zijn via de concepten in de volgende gedeelten heel handig zijn de voorbeelden begrijpen.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Een Java-toepassing maken

Als u wilt maken in de voorbeelden, moet u de Java Development Kit (JDK) en de [Azure opslag SDK for Java]-[]. U moet ook een account hebt gemaakt Azure opslag.

## <a name="setup-your-application-to-use-file-storage"></a>Uw toepassing bestandsopslag instellen

Als u wilt gebruiken de Azure opslag API's, de volgende instructie toe te voegen naar het begin van het Java-bestand waarop u van plan bent voor toegang tot de storage-service uit.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag instellen

Als u wilt gebruiken bestandsopslag, moet u verbinding maken met uw account Azure opslag. De eerste stap is voor het configureren van een verbindingsreeks die we gebruiken om verbinding met uw account opslag. Laten we definiëren een statische variabele om dat te doen.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] Your_storage_account_name en your_storage_account_key vervangen door de werkelijke waarden voor uw account opslag.

## <a name="connecting-to-an-azure-storage-account"></a>Verbinding maken met een account Azure opslag

Als u wilt koppelen aan een account opslag, moet u het object **CloudStorageAccount** gebruiken een verbindingsreeks doorgeven aan de methode **parseren** .

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** genereert een InvalidKeyException, dus moet u plaatst u deze in een tekstblok proberen/variabel.

## <a name="how-to-create-a-share"></a>Hoe u: een gedeelde map maken

Alle bestanden en mappen in bestandsopslag bevinden zich in een zogenaamd een **delen**. Uw account opslag kunt zo veel waarden voor aandelen als uw account capaciteit kan hebben. Als u toegang tot een delen en de inhoud ervan, moet u een bestand opslag-client gebruiken.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

Met de client van de opslagruimte bestand, kunt u vervolgens verkrijgen een verwijzing naar een delen.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

Als u wilt dat is wel maken de optie voor delen, gebruikt u de methode **createIfNotExists** van het object CloudFileShare.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

Op dit moment bevat **delen** een verwijzing naar de share **sampleshare**.

## <a name="how-to-upload-a-file"></a>Hoe u: een bestand uploaden

Een opslag van Azure bestandsshare op ieder geval bevat een hoofdmap waar de bestanden kunnen zich bevinden. In dit gedeelte leert u hoe u een bestand vanaf een lokale opslag naar de hoofdmap van een delen uploaden.

De eerste stap bij het uploaden van een bestand is verkrijgen van een verwijzing naar de map waarin deze zich moet bevinden. U doen dit door de ondersteuning voor de methode **getRootDirectoryReference** van het object delen.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Nu dat u een verwijzing naar de hoofdmap van de optie voor delen hebt, kunt u een bestand op deze met de volgende code kunt uploaden.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>Hoe u: een map maken

U kunt ook opslag indelen door te plaatsen van bestanden in submappen in plaats van dat alle labels in de hoofdmap. De Azure bestand storage-service kunt u zoveel mappen maken als uw account wordt toestaan. De onderstaande code maakt een submap met de naam **sampledir** onder de hoofdmap.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>Hoe u: een lijst met bestanden en mappen in een delen

Het verkrijgen van een lijst met bestanden en mappen binnen een delen is eenvoudig klaar door de ondersteuning voor **listFilesAndDirectories** in een verwijzing CloudFileDirectory. De methode geeft als resultaat een lijst met ListFileItem objecten die u kunt doorgaan met het ontwikkelen op. Als u bijvoorbeeld de volgende code wordt een lijst met bestanden en mappen in de hoofdmap.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Hoe u: een bestand downloaden

Een van de vaker bewerkingen die u tegen bestandsopslag uitvoert is om bestanden te downloaden. Klik in het volgende voorbeeld wordt de code SampleFile.txt-downloads en de inhoud weergeven.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>Hoe u: een bestand verwijderen

Een andere algemene opslagbewerking voor het bestand is bestand verwijderen. De volgende code Hiermee verwijdert u een bestand met de naam SampleFile.txt die zijn opgeslagen in een map met de naam **sampledir**.


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>Hoe u: een map verwijderen

Als u een map is een eenvoudig taak, hoewel het dat u een map waarin nog steeds bestanden of andere mappen niet verwijderen.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>Hoe u: een Share verwijderen

Verwijderen van een delen wordt uitgevoerd door de methode **deleteIfExists** bellen op een object CloudFileShare. Hier ziet u een voorbeeld van een code die die doet.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Volgende stappen

Als u meer informatie over andere Azure opslag API's wilt, volgt u deze koppelingen.

- [Java Developer Center](http://azure.microsoft.com/develop/java/)
- [Azure opslag SDK for Java](https://github.com/azure/azure-storage-java)
- [Azure opslag SDK voor Android](https://github.com/azure/azure-storage-android)
- [Azure opslag Client SDK verwijzing](http://dl.windowsazure.com/storage/javadoc/)
- [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure opslag teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Gegevens met het hulpprogramma AzCopy-opdrachtregel overbrengen](storage-use-azcopy.md)
