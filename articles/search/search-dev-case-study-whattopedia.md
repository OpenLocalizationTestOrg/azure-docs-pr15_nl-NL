<properties 
    pageTitle="Azure zoeken ontwikkelaars-casestudy: Hoe WhatToPedia een portal infomedia gebaseerd op Microsoft Azure | Microsoft Azure | De zoekservice gehoste cloud" 
    description="Informatie over het maken van een portal en metagegevens-zoekmachine van gegevens, met Azure zoeken, een wolk die worden gehost search-service voor ontwikkelaars." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Casestudy van Azure zoeken ontwikkelaars

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Hoe [WhatToPedia.com](http://whattopedia.com/) een portal infomedia gebaseerd op Microsoft Azure

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">De grote idee</font> 


Onze idee is voor het maken van een informatieportal waarmee als bezoeken verbinding maken met winkels tot en met een hoge relevantie, beperkt zoeken ervaring, vergelijkbaar met hoe meenemen portals overeenkomst toeristen van de hotels, restaurants en ontspanning wanneer u zich in de uncharted rayon. 

De portal die we voorzien wordt een gebruikerservaring bij uitzondering kwalitatief hoogwaardig zoeken over winkel gegevens in een bepaalde markt bevat helpen als bezoeken winkels op basis van de locatie en een andere faciliteiten de winkel. We het zoekprogramma met een initiële gegevensset wordt vullen, maar grondigere waarde wordt opgezet na verloop van tijd met behulp van winkel abonnees met die gegevens over hun bedrijf posten. Promoties, nieuwe producten, populaire merken, interne speciale services-– alle voorbeelden van gegevens die wordt waarde opgeteld bij de site zijn. Deze gegevens zelf gerapporteerd en geïntegreerd in de corpus zoeken zodra de winkel zich registreert als abonnee. Reclame, plus het abonnementsmodel, bieden de stream inkomsten voor onze nieuwe bedrijven.

Zoeken, is het model in de belangrijkste interactie, op een puur cloud-platform. Voor toepassing van schaal en laag-kosten is bijna alles die we, op de portal uit naar besturingselement voor gegevensbronnen uitvoeren, door een online-service. Met een zoekmachine die biedt de functies die we nodig uit het vak te kunnen we snel een search-servicetoepassing maken, een zoekopdracht engine zonder te hoeven maken en beheren onszelf.

## <a name="what-we-built"></a>Wat we ingebouwd

WhatToPedia is een opstart infomedia bedrijf. We [WhatToPedia.com](http://whattopedia.com/) – ingebouwd - die momenteel in test-market in Noord-Europa met een startdatum van 2 februari 2015. Onze klanten base is hoofdzakelijk markering fysieke winkels die een betaalbare onlineaanwezigheid dat gemakkelijk is te beheren en onderhouden nodig.

Onze taak is om te trekken als bezoeken tot en met een uitstekende onlinezoekervaring, waarbij de resultaten op basis op stad of groep, merken, namen opslaan of opslaan typen. Als bezoeken langetermijnmiddelen heeft een rimpeleffect belangrijke winkels zich kunnen abonneren op onze portal-site. Abonnementen zijn kosten op basis van een betaalbare snelheid.

 ![][7] 

Nadat u zich registreert voor een abonnement, heeft de winkel op hun bestaand profiel (gemaakt in eerste instantie door ons van gekochte gegevens), bijgewerkt met het aanvullende gegevens over promoties, aanbevolen merken of aankondigingen. Interne-functies, zoals talen die worden gesproken, valuta's geaccepteerd, zonder btw winkelen, kan zijn zelf gerapporteerde om aan te trekken beter als gebruikers deze faciliteiten zoekt bezoeken.

## <a name="who-we-are"></a>Die we zijn

U kunt de naam van mijn is Thomas Segato (Microsoft Consulting) en ik heb gewerkt met Jesper Boelling, Lead Developer bij WhatToPedia, voor het ontwerpen van de oplossing. 

WhatToPedia is een opstart, test marketing van de nieuwe portal bedrijven in Zweden, waar de meeste van de 60.000 winkels markering fysieke kleine en middelgrote ondernemingen (kleine en middelgrote grote ondernemingen) zijn. Omdat we weten dat een persoon winkelen in Europa speaks van meerdere talen en voert meerdere valuta's, maken we oplossingen die voldoen aan een meertalige klant. We nodig en gevonden, een zoekmachine die ondersteuning biedt voor onze meertalige vereisten in Azure zoeken.

Azure zoeken is een spel-bevindt voor onze project. Voordat u de beschikbaarheid van Azure zoeken beide we aanzienlijke energie werken via het kinks van het samenstellen van onze eigen zoekprogramma. Lukt Azure zoeken, zoals een onlineservice de grootste technische en administratieve drempel verwijderd uit de oplossing, waarin de toegang tot de markt sneller en met een krachtiger zoekfunctie bedoeld.  

## <a name="how-we-did-it"></a>Hoe we deze hebben gedaan

Onze vision is voor het maken van een complete infrastructuur op basis van alleen cloudservices. Microsoft als de strategische platform is gekozen, omdat deze de provider die de benodigde aangeboden services (voor samenwerking en ontwikkeling), schaal op aanvraag en betaalbare prijzen.
 
### <a name="high-level-components"></a>High-level componenten

We een bedrijf, niet alleen een site hebt gemaakt. De hele inspanning ondersteunende vereist een uitgebreide reeks hulpprogramma's en toepassingen. We vastgesteld Visual Studio en Visual Studio Team Services voor ontwikkeling, Team Foundation-Service (TFS) Online voor een besturingselement voor gegevensbronnen en scrum management, Office 365 voor communicatie en samenwerking en natuurlijk Microsoft Azure voor alle site-gerelateerde bewerkingen en opslag. Met Visual Studio, de IDE aangeboden directe inrichting naar Azure met integratie met TFS Online een extra productiviteit voor Microfoonversterking leveren.

Het onderstaande diagram ziet u de op hoog niveau onderdelen die in de infrastructuur WhatToPedia gebruikt.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Gebruik van Microsoft Azure

De groene vakken in het vorige diagram bekijkt, ziet u dat de oplossing WhatToPedia is gebaseerd op de volgende services:

- [Azure zoeken](https://azure.microsoft.com/services/search/)
- [Azure Websites met MVC 4](https://azure.microsoft.com/services/websites/)
- [Azure WebJobs voor geplande taken](../app-service-web/websites-webjobs-resources.md)
- [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/)
- [Azure-blobopslag](https://azure.microsoft.com/services/storage/)
- [Bezorging van E-mail SendGrid](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

De zeer kern van de oplossing is gegevens en zoeken. De stroom van gegevens uit de provider wederverkoper bij de klant wordt geïllustreerd hieronder:

  ![][9]

Primaire gegevensopslag zijn de wederverkoper en accounting-gegevens in Azure SQL-Database. Dit bestaat uit de eerste gegevensset, plus de winkel-specifieke gegevens toegevoegd na verloop van tijd. Een WebJob Azure gebruiken we het posten van updates vanuit SQL-Database naar de corpus zoeken in Azure zoeken.

### <a name="presentation-layer"></a>Presentatie laag

De portal is een Azure-Website, geïmplementeerd in MVC 4 en [Twitter-Bootstrap](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29). We hebben gekozen MVC omdat het een veel duidelijkere aanpak naar HTML dan op formulieren gebaseerde ontwikkeling van ASP.NET biedt. Twitter-Bootstrap is om te voorkomen dat apps voor meerdere apparaten maken en beheren van meerdere mobiele platforms, gekozen om alle apparaten en platforms te ondersteunen.

### <a name="authentication-permissions-and-sensitive-data"></a>Verificatie, machtigingen en vertrouwelijke gegevens

Als bezoeken bladeren anoniem in de site. Als zodanig zijn er geen aanmelding vereisten voor als bezoeken, noch doen we consumentgegevens opslaan. 

Winkels zijn een ander artikel. Hier opgeslagen openbare profielgegevens, factureringsgegevens en mediainhoud die u wilt laten zien op de site. Elke winkel die geabonneerd naar de site krijgen een gebruikersaanmelding, gebruikt om te verifiëren van de gebruiker voordat u van updates naar het profiel van de abonnee.  We zorgen dat alle abonneeservergegevens veilig worden opgeslagen in Azure SQL-Database en Azure BLOB storage.
We gekozen voor een verificatiemodel op basis van .NET op formulieren gebaseerde verificatie. We hebben gekozen deze methode voor het eenvoudig; we nodig de rollen, UI-ondersteuning en andere overbodige functies die worden meegeleverd bij andere methoden niet. 

Om ervoor te zorgen dat winkels alleen de gegevens zien die ze behoort, die u hebt gemaakt winkel-ID voor elke winkel die vervolgens wordt gebruikt op alle lezen en schrijven bewerkingen winkel / regiospecifieke gegevens betrekking hebben op personen. Met deze benadering gevonden dat we hoeft te worden databasemachtigingen verlenen aan afzonderlijke winkels. Alle winkels werken met het systeem onder een databaserol één, klikt u met de winkel-ID als onze gegevens moeten worden geïsoleerd-methode.

Omdat onze bedrijven wordt gevormd door alle over de volgende effecten (waarover meer bedrijven naar winkels, zodat het extra aantrekkelijk te kondigen abonneren maken), we kunnen de lijn tekent bij het afhandelen van aankopen via het web. U kunt een winkelwagen op onze site, waardoor eenvoudiger onze beveiligingsvereisten die zijn als zodanig geen vinden. 

Een andere vereenvoudigen die we werkzaam is voor het uitbesteden van onze facturering en accounts dat uitbetaald moet worden bewerkingen. Door te routeren klant betalingsgegevens rechtstreeks naar een derde partij ([SveaWebPay](http://www.sveawebpay.se/)), wordt de risico's koppelen aan op te slaan en beveiligen van vertrouwelijke gegevens in onze winkels gegevens beperken. 

### <a name="search-engine"></a>Zoekprogramma

De kerninhoud van onze oplossing is gebouwd op Azure zoekservice met het zoekprogramma. In eerste instantie we een aangepaste zoekmachine ingebouwd, maar tijdens dit proces we de complexiteit gerealiseerd hoeveelheid zeer hoog daadwerkelijk is en dat wij overweeg dan alternatieven voor u wordt gevraagd. 

Eenvoudige functies die de belangrijkste met ons opnemen zijn:

- Filters
- Beperkte navigatie
- Resultaten verhogen
- Paginering AJAX

Een zoekopdracht internet ons overgebracht naar de volgende video, die wij probeer Azure zoeken eens geïnspireerd: [Uitgebreide Kennismaking bij TechEd Europe](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Na het bekijken van de video is klaar om u te maken van een prototype op basis van wat hebt gezien. Omdat we al een gegevensmodel in MVC, is het prototype maken ingewikkelde omdat de gegevens doorzoekbare termen bevatten en we de vereisten voor hoe we wilt sorteren, facet, en de gegevens filteren die al heeft gewerkt. 

Dit is hoe we het prototype hebt gemaakt.

**Azure Search-Service configureren**

1. Meld u aan bij klassieke Azure-Portal en de zoekservice aan onze abonnement toegevoegd. We de gedeelde versie (gratis met onze abonnement) gebruikt.
2. Een index maken. Voor het prototype, is gebruikt om de UI-portal te definiëren de zoekvelden en de score profielen maken. Onze score profiel is gebaseerd op locatiegegevens: land | plaats | adres (Zie: score profielen toevoegen).
3. Kopieer de URL van de service en de beheerder api-toets tot onze configuratiebestanden. Deze toets vindt plaats via de pagina met zoeken in de portal en deze worden gebruikt om te verifiëren bij de service.
    
**Ontwikkel een zoekopdracht indexering taak: Windows-Console**

1. Lees alle wederverkopers uit database.
2. Bellen van de Azure-Service-API voor zoeken als u wilt uploaden wederverkopers één voor één (Zie: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Een eigenschap instellen in de database die wederverkoper voor incrementele indexeren is geïndexeerd. We hebben dit gedaan door toe te voegen een 'indexering'-veld waarin de status van de index van elk profiel (geïndexeerd al dan niet). 

Zie de bijlage voor het codefragment die de taak indexering maakt.

**Een zoekopdracht webportal – MVC ontwikkelen**

1. Azure-zoekservice om alle documenten van uw zoekopdracht bellen (Zie: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. Uitpakken volgen uit het antwoord van de service zoeken (via json.net http://james.newtonking.com/json)
   - Resultaten
   - Aspecten
   - Resultatentellingen
   - Ontwikkel een gebruikersinterface voor het weergeven van zoekresultaten, aspecten en telt (we al had dit).

Dit is de code die is gebruikt om te krijgen van de resultaten van Azure zoeken:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Waarbij de locatie**

De belangrijkste vereiste om te controleren of in het prototype opgenomen waarschijnlijk een zoekwoord locatie toevoegen aan de query. Onze portal die als een gebruiker een plaatsnaam invoert in de zoekresultaten voor een query, die noodzakelijk is de wederverkopers in de opgegeven stad hoger dan leveranciers het trefwoord plaats in de beschrijving wilt rangschikken. Voor deze vereiste is gebruikt om een score profiel te rangschikken van het plaatsveld die hoger zijn dan de andere velden.

**Ondersteuning van meerdere talen**

We die nodig zijn voor de juiste zoekresultaten weergeven in de juiste talen en geef een optie voor het zoeken van dezelfde resultaten in verschillende talen. De twee zijden voor dit probleem zijn: 

- Zoeken naar woorden in meerdere talen
- Zoekresultaten in de juiste taal weergeven

We kan worden opgelost het deel achter de presentatie aan een document voor elke taal met gelokaliseerde tekst en een eigenschap met de taal toevoegen. Wanneer een gebruiker een zoekterm voert we gebruiker `$filter` expressies om te filteren op de taal van de gebruiker ervoor heeft gekozen.

Elk van de documenten heeft een verborgen eigenschap met de naam 'steden"gebaseerd op het type van de siteverzameling. Deze eigenschap opgeslagen stad namen in alle talen, zodat de gebruiker wilt zoeken in meerdere talen.

###<a name="data-storage"></a>Gegevensopslag

Alle gegevens (profiel, abonnement en accounting) is opgeslagen in SQL-Database. Alle media-bestanden worden opgeslagen in Azure-blobopslag, met inbegrip van afbeeldingen en video's verstrekt door de winkel. Gebruik van afzonderlijke blobopslag geïsoleerd de effecten van de bestanden zijn geüpload; bestanden, zijn nooit samen mingled met de website, zodat we niet nodig hebt om de site opnieuw wanneer we u bestanden toevoegen te maken.

Een belangrijk voordeel van onze Opslagontwerp is dat meerdere ontwikkelaars een enkel development-opslag kunnen delen. Een van de vereisten voor het project WhatToPedia was moeten kunnen maken van een ontwikkelomgeving binnen 15 minuten, inclusief wederverkoper gegevens, afbeeldingen en video's. Door de meest recente gegevens ophalen uit TFS Online, een SQL-script uitvoeren en uitvoeren van de taak importeren, een volledige-omgeving kan worden bevond zich omhoog binnen de kortste keren helemaal. Deze procedure wordt ook verbeterd voor het tijdelijk opslaan proces.

###<a name="webjobs"></a>WebJobs

We Azure WebJobs gebruiken om gegevens te werken aan de index. Door een zoekopdracht indexering taak maakt, is het deel achter de indexing heel eenvoudig integreren in onze oplossing. De enige codewijziging wordt aangebracht is zijn voor de taak indexering is om toe te voegen een `Indexed` veld aan onze gegevensmodel om aan te geven van de index staat. Wanneer een nieuw profiel wordt toegevoegd of bijgewerkt, de `Indexed` veld is ingesteld op onwaar. Hetzelfde geldt als de winkel zijn of haar profielgegevens via de portal wijzigingen.  

De taak Hiermee wordt gezocht naar alle rijen moeten `Indexed` ingesteld op onwaar. Als dit de rij wordt gevonden, het document is gepost naar Azure zoeken en typt u vervolgens de `Indexed` veld is ingesteld op waar. Wat u niet hoeft te plannen voor het toevoegen van versus het bijwerken van gegevens, omdat de zoekservice van Azure daadwerkelijk voor dit zorgt. Als u een document dat zich al hebt toegevoegd, wordt de service een update automatisch doet.

Alle taken van het web zijn ontwikkeld als consoletoepassingen die kunnen worden geüpload naar Azure-websites ZIP-bestanden, bestanden en klik vervolgens gepland.

De taak is gepland voor het uitvoeren van elke 5 minuten als een webtaak geplande. We berekend dat duurt de service ongeveer drie minuten 3000 documenten uploaden die binnen onze vereisten is. 

> [AZURE.NOTE] Er is een functie van prototype indexering die onlangs is geïntroduceerd in Azure zoeken. Deze functie hebt gekregen te laat voor wij in onze first release-programma gebruiken, maar deze wordt weergegeven voor het oplossen van hetzelfde probleem die we onze taak indexering voor, gebruikt welke is om gegevens laden gegevensbewerkingen te automatiseren.


###<a name="backup-strategy"></a>Back-up strategie

We ontworpen een meerlagige back-up strategie herstellen uit een bereik van scenario's, na een volledige uitval, naar beneden af op herstel van een afzonderlijke transactie. De activa te beveiligen hebben drie soorten gegevens (website, abonneeservergegevens en media-bestanden). 

Eerst doordat de website-broncode op Online TFS weten we dat als de site uitvalt, we opnieuw opbouwen kunnen door opnieuw uit TFS te publiceren. 

Gegevens van de abonnee in Azure SQL-Database is het meest gevoelige actief. We terug dit met behulp van de ingebouwde aanbevelen (Zie [Azure SQL Database-back-up en herstellen](http://msdn.microsoft.com/library/azure/jj650016.aspx)). De back-planning is volledige databaseback-up eenmaal per week, differentiële back-ups één keer per dag en transactie log back-ups elke 5 minuten.  Deze oplossing is gegeven de grootte van de gegevens, meer dan voldoende zijn voor ons direct en verwachte gegevensvolumes.

Derde, we afbeeldings- en videobestanden bestanden opslaan in Azure-blobopslag. We zijn nog steeds evaluatie van het gebruik van deze gegevens, de ultimate back-ups plannen Bezig Cloudberry Explorer voor Azure als een mogelijke oplossing. Nu we een WebJob gebruiken om afbeeldingen en video's kopiëren naar een andere locatie.

##<a name="what-we-learned"></a>Wat we hebt geleerd

Omdat we al gegevens, is het gemakkelijk tot stand brengen van bewijs-van-concept. Binnen uur hebben we een prototype met aspecten en items, pagineren, rangschikking profielen met zoekresultaten. De lijst met zoekresultaten zijn zo nauwkeurig, dat we besloten om te verwijderen enkele van de filters gepresenteerd bij de klant. 

De grootste verrassing voor ons is hoe snel we Azure zoeken kan leren en hoeveel we hij terugkomt. Letterlijk, we tot stand gebracht bewijs van concept in een paar uur (Zie de onderstaande notitie), 500 programmacoderegels vervangen door 3 regels met code in de front-end-toepassing (plus een nieuwe WebJob) en het verkrijgen van betere resultaten. 

Onze code geïmplementeerd eerder, paginering, telt en andere problemen die zijn standaard om te zoeken. Met de zoekfunctie van Azure, opnemen de resultaten die we terug te gaan de treffers zoeken, aspecten, gegevens, telt--alle items die we nodig en zijn dat u hoeft te leveren onszelf pagineren. Deze ook prestatieverbetering en ingebouwde beperkte navigatie die we in de oorspronkelijke oplossing hebben opgenomen.

De grootste uitdaging tijdens implementatie is dat deze een voorbeeldversie was en zoeken naar informatie en gedeelde ervaringen moeilijk is. Als we een paar punten verbonden, gevonden dat met Azure-zoekservice heel eenvoudig vanwege de REST API en JSON-gegevensindeling is. Verwijst naar het kader kan rechtstreeks vanuit meest openen bron Plug-ins zoals JQuery JSON.Net en hulpprogramma's zoals Fiddler kan worden gebruikt voor snelle experimenten en foutopsporing. 

> [AZURE.NOTE] Naast de gegevens domainprep lukt, geholpen die van ons het prototype al samenstellen begrepen hoe zoek technologie werken, waardoor u ons productiever en meer beleggingsfaciliteit van de ingebouwde functies. Als u output op samenstellen van de query zoeken moet, beperkte navigatie, filters, enz. u kunt verwachten prototypen naar langer duren. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>Aspecten in de presentatie zoekpagina bepalen

Een van onze geleerde lessen tijdens het bewijs-van-concept was aspecten zorgvuldig voorhand plannen. Nadat een groot aantal gegevens in de oplossing is geladen, gezien we dat het grote aantal aspecten is te hoog is om aan te bieden aan de gebruikers. 

We kan dit al worden opgelost aan de parameter facet aantal met beperkingen. De parameter aantal legt een vaste limiet van het aantal aspecten als resultaat gegeven aan de gebruiker. Een koppeling met een discussie van de parameter aantal vindt u [hier](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>WebJobs voor het plannen van taken

Azure zoeken is het alleen prettig verrassing voor ons niet. We ontdekt dat WebJobs gebruiken om te automatiseren van onze gegevensbewerkingen laden naar Azure zoeken sterk boven onze vorige benadering, die tot gevolg dat met een speciale VM Windows Scheduler, met de geplande taken is voor het bijwerken van de zoekindex uitgevoerd. WebJobs is eenvoudiger te configureren en gebruiksvriendelijker foutopsporing en natuurlijk nog goedkoper dan dat u hoeft te betalen voor een speciale VM.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure BLOB Storage Explorer voor het bijwerken van afbeeldingen

Gevonden die zeer nuttig zijn bij het beheren van afbeeldings- en videobestanden updates voor de site met [Azure BLOB Storage Explorer](https://azurestorageexplorer.codeplex.com/) (beschikbaar op codeplex). We gebruiken deze als een hulpmiddel ontwikkelaars handmatig bijwerken afbeeldingen en video's die deel uitmaken van onze hoofdsite. We gevonden ervan kan worden flexibeler dan het implementeren van wijzigingen bij de portal en lost een volledige test iteratie wanneer moeten we bijwerken van een afbeelding. 

##<a name="a-few-final-words"></a>Een paar definitief woorden

Dankzij de geweldige mensen op WhatToPedia voor ons toestemming te delen hun verhaal!  

We hopen dat u deze casestudy nuttig zijn gevonden. Als u verdergaat met Azure zoeken gebruiken, aanbevolen ik enkele bronnen om u langs snel:

- [MSDN-forum specifiek voor Azure zoeken](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow, heeft ook een tag](http://stackoverflow.com/questions/tagged/azure-search)
- [Documentatiepagina op Azure.com](https://azure.microsoft.com/documentation/services/search/)
- [Azure zoeken documentatie op MSDN](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>Bijlage: Zoeken indexering WebJob

De volgende code genereert de indexering vermeld in de sectie voor het bouwen van het prototype.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 
