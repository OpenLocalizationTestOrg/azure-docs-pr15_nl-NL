<properties 
    pageTitle="Een elastische database-toepassingen (PowerShell) beheren | Microsoft Azure" 
    description="Leer hoe u PowerShell gebruiken voor het beheren van een elastische database-toepassingen."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Bewaken en beheren van een toepassingen elastische database met PowerShell 

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Het beheren van een [groep elastische database](sql-database-elastic-pool.md) met PowerShell-cmdlets. 

Zie voor algemene foutcodes, [SQL-foutcodes voor SQL-Database-clienttoepassingen: Database verbindingsfout en andere problemen](sql-database-develop-error-messages.md).

Waarden voor toepassingen vindt u in de [limieten voor eDTU en opslag](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases). 

## <a name="prerequisites"></a>Vereisten voor

* Azure PowerShell 1.0 of hoger. Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor gedetailleerde informatie.
* Elastische database-toepassingen zijn alleen beschikbaar met SQL-Database V12-servers. Als u een V11: SQL-Database-server, [PowerShell als u wilt upgraden naar V12 en maken van een groep gebruiken](sql-database-upgrade-server-portal.md) in één stap hebt.


## <a name="move-a-database-into-an-elastic-pool"></a>Een database verplaatsen naar een elastische toepassingen

U kunt een database verplaatsen in- of uitzoomen op een groep met de [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx). 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Instellingen van de prestaties van een groep wijzigen

Wanneer de prestaties suffers, kunt u de instellingen van de groep zodat groei. Gebruik de cmdlet [Set-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) . Stel de parameter - Dtu op de eDTUs per toepassingen. Zie [limieten voor eDTU en opslag](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) voor mogelijke waarden.  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>De status van de groep bewerkingen ophalen

Maken van een groep kan duren. Als u wilt de status van toepassingen bewerkingen kunnen maken en updates bijhouden, gebruikt u de cmdlet [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) .

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>De status van een elastische-database verplaatsen in- en afmelden bij een groep ophalen

Een database verplaatsen kan duren. De status van een verplaatsen met de cmdlet [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) bijhouden.

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>Resource gebruiksgegevens ophalen voor een groep

Aan de doelstellingen die kunnen worden opgehaald als een percentage van de limiet van de resource:   


| De naam van de metrische | Beschrijving |
| :-- | :-- |
| CPU\_procent | Het gemiddelde berekenen gebruik in percentage van de limiet van de groep. |
| Fysiek\_gegevens\_lezen\_procent | Gemiddeld i/o-gebruik percentage op basis van de limiet van de groep. |
| Log\_schrijven\_procent | Gemiddelde schrijven Resourcegebruik in percentage van de limiet van de groep. | 
| DTU\_verbruik\_procent | Gebruik van de gemiddelde eDTU in percentage van eDTU limiet voor de toepassingen | 
| opslag\_procent | Het gebruik van de gemiddelde opslagruimte in percentage van de opslaglimiet van de groep. |  
| werknemers\_procent | Maximale gelijktijdige werknemers (aanvragen) percentage op basis van de limiet van de groep. |  
| sessies\_procent | Maximale aantal gelijktijdige sessies percentage op basis van de limiet van de groep. | 
| eDTU_limit | Huidige max elastische groep DTU instelling voor deze elastische toepassingen tijdens dit interval. |
| opslag\_limiet | Huidige max elastische groep opslaglimiet instellen voor deze elastische toepassingen in megabytes tijdens dit interval. |
| eDTU\_gebruikt | Gemiddelde eDTUs gebruikt door de toepassingen in dit interval. |
| opslag\_gebruikt | Gemiddelde opslag gebruikt door de toepassingen in dit interval in bytes |

**Aan de doelstellingen granulatie/bewaarperiode:**

* Gegevens worden geretourneerd bij 5 minuten granulatie.  
* Gegevensretentie is 35 dagen.  

Deze cmdlet en API beperkt het aantal rijen die kunnen worden opgehaald met één aanroep 1000 rijen (bijna 3 dagen bij 5 minuten granulatie). Maar deze opdracht kan worden aangeroepen meerdere keren met tijdsintervallen van verschillende begin/einde meer gegevens worden opgehaald 

Aan de doelstellingen ophalen:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Resource gebruiksgegevens ophalen voor een elastische-database

Deze API's zijn hetzelfde als de huidige (V12)-API's gebruikt voor het Resourcegebruik van een database zelfstandige, behalve voor de volgende semantische verschil cmdlets voor controle. 

Opgehaald van de doelstellingen zijn voor deze API, uitgedrukt als een percentage van de per max eDTUs (of gelijkwaardige initiaal voor de onderliggende meetwaarde zoals CPU, IO enzovoort) instellen voor die groep. Bijvoorbeeld 50%-gebruik van een van deze gegevens wordt aangeduid dat het verbruik van specifieke middelen op 50% van de database initiaal limiet voor de desbetreffende resource in de bovenliggende groep per. 

Aan de doelstellingen ophalen:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Een waarschuwing toevoegen aan een resourceweergave van toepassingen

U kunt waarschuwingsregels toevoegen aan resources e-mailmeldingen of tekenreeksen met de waarschuwing verzenden naar [URL eindpunten](https://msdn.microsoft.com/library/mt718036.aspx) wanneer de resource raakt een gebruik drempel die u hebt ingesteld. Gebruik de cmdlet toevoegen-AzureRmMetricAlertRule. 

> [AZURE.IMPORTANT]Resourcegebruik monitoring voor elastische toepassingen heeft een vertraging van ten minste 20 minuten. Instellen van waarschuwingen van minder dan 30 minuten voor elastische toepassingen wordt momenteel niet ondersteund. Waarschuwingen instellen voor elastische toepassingen op een punt (parameter met de naam '-venstergrootte "in PowerShell-API) van minder dan 30 minuten niet geactiveerd. Zorg ervoor dat die u voor elastische toepassingen definieert waarschuwingen een bepaalde periode (venstergrootte) 30 minuten of langer gebruikt.

In dit voorbeeld wordt een waarschuwing voor het ophalen van een melding ontvangen wanneer een groep van toepassingen eDTU verbruik boven bepaalde drempel overschrijdt gaat toegevoegd.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Waarschuwingen op alle databases in een groep toevoegen

U kunt waarschuwingsregels toevoegen aan alle database in een elastische toepassingen meldingen per e-mail verzenden of waarschuwing tekenreeksen naar [URL eindpunten](https://msdn.microsoft.com/library/mt718036.aspx) wanneer een resource raakt een drempelwaarde voor gebruik door de melding voor een worden ingesteld. 

> [AZURE.IMPORTANT] Resourcegebruik monitoring voor elastische toepassingen heeft een vertraging van ten minste 20 minuten. Instellen van waarschuwingen van minder dan 30 minuten voor elastische toepassingen wordt momenteel niet ondersteund. Waarschuwingen instellen voor elastische toepassingen op een punt (parameter met de naam '-venstergrootte "in PowerShell-API) van minder dan 30 minuten niet geactiveerd. Zorg ervoor dat die u voor elastische toepassingen definieert waarschuwingen een bepaalde periode (venstergrootte) 30 minuten of langer gebruikt.

In dit voorbeeld wordt een melding aan elk van de databases in een groep voor het ophalen van een melding ontvangen wanneer de die database DTU verbruik boven bepaalde drempel overschrijdt gaat.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Verzamelen en bijhouden van gegevens over zoekgebruik resource over meerdere toepassingen in een abonnement

Wanneer u een groot aantal databases in een abonnement hebt, is het lastig zijn om de elke elastische toepassingen afzonderlijk te houden. In plaats daarvan kunnen SQL-database PowerShell-cmdlets en T-SQL-query's worden gecombineerd resource gebruiksgegevens verzamelen van meerdere toepassingen en hun databases voor controle en analyse van Resourcegebruik. Een [Voorbeeldimplementatie](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) van dergelijke een reeks powershell-scripts vindt u in de bibliotheek GitHub SQL Server voorbeelden samen met documentatie op acties en hoe u deze gebruiken.

Als u wilt gebruiken in dit voorbeeld-implementatie volgt onderstaande.


1. Download de [scripts en documentatie](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Wijzig de scripts voor uw omgeving. Geef een of meer servers waarop elastische groepen worden gehost.
3. Geef een telemetry database waar de doelstellingen van de verzamelde zijn opgeslagen. 
4. Pas het script om op te geven van de duur van de de scripts worden uitgevoerd.

Op hoog niveau volgt de scripts als:

*   Opgesomd in een bepaald Azure abonnement (of een opgegeven aantal servers) op alle servers.
*   Een achtergrondtaak voor elke server uitvoert. De taak wordt uitgevoerd in een lus met regelmatige tussenpozen en verzamelt telemetriegegevens voor alle toepassingen op de server. De verzamelde gegevens laadt deze vervolgens in de opgegeven telemetrielogboek-database.
*   Een lijst met databases in elke toepassingen voor het verzamelen van het hulpprogramma voor het gebruik van de database-resourcegegevens opgesomd. De verzamelde gegevens laadt deze vervolgens in het telemetrielogboek-database.

De doelstellingen van de verzameld in de database telemetrielogboek kunnen worden geanalyseerd om de status van elastische pools en de databases erin de te houden. Het script installeert ook een vooraf gedefinieerde tabel-waarde, functie (TVF) in de database telemetrielogboek aggregatie zodat de doelstellingen voor een opgegeven tijdvenster valt. Bijvoorbeeld kunnen resultaten van de TVF worden gebruikt om weer te geven "bovenste N elastische pools met het gebruik van de maximale eDTU in een opgegeven tijdvenster." Gebruik desgewenst analytische hulpprogramma's zoals Excel of Power BI opvragen en de verzamelde gegevens te analyseren.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Voorbeeld: resource verbruik van de doelstellingen voor een groep en de databases ophalen

In dit voorbeeld wordt het gebruik van de doelstellingen voor een bepaald elastische resourcegroep en alle bijbehorende databases opgehaald. Verzamelde gegevens is opgemaakt en naar een CSV-bestand voor opgemaakte geschreven. Het bestand kan worden bekeken met Excel.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Latentie van elastische groep bewerkingen

- Het min-eDTUs per database of max eDTUs per database meestal wijzigen is voltooid, 5 minuten of minder.
- De eDTUs per van toepassingen wijzigen, is afhankelijk van de totale hoeveelheid ruimte gebruikt door alle databases in de groep. Wijzigingen gemiddelde 90 minuten of minder per 100 GB. Als de totale ruimte gebruikt door alle databases in de groep is bijvoorbeeld 200 GB, en vervolgens de verwachte latentie voor het wijzigen van de groep eDTU per toepassingen is 3 uur of minder.

## <a name="migrate-from-v11-to-v12-servers"></a>Migreren van V11: naar V12 servers

PowerShell-cmdlets zijn beschikbaar voor starten, stoppen of een upgrade naar Azure SQL Database V12 uit V11: of een andere versie van de pre-V12 controleren.

- [Upgrade naar SQL-Database V12 via PowerShell](sql-database-upgrade-server-powershell.md)

Voor de documentatie over deze PowerShell-cmdlets, raadpleegt u:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Start-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Stop-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


De Stop-cmdlet betekent annuleren, niet onderbreken. Er is geen manier om een upgrade, dan opnieuw te starten vanaf het begin hervatten. De Stop-cmdlet opgeschoond en versies van alle gewenste resources.

## <a name="next-steps"></a>Volgende stappen

- [Elastische taken maken](sql-database-elastic-jobs-overview.md) Elastische taken kunnen u T-SQL-scripts uitvoeren op een willekeurig aantal databases in de groep.
- Zie [schaal af met Azure SQL-Database](sql-database-elastic-scale-introduction.md): elastische Databasehulpmiddelen gebruiken als u wilt schalen, verplaats gegevens, query, of transacties maken.
