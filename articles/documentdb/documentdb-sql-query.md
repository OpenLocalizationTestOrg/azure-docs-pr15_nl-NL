<properties 
    pageTitle="SQL-syntaxis en het SQL-query voor DocumentDB | Microsoft Azure" 
    description="Lees meer over SQL-syntaxis, database concepten en SQL-query's voor DocumentDB, een NoSQL-database. SQL kunnen gebruikt als een querytaal JSON in DocumentDB." 
    keywords="SQL-syntaxis, sql-query, sql-query's, json querytaal, database concepten en sql-query's, statistische functies"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>SQL-query en SQL-syntaxis in DocumentDB
Microsoft Azure DocumentDB documenten opvragen SQL (Structured Query Language) als een querytaal JSON gebruiken. DocumentDB is gekomen door echt schema-vrij te geven. Op grond van het gebied van het gegevensmodel JSON rechtstreeks vanuit de database-engine biedt deze automatische indexeren van JSON documenten zonder expliciete schema of secundaire indexen worden gemaakt. 

Tijdens het ontwerpen van de querytaal voor DocumentDB hebben we twee doelstellingen in gedachten:

-   In plaats van een nieuwe JSON-querytaal vruchtbare, willen we ondersteuning voor SQL. SQL is een van de meest vertrouwde en populaire querytalen. DocumentDB SQL biedt een formele programming model voor uitgebreide query's via JSON-documenten.
-   Als een JSON document database kan worden uitgevoerd JavaScript rechtstreeks in de database engine, die we wil JavaScript van programmeren model gebruikt als basis voor onze querytaal. De SQL DocumentDB zich bevindt in de JavaScript typesysteem, evaluatie van de expressie en functie aanroep. Deze inschakelen biedt een natuurlijke programming model voor relationele prognoses, hiërarchische navigatie JSON-documenten, self joins, ruimte query's en-aanroep van de gebruiker gedefinieerde functies (UDF) geschreven volledig in JavaScript, onder andere functies. 

We denken dat deze mogelijkheden om te verkleinen van de wrijving tussen de toepassing en de database en essentieel voor ontwikkelaars productiviteit zijn.

Het is raadzaam om aan de slag door te bekijken van de volgende video, Aravind Ramachandran ziet u waar de DocumentDB opvragen mogelijkheden, en via onze [Query Speelplaats](http://www.documentdb.com/sql/demo), waar u kunt uitproberen DocumentDB en SQL-query's voor onze gegevensset uitgevoerd.

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

Vervolgens terug naar dit artikel, waarbij we beginnen met een SQL-query zelfstudie dat u tijdens enkele eenvoudige JSON-documenten en SQL-opdrachten begeleidt.

## <a name="getting-started-with-sql-commands-in-documentdb"></a>Aan de slag met SQL-opdrachten in DocumentDB
Als u wilt zien DocumentDB SQL in de praktijk, laten we beginnen met een paar eenvoudige JSON-documenten en enkele eenvoudige query's op deze doorloop. Houd rekening met deze twee JSON-documenten over twee gezinnen. Houd er rekening mee dat met DocumentDB, we niet hoeft te maken van een schema's maken of secundaire indexen expliciet. Gewoon moeten we de JSON-documenten aan een verzameling DocumentDB invoegen en vervolgens query. Hier zijn er een eenvoudige JSON adres-en registratiegegevens voor het gezin Andersen, bovenliggende items, kinderen (en hun huisdieren), een document. Het document bevat tekenreeksen, getallen, Booleaanse waarden, matrices en geneste eigenschappen. 

**Document**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


Hier ziet u een tweede document met één kleine-verschil – `givenName` en `familyName` worden gebruikt in plaats van `firstName` en `lastName`.

**Document**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



Nu een paar query's op deze gegevens om een idee van de belangrijkste aspecten van DocumentDB SQL te laten we eens proberen. Bijvoorbeeld de volgende query resulteert in de documenten waarvan het veld id overeenkomt met `AndersenFamily`. Gezien het feit is een `SELECT *`, de uitvoer van de query is het volledige JSON-document:

**Query**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Nu kunt u het hoofdlettergebruik waar moeten we de JSON-uitvoer in een andere shape opmaken. Deze query projecteert een nieuw JSON-object met twee geselecteerde velden, naam en plaats, wanneer het adres van de stad dezelfde naam als de staat heeft. In dit geval "Za, za" komt overeen met.

**Query**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Resultaten**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


De volgende query resulteert in de opgegeven namen van kinderen in het gezin waarvan de id overeenkomt met `WakefieldFamily` gesorteerd op de plaats van wonen.

**Query**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Resultaten**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Willen we de aandacht vestigen op een paar wijze aspecten van de querytaal DocumentDB tot en met de voorbeelden die we tot nu toe hebt gezien:  
 
-   Aangezien DocumentDB SQL op JSON waarden werkt, behandelt structuur vorm entiteiten in plaats van rijen en kolommen. De taal kunt u verwijzen naar knooppunten van de structuur op een willekeurige diepte, zoals daarom `Node1.Node2.Node3…..Nodem`, vergelijkbaar met relationele SQL verwijzen naar de verwijzing twee deel uit van `<table>.<column>`.   
-   De structured query language werkt met schema zonder gegevens. Daarom moet het typesysteem dynamisch worden verbonden. Dezelfde expressie kan verschillende typen op verschillende documenten rendement. Het resultaat van een query is een geldige JSON-waarde, maar het is niet gegarandeerd van een vaste schema.  
-   DocumentDB ondersteunt alleen strikte JSON-documenten. Dit betekent dat het typesysteem en expressies zijn beperkt tot alleen afhandelen JSON typen. Raadpleeg de [specificatie van JSON](http://www.json.org/) voor meer informatie.  
-   Een verzameling DocumentDB is een container schema-vrije van JSON-documenten. De relaties in gegevensentiteiten binnen en tussen documenten in een verzameling worden impliciet vastgelegd door opname en niet door de primaire sleutel en refererende sleutel relaties. Dit is een belangrijk aspect zegt wijzen op gezien het binnen-document joins besproken verderop in dit artikel.

## <a name="documentdb-indexing"></a>DocumentDB indexeren

Voordat we naar de DocumentDB SQL-syntaxis sturen, is het meer zegt dan het ontwerp van de indexing in DocumentDB verkennen. 

Het doel van de database indexen is moet fungeren van query's in hun verschillende formulieren en shapes met minimale resourceverbruik (zoals CPU en invoer/uitvoer) terwijl goede doorvoer en lage latentie. Vaak vereist de keuze van de juiste index voor query's in een database veel plannen en experimenten. Deze methode vormt een uitdaging voor schema-minder databases waar de gegevens niet voldoet aan een strikte schema en snel ontwikkeld. 

Daarom als we de DocumentDB indexeren subsysteem ontworpen, instellen we de doelen van het volgende:

-   Indexeren van documenten zonder schema: het indexeren subsysteem vereist een schema-informatie of veronderstellingen over schema van de documenten. 

-   Ondersteuning voor efficiënte, uitgebreide hiërarchische en relationele query's: de index ondersteunt de querytaal DocumentDB efficiënt, inclusief ondersteuning voor hiërarchische en relationele prognoses.

-   Ondersteuning voor consistente query's in face of een continue hoeveelheid schrijven: voor hoge schrijven doorvoer werkbelastingen met consistente query's, de index stapsgewijs, efficiënt en online vlak van een continue hoeveelheid schrijft wordt bijgewerkt. De indexupdate consistente is essentieel voor dienen de query's op het niveau van de consistentie waarin de gebruiker de documentservice geconfigureerd.

-   Ondersteuning voor meerdere pachtadres: gegeven het model op basis van het reserveren voor de resource beheermodel over tenants, index-updates worden uitgevoerd binnen het budget van systeembronnen (CPU, geheugen en invoer/uitvoer-bewerkingen per seconde) per replica toegewezen. 

-   Opslag efficiency: voor kosteneffectiviteit, de realiseren op schijf opslag van de index is gebonden en overzichtelijk. Dit is essentieel, omdat DocumentDB kan de ontwikkelaar maken op basis van kosten compromissen nodig zijn tussen index belasting ten opzichte van de prestaties van de query's.  

Raadpleeg de [DocumentDB voorbeelden](https://github.com/Azure/azure-documentdb-net) op MSDN voor voorbeelden waarin wordt getoond hoe de indexing beleid voor een siteverzameling configureren. We gaan nu op de details van de DocumentDB SQL-syntaxis.


## <a name="basics-of-a-documentdb-sql-query"></a>Basisbeginselen van een DocumentDB SQL-query
Elke query bestaat uit een SELECT-component en een optionele FROM en WHERE-componenten per ANSI SQL-standaarden. Voor elke query, de bron in de FROM-component meestal opgesomd. Het filter in de WHERE-component wordt toegepast op de bron om op te halen, een subset van JSON-documenten. Ten slotte wordt de SELECT-component gebruikt voor het project de gevraagde JSON-waarden in de lijst selecteren.
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>FROM-component
De `FROM <from_specification>` -component is optioneel, tenzij de bron is gefilterd of verderop in de query geschatte. Het doel van deze component is om op te geven van de gegevensbron waarop u de query hanteert. Meestal de hele verzameling de bron is, maar een een subset van de siteverzameling in plaats daarvan kunt opgeven. 

Een query zoals `SELECT * FROM Families` wordt aangeduid dat de hele gezinnen verzameling de bron waarover u opsommen. Een speciaal HOOFDSITE-id kan worden gebruikt om aan te geven van de siteverzameling in plaats van de naam van de siteverzameling. De volgende lijst bevat de regels die worden afgedwongen per query:

- De verzameling, zoals alias, kan zijn `SELECT f.id FROM Families AS f` of gewoon `SELECT f.id FROM Families f`. Hier `f` is het equivalent van `Families`. `AS`is een optioneel trefwoord tot de alias van de identificatie.

-   Opmerking dat eenmaal alias, de oorspronkelijke bron kan niet worden gekoppeld. Bijvoorbeeld `SELECT Families.id FROM Families f` syntaxis is ongeldig sinds de id "Gezinnen" kan niet meer worden omgezet.

-   Alle eigenschappen die moeten worden verwezen moeten volledig gekwalificeerde. Geen van de toepassing van strikte schema, wordt dit om te voorkomen dat een niet-eenduidige bindingen afgedwongen. Daarom `SELECT id FROM Families f` syntaxis is ongeldig sinds de eigenschap `id` niet is gekoppeld.
    
### <a name="sub-documents"></a>Submenu documenten
De bron kan ook worden verminderd tot een subset. Bijvoorbeeld naar opsommen alleen een onderliggende boomstructuur in elk document, kan de onderliggend hoofdsite dan de bron, zoals wordt weergegeven in het volgende voorbeeld.

**Query**

    SELECT * 
    FROM Families.children

**Resultaten**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Terwijl het bovenstaande voorbeeld wordt een matrix als de bron gebruikt, kan een object ook worden gebruikt als bron, dat wil zeggen wat wordt weergegeven in het volgende voorbeeld. Elke geldige JSON waarde (niet ongedefinieerd) die kan worden gevonden in de bron wordt beschouwd voor opname in het resultaat van de query. Als geen enkele gezinnen een `address.state` waarde, ze worden uitgesloten van het queryresultaat.

**Query**

    SELECT * 
    FROM Families.address.state

**Resultaten**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>WHERE-component
De WHERE-component (**`WHERE <filter_condition>`**) is optioneel. Hiermee geeft u de voorwaarden die u dat de JSON-documenten die is verstrekt door de bron moet voldoen om te worden opgenomen als onderdeel van het resultaat. Elk document JSON moet de opgegeven voorwaarden op "true", als voor het resultaat worden beschouwd als resultaat. De WHERE-component wordt gebruikt door de laag index om te bepalen de absolute kleinste subset van de brondocumenten die onderdeel van het resultaat uitmaken. 

De volgende query aanvraagt documenten met een naameigenschap waarvan de waarde `AndersenFamily`. Andere documenten die geen een naameigenschap of waar de waarde niet overeenkomen met `AndersenFamily` wordt uitgesloten. 

**Query**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Het vorige voorbeeld blijkt een eenvoudige gelijke query. DocumentDB SQL ondersteunt ook een groot aantal scalaire expressies. De meest gebruikte zijn binair getal en unaire expressies. Eigenschap verwijzingen uit het bronobject JSON zijn ook ongeldige expressies. 

De volgende binaire operatoren worden momenteel ondersteund en kunnen worden gebruikt in query's, zoals wordt weergegeven in de volgende voorbeelden:  
<table>
<tr>
<td>Rekenkundig</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitsgewijze</td>    
<td>|, &, ^, <<, >>, >>> (shift-nul-opvulling rechts) </td>
</tr>
<tr>
<td>Logische</td>
<td>EN, OF NIET</td>
</tr>
<tr>
<td>Vergelijking</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Tekenreeks</td> 
<td>|| (tekst.samenvoegen)</td>
</tr>
</table>  

Laten we eens kijken sommige query's met binaire operatoren.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Unaire operatoren +,-, ~ worden ook ondersteund, en niet kunnen worden gebruikt in query's, zoals wordt weergegeven in het volgende voorbeeld:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Naast de binaire en unaire operatoren, zijn eigenschap-verwijzingen ook toegestaan. Bijvoorbeeld `SELECT * FROM Families f WHERE f.isRegistered` geeft als resultaat de JSON-document met de eigenschap `isRegistered` waar de waarde van de eigenschap is gelijk aan de JSON `true` waarde. Andere waarden (ONWAAR, null, ongedefinieerd, `<number>`, `<string>`, `<object>`, `<array>`, enz.) leidt naar het brondocument zijn uitgesloten van het resultaat. 

### <a name="equality-and-comparison-operators"></a>Gelijke en vergelijking operatoren
De volgende tabel ziet het resultaat van gelijke vergelijkingen in DocumentDB SQL tussen elke twee JSON-typen.
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Op</strong>
         </td>
         <td valign="top">
            <strong>Ongedefinieerd</strong>
         </td>
         <td valign="top">
            <strong>Null-waarden</strong>
         </td>
         <td valign="top">
            <strong>Booleaanse waarde</strong>
         </td>
         <td valign="top">
            <strong>Getal</strong>
         </td>
         <td valign="top">
            <strong>Tekenreeks</strong>
         </td>
         <td valign="top">
            <strong>Object</strong>
         </td>
         <td valign="top">
            <strong>Matrix</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Ongedefinieerd<strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Null-waarden<strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Booleaanse waarde<strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Getal<strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Tekenreeks<strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Object<strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Matrix<strong>
         </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
Ongedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
      </tr>
   </tbody>
</table>

Voor andere vergelijkingsoperatoren zoals >, > =,! =, < en < =, de volgende regels toepassen:   

-   Vergelijking in verschillende typen levert ongedefinieerd.
-   Vergelijking tussen twee objecten of twee matrices resultaten in ongedefinieerd.   

Als het resultaat van de scalaire expressie in het filter is niet gedefinieerd, het bijbehorende document zou niet opgenomen in het resultaat, aangezien ongedefinieerd niet logisch te met "true vergelijken".

### <a name="between-keyword"></a>TUSSEN trefwoord
U kunt ook het trefwoord BETWEEN gebruiken in query's op bereiken met waarden zoals in ANSI SQL express. TUSSEN kan worden gebruikt voor tekenreeksen of getallen.

Deze query wordt bijvoorbeeld alle family documenten waarin het eerste onderliggende grade tussen 1-5 (beide volledig is). 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

In tegenstelling tot in ANSI-SQL, kunt u ook gebruiken de component BETWEEN in de FROM-component zoals in het volgende voorbeeld.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Voor snellere momenten in de uitvoering, moet u een indexing-beleid dat een type bereik index ten opzichte van een numerieke eigenschappen/paden die worden gefilterd in de component BETWEEN gebruikt maken. 

Het belangrijkste verschil tussen het gebruik van BETWEEN in DocumentDB en ANSI SQL is dat u query's bereik ten opzichte van de eigenschappen van gemengde typen uitdrukken kunt – bijvoorbeeld "grade wellicht" een getal (5) in sommige documenten en tekenreeksen in anderen ("grade4"). In deze gevallen, wordt zoals in JavaScript, een vergelijking tussen twee verschillende typen resulteert in "ongedefinieerd" en het document overgeslagen.

### <a name="logical-and-or-and-not-operators"></a>Logische (AND, OR en niet) operatoren
Logische operators worden uitgevoerd op Booleaanse waarden. De logische waarheid tabellen voor deze operatoren worden weergegeven in de volgende tabellen.

OF-BEWERKING|Waar|ONWAAR|Ongedefinieerd
---|---|---|---
Waar|Waar|Waar|Waar
ONWAAR|Waar|ONWAAR|Ongedefinieerd
Ongedefinieerd|Waar|Ongedefinieerd|Ongedefinieerd

EN|Waar|ONWAAR|Ongedefinieerd
---|---|---|---
Waar|Waar|ONWAAR|Ongedefinieerd
ONWAAR|ONWAAR|ONWAAR|ONWAAR
Ongedefinieerd|Ongedefinieerd|ONWAAR|Ongedefinieerd

NIET|  |
---|---
Waar|ONWAAR
ONWAAR|Waar
Ongedefinieerd|Ongedefinieerd

### <a name="in-keyword"></a>TREFWOORD
Het sleutelwoord kan worden gebruikt om te controleren of een opgegeven waarde overeenkomt met een waarde in een lijst. Bijvoorbeeld retourneert deze query alle family documenten waarbij de-id "WakefieldFamily" of "AndersenFamily". 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

In dit voorbeeld retourneert alle documenten die waar is de status een van de opgegeven waarden.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternair (?) en operatoren Coalesce (?)
De operators Ternair en Coalesce kunnen voorwaardelijke expressies, vergelijkbaar met populaire programming talen zoals C# en JavaScript samen worden gebruikt. 

De operator Ternair (?) zijn heel handig wanneer het bouwen van nieuwe JSON-eigenschappen in de browser. Bijvoorbeeld: nu kunt u query's om te classificeren class niveaus in een menselijke leesbare vorm zoals beginners/Intermediate/Geavanceerd zoals hieronder wordt weergegeven.
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

U kunt ook de oproepen naar de operator like in de onderstaande query nesten.
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Als met andere query-operators, worden als de eigenschappen van het waarnaar wordt verwezen in de voorwaardelijke expressie ontbreken in een document of als de typen worden vergeleken verschillen, klikt u vervolgens die documenten uitgesloten van de queryresultaten.

De operator Coalesce (?) kan worden gebruikt om efficiënt (ook controleren op de aanwezigheid van een eigenschap maken is gedefinieerd) in een document. Dit is handig wanneer u query's uitvoeren tegen gedeeltelijk gestructureerd of gegevens van gemengde typen. Bijvoorbeeld: deze query geeft als resultaat de "Achternaam" Als deze aanwezig zijn, of de "Achternaam" Als dit niet aanwezig.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>Tussen aanhalingstekens eigenschapsaccessor
U kunt ook met de operator tussen aanhalingstekens eigenschap eigenschappen weer `[]`. Bijvoorbeeld `SELECT c.grade` en `SELECT c["grade"]` letters. Deze syntaxis is handig wanneer u een eigenschap waarmee bevat spaties en speciale tekens, of er gebeurt moet met dezelfde naam als een SQL-trefwoord of gereserveerd woord druk op ESC.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>SELECT-component
De SELECT-component (**`SELECT <select_list>`**) is verplicht en geeft aan welke waarden worden opgehaald door de query, net als op de ANSI-SQL. De subset die zijn gefilterd boven aan de brondocumenten worden doorgegeven naar de raming fase, waarbij de opgegeven JSON-waarden worden opgehaald en een nieuw JSON-object wordt opgesteld, voor elke invoer doorgegeven op deze. 

Het volgende voorbeeld ziet u een gewone selectiequery. 

**Query**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Geneste eigenschappen
In het volgende voorbeeld, zijn er twee geneste eigenschappen projecteren `f.address.state` en `f.address.city`.

**Query**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Raming ondersteunt ook JSON expressies zoals wordt weergegeven in het volgende voorbeeld.

**Query**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Bekijk de rol van `$1` hier. De `SELECT` -component moet maken van een object JSON en aangezien er geen toets wordt geleverd, gebruiken we impliciete argument variabele namen die beginnen met `$1`. Deze query wordt bijvoorbeeld twee impliciete argumentvariabelen, het label `$1` en `$2`.

**Query**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Aliasing
Nu we het bovenstaande voorbeeld expliciete aliasing waarden uitbreiden. Is het trefwoord aliassen gebruikt. Houd er rekening mee dat dit optioneel is zoals wordt weergegeven tijdens het projecteren van de tweede waarde als `NameInfo`. 

Als een query twee eigenschappen met dezelfde naam bevat, moet aliasing worden gebruikt om de naam van een of beide van de eigenschappen, zodat ze zijn disambiguated in het verwachte resultaat te.

**Query**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Scalaire expressies
Naast de eigenschap verwijzingen wordt gemaakt ondersteunt de SELECT-component ook scalaire expressies, zoals constanten, rekenkundige expressies, logische expressies, enzovoort. Hier is bijvoorbeeld een eenvoudige "Hallo wereld" query.

**Query**

    SELECT "Hello World"

**Resultaten**

    [{
      "$1": "Hello World"
    }]


Hier volgt een meer complexe voorbeeld waarbij een scalaire expressie.

**Query**

    SELECT ((2 + 11 % 7)-2)/3   

**Resultaten**

    [{
      "$1": 1.33333
    }]


In het volgende voorbeeld is het resultaat van de scalaire expressie die een Booleaanse waarde.

**Query**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**Resultaten**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Object en matrix maken
Een andere belangrijke functie van DocumentDB SQL is matrix/objecten maken. In het vorige voorbeeld, houd er rekening mee dat we hebben gemaakt een nieuw JSON-object. Een kunt op dezelfde manier ook matrices maken zoals wordt weergegeven in de volgende voorbeelden.

**Query**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**Resultaten**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>WAARDE trefwoord
Het trefwoord **waarde** biedt een manier om JSON waarde te retourneren. De query die hieronder wordt bijvoorbeeld de scalaire `"Hello World"` in plaats van `{$1: "Hello World"}`.

**Query**

    SELECT VALUE "Hello World"

**Resultaten**

    [
      "Hello World"
    ]


De volgende query retourneert de waarde JSON zonder de `"address"` label in de zoekresultaten.

**Query**

    SELECT VALUE f.address
    FROM Families f 

**Resultaten**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

Het volgende voorbeeld breidt deze om te zien hoe om terug te keren JSON primitieve waarden (de knooppuntniveau van de JSON-structuur). 

**Query**

    SELECT VALUE f.address.state
    FROM Families f 

**Resultaten**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* Operator
De speciale operator (*) wordt ondersteund voor het project van het document als-is. Wanneer gebruikt, moet deze het enige verwachte veld. Wanneer u een query zoals `SELECT * FROM Families f` geldig is, `SELECT VALUE * FROM Families f ` en `SELECT *, f.id FROM Families f ` zijn niet geldig.

**Query**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>TOP-operatoren
Het bovenste trefwoord kan worden gebruikt om het aantal waarden uit een query te beperken. Wanneer boven wordt gebruikt in combinatie met de ORDER BY-component, is de resultatenset beperkt tot het eerste N aantal bestelde waarden; anders wordt het eerste N aantal resultaten in een niet-gedefinieerde volgorde. Een aanbevolen om in een SELECT-instructie is altijd een ORDER BY-component met gebruiken de TOP-component. Dit is de enige manier om aan te geven wat welke rijen worden beïnvloed door de BOVENKANT. 


**Query**

    SELECT TOP 1 * 
    FROM Families f 

**Resultaten**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

BOVENSTE kan worden gebruikt met een constante waarde (zoals hierboven) of met de waarde van een variabele query's met parameters. Zie onderstaande parameterquery voor meer informatie.

## <a name="order-by-clause"></a>ORDER BY-component
Zoals in ANSI-SQL, kunt u ook een optioneel Order By-component terwijl de query's uitvoeren. De component kan bevatten een optioneel ASC/DESC argument de volgorde waarin de resultaten moeten worden opgehaald. Voor een gedetailleerde beschrijving Order By, raadpleegt u [DocumentDB volgorde door stapsgewijze instructies](documentdb-orderby.md).

Hier is bijvoorbeeld een query waarmee gezinnen in volgorde van de woont plaatsnaam worden opgehaald.

**Query**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**Resultaten**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

En Hier volgt een query waarmee opgehaald gezinnen in volgorde van aanmaakdatum, die is opgeslagen als een getal dat staat voor de epoche tijd, Internet Explorer, verstreken tijd sinds 1 januari 1970 in seconden.

**Query**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**Resultaten**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>Geavanceerde database concepten en SQL-query 's
### <a name="iteration"></a>Iteratie
Een nieuwe constructie is toegevoegd via **het sleutelwoord in DocumentDB SQL ter ondersteuning van iteratie van JSON matrices** . De bron van biedt ondersteuning voor iteratie. Laten we beginnen met het volgende voorbeeld:

**Query**

    SELECT * 
    FROM Families.children

**Resultaten**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Nu we een andere query waarmee iteratie via kinderen in de verzameling bekijken. Houd rekening met het verschil in de resulterende matrix. In dit voorbeeld wordt gesplitst `children` en worden de resultaten in een matrix samengevoegd.  

**Query**

    SELECT * 
    FROM c IN Families.children

**Resultaten**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

Dit kan verder worden gebruikt om te filteren op elke afzonderlijke vermelding van de matrix, zoals wordt weergegeven in het volgende voorbeeld.

**Query**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Resultaten**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>Joins
In een relationele database is dat u wilt deelnemen aan op tabellen heel belangrijk. Is de logische gevolg van ontwerpen genormaliseerde schema's. In tegenstelling tot dit behandelt DocumentDB het gedenormaliseerde gegevensmodel van documenten schema-vrij te geven. Dit is het logische equivalent van een "self join".

De syntaxis van de die ondersteuning biedt voor de taal is < from_source1 > JOIN < from_source2 > JOIN... Deelnemen aan < from_sourceN >. Algemene, wordt dit een reeks **N**- tupels (tupel met **N** waarden). Elke tupel heeft gemaakt met de iteratie alle siteverzamelingen aliassen van de desbetreffende sets waarden. Dit is met andere woorden, een volledige kruisproduct van de sets deelneemt aan de join definieert.

De volgende voorbeelden wordt de werking van de JOIN-component. In het volgende voorbeeld wordt het resultaat is leeg sinds de kruisproduct van elk document uit de bron en een lege set leeg is.

**Query**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Resultaten**  

    [{
    }]


Klik in het volgende voorbeeld wordt de join is tussen de hoofdmap van het document en de `children` onderliggend hoofdmap. Het is een kruisproduct tussen twee JSON-objecten. Het feit dat kinderen een matrix werkt niet in de JOIN omdat we zijn omgaan met één hoofd die de kinderen-matrix. Het resultaat bevat dus slechts twee resultaten, omdat de kruisproduct van elk document met de matrix precies slechts één document oplevert.

**Query**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**Resultaten**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Het volgende voorbeeld ziet een meer gebruikelijke join:

**Query**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Resultaten**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Het eerste wat u moet Houd er rekening mee dat is de `from_source` van de **JOIN** -component een herhalend element is. Ja, de stroom in dit geval is als volgt:  

-   Elke onderliggend element **c** in de matrix uitvouwen.
-   Een kruisproduct met de hoofdsite van de **f** -document met elke onderliggend element **c** die in de eerste stap is ze zijn teruggebracht toepassen.
-   Ten slotte de hoofdmap object **f** -naameigenschap alleen project. 

Het eerste document (`AndersenFamily`) bevat slechts één onderliggend element, zodat de resultatenset slechts één object overeenkomt met dit document bevat. Het tweede document (`WakefieldFamily`) bevat twee kinderen. Ja, levert het kruisproduct een afzonderlijk object voor elke onderliggende, daarmee ook resulteert in twee objecten, één voor elke onderliggende die overeenkomt met dit document. Houd er rekening mee dat de velden hoofdmap in beide deze documenten worden dezelfde, net zoals u in een kruisproduct verwachten zou.

Formulier tupels is uit de kruisproduct in een vorm die is anderszins moeilijk te project het reële hulpprogramma van de join DEFINIEERT. Zoals we in het onderstaande voorbeeld ziet, kunt u kan bovendien voor filteren op de combinatie van een tupel die kunt de gebruiker hebt gekozen een voorwaarde is voldaan door de tupels algemene.

**Query**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**Resultaten**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



In dit voorbeeld is een natuurlijke uitbreiding van het voorgaande voorbeeld, en een dubbele join uitvoert. Zo is, kunnen de kruisproduct worden weergegeven als de volgende pseudo code.

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`heeft een onderliggende die één huisdier heeft. Zo is, resulteert het kruisproduct in één rij (1*1*1) in deze reeks. WakefieldFamily echter heeft twee onderliggende, maar slechts één onderliggende "Jesse" huisdieren. Jesse heeft door 2 huisdieren. De kruisproduct oplevert dus 1*1*, 2 = 2 rijen uit deze reeks.

In het volgende voorbeeld wordt er is een filter op `pet`. Dit omvat alle tupels waar de bijnaam niet "Schaduw" is niet. Zoals u ziet dat we kunnen bouwen tupels uit matrices, filter op een van de elementen van de tupel, en elke combinatie van de elementen project. 

**Query**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Resultaten**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>JavaScript-integratie
DocumentDB biedt een programming model voor het uitvoeren van JavaScript op basis van toepassingslogica rechtstreeks op de collecties met opgeslagen procedures en triggers. In deze sectie geeft voor beide:

-   Mogelijkheid om te doen krachtige transacties CRUD-bewerkingen en query's voor documenten in een siteverzameling op grond van de naadloze integratie van JavaScript-runtime rechtstreeks vanuit de database-engine. 
-   Een natuurlijke modellering van besturing, variabele een bereik instellen, en toewijzing en integratie van primitieven met databasetransacties verwerking van uitzonderingen. Raadpleeg de JavaScript server kant programmeerfuncties documentatie voor meer informatie over de ondersteuning van DocumentDB voor JavaScript-integratie.

###<a name="user-defined-functions-udfs"></a>Door gebruiker gedefinieerde functies (UDF)
Samen met de typen die al in dit artikel zijn gedefinieerd, biedt DocumentDB SQL ondersteuning voor gebruiker gedefinieerde functies (UDF). Scalaire UDF's worden met name ondersteund waar de ontwikkelaars kunnen doorgeven in nul of meer argumenten en resulteren één argument terug. Elk van deze argumenten zijn ingeschakeld voor juridische JSON-waarden.  

De syntaxis van de DocumentDB SQL is uitgebreid met ondersteuning maatwerktoepassing logica met de gebruiker gedefinieerde functies. UDF's kunnen worden geregistreerd met DocumentDB en vervolgens worden verwezen als onderdeel van een SQL-query. De UDF's zijn in feite exquisitely ontworpen om te worden aangeroepen door de query's. UDF's doen als gevolg van deze keuze geen toegang tot het contextobject waarin de andere JavaScript typen (opgeslagen procedures en triggers) hebben. Aangezien query's als alleen-lezen uitvoeren, kunnen ze op primair of op secundaire replica's worden uitgevoerd. Daarom zijn UDF's ontworpen om uit te voeren op secundaire replica's in tegenstelling tot andere typen JavaScript.

Hieronder volgt een voorbeeld van hoe een UDF kan worden geregistreerd bij de database DocumentDB specifiek onder een documentverzameling.

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
Het voorgaande voorbeeld Hiermee maakt u een UDF waarvan de naam is `REGEX_MATCH`. Deze accepteert twee JSON-tekenreekswaarden `input` en `pattern` en controles als de eerste overeenkomsten het patroon dat is opgegeven in de tweede van JavaScript string.match() functie te gebruiken.


Nu kunnen we dit UDF gebruiken in een query in een raming. UDF's moeten worden gekwalificeerd met het hoofdlettergevoelig voorvoegsel "udf." Wanneer aangeroepen vanuit query's. 

>[AZURE.NOTE] Voordat u 3/17/2015 verlengt ondersteund DocumentDB UDF oproepen zonder de "udf." voorvoegsel zoals REGEX_MATCH() selecteren. Dit bellen patroon is afgeschaft.  

**Query**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Resultaten**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

De UDF kan ook worden gebruikt in een filter, zoals wordt weergegeven in het onderstaande voorbeeld ook gekwalificeerd met de 'udf." voorvoegsel:

**Query**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Resultaten**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


In wezen UDF's geldig scalaire expressies zijn en kunnen worden gebruikt in zowel prognoses en filters. 

Als u wilt weergeven op de kracht van UDF's, laten we even naar een ander voorbeeld met voorwaardelijke logica:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
Hieronder vindt u een voorbeeld van de UDF oefeningen.

**Query**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**Resultaten**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Als de voorgaande voorbeelden laten, integreren UDF's de kracht van JavaScript-taal met de SQL DocumentDB biedt een uitgebreide programmeerbaar interface om uit te voeren complexe procedurele, voorwaardelijke logica met behulp van ingebouwde JavaScript runtime mogelijkheden.

DocumentDB SQL biedt de argumenten naar de UDF's voor elk document in de bron in de huidige fase (WHERE-component of SELECT-component) van het verwerken van de UDF. Het resultaat is opgenomen in de algehele pijplijn naadloos. Als de eigenschappen waarnaar wordt verwezen door de UDF parameters zijn niet beschikbaar in de JSON-waarde, de parameter beschouwd als niet gedefinieerd en dus de aanroep UDF volledig is overgeslagen. Op dezelfde manier als het resultaat van de UDF gedefinieerd is, deze niet opgenomen in het resultaat. 

In het overzicht zijn UDF's handige hulpmiddelen complexe bedrijfslogica doen als onderdeel van de query.

### <a name="operator-evaluation"></a>Evaluatie van de operator
DocumentDB, tekent door het feit dat een database JSON, u specifiek met JavaScript-operators en de semantiek evaluatie. Terwijl DocumentDB probeert te bewaren JavaScript semantiek in JSON-ondersteuning, wordt de bewerking evaluatie afwijkt in sommige gevallen.

In DocumentDB SQL, zijn in tegenstelling tot in traditionele SQL, de typen waarden vaak niet bekend totdat de waarden daadwerkelijk worden opgehaald uit een database. Om te kunnen efficiënt uitvoeren van query's, hebben meestal de operatoren strikte type vereisten. 

DocumentDB SQL uitvoeren niet impliciete conversies, in tegenstelling tot JavaScript. Bijvoorbeeld een query zoals `SELECT * FROM Person p WHERE p.Age = 21` komt overeen met documenten die een leeftijd eigenschap waarvan de waarde 21 bevatten. Andere documenten waarvan leeftijd de eigenschap komt overeen met de tekenreeks '21' of andere mogelijk oneindig variaties zoals "021", "21,0", "0021", "00021", enzovoort wordt niet overeen. Hiermee wordt daarentegen de JavaScript waar de tekenreekswaarden impliciet omgezet naar getallen zijn (op basis van de operator, ex: ==). Deze optie is essentieel voor efficiënte index zoeken in DocumentDB SQL. 

## <a name="parameterized-sql-queries"></a>SQL-query's met parameters
DocumentDB ondersteuning biedt voor query's met parameters uitgedrukt met de vertrouwde @ notatie. Met parameters SQL biedt robuuste afhandelen en ontsnappen van invoer van de gebruiker, per ongeluk weergeven van gegevens via SQL uitvoering voorkomen. 

U kunt bijvoorbeeld een query waarmee achternaam en adres status als parameters te schrijven en vervolgens uitvoeren voor verschillende waarden van de achternaam en adres status op basis van de invoer van de gebruiker.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Deze aanvraag kunt vervolgens worden verzonden naar DocumentDB als een query met parameters JSON zoals hieronder wordt weergegeven.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Het argument naar begin kan worden ingesteld met query's met parameters, zoals hieronder wordt weergegeven.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parameterwaarden kunnen bestaan uit een geldige JSON (tekenreeksen, getallen, Booleaanse waarden, null, zelfs matrices of geneste JSON). Ook aangezien DocumentDB schema minder, worden parameters niet gevalideerd op basis van een willekeurig type.

##<a name="built-in-functions"></a>Ingebouwde functies
DocumentDB ondersteunt ook een aantal ingebouwde functies voor veelgebruikte bewerkingen, die kunnen worden gebruikt in query's zoals door de gebruiker gedefinieerde functies (UDF's).

<table>
<tr>
<td>Wiskundige functies</td> 
<td>ABS, CEILING, EXP, FLOOR, logboek, LOG10, POWER, ronde, aanmelden, WORTEL, vierkante, TRUNC, BOOGCOS, BOOGSIN, BOOGTAN, ATN2, COS, COT, graden PI, radialen, SIN en TAN</td>
</tr>
<tr>
<td>Typ foutcontrole functies</td>    
<td>IS_ARRAY, IS_BOOL IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED en IS_PRIMITIVE</td>
</tr>
<tr>
<td>Tekenreeksfuncties</td>   
<td>Functie samenvoegen, bevat ENDSWITH, INDEX_OF, links, lengte, kleine.letters, de functies LTRIM, vervangen, REPLICEREN, OMKEREN, rechts, RTRIM, STARTSWITH, SUBTEKENREEKS en hoofdletters</td>
</tr>
<tr>
<td>Matrixfuncties</td>    
<td>ARRAY_CONCAT, ARRAY_CONTAINS ARRAY_LENGTH en ARRAY_SLICE</td>
</tr>
<tr>
<td>Ruimte-functies</td>  
<td>ST_DISTANCE, ST_WITHIN ST_ISVALID en ST_ISVALIDDETAILED</td>
</tr>
</table>  

Als u een door de gebruiker gedefinieerde functie (UDF) waarvoor een ingebouwde functie nu beschikbaar is op dit moment gebruikt, moet u de bijbehorende ingebouwde functie gebruiken als dit is het verstandig om sneller om uit te voeren en efficiënter. 

### <a name="mathematical-functions"></a>Wiskundige functies
De wiskundige functies elke uitvoeren een berekening, meestal op basis van invoerwaarden die als argumenten worden verstrekt en een numerieke waarde te retourneren. Hier volgt een lijst met ondersteunde ingebouwde wiskundige functies.

<table>
<tr>
<td><strong>Gebruik</strong></td>
<td><strong>Beschrijving</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (num_expr)</a></td> 
<td>Geeft als resultaat de absolute (positieve) waarde van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">CEILING (num_expr)</a></td> 
<td>Geeft als resultaat de kleinste waarde in de gehele getal groter dan of gelijk is aan de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">FLOOR (num_expr)</a></td> 
<td>Geeft als resultaat de grootste integer die kleiner is dan of gelijk is aan de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (num_expr)</a></td> 
<td>Geeft als resultaat de exponent van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">LOG (num_expr [, base])</a></td> 
<td>Geeft als resultaat de natuurlijke logaritme van de opgegeven numerieke expressie of de logaritme met het opgegeven grondtal</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">LOG10 (num_expr)</a></td> 
<td>Geeft als resultaat de logaritmische waarde grondtal 10 van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">AFRONDEN (num_expr)</a></td> 
<td>Geeft als resultaat een numerieke waarde, afgerond op het dichtstbijzijnde gehele getal.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">TRUNC (num_expr)</a></td> 
<td>Geeft als resultaat een numerieke waarde, afgekapt tot het dichtstbijzijnde gehele getal.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">WORTEL (num_expr)</a></td>   
<td>Geeft als resultaat de vierkantswortel van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">Vierkante (num_expr)</a></td>   
<td>Geeft als resultaat het kwadraat van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">POWER (num_expr, num_expr)</a></td>   
<td>Geeft als resultaat de kracht van de opgegeven numerieke expressie met de opgegeven waarde.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">TEKEN (num_expr)</a></td>   
<td>Retourneert de waarde teken (-1, 0, 1) van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">BOOGCOS (num_expr)</a></td>   
<td>Geeft als resultaat de hoek in radialen, waarvan de cosinus de opgegeven numerieke expressie is. een afkorting boogcosinus.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">BOOGSIN (num_expr)</a></td>   
<td>Geeft als resultaat de hoek in radialen, waarvan de sinus de opgegeven numerieke expressie is. Dit wordt ook boogsinus genoemd.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">BOOGTAN (num_expr)</a></td>   
<td>Geeft als resultaat de hoek in radialen, waarvan de tangens de opgegeven numerieke expressie is. Dit wordt ook boogtangens genoemd.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (num_expr)</a></td>   
<td>Geeft als resultaat de hoek in radialen, tussen de positieve x-as en de ray vanaf de oorsprong bondig (y, x), waarbij x en y de waarden van de twee opgegeven toegestane achterstand-expressies zijn.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (num_expr)</a></td> 
<td>Geeft als resultaat de cosinus trigonometrische van de opgegeven hoek in radialen, in de opgegeven expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (num_expr)</a></td> 
<td>Geeft als resultaat de trigonometrische cotangens van de opgegeven hoek in radialen, in de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">GRADEN (num_expr)</a></td> 
<td>Geeft als resultaat de bijbehorende hoek in graden voor een in radialen uitgedrukte hoek.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">PI)</a></td>   
<td>Geeft als resultaat de waarde van de constante pi.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">RADIALEN (num_expr)</a></td> 
<td>Geeft als resultaat radialen wanneer een numerieke expressie, in graden wordt ingevoerd.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (num_expr)</a></td> 
<td>Geeft als resultaat de trigonometrische sinus van de opgegeven hoek in radialen, in de opgegeven expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">TAN (num_expr)</a></td> 
<td>Geeft als resultaat de tangens van de invoer expressie, in de opgegeven expressie.</td>
</tr>

</table> 

U kunt nu bijvoorbeeld query's zoals de volgende uitvoeren:

**Query**

    SELECT VALUE ABS(-4)

**Resultaten**

    [4]

Het belangrijkste verschil tussen de DocumentDB functies, vergeleken met ANSI SQL is dat ze zijn ontworpen om te werken ook met schema-minder en gemengde schemagegevens. Bijvoorbeeld, als er een document waarop de eigenschap Size ontbreekt of is een niet-numerieke waarde, zoals 'Onbekend' en het document is overgeslagen, klikt u in plaats van een fout wordt geretourneerd.

### <a name="type-checking-functions"></a>Typ foutcontrole functies
Het type foutcontrole functies kunnen u het type van een expressie in SQL-query's. Type foutcontrole functies kunnen worden gebruikt om te bepalen het type van eigenschappen in documenten in de browser wanneer het variabel of onbekend is. Hier volgt een lijst met ondersteunde ingebouwd type functies controleren.

<table>
<tr>
  <td><strong>Gebruik</strong></td>
  <td><strong>Beschrijving</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (expr)</a></td>
  <td>Geeft als resultaat een Booleaanse waarde die aangeeft dat u als het type van de waarde een matrix is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (expr)</a></td>
  <td>Geeft als resultaat een Booleaanse waarde die aangeeft dat u als het type van de waarde een Booleaans is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (expr)</a></td>
  <td>Geeft als resultaat een Booleaanse waarde die aangeeft of het type van de waarde null is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (expr)</a></td>
  <td>Geeft als resultaat een Booleaanse waarde die aangeeft dat u als het type van de waarde een getal is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (expr)</a></td>
  <td>Geeft als resultaat een Booleaanse waarde die aangeeft dat u als het type van de waarde een JSON-object is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (expr)</a></td>
  <td>Geeft als resultaat een Booleaanse waarde die aangeeft dat u als het type van de waarde een tekenreeks is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (expr)</a></td>
  <td>Geeft als resultaat een Booleaanse waarde die aangeeft dat u als de eigenschap een waarde is toegewezen.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (expr)</a></td>
  <td>Geeft als resultaat een Booleaanse waarde die aangeeft of het type van de waarde een tekenreeks, een getal, een Booleaanse waarde of een null-waarden is.</td>
</tr>

</table>

Met deze functies, kunt u nu uitvoeren query's als volgt uit:

**Query**

    SELECT VALUE IS_NUMBER(-4)

**Resultaten**

    [true]

### <a name="string-functions"></a>Tekenreeksfuncties
De volgende scalaire functies een bewerking uitvoeren op een tekenreekswaarde van de invoer en terug te keren een tekenreeks, numerieke of Booleaanse waarde. Hier volgt een tabel met ingebouwde tekenreeksfuncties:

Gebruik|Beschrijving
---|---
[LENGTE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|Geeft als resultaat het aantal tekens van de opgegeven tekenreeksexpressie
[SAMENVOEGEN (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|Geeft als resultaat een tekenreeks die het resultaat is van twee of meer tekenreekswaarden aaneen.
[SUBTEKENREEKS (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|Geeft als resultaat een deel van een tekenreeksexpressie.
[STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|Geeft een Booleaanse die aangeeft of de eerste tekenreeksexpressie die eindigt met de tweede
[ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|Geeft een Booleaanse die aangeeft of de eerste tekenreeksexpressie die eindigt met de tweede
[BEVAT (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|Geeft een Booleaanse die aangeeft of de eerste tekenreeksexpressie die het tweede bevat.
[INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|Geeft als resultaat de beginpositie van het eerste exemplaar van de tweede tekenreeksexpressie die binnen de eerste expressie in de aangegeven tekenreeks of -1 als de tekenreeks niet wordt gevonden.
[LEFT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|Geeft als resultaat het linkerdeel van een tekenreeks met het opgegeven aantal tekens.
[RIGHT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|Geeft als resultaat het juiste deel van een tekenreeks met het opgegeven aantal tekens.
[LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|Geeft als resultaat een tekenreeksexpressie na het verwijderen van toonaangevende lege waarden.
[RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|Geeft als resultaat een tekenreeksexpressie nadat alle volgspaties afgekapt.
[KLEINE.letters (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|Geeft als resultaat een tekenreeksexpressie na hoofdletters Tekengegevens converteren naar kleine letters.
[HOOFDLETTERS (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|Geeft als resultaat een tekenreeksexpressie na het converteren van een kleine letter gegevens om in hoofdletters.
[VERVANGEN (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|Alle exemplaren van de waarde van een opgegeven tekenreeks vervangen door een andere tekenreekswaarde.
[REPLICEREN (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|Hiermee herhaalt u de tekenreekswaarde van een in een opgegeven aantal keren.
[OMKEREN (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|Geeft als resultaat de omgekeerde volgorde van een tekenreekswaarde.

U kunt nu een query's zoals de volgende uitvoeren voor het gebruik van deze functies. Bijvoorbeeld: u kunt de achternaam in retourneren hoofdletters als volgt:

**Query**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Resultaten**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Of tekst.samenvoegen tekenreeksen zoals in dit voorbeeld:

**Query**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Resultaten**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Tekenreeksfuncties kunnen ook worden gebruikt in de WHERE-component om resultaten te filteren, zoals in het volgende voorbeeld:

**Query**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Resultaten**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Matrixfuncties
De volgende scalaire functies een bewerking uitvoeren op een matrix invoerwaarde en return numerieke, Booleaanse waarde of een matrix-waarde. Hier volgt een tabel met ingebouwde matrixfuncties:

Gebruik|Beschrijving
---|---
[ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|Geeft als resultaat het aantal elementen van de opgegeven matrix-expressie.
[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|Geeft als resultaat een matrix die het resultaat is van het samenvoegen van twee of meer matrixwaarden.
[ARRAY_CONTAINS (arr_expr, expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|Geeft als resultaat een Booleaanse waarde die aangeeft of de matrix de opgegeven waarde bevat.
[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|Geeft als resultaat een onderdeel van een matrix-expressie.

Matrixfuncties kunnen worden gebruikt voor het bewerken van matrices in JSON. Hier is bijvoorbeeld een query die retourneert alle documenten waarbij een van de bovenliggende items "Robin Wakefield". 

**Query**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Resultaten**

    [{
      "id": "WakefieldFamily"
    }]

Hier ziet u een ander voorbeeld die wordt gebruikt ARRAY_LENGTH om het aantal kinderen per familie.

**Query**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Resultaten**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Ruimte-functies

DocumentDB ondersteunt de volgende geopende georuimtelijke Consortium (OGC) ingebouwde functies voor georuimtelijke query's uitvoeren. Voor meer informatie over georuimtelijke ondersteuning in DocumentDB, raadpleegt u [werken met georuimtelijke gegevens in Azure DocumentDB](documentdb-geospatial.md). 

<table>
<tr>
  <td><strong>Gebruik</strong></td>
  <td><strong>Beschrijving</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Retourneert de afstand tussen de twee GeoJSON punt expressies.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Geeft als resultaat een Booleaanse expressie die aangeeft of de komma GeoJSON is opgegeven in het eerste argument binnen de veelhoek GeoJSON in het tweede argument is.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Geeft als resultaat een Booleaanse waarde die aangeeft of de opgegeven GeoJSON punt of veelhoek expressie geldig is.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Geeft als resultaat de waarde van een JSON met een Booleaanse waarde als de opgegeven GeoJSON punt of veelhoek expressie geldig is en ongeldige, bovendien de reden als een tekenreekswaarde.</td>
</tr>
</table>

Ruimte functies kunnen worden gebruikt om uit te voeren nabijheid querries ten opzichte van de ruimte gegevens. Hier is bijvoorbeeld een query die alle family documenten die zijn binnen 30 kilometer van de opgegeven locatie met de ingebouwde functie ST_DISTANCE retourneert. 

**Query**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Resultaten**

    [{
      "id": "WakefieldFamily"
    }]

Als u ruimte indexering in uw indexing beleid, moeten klikt u vervolgens 'afstand query's ' worden verzonden efficiënt tot en met de index. Voor meer informatie over de ruimte indexeren, raadpleegt u de sectie hieronder. Als u een ruimte index voor de opgegeven paden niet hebt, kunt u nog steeds ruimte query's uitvoeren door op te geven `x-ms-documentdb-query-enable-scan` verzoek koptekst met de waarde ingesteld op "true". In .NET, kan dit doen door het optionele argument van de **FeedOptions** tot query's met [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) ingesteld op waar. 

ST_WITHIN kan worden gebruikt om te controleren als een punt binnen een veelhoek veroorzaakt. Veelhoek worden meestal gebruikt om aan te geven grenzen zoals postcodes verwijderd, staat grenzen of natuurlijke formaties. Opnieuw als u ruimte indexering in uw indexing beleid, klikt u vervolgens 'haaienvin' query's moeten worden verzonden efficiënt tot en met de index. 

Veelhoek argumenten in ST_WITHIN kunnen slechts één keer bevatten, dat wil zeggen de veelhoek mag geen gaten in deze. Controleer de [DocumentDB limieten](documentdb-limits.md) voor het maximum aantal punten in een veelhoek toegestaan voor een query ST_WITHIN.

**Query**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Resultaten**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Vergelijkbaar met hoe niet-overeenkomende typen werkt in DocumentDB query, als de locatiewaarde die is opgegeven in een van de argumenten is onjuist of ongeldige en klik vervolgens deze **ongedefinieerd** en het geëvalueerde document van de queryresultaten worden overgeslagen wordt geëvalueerd. Als uw query geen resultaten worden geretourneerd, voert u ST_ISVALIDDETAILED naar foutopsporing waarom het type spatail ongeldig is.     

ST_ISVALID en ST_ISVALIDDETAILED kan worden gebruikt om te controleren of een ruimte object geldig is. De volgende query wordt bijvoorbeeld de geldigheid van een komma met een automatisch niet bereik Breedtegraadwaarde (-132.8) gecontroleerd. ST_ISVALID alleen een Booleaanse waarde retourneert en ST_ISVALIDDETAILED geeft als resultaat de Booleaanse waarde en een tekenreeks met de reden waarom het volgende ongeldig wordt.

**Query**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Resultaten**

    [{
      "$1": false
    }]

Deze functies kunnen ook worden gebruikt voor het valideren van veelhoek. Bijvoorbeeld hier we gebruiken ST_ISVALIDDETAILED voor het valideren van een veelhoek die niet is gesloten. 

**Query**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Resultaten**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
Die terugloopt van ruimte functies en de SQL-syntaxis voor DocumentDB. Nu eens kijken hoe LINQ query's uitvoeren werkt en hoe deze samenwerkt met de syntaxis van de we hebt gezien dusverre.

## <a name="linq-to-documentdb-sql"></a>LINQ naar DocumentDB SQL
LINQ is een .NET programming model waarmee berekening als query's op streams van objecten. DocumentDB biedt een client vragen die aan de bibliotheek om een koppeling met LINQ doordat een conversie tussen JSON en .NET-objecten en een toewijzing van een subset van LINQ naar DocumentDB query's. 

De onderstaande afbeelding ziet de architectuur van ondersteunende LINQ query's met DocumentDB.  Met de client DocumentDB, kunnen ontwikkelaars een **IQueryable** -object dat de DocumentDB query provider, klikt u vervolgens de query LINQ in de query DocumentDB equivalent rechtstreeks een query maken. De query vervolgens naar de server DocumentDB om op te halen van een reeks resultaten in de indeling van JSON doorgegeven. De geretourneerde resultaten zijn in een stroom van .NET-objecten aan de clientzijde gedeserialiseerd.

![Architectuur van ondersteunende LINQ query's met DocumentDB - SQL-syntaxis, JSON querytaal, database concepten en SQL-query 's][1]
 


### <a name="net-and-json-mapping"></a>.NET en JSON-toewijzing
De toewijzing tussen .NET-objecten en JSON documenten is natuurlijke - elk lid gegevensveld is toegewezen aan een JSON-object, waarbij de naam van het veld is toegewezen aan de "sleutel" deel van het object en het gedeelte 'waarde' recursief toegewezen aan het deel van de waarde van het object. Bekijk het volgende voorbeeld. Het familie object gemaakt wordt toegewezen aan de JSON-document, zoals hieronder wordt weergegeven. En omgekeerd, het document JSON terug naar een .NET-object is toegewezen.

**C#-klasse**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ SQL vertaling
De provider van de query DocumentDB kunt u een aanbevolen hoeveelheid toewijzing uitvoeren vanuit een LINQ-query in een DocumentDB SQL-query. In de volgende beschrijving, nemen we aan dat de lezer heeft een eenvoudige vertrouwde LINQ.

Voor het typesysteem ondersteunen we eerst alle JSON primitieve typen – numerieke typen, Booleaanse waarde tekenreeks en null-waarden. Alleen deze typen JSON worden ondersteund. De volgende scalaire expressies worden ondersteund.

-   Constante waarden – deze constante waarden van de primitieve gegevenstypen bevat op het moment dat de query wordt geëvalueerd.

-   Expressies in de eigenschap/matrix-index – deze expressies raadpleegt u de eigenschap van een object of een matrixelement.

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   Rekenkundige expressies - hierbij veelgebruikte rekenkundige expressies van numerieke en Booleaanse waarden. Raadpleeg de SQL-specificatie voor de volledige lijst.

        2 * family.children[0].grade;
        x + y;

-   Vergelijking van tekenreeksexpressie - hierbij een tekenreekswaarde op sommige constante tekenreekswaarde vergelijken.  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   Object/matrix maken expressie - deze afzender expressies een object van samengestelde waardetype of anonieme of een matrix van dergelijke objecten. Deze waarden kunnen worden genest.

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>Lijst met ondersteunde LINQ-operators
Hier volgt een lijst met ondersteunde LINQ operatoren in de LINQ-provider wordt geleverd bij de DocumentDB .NET SDK.

-   **Selecteer**: prognoses vertalen naar de SQL SELECT inclusief objectconstructie
-   **Waar**: Filters vertalen naar de SQL WHERE en ondersteuning voor vertaling tussen & &, || en! naar de SQL-operatoren
-   **SelectMany**: kan ongedaan maken van matrices naar de SQL-JOIN-component. Kunnen worden gebruikt om ketting/nesten expressies om te filteren op matrixelementen
-   **Sorteren op en OrderByDescending**: equivalent ORDER BY oplopend/aflopend:
-   **CompareTo**: equivalent bereik vergelijkingen. Veelgebruikte voor tekenreeksen aangezien die geen vergelijkbare in .NET
-   **Nemen**: equivalent naar het begin van de SQL voor het beperken van de resultaten van een query
-   **Wiskundige functies**: ondersteuning biedt voor vertaling uit. NET van Abs, BOOGCOS, BOOGSIN, BOOGTAN, maximum, Cos, Exp, Floor, logboek, Log10, Pow, afronden, aanmelden, Sin, WORTEL, Tan, Truncate aan de overeenkomstige ingebouwde SQL-functies.
-   **Tekenreeksfuncties**: ondersteuning biedt voor vertaling uit. NET van samenvoegen, bevat EndsWith, IndexOf, aantal, ToLower, TrimStart, vervangen, omkeren, TrimEnd, StartsWith, subtekenreeks, ToUpper aan de overeenkomstige ingebouwde SQL-functies.
-   **Matrixfuncties**: ondersteuning biedt voor vertaling uit. NET van samenvoegen, bevat en tellen aan de overeenkomstige ingebouwde SQL-functies.
-   **Georuimtelijke Extensiefuncties**: vertaling van stub methoden afstand haaienvin, IsValid en IsValidDetailed naar de overeenkomstige ingebouwde SQL-functies ondersteunt.
-   **De gebruiker gedefinieerde functie extensie functie**: ondersteunt vertaling van de methode stub UserDefinedFunctionProvider.Invoke naar de bijbehorende gebruiker gedefinieerde functie.
-   **Overige**: vertaling van de coalesce en voorwaardelijke operators ondersteunt. Kunnen vertalen bevat tekenreeks bevat, ARRAY_CONTAINS of de SQL IN afhankelijk van de context.

### <a name="sql-query-operators"></a>SQL-query 's
Hier volgen enkele voorbeelden die aangeven hoe enkele van de standaard LINQ voor query's worden geconverteerd naar beneden af op DocumentDB query's.

#### <a name="select-operator"></a>Selecteer Operator
De syntaxis van de is `input.Select(x => f(x))`, waarbij `f` is een scalaire expressie.

**LINQ lambda-expressie**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda-expressie**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda-expressie**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany operator
De syntaxis van de is `input.SelectMany(x => f(x))`, waarbij `f` is een scalaire expressie die resulteert in een siteverzameling type.

**LINQ lambda-expressie**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Waar operator
De syntaxis van de is `input.Where(x => f(x))`, waarbij `f` is een scalaire expressie die een Booleaanse waarde retourneert.

**LINQ lambda-expressie**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda-expressie**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Samengestelde SQL-query 's
De bovenstaande operatoren kunnen bestaan om krachtige query's. Aangezien DocumentDB geneste verzamelingen ondersteunt, kan de samenstelling worden samengevoegd of geneste.

#### <a name="concatenation"></a>Samenvoeging 

De syntaxis van de is `input(.|.SelectMany())(.Select()|.Where())*`. Een samengevoegde query kunt beginnen met een optionele `SelectMany` query, gevolgd door meerdere `Select` of `Where` operatoren.


**LINQ lambda-expressie**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda-expressie**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda-expressie**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda-expressie**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Geneste

De syntaxis van de is `input.SelectMany(x=>x.Q())` waar Q is een `Select`, `SelectMany`, of `Where` operator.

In een geneste query, wordt de binnenste query toegepast op elk element van de buitenste verzameling. Een belangrijke mogelijkheid is dat de binnenste query naar de velden van de elementen in de buitenste verzameling zoals verwijzen kan Self-joins.

**LINQ lambda-expressie**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda-expressie**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda-expressie**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>SQL-query's uitvoeren
DocumentDB beschrijft bronnen via een REST API die kan worden aangeroepen door een staat HTTP/HTTPS-verzoeken taal. Bovendien biedt DocumentDB programming bibliotheken voor verschillende populaire talen zoals .NET, Node.js, JavaScript en Python. De REST API en de verschillende bibliotheken ondersteuning voor query's uitvoeren via SQL. De .NET SDK ondersteunt LINQ query's uitvoeren naast SQL.

De volgende voorbeelden wordt getoond hoe u een query maken en dien het in tegen een account van de database DocumentDB.

### <a name="rest-api"></a>REST API
DocumentDB biedt een RESTful programming model openen via HTTP. Accounts van de database kunnen worden ingericht met een Azure-abonnement. Het model van de resource DocumentDB bestaat uit een sets met bronnen onder een databaseaccount, die wordt gebruikt met een logische en stabiele-URI. Een reeks informatiebronnen is een feed in dit document genoemd. Een databaseaccount bestaat uit een set-databases, elk met meerdere siteverzamelingen, elk van welke omslaan in documenten, UDF's en andere resourcetypen bevatten.

Het model eenvoudige interactie met de volgende informatiebronnen is via de HTTP-woorden GET, opslag, POST en verwijderen met hun standaard interpretatie. De bewerking bericht wordt gebruikt voor het maken van een nieuwe resource, voor het uitvoeren van een opgeslagen procedure of voor het verlenen van een query DocumentDB. Query's worden altijd alleen bewerkingen met geen bijwerkingen opgelezen.

De volgende voorbeelden wordt een bericht voor een DocumentDB query ten opzichte van een siteverzameling met de twee steekproeven documenten dat we tot nu toe hebt bekeken. De query heeft een eenvoudig filter van de JSON-naameigenschap. Houd rekening met het gebruik van de `x-ms-documentdb-isquery` en inhoudstype: `application/query+json` kopteksten om aan te geven dat de bewerking een query.


**Aanvragen**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**Resultaten**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Het tweede voorbeeld ziet een meer complexe query waarmee meerdere resultaten geretourneerd uit de join definieert.

**Aanvragen**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Resultaten**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Als de resultaten van een query niet binnen één pagina met resultaten past en vervolgens de REST API geeft als een token voortgezet tot en met resultaat de `x-ms-continuation-token` tekenarcering. Clients kunnen resultaten pagineren door de koptekst op te nemen in de bijbehorende resultaten. Het aantal resultaten per pagina kan ook worden beheerd via de `x-ms-max-item-count` getal koptekst.

Als u wilt het beleid voor de consistentie van gegevens voor query's beheren, gebruikt u de `x-ms-consistency-level` kop bijvoorbeeld alle REST API aanvragen. Voor sessie consistentie, is dit moet u ook de meest recente echo `x-ms-session-token` Cookie kop in het verzoek van de query. Houd er rekening mee dat de aangevraagde verzameling indexing beleid ook de consistentie van queryresultaten kunt beïnvloeden. Met standaard indexeren beleidsinstellingen voor siteverzamelingen de index wordt gebruikt met de inhoud van het document en queryresultaten komen overeen met de consistentie gekozen voor gegevens. Als het indexeren beleid is versoepeld naar Lazy, kunnen klikt u vervolgens query's verouderde resultaten geretourneerd. Voor meer informatie raadpleegt u [DocumentDB consistentie niveaus] [consistency-levels].

Als de opgegeven query kan niet worden ondersteund in de geconfigureerde indexing beleid voor de collectie, retourneert de server DocumentDB 400 'Ongeldige aanvraag". Dit is geretourneerd voor query's bereik tegen paden geconfigureerd voor zoekacties hash (gelijke) en voor paden expliciet die zijn uitgesloten van indexering. De `x-ms-documentdb-query-enable-scan` kop kunt opgeven dat de query scannen wanneer een index niet beschikbaar is zijn toegestaan.

### <a name="c-net-sdk"></a>C# (.NET) SDK
De .NET SDK ondersteunt LINQ zowel SQL query's uitvoeren. Het volgende voorbeeld ziet u hoe u de query eenvoudig filter is geïntroduceerd eerder in dit document uitvoert.


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


In dit voorbeeld worden twee eigenschappen voor gelijke binnen elk document vergeleken en anonieme prognoses gebruikt. 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Het volgende voorbeeld ziet joins af wijzen, uitgedrukt tot en met LINQ SelectMany.


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



De .NET-client is automatisch doorlopen van alle pagina's van queryresultaten in de blokken foreach zoals hierboven. De queryopties in de sectie REST API kennis zijn ook beschikbaar in het .NET SDK met de `FeedOptions` en `FeedResponse` klassen in de methode CreateDocumentQuery. Het aantal pagina's kan worden beheerd met de `MaxItemCount` instelling. 

U kunt ook expliciet paginering bepalen door te maken `IDocumentQueryable` met de `IQueryable` object, klikt u vervolgens op het lezen van de` ResponseContinuationToken` back-waarden en deze worden doorgegeven als `RequestContinuationToken` in `FeedOptions`. `EnableScanInQuery`kan worden ingesteld op scannen inschakelen wanneer de query kan niet worden ondersteund door de geconfigureerde indexing beleid. Voor de gepartitioneerde collecties, kunt u `PartitionKey` aan de query uitvoeren voor één partitioneren (Hoewel DocumentDB dit automatisch van de querytekst ophalen kunt), en `EnableCrossPartitionQuery` uitvoeren van query's die mogelijk moet u voor meerdere partities worden uitgevoerd. 

Raadpleeg [DocumentDB .NET voorbeelden](https://github.com/Azure/azure-documentdb-net) voor meer voorbeelden met query's. 

### <a name="javascript-server-side-api"></a>JavaScript-API voor servers 
DocumentDB biedt een programming model voor het uitvoeren van JavaScript op basis van toepassingslogica rechtstreeks op de collecties met opgeslagen procedures en triggers. De JavaScript-logica geregistreerd bij een niveau van de siteverzameling kunt databasebewerkingen op de bewerkingen van de documenten van de opgegeven verzameling verlenen. Deze bewerkingen zijn samengevoegd in de ZURE transacties.

Het volgende voorbeeld wordt aangegeven hoe de queryDocuments gebruik in de JavaScript-server API om query's vanuit binnenkant opgeslagen procedures en triggers.


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>Statistische functies

Ondersteuning voor aggregatiefuncties is in het identiteitsprogramma werkt, maar als u aantal of som. functionaliteit in de tussentijd nodig hebt, kunt u hetzelfde resultaat met behulp van verschillende methoden bereiken.  

Klik op meer pad:

- U kunt statistische functies uitvoeren met de gegevens ophalen en een telling lokaal doen. Het gebruik van een raming goedkope query zoals heeft aanbevolen `SELECT VALUE 1` in plaats van de volledige document zoals `SELECT * FROM c`. Hiermee kunt maximaliseren van het aantal documenten verwerkt in elke pagina met resultaten, aanvullende gegevens van en naar de service zodat er geen indien nodig.
- U kunt een opgeslagen procedure ook gebruiken om te minimaliseren netwerklatentie op herhaalde interactie. Zie [Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js)voor een steekproef opgeslagen procedure waarmee het aantal voor een bepaald filterquery's wordt berekend. De opgeslagen procedure kunt kunnen gebruikers uitgebreide bedrijfslogica samen met aggregaties doen op efficiënte wijze combineren.

Klik op schrijven pad:

- Een andere algemene patroon is vooraf aggregeren de resultaten in het pad 'schrijven'. Dit is bijzonder aantrekkelijk wanneer u het volume van aanvragen 'gelezen' hoger zijn dan die van aanvragen 'schrijven' is. Eenmaal vooraf samengevoegde, de resultaten zijn beschikbaar met één punt verzoek lezen.  De beste manier om vooraf aggregeren in DocumentDB is een trigger dat wordt aangeroepen met elke 'schrijven' instellen en bijwerken van een metagegevensdocument met de meest recente resultaten voor de query die wordt worden gerealiseerd. Bijvoorbeeld: Bekijk de steekproef [UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js) , waarin de minimaal, maxSize en totalSize van het metagegevensdocument voor de collectie wordt bijgewerkt. De steekproef kan worden verlengd als u wilt bijwerken van een item, som, enzovoort.

##<a name="references"></a>Verwijzingen
1.  [Inleiding tot Azure DocumentDB][introduction]
2.  [DocumentDB SQL-specificatie](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [Voorbeelden van DocumentDB .NET](https://github.com/Azure/azure-documentdb-net)
4.  [DocumentDB consistentie niveaus][consistency-levels]
5.  ANSI SQL-2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON [http://json.org/](http://json.org/)
7.  JavaScript specificatie [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  Query evaluatietechnieken voor grote databases [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Queryverwerking in parallelle relationele databasesystemen, IEEE Computer Society Press, 1994
11. Lu, Ooi, Tan, queryverwerking in parallelle relationele databasesystemen, IEEE Computer Society Press, 1994.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: varken Latijns: een niet-zodat-vreemde talen voor gegevensverwerking SIGMOD 2008.
13.     G Graefe. Het kader trapsgewijs voor queryoptimalisatie. IEEE gegevens eng Bull., 18, lid 3: 1995.


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
