<properties
   pageTitle="Transparante gegevenscodering (TDE) voor SQL Server-uitrekken Database op Azure TSQL inschakelen | Microsoft Azure"
   description="Transparante gegevenscodering (TDE) voor SQL Server-uitrekken Database op Azure TSQL inschakelen"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Transparante gegevensversleuteling (TDE) inschakelen voor uitrekken Database op Azure (Transact-SQL)
> [AZURE.SELECTOR]
- [Azure-portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Transparante gegevens versleuteling (TDE) helpt beschermen tegen schadelijke activiteit door realtime coderen en decoderen van de database, gekoppeld back-ups en transactie-logbestanden in rust zonder wijzigingen in de toepassing in te voeren.

TDE versleutelt de opslag van een volledige database met behulp van een symmetric sleutel database versleutelingssleutel genoemd. De databasesleutel is beveiligd met een ingebouwde servercertificaat. Het ingebouwde servercertificaat is uniek voor elke Azure server. Microsoft automatisch Hiermee draait u deze certificaten op minstens elke 90 dagen duurt. Zie voor een algemene beschrijving van TDE, [Transparante gegevens versleuteling (TDE)].

##<a name="enabling-encryption"></a>Codering inschakelen

Als u wilt TDE inschakelen voor een Azure database die de gegevens worden opgeslagen gemigreerd uit een uitrekken ingeschakelde SQL Server-database, het volgende doen:

1. Verbinding maken met *de hoofddatabase op de Azure server waarop de database met een aanmelding die een beheerder of een lid van de rol **dbmanager** in de hoofddatabase*
2. Voer de volgende instructie voor het coderen van de database.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Codering uitschakelen

Om te schakelen TDE voor een Azure de database die de gegevens worden opgeslagen die zijn gemigreerd van een uitrekken ingeschakelde SQL Server-database, het volgende doen:

1. Verbinding maken met *de hoofddatabase met een aanmelding die een beheerder of een lid van de rol **dbmanager** in de hoofddatabase*
2. Voer de volgende instructie voor het coderen van de database.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Bevestigt versleuteling

Als u wilt controleren of de status versleuteling voor een Azure-database die de gegevens worden opgeslagen gemigreerd van een uitrekken ingeschakelde SQL Server-database, het volgende doen:

1. Verbinding maken met de *basispagina* of exemplaar database met een aanmelding die een beheerder of een lid van de rol **dbmanager** in de hoofddatabase
2. Voer de volgende instructie voor het coderen van de database.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Als resultaat ```1``` geeft aan dat een versleutelde database, ```0``` geeft aan dat een niet-versleutelde database.


<!--Anchors-->
[Transparante gegevensversleuteling (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
