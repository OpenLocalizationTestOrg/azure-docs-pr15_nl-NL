<properties
   pageTitle="Een SQL Azure datawarehouse (REST API) herstellen | Microsoft Azure"
   description="REST API taken voor het herstellen van een Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Een SQL Azure datawarehouse (REST API) herstellen

> [AZURE.SELECTOR]
- [Overzicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In dit artikel leert u hoe u herstelt een Azure SQL Data Warehouse de REST API gebruiken.

## <a name="before-you-begin"></a>Voordat u begint

**Controleer of de capaciteit van de DTU.** Elke SQL Data Warehouse wordt gehost door een SQL-server (bijvoorbeeld myserver.database.windows.net) waarvoor een standaard DTU quotum.  Voordat u een SQL-Data Warehouse terugzetten kunt, Controleer de uw SQL-server heeft onvoldoende resterende DTU quotum voor de database wordt teruggezet. Informatie over het berekenen van DTU nodig of dat er meer DTU, raadpleegt u [een DTU quotum wijziging aanvragen][].

## <a name="restore-an-active-or-paused-database"></a>Een database actief of onderbroken terugzetten

Een database terugzetten:

1. De lijst met database herstellen punten met de bewerking Get-Database herstellen punten krijgen.
2. Begin uw herstellen met behulp van de [database maken herstellen verzoek][] bewerking.
3. De status van uw herstellen met behulp van de [status van de Database-bewerking][] bewerking bijhouden.

>[AZURE.NOTE] Nadat de herstellen is voltooid, kunt u de herstelde database door de volgende [configureren van uw database na herstel][]configureren.

## <a name="restore-a-deleted-database"></a>Een verwijderde database terugzetten

Een verwijderde database herstellen:

1.  Een lijst met al uw terug te zetten verwijderde databases met behulp van de [lijst terug te zetten decoratieve databases][] -bewerking.
2.  Voor de verwijderde database die u herstellen wilt met de bewerking [te herstellen decoratieve database ophalen][] , zodat u de details.
3.  Begin uw herstellen met behulp van de [database maken herstellen verzoek][] bewerking.
4.  De status van uw herstellen met behulp van de [status van de Database-bewerking][] bewerking bijhouden.

>[AZURE.NOTE] Zie het [configureren van uw database na herstel][]uw database configureren nadat de herstellen is voltooid. 


## <a name="next-steps"></a>Volgende stappen
Lees meer informatie over de functies van de bedrijfscontinuïteit bedrijven van Azure SQL Database-edities, het [Azure SQL-Database business bedrijfscontinuïteit overzicht][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Database business bedrijfscontinuïteit-overzicht]: ./sql-database-business-continuity.md
[Een DTU quotum wijziging aanvragen]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[De database na herstel configureren]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Overzicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Aanvraag voor het herstellen van database maken]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Status van de database-bewerking]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Terug te zetten decoratieve database ophalen]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Keuzelijst terug te zetten geopend databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
