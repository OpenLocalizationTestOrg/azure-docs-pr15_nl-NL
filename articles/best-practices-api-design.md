<properties
   pageTitle="API ontwerp richtlijnen | Microsoft Azure"
   description="Hulp bij het maken van een bron ontworpen API."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="api-design-guidance"></a>Richtlijnen voor API-ontwerp

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Overzicht

Het gebruik van webservices, die worden gehost door endwebservers, functionaliteit biedt voor externe client toepassingen maken veel moderne web gebaseerde oplossingen De bewerkingen die een webservice toegankelijk wordt gemaakt, vormen gezamenlijk een web API. Een goed ontworpen web API moet zijn gericht ter ondersteuning van:

- **Platformonafhankelijkheid**. Clienttoepassingen moet gebruikmaken van de API waarmee de webservice zonder hoe de gegevens of de bewerkingen die API maakt fysiek worden geïmplementeerd. Hiervoor is vereist dat de API houdt zich door gemeenschappelijke normen waarmee een client-toepassing en Internet service afspreken welke gegevensindelingen wilt gebruiken en de structuur van de gegevens die worden uitgewisseld tussen clienttoepassingen en de webservice.

- **Ontwikkeling van de Service**. De webservice moet kunnen ontwikkelen en tellen (of verwijderen) functionaliteit onafhankelijk van clienttoepassingen. Bestaande clienttoepassingen moeten mogelijk in te blijven functioneren ongewijzigd als de functies van de wijziging van de web-service. Alle functionaliteit moet ook worden gedetecteerd, zodat volledig door clienttoepassingen kunnen gebruiken.

Het doel van deze instructies is om te beschrijven van de problemen waarmee u rekening houden moet bij het ontwerpen van een web API.

## <a name="introduction-to-representational-state-transfer-rest"></a>Inleiding tot het beschrijvende provinciale doorverbinden (REST)

In zijn dialoogvenster 2000, Roy Fielding kan worden voorgesteld een andere architectuur aanpak structureren de bewerkingen die worden aangeboden door webservices; REST. REST is een architectuur stijl voor het samenstellen van gedistribueerde systemen op basis van hypermedia op basis. Een belangrijk voordeel van het model REST is dat deze is gebaseerd op open standaarden en de uitvoering van het model of de clienttoepassingen die toegang toe voor een specifieke implementatie niet kan worden gebonden. Bijvoorbeeld een REST-webservice kan worden geïmplementeerd via de Microsoft ASP.NET Web API en clienttoepassingen kunnen worden ontwikkeld met behulp van de gewenste taal en de set hulpmiddelen waarmee u kunt genereren HTTP-aanvragen en parseren HTTP-antwoorden.

> [AZURE.NOTE] REST werkelijk onafhankelijk is van een onderliggend protocol en is niet per se gekoppeld aan HTTP. Meest voorkomende implementaties van systemen die zijn gebaseerd op de REST gebruiken echter HTTP als het toepassingsprotocol voor verzenden en ontvangen van aanvragen. In dit document bevat informatie over het toewijzen van REST beginselen tot ontworpen voor het werken met behulp van HTTP-systemen.

Het model REST gebruikt een navigatie schema om aan te geven van objecten en -services via een netwerk (waarnaar wordt verwezen als _resources_). Veel systemen die meestal REST implementeren gebruikmaken van het HTTP-protocol voor het verzenden van verzoeken om toegang tot deze resources. In deze systemen dient een clienttoepassing een aanvraag in de vorm van een URI waarmee een resource en een HTTP-methode (het meest voorkomende wordt u, bericht, opslag of verwijderen) die wordt aangegeven welke bewerking moet worden uitgevoerd voor die resource.  De hoofdtekst van de HTTP-aanvraag bevat de gegevens die zijn vereist om de bewerking uitvoeren. Het belangrijkste voor meer informatie over is dat REST een model stateless verzoek definieert. HTTP-aanvragen moeten onafhankelijke en kunnen optreden in willekeurige volgorde, zodat u probeert te bewaren tijdelijke statusgegevens tussen aanvragen niet mogelijk gecombineerd is.  De enige plaats waarin gegevens zijn opgeslagen in de resources zelf is en elke aanvraag moet een atomaire bewerking. Een REST-model implementeert effectief, een eindige toestandsmachine waar een verzoek om een resource uit een duidelijke tijdelijke staat op een ander overgangen.

> [AZURE.NOTE] De staatloze aard van afzonderlijke aanvragen in het model REST kunt een systeem opgesteld aan de hand van deze beginselen om te worden ten zeerste scalable. Er is niet nodig moeten worden bewaard eventuele affiniteit tussen een clienttoepassing maken van een reeks aanvragen en het verwerken van aanvragen die afkomstig zijn specifieke-endwebservers.

Een ander essentieel punt bij de uitvoering van een effectieve REST-model is voor meer informatie over de relaties tussen de verschillende bronnen waaraan het model toegang biedt. Deze resources worden meestal ingedeeld als verzamelingen en relaties. Stel dat een snelle analyse van een e-commerce-systeem ziet u dat er twee verzamelingen in welke client toepassingen zijn waarschijnlijk geïnteresseerd bent: orders en klanten. Elke volgorde en de klant moet hebben een eigen unieke sleutel ter identificatie. De URI voor toegang tot de verzameling orders mogelijk iets net zo eenvoudig als _/orders_en ook de URI voor het ophalen van alle klanten _/customers_kan zijn. Een HTTP GET-aanvraag uitgifte aan de _/orders_ URI moet een lijst die alle orders weergeven in de verzameling gecodeerd als een HTTP-antwoord worden geretourneerd:

```HTTP
GET http://adventure-works.com/orders HTTP/1.1
...
```

Het antwoord hieronder worden de orders gecodeerd als een structuur van de lijst JSON:

```HTTP
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
[{"orderId":1,"orderValue":99.90,"productId":1,"quantity":1},{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2},{"orderId":3,"orderValue":16.60,"productId":2,"quantity":4},{"orderId":4,"orderValue":25.90,"productId":3,"quantity":1},{"orderId":5,"orderValue":99.90,"productId":1,"quantity":1}]
```
Als u wilt ophalen van een afzonderlijke order vereist de id voor de volgorde van de resource _orders_ , zoals _/orders/2_opgeven:

```HTTP
GET http://adventure-works.com/orders/2 HTTP/1.1
...
```

```HTTP
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2}
```

> [AZURE.NOTE] Deze voorbeelden wordt de informatie voor eenvoudig getoond in antwoorden worden geretourneerd als JSON tekstgegevens. Er is echter geen reden waarom resources geen andere soorten gegevens die worden ondersteund door HTTP, zoals binaire of versleutelde informatie; moeten bevatten het inhoudstype in de HTTP-reactie moet het type opgeven. Ook een REST-model mogelijk om terug te keren van dezelfde gegevens in verschillende notaties, zoals XML- of JSON. In dit geval moet de webservice kunnen inhoud onderhandelingen uitvoeren met de client die het verzoek. Het verzoek kan een koptekst _accepteren_ waarmee de gewenste notatie die de client wilt ontvangen en de webservice moet proberen om deze indeling indien mogelijk bevatten.

U ziet dat de reactie van een verzoek voor een REST maakt gebruik van de standaard-HTTP-statuscodes. Bijvoorbeeld een aanvraag die geldige gegevens retourneert die de HTTP-antwoordcode 200 (OK), moet bevatten, terwijl een antwoord waarin de HTTP-statuscode 404 (niet gevonden) moet worden geretourneerd door een aanvraag die niet vinden of verwijderen van een opgegeven resource.

## <a name="design-and-structure-of-a-restful-web-api"></a>Ontwerp en structuur van een RESTful web API

De toetsen tot het ontwerpen van een succesvolle web API zijn eenvoudig en consistentie. Een Web-API die deze twee factoren vertoont vergemakkelijkt het bouwen van clienttoepassingen die moeten de API gebruiken.

Een RESTful web API is gericht op een reeks verbonden resources weergeeft, en trainingen de core-bewerkingen die zodat deze werken met deze resources en eenvoudig te navigeren tussen de notitieblokken. Daarom moet de URI's die een typisch RESTful web API vormen worden gericht op de gegevens die worden getoond en gebruik van de faciliteiten die HTTP om te werken met deze gegevens. Hiervoor is een andere denkt uit die meestal gebruikt bij het ontwerpen van een set klassen in een object-georiënteerd API doorgaans meer door het gedrag van objecten en klassen worden gemotiveerd. Een RESTful web API moet bovendien geen status en niet afhankelijk zijn van bewerkingen in een bepaalde volgorde wordt aangeroepen. De volgende secties samenvatting maken van de punten die u rekening houden moet bij het ontwerpen van een RESTful web API.

### <a name="organizing-the-web-api-around-resources"></a>Het web API rond resources ordenen

> [AZURE.TIP] De URI's die worden aangeboden door een REST-webservice moet worden gebaseerd op zelfstandige naamwoorden (de gegevens waarnaar de web API toegang biedt) en niet werkwoorden (wat een toepassing kunt doen met de gegevens).

De business-entiteiten die het web API stelt belichten. In een web API ter ondersteuning van het e-commerce-systeem eerder beschreven, zijn de primaire entiteiten bijvoorbeeld klanten en orders. Processen, zoals de act van een bestelling kunnen worden bereikt doordat een HTTP POST-bewerking die ervoor zorgt dat de gegevens toegevoegd aan de lijst met orders voor de klant. Deze bewerking bericht kunt intern taken zoals voorraad niveaus controleren en facturering de klant uitvoeren. Het HTTP-antwoord kunt aangeven of de volgorde wel of niet is geplaatst. Bedenk ook dat een resource geen wilt baseren op een item met één fysieke gegevens. Als u bijvoorbeeld mogelijk een resource volgorde intern worden uitgevoerd met behulp van informatie samengevoegd van veel rijen verdeeld over verschillende tabellen in een relationele database, maar de client als een eenheid gepresenteerd.

> [AZURE.TIP] Vermijd het ontwerpen van een REST-interface die komt overeen met of is afhankelijk van de interne structuur van de gegevens die worden getoond. REST gaat over het meer dan eenvoudige CRUD (maken, ophalen, Update, Delete) bewerkingen implementatie via afzonderlijke tabellen in een relationele database. Het doel van de REST is zakelijke entiteiten en de bewerkingen die een toepassing kunt uitvoeren op deze entiteiten de fysieke uitvoering van deze entiteiten, maar een client niet moet worden weergegeven om aan te wijzen deze fysiek details.

Afzonderlijke zakelijke entiteiten bestaan zelden in moeten worden geïsoleerd (Hoewel sommige objecten singleton bestaat), maar in plaats daarvan worden gegroepeerd in siteverzamelingen. Overige zijn termen, elke entiteit en elke verzameling resources. Elke siteverzameling heeft een eigen URI binnen de webservice in een RESTful web API, en een lijst met items in die verzameling uitvoering van een HTTP GET-aanvraag via een URI voor een siteverzameling zijn opgehaald. Elk afzonderlijk item, heeft ook een eigen URI en een toepassing nog een HTTP GET-aanvraag die URI gebruiken voor het ophalen van de details van dat item kunt verzenden. U moet de URI's voor siteverzamelingen en items hiërarchisch indelen. In het e-commerce-systeem, de URI _/customers_ geeft de siteverzameling van de klant en _/customers/5_ haalt de details voor de één klant met de 5-ID van deze siteverzameling. Deze methode helpt voorkomen dat het web API intuïtieve.

> [AZURE.TIP] Bij het gebruik van een consistente naamgeving in URI's; in het algemeen is het handig meervoud zelfstandige naamwoorden gebruiken voor URI's die verzamelingen verwijzing.

Ook moet u rekening houden de relaties tussen de verschillende soorten resources en hoe u deze koppelingen mogelijk weer te geven. Klanten kunnen bijvoorbeeld nul of meer orders plaatsen. Een natuurlijke manier om aan te geven van deze relatie zou tot en met een URI zoals _/customers/5/orders_ om te zoeken van alle orders voor klanten 5. U kunt ook resultaat van de koppeling uit een order terug naar een bepaalde klant via een URI zoals _/orders/99/customer_ om de klant voor volgorde 99, maar dit model te ver uitbreiden kan worden omgezet in omslachtig willen implementeren. Er is een beter bieden bevinden zich koppelingen naar die is gekoppeld resources, zoals de klant, in de hoofdtekst van het HTTP-antwoordbericht geretourneerd wanneer de volgorde wordt gevraagd. Deze methode wordt beschreven in de sectie met de HATEOAS Approach to navigatie naar verwante Resources inschakelen verderop in deze instructies uitgebreider.

In meer complexe systemen kunnen er veel meer soorten entiteit, en het kan zijn verleidelijk om te leveren URI's waarmee een clienttoepassing om te navigeren door verschillende niveaus van relaties, zoals _/customers/1/orders/99/products_ voor de lijst met producten in volgorde 99 door klant 1 geplaatst. Echter kan dit niveau van de complexiteit moeilijk te onderhouden en is inflexibele als de relaties tussen resources in de toekomst wijzigen. Liever, moet u contact wilt behouden URI's relatief eenvoudig. Houd er rekening mee dat wanneer een toepassing een verwijzing naar een resource heeft, moet het mogelijk met deze verwijzing zoeken naar items met betrekking tot deze resource. De voorgaande query kan worden vervangen door de URI _/customers/1/orders_ om alle orders voor klanten 1 en klikt u vervolgens de URI _/orders/99/products_ query om te zoeken van de producten in deze volgorde (ervan uitgaande order 99 is geplaatst door klant 1).

> [AZURE.TIP] Voorkom het vereisen van resource URI's complexer dan _siteverzameling/item/siteverzameling_.

Een ander punt u rekening moet houden is dat alle webaanvragen een belasting van de webserver leggen, en des te groter het aantal de groter het selectievakje laden aanvragen. Probeer uw resources om te voorkomen dat "chatty" web API's die laten zien van een groot aantal bronnen voor kleine definiëren. Een dergelijke API kan vragen om een clienttoepassing meerdere verzoeken om alle gegevens die er moeten te verzenden. Het is mogelijk nuttig om gegevens denormalize en verwante informatie samenvoegen tot groter resources die kan worden opgehaald door één verzenden combineren. U moet echter saldo vanaf deze methode ten opzichte van de belasting van het ophalen van gegevens die niet vaak door de client vereist mogelijk. Grote objecten ophalen, kan de latentie van een verzoek om te vergroten en kosten extra bandbreedte voor little gebruiken als de aanvullende gegevens wordt niet vaak gebruikt.

Voorkom Inleiding tot afhankelijkheden tussen het web API de structuur, type of locatie van de onderliggende gegevensbronnen. Bijvoorbeeld: als uw gegevens in een relationele database bevindt zich, de web API hoeft niet elke tabel weergegeven als een verzameling resources. Beschouw van het web API als een abstractie van de database en indien nodig een laag toewijzing tussen de database en het web API introduceren. Op deze manier als het ontwerp of de uitvoering van de database wordt gewijzigd (bijvoorbeeld u verplaatsen uit een relationele database met een verzameling genormaliseerde tabellen aan een systeem gedenormaliseerde NoSQL opslag, zoals een document-database) clienttoepassingen worden geïsoleerd uit deze wijzigingen.
> [AZURE.TIP] De bron van de gegevens die dient ter ondersteuning van een web-API hoeft niet te worden van een gegevensopslag; Dit kan een andere service of LOB-toepassing of zelfs een oudere toepassing met on-premises implementatie binnen een organisatie.

Ten slotte is het niet mogelijk elke bewerking geïmplementeerd door een web API voor een bepaalde resource toewijzen. U kunt deze _niet-resource_ scenario's via HTTP GET-aanvragen die een deel van de functies roepen en de resultaten worden geretourneerd als een HTTP-antwoordbericht verwerken. Een web API waarmee eenvoudige rekenmachine-achtige bewerkingen, zoals optellen en aftrekken kan URI's die deze bewerkingen als pseudo resources weergeven en gebruiken van de queryreeks om op te geven van de vereiste parameters bieden. Bijvoorbeeld een GET-aanvraag met de URI _/add?operand1 = 99 & operand2 = 1_ kan een antwoord geretourneerd met de instantie met de waarde 100 en het plaatsen van de aanvraag voor de URI _/subtract?operand1 = 50 & operand2 = 20_ kan een antwoordbericht retourneren met de instantie met de waarde 30. Alleen echter deze vormen van URI's Wees spaarzaam met gebruiken.

### <a name="defining-operations-in-terms-of-http-methods"></a>Bewerkingen in HTTP-methoden definiëren

Het HTTP-protocol Hiermee definieert u een aantal manieren die semantische betekenis aan een nieuw vergaderverzoek toewijzen. De algemene HTTP-methoden die worden gebruikt door de meest RESTful web API's zijn:

- **Ophalen**, met een kopie van de resource aan de opgegeven URI ophalen. De hoofdtekst van het antwoordbericht bevat de details van de gevraagde resource.

- **Bericht**, naar een nieuwe resource bij de opgegeven URI maken. De hoofdtekst van het bericht bevat de informatie van de nieuwe resource. Houd er rekening mee bericht kan ook worden gebruikt om te activeren bewerkingen die geen resources daadwerkelijk maakt.

- **Plaatsen**, vervangen of de resource aan de opgegeven URI bijwerken. De hoofdtekst van het verzoekbericht Hiermee geeft u de resource kan worden gewijzigd en de waarden moeten worden toegepast.

- **Verwijderen**, de resource aan de opgegeven URI te verwijderen.

> [AZURE.NOTE] Het HTTP-protocol definieert ook andere methoden minder gebruikt, zoals PATCH die wordt gebruikt om te vragen selectief updates voor een resource, HEAD die wordt gebruikt voor het aanvragen van een beschrijving van een resource, opties waarmee de clientgegevens van een voor informatie over de opties voor het communicatiebeleid ondersteund door de server, en AANWIJZEN die kan een client informatie opvragen die kan worden gebruikt voor test-en diagnostische gegevens.

Het effect van een specifiek verzoek dient afhankelijk of de resource is waarop deze is toegepast een siteverzameling of een afzonderlijk item is. De volgende tabel bevat een overzicht van de algemene conventies volgens meest RESTful implementaties volgens het voorbeeld e-commerce. Houd er rekening mee dat mogelijk niet voor alle volgende aanvragen worden uitgevoerd; Dit is afhankelijk van het specifieke scenario.

| **Resource** | **Verzenden** | **Toevoegen** | **PLAATSEN** | **VERWIJDEREN** |
|--------------|----------|---------|---------|------------|
| /Customers | Een nieuwe klant | Alle klanten ophalen | Bulksgewijs bijwerken van klanten (_als geïmplementeerd_) | Alle klanten verwijderen |
| / klanten/1 | Fout | De details voor klant 1 ophalen | Werk de details van klant 1 indien aanwezig, anders return een fout | Klant 1 verwijderen |
| /Customers/1/orders | Maak een nieuwe order voor klant 1 | Alle orders voor klanten 1 ophalen | Bulksgewijs bijwerken van orders voor klanten 1 (_als geïmplementeerd_) | Alle orders voor klanten 1 (_als geïmplementeerd_) verwijderen |

Het doel van aanvragen ophalen en verwijderen zijn relatief ingewikkelde, maar er is bereik voor verwarring betreffende het doel en de gevolgen van het bericht en plaatsen aanvragen.

Een POST-aanvraag moet een nieuwe resource maakt met gegevens die beschikbaar zijn in de hoofdtekst van het verzoek. In het model REST toepassen u vaak POST-aanvragen op resources die verzamelingen zijn; de nieuwe resource wordt toegevoegd aan de siteverzameling.

> [AZURE.NOTE] U kunt ook definiëren de POST-aanvragen dat sommige functies activeren (en die niet per se resultaatgegevens), en deze soorten verzoek kunnen worden toegepast op siteverzamelingen. U kunt bijvoorbeeld een POST-aanvraag aan een rooster doorgeven aan een service van de verwerking salarisadministratie en de berekende btw als reactie terug te gaan.

Een aanvraag opslag is bedoeld voor het wijzigen van een bestaande bron. Als de opgegeven resource niet bestaat, het verzoek opslag een fout kan retourneren (in sommige gevallen het mogelijk dat is wel maken de resource). Plaats aanvragen worden meest op resources die zijn van afzonderlijke items (zoals een specifieke klant of order), maar ze kunnen worden toegepast op verzamelingen, hoewel dit minder gebruikte wordt geïmplementeerd toegepast. Opmerking waarin opslag zijn idempotency is ingeschakeld POST-aanvragen zijn Als een toepassing hetzelfde indient moeten plaatsen verzoek meerdere keren de resultaten altijd hetzelfde (dezelfde resource wordt gewijzigd met dezelfde waarde), maar een toepassing Hiermee herhaalt u de dezelfde POST-aanvraag het resultaat is als het maken van meerdere resources.

> [AZURE.NOTE] Strikt genomen, vervangen een aanvraag HTTP plaatsen door een bestaande bron de resource die is opgegeven in de hoofdtekst van het verzoek. Als de bedoeling is om te wijzigen van een selectie van eigenschappen in een resource, maar andere eigenschappen die ongewijzigd laten, moet klikt u vervolgens dit worden geïmplementeerd met behulp van een aanvraag HTTP PATCH. Echter veel RESTful implementaties Ontspanning deze regel en het gebruik van opslag in beide gevallen.

### <a name="processing-http-requests"></a>Verwerking van HTTP-aanvragen
De gegevens die zijn opgenomen door een clienttoepassing in veel HTTP aanvragen en de bijbehorende berichten van de reactie van de webserver, kan worden weergegeven in diverse indelingen (of mediatypen). De gegevens die Hiermee geeft u de details van een klant of order kan bijvoorbeeld worden verstrekt als XML-, JSON of een andere gecodeerd en gecomprimeerde indeling. Een RESTful web API moet ondersteuning voor verschillende mediatypen verzoek door de clienttoepassing waarmee een nieuw vergaderverzoek.

Wanneer een clienttoepassing een aanvraag die haalt gegevens op in de hoofdtekst van een bericht verzendt, kan deze de mediatypen die kan verwerken in de koptekst accepteren van de aanvraag opgeeft. De volgende code ziet u een HTTP GET-aanvraag die de details van klant 1 zijn opgehaald en daarom van het resultaat moet worden geretourneerd als JSON (de client moet nog steeds het mediatype van de gegevens in het antwoord om te controleren of de opmaak van de geretourneerde gegevens onderzoeken):

```HTTP
GET http://adventure-works.com/orders/2 HTTP/1.1
...
Accept: application/json
...
```

Als de webserver dit mediatype ondersteunt, kan het dan beantwoorden via een antwoord dat inhoudstype kop waarmee de indeling van de gegevens in de hoofdtekst van het bericht is opgenomen:

> [AZURE.NOTE] De media typen waarnaar wordt verwezen in de koppen accepteren en inhoudstype moeten worden herkend MIME-typen voor maximale interoperabiliteit, in plaats van enkele aangepaste mediatype.

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"orderID":2,"productID":4,"quantity":2,"orderValue":10.00}
```

Als de aangevraagde medium-type wordt niet ondersteund door de webserver, kan de gegevens in een andere indeling verzenden. IN alle gevallen moet deze het mediatype (zoals _toepassing/json_) in de kop van het inhoudstype opgeven. Dit is de verantwoordelijkheid van de clienttoepassing te parseren van het antwoordbericht en de resultaten in de berichttekst correct interpreteren.

Houd er rekening mee dat in dit voorbeeld de webserver is de gevraagde gegevens zijn opgehaald en geeft aan success door door te geven een statuscode van 200 terug in de kop van de reactie. Als er geen overeenkomende gegevens wordt gevonden, wordt de statuscode 404 (niet gevonden) in plaats daarvan moeten worden geretourneerd en de hoofdtekst van het antwoordbericht aanvullende informatie kan bevatten. De opmaak van deze informatie wordt bepaald door de kop van het inhoudstype, zoals wordt weergegeven in het volgende voorbeeld:

```HTTP
GET http://adventure-works.com/orders/222 HTTP/1.1
...
Accept: application/json
...
```

Volgorde 222 bestaat niet, zodat het antwoordbericht ziet er zo uit:

```HTTP
HTTP/1.1 404 Not Found
...
Content-Type: application/json; charset=utf-8
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"message":"No such order"}
```

Wanneer een toepassing een aanvraag HTTP plaatsen voor het bijwerken van een resource stuurt, wordt er URI van de resource aangeeft en vindt u de gegevens die moeten worden gewijzigd in de hoofdtekst van het verzoekbericht. Dit moet ook de opmaak van deze gegevens opgeven met behulp van de kop van het inhoudstype. Een algemene indeling die wordt gebruikt voor gegevens op tekst gebaseerde uit is _toepassing/x-1-800-www-Dell / form-urlencoded_, die bestaat uit een reeks naam/waardeparen gescheiden door de & teken. Het volgende voorbeeld ziet u een verzoek voor HTTP plaatsen die de informatie in de volgorde 1 wijzigt:

```HTTP
PUT http://adventure-works.com/orders/1 HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
ProductID=3&Quantity=5&OrderValue=250
```

Als de wijziging geslaagd is, moet het ideale geval reageren met een statuscode HTTP 204, die aangeeft dat het proces met succes is verwerkt, maar dat de hoofdtekst van het antwoord geen aanvullende informatie bevat. De kop van de locatie in het antwoord bevat de URI van de bijgewerkte resource:

```HTTP
HTTP/1.1 204 No Content
...
Location: http://adventure-works.com/orders/1
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
```

> [AZURE.TIP] Als de gegevens in een bericht van de aanvraag HTTP plaatsen bevat de datum en tijd, zorgen dat uw webservice accepteert van datums en tijden opgemaakt de standaard ISO 8601 volgen.

Als u de resource kan worden bijgewerkt niet bestaat, wordt de webserver kan reageren met een reactie niet gevonden zoals eerder is beschreven. U kunt ook als de server het object zelf maakt kan deze retourneren de status codes HTTP 200 (OK) of HTTP 201 (gemaakt) en de hoofdtekst van het antwoord kunnen de gegevens voor de nieuwe resource bevatten. Als de koptekst van het inhoudstype van de aanvraag Hiermee geeft u een indeling die kan worden verwerkt door de webserver, moet deze reageren met HTTP-statuscode 415 (niet-ondersteunde Media Type).

> [AZURE.TIP] Houd rekening met bulkbewerkingen op HTTP-opslag die batch-updates voor meerdere resources in een siteverzameling kunnen implementeren. Het verzoek opslag moet Geef de URI van de siteverzameling en het hoofdgedeelte van de aanvraag moet Geef de details van de resources worden gewijzigd. Deze methode kunt verminderen chattiness en de prestaties verbeteren.

De opmaak van een HTTP POST-aanvragen die maken van nieuwe resources zijn vergelijkbaar met die van opslag aanvragen; de hoofdtekst van het bericht bevat de details van de nieuwe resource moet worden opgeteld. Echter de URI Hiermee geeft u meestal de verzameling waaraan de resource moet worden toegevoegd. Het volgende voorbeeld wordt gemaakt van een andere volgorde en toevoegt aan de collectie orders:

```HTTP
POST http://adventure-works.com/orders HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
productID=5&quantity=15&orderValue=400
```

Als de aanvraag geslaagd is, wordt de webserver moet reageren met een Berichtcode met HTTP-statuscode 201 (gemaakt). De koptekst locatie de URI van de zojuist gemaakte bron moet bevatten en de hoofdtekst van het antwoord moet bevatten een kopie van de nieuwe resource; de kop van het inhoudstype bevat het formaat van deze gegevens:

```HTTP
HTTP/1.1 201 Created
...
Content-Type: application/json; charset=utf-8
Location: http://adventure-works.com/orders/99
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"orderID":99,"productID":5,"quantity":15,"orderValue":400}
```

> [AZURE.TIP] Als de gegevens van een verzoek om te plaatsen of POST ongeldig is, wordt de webserver moet reageren met een bericht met HTTP-statuscode 400 (ongeldige aanvraag). De hoofdtekst van dit bericht kan bevatten aanvullende informatie over het probleem met de aanvraag en de indelingen verwacht of een koppeling naar een URL vindt u meer details kan bevatten.

Als u wilt verwijderen van een resource, biedt een aanvraag HTTP verwijderen gewoon de URI van de resource moet worden verwijderd. Het volgende voorbeeld wordt geprobeerd volgorde 99 verwijderen:

```HTTP
DELETE http://adventure-works.com/orders/99 HTTP/1.1
...
```

Als de verwijderbewerking geslaagd is, moet de webserver reageren met HTTP-statuscode 204, die aangeeft dat het proces met succes is verwerkt, maar dat de hoofdtekst van het antwoord bevat geen aanvullende informatie (dit is hetzelfde antwoord geretourneerd door een succesvolle bewerking voor de opslag, maar zonder de kop van een locatie als de resource niet langer bestaat.) Het is ook mogelijk dat een aanvraag verwijderen om te retourneren van HTTP-statuscode 200 (OK) of 202 (geaccepteerde) als de verwijdering asynchroon wordt uitgevoerd.

```HTTP
HTTP/1.1 204 No Content
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
```

Als de resource niet wordt gevonden, moet de webserver in plaats daarvan een 404 (niet gevonden) bericht retourneren.

> [AZURE.TIP] Als alle resources in een verzameling worden verwijderd moeten, kunt u een verzoek HTTP verwijderen om te worden opgegeven voor de URI van de siteverzameling in plaats van een toepassing elke resource beurtelings verwijderen uit de verzameling afdwingen.

### <a name="filtering-and-paginating-data"></a>Filteren en pagineren van gegevens

U moet ernaar te behouden de URI's eenvoudige en intuïtieve. Een verzameling bronnen via één weergeeft URI hierbij helpt, maar kan leiden tot toepassingen grote hoeveelheden gegevens ophalen als er slechts een subset van de gegevens worden ondernomen. Een grote hoeveelheid verkeer genereren van invloed is niet alleen de prestaties en schaalbaarheid van de webserver, maar ook nadelig beïnvloeden de reactiesnelheid van clienttoepassingen om de gegevens vraagt.

Bijvoorbeeld als orders de prijs is voldaan bevatten, een clienttoepassing die vereist zijn voor het ophalen van alle orders met kosten over een bepaalde waarde mogelijk eerst alle orders halen uit de _/orders_ URI en vervolgens deze orders lokaal filteren. Duidelijk wordt dit proces ten zeerste niet efficiënt; Dit verspild netwerk bandbreedte en verwerking power op de server met het web API.

Een oplossing mogelijk op te geven van een kleurenschema URI zoals _/orders/ordervalue_greater_than_n_ , waarbij _n_ staat voor de prijs volgorde, maar voor alle maar een beperkt aantal prijzen deze manier is niet praktisch. Ook als u de query orders op basis van andere criteria moet, kunt u uiteindelijk wordt geconfronteerd met een lange lijst met URI's met mogelijk niet-intuïtieve namen voorzien.

Een betere strategie voor het filteren van gegevens is om te creëren van de filtercriteria van de queryreeks dat is doorgegeven op het web API, zoals _/orders?ordervaluethreshold = n_. In dit voorbeeld de bijbehorende bewerking in het web API is verantwoordelijk voor parseren en afhandeling de `ordervaluethreshold` parameter in de queryreeks en de gefilterde resultaten retourneren in de HTTP-reactie.

Enkele eenvoudige HTTP GET-aanvragen voor bronnen van de siteverzameling kunnen mogelijk een groot aantal items retourneren. Als u wilt de mogelijkheid van deze optreedt u combat, moeten het web API beperken de hoeveelheid gegevens die worden geretourneerd door een enkele aanvraag ontwerpen. Dit kunt u bereiken door de ondersteuning van querytekenreeksen met de waarmee de gebruiker kan opgeven van het maximum aantal items moeten worden opgehaald (dit kan zelf onderhevig aan een limiet bovengrens om te voorkomen dat weigering van Service-aanvallen), en een begindatum verschuiving in de siteverzameling. Bijvoorbeeld: de query-tekenreeks in de URI _/orders?limit = 25 & verschuiving = 50_ 25 orders beginnen met de 50e volgorde gevonden in de verzameling orders moeten worden opgehaald. Als met het filteren van gegevens, de bewerking waarmee de GET-aanvraag in het web API verantwoordelijk voor parseren en afhandeling is de `limit` en `offset` parameters in de query. Om te helpen clienttoepassingen, krijgen aanvragen waarmee geretourneerd gepagineerde gegevens behoren ook enkele vorm van metagegevens die aangeven van het totale aantal bronnen die beschikbaar zijn in de siteverzameling. U kunt ook andere intelligente paginering strategieën; Zie voor meer informatie [API ontwerpnotities: Smart pagineren](http://bizcoder.com/api-design-notes-smart-paging)

U kunt een soortgelijke strategie voor het sorteren van gegevens, zoals deze wordt opgehaald; volgen Zo kunt u een parameter sorteren waarmee de naam van een veld als de waarde, zoals opgeven _/orders?sort = product-id_. Bedenk wel dat deze methode een schadelijke gevolgen hebben kan voor caching (query tekenreeksparameters deel uitmaken van de resource-id die door veel cache implementaties als de sleutel in de cache opgeslagen gegevens).

U kunt deze methode op limiet (project) uitbreiden de velden geretourneerd als een item één resource een grote hoeveelheid gegevens bevat. Bijvoorbeeld, kunt u een queryreeksparameter waarin een door komma's gescheiden lijst met velden, zoals _/orders?fields productnummer, hoeveelheid =_.

> [AZURE.TIP] Geef alle optionele parameters in querytekenreeksen zinvolle standaardwaarden. Bijvoorbeeld instellen de `limit` -parameter voor 10 en de `offset` parameter 0 als u paginering implementeert, stelt u de parameter sorteren op de toets van de resource als u implementeert ordening en instellen de `fields` -parameter voor alle velden in de bron als u prognoses ondersteunt.

### <a name="handling-large-binary-resources"></a>Verwerking van grote binaire resources

Één resource kan grote binaire velden, zoals bestanden of afbeeldingen bevatten. Naar het opheffen van de overdracht problemen die worden veroorzaakt door onbetrouwbare en periodieke verbindingen en om te verbeteren antwoord tijden, overwegen bewerkingen die deze resources moeten worden opgehaald in stukken door de clienttoepassing inschakelen. Hiervoor moet het web API ondersteuning van de koptekst accepteren bereiken voor GET-aanvragen voor grote bronnen en ideale geval implementeren HTTP hoofd aanvragen voor deze bronnen. De koptekst accepteren bereiken wordt aangegeven dat de GET-bewerking gedeeltelijke resultaten ondersteunt en dat een clienttoepassing GET-aanvragen waarmee een subset van een resource die is opgegeven als een aantal bytes dat wordt geretourneerd kunt verzenden. Een hoofd-aanvraag is vergelijkbaar met een GET-aanvraag, behalve dat deze alleen geeft als resultaat een koptekst waarmee de resource en een lege berichttekst wordt beschreven. Een clienttoepassing kan een hoofd-verzoek om te bepalen of een resource ophalen met behulp van gedeeltelijke GET-aanvragen verlenen. Het volgende voorbeeld ziet een hoofd-aanvraag die wordt opgehaald van informatie over de productafbeelding van een:

```HTTP
HEAD http://adventure-works.com/products/10?fields=productImage HTTP/1.1
...
```

Het antwoordbericht bevat een koptekst met de grootte van de resource (4580 bytes) en de accepteren bereiken koptekst of de bijbehorende GET-bewerking ondersteunt gedeeltelijke resultaten:

```HTTP
HTTP/1.1 200 OK
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 4580
...
```

De clienttoepassing kan deze gegevens gebruiken om een reeks GET-bewerkingen om op te halen van de afbeelding in kleinere stukken te maken. De eerste aanvraag ophaalt de eerste 2500 bytes met behulp van de kop van het bereik:

```HTTP
GET http://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=0-2499
...
```

Het antwoordbericht geeft aan dat dit een gedeeltelijk antwoord door te retourneren van HTTP-statuscode 206. De koptekst inhoud lengte Hiermee geeft u het werkelijke aantal bytes dat als resultaat gegeven in de berichttekst (niet de grootte van de resource) en in de koptekst inhoud-bereik wordt aangegeven welk deel van de resource dit is (0-2499 bytes afwezigheidsberichten 4580):

```HTTP
HTTP/1.1 206 Partial Content
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2500
Content-Range: bytes 0-2499/4580
...
_{binary data not shown}_
```

Een volgende aanvraag van de clienttoepassing kunt u de rest van de resource ophalen met behulp van een juiste bereik kop:

```HTTP
GET http://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=2500-
...
```

Het bijbehorende bericht voor resultaat ziet er als volgt:

```HTTP
HTTP/1.1 206 Partial Content
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2080
Content-Range: bytes 2500-4580/4580
...
```

## <a name="using-the-hateoas-approach-to-enable-navigation-to-related-resources"></a>De HATEOAS-methode gebruiken om te navigeren naar verwante resources

Een van de primaire motivaties achter REST is dat moet het mogelijk om te navigeren van de gehele set resources zonder voorafgaande kennis van het URI-schema. Elke HTTP GET-aanvraag moet worden geretourneerd de informatie die nodig is om te vinden van de resources die betrekking hebben rechtstreeks op het gewenste object door hyperlinks in het antwoord opgenomen en feedback ook moet worden geleverd met gegevens die worden beschreven van de beschikbare bewerkingen op elk van deze resources. Dit principe heet HATEOAS of Hypertext als de status Engine van toepassing. Het systeem is in feite een eindige toestandsmachine, en het antwoord op elk verzoek om een bevat informatie die nodig bij het verplaatsen van de ene staat naar een andere; Er worden geen andere gegevens moet nodig zijn.

> [AZURE.NOTE] Er zijn momenteel geen normen of specificaties die hoe bepalen u het principe HATEOAS model. De voorbeelden in deze sectie illustreren een mogelijke oplossing.

Als u bijvoorbeeld voor het verwerken van de relatie tussen klanten en orders, mogen de gegevens die zijn geretourneerd door het antwoord voor een specifieke volgorde bevatten URI's in de vorm van een hyperlink identificatie van de klant die de volgorde geplaatst en de bewerkingen die kunnen worden uitgevoerd op die klant.

```HTTP
GET http://adventure-works.com/orders/3 HTTP/1.1
Accept: application/json
...
```

De hoofdtekst van het antwoordbericht bevat een `links` matrix (gemarkeerd in het voorbeeld) die de aard van de relatie (_klanten_), de URI van de klant (_http://adventure-works.com/customers/3_), het ophalen van de details van deze klant (_ophalen_) en de MIME-typen die ondersteuning biedt voor het ophalen van deze informatie (_tekst/XML-_ en _toepassing/json_) voor de webserver. Dit is de gegevens waarmee een clienttoepassing kunnen ophalen van de details van de klant. Daarnaast bevat de matrix koppelingen ook koppelingen voor de andere bewerkingen die kunnen worden uitgevoerd, zoals plaatsen (aan het wijzigen van de klant, samen met de indeling die de webserver verwacht de client op te geven) en verwijderen.

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[(some links omitted){"rel":"customer","href":" http://adventure-works.com/customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":"
customer","href":" http://adventure-works.com /customers/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"customer","href":" http://adventure-works.com /customers/3","action":"DELETE","types":[]}]}
```

De matrix koppelingen moet voor volledigheid ook naar zichzelf verwijst informatie die betrekking hebben op de resource die zijn opgehaald. Deze koppelingen uit het vorige voorbeeld zijn weggelaten, maar zijn gemarkeerd in de volgende code. U ziet dat in deze koppelingen, de relatie _self_ is gebruikt om aan te geven dat dit een verwijzing naar de resource wordt geretourneerd door de bewerking is:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[{"rel":"self","href":" http://adventure-works.com/orders/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" self","href":" http://adventure-works.com /orders/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"self","href":" http://adventure-works.com /orders/3", "action":"DELETE","types":[]},{"rel":"customer",
"href":" http://adventure-works.com /customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" customer" (customer links omitted)}]}
```

Voor deze methode werkt, moeten de clienttoepassingen worden bereid kunt ophalen en parseren van de volgende aanvullende informatie.

## <a name="versioning-a-restful-web-api"></a>Versiebeheer een RESTful web-API

Zeer waarschijnlijk niet die in alle maar de eenvoudigste situaties dat een web API statische blijft. Als business vereisten nieuwe collecties van veranderen resources kunnen worden toegevoegd, de relaties tussen resources kunnen worden gewijzigd en de structuur van de gegevens in resources kan worden gewijzigd. Tijdens het bijwerken van een web API u omgaat met nieuwe of verschillende vereisten relatief eenvoudig is, moet u rekening houden met de effecten die deze wijzigingen heeft op het web API clienttoepassingen. Het probleem is dat de ontwikkelaar ontwerpen en implementeren van een web API heeft volledige controle over die API, de ontwikkelaar hoeft niet dezelfde mate van controle over clienttoepassingen die kan worden gemaakt door derden organisaties extern besturingsomgeving. De primaire imperatieve is het inschakelen van bestaande clienttoepassingen blijft werken ongewijzigd terwijl u nieuwe client toepassingen om te profiteren van nieuwe functies en resources.

Met versiebeheer kunt een web API om aan te geven van de functies en de resources die worden getoond en een clienttoepassing aanvragen die worden doorgestuurd naar een specifieke versie van een functie of resources kan worden verzonden. De volgende secties worden meerdere benaderingen, die elk een eigen voordelen en de voor-en nadelen heeft.

### <a name="no-versioning"></a>Geen versies bijhouden

Dit is de eenvoudigste benadering en is aanvaardbaar voor sommige interne API's. Grote wijzigingen kunnen worden weergegeven als nieuwe resources of nieuwe koppelingen.  Inhoud toevoegen aan bestaande resources worden mogelijk een recente wijziging niet weergegeven als clienttoepassingen die niet zijn verwacht te zien dat gewoon door dit inhoudstype worden genegeerd.

Bijvoorbeeld de details van één klant met moet worden geretourneerd door een verzoek om de URI _http://adventure-works.com/customers/3_ `id`, `name`, en `address` velden verwacht door de clienttoepassing:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

> [AZURE.NOTE] Voor de toepassing van eenvoudig en de helderheid bevatten het voorbeeld antwoorden weergegeven in deze sectie geen HATEOAS koppelingen.

Als de `DateCreated` veld wordt toegevoegd aan het schema van de resource klant, en vervolgens het antwoord er als volgt:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":"1 Microsoft Way Redmond WA 98053"}
```

Bestaande clienttoepassingen mogelijk verder goed als worden genegeerd niet-herkende velden kunnen, terwijl de nieuwe clienttoepassingen kunnen worden uitgevoerd dit nieuwe veld. Echter als meer radicale wijzigingen in het schema van resources optreden (zoals verwijderen of de naam van velden wijzigen) of wijzigen van de relaties tussen resources inhouden vervolgens deze recente wijzigingen die verhinderen dat bestaande clienttoepassingen goed werkt. In deze situaties moet u overwegen een van de volgende manieren.

### <a name="uri-versioning"></a>URI versiebeheer

Telkens wanneer u het web API wijzigen of het schema van resources, wijzigt toevoegen u een versienummer aan de URI voor elke resource. De bestaande URI's moet blijven functioneren als voorheen, resources die voldoen aan de oorspronkelijke schema retourneren.

Het vorige voorbeeld, uitbreiden als de `address` veld is herzien in subvelden met elk onderdeel deel van het adres (zoals `streetAddress`, `city`, `state`, en `zipCode`), deze versie van de resource kan worden weergegeven via een URI met een versienummer, zoals http://adventure-works.com/v2/customers/3:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

Deze methode versiebeheer is heel eenvoudig, maar is afhankelijk van de server de aanvraag zoekresultaten omleiden naar het juiste eindpunt. Echter kan deze lastig zijn als het web dat API ouder tot en met verschillende iteraties en de server moet een aantal verschillende versies ondersteunen. Ook vanuit het oogpunt van een purist in alle gevallen de clienttoepassingen worden ophalen van dezelfde gegevens (klant 3), zodat de URI niet echt verschillen afhankelijk van de versie moeten. Implementatie van HATEOAS ingewikkelder dit schema ook omdat alle koppelingen moeten opnemen het versienummer in hun URI's.

### <a name="query-string-versioning"></a>Query tekenreeks versiebeheer

In plaats van meerdere URI's bieden, kunt u de versie van de resource door een parameter binnen de queryreeks toegevoegd aan de HTTP-aanvraag, zoals _http://adventure-works.com/customers/3?version=2_te gebruiken. De parameter versie moet standaard naar een zinvolle waarde zoals 1 als dit wordt weggelaten, wordt door oudere clienttoepassingen.

Deze methode heeft het semantische voordeel dat dezelfde resource wordt altijd opgehaald uit de dezelfde URI, maar dit hangt af van de code die het verzoek om de querytekenreeks parseert en weer de desbetreffende HTTP-antwoord verzenden verwerkt. Deze methode heeft ook de dezelfde problemen voor de uitvoering van HATEOAS als de URI versiebeheer om.

> [AZURE.NOTE] Sommige oudere webbrowsers en web proxy's worden niet reacties op vergaderverzoeken die een queryreeks in de URL opnemen in cache. Dit kan een negatieve invloed hebben op prestaties voor webtoepassingen die gebruikmaken van een web API en die uitvoeren vanuit een webbrowser.

### <a name="header-versioning"></a>Koptekst versiebeheer

In plaats van het versienummer weergegeven als een tekenreeks queryparameter toevoegt, kunt u een aangepaste kop die de versie van de resource aangeeft implementeren. Deze methode is vereist dat de clienttoepassing wordt toegevoegd voor de gewenste koptekst aan verzoeken, hoewel de afhandeling van het clientverzoek code een standaardwaarde (version 1) gebruiken kan als de koptekst versie wordt weggelaten. De volgende voorbeelden gebruikmaken van een aangepaste kop _Aangepaste koptekst_met de naam. De waarde van deze koptekst geeft de versie van web API.

Versie 1:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Custom-Header: api-version=1
...
```

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

Versie 2:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Custom-Header: api-version=2
...
```

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

Houd er rekening mee dat net als met de vorige twee methoden, HATEOAS implementeren vereist inclusief de juiste aangepaste kop in eventuele koppelingen.

### <a name="media-type-versioning"></a>Mediatype versiebeheer

Wanneer een clienttoepassing een HTTP GET-aanvraag tot een webserver stuurt moet deze de opmaak van de inhoud die deze kan omgaan met behulp van een koptekst accepteren, zoals eerder in deze instructies beschreven bepalen. Het doel van de koptekst _accepteren_ is vaak toe te staan dat de clienttoepassing kunt u opgeven of de hoofdtekst van het antwoord moeten XML, JSON of enkele andere veelgebruikte indelingen die kan worden geparseerd door de client. Het is echter mogelijk definiëren van aangepaste mediatypen die gedetailleerde omschrijving van de clienttoepassing om aan te geven welke versie van een resource deze verwacht bevatten. De volgende voorbeeldformule wordt een aanvraag die Hiermee geeft u een kop _accepteren_ met de waarde _application/vnd.adventure-works.v1+json_. Het element _vnd.adventure-works.v1_ wordt naar de webserver wordt aangegeven dat versie 1 van de resource, moeten worden geretourneerd terwijl de _json_ -element aangeeft dat de opmaak van de hoofdtekst van het antwoord JSON moet worden:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Accept: application/vnd.adventure-works.v1+json
...
```

De code die de aanvraag voor de verwerking is verantwoordelijk voor het verwerken van de koptekst _accepteren_ en deze behouden blijft zoveel mogelijk (de clienttoepassing mogelijk verschillende indelingen in de koptekst _accepteren_ waarin hoofdletters/kleine letters de webserver de meest geschikte indeling voor de hoofdtekst van het antwoord kunt kiezen opgeven). De webserver bevestigt de opmaak van de gegevens in de hoofdtekst van het antwoord met behulp van de kop van het inhoudstype:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/vnd.adventure-works.v1+json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

Als de koptekst accepteren niet alle bekende mediatypen is opgegeven, kan de webserver genereren een antwoordbericht HTTP 406 (niet geaccepteerd) of een bericht met een standaardtype media terug.

Deze methode is Hoewel de purest van de regelingen versiebeheer en gepaard natuurlijk met HATEOAS, waaronder het type MIME gerelateerde gegevens in resource koppelingen kunt.

> [AZURE.NOTE] Wanneer u een strategie versiebeheer selecteert, u moet ook rekening houden met de gevolgen voor de prestaties, met name in cache opslaan op de webserver. De URI versiebeheer en queryreeks versiebeheer schema zijn cache-vriendelijke zover dezelfde URI/query tekenreeks combinatie naar dezelfde gegevens telkens wanneer verwijst.

> De kop versiebeheer en het mediatype versiebeheer regelingen moeten meestal extra logica om te bekijken van de waarden in de aangepaste kop- of de koptekst accepteren. In een grootschalige omgeving, kunnen veel klanten met verschillende versies van een web-API resulteren in een aanzienlijke hoeveelheid dubbele gegevens in een cache servers. Dit probleem kan worden omgezet in acute als een clienttoepassing met een webserver via een proxy die wordt geïmplementeerd in cache opslaan communiceert, en die alleen een verzoek om de webserver doorgestuurd als het niet momenteel een kopie van de gevraagde gegevens in de cache dat geval.

## <a name="more-information"></a>Meer informatie

- Het [RESTful kookboek](http://restcookbook.com/) bevat een inleiding tot het bouwen van RESTful API's.
- De Web- [API controlelijst](https://mathieu.fenniak.net/the-api-checklist/) bevat een handig lijst met items u rekening moet houden bij het ontwerpen en implementeren van een Web-API.
