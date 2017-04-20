<properties
   pageTitle="Transparante gegevensversleuteling in SQL datawarehouse (Portal) | Microsoft Azure"
   description="Transparante gegevensversleuteling (TDE) in SQL datawarehouse"
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

# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Aan de slag met transparante gegevens versleuteling (TDE) in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Beveiligingsoverzicht](sql-data-warehouse-overview-manage-security.md)
- [Verificatie](sql-data-warehouse-authentication.md)
- [Versleuteling (Portal)](sql-data-warehouse-encryption-tde.md)
- [Versleuteling (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Vereiste bevoegdheden

Als u wilt inschakelen transparante gegevens versleuteling (TDE), moet u een beheerder of een lid van de rol dbmanager zijn.

## <a name="enabling-encryption"></a>Codering inschakelen

Schakel TDE voor een SQL-Data Warehouse door de volgende stappen uit te voeren:

1. Open de database in de [portal van Azure](https://portal.azure.com)
2. Klik in het blad database op de knop **Instellingen**
3. Selecteer de optie **transparante gegevensversleuteling**![][1]
4. Selecteer de instelling **op**![][2]
5. Selecteer **Opslaan**
![][3]  

## <a name="disabling-encryption"></a>Codering uitschakelen

Om te schakelen TDE voor een SQL-Data Warehouse, de volgende stappen uit te voeren:

1. Open de database in de [portal van Azure](https://portal.azure.com)
2. Klik in het blad database op de knop **Instellingen**
3. Selecteer de optie **transparante gegevensversleuteling**![][1]
4. Selecteer de instelling **uitschakelen**![][4]
5. Selecteer **Opslaan**
![][5]  

## <a name="encryption-dmvs"></a>Versleuteling DMVs

Versleuteling kan worden bevestigd met de volgende DMVs:

- [vinden]
- [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[vinden]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
