<properties
    pageTitle="Het gebruik van de wachtrij opslag (C++) | Microsoft Azure"
    description="Informatie over het gebruik van de wachtrij storage-service in Azure wordt aangegeven. Voorbeelden worden geschreven in C++."
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

# <a name="how-to-use-queue-storage-from-c"></a>Het gebruik van de wachtrij opslag van C++  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Deze handleiding leert u hoe u veelvoorkomende scenario's voor het gebruik van de Azure wachtrij storage-service uitvoert. In de voorbeelden in C++ zijn geschreven en gebruikt u de [Bibliotheek van Azure opslag Client voor C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). De scenario's waarvoor opnemen **Invoegen**, **inspecteren**, **ophalen**en **verwijderen van** berichten in wachtrij plaatsen, evenals **maken en verwijderen van wachtrijen**.

>[AZURE.NOTE] Deze handleiding is bedoeld voor de bibliotheek van Azure opslag-Client voor C++ versie 1.0.0 en hoger. De aanbevolen versie is opslag clientbibliotheek 2.2.0, verkrijgbaar via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](http://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Een C++-toepassing maken  
In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een C++-toepassing.

Hiervoor moet u de bibliotheek van Azure opslag-Client voor C++ installeren en een account Azure opslagruimte in uw Azure abonnement maken.

Als u wilt de bibliotheek van Azure opslag-Client voor C++ hebt geïnstalleerd, kunt u de volgende methoden:

-   **Linux:** Volg de instructies in de [Azure opslag Client voor C++ Leesmij](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) bibliotheekpagina.
-   **Windows:** Klik in Visual Studio, op **Hulpmiddelen > NuGet Package Manager > Package Manager-Console**. Typ de volgende opdracht in de [beheerconsole van NuGet pakket](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) en druk op **ENTER**.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Uw toepassing toegang hebben tot wachtrij opslag configureren
Toevoegen aan dat de volgende beweringen naar het begin van het C++-bestand waarop u gebruiken van de Azure opslag API's wilt voor toegang tot wachtrijen opnemen:  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag instellen

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

## <a name="how-to-create-a-queue"></a>Hoe u: een wachtrij maken
Een object **cloud_queue_client** kunt u verwijzingsobjecten voor wachtrijen ophalen. De volgende code maakt een object **cloud_queue_client** .

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

Gebruik het object **cloud_queue_client** om een verwijzing naar de wachtrij die u wilt gebruiken. Als deze niet bestaat, kunt u de wachtrij maken.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>Hoe u: een bericht invoegen in een wachtrij
U een bericht in een bestaande wachtrij invoegt, moet u eerst een nieuwe **cloud_queue_message**maken. Vervolgens de methode **add_message** aanroepen. Een **cloud_queue_message** kan worden gemaakt van een tekenreeks of een matrix van **bytes** . Hier ziet u code die wordt gemaakt van een wachtrij (als deze niet bestaat) en het invoegen van het bericht 'Hallo, wereld':

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>Hoe u: bekijken van het volgende bericht
U kunt het bericht aan de voorzijde van een wachtrij bekijken zonder het te verwijderen uit de wachtrij door te bellen van de methode **peek_message** .

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Hoe u: de inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in-place in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie kunt gebruiken bij de status van de werktaak. De volgende code het bericht wachtrij bijgewerkt met nieuwe inhoud en de zichtbaarheid time-out voor het uitbreiden van een andere 60 seconden is ingesteld. De status van het werk dat is gekoppeld aan het bericht wordt opgeslagen, en geeft de klant een ander minuten om verder te werken op het bericht. U kunt deze techniek voor het bijhouden van meerdere stappen werkstromen op berichten in wachtrij plaatsen, zonder dat u moet beginnen vanaf het begin als een verwerkingsstap vanwege hardware of software mislukt. Normaal gesproken kunt u een aantal nieuwe pogingen ook wilt behouden en als het bericht opnieuw meer dan n tijden wordt gestart, kunt u dit wilt verwijderen. Hiermee voorkomt u een bericht dat wordt een fout gegenereerd telkens wanneer die deze wordt verwerkt.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>Hoe: het volgende bericht uit de wachtrij
Uw code wachtrijen uit een bericht van een wachtrij in twee stappen. Als u **get_message belt**, krijgt u het volgende bericht in een wachtrij. Er wordt een bericht dat uit **get_message** geretourneerd onzichtbare naar een andere code lezen van berichten van deze wachtrij. Als u klaar bent met het bericht verwijderen uit de wachtrij, moet u ook **delete_message**bellen. In dit proces van het verwijderen van een bericht zorgt ervoor dat als uw code mislukt verwerkingstijd van een bericht vanwege hardware of software, een ander exemplaar van uw code kunt het bericht verschijnt en probeer het opnieuw. Uw code oproepen **delete_message** rechts nadat het bericht is verwerkt.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Hoe u: gebruikmaken van extra opties voor berichten in de wachtrij plaatsen
Er zijn twee manieren kunt u voor het bericht ophalen uit een wachtrij aanpassen. U gaat eerst een reeks berichten (maximaal 32). Tweede, kunt u een langer of korter invisibility time-out instellen, zodat uw code meer of minder tijd volledig verwerkingstijd van elk bericht. Het volgende voorbeeld wordt de **get_messages** -methode 20 berichten in een oproep ontvangt. Vervolgens wordt elk bericht met een lus **voor** verwerkt. Ook wordt de time-out invisibility ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de gestart 5 minuten voor alle berichten tegelijk, dus als u na 5 minuten hebt doorgegeven aangezien de oproep door naar **get_messages**, alle berichten die niet zijn verwijderd weer zichtbaar worden.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>Hoe u: lengte van de wachtrij ophalen
U kunt een schatting van het aantal berichten in een wachtrij krijgen. De methode **download_attributes** wordt gevraagd om de service wachtrij om op te halen de wachtrij-kenmerken, zoals het aantal berichten. De methode **approximate_message_count** krijgt het benadering aantal berichten in de wachtrij.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>Hoe u: een wachtrij verwijderen
Als u wilt een wachtrij en alle bijbehorende berichten die dit verwijderen, belt u de methode **delete_queue_if_exists** op het object in de wachtrij.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van wachtrij opslagruimte hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure opslag.

-   [Het gebruik van C++-blobopslag](storage-c-plus-plus-how-to-use-blobs.md)
-   [Het gebruik van Table Storage vanuit C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Lijst met Azure opslag Resources in C++](storage-c-plus-plus-enumeration.md)
-   [Opslag clientbibliotheek voor C++ verwijzing](http://azure.github.io/azure-storage-cpp)
-   [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)

