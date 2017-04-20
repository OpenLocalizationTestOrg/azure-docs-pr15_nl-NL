<properties
   pageTitle="Een SQL Azure datawarehouse (Portal) herstellen | Microsoft Azure"
   description="Azure portal taken voor het herstellen van een Azure SQL Data Warehouse."
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

# <a name="restore-an-azure-sql-data-warehouse-portal"></a>Een SQL Azure datawarehouse (Portal) herstellen

> [AZURE.SELECTOR]
- [Overzicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In dit artikel leert u hoe u herstelt een Azure SQL Data Warehouse met behulp van de Azure-Portal.

## <a name="before-you-begin"></a>Voordat u begint

**Controleer of de capaciteit van de DTU.** Elke SQL Data Warehouse wordt gehost door een SQL-server (bijvoorbeeld myserver.database.windows.net) waarvoor een standaard DTU quotum.  Voordat u een SQL-Data Warehouse terugzetten kunt, Controleer de uw SQL-server heeft onvoldoende resterende DTU quotum voor de database wordt teruggezet. Informatie over het berekenen van DTU nodig of dat er meer DTU, raadpleegt u [een DTU quotum wijziging aanvragen][].


## <a name="restore-an-active-or-paused-database"></a>Een database actief of onderbroken terugzetten

Een database terugzetten:

1. Meld u aan bij de [portal van Azure][]
2. Aan de linkerkant van het scherm Selecteer **Bladeren** en selecteer vervolgens **SQL-servers**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Ga naar de server en selecteert u deze
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. De SQL-Data Warehouse die u wilt herstellen uit en selecteert u deze zoeken
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Aan de bovenkant van het blad Data Warehouse, klikt u op **herstellen**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Een nieuwe **naam van de Database** opgeven
7. Selecteer de meest recente **Herstellen punt**
    1. Zorg ervoor dat u de meest recente herstellen komma kiezen.  Aangezien herstellen wordt verwezen in UTC worden getoond, is soms de standaardoptie weergegeven niet het meest recente herstellen.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Klik op **OK**
9. Het herstelproces database wordt gestart en kan worden gecontroleerd met **meldingen**

>[AZURE.NOTE] Nadat de herstellen is voltooid, kunt u de herstelde database door de volgende [configureren van uw database na herstel][]configureren.


## <a name="restore-a-deleted-database"></a>Een verwijderde database terugzetten

Een verwijderde database herstellen:

1. Meld u aan bij de [portal van Azure][]
2. Aan de linkerkant van het scherm Selecteer **Bladeren** en selecteer vervolgens **SQL-servers**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Ga naar de server en selecteert u deze
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Schuif omlaag naar de sectie bewerkingen op van de server blade
5. Klik op de tegel **Databases verwijderd**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Selecteer de verwijderde database die u wilt herstellen
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Een nieuwe **naam van de Database** opgeven
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Klik op **OK**
9. Het herstelproces database wordt gestart en kan worden gecontroleerd met **meldingen**

>[AZURE.NOTE] Zie het [configureren van uw database na herstel][]uw database configureren nadat de herstellen is voltooid. 

## <a name="next-steps"></a>Volgende stappen
Lees meer informatie over de functies van de bedrijfscontinuïteit bedrijven van Azure SQL Database-edities, het [Azure SQL-Database business bedrijfscontinuïteit overzicht][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Database business bedrijfscontinuïteit-overzicht]: ./sql-database-business-continuity.md
[Overzicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[De database na herstel configureren]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Een DTU quotum wijziging aanvragen]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure-portal]: https://portal.azure.com/
