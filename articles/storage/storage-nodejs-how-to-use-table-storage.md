<properties
    pageTitle="Het gebruik van Azure-tabelopslag van Node.js | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud met Azure-tabelopslag, een gegevensopslag NoSQL."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Het gebruik van Azure-tabelopslag van Node.js

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Dit onderwerp wordt uitgelegd hoe u veelvoorkomende scenario's met de tabel Azure-service in een toepassing voor Node.js uitvoert.

In de codevoorbeelden in dit onderwerp wordt ervan uitgegaan dat u al een Node.js-toepassing. Zie een van de volgende onderwerpen voor informatie over het maken van een toepassing voor de Node.js in Azure wordt aangegeven:

- [Een Node.js web-app maakt in Azure App-Service](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Bouw en implementeer een Node.js web-app in Azure WebMatrix gebruiken](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Bouwen en implementeren van een toepassing Node.js naar een Cloudservice van Azure](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (via Windows PowerShell)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Uw toepassing toegang hebben tot Azure-opslag configureren

Als u wilt gebruiken Azure opslag, moet u de SDK van Azure opslag voor Node.js, waaronder een reeks gemak bibliotheken die met de opslagruimte REST-services communiceren.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Knooppunt pakket Manager (NPM) gebruiken voor het installeren van het pakket

1.  Gebruik een opdrachtregel-interface zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix) en navigeer naar de map waar u uw toepassing hebt gemaakt.

2.  Typ **npm Installeer azure-opslag** in het opdrachtvenster. Uitvoer van de opdracht is vergelijkbaar met het volgende voorbeeld.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  U kunt handmatig uitvoeren met de opdracht **ls** om te controleren of die een **knooppunt\_modules** map is gemaakt. In deze map vindt u het pakket **azure-opslag** , waarin de bibliotheken die u nodig hebt voor toegang tot opslag.

### <a name="import-the-package"></a>Het pakket importeren

Voeg de volgende code naar het begin van het bestand **server.js** in uw toepassing:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Een Azure Storage-verbinding instellen

De azure module leest de omgevingsvariabelen AZURE\_opslag\_ACCOUNT en AZURE\_opslag\_ACCESS\_toets of AZURE\_opslag\_verbinding\_tekenreeks voor informatie die nodig is om te koppelen aan een account Azure opslag. Als deze omgevingsvariabelen niet zijn ingesteld, moet u de accountgegevens bij het aanroepen van **TableService**is ingesteld.

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in de [Portal van Azure](https://portal.azure.com) voor een Website Azure [Node.js WebApp met de Service van de tabel Azure].

## <a name="create-a-table"></a>Een tabel maken

De volgende code wordt gemaakt van een object **TableService** en wordt deze gebruikt om een nieuwe tabel te maken. De volgende Klik boven aan **server.js**toevoegen.

    var tableSvc = azure.createTableService();

De oproep door naar **createTableIfNotExists** maakt een nieuwe tabel met de opgegeven naam als deze nog niet bestaat. Het volgende voorbeeld wordt een nieuwe tabel MijnTabel '' als deze nog niet bestaat.

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

De `result.created` worden `true` als een nieuwe tabel is gemaakt, en `false` als de tabel al bestaat. De `response` bevat informatie over het verzoek.

### <a name="filters"></a>Filters

Optionele filterbewerkingen kunnen worden toegepast op bewerkingen die zijn uitgevoerd met **TableService**. Bewerkingen filteren kunt opnemen logboekregistratie, automatisch opnieuw proberen, enzovoort. Filters zijn objecten die implementeren van een methode bij de handtekening:

    function handle (requestOptions, next)

Na de voorverwerking op de opties van de aanvraag, de methode moet bellen 'volgende', doorgeven van een terugbellen met de volgende handtekening:

    function (returnObject, finalCallback, next)

In dit terugbellen en na het verwerken van de returnObject (de reactie van de aanvraag op de server), moet de terugbellen roepen volgende indien aanwezig als u wilt doorgaan met het verwerken van andere filters of roepen finalCallback gewoon anders als u wilt de service-aanroep beëindigen.

Twee filters die opnieuw logica implementeren zijn opgenomen in de SDK Azure voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende maakt een **TableService** -object dat de **ExponentialRetryPolicyFilter**gebruikt:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u wilt toevoegen een entiteit, moet u eerst een object dat de entiteitseigenschappen van uw definieert maken. Alle entiteiten moeten bevatten een **PartitionKey** en **RowKey**, die unieke id's voor de entiteit.

* **PartitionKey** - bepaalt de partition die de entiteit worden opgeslagen in

* **RowKey** - id heeft een unieke van de entiteit binnen de partition

Zowel **PartitionKey** en **RowKey** moeten tekenreekswaarden. Zie [informatie over het gegevensmodel van de tabel-Service](http://msdn.microsoft.com/library/azure/dd179338.aspx)voor meer informatie.

Hier volgt een voorbeeld van een entiteit definiëren. Houd er rekening mee dat **voorbeeld** is gedefinieerd als een soort **Edm.DateTime**. Opgeven van het type is optioneel en typen wordt afgeleid als niet is opgegeven.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] Er is ook een veld **tijdstempel** voor elke record, die zijn ingesteld door Azure wanneer een entiteit wordt ingevoegd of bijgewerkt.

U kunt ook de **entityGenerator** entiteiten maken. Het volgende voorbeeld wordt dezelfde taak entiteit met de **entityGenerator**.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Als u wilt een entiteit aan uw tabel toevoegt, geeft u het entiteitsobject aan de methode **insertEntity** .

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Als de bewerking geslaagd is, `result` bevat de [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) van de ingevoegde record en `response` bevat informatie over de bewerking.

Voorbeeld antwoord:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] Standaard **insertEntity** resulteert niet in de ingevoegde entity als onderdeel van de `response` informatie. Als u van plan bent om het uitvoeren van andere bewerkingen op deze entiteit of de cache van de informatie wilt, kan het handig zijn om deze geretourneerd als onderdeel van de `result`. U kunt dit doordat **echoContent** als volgt doen:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Een entiteit bijwerken

Er zijn meerdere methoden beschikbaar voor het bijwerken van een bestaande entiteit:

* **replaceEntity** - een bestaande entiteit bijgewerkt door deze te vervangen

* **mergeEntity** - een bestaande entiteit bijgewerkt met het samenvoegen van nieuwe eigenschapswaarden in de bestaande entiteit

* een bestaande entiteit **insertOrReplaceEntity** - bijgewerkt door deze te vervangen. Als er geen entiteit bestaat, wordt een nieuwe record ingevoegd

* een bestaande entiteit **insertOrMergeEntity** - bijgewerkt door het samenvoegen van nieuwe eigenschapswaarden in de bestaande. Als er geen entiteit bestaat, wordt een nieuwe record ingevoegd

Het volgende voorbeeld wordt een entiteit met **replaceEntity**bijwerken:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] De standaardinstelling wordt voor het bijwerken van een entiteit niet gecontroleerd om te zien of de gegevens worden bijgewerkt eerder is gewijzigd door een ander proces. Gelijktijdige updates ondersteunen:
>
> 1. Krijgen de ETag van het object is bijgewerkt. Hiermee wordt geretourneerd als onderdeel van de `response` gedurende een entiteit betrekking betrekking heeft hebben en kunnen worden opgehaald met `response['.metadata'].etag`.
>
> 2. Wanneer er een updatebewerking op een entiteit, moet u de gegevens van de ETag eerder opgehaald naar de nieuwe entiteit toevoegen. Bijvoorbeeld:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. De updatebewerking uitvoeren. Als de entiteit is gewijzigd sinds u hebt opgehaald ETag-waarde, zoals een ander exemplaar van uw toepassing, een `error` worden geretourneerd met de mededeling dat de update-voorwaarde dat is opgegeven in de aanvraag niet is voldaan.

Met **replaceEntity** en **mergeEntity**, als de entiteit die wordt bijgewerkt, niet bestaat, klikt u vervolgens de updatebewerking, mislukt. Daarom als u wilt opslaan van een entiteit ongeacht of deze al bestaat, **insertOrReplaceEntity** of **insertOrMergeEntity**gebruiken.

De `result` voor geslaagd update bewerkingen de **Etag** van de bijgewerkte entiteit moeten bevatten.

## <a name="work-with-groups-of-entities"></a>Werken met groepen van entiteiten

Soms is het handig om in te dienen meerdere bewerkingen samen in een batch om ervoor te zorgen atomaire verwerkt door de server. U kunt doen die, de klas **TableBatch** u een batch wilt maken, en klikt u vervolgens de methode **executeBatch** van **TableService** de batch bewerkingen uit te voeren.

 Het volgende voorbeeld ziet u twee entiteiten in een batch verzendt:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

Voor succesvolle batchbewerkingen, `result` bevat informatie voor elke bewerking in de batch.

### <a name="work-with-batched-operations"></a>Werken met gegroepeerde bewerkingen

Bewerkingen die zijn toegevoegd aan een batch kunnen worden gecontroleerd door de weergave de `operations` eigenschap. U kunt ook de volgende methoden gebruiken om te werken met bewerkingen:

* alle bewerkingen uit een batch opheffen **wissen** -

* **getOperations** - formule een bewerking worden opgehaald uit de batch

* **hasOperations** - geeft als resultaat waar als de batch bevat bewerkingen

* **removeOperations** - Hiermee verwijdert u een bewerking

* **grootte** - geeft als resultaat het aantal bewerkingen in de batch

## <a name="retrieve-an-entity-by-key"></a>Een entiteit door sleutel ophalen

Gebruik de methode **retrieveEntity** om een specifieke entiteit op basis van de **PartitionKey** en **RowKey**te retourneren.

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Wanneer deze bewerking voltooid is, `result` bevat de entiteit.

## <a name="query-a-set-of-entities"></a>Een reeks entiteiten query

Om een tabel een query uitvoert, gebruikt u het object **TableQuery** maken van een query-expressie met de volgende componenten:

* **Selecteer** - de velden die moeten worden geretourneerd door de query

* **waar** - waar u de component

    * **en** - een `and` where-voorwaarde

    * **of** - een `or` where-voorwaarde

* **top** - het aantal items kunt ophalen


Het volgende voorbeeld wordt een query die de bovenste vijf items met een PartitionKey van 'hometasks'.

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

Aangezien **Selecteer** niet wordt gebruikt, worden alle velden worden geretourneerd. Als u wilt uitvoeren in de query ten opzichte van een tabel, gebruikt u **queryEntities**. Het volgende voorbeeld wordt deze query om terug te keren entiteiten from 'MijnTabel'.

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

Als dit lukt, `result.entries` bevat een matrix van entiteiten die overeenkomen met de query. Als de query is niet mogelijk om terug te keren alle entiteiten `result.continuationToken` niet -*null* en kunnen worden gebruikt als de derde parameter van **queryEntities** om meer resultaten te halen. Voor de eerste query, gebruikt u *null* voor de derde parameter.

### <a name="query-a-subset-of-entity-properties"></a>Query een subset van entiteitseigenschappen

Een query toevoegen aan een tabel kunt u slechts een paar velden ophalen uit een entiteit.
Dit bandbreedte voorkomt en prestaties van query's, met name voor grote entiteiten kunt verbeteren. Gebruik de component **selecteren** en geven van de namen van de velden moeten worden geretourneerd. De volgende query resulteert bijvoorbeeld alleen de velden **Beschrijving** en **voorbeeld** .

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Een entiteit verwijderen

U kunt een entiteit met de toetsen partition en rij verwijderen. In dit voorbeeld bevat het object **task1** **RowKey** en **PartitionKey** waarden van de entiteit zijn verwijderd. Klik op het object wordt doorgegeven aan de methode **deleteEntity** .

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Kunt u overwegen ETags bij het verwijderen van items, om ervoor te zorgen dat het item niet is gewijzigd door een ander proces. Zie [een entiteit bijwerken](#update-an-entity) voor informatie over het gebruik van ETags.

## <a name="delete-a-table"></a>Een tabel verwijderen

Een tabel verwijdert met de volgende code uit een opslag-account.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Als u niet zeker of de tabel bestaat weet, gebruikt u **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Gebruik voortgezet tokens

Wanneer u de tabellen voor grote hoeveelheden resultaten zoeken wilt, zoekt u voortgezet tokens. Er zijn mogelijk grote hoeveelheden gegevens beschikbaar zijn voor uw query die u niet weet mogelijk als u niet maken kan herkennen wanneer een token voortgezet aanwezig is.

De resultaten object geretourneerd tijdens het uitvoeren van query's entiteiten sets een `continuationToken` eigenschap wanneer deze een token aanwezig is. Vervolgens kunt u dit bij het uitvoeren van een query om verder te Houd de cursor boven de entiteiten partition en de tabel.

Wanneer de query wordt uitgevoerd, kan een parameter continuationToken tussen het exemplaar van de query-object en de functie terugbellen worden verstrekt:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Als u de `continuationToken` object, zult u eigenschappen, zoals `nextPartitionKey`, `nextRowKey` en `targetLocation`, die kan worden gebruikt om te doorlopen van de resultaten te bekijken.

Er is ook een steekproef voortgezet binnen de cessies‑retrocessies Azure opslag Node.js op GitHub. Zoekt u `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Werken met gedeelde toegang handtekeningen

Gedeelde toegang handtekeningen (SA's) zijn een veilige manier voor gedetailleerde toegang tot tabellen zonder dat uw opslagaccountnaam of toetsen. SA's worden gebruikt voor beperkte toegang tot uw gegevens, zoals een mobiele app records wilt zoeken toestaan.

Een vertrouwde toepassing, bijvoorbeeld een cloudservice genereert een SA's met de **generateSharedAccessSignature** van de **TableService**en wordt dit aangeboden aan een niet-vertrouwde of gedeeltelijk vertrouwde-toepassing zoals een mobiele app. De SA's wordt gegenereerd op basis van een beleid, waarin de begin- en einddatums waarin de SA's geldig is, alsmede het toegangsniveau verleend degene die SA's.

Het volgende voorbeeld wordt een nieuwe gedeelde-beleid, zodat de gebruiker SA's query ('r') in de tabel gegenereerd en verloopt 100 minuten na het tijdstip waarop dat deze is gemaakt.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Houd er rekening mee dat de host moet worden geïnformeerd ook als dit nodig is als degene die SA's probeert te krijgen van de tabel.

De clienttoepassing vervolgens wordt de SA's met **TableServiceWithSAS** gebruikt om te worden bewerkingen uitgevoerd op de tabel. Het volgende voorbeeld is verbonden met de tabel en voert een query.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

Aangezien de SA's is gegenereerd met alleen query toegang als een poging zijn aangebracht in invoegen, bijwerken of verwijderen van entiteiten, wordt een fout geretourneerd.

### <a name="access-control-lists"></a>Toegangscontrolelijsten

U kunt ook een Access (Toegangsbeheerlijst) gebruiken voor het instellen van het access-beleid voor een SA's. Dit is handig als u toestaan meerdere klanten toegang tot de tabel wilt, maar bieden verschillende clienttoegangsbeleid voor elke klant.

Een ACL wordt geïmplementeerd met van een matrix van clienttoegangsbeleid, met een ID die is gekoppeld aan elke beleid. Het volgende voorbeeld worden twee beleidsregels, één voor 'gebruiker1' en één voor 'gebruiker2' gedefinieerd:

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Het volgende voorbeeld wordt de huidige ACL voor de tabel **hometasks** en telt vervolgens het nieuwe beleid **setTableAcl**gebruiken. Deze methode kunt:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Wanneer de ACL is ingesteld, kunt u vervolgens een SA's op basis van de ID voor een beleid maken. Het volgende voorbeeld wordt een nieuwe SA's voor 'gebruiker2':

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie.

-   [Teamblog van azure opslag][].
-   De bibliotheek van de [Azure opslag SDK voor knooppunt][] op GitHub.
-   [Node.js Developer Center](/develop/nodejs/)

  [Azure opslag SDK voor knooppunt]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Azure opslag teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js WebApp met de Service van de tabel Azure]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md
