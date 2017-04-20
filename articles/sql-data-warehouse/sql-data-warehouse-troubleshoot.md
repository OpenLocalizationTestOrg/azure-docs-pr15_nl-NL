<properties
   pageTitle="Problemen met Azure SQL datawarehouse | Microsoft Azure"
   description="Voor probleemoplossing Azure SQL datawarehouse."
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
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# <a name="troubleshooting-azure-sql-data-warehouse"></a>Azure SQL datawarehouse probleemoplossing

In dit onderwerp worden enkele van de meer veelvoorkomende vragen die we van onze klanten krijgen.

## <a name="connecting"></a>Verbinding maken

| Probleem                              | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Aanmelding voor gebruiker 'NT AUTHORITY\ANONYMOUS-aanmelding'. (Microsoft SQL Server, fout: 18456) | Deze fout treedt op wanneer een AAD-gebruiker probeert verbinding maken met de hoofddatabase, maar geen een gebruiker in het model.  U kunt dit probleem op de gewenste verbinding maken met stand of de gebruiker toevoegen aan de hoofddatabase van SQL Data Warehouse opgeven.  Zie [overzicht van de beveiliging][] artikel voor meer informatie.|
|De server principal 'gebruik met Windows"is niet toegang tot de database"outmodel"onder de huidige beveiligingscontext. Gebruiker standaarddatabase openen niet. Aanmelding is mislukt. Aanmelding voor gebruiker 'Gebruik met Windows'. (Microsoft SQL Server, fout: 916) | Deze fout treedt op wanneer een AAD-gebruiker probeert verbinding maken met de hoofddatabase, maar geen een gebruiker in het model.  U kunt dit probleem op de gewenste verbinding maken met stand of de gebruiker toevoegen aan de hoofddatabase van SQL Data Warehouse opgeven.  Zie [overzicht van de beveiliging][] artikel voor meer informatie.|
| CTAIP fout                        | Deze fout kan optreden wanneer een aanmelding is gemaakt op de hoofddatabase van SQL server, maar niet in de database van SQL Data Warehouse.  Als u dit foutbericht krijgt, raadpleegt u het artikel [overzicht van de beveiliging][] .  In dit artikel wordt uitgelegd hoe u maakt een login en de gebruiker op diamodel en klik vervolgens op het maken van een gebruiker in de database van SQL Data Warehouse maken.|
| Geblokkeerd door de Firewall                |Azure SQL-databases zijn beveiligd met DRM server en database niveau firewalls om ervoor te zorgen alleen bekende IP-adressen toegang hebt tot een database. De firewalls zijn standaard, wat betekent dat u moet expliciet is inschakelen en IP-adres of reeks adressen secure voordat u verbinding kunt maken.  Volg de stappen in [configureren server firewalltoegang voor uw IP client][] in de [Provisioning instructies][]configureren uw firewall voor toegang.|
| Kan geen verbinding maken met het hulpmiddel of -stuurprogramma | SQL Data Warehouse raadt [SSMS][], [SSDT voor Visual Studio-2015][]of [sqlcmd][] om query's in uw gegevens. Zie voor meer informatie over het stuurprogramma's en verbinding maken met SQL Data Warehouse [stuurprogramma's voor Azure SQL Data Warehouse][] en [verbinding maken met Azure SQL Data Warehouse][] artikelen.|


## <a name="tools"></a>Hulpmiddelen voor

| Probleem                              | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Visual Studio object explorer ontbreekt AAD-gebruikers | Dit is een bekend probleem.  Als een tijdelijke oplossing de gebruikers in [sys.database_principals][]te bekijken.  Zie [verificatie Azure SQL Data Warehouse][] voor meer informatie over het gebruik van Azure Active Directory met SQL Data Warehouse.|

## <a name="performance"></a>Prestaties

|  Probleem                             | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Query met het oplossen van prestatieproblemen gaat  | Als u probeert een bepaalde query oplossen, begint u met het [leren hoe u uw query's controleren][].|
| Slechte queryprestaties en abonnementen is vaak het resultaat van de ontbrekende statistieken   | De meest voorkomende oorzaak van slechte prestaties is gebrek aan statistieken over tabellen.  Zie [Maintaining tabelstatistieken] [ Statistics] voor meer informatie over het maken van statistieken en waarom ze zijn essentieel voor uw optreden.|
| Lage gelijktijdigheid / query's in de wachtrij   | Lidmaatschap [werkbelasting management][] is het belangrijk om te weten hoe u kunt saldo vanaf geheugentoewijzing met gelijktijdigheid.|
| Het implementeren van aanbevolen procedures    | De beste plaats om te beginnen voor meer informatie over queryprestaties verbeteren wordt [Aanbevolen procedures voor SQL Data Warehouse][] artikel.|
| Tips voor het verbeteren van prestaties en schaalbaarheid  | De oplossing voor het verbeteren van de prestaties is soms gewoon toevoegen dat meer rekenkracht aan uw query's door [uw SQL Data Warehouse schaalbaarheid][].|
| Slechte queryprestaties grond slechte index kwaliteit | Sommige momenten query's kunnen vertraging vanwege [slechte columnstore index kwaliteit][].  Zie dit artikel voor meer informatie en hoe u [indexen segment kwaliteitsverbetering opnieuw opbouwen][].|

## <a name="system-management"></a>Systeembeheer

|  Probleem                             | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Msg 40847: Kan de bewerking niet uitvoeren, omdat de server overschrijdt de toegestane Database transactie-eenheid quotum van 45000. | Verlaagt het [DWU][] van de database die u probeert te maken of [een stijging van het quotum aanvragen][].|
| Wordt onderzocht gebruik van de ruimte    | Zie de [grootte van de tabel][] voor meer informatie over het gebruik van de ruimte van uw systeem.|
| Help bij het beheren van tabellen          | Zie de [tabel overzicht] [ Overview] artikel voor hulp bij het beheren van uw tabellen.  In dit artikel bevat ook koppelingen naar meer gedetailleerde onderwerpen zoals [tabel gegevenstypen][Data types], [een tabel distribueren][Distribute], [indexeren van een tabel][Index], [partitioneren van een tabel][Partition], [Maintaining tabelstatistieken] [ Statistics] en [tijdelijke tabellen][Temporary].|

## <a name="polybase"></a>Polybase

|  Probleem                             | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| UTF-8-fout                        |  PolyBase ondersteunt momenteel alleen gegevensbestanden die UTF-8-codering zijn geladen.  Zie [werken rond de vereiste PolyBase UTF-8][] voor instructies over het omgaan met deze beperking.|
| Laden mislukt vanwege grote rijen   | Ondersteuning voor grote rij is momenteel niet beschikbaar voor Polybase.  Dit betekent dat als de tabel VARCHAR(MAX), NVARCHAR(MAX) of VARBINARY(MAX) bevat, externe tabellen kunnen niet worden gebruikt om uw gegevens te laden.  Laadtijd voor grote rijen is momenteel alleen ondersteund via Azure gegevens Factory (met BCP), Azure Stream analyses, SSIS, BCP of de klas .NET SQLBulkCopy. PolyBase ondersteuning voor grote rijen wordt toegevoegd aan een toekomstige release.|
| BCP laden van de tabel met het gegevenstype MAX is verbroken | Er is een bekend probleem die is vereist dat VARCHAR(MAX), NVARCHAR(MAX) of VARBINARY(MAX) aan het einde van de tabel in sommige gevallen moeten worden geplaatst.  Verplaats uw MAX-kolommen naar het einde van de tabel.|

## <a name="differences-from-sql-database"></a>Verschillen van de SQL-Database

|  Probleem                             | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Niet-ondersteunde functies voor SQL-Database  | Zie de [niet-ondersteunde tabelfuncties][].|
| Niet-ondersteunde gegevenstypen van de SQL-Database  | Zie de [niet-ondersteunde gegevenstypen][].|
| DELETE en UPDATE beperkingen      | Zie [UPDATE tijdelijke oplossingen][], [tijdelijke oplossingen verwijderen][] en [Gebruik CTAS omgaan met niet-ondersteunde syntaxis van de bijwerken en verwijderen][].  |
| SAMENVOEGEN instructie wordt niet ondersteund   | Zie [samenvoegen tijdelijke oplossingen][].|
| Beperkingen van de opgeslagen procedure       | Zie [opgeslagen procedure beperkingen][] voor meer informatie over enkele van de beperkingen van opgeslagen procedures.|
| UDF's ondersteunen geen SELECT-instructies | Dit is een beperking van onze UDF's.  Zie de [Functie maken][] voor de syntaxis van de die wordt ondersteund.   |

## <a name="next-steps"></a>Volgende stappen

Als u bent zijn niet vinden van een oplossing voor het bovenstaande probleem, Hier volgen enkele andere bronnen die u kunt proberen.

- [Blogs]
- [Functieverzoeken verzenden]
- [Video 's]
- [KAT team blogs]
- [Ondersteuningsticket maken]
- [MSDN-forum]
- [Stapel overloop-forum]
- [Twitter]

<!--Image references-->

<!--Article references-->
[Beveiligingsoverzicht]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT voor Visual Studio-2015]: ./sql-data-warehouse-install-visual-studio.md
[Stuurprogramma's voor SQL Azure datawarehouse]: ./sql-data-warehouse-connection-strings.md
[Verbinding maken met SQL Azure datawarehouse]: ./sql-data-warehouse-connect-overview.md
[Ondersteuningsticket maken]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Schaalbaarheid van uw SQL datawarehouse]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[een stijging van het quotum aanvragen]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[Leren hoe u uw query's controleren]: ./sql-data-warehouse-manage-monitor.md
[Instructies inrichting]: ./sql-data-warehouse-get-started-provision.md
[Toegang tot de firewall van de server configureren voor uw client-IP]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[Aanbevolen procedures voor SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Grootte van de tabel]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Niet-ondersteunde tabelfuncties]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Niet-ondersteunde gegevenstypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Slechte columnstore index kwaliteit]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Indexen om te verbeteren segment kwaliteit opnieuw opbouwen]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Beheer van de werklast]: ./sql-data-warehouse-develop-concurrency.md
[Gebruik van CTAS omgaan met niet-ondersteunde syntaxis van de bijwerken en verwijderen]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[Tijdelijke oplossingen bijwerken]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Tijdelijke oplossingen verwijderen]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Tijdelijke oplossingen samenvoegen]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Beperkingen van de opgeslagen procedure]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Verificatie van Azure SQL datawarehouse]: ./sql-data-warehouse-authentication.md
[Werken rond de vereiste PolyBase UTF-8]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[FUNCTIE MAKEN]: https://msdn.microsoft.com/library/mt203952.aspx
[Sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[KAT team blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Functieverzoeken verzenden]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stapel overloop-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video 's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

