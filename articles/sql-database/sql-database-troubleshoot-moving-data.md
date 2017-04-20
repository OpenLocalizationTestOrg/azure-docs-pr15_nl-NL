<properties
    pageTitle="Databases navigeren tussen abonnementen en en afmelden bij Azure-servers."
    description="Snelle stappen om te kopiëren, verplaatsen en migreren van gegevens en databases in Azure SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Databases verplaatsen tussen servers, tussen abonnementen en en afmelden bij Azure

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Een database te verplaatsen naar een andere server in hetzelfde abonnement
- Klik in de [Portal van Azure](https://portal.azure.com) **SQL-databases**op, selecteer een database in de lijst en klik vervolgens op **kopiëren**. Zie [een Azure SQL-database kopiëren](sql-database-copy.md) voor meer informatie.

## <a name="to-move-a-database-between-subscriptions"></a>Een database verplaatsen tussen abonnementen
- Klik in de [Portal van Azure](https://portal.azure.com) **SQL-servers** op en selecteer vervolgens de server waarop uw database in de lijst. Klik op **verplaatsen**en kiest u de bronnen om te gaan en het abonnement om naar te gaan.

## <a name="to-migrate-a-sql-database-into-azure"></a>Een SQL-database migreren naar Azure
- Databasecompatibiliteit bepalen en kies vervolgens de juiste migratiemethode op basis van uw behoeften voldoet. Ga als volgt de richtlijnen en opties in het [migreren van een SQL Server-database](sql-database-cloud-migrate.md).

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>Een kopie van een database voor gebruik buiten Azure maken
- [Een Bacpac-bestand exporteren.](sql-database-export.md)
