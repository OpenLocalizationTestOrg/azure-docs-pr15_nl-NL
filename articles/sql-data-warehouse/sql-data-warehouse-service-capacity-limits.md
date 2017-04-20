<properties
   pageTitle="SQL Data Warehouse Capaciteitslimieten | Microsoft Azure"
   description="Maximumwaarden voor verbindingen, databases, tabellen en query's voor SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="sql-data-warehouse-capacity-limits"></a>SQL Data Warehouse Capaciteitslimieten

De volgende tabellen bevatten de maximumwaarden toegestaan voor verschillende onderdelen van Azure SQL Data Warehouse.


## <a name="workload-management"></a>Beheer van de werklast

| Categorie            | Beschrijving                                       | Maximum            |
| :------------------ | :------------------------------------------------ | :----------------- |
| [Data Warehouse eenheden (DWU)][]| Max DWU voor een enkele SQL Data Warehouse | 6000               |
| [Data Warehouse eenheden (DWU)][]| Max DWU voor één SQL server         | 6000 al dan niet standaard<br/><br/> Standaard bevat elk SQL-server (bijvoorbeeld myserver.database.windows.net) een DTU quotum van 45,000 waarmee maximaal 6000 DWU. Dit quotum is alleen een limiet veiligheid. U kunt de quota voor uw door te [maken van een ondersteuningsticket][] en *Quotum* selecteren als het aanvraagtype verhogen.  Voor het berekenen van uw DTU moeten, vermenigvuldigt u het 7.5 hebt met het totale dat DWU nodig. U kunt uw huidige DTU verbruik van het SQL server-blad weergeven in de portal. Onderbroken en voortgezet databases tellen naar het quotum voor DTU. |
| Database-verbinding | Gelijktijdige geopende sessies                          | 1024<br/><br/>Microsoft ondersteuning voor maximaal 1024 actieve verbindingen, die elk aanvragen met een SQL Data Warehouse-database op hetzelfde moment kan worden verzonden. Houd er rekening mee dat er beperkingen ten aanzien van het aantal query's die daadwerkelijk gelijktijdig kan worden uitgevoerd. Wanneer de gelijktijdigheid limiet is overschreden, wordt het verzoek gaan in een interne wachtrij waar deze wacht moeten worden verwerkt.|
| Database-verbinding | Maximum geheugen voor voorbereide instructies            | 20 MB              |
| [Beheer van de werklast][] | Maximumaantal gelijktijdige query 's                    | 32<br/><br/> Standaard kunt SQL Data Warehouse maximaal 32 gelijktijdige query's en wachtrijen resterende van query's uitvoeren.<br/><br/>Het niveau van de gelijktijdigheid mogelijk verkleinen wanneer gebruikers zijn toegewezen aan een hogere resource klasse of wanneer SQL Data Warehouse is geconfigureerd met lage DWU. Sommige query's, zoals DMV query's zijn altijd toegestaan om uit te voeren.|
| [TempDB][]          | Maximale grootte van Tempdb                                | 399 GB per DW100. Daarom bij DWU1000 Tempdb is waarvan het formaat 3,99 TB |


## <a name="database-objects"></a>Databaseobjecten

| Categorie          | Beschrijving                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Database          | Maximumgrootte                                     | 240 TB gecomprimeerd op schijf<br/><br/>Deze ruimte onafhankelijke tempdb of log ruimte is en daarom deze ruimte is gewijd aan permanente tabellen.  Gegroepeerd columnstore compressie schatting bij 5 X.  Deze compressie kunt u de database te laten groeien tot ongeveer 1 PB wanneer alle tabellen gegroepeerd columnstore (de standaardinstelling tabeltype zijn).|
| Tabel             | Maximumgrootte                                     | 60 TB gecomprimeerd op schijf   |
| Tabel             | Tabellen per database                          | 2 miljard          |
| Tabel             | Kolommen per tabel                            | 1024 kolommen       |
| Tabel             | Bytes per kolom                             | Afhankelijk van de kolom [gegevenstype][].  Limiet is 8000 voor de gegevenstypen char, 4000 voor nvarchar of 2 GB voor de gegevenstypen MAX.|
| Tabel             | Bytes per rij, gedefinieerde grootte                  | 8060 bytes<br/><br/>Het aantal bytes dat per rij wordt berekend op dezelfde manier als voor SQL Server bij compressie van de pagina. Zoals SQL Server ondersteunt SQL Data Warehouse overloop rij opslag waarmee **kolommen met variabele lengte** moet buiten een rij worden geplaatst. Waar variabele lengte rijen uit de rij aangeeft worden, worden alleen de 24-byte hoofdsite wordt opgeslagen in de belangrijkste record. Zie de [overloop van rij gegevens van meer dan 8 KB][] MSDN-artikel voor meer informatie.|
| Tabel             | Partities per tabel                    | 15.000<br/><br/>Voor krachtige, wordt u geadviseerd minimaliseren van het getal met partities die u nodig hebt tijdens het nog steeds ondersteuning van uw vereisten voor bedrijven. Als het aantal partities omvang groeit, wordt de belasting voor Data Definition Language (DDL) en gegevens bewerken taal (DML) bewerkingen in omvang groeit en zorgt ervoor dat trager.|
| Tabel             | Tekens per partition grenswaarde.| 4000 |
| Index             | Niet-gegroepeerde indexen per tabel.        | 999<br/><br/>Dit geldt voor alleen rowstore tabellen.|
| Index             | Gegroepeerde indexen per tabel.            | 1<br><br/>Dit geldt voor zowel rowstore als columnstore tabellen.|
| Index             | Index sleutelgrootte.                          | 900 bytes.<br/><br/>Dit geldt voor alleen rowstore indexen.<br/><br/>Indexen voor varchar kolommen met een maximale grootte van meer dan 900 bytes kunnen worden gemaakt als de bestaande gegevens in de kolommen niet groter is dan 900 bytes wanneer de index wordt gemaakt. Echter later invoegen of bijwerken acties op de kolommen die leiden tot de totale grootte 900 bytes overschrijdt, mislukt.|
| Index             | Belangrijke kolommen per index.                   | 16<br/><br/>Dit geldt voor alleen rowstore indexen. Gegroepeerd columnstore indexen neemt u alle kolommen.|
| Statistieken        | Grootte van de gecombineerde kolomwaarden.      | 900 bytes.         |
| Statistieken        | Kolommen per statistieken object.           | 32                 |
| Statistieken        | Statistieken op kolommen per tabel gemaakt. | 30.000            |
| Opgeslagen Procedures | Maximale aantal geneste niveaus.               | 8                 |
| Weergave              | Kolommen per weergave                         | maximaal 1024             |


## <a name="loads"></a>Laadtijd

| Categorie          | Beschrijving                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Polybase laadtijd    | Bytes per rij                                | 32.768<br/><br/>Polybase laadtijd zijn beperkt tot het laden van rijen, beide kleiner dan 32K en kunnen niet worden geladen VARCHR(MAX), NVARCHAR(MAX) of VARBINARY(MAX).  Terwijl deze limiet vandaag bestaat, wordt verwijderd vrij binnenkort.<br/><br/>


## <a name="queries"></a>Query 's

| Categorie          | Beschrijving                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Query             | In de wachtrij query's op gebruikerstabellen.               | 1000               |
| Query             | Gelijktijdige query's op systeemweergaven.          | 100                |
| Query             | In de wachtrij query's op systeemweergaven               | 1000               |
| Query             | Maximum aantal parameters                           | 2098               |
| Batch             | Maximale grootte                                 | 65, 536 * 4096        |
| Selecteer resultaten    | Kolommen per rij                              | 4096<br/><br/>U kunt meer dan 4096 kolommen per rij nooit hebben in het resultaat van selecteren. Er is geen garantie die u kunt altijd 4096 hebben. Als het abonnement query vereist is voor een tijdelijke tabel, wordt de 1024 kolommen per tabel maximale oorzaken hebben.|
| SELECTEER            | Geneste subquery 's                            | 32<br/><br/>U kunt nooit meer dan 32 geneste subquery's hebben in een SELECT-instructie. Er is geen garantie die u kunt altijd 32 hebben. Een JOIN kunt bijvoorbeeld een subquery introduceren in de query-indeling. Het aantal subquery's kan ook worden beperkt door beschikbare geheugen.|
| SELECTEER            | Kolommen per JOIN                             | 1024 kolommen<br/><br/>U kunt nooit meer dan 1024 kolommen hebben in de join DEFINIEERT. Er is geen garantie die u kunt altijd 1024 hebben. Als het abonnement JOIN vereist is voor een tijdelijke tabel met meer kolommen dan het resultaat van de JOIN, wordt de limiet van 1024 toegepast op de tijdelijke tabel. |
| SELECTEER            | Aantal bytes per GROEPEREN op kolommen.                  | 8060<br/><br/>De kolommen in de GROUP BY-component kunnen maximaal 8060 bytes hebben.|
| SELECTEER            | Bytes per ORDER BY kolommen                   | 8060 bytes.<br/><br/>De kolommen in de ORDER BY-component kunnen maximaal 8060 bytes hebben.|
| Id's en -constanten per instructie | Het aantal waarnaar wordt verwezen-id's en -constanten. | 65535<br/><br/>SQL Data Warehouse beperkt het aantal id's en -constanten die kunnen worden opgenomen in één expressie van een query. Deze limiet is 65535. In dit getal resulteert in SQL Server-fout 8632 overschrijden. Zie voor meer informatie [interne fout: de limiet van een expressie services is bereikt][].|


## <a name="metadata"></a>Metagegevens

| Systeemweergave                        | Maximum aantal rijen |
| :--------------------------------- | :------------|
| sys.dm_pdw_component_health_alerts | 10.000       |
| sys.dm_pdw_dms_cores               | 100          |
| sys.dm_pdw_dms_workers             | Totaal aantal DMS werknemers voor de meest recente 1000 aanvragen voor SQL. |
| sys.dm_pdw_errors                  | 10.000       |
| sys.dm_pdw_exec_requests           | 10.000       |
| sys.dm_pdw_exec_sessions           | 10.000       |
| sys.dm_pdw_request_steps           | Totaal aantal stappen voor de meest recente versie van 1000 SQL aanvragen die zijn opgeslagen in sys.dm_pdw_exec_requests. |
| sys.dm_pdw_os_event_logs           | 10.000       |
| sys.dm_pdw_sql_requests            | De meest recente SQL aanvragen voor 1000 die zijn opgeslagen in sys.dm_pdw_exec_requests. |


## <a name="next-steps"></a>Volgende stappen
Zie [SQL Data Warehouse verwijzing overzicht][]voor meer informatie.

<!--Image references-->

<!--Article references-->
[Data Warehouse eenheden (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Overzicht van SQL Data Warehouse verwijzing]: ./sql-data-warehouse-overview-reference.md
[Beheer van de werklast]: ./sql-data-warehouse-develop-concurrency.md
[TempDB]: ./sql-data-warehouse-tables-temporary.md
[gegevenstype]: ./sql-data-warehouse-tables-data-types.md
[een ondersteuningsticket maken]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Rij-overloop gegevens van meer dan 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Interne fout: de limiet van een expressie services is bereikt]: https://support.microsoft.com/kb/913050
