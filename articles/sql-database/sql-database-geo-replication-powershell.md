<properties 
    pageTitle="Actieve geografische-replicatie configureren voor Azure SQL-Database via PowerShell | Microsoft Azure" 
    description="Actieve geografische-replicatie configureren voor Azure SQL-Database via PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
   ms.workload="NA"
    ms.date="07/14/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-powershell"></a>Geografische-replicatie configureren voor Azure SQL-Database met PowerShell

> [AZURE.SELECTOR]
- [Overzicht](sql-database-geo-replication-overview.md)
- [Azure-Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

In dit artikel leest u hoe actieve geografische-replicatie configureren voor SQL-Database met PowerShell.

Als u wilt starten failover via PowerShell, raadpleegt u [een failover- of niet gepland voor Azure SQL-Database met PowerShell](sql-database-geo-replication-failover-powershell.md).

>[AZURE.NOTE] Actieve geografische-herhaling (leesbare ontvangen) is nu beschikbaar voor alle databases in alle Servicelagen. In April 2017 de niet-leesbare secundaire type wordt ingetrokken en bestaande niet-leesbare databases automatisch worden bijgewerkt naar leesbare secundaire servers.



Actieve geografische-replicatie configureren via PowerShell, moet u de volgende handelingen uit:

- Een Azure-abonnement. 
- Een Azure SQL database - de primaire database die u wilt repliceren.
- Azure PowerShell 1.0 of hoger. U kunt downloaden en installeren van de Azure PowerShell-modules door de volgende [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Uw referenties configureren en selecteert u uw abonnement

Eerst moet u toegang instellen bij uw Azure-account zo PowerShell starten en vervolgens de volgende cmdlet uitvoeren. Voer de e-mailbericht en het wachtwoord waarmee u zich aanmelden bij de portal van Azure in het aanmeldingsvenster.


    Login-AzureRmAccount

Na het succes aanmelden ziet u enkele informatie op scherm dat bevat de Id die u aangemeld met en de Azure abonnementen die u toegang hebt.


### <a name="select-your-azure-subscription"></a>Selecteer uw Azure abonnement

Als u wilt selecteren van het abonnement dat u uw abonnement id nodig hebt U kunt de abonnements-Id van de gegevens uit de vorige stap kopiëren of als u meerdere abonnementen hebt en meer informatie nodig kunt u de cmdlet **Get-AzureRmSubscription** uitvoeren en kopieer de gewenste abonnementsgegevens uit de resultaatset. De volgende cmdlet wordt de abonnements-Id gebruikt voor het instellen van het huidige abonnement:

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Na het succes uitvoeren **Selecteer-AzureRmSubscription** keert u terug naar de PowerShell-prompt.


## <a name="add-secondary-database"></a>Secundaire database toevoegen


Een nieuwe secundaire database maken de volgende stappen in een samenwerkingsverband geografische-herhaling.  
  
Als u wilt inschakelen, een secundair moet u de eigenaar van het abonnement of een mede-eigenaar zijn. 

U kunt de cmdlet **New-AzureRmSqlDatabaseSecondary** gebruiken om toe te voegen een secundaire database op een partnerserver met een lokale database op de server waarop u bent verbonden (de hoofddatabase). 

Deze cmdlet vervangen **Start-AzureSqlDatabaseCopy** door de parameter **– IsContinuous** .  Dit wordt uitvoer van een object **AzureRmSqlDatabaseSecondary** die kan worden gebruikt door andere cmdlets duidelijk aangeven dat een specifieke replicatiekoppeling. Deze cmdlet retourneert wanneer de secundaire database is gemaakt en volledig agarvoedingsbodem. Afhankelijk van de grootte van de database kan deze minuten tot uur duren.

De gerepliceerde database op de secundaire server heeft dezelfde naam als de database op de primaire server en, al dan niet standaard, heeft hetzelfde. De secundaire database kunt werken met leesbare of niet-leesbare en kunt werken met één database of een elastische-database. Zie [Nieuw AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) en [Servicelagen](sql-database-service-tiers.md)voor meer informatie.
Nadat de secundaire is gemaakt en agarvoedingsbodem, begint gegevens uit de hoofddatabase repliceren naar de nieuwe secundaire database. De onderstaande stappen wordt uitgelegd hoe u het uitvoeren van deze taak PowerShell gebruiken voor het maken van niet-leesbare en leesbaar ontvangen, met één database of een elastische-database.

Als de database partner al bestaat (bijvoorbeeld - grond van een vorige geografische herhaling relatie beëindigd), de opdracht mislukt.



### <a name="add-a-non-readable-secondary-single-database"></a>Een niet-leesbare secundair (één database) toevoegen

De volgende opdracht maakt een niet-leesbare secundair van database 'mijndb' van server "srv2" in resource groep "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "No"



### <a name="add-readable-secondary-single-database"></a>Leesbare secundair (één database) toevoegen

De volgende opdracht maakt een leesbare secundair van database 'mijndb' van server "srv2" in resource groep "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### <a name="add-a-non-readable-secondary-elastic-database"></a>Een niet-leesbare secundair (elastische database) toevoegen

De volgende opdracht maakt een niet-leesbare secundair van database 'mijndb' in de elastische database-toepassingen met de naam 'ElasticPool1' van server "srv2" in resource groep "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "No"


### <a name="add-a-readable-secondary-elastic-database"></a>Een leesbare secundair (elastische database) toevoegen

De volgende opdracht maakt een leesbare secundair van database 'mijndb' in de elastische database-toepassingen met de naam 'ElasticPool1' van server "srv2" in resource groep "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## <a name="remove-secondary-database"></a>Secundaire database verwijderen

Gebruik de cmdlet **Verwijderen AzureRmSqlDatabaseSecondary** de replicatiesamenwerking tussen een secundaire database en de primaire permanent te beëindigen. Na de beëindiging van de relatie wordt de secundaire database een alleen-lezen-database. Als de connectiviteit met secundaire database niet meer werkt de opdracht is geslaagd, maar de secundaire worden alleen-lezen-schrijven nadat verbinding is hersteld. Zie [Verwijderen-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) en [Servicelagen](sql-database-service-tiers.md)voor meer informatie.

Deze cmdlet vervangt stoppen-AzureSqlDatabaseCopy voor replicatie. 

Er is een equivalent van een afgedwongen beë die wordt verwijderd van de koppeling herhaling en de voormalige secundaire blijft als een zelfstandig product-database die is niet volledig gerepliceerd vóór deze te verwijderen. Zorgen dat alle koppelingsgegevens worden opgeschoond op zowel de voormalige primaire en secundaire voormalige als of wanneer ze beschikbaar zijn. Deze cmdlet retourneert wanneer de replicatiekoppeling is verwijderd. 


Om te kunnen verwijderen secundaire, moeten gebruikers schrijftoegang voor primaire en secundaire databases op basis van de RBAC hebben. Zie Rolgebaseerd toegangsbeheer voor meer informatie.

De volgende Hiermee verwijdert u replicatiekoppeling van de database met de naam 'mijndb' naar de server "srv2" van de resource groep "rg2". 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 


## <a name="monitor-geo-replication-configuration-and-health"></a>Geografische herhaling configuratie en servicestatus bewaken

Controle taken opnemen van de geografische-replicatie-configuratie voor controle en gegevens herhaling servicestatus bewaken.  

[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) kan worden gebruikt om op te halen van de informatie over de koppelingen doorsturen herhaling in de weergave van de catalogus sys.geo_replication_links zichtbaar.

De volgende opdracht opgehaald status van de replicatiekoppeling tussen de hoofddatabase 'mijndb' en de secundaire op server "srv2" van de resource groep "rg2".

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”


## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over actieve geografische-replicatie, - [Actieve geografische-herhaling](sql-database-geo-replication-overview.md)
- Zie [overzicht van Business-bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's,

