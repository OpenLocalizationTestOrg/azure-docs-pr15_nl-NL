##<a name="create-client"></a>Een clientverbinding maken

Een clientverbinding maakt met het maken van een `WindowsAzure.MobileServiceClient` object.  Vervang `appUrl` door de URL naar uw mobiele App.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>Werken met tabellen

Met access of update gegevens, een verwijzing naar de backend-tabel maken. Vervang `tableName` met de naam van de tabel

```
var table = client.getTable(tableName);
```

Nadat u een tabelverwijzing hebt, kunt u werken met uw tabel verdere:

* [Een tabel](#querying)
  * [Filteren van gegevens](#table-filter)
  * [Paginering van gegevens](#table-paging)
  * [Gegevens sorteren](#sorting-data)
* [Gegevens invoegen](#inserting)
* [Wijzigen van gegevens](#modifying)
* [U gegevens verwijdert](#deleting)

###<a name="querying"></a>Hoe u: een tabelverwijzing Query

Nadat u een tabelverwijzing hebt, kunt u deze gegevens op de server worden opgevraagd.  Query's worden gemaakt in een taal "LINQ-achtige".
Om terug te keren zorgen dat alle gegevens uit de tabel, gebruikt u het volgende:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

De functie succes wordt aangeroepen met de resultaten.   Gebruik geen `for (var i in results)` in al dan niet geslaagde werkt zoals die wordt herhaald over informatie die is opgenomen in de resultaten wanneer andere queryfuncties (zoals `.includeTotalCount()`) worden gebruikt.

Raadpleeg voor meer informatie over de syntaxis van de Query, de [Query objectdocumentatie].

####<a name="table-filter"></a>Filteren van gegevens op de server

U kunt een `where` component in de tabelverwijzing:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

U kunt ook een functie die het object worden gefilterd.  In dit geval de `this` variabele is toegewezen aan de huidige object wordt gefilterd.  Hieronder ziet u functioneel gelijk aan het voorgaande voorbeeld.

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>Paginering van gegevens

Gebruik de methoden take() en skip().  Bijvoorbeeld als u wilt splitsen in de tabel in records 100-rij:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

De `.includeTotalCount()` methode wordt gebruikt om een veld totalCount toevoegen aan het object resultaten.  Het veld totalCount wordt gevuld met het totale aantal records die wordt geretourneerd als geen paginering wordt gebruikt.

U kunt de variabele pagina's en sommige UI-knoppen bieden van een paginalijst. Gebruik loadPage() om het laden van de nieuwe records voor elke pagina.  U moet enkele sorteren van caching om snelheid toegang tot de records die al zijn geladen implementeren.


####<a name="sorting-data"></a>Hoe u: keren gegevens die zijn gesorteerd

Gebruik de query .orderBy() of .orderByDescending() methoden:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Voor meer informatie over het Query-object, raadpleegt u de [Query objectdocumentatie].

###<a name="inserting"></a>Hoe u: gegevens invoegen

Een JavaScript-object maken met de juiste datum en table.insert() asynchroon bellen:

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Op succesvolle invoeging het ingevoegde item geretourneerd met de extra velden die vereist voor synchronisatie bewerkingen zijn.  Met deze informatie voor latere updates, moet u uw eigen cache bijwerken.

Houd er rekening mee dat de Azure Mobile-Apps Node.js Server SDK dynamische schema om software te ontwikkelen ondersteunt.
Voor dynamische schema, wordt het schema van de tabel in de browser, zodat u kunt kolommen toevoegen aan de tabel door ze op te geven in een invoegen of bijwerken betrekking heeft bijgewerkt.  Het is raadzaam dat u dynamische schema uitschakelen voordat u verdergaat uw toepassing op de productie.

###<a name="modifying"></a>Hoe u: gegevens wijzigen

Vergelijkbaar met de methode .insert(), u moet een Update-object maken en vervolgens bellen .update().  Het update-object moet bevatten de ID van de record moeten worden bijgewerkt: Hiermee worden opgehaald tijdens het lezen van de record of bij het aanroepen van .insert().

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>Hoe u: gegevens verwijderen

Bel de methode .del() als een record wilt verwijderen.  De ID doorgeeft in een objectverwijzing:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
