<properties
   pageTitle="Gebruik Hadoop varken met PowerShell in HDInsight | Microsoft Azure"
   description="Leer hoe u taken varken met een Hadoop-cluster op via PowerShell Azure HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>Varken taken via PowerShell uitvoeren

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell varken taken met een Hadoop op HDInsight cluster. Varken kunt u MapReduce taken schrijven met behulp van een taal (varken Latijns), die gegevenstransformaties modellen, in plaats van toewijzen en functies te verkleinen.

> [AZURE.NOTE] In dit document biedt geen een gedetailleerde beschrijving van wat het varken Latijns-instructies in de voorbeelden gebruikt doen. Zie voor informatie over de varken Latijnse in dit voorbeeld gebruikt, [Gebruik varken met Hadoop op HDInsight](hdinsight-use-pig.md).

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende.

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>Varken taken via PowerShell uitvoeren

Azure PowerShell biedt *cmdlets* waarmee u kunt op afstand uitvoeren varken taken op HDInsight. Dit gebeurt intern, met behulp van oproepen van de REST naar [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (voorheen genoemd Templeton) op de cluster HDInsight.

De volgende cmdlets worden gebruikt wanneer u zich varken taken in een externe HDInsight cluster:

* **Login-AzureRmAccount**: verifieert Azure PowerShell aan uw Azure-abonnement

* **Nieuw-AzureRmHDInsightPigJobDefinition**: Hiermee maakt u een nieuwe *taakdefinitie* met behulp van de opgegeven varken Latijns-instructies

* **Start-AzureRmHDInsightJob**: Hiermee worden de taakdefinitie met HDInsight, de taak begint en retourneert een *taak* die kunnen worden gebruikt om de status van de taak controleren

* **Wachten-AzureRmHDInsightJob**: Controleer de status van de taak wordt de taakobject. Deze wacht totdat de taak is voltooid, of de wachttijd is overschreden.

* **Get-AzureRmHDInsightJobOutput**: gebruikt voor het ophalen van de uitvoer van de taak

De volgende stappen demonstreren het gebruik van deze cmdlets voor het uitvoeren van een taak op uw cluster HDInsight.

1. Met een editor, de volgende code opslaan als **pigjob.ps1**. U moet **CLUSTERNAAM** vervangen door de naam van uw cluster HDInsight.

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Open een nieuwe Windows PowerShell-prompt. Mappen naar de locatie van het bestand **pigjob.ps1** wijzigen en het gebruik van de volgende opdracht uit om het script uitvoeren:

        .\pigjob.ps1
        
    U wordt eerst aanmelden bij uw Azure abonnement gevraagd. Klik u krijgt het verzoek voor de naam HTTPs/beheerder en het wachtwoord voor het cluster HDInsight.

7. Als de taak is voltooid, moeten worden geretourneerd informatie ongeveer als volgt uit:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>Problemen oplossen

Als er worden geen gegevens wordt geretourneerd als de taak is voltooid, mogelijk een fout opgetreden tijdens het verwerken. Als u wilt bekijken foutgegevens voor deze taak, de volgende opdracht toevoegen aan het einde van het **pigjob.ps1** -bestand, opslaan en vervolgens opnieuw uit te voeren.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Hiermee herstelt u de informatie die is geschreven naar STDERR op de server wanneer u de taak hebt uitgevoerd en deze kan helpen bepalen waarom de taak is verbroken.

##<a id="summary"></a>Overzicht

Zoals u ziet, u Azure PowerShell een eenvoudige manier varken taken uitvoeren op een cluster HDInsight, het controleren van de taakstatus en het ophalen van de uitvoer.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over varken in HDInsight:

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
