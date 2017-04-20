<properties
    pageTitle="Hoe moet u doen beheertaken bijvoorbeeld beheerderswachtwoord opnieuw instellen | Microsoft Azure"
    description="Beschrijving van het uitvoeren van veelvoorkomende beheertaken in SQL-Database. Bijvoorbeeld beheerderswachtwoord opnieuw in te stellen, toewijzen en verwijderen van access."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="beheerderswachtwoord opnieuw instellen"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>Het uitvoeren van veelvoorkomende beheertaken zoals opnieuw instellen van beheerderswachtwoord in Azure SQL-Database
Gebruik dit onderwerp voor snelle stappen om te verlenen en toegang tot een Azure SQL-database verwijderen. Zie voor meer uitgebreide informatie:

- [Databases en aanmeldingen in Azure SQL-Database beheren](sql-database-manage-logins.md)
- [Beveiliging van uw SQL-database](sql-database-security.md)
- [Beveiligingscentrum voor SQL Server-Database Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>Beheerderswachtwoord voor een logische server opnieuw instellen

- Klik in de [Portal van Azure](https://portal.azure.com) **SQL-Servers**op, selecteer de server in de lijst en klik vervolgens op **Wachtwoord opnieuw instellen**.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>Als u wilt zorgen weet u zeker alleen geautoriseerde IP zijn-adressen toegestaan voor toegang tot de server
- Zie [hoe: firewallinstellingen configureren op SQL-Database](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>Container om databasegebruikers te maken in de database
- Gebruik de instructie [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) en [Gebruikers](https://msdn.microsoft.com/library/ff929188.aspx)ziet die deel uit van Database - waardoor uw Database geschikter.

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>Container om databasegebruikers te verifiëren met behulp van de Azure Active Directory
- Zie [verbinding maakt met SQL-Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>Extra aanmeldingen voor veel rechten gebruikers in de virtuele hoofddatabase maken
- Gebruik de instructie [LOGIN maken](https://msdn.microsoft.com/library/ms189751.aspx) en Zie de sectie beheren aanmeldingen van [databases beheren en aanmeldingen in Azure SQL-Database](sql-database-manage-logins.md) voor meer informatie.
