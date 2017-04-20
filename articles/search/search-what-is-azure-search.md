<properties
    pageTitle="Wat is Azure zoeken | Microsoft Azure | De zoekservice gehoste cloud"
    description="Azure tijdens het zoeken is een gehoste cloud volledig beheerde search-service. Meer informatie in dit Functieoverzicht."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="what-is-azure-search"></a>Wat is Azure zoeken?

Azure tijdens het zoeken is een oplossing voor het zoeken-als-een-service van cloud die gedelegeerd beheer van de server en infrastructuur naar Microsoft, zodat u met een kant-en-klare-service die u kunt vullen met uw gegevens en vervolgens gebruiken om te zoeken toevoegen aan uw web- of mobiele toepassing. Azure zoeken kunt u eenvoudig worden toegevoegd aan een krachtige zoekfunctie naar uw toepassingen met een eenvoudige [REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) of [.NET SDK](search-howto-dotnet-sdk.md) zonder zoeken infrastructuur beheren of dat u een expert in zoeken.

## <a name="give-your-users-a-powerful-search-experience"></a>Geef uw gebruikers een krachtige zoekfunctie

**Krachtige query's** kunnen worden geformuleerd met de [syntaxis van de eenvoudige query](https://msdn.microsoft.com/library/azure/dn798920.aspx), dat logische operatoren, woordgroep search operatoren, achtervoegsel operatoren, operatoren voor de prioriteit van regels bevat. De [syntaxis van de query Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) kunt daarnaast kunnen bij benadering zoeken, nabijheid zoeken, waarbij de term en reguliere expressies inschakelen. Azure zoeken ondersteunt ook aangepaste lexicale analyzers zodat uw toepassing u omgaat met complexe zoekopdrachten gaat uitvoeren met fonetische overeenkomende en reguliere expressies.

**Talen** is [inbegrepen voor 56 andere talen activeren](https://msdn.microsoft.com/library/azure/dn879793.aspx). Zowel Lucene analyzers en Microsoft analyzers (verfijnd op jaren van de natuurlijke taal in Office en Bing processing) gebruikt, kunt Azure zoeken de tekst in het zoekvak van de toepassing u omgaat met slim taalspecifieke taalkundige zoals werkwoordtijd, geslacht, onregelmatig meervoud zelfstandige naamwoorden (bijvoorbeeld ' muis' versus 'muis'), wordt samengesteld uit word, woordafbreking (voor talen zonder spaties), en meer analyseren.

**Zoeksuggesties** kan worden ingeschakeld voor autocompleted zoekbalken en aanvullen tijdens typen query's. [Werkelijk documenten in de index worden voorgesteld](https://msdn.microsoft.com/library/azure/dn798936.aspx) als gebruikers Voer automatische zoekfunctie invoer.

**Klik op markeren** [kunnen](https://msdn.microsoft.com/library/azure/dn798927.aspx) gebruikers om het fragment van tekst in elke resultaat waarin de overeenkomsten voor hun query weer te geven. U kunt kiezen welke velden terug gemarkeerde fragmenten.

**Beperkte navigatie** wordt eenvoudig worden toegevoegd aan de pagina met zoekresultaten met Azure zoeken. [Alleen een enkele queryparameter](https://msdn.microsoft.com/library/azure/dn798927.aspx)gebruikt, retourneert Azure zoekactie alle benodigde informatie als u wilt samenstellen van een beperkte zoekfunctie in de gebruikersinterface van uw app toe te staan dat uw gebruikers kunnen inzoomen en zoekresultaten (bijvoorbeeld catalogusitems filteren op prijs-bereik of de huisstijl) filteren.

**Geografische-ruimte** ondersteuning kunt u Slim proces, filteren en weergeven van geografische locaties. Azure zoeken kunt uw gebruikers kunnen gegevens op basis van de nabijheid van een zoekresultaat naar een opgegeven locatie of op basis van een bepaalde geografische regio verkennen. Deze video wordt uitgelegd hoe dit werkt: [kanaal 9: Azure zoeken en georuimtelijke gegevens](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Filters** kunnen worden gebruikt om eenvoudig beperkte navigatie opnemen in de gebruikersinterface van de toepassing, query formulering verbeteren en filteren op basis van gebruiker of ontwikkelaars-opgegeven criteria. Maken van krachtige filters met de [syntaxis van de OData](https://msdn.microsoft.com/library/azure/dn798921.aspx).

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>Maken van uw ontwikkelaars met een eenvoudig-en-klare-service

**Beschikbaarheid** zorgt ervoor dat een bijzonder betrouwbaar service van de zoekfunctie. Wanneer schaal goed [Azure Search biedt een SLA 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

**Volledig beheerd** als een end-to-end-oplossing, Azure zoeken is absoluut geen management infrastructuur vereist. Uw service kan eenvoudig worden aangepast aan uw behoeften door schaling in twee dimensies u omgaat met meer opslagruimte van het document, hoger query laadtijd of beide.

**Integratie van gegevens** met [indexeerfuncties](https://msdn.microsoft.com/library/azure/dn946891.aspx) kunt Azure zoeken automatisch verkend Azure SQL-Database, Azure DocumentDB of [Azure-blobopslag](search-howto-indexing-azure-blob-storage.md) als u wilt synchroniseren van inhoud van de zoekindex met uw primaire gegevensopslag.

**Document programma** is beschikbaar (die momenteel in preview) [te lezen en indexeren belangrijkste bestandsindelingen](search-howto-indexing-azure-blob-storage.md) met inbegrip van Microsoft Office, evenals de PDF- en HTML-documenten.

**Zoeken verkeer analytics** zijn [eenvoudig verzameld en geanalyseerd](search-traffic-analytics.md) te ontgrendelen inzichten die wat gebruikers in het zoekvak typt.

**Eenvoudige scoren** , is een belangrijk voordeel van Azure zoeken. [Scoren profielen](https://msdn.microsoft.com/library/azure/dn798928.aspx) worden gebruikt om te kunnen organisaties model relevantie te verkrijgen als een functie van waarden in de documenten zelf. Zoals u eventueel de nieuwere producten of korting producten hoger in de lijst met zoekresultaten weergegeven. U kunt ook score profielen-labels gebruiken voor het persoonlijke scoren op basis van de voorkeuren van klant zoeken u hebt bijgehouden en afzonderlijk opgeslagen maken.

**Sorteren** wordt aangeboden voor meerdere velden via het schema index en klik vervolgens op query-moment met een enkele zoekactie parameter is ingeschakeld.

**Paginering** en zoekresultaten weer te geven beperken is [ingewikkelde met het besturingselement goed lopende](search-pagination-page-layout.md) die Azure Search biedt over zoekresultaten weer te geven.  

**Search explorer** kunt u probleem query's op al uw indexen rechts van de Azure portal van uw account zodat u eenvoudig kunt testen van query's en score profielen verfijnen.

## <a name="how-it-works"></a>Werkwijze

### <a name="1-provision-service"></a>1. voorziening-service
U kunt draaien van een Azure-zoekservice met behulp van de [Azure-Portal](https://portal.azure.com/) of de [Azure Resource Management API](https://msdn.microsoft.com/library/azure/dn832684.aspx).

Afhankelijk van hoe u de zoekservice configureert, gebruikt u op de gratis laag van de service die worden gedeeld met andere abonnees Azure zoeken of een [laag betaald](https://azure.microsoft.com/pricing/details/search/) die dedicates bronnen die u wilt alleen worden gebruikt door uw service. Bij de inrichting van uw service, kiezen u ook het gebied van het datacenter waarop uw service.

Afhankelijk van welke laag van de service die u kiest, kunt u uw service schaal in twee dimensies: 1) replica's toevoegen om te laten groeien uw capaciteit afhandelen, dik query laadtijd en 2) partities om toe te voegen opslag voor meer documenten toevoegen. Door het document opslagcapaciteit en de query doorvoer afzonderlijk afhandelen, kunt u uw search-service voor uw specifieke behoeften aanpassen.

### <a name="2-create-index"></a>2. index maken
Voordat u uw inhoud naar uw Azure Search-service uploaden kunt, moet u eerst een Azure zoekindex definiëren. Een index is vergelijkbaar met een databasetabel die uw gegevens bevat en zoekquery's kunt accepteren. U definiëren het schema index toewijzen aan de structuur van de documenten die u zoeken wilt, vergelijkbaar met velden in een database.

Het schema van deze indexen kan worden gemaakt in de Portal Azure, of via een programma [via de .NET SDK](search-howto-dotnet-sdk.md) of [REST API](https://msdn.microsoft.com/library/azure/dn798941.aspx). Zodra de index is gedefinieerd, kunt u uw gegevens kunt vervolgens uploaden naar de zoekservice van Azure waar deze vervolgens wordt geïndexeerd.

### <a name="3-index-data"></a>3. gegevens van de Index
Als u de velden en de kenmerken van de Zijtabs met letter hebt gedefinieerd, bent u gereed is voor het uploaden van uw inhoud in de index. U kunt een push- of halen model gebruiken om gegevens te uploaden naar de index.

Het model halen is beschikbaar via indexeerfuncties die kunnen worden geconfigureerd voor op aanvraag of geplande updates (Zie [indexering bewerkingen (Azure zoeken Service REST API)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), zodat u eenvoudig nemen gegevens en wijzigingen aan de gegevens uit een Azure DocumentDB, Azure SQL-Database, Azure-blobopslag of ingesloten in een VM Azure SQL-Server.

De push-model is beschikbaar via de SDK of REST API's gebruikt voor bijgewerkte documenten verzenden naar een index. U kunt gegevens push vanaf vrijwel elke gegevensset dat de JSON-indeling. Zie [toevoegen, bijwerken, of documenten verwijderen](https://msdn.microsoft.com/library/azure/dn798930.aspx) of [het gebruik van de .NET SDK)](search-howto-dotnet-sdk.md) voor hulp bij het laden van gegevens.

### <a name="4-search"></a>4. zoeken
Zodra u hebt de zoekindex van Azure gevuld, kunt u nu [probleem zoekquery's](https://msdn.microsoft.com/library/azure/dn798927.aspx) aan uw service-eindpunt eenvoudige HTTP aanvragen gebruikt met REST API of de .NET SDK.

## <a name="try-it-now-for-free"></a>Probeer het zelf (gratis!)
U kunt zoeken Azure vandaag nog! Als u al een Azure-account hebt, kunt u [een service in de gratis laag inrichten](search-create-service-portal.md).

Als u geen Azure-account kunt u een gratis, 60 minuten-sessie met geen aanmelden vereist. Ga naar de [Probeert Azure App Service](http://go.microsoft.com/fwlink/p/?LinkId=618214) en selecteer "Web App." Selecteer de sjabloon 'ASP.NET + Azure Search' aan de slag.
