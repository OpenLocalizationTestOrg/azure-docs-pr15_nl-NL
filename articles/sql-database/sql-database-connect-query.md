<properties
    pageTitle="Verbinding maken met SQL-Database met een query C# | Microsoft Azure"
    description="Schrijf een programma in C# query en verbinding maken met SQL-database. Informatie over het IP-adressen, verbindingstekenreeksen secure login en gratis Visual Studio."
    services="sql-database"
    keywords="c# databasequery's, c# query verbinding maken met database, SQL C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# <a name="connect-to-a-sql-database-with-visual-studio"></a>Verbinding maken met een SQL-Database met Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Leer hoe u verbinding maken met een Azure SQL-database in Visual Studio. 

## <a name="prerequisites"></a>Vereisten voor


Als u verbinding maakt met een SQL-database met Visual Studio, wilt nodig u het volgende: 


- Een SQL-database kunt verbinden. In dit artikel worden de voorbeelddatabase **AdventureWorks** gebruikt. Als u de voorbeelddatabase AdventureWorks, Zie [maken de voorbeelddatabase](sql-database-get-started.md).


- Visual Studio 2013 update 4 (of later). Microsoft biedt nu Visual Studio-Community voor *gratis*.
 - [Visual Studio-Community, downloaden](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Meer opties voor Visual Studio vrij te geven](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## <a name="open-visual-studio-from-the-azure-portal"></a>Visual Studio openen vanuit de Azure-portal


1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).

2. Klik op **Meer Services** > **SQL-databases**
3. Open het blad **AdventureWorks** -database te zoeken en te klikken op de *AdventureWorks* -database.

6. Klik op de knop **Extra** aan de bovenkant van het blad database:

    ![Nieuwe query. Verbinding maken met SQL-databaseserver: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Klik op **openen in Visual Studio** (als u nodig hebt voor Visual Studio, klikt u op de koppeling):

    ![Nieuwe query. Verbinding maken met SQL-databaseserver: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio wordt geopend met het venster **verbinding maken met de Server** is al ingesteld op verbinding maken met de server en database die u hebt geselecteerd in de portal.  (Klik op **Opties** om te bevestigen dat de verbinding is ingesteld op de juiste database). Typ uw beheerderswachtwoord server en klik op **verbinding maken**.


    ![Nieuwe query. Verbinding maken met SQL-databaseserver: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Als u nog geen een firewall regelset voor IP-adres van uw computer, krijgt u een bericht *kan geen verbinding maken* hier. Een als firewallregel wilt maken, raadpleegt u [een regel voor het niveau van de server firewall van Azure SQL-Database configureren](sql-database-configure-firewall-settings.md).


9. Nadat de verbinding tot stand, is het **SQL Server-Objectverkenner** -venster wordt geopend met een verbinding met uw database.

    ![Nieuwe query. Verbinding maken met SQL-databaseserver: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## <a name="run-a-sample-query"></a>Voorbeeld van query's uitvoeren

Nu dat we met de database verbonden bent, wordt in de volgende stappen uitvoeren van een eenvoudige query weergeven:

2. Met de rechtermuisknop op de database en selecteer vervolgens **Nieuwe Query**.

    ![Nieuwe query. Verbinding maken met SQL-databaseserver: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. Kopieer en plak de volgende code in het queryvenster.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Klik op de knop **uitvoeren** als de query wilt uitvoeren:

    ![Success. Verbinding maken met SQL-databaseserver: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Volgende stappen

- SQL-databases openen in Visual Studio, gebruikt SQL Server Data Tools. Zie [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx)voor meer informatie.
- Als u wilt verbinden met een SQL-database met code, Zie [verbinding maken met SQL-Database met behulp van .NET (C#)](sql-database-develop-dotnet-simple.md).



