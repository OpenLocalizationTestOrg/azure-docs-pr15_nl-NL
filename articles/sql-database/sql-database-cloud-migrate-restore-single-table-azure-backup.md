<properties
    pageTitle="Één tabel back-up van Azure SQL-Database terugzetten | Microsoft Azure"
    description="Informatie over het herstellen van één tabel van back-up van Azure SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>


# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Het herstellen van één tabel van een back-up van Azure SQL-Database

Een situatie waarin u sommige gegevens in een SQL-database voor het per ongeluk hebt gewijzigd en nu wilt u de desbetreffende één tabel herstellen kunnen optreden. In dit artikel wordt beschreven hoe één tabel in een database terugzetten uit een van de SQL-Database [Automatische back-ups](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Voorbereidende stappen: de naam van de tabel wijzigen en een kopie van de database herstellen
1. Geef aan dat de tabel in uw Azure SQL-database die u wilt vervangen door de herstelde kopie. Microsoft SQL Management Studio om de naam van de tabel te gebruiken. De naam van de tabel als bijvoorbeeld wijzigen &lt;tabelnaam&gt;_old.

    **Opmerking** Als u wilt voorkomen dat wordt geblokkeerd, zorg ervoor dat er geen activiteit dat wordt uitgevoerd op de tabel waarvan u de naam wijzigt. Als u problemen ondervindt, zorg dat deze procedure uitvoeren tijdens een onderhoudsvenster.

2. Herstel een back-up van uw database een punt in de tijd die u terugzetten wilt naar de stappen [Punt-In_Time herstellen](sql-database-recovery-using-backups.md#point-in-time-restore) uit te voeren.

    **Notities**:
    - De naam van de herstelde database is in de indeling DBName + tijdstempel; bijvoorbeeld: **Adventureworks2012_2016-01-01T22-12Z**. Deze stap overschrijft geen de naam van de bestaande database op de server. Dit is een maateenheid voor de veiligheid en deze toe te staan dat u voor de verificatie van de herstelde database voordat ze hun huidige database weghalen en de naam van de herstelde database voor gebruik in productieomgeving wijzigen is bedoeld.
    - Alle prestaties lagen van eenvoudig naar Premium worden automatisch back-up gemaakt door de service, met variërende back-up bewaarbeleid aan de doelstellingen, afhankelijk van de laag:

| DB herstellen | Eenvoudige laag | Standaard lagen | Premium lagen |
| :-- | :-- | :-- | :-- |
|  Tijdstip herstellen |  Een punt binnen zeven dagen herstellen|Een punt in 35 dagen herstellen| Een punt in 35 dagen herstellen|

## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>De tabel kopiëren uit de herstelde database via het migratieprogramma van SQL-Database
1. Download en installeer de [Migratie-Wizard voor SQL-Database](https://sqlazuremw.codeplex.com).

2. De Wizard SQL Database-migratie, klik op de pagina **Selecteer proces** openen, selecteer **Database onder analyseren/migreren**en klik vervolgens op **volgende**.
![SQL-databasemigratie wizard - proces selecteren](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)
3. Pas de volgende instellingen in het dialoogvenster **verbinding maken met de Server** :
 - **Servernaam**: exemplaar van uw SQL Azure wordt aangegeven
 - **Verificatie**: **SQL Server-verificatie**. Voer uw aanmeldingsreferenties.
 - **Database**: **outmodel DB (een lijst met alle databases)**.
 - **Opmerking** De wizard slaat al dan niet standaard uw aanmeldingsgegevens. Als u deze niet wilt, selecteert u de **Aanmeldingsgegevens vergeet**.
![SQL-databasemigratie wizard - bron selecteren - stap 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. In het dialoogvenster **Gegevensbron selecteren** , selecteert u de databasenaam van de herstelde in de sectie **voorbereidende stappen nodig** als de bron en klik vervolgens op **volgende**.

    ![SQL-databasemigratie wizard - bron selecteren - stap 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)

5. Selecteer de optie **specifieke databaseobjecten selecteren** in het dialoogvenster **Kies objecten** en selecteer vervolgens de table(or tables) die u wilt migreren naar de doelserver.
![SQL-databasemigratie wizard - objecten kiezen](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)

6. Klik op de pagina **Overzicht van de Wizard Script** op **Ja** wanneer u wordt gevraagd over of u nu een SQL-script genereren. U hebt ook de optie voor het opslaan van het Script TSQL voor later gebruik.
![SQL-databasemigratie wizard - samenvatting van de Wizard Script](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)

7. Klik op **volgende**op de pagina **Overzicht van de resultaten** .
![SQL-databasemigratie wizard - resultaten overzicht](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)

8. Klik op de pagina **Instellen doel serververbinding** op **verbinding maken met de Server**en voer vervolgens de details als volgt:
    - **Servernaam**: doel server-instantie
    - **Verificatie**: **SQL Server-verificatie**. Voer uw aanmeldingsreferenties.
    - **Database**: **outmodel DB (een lijst met alle databases)**. Deze optie bevat alle databases op de doelserver.

    ![SQL-databasemigratie wizard - verbinding instellen doel-Server](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)

9. Op **verbinding maken**, selecteert u de doeldatabase waarop u wilt verplaatsen in de tabel aan en klik vervolgens op **volgende**. Dit moet stoppen met het eerder gegenereerde script uitvoeren en ziet u de zojuist verplaatste tabel die is gekopieerd naar de doeldatabase.

## <a name="verification-step"></a>Verificatiestap
1. Query en testen van de gekopieerde tabel om ervoor te zorgen dat de gegevens intact is. U kunt het formulier hernoemde tabel **voorbereidende stappen nodig** sectie weghalen na bevestiging. (bijvoorbeeld &lt;tabelnaam&gt;_old).

## <a name="next-steps"></a>Volgende stappen

[Automatische back-ups van SQL-Database](sql-database-automated-backups.md)
