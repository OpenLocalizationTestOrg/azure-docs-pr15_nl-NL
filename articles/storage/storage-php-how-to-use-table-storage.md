<properties
    pageTitle="Het gebruik van tabelopslag van PHP | Microsoft Azure"
    description="Meer informatie over het gebruik van de tabel-service van PHP maken en verwijderen van een tabel, en invoegen, verwijderen en de tabel query."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>Het gebruik van PHP-tabelopslag

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Deze handleiding ziet u hoe u het uitvoeren van veelvoorkomende scenario's met de tabel Azure-service. In de voorbeelden in PHP zijn geschreven en gebruiken van de [Azure SDK voor PHP][download]. De scenario's waarvoor opnemen **maken en verwijderen van een tabel, en invoegen, verwijderen, en query's uitvoeren entiteiten in een tabel**. Zie voor meer informatie over de service Azure tabel, de sectie van de [volgende stappen](#next-steps) .

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken

De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de service Azure-tabel is de verwijzing naar klassen in de SDK Azure voor PHP uit uw code. U kunt elke extra ontwikkeling maken uw toepassing, zoals Kladblok.

In deze handleiding gebruikt u de service tabelfuncties die kunnen worden aangeroepen vanuit binnen een PHP-toepassing lokaal of in code die wordt uitgevoerd vanuit een Azure Webrol, werknemer rol of website.

## <a name="get-the-azure-client-libraries"></a>Ophalen van de Azure clientbibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Uw toepassing voor de tabel-service configureren

Als u wilt de tabel Azure-service API's gebruikt, moet u:

1. Een verwijzing naar de autoloader-bestand met de [require_once] [ require_once] instructie, en
2. Overzicht van alle klassen die u kunt gebruiken.

Het volgende voorbeeld ziet u hoe u het bestand autoloader nemen en om te verwijzen naar de klasse **ServicesBuilder** .

> [AZURE.NOTE] In dit voorbeeld (en andere voorbeelden in dit artikel) wordt ervan uitgegaan dat u de bibliotheken van de Client PHP voor Azure via Composer hebt geïnstalleerd. Als u de bibliotheken handmatig hebt geïnstalleerd, moet u verwijzen naar de <code>WindowsAzure.php</code> autoloader-bestand.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


In de onderstaande voorbeelden de `require_once` instructie wordt altijd weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uitvoeren wordt verwezen.

## <a name="set-up-an-azure-storage-connection"></a>Een verbinding Azure opslag instellen

Als u wilt een tabel van Azure-service-client wordt gestart, moet u eerst een geldige verbindingsreeks hebben. De notatie voor de verbindingsreeks van de tabel-service is:

Voor toegang tot een live service:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Voor toegang tot de opslag emulator:

    UseDevelopmentStorage=true


Als u wilt maken van een serviceclient Azure-, moet u de klasse **ServicesBuilder** gebruiken. U kunt:

* doorgeven de verbindingsreeks rechtstreeks of
* Gebruik de **CloudConfigurationManager (CCM)** om te controleren van meerdere externe bronnen voor de verbindingsreeks:
    * Standaard wordt geleverd met ondersteuning voor één externe bron - milieu variabelen
    * u kunt nieuwe bronnen toevoegen door de klasse **ConnectionStringSource** uitbreiden

Voor de voorbeelden hier beschreven, wordt de verbindingsreeks rechtstreeks worden doorgegeven.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Een tabel maken

Een object **TableRestProxy** kunt u een tabel maken met de methode **createTable** . Wanneer u een tabel maakt, kunt u de tabel service time-out instellen. (Zie voor meer informatie over de tabel service time-out [Instelling time-out voor tabel servicebewerkingen][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Zie [informatie over het gegevensmodel van de tabel-Service]voor informatie over beperkingen op tabelnamen[table-data-model].

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u wilt een entiteit toevoegen aan een tabel, een nieuw **entiteit** -object maken en doorgegeven aan **TableRestProxy -> insertEntity**. Houd er rekening mee dat wanneer u een entiteit maakt, moet u een `PartitionKey` en `RowKey`. Dit zijn de unieke id's voor een entiteit en waarden die veel sneller dan andere entiteitseigenschappen kunnen worden doorzocht. Gebruikt het systeem `PartitionKey` van de tabel entiteiten automatisch verdeeld over veel opslagruimte knooppunten. Entiteiten met dezelfde `PartitionKey` zijn opgeslagen op hetzelfde knooppunt. (Bewerkingen op meerdere entiteiten die zijn opgeslagen op hetzelfde knooppunt uitvoeren beter dan op entiteiten die zijn opgeslagen op verschillende knooppunten.) De `RowKey` is de unieke ID van een entiteit binnen een partition.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Zie [informatie over het gegevensmodel van de tabel-Service]voor informatie over tabeleigenschappen en typen[table-data-model].

De klasse **TableRestProxy** biedt twee methoden voor het invoegen van entiteiten: **insertOrMergeEntity** en **insertOrReplaceEntity**. Als u deze methoden, een nieuwe **entiteit** maakt en geeft u deze als een parameter twee methoden. Elke methode wordt de entiteit ingevoegd als deze nog niet bestaat. Als de entiteit al bestaat, **insertOrMergeEntity** eigenschapswaarden bijgewerkt als de eigenschappen al aanwezig en wordt nieuwe eigenschappen toegevoegd als deze nog niet bestaan, terwijl **insertOrReplaceEntity** Hiermee vervangt u een bestaande entiteit. Het volgende voorbeeld ziet u hoe u **insertOrMergeEntity**gebruikt. Als de entiteit met `PartitionKey` "tasksSeattle" en `RowKey` '1' nog niet bestaat, wordt ingevoegd. Echter als het eerder is ingevoegd (zoals weergegeven in het bovenstaande voorbeeld), de `DueDate` eigenschap wordt bijgewerkt, en de `Status` eigenschap wordt toegevoegd. De `Description` en `Location` eigenschappen ook worden bijgewerkt, maar met waarden die effectief laten hen ongewijzigd. Als deze laatste twee eigenschappen zijn niet toegevoegd, zoals in het voorbeeld, maar bestaan in de doelentiteit, zou hun bestaande waarden ongewijzigd blijven.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Een enkele entiteit ophalen

De methode **TableRestProxy -> getEntity** kunt u één enkele entiteit ophalen via het opvragen van de `PartitionKey` en `RowKey`. Klik in het volgende voorbeeld wordt de toets partition `tasksSeattle` en rijsleutel `1` worden doorgegeven aan de methode **getEntity** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Alle entiteiten in een partition ophalen

Entiteit-query's worden gemaakt met behulp van filters (Zie voor meer informatie, [query's uitvoeren tabellen en entiteiten][filters]). Als u wilt ophalen alle entiteiten in partition, gebruikt u het filter "PartitionKey eq *partitienaam*". Het volgende voorbeeld ziet u het ophalen van alle entiteiten in de `tasksSeattle` partition door te geven van een filter aan de methode **queryEntities** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Een subset van entiteiten in een partition ophalen

Hetzelfde patroon in het vorige voorbeeld gebruikt, kan worden gebruikt om op te halen, een subset van entiteiten in een partition. De subset van entiteiten u ophalen wordt bepaald door het filter dat u gebruikt (Zie voor meer informatie, [query's uitvoeren tabellen en entiteiten][filters]). Het volgende voorbeeld ziet u hoe u een filter gebruiken om op te halen, alle entiteiten met een specifiek `Location` en een `DueDate` kleiner is dan een bepaalde datum.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Een subset met entiteitseigenschappen ophalen

Een query kunt een subset van entiteitseigenschappen ophalen. Deze techniek, *raming*, genaamd Hiermee reduceert u bandbreedte en prestaties van query's, met name voor grote entiteiten kunt verbeteren. Als u een eigenschap moeten worden opgehaald, geeft u de naam van de eigenschap aan de **Query -> addSelectField** -methode. U kunt deze methode meerdere keren bellen om toe te voegen meer eigenschappen. Na het uitvoeren van **TableRestProxy -> queryEntities**, heeft de geretourneerde entiteiten alleen de geselecteerde eigenschappen. (Als u een subset van de tabel entiteiten retourneren wilt, gebruikt u een filter zoals wordt weergegeven in de bovenstaande query's.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Een entiteit bijwerken

Een bestaande entiteit kan worden bijgewerkt met de methoden **entiteit -> EigenschapInstellen** en **entiteit -> addProperty** op de entiteit, en vervolgens roepen **TableRestProxy -> updateEntity**. Het volgende voorbeeld een entiteit zijn opgehaald, wijzigt van één eigenschap verwijdert nog een eigenschap en wordt een nieuwe eigenschap toegevoegd. Houd er rekening mee dat u een eigenschap verwijderen kunt door de waarde instellen op **null**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Een entiteit verwijderen

Als u wilt verwijderen van een entiteit, geven de tabelnaam en van de entiteit `PartitionKey` en `RowKey` met de methode **TableRestProxy -> deleteEntity** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Houd er rekening mee dat voor gelijktijdigheid controles, u de Etag voor een entiteit zijn verwijderd instellen kunt door met de methode **DeleteEntityOptions -> setEtag** en doorgeven van het object **DeleteEntityOptions** aan **deleteEntity** als een vierde parameter.

## <a name="batch-table-operations"></a>Batch tabelbewerkingen

De methode **TableRestProxy -> batch** kunt u meerdere bewerkingen uitvoeren in een enkele aanvraag. Het patroon hier houdt bewerkingen toevoegen aan **BatchRequest** object en klikt u vervolgens het object **BatchRequest** doorgeven aan de methode **TableRestProxy -> batch** . Als u wilt een bewerking toevoegen aan een object **BatchRequest** , kunt u bellen een van de volgende methoden meerdere keren:

* **addInsertEntity** (een bewerking insertEntity toevoegen)
* **addUpdateEntity** (een bewerking updateEntity toevoegen)
* **addMergeEntity** (een bewerking mergeEntity toevoegen)
* **addInsertOrReplaceEntity** (een bewerking insertOrReplaceEntity toevoegen)
* **addInsertOrMergeEntity** (een bewerking insertOrMergeEntity toevoegen)
* **addDeleteEntity** (een bewerking deleteEntity toevoegen)

Het volgende voorbeeld ziet u hoe **insertEntity** en **deleteEntity** bewerkingen uitvoeren in een enkele aanvraag:

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Zie voor meer informatie over batchen tabelbewerkingen, [Uitvoering entiteit groepstransacties][entity-group-transactions].

## <a name="delete-a-table"></a>Een tabel verwijderen

Ten slotte, als u wilt een tabel verwijdert, geeft u de naam van de tabel aan de methode **TableRestProxy -> deleteTable** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van de tabel Azure-service hebt geleerd, volgt u deze koppelingen voor meer informatie over opslagtaken voor meer complexe.

- Ga naar de [opslag van Azure-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

Zie ook het [PHP Developer Center](/develop/php/)voor meer informatie.

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
