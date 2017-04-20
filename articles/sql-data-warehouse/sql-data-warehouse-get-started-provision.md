<properties
   pageTitle="Een SQL-Data Warehouse maken in de portal van Azure | Microsoft Azure"
   description="Informatie over het maken van een Azure SQL Data Warehouse in de portal van Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# <a name="create-an-azure-sql-data-warehouse"></a>Een Azure SQL datawarehouse maken

> [AZURE.SELECTOR]
- [Azure-portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Deze zelfstudie gebruikt de Azure-portal maken van een SQL-Data Warehouse die een voorbeelddatabase AdventureWorksDW bevat.


## <a name="prerequisites"></a>Vereisten voor

Als u wilt beginnen, hebt u het volgende nodig:

- **Azure-account**: Bezoek [Azure gratis proefversie][] of [MSDN Azure tegoeden][] om een account te maken.
- **Azure SQL-server**: Zie [een logische Azure SQL Database-server in de portal van Azure maken][] voor meer informatie.

> [AZURE.NOTE] Maken van een SQL-Data Warehouse kan resulteren in een nieuwe factureerbare service.  Zie [SQL Data Warehouse prijzen][] voor meer informatie.

## <a name="create-a-sql-data-warehouse"></a>Een SQL-datawarehouse maken

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).

2. Klik op **+ nieuwe** > **gegevens + opslagruimte** > **SQL datawarehouse**.

    ![Maken](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. In het blad **SQL Data Warehouse** , vul de informatie die nodig zijn, en druk vervolgens 'Maken' om te maken.

    ![Database maken](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Server**: wordt u aangeraden de server eerst.  

    - **Naam van de database**: de naam die wordt gebruikt om te verwijzen naar de SQL-Data Warehouse.  Dit moet uniek zijn voor de server.
    
    - **Prestaties**: het is raadzaam beginnen met 400 [DWUs][DWU]. U kunt de schuifregelaar naar links of naar rechts de prestaties van uw gegevenswarehouse aanpassen of omhoog of omlaag schaal nadat het is gemaakt.  Meer informatie over DWUs, leest u onze documentatie op [schaal](./sql-data-warehouse-manage-compute-overview.md) of onze [pagina prijzen][SQL Data Warehouse prijzen]. 

    - **Abonnement**: Selecteer het [abonnement] waaraan dit SQL Data Warehouse wordt factureren aan.

    - **Resourcegroep**: [resourcegroepen] [ Resource group] zijn containers zo ontworpen dat u een verzameling Azure bronnen beheren. Meer informatie over [resourcegroepen](../azure-resource-manager/resource-group-overview.md).

    - **Bron selecteren**: klik op **bron selecteren** > **voorbeeld**. Azure vult automatisch de optie **selectie** met AdventureWorksDW.

> [AZURE.NOTE] De standaardsortering voor een SQL-Data Warehouse is SQL_Latin1_General_CP1_CI_AS. Als een andere sortering nodig is, kan de [T-SQL][] worden gebruikt om te maken van de database met een andere sortering.

4. Klik op **maken** om te maken van uw SQL Data Warehouse.

5. Wacht een paar minuten. Wanneer uw BTW-records voor de gegevens klaar is, moet u bij de [portal van Azure](https://portal.azure.com)worden geretourneerd. U kunt uw SQL Data Warehouse vinden in uw dashboard, weergegeven bij uw SQL-Databases, of in de resourcegroep waarmee u deze hebt gemaakt. 

    ![Portal weergeven](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## <a name="next-steps"></a>Volgende stappen

Nu dat u een SQL-Data Warehouse hebt gemaakt, kunt u klaar bent om te [verbinden](./sql-data-warehouse-connect-overview.md) en begint query's uitvoeren.

Als u wilt gegevens laadt in SQL Data Warehouse, raadpleegt u het [laden van overzicht](./sql-data-warehouse-overview-load.md).

Als u probeert een bestaande database migreren naar SQL Data Warehouse, raadpleegt u het [overzicht van de migratie](./sql-data-warehouse-overview-migrate.md) of [Hulpprogramma voor migratie](./sql-data-warehouse-migrate-migration-utility.md)gebruiken.

Firewallregels kunnen ook worden geconfigureerd met Transact-SQL. Zie [sp_set_firewall_rule][] en [sp_set_database_firewall_rule][]voor meer informatie.

Het is ook een goed idee om de [Aanbevolen procedures][]te bekijken.

<!--Article references-->
[Een logische Azure SQL Database-server maken met de portal van Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../resource-group-template-deploy-portal.md
[Aanbevolen procedures]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[abonnement]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse prijzen]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure gratis proefversie]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure tegoeden]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

