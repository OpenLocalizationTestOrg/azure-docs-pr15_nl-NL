<properties 
    pageTitle="Maken en beheren van elastische Database taken via PowerShell" 
    description="PowerShell gebruikt voor het beheren van Azure SQL Database-toepassingen" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Maken en beheren van een SQL-Database elastische database taken via PowerShell (preview)

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)



De PowerShell-APIs voor **elastische Database taken** (in preview), kunt u definiëren van een groep van databases die scripts worden uitgevoerd. In dit artikel leest hoe u maken en beheren van **elastische Database taken** met PowerShell-cmdlets. Zie [overzicht van de elastische taken](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Vereisten voor
* Een Azure-abonnement. Zie voor een gratis proefversie, [gratis proefversie voor één maand](https://azure.microsoft.com/pricing/free-trial/).
* Een reeks databases die zijn gemaakt met de hulpmiddelen voor elastische databases. Zie [aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor gedetailleerde informatie.
* **Elastische Database taken** PowerShell-pakket: overzicht van [projecten elastische Database installeren](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Selecteer uw Azure abonnement

Het abonnement dat u uw abonnements-Id (**-SubscriptionId**) of een abonnement nodig een naam geven (**-SubscriptionName**) om te selecteren. Als u meerdere abonnementen kunt u de cmdlet **Get-AzureRmSubscription** uitvoeren en kopieer de gewenste abonnementsgegevens uit de resultatenset hebt. Nadat u de informatie over uw abonnement hebt, voert u de volgende commandlet dit abonnement als het standaard, namelijk het doel voor het maken en beheren van taken wilt weergeven:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

Het [Filter wissen](https://technet.microsoft.com/library/dd315244.aspx) geschikt is voor gebruik ontwikkelen en PowerShell-scripts ten opzichte van de elastische Database taken uitvoeren.

## <a name="elastic-database-jobs-objects"></a>Elastische databaseobjecten van taken

De volgende tabel bevat alle de objecttypen **elastische Database taken** samen met de beschrijving en de relevante PowerShell APIs.

<table style="width:100%">
  <tr>
    <th>Objecttype</th>
    <th>Beschrijving</th>
    <th>Gerelateerde PowerShell-API 's</th>
  </tr>
  <tr>
    <td>Referentie</td>
    <td>Gebruikersnaam en wachtwoord als u wilt gebruiken wanneer u verbinding maken met databases voor de uitvoering van scripts of toepassing van DACPACs. <p>Het wachtwoord is voordat naar verzenden en opslaan in de database elastische taken van de Database versleuteld.  Het wachtwoord is ontsleuteld door de service elastische taken van de Database via de referentie gemaakt en die vanaf de installatiescript zijn geüpload.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Nieuwe AzureSqlJobCredential</p><p>Set-AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Script</td>
    <td>Transact-SQL-script moet worden gebruikt voor uitvoering in databases.  Het script moet worden gemaakt voor worden idempotency is ingeschakeld, omdat de service wordt opnieuw geprobeerd de uitvoering van het script op fouten.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Nieuwe AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Gegevens laag toepassing </a> pakket dat moet worden toegepast op databases.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Nieuwe AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Doel van de database</td>
    <td>Database en server naam verwijzen naar een Azure SQL-Database.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nieuwe AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Shard kaart doel</td>
    <td>Combinatie van het doel van een database en een referentie moet worden gebruikt om te bepalen de informatie in een Database elastische shard toewijzing opgeslagen.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nieuwe AzureSqlJobTarget</p>
    <p>Set-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Aangepaste siteverzameling doel</td>
    <td>Groep gedefinieerde van databases gezamenlijk gebruiken voor uitvoering.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nieuwe AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Aangepaste siteverzameling onderliggende doel</td>
    <td>Doel database waarnaar wordt verwezen vanuit een aangepaste verzameling.</td>
    <td>
    <p>Toevoegen AzureSqlJobChildTarget</p>
    <p>Verwijderen AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Taak</td>
    <td>
    <p>De definitie van parameters voor een taak die kunnen worden gebruikt om te worden uitgevoerd activeren of om te voldoen aan een planning.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Nieuwe AzureSqlJob</p>
    <p>Set-AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Taak kan worden uitgevoerd</td>
    <td>
    <p>Container van taken die nodig zijn om op een script uitvoeren of een DACPAC toepassen op een doellijst met referenties voor de databaseverbindingen met fouten afgehandeld overeenkomstig een beleid kan worden uitgevoerd.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Stop-AzureSqlJobExecution</p>
    <p>Wachten-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Uitvoering van de taken van taak</td>
    <td>
    <p>Één geheel om te voldoen aan een taak.</p>
    <p>Als een taak is niet kan uitvoeren, de resulterende uitzonderingsbericht worden geregistreerd en een nieuwe overeenkomende projecttaak wordt gemaakt en uitgevoerd overeenkomstig de opgegeven beleid.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Stop-AzureSqlJobExecution</p>
    <p>Wachten-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Taak kan worden uitgevoerd beleid</td>
    <td>
    <p>Besturingselementen taak kan worden uitgevoerd-outs, opnieuw limieten en intervallen tussen nieuwe pogingen.</p>
    <p>Elastische Database taken bevat een taak kan worden uitgevoerd standaardbeleid die in principe oneindig pogingen van taak fouten veroorzaken met exponentiële backoff van intervallen tussen nieuwe pogingen.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Nieuwe AzureSqlJobExecutionPolicy</p>
    <p>Set-AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Planning</td>
    <td>
    <p>Specificatie van voor de uitvoering kan worden uitgevoerd op een terugkerende interval of één keer op basis van tijd.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Nieuwe AzureSqlJobSchedule</p>
    <p>Set-AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Taak-Triggers</td>
    <td>
    <p>Een koppeling tussen een taak en een plan voor uitvoering van de trigger taak volgens de planning.</p>
    </td>
    <td>
    <p>Nieuwe AzureSqlJobTrigger</p>
    <p>Verwijderen AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Ondersteunde elastische Database taken groeperen typen
De taak voert Transact-SQL (T-SQL)-scripts of toepassing van DACPACs binnen een groep van databases. Wanneer een taak moet worden uitgevoerd binnen een groep van databases wordt verzonden, de taak 'wordt uitgebreid met"de in onderliggende taken waarbij elk de gevraagde uitvoering ten opzichte van één database in de groep uitvoert. 
 
Er zijn twee soorten groepen die u kunt maken: 

* Groep [Shard kaart](sql-database-elastic-scale-shard-map-management.md) : wanneer een taak is verzonden naar een kaart shard afstemmen, de taak op de query de kaart shard om te bepalen de huidige set shards's en maakt vervolgens onderliggende taken voor elke shard in de map shard.
* Aangepaste groep van de siteverzameling: een aangepaste reeks databases gedefinieerd. Wanneer een taak is bedoeld voor een aangepaste verzameling, dat wordt gemaakt onderliggende taken voor elke database momenteel in de aangepaste verzameling.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Taken om in te stellen van de elastische Database verbinding

Een verbinding moet worden ingesteld met de taken *besturingselement database* voordat u de taken API's gebruikt. Deze cmdlet wordt uitgevoerd, wordt een referentie-venster moet worden weergegeven voor het aanvragen van de gebruikersnaam en wachtwoord hebt gemaakt tijdens de installatie van elastische Database taken geactiveerd. Alle voorbeelden die zijn opgegeven in dit onderwerp wordt ervan uitgegaan dat deze eerste stap al is uitgevoerd.

Een verbinding met de taken elastische Database te openen:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Versleutelde referenties binnen de taken elastische Database

Databasereferenties kunnen in de taken in het *besturingselement database* worden ingevoegd met het wachtwoord versleuteld. Het is nodig voor de opslag van referenties om in te schakelen van taken op een later tijdstip wordt uitgevoerd (met behulp van Project-planningen).
 
Versleuteling werkt via een certificaat dat als onderdeel van de installatiescript is gemaakt. Het installatiescript wordt gemaakt en uploadt het certificaat in de Cloud Azure-Service voor decoderen van de opgeslagen versleutelde wachtwoorden. De Cloudservice Azure opgeslagen later de openbare sleutel binnen de taken *besturingselement database* waarmee de PowerShell-API of Azure klassieke Portal interface voor het coderen van een opgegeven wachtwoord zonder het certificaat lokaal zijn geïnstalleerd.
 
De referentie-wachtwoorden zijn versleuteld en beveiligen tegen gebruikers met alleen-lezen toegang aan taken van elastische databaseobjecten. Maar het is een kwaadwillende gebruiker met alleen-lezen-schrijven toegang tot elastische Database taken objecten kunt ophalen van een wachtwoord. Referenties zijn ontworpen om de hele taak executions worden gebruikt. Referenties worden doorgegeven aan de doeldatabases bij het maken van verbindingen. Er zijn momenteel geen beperkingen van de doeldatabases die worden gebruikt voor elke referentie, kwaadwillende gebruiker kan een database-doel voor een database onder beheer van de schadelijke gebruiker toevoegen. De gebruiker kan vervolgens starten voor een taak hebt samengesteld deze database om te krijgen van de referentie-wachtwoord.

Aanbevolen procedures voor beveiliging voor elastische Database taken zijn:

* Beperkingen instellen voor gebruik van de API's vertrouwde personen.
* Referenties moeten de minimaal benodigde bevoegdheden nodig om uit te voeren de projecttaak hebben.  Meer informatie kan worden bekeken in dit artikel van de SQL Server MSDN [autorisatie en machtigingen](https://msdn.microsoft.com/library/bb669084.aspx) .

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Naar een versleutelde referentie voor uitvoering tussen databases maken

Als u wilt een nieuwe versleutelde referentie hebt gemaakt, wordt de [**cmdlet Get-referentie**](https://technet.microsoft.com/library/hh849815.aspx) vraagt om een gebruikersnaam en wachtwoord die kan worden doorgegeven aan de [**cmdlet New-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346063.aspx).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Referenties bijwerken

Wanneer wachtwoorden wijzigt, gebruik de [**cmdlet Set-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346062.aspx) en stelt u de parameter **CredentialName** .

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Een Database elastische shard kaart doel definiëren

Als u wilt een taak op alle databases uitvoeren in een shard set (gemaakt met behulp van [elastische Database clientbibliotheek](sql-database-elastic-database-client-library.md)), een kaart shard als het doel van de database te gebruiken. In dit voorbeeld is een toepassing voor een laptopgeheugen gemaakt met behulp van de bibliotheek van de client elastische Database vereist. Zie [aan de slag met elastische Database extra steekproef](sql-database-elastic-scale-get-started.md).

De shard kaart manager-database moet worden ingesteld als het doel van een database en moet de kaart bepaald shard worden opgegeven als doel.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Een T-SQL-Script voor uitvoering tussen databases maken

Wanneer u T-SQL-scripts voor uitvoering maakt, is het ten zeerste aanbevolen maken van deze als [idempotency is ingeschakeld](https://en.wikipedia.org/wiki/Idempotence) en robuuste tegen fouten. Uitvoering van een script wordt opnieuw door elastische Database taken worden uitgevoerd als een fout, ongeacht de indeling van de fout wordt aangetroffen die kan worden uitgevoerd.

Gebruik de [**cmdlet New-AzureSqlJobContent**](https://msdn.microsoft.com/library/mt346085.aspx) maken en opslaan van een script voor uitvoering en de **- ContentName** en **-CommandText** parameters instellen.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Een nieuw script maken vanuit een bestand
Als de T-SQL-script binnen een bestand is gedefinieerd, gebruikt u dit het script importeren:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Naar een T-SQL-script voor uitvoering op databases worden bijgewerkt  

Deze PowerShell-script bijgewerkt de tekst van de T-SQL-opdracht voor een bestaand script.

Stel de volgende variabelen aan de definitie van de gewenste script worden ingesteld:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>De definitie bijwerken naar een bestaand script

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Een taak als u wilt een script uitvoeren op een kaart shard wilt maken

Een taak voor de uitvoering van een script start deze PowerShell-script over elke shard in een elastische schaal shard toewijzing.

De volgende variabelen aan het gewenste script en de doeltoepassing instellen:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Als een taak wilt uitvoeren 

Deze PowerShell-script uitvoert een bestaande taak:

Update voor de volgende variabele zodat de gewenste taaknaam hebt uitgevoerd:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>De status van een uitvoering één taak ophalen

Gebruik de [**cmdlet Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) en stel de parameter **JobExecutionId** om weer te geven van de status van een taak kan worden uitgevoerd.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Gebruik de dezelfde **Get-AzureSqlJobExecution** -cmdlet met de parameter **IncludeChildren** om de status van de onderliggende taak executions, namelijk de specifieke staat voor de taakuitvoering van elke ten opzichte van elke database door de taak is gericht weergeven.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Naar de stand in meerdere taak executions weergeven

De [**cmdlet Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) heeft meerdere optionele parameters die kunnen worden gebruikt om meerdere uitvoering, gefilterd door de meegeleverde parameters weer te geven. Het volgende voorbeeld toont aantal mogelijke manieren Get-AzureSqlJobExecution gebruiken:

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

## <a name="to-retrieve-failures-within-job-task-executions"></a>Fouten in uitvoering taak ophalen

Het **JobTaskExecution-object** bevat een eigenschap voor de levenscyclus van de taak samen met de berichteigenschap van een. Als een taak taak niet uitvoeren, de levenscyclus van de eigenschap wordt ingesteld op *mislukt* en de berichteigenschap ingesteld op de resulterende uitzonderingsbericht en de stapel. Als een taak is mislukt, is het belangrijk om weer te geven van de details van projecttaken die voor een bepaalde taak is mislukt.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Moet wachten om een taak kan worden uitgevoerd om te voltooien

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
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
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

Elastische Database taken ondersteunt annulering aanvragen van taken.  Als elastische Database taken worden gedetecteerd een annuleringsverzoek voor een taak die momenteel wordt uitgevoerd, wordt geprobeerd de taak beëindigen.

Er zijn twee manieren dat elastische Database taken annulering kunnen uitvoeren:

1. Annuleren taken dat momenteel wordt uitgevoerd: als annulering wordt aangetroffen terwijl er een taak momenteel wordt uitgevoerd, annulering wordt geprobeerd binnen de momenteel uitgevoerde aspecten van de taak.  Bijvoorbeeld: als er een langdurige query momenteel wordt uitgevoerd wanneer een annulering wordt uitgevoerd, wordt er een poging om te annuleren van de query.
2. Taak pogingen om opnieuw te heffen: als annulering wordt gedetecteerd door de control-thread voordat een taak voor uitvoering wordt gestart, de control-thread wordt voorkomen dat het starten van de taak en het verzoek declareren als geannuleerd.

Als een taak is geannuleerd voor een bovenliggende taak wordt aangevraagd, wordt het annuleringsverzoek worden uitgevoerd voor de bovenliggende taak en alle bijbehorende onderliggende taken.
 
Als u wilt een aanvraag annulering indient, gebruik de [**cmdlet Stop-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) en de parameter **JobExecutionId** instellen.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Verwijderen van een taak en geschiedenis asynchroon taak

Elastische Database taken ondersteunt asynchroon verwijdering van taken. Een taak kan worden gemarkeerd om te worden verwijderd en wordt verwijderd de taak en alle bijbehorende werkervaring nadat u alle taak-executions voor de taak hebt voltooid. Het systeem wordt niet automatisch actieve taak executions geannuleerd.  

Roepen [**Stoppen-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) om te annuleren actieve taak executions.

Als u wilt activeren taak verwijdering, gebruik de [**cmdlet verwijderen-AzureSqlJob**](https://msdn.microsoft.com/library/mt346083.aspx) en stel de parameter **Taaknaam** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Om het doel van een aangepaste database te maken

U kunt aangepaste database doelen voor directe uitvoering of voor opname in een groep aangepaste database definiëren. Bijvoorbeeld omdat **elastische Database-toepassingen** worden niet nog rechtstreeks ondersteund met PowerShell APIs, kunt u een aangepaste database doel- en aangepaste database siteverzameling doel waarin alle databases in de groep omvat.

De volgende variabelen aan de gewenste database-informatie instellen:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Om een doel van de siteverzameling aangepaste database te maken

Gebruik de cmdlet [**New-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) definiëren van een aangepaste database siteverzameling doel om in te schakelen worden uitgevoerd in meerdere gedefinieerde database doelen. Na het maken van een groep database kunnen databases worden gekoppeld aan het doel van de aangepaste siteverzameling.

De volgende variabelen aan de gewenste aangepaste verzameling doel-configuratie instellen:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Databases toevoegen aan een aangepaste database siteverzameling doel

Voeg een database aan een specifieke aangepaste verzameling via de cmdlet [**Toevoegen-AzureSqlJobChildTarget**](https://msdn.microsoft.comlibrary/mt346064.aspx) .

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>De databases binnen een doel van de siteverzameling aangepaste database controleren

Gebruik de cmdlet [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) om op te halen van de onderliggende databases binnen een doel van de siteverzameling aangepaste database. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Een taak als u wilt een script uitvoeren via een doel van de siteverzameling aangepaste database maken

Gebruik de cmdlet [**New-AzureSqlJob**](https://msdn.microsoft.com/library/mt346078.aspx) een taak ten opzichte van een groep van databases die zijn gedefinieerd door een doel van de siteverzameling aangepaste database wilt maken. Elastische Database taken wordt de taak in meerdere onderliggende taken elke overeenkomt met een database die is gekoppeld aan het doel van de siteverzameling aangepaste database uitvouwen en zorg ervoor dat het script is uitgevoerd voor elke database. Nogmaals, is het belangrijk dat scripts idempotency is ingeschakeld zijn tegen pogingen om opnieuw te worden.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Gegevens verzamelen over databases

U kunt een taak een query uitvoeren op een groep van databases en de resultaten verzenden naar een bepaalde tabel. De tabel kan worden doorzocht na het feit om de resultaten van de query uit elke database weer te geven. Hier vindt u een asynchrone methode voor het uitvoeren van een query over veel databases. Mislukte pogingen worden automatisch verwerkt via pogingen.

De opgegeven doeltabel worden automatisch gemaakt als dit nog niet bestaat. De nieuwe tabel komt overeen met het schema van de geretourneerde resultaten. Als een script meerdere resultatensets retourneert, worden elastische Database taken alleen de eerste verzenden naar de doeltabel.

Het volgende PowerShell-script een script wordt uitgevoerd en de resultaten verzamelt in een opgegeven tabel. Dit script wordt ervan uitgegaan dat een T-SQL-script is gemaakt waarin een set één resultaat oplevert en dat een doel van de siteverzameling aangepaste database is gemaakt.

Dit script wordt gebruikt voor de cmdlet [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) . De parameters voor script, referenties en execution doel instellen:

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Maken en starten van een taak voor scenario's voor het verzamelen van gegevens

Dit script wordt de [**Begin-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346055.aspx) -cmdlet gebruikt.
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Een taak kan worden uitgevoerd trigger plannen

Het volgende PowerShell-script kan worden gebruikt om een terugkerende schema te maken. Dit script gebruikt een minuut interval, maar [**Nieuw-AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) ondersteunen ook - DayInterval, - HourInterval, - MonthInterval, en -WeekInterval parameters. Schema's die slechts één keer uitvoeren kunnen worden gemaakt door doorgeven - eenmalige.

Een nieuwe planning maken:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Voor het starten van een taak volgens een tijdschema wordt uitgevoerd

Een taak-trigger kan worden gedefinieerd als u wilt dat een taak volgens de planning van een tijd wordt uitgevoerd. Het volgende PowerShell-script kan worden gebruikt voor het maken van een taak-trigger.

[Nieuw-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346069.aspx) gebruiken en instellen van de volgende variabelen zodat ze overeenkomen met de gewenste taak en planning:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Een geplande koppeling om te stoppen taak wordt uitgevoerd op de planning verwijderen

Als u wilt voorkomen dat bepaalde terugkerende taak kan worden uitgevoerd door een taak-trigger, kan de trigger taak worden verwijderd. Een taak-trigger als u wilt stoppen van een taak uit te voeren op basis van een planning met de [**cmdlet verwijderen-AzureSqlJobTrigger**](https://msdn.microsoft.com/library/mt346070.aspx)verwijderen.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Taak triggers die is gebonden aan een tijdschema ophalen

Het volgende PowerShell-script kan worden gebruikt om te verkrijgen en de taak triggers geregistreerd aan de planning van een bepaald tijdstip weer te geven.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Als u wilt ophalen van de taak triggers die is gebonden aan een taak 

Gebruik [Get-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) om te verkrijgen en schema's met een geregistreerde taak weer te geven.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Maken van een toepassing gegevens laag (DACPAC) voor uitvoering voor databases

Zie [gegevens laag toepassingen](https://msdn.microsoft.com/library/ee210546.aspx)maken van een DACPAC. Als u wilt een DACPAC implementeert, gebruikt u de [cmdlet New-AzureSqlJobContent](https://msdn.microsoft.com/library/mt346085.aspx). De DACPAC moet toegankelijk zijn voor de service. Het wordt aanbevolen voor het uploaden van een gemaakte DACPAC met Azure Storage en een [Gedeeld Access handtekening](../storage/storage-dotnet-shared-access-signature-part-1.md) maken voor de DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Naar een toepassing gegevens laag (DACPAC) voor uitvoering op databases worden bijgewerkt

Bestaande DACPACs geregistreerd binnen elastische Database taken kunnen worden bijgewerkt om te laten verwijzen naar de nieuwe URI's. Gebruik de [**cmdlet Set-AzureSqlJobContentDefinition**](https://msdn.microsoft.com/library/mt346074.aspx) om de DACPAC URI op een bestaande geregistreerde DACPAC bijwerken:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Een taak als u wilt toepassen op een toepassing voor de gegevens laag (DACPAC) alle databases maken

Nadat een DACPAC gedurende elastische taken van de Database is gemaakt, kan een taak kan worden gemaakt als u wilt de DACPAC toepassen op een groep van databases. Het volgende PowerShell-script kan worden gebruikt om een taak DACPAC tussen een aangepaste verzameling databases maken:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
