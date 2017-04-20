<properties
   pageTitle="Azure SQL Database-algemeen beperkingen en richtlijnen"
   description="Enkele algemene beperkingen voor Azure SQL-Database, evenals de gebieden van interoperabiliteit en ondersteuning voor deze pagina wordt beschreven."
   services="sql-database"
   documentationCenter="na"
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/06/2016"
   ms.author="carlrab" />

# <a name="azure-sql-database-general-limitations-and-guidelines"></a>Azure SQL Database-algemeen beperkingen en richtlijnen

In dit onderwerp vindt u algemene beperkingen en richtlijnen voor Azure SQL-Database. Voor een compleet begrip van de quota en resourcebeheer ondersteuning, raadpleegt u de [Extra bronnen](#additional-guidelines) aan het einde van dit onderwerp.

## <a name="connectivity-and-authentication"></a>Connectiviteit en -verificatie

  - Windows-verificatie wordt niet ondersteund. Zie [Databases en aanmeldingen in Azure SQL-Database beheren](sql-database-manage-logins.md). Azure Active Directory-verificatie is echter wel ondersteund met bepaalde beperkingen. Zie [verbinding maken met SQL-Database met Azure Active Directory-verificatie](sql-database-aad-authentication.md).

  - Microsoft Azure SQL Database ondersteunt tabelgegevens stream (TDS) protocol clientversie 7.3 of hoger.

  - Alleen TCP/IP-verbindingen zijn toegestaan.

  - De SQL Server 2008 SQL Server-browser wordt niet ondersteund omdat Microsoft Azure SQL Database geen dynamische poorten, alleen poort 1433.

## <a name="sql-server-agentjobs"></a>SQL Server-Agent/taken

Microsoft Azure SQL Database biedt geen ondersteuning voor SQL Server-Agent, maar u kunt elastische taken taken uitvoeren via een-op-veel-databases. Zie voor meer informatie over elastische taken, [elastische taken](sql-database-elastic-jobs-overview.md).

## <a name="sql-server-collation-support"></a>Ondersteuning voor SQL Server-sortering

De standaard-databasesortering die worden gebruikt door Microsoft Azure SQL-Database is **SQL_LATIN1_GENERAL_CP1_CI_AS**, waar **LATIN1_GENERAL** is Engels (Verenigde Staten), **CP1** is codetabel 1252 **CI** is niet hoofdlettergevoelig en **accent gevoelige valt** . Het kan niet naar de sortering van V12 databases. Zie voor meer informatie over het instellen van de sortering [sorteren (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="naming-requirements"></a>Naamgevingsvereisten

Bepaalde gebruikersnamen zijn niet toegestaan vanwege de beveiliging. U kunt de volgende namen niet gebruiken:

 - **beheerder**
 - **beheerder**
 - **Gast**
 - **hoofdmap**
 - **SA**

Namen voor alle nieuwe objecten moeten voldoen aan de SQL Server-regels voor id's. Zie [-id's](https://msdn.microsoft.com/library/ms175874.aspx)voor meer informatie.

Daarnaast kunnen aanmelden en gebruikersnamen mogen geen bevatten de \ teken (Windows-verificatie wordt niet ondersteund).

## <a name="additional-guidelines"></a>Aanvullende richtlijnen

- Naast de algemene beperkingen in dit artikel beschreven, heeft SQL-Database specifieke resource quota en beperkingen op basis van uw **servicelaag**. Zie voor een overzicht van service niveaus, [SQL-Database Servicelagen](sql-database-service-tiers.md).

- Zie [Azure SQL Database Resource limieten](sql-database-resource-limits.md)voor andere limieten SQL-Database.

- Gerelateerde richtlijnen, Zie [Azure SQL Database-richtlijnen voor beveiliging en beperkingen](sql-database-security-guidelines.md)voor beveiliging.

- Een ander gerelateerde gebied rondom de compatibiliteit met Azure SQL-Database met on-premises implementatie-versies van SQL Server, zoals SQL Server-2014 en SQL Server-2016. De meest recente V12-versie van Azure SQL-Database heeft vele verbeteringen aangebracht in dit gedeelte. Zie [Wat is er nieuw in V12 voor SQL-Database](sql-database-v12-whats-new.md)voor meer informatie.

- Zie voor informatie over de beschikbaarheid van de stuurprogramma en ondersteuning voor SQL-Database, [Verbinding bibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md).
