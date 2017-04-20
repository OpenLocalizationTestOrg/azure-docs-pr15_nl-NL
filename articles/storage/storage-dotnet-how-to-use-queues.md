<properties
    pageTitle="Aan de slag met Azure wachtrij opslagruimte met .NET | Microsoft Azure"
    description="Azure wachtrijen bieden betrouwbare, asynchrone messaging tussen toepassingsonderdelen. SMS-berichten kunt uw toepassingsonderdelen aan de nieuwe schaal onafhankelijk cloud."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Aan de slag met Azure wachtrij opslagruimte met .NET

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Azure wachtrij opslagruimte biedt cloud messaging tussen toepassingsonderdelen. Bij het ontwerpen van toepassingen voor schaal, zijn toepassingsonderdelen vaak ontkoppelde, zodat ze onafhankelijk schaal kunnen aanpassen. Wachtrij opslagruimte biedt asynchrone messaging voor communicatie tussen toepassingsonderdelen, ongeacht of ze worden uitgevoerd in de cloud, op het bureaublad, op een on-premises implementatie-server of op een mobiel apparaat. Wachtrij opslag ondersteunt ook asynchroon taken beheren en het bouwen van processen proces.

### <a name="about-this-tutorial"></a>Over deze zelfstudie

Deze zelfstudie wordt getoond hoe u schrijft .NET-code voor enkele veelvoorkomende scenario's met Azure wachtrij opslag. Scenario's waarvoor opnemen maken en verwijderen van wachtrijen en toe te voegen, lezen en verwijderen van berichten in wachtrij plaatsen.

**Geschatte tijd in beslag:** 45 minuten

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure opslag clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Configuration Manager voor .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Een [account van Azure opslag](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen

Voeg de volgende `using` instructies naar het begin van de `program.cs` bestand:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>Parseren van de verbindingsreeks

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>De client van de service wachtrij maken

De klasse **CloudQueueClient** kunt ophalen wachtrijen die zijn opgeslagen in de wachtrij opslag. Hier volgt een manier om u te maken van de serviceclient:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

U bent nu klaar om te schrijven code die gegevens ophaalt uit en schrijft gegevens naar wachtrij opslag.

## <a name="create-a-queue"></a>Een wachtrij maken

In dit voorbeeld ziet u hoe u kunt maken van een wachtrij als deze nog niet bestaat:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Een bericht invoegen in een wachtrij

U een bericht in een bestaande wachtrij invoegt, moet u eerst een nieuwe **CloudQueueMessage**maken. Vervolgens de methode **AddMessage** aanroepen. Een **CloudQueueMessage** kan worden gemaakt van een tekenreeks (in de indeling UTF-8) of een matrix van **bytes** . Hier ziet u code die wordt gemaakt van een wachtrij (als deze niet bestaat) en het invoegen van het bericht 'Hallo, wereld':

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Bekijken van het volgende bericht

U kunt het bericht aan de voorzijde van een wachtrij bekijken zonder het te verwijderen uit de wachtrij door de methode **PeekMessage** roepen.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>De inhoud van een bericht in de wachtrij wijzigen

U kunt de inhoud van een bericht in-place in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie kunt gebruiken bij de status van de werktaak. De volgende code het bericht wachtrij bijgewerkt met nieuwe inhoud en de zichtbaarheid time-out voor het uitbreiden van een andere 60 seconden is ingesteld. De status van het werk dat is gekoppeld aan het bericht wordt opgeslagen, en geeft de klant een ander minuten om verder te werken op het bericht. U kunt deze techniek voor het bijhouden van meerdere stappen werkstromen op berichten in wachtrij plaatsen, zonder dat u moet beginnen vanaf het begin als een verwerkingsstap vanwege hardware of software mislukt. Normaal gesproken kunt u een aantal nieuwe pogingen ook wilt behouden en als het bericht opnieuw meer dan *n* keer gestart wordt, kunt u dit wilt verwijderen. Hiermee voorkomt u een bericht dat wordt een fout gegenereerd telkens wanneer die deze wordt verwerkt.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>Het volgende bericht uit de wachtrij

Uw code wachtrijen uit een bericht van een wachtrij in twee stappen. Als u **GetMessage belt**, krijgt u het volgende bericht in een wachtrij. Er wordt een bericht dat uit **GetMessage** geretourneerd onzichtbare naar een andere code lezen van berichten van deze wachtrij. Al dan niet standaard blijft dit bericht onzichtbare gedurende 30 seconden. Als u klaar bent met het bericht verwijderen uit de wachtrij, moet u ook **DeleteMessage**bellen. In dit proces van het verwijderen van een bericht zorgt ervoor dat als uw code mislukt verwerkingstijd van een bericht vanwege hardware of software, een ander exemplaar van uw code kunt het bericht verschijnt en probeer het opnieuw. Uw code oproepen **DeleteMessage** rechts nadat het bericht is verwerkt.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Asynchrone-wachten op een patroon met algemene wachtrij opslagmedia API's gebruiken

In dit voorbeeld ziet u hoe u het patroon dat in afwachting van asynchrone met gezamenlijke wachtrij opslag API's. De steekproef oproepen de asynchroon versie van elk van de opgegeven methoden, zoals aangegeven met het achtervoegsel *asynchrone* van elke methode. Wanneer u een asynchrone methode gebruikt, het asynchrone-wachten op een patroon de lokale uitvoering onderbreekt totdat het gesprek is voltooid. Dit gedrag kunt de huidige thread andere werkzaamheden, die helpt voorkomen van prestatieproblemen en verbetert de algehele respons van uw toepassing. Zie voor meer informatie over het gebruik van het patroon asynchrone-Await in .NET [asynchrone en Await (C# en Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Gebruikmaken van extra opties voor berichten in de wachtrij plaatsen

Er zijn twee manieren kunt u voor het bericht ophalen uit een wachtrij aanpassen.
U gaat eerst een reeks berichten (maximaal 32). Tweede, kunt u een langer of korter invisibility time-out instellen, zodat uw code meer of minder tijd volledig verwerkingstijd van elk bericht. Het volgende voorbeeld wordt de **GetMessages** -methode 20 berichten in een oproep ontvangt. Vervolgens wordt elk bericht met een lus **foreach** verwerkt. Ook wordt de time-out invisibility ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de gestart 5 minuten voor alle berichten tegelijk, dus als u na 5 minuten hebt doorgegeven aangezien de oproep door naar **GetMessages**, alle berichten die niet zijn verwijderd weer zichtbaar worden.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen

U kunt een schatting van het aantal berichten in een wachtrij krijgen. De methode **FetchAttributes** wordt gevraagd om de service wachtrij om op te halen de wachtrij-kenmerken, zoals het aantal berichten. De eigenschap **ApproximateMessageCount** retourneert de laatste waarde opgehaald door de methode **FetchAttributes** zonder het bellen van de wachtrij-service.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Een wachtrij verwijderen

Als u wilt een wachtrij en alle bijbehorende berichten die dit verwijderen, belt u de methode **Delete** op het object in de wachtrij.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van wachtrij opslagruimte hebt geleerd, volgt u deze koppelingen voor meer informatie over opslagtaken voor meer complexe.

- De wachtrij verwijzing servicedocumentatie voor volledige informatie over de beschikbare API's weergeven:
    - [Opslag clientbibliotheek voor .NET-verwijzing](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Overzicht van de REST API](http://msdn.microsoft.com/library/azure/dd179355)
- Leer hoe u de code die u schrijft voor gebruik met Azure opslagruimte met behulp van de [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md)vereenvoudigen.
- Bekijk meer functie hulplijnen voor meer informatie over extra opties voor het opslaan van gegevens in Azure wordt aangegeven.
    - [Aan de slag met Azure-tabelopslag .NET gebruiken](storage-dotnet-how-to-use-tables.md) om op te slaan gestructureerde gegevens.
    - [Aan de slag met Azure-blobopslag .NET gebruiken](storage-dotnet-how-to-use-blobs.md) om op te slaan ongestructureerde gegevens.
    - [Verbinding maken met SQL-Database met behulp van .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) voor de opslag van relationele gegevens.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
