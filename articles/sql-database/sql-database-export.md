<properties
    pageTitle="Een Azure SQL-database naar een Bacpac-bestand met behulp van de Portal Azure archiveren"
    description="Een Azure SQL-database naar een Bacpac-bestand met behulp van de Portal Azure archiveren"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Een Azure SQL-database naar een Bacpac-bestand met behulp van de Portal Azure archiveren

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)

In dit artikel vindt u aanwijzingen voor het archiveren van uw Azure SQL-database naar een Bacpac-bestand (opgeslagen in Azure-blobopslag) met behulp van de [Azure-portal](https://portal.azure.com).

Wanneer u een archief van een Azure SQL-database maken moet, kunt u de databaseschema en gegevens exporteren naar een Bacpac-bestand. Een bestand Bacpac-is gewoon een ZIP-bestand met de extensie BACPAC. Een bestand Bacpac-later kan worden opgeslagen in Azure-blobopslag of in de lokale opslag in een on-premises locatie en later geïmporteerde terug naar Azure SQL-Database of naar een SQL Server on-premises installatie. 

***Overwegingen***

- Een archief transactioneel consistent, moet u ervoor zorgen beide die geen schrijven activiteit optreedt tijdens het exporteren of die u wilt exporteren uit een [Transactioneel consistente kopie](sql-database-copy.md) van uw Azure SQL-database.
- De maximale grootte van een Bacpac-bestand met Azure-blobopslag gearchiveerd is 200 GB. Als u wilt archiveren een groter Bacpac-bestand naar de lokale opslag, moet u het hulpprogramma van de opdrachtprompt [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) gebruiken. Dit hulpprogramma wordt geleverd met zowel Visual Studio en SQL Server. U kunt ook [download](https://msdn.microsoft.com/library/mt204009.aspx) de nieuwste versie van SQL Server Data Tools om dit hulpprogramma.
- Archiveren met Azure premium storage met behulp van een Bacpac-bestand wordt niet ondersteund.
- Als de exportbewerking groter is dan 20 uur, kan deze worden geannuleerd. Als u wilt vergroten prestaties tijdens het exporteren, kunt u het volgende doen:
 - Tijdelijk uw serviceniveau verhogen.
 - Niet meer alle lezen en schrijven activiteit tijdens het exporteren.
 - Gebruik een [geclusterde index](https://msdn.microsoft.com/library/ms190457.aspx) met niet-null-waarden in alle grote tabellen. Zonder gegroepeerde indexen, kan een exporteren als het duurt langer dan 6-12 uur mislukken. Dit komt omdat de service exporteren scannen van de tabel om te proberen moet te exporteren van de hele tabel uitvoeren. Een goede manier om te bepalen als uw tabellen zijn geoptimaliseerd voor exporteren is **DBCC SHOW_STATISTICS** uitvoeren en zorg ervoor dat de *RANGE_HI_KEY* niet null is en de waarde goede verdeling. Zie [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx)voor meer informatie.


> [AZURE.NOTE] BACPACs niet moeten worden gebruikt voor back-up en herstellen van bewerkingen. Azure SQL-Database automatisch back-ups voor elke gebruikersdatabase gemaakt. Zie [Business bedrijfscontinuïteit overzicht](sql-database-business-continuity.md)voor meer informatie.

Als u wilt voltooien van dit artikel nodig u het volgende:

- Een Azure-abonnement.
- Een Azure SQL-Database. 
- Een [standaardopslag van Azure-account](../storage/storage-create-storage-account.md) met een container blob voor de opslag van de BACPAC in standard opslag.

## <a name="export-your-database"></a>De database exporteren

Open het blad SQL-Database voor de database die u wilt exporteren.

> [AZURE.IMPORTANT] Om te garanderen een transactioneel consistente Bacpac-bestand moet eerst [een kopie van uw database maken](sql-database-copy.md) en exporteer het bestand de databasekopie. 

1.  Ga naar de [Azure-portal](https://portal.azure.com).
2.  Klik op **SQL-databases**.
3.  Klik op de database archiveren.
4.  Klik in het blad SQL-Database op **exporteren** als u wilt openen van het blad **database exporteren** :

    ![knop exporteren][1]

5.  Klik op **opslag** en selecteer uw account en blob container van opslagruimte waar de BACPAC worden opgeslagen:

    ![database exporteren][2]

6. Selecteer het verificatietype. 
7.  Voer de referenties van de juiste verificatiemethode voor de Azure SQL-server met de database die u wilt exporteren.
8.  Klik op **OK** als u wilt archiveren van de database. Klik op **OK** Hiermee maakt u een verzoek van de database exporteren en verstuurt dit naar de service. Hoe lang de export duurt, is afhankelijk van de grootte en complexiteit van uw database en het serviceniveau van uw. U ontvangt een melding.

    ![melding exporteren][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>De voortgang van de exportbewerking

1.  Klik op **SQL-servers**.
2.  Klik op de server met de oorspronkelijke (bron)-database die u zojuist gearchiveerd.
3.  Schuif omlaag naar bewerkingen.
4.  Klik op **importeren/exporteren geschiedenis**in het SQL server-blad:

    ![Geschiedenis van importeren exporteren][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>Controleer of dat de BACPAC alleen in de container opslag

1.  Klik op **opslag-accounts**.
2.  Klik op de opslag-account waar u het archief Bacpac-hebt opgeslagen.
3.  Klik op **Containers** en selecteer de container die u hebt geïmporteerd de database naar voor meer informatie (u kunt downloaden en sla de BACPAC vanaf hier).

    ![details van .bacpac-bestand][5]  

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het importeren van een BACPAC met een Azure SQL-Database, [een BACPCAC met een Azure SQL-database importeren](sql-database-import.md)
- Zie voor meer informatie over het importeren van een BACPAC met een SQL Server-database, [een BACPCAC met een SQL Server-database importeren](https://msdn.microsoft.com/library/hh710052.aspx)



<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

