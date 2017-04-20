<properties
   pageTitle="SQL Data Warehouse maken met behulp van PowerShell | Microsoft Azure"
   description="SQL Data Warehouse maken met behulp van PowerShell"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>SQL Data Warehouse via PowerShell maken

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In dit artikel leest u het maken van een SQL-Data Warehouse via PowerShell.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt beginnen, hebt u het volgende nodig:

- **Azure-account**: Bezoek [Azure gratis proefversie][] of [MSDN Azure tegoeden][] om een account te maken.
- **Azure SQL-server**: [een logische Azure SQL Database-server in de Portal van Azure maken][] of [maken een logische Azure SQL Database-server met PowerShell][] Zie voor meer informatie.
- **Resourcegroep**: dezelfde resourcegroep gebruiken als uw Azure SQL-server of Lees [hoe u een resourcegroep maken][].
- **PowerShell versie 1.0.3 of groter**: U kunt uw versie controleren door te voeren **Get-Module - ListAvailable-naam Azure**.  De meest recente versie kan worden geïnstalleerd van [Microsoft Web Platform Installer][].  Zie voor meer informatie over het installeren van de meest recente versie [installeren en configureren van Azure PowerShell][].

> [AZURE.NOTE] Maken van een SQL-Data Warehouse, kan dit leiden tot een nieuwe factureerbare service.  Zie [SQL Data Warehouse prijzen][] voor meer informatie over de prijzen.

## <a name="create-a-sql-data-warehouse"></a>Een SQL-datawarehouse maken

1. Open Windows PowerShell.
2. Deze cmdlet uitvoeren om aan te melden aan Azure resourcemanager.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Selecteer het abonnement dat u wilt gebruiken voor de huidige sessie.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Database maken. In dit voorbeeld wordt gemaakt van een database met de naam 'mynewsqldw', met service doelstelling niveau "DW400", op de server met de naam 'sqldwserver1', dat wil in de resourcegroep met de naam 'mywesteuroperesgp1 zeggen'.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Vereiste Parameters zijn:

- **RequestedServiceObjectiveName**: de hoeveelheid [DWU][] u aanvraagt.  Ondersteunde waarden zijn: DW100, DW200 DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 en DW6000.
- **Databasenaam**: de naam van de SQL-Data Warehouse die u maakt.
- **Servernaam**: de naam van de server die u om te maken gebruikt (moet V12 zijn).
- **ResourceGroupName**: resourcegroep die u gebruikt.  Groepen in uw abonnement gebruiken om te zoeken beschikbaar resource Get-AzureResource.
- **Edition**: "DataWarehouse" moet maken van een SQL-Data Warehouse.

Optionele Parameters zijn:

- **CollationName**: de standaardsortering als niet opgegeven SQL_Latin1_General_CP1_CI_AS is.  Sortering kan niet worden gewijzigd in een database.
- **MaxSizeBytes**: de maximale grootte van een database is 10 GB.


Zie voor meer informatie over de parameteropties voor de, [Nieuw-AzureRmSqlDatabase][] en [Database maken (Azure SQL Data Warehouse)][].

## <a name="next-steps"></a>Volgende stappen

Nadat de inrichting van uw SQL Data Warehouse is voltooid wilt u mogelijk Laad [Voorbeeldgegevens][] of informatie over het [ontwikkelen][], [laden][]of [migreren][]controleren.

Als u meer informatie over het beheren van SQL Data Warehouse via programmacode geïnteresseerd bent, raadpleegt u onze artikel over het gebruik van de [PowerShell-cmdlets en REST API's][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[migreren]: ./sql-data-warehouse-overview-migrate.md
[ontwikkelen]: ./sql-data-warehouse-overview-develop.md
[laden]: ./sql-data-warehouse-load-with-bcp.md
[Voorbeeldgegevens laden]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell-cmdlets en REST API 's]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Het installeren en configureren van Azure PowerShell]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Een logische Azure SQL Database-server maken met de Portal van Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Een logische Azure SQL Database-server maken met PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[het maken van een resourcegroep]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Nieuwe AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Database (Azure SQL datawarehouse) maken]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse prijzen]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure gratis proefversie]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure tegoeden]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
