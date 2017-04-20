<properties 
    pageTitle="Kopieer een met Transact-SQL Azure SQL-database | Microsoft Azure" 
    description="Kopie van een met Transact-SQL Azure SQL-database maken" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Kopieer een met Transact-SQL Azure SQL-database


> [AZURE.SELECTOR]
- [Overzicht](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)


Deze volgt uitgelegd hoe u een SQL-database met Transact-SQL kopiëren naar dezelfde server of een andere server. De kopieeropdracht database wordt de instructie [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) .

U voltooit de stappen in dit artikel nodig u het volgende:

- Een Azure-abonnement. Klik op **Gratis PROEFVERSIE** boven aan deze pagina en keert u terug naar het voltooien van dit artikel als u een abonnement op Azure gewoon nodig hebt.
- Een Azure SQL-Database. Als u een SQL-database niet hebt, maakt u één de stappen in dit artikel: [uw eerste Azure SQL-Database maken](sql-database-get-started.md).
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Als er geen SSMS of als functies die worden beschreven in dit artikel zijn niet beschikbaar, [download de nieuwste versie](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="copy-your-sql-database"></a>Kopieer de SQL-database

Meld u aan bij de hoofddatabase via de hoofdsom login serverniveau of de aanmelding die gemaakt van de database die u wilt kopiëren. Aanmeldingen die niet de hoofdsom serverniveau zijn moeten zijn leden van de functie dbmanager databases kopiëren. Zie voor meer informatie over aanmeldingen en verbinding met de server, [aanmeldingen beheren](sql-database-manage-logins.md).

Beginnen met het kopiëren van de brondatabase met de instructie [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) . Deze instructie uitvoert, start de database proces kopiëren. Omdat kopiëren van een database een asynchroon proces is, retourneert de instructie CREATE DATABASE voordat de database is voltooid kopiëren.


### <a name="copy-a-sql-database-to-the-same-server"></a>Een SQL-database naar dezelfde server kopiëren

Meld u aan bij de hoofddatabase via de hoofdsom login serverniveau of de aanmelding die gemaakt van de database die u wilt kopiëren. Aanmeldingen die niet de hoofdsom serverniveau zijn moeten zijn leden van de functie dbmanager databases kopiëren.

Deze opdracht kopieën Database1 bij een nieuwe database met de naam van Database2 op dezelfde server. De kopieeropdracht duurt enige tijd om uit te voeren, afhankelijk van de grootte van uw database.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Een SQL-database naar een andere server kopiëren

Meld u aan bij de hoofddatabase van de doelserver, de Azure SQL Database-server waar de nieuwe database wordt gemaakt. Gebruik een aanmelding met dezelfde naam en hetzelfde wachtwoord als de database-eigenaar van de brondatabase op de bron Azure SQL Database-server. De aanmelding op de doelserver moet ook zijn van een lid van de rol dbmanager of de serverniveau principal login.

Deze opdracht kopieert Database1 op server1 - met een nieuwe database met de naam Database2 op server2. Afhankelijk van de grootte van uw database duurt de kopie enige tijd om te voltooien.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## <a name="monitor-the-progress-of-the-copy-operation"></a>De voortgang van de kopieeropdracht

De kopieeropdracht door query's uitvoeren van de weergaven vinden en sys.dm_database_copies controleren. Terwijl de kopiëren uitgevoerd wordt, wordt de kolom state_desc van de weergave vinden voor de nieuwe database is ingesteld op kopiëren.


- Als het kopiëren mislukt, wordt de kolom state_desc van de weergave vinden voor de nieuwe database is ingesteld op verdacht. In dit geval de instructie DROP uitvoeren op de nieuwe database en probeer het later opnieuw.
- Als het kopiëren is geslaagd, wordt de kolom state_desc van de weergave vinden voor de nieuwe database is ingesteld op ONLINE. In dit geval de kopiëren is voltooid en de nieuwe database is een normale database, kunnen worden gewijzigd onafhankelijk van de brondatabase.

> [AZURE.NOTE] -Als u besluit om op te zeggen het kopiëren terwijl deze uitgevoerd wordt, moet u de [DATABASE weghalen](https://msdn.microsoft.com/library/ms178613.aspx) -instructie uitvoeren op de nieuwe database. U kunt ook annuleert de instructie DROP DATABASE uitvoeren op de brondatabase ook kopiëren.


## <a name="resolve-logins-after-the-copy-operation-completes"></a>Aanmeldingen oplossen nadat u de kopieerbewerking is voltooid

Nadat de nieuwe database online op de doelserver is, gebruikt u de instructie [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) om de gebruikers van de nieuwe database naar aanmeldingen op de doelserver opnieuw toe te. Om op te lossen zwevende gebruikers, raadpleegt u [Problemen met zwevende gebruikers](https://msdn.microsoft.com/library/ms175475.aspx). Zie ook [het beheren van Azure SQL database-beveiliging na herstel](sql-database-geo-replication-security-config.md).

Alle gebruikers in de nieuwe database voor het behoud van de machtigingen die ze in de brondatabase hadden. De gebruiker van wie de databasekopie wordt gestart, wordt de database-eigenaar van de nieuwe database en een nieuwe beveiligings-id (beveiligings-id) is toegewezen. Nadat het kopiëren is geslaagd en voordat u andere gebruikers opnieuw worden toegewezen, kunnen alleen de aanmelding die het kopiëren gestart, de database-eigenaar, zich kunt aanmelden bij de nieuwe database.


## <a name="next-steps"></a>Volgende stappen

- Zie [kopiëren van een Azure SQL-database](sql-database-copy.md) voor een overzicht van het kopiëren van een Azure SQL-Database.
- Zie [een Azure SQL-database met behulp van de Azure portal kopiëren](sql-database-copy-portal.md) naar het kopiëren van een database met behulp van de Azure-portal.
- Zie [een Azure SQL-database kopiëren via PowerShell](sql-database-copy-powershell.md) om te kopiëren van een database via PowerShell.
- Lees [hoe u de beveiliging van de Azure SQL-database na herstel beheren](sql-database-geo-replication-security-config.md) voor meer informatie over het beheren van gebruikers en aanmeldingen bij het kopiëren van een database naar een andere logische server.



## <a name="additional-resources"></a>Aanvullende informatie

- [Aanmeldingen beheren](sql-database-manage-logins.md)
- [Verbinding maken met SQL-Database met SQL Server Management Studio en uitvoeren van een steekproef T-SQL-query](sql-database-connect-query-ssms.md)
- [De database exporteren naar een BACPAC](sql-database-export.md)
- [Bedrijfscontinuïteit voor bedrijfsoverzichten](sql-database-business-continuity.md)
- [SQL-Database documentatie](https://azure.microsoft.com/documentation/services/sql-database/)


