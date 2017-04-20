<properties
    pageTitle="Upgrade naar Azure SQL Database V12 via PowerShell | Microsoft Azure"
    description="Wordt uitgelegd hoe u een upgrade uitvoeren naar Azure SQL Database V12 waaronder het Web en bedrijfsdatabases upgraden en het upgraden van een V11: server zijn databases migreren rechtstreeks in een elastische database-toepassingen via PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Upgrade naar Azure SQL Database V12 via PowerShell


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


SQL-Database V12 is de nieuwste versie, dus het upgraden naar SQL-Database V12 wordt aanbevolen.
SQL-Database V12 heeft vele [voordelen ten opzichte van de vorige versie](sql-database-v12-whats-new.md) waaronder:

- Verbeterde compatibiliteit met SQL Server.
- Verbeterde premium prestaties en nieuwe prestaties.
- [Elastische database-toepassingen](sql-database-elastic-pool.md).

In dit artikel vindt u aanwijzingen voor het upgraden van bestaande V11: SQL-Database-servers en databases naar SQL-Database V12.

Tijdens het proces voor het upgraden naar V12, upgraden u eventuele Web en bedrijven databases naar een nieuwe servicelaag zodat aanwijzingen voor het upgraden van Web en bedrijfsdatabases opgenomen zijn.

Daarnaast kan migreren naar een [groep elastische database](sql-database-elastic-pool.md) zijn kosten efficiënter dan een upgrade naar individuele prestatieniveaus (prijzen lagen) voor één databases. Databasebeheer van de vereenvoudigen van toepassingen ook omdat u hoeft slechts eenmaal voor het beheren van de prestatie-instellingen voor de groep in plaats van de prestaties van afzonderlijke databases afzonderlijk beheren. Als u databases op meerdere servers hebt, kunt u ze worden verplaatst naar de dezelfde server en profiteert van ze in een groep te plaatsen.

U kunt de stappen in dit artikel voor eenvoudig migreren van databases V11: rechtstreeks in elastische database pools.

Houd er rekening mee dat uw databases online blijft en gaat u verder met het werk voor de hele de upgrade uitvoert. Op het moment van de werkelijke overgang naar de nieuwe prestatieniveau tijdelijke weghalen verbindingen met de database kan kan optreden voor de duur van een zeer klein die meestal ongeveer 90 seconden maar zoveel 5 minuten. Als uw toepassing [tijdelijke foutenstructuuranalyse verwerking bij verbinding afsluitingen worden](sql-database-connectivity-issues.md) is het voldoende bescherming tegen decoratieve verbindingen aan het einde van de upgrade.

Een upgrade naar SQL-Database V12 kan niet ongedaan worden gemaakt. Na een upgrade kan niet de server naar V11: worden hersteld.

Na een upgrade naar V12, wordt [service laag aanbevelingen](sql-database-service-tier-advisor.md) en [elastische groep aanbevelingen](sql-database-elastic-pool-create-portal.md) onmiddellijk pas beschikbaar als de service heeft tijd uw werkbelasting op de nieuwe server wordt berekend. V11: server aanbeveling geschiedenis geldt niet voor V12 servers, zodat deze blijft niet behouden.  

## <a name="prepare-to-upgrade"></a>Upgrade voorbereiden

- **Alle Web en bedrijfsdatabases upgraden**: de portal gebruikt of [PowerShell voor het upgraden van databases en server](sql-database-upgrade-server-powershell.md).
- **Controleren en geografische herhaling schorten**: als uw Azure SQL-database is geconfigureerd voor geografische-replicatie moet u de huidige configuratie en de [geografische herhaling stoppen](sql-database-geo-replication-portal.md#remove-secondary-database)document. Nadat de upgrade is voltooid de configuratie van uw database voor geografische-replicatie.
- **Open deze poorten als er clients op een Azure-VM**: als uw clientprogramma verbinding maakt met SQL-Database V12 terwijl de klant wordt uitgevoerd op een Azure virtuele machine (VM), moet u bereikwaarden 11000-11999 en 14000-14999 op de VM openen. Zie [poorten voor V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)voor meer informatie.


## <a name="prerequisites"></a>Vereisten voor

Als u wilt een server bijwerkt naar V12 met PowerShell, moet u beschikken over de meest recente Azure PowerShell geïnstalleerd en uit te voeren. Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor gedetailleerde informatie.


## <a name="configure-your-credentials-and-select-your-subscription"></a>Uw referenties configureren en selecteert u uw abonnement

Als u wilt PowerShell-cmdlets voor uw abonnement op Azure uitgevoerd moet u eerst access instellen bij uw Azure-account. Voer de volgende opdracht uit en u krijgt een aanmeldingsscherm uw referenties invoeren. Gebruik de dezelfde e-mailadres en wachtwoord waarmee u zich aanmelden bij de portal van Azure.

    Add-AzureRmAccount

Na het succes aanmelden ziet u enkele informatie op scherm dat bevat de Id die u aangemeld met en de Azure abonnementen die u toegang tot hebt.

Als u wilt selecteren van het abonnement dat u wilt werken met u moet uw abonnements-Id (**-SubscriptionId**) of een abonnement een naam geven (**-SubscriptionName**). U kunt deze van de vorige stap kopiëren of als u meerdere abonnementen hebt u de cmdlet **Get-AzureRmSubscription** kunt uitvoeren en kopieer de gewenste abonnementsgegevens uit de resultaatset.

De volgende cmdlet uitvoeren met de informatie over uw abonnement voor het instellen van uw huidige abonnement:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

De volgende opdrachten wordt uitgevoerd voor het abonnement dat u zojuist hebt geselecteerd hierboven.

## <a name="get-recommendations"></a>Aanbevelingen krijgen

Om de aanbeveling alleen voor de upgrade van de server de volgende cmdlet uitvoeren:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

Zie [een toepassingen elastische database maken](sql-database-elastic-pool-create-portal.md) en [Azure SQL-Database prijzen laag aanbevelingen](sql-database-service-tier-advisor.md)voor meer informatie.



## <a name="start-the-upgrade"></a>De upgrade starten

Start de upgrade van de server de volgende cmdlet uitvoeren:

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Tijdens het uitvoeren van wordt deze opdracht-upgradeproces gestart. U kunt de uitvoer van de aanbeveling aanpassen en geef de bewerkte aanbeveling voor deze cmdlet.


## <a name="upgrade-a-server"></a>Upgraden van een server


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Aangepaste toewijzing van upgrade

Als de aanbevelingen niet geschikt voor de server en bedrijven hoofdletters/kleine letters zijn, kunt klikt u vervolgens u kiezen hoe uw databases worden bijgewerkt en deze kunnen toewijzen aan enkele of elastische databases.

Parameters ElasticPoolCollection en DatabaseCollection zijn optioneel:

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Databases na een upgrade naar SQL-Database V12 controleren


Na een upgrade uitvoert, wordt het aanbevolen om de database actief om ervoor te zorgen toepassingen op de gewenste prestaties worden uitgevoerd en gebruik optimaliseren zo nodig de te houden.

Bovendien naar afzonderlijke databases die u kunt controleren elastische database groepen [met behulp van de portal](sql-database-elastic-pool-manage-portal.md) voor controle of met [PowerShell](sql-database-elastic-pool-manage-powershell.md)


**Verbruik resourcegegevens:** Resourcegegevens verbruik is voor Basic, Standard en Premium databases beschikbaar via de [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV in de database. Deze DMV vindt u in de buurt van realtime verbruik resourcegegevens op 15 tweede granulatie het vorige uur bewerking. Het DTU percentage verbruik op een interval wordt berekend als het maximumpercentage verbruik van de processor, IO en log dimensies. Hier ziet u een query te berekenen van het gemiddelde DTU percentage verbruik via het laatste uur:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Aanvullende controlegegevens:

- [Azure SQL-Database prestaties richtlijnen voor één databases](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Aandachtspunten voor de prijs en prestaties voor een elastische database-toepassingen](sql-database-elastic-pool-guidance.md).
- [Cmdlets voor controle Azure SQL-Database dynamische management weergaven gebruiken](sql-database-monitoring-with-dmvs.md)



**Waarschuwingen:** 'Waarschuwingen instellen ' in de portal van Azure om u te waarschuwen wanneer het verbruik DTU voor de upgrade van database bepaalde hoog niveau nadert. Meldingen van de database kunnen worden ingesteld in Azure portal voor verschillende prestatiegegevens zoals DTU, CPU, IO en Log. Blader naar de database en selecteer **waarschuwingsregels** in het blad **Instellingen** .

U kunt bijvoorbeeld instellen van een e-mailwaarschuwing van "DTU Percentage" Als de gemiddelde waarde voor DTU percentage groter is dan 75% over de laatste 5 minuten. Raadpleegt u [meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) voor meer informatie over het configureren van waarschuwingen.



## <a name="next-steps"></a>Volgende stappen

- [Een toepassingen elastische database maken](sql-database-elastic-pool-create-portal.md) en toevoegen van sommige of alle databases in de groep.
- [Het serviceniveau laag en prestaties van uw database wijzigen](sql-database-scale-up.md).



## <a name="related-information"></a>Gerelateerde informatie

- [Get-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Start-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Stop-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)
