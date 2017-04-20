<properties
    pageTitle="SQL-Database zelfstudie: aan de slag met beveiliging"
    description="Leer hoe u gebruikersaccounts wilt openen en voor het beheren van een database maken."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>SQL-Database zelfstudie: gebruikersaccounts kunnen openen en beheren van een database van SQL-database maken


> [AZURE.SELECTOR]
- [Get-slag zelfstudie](sql-database-get-started-security.md)
- [Toegang verlenen](sql-database-manage-logins.md)

In deze zelfstudie leert u hoe u SQL Server Management Studio (SSMS) om te gebruiken:

- Meld u aan met behulp van een principal aanmelding serverniveau SQL-Database.
- Maak een gebruikersaccount SQL-Database.
- Een SQL-Database gebruiker [db_owner machtigingen](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0)verlenen.
- Verbinding maken met een SQL-database aan een gebruikersaccount die is niet een principal serverniveau.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## <a name="next-steps"></a>Volgende stappen
Nu u hebt voltooid deze zelfstudie SQL-Database en u kunt ook een gebruikersaccount gemaakt en de gebruiker account dbo machtigingen verleend, bent u gereed voor meer informatie over de [beveiliging van de SQL-Database](sql-database-manage-logins.md).


