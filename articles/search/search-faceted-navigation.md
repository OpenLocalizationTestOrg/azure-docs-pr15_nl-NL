<properties 
    pageTitle="Het implementeren van beperkte navigatie in Azure zoeken | Microsoft Azure | navigatie zoekcategorieën" 
    description="Beperkte navigatie naar toepassingen die zijn geïntegreerd met Azure zoeken, de zoekservice van een wolk die worden gehost op Microsoft Azure toe te voegen." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

#<a name="how-to-implement-faceted-navigation-in-azure-search"></a>Het implementeren van beperkte navigatie in Azure zoeken

Beperkte navigatie is filters om waarmee gericht analyse navigatie in zoektoepassingen. Terwijl de term 'beperkte navigatie' Onbekende zijn mogelijk, het bijna is een gezien het feit dat u hebt een voordat u gebruikt. Als u het volgende voorbeeld ziet, is beperkte navigatie niets meer dan de categorieën die worden gebruikt om resultaten te filteren.

## <a name="what-it-looks-like"></a>Hoe dit eruitziet

 ![][1]
  
Aspecten kunt u vinden wat u zoekt, en ervoor zorgen dat die u nul resultaten won't downloaden. Als ontwikkelaar kunnen aspecten u laten zien van de meest handige zoekcriteria voor het navigeren in uw corpus zoeken. Beperkte navigatie is vaak in onlinedetailhandel-toepassingen ingebouwd via merken, afdelingen (kinderen segmenten), grootte, prijs, populariteit en classificaties. 

Beperkte navigatie niet in alle zoeken technologieën en zeer complex kunnen zijn. In Azure zoeken, beperkte navigatie gebaseerd op query moment toegekend velden die eerder zijn opgegeven in uw schema gebruiken. In de query's die uw toepassing maakt, moet een query *facet queryparameters* ontvangen van de beschikbare facet filterwaarden voor dat resultaat documentenset verzenden. Het resultaat van het document dat is wel knippen instellen, moet past u de toepassing een `$filter` expressie.

Toepassing ontwikkeling, met de code die wordt gemaakt van query's schrijft vormt het grootste deel van het werk. Veel van de toepassing problemen die u van beperkte navigatie wilt wordt aangeboden door de service, inclusief ingebouwde ondersteuning voor instellen van bereiken en telt ophalen voor facet resultaten. De service bevat ook praktische standaardwaarden die u helpen voorkomen lastig navigatie-structuren. 

In dit artikel bevat de volgende secties:

- [Het maken van deze](#howtobuildit)
- [De laag presentatie maken](#presentationlayer)
- [De index maken](#buildindex)
- [Controleren op de kwaliteit van de gegevens](#checkdata)
- [De query maken](#buildquery)
- [Tips over het beheren van beperkte navigatie](#tips)
- [Beperkte navigatie op basis van waarden](#rangefacets)
- [Beperkte navigatie op basis van GeoPoints](#geofacets)
- [Probeer het zelf](#tryitout)

##<a name="why-use-it"></a>Waarom deze gebruiken
De meest effectieve zoektoepassingen, hebben meerdere interactie modellen naast een zoekvak. Beperkte navigatie is een alternatief ingangspunt om te zoeken, biedt een alternatief voor het zoeken van complexe expressies met de hand te typen.

##<a name="know-the-fundamentals"></a>De basisprincipes weten

Als u nog niet eerder zoeken ontwikkeling, is de beste manier om na te denken van beperkte navigatie dat de mogelijkheden voor zoekprogramma's gericht worden weergegeven. Dit is een type inzoomen zoekervaring, op basis van vooraf gedefinieerde filters, die wordt gebruikt voor het snel verkleinen omlaag zoekresultaten tot en met punt-en-klik acties. 

**Interactie Model**

De zoekfunctie voor beperkte navigatie is iteratieve, zodat we beginnen door deze als een reeks query's die in antwoord op gebruikersacties uitgevouwen begrijpen.

Het beginpunt is een toepassing-pagina die beperkte navigatie biedt, meestal op de omtrek geplaatst. Beperkte navigatie is vaak boomstructuur, met selectievakjes voor elke waarde of tekst met waarop kan worden geklikt. 

1.  Een query die is verzonden naar Azure zoeken Hiermee geeft u de structuur van de beperkte navigatie via een of meer facet queryparameters. Bijvoorbeeld de query mogelijk ook `facet=Rating`, bijvoorbeeld met een `:values` of `:sort` optie om de presentatie verder te verfijnen.
2.  De laag presentatie worden weergegeven door een zoekpagina waarmee beperkte navigatie, met de aspecten in de aanvraag vermeld.
3.  Een beperkte navigatie-structuur met classificatie gegeven, de gebruiker "4" om aan te geven dat alleen producten met een waardering van 4 of hoger moeten worden weergegeven. 
4.  Antwoord wordt stuurt de toepassing een query`$filter=Rating ge 4` 
5.  De presentatie laag de pagina, met een verminderde resultatenset, met alleen de items die voldoen aan criteria van het nieuwe bijgewerkt (in dit geval producten geclassificeerd 4 en hoger).

Een facet is een queryparameter, maar niet verwarren met invoer van de query. Het is nooit gebruikt als selectiecriteria in een query. In plaats daarvan beschouwen facet queryparameters als invoer voor de navigatiestructuur die in het antwoord terugkomt. Voor elke facet queryparameter die u opgeeft, wordt Azure zoeken beoordelen hoeveel documenten zijn in de gedeeltelijke resultaten voor elke facetwaarde.

Kennisgeving de `$filter` in stap 4. Dit is een belangrijk aspect van beperkte navigatie. Hoewel aspecten en filters in de API onafhankelijke, moet u beide voor het leveren van de ervaring die u van plan bent. 

**Ontwerppatronen**

In de toepassingscode is het patroon facet queryparameters gebruiken om terug te keren de structuur van de beperkte navigatie samen met facet resultaten, plus een $filter-expressie die de gebeurtenis click verwerkt. Zie de `$filter` expressie als de code achter de werkelijke inkorten van zoekresultaten geretourneerd naar de presentatie laag. Gegeven een facet kleuren, te klikken op de kleur rood wordt geïmplementeerd via een `$filter` -expressie die Hiermee selecteert u alleen de items die u een kleur rood hebt. 

**Basisbewerkingen met de query in Azure zoeken**

In Azure zoeken, is via een of meer queryparameters (Zie [Documenten zoeken](http://msdn.microsoft.com/library/azure/dn798927.aspx) voor een beschrijving van elke) een aanvraag opgegeven. Geen van de queryparameters zijn vereist, maar u moet ten minste één om een query geldig.

Precisie van formules, in het algemeen begrepen als de mogelijkheid om uit niet-relevante treffers, te filteren wordt bereikt door een of beide van deze expressies:

- **zoeken =**<br/>
De waarde van deze parameter vormt de zoekexpressie. Het is mogelijk een enkel stuk tekst of een complexe zoekexpressie met meerdere voorwaarden en operatoren. Op de server, wordt een zoekexpressie gebruikt voor volledige tekst zoeken, query's uitvoeren doorzoekbare velden in de index voor overeenkomende termen, levert resultaten in rang volgorde. Als u `search` op null, query uitvoering is via de volledige index (dat wil zeggen `search=*`). In dit geval andere elementen van de query, zoals een `$filter` of profiel scoren, wordt de primaire factoren die van welke documenten worden geretourneerd `($filter`) en in welke volgorde (`scoringProfile` of `$orderb`y).

- **$filter =**<br/>
Een filter is een krachtige methode voor het beperken van de grootte van de lijst met zoekresultaten op basis van de waarden van een specifiek documentkenmerken. A `$filter` is eerst geëvalueerd, gevolgd door faceting logica die wordt gegenereerd van de beschikbare waarden en de bijbehorende aantallen voor elke waarde

Complexe zoeken expressies wordt de prestaties van de query verkleinen. Indien mogelijk, maken gebruik van goed gebouwd filterexpressies als u wilt vergroten precisie van formules en prestaties van query's te verbeteren.

Meer informatie over hoe een filter worden toegevoegd exacter, vergelijken een complexe zoekexpressie naar een met een filterexpressie:

- `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`

- `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Hoewel beide query's geldig zijn, is de tweede bovenliggende als u niet-motels met parkeren in Seattle zoekt. De eerste query, is afhankelijk van deze specifieke woorden wordt vermeld of niet wordt vermeld in de tekenreeksvelden, zoals naam, beschrijving en andere velden die doorzoekbare gegevens bevat. De tweede query Hiermee wordt gezocht naar exacte overeenkomsten op gestructureerde gegevens en waarschijnlijk nauwkeuriger.

In de toepassingen die beperkte navigatie bevatten, wordt u wilt om ervoor te zorgen dat elke gebruikersactie over de structuur van een beperkte navigatie is vergezeld van een verkleinen van de lijst met zoekresultaten tot en met een filterexpressie bereikt.

<a name="howtobuildit"></a>
##<a name="how-to-build-it"></a>Het maken van deze

Beperkte navigatie in Azure zoeken is geïmplementeerd in uw toepassingscode die de aanvraag genereert, maar is afhankelijk van de vooraf gedefinieerde elementen in uw schema.

Vooraf gedefinieerde in uw zoekopdracht index is de `Facetable [true|false]` indexkenmerk, is ingesteld op geselecteerde velden-of uitschakelen van hun gebruiken in een beperkte navigatie-structuur. Zonder `"Facetable" = true`, een veld kan niet worden gebruikt in facet-navigatie.

Query keer, uw toepassingscode maakt u een nieuw vergaderverzoek met `facet=[string]`, een aanvraag parameter waarmee u het veld aan facet door. Een query kan meerdere aspecten, zoals hebben `&facet=color&facet=category&facet=rating`, elkaar gescheiden door een ampersand-operator (&).

Toepassingscode moet ook opgeven, een `$filter` expressie om de klikgebeurtenissen in beperkte navigatie te verwerken. A `$filter` Hiermee reduceert u de lijst met zoekresultaten met de facetwaarde als filtercriteria.

Azure zoeken geeft als resultaat de lijst met zoekresultaten per de items die zijn ingevoerd door de gebruiker, samen met updates voor de beperkte navigatie-structuur. In Azure zoeken, beperkte navigatie is een bouw één niveau, met facet waarden, en wordt geteld hoeveel resultaten voor elke batch zijn gevonden.

De presentatie laag in code bevat de gebruikerservaring. Het moet delen van de beperkte navigatie, zoals het label, waarden, selectievakjes en het aantal bevatten. De Azure zoeken REST API is platform agnostic, dus welke taal en de platform die u wilt gebruiken. Het belangrijkste is om op te nemen gebruikersinterface-elementen die ondersteuning bieden voor incrementele vernieuwen, met de bijgewerkte gebruikersinterface staat, waarbij elke extra facet wordt geselecteerd. 

In de volgende secties wordt we nader bij het maken van elk onderdeel, beginnen met de presentatie laag.

<a name="presentationlayer"></a>
##<a name="build-the-presentation-layer"></a>De laag presentatie maken

Werken vanaf de laag presentatie terug kunt u de vereisten die anders kunnen worden overgeslagen en begrijpt welke functies essentieel voor de zoekervaring zijn schuiven.

Met beperkte navigatie, uw web- of pagina geeft de structuur beperkte navigatie wordt gedetecteerd invoer van de gebruiker op de pagina en de gewijzigde elementen invoegt. 

Voor webtoepassingen wordt AJAX gebruikt in de presentatie laag omdat Hiermee kunt u incrementele wijzigingen vernieuwen. U kunt ook ASP.NET MVC of een andere visualisatie-platform die verbinding met een Azure-zoekservice via HTTP maken kunt. De voorbeeldtoepassing waarnaar wordt verwezen in dit artikel-- **AdventureWorks catalogus** – gebeurt een aanvraag ASP.NET MVC.

Het volgende voorbeeld wordt overgenomen van het bestand **index.cshtml** van de steekproef-toepassing, genereert een dynamische HTML-structuur voor het weergeven van beperkte navigatie op uw pagina met zoekresultaten. In de steekproef, beperkte navigatie deel uitmaakt van de pagina met zoekresultaten en wordt weergegeven nadat de gebruiker heeft een zoekterm verzonden.

Melding dat elke facet een label (kleuren, categorieën prijzen), een binding met een beperkte veld (kleur, categorienaam, listPrice) en een heeft `.count` parameter, gebruikt om het aantal objecten gevonden voor dat resultaat facet te retourneren.

  ![][2]
 

> [AZURE.TIP] Bij het ontwerpen van de pagina met zoekresultaten, moet u een methode voor het schakelen aspecten toevoegen. Als u de selectievakjes gebruikt, kunnen gebruikers eenvoudig hoe u de filters intuit. Voor andere indelingen moet u wellicht een patroon breadcrumb of een andere creatieve benadering. Bijvoorbeeld, in de toepassing van de steekproef AdventureWorks catalogus kunt u de titel, AdventureWorks catalogus, de zoekpagina opnieuw in te stellen.

<a name="buildindex"></a>
##<a name="build-the-index"></a>De index maken

Faceting is ingeschakeld op basis van velden voor veld in de index via dit indexkenmerk: `"Facetable": true`.  
Alle veldtypen die mogelijk kunnen worden gebruikt in beperkte navigatie zijn `Facetable` al dan niet standaard. Deze veldtypen opnemen `Edm.String`, `Edm.DateTimeOffset`, en alle numerieke veldtypen (in feite alle veldtypen zijn facetable behalve `Edm.GeographyPoint`, die niet worden gebruikt in beperkte navigatie). 

Als u een index maakt, is een goede gewoonte voor beperkte navigatie expliciet faceting om uit te schakelen voor velden die nooit als een facet moeten worden gebruikt.  Tekenreeksvelden voor singleton waarden, zoals de naam van een ID of product, met name moeten worden ingesteld op `"Facetable": false` om te voorkomen dat per ongeluk (en niet effectief) gebruik in een beperkte navigatie.

Dit is het schema voor de catalogus met AdventureWorks steekproef-app (bijgesneden van bepaalde kenmerken verkleinen van de totale grootte):

 ![][3]
 
Houd er rekening mee dat `Facetable` voor tekenreeksvelden die niet mag worden gebruikt als aspecten, zoals een ID of de naam is uitgeschakeld. Faceting uitschakelen wanneer u deze niet nodig schakelen zorgt u ervoor dat de grootte van de index kleine dat en over het algemeen verbeterde prestaties.

> [AZURE.TIP] Als een goede gewoonte, bevatten de volledige reeks indexkenmerken voor elk veld. Hoewel `Facetable` is standaard voor bijna alle velden, opzettelijk instellen elk kenmerk kunt u de gevolgen van elke beslissing schema analyseren. 

<a name="checkdata"></a>
##<a name="check-for-data-quality"></a>Controleren op de kwaliteit van de gegevens 

Wanneer u een gegevensgerichte toepassing ontwikkelt, is voorbereiden van de gegevens vaak een groter deel van de taak. Zoektoepassingen zijn er geen uitzondering. De kwaliteit van uw gegevens heeft Gewest op of de structuur van de beperkte navigatie gebeurtenis zich voordoet zoals u verwacht, evenals de doeltreffendheid in zodat u filters die het resultaat verkleinen samenstellen instellen.

In Azure zoeken, wordt de zoeken-corpus gevormd door documenten die een index te vullen. Documenten bevatten de waarden die worden gebruikt voor het berekenen van aspecten. Als u laten facet door huisstijl of prijs wilt, moet elk document waarden bevatten voor *merknaam* en *ProductPrice* die zijn geldig, consistente en productief als een filteroptie.

Hieronder ziet u enkele herinneringen over wat u te corrigeren voor:

- Voor elk veld dat u wilt facet door, Stel uzelf of deze waarden die geschikt als filters in gericht zoeken zijn bevat. De waarden worden moeten korte, beschrijvende en voldoende opvallend wissen kiezen tussen concurrerende opties bieden.
- Spelfouten of bijna overeenkomende waarden. Als u op kleur en veldwaarden facet oranje en opnemen Ornage (een spelfout), een facet op basis van het veld kleur beide wilt ophalen.
- Gemengde hoofdletters/kleine letters tekst kan ook aanrichten voor functies in beperkte navigatie, met oranje en oranje die als twee verschillende waarden worden weergegeven. 
- Eén en het meervoud versies van dezelfde waarde kunnen resulteren in een afzonderlijk facet voor elk label.

Als u zich voorstellen kunt, is voortvarendheid bij het voorbereiden van de gegevens in een essentiële aspecten van effectieve beperkte navigatie.

<a name="buildquery"></a>
##<a name="build-the-query"></a>De query maken

De code die u voor het samenstellen van query's schrijft moet alle onderdelen van een geldige query, inclusief zoeken expressies, aspecten, filters, scoren profielen – gebruikt voor het opstellen van een nieuw vergaderverzoek opgeven. In dit gedeelte bekijken we waar aspecten zodat deze past in de query en hoe filters worden gebruikt met aspecten voor het leveren van een verminderde resultatenset.

Een voorbeeld is het meestal een goede locatie om te beginnen. Het volgende voorbeeld wordt overgenomen van het bestand **CatalogSearch.cs** , genereert een aanvraag die wordt gemaakt op basis van kleur, categorie en prijs facet-navigatie. 

U ziet dat aspecten integraal in dit voorbeeldtoepassing. De zoekervaring in AdventureWorks catalogus is ontworpen rond beperkte navigatie en filters. Dit is duidelijk in de laten opvallen plaatsing van beperkte navigatie op de pagina. De voorbeeldtoepassing bevat URI parameters voor aspecten (kleur, categorie, prijzen) als de eigenschappen van de methode zoeken (zoals deze is gebouwd in de steekproef-toepassing).

  ![][4]
 
Een queryparameter facet is ingesteld op een veld en afhankelijk van het gegevenstype, kan worden verder parameters door komma's gescheiden lijst met `count:<integer>`, `sort:<>`, `intervals:<integer>`, en `values:<list>`. Een lijst met waarden wordt ondersteund voor numerieke gegevens bij het instellen van bereiken. Zie [Zoeken-documenten (Azure zoeken API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) voor meer informatie voor gebruik.

Samen met aspecten, moet het verzoek geformuleerd door de toepassing ook filters om de set candidate documenten op basis van een selectie van de waarde facet vast te maken. Voor een archief fietsen, biedt beperkte navigatie aanwijzingen om te vragen zoals "wat kleuren, fabrikanten en soorten fietsen beschikbaar zijn', terwijl het filteren van antwoorden vragen zoals"welke exacte fietsen worden rood, mountainbikes, in dit prijs bereik".

Wanneer een gebruiker op "Rood" om aan te geven dat alleen rode producten moeten worden weergegeven, de volgende query de toepassing wordt verzonden wilt opnemen `$filter=Color eq ‘Red’`.

## <a name="best-practices-for-faceted-navigation"></a>Aanbevolen procedures voor beperkte navigatie

De volgende lijst bevat een overzicht van enkele aanbevolen procedures.

- **Precisie van formules**<br/>
Filters gebruiken. Als u aanmelding alleen zoeken expressies alleen afleiding kan leiden tot een document, dat er geen de exacte facetwaarde van de velden worden geretourneerd. 

- **Doelvelden**<br/>
Beperkte minder details wilt u meestal alleen documenten waarin de facetwaarde in een bepaald (beperkt) veld, niet ergens op alle doorzoekbare gebieden opnemen. Een filter toevoegen versterkt het doelveld door te laten lopen de service die u wilt zoeken alleen in het beperkte veld voor een overeenkomstige waarde.

- **Index efficiency**<br/>
Als uw toepassing gebruikmaakt van beperkte navigatie uitsluitend (dat wil zeggen, geen zoekvak), kunt u het veld als markeren `searchable=false`, `facetable=true` tot een compactere index. Daarnaast indexeren doet zich alleen op hele facet waarden, waarbij geen word-einde of indexeren van de onderdelen van een waarde meerdere woorden.

- **Prestaties**<br/>
Filters de set candidate documenten voor zoekprogramma's verfijnen en classificatie uitsluiten. Als er een groot aantal documenten, krijgt met behulp van een zeer voorzichtig facet meer details omlaag vaak u aanzienlijk betere prestaties.


<a name="tips"></a> 
##<a name="tips-on-how-to-control-faceted-navigation"></a>Tips over het beheren van beperkte navigatie

Hieronder vindt u een tip-blad met richtlijnen voor specifieke problemen.

**Labels voor elk veld toevoegen in facet navigatie**

Labels worden gewoonlijk gedefinieerd in de HTML- of formulierbibliotheek (**index.cshtml** in de steekproef-toepassing). Er is geen API in Azure zoeken facet navigatielabels of een ander soort metagegevens.

**Bepalen welke velden kunnen worden gebruikt als facet**

Let erop dat het schema van de index bepaalt welke velden beschikbaar als een facet moet worden gebruikt. Als er dat een veld is facetable en de query geeft aan welke velden moeten worden facet door. Het veld waarop u faceting zijn bevat de waarden die worden weergegeven onder het label. 

De waarden die worden weergegeven onder elk etiket worden opgehaald uit de index. Bijvoorbeeld, als het veld facet *kleur is*, zijn de waarden die beschikbaar zijn voor het filteren van extra de waarden voor dat veld (rood, zwart, enzovoort).

Voor numerieke en DateTime alleen waarden, kunt u waarden expliciet instellen op het veld facet (bijvoorbeeld `facet=Rating,values:1|2|3|4|5`). Een lijst met waarden is toegestaan voor de volgende veldtypen te vereenvoudigen de scheiding facet resultaten in aaneengesloten bereiken (beide bereiken op basis van numerieke waarden of perioden). 

**Facet resultaten knippen**

Facet resultaten zijn gevonden in de lijst met zoekresultaten die overeenkomen met een term facet documenten. In het volgende voorbeeld hebt in de lijst met zoekresultaten voor *cloud computing*, 254 items ook *interne specificatie* als een inhoudstype. Items zijn niet per se elkaar wederzijds uitsluiten. Als een item voldoet aan de criteria van beide filters, is het in elk geteld. Dit is mogelijk wanneer faceting op `Collection(Edm.String)` velden, die vaak worden gebruikt voor het implementeren van document labelen.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

In het algemeen, als u dat facet resultaten zijn persistent te groot is, is het aanbevolen dat vindt toevoegen u meer filters, zoals wordt uitgelegd in eerdere secties, klikt u op meer opties voor het verkleinen van de zoekopdracht voor de toepassingsgebruikers van uw geven.

**Items in de navigatie facet beperken**

Er is een standaardlimiet van 10 waarden voor elk beperkte veld in de boomstructuur. Deze standaard relevant voor navigatie-structuren omdat het zorgt ervoor dat de lijst met waarden naar een hanteerbare grootte overblijft. U kunt de standaard negeren door het toewijzen van een waarde om te tellen.

- `&facet=city,count:5`Hiermee wordt opgegeven dat alleen de eerste 5 steden zijn gevonden in de bovenkant gerangschikte resultaten worden geretourneerd als het resultaat van een facet. Een zoekterm van "airport" en 32 overeenkomsten, opgegeven als de query geeft `&facet=city,count:5`, alleen de eerste vijf unieke steden met de meest-documenten in de lijst met zoekresultaten worden opgenomen in de resultaten facet.

Kennisgeving het verschil tussen facet resultaten en zoekresultaten. Zoekresultaten worden alle documenten die overeenkomen met de query. Facet resultaten zijn de resultaten voor elke facetwaarde. Zoekresultaten vindt plaatsnamen die niet in de facet classificatie lijst (5 in ons voorbeeld) in het voorbeeld. Resultaten die worden uitgefilterd tot en met beperkte navigatie zichtbaar voor de gebruiker wanneer hij of zij aspecten opheffen of andere aspecten naast stad kiest. 

> [AZURE.NOTE] Bespreekt `count` wanneer er meer dan één type kan verwarrend. De volgende tabel biedt een kort overzicht van hoe de term in Azure zoeken API, voorbeeld van een code en documentatie wordt gebruikt. 

- `@colorFacet.count`<br/>
In de presentatiecode ziet u een parameter tellen op het facet, wordt gebruikt voor weergave van het aantal facet resultaten. In facet resultaten geeft aantal het aantal documenten die overeenkomen met op de facet term of het bereik.

- `&facet=City,count:12`<br/>
U kunt in een query facet tellen instellen op een waarde.  De standaardinstelling is 10, maar kunt u deze hogere of lagere instellen. Instelling `count:12` haalt de bovenkant 12 komt overeen met in facet resultaten door het aantal documenten.

- "`@odata.count`"<br/>
Deze waarde geeft in antwoord op de query, het aantal overeenkomende items in de lijst met zoekresultaten. Gemiddeld het groter is dan de som van alle facet resultaten gecombineerd, vanwege de aanwezigheid van items die overeenkomen met de zoekterm, maar wel geen overeenkomsten facet-waarde.


**Niveaus in beperkte navigatie** 

Zoals aangegeven, is er geen directe ondersteuning voor het nesten van aspecten in een hiërarchie. Afmelden bij het vak biedt één niveau van filters alleen ondersteuning voor beperkte navigatie. Echter bestaan tijdelijke oplossingen. U kunt een hiërarchische facet-structuur in coderen een `Collection(Edm.String)` wijst u met één item per hiërarchie. Deze oplossing implementeren valt buiten het bereik van dit artikel, maar u kunt lezen over collecties in [OData door voorbeeld](http://msdn.microsoft.com/library/ff478141.aspx). 

**Valideer de velden**

Als u de lijst met aspecten dynamisch op basis van de invoer van de niet-vertrouwde gebruiker samenstelt, moet u ofwel valideren dat de namen van de beperkte velden geldig zijn, of druk op ESC om de namen bij het maken van URL's die gebruikmaakt van een `Uri.EscapeDataString()` in .NET of het overeenkomstige bedrag in uw platform van keuze.

**Hiermee wordt geteld in facet resultaten**

Wanneer u een filter toevoegt aan een beperkte query, wilt u mogelijk de instructie facet behouden (bijvoorbeeld `facet=Rating&$filter=Rating ge 4`). Technisch gesproken facet = beoordeling is niet nodig, maar deze te houden geeft als resultaat de aantallen facet waarden voor classificaties 4 en hoger. Bijvoorbeeld als een gebruiker '4' en de query een filter voor groter of gelijk is aan '4 bevat', worden telt geretourneerd voor elke beoordeling die is 4 en omhoog.  

**Sharding consequenties op facet-tellingen komen**

Onder bepaalde omstandigheden kan het gebeuren dat facet-tellingen komen niet overeenkomen met de resultatensets (Zie [beperkte navigatie in Azure zoeken (forum post)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Facet-tellingen komen kunnen zijn onjuist vanwege de architectuur sharding. Elke zoekindex heeft meerdere shards en elkaar de aspecten van de bovenste N-rapporten door het aantal documenten, die vervolgens worden gecombineerd tot één resultaat. Als sommige shards een groot aantal overeenkomende waarden, terwijl andere een kleiner, kan het gebeuren dat sommige facet-waarden ontbreken of onder-geteld in de zoekresultaten.

Hoewel dit probleem op elk moment wijzigen kan als u dit gedrag vandaag, kunt u omzeilen deze door het aantal kunstmatig verversen:<number> naar een zeer grote getal om af te dwingen volledige rapportage vanuit elke shard. Als de waarde van count: groter is dan of gelijk is aan het aantal unieke waarden in het veld, bent u nauwkeurige resultaten gegarandeerd. Wanneer echter document telt echt hoog, dat er is een op de prestaties zijn, dus gebruikt deze optie overgangseffecten.

<a name="rangefacets"></a>
##<a name="facet-navigation-based-on-a-range-values"></a>Facet-navigatie op basis van de bereikwaarden van een

Faceting over bereiken is een algemene vereiste van de zoeken-toepassing. Bereiken worden voor numerieke gegevens en DateTime-waarden ondersteund. U vindt meer informatie over elke methode in [Zoeken-documenten (Azure zoeken API)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Azure zoeken eenvoudiger bereik constructie doordat twee methoden voor het berekenen van een bereik. Voor beide methoden maakt Azure zoeken de juiste bereiken gegeven van de invoer die u hebt opgegeven. Bijvoorbeeld als u de waarden van 10 opgeeft | 20 | 30, wordt er automatisch bereiken van-0 10, 10-20, 20-30 onderneming gemaakt. De toepassing van de steekproef Hiermee verwijdert u alle intervallen die leeg zijn. 

**Methode 1: De parameter interval gebruiken**<br/>
Als u wilt instellen prijs aspecten in $10 stappen, zou u het volgende opgeven:`&facet=price,interval:10`


**Methode 2: Een lijst met waarden gebruiken**<br/>
Voor numerieke gegevens, kunt u een lijst met waarden.  Houd rekening met het bereik facet voor listPrice, als volgt weergegeven:

  ![][5]

Het bereik is opgegeven in het **CatalogSearch.cs** -bestand met behulp van een waardenlijst:

    facet=listPrice,values:10|25|100|500|1000|2500

Elk bereik is gebaseerd, waarbij 0 als uitgangspunt, een waarde uit de lijst als een eindpunt, en wordt vervolgens gesneden van het vorige bereik aparte intervallen maken. Azure zoeken wordt als volgt als onderdeel van beperkte navigatie. U hoeft niet te schrijven code voor het structureren van elk interval.

### <a name="build-a-filter-for-facet-ranges"></a>Een filter voor facet bereiken maken ###

Als u wilt filteren op basis van een bereik selecteren door de gebruiker documenten, kunt u de `"ge"` en `"lt"` operatoren in een expressie uit twee delen waarin de eindpunten van het bereik filteren. Bijvoorbeeld als de gebruiker het bereik 10-25, het filter is `$filter=listPrice ge 10 and listPrice lt 25`.

Klik in de voorbeeldtoepassing gebruikt de filterexpressie **priceFrom** en **priceTo** parameters voor het instellen van de eindpunten. De methode **BuildFilter** in **CatalogSearch.cs** bevat de filterexpressie waarmee u de documenten in een bereik.

  ![][6]

<a name="geofacets"></a> 
##<a name="filtered-navigation-based-on-geopoints"></a>Gefilterde navigatie op basis van GeoPoints

Gemeenschappelijke om te zien waarmee u een store, restaurant of bestemming op basis van de nabijheid van uw huidige locatie kiezen worden gefilterd. Dit type filter lijkt op beperkte navigatie, is maar eigenlijk gewoon een filter. We vermelden hier voor mensen die specifiek implementatie wijze Raad voor dat bepaalde ontwerpprobleem zoekt.

Er zijn twee georuimtelijke functies in Azure zoeken, **geo.distance** en **geo.intersects**.

- De functie **geo.distance** retourneert de afstand in kilometer tussen twee punten, één wordt een veld en één een constante wordt doorgegeven als onderdeel van het filter. 

- De functie **geo.intersects** retourneert waar als een bepaald moment binnen een bepaald veelhoek is, waar de komma is een veld en de veelhoek is opgegeven als een constant aantal coördinaten doorgegeven als onderdeel van het filter.

U vindt de filter voorbeelden in de [syntaxis van de OData-expressies (Azure zoeken)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>
##<a name="try-it-out"></a>Probeer het zelf

Azure zoeken Adventure Works Demo op Codeplex bevat de voorbeelden in dit artikel wordt verwezen. Terwijl u met zoekresultaten werkt, bekijk de URL voor wijzigingen in het samenstellen van query. Deze toepassing gebeurt aspecten toevoegen aan de URI bij het selecteren van elkaar.

1.  De steekproef toepassing configureren voor gebruik van uw service URL en api-sleutel. 

    Zoals u ziet het schema die is gedefinieerd in het bestand Program.cs van het project CatalogIndexer. Hiermee geeft u facetable velden voor kleur, listPrice, grootte, gewicht, categorienaam en modelName.  Slechts een paar van deze (kleur, listPrice, categorienaam) zijn daadwerkelijk geïmplementeerd in beperkte navigatie.

3.  Voer de toepassing. 

    Aanvankelijk is alleen het zoekvak zichtbaar. U kunt klikt u op de knop Zoeken direct af om alle resultaten, of typ een zoekterm.

    ![][7]
 
4.  Voer een zoekterm op, zoals fietsen, en klik op **Zoeken**. Snel de query wordt uitgevoerd.
 
    Een beperkte navigatie-structuur ook met de lijst met zoekresultaten geretourneerd.  In de URL zijn de aspecten voor kleuren, categorieën en prijzen null. In de pagina met zoekresultaten bevat de structuur van de beperkte navigatie telt voor elk facet-resultaat.

     ![][8]
 
5.  Klik op een kleur, categorie en prijsbereik. Aspecten zijn null op een eerste zoeken, maar als ze waarden overnemen, de lijst met zoekresultaten worden afgekapt van items die niet meer voldoen aan. U ziet dat de URI neemt af van de wijzigingen aan uw query.

    ![][9]
 
6.  Schakel de beperkte query zodat u kunt verschillende query uitproberen, klikt u op **AdventureWorks catalogus** boven aan de pagina.

    ![][10]
 
<a name="nextstep"></a>
##<a name="next-step"></a>Volgende stap

Als u wilt testen van uw kennis, kunt u een veld facet voor *modelName*toevoegen. De index is al ingesteld voor deze facet, zodat er geen wijzigingen aan de index vereist zijn. Maar moet u de HTML-code als u wilt opnemen van een nieuwe facet voor modellen en voeg het veld facet aan de query-constructor wijzigen.

Voor meer informatie over ontwerp beginselen voor beperkte navigatie, raden we aan de volgende koppelingen:

- [Ontwerpen voor beperkte zoeken](http://www.uie.com/articles/faceted_search/)
- [Ontwerppatronen: Beperkte navigatie](http://alistapart.com/article/design-patterns-faceted-navigation)

U kunt ook [Azure zoeken uitgebreide Kennismaking](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410)bekijken. Bij 45:25 is er een demo over het implementeren van aspecten.

<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/Facet-1-slide.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

 