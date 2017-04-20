<properties 
    pageTitle="Een SQL-Database met SSMS beheren | Microsoft Azure" 
    description="Informatie over het gebruik van SQL Server Management Studio SQL-Database servers en databases beheren." 
    services="sql-database" 
    documentationCenter=".net" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="tysonn"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sstein"/>


# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Azure SQL-Database met SQL Server Management Studio beheren 


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

Azure SQL Database-servers en databases beheren kunt u SQL Server Management Studio (SSMS). In dit onderwerp begeleidt u bij algemene taken met SSMS. U moet al een server en database gemaakt in Azure SQL-Database voordat u begint. Zie [uw eerste Azure SQL-Database maken](sql-database-get-started.md) en [verbinding maken en Query maken met de SSMS](sql-database-connect-query-ssms.md) voor meer informatie.

Het wordt aanbevolen dat u de nieuwste versie van SSMS gebruiken wanneer u met Azure SQL-Database werkt. 

> [AZURE.IMPORTANT] Gebruik altijd de nieuwste versie van SSMS omdat deze voortdurend wordt verbeterd als u wilt werken met de meest recente updates voor Azure en SQL-Database. Als u de meest recente versie, raadpleegt u [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx).



## <a name="create-and-manage-azure-sql-databases"></a>Maken en beheren van Azure SQL-databases

Terwijl u verbinding hebben met **de hoofddatabase** , kunt u databases maken op de server en wijzigen of bestaande databases neerzetten. De volgende stappen wordt beschreven hoe voor het uitvoeren van verschillende veelgebruikte database-beheertaken tot en met Management Studio. Als u wilt deze taken uitvoeren, zorg dat er verbinding is met **de hoofddatabase met de serverniveau principal login die u hebt gemaakt bij het instellen van uw server** .

Een in queryvenster wilt openen Management Studio, open de map Databases, vouwt u de map **System Databases** met de rechtermuisknop op de **basispagina**en klik vervolgens op **Nieuwe Query**.

-   De instructie **CREATE DATABASE** gebruiken om een database te maken. Zie [CREATE DATABASE (SQL-Database)](https://msdn.microsoft.com/library/dn268335.aspx)voor meer informatie. De volgende instructie maakt u een database met de naam **myTestDB** en wordt aangegeven dat dit een Standard S0 Edition database maken met een standaard maximale grootte van 250 GB.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Klik op **uitvoeren** als de query wilt uitvoeren.

-   Gebruik de instructie **ALTER DATABASE** voor het wijzigen van een bestaande database, bijvoorbeeld als u wilt wijzigen van de naam en -editie van de database. Zie [DATABASE wijzigen (SQL-Database)](https://msdn.microsoft.com/library/ms174269.aspx)voor meer informatie. De volgende instructie Hiermee wijzigt u de database die u hebt gemaakt in de vorige stap wijzigen edition naar standaard S1.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   Gebruik **De DATABASE weghalen** instructie om te verwijderen van een bestaande database. Zie [Neerzetten DATABASE (SQL-Database)](https://msdn.microsoft.com/library/ms178613.aspx)voor meer informatie. De volgende instructie wordt de database **myTestDB** verwijderd, maar niet zet deze neer nu omdat u deze gebruiken wilt voor het maken van aanmeldingen in de volgende stap.

        DROP DATABASE myTestBase;

-   De hoofddatabase bevat de weergave **vinden** die u gebruiken kunt om details over alle databases te bekijken. Als u wilt weergeven in alle bestaande databases, voer de volgende instructie:

        SELECT * FROM sys.databases;

-   In SQL-Database, worden de instructie **gebruiken** wordt niet ondersteund voor het schakelen tussen databases. In plaats daarvan, moet u eerst verbinding maken met de doeldatabase rechtstreeks.

>[AZURE.NOTE] Veel van de Transact-SQL-instructies die maken of wijzigen van een database moeten worden uitgevoerd binnen hun eigen batch en kunnen niet worden gegroepeerd met andere Transact-SQL-instructies. Zie de instructie specifieke informatie voor meer informatie.

## <a name="create-and-manage-logins"></a>Maken en beheren van aanmeldingen

**De hoofddatabase** aanmeldingen bevat en welke aanmeldingen zijn gemachtigd om databases of andere aanmeldingen te maken. Het beheren van aanmeldingen door verbinding maakt met **de hoofddatabase met de serverniveau principal login die u hebt gemaakt bij het instellen van uw server** . U kunt de **LOGIN maken**, **Wijzigen LOGIN**of **Neerzetten LOGIN** instructies gebruiken voor het uitvoeren van query's op de hoofddatabase die worden beheerd door aanmeldingen overal op de hele server. Zie [Databases beheren en aanmeldingen in SQL-Database](http://msdn.microsoft.com/library/azure/ee336235.aspx)voor meer informatie. 


-   Gebruik de instructie **LOGIN maken** een aanmelding serverniveau maken. Zie [Maken LOGIN (SQL-Database)](https://msdn.microsoft.com/library/ms189751.aspx)voor meer informatie. De volgende instructie Hiermee maakt u een aanmelding **login1**genoemd. **Wachtwoord1** vervangen door het wachtwoord van uw keuze.

        CREATE LOGIN login1 WITH password='password1';

-   Gebruik de instructie **CREATE USER** machtigingen van de database niveau. Alle aanmeldingen moeten worden gemaakt in **de hoofddatabase** . Voor een aanmelding verbinding maken met een andere database, moet u deze database niveau machtigingen verlenen met de instructie **CREATE USER** op die database. Zie [CREATE USER (SQL-Database)](https://msdn.microsoft.com/library/ms173463.aspx)voor meer informatie. 

-   Login1 om machtigingen te geven met een database **myTestDB**genoemd, moet u de volgende stappen uitvoeren:

 1.  Als u wilt vernieuwen Object Explorer om weer te geven van de **myTestDB** -database die u hebt gemaakt, met de rechtermuisknop op de naam van de server in Object Explorer en klik vervolgens op **vernieuwen**.  

     Als u de verbinding hebt gesloten, kunt u opnieuw verbinding maakt door te **Verbinden Object Explorer** selecteren in het menu bestand.

 2. Met de rechtermuisknop op **myTestDB** database en selecteer **Nieuwe Query**.

    3.  Voer de volgende instructie ten opzichte van de database myTestDB voor het maken van de databasegebruiker van een met de naam **login1User** die met het niveau van de server login **login1 overeenkomt**.

            CREATE USER login1User FROM LOGIN login1;

-   Gebruik de **sp\_addrolemember** opgeslagen procedure als u wilt geven het gebruikersaccount het toepasselijke toegangsniveau machtigingen voor de database. Zie [sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx)voor meer informatie. De volgende instructie geeft **login1User** alleen-lezen machtigingen voor de database door toe te voegen **login1User** naar de **db\_datareader** rol.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Gebruik de instructie **LOGIN wijzigen** voor het wijzigen van een bestaande aanmelding, bijvoorbeeld als u wilt wijzigen van het wachtwoord voor de aanmelding. Zie [ALTER LOGIN (SQL-Database)](https://msdn.microsoft.com/library/ms189828.aspx)voor meer informatie. De instructie **ALTER LOGIN** moet worden uitgevoerd ten opzichte van **de hoofddatabase** . Ga terug naar het venster van de query die is gekoppeld aan die database. De volgende instructie rollupaddissubtotal() toevoegen om de aanmelding **login1** om het wachtwoord opnieuw instellen. **NewPassword** vervangen door het wachtwoord van uw keuze en **OudWachtwoord** met het huidige wachtwoord voor de aanmelding.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   Gebruik **De LOGIN neerzetten** instructie verwijderen van een bestaande aanmelding. Verwijderen van een aanmelding op serverniveau verwijderd gebruikersaccounts gekoppelde database ook. Zie [Neerzetten DATABASE (SQL-Database)](https://msdn.microsoft.com/library/ms178613.aspx)voor meer informatie. De instructie **LOGIN neerzetten** moet worden uitgevoerd ten opzichte van **de hoofddatabase** . De instructie Hiermee verwijdert u de **login1** login.

        DROP LOGIN login1;

-   De hoofddatabase heeft de **sys.sql\_aanmeldingen** weergeven waarmee u kunt aanmeldingen weergeven. Als u wilt weergeven in alle bestaande aanmeldingen, voer de volgende instructie:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>SQL-Database met dynamische Management weergaven controleren

SQL-Database ondersteunt verschillende dynamische management weergaven die u gebruiken kunt om de een afzonderlijke database te houden. Een paar voorbeelden van het type monitor gegevens die u via deze weergaven ophalen kunt zijn uit te voeren. Zie [cmdlets voor controle van de SQL-Database dynamische Management weergaven gebruiken](https://msdn.microsoft.com/library/azure/ff394114.aspx)voor uitgebreide informatie en meer voorbeelden van het gebruik.

-   Query's uitvoeren van een weergave dynamische management vereist **Weergave DATABASE staat** machtigingen. De **Weergave DATABASE staat** als toestemming wilt verlenen aan een specifieke databasegebruiker, verbinding maken met de database en voer de volgende instructie ten opzichte van de database:

        GRANT VIEW DATABASE STATE TO login1User;

-   Berekenen met behulp van de database-grootte de **sys.dm\_db\_partition\_stat** weergave. De **sys.dm\_db\_partition\_stat** weergave geeft als resultaat informatie over de pagina en aantal rijen voor elke partition in de database die u gebruiken kunt om de databasegrootte te berekenen. De volgende query wordt de grootte van uw database in megabytes op dat:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Gebruik de **sys.dm\_Raad\_verbindingen** en **sys.dm\_Raad\_sessies** weergaven om op te halen informatie over de huidige gebruikersverbindingen en interne taken die zijn gekoppeld aan de database. De volgende query retourneert informatie over de huidige verbinding:

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Gebruik de **sys.dm\_Raad\_query\_stat** weergave om op te halen statistische prestatiestatistieken voor: in cache opgeslagen query-abonnementen. De volgende query geeft informatie over de bovenste vijf query's rangschikking door gemiddelde CPU-tijd.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 
