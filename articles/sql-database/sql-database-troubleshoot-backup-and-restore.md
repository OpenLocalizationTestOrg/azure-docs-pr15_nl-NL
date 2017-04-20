<properties
    pageTitle="Problemen met back-up en herstellen met Azure SQL-Database"
    description="Informatie over het herstellen van een database cloud van fouten en bijvoorbeeld back-ups en replica's gebruiken in Azure SQL-Database."
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

# <a name="restore-a-database-to-a-previous-point-in-time-restore-a-deleted-database-or-recover-from-a-data-center-outage"></a>Een database terugzetten naar een vorig punt in tijd, een verwijderde database terugzetten of herstellen van een storing gegevens-beheercentrum

SQL-Database gehouden kopieën van uw database, zodat u uit bijvoorbeeld en gebruikersfout herstellen kunt. Beschikbare opties, is afhankelijk van de database servicelaag en de opties die u kiest. Zie de [Business bedrijfscontinuïteit overzicht](sql-database-business-continuity.md) voor informatie en ontwerpoverwegingen.

## <a name="to-restore-a-database-to-a-previous-point-in-time"></a>Een database terugzetten naar een vorig punt in tijd
1.  Klik in de [Portal van Azure](https://azure.microsoft.com/)op **SQL-databases**.
2.  Selecteer de database in de lijst en klik vervolgens op **herstellen**.
3.  Typ een nieuwe naam voor de database, kiest u de datum en tijd om te herstellen uit en klik vervolgens op **maken.**
4.  Breng wijzigingen aan de app zo nodig is om te verwijzen naar de nieuwe database. Zie [herstellen van een database naar een tijdstip](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="to-restore-an-accidentally-deleted-database"></a>Een per ongeluk verwijderde database terugzetten
1.  Klik in de [Portal van Azure](https://azure.microsoft.com/)op **SQL-servers**.
2.  Selecteer de server waarop de database in de lijst die worden gehost.
3.  Klik op het blad Server, schuif omlaag en klik op **Verwijderde databases**.
4.  Selecteer de database herstellen en klik vervolgens op **maken**.
5.  Breng wijzigingen aan de app zo nodig is om te verwijzen naar de nieuwe database. Zie [een verwijderde database herstellen](sql-database-recovery-using-backups.md#deleted-database-restore).

## <a name="to-recover-from-a-regional-datacenter-outage"></a>Herstellen uit een storing regionale datacenter
Met Standard en Premium-databases, als u ingesteld geografische gerepliceerd ontvangen, kunt u herstellen met deze secundaire servers. Hiermee kunt u een database terugzetten met een minder schadelijk verlies van gegevens. Zie [een Azure SQL-database met geautomatiseerde back-ups herstellen](sql-database-disaster-recovery.md) voor meer informatie.

Azure bevat ook een back-ups van elke database in een ander gebied (een geografische-redundante back-up). U kunt een nieuwe database maken vanuit deze back-ups, dat wordt genoemd geografische herstellen, maar het is waarschijnlijk dat u hebt nog steeds ondervindt gegevensverlies als u met deze methode alleen werkt.

**Het herstellen van een database met behulp van geografische herstellen:**

- In de [Portal van Azure](https://azure.microsoft.com/)op **Nieuw**, klikt u op **gegevens en opslag**, klikt u op **SQL-Database**en selecteer **back-up** als de databasebron. Zie [een Azure SQL-database van een storing herstellen](sql-database-disaster-recovery.md) voor meer informatie.