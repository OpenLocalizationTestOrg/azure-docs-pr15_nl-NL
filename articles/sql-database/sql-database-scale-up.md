<properties
    pageTitle="De service laag en prestaties het niveau van een Azure SQL-database wijzigen | Microsoft Azure"
    description="Wijzig de servicelaag en prestatieniveau van een Azure SQL-database ziet u hoe u de schaal van de SQL-database omhoog of omlaag. Het wijzigen van de prijzen laag van een Azure SQL-database."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-using-the-azure-portal"></a>De service laag en prestaties niveau wijzigen (prijzen laag) van een SQL-database met behulp van de Azure portal


> [AZURE.SELECTOR]
- [**Azure-portal**](sql-database-scale-up.md)
- [PowerShell](sql-database-scale-up-powershell.md)


Service niveaus en prestaties beschrijven van de functies en bronnen die beschikbaar zijn voor uw SQL-database en kunnen worden bijgewerkt als de behoeften van uw wijziging toepassing. Zie [Servicelagen](sql-database-service-tiers.md)voor meer informatie.

Houd er rekening mee dat de servicelaag wijzigt en/of prestatieniveau van een database Hiermee maakt u een replica van de oorspronkelijke database op het prestatieniveau van de nieuwe en vervolgens wordt overgeschakeld verbindingen naar de replica. Er zijn geen gegevens gaat verloren tijdens dit proces, maar tijdens het kort moment wanneer we naar de replica overzet, verbindingen met de database zijn uitgeschakeld, zodat sommige transacties in flight kunnen worden hersteld. In dit venster varieert, maar is gemiddeld onder 4 seconden en in meer dan 99% van zaken is minder dan 30 seconden. Zeer af en toe zijn met name als er grote aantallen transacties in flight op het moment dat verbindingen zijn uitgeschakeld, dit venster mogelijk langer.  

De duur van het hele schaal-up-proces is afhankelijk van de grootte en service laag van de database voor en na de wijziging. Een database 250 GB dat is wijzigen aan, uit of binnen een laag Standard service moet bijvoorbeeld voltooid binnen 6 uur. Voor een database van dezelfde grootte prestatieniveaus binnen de Premium-servicelaag wordt gewijzigd, moet deze voltooid binnen 3 uur.


Gebruik de informatie in [Azure SQL Database Service niveaus en prestaties](sql-database-service-tiers.md) het juiste service laag en prestaties niveau vaststellen voor uw Azure SQL-Database.

- Als u wilt een database downgraden, moet de database kleiner zijn dan de toegestane maximumgrootte van de laag doel-service. 
- Bij het bijwerken van een database met [Geografische-replicatie](sql-database-geo-replication-overview.md) is ingeschakeld, moet u eerst de secundaire databases upgraden naar de gewenste prestaties laag voordat u een upgrade van de hoofddatabase.
- Wanneer u een servicelaag Downgrade, moet u eerst alle geografische herhaling relaties beëindigen. 
- De diensten herstellen zijn verschillend voor de verschillende niveaus van de service. Als u zijn Downgrade u mogelijk de mogelijkheid om te zetten naar een punt in tijd kwijtraakt of hebben een lagere back-up bewaarperiode. Zie [Azure SQL Database-back-up en herstellen](sql-database-business-continuity.md)voor meer informatie.
- Wijzigen van uw database prijzen van laag, wordt de maximale databasegrootte niet gewijzigd. Als u wilt wijzigen van uw database gebruik maximaal [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) of [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
- De nieuwe eigenschappen voor de database worden niet toegepast totdat de wijzigingen voltooid zijn.



**Als u wilt voltooien van dit artikel nodig u het volgende:**

- Een Azure SQL-database. Als u een SQL-database niet hebt, maakt u één de stappen in dit artikel: [uw eerste Azure SQL-Database maken](sql-database-get-started.md).


## <a name="change-the-service-tier-and-performance-level-of-your-database"></a>Het serviceniveau laag en prestaties van uw database wijzigen


Het blad SQL-Database voor de database die u wilt schalen omhoog of omlaag te openen:

1.  Klik op **meer services**in de [portal van Azure](https://portal.azure.com) > **SQL-databases**.
2.  Klik op de database die u wilt wijzigen.
3.  Klik op het blad **SQL-database** op **prijzen laag (schaal DTUs)**:

    ![prijzen van laag][1]

1.  Kies een nieuwe laag en klik op **selecteren**:

    **Selecteer** klikken, dient een aanvraag schaal wijzigen van de prijzen laag. De schaal bewerking kan enige tijd duren naar voltooid (Zie de informatie aan de bovenkant van dit artikel) afhankelijk van de grootte van uw database.

    > [AZURE.NOTE] Wijzigen van uw database prijzen van laag, wordt de maximale databasegrootte niet gewijzigd. Als u wilt wijzigen van uw database gebruik maximaal [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) of [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).

    ![Selecteer prijzen laag][2]

3.  Klik op het meldingspictogram (bel), in de rechterbovenhoek:

    ![meldingen][3]

    Klik op de tekst van het bericht als u wilt openen, het detailvenster waar u de status van de aanvraag kunt zien.




## <a name="next-steps"></a>Volgende stappen

- Wijzig uw maximale grootte van de database met [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) of [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
- [Uit- en schaal](sql-database-elastic-scale-get-started.md)
- [Verbinding maken en een SQL-database met SSMS query](sql-database-connect-query-ssms.md)
- [Een Azure SQL-database exporteren](sql-database-export.md)

## <a name="additional-resources"></a>Aanvullende informatie

- [Bedrijfscontinuïteit voor bedrijfsoverzichten](sql-database-business-continuity.md)
- [SQL-Database documentatie](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-scale-up/new-tier.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png
