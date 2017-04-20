<properties
   pageTitle="Wat is Azure SQL Data Warehouse? | Microsoft Azure"
   description="Enterprise-klasse verdeeld database kunnen petabyte hoeveelheden relationele en niet-relationele gegevens verwerken. Het is van de bedrijfstak eerste cloud datawarehouse met vergroten, verkleinen en plaats de aanwijzer in seconden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# <a name="what-is-azure-sql-data-warehouse"></a>Wat is Azure SQL Data Warehouse?

Azure SQL Data Warehouse is een cloudgebaseerde, schalen database kunnen grote hoeveelheden gegevens, relationele en niet-relationele verwerken. Gebaseerd op de architectuur van onze massively parallelle verwerking (MPP), kan SQL Data Warehouse uw werkzaamheden enterprise verwerken.

SQL datawarehouse:

- Hiermee combineert de relationele SQL Server-database met Azure cloud schalen mogelijkheden. U kunt vergroten, verkleinen, onderbreken of hervatten berekeningscluster in seconden. U opslaan kosten door CPU schalen wanneer u deze nodig hebt en knippen terug gebruik momenten van niet-piek.
- Maakt gebruik van de Azure platform. Het is eenvoudig te implementeren, naadloos onderhouden en volledig fault fouttolerante vanwege automatische back-ups.
- Aanvulling op de SQL Server-systeem. U kunt ontwikkelen met vertrouwde SQL Server Transact-SQL-(T-SQL) en hulpprogramma's.

In dit artikel worden de belangrijkste functies van SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>Massively parallelle verwerking architectuur

SQL Data Warehouse is dat een massively parallelle verwerking (MPP) distributed databasesysteem. Door gegevens delen en de mogelijkheid op verschillende knooppunten verwerkt, kan SQL Data Warehouse enorme schaalbaarheid - uiterst buiten een enkel systeem bieden.  Uw gegevens worden in SQL Data Warehouse achter de schermen dubbele over veel gedeeld opslag en verwerkingseenheden. De gegevens is opgeslagen in Premium lokaal overtollige opslagruimte en gekoppeld aan het berekenen van knooppunten voor de uitvoering van de query. Met deze architectuur gaat SQL Data Warehouse een aanpak "Verdeel en heers" aan de laadtijd en complexe query's uitvoeren. Aanvragen worden ontvangen door het knooppunt besturingselement, geoptimaliseerd en vervolgens doorgegeven aan de knooppunten berekeningscluster kunnen werken parallel.

MPP-architectuur en Azure opslagmogelijkheden worden gecombineerd, SQL Data Warehouse kunt doen:

- Groter of kleiner opslag onafhankelijk van berekeningscluster.
- Groter of kleiner berekeningscluster zonder gegevens te verplaatsen.
- Een pauze invoegen berekenen capaciteit terwijl gegevens intact blijft.
- Cv berekenen capaciteit op elk gewenst moment.

In het volgende diagram ziet u de architectuur uitgebreider.

![SQL Data Warehouse architectuur][1]


**Besturingselement knooppunt:** Het knooppunt besturingselement worden beheerd en query's worden geoptimaliseerd. Dit is de front-end waarin interactie met alle toepassingen en verbindingen. Klik in SQL Data Warehouse, het knooppunt besturingselement is mogelijk gemaakt door SQL-Database en verbinden met dit eruitziet en dat lijkt op hetzelfde. Klik onder het oppervlak coördinaten het knooppunt besturingselement alle verplaatsing van gegevens en berekening vereist parallelle query's uitvoeren op uw gedistribueerde gegevens. Wanneer u een T-SQL-query in SQL Data Warehouse indient, het knooppunt besturingselement deze worden omgezet in afzonderlijke query's die op elk knooppunt berekeningscluster parallel worden uitgevoerd.

**Berekenen van knooppunten:** De knooppunten berekeningscluster fungeren als de kracht achter SQL Data Warehouse. Ze zijn SQL-Databases die uw gegevens op te slaan en het verwerken van uw query. Wanneer u gegevens toevoegt, worden de rijen aan de knooppunten berekenen in SQL Data Warehouse verdeeld. De knooppunten berekeningscluster zijn de werknemers die de parallelle query's op uw gegevens uitvoeren. Na verwerking geven deze de resultaten terug naar het knooppunt besturingselement. U voltooit de query door het knooppunt besturingselement de resultaten is samengevoegd en retourneert het uiteindelijke resultaat.

**Opslag:** Uw gegevens worden opgeslagen in Azure-blobopslag. Wanneer berekenen knooppunten werken met de gegevens, ze rechtstreeks naar en vanuit blobopslag lezen en schrijven. Aangezien Azure opslag wordt uitgebreid met transparant en sterk, SQL Data Warehouse hetzelfde kunt doen. Aangezien berekeningscluster en opslagruimte onafhankelijke, kan SQL Data Warehouse opslag afzonderlijk van schaalbaarheid berekenen en vice versa automatisch schalen. Azure-blobopslag is ook volledig fouttolerantie en stroomlijnt het proces back-up en herstellen.

**Verplaatsing gegevensservice:** Gegevens verkeer Service (DMS) verplaatst gegevens tussen de knooppunten. DMS geeft de berekeningscluster knooppunten-toegang tot gegevens die ze nodig voor joins en aggregaties. DMS is niet een Azure-service. Dit is een Windows-service die op alle knooppunten samen met SQL-Database wordt uitgevoerd. Aangezien DMS uitgevoerd achter de schermen, won't u rechtstreeks ermee werken. Echter wanneer u de query-abonnementen bekijkt, ziet u dat de opties omvatten sommige bewerkingen DMS aangezien verplaatsing van gegevens nodig is om elke query parallel.


## <a name="optimized-for-data-warehouse-workloads"></a>Geoptimaliseerd voor data warehouse werkbelasting

De MPP-methode wordt ondersteund door een aantal specifieke prestatieverbeteringen, inclusief voor gegevensopslag:

- Een gedistribueerde query optimizer en het instellen van complexe statistieken over alle gegevens. De service is met informatie in de gegevensgrootte- en distributiegroepen, query's optimaliseren door vast te stellen de kosten van specifieke gedistribueerde querybewerkingen.

- Geavanceerde algoritmen en technieken geïntegreerd in het proces van de verplaatsingsopties gegevens efficiënt om gegevens te verplaatsen tussen resources computing zo nodig de query uitvoeren. Deze bewerkingen van de verplaatsingsopties gegevens zijn gemaakt en alle optimalisaties toe aan de verplaatsing gegevensservice automatisch plaats.

- Gegroepeerde **columnstore** indexen al dan niet standaard. Met behulp van kolom gebruikgemaakt van opslag, krijgt SQL Data Warehouse gemiddeld 5 x compressie winst via traditionele rij gebaseerde opslag, plus maximaal 10 x of meer query prestaties verbeteren. Gebruiksanalyses-query's die te hoeft scannen een groot aantal rijen werk op columnstore indexen perfect uitzien.


## <a name="predictable-and-scalable-performance"></a>Overzichtelijk en scalable prestaties

SQL Data Warehouse worden gescheiden door opslagruimte en berekeningscluster, waarmee elk afzonderlijk schaal. SQL Data Warehouse kan snel en eenvoudig schalen dat het toevoegen van extra berekeningscluster resources op elk gewenst moment. Aanvulling dit is het gebruik van Azure-blobopslag. BLOB's bieden niet alleen stabiele, gerepliceerde opslag, maar ook de infrastructuur voor probleemloze uitbreiding tegen een lage prijs. SQL Data Warehouse kunt deze combinatie van cloud-schaal opslag en Azure berekeningscluster gebruikt, u betalen voor queryprestaties en opslag wanneer u deze nodig hebt. Wijzigen van de hoeveelheid berekeningscluster is net zo eenvoudig als het een schuifregelaar in de portal van Azure naar links of naar rechts verplaatst of deze kan ook worden gepland T-SQL en PowerShell gebruiken.

Samen met de mogelijkheid om te bepalen volledig de hoeveelheid berekeningscluster afzonderlijk van opslag, kunt SQL Data Warehouse u volledig onderbreken uw datawarehouse, wat betekent dat u niet betalen voor berekeningscluster dat wanneer u deze niet nodig hebt. Alle berekeningscluster is terwijl uw opslagruimte op plek blijft, in de belangrijkste toepassingen van Azure, zodat u geld uitgebracht. Wanneer nodig hebt, gewoon de berekeningscluster hervatten en uw gegevens hebt en beschikbaar voor uw werkzaamheden berekenen.

## <a name="data-warehouse-units"></a>Data Warehouse eenheden

Toewijzing van resources, uw SQL Data Warehouse wordt gemeten in Data Warehouse eenheden (DWUs). DWUs zijn een maateenheid voor onderliggende resources zoals CPU, geheugen, IO's / s, die zijn toegewezen aan uw SQL Data Warehouse. Het aantal DWUs verhoogt, worden resources en prestaties verwijderd. Name in DWUs ervoor te zorgen dat:

- U bent mogen schalen dat uw BTW-records voor de gegevens eenvoudig, zonder dat de onderliggende hardware of software.

- U kunt prestatieverbetering bij voor een niveau DWU voorspellen voordat u de grootte van uw BTW-records voor de gegevens wijzigen.

- De onderliggende hardware en software die van uw exemplaar kunnen wijzigen of verplaatsen zonder dat dit gevolgen heeft de prestaties van uw werkbelasting.

- Microsoft kan pas de onderliggende architectuur van de service handhaven de prestaties van uw werkzaamheden.

- Microsoft kunt snel de prestaties in SQL Data Warehouse, op een manier die kan worden aangepast en gelijkmatig effecten het systeem verbeteren.

Data Warehouse eenheden bieden een maateenheid drie nauwkeurig parameters die ten zeerste aan elkaar zijn gekoppeld met werkbelasting prestaties voor gegevensopslag. Het doel is dat de doelstellingen van de volgende belangrijke werkbelasting wordt evenredig met de DWUs die u hebt gekozen voor uw BTW-records voor de gegevens.

**Scan/aggregatie:** Deze metrisch werkbelasting gaat de query die een groot aantal rijen gescand en voert een complexe aggregatie van een standaard gegevensopslag. Dit is een i/o- en CPU vergt.

**Laden:** Deze metrisch meet de mogelijkheid om op te nemen van gegevens in de service. Laadtijd zijn met PolyBase een vertegenwoordiger gegevensverzameling laden uit Azure-blobopslag voltooid. Deze metrisch is bedoeld om belasting netwerk en Processorsnelheid aspecten van de service.

**Tabel maken als selecteren (CTAS):** CTAS meet de mogelijkheid om te kopiëren van een tabel. Dit heeft betrekking op het lezen van gegevens uit de opslag, deze op de knooppunten van het apparaat te distribueren en opnieuw met storage te schrijven. Het is een vergt CPU, IO en netwerk.

## <a name="pause-and-scale-on-demand"></a>Een pauze invoegen en schaal op aanvraag

Wanneer u sneller resultaten nodig hebt, wordt uw DWUs vergroten en betalen voor betere prestaties. Wanneer u hoeft minder rekenkracht, uw DWUs verkleinen en betalen alleen voor wat u nodig hebt. U denkt over het wijzigen van uw DWUs in deze scenario's:

- Wanneer u niet moet query's uitvoeren, bijvoorbeeld in de avonden of in het weekend, stilleggen uw query's. Vervolgens onderbreken uw resources berekeningscluster om te voorkomen dat betaalt voor DWUs wanneer u deze niet nodig hebt.

- Wanneer uw systeem lage aanvraag bevat, kunt u overwegen DWU naar een kleine grootte. U kunt nog steeds toegang tot de gegevens, maar op een significant kosten te besparen.

- Bij het uitvoeren van een veel gegevens laden of transformatie betrekking heeft, is het raadzaam om uit te breiden zodat uw gegevens sneller beschikbaar is.

Om te begrijpen wat uw ideaal DWU waarde is, probeer schalen omhoog en omlaag en een aantal query's uitvoeren na het laden van uw gegevens. Aangezien schaalbaarheid snelle is, kunt u een aantal verschillende niveaus van prestaties in een uur of minder proberen.  Houd er rekening mee dat SQL Data Warehouse is bedoeld om het verwerken van grote hoeveelheden gegevens en om te zien van de werkelijke capaciteit voor schaalbaarheid, met name bij de grotere schaal die we bieden, wilt u een grote gegevensverzameling die nadert of groter is dan 1 TB gebruiken.


## <a name="built-on-sql-server"></a>Gebouwd op SQL Server

SQL Data Warehouse is gebaseerd op de SQL Server relationele database engine en bevat veel van de functies die u uit een enterprise datawarehouse verwachten. Als u al weet hoe T-SQL, is het gemakkelijk overbrengen van uw kennis naar SQL Data Warehouse. Opgeven of u zijn geavanceerde of gaat voor het eerst, helpt de voorbeelden in de documentatie begint. Algemene, kunt u Denk na over de manier waarop we de taalelementen van SQL Data Warehouse als volgt hebt opgesteld:

- SQL Data Warehouse T-SQL-syntaxis voor veel bewerkingen wordt gebruikt. Ondersteunt ook een uitgebreide reeks traditionele SQL-constructies, zoals opgeslagen procedures, gebruiker gedefinieerde functies, tabel partitioneren, indexen en sorteringen.

- SQL Data Warehouse bevat ook een aantal nieuwere SQL Server-functies, waaronder: gegroepeerde **columnstore** indexen, PolyBase integratie en gegevens controleren (inclusief bedreiging assessment).

- Bepaalde T-SQL-taal-elementen die zijn minder gebruikelijk voor gegevensopslag werkbelasting of nieuwer met SQL Server, zijn mogelijk niet beschikbaar. Zie de [documentatie van de migratie][]voor meer informatie.

Met de functie en Transact-SQL overeenkomsten aangeven tussen SQL Server, SQL Data Warehouse SQL-Database en Platform analysesysteem, kunt u een oplossing die voldoet aan de gegevensbehoeften van uw ontwikkelen. U kunt bepalen waar wilt u uw gegevens, op basis van prestaties, beveiliging en schaal vereisten, en vervolgens gegevens zo nodig overbrengen van andere systemen voor.

## <a name="data-protection"></a>Gegevensbescherming

Alle gegevens SQL Data Warehouse opgeslagen in Azure Premium lokaal redundante opslag. Meerdere synchroon kopieën van de gegevens blijven behouden in het midden van de lokale gegevens te garanderen transparante gegevensbescherming voor het geval gelokaliseerde fouten. Daarnaast SQL Data Warehouse automatisch back-ups van uw actieve (voortgezet) databases met regelmatige tussenpozen met Azure opslag momentopnamen. Zie voor meer informatie over het back-up maken en terugzetten werkt, het [overzicht van back-up en herstellen][].

## <a name="integrated-with-microsoft-tools"></a>Geïntegreerd met Microsoft-hulpmiddelen

SQL Data Warehouse ook werkt naadloos samen veel van de hulpmiddelen die gebruikers kunnen mogelijk kennis met SQL-Server. Hierbij:

**Traditionele SQL Server tools:** SQL Data Warehouse is volledig geïntegreerd met SQL Server Analysis Services, Integration Services en Reporting Services.

**Extra cloudgebaseerde:** SQL Data Warehouse kan worden gebruikt samen met een aantal nieuwe hulpmiddelen in Azure wordt aangegeven, waaronder gegevens Factory, Stream analyses, Machine Learning en Power BI. Zie [overzicht van de geïntegreerde hulpmiddelen][]voor een gedetailleerde lijst.

**Hulpprogramma's van derden:** Een groot aantal hulpprogramma derden providers hebt integratie van hun's met SQL Data Warehouse gecertificeerd. Zie voor een volledige lijst met [SQL Data Warehouse oplossingspartners][].

## <a name="hybrid-data-sources-scenarios"></a>Scenario's voor bronnen van hybride-gegevens

SQL Data Warehouse gebruikt met PolyBase geeft gebruikers ongekende mogelijkheden om gegevens te verplaatsen in hun-systeem de mogelijkheid voor het instellen van geavanceerde hybride scenario's met niet-relationele ontgrendelen en on-premises gegevensbronnen.

Polybase kunt u gebruikmaken van uw gegevens uit verschillende gegevensbronnen met behulp van de vertrouwde T-SQL-opdrachten. Polybase kunt u query niet-relationele gegevens in Azure-blobopslag al dit een normale tabel is. Gebruik Polybase met niet-relationele querygegevens of niet-relationele gegevens te importeren in SQL Data Warehouse.

- Externe tabellen PolyBase gebruikt voor toegang tot de niet-relationele gegevens. De tabeldefinities worden opgeslagen in SQL Data Warehouse, en kunt u deze openen met behulp van SQL en hulpprogramma's zoals u zou normale relationele gegevens gebruiken.

- Polybase is agnostische in de integratie. De functies en functionaliteit aan alle bronnen die wordt ondersteund voor dezelfde worden getoond. De gegevens gelezen door Polybase kan zijn in verschillende indelingen, inclusief bestanden met scheidingstekens of ORC bestanden.

- PolyBase kan worden gebruikt voor toegang tot blobopslag die ook wordt gebruikt als de opslagruimte voor een cluster HDInsight. Hiermee geeft u toegang tot dezelfde gegevens met hulpmiddelen voor relationele en niet-relationele.

## <a name="sla"></a>SLA

SQL Data Warehouse biedt een product niveau SLA service level agreement () als onderdeel van Microsoft Online Services SLA. Ga naar [SLA voor SQL Data Warehouse][]voor meer informatie. SLA alle andere producten u kunt vindt u informatie over het [Niveau van serviceovereenkomsten] Azure-pagina of kunt u deze downloaden op de pagina [Volume Licensing][] . 

## <a name="next-steps"></a>Volgende stappen

Nu u weet wat over SQL Data Warehouse, leren hoe u snel [een SQL-Data Warehouse maken][] en [Voorbeeldgegevens laden][]. Als nieuw voor u Azure zijn, vindt u mogelijk de [woordenlijst voor Azure][] handig als u nieuwe terminologie optreden. Of Bekijk enkele van deze andere SQL Data Warehouse Resources.  

- [Verhalen]
- [Blogs]
- [Functieverzoeken verzenden]
- [Video 's]
- [Klant advies Team blogs]
- [Ondersteuningsticket maken]
- [MSDN-forum]
- [Stapel overloop-forum]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Ondersteuningsticket maken]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Voorbeeldgegevens laden]: ./sql-data-warehouse-load-sample-databases.md
[een SQL-Data Warehouse maken]: ./sql-data-warehouse-get-started-provision.md
[Migratie documentatie]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse oplossingspartners]: ./sql-data-warehouse-partner-business-intelligence.md
[Overzicht van de geïntegreerde hulpmiddelen]: ./sql-data-warehouse-overview-integrate.md
[Overzicht van back-up en herstellen]: ./sql-data-warehouse-restore-database-overview.md
[Azure woordenlijst]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Verhalen]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Klant advies Team blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Functieverzoeken verzenden]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stapel overloop-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video 's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA voor SQL datawarehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volumelicenties]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Serviceovereenkomsten]: https://azure.microsoft.com/en-us/support/legal/sla/
