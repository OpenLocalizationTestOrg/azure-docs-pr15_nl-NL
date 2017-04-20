<properties
   pageTitle="Transparante gegevensversleuteling in SQL datawarehouse (T-SQL) | Microsoft Azure"
   description="Transparante gegevensversleuteling (TDE) in SQL datawarehouse (T-SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Aan de slag met transparante gegevens versleuteling (TDE)


> [AZURE.SELECTOR]
- [Beveiligingsoverzicht](sql-data-warehouse-overview-manage-security.md)
- [Verificatie](sql-data-warehouse-authentication.md)
- [Versleuteling (Portal)](sql-data-warehouse-encryption-tde.md)
- [Versleuteling (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Vereiste bevoegdheden

Als u wilt inschakelen transparante gegevens versleuteling (TDE), moet u een beheerder of een lid van de rol dbmanager zijn.

## <a name="enabling-encryption"></a>Codering inschakelen

Volg deze stappen om TDE inschakelen voor een SQL-Data Warehouse:

1. Verbinding maken met *de hoofddatabase op de server waarop de database met een aanmelding die een beheerder of een lid van de rol **dbmanager** in de hoofddatabase*
2. Voer de volgende instructie voor het coderen van de database.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Codering uitschakelen

Volg deze stappen om TDE voor een SQL-Data Warehouse uitschakelen:

1. Verbinding maken met *de hoofddatabase met een aanmelding die een beheerder of een lid van de rol **dbmanager** in de hoofddatabase*
2. Voer de volgende instructie voor het coderen van de database.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] Een onderbroken SQL Data Warehouse moet worden hervat voordat u wijzigingen aanbrengt in de TDE-instellingen.

## <a name="verifying-encryption"></a>Bevestigt versleuteling

U kunt bevestigen versleuteling status voor een SQL-Data Warehouse door de volgende stappen uit te voeren:

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

## <a name="encryption-dmvs"></a>Versleuteling DMVs  

- [vinden][] 
- [sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[vinden]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
