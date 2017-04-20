<properties
    pageTitle="Azure functies triggers en bindingen voor Azure Storage | Microsoft Azure"
    description="Meer informatie over het gebruik van Azure Storage triggers en bindingen in Azure-functies."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure werkt, functies, verwerking van gebeurtenis, dynamische berekeningscluster, als u kiest architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure functies triggers en bindingen voor de opslag van Azure

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe u configureert en code Azure Storage triggers en bindingen in Azure-functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Azure opslag wachtrij trigger

#### <a name="functionjson-for-storage-queue-trigger"></a>Function.JSON voor opslag wachtrij trigger

Het bestand *function.json* Hiermee geeft u de volgende eigenschappen.

- `name`: De naam van de variabele gebruikt in de functiecode voor de wachtrij of het bericht wachtrij. 
- `queueName`: De naam van de wachtrij worden gecontroleerd. Zie voor wachtrij naming regels, [naamgevingsconventies wachtrijen en metagegevens](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: De naam van de instelling voor een app waarin een verbindingsreeks opslag. Als u laat `connection` leeg en de inwerkingtreding werkt met de verbindingsreeks van de standaard-opslag voor de functie-app, dat is opgegeven door de instelling van de app AzureWebJobsStorage.
- `type`: Moet zijn ingesteld op *queueTrigger*.
- `direction`: Moet worden ingesteld *in*. 

Voorbeeld *function.json* voor een opslagruimte wachtrij trigger:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Wachtrij trigger ondersteunde typen

Het bericht wachtrij kan worden gedeserialiseerd naar een van de volgende typen:

* Object (van JSON)
* Tekenreeks
* Byte-matrix 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>Wachtrij trigger metagegevens

U kunt wachtrij metagegevens op uw functie verkrijgen met behulp van de namen van deze variabelen:

* expirationTime
* insertionTime
* nextVisibleTime
* ID
* popReceipt
* dequeueCount
* queueTrigger (een andere manier om op te halen van de tekst van het bericht wachtrij als een tekenreeks)

In dit voorbeeld C#-code wordt opgehaald en logboeken wachtrij metagegevens:

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Verwerking van berichten poison wachtrij plaatsen

Berichten waarvan de inhoud wordt door een functie mislukt heten *poison berichten*. Wanneer de functie mislukt, wordt het bericht wachtrij wordt niet verwijderd en uiteindelijk wordt opgehaald nogmaals veroorzaakt door de cyclus worden herhaald. De SDK kan automatisch de cyclus worden onderbroken na een beperkt aantal iteraties of u deze handmatig kunt doen.

De SDK wordt een functie maximaal 5 keer aanroep aan een bericht wachtrij verwerken. Als het vijfde uitproberen mislukt, wordt het bericht wordt verplaatst naar een poison wachtrij.

De poison wachtrij heet *{originalqueuename}*-poison. Een functie op berichten uit de poison wachtrij door ze logboekregistratie of het verzenden van een melding dat handmatige aandacht nodig is, kunt u schrijven. 

Als u poison berichten handmatig verwerken wilt, kunt u het aantal keren een bericht is opgenomen omhoog voor verwerking verkrijgen door te schakelen `dequeueCount`.

## <a id="storagequeueoutput"></a>Azure opslag wachtrij uitvoer binding

#### <a name="functionjson-for-storage-queue-output-binding"></a>Function.JSON voor opslag-wachtrij uitvoer binding

Het bestand *function.json* Hiermee geeft u de volgende eigenschappen.

- `name`: De naam van de variabele gebruikt in de functiecode voor de wachtrij of het bericht wachtrij. 
- `queueName`: De naam van de wachtrij. Zie voor wachtrij naming regels, [naamgevingsconventies wachtrijen en metagegevens](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: De naam van de instelling voor een app waarin een verbindingsreeks opslag. Als u laat `connection` leeg en de inwerkingtreding werkt met de verbindingsreeks van de standaard-opslag voor de functie-app, dat is opgegeven door de instelling van de app AzureWebJobsStorage.
- `type`: Moet zijn ingesteld op *wachtrij*.
- `direction`: Moet zijn ingesteld op *af*. 

Voorbeeld *function.json* voor een wachtrij opslag uitvoer binden die gebruikmaakt van een trigger wachtrij en een wachtrij bericht schrijft:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Wachtrij uitvoer binding ondersteund typen

De `queue` binding kan de volgende typen aan een bericht wachtrij serialiseren:

* Object (`out T` in C#, maakt u een bericht met een null-object als de parameter null is wanneer de functie eindigt)
* Tekenreeks (`out string` in C#, gemaakt wachtrij bericht als parameterwaarde geen null-is wanneer de functie eindigt)
* Byte-matrix (`out byte[]` in C#, werkt als tekenreeks) 
* `out CloudQueueMessage`(C#, werkt als tekenreeks) 

In C# kunt u ook binden aan `ICollector<T>` of `IAsyncCollector<T>` waar `T` is een van de ondersteunde typen.

#### <a name="queue-output-binding-code-examples"></a>Voorbeelden van de wachtrij uitvoer binding-code

In dit voorbeeld C#-code wordt een enkel uitvoer wachtrij voor elk bericht invoer wachtrij.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

In dit voorbeeld C#-code meerdere berichten schrijft met behulp van `ICollector<T>` (gebruiken `IAsyncCollector<T>` in een functie asynchrone):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Azure blob-trigger van opslag

#### <a name="functionjson-for-storage-blob-trigger"></a>Function.JSON voor opslag blob trigger

Het bestand *function.json* Hiermee geeft u de volgende eigenschappen.

- `name`: De naam van de variabele gebruikt in de functiecode voor het blob. 
- `path`: Een pad dat Hiermee geeft u de container om te controleren, en eventueel een blob naampatroon.
- `connection`: De naam van de instelling voor een app waarin een verbindingsreeks opslag. Als u laat `connection` leeg en de inwerkingtreding werkt met de verbindingsreeks van de standaard-opslag voor de functie-app, dat is opgegeven door de instelling van de app AzureWebJobsStorage.
- `type`: Moet zijn ingesteld op *blobTrigger*.
- `direction`: Moet worden ingesteld *in*.

Voorbeeld *function.json* voor een opslagruimte blob trigger dat controleert of BLOB's die zijn toegevoegd aan de container voorbeelden-workitems:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>BLOB trigger ondersteunde typen

De label kunt op elk van de volgende typen in het knooppunt of C#-functies worden gedeserialiseerd:

* Object (van JSON)
* Tekenreeks

In C#-functies kunt u ook gebonden aan een van de volgende typen:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Andere typen gedeserialiseerd door [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) 

#### <a name="blob-trigger-c-code-example"></a>BLOB trigger C#-codevoorbeeld

In dit voorbeeld C#-code Logboeken de inhoud van elk blob die is toegevoegd aan de gecontroleerde container.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>BLOB trigger naam patronen

Kunt u een patroon blob naam in de `path` eigenschap. Bijvoorbeeld:

```json
"path": "input/original-{name}",
```

Dit pad wilt zoeken naar een blob met de *Oorspronkelijke Blob1.txt* in de container *invoer* en de waarde van naam de `name` variabele in de functiecode zou `Blob1`.

Een ander voorbeeld:

```json
"path": "input/{blobname}.{blobextension}",
```

Dit pad ook een blob met de *Oorspronkelijke Blob1.txt*en de waarde van naam vinden het `blobname` en `blobextension` variabelen in de functiecode zou *Origineel-Blob1* en *txt*.

U kunt de soorten BLOB's die door de functie wordt geactiveerd door op te geven van een patroon met een vaste waarde voor de bestandsextensie beperken. Als u de `path` *naar/voorbeelden / {naam} .png*, alleen *.png* BLOB's in de container *voorbeelden van* de functie wordt geactiveerd.

Als u een naampatroon opgeven voor blob namen die accolades in de naam moet, dubbelklikt u de accolades. Stel dat u wilt zoeken BLOB's in de container met *afbeeldingen* met namen als volgt:

        {20140101}-soundfile.mp3

Gebruik deze optie voor het `path` eigenschap:

        images/{{20140101}}-{name}

In het voorbeeld wordt de `name` variabele waarde wordt *soundfile.mp3*. 

#### <a name="blob-receipts"></a>BLOB bevestigingen

De functies van Azure-runtime zorgt ervoor dat geen blob trigger, functie voor de dezelfde nieuwe of bijgewerkte blob meer dan één keer wordt genoemd. Dit gebeurt door *blob bevestigingen* onderhouden om te bepalen of een versie van de opgegeven blob is verwerkt.

BLOB ontvangstbevestigingen worden opgeslagen in een container met de naam *azure-webjobs-hosts* in het opgegeven door de verbindingsreeks AzureWebJobsStorage Azure opslag-account. Een leesbevestiging blob heeft de volgende informatie:

* De functie die heet voor de blob ("*{app functienaam}*. Functies zijn opgenomen. *{functienaam}*", bijvoorbeeld:"functionsf74b96f7. Functions.CopyBlob")
* De containernaam van de
* Het type blob ("BlockBlob" of "PageBlob")
* De naam van de blob
* De ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

Als u afdwingen opnieuw verwerken van een blob wilt, kunt u de ontvangst blob voor die blob handmatig verwijderen uit de container *azure-webjobs-hosts* .

#### <a name="handling-poison-blobs"></a>Verwerking van poison BLOB 's

Wanneer een functie van de trigger blob mislukt, oproepen de SDK deze opnieuw, als het probleem wordt veroorzaakt door een tijdelijke fout. Als het probleem wordt veroorzaakt door de inhoud van de blob, wordt de functie mislukt telkens wanneer de blob wordt verwerkt. Standaard roept de SDK een functie maximaal 5 tijden voor een bepaald blob. Als het vijfde uitproberen mislukt, wordt een bericht in de SDK toegevoegd aan een wachtrij *webjobs-blobtrigger-poison*met de naam.

Het bericht wachtrij voor poison BLOB's is een JSON-object dat de volgende eigenschappen heeft:

* FunctionId (in de indeling *{app functienaam}*. Functies zijn opgenomen. *{functienaam}*)
* BlobType ("BlockBlob" of "PageBlob")
* ContainerName
* BlobName
* ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>BLOB polling voor grote containers

Als de container blob die de trigger worden gecontroleerd meer dan 10.000 BLOB's bevat, logboekbestanden de functies runtime scant wil bekijken voor nieuwe of gewijzigde BLOB's. Dit proces is niet realtime; een functie mogelijk niet worden geactiveerd tot enkele minuten of langer nadat de blob is gemaakt. Daarnaast [opslag logboeken zijn gemaakt op een "beste inspanningen"](https://msdn.microsoft.com/library/azure/hh343262.aspx) soort_jaar; Er is geen garantie dat alle gebeurtenissen worden worden opgenomen. Onder bepaalde omstandigheden de logboeken kunnen worden overgeslagen. Als u de snelheid en betrouwbaarheid beperkingen van blob triggers voor grote containers zijn niet toegestaan voor uw toepassing, wordt de aanbevolen methode is een bericht wachtrij maken wanneer u de blob maken en het gebruik van een wachtrij-trigger in plaats van een trigger blob verwerkingstijd van de blob.
 
## <a id="storageblobbindings"></a>Azure blob van opslag invoer en uitvoer bindingen

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>Function.JSON voor een blob storage of uitvoer binding

Het bestand *function.json* Hiermee geeft u de volgende eigenschappen.

- `name`: De naam van de variabele gebruikt in de functiecode voor het blob. 
- `path`: Een pad dat Hiermee geeft u de container als u wilt de blob van lezen of schrijven van de blob naar, en eventueel een blob naampatroon.
- `connection`: De naam van de instelling voor een app waarin een verbindingsreeks opslag. Als u laat `connection` leeg en de binding werkt met de verbindingsreeks van de standaard-opslag voor de functie-app, dat is opgegeven door de instelling van de app AzureWebJobsStorage.
- `type`: Moet zijn ingesteld op *blob*.
- `direction`: Ingesteld op *in* - of *Uitzoomen*. 

Voorbeeld *function.json* voor een opslag blob invoer of binding, een trigger wachtrij gebruiken om te kopiëren van een blob uitvoer:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>Invoer- en uitvoerbereik ondersteunde typen BLOB

De `blob` binding kunt serialiseren of terugconverteren van de volgende typen in Node.js of C#-functies:

* Object (`out T` in C# voor uitvoer blob: Hiermee maakt u een blob als null-object als parameterwaarde null is wanneer de functie eindigt)
* Tekenreeks (`out string` in C# voor uitvoer blob: Hiermee maakt u een blob alleen als de tekenreeksparameter niet-null wanneer de functie retourneert)

In C#-functies, kunt u ook binding maken met de volgende typen:

* `TextReader`(alleen invoer)
* `TextWriter`(alleen uitvoer)
* `Stream`
* `CloudBlobStream`(alleen uitvoer)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>BLOB uitvoer C#-codevoorbeeld

In dit voorbeeld C#-code wordt een blob waarvan de naam wordt ontvangen in een bericht wachtrij gekopieerd.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Azure opslag tabellen invoer en uitvoer bindingen

#### <a name="functionjson-for-storage-tables"></a>Function.JSON voor opslag-tabellen

De *function.json* Hiermee geeft u de volgende eigenschappen.

- `name`: De naam van de variabele gebruikt in de functiecode voor de tabelbinding. 
- `tableName`: De naam van de tabel.
- `partitionKey`en `rowKey` : een eenheid in een functie C# of knooppunt lezen of schrijven van één enkele entiteit in een functie knooppunt samen worden gebruikt.
- `take`: Het maximum aantal rijen te lezen voor tabel invoer in een functie knooppunt.
- `filter`: Filterexpressie OData voor tabel invoer in een functie knooppunt.
- `connection`: De naam van de instelling voor een app waarin een verbindingsreeks opslag. Als u laat `connection` leeg en de binding werkt met de verbindingsreeks van de standaard-opslag voor de functie-app, dat is opgegeven door de instelling van de app AzureWebJobsStorage.
- `type`: Moet zijn ingesteld op *tabel*.
- `direction`: Ingesteld op *in* - of *Uitzoomen*. 

Het volgende voorbeeld *function.json* gebruikt een trigger wachtrij om te lezen van een rij één tabel. De JSON biedt een sleutelwaarde vastgelegde partition en geeft aan dat de toets rij afkomstig van het bericht wachtrij is.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Opslag tabellen invoer en uitvoer ondersteunde typen

De `table` binding kunt serialiseren of terugconverteren van objecten in Node.js of C#-functies. De objecten heeft eigenschappen RowKey en PartitionKey. 

In C#-functies, kunt u ook binding maken met de volgende typen:

* `T`waar `T` implementeert`ITableEntity`
* `IQueryable<T>`(alleen invoer)
* `ICollector<T>`(alleen uitvoer)
* `IAsyncCollector<T>`(alleen uitvoer)

#### <a name="storage-tables-binding-scenarios"></a>Opslag tabellen binding scenario 's

De tabelbinding ondersteunt de volgende scenario's:

* Lees een enkele rij in een functie C# of knooppunt.

    Stel `partitionKey` en `rowKey`. De `filter` en `take` eigenschappen niet worden gebruikt in dit scenario.

* Lees meerdere rijen in een C#-functie.

    De functies runtime biedt een `IQueryable<T>` object is gebonden aan de tabel. Type `T` moet zijn afgeleid van `TableEntity` of implementeert `ITableEntity`. De `partitionKey`, `rowKey`, `filter`, en `take` eigenschappen niet worden gebruikt in dit scenario; u kunt de `IQueryable` -object om alle filters die is vereist. 

* Meerdere rijen in een functie knooppunt lezen.

    Stel de `filter` en `take` eigenschappen. Geen `partitionKey` of `rowKey`.

* Schrijf een of meer rijen in een C#-functie.

    De functies runtime biedt een `ICollector<T>` of `IAsyncCollector<T>` die is gebonden aan de tabel waar `T` Hiermee geeft u het schema van de entiteiten die u wilt toevoegen. Meestal Typ `T` is afgeleid van `TableEntity` of implementeert `ITableEntity`, maar deze niet hoeft te worden. De `partitionKey`, `rowKey`, `filter`, en `take` eigenschappen niet worden gebruikt in dit scenario.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Opslag tabellen voorbeeld: een entiteit één tabel in C# of knooppunt lezen

Het volgende C#-codevoorbeeld werkt met het voorgaande *function.json* -bestand dat eerder aan het lezen van een entiteit één tabel. Het bericht wachtrij heeft de waarde van de rij en de tabel entity in een type die is gedefinieerd in het bestand *run.csx* wordt gelezen. Het type bevat `PartitionKey` en `RowKey` eigenschappen en wordt niet afgeleid van `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

Het volgende F # voorbeeld werkt ook met de voorgaande *function.json* -bestand te lezen van een entiteit één tabel.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

Het volgende voorbeeld knooppunt werkt ook met de voorgaande *function.json* -bestand te lezen van een entiteit één tabel.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Voorbeeld van opslag-tabellen: meerdere tabel entiteiten in C lezen# 

De volgende *function.json* en C#-code voorbeeld leest entiteiten partition sleutels die is opgegeven in het bericht wachtrij.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

De C#-code voegt u een verwijzing naar de SDK van Azure opslag, zodat het entiteitstype kan worden afgeleid `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Voorbeeld van opslag-tabellen: tabel entiteiten maken in C# 

Het volgende *function.json* en *run.csx* -voorbeeld ziet hoe u schrijft tabel entiteiten in C#.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Voorbeeld van opslag-tabellen: tabel entiteiten in F maken#

Het volgende *function.json* en *run.fsx* -voorbeeld ziet hoe u schrijft tabel entiteiten in F #.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Opslag tabellen voorbeeld: een Tabelentiteit in knooppunt maken

In het volgende *function.json* en *run.csx* voorbeeld ziet u hoe u een Tabelentiteit schrijft in knooppunt.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
