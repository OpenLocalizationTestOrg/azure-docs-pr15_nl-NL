<properties
    pageTitle="Overzicht: beheerprogramma's voor SQL-Database | Microsoft Azure"
    description="Hulpprogramma's en opties voor het beheren van Azure SQL-Database wordt vergeleken"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Overzicht: beheerprogramma's voor SQL-Database

In dit onderwerp behandelt en hulpprogramma's en opties voor het beheren van Azure SQL-databases, zodat u het juiste hulpmiddel voor de taak, uw bedrijf en u kunt kiezen wordt vergeleken. De juiste hulpmiddel is afhankelijk van hoeveel databases kiezen beheert u, de taak en hoe vaak een taak is uitgevoerd.

## <a name="azure-portal"></a>Azure-portal

De [Azure-portal](https://portal.azure.com) is een webtoepassing waar u kunt maken, bijwerken, en databases en logische servers verwijderen en controleren van de activiteit in een database. Dit hulpmiddel is handig als u gaat voor het eerst met Azure-databases, een paar beheren of wilt bewerkingen snel uitvoeren.

Zie de [SQL-Databases beheren met behulp van de Azure portal](sql-database-manage-portal.md)voor meer informatie over het gebruik van de portal.

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio en SQL Server Data Tools in Visual Studio

SQL Server Management Studio (SSMS) en SQL Server Data Tools (SSDT) zijn voor clienthulpprogramma's die op uw computer voor het beheren en ontwikkelen van uw database in de cloud worden uitgevoerd. Als u vertrouwd met Visual Studio of andere geïntegreerde ontwikkelomgeving biedt (IDEs), [gebruikt u SSDT in Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)toepassingen ontwikkelt bent. Veel databasebeheerders bent bekend met SSMS, die kan worden gebruikt met Azure SQL-databases. [Download de meest recente versie van SSMS](https://msdn.microsoft.com/library/mt238290) en altijd de meest recente versie gebruiken tijdens het werken met Azure SQL-Database. Zie voor meer informatie over het beheren van uw Azure SQL-Databases met SSMS [SQL-Databases beheren SSMS](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Gebruik altijd de meest recente versie van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) en [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) moet blijven gesynchroniseerde met updates voor Microsoft Azure en SQL-Database.


## <a name="powershell"></a>PowerShell

U kunt PowerShell gebruiken voor het beheren van databases en elastische database van toepassingen en Azure resource implementaties automatiseren. Microsoft raadt dit hulpmiddel voor het beheren van een groot aantal databases en implementatie en wijzigingen van de resource in een productieomgeving automatiseren.

Zie voor meer informatie [SQL-Database beheren met PowerShell](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Elastische hulpmiddelen voor databases
Gebruik van de Databasehulpmiddelen elastische acties wilt uitvoeren, zoals 

* Een T-SQL-script ten opzichte van een reeks databases met een [elastische taak](sql-database-elastic-jobs-overview.md) uitvoeren
* Meerdere tenant model databases verplaatsen naar een enkel-tenant model met het [hulpmiddel gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md)
* Het beheren van databases in een enkel-tenant-model of een multi-tenant met behulp van de [elastische schalen client-bibliotheek](sql-database-elastic-database-client-library.md).
 

## <a name="additional-resources"></a>Aanvullende informatie

- [Azure resourcemanager](https://azure.microsoft.com/features/resource-manager/)
- [Azure automatisering](https://azure.microsoft.com/documentation/services/automation/)
- [Azure Scheduler](https://azure.microsoft.com/documentation/services/scheduler/)