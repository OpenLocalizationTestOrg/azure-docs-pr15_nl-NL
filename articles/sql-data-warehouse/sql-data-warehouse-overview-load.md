   <properties
   pageTitle="Gegevens laadt in Azure SQL Data Warehouse | Microsoft Azure"
   description="Informatie over de gebruikelijke scenario's voor gegevens laden in SQL Data Warehouse. Hierbij met PolyBase, Azure-blobopslag, bestanden en schijf verzending. U kunt ook hulpprogramma's van derden gebruiken."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/12/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-into-azure-sql-data-warehouse"></a>Gegevens laadt in Azure SQL Data Warehouse

Een overzicht van de opties voor scenario en aanbevelingen voor het laden van gegevens in SQL Data Warehouse.

Het moeilijkst deel van het laden van gegevens is meestal met het voorbereiden van de gegevens voor het selectievakje laden. Azure eenvoudiger laden met Azure-blobopslag als een algemene gegevensopslag voor veel van de services en Azure gegevens Factory goedkeuringen communicatie en gegevens verplaatsing tussen de Azure services gebruiken. Deze processen worden geïntegreerd met PolyBase-technologie die gegevens parallel van Azure-blobopslag laadt in SQL Data Warehouse met behulp van massively parallelle verwerking (MPP). 

Zie de [steekproef databases laden][]voor zelfstudies met laden van de steekproef-databases.

## <a name="load-from-azure-blob-storage"></a>Laden van Azure-blobopslag
De snelste manier om gegevens te importeren in SQL Data Warehouse is PolyBase gebruiken om gegevens te laden van Azure-blobopslag. SQL Data Warehouse massively parallelle verwerking (MPP) ontwerp PolyBase gebruikt om gegevens te laden parallel van Azure-blobopslag. Als u wilt gebruiken PolyBase, kunt u T-SQL-opdrachten of een verkooppijplijn Factory van Azure-gegevens.

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase en T-SQL gebruiken

Overzicht van het laden van proces:

2. Maak uw gegevens als UTF-8 aangezien PolyBase worden momenteel niet ondersteund voor UTF-16.
2. Uw gegevens verplaatsen naar Azure-blobopslag en sla deze op tekstbestanden.
3. Externe objecten in SQL Data Warehouse definiëren van de locatie en de opmaak van de gegevens configureren
4. Een T-SQL-opdracht de gegevens parallel laden in een nieuwe databasetabel uitvoeren.

<!-- 5. Schedule and run a loading job. --> 

Zie voor een zelfstudie [laden gegevens van Azure-blobopslag naar SQL Data Warehouse (PolyBase)][].

### <a name="2-use-azure-data-factory"></a>2. Azure gegevens Factory gebruiken

Voor een eenvoudigere manier PolyBase gebruiken, kunt u een Azure gegevens Factory-verkooppijplijn waarin PolyBase voor het laden van gegevens van Azure-blobopslag in SQL Data Warehouse wordt gebruikt. Dit is snel configureren omdat u niet nodig hebt om de T-SQL-objecten te definiëren. Als u nodig hebt om query's in de externe gegevens zonder het te importeren, gebruikt u de T-SQL. 

Overzicht van het laden van proces:

2. Maak uw gegevens als UTF-8 aangezien PolyBase worden momenteel niet ondersteund voor UTF-16.
2. Uw gegevens verplaatsen naar Azure-blobopslag en sla deze op tekstbestanden.
3. Maak een pijplijn Azure gegevens Factory nemen van de gegevens. Gebruik de optie PolyBase.
4. Plannen en uitvoeren van de pijplijn.

Zie voor een zelfstudie [laden gegevens van Azure-blobopslag naar SQL Data Warehouse (Azure Data Factory genoemd)][].


## <a name="load-from-sql-server"></a>Laden uit SQL Server
U kunt Integration Services (SSIS), doorverbinden platte bestanden of schip schijven naar Microsoft gebruiken om gegevens te laden uit SQL Server naar SQL Data Warehouse. Alleen bij Zie een samenvatting van de verschillende processen en koppelingen naar zelfstudies laden.

Als u wilt een migratie volledige gegevens uit SQL Server naar SQL Data Warehouse van plan bent, raadpleegt u het [overzicht van de migratie][]. 

### <a name="use-integration-services-ssis"></a>Gebruik integratieservices (SSIS)
Als u al Integration Services (SSIS)-pakketten laden in SQL Server gebruikt, kunt u uw pakketten SQL Server als de bron- en SQL Data Warehouse gebruiken als de bestemming bijwerken. Dit is snel en eenvoudig te doen, en is een goede keuze als u niet migreren van uw proces laden wilt zodat u al gegevens gebruiken in de cloud. De verhouding is dat het selectievakje laden zijn lager dan PolyBase omdat deze SSIS geen het selectievakje laden parallel voert.

Overzicht van het laden van proces:

1. Wijzig uw Integration Services-pakket te laten verwijzen naar de SQL Server-instantie voor de bron en de SQL Data Warehouse-database voor de bestemming.
2. Migreren naar SQL Data Warehouse, uw schema als deze nog niet weergegeven.
3. Verander de mapping in uw pakketten gebruik alleen de gegevenstypen die worden ondersteund door SQL Data Warehouse.
3. Plannen en uitvoeren van het pakket.

Zie voor een zelfstudie [laden gegevens uit SQL Server naar Azure SQL Data Warehouse (SSIS)][].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Gebruik AZCopy (aanbevolen voor < 10 TB gegevens)
Als de gegevensgrootte van uw < 10 TB, kunt u de gegevens uit SQL Server naar platte bestanden exporteren, Kopieer de bestanden naar Azure-blobopslag en gebruik vervolgens PolyBase naar de gegevens laadt in SQL Data Warehouse

Overzicht van het laden van proces:

1. Gebruikt u het hulpprogramma bcp exporteren van gegevens uit SQL Server naar platte bestanden.
2. Gebruikt u het hulpprogramma AZCopy om gegevens te kopiëren van alle bestanden in met Azure-blobopslag.
3. Gebruik PolyBase in SQL Data Warehouse laden.

Zie voor een zelfstudie [laden gegevens van Azure-blobopslag naar SQL Data Warehouse (PolyBase)][].

### <a name="use-bcp"></a>Bcp gebruiken
Als er een kleine hoeveelheid gegevens kunt u bcp rechtstreeks in Azure SQL Data Warehouse laden.

Overzicht van het laden van proces:
1. Gebruikt u het hulpprogramma bcp exporteren van gegevens uit SQL Server naar platte bestanden.
2. Gebruik bcp gegevens laden vanuit platte bestanden rechtstreeks naar SQL Data Warehouse.

Zie voor een zelfstudie [laden gegevens uit SQL Server Azure SQL Data Warehouse (bcp)][].


### <a name="use-importexport-recommended-for--10-tb-data"></a>Gebruik importeren/exporteren (aanbevolen voor > 10 TB gegevens)
Als de gegevensgrootte van uw > 10 TB is en u wilt verplaatsen naar Azure, is het raadzaam dat u gebruikt onze schijf verzend- [Importeren/exporteren][]. 

Overzicht van het proces laden
2. Gebruikt u het hulpprogramma bcp exporteren van gegevens uit SQL Server naar platte bestanden op overdraagbare schijven.
3. De schijven naar Microsoft verzenden.
4. Microsoft laadt de gegevens in SQL Data Warehouse

## <a name="load-from-hdinsight"></a>Laden uit HDInsight
SQL Data Warehouse ondersteunt laden van gegevens uit HDInsight via PolyBase. Het proces is hetzelfde als de gegevens laden van Azure-blobopslag - gebruiken PolyBase om verbinding te HDInsight om gegevens te laden. 

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase en T-SQL gebruiken

Overzicht van het laden van proces:

2. Maak uw gegevens als UTF-8 aangezien PolyBase worden momenteel niet ondersteund voor UTF-16.
2. Uw gegevens verplaatsen naar HDInsight en sla deze op tekstbestanden, ORC of parketvloeren-indeling.
3. Externe objecten in SQL Data Warehouse definiëren van de locatie en de opmaak van de gegevens configureren.
4. Een T-SQL-opdracht de gegevens parallel laden in een nieuwe databasetabel uitvoeren.

Zie voor een zelfstudie [laden gegevens van Azure-blobopslag naar SQL Data Warehouse (PolyBase)][].

## <a name="recommendations"></a>Aanbevelingen

Veel van onze partners hebt geladen oplossingen. Meer informatie, een overzicht van onze [oplossingspartners][]uit te voeren. 

Als uw gegevens is die afkomstig zijn uit een niet-relationele bron en u laden in SQL Data Warehouse moet u deze in rijen en kolommen transformeren wilt voordat u deze hebt geladen. De getransformeerd gegevens geen hoeft te worden opgeslagen in een database, deze kan worden opgeslagen in tekstbestanden.

Statistieken maken voor nieuwe geladen gegevens. Azure SQL Data Warehouse biedt nog geen ondersteuning auto maken of bijwerken statistieken jaarlijks automatisch.  Om te krijgen de beste prestaties van uw query's, is het belangrijk om de statistieken maken voor alle kolommen van alle tabellen na de eerste laden of belangrijke wijzigingen optreden in de gegevens.  Zie voor meer informatie, [Statistieken][].


## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor de ontwikkeling, het [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[Gegevens van Azure-blobopslag laden naar SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Gegevens van Azure-blobopslag laden naar SQL Data Warehouse (Azure Data Factory genoemd)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Gegevens laden uit SQL Server naar Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Gegevens uit SQL Server laden naar Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Voorbeelddatabases laden]: ./sql-data-warehouse-load-sample-databases.md
[Migratieoverzicht]: ./sql-data-warehouse-overview-migrate.md
[oplossingspartners]: ./sql-data-warehouse-partner-business-intelligence.md
[ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Importeren/exporteren]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
