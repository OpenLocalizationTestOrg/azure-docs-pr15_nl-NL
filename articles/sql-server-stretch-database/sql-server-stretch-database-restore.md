<properties
    pageTitle="Databases uitrekken ingeschakelde herstellen | Microsoft Azure"
    description="Informatie over het herstellen van uitrekken\-databases ingeschakeld."
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
    ms.date="08/01/2016"
    ms.author="douglasl"/>

# <a name="restore-stretch-enabled-databases"></a>Databases uitrekken ingeschakelde herstellen

Een back-up van database wanneer dat nodig is om te herstellen van verschillende soorten fouten, fouten en systeemfouten herstellen.

Zie voor meer informatie over het back-up [back-up uitrekken ingeschakelde databases](sql-server-stretch-database-backup.md).

>   [AZURE.NOTE] Back-up is slechts één onderdeel van een volledige beschikbaarheid en bedrijfscontinuïteit bedrijfsoplossing. Zie voor meer informatie over de beschikbaarheid, [Hoge beschikbaarheid oplossingen](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="restore-your-sql-server-data"></a>De SQL Server-gegevens herstellen
Als u wilt herstellen van hardware mislukt of beschadigde bestanden, herstellen de uitrekken\-ingeschakeld van SQL Server-database uit een back-up. U kunt blijven de methoden voor het herstellen van SQL Server die u momenteel gebruikt. Zie voor meer informatie, [herstellen en herstel overzicht](https://msdn.microsoft.com/library/ms191253.aspx).

Nadat u de SQL Server-database terugzet, u moet uitvoeren van de opgeslagen procedure **sys.sp_rda_reauthorize_db** opnieuw de om verbinding te maken tussen de uitrekken\-ingeschakeld van SQL Server-database en de externe Azure-database. Zie [herstellen van de verbinding tussen de SQL Server-database en de externe Azure-database](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database)voor meer informatie.

## <a name="restore-your-remote-azure-data"></a>Uw externe Azure gegevens herstellen

### <a name="recover-a-live-azure-database"></a>Een live Azure-database herstellen
De SQL Server-Database uitrekken service op Azure momentopnamen alle persoonlijke gegevens op minstens elke 8 uur met Azure opslag momentopnamen. Deze momentopnamen worden zeven dagen bijgehouden. Hiermee kunt u de gegevens naar een van de ten minste 21 punten herstellen in tijd binnen de afgelopen zeven dagen tot aan de tijd waarop de laatste momentopname is gemaakt.

Als u wilt herstellen een live Azure-database op een eerder in tijd met behulp van de Azure-portal, het volgende doen.

1. Meld u aan bij de Azure-portal.
2. Selecteer **Bladeren** en selecteer **SQL-Databases**aan de linkerkant van het scherm.
3. Ga naar uw database en selecteer deze.
4. Aan de bovenkant van het blad database, klikt u op **herstellen**.
5. Een nieuwe **naam van de Database**opgeven, selecteert u een **Punt herstellen** en klik op **maken**.
6. Het herstelproces database wordt gestart en **meldingen**gebruiken kan worden gecontroleerd.

### <a name="recover-a-deleted-azure-database"></a>Een verwijderde Azure-database herstellen
De service uitrekken van SQL Server-Database op Azure maakt een databasemomentopname voordat u een database is verbroken en tevens zeven dagen. Hierna worden momentopnamen van de actieve database niet langer behouden. Hiermee kunt u een verwijderde database terugzetten naar de komma wanneer deze is verwijderd.

Als u wilt terugzetten op het punt een verwijderde Azure-database wanneer deze is verwijderd met behulp van de Azure-portal, het volgende doen.

1. Meld u aan bij de Azure-portal.
2. Aan de linkerkant van het scherm Selecteer **Bladeren** en selecteer vervolgens **SQL-Servers**.
3. Navigeer naar de server en selecteer deze.
4. Schuif omlaag naar bewerkingen op van de server blade, klikt u op de tegel **Databases verwijderd** .
5. Selecteer de verwijderde database die u wilt herstellen.
5. Een nieuwe **naam van de Database** opgeven en op **maken**.
6. Het herstelproces database wordt gestart en **meldingen**gebruiken kan worden gecontroleerd.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>De verbinding tussen de SQL Server-database en de externe Azure-database herstellen

1.  Als u gaat nu verbinding maken met een herstelde Azure-database met een andere naam of in een andere regio, voert u de opgeslagen procedure [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) met de vorige Azure-database verbreken.  

2.  De opgeslagen procedure [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) om de verbinding van de lokale uitrekken te worden uitgevoerd\-ingeschakeld database met de Azure-database.  

    -   De bestaande database beperkt referentie als een sysname of een varchar opgeeft\(128\) waarde. \(Gebruik geen varchar\(max\).\) U kunt de naam van de referentie in de weergave opzoeken **sys.database\_beperkt\_referenties**.  

    -   Opgeven of een kopie van de externe gegevens en verbinding maken met de kopie (aanbevolen).  

    ```tsql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO
    ```  

## <a name="see-also"></a>Zie ook

[Beheren en problemen met uitrekken Database](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Een back-Up en herstellen van SQL Server-Databases](https://msdn.microsoft.com/library/ms187048.aspx)
