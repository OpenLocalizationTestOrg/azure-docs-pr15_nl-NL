<properties
    pageTitle="Het gebruik van de wachtrij opslag van Java | Microsoft Azure"
    description="Meer informatie over het gebruik van de wachtrij Azure-service maken en wachtrijen, verwijderen en invoegen, krijgen en berichten verwijderen. Voorbeelden geschreven in Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>Het gebruik van de wachtrij opslag van Java

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Deze handleiding leert u hoe u veelvoorkomende scenario's voor het gebruik van de Azure wachtrij storage-service uitvoert. In de voorbeelden in Java zijn geschreven en gebruik van de [Azure opslag SDK for Java][]. De scenario's waarvoor opnemen **Invoegen**, **inspecteren** **ophalen**en **verwijderen van** berichten in wachtrij plaatsen, evenals wachtrijen voor het **maken** en **verwijderen** . Zie voor meer informatie over wachtrijen, de sectie van de [volgende stappen](#Next-Steps) .

Opmerking: Een SDK is beschikbaar voor ontwikkelaars die Azure opslag op Android-apparaten gebruikt. Zie de [SDK van Azure-opslag voor Android][]voor meer informatie.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Een Java-toepassing maken

In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een Java-toepassing lokaal of in code die wordt uitgevoerd vanuit een Webrol of werknemer rol in Azure wordt aangegeven.

Hiervoor moet u voor het installeren van de Java Development Kit (JDK) en een account Azure opslagruimte in uw Azure abonnement maken. Nadat u dit hebt gedaan, moet u controleren of uw systeem voor de ontwikkeling voldoet aan de minimale vereisten en afhankelijkheden die worden weergegeven in de bibliotheek [Azure opslag SDK for Java][] op GitHub. Als uw systeem aan deze vereisten voldoet, kunt u de instructies voor het downloaden en installeren van de Azure opslag bibliotheken voor Java op uw systeem van die opslagplaats. Als u deze taken hebt voltooid, is mogelijk te maken van een Java-toepassing waarin de voorbeelden in dit artikel.

## <a name="configure-your-application-to-access-queue-storage"></a>Uw toepassing toegang hebben tot wachtrij opslag configureren

De volgende importinstructies toevoegen aan het begin van het gewenste waar Azure opslag API's gebruiken voor toegang tot wachtrijen Java-bestand:

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## <a name="setup-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag instellen

Een Azure opslag-client gebruikt een verbindingsreeks opslag voor de opslag van de eindpunten en referenties voor toegang tot data management-services. Wanneer u zich in een clienttoepassing, moet u de verbindingsreeks van opslag in de volgende indeling opgeven met behulp van de naam van uw opslag-account en de primaire access-toets voor de opslag-account wordt vermeld in de [Portal van Azure](https://portal.azure.com) voor de waarden *accountnaam* en *AccountKey* aan te geven. In dit voorbeeld ziet u hoe u een statische veld houdt u de verbindingsreeks kunt declareren:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

In een toepassing binnen een rol in Microsoft Azure uitgevoerd, wordt deze tekenreeks kan worden opgeslagen in het configuratiebestand service, *ServiceConfiguration.cscfg*, en kan worden geopend met een gesprek met de methode **RoleEnvironment.getConfigurationSettings** . Hier volgt een voorbeeld van de verbindingsreeks ophalen uit een **instelling** -element met de naam *StorageConnectionString* in het configuratiebestand service:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

In de volgende voorbeelden wordt ervan uitgegaan dat u een van de volgende twee manieren hebt gebruikt om de opslag-verbindingsreeks.

## <a name="how-to-create-a-queue"></a>Hoe u: een wachtrij maken

Een object **CloudQueueClient** kunt u verwijzingsobjecten voor wachtrijen ophalen. De volgende code maakt een object **CloudQueueClient** . (Notitie: Er zijn andere manieren om te maken van **CloudStorageAccount** objecten; Zie **CloudStorageAccount** in de [Azure opslag Client SDK verwijzing]voor meer informatie.)

Gebruik het object **CloudQueueClient** om een verwijzing naar de wachtrij die u wilt gebruiken. Als deze niet bestaat, kunt u de wachtrij maken.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>Hoe u: een bericht toevoegen aan een wachtrij

U een bericht in een bestaande wachtrij invoegt, moet u eerst een nieuwe **CloudQueueMessage**maken. Vervolgens de methode **addMessage** aanroepen. Een **CloudQueueMessage** kan worden gemaakt van een tekenreeks (in de indeling UTF-8) of een matrix van bytes. Hier ziet u code die wordt gemaakt van een wachtrij (als deze niet bestaat) en wordt het bericht 'Hallo, wereld' ingevoegd.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>Hoe u: bekijken van het volgende bericht

U kunt het bericht aan de voorzijde van een wachtrij bekijken zonder het te verwijderen uit de wachtrij door te bellen **peekMessage**.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Hoe u: de inhoud van een bericht in de wachtrij wijzigen

U kunt de inhoud van een bericht in-place in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie kunt gebruiken bij de status van de werktaak. De volgende code het bericht wachtrij bijgewerkt met nieuwe inhoud en de zichtbaarheid time-out voor het uitbreiden van een andere 60 seconden is ingesteld. De status van het werk dat is gekoppeld aan het bericht wordt opgeslagen, en geeft de klant een ander minuten om verder te werken op het bericht. U kunt deze techniek voor het bijhouden van meerdere stappen werkstromen op berichten in wachtrij plaatsen, zonder dat u moet beginnen vanaf het begin als een verwerkingsstap vanwege hardware of software mislukt. Normaal gesproken kunt u een aantal nieuwe pogingen ook wilt behouden en als het bericht opnieuw meer dan *n* keer gestart wordt, kunt u dit wilt verwijderen. Hiermee voorkomt u een bericht dat wordt een fout gegenereerd telkens wanneer die deze wordt verwerkt.

De volgende code steekproef gezocht in de wachtrij van berichten, zoekt het eerste bericht dat overeenkomt met 'Hallo, wereld' voor de inhoud, en vervolgens het bericht inhoud wijzigt en afgesloten.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

U kunt ook werkt het volgende voorbeeld alleen het eerste zichtbaar bericht in de wachtrij.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>Hoe u: lengte van de wachtrij ophalen

U kunt een schatting van het aantal berichten in een wachtrij krijgen. De methode **downloadAttributes** wordt gevraagd om de service wachtrij voor verschillende huidige waarden, inclusief een telling van hoeveel berichten in een wachtrij worden. Het aantal is alleen bij benadering omdat berichten kunnen worden toegevoegd of verwijderd nadat de wachtrij-service moet op uw aanvraag kunt invullen reageren. De methode **getApproximateMessageCount** retourneert de laatste waarde opgehaald door de oproep door naar **downloadAttributes**, zonder het bellen van de wachtrij-service.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>Hoe: het volgende bericht in wachtrij

Uw code dequeues een bericht van een wachtrij in twee stappen. Als u **retrieveMessage belt**, krijgt u het volgende bericht in een wachtrij. Er wordt een bericht dat uit **retrieveMessage** geretourneerd onzichtbare naar een andere code lezen van berichten van deze wachtrij. Al dan niet standaard blijft dit bericht onzichtbare gedurende 30 seconden. Als u klaar bent met het bericht verwijderen uit de wachtrij, moet u ook **deleteMessage**bellen. In dit proces van het verwijderen van een bericht zorgt ervoor dat als uw code mislukt verwerkingstijd van een bericht vanwege hardware of software, een ander exemplaar van uw code kunt het bericht verschijnt en probeer het opnieuw. Uw code oproepen **deleteMessage** rechts nadat het bericht is verwerkt.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Aanvullende opties voor berichten waarbij

Er zijn twee manieren kunt u voor het bericht ophalen uit een wachtrij aanpassen. U gaat eerst een reeks berichten (maximaal 32). Tweede, kunt u een langer of korter invisibility time-out instellen, zodat uw code meer of minder tijd volledig verwerkingstijd van elk bericht.

Het volgende voorbeeld wordt de **retrieveMessages** -methode 20 berichten in een oproep ontvangt. Vervolgens wordt elk bericht met een lus **voor** verwerkt. Ook wordt de time-out invisibility ingesteld op vijf minuten (300 seconden) voor elk bericht. Opmerking waarmee de vijf minuten kan worden gestart voor alle berichten tegelijk, zodat wanneer vijf minuten verstreken sinds de oproep door naar **retrieveMessages**, alle berichten die niet zijn verwijderd weer zichtbaar worden.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>Hoe u: de wachtrijen voor een lijst met

Als u een lijst van de huidige wachtrijen, belt u de methode **CloudQueueClient.listQueues()** , die een verzameling objecten **CloudQueue** zullen retourneren.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>Hoe u: een wachtrij verwijderen

Als u wilt een wachtrij en alle bijbehorende berichten die dit verwijderen, belt u de methode **deleteIfExists** op het object **CloudQueue** .

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van wachtrij opslagruimte hebt geleerd, volgt u deze koppelingen voor meer informatie over opslagtaken voor meer complexe.

- [Azure opslag SDK for Java][]
- [Azure opslag Client SDK verwijzing][]
- [Azure opslagservices REST API][]
- [Azure opslag teamblog][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure opslag SDK for Java]: https://github.com/azure/azure-storage-java
[Azure opslag SDK voor Android]: https://github.com/azure/azure-storage-android
[Azure opslag Client SDK verwijzing]: http://dl.windowsazure.com/storage/javadoc/
[Azure opslagservices REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure opslag teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
