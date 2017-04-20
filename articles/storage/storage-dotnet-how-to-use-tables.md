<properties
    pageTitle="Aan de slag met Azure-tabelopslag met .NET | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud met Azure-tabelopslag, een gegevensopslag NoSQL."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-table-storage-using-net"></a>Aan de slag met Azure-tabelopslag met .NET

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Azure-tabelopslag is een service die gestructureerde NoSQL gegevens in de cloud opslaat. Tabelopslag is een toets/kenmerk winkel met het ontwerp van een schemaless. Omdat Table storage schemaless is, is het is eenvoudig kunt aanpassen van uw gegevens als de behoeften van uw toepassing evolve. Toegang tot gegevens is snel en efficiënt voor allerlei soorten toepassingen. Tabelopslag is meestal aanzienlijk lager in kosten dan traditionele SQL voor soortgelijke hoeveelheden gegevens.

U kunt Table storage gebruiken om op te slaan flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, gegevens van een apparaat en een ander type metagegevens die uw service vereist. U kunt een willekeurig aantal entiteiten opslaan in een tabel en een account opslag mogelijk niet het getal van tabellen, tot aan de capaciteitslimiet van de van het account opslagruimte bevatten.

### <a name="about-this-tutorial"></a>Over deze zelfstudie

Deze zelfstudie wordt getoond hoe u schrijft .NET-code voor enkele veelvoorkomende scenario's met Azure Table storage, inclusief maken en verwijderen van een tabel invoegen, bijwerken, verwijderen en query's uitvoeren gegevens in een tabel.

**Geschatte tijd in beslag:** 45 minuten

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure opslag clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Configuration Manager voor .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Een [account van Azure opslag](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Meer voorbeelden

Zie [Aan de slag met Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)voor aanvullende voorbeelden met Table storage. U kunt downloaden van de steekproef-toepassing en worden uitgevoerd, of de code op GitHub bladeren.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen

Voeg de volgende `using` instructies naar het begin van de `program.cs` bestand:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>Parseren van de verbindingsreeks

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Maken van de tabel service-client

De klasse **CloudTableClient** kunt u tabellen en entiteiten die zijn opgeslagen in Table storage ophalen. Hier volgt een manier om u te maken van de serviceclient:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

U bent nu klaar om te schrijven code die gegevens ophaalt uit en schrijft gegevens naar Table storage.

## <a name="create-a-table"></a>Een tabel maken

In dit voorbeeld ziet u hoe u kunt een tabel maken als deze nog niet bestaat:

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Entiteiten toewijzen aan C\# objecten met behulp van een aangepaste klasse afgeleid van **TableEntity**. Als u wilt een entiteit toevoegen aan een tabel, een klasse maken die de eigenschappen van uw entiteit definieert. De volgende code definieert een entity-klasse die gebruikmaakt van de voornaam van de klant als de rijsleutel en de achternaam als de partitiesleutel. Samen en identificatie van een entiteit partition en rijsleutel unieke de entiteit in de tabel. Entiteiten met dezelfde partitiesleutel sneller dan bij andere partition toetsen kunnen worden opgezocht, maar met diverse partition toetsen kunt grotere schaalbaarheid van parallelle bewerkingen.  De eigenschap moet van een eigenschap die moet worden opgeslagen in de tabel-service, een openbare eigenschap met een ondersteund type dat toegang biedt tot beide `get` en `set`.
Uw entiteit type *moet* worden ook een parameter zonder constructor.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Tabelbewerkingen waarbij u gebruikmaakt van entiteiten worden uitgevoerd via het **CloudTable** -object dat u eerder hebt gemaakt in de sectie 'Een tabel maken'. De bewerking moet worden uitgevoerd wordt door een **TableOperation** -object weergegeven.  Het volgende voorbeeld ziet u het maken van het object **CloudTable** en klik vervolgens op een object **CustomerEntity** .  Als u wilt voorbereiden het betrekking heeft, een object **TableOperation** gemaakt de klantentiteit in de tabel invoegen.  Ten slotte is de bewerking uitgevoerd door de ondersteuning voor **CloudTable.Execute**.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Een reeks entiteiten invoegen

U kunt een reeks entiteiten invoegen in een tabel in één bewerking voor schrijven. Enkele andere opmerkingen op batchbewerkingen:

-  U kunt uitvoeren-updates worden verwijderd en wordt ingevoegd in dezelfde één batchbewerking.
-  Een batchbewerking één kan maximaal 100 entiteiten bevatten.
-  Alle entiteiten in één batchbewerking moeten dezelfde partitiesleutel hebben.
-  Tijdens het uitvoeren van een query als een batchbewerking mogelijk is, moet het alleen betrekking heeft in de batch zijn.

<!-- -->
Het volgende voorbeeld wordt gemaakt twee entiteitsobjecten en voegt elk naar **TableBatchOperation** met behulp van de methode **Invoegen** . Vervolgens **CloudTable.Execute** aangeroepen om de bewerking niet uitvoeren.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Alle entiteiten in een partition ophalen

Als u wilt een tabel voor alle entiteiten in een partition een query uitvoert, gebruikt u een object **TableQuery** .
Het volgende voorbeeld bevat een filter voor entiteiten waar 'Smit' de toets partition is. In dit voorbeeld wordt de velden van elke entiteit in de resultaten van de query aan de console.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Een bereik van entiteiten in een partition ophalen

Als u niet dat alle entiteiten in een partition query wilt, kunt u een bereik opgeven door te combineren het partition belangrijke filter met een rij belangrijke filter. Het volgende voorbeeld wordt gebruikt twee filters om alle entiteiten in partition 'Smit' waar de rij-toets (voornaam) met een letter begint eerder dan "E" in het alfabet en klik vervolgens op resultaten van de query wordt afgedrukt.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Een enkele entiteit ophalen

U kunt een query voor het ophalen van een eenmalige, specifieke eenheid schrijven. De volgende code gebruikt **TableOperation** om op te geven van de klant 'Ben Smith'.
Deze methode retourneert één entiteit in plaats van een siteverzameling en de resulterende waarde in **TableResult.Result** is een object **CustomerEntity** .
Partition zowel rij toetsen op te geven in een query is de snelste manier om op te halen een eenheid van de tabel-service.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Een entiteit vervangen

Als u wilt bijwerken een entiteit, opgehaald van de service van de tabel, het entiteitsobject wijzigen en sla de wijzigingen weer terug naar de service van de tabel. De volgende programmacode wijzigt telefoonnummer van een bestaande klant. In plaats van de bellen **Invoegen**, met deze code wordt **vervangen**. Hierdoor wordt de entiteit volledig vervangen op de server, tenzij de entiteit op de server is niet gewijzigd sinds deze is opgehaald, zodat de bewerking, mislukt.  Deze fout wordt voorkomen dat uw toepassing een wijziging die zijn gedaan tussen de ophalen en bijwerken door een ander onderdeel van uw toepassing, automatisch worden overschreven.  De juiste afhandeling van deze fout is de entiteit opnieuw ophalen, breng uw wijzigingen (als deze nog geldig is) en vervolgens een andere **vervangen** bewerking uitvoeren.  Het volgende gedeelte wordt uitgelegd hoe u dit gedrag wijzigen.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Invoegen of-vervangen een entiteit

Bewerkingen **vervangen** loopt vast als de entiteit is gewijzigd nadat deze is opgehaald uit de server.  Daarnaast moet u de entiteit ophalen van de server eerst in om de bewerking **vervangen** te laten slagen.
Soms echter weet u niet of de entiteit op de server bestaat en de huidige waarden die zijn opgeslagen in deze niet relevant zijn. De update, moet ze allemaal overschrijven.  U kunt dit doen, gebruikt u een bewerking **InsertOrReplace** .  Deze bewerking wordt de entiteit ingevoegd als deze niet bestaat, of vervangt u deze zo ja, ongeacht de waarop de laatste update plaatsvond.  In het volgende voorbeeld, de klant entity voor Ben Smith zijn nog steeds opgehaald, maar deze vervolgens terug naar de server via **InsertOrReplace**is opgeslagen.  Wijzigingen in de entity tussen de bewerkingen ophalen en bijwerken worden overschreven.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>Query een subset van entiteitseigenschappen

Een tabelmaakquery kunt u slechts een paar eigenschappen ophalen uit een entiteit in plaats van alle entiteitseigenschappen. Deze techniek, raming, genaamd Hiermee reduceert u bandbreedte en prestaties van query's, met name voor grote entiteiten kunt verbeteren. De query in de volgende code retourneert alleen de e-mailadressen van entiteiten in de tabel. Dit gebeurt met behulp van een query van **DynamicTableEntity** en ook **EntityResolver**. U kunt meer informatie over raming op de [Inleiding tot Upsert en Query raming weblog posten][]. Houd er rekening mee dat raming niet wordt ondersteund op de lokale opslag-emulator, zodat u deze code wordt uitgevoerd alleen wanneer u een account op de tabel-service gebruikt.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Een entiteit verwijderen

Nadat u deze, met behulp van hetzelfde patroon weergegeven voor het bijwerken van een entiteit hebt opgehaald, kunt u gemakkelijk een entiteit verwijderen.  De volgende code worden opgehaald en Hiermee verwijdert u een klantentiteit.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Een tabel verwijderen

Ten slotte het volgende voorbeeld Hiermee verwijdert u een tabel uit een opslag-account. Een tabel die is verwijderd niet opnieuw te worden gemaakt voor een periode na de verwijdering beschikbaar.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Asynchroon entiteiten in pagina's ophalen

Als u een groot aantal entiteiten leest en u proces wilt/weergave entiteiten terwijl ze worden opgehaald, in plaats van wachten op deze allemaal om terug te keren, kunt u entiteiten kunt ophalen met behulp van een gesegmenteerde query. In dit voorbeeld wordt getoond hoe resultaten op pagina's met behulp van het patroon dat in afwachting van asynchrone zodat uitvoering niet is geblokkeerd terwijl u wacht om een groot aantal weer te geven resultaten worden geretourneerd. Zie voor meer informatie over het gebruik van het patroon asynchrone-Await in .NET [asynchroon programmeren met asynchrone en Await (C# en Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Table storage hebt geleerd, volgt u deze koppelingen voor meer informatie over complexere opslagtaken:

- Zie meer tabel opslag samples in [Aan de slag met Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
- De tabel verwijzing servicedocumentatie voor volledige informatie over de beschikbare API's weergeven:
    - [Opslag clientbibliotheek voor .NET-verwijzing](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Overzicht van de REST API](http://msdn.microsoft.com/library/azure/dd179355)
- Leer hoe u de code die u schrijft voor gebruik met Azure opslagruimte met behulp van de [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md) vereenvoudigen
- Bekijk meer functie hulplijnen voor meer informatie over extra opties voor het opslaan van gegevens in Azure wordt aangegeven.
    - [Aan de slag met Azure-blobopslag met .NET](storage-dotnet-how-to-use-blobs.md) ongestructureerde gegevens wilt opslaan.
    - [Verbinding maken met SQL-Database met behulp van .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) voor de opslag van relationele gegevens.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Kennismaking Upsert en Query raming blogbericht]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage
