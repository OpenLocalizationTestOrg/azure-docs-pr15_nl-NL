<properties
   pageTitle="Query Azure SQL Data Warehouse (sqlcmd) | Microsoft Azure"
   description="Query's uitvoeren Azure SQL Data Warehouse met de opdrachtregel hulpprogramma sqlcmd."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>

# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Query Azure SQL Data Warehouse (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

In dit scenario wordt het hulpprogramma [sqlcmd][] gebruikt om query's in een Azure SQL Data Warehouse.  

## <a name="1-connect"></a>1. verbinding maken

Aan de slag met [sqlcmd][], open de opdrachtprompt en voer **sqlcmd** , gevolgd door de verbindingsreeks voor de database van SQL Data Warehouse. De verbindingsreeks worden de volgende parameters:

+ **Server (-S):** Server in het formulier `<`servernaam`>`. database.windows.net
+ **Database (-d):** De naam van de database.
+ **Inschakelen id's tussen aanhalingstekens (-ik):** Id's tussen aanhalingstekens moeten zijn ingeschakeld verbinding maken met een exemplaar van SQL Data Warehouse.

Als u wilt gebruiken in SQL Server-verificatie, moet u de gebruikersnaam en wachtwoord van parameters toevoegen:

+ **Gebruiker (-V):** Server-gebruiker in het formulier `<`gebruiker`>`
+ **Wachtwoord (-P):** Het wachtwoord die zijn gekoppeld aan de gebruiker.

De verbindingsreeks kan er bijvoorbeeld als volgt uitzien:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Als u wilt Azure Active Directory geïntegreerde verificatie gebruikt, moet u de Azure Active Directory-parameters toevoegen:

+ **Azure Active Directory-verificatie (-G):** Azure Active Directory gebruiken voor verificatie

De verbindingsreeks kan er bijvoorbeeld als volgt uitzien:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] Moet u [Azure Active Directory-verificatie inschakelen](sql-data-warehouse-authentication.md) om te verifiëren met Active Directory.

## <a name="2-query"></a>2. query

Nadat de verbinding, kunt u een ondersteunde Transact-SQL-instructies voor het exemplaar uitgeven.  In dit voorbeeld worden de query's in de interactieve modus ingediend.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Deze volgende voorbeelden wordt getoond hoe u uw query's in de batchmodus met de optie -Q of uw SQL naar sqlcmd pijpleidingplan kunt uitvoeren.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Volgende stappen

Zie [de documentatie sqlcmd][sqlcmd] voor meer informatie over de informatie over de opties die beschikbaar zijn in sqlcmd.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[Sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
