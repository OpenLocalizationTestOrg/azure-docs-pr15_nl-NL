<properties
    pageTitle="Aan de slag met elastische database taken"
    description="het gebruik van elastische database taken"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Aan de slag met elastische Database taken

Elastische Database taken (preview) voor Azure SQL-Database, kunt u betrouwbaarheid T-SQL-scripts die meerdere databases beslaan tijdens het automatisch opnieuw en eventuele voltooiing garanties trainingen uitvoeren. Voor meer informatie over de functie van de taak elastische Database, raadpleegt u de [overzichtspagina van functies](sql-database-elastic-jobs-overview.md).

In dit onderwerp breidt de steekproef zijn gevonden in [aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md). Als voltooid, wordt u: meer informatie over het maken en beheren van taken die een groep van gerelateerde databases beheren. Het is niet verplicht voor het gebruik van de hulpmiddelen elastische schaal om optimaal te profiteren van de voordelen van elastische taken.

## <a name="prerequisites"></a>Vereisten voor

Download en voer de [aan de slag met elastische Database extra steekproef](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Een shard kaart manager gebruik van de steekproef-app maken

Hier maakt u een kaart shard manager samen met verschillende shards, gevolgd door het invoegen van gegevens in de shards. Als u al shards instellen met een laptopgeheugen gegevens erin hebt, kunt u de volgende stappen overslaan en verplaatsen naar de volgende sectie.

1. Maken en uitvoeren van de steekproef **aan de slag met elastische Databasehulpmiddelen** toepassing. Volg de stappen tot stap 7 in de sectie [downloaden en uitvoeren van de steekproef-app](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools). Aan het einde van stap 7 ziet u de volgende opdrachtprompt:

    ![opdrachtprompt][1]

2.  Typ '1' in het opdrachtvenster en druk op **Enter**. Hiermee wordt gemaakt van de shard kaart manager en twee shards toevoegen aan de server. Typ "3" vervolgens en druk op **Enter**. Herhaal deze actie viermaal drukken. Dit voorbeeld gegevensrijen ingevoegd in uw shards.

3.  De [Portal van Azure](https://portal.azure.com) drie nieuwe databases moet worden weergegeven in uw v12-server:

    ![Bevestiging voor Visual Studio][2]

    We gaan nu een aangepaste database-siteverzameling die overeenkomt met alle databases in de map shard maken. Hierdoor kunnen wij maken en uitvoeren van een taak die een nieuwe tabel toevoegen via shards.

Hier zou meestal maken we een doel voor shard map met de cmdlet **New-AzureSqlJobTarget** . De shard kaart manager-database moet worden ingesteld als het doel van een database en vervolgens de kaart bepaald shard is opgegeven als doel. We gaan in plaats daarvan opsommen alle databases in de server en de databases toevoegen aan de nieuwe aangepaste verzameling met uitzondering van de hoofddatabase.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Hiermee maakt u een aangepaste verzameling en telt alle databases in de server naar de doelsite aangepaste siteverzameling met uitzondering van de basispagina.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Een T-SQL-Script voor uitvoering tussen databases maken

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>De taak om een script uitvoeren in de aangepaste groep van databases maken

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>De taak uitvoeren 

Het volgende PowerShell-script kan worden gebruikt om een bestaande taak uitvoeren:

Update voor de volgende variabele zodat de gewenste taaknaam hebt uitgevoerd:

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>De status van een uitvoering één taak ophalen

Gebruik de dezelfde **Get-AzureSqlJobExecution** -cmdlet met de parameter **IncludeChildren** om de status van de onderliggende taak executions, namelijk de specifieke staat voor de taakuitvoering van elke ten opzichte van elke database door de taak is gericht weergeven.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>De staat in meerdere taak executions weergeven

De cmdlet **Get-AzureSqlJobExecution** heeft meerdere optionele parameters die kunnen worden gebruikt om meerdere uitvoering, gefilterd door de meegeleverde parameters weer te geven. Het volgende voorbeeld toont aantal mogelijke manieren Get-AzureSqlJobExecution gebruiken:

Alle actieve bovenste niveau taak executions ophalen:

    Get-AzureSqlJobExecution

Alle bovenste niveau uitvoering, inclusief inactieve taken executions ophalen:

    Get-AzureSqlJobExecution -IncludeInactive

Alle onderliggende taak executions van een opgegeven taak kan worden uitgevoerd-ID, inclusief inactieve taken executions ophalen:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Alle uitvoering gemaakt met behulp van een planning ophalen / combinatie, met inbegrip van niet-actieve taken van taken:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Alle taken die een opgegeven shard kaart, met inbegrip van niet-actieve taken doelgroepen ophalen:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Alle taken die een opgegeven aangepaste verzameling, met inbegrip van niet-actieve taken doelgroepen ophalen:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
De lijst met taak taak executions binnen een bepaalde taak kan worden uitgevoerd ophalen:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Taakgegevens voor uitvoering taak ophalen:

Het volgende PowerShell-script kan worden gebruikt om weer te geven van de details van een taak taak worden uitgevoerd, dat vooral handig is als foutopsporing fouten bij de uitvoering.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="retrieve-failures-within-job-task-executions"></a>Fouten in uitvoering taak ophalen

Het object JobTaskExecution bevat een eigenschap voor de levenscyclus van de taak samen met de berichteigenschap van een. Als een taak taak niet uitvoeren, de levenscyclus van de eigenschap wordt ingesteld op *mislukt* en de bericht-eigenschap ingesteld op de resulterende uitzonderingsbericht en de stapel. Als een taak is mislukt, is het belangrijk om weer te geven van de details van projecttaken die voor een bepaalde taak is mislukt.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Wachten op een taak kan worden uitgevoerd om te voltooien

Het volgende PowerShell-script kan worden gebruikt om te wachten op een taak definiëren om uit te voeren:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Een aangepaste uitvoering-beleid maken

Elastische Database taken ondersteunt het maken van aangepaste uitvoering beleidsregels die kunnen worden toegepast tijdens het starten van taken.
  
Beleid voor het uitvoeren van mogen momenteel voor definiëren:

* Naam:-Id voor het beleid kan worden uitgevoerd.
* Time-out van de taak: Totale tijd voordat een taak door elastische Database taken worden geannuleerd.
* Eerste Interval voor nieuwe pogingen: Interval wachten voordat de eerste opnieuw.
* Maximale Interval voor nieuwe pogingen: Initiaal van intervallen voor nieuwe pogingen om te gebruiken.
* Probeer Interval Backoff coëfficiënt: Coëfficiënt wordt gebruikt voor het volgende interval tussen pogingen om opnieuw te berekenen.  De volgende formule wordt gebruikt: (eerste opnieuw Interval) * Math.pow ((Interval Backoff coëfficiënt), (getal van pogingen) - 2). 
* Maximum aantal pogingen: Het maximum aantal opnieuw probeert uit te voeren binnen een project.

Het standaardbeleid voor uitvoering gebruikt de volgende waarden:

* Naam: Uitvoering standaardbeleid
* Time-out van de taak: 1 week
* Eerste Interval voor nieuwe pogingen: 100 milliseconden
* Maximale Interval voor nieuwe pogingen: 30 minuten
* Probeer Interval coëfficiënt: 2
* Maximum aantal pogingen: 2.147.483.647

Het gewenste execution-beleid maken:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Een aangepaste uitvoeringsbeleid bijwerken

Update voor de gewenste beleid bijwerken:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Een taak annuleren

Elastische Database taken ondersteunt taken annulering aanvragen.  Als elastische Database taken worden gedetecteerd een annuleringsverzoek voor een taak die momenteel wordt uitgevoerd, wordt geprobeerd de taak beëindigen.

Er zijn twee manieren dat elastische Database taken annulering kunnen uitvoeren:

1. Heffen momenteel uitgevoerd taken: als annulering wordt aangetroffen terwijl er een taak momenteel wordt uitgevoerd, annulering wordt geprobeerd binnen de momenteel uitgevoerde aspecten van de taak.  Bijvoorbeeld: als er een langdurige query momenteel wordt uitgevoerd wanneer een annulering wordt uitgevoerd, wordt er een poging om te annuleren van de query.
2. Annuleren taak pogingen: Als annulering wordt gedetecteerd door de control-thread voordat een taak voor uitvoering wordt gestart, de control-thread wordt voorkomen dat het starten van de taak en het verzoek declareren als geannuleerd.

Als een taak is geannuleerd voor een bovenliggende taak wordt aangevraagd, wordt het annuleringsverzoek worden uitgevoerd voor de bovenliggende taak en alle bijbehorende onderliggende taken.
 
Als u wilt een aanvraag annulering indient, gebruik de cmdlet **Stop-AzureSqlJobExecution** en de parameter **JobExecutionId** instellen.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Een taak door de naam van de taak geschiedenis verwijderen

Elastische Database taken ondersteunt asynchroon verwijdering van taken. Een taak kan worden gemarkeerd om te worden verwijderd en wordt verwijderd de taak en alle bijbehorende werkervaring nadat u alle taak-executions voor de taak hebt voltooid. Het systeem wordt niet automatisch actieve taak executions geannuleerd.  

Stop-AzureSqlJobExecution moet in plaats daarvan worden aangeroepen om te annuleren actieve taak executions.

Als u wilt activeren taak verwijdering, gebruik de cmdlet **Verwijderen-AzureSqlJob** en stel de parameter **Taaknaam** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Het doel van een aangepaste database maken
Aangepaste database doelen kunnen worden gedefinieerd in elastische Database taken die kunnen worden gebruikt voor uitvoering rechtstreeks of voor opname in een aangepaste database-groep. Aangezien **elastische Database-toepassingen** worden niet nog rechtstreeks ondersteund via de PowerShell APIs, maak u gewoon een aangepaste database doel- en aangepaste database siteverzameling doel waarin alle databases in de groep omvat.

De volgende variabelen aan de gewenste database-informatie instellen:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Een doel van de siteverzameling aangepaste database maken
Een doel van de siteverzameling aangepaste database kan worden gedefinieerd om in te schakelen worden uitgevoerd in meerdere gedefinieerde database doelen. Nadat een groep database is gemaakt, is databases kunnen worden gekoppeld aan het doel van de aangepaste siteverzameling.

De volgende variabelen aan de gewenste aangepaste verzameling doel-configuratie instellen:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Databases toevoegen aan een aangepaste database siteverzameling doel

Database doelen kunnen worden gekoppeld aan de aangepaste database siteverzameling doelen een groep van databases maken. Wanneer een taak is gemaakt die is bedoeld voor een doel van de siteverzameling aangepaste database, wordt deze uitgepakt als u wilt afstemmen van de databases die zijn gekoppeld aan de groep op het moment van worden uitgevoerd.

De gewenste database toevoegen aan een specifieke aangepaste verzameling:

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>De databases binnen een doel van de siteverzameling aangepaste database controleren

Gebruik de cmdlet **Get-AzureSqlJobTarget** om op te halen van de onderliggende databases binnen een doel van de siteverzameling aangepaste database. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Een taak als u wilt een script uitvoeren via een doel van de siteverzameling aangepaste database maken

Gebruik de cmdlet **New-AzureSqlJob** een taak ten opzichte van een groep van databases die zijn gedefinieerd door een doel van de siteverzameling aangepaste database wilt maken. Elastische Database taken wordt de taak in meerdere onderliggende taken elke overeenkomt met een database die is gekoppeld aan het doel van de siteverzameling aangepaste database uitvouwen en zorg ervoor dat het script is uitgevoerd voor elke database. Nogmaals, is het belangrijk dat scripts idempotency is ingeschakeld zijn tegen pogingen om opnieuw te worden.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Gegevens verzamelen over databases

**Elastische Database taken** ondersteuning biedt voor een query wordt uitgevoerd binnen een groep van databases en stuurt de resultaten naar een opgegeven databasetabel. De tabel kan worden doorzocht na het feit om de resultaten van de query uit elke database weer te geven. Hier vindt u een asynchroon manier voor het uitvoeren van een query over veel databases. Fout bij zaken zoals een van de databases wordt tijdelijk niet beschikbaar is worden automatisch verwerkt via pogingen.

De opgegeven doeltabel worden automatisch gemaakt als dit nog niet bestaat, overeenkomen met het schema van de geretourneerde resultaten. Als een script uitvoeren van meerdere resultatensets retourneert, wordt de eerste fase naar de meegeleverde doeltabel alleen in elastische Database taken verzenden.

Het volgende PowerShell-script kan worden gebruikt voor het uitvoeren van een script voor het verzamelen van de resultaten in een opgegeven tabel. Dit script wordt ervan uitgegaan dat een T-SQL-script is gemaakt waarin een set één resultaat oplevert en een doel van de siteverzameling aangepaste database is gemaakt.

Stel de volgende handelingen uit om de gewenste script, referenties en execution doel aan te geven:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Maken en starten van een taak voor scenario's voor het verzamelen van gegevens
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Maken van een schema voor uitvoering met een taak-trigger

Het volgende PowerShell-script kan worden gebruikt om een terugkerende schema te maken. Dit script gebruikt een interval één minuut, maar nieuw-AzureSqlJobSchedule ondersteunen ook - DayInterval, - HourInterval, - MonthInterval, en -WeekInterval parameters. Schema's die slechts één keer uitvoeren kunnen worden gemaakt door doorgeven - eenmalige.

Een nieuwe planning maken:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Een taak-trigger om een taak is uitgevoerd op een tijdschema maken

Een taak-trigger kan worden gedefinieerd als u wilt dat een taak volgens de planning van een tijd wordt uitgevoerd. Het volgende PowerShell-script kan worden gebruikt voor het maken van een taak-trigger.

De volgende variabelen zodat ze overeenkomen met de gewenste taak en planning instellen:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Een geplande koppeling stoppen met taak wordt uitgevoerd op de planning verwijderen

Als u wilt voorkomen dat bepaalde terugkerende taak kan worden uitgevoerd door een taak-trigger, kan de trigger taak worden verwijderd. Een taak-trigger als u wilt stoppen van een taak uit te voeren op basis van een planning met de cmdlet **Verwijderen-AzureSqlJobTrigger** verwijderen.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>De queryresultaten elastische-database importeren in Excel

 U kunt de resultaten van van een query naar een Excel-bestand importeren.

1. Start Excel 2013.
2.  Ga naar het lint **gegevens** .
3.  Klik op **Uit een andere bron** en klik op **Van SQL Server**.

    ![Excel importeren uit andere bronnen][5]
4.  Typ in de **Wizard Gegevensverbinding** de referenties voor de naam en login. Klik vervolgens op **volgende**.
5.  Selecteer in het dialoogvenster **Selecteer de database die de gewenste gegevens bevat**, de **ElasticDBQuery** -database.
6.  Selecteer de tabel **klanten** in de lijstweergave en klik op **volgende**. Klik vervolgens op **Voltooien**.
7.  Selecteer **tabel** in het formulier **Gegevens importeren** , klikt u onder **Selecteer de gewenste deze gegevens in uw werkmap moeten worden weergegeven**, en klik op **OK**.

Alle rijen uit de tabel **klanten** , die zijn opgeslagen in verschillende shards vullen het Excel-werkblad.

## <a name="next-steps"></a>Volgende stappen
Nu kunt u de Excel-gegevens-functies. De verbindingsreeks gebruiken met uw servernaam, de naam van de database en de referenties verbinding maken met de hulpmiddelen voor BI en gegevens-integratie voor de querydatabase elastische. Zorg ervoor dat de SQL Server als gegevensbron voor een hulpmiddel in uw wordt ondersteund. Raadpleeg de elastische querydatabase en de externe tabellen net zoals elke andere database van SQL Server en SQL Server-tabellen die u met uw hulpmiddel verbinden wilt.

### <a name="cost"></a>Kosten
Er is geen extra kosten voor het gebruik van de functie van de query elastische Database. Echter op dit moment deze functie is alleen beschikbaar op premium-databases als het eindpunt te slepen, maar de shards kunnen worden van een service.

Zie [Meer informatie het prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)voor de prijsinformatie.


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
