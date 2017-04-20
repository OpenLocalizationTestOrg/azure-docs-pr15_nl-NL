<properties
   pageTitle="Data warehouse werkbelasting"
   description="SQL Data Warehouse elasticiteit kunt u vergroten, verkleinen of rekenkracht onderbreken met behulp van een kleurenschaal beweegbare data warehouse eenheden (DWUs). In dit artikel wordt uitgelegd voor de doelstellingen van de data warehouse en hoe ze zich verhouden tot DWUs. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/25/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# <a name="data-warehouse-workload"></a>Data warehouse werkbelasting
Een data warehouse werkbelasting verwijst naar alle bewerkingen die ten opzichte van een data-warehouse gegevenstoegangsbewerkingen. De werklast van de BTW-records gegevens omvat het hele proces van het laden van gegevens in de BTW-records, het uitvoeren van analyses en rapportage op het datawarehouse, gegevens in het datawarehouse beheren en gegevens uit het datawarehouse exporteren. De diepteas en de breedte van de volgende onderdelen zijn vaak evenredig met het niveau van de vervaldatum van datawarehouse.


## <a name="new-to-data-warehousing"></a>Nieuw bij gegevensopslag?
Een data-warehouse is een verzameling gegevens die is geladen uit een of meer gegevensbronnen en wordt gebruikt om uit te voeren zoals rapporten en gegevensanalyse business intelligence-taken.

Datawarehouses worden gekenmerkt door query's die groter aantal rijen, grote gegevensbereiken scannen en kunnen relatief grote resultaten retourneren voor de toepassing van analyse en rapportage. Datawarehouses worden ook gekenmerkt door relatief grote gegevens laadtijd versus kleine transactie niveau wordt ingevoegd/updates/verwijderen.

- Een data-warehouse werkt het beste wanneer de gegevens worden opgeslagen op een wijze die in query's die een groot aantal rijen of grote gegevensbereiken scannen moeten worden geoptimaliseerd. Dit soort scannen werkt het best wanneer de gegevens worden opgeslagen en door kolommen, in plaats van op rijen doorzocht.

>[AZURE.NOTE] De index in het geheugen columnstore, die opslag in de kolom wordt, biedt maximaal 10 x compressie winst- en 100 x query prestatieverbeteringen via traditionele binaire bomen voor rapportage en analyse query's. We beschouwd columnstore indexen als de standaard voor het opslaan en het scannen van grote gegevens in een data-warehouse.

- Een data-warehouse heeft verschillende vereisten dan een systeem dat wordt geoptimaliseerd voor online transactie processing (OLTP). Het systeem OLTP heeft veel invoegen, bijwerken en verwijderen. Deze bewerkingen zoeken naar specifieke rijen in de tabel. Tabel zoekt best uitvoeren wanneer de gegevens worden opgeslagen op een wijze per rij. De gegevens kunnen worden gesorteerd en snel gezocht met een delen en heers benadering een binaire structuur of btree zoekopdracht genoemd.


## <a name="data-loading"></a>Gegevens laden
Gegevens laden is een belangrijk onderdeel van de werklast van de BTW-records gegevens. Bedrijven hebben meestal een bezet OLTP-systeem dat wordt bijgehouden wijzigingen hele dag wanneer klanten zakelijke transacties genereert. Regelmatig, vaak nachts tijdens een onderhoudsvenster, zijn de transacties verplaatst of gekopieerd naar het datawarehouse. Als u de gegevens hebt in datawarehouse, kunnen de analisten analyses uitvoeren en de zakelijke beslissingen nemen op de gegevens.

- Het proces van het laden heet traditioneel ETL voor uitpakken, transformeren en laden. Gegevens moeten meestal zodat u consistent is met andere gegevens in de BTW-records voor de gegevens worden getransformeerd. Eerder, wordt door bedrijven speciale ETL servers gebruikt om de transformaties. U kunt nu met deze snel massively parallelle verwerking eerst gegevens laadt in SQL Data Warehouse en voert u de transformaties. Dit proces heet extraheren, laden en transformeren (ELT), en wordt een nieuwe standaard voor de werklast van de BTW-records gegevens.

> [AZURE.NOTE] Met SQL Server-2016, kunt u nu analytics in uitvoeren op een tabel OLTP realtime. Hiermee vervangt niet dat u een data-warehouse om te slaan en te analyseren van gegevens, maar biedt een manier om analyse in realtime.

### <a name="reporting-and-analysis-queries"></a>Rapportage en analyse query 's
Query's voor rapportage en analyse worden vaak ingedeeld in klein, gemiddeld en grote op basis van een aantal criteria, maar gewoonlijk is gebaseerd op tijd. In de meeste datawarehouses is er een gemengde werkbelasting snel-te weinig versus langdurige query's. In elk geval is het belangrijk om te bepalen van deze combinatie en om te bepalen de frequentie (per uur, dagelijks, einde van de maand, einde van het kwartaal, enzovoort). Het is belangrijk om te begrijpen dat de werklast gemengde query, in combinatie met gelijktijdigheid, potentiële klanten met BEGINLETTERS capaciteit, planning voor een data-warehouse.

- Capaciteitsplanning kan complexe taak van een werkbelasting gemengde query zijn met name wanneer u een lange levertijd capaciteit toevoegen aan het datawarehouse nodig hebt. SQL Data Warehouse Hiermee verwijdert u de urgentie van het plannen van capaciteit sinds u kunt vergroten en verkleinen berekeningscluster capaciteit op elk gewenst moment en sinds opslagcapaciteit en berekeningscluster capaciteit onafhankelijk worden aangepast.

### <a name="data-management"></a>Gegevensbeheer
Gegevens beheren is belangrijk, vooral als u weet dat u mogelijk onvoldoende schijfruimte in de nabije toekomst. De gegevens verdeeld datawarehouses meestal over zinvolle bereiken, die zijn opgeslagen als partities in een tabel. Alle producten op basis van SQL Server kunnen u partities en afmelden bij de tabel te verplaatsen. Deze partition veranderen, kunt u oudere gegevens verplaatsen naar een minder dure opslag en houdt u de meest recente gegevens beschikbaar op online-opslag.

- Columnstore indexen ondersteuning gepartitioneerde tabellen. Voor columnstore indexen, worden de gepartitioneerde tabellen worden gebruikt voor het beheren van gegevens en het archiveren. Voor tabellen die zijn opgeslagen rij-voor-rij hebben partities een grotere rol in de prestaties van query's.  

- PolyBase speelt een belangrijke rol bij het beheren van gegevens. PolyBase gebruikt, hebt u de optie oudere gegevens naar Hadoop of Azure-blobopslag.  Hierdoor wordt een groot aantal opties aangezien de gegevens die zich nog steeds online.  Het duurt langer gegevens ophalen uit Hadoop, maar de verhouding van de tijd voor het ophalen mogelijk wegen tegen de kosten opslag.

### <a name="exporting-data"></a>Gegevens exporteren
Een manier om gegevens beschikbaar maken voor rapporten en analyse is om gegevens te verzenden vanaf het datawarehouse met servers speciale voor het uitvoeren van rapporten en analyse. Deze servers worden gegevensmarkten genoemd. U kunt bijvoorbeeld vooraf verwerken report with data en vervolgens uit het datawarehouse exporteren naar veel servers overal ter wereld, zodat u deze ruim zijn beschikbaar voor klanten en analisten.

- Voor het genereren van rapporten, elke nachtdienst kan u alleen-lezen rapportage servers met een momentopname van de dagelijkse gegevens vullen. Het resultaat is hogere bandbreedte voor klanten tijdens de behoeften van de resource berekenen op het datawarehouse verlagen. Vanuit het perspectief beveiliging kunnen u Beperk het aantal gebruikers die toegang tot het datawarehouse hebben gegevensmarkten.
- Van analysegegevens, kunt u een kubus analyse op het datawarehouse bouwen en analyses uitvoeren op het warehouse, of vooraf verwerken gegevens en exporteren naar de analyseserver voor verdere analyse.

## <a name="next-steps"></a>Volgende stappen
Nu u weet wat over SQL Data Warehouse, leren hoe u snel [een SQL-Data Warehouse maken][] en [Voorbeeldgegevens laden][].

<!--Image references-->

<!--Article references-->
[Voorbeeldgegevens laden]: ./sql-data-warehouse-load-sample-databases.md
[een SQL-Data Warehouse maken]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
