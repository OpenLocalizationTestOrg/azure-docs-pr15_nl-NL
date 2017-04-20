<properties
    pageTitle="Het gebruik van Table storage (C++) | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud met Azure-tabelopslag, een gegevensopslag NoSQL."
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

# <a name="how-to-use-table-storage-from-c"></a>Het gebruik van Table storage vanuit C++

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht  
Deze handleiding wordt uitgelegd hoe u veelvoorkomende scenario's uitvoeren met behulp van de Azure tabel storage-service. In de voorbeelden in C++ zijn geschreven en gebruikt u de [Bibliotheek van Azure opslag Client voor C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). De scenario's waarvoor opnemen **maken en verwijderen van een tabel** en **werken met tabel entiteiten**.

>[AZURE.NOTE] Deze handleiding is bedoeld voor de bibliotheek van Azure opslag-Client voor C++ versie 1.0.0 en hoger. De aanbevolen versie is opslag clientbibliotheek 2.2.0, verkrijgbaar via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>Een C++-toepassing maken  
In deze handleiding gebruikt u opslagfuncties die in een C++-toepassing kunnen worden uitgevoerd. Hiervoor moet u de bibliotheek van Azure opslag-Client voor C++ installeren en een account Azure opslagruimte in uw Azure abonnement maken.  

Als u wilt de bibliotheek van Azure opslag-Client voor C++ hebt geïnstalleerd, kunt u de volgende methoden:

-   **Linux:** Volg de instructies op de pagina [Azure opslag clientbibliotheek voor C++ Leesmij](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Windows:** Klik in Visual Studio, op **Hulpmiddelen > NuGet Package Manager > Package Manager-Console**. Typ de volgende opdracht in de [beheerconsole van NuGet pakket](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) en druk op Enter.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Uw toepassing voor toegang tot Table storage configureren  
Toevoegen aan dat de volgende beweringen naar het begin van het C++-bestand waarop u gebruiken van de Azure opslag API's wilt voor toegang tot tabellen bevatten:  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag instellen  
Een Azure opslag-client gebruikt een verbindingsreeks opslag voor de opslag van de eindpunten en referenties voor toegang tot data management-services. Wanneer een clienttoepassing actief is, moet u de verbindingsreeks van opslag in de volgende notatie opgeven. Gebruik de naam van uw opslag-account en de opslag access-toets voor de opslag-account wordt vermeld in de [Portal van Azure](https://portal.azure.com) voor de *accountnaam* en *AccountKey* waarden. Zie voor informatie over de opslag accounts en toegangstoetsen, [over Azure opslag-accounts](storage-create-storage-account.md). In dit voorbeeld ziet u hoe u een statische veld houdt u de verbindingsreeks kunt declareren:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Als u wilt testen van de toepassing in uw lokale Windows-computer, kunt u de Azure [opslag emulator](storage-use-emulator.md) die is geïnstalleerd met de [SDK van Azure](https://azure.microsoft.com/downloads/). De emulator opslag is een hulpprogramma die overeenkomt met de services Azure Blob, wachtrij en tabel beschikbaar is op uw computer plaatselijke ontwikkeling. Het volgende voorbeeld ziet hoe u een statische veld houdt u de verbindingsreeks naar uw lokale opslag emulator kunt declareren:  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Als u wilt de emulator Azure opslag, klikt u op de knop **Start** of druk op de Windows-toets. Begin te typen **Azure opslag Emulator**en selecteer vervolgens **Microsoft Azure opslag Emulator** in de lijst met toepassingen.  

In de volgende voorbeelden wordt ervan uitgegaan dat u een van de volgende twee manieren hebt gebruikt om de opslag-verbindingsreeks.  

## <a name="retrieve-your-connection-string"></a>De verbindingsreeks ophalen  
U kunt de klas **cloud_storage_account** gebruiken om aan te geven van de gegevens van uw opslag-account. Als u wilt uw accountgegevens opslag opgehaald uit de verbindingsreeks opslag, kunt u de methode parseren gebruiken.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Vervolgens krijgt u een verwijzing naar een klasse **cloud_table_client** , als deze, kunt u verwijzingsobjecten krijgen voor tabellen en entiteiten die zijn opgeslagen in de tabel storage-service. De volgende code maakt een object **cloud_table_client** met behulp van het object voor het account van opslag dat we opgehaald hierboven:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Een tabel maken
Een object **cloud_table_client** kunt u verwijzingsobjecten voor tabellen en entiteiten ophalen. De volgende code wordt gemaakt van een object **cloud_table_client** en wordt deze gebruikt om een nieuwe tabel te maken.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel
Als u wilt een entiteit toevoegen aan een tabel, een nieuw **table_entity** -object maken en doorgegeven aan **table_operation::insert_entity**. De volgende code gebruikt de voornaam van de klant als de rij-toets en de achternaam als de partitiesleutel. Samen en identificatie van een entiteit partition en rijsleutel unieke de entiteit in de tabel. Entiteiten met dezelfde partitiesleutel sneller dan bij andere partition toetsen kunnen worden opgezocht, maar met diverse partition toetsen kunt grotere parallelle bewerking schaalbaarheid. Zie [Microsoft Azure opslag prestaties en schaalbaarheid controlelijst](storage-performance-checklist.md)voor meer informatie.

De volgende code maakt een nieuw exemplaar van **table_entity** met enkele klantgegevens worden opgeslagen. De code vervolgens belt **table_operation::insert_entity** als u wilt maken van een object **table_operation** als u wilt een entiteit in een tabel invoegen en de nieuwe Tabelentiteit gekoppeld. De code oproepen ten slotte de methode execute op het object **cloud_table** . En de nieuwe **table_operation** wordt een aanvraag verzonden naar de service van de tabel de nieuwe klantentiteit invoegen in de tabel "personen".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>Een reeks entiteiten invoegen
U kunt een reeks entiteiten met de service tabel invoegen in één bewerking voor schrijven. De volgende code wordt gemaakt van een object **table_batch_operation** en telt vervolgens dat drie bewerkingen wilt invoegen. Elke invoegbewerking is toegevoegd door er een nieuw entiteitsobject, het instellen van de waarden wilt opzoeken en bellen van de methode voor invoegen klikt u vervolgens op het object **table_batch_operation** de entiteit koppelen aan een nieuwe invoegbewerking. Vervolgens **cloud_table.execute** aangeroepen om de bewerking niet uitvoeren.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Enkele dingen die u moet de opmerking over de batchbewerkingen:  

-   U kunt maximaal 100 invoegen, verwijderen, samenvoegen, vervangen, invoegen of samenvoegen en invoegen of vervangen bewerkingen uitvoeren in een willekeurige combinatie in één batch.  
-   Een batchbewerking kan een bewerking ophalen, hebben als dit de enige bewerking in de batch is.  
-   Alle entiteiten in één batchbewerking moeten dezelfde partitiesleutel hebben.  
-   Een batchbewerking is beperkt tot een 4-MB nettolading.  

## <a name="retrieve-all-entities-in-a-partition"></a>Alle entiteiten in een partition ophalen
Als u wilt een tabel voor alle entiteiten in een partition query, gebruikt u een object **table_query** . Het volgende voorbeeld bevat een filter voor entiteiten waar 'Smit' de toets partition is. In dit voorbeeld wordt de velden van elke entiteit in de resultaten van de query aan de console.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

De query in dit voorbeeld kunt u alle entiteiten die overeenkomen met de filtercriteria. Als u grote tabellen hebt en nodig hebt om te downloaden van de tabel entiteiten vaak, we raden opslaan u uw gegevens in opslagruimte Azure BLOB's in plaats daarvan.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Een bereik van entiteiten in een partition ophalen
Als u niet dat alle entiteiten in een partition query wilt, kunt u een bereik opgeven door te combineren het partition belangrijke filter met een rij belangrijke filter. Het volgende voorbeeld wordt gebruikt twee filters om alle entiteiten in partition 'Smit' waar de rij-toets (voornaam) met een letter begint eerder dan "E" in het alfabet en klik vervolgens op resultaten van de query wordt afgedrukt.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Een enkele entiteit ophalen
U kunt een query voor het ophalen van een eenmalige, specifieke eenheid schrijven. De volgende code gebruikt **table_operation::retrieve_entity** om op te geven van de klant 'Jeff Smith'. Deze methode retourneert één entiteit, in plaats van een siteverzameling en de resulterende waarde is in **table_result**. Partition zowel rij toetsen op te geven in een query is de snelste manier om op te halen een eenheid van de tabel-service.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Een entiteit vervangen
Als u wilt vervangen een entiteit, opgehaald van de service van de tabel, het entiteitsobject wijzigen en sla de wijzigingen weer terug naar de service van de tabel. De volgende programmacode wijzigt van een bestaande klant telefoonnummer en e-mailadres. In plaats van **table_operation::insert_entity**bellen, wordt deze **table_operation::replace_entity**gebruikt. Hierdoor wordt de entiteit volledig vervangen op de server, tenzij de entiteit op de server is niet gewijzigd sinds deze is opgehaald, zodat de bewerking, mislukt. Deze fout wordt voorkomen dat uw toepassing een wijziging die zijn gedaan tussen de ophalen en bijwerken door een ander onderdeel van uw toepassing, automatisch worden overschreven. De juiste afhandeling van deze fout is de entiteit opnieuw ophalen, breng uw wijzigingen (als deze nog geldig is) en vervolgens een andere **table_operation::replace_entity** -bewerking uitvoeren. Het volgende gedeelte wordt uitgelegd hoe u dit gedrag wijzigen.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Invoegen of-vervangen een entiteit
**table_operation::replace_entity** bewerkingen loopt vast als de entiteit is gewijzigd nadat deze is opgehaald uit de server. Daarnaast moet u de entiteit ophalen van de server eerst in de volgorde voor **table_operation::replace_entity** te laten slagen. Soms echter u niet weet of de entiteit op de server bestaat en de huidige waarden die zijn opgeslagen in deze niet relevant zijn, de update ze allemaal moet overschrijven. U kunt dit doen, gebruikt u een bewerking **table_operation::insert_or_replace_entity** . Deze bewerking wordt de entiteit ingevoegd als deze niet bestaat, of vervangt u deze zo ja, ongeacht de waarop de laatste update plaatsvond. In het volgende voorbeeld, de klantentiteit voor Jeff Smith zijn nog steeds opgehaald, maar deze vervolgens terug naar de server via **table_operation::insert_or_replace_entity**is opgeslagen. Wijzigingen in de entity tussen het ophalen en Bijwerkbewerking worden overschreven.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>Query een subset van entiteitseigenschappen  
Een query toevoegen aan een tabel kunt u slechts een paar eigenschappen ophalen uit een entiteit. De query in de volgende code wordt de methode **table_query::set_select_columns** gebruikt om terug te keren alleen de e-mailadressen van entiteiten in de tabel.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Query's uitvoeren enkele eigenschappen van een entiteit is een efficiënter bewerking dan het ophalen van alle eigenschappen.

## <a name="delete-an-entity"></a>Een entiteit verwijderen
Nadat u deze hebt opgehaald, kunt u gemakkelijk een entiteit verwijderen. Zodra de entiteit is opgehaald, belt u **table_operation::delete_entity** aan de entiteit te verwijderen. Vervolgens de methode **cloud_table.execute** aanroepen. De volgende code worden opgehaald en Hiermee verwijdert u een entiteit met een partitiesleutel van 'Smit' en een rij-toets van "Jeff".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Een tabel verwijderen
Ten slotte het volgende voorbeeld Hiermee verwijdert u een tabel uit een opslag-account. Een tabel die is verwijderd niet opnieuw te worden gemaakt voor een periode na de verwijdering beschikbaar.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Volgende stappen
U kunt de basisbeginselen van tabelopslag hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure opslag:  

-   [Het gebruik van C++-blobopslag](storage-c-plus-plus-how-to-use-blobs.md)
-   [Het gebruik van de wachtrij opslag van C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Lijst van Azure opslag resources in C++](storage-c-plus-plus-enumeration.md)
-   [Opslag clientbibliotheek voor C++ verwijzing](http://azure.github.io/azure-storage-cpp)
-   [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
