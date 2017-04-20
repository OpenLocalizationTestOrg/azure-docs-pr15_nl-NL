<properties
    pageTitle="Aan de slag met meerdere databases query's (verticale partities) | Microsoft Azure"   
    description="het gebruik van elastische databasequery's met verticaal partitioneren databases"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Aan de slag met meerdere databases query's (verticale partities) (preview)

Elastische databasequery's (preview) voor Azure SQL-Database kunt u uitvoeren van de T-SQL-query's die meerdere databases met één verbindingspunt's beslaan. In dit onderwerp is van toepassing op [databases verticaal partitioneren](sql-database-elastic-query-vertical-partitioning.md).  

Als voltooid, wordt u: meer informatie over het configureren en gebruiken van een Azure SQL-Database voor het uitvoeren van query's die meerdere verwante databases's beslaan. 

Voor meer informatie over de functie van de query elastische database, raadpleegt u [queryoverzicht Azure SQL Database-elastische database](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>Maken van de steekproef-databases

Eerst moeten we twee databases, **klanten** en **Orders**, in de dezelfde of een andere logische servers maken.   

De volgende query's uitvoeren op de database **Orders** voor het maken van de tabel **OrderInformation** en invoeren van de voorbeeldgegevens. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Nu uitvoeren volgen query op de **klanten** -database voor het maken van de tabel **CustomerInformation** en invoeren van de voorbeeldgegevens. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Databaseobjecten maken
### <a name="database-scoped-master-key-and-credentials"></a>Database beperkt outmodel sleutel en referenties

1. Open SQL Server Management Studio of SQL Server Data Tools in Visual Studio.
2. Verbinding maken met de database Orders en uitvoeren van de volgende T-SQL-opdrachten:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    "Gebruikersnaam" en "wachtwoord" moet de gebruikersnaam en wachtwoord gebruikt om aan te melden bij de database klanten.
    Verificatie met behulp van Azure Active Directory met elastische query's wordt momenteel niet ondersteund.

### <a name="external-data-sources"></a>Externe gegevensbronnen
Als u wilt maken van een externe gegevensbron, voer de volgende opdracht op de database Orders: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Externe tabellen
Een externe tabel op de database Orders, die overeenkomt met de definitie van de tabel CustomerInformation wilt maken:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Een voorbeeld elastische database T-SQL-query uitvoert

Als u de externe gegevensbron en uw externe tabellen die u kunt nu T-SQL gebruiken om uw externe tabellen query hebt gedefinieerd. Klik op de database Orders bij het uitvoeren van deze query: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Kosten

De functie van de query elastische database is op dit moment opgenomen in de kosten van uw Azure SQL-Database.  

Zie de [Prijzen van de SQL-Database](/pricing/details/sql-database)voor de prijsinformatie. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->
