<properties
    pageTitle="Beheren en problemen met uitrekken Database | Microsoft Azure"
    description="Informatie over het beheren en problemen met uitrekken Database."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="manage-and-troubleshoot-stretch-database"></a>Beheren en problemen met uitrekken Database

Om te beheren en problemen met uitrekken Database, gebruikt u de hulpprogramma's en methoden beschreven in dit onderwerp.

## <a name="manage-local-data"></a>Lokale gegevens beheren

### <a name="LocalInfo"></a>Toon info over lokale databases en tabellen die zijn ingeschakeld voor de Database uitrekken
Open de catalogus weergaven **vinden** en **systeemtabellen** om te zien van info over uitrekken\-ingeschakeld van SQL Server-databases en tabellen. Zie voor meer informatie [vinden (Transact-SQL)](https://msdn.microsoft.com/library/ms178534.aspx) en [systeemtabellen (Transact-SQL)](https://msdn.microsoft.com/library/ms187406.aspx).

Om te zien hoeveel ruimte een uitrekken\-ingeschakeld tabel wordt gebruikt in SQL Server, voer de volgende instructie.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>Gegevensmigratie beheren

### <a name="check-the-filter-function-applied-to-a-table"></a>De filterfunctie is toegepast op een tabel controleren
Open de catalogusweergave van de **sys.remote\_gegevens\_archief\_tabellen** en controleert u de waarde van de **filter\_predikaat** kolom om de functie waarin uitrekken Database wordt gebruikt om te selecteren van rijen wilt migreren. Als de waarde null is, is de hele tabel in aanmerking om te worden gemigreerd. Zie [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx) en [selecteert u rijen wilt migreren met behulp van een filterfunctie](sql-server-stretch-database-predicate-function.md)voor meer informatie.

### <a name="Migration"></a>Controleer de status van de gegevensmigratie
Selecteer **taken | Uitrekken | Monitor** voor een database in SQL Server Management Studio om de gegevensmigratie in uitrekken Database Monitor te houden. Zie voor meer informatie [controleren en problemen met gegevensmigratie (uitrekken-Database)](sql-server-stretch-database-monitor.md).

Of, opent u de weergave dynamische management **sys.dm\_db\_rda\_migratie\_status** om te zien hoeveel batches en rijen met gegevens zijn gemigreerd.

### <a name="Firewall"></a>Problemen met de gegevensmigratie
Raadpleeg voor meer suggesties [controleren en problemen met gegevensmigratie (uitrekken-Database)](sql-server-stretch-database-monitor.md).

## <a name="manage-remote-data"></a>Externe gegevens beheren

### <a name="RemoteInfo"></a>Toon info over externe databases en tabellen die worden gebruikt door uitrekken Database
Open de catalogusweergaven **sys.remote\_gegevens\_archief\_databases** en **sys.remote\_gegevens\_archief\_tabellen** om te zien van informatie over de externe databases en de tabellen waarin gemigreerde gegevens worden opgeslagen. Zie [sys.remote_data_archive_databases (Transact-SQL)](https://msdn.microsoft.com/library/dn934995.aspx) en [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx)voor meer informatie.

Als u wilt zien hoeveel ruimte wordt een tabel uitrekken ingeschakelde gebruiken in Azure wordt aangegeven, voer de volgende instructie.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Gemigreerde gegevens verwijderen  
Als u gegevens verwijderen die al zijn gemigreerd naar Azure wilt, volgt u de stappen in [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx).

## <a name="manage-table-schema"></a>Tabelschema beheren

### <a name="dont-change-the-schema-of-the-remote-table"></a>Het schema van de externe tabel niet wijzigen
Het schema van een externe Azure tabel die is gekoppeld aan een SQL Server-tabel die is geconfigureerd voor uitrekken Database hoeft niet te wijzigen. Niet wijzigen met name de naam of het gegevenstype van een kolom. De functie uitrekken Database kunt verschillende hypothesen over het schema van de externe tabel ten opzichte van het schema van de SQL Server-tabel. Als u de externe schema wijzigt, uitrekken Database werkt niet voor de gewijzigde tabel.

### <a name="reconcile-table-columns"></a>Tabelkolommen afstemmen  
Als u kolommen uit de tabel met externe per ongeluk hebt verwijderd, voert u **sp_rda_reconcile_columns** als kolommen wilt toevoegen aan de externe tabel die in de uitrekken aanwezig\-ingeschakeld van SQL Server-tabel maar niet in de tabel met externe. Zie [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx)voor meer informatie.  

  > [!IMPORTANT] Wanneer **sp_rda_reconcile_columns** wordt opnieuw gemaakt van de kolommen die u per ongeluk hebt verwijderd uit de externe tabel, worden de gegevens die zich eerder in de verwijderde kolommen niet hersteld.

**sp_rda_reconcile_columns** bevat geen kolommen gegevens verwijderen uit de externe tabel die voorkomt in de tabel met externe maar niet in de uitrekken\-ingeschakeld van SQL Server-tabel. Als er op kolommen in de tabel met externe Azure die niet langer bestaat niet in de uitrekken\-ingeschakeld van SQL Server tabel deze extra kolommen voorkom niet dat uitrekken Database normale werking. Desgewenst kunt u de extra kolommen handmatig verwijderen.  

## <a name="manage-performance-and-costs"></a>Prestaties en kosten beheren  

### <a name="troubleshoot-query-performance"></a>Problemen met de prestaties van query 's
Query's met uitrekken\-ingeschakelde tabellen naar verwachting trager dan dan vóór de tabellen zijn ingeschakeld voor uitrekken uitvoeren. Als queryprestaties aanzienlijk afnemen, raadpleegt u de volgende mogelijke problemen.

-   Is uw Azure-server in een ander geografisch gebied dan uw SQL-Server? Azure server zich in dezelfde geografische regio als uw SQL-Server te beperken netwerklatentie configureren.

-   Uw netwerkcondities mogelijk niet beschikbaar is hebt weergegeven. Neem contact op met uw netwerkbeheerder voor informatie over de recente problemen of bijvoorbeeld.

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Azure prestatieniveau voor resource vergroten\-intensief bewerkingen zoals indexeren
Wanneer u maken, opnieuw opbouwen of reorganiseren indexen voor een grote tabel die geconfigureerd voor uitrekken Database en u verwacht dat u veel query's uitvoeren van de gemigreerde gegevens in Azure wordt aangegeven tijdens deze periode, kunt u het prestatieniveau van de bijbehorende externe Azure-database voor de duur van de bewerking te vergroten. Zie de [Prijzen van SQL Server uitrekken Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer informatie over prestaties en prijzen.

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>U kunt niet de service uitrekken van SQL Server-Database op Azure pauzeren  
 Zorg ervoor dat u selecteert de juiste prestaties en prijzen van niveau. Als u het prestatieniveau van de tijdelijk voor een resource verhogen\-vergt, het vorige niveau te herstellen nadat de bewerking is voltooid. Zie de [Prijzen van SQL Server uitrekken Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer informatie over prestaties en prijzen.  

## <a name="change-the-scope-of-queries"></a>Het bereik van query's wijzigen  
 Query's op uitrekken\-ingeschakelde tabellen lokale en externe gegevens retourneren al dan niet standaard. U kunt het bereik van query's voor alle query's door alle gebruikers of alleen voor één query door een beheerder wijzigen.  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Het bereik van query's voor alle query's door alle gebruikers wijzigen  
 Als u wilt wijzigen van het bereik van alle query's door alle gebruikers, voert u de opgeslagen procedure **sys.sp_rda_set_query_mode**. U kunt het bereik als u wilt alleen lokale gegevens query, alle query's uitschakelen of de standaardinstelling herstellen verkleinen. Zie [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx)voor meer informatie.  

### <a name="queryHints"></a>Het bereik van query's voor één query door een beheerder wijzigen  
 Toevoegen als u wilt wijzigen van het bereik van één query door een lid van de db_owner, de * *WITH \( REMOTE_DATA_ARCHIVE_OVERRIDE = *waarde* \)** query Tip de SELECT-instructie. De aanwijzing van de query REMOTE_DATA_ARCHIVE_OVERRIDE kan de volgende waarden hebben.  
 -   **LOCAL_ONLY**. Alleen lokale gegevens query.  

 -   **REMOTE_ONLY**. Query alleen externe gegevens.  

 -   **STAGE_ONLY**. Query alleen de gegevens in de tabel waar uitrekken Database fasen rijen in aanmerking komen voor de migratie en behoudt gemigreerde rijen voor de opgegeven periode na de migratie. Deze geheugensteun query is de enige manier om query's in de tabel tijdelijk opslaan.  

De volgende query wordt bijvoorbeeld alleen lokale resultaten.  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="adminHints"></a>Controleer administratieve bijwerken en verwijderen  
 Door standaard die u kunt niet bijwerken of verwijderen rijen die in aanmerking voor de migratie komen of rijen die al zijn gemigreerd, in een uitrekken\-tabel ingeschakeld. Als er een probleem op te lossen, een bewerking bijwerken of verwijderen van een lid van de db_owner kan worden uitgevoerd door toe te voegen de * *WITH \( REMOTE_DATA_ARCHIVE_OVERRIDE = *waarde* \)** Tip van de query met de instructie. De aanwijzing van de query REMOTE_DATA_ARCHIVE_OVERRIDE kan de volgende waarden hebben.  
 -   **LOCAL_ONLY**. Bijwerken of lokale gegevens alleen verwijderen.  

 -   **REMOTE_ONLY**. Bijwerken of verwijderen van alleen externe gegevens.  

 -   **STAGE_ONLY**. Bijwerken of verwijderen van alleen de gegevens in de tabel waar uitrekken Database fasen rijen in aanmerking komen voor de migratie en behoudt gemigreerde rijen voor de opgegeven periode na de migratie.  

## <a name="see-also"></a>Zie ook

[Monitor uitrekken Database](sql-server-stretch-database-monitor.md)

[Back-up uitrekken ingeschakelde databases](sql-server-stretch-database-backup.md)

[Databases uitrekken ingeschakelde herstellen](sql-server-stretch-database-restore.md)
