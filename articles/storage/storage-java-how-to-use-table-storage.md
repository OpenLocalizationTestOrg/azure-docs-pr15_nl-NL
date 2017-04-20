<properties
    pageTitle="Het gebruik van Table storage uit Java | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud met Azure-tabelopslag, een gegevensopslag NoSQL."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-java"></a>Het gebruik van Java-tabelopslag

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Deze handleiding leert u hoe u veelvoorkomende scenario's voor het gebruik van de Azure tabel storage-service uitvoert. In de voorbeelden in Java zijn geschreven en gebruik van de [Azure opslag SDK for Java][]. De scenario's waarvoor opnemen **maken**, **weergeven**en **verwijderen van** tabellen, evenals **Invoegen**, **query's uitvoeren**, **wijzigen**en **verwijderen van** entiteiten in een tabel. Zie voor meer informatie over tabellen, de sectie van de [volgende stappen](#Next-Steps) .

Opmerking: Een SDK is beschikbaar voor ontwikkelaars die Azure opslag op Android-apparaten gebruikt. Zie de [SDK van Azure-opslag voor Android][]voor meer informatie.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Een Java-toepassing maken

In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een Java-toepassing lokaal of in code die wordt uitgevoerd vanuit een Webrol of werknemer rol in Azure wordt aangegeven.

Hiervoor moet u voor het installeren van de Java Development Kit (JDK) en een account Azure opslagruimte in uw Azure abonnement maken. Nadat u dit hebt gedaan, moet u controleren of uw systeem voor de ontwikkeling voldoet aan de minimale vereisten en afhankelijkheden die worden weergegeven in de bibliotheek [Azure opslag SDK for Java][] op GitHub. Als uw systeem aan deze vereisten voldoet, kunt u de instructies voor het downloaden en installeren van de Azure opslag bibliotheken voor Java op uw systeem van die opslagplaats. Als u deze taken hebt voltooid, is mogelijk te maken van een Java-toepassing waarin de voorbeelden in dit artikel.

## <a name="configure-your-application-to-access-table-storage"></a>Uw toepassing voor toegang tot tabelopslag configureren

De volgende importinstructies toevoegen aan het begin van het gewenste waar Microsoft Azure-opslag API's gebruiken voor toegang tot tabellen Java-bestand:

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

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

## <a name="how-to-create-a-table"></a>Hoe u: een tabel maken

Een object **CloudTableClient** kunt u verwijzingsobjecten voor tabellen en entiteiten ophalen. De volgende code maakt een object **CloudTableClient** en wordt deze gebruikt om een nieuwe **CloudTable** -object waarmee een tabel met de naam "personen" te maken. (Notitie: Er zijn andere manieren om te maken van **CloudStorageAccount** objecten; Zie **CloudStorageAccount** in de [Azure opslag Client SDK verwijzing]voor meer informatie.)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>Hoe u: de tabellen

Als u een lijst met tabellen, belt u de methode **CloudTableClient.listTables()** om op te halen een iterable lijst met tabelnamen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>Hoe u: een entiteit toevoegen aan een tabel

Entiteiten toewijzen aan Java-objecten met behulp van een aangepaste klasse **TableEntity**implementeren. Uitkomt, de klas **TableServiceEntity** **TableEntity** implementeert en weerspiegeling eigenschappen toe te wijzen aan methoden getter en setter met de naam voor de eigenschappen gebruikt. Als u wilt een entiteit aan een tabel toevoegt, moet u eerst een klasse die definieert de eigenschappen van uw entiteit maken. De volgende code definieert een entity-klasse die de voornaam van de klant als de rij-toets vast en achternaam als de partitiesleutel gebruikt. Samen en identificatie van een entiteit partition en rijsleutel unieke de entiteit in de tabel. Entiteiten met dezelfde partitiesleutel kunnen sneller dan bij andere partition toetsen worden doorzocht.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Tabelbewerkingen die betrekking hebben op personen entiteiten vereist een object **TableOperation** . Dit object definieert de bewerking moet worden uitgevoerd op een entiteit, dat kan worden uitgevoerd met een object **CloudTable** . De volgende code maakt een nieuw exemplaar van de klasse **CustomerEntity** met enkele klantgegevens worden opgeslagen. De code vervolgens belt **TableOperation.insertOrReplace** als u wilt maken van een object **TableOperation** als u wilt een entiteit in een tabel invoegen en de nieuwe **CustomerEntity** gekoppeld. De code oproepen ten slotte de methode **uitvoeren** op de **CloudTable** -object, waarbij de tabel "personen" en de nieuwe **TableOperation**, waarin stuurt een verzoek om naar de storage-service voor de nieuwe klantentiteit in de tabel "personen" invoegen of de entiteit vervangen als deze al bestaat.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>Hoe u: een reeks entiteiten invoegen

U kunt een reeks entiteiten met de service tabel invoegen in één bewerking voor schrijven. De volgende code wordt gemaakt van een object **TableBatchOperation** en worden vervolgens opgeteld dat drie bewerkingen wilt invoegen. Elke invoegbewerking is toegevoegd door er een nieuw entiteitsobject, het instellen van de waarden wilt opzoeken en vervolgens aanroepen van de methode **Invoegen** op het object **TableBatchOperation** de entiteit koppelen aan een nieuwe invoegbewerking. Vervolgens oproepen de code **uitvoeren** op het object **CloudTable** , waarbij de tabel 'personen' en het object **TableBatchOperation** , waarmee de batch van tabelbewerkingen wordt verzonden naar de storage-service in één aanvraag opgeeft.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Enkele dingen die u moet de opmerking over de batchbewerkingen:

- U kunt uitvoeren van maximaal 100 invoegen, verwijderen, samenvoegen, vervangen, invoegen of samenvoegen, en invoegen of bewerkingen in een willekeurige combinatie in één batch vervangen.
- Een batchbewerking kan een bewerking ophalen, hebben als dit de enige bewerking in de batch is.
- Alle entiteiten in één batchbewerking moeten dezelfde partitiesleutel hebben.
- Een batchbewerking is beperkt tot een nettolading 4MB.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Hoe u: alle entiteiten in een partition ophalen

Als u wilt een tabel voor entiteiten in een partition query, kunt u een **TableQuery**. Bel **TableQuery.from** om te maken van een query in een bepaalde tabel die resulteert in een opgegeven resultaattype. De volgende code bevat een filter voor entiteiten waar 'Smit' de toets partition is. **TableQuery.generateFilterCondition** is een helpmethode maken van filters voor query's. Bel **waar** in de verwijzing die is geretourneerd door de methode **TableQuery.from** het filter toepassen op de query. Wanneer de query wordt uitgevoerd via een oproep **uitvoeren** op het object **CloudTable** , wordt een **Iterator** met het resultaattype **CustomerEntity** is opgegeven. U kunt de **Iterator** geretourneerd door een voor elke lus de resultaten in beslag neemt. Deze code wordt afgedrukt de velden van elke entiteit in de resultaten van de query aan de console.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Hoe u: een bereik van entiteiten in een partition ophalen

Als u niet dat alle entiteiten in een partition query wilt, kunt u een bereik met behulp van vergelijkingsoperatoren in een filter. De volgende code combineert twee filters voor alle entiteiten in partition 'Smit' waar de rij-toets (voornaam) met een letter maximaal "E begint" in het alfabet. Vervolgens wordt de tekening afgedrukt de queryresultaten. Als u de entiteiten toegevoegd aan de tabel in de batch invoegen sectie van deze handleiding, slechts twee entiteiten worden geretourneerd ditmaal (Ben en Denise Smith); Jeff Smith is niet opgenomen.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>Hoe u: één enkele entiteit ophalen

U kunt een query voor het ophalen van een eenmalige, specifieke eenheid schrijven. De volgende code oproepen **TableOperation.retrieve** met partitiesleutel en rij belangrijke parameters om op te geven van de klant "Jeff Smith', in plaats van een **TableQuery** maken en gebruiken van filters om hetzelfde te doen. Tijdens de uitvoering van geeft de bewerking ophalen slechts één entiteit, in plaats van een siteverzameling. De methode **getResultAsType** zet het resultaat met het type van het doel van de toewijzing, een object **CustomerEntity** . Als dit type niet compatibel met het type dat is opgegeven voor de query is, wordt een uitzondering worden gegenereerd. Als geen entiteit heeft een exacte partition en de rijsleutel overeenkomen met een null-waarde geretourneerd. Partition zowel rij toetsen op te geven in een query is de snelste manier om op te halen een eenheid van de tabel-service.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>Hoe u: een entiteit wijzigen

Als u wilt wijzigen van een entiteit, opgehaald van de tabel-service, wijzigingen aanbrengen in het entiteitsobject en sla de wijzigingen weer terug naar de service van de tabel met een bewerking vervangen of samenvoegen. De volgende programmacode wijzigt telefoonnummer van een bestaande klant. In plaats van de bellen **TableOperation.insert** zoals we hebben gedaan als u wilt invoegen, oproepen deze code **TableOperation.replace**. De methode **CloudTable.execute** roept de service van de tabel en de entiteit wordt vervangen, tenzij een andere toepassing niet gewijzigd deze in de tijd sinds deze toepassing opgehaald deze. In dat geval een uitzondering is opgetreden en de entiteit moet worden opgehaald, gewijzigd en opnieuw hebt opgeslagen. Dit optimistische gelijktijdigheid opnieuw patroon is algemene in systeem verdeelde opslag.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>Hoe u: Query een subset van entiteitseigenschappen

Een query toevoegen aan een tabel kunt u slechts een paar eigenschappen ophalen uit een entiteit. Deze techniek, raming, genaamd Hiermee reduceert u bandbreedte en prestaties van query's, met name voor grote entiteiten kunt verbeteren. De query in de volgende code wordt de methode **selecteert u** alleen de e-mailadressen van entiteiten in de tabel te retourneren. De resultaten zijn geprojecteerd in een verzameling **tekenreeks** met behulp van een **EntityResolver**, zoals in de typeconversie van het bij de entiteiten geretourneerd door de server. U kunt meer informatie over raming in [Azure-tabellen: Inleiding tot Upsert en Query raming][]. Houd er rekening mee dat raming niet wordt ondersteund op de lokale opslag-emulator, zodat u deze code wordt uitgevoerd alleen bij gebruik van een account in de tabel-service.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>Hoe u: invoegen of een entiteit vervangen

Vaak wilt u een entiteit toevoegen aan een tabel zonder te weten te komen of deze al in de tabel bestaat. Een bewerking invoegen of vervangen kunt u enkele vragen die de entiteit wordt ingevoegd als deze bestaat niet of het bestaande bestand vervangen als dat zo is. Maken op de voorgaande voorbeelden, de volgende code wordt ingevoegd of vervangen van de entiteit voor "Walter Harp". Na het maken van een nieuwe entiteit, roept deze code de methode **TableOperation.insertOrReplace** . Deze code vervolgens oproepen **uitvoeren** op het object **CloudTable** met de tabel en het invoegen of -bewerking tabel als de parameters vervangen. Als u slechts een deel van een entiteit bijwerken, kan de methode **TableOperation.insertOrMerge** in plaats daarvan kan worden gebruikt. Houd er rekening mee dat invoegen of-vervangen wordt niet ondersteund op de lokale opslag-emulator, zodat u deze code wordt uitgevoerd alleen bij gebruik van een account in de tabel-service. U kunt meer informatie over invoegen of vervangen en invoegen of-samenvoegen in deze [Azure-tabellen: Inleiding tot Upsert en Query raming][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>Hoe u: een entiteit verwijderen

Nadat u deze hebt opgehaald, kunt u gemakkelijk een entiteit verwijderen. Zodra de entiteit is opgehaald, belt u **TableOperation.delete** aan de entiteit te verwijderen. Contact opnemen met **uitvoeren** op het object **CloudTable** . De volgende code worden opgehaald en Hiermee verwijdert u een klantentiteit.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>Hoe u: een tabel verwijderen

De volgende code wordt ten slotte een tabel verwijdert uit een opslag-account. Een tabel die is verwijderd niet beschikbaar worden gemaakt voor een periode na de verwijdering, meestal minder dan 40 seconden.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van tabelopslag hebt geleerd, volgt u deze koppelingen om te leren complexere opslagtaken uitvoeren.

- [Azure opslag SDK for Java][]
- [Azure opslag Client SDK verwijzing][]
- [Azure opslag REST API][]
- [Azure opslag teamblog][]

Zie ook het [Java Developer Center](/develop/java/)voor meer informatie.


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure opslag SDK for Java]: https://github.com/azure/azure-storage-java
[Azure opslag SDK voor Android]: https://github.com/azure/azure-storage-android
[Azure opslag Client SDK verwijzing]: http://dl.windowsazure.com/storage/javadoc/
[Azure opslag REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure opslag teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure tabellen: Inleiding tot Upsert en Query raming]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
