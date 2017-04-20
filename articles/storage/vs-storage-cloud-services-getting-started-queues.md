<properties
    pageTitle="Aan de slag met wachtrij opslagruimte en Visual Studio verbonden services (cloudservices) | Microsoft Azure"
    description="Hoe u aan de slag met Azure wachtrij opslag in een project cloud-service in Visual Studio nadat de verbinding met een opslag-account gebruik van Visual Studio services verbonden"
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

# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Aan de slag met Azure wachtrij opslagruimte en Visual Studio verbonden services (cloud services projecten)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe aan de slag met Azure wachtrij opslagruimte in Visual Studio nadat u hebt gemaakt of een account Azure opslag in een cloud services-project waarnaar wordt verwezen in het dialoogvenster Visual Studio **Verbonden Services toevoegen** .

Leert u hoe u een wachtrij maakt in code. Ook leert u hoe u eenvoudige wachtrij bewerkingen uitvoeren, zoals toevoegen, wijzigen, lezen en verwijderen van berichten in wachtrij plaatsen. In de voorbeelden in C#-code zijn geschreven en gebruikt u de [Bibliotheek van Microsoft Azure opslag Client voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

De bewerking **Verbonden Services toevoegen** de juiste NuGet-pakketten voor toegang tot Azure opslagruimte in uw project is geïnstalleerd en voegt de verbindingsreeks voor het account opslagruimte aan uw project configuratie-bestanden.

 - Zie [aan de slag met Azure wachtrij opslagruimte met .NET](storage-dotnet-how-to-use-queues.md) voor meer informatie over manipuleren wachtrijen in code.
 - Zie [opslagruimte documentatie](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over de opslag van Azure.
 - Zie [Cloud Services-documentatie](https://azure.microsoft.com/documentation/services/cloud-services/) voor algemene informatie over Azure cloudservices.
 - Zie [ASP.NET](http://www.asp.net) voor meer informatie over het programmeren ASP.NET-toepassingen.


Azure wachtrij opslag is een service voor het opslaan van grote aantallen van berichten die toegankelijk is vanuit een willekeurige plaats in de wereld via geverifieerde oproepen via HTTP of HTTPS. Een enkele wachtrij bericht kan maximaal 64 KB groot zijn en een wachtrij miljoenen berichten, tot aan de limiet van de totale capaciteit van een opslag-account kan bevatten.


## <a name="access-queues-in-code"></a>Access-wachtrijen in code

Voor toegang tot wachtrijen in Visual Studio Cloud Services-projecten, moet u de volgende items naar een C#-bronbestand die toegang hebben tot Azure wachtrij opslag bevatten.

1. Zorg ervoor dat de naamruimtedeclaraties boven aan het bestand C# opnemen deze instructies **gebruiken** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag-account. De volgende code gebruiken om de uw opslagruimte verbindingsreeks en de accountgegevens opslag van de Azure-service te configureren.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Een object **CloudQueueClient** om te verwijzen naar de wachtrij-objecten in uw account opslag krijgen.  

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Een object **CloudQueue** verwijzen naar een specifieke wachtrij krijgen.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**NOTITIE:** Voer alle bovenstaande code vóór de code in de volgende voorbeelden.

## <a name="create-a-queue-in-code"></a>Maken van een wachtrij in code

Maak de wachtrij in code, net een oproep toevoegen aan **CreateIfNotExists**.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Een bericht toevoegen aan een wachtrij

Als u wilt een bericht invoegen op een bestaande wachtrij, een nieuw **CloudQueueMessage** -object maken en klik vervolgens de methode **AddMessage** bellen.

Een object **CloudQueueMessage** kan worden gemaakt van een tekenreeks (in de indeling UTF-8) of een matrix van bytes.

Hier volgt een voorbeeld waarin het invoegen van het bericht 'Hallo, wereld'.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Een bericht in een wachtrij lezen

U kunt het bericht aan de voorzijde van een wachtrij bekijken zonder het te verwijderen uit de wachtrij door de methode **PeekMessage** roepen.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Lezen en verwijderen van een bericht in een wachtrij

Uw code kunt verwijderen (uit de wachtrij) een bericht van een wachtrij in twee stappen.

1. Bel **GetMessage** voor het volgende bericht in een wachtrij. Er wordt een bericht dat uit **GetMessage** geretourneerd onzichtbare naar een andere code lezen van berichten van deze wachtrij. Al dan niet standaard blijft dit bericht onzichtbare gedurende 30 seconden.
2.  Als u klaar bent met het bericht verwijderen uit de wachtrij, belt u **DeleteMessage**.

In dit proces van het verwijderen van een bericht zorgt ervoor dat als uw code mislukt verwerkingstijd van een bericht vanwege hardware of software, een ander exemplaar van uw code kunt het bericht verschijnt en probeer het opnieuw. De volgende code oproepen **DeleteMessage** rechts nadat het bericht is verwerkt.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>Aanvullende opties gebruiken om te verwerken en verwijderen van berichten in wachtrij plaatsen

Er zijn twee manieren kunt u voor het bericht ophalen uit een wachtrij aanpassen.

 - Hier krijgt u een reeks berichten (maximaal 32).
 - U kunt een time-out langer of korter invisibility instellen zodat uw code meer of minder tijd volledig verwerkingstijd van elk bericht. Het volgende voorbeeld wordt de **GetMessages** -methode 20 berichten in een oproep ontvangt. Vervolgens wordt elk bericht met een lus **foreach** verwerkt. Ook wordt de time-out invisibility ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de gestart 5 minuten voor alle berichten tegelijk, dus als u na 5 minuten hebt doorgegeven aangezien de oproep door naar **GetMessages**, alle berichten die niet zijn verwijderd weer zichtbaar worden.

Hier volgt een voorbeeld:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen

U kunt een schatting van het aantal berichten in een wachtrij krijgen. De methode **FetchAttributes** wordt gevraagd om de service wachtrij om op te halen de wachtrij-kenmerken, zoals het aantal berichten. De eigenschap **ApproximateMethodCount** retourneert de laatste waarde opgehaald door de methode **FetchAttributes** zonder het bellen van de wachtrij-service.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>Het patroon dat in afwachting van asynchrone gebruiken met algemene Azure wachtrij-API 's

In dit voorbeeld ziet u hoe u het patroon dat in afwachting van asynchrone met algemene Azure wachtrij-API's. De asynchrone-versie van elk van de opgegeven methoden voor de steekproef-oproepen, dit kan worden bekeken door het **asynchrone** post fix van elke methode. Wanneer u een asynchrone methode gebruikt de asynchrone-wachten op een patroon de lokale uitvoering onderbreekt totdat het gesprek is voltooid. Dit gedrag kunt de huidige thread andere werkzaamheden die helpt voorkomen van prestatieproblemen en verbetert de algehele respons van uw toepassing. Voor meer informatie over het gebruik van het patroon asynchrone-Await in .NET Zie [asynchrone en Await (C# en Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Een wachtrij verwijderen

Als u wilt een wachtrij en alle bijbehorende berichten die dit verwijderen, belt u de methode **Delete** op het object in de wachtrij.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
