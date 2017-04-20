<properties
    pageTitle="Kopieer een Azure SQL-database met behulp van de Azure portal | Microsoft Azure"
    description="Een kopie van een Azure SQL-database maken"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Kopieer een Azure SQL-Database met behulp van de Azure portal

> [AZURE.SELECTOR]
- [Overzicht](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

De volgende stappen hoe u een SQL-database met de [portal van Azure](https://portal.azure.com) kopiëren naar dezelfde server of een andere server.

Als u wilt kopiëren van een SQL-database, moet u de volgende items:

- Een Azure-abonnement. Klik op **Gratis PROEFVERSIE** boven aan deze pagina en keert u terug naar het voltooien van dit artikel als u een abonnement op Azure gewoon nodig hebt.
- Een SQL-database om te kopiëren. Als u een SQL-database niet hebt, maakt u één de stappen in dit artikel: [uw eerste Azure SQL-Database maken](sql-database-get-started.md).


## <a name="copy-your-sql-database"></a>Kopieer de SQL-database

Open de pagina SQL-database voor de database die u wilt kopiëren:

1.  Ga naar de [Azure-portal](https://portal.azure.com).
2.  Klik op **meer services** > **SQL-databases**, en klik vervolgens op de gewenste database.
3.  Klik op **kopiëren**op de pagina SQL-database:

    ![SQL-Database](./media/sql-database-copy-portal/sql-database-copy.png)

1.  Klik op de pagina **kopie** krijgt u de naam van een standaard-database. Typ een andere naam desgewenst (alle databases op een server moeten een unieke naam hebben).
2.  Selecteer een **doelserver**. De doelserver is waar de databasekopie is gemaakt. U kunt de database kopiëren naar dezelfde server of een andere server. U kunt maken van een server of Selecteer een bestaande server in de lijst. 
3.  Nadat u de **doelserver**, **elastische database van toepassingen**en **prijzen laag** hebt geselecteerd worden opties ingeschakeld. Als de server een groep heeft, kunt u de database kopiëren erin.
3.  Klik op **OK** om de kopieerproces te starten.

    ![SQL-Database](./media/sql-database-copy-portal/copy-page.png)


## <a name="monitor-the-progress-of-the-copy-operation"></a>De voortgang van de kopieeropdracht

- Nadat de kopie is gestart, klikt u op de melding van de portal voor meer informatie.

    ![melding][3]
 
    ![monitor][4]


## <a name="verify-the-database-is-live-on-the-server"></a>Controleer of dat de database is op de server

- Klik op **meer services** > **SQL-databases** en controleer of de nieuwe database **Online**is.


## <a name="resolve-logins"></a>Aanmeldingen oplossen

Om op te lossen aanmeldingen nadat de kopieerbewerking is voltooid, raadpleegt u [aanmeldingen oplossen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="next-steps"></a>Volgende stappen

- Zie [kopiëren van een Azure SQL-database](sql-database-copy.md) voor een overzicht van het kopiëren van een Azure SQL-Database.
- Zie [een Azure SQL-database kopiëren via PowerShell](sql-database-copy-powershell.md) om te kopiëren van een database via PowerShell.
- Zie [kopiëren een Azure SQL-database T-SQL gebruiken](sql-database-copy-transact-sql.md) om te kopiëren van een database met behulp van Transact-SQL.
- Lees [hoe u de beveiliging van de Azure SQL-database na herstel beheren](sql-database-geo-replication-security-config.md) voor meer informatie over het beheren van gebruikers en aanmeldingen bij het kopiëren van een database naar een andere logische server.



## <a name="additional-resources"></a>Aanvullende informatie

- [Aanmeldingen beheren](sql-database-manage-logins.md)
- [Verbinding maken met SQL-Database met SQL Server Management Studio en uitvoeren van een steekproef T-SQL-query](sql-database-connect-query-ssms.md)
- [De database exporteren naar een BACPAC](sql-database-export.md)
- [Bedrijfscontinuïteit voor bedrijfsoverzichten](sql-database-business-continuity.md)
- [SQL-Database documentatie](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

