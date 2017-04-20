<properties
    pageTitle="Azure SQL-Database elastische database-queryoverzicht | Microsoft Azure"
    description="Overzicht van de functie elastische query"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/27/2016"
    ms.author="torsteng" />

# <a name="azure-sql-database-elastic-database-query-overview-preview"></a>Azure SQL-Database elastische database-queryoverzicht (preview)

De functie van de query elastische database (in preview) kunt u een Transact-SQL-query die zich uitstrekt over meerdere databases in Azure SQL Database (SQLDB) uit te voeren. Dit kunt u meerdere databases query's voor toegang tot externe tabellen en om verbinding maken met Microsoft en derden hulpprogramma's (Excel, PowerBI, Tableau, enzovoort) aan een query met betrekking tot gegevens lagen met meerdere databases kan uitvoeren. Deze functie gebruikt, kunt u schaal query's in grote gegevens niveaus in SQL-Database en de resultaten in business intelligence (BI) rapporten visualiseren.

## <a name="documentation"></a>Documentatie

* [Aan de slag met cross-databasequery 's](sql-database-elastic-query-getting-started-vertical.md)
* [Rapport over schaal-out cloud-databases](sql-database-elastic-query-getting-started.md)
* [Query met betrekking tot een laptopgeheugen cloud-databases (horizontaal partitioneren)](sql-database-elastic-query-horizontal-partitioning.md)
* [Query met betrekking tot cloud-databases met verschillende schema's (verticaal partities)](sql-database-elastic-query-vertical-partitioning.md)
* [SP\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714)


## <a name="why-use-elastic-queries"></a>Het nut van elastische query's?

**Azure SQL-Database**

Query met betrekking tot Azure SQL-databases volledig in T-SQL. Hiermee kunnen alleen-lezen query's uitvoeren van externe databases. Dit is een optie voor huidige on-premises implementatie SQL Server-klanten toepassingen met drie en vierkleuren part namen of gekoppelde server naar SQL DB migreren.

**Beschikbaar op standaard laag** Elastische query wordt ondersteund op de laag standaard prestaties naast de Premium prestaties laag. Zie het gedeelte van de Preview-beperkingen onder van de prestaties beperkingen voor lagere niveaus van de prestaties.

**Push met externe databases**

Elastische query's kunnen SQL-parameters nu push met de externe databases voor uitvoering.

**Opgeslagen procedure worden uitgevoerd**

Uitvoeren van oproepen voor externe opgeslagen procedure of externe functies met [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714).

**Flexibiliteit**

Externe tabellen met elastische query kunnen nu verwijzen naar externe tabellen met een ander schema of de naam van de tabel.

## <a name="elastic-database-query-scenarios"></a>Elastische database query scenario 's

Het doel is om te vergemakkelijken het opvragen scenario's wanneer meerdere databases rijen in een enkel resultaat van de algehele bijdragen. De query kan ofwel bestaan door de gebruiker of een toepassing rechtstreeks of onrechtstreeks via hulpmiddelen die verbonden zijn met de database. Dit is vooral handig bij het maken van rapporten, met behulp van de opties voor commerciële gegevens of BI-integratie, of een toepassing die niet worden gewijzigd. Met een elastische query, kunt u query met betrekking tot meerdere databases die gebruikmaken van de vertrouwde ervaring voor SQL Server-connectiviteit in hulpprogramma's zoals Excel, PowerBI, Tableau of Hiermee.
Een elastische query kunt eenvoudig toegang tot een hele verzameling databases through-query's uitgegeven door SQL Server Management Studio of Visual Studio en vergemakkelijkt meerdere databases query's uitvoeren vanuit entiteit Framework of andere ORM-omgevingen. Afbeelding 1 ziet u een scenario waarbij een bestaande cloud-toepassing (die gebruikmaakt van de [elastische database clientbibliotheek](sql-database-elastic-database-client-library.md)) is gebaseerd op een laag schaal uitbreiden en een elastische query wordt gebruikt voor het melden van meerdere databases.

**Afbeelding 1** Elastische databasequery's die worden gebruikt op schaal-out van gegevens

![Elastische databasequery's die worden gebruikt op schaal-out van gegevens][1]

Klant-scenario's voor elastische query worden gekenmerkt door de volgende topologieën:

* **Verticale partitioneren – Cross-databasequery 's** (Topologie 1): de gegevens is verticaal partities tussen een aantal databases in een gegevenslaag. Verschillende groepen van tabellen bevinden zich gewoonlijk, op verschillende databases. Dat betekent dat het schema verschillende gegevens weergeven op verschillende databases. Alle tabellen voor voorraad zijn bijvoorbeeld op één database terwijl u alle financieel-gerelateerde tabellen op een tweede database. Veelvoorkomende gebruik gevallen met deze topologie vragen om een query met betrekking tot of compilatie-rapporten op tabellen in verschillende databases.
* **Horizontale partitioneren – Sharding** (Topologie 2): gegevens is horizontaal partitioneren om rijen verdelen over een scaled out gegevens laag. Met deze methode is het schema identiek zijn op alle deelnemende databases. Deze methode wordt ook 'sharding' genoemd. Sharding kan worden uitgevoerd en beheerde gebruiken (1) de elastische database hulpmiddelen voor bibliotheken of (2) self-sharding. Een elastische query wordt gebruikt voor de query of rapporten over veel shards compileren.

> [AZURE.NOTE] Elastische databasequery's werkt het best voor incidentele rapportage scenario's waarin de meeste van de verwerking kan worden uitgevoerd op de gegevenslaag. Voor veel rapportage werkbelasting of gegevens magazijnbeheer scenario's met meer complexe query's, ook rekening houden met behulp van [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).


## <a name="elastic-database-query-topologies"></a>Elastische topologieën voor query

### <a name="topology-1-vertical-partitioning--cross-database-queries"></a>Topologie 1: Verticale partities – meerdere databases query's

Zie [aan de slag met cross-databasequery's (verticale partities)](sql-database-elastic-query-getting-started-vertical.md)wilt beginnen kleurcodering aan.

Een elastische query kan worden gebruikt om gegevens zich in een database SQLDB voor andere databases SQLDB te maken. Hiermee kunt query's vanuit één database te verwijzen naar tabellen in een andere externe SQLDB-database. De eerste stap is het definiëren van een externe gegevensbron voor elke externe database. De externe gegevensbron is gedefinieerd in de lokale database waaruit u wilt toegang te krijgen tot de tabellen die zich bevindt op de externe database. Geen wijzigingen zijn nodig op de externe database. Voor typische verticale partities scenario's waarin verschillende databases verschillende schema's hebt, kunnen elastische query's worden gebruikt om te implementeren algemene gebruik zaken zoals toegang tot referentiegegevens en meerdere databases query's uitvoeren.

**Referentiegegevens**: topologie 1 wordt gebruikt voor het beheren van gegevens voor verwijzing. In de onderstaande afbeelding twee tabellen (t 1 en t 2) met referentiegegevens bewaard op een speciale database. Een elastische query gebruikt, u kunt nu toegang tot tabellen t 1 en t 2 extern uit andere databases, zoals wordt weergegeven in de afbeelding. Gebruik topologie 1 als verwijzingstabellen kleine of externe query's in de tabel hebt selectief predicaten.

**Afbeelding 2** Verticale partitioneren - elastische referentiegegevens van query-query gebruiken

![Verticale partitioneren - elastische referentiegegevens van query-query gebruiken][3]

**Meerdere databases query's uitvoeren**: elastische query's inschakelen gebruik gevallen waarvoor query's uitvoeren in verschillende SQLDB-databases. Afbeelding 3 ziet u vier verschillende databases: CRM, voorraad, HR en producten. Query's die worden uitgevoerd in een van de databases moeten ook toegang tot één of alle de andere databases. Een elastische query gebruikt, kunt u uw database voor deze zaak configureren met behulp van een paar eenvoudige DDL-instructies op elk van de vier databases. Na deze eenmalige configuratie is de toegang tot een externe tabel net zo eenvoudig als het verwijzen naar een lokale tabel vanuit uw T-SQL-query's of uw BI-functies. Deze methode wordt aanbevolen als de externe query's geen grote resultaten leveren.

**Afbeelding 3** Verticale partitioneren - elastische query-query gebruiken voor verschillende databases

![Verticale partitioneren - elastische query-query gebruiken voor verschillende databases][4]

### <a name="topology-2-horizontal-partitioning--sharding"></a>Topologie 2: Horizontaal partitioneren – sharding

Van gegevens met behulp van elastische query uitvoeren van taken van de rapportage via een een laptopgeheugen, dat wil zeggen, horizontaal partitioneren, is een [elastische database shard kaart](sql-database-elastic-scale-shard-map-management.md) om aan te geven van de databases van de gegevenslaag vereist. Meestal alleen een enkele shard-kaart in dit scenario wordt gebruikt en een speciale database maken met elastische querymogelijkheden fungeert als het ingangspunt voor het melden van query's. Alleen deze speciale database toegang tot de kaart shard nodig heeft. Afbeelding 4 ziet u deze topologie en de configuratie terwijl de kaart is database en shard elastische query. De databases in de gegevenslaag kunnen zijn met een bepaalde versie van Azure SQL-Database of edition. Zie voor meer informatie over de bibliotheek van de client elastische database en het maken van shard kaarten [Shard management toewijzen](sql-database-elastic-scale-shard-map-management.md).

**Afbeelding 4** Horizontale partitioneren - elastische query gebruiken voor het melden van via een laptopgeheugen gegevens lagen

![Horizontale partitioneren - elastische query gebruiken voor het melden van via een laptopgeheugen gegevens lagen][5]

> [AZURE.NOTE] De database van de query speciale elastische database moet een database van SQL DB v12. Er zijn geen beperkingen van de shards zelf.

Zie [aan de slag met elastische databasequery's voor horizontaal partitioneren (sharding)](sql-database-elastic-query-getting-started.md)wilt beginnen kleurcodering aan.

## <a name="implementing-elastic-database-queries"></a>Elastische databasequery's implementeren

De stappen voor het implementeren van elastische query voor de verticale en horizontale partities scenario's worden in de volgende secties besproken. Ze ook verwijzen naar meer gedetailleerde documentatie voor de verschillende partities scenario's.

### <a name="vertical-partitioning---cross-database-queries"></a>Verticale partities - query's cross-database

De volgende stappen configureren elastische databasequery's voor verticale partities scenario's waarvoor toegang tot een tabel die zich op externe SQLDB databases met hetzelfde schema:

*    Mymasterkey [OUTMODEL sleutel maken](https://msdn.microsoft.com/library/ms174382.aspx)
*    [Referentie voor DATABASE beperkt maken](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    [De externe gegevensbron maken/DECORATIEVE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource van het type **RDBMS**
*    [Externe tabel maken/DECORATIEVE](https://msdn.microsoft.com/library/dn935021.aspx) MijnTabel

Na het uitvoeren van de DDL-instructies, kunt u de tabel met externe "MijnTabel" openen alsof het een lokale tabel. Azure SQL-Database automatisch wordt geopend een verbinding met de externe database, verwerkt uw aanvraag op de externe database en geeft als resultaat de resultaten.
Meer informatie over de stappen die zijn vereist voor de verticale partities scenario vindt u in [elastische query voor verticale partities](sql-database-elastic-query-vertical-partitioning.md).  

### <a name="horizontal-partitioning---sharding"></a>Horizontaal partitioneren - sharding

De volgende stappen configureren elastische databasequery's voor horizontale partities scenario's waarvoor toegang tot een set van de tabel die zich bevinden op (meestal) verschillende externe SQLDB-databases:

*    Mymasterkey [OUTMODEL sleutel maken](https://msdn.microsoft.com/library/ms174382.aspx)
*    [Referentie voor DATABASE beperkt maken](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    Maak een [shard kaart](sql-database-elastic-scale-shard-map-management.md) dat staat voor de gegevenslaag van uw met behulp van de bibliotheek van de client elastische database.   
*    [De externe gegevensbron maken/DECORATIEVE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource van het type **SHARD_MAP_MANAGER**
*    [Externe tabel maken/DECORATIEVE](https://msdn.microsoft.com/library/dn935021.aspx) MijnTabel

Nadat u deze stappen hebt uitgevoerd, kunt u de horizontaal gepartitioneerde tabel "MijnTabel" kunt openen, alsof het een lokale tabel. Azure SQL-Database automatisch geopend meerdere parallelle verbindingen met de externe databases waar de tabellen fysiek worden opgeslagen, de aanvragen voor de externe databases worden verwerkt, en geeft als resultaat de resultaten.
Meer informatie over de stappen die zijn vereist voor de horizontale partities scenario vindt u in [elastische query voor horizontaal partitioneren](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="t-sql-querying"></a>T-SQL-query's uitvoeren
Als u uw externe gegevensbronnen en uw externe tabellen hebt gedefinieerd, kunt u gewone tekenreeksen voor SQL Server verbinding maken met de databases waar u uw externe tabellen hebt gedefinieerd. U kunt vervolgens T-SQL-instructies via externe tabellen uitvoeren op die verbinding met de onderstaande beperkingen. U vindt meer informatie en voorbeelden van de T-SQL-query's in de documentatie-onderwerpen voor [partitioneren van horizontale](sql-database-elastic-query-horizontal-partitioning.md) en [Verticale partities](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Connectiviteit voor hulpmiddelen voor
U kunt gewone tekenreeksen voor SQL Server verbinding te maken van uw toepassingen en hulpmiddelen voor BI of gegevens integratie met databases met externe tabellen. Zorg ervoor dat de SQL Server als gegevensbron voor een hulpmiddel in uw wordt ondersteund. Zodra u verbinding hebt, raadpleegt u de querydatabase elastische en de externe tabellen in die database net zoals u zou doen met elke andere SQL Server-database waarmee u verbinding met uw hulpmiddel maakt.

> [AZURE.IMPORTANT] Verificatie met behulp van Azure Active Directory met elastische query's wordt momenteel niet ondersteund.

## <a name="cost"></a>Kosten

Elastische query is opgenomen in de kosten van Azure SQL Database-databases. Houd er rekening mee dat topologieën waar uw externe databases zich in een ander datacenter dan het eindpunt elastische query bevinden worden ondersteund, maar egress gegevens uit externe databases normale [Azure tarieven](https://azure.microsoft.com/pricing/details/data-transfers/)in rekening worden gebracht.

## <a name="preview-limitations"></a>Beperkingen voor de Preview-versie
* Uw eerste elastische query wordt uitgevoerd kan een paar minuten duren op de laag standaard prestaties. Deze tijd is nodig om te laden van de queryfunctionaliteit voor elastische; met hogere prestaties lagen verbetert laden prestaties.
* Uitvoeren van scripts van externe gegevensbronnen of externe tabellen uit SSMS of SSDT wordt nog niet ondersteund.
* Importeren/exporteren voor SQL DB ondersteunt nog geen externe gegevensbronnen en externe tabellen. Als u importeren/exporteren gebruiken wilt, Sleep van deze objecten voordat u het exporteert en vervolgens opnieuw maken nadat u hebt geïmporteerd.
* Elastische databasequery's ondersteunt momenteel alleen-lezen toegang tot externe tabellen. U kunt volledige functionaliteit van de T-SQL echter gebruiken voor de database waarin de externe tabel is gedefinieerd. Dit is nuttig, bijvoorbeeld tijdelijke resultaten gebruikt, bijvoorbeeld aanhouden, selecteert u < column_list > in < local_table >, of voor opgeslagen procedures definiëren op de elastische querydatabase die verwijzen naar externe tabellen.
* Behalve nvarchar(max), worden LOB typen niet ondersteund in externe tabeldefinities. Als een tijdelijke oplossing kunt u een weergave op de externe database die het type LOB in nvarchar(max) cast maken, definiëren de externe tabel via de weergave in plaats van de basistabel en cast deze weer in het oorspronkelijke LOB-type in uw query's.
* Kolom statistieken over externe tabellen worden momenteel niet ondersteund. Tabellen statistieken worden ondersteund, maar moeten handmatig worden gemaakt.

## <a name="feedback"></a>Feedback
Deel feedback over uw ervaring met elastische query's met ons op Disqus hieronder de MSDN-forums, of op Stackoverflow. We zijn geïnteresseerd in alle soorten feedback over de service (afwijkingen, ruw randen, functie voor hiaten).

## <a name="more-information"></a>Meer informatie

U vindt meer informatie over de meerdere databases query- en verticale partities scenario's in de volgende documenten:

* [Meerdere databases query's uitvoeren en verticale partities overzicht](sql-database-elastic-query-vertical-partitioning.md)
* Probeer onze stapsgewijze zelfstudie om een volledige werken voorbeeld uitgevoerd in minuten: [aan de slag met cross-databasequery's (verticale partities)](sql-database-elastic-query-getting-started-vertical.md).


Meer informatie over horizontale partitioneren en sharding-scenario's is hier beschikbaar:

* [Horizontale partitioneren en sharding overzicht](sql-database-elastic-query-horizontal-partitioning.md)
* Probeer onze stapsgewijze zelfstudie om een volledige werken voorbeeld uitgevoerd in minuten: [aan de slag met elastische databasequery's voor horizontaal partitioneren (sharding)](sql-database-elastic-query-getting-started.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
