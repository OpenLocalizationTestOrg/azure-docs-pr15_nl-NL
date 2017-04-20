<properties
    pageTitle="Instellen en eigenschappen en metagegevens voor objecten in Azure opslagruimte op te halen | Microsoft Azure"
    description="Aangepaste metagegevens worden opgeslagen op de objecten in Azure-opslag, en stel en Systeemeigenschappen ophalen."
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

# <a name="set-and-retrieve-properties-and-metadata"></a>Instellen en eigenschappen en metagegevens op te halen #

## <a name="overview"></a>Overzicht

Objecten in Azure Storage ondersteuning Systeemeigenschappen en de gebruiker gedefinieerde metagegevens, naast de gegevens bevatten:

*   **Systeemeigenschappen.** Er bestaan Systeemeigenschappen op elke resource opslag. Sommige van deze kan worden gelezen of instellen, terwijl de anderen zijn alleen-lezen. Achter de komen enkele Systeemeigenschappen overeen met bepaalde standaard HTTP-headers. De bibliotheek van de client Azure opslag onderhoudt deze voor u.  

*   **De gebruiker gedefinieerde metagegevens.** De gebruiker gedefinieerde metagegevens zijn metagegevens die u op een bepaalde bron, in de vorm van een paar naam-waarde opgeeft. U kunt de metagegevens gebruiken om op te slaan extra waarden met een resource opslag; Deze waarden zijn voor uw eigen doeleinden alleen en hebben geen invloed op de werking van de resource.  

Eigenschap en metagegevens waarden voor een resource opslag ophalen is een proces. Voordat u deze waarden lezen kunt, moet u deze expliciet door de ondersteuning voor de methode **FetchAttributes** ophalen.

> [AZURE.IMPORTANT] Eigenschap en metagegevens waarden voor een resource opslag worden niet ingevuld, tenzij u een van de methoden **FetchAttributes** belt. 

## <a name="setting-and-retrieving-properties"></a>Instellen en eigenschappen ophalen

Om eigenschapswaarden te vinden, de methode **FetchAttributes** bellen op uw blob of om de eigenschappen vullen de container en vervolgens de waarden te lezen.

Geef de eigenschapswaarde voor informatie over het instellen van eigenschappen voor een object en klik de **SetProperties** aanroept.

Het volgende voorbeeld wordt gemaakt van een container en schrijft enkele van de eigenschapswaarden naar een consolevenster:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Instellen en het terughalen van metagegevens

U kunt de metagegevens opgeven als een of meer naam-waardeparen op een resource blob of container. Metagegevens stelt naam-waardeparen toevoegen aan de collectie **metagegevens** op de resource, en vervolgens de **SetMetadata** aanroept om op te slaan van de waarden met de service.

> [AZURE.NOTE] De naam van de metagegevens van uw moet voldoen aan de naamgeving van C#-id's.
 
Het volgende voorbeeld wordt de metagegevens op een container ingesteld. Een bepaalde tekenreeks is ingesteld met de methode **Add** van de siteverzameling. De andere waarde is ingesteld met de syntaxis van de impliciete sleutelwaarde. Beide zijn geldig.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

Als u wilt ophalen metagegevens, de methode **FetchAttributes** bellen op uw blob of de container om de collectie **metagegevens** te vullen en klik vervolgens lezen van de waarden, zoals wordt weergegeven in het onderstaande voorbeeld.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Zie ook  

- [Azure opslag clientbibliotheek voor .NET-verwijzing](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [Azure opslag Client-bibliotheek voor .NET-pakket](https://www.nuget.org/packages/WindowsAzure.Storage/) 
