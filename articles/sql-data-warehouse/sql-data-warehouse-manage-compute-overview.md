<properties
   pageTitle="Beheren rekenkracht in Azure SQL Data Warehouse (overzicht) | Microsoft Azure"
   description="De schaal van de prestaties meer mogelijkheden in Azure SQL Data Warehouse. Met aanpassen DWUs wilt verkleinen of onderbreken en hervatten berekeningscluster resources om op te slaan kosten."
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
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Beheren rekenkracht in Azure SQL Data Warehouse (overzicht)

> [AZURE.SELECTOR]
- [Overzicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

De architectuur van SQL Data Warehouse worden gescheiden door opslagruimte en berekeningscluster, zodat elk afzonderlijk schaal. Hierdoor kunt u de schaal prestaties bij het opslaan van kosten alleen betaalt prestaties wanneer u deze nodig hebt. 

Dit overzicht wordt beschreven van de volgende mogelijkheden van de schalen prestaties van SQL Data Warehouse en aanbevelingen over hoe en wanneer u ze kunt gebruiken. 

- Schaal rekenkracht door aan te passen [datawarehouse eenheden (DWUs)][]
- Een pauze invoegen of cv berekeningscluster resources

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Prestaties van de schaal

In SQL Data Warehouse, kunt u snel de prestaties af of terug door oplopende of aflopende volgorde berekeningscluster bronnen van processor en geheugen o bandbreedte schaal. Als prestaties wilt verkleinen, enige u moet doen, is het aantal [eenheden (DWUs) datawarehouse][] die SQL Data Warehouse worden toegewezen aan uw database aanpassen. SQL Data Warehouse snel zorgt ervoor dat de wijziging en alle onderliggende wijzigingen in hardware en software verwerkt.

De dagen zijn voorbij waar moet u welk type processors onderzoeken, hoeveel geheugen of welk type opslag moet u beschikken over goede prestaties in uw BTW-records voor de gegevens. Door te plaatsen uw Data Warehouse in de cloud, hoeft u niet langer te handelen op laag niveau hardwareproblemen. In plaats daarvan SQL Data Warehouse wordt u gevraagd deze vraag: hoe snel wilt u uw gegevens analyseren? 

### <a name="how-do-i-scale-performance"></a>Hoe ik de prestaties schalen?

Als u wilt elastically vergroten of verkleinen van uw rekenkracht, moet u gewoon de instelling [datawarehouse eenheden (DWUs)][] voor de database wijzigen. Prestaties, lineair neemt toe naarmate u meer DWU toevoegt.  Op een hoger niveau DWU, die u wilt toevoegen van meer dan 100 DWUs om zoals u ziet, aanmerkelijk prestaties te verbeteren. Als u betekenisvolle sprongen selecteert in DWUs, bieden we DWU niveaus die de beste resultaten krijgt.
 
U kunt elk van de afzonderlijke methoden gebruiken om aan te passen DWUs.

- [Schaal rekenkracht met Azure-portal][]
- [Schaal rekenkracht met PowerShell][]
- [Rekenkracht schaal met REST API 's][]
- [Schaal rekenkracht met TSQL][]

### <a name="how-many-dwus-should-i-use"></a>Hoeveel DWUs moet ik gebruiken?
 
Prestaties in SQL Data Warehouse lineair wordt aangepast en wijzigen van een berekeningscluster schaal naar een andere (bijvoorbeeld van 100 DWUs naar 2000 DWUs) gebeurt er in seconden. Hiermee kunt u de flexibiliteit om te experimenteren met verschillende DWU instellingen totdat u de meest geschikte van uw scenario bepalen.

Om te begrijpen wat uw ideaal DWU waarde is, probeer schalen omhoog en omlaag en een aantal query's uitvoeren na het laden van uw gegevens. Aangezien schaalbaarheid snelle is, kunt u een aantal verschillende niveaus van prestaties in een uur of minder proberen. Houd er rekening mee dat SQL Data Warehouse is bedoeld om het verwerken van grote hoeveelheden gegevens en om te zien van de werkelijke capaciteit voor schaalbaarheid, met name bij de grotere schaal die we bieden, wilt u een grote gegevensverzameling die nadert of groter is dan 1 TB gebruiken.

Aanbevelingen voor het vinden van de beste DWU voor uw werkzaamheden:

1. Voor een data-warehouse in de ontwikkelingsfase bevindt, gaat u eerst een klein aantal DWUs selecteren.  Een goed beginpunt is DW400 of DW200.
2. De toepassingsprestaties van uw controleren, naleving van dat het aantal DWUs geselecteerd vergeleken met de prestaties die u toekijken.
3. Hiermee bepaalt u hoeveel sneller of langzamer prestaties moet u het niveau van de optimale prestaties voor uw vereisten wordt ervan uitgegaan lineaire schaal hebt bereikt.
4. Verhoog of verlaag het aantal DWUs in verhouding tot hoe veel sneller of langzamer u wilt dat uw werkzaamheden om uit te voeren. De service wordt snel reageren en de resources berekeningscluster om te voldoen aan nieuwe DWU aanpassen.
5. Ga verder correcties totdat u het niveau van een optimale prestaties voor uw zakelijke behoeften hebt bereikt.

### <a name="when-should-i-scale-dwus"></a>Wanneer moet ik DWUs schalen?

Wanneer u sneller resultaten nodig hebt, wordt uw DWUs vergroten en betalen voor betere prestaties.  Wanneer u hoeft minder rekenkracht, uw DWUs verkleinen en betalen alleen voor wat u nodig hebt. 

Aanbevelingen voor wanneer u de schaal van DWUs aanpassen:

1. Als uw toepassing een wisselende werkbelasting, schaal DWU worden geëffend bevat omhoog of omlaag naar geschikt voor pieken en lage punten. Bijvoorbeeld: als uw werkzaamheden waar meestal de pieken aan het einde van de maand, wilt meer DWUs tijdens deze Piekdagen toevoegen en klik vervolgens verkleinen zodra de piekperiode verstreken is.
2. Voordat u een veel gegevens laden of -transformatie-bewerking uitvoeren, vergroten DWUs zodat uw gegevens sneller beschikbaar is.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Een pauze invoegen berekenen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Een database onderbreken, gebruikt u elk van de afzonderlijke methoden.

- [Een pauze invoegen berekenen met Azure-portal][]
- [Een pauze invoegen berekenen met PowerShell][]
- [Een pauze invoegen berekenen met REST API 's][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Cv berekenen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Als u wilt doorgaan met een database, gebruikt u een van deze afzonderlijke methoden.

- [Cv berekenen met Azure-portal][]
- [Cv berekenen met PowerShell][]
- [Cv berekenen met REST API 's][]

## <a name="permissions"></a>Machtigingen

Schaalbaarheid van de database moeten worden de machtigingen beschreven in de [DATABASE wijzigen][].  Onderbreken en hervatten moet de machtiging [SQL DB Inzender][] , specifiek Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen kunt u enkele extra prestatie basisbegrippen:

- [Werkbelasting en gelijktijdigheid beheren][]
- [Overzicht van de tabel ontwerpen][]
- [Tabel-verdeling][]
- [Tabel indexeren][]
- [Tabel partitioneren][]
- [Tabelstatistieken][]
- [Aanbevolen procedures][]

<!--Image reference-->

<!--Article references-->
[data warehouse eenheden (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Schaal rekenkracht met Azure-portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Schaal rekenkracht met PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Rekenkracht schaal met REST API 's]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Schaal rekenkracht met TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Een pauze invoegen berekenen met Azure-portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Een pauze invoegen berekenen met PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Een pauze invoegen berekenen met REST API 's]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Cv berekenen met Azure-portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Cv berekenen met PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Cv berekenen met REST API 's]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Werkbelasting en gelijktijdigheid beheren]: ./sql-data-warehouse-develop-concurrency.md
[Overzicht van de tabel ontwerpen]: ./sql-data-warehouse-tables-overview.md
[Tabel-verdeling]: ./sql-data-warehouse-tables-distribute.md
[Tabel indexeren]: ./sql-data-warehouse-tables-index.md
[Tabel partitioneren]: ./sql-data-warehouse-tables-partition.md
[Tabelstatistieken]: ./sql-data-warehouse-tables-statistics.md
[Aanbevolen procedures]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Inzender]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[DATABASE WIJZIGEN]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
