<properties
   pageTitle="Patronen voor multitenant SaaS-toepassingen en Azure SQL-Database ontwerpen | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe de vereisten en algemene gegevens architectuur patronen van multitenant database toepassingen die worden uitgevoerd in een cloud-omgeving op moet letten en de verschillende compromissen nodig zijn die is gekoppeld aan deze patronen. Ook wordt uitgelegd hoe Azure SQL-Database, met de elastische van toepassingen en hulpmiddelen voor elastische te herstellen op een wijze zonder deze vereisten is voldaan."
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-design"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-sql-database"></a>Patronen voor multitenant SaaS-toepassingen en Azure SQL-Database ontwerpen

In dit artikel u kunt meer informatie over de vereisten en algemene gegevens architectuur patronen van multitenant software als een service (SaaS)-databasetoepassingen die worden uitgevoerd in een cloud-omgeving. Ook wordt uitgelegd de factoren moet u rekening houden en de voor-en nadelen van verschillende Ontwerpeffecten patronen. Elastische pools en elastische hulpmiddelen in Azure SQL-Database kunt u aan uw specifieke vereisten voldoet zonder verlies van andere doelstellingen.

Ontwikkelaars aanbrengt soms keuzemogelijkheden waarmee u kunt ten opzichte van hun langdurige belang werken wanneer ze pachtadres modellen voor de lagen gegevens van multitenant toepassingen ontwerpt. In eerste instantie ten minste kan een ontwikkelaar zien gebruiksgemak ontwikkelen en lagere cloud serviceprovider kosten belangrijker dan tenant moeten worden geïsoleerd of de schaalbaarheid van een toepassing. Deze keuze kan leiden tot de tevredenheid van vragen van klanten en een dure cursus-correctie later.

Een toepassing voor de multitenant is een toepassing die wordt gehost in een cloud-omgeving en die dezelfde reeks services naar honderden of duizenden tenants die niet delen of Zie elkaars gegevens bevat. Een voorbeeld is een toepassing SaaS waarmee services met tenants in een omgeving cloud-die worden gehost.

## <a name="multitenant-applications"></a>Multitenant toepassingen

In multitenant toepassingen, kunnen gegevens en werkbelasting eenvoudig partitioneren. U kunt partitioneren gegevens en werkbelasting, bijvoorbeeld langs de grenzen van de tenant, omdat de meeste aanvragen optreden binnen de grenzen van een tenant. Deze eigenschap is inherent aan de gegevens en de werkbelasting en dit geeft voorrang aan de toepassing patronen in dit artikel beschreven.

Dit type toepassing ontwikkelaars gebruiken in de hele verschillende cloudgebaseerde toepassingen, waaronder:

- Partner-databasetoepassingen die zijn die worden overgebracht naar de cloud als SaaS-toepassingen
- SaaS toepassingen voor de cloud helemaal omhoog
- Toepassingen voor directe, klant-omlaag
- Bedrijfstoepassingen werknemer-omlaag

SaaS-toepassingen die zijn ontworpen voor de cloud en personen met worteltrekken zijn zoals databasetoepassingen meestal partner multitenant toepassingen. Deze toepassingen SaaS geven een speciale software-toepassing als een service voor hun tenants. Tenants kunnen toegang heeft tot de application service en volledige eigendom van de bijbehorende gegevens die zijn opgeslagen als onderdeel van de toepassing. Maar om te profiteren van de voordelen van SaaS, tenants sommige controle over hun eigen gegevens moeten afstand. Ze vertrouwen de SaaS-serviceprovider om te voorkomen dat hun gegevens veilig en geïsoleerd andere tenants gegevens. Voorbeelden van dergelijke multitenant SaaS toepassing zijn MYOB, SnelStart en Salesforce.com. Elk van deze toepassingen kunt partitioneren langs de grenzen van de tenant en ondersteuning voor de toepassing ontwerpen patronen wordt besproken in dit artikel.

Toepassingen die directe-service biedt voor klanten of werknemers binnen een organisatie (ook wel als gebruikers, in plaats van tenants) zijn een andere categorie op de verschillende multitenant-toepassing. Klanten Abonneer u op de service en bent niet de eigenaar van de gegevens die de serviceprovider verzameld en opgeslagen. Serviceproviders hebben minder strikte vereisten om de gegevens van hun klanten geïsoleerd uit elkaar voorbij overheid verplicht bescherming van persoonsgegevens. Voorbeelden van dit soort klant is gericht multitenant toepassing zijn media inhoudsproviders zoals Netflix, Spotify en Xbox LIVE. Andere voorbeelden van eenvoudig partitionable toepassingen zijn klant-omlaag, Internet-toepassingen, of Internet van dingen (IoT) toepassingen in elke klant of apparaat kan dienen als een partition. Partition grenzen kunnen scheiden gebruikers en apparaten.

Niet alle toepassingen partition eenvoudig langs één eigenschap zoals tenant, klant, gebruiker of apparaat. Een complexe enterprise resource planning (ERP)-toepassing, bijvoorbeeld heeft producten, orders en klanten. Er wordt meestal een complexe schema met duizenden ten zeerste overlappende tabellen.

Geen enkele partition strategie kunt toepassen op alle tabellen en werk binnen de werklast van een toepassing. In dit artikel ligt de nadruk op multitenant toepassingen eenvoudig partitionable gegevens en werkbelasting hebben.

## <a name="multitenant-application-design-trade-offs"></a>Multitenant toepassing ontwerpen voor-en nadelen

Het ontwerppatroon dat een toepassingsontwikkelaar multitenant meestal kiest is gebaseerd op een aandacht van de volgende factoren:

-   **Tenant moeten worden geïsoleerd**. De ontwikkelaar nodig heeft om ervoor te zorgen dat er geen tenant ongewenste toegang tot de andere tenants gegevens heeft. De vereiste moeten worden geïsoleerd breidt naar andere eigenschappen, zoals een bescherming van ruis neighbors, wordt het herstellen van een tenant-gegevens en implementeren van tenant / regiospecifieke aanpassingen.
-   **Cloud resourcekosten**. Een toepassing SaaS moet kosten-Concurrentieanalyse. Een toepassingsontwikkelaar multitenant kunt optimaliseren voor lagere kosten in het gebruik van cloud resources, zoals opslagruimte en berekenen van kosten.
-   **Gebruiksgemak DevOps**. Een toepassingsontwikkelaar multitenant moet nemen moeten worden geïsoleerd beveiliging, behouden, en de status van hun toepassingen en databaseschema controleren en problemen met tenant. Complexiteit in ontwikkelen van toepassingen en -bewerking rechtstreeks naar extra kosten vertaalt en uitdagingen met de tevredenheid van de tenant.
-   **Schaalbaarheid**. De mogelijkheid om meer tenants en capaciteit voor tenants die nodig hebben deze stapsgewijs toevoegen is noodzakelijk om een succesvolle SaaS-bewerking.

Elk van deze factoren heeft voor-en nadelen, vergeleken met een ander. De laagste kosten cloud aanbod mogelijk niet de meest geschikte development-ervaring bieden. Het is belangrijk is voor een ontwikkelaar op de hoogte keuzes maken over deze opties en hun voor-en nadelen tijdens het ontwerpproces van toepassing.

Een patroon populaire ontwikkeling is pack meerdere tenants in een of meer databases. De voordelen van deze methode zijn lagere kosten, omdat u een paar databases, en de relatieve werken met een beperkt aantal databases betalen. Maar na verloop van tijd een ontwikkelaar van de multitenant toepassingen SaaS realiseren dat deze keuze in de tenant moeten worden geïsoleerd en schaalbaarheid van de aanzienlijke nadelen heeft. Als tenant moeten worden geïsoleerd belangrijke verandert, is extra hoeveelheid gegevens van de tenant in gedeelde opslag beschermen tegen ongeoorloofde toegang of ruis neighbors vereist. Deze aanvullende inspanning mogelijk aanzienlijk verhogen ontwikkeling overzichtelijk houden en de onderhoudskosten moeten worden geïsoleerd. Op dezelfde manier als tenants toevoegen vereist is, vereist dit ontwerppatroon meestal expertise opnieuw te verspreiden tenant gegevens over databases naar behoren schaal het niveau van de gegevens van een toepassing.  

Tenant moeten vaak worden geïsoleerd is een fundamentele behoefte in SaaS multitenant toepassingen die geschikt zijn voor bedrijven en organisaties. Een ontwikkelaar kan door waargenomen voordelen in eenvoudig en kosten ten opzichte van de tenant moeten worden geïsoleerd en schaalbaarheid verleiding komen. Deze verhouding kunt bewijzen complexe en dure terwijl de service in omvang groeit en tenant geïsoleerd moeten worden meer belangrijke en beheerde bij de toepassingslaag. In multitenant toepassingen waarmee een service directe, op consumenten-omlaag naar klanten, tenant moeten worden geïsoleerd mogelijk wel een lagere prioriteit dan optimaliseren voor resourcekosten cloud.

## <a name="multitenant-data-models"></a>Multitenant gegevensmodellen

Algemene ontwerp procedures voor het plaatsen van gegevens van de tenant Volg drie afzonderlijke modellen, zie afbeelding 1.


  ![Gegevensmodellen multitenant toepassing](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)
 afbeelding 1: algemene ontwerp procedures voor het multitenant gegevensmodellen

-   **Database per tenant**. Elke tenant heeft een eigen database. Alle gegevens van de tenant / regiospecifieke is beperkt tot de database van de tenant en geïsoleerd van andere tenants en hun gegevens.
-   **Een database laptopgeheugen gedeeld**. Meerdere tenants delen een van meerdere databases. Een unieke set tenants wordt toegewezen aan elke database met behulp van een partities strategie zoals hash, bereik of lijst partitioneren. Deze gegevens verdeling strategie wordt vaak genoemd sharding.
-   **Gedeelde database-enkel**. Een soms grote, database bevat gegevens voor alle tenants, die zijn disambiguated in de kolom van een tenant-ID.

> [AZURE.NOTE] Een ontwikkelaar van toepassingen kunt plaatsen van verschillende tenants in andere database schema's en gebruikt u de naam van het schema te onderscheiden van de verschillende tenants. Deze methode wordt niet aanbevolen omdat er meestal het gebruik van dynamische SQL moeten en deze niet kunnen effectieve in caching van abonnement worden. In de rest van dit artikel, richten we ons op de gedeelde tabel methode voor deze categorie van multitenant toepassing.

## <a name="popular-multitenant-data-models"></a>Populaire multitenant gegevensmodellen

Het is belangrijk de verschillende soorten multitenant gegevensmodellen in de toepassing ontwerpen voor-en nadelen die we al hebt geïdentificeerd wordt berekend. Deze factoren helpen beschrijving van de drie meest voorkomende multitenant gegevensmodellen hierboven en het gebruik van de database zoals wordt weergegeven in de afbeelding 2.

-   **Moeten worden geïsoleerd**. De mate van moeten worden geïsoleerd tussen tenants is een maateenheid voor hoeveel tenant moeten worden geïsoleerd een gegevensmodel realiseert.
-   **Cloud resourcekosten**. De hoeveelheid bronnen delen tussen tenants kunt cloud resourcekosten optimaliseren. Een resource kan worden gedefinieerd als de opslag en rekenkracht kosten.
-   **DevOps kosten**. Het gemak van de toepassingsontwikkeling, implementatie en beheerbaarheid die u Hiermee reduceert u totale kosten voor SaaS-bewerking.  

Afbeelding 2 ziet u de Y-as het niveau van de tenant moeten worden geïsoleerd. De X-as ziet u het niveau van het delen van de resource. De grijze, diagonale pijl in het midden geeft de richting van de kosten voor DevOps, waarbij verhogen of verlagen.

![Populaire multitenant toepassing ontwerppatronen](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png) afbeelding 2: populaire multitenant gegevensmodellen

De rechterbenedenhoek in afbeelding 2 wordt gebruikt om een patroon toepassing die wordt gebruikt een mogelijk grote, gedeelde één database, en de gedeelde tabel (of apart schema) benadering. Het is handig voor resource omdat alle tenants dezelfde database resources (CPU, geheugen, invoer/uitvoer) in één database gebruiken delen. Maar tenant moeten worden geïsoleerd is beperkt. U moet mogelijk extra stappen uitvoeren om te beveiligen met een tenants van elkaar op de toepassingsobjectlaag. De kosten DevOps van ontwikkelen en beheren van de toepassing kunnen aanzienlijk verbeteren door deze extra stappen uit. Schaalbaarheid wordt beperkt door de schaal van de hardware waarop de database.

De linkerkant Kwadrant in afbeelding 2 ziet u meerdere tenants een laptopgeheugen over meerdere databases (meestal verschillende hardware schalen voor eenheden). Elke database host een subset van tenants, waarin de belangstelling schaalbaarheid van andere patronen. Als u meer capaciteit is vereist voor meer tenants, kunt u gemakkelijk de tenants plaatsen op nieuwe databases die zijn toegewezen aan nieuwe hardware schaal-eenheden. Echter is van het delen van de resource minder. Alleen tenants geplaatst op dezelfde schaaleenheden resources delen. Deze methode biedt veel verbetering om te moeten worden geïsoleerd tenant omdat veel tenants zijn nog steeds collocated zonder te worden automatisch beschermd tegen elkaars acties. Toepassing complexiteit blijft hoog.

De linkerbovenhoek Kwadrant in afbeelding 2 is de derde wijze. Gegevens van elke tenant in een eigen database geplaatst. Deze methode heeft goede tenant-moeten worden geïsoleerd eigenschappen, maar beperkt resource delen: wanneer u elke database heeft een eigen toegewezen resources. Deze methode werkt goed als alle tenants overzichtelijk werkbelasting hebt. Als tenant werkbelasting minder overzichtelijk worden, optimaliseren niet de provider resources delen. Onvoorspelbaarheid geldt voor SaaS-toepassingen. De provider moet op te veel inrichten om te voldoen aan vereisten of onderste resources. Een actie levert kosten hogere of lagere tevredenheid van de tenant. Er wordt een hogere mate van resources delen met tenants wenselijk om de oplossing meest efficiënt. Het aantal databases ook verhoogt DevOps kosten om te implementeren en voor het behoud van de toepassing. Deze methode biedt ondanks deze problemen de beste en eenvoudigste moeten worden geïsoleerd voor tenants.

Deze factoren ook van invloed zijn op het ontwerppatroon dat in een klant kiest:

-   **Eigendom van gegevens van de tenant**. Een toepassing waarin tenants eigendom van hun eigen gegevens bewaard meer gericht op het patroon van één database per pachter.
-   **Schaal**. Database delen methoden zoals sharding meer gericht op een toepassing die is bedoeld voor honderden duizenden of miljoenen tenants. Geïsoleerd moeten kunnen nog steeds uitdagingen vormen.
-   **Waarde en bedrijven model**. Als een toepassing per-tenant inkomsten als kleine (minder dan een euro), geïsoleerd moeten worden minder kritieke en een gedeelde database relevant. Als de omzet per tenant een paar euro of meer is, is een database per tenant-model meer haalbaar is. Het mogelijk handiger ontwikkeling verlagen.

Het ontwerp voor-en nadelen weergegeven in afbeelding 2 gegeven, moet een ideaal multitenant model goede tenant moeten worden geïsoleerd eigenschappen verwerken met optimale resource delen tussen tenants. Dit model past in de categorie die worden beschreven in de rechterbovenhoek pas van figuur 2.

## <a name="multitenancy-support-in-azure-sql-database"></a>Multitenancy ondersteuning in Azure SQL-Database

Azure SQL-Database ondersteunt alle multitenant toepassing patronen die worden beschreven in de afbeelding 2. Ondersteunt ook een toepassing patroon dat goede resources delen combineert met elastische van toepassingen, en de voordelen moeten worden geïsoleerd van de database per tenant benaderen (Zie de hoofdletters pas in afbeelding 3). Hulpmiddelen voor databases elastische en mogelijkheden in SQL-Database helpen verkleinen van de kosten voor ontwikkelen en beheren van een toepassing met veel databases (weergegeven in het gearceerde gebied in de afbeelding 3). Deze hulpprogramma's kunt u maken en beheren van toepassingen die gebruikmaken van een van de database voor meerdere patronen.

![Patronen in Azure SQL-Database](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png) afbeelding 3: Multitenant toepassing patronen in Azure SQL-Database

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Database per tenant model met elastische van toepassingen en hulpprogramma 's

Elastische pools in SQL-Database combineren tenant moeten worden geïsoleerd bij resource delen tussen tenant-databases voor betere ondersteuning van de database per tenant-methode. SQL-Database is een oplossing voor het niveau van gegevens voor SaaS-providers die multitenant toepassingen maken. De werklast van de resource delen tussen tenants verplaatst van de toepassingslaag aan de database service laag. De complexiteit van beheren en query's uitvoeren bij het op schaal in databases is eenvoudiger geworden met elastische taken, elastische query elastische transacties en de bibliotheek van de client elastische database.

| Toepassingsvereisten voor de | Mogelijkheden voor SQL-database |
| ------------------------ | ------------------------- |
| Tenant moeten worden geïsoleerd en resources delen | [Elastische pools](sql-database-elastic-pool.md): toewijzen van een resourcegroep SQL-Database en de resources delen met andere verschillende databases. Afzonderlijke databases kunnen ook de zo veel resources uit de groep zo nodig zijn voor capaciteit aanvraag pieken gevolg van wijzigingen in de tenant werkbelasting aandacht. De elastische toepassingen zelf kan worden vergroot omhoog of omlaag naar wens. Elastische groepen ook een eenvoudige beheerbaarheid en controle en probleemoplossing op het niveau van de groep. |
| Gebruiksgemak DevOps over databases | [Elastische pools](sql-database-elastic-pool.md): zoals hierboven is.|
||[Elastische query](sql-database-elastic-query-horizontal-partitioning.md): Query over databases voor rapporten of cross-tenant analyse.|
||[Elastische taken](sql-database-elastic-jobs-overview.md): pakket en veilig database onderhoudsbewerkingen of wijzigingen in de database schema tot meerdere databases te implementeren.|
||[Elastische transacties](sql-database-elastic-transactions-overview.md): proces wordt gewijzigd in verschillende databases op een manier atomisch en geïsoleerd. Elastische transacties zijn nodig wanneer toepassingen "Alles of niets" garanties over meerdere databasebewerkingen moeten. |
||[Elastische database client-bibliotheek](sql-database-elastic-database-client-library.md): gegevens onderzoeken te beheren en in kaart brengen tenants aan databases. |

## <a name="shared-models"></a>Gedeelde modellen

Zoals eerder beschreven, voor de meeste providers SaaS, opleveren een aanpak gedeeld model voor problemen met tenant moeten worden geïsoleerd problemen en complexiteit met het ontwikkelen van toepassingen en onderhoud. Echter voor multitenant toepassingen die dienstverlening rechtstreeks op consumenten, tenant moeten worden geïsoleerd vereisten mogelijk niet als hoge prioriteit als kosten minimaliseren. Ze mogelijk inpakken tenants in een of meer databases met een hoge dichtheid kosten te verlagen. Gedeeld databasemodellen met één database of meerdere een laptopgeheugen databases kunnen leiden tot extra efficiëntie in de kosten van de algehele en delen van de resource. Azure SQL-Database bevat enkele functies waarmee klanten moeten worden geïsoleerd voor betere beveiliging en beheer op schaal in de gegevenslaag maken.

| Toepassingsvereisten voor de | Mogelijkheden voor SQL-database |
| ------------------------ | ------------------------- |
| Moeten worden geïsoleerd beveiligingsfuncties | [Beveiliging op gebruikersniveau rij](https://msdn.microsoft.com/library/dn765131.aspx) |
|| [Databaseschema](https://msdn.microsoft.com/library/dd207005.aspx) |
| Gebruiksgemak DevOps over databases | [Elastische query](sql-database-elastic-query-horizontal-partitioning.md) |
|| [Elastische taken](sql-database-elastic-jobs-overview.md) |
|| [Elastische transacties](sql-database-elastic-transactions-overview.md) |
|| [Elastische database client-bibliotheek](sql-database-elastic-database-client-library.md) |
|| [Samenvoegen en elastische database splitsen](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Overzicht

Tenant geïsoleerd moeten zijn belangrijk is voor de meeste SaaS multitenant toepassingen. De beste optie op te geven, moeten worden geïsoleerd leans intensief richting van de database per tenant-methode. De andere twee methoden vereisen investeringen in complexe toepassingslagen die moeten worden ervaren ontwikkeling personeelsleden moeten worden geïsoleerd, waardoor aanzienlijk kosten en risico's bieden. Als geïsoleerd moeten niet vroeg in de serviceontwikkeling verwerkt worden, kan aanpassing ze zijn nog meer dure in de eerste twee modellen. De belangrijkste nadelen die is gekoppeld aan het model database per tenant betrekking hebben op verbeterde cloud resourcekosten vanwege verminderde delen, en onderhouden en veel databases beheren. SaaS softwareontwikkelaars regelmatig te maken hebben wanneer ze deze voor-en nadelen.

Hoewel de voor-en nadelen mogelijk belangrijkste afsluitingen waarvan de meeste cloud database-mailproviders, voorkomt Azure SQL-Database de afsluitingen waarvan de elastische van toepassingen en mogelijkheden van de elastische database. SaaS ontwikkelaars kunnen de kenmerken moeten worden geïsoleerd van een database per tenant-model combineren en optimaliseren van resources delen en de verbeteringen in de beheerbaarheid van verschillende databases met behulp van elastische pools en bijbehorende hulpprogramma's.

Multitenant toepassing providers die geen tenant moeten worden geïsoleerd vereisten hebben en wie tenants in een database op een hoge dichtheid kunt inpakken mogelijk vinden dat gedeelde gegevens resultaat in extra efficiency in resources delen modellen en verlagen de totale kosten. Azure SQL-Database elastische databasehulpmiddelen, sharding bibliotheken en beveiligingsfuncties helpen SaaS providers bouwen en multitenant-toepassingen beheren.

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met hulpmiddelen voor elastische databases](sql-database-elastic-scale-get-started.md) met een steekproef-app die laat zien van de clientbibliotheek.

Een [aangepaste dashboard elastische toepassingen voor SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) maken met een steekproef-app die elastische toepassingen voor een efficiënte, scalable database-oplossing wordt gebruikt.

Gebruik de hulpmiddelen Azure SQL-Database wilt [migreren van bestaande databases aan de nieuwe schaal af](sql-database-elastic-convert-to-use-elastic-tools.md).

Bekijk onze zelfstudie voor het [maken van een elastische toepassingen](sql-database-elastic-pool-create-portal.md).  

Informatie over het [controleren en beheren van een elastische toepassingen](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Aanvullende informatie

- [Wat is een Azure elastische toepassingen?](sql-database-elastic-pool.md)
- [Schalen met Azure SQL-Database](sql-database-elastic-scale-introduction.md)
- [Multitenant toepassingen met hulpmiddelen voor databases elastische en beveiliging op gebruikersniveau rij](sql-database-elastic-tools-multi-tenant-row-level-security.md)
- [Verificatie in multitenant-apps met behulp van Azure Active Directory en verbindt u OpenID](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin enquêtes-toepassing](../guidance/guidance-multitenant-identity-tailspin.md)
- [Oplossing snel aan de slag](sql-database-solution-quick-starts.md)

## <a name="questions-and-feature-requests"></a>Vragen en functieverzoeken verzenden

Zoek us voor vragen, in het [forum van de SQL-Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Een verzoek voor een functie toevoegen in het [SQL-Database feedback-forum](https://feedback.azure.com/forums/217321-sql-database/).
