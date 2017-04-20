<properties
    pageTitle="Aan de slag met tabelopslag en Visual Studio verbonden services (cloudservices) | Microsoft Azure"
    description="Hoe u aan de slag met Azure-tabelopslag in een project cloud-service in Visual Studio nadat de verbinding met een opslag-account gebruik van Visual Studio services verbonden"
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

# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Aan de slag met Azure-tabelopslag en Visual Studio verbonden services (cloud services projecten)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

##<a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe aan de slag met Azure-tabelopslag in Visual Studio nadat u hebt gemaakt of een account Azure opslag in een cloud services-project waarnaar wordt verwezen in het dialoogvenster Visual Studio **Verbonden Services toevoegen** . De bewerking **Verbonden Services toevoegen** de juiste NuGet-pakketten voor toegang tot Azure opslagruimte in uw project is geïnstalleerd en voegt de verbindingsreeks voor het account opslagruimte aan uw project configuratie-bestanden.

De tabel Azure storage-service kunt u voor de opslag van grote hoeveelheden gestructureerde gegevens. De service is een NoSQL gegevensopslag die geverifieerde oproepen van binnen en buiten de Azure cloud accepteert. Azure tabellen zijn geschikt om gestructureerde, niet-relationele gegevens op te slaan.

Als u wilt beginnen, moet u eerst een tabel maken in uw account opslag. Leert u hoe u een Azure-tabel maakt in code en ook hoe u de basistabel en entiteit-bewerkingen uitvoeren, zoals toevoegen, wijzigen, lezen en lezen tabel entiteiten uitvoeren. In de voorbeelden zijn geschreven in C\# code en het gebruik van de [bibliotheek van Microsoft Azure Storage-client voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**NOTITIE:** Enkele van de API's die uitvoeren van oproepen af naar Azure opslagruimte zijn asynchroon. Zie [asynchroon asynchrone en Await programmeren](http://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De onderstaande code wordt ervan uitgegaan asynchrone programming methoden worden gebruikt.

- Zie [aan de slag met Azure-tabelopslag met .NET](storage-dotnet-how-to-use-tables.md) voor meer informatie over het bewerken van programmacode tabellen.
- Zie [opslagruimte documentatie](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over de opslag van Azure.
- Zie [Cloud Services-documentatie](https://azure.microsoft.com/documentation/services/cloud-services/) voor algemene informatie over Azure cloudservices.
- Zie [ASP.NET](http://www.asp.net) voor meer informatie over het programmeren ASP.NET-toepassingen.

## <a name="access-tables-in-code"></a>Access-tabellen in code

Voor toegang tot tabellen in de cloud serviceprojecten, moet u de volgende items aan alle C# bronbestanden die toegang Azure-tabelopslag tot bevatten.

1. Zorg ervoor dat de naamruimtedeclaraties boven aan het bestand C# opnemen deze instructies **gebruiken** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag-account. Gebruik de volgende code aan de verbindingsreeks van de opslag en accountgegevens opslag van de Azure-service te configureren.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
> [AZURE.NOTE]  Voer alle bovenstaande code vóór de code in de volgende voorbeelden.

3. Een object **CloudTableClient** om te verwijzen naar de tabelobjecten in uw account opslag krijgen.

         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Een object **CloudTable** verwijzing verwijzen naar een bepaalde tabel en entiteiten krijgen.

        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Een tabel maken in code

Als u wilt de Azure tabel hebt gemaakt, toe te voegen een oproep naar **CreateIfNotExistsAsync** naar de nadat u een object **CloudTable** hebt ontvangen, zoals beschreven in de sectie "Toegang tabellen in code".

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u wilt een entiteit toevoegen aan een tabel, een klasse maken die de eigenschappen van uw entiteit definieert. De volgende code definieert een entity-klasse **CustomerEntity** die gebruikmaakt van de voornaam van de klant als de rijsleutel en de achternaam als partitiesleutel genoemd.

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

Tabelbewerkingen die betrekking hebben op personen entiteiten klaar bent met het **CloudTable** -object dat u eerder hebt gemaakt in "toegang tot tabellen in de code." Het object **TableOperation** vertegenwoordigt de bewerking te worden uitgevoerd. Het volgende voorbeeld ziet u hoe u een object **CloudTable** en een object **CustomerEntity** maakt. Als u wilt voorbereiden het betrekking heeft, wordt een **TableOperation** gemaakt als u wilt de klantentiteit in de tabel invoegen. Ten slotte is de bewerking uitgevoerd door de ondersteuning voor **CloudTable.ExecuteAsync**.

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
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

    return View();


## <a name="get-a-single-entity"></a>Een enkele entiteit ophalen

U kunt een query als u een eenmalige, specifieke eenheid schrijven. De volgende code wordt een object **TableOperation** om op te geven van een klant met de naam 'Ben Smith'. Deze methode geeft als resultaat één entiteit in plaats van een siteverzameling en de resulterende waarde in **TableResult.Result** is een object **CustomerEntity** . Partition zowel rij toetsen op te geven in een query is de snelste manier om op te halen een eenheid van de **tabel** -service.

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
Nadat u deze hebt gevonden, kunt u een entiteit verwijderen. De volgende code Hiermee wordt gezocht naar een klantentiteit met de naam 'Ben Smit' en als deze worden gevonden, wordt de snelkoppeling verwijderd.

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
