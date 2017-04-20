<properties
    pageTitle="Maken en beheren scaled out Azure SQL-Databases met behulp van elastische taken | Micosoft Azure"
    description="Doorloop maken en beheren van een taak elastische database."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Maken en beheren scaled out Azure SQL-Databases met behulp van elastische taken (preview)

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)


Beheer van groepen van databases vereenvoudigen **elastische Database taken** door beheerdersbewerkingen zoals wijzigingen in het schema, referenties management, verwijzing Gegevensupdates, prestatiegegevens verzamelen of tenant (klant) telemetrielogboek siteverzameling uit te voeren. Elastische taken van de Database is momenteel beschikbaar via de portal van Azure en de PowerShell-cmdlets. Echter beperkte de Azure portal vlakken functionaliteit beperkt tot het uitvoeren van over alle databases in een [groep elastische Database (preview)](sql-database-elastic-pool.md). Zie toegang tot aanvullende voorzieningen en scripts worden uitgevoerd binnen een groep van databases, met inbegrip van een siteverzameling aangepast of een shard set (gemaakt met behulp van [elastische Database clientbibliotheek](sql-database-elastic-scale-introduction.md)), [maken en beheren van taken via PowerShell](sql-database-elastic-jobs-powershell.md). Zie voor meer informatie over taken, [elastische Database taken overzicht](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Vereisten voor

* Een Azure-abonnement. Zie voor een gratis proefversie, [gratis proefversie voor één maand](https://azure.microsoft.com/pricing/free-trial/).
* Een elastische database-toepassingen. Zie [over elastische database van toepassingen](sql-database-elastic-pool.md)
* Installatie van de onderdelen van de service taak elastische database. Zie [de service van de taak elastische database installeren](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Taken te maken

1. Klik met de [portal van Azure](https://portal.azure.com)uit een bestaande taak van toepassingen elastische database op **taak maken**.
2. Typ in de gebruikersnaam en wachtwoord van de beheerder van de database (gemaakt tijdens de installatie van taken) voor de database taken (metagegevensopslag voor taken).

    ![De taak een naam, typ of plak code en klik op uitvoeren][1]
2. Typ een naam voor de taak in het blad **Taak maken** .
3. Typ de gebruikersnaam en wachtwoord verbinding maken met de doeldatabases met voldoende machtigingen uitvoeren van scripts te kunnen uitvoeren.
4. Plak of typ in het script T-SQL.
5. Klik op **Opslaan** en klik vervolgens op **uitvoeren**.

    ![Taken maken en uitvoeren][5]

## <a name="run-idempotent-jobs"></a>Idempotency is ingeschakeld taken uitvoeren

Wanneer u een script op een set van databases uitvoeren, kunt u ervoor dat het script idempotency is ingeschakeld moet zijn. Dat wil zeggen moet het script kunnen uitvoeren van meerdere keren, zelfs als deze voordat in onvolledig is mislukt. Bijvoorbeeld wanneer een script mislukt, de taak wordt automatisch opnieuw geprobeerd totdat deze is uitgevoerd (in een bepaald bereik, als de poging logica uiteindelijk niet meer het opnieuw). De manier hiervoor is via de een "Als bestaat"-component en eventuele gevonden exemplaar verwijderen voordat er een nieuw object wordt gemaakt. Hier ziet u een voorbeeld:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

U kunt ook de component 'IF niet bestaat' voordat u een nieuw exemplaar gebruiken:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Dit script werkt vervolgens de tabel die eerder hebt gemaakt.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Foutcontrole taakstatus

Nadat u een taak is begonnen, kunt u op de voortgang controleren.

1. Klik op **taken beheren**vanaf de pagina van de groep elastische database.

    ![Klik op "Projecten beheren"][2]

2. Klik op de naam (a) van een taak. De **STATUS** kan worden "Voltooid" of "Mislukt". Details van het project weergegeven (b) met de datum en tijd van maken en uitvoeren. De lijst (c) onder de die wordt de voortgang van het script ten opzichte van elke database in de groep, geeft de datum en tijd details.

    ![Een voltooide taak controleren][3]


## <a name="checking-failed-jobs"></a>Controleren is mislukt taken

Als een taak mislukt, wordt een logboek van de uitvoering kunt vinden. Klik op de naam van de mislukte taak om de details weer te geven.

![Een mislukte taak controleren][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
