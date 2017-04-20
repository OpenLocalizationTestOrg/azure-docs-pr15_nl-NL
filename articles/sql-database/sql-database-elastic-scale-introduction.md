<properties
    pageTitle="Schaal uitbreiden met Azure SQL-Database | Microsoft Azure"
    description="Software als een Service (SaaS)-ontwikkelaars eenvoudig elastische, scalable databases kan maken in de cloud met deze hulpmiddelen"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="scaling-out-with-azure-sql-database"></a>Schalen met Azure SQL-Database

U kunt eenvoudig schalen out Azure SQL-databases met de hulpmiddelen **Elastische Database** . Deze hulpprogramma's en functies kunnen u gebruik van de bronnen vrijwel onbeperkte database van **Azure SQL-Database** om oplossingen voor transacties werkbelasting en met name Software als een servicetoepassingen (SaaS) te maken. Elastische databasefuncties bestaan uit de volgende:

* [Elastische Database client-bibliotheek](sql-database-elastic-database-client-library.md): de clientbibliotheek is een functie waarmee u kunt maken en onderhouden van een laptopgeheugen databases.  Zie [aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md).
* [Elastische databasehulpmiddel voor gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md): verplaatst gegevens tussen een laptopgeheugen databases. Dit is handig voor het verplaatsen van gegevens uit een database meerdere tenant naar een enkel-tenant-database (of vice versa). Zie [elastische database splitsen samenvoegen hulpmiddel zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Elastische Database taken](sql-database-elastic-jobs-overview.md) (preview): gebruik taken voor het beheren van grote aantallen Azure SQL-databases. Eenvoudig administratieve bewerkingen zoals wijzigingen in het schema, referenties management, verwijzing Gegevensupdates, prestatiegegevens verzamelen of tenant (klant) telemetrielogboek verzamelen met behulp van taken uitvoeren.
* [Elastische databasequery 's](sql-database-elastic-query-overview.md) (preview): kunt u een Transact-SQL-query die zich uitstrekt over meerdere databases uitvoeren. Hiermee worden de verbinding met rapportagehulpmiddelen zoals Excel, PowerBI, Tableau, enzovoort.
* [Elastische transacties](sql-database-elastic-transactions-overview.md): deze functie kunt u uitvoeren van transacties die meerdere databases in Azure SQL-Database's beslaan. Elastische databasetransacties zijn beschikbaar voor .NET-toepassingen met behulp van ADO .NET en integreren met de vertrouwde programmeren met de [System.Transaction klassen](https://msdn.microsoft.com/library/system.transactions.aspx).

De onderstaande afbeelding ziet u een architectuur waarin de **elastische databasefuncties** ten opzichte van een verzameling databases.

In deze afbeelding geven de kleuren van de database schema's. Databases met dezelfde kleur delen hetzelfde schema.

1. Een reeks **Azure SQL-databases** worden gehost op Azure sharding architectuur gebruiken.
2. De **elastische Database clientbibliotheek** wordt gebruikt voor het beheren van een set shard.
3. Een subset van de databases worden in een **groep elastische Database**geplaatst. (Zie [Wat is een groep?](sql-database-elastic-pool.md)).
4. Een **taak elastische Database** wordt uitgevoerd op een geplande of ad-hoc T-SQL scripts op alle databases.
5. Het **hulpmiddel gesplitste samenvoegen** wordt gebruikt om gegevens te verplaatsen vanaf één shard naar een andere.
6. De **elastische databasequery's** kunt u een query die betrekking hebben op alle databases in de set shard schrijven.
7. **Elastische transacties** kunt u uitvoeren van transacties die meerdere databases's beslaan. 


![Elastische hulpmiddelen voor databases][1]


## <a name="why-use-the-tools"></a>Waarom de hulpmiddelen voor gebruiken?

Belangengroepen elasticiteit en schaal voor cloud-toepassingen is ingewikkelde voor VMs en blobopslag--gewoon toevoegen of aftrekken of power vergroten. Maar dit is een uitdaging voor statuscontrole gegevensverwerking in relationele databases gebleven. Uitdagingen ontstaan in deze scenario's:

* Vergroten en verkleinen capaciteit voor de relationele database-gedeelte van uw werkzaamheden.
* Het beheren van hotspots die zich kunnen voordoen dat dit gevolgen heeft een specifiek deel van gegevens, zoals een vooral bezet einde-klant (tenant).

Traditioneel zijn scenario's zoals deze gericht door te investeren in grotere schaal databaseservers ter ondersteuning van de toepassing. Deze optie is echter beperkt in de cloud waarbij alle verwerking op vooraf gedefinieerde rol hardware gebeurt. Gegevens distribueren en verwerking via veel identiek zijn gestructureerd databases bevat (een patroon schalen 'sharding' genoemd) in plaats daarvan een alternatief traditionele schaal-up methoden kosten en elasticiteit.

## <a name="horizontal-and-vertical-scaling"></a>Horizontale en verticale schaalbaarheid

De onderstaande afbeelding ziet u de horizontale en verticale afmetingen van de schaalbaarheid, waarin de eenvoudige manieren die de elastische databases kunnen worden aangepast.

![Horizontale en verticale Scaleout][2]

Horizontale schaalbaarheid verwijst naar toevoegen of verwijderen van databases om aan te passen capaciteit of algehele prestaties. Dit wordt ook 'schaalbaarheid' genoemd af. Sharding, waarin de gegevens in een verzameling identiek gestructureerde-databases, is partitioneren is een gebruikelijke manier willen implementeren horizontaal schalen.  

Verticale schaalbaarheid verwijst naar vergroten of verkleinen van de prestaties van een afzonderlijke database hoog, dit wordt ook wel 'schaalbaarheid van'.

De meeste cloud-schaal databasetoepassingen gebruikt een combinatie van deze twee strategieën. Bijvoorbeeld mogelijk een Software als een servicetoepassing horizontale schaalbaarheid om het inrichten van nieuwe einde-klanten en verticale schaal gebruikt dat elke einde-klant-database groter of kleiner resources naar wens door de werklast.

* Horizontale schaalbaarheid wordt beheerd met de [elastische Database client-bibliotheek](sql-database-elastic-database-client-library.md).

* Verticale schalen moet worden uitgevoerd Azure PowerShell-cmdlets gebruiken om te wijzigen van de servicelaag, of door te plaatsen van databases in een elastische toepassingen.

## <a name="sharding"></a>Sharding

*Sharding* is een techniek grote hoeveelheden identiek gestructureerde gegevens over een aantal onafhankelijke databases verdelen. Het is met name populaire met cloud ontwikkelaars Software als een Service (SAAS)-aanbiedingen maken voor einde klanten of bedrijven. Deze klanten einde zijn vaak 'tenants' genoemd. Het is mogelijk dat sharding vereist voor een aantal redenen:  

* De totale hoeveelheid gegevens is te groot binnen de beperkingen van één database
* De transactiedoorvoer van de algehele werklast groter is dan de mogelijkheden van één database
* Tenants mogelijk fysieke moeten worden geïsoleerd van elkaar verschillen, zodat een afzonderlijke database nodig zijn voor elke tenant
* Verschillende secties van een database moet mogelijk bevinden zich in verschillende locaties voor naleving, prestaties of geopolitieke redenen.

In andere scenario's, zoals de opname van gegevens uit verdeelde apparaten, kan sharding worden gebruikt om te vullen van een set van databases ondersteuningsbeheerder geordend. Een afzonderlijke database kan bijvoorbeeld worden toegewezen aan elke dag of week. In dat geval de sleutel sharding dient een geheel getal dat staat voor de datum (aanwezig in alle rijen van de tabellen een laptopgeheugen) en query's voor het ophalen van gegevens voor een bepaald datumbereik moeten worden doorgestuurd door de toepassing naar de deelverzameling van databases die betrekking hebben op het betreffende bereik.

Sharding werkt het best wanneer elke transactie in een toepassing beperkt tot één waarde van een sleutel sharding worden kan. Die zorgt ervoor dat alle transacties zal lokale met een bepaalde database.

## <a name="multi-tenant-and-single-tenant"></a>Meerdere tenant en één-tenant

Sommige toepassingen gebruiken de eenvoudigste manier voor het maken van een afzonderlijke database voor elke tenant. Dit is het **patroon van één tenant sharding** die moeten worden geïsoleerd, maken en terugzetten mogelijkheid en resource schaal de granulatie van de tenant biedt. Met één tenant sharding, elke database is gekoppeld aan een specifieke tenant-id-waarde (of klant-sleutelwaarde), maar die toets nodig niet altijd aanwezig zijn in de gegevens zelf. Het is van de toepassing verantwoordelijkheid voor het routeren van elk verzoek om naar de juiste database – en de clientbibliotheek kunt u dit vereenvoudigen.

![Eén tenant versus meerdere tenant][4]

Anderen scenario's, meerdere tenants samen pack in databases, in plaats van deze isoleren in afzonderlijke databases. Dit is een typisch **meerdere tenant sharding patroon** – en deze kan worden bepaald door het feit dat een toepassing grote aantallen zeer klein tenants beheert. In meerdere tenant sharding, de rijen in de databasetabellen alle ontworpen voor de uitvoering van een identificeren van de tenant-ID of sharding sleutel. Klik nogmaals de toepassingslaag is verantwoordelijk voor het verzoek van een tenant zoekresultaten omleiden naar de juiste database en dit kan worden ondersteund door de bibliotheek van de client elastische database. Beveiliging op gebruikersniveau rij kan ook worden gebruikt om te filteren welke rijen toegang elke tenant – voor meer informatie tot hebben raadpleegt u [meerdere tenant-toepassingen met hulpmiddelen voor databases elastische en beveiliging op gebruikersniveau rij](sql-database-elastic-tools-multi-tenant-row-level-security.md). Gegevens tussen databases die mogelijk zijn er nodig met het patroon dat in meerdere tenant sharding en dit wordt vergemakkelijkt door het elastische databasehulpmiddel gesplitste samenvoegen. Meer informatie over ontwerppatronen voor SaaS toepassingen elastische van toepassingen gebruiken, raadpleegt u [Ontwerppatronen voor meerdere tenant SaaS toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Verplaats gegevens van meerdere aan één pachtadres databases

Wanneer u een toepassing voor SaaS maakt, is het normale voor het aanbieden van potentiële klanten een evaluatieversie van de software. In dit geval is het efficiënt gebruik van een database meerdere tenant voor de gegevens. Wanneer een potentiële klant een klant wordt, is een database één-tenant echter betere omdat deze betere prestaties biedt. Als de klant gegevens tijdens de proefperiode gemaakt had, gebruikt u de [gesplitste samenvoegen hulpmiddel](sql-database-elastic-scale-overview-split-and-merge.md) verplaatsen van de gegevens van de meerdere tenant naar de nieuwe één-tenant-database.

## <a name="next-steps"></a>Volgende stappen

Zie [aan de slag met elastische Datababase hulpprogramma's](sql-database-elastic-scale-get-started.md)voor een voorbeeld-app die laat zien van de clientbibliotheek.

Naar bestaande databases voor de hulpmiddelen gebruiken, raadpleegt u [de bestaande databases migreren naar schalen](sql-database-elastic-convert-to-use-elastic-tools.md)converteren.

Als u wilt zien van de details van de groep elastische Database, leest u [Aandachtspunten voor de prijs en prestaties voor een toepassingen elastische database](sql-database-elastic-pool-guidance.md)of een nieuwe groep maken met de [zelfstudie](sql-database-elastic-pool-create-portal.md).  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

