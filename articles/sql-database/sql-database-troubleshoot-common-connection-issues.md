<properties
    pageTitle="Algemene problemen verbinding met Azure SQL-Database"
    description="Stappen om te identificeren en oplossen van veelvoorkomende verbindingsfouten in voor Azure SQL-Database."
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

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Problemen met verbinding oplossen met Azure SQL-Database

Wanneer de verbinding met Azure SQL-Database mislukt, ontvangt u [foutberichten](sql-database-develop-error-messages.md). In dit artikel is een gecentraliseerde onderwerp waarmee u problemen met Azure SQL-Database. Het maakt u kennis met [de gemeenschappelijke treedt](#cause) van verbindingsproblemen, wordt aanbevolen [een hulpprogramma voor probleemoplossing](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) die helpt u het probleem identiteit, en vindt u stappen voor probleemoplossing om [fouten in tijdelijke](#troubleshoot-transient-errors) en [permanente of tijdelijke](#troubleshoot-the-persistent-errors)op te lossen. Tot slot worden [alle relevante artikelen voor verbindingsproblemen Azure SQL-Database](#all-topics-for-azure-sql-database-connection-problems).

Als u de verbindingsproblemen ondervindt, kunt u de problemen oplossen met de stappen die worden beschreven in dit artikel.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Oorzaak

Verbindingsproblemen oplossen kunnen worden veroorzaakt door een van de volgende opties:

- Aanbevolen procedures en richtlijnen voor het ontwerpen toepassen tijdens het ontwerpproces van toepassing is mislukt.  Zie [Ontwikkelen-overzicht voor SQL-Database](sql-database-develop-overview.md) aan de slag.
- Azure SQL-Database opnieuw configureren
- Firewallinstellingen
- Time-out voor verbinding
- Onjuiste aanmeldingsgegevens
- Maximumlimiet bereikt op enkele bronnen Azure SQL-Database

Verbindingsproblemen met Azure SQL-Database kunnen in het algemeen wordt als volgt worden onderverdeeld:

- [Tijdelijke fouten (tijdelijk of door onregelmatige)](#troubleshoot-transient-errors)
- [Permanente of tijdelijke fouten (fouten die regelmatig terugkerend)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Probeer de probleemoplosser voor voor verbindingsproblemen Azure SQL-Database

Als u een specifieke verbindingsfout ondervindt, probeer [Dit hulpmiddel](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), waarmee u snel identiteit en uw probleem is opgelost.

## <a name="troubleshoot-transient-errors"></a>Tijdelijke oplossen
Als uw toepassing tijdelijke problemen is, raadpleegt u de volgende onderwerpen voor tips over het oplossen van problemen en de frequentie van deze fouten te beperken:

- [Problemen met Database oplossen &lt;x&gt; op Server &lt;y&gt; niet beschikbaar is (fout: 40613)](sql-database-troubleshoot-connection.md)
- [Problemen met, een diagnose stellen bij en SQL-verbindingsfouten en tijdelijke fouten te voorkomen voor SQL-Database](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Permanente oplossen (tijdelijke fouten)

Als de toepassing niet persistent verbinding maken met Azure SQL-Database, meestal geeft u aan een probleem met een van de volgende opties:

- Configuratie van de firewall. De SQL Azure-database of aan de clientzijde firewall wordt geblokkeerd door verbindingen met Azure SQL-Database.
- Nieuwe configuratie aan de clientzijde netwerk: bijvoorbeeld een nieuwe IP-adres of een proxyserver.
- Gebruikersfout: bijvoorbeeld getypte verbindingsparameters, zoals de naam van de server in de verbindingstekenreeks.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Stappen voor het oplossen van problemen met de permanente serververbinding

1.  [Firewallregels](sql-database-configure-firewall-settings.md) instellen de client IP-adres toestaan.
2.  Klik op alle firewalls tussen de client en Internet, zorg dat poort 1433 geopend voor uitgaande verbindingen is. Lees [configureren Windows Firewall SQL Server-toegang toestaan](https://msdn.microsoft.com/library/cc646023.aspx) voor extra tips.
3.  Controleer of de verbindingsreeks en andere verbindingsinstellingen. Zie het gedeelte van de verbindingsreeks in het [onderwerp van connectivity problemen](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4.  Controleer de servicestatus in het dashboard. Als u denkt er een regionale storing dat, raadpleegt u [terughalen uit een storing](sql-database-disaster-recovery.md) voor stappen herstellen naar een nieuwe gebied.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Alle onderwerpen voor Azure SQL Database-verbinding oplossen

De volgende tabel bevat elke verbinding probleem onderwerp die van toepassing rechtstreeks naar de service Azure SQL-Database is.


| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 1 | [Problemen met verbinding oplossen met Azure SQL-Database](sql-database-troubleshoot-common-connection-issues.md) | Dit is de aantekening toevoegen voor het oplossen van verbindingsproblemen in Azure SQL-Database. Dit wordt beschreven hoe identificeren en oplossen van fouten in tijdelijke en permanente of tijdelijke in Azure SQL-Database. |
| 2 | [Problemen met, een diagnose stellen bij en SQL-verbindingsfouten en tijdelijke fouten te voorkomen voor SQL-Database](sql-database-connectivity-issues.md) | Leer hoe u problemen met, een diagnose stellen bij en voorkomen dat een SQL-verbinding of een tijdelijke fout in Azure SQL-Database. |
| 3 | [Algemene richtlijnen voor tijdelijke foutenstructuuranalyse foutafhandeling](best-practices-retry-general.md) | Algemene richtlijnen voor het verwerken van de tijdelijke foutenstructuuranalyse biedt wanneer u verbinding maakt met Azure SQL-Database. |
| 4 | [Problemen oplossen connectiviteit met Microsoft Azure SQL-Database](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | Dit hulpmiddel kunt identiteit uw probleem oplossen van fouten. |
| 5 | [Problemen met ' Database &lt;x&gt; op server &lt;y&gt; is momenteel niet beschikbaar. Probeer de verbinding later"fout](sql-database-troubleshoot-connection.md) | Wordt beschreven hoe u identificeren en oplossen van een 40613-fout: "Database &lt;x&gt; op server &lt;y&gt; is momenteel niet beschikbaar. Probeer de verbinding later." |
| 6 | [SQL-foutcodes voor SQL-Database-clienttoepassingen: Database verbindingsfout en andere problemen](sql-database-develop-error-messages.md) | Informatie over foutcodes SQL bevat voor SQL-Database client-toepassingen, zoals veelvoorkomende fouten in database-verbinding, problemen met de database kopiëren en algemene fouten. |
| 7 | [Azure SQL-Database prestaties richtlijnen voor één databases](sql-database-performance-guidance.md) | Bevat richtlijnen waarmee u kunt bepalen welke servicelaag geschikt is voor uw toepassing. Ook vindt u aanbevelingen voor het optimaliseren van de toepassing optimaal gebruikmaken van uw Azure SQL-Database. |
| 8 | [SQL-Database ontwikkelen-overzicht](sql-database-develop-overview.md) | Koppelingen naar voorbeelden van code voor verschillende technologieën die u kunt verbinding maken met van en werken met Azure SQL-Database. |
| 9 | Upgrade uitvoeren naar Azure SQL-Database v12 pagina ([Azure-portal](sql-database-upgrade-server-portal.md), [PowerShell](sql-database-upgrade-server-powershell.md)) | Aanwijzingen voor het upgraden van bestaande V11: Azure SQL Database-servers en databases naar Azure SQL Database V12 met Azure portal of PowerShell biedt. |


## <a name="next-steps"></a>Volgende stappen

- [Oplossen van prestatieproblemen Azure SQL-Database](sql-database-troubleshoot-performance.md)
- [Machtigingen voor een Azure SQL-Database oplossen](sql-database-troubleshoot-permissions.md)
- [Zie alle onderwerpen voor de service Azure SQL-Database](sql-database-index-all-articles.md)
- [De documentatie over Microsoft Azure zoeken](http://azure.microsoft.com/search/documentation/)
- [De meest recente updates voor de Azure SQL Database-service weergeven](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database ontwikkelen-overzicht](sql-database-develop-overview.md)
- [Algemene richtlijnen voor tijdelijke foutenstructuuranalyse foutafhandeling](../best-practices-retry-general.md)
- [Bibliotheken van de verbinding voor SQL-Database en SQL Server](sql-database-libraries.md)
- [De leerpad voor het gebruik van Azure SQL-Database](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [Leerpad voor het gebruik van elastische databasefuncties en hulpprogramma's voor](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 
