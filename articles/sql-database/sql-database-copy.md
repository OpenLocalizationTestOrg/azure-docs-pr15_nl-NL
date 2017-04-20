<properties
    pageTitle="Kopieer een Azure SQL-database | Microsoft Azure"
    description="Een kopie van een Azure SQL-database maken"
    services="sql-database"
    documentationCenter=""
    authors="anosov1960"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/24/2016"
    ms.author="sstein; sashan"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database"></a>Kopieer een Azure SQL-Database

> [AZURE.SELECTOR]
- [Overzicht](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

U kunt de Azure [SQL-Database automatische back-ups](sql-database-automated-backups.md) gebruiken om een kopie van uw SQL-database te maken. De databasekopie gebruikt dezelfde technologie als de functie geografische-herhaling. Maar in tegenstelling tot geografische herhaling beëindigt de koppeling herhaling als zodra de seeding fase is voltooid. De database kopiëren is dus een momentopname van de brondatabase op het tijdstip van de aanvraag kopiëren.  
U kunt de databasekopie op dezelfde server of een andere server maken. De service laag en prestaties niveau (prijzen laag) van de databasekopie zijn hetzelfde als de brondatabase al dan niet standaard. Wanneer u met de API, kunt u een ander prestatieniveau binnen de servicelaag met dezelfde (versie). Nadat de kopie voltooid is, wordt de kopie een volledig functioneel, onafhankelijke database. U kunt nu upgraden of deze downgraden naar een edition. De aanmeldingen, gebruikers en machtigingen kunnen onafhankelijk worden beheerd.  

Wanneer u een database naar dezelfde logische server kopiëren, kunnen de dezelfde aanmeldingen worden gebruikt voor beide databases. De beveiligings-principal dat u gebruiken om de database wordt de database-eigenaar op de nieuwe database. Alle databasegebruikers, hun machtigingen en hun beveiliging-id's worden gekopieerd naar de databasekopie.  

Wanneer u een database naar een andere logische server kopiëren, wordt de beveiligings-principal op de nieuwe server de eigenaar van de database op de nieuwe database. Als u [opgenomen databasegebruikers](sql-database-manage-logins.md) voor toegang tot gegevens zorgt ervoor dat zowel primaire als secundaire databases hebt altijd dezelfde gebruikersreferenties zodat nadat kopiëren is voltooid u direct toegang hebt tot deze met dezelfde referenties. Als u [Azure Active Directory](../active-directory/active-directory-whatis.md)gebruikt, kunt u dat u voor het beheren van referenties in de kopie volledig voorkomen. Echter wanneer u de database naar een nieuwe server kopieert, werkt de aanmelding op basis van access in het algemeen niet omdat de aanmeldingen niet op de nieuwe server bestaat. Lees [hoe u de beveiliging van de Azure SQL-database na herstel beheren](sql-database-geo-replication-security-config.md) voor meer informatie over het beheren van aanmeldingen bij het kopiëren van een database naar een andere logische server. 

Als u wilt kopiëren van een SQL-database, hebt u het volgende nodig:

- Een Azure-abonnement. Klik op **Gratis PROEFVERSIE** boven aan deze pagina en keert u terug naar het voltooien van dit artikel als u een abonnement op Azure gewoon nodig hebt.
- Een SQL-database om te kopiëren. Als u een SQL-database niet hebt, maakt u één de stappen in dit artikel: [uw eerste Azure SQL-Database maken](sql-database-get-started.md).

## <a name="next-steps"></a>Volgende stappen

- Zie [een Azure SQL-database met behulp van de Azure portal kopiëren](sql-database-copy-portal.md) naar het kopiëren van een database met behulp van de Azure-portal.
- Zie [een Azure SQL-database kopiëren via PowerShell](sql-database-copy-powershell.md) om te kopiëren van een database via PowerShell.
- Zie [kopiëren een Azure SQL-database T-SQL gebruiken](sql-database-copy-transact-sql.md) om te kopiëren van een database met behulp van Transact-SQL.
- Lees [hoe u de beveiliging van de Azure SQL-database na herstel beheren](sql-database-geo-replication-security-config.md) voor meer informatie over het beheren van gebruikers en aanmeldingen bij het kopiëren van een database naar een andere logische server.



## <a name="additional-resources"></a>Aanvullende informatie

- [Aanmeldingen beheren](sql-database-manage-logins.md)
- [Verbinding maken met SQL-Database met SQL Server Management Studio en uitvoeren van een steekproef T-SQL-query](sql-database-connect-query-ssms.md)
- [De database exporteren naar een BACPAC](sql-database-export.md)
- [Bedrijfscontinuïteit voor bedrijfsoverzichten](sql-database-business-continuity.md)
- [SQL-Database documentatie](https://azure.microsoft.com/documentation/services/sql-database/)
