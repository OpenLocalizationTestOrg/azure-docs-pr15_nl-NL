<properties
   pageTitle="Een SQL-datawarehouse maken met TSQL | Microsoft Azure"
   description="Informatie over het maken van een Azure SQL Data Warehouse met TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Een SQL Data Warehouse-database maken met behulp van Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In dit artikel leest u het maken van een SQL-Data Warehouse met T-SQL.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt beginnen, hebt u het volgende nodig: 

- **Azure-account**: Bezoek [Azure gratis proefversie][] of [MSDN Azure tegoeden][] om een account te maken.
- **Azure SQL-server**: [een logische Azure SQL Database-server in de Portal van Azure maken][] of [maken een logische Azure SQL Database-server met PowerShell][] Zie voor meer informatie.
- **Resourcegroep**: dezelfde resourcegroep gebruiken als uw Azure SQL-server of Lees [hoe u een resourcegroep maken][].
- **Omgeving T-SQL uitvoeren**: kunt u [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][]of [SSMS][] uitvoeren T-SQL.

> [AZURE.NOTE] Maken van een SQL-Data Warehouse, kan dit leiden tot een nieuwe factureerbare service.  Zie [SQL Data Warehouse prijzen][] voor meer informatie over de prijzen.

## <a name="create-a-database-with-visual-studio"></a>Een database maken met Visual Studio

Als u nog niet eerder naar Visual Studio, raadpleegt u het artikel [Query Azure SQL Data Warehouse (Visual Studio)][].  Als u wilt beginnen, SQL Server-Object Explorer openen in Visual Studio en verbinding maken met de server waarop uw SQL Data Warehouse-database.  Wanneer u verbinding hebt, kunt u een SQL-Data Warehouse maken door de volgende SQL-opdracht uit te voeren ten opzichte van **de hoofddatabase** .  Deze opdracht Hiermee maakt u de database MySqlDwDb met een Service doelstelling van DW400 en dat de database kan toenemen tot een maximale grootte van 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Een database maken met sqlcmd

U kunt ook dezelfde opdracht uitvoeren met sqlcmd door de volgende handelingen uit te voeren op een opdrachtprompt.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

De standaardsortering wanneer niet opgegeven is SQL_Latin1_General_CP1_CI_AS sorteren.  De `MAXSIZE` tussen 250 GB en 240 TB kan zijn.  De `SERVICE_OBJECTIVE` kan zijn tussen DW100 en DW2000 [DWU][].  Zie de documentatie MSDN voor [DATABASE maken][]voor een lijst met alle geldige waarden.  MAXSIZE zowel SERVICE_OBJECTIVE kunnen worden gewijzigd met behulp van een [DATABASE wijzigen][] T-SQL-opdracht.  De sortering van een database kan niet worden gewijzigd nadat de is gemaakt.   Let op moet worden gebruikt als de SERVICE_OBJECTIVE als het wijzigen van DWU wijzigt, worden een herstart van services, waarin alle query's in flight annuleert.  Wijzigen van MAXSIZE niet opnieuw wordt opgestart services als ze een eenvoudige Metagegevensbewerking.

## <a name="next-steps"></a>Volgende stappen

Nadat uw SQL Data Warehouse is voltooid inrichting u kunt [Voorbeeldgegevens laden][] of controleren informatie over het [ontwikkelen][], [laden][]of [migreren][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL datawarehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migreren]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md
[laden]: sql-data-warehouse-overview-load.md
[Voorbeeldgegevens laden]: sql-data-warehouse-load-sample-databases.md
[Een logische Azure SQL Database-server maken met de Portal van Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Een logische Azure SQL Database-server maken met PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[het maken van een resourcegroep]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[DATABASE MAKEN]: https://msdn.microsoft.com/library/mt204021.aspx
[DATABASE WIJZIGEN]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse prijzen]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure gratis proefversie]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure tegoeden]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
