<properties
    pageTitle="Aan de slag met tabelopslag en Visual Studio verbonden services (ASP.NET) | Microsoft Azure"
    description="Hoe u aan de slag met Azure-tabelopslag in een ASP.NET-project in Visual Studio nadat de verbinding met een opslag-account gebruik van Visual Studio services verbonden"
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

# <a name="get-started-with-table-storage-and-visual-studio-connected-services-aspnet"></a>Aan de slag met tabelopslag en Visual Studio verbonden services (ASP.NET)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht
In dit artikel wordt beschreven hoe aan de slag met Azure-tabelopslag in Visual Studio nadat u hebt gemaakt of een account Azure opslag in een ASP.NET-project waarnaar wordt verwezen in het dialoogvenster Visual Studio **Verbonden Services toevoegen** . In dit artikel leest u hoe u veelvoorkomende taken kunt uitvoeren in Azure-tabellen, inclusief maken en verwijderen van een tabel, evenals werken met tabel entiteiten. In de voorbeelden zijn geschreven in C\# code en het gebruik van de [Bibliotheek van Microsoft Azure opslag Client voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx). Voor algemene informatie over het gebruik van Azure opslag van een tabel, raadpleegt u [aan de slag met Azure-tabelopslag met .NET](storage-dotnet-how-to-use-tables.md).

Azure-tabelopslag kunt u voor de opslag van grote hoeveelheden gestructureerde gegevens. De service is een NoSQL gegevensopslag die geverifieerde oproepen van binnen en buiten de Azure cloud accepteert. Azure tabellen zijn geschikt om gestructureerde, niet-relationele gegevens op te slaan.


## <a name="access-tables-in-code"></a>Access-tabellen in code

1. Zorg ervoor dat de naamruimtedeclaraties boven aan het bestand C# opnemen deze instructies **gebruiken** .

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag-account. De volgende code gebruiken om de uw opslagruimte verbindingsreeks en de accountgegevens opslag van de Azure-service te configureren.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Opmerking** : alle bovenstaande code vóór de code in de volgende voorbeelden te gebruiken.

3. Een object **CloudTableClient** om te verwijzen naar de tabelobjecten in uw account opslag krijgen.  

        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Een object **CloudTable** verwijzing verwijzen naar een bepaalde tabel en entiteiten krijgen.

        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Een tabel maken in code

Als u wilt de Azure tabel hebt gemaakt, net een oproep toevoegen aan **CreateIfNotExistsAsync()** naar de vorige code.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u wilt een entiteit toevoegen aan een tabel kunt u een klasse die definieert de eigenschappen van uw entiteit maken. De volgende code definieert een entity-klasse **CustomerEntity** die gebruikmaakt van de voornaam van de klant als de rijsleutel en de achternaam als partitiesleutel genoemd.

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

Tabelbewerkingen die betrekking hebben op personen entiteiten zijn gereed het gebruik van de **CloudTable** object dat u eerder in 'Access-tabellen in de code.' gemaakt Het object **TableOperation** vertegenwoordigt de bewerking te worden uitgevoerd. Het volgende voorbeeld ziet u hoe u een object **CloudTable** en een object **CustomerEntity** maakt. Als u wilt voorbereiden het betrekking heeft, wordt een **TableOperation** gemaakt als u wilt de klantentiteit in de tabel invoegen. Ten slotte is de bewerking uitgevoerd door de ondersteuning voor CloudTable.ExecuteAsync.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Een reeks entiteiten invoegen

U kunt meerdere entiteiten invoegen in een tabel in een bewerking één schrijven. Het volgende voorbeeld wordt gemaakt van twee entiteitsobjecten ("Jeff Smith" en "Ben Smith"), toevoegt aan een **TableBatchOperation** -object met de methode invoegen en begint vervolgens met de bewerking door te bellen **CloudTable.ExecuteBatchAsync**.

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Alle entiteiten in een partition ophalen
Als u wilt een tabel voor alle van de entiteiten in een partition query, gebruikt u een object **TableQuery** . Het volgende voorbeeld bevat een filter voor entiteiten waar 'Smit' de toets partition is. In dit voorbeeld wordt de velden van elke entiteit in de resultaten van de query aan de console.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity>
        resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
        }
        } while (token != null);

        return View();


## <a name="get-a-single-entity"></a>Een enkele entiteit ophalen
U kunt een query als u een eenmalige, specifieke eenheid schrijven. De volgende code wordt een object **TableOperation** om op te geven van een klant met de naam 'Ben Smith'. Deze methode geeft als resultaat één entiteit in plaats van een siteverzameling en de resulterende waarde in **TableResult.Result** is een object **CustomerEntity** . Partition zowel rij toetsen op te geven in een query is de snelste manier om op te halen een eenheid van de tabel-service.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);
    
    // Print the phone number of the result.
    if (retrievedResult.Result != null)
        Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Een entiteit verwijderen
Nadat u deze hebt gevonden, kunt u een entiteit verwijderen. De volgende code Hiermee wordt gezocht naar een klantentiteit met de naam "Ben Smith" en als deze worden gevonden, wordt de snelkoppeling verwijderd.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
