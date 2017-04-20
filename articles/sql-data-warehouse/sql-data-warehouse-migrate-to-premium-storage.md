<properties
   pageTitle="Uw bestaande Azure SQL Data Warehouse migreren naar premium opslag | Microsoft Azure"
   description="Instructies voor het migreren van een bestaande SQL Data Warehouse naar premium-opslag"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="nicw;barbkess;sonyama"/>

# <a name="migration-to-premium-storage-details"></a>Migratie naar Premium opslag Details
SQL Data Warehouse geïntroduceerd onlangs [Premium-opslag voor groter prestaties voorspelbaarheid][].  We gaan nu bestaande datawarehouses momenteel op standaardopslag migreren naar Premium opslag.  Lees verder voor meer informatie over hoe en wanneer automatische migraties optreden en hoe u zelf migreren als u wilt bepalen wanneer de downtime plaatsvindt.

Als u meer dan één Data Warehouse hebt, gebruikt u de onderstaande [automatische migratie plannen][] om te bepalen wanneer dit wordt ook gemigreerd.

## <a name="determine-storage-type"></a>Opslagtype bepalen
Als u een DW voordat u de onderstaande datums gemaakt, gebruikt u momenteel standaardopslag.  Elke Data Warehouse op standaardopslag die is onderhevig aan automatische migratie heeft een kennisgeving aan de bovenkant van het blad Data Warehouse in de [Portal van Azure][] met de mededeling dat "*een latere versie bijwerken naar premium opslag vereist een storing.  Leer meer ->*. "

| **Regio**          | **DW gemaakt voor deze datum**   |
| :------------------ | :-------------------------------- |
| Australië Oost      | Premium opslag nog niet beschikbaar |
| Australië Zuidoost | 5 augustus 2016                    |
| Brazilië Zuid        | 5 augustus 2016                    |
| Canada centraal      | 25 mei 2016                      |
| Canada Oost         | 26 mei 2016                      |
| Central US          | 26 mei 2016                      |
| China Oost          | Premium opslag nog niet beschikbaar |
| China Noord         | Premium opslag nog niet beschikbaar |
| Oost-Azië           | 25 mei 2016                      |
| Oost-VS             | 26 mei 2016                      |
| Oost-US2            | 27 mei 2016                      |
| India centraal       | 27 mei 2016                      |
| India Zuid         | 26 mei 2016                      |
| India West          | Premium opslag nog niet beschikbaar |
| Japan Oost          | 5 augustus 2016                    |
| Japan West          | Premium opslag nog niet beschikbaar |
| Centraal Noord-Amerikaanse    | Premium opslag nog niet beschikbaar |
| Noord-Europa        | 5 augustus 2016                    |
| Zuid-Central US    | 27 mei 2016                      |
| Zuidoost-Azië      | 24 mei 2016                      |
| West Europa         | 25 mei 2016                      |
| West Centraal VS     | 26 augustus 2016                   |
| West VS             | 26 mei 2016                      |
| West US2            | 26 augustus 2016                   |

## <a name="automatic-migration-details"></a>Automatische migratie details
Standaard wordt we uw database voor u tijdens de 18: 00 uur en 6 uur in uw regio van de lokale tijd migreren tijdens de [migratie van automatische planning][] hieronder.  Uw bestaande Data Warehouse kunnen niet worden gebruikt tijdens de migratie.  We verwachten dat de migratie het ongeveer een uur per TB aan opslagcapaciteit per Data Warehouse duurt.  We zijn er ook voor zorgen dat u geen btw tijdens een gedeelte van de automatische migratie geheven.

> [AZURE.NOTE] Niet is mogelijk om te gebruiken van uw bestaande Data Warehouse tijdens de migratie.  Nadat de migratie voltooid is, wordt uw Data Warehouse weer online zijn.

De onderstaande details zijn stappen dat Microsoft namens u neemt om de migratie te voltooien en niet voor eventuele betrokkenheid van uw kant vereist is.  In dit voorbeeld Stel dat uw bestaande DW op standaardopslag is momenteel met de naam "MyDW."

1.  Microsoft wijzigt de naam van 'MyDW' naar "MyDW_DO_NOT_USE_ [tijdstempel]"
2.  Microsoft onderbreekt "MyDW_DO_NOT_USE_ [tijdstempel]."  Tijdens deze periode, een back-up is die u hebt gemaakt.  U ziet mogelijk meerdere onderbreken/cv's als we eventuele problemen tijdens dit proces optreden.
3.  Microsoft Hiermee maakt u een nieuwe DW met de naam 'MyDW' op Premium opslag uit de back-up die u hebt gemaakt in stap 2.  "MyDW" wordt niet weergegeven totdat nadat het herstellen voltooid is.
4.  Zodra de herstellen voltooid is, "MyDW" geeft als resultaat het dezelfde DWUs en onderbroken of actieve staat die deze voordat de migratie was.
5.  Nadat de migratie voltooid is, Microsoft Hiermee verwijdert u "MyDW_DO_NOT_USE_ [tijdstempel]"
    
> [AZURE.NOTE] Deze instellingen overgenomen niet als onderdeel van de migratie:
> 
>   -  Controle op het niveau van de Database moet opnieuw worden ingeschakeld
>   -  Firewallregels op het niveau van de **Database** moeten worden opnieuw wordt toegevoegd.  Firewallregels op **serverniveau** zijn niet van invloed zijn op.

### <a name="automatic-migration-schedule"></a>Automatische migratie plannen
Automatische migraties optreden uit 18: 00-06: 00 (lokale tijd per regio) tijdens de volgende storing planning.

| **Regio**          | **Geschatte begindatum**     | **Geschatte einddatum in te voeren**       |
| :------------------ | :--------------------------- | :--------------------------- |
| Australië Oost      | Nog niet wordt bepaald           | Nog niet wordt bepaald           |
| Australië Zuidoost | 10 augustus 2016              | 24 augustus 2016              |
| Brazilië Zuid        | 10 augustus 2016              | 24 augustus 2016              |
| Canada centraal      | 23 juni 2016                | 1 juli 2016                 |
| Canada Oost         | 23 juni 2016                | 1 juli 2016                 |
| Central US          | 23 juni 2016                | 4 juli 2016                 |
| China Oost          | Nog niet wordt bepaald           | Nog niet wordt bepaald           |
| China Noord         | Nog niet wordt bepaald           | Nog niet wordt bepaald           |
| Oost-Azië           | 23 juni 2016                | 1 juli 2016                 |
| Oost-VS             | 23 juni 2016                | 11 juli 2016                |
| Oost-US2            | 23 juni 2016                | 8 juli 2016                 |
| India centraal       | 23 juni 2016                | 1 juli 2016                 |
| India Zuid         | 23 juni 2016                | 1 juli 2016                 |
| India West          | Nog niet wordt bepaald           | Nog niet wordt bepaald           |
| Japan Oost          | 10 augustus 2016              | 24 augustus 2016              |
| Japan West          | Nog niet wordt bepaald           | Nog niet wordt bepaald           |
| Centraal Noord-Amerikaanse    | Nog niet wordt bepaald           | Nog niet wordt bepaald           |
| Noord-Europa        | 10 augustus 2016              | 31 augustus 2016              |
| Zuid-Central US    | 23 juni 2016                | 2 juli 2016                 |
| Zuidoost-Azië      | 23 juni 2016                | 1 juli 2016                 |
| West Europa         | 23 juni 2016                | 8 juli 2016                 |
| West Centraal VS     | 14 augustus 2016              | 31 augustus 2016              |
| West VS             | 23 juni 2016                | 7 juli 2016                 |
| West US2            | 14 augustus 2016              | 31 augustus 2016              |

## <a name="self-migration-to-premium-storage"></a>Zelf migratie naar Premium-opslag
Als u bepalen wanneer uw downtime plaatsvindt wilt, kunt u de volgende stappen uit om te migreren van een bestaande Data Warehouse op standaardopslag naar Premium opslag.  Als u besluit om zelf te migreren, moet u de zelf migratie uitvoeren voordat u de automatische migratie begint in dat gebied dat elk gevaar van de automatische migratie een conflict veroorzaken (Zie de [automatische migratie planning][]).

### <a name="self-migration-instructions"></a>Zelf migratie instructies
Als u uw downtime bepalen wilt, kunt u zelf uw Data Warehouse migreren met behulp van back-up/herstellen.  Het gedeelte herstellen van de migratie is ongeveer een uur per TB aan opslagcapaciteit per DW verwacht.  Als u dezelfde naam houden wilt nadat de migratie is voltooid, kunt u de stappen voor de [stappen om de naam tijdens de migratie te][]volgen. 

1.  [Een pauze invoegen][] uw DW waarmee een automatische back-up
2.  [Terugzetten][] vanuit uw meest recente momentopname
3.  Uw bestaande DW op standaard opslagruimte verwijderen. **Als u niet moet deze stap, moet u betalen voor beide DWs.**

> [AZURE.NOTE] Deze instellingen overgenomen niet als onderdeel van de migratie:
> 
>   -  Controle op het niveau van de Database moet opnieuw worden ingeschakeld
>   -  Firewallregels op het niveau van de **Database** moeten worden opnieuw wordt toegevoegd.  Firewallregels op **serverniveau** zijn niet van invloed zijn op.

#### <a name="optional-steps-to-rename-during-migration"></a>Optioneel: stappen om de naam tijdens de migratie te 
Twee databases op dezelfde logische server kunnen niet dezelfde naam hebben. SQL Data Warehouse ondersteunt nu de mogelijkheid om de naam van een DW te.

In dit voorbeeld Stel dat uw bestaande DW op standaardopslag is momenteel met de naam "MyDW."

1.  Naam 'MyDW' met de opdracht ALTER DATABASE dat volgt naar een ander nummer tevreden bent over "MyDW_BeforeMigration."  Deze opdracht alle bestaande transacties beëindigd en moet worden uitgevoerd in de hoofddatabase te kunnen uitvoeren.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.  [Een pauze invoegen][] "MyDW_BeforeMigration" waarmee een automatische back-up
3.  Een nieuwe database maken met de naam die u hebt gebruikt om te laten [herstellen][] uit uw meest recente momentopname (ex: "MyDW")
4.  "MyDW_BeforeMigration" verwijderen.  **Als u niet moet deze stap, moet u betalen voor beide DWs.**

> [AZURE.NOTE] Deze instellingen overgenomen niet als onderdeel van de migratie:
> 
>   -  Controle op het niveau van de Database moet opnieuw worden ingeschakeld
>   -  Firewallregels op het niveau van de **Database** moeten worden opnieuw wordt toegevoegd.  Firewallregels op **serverniveau** zijn niet van invloed zijn op.

## <a name="next-steps"></a>Volgende stappen
Met de wijziging van Premium opslag, hebben we ook het aantal blob databasebestanden in de onderliggende architectuur van uw Data Warehouse verhoogd.  Als u wilt de prestaties voordelen van deze wijziging, is het raadzaam dat u uw Columnstore gegroepeerde indexen met het volgende script opnieuw.  De onderstaande script werkt door af te dwingen deel van uw bestaande gegevens aan de extra BLOB's.  Als u geen actie ondernemen, wordt de gegevens natuurlijk verspreiden na verloop van tijd als u meer gegevens in uw tabellen Data Warehouse laden.

**Minimumvereisten:**

1.  Datawarehouse moet worden uitgevoerd met 1000 DWUs of hoger (Zie [schaal rekenkracht][])
2.  Gebruiker die het script moet zijn in de [rol van mediumrc][] of hoger
    1.  Als u wilt een gebruiker toevoegen aan deze rol, uitvoeren het volgende: 
        1.  ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select 
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from 
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go
 
--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Als u problemen hebt met uw Data Warehouse, het [maken van een ondersteuningsticket][] en de verwijzing 'Migratie naar Premium opslag' als de mogelijke oorzaak.

<!--Image references-->

<!--Article references-->
[automatische migratie plannen]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[een ondersteuningsticket maken]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Een pauze invoegen]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Herstellen]: sql-data-warehouse-restore-database-portal.md
[stappen om de naam tijdens de migratie te]: #optional-steps-to-rename-during-migration
[rekenkracht schaal]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[mediumrc rol]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Premium-opslag voor groter prestaties voorspelbaarheid]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure-Portal]: https://portal.azure.com
