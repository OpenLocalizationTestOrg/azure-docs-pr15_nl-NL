<properties
   pageTitle="Gebruik MapReduce en PowerShell met Hadoop | Microsoft Azure"
   description="Leer hoe u PowerShell gebruiken om te MapReduce taken op afstand uitvoeren met Hadoop op HDInsight."
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
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>MapReduce taken uitvoeren met Hadoop op HDInsight via PowerShell

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Dit document bevat een voorbeeld van Azure PowerShell gebruiken voor het uitvoeren van een taak MapReduce in een Hadoop op HDInsight cluster.

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

- **Een cluster Azure HDInsight (Hadoop op HDInsight) (Windows- of Linux-gebaseerde)**

- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a id="powershell"></a>Een MapReduce taak via Azure PowerShell uitvoeren

Azure PowerShell biedt *cmdlets* waarmee u kunt op afstand uitvoeren MapReduce taken op HDInsight. Dit gebeurt intern, met behulp van oproepen van de REST naar [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (voorheen genoemd Templeton) op de cluster HDInsight.

De volgende cmdlets worden gebruikt wanneer MapReduce taken op een externe HDInsight cluster uitgevoerd.

* **Login-AzureRmAccount**: verifieert Azure PowerShell aan uw Azure-abonnement

* **Nieuw-AzureRmHDInsightMapReduceJobDefinition**: Hiermee maakt u een nieuwe *taakdefinitie* met behulp van de opgegeven MapReduce informatie

* **Start-AzureRmHDInsightJob**: Hiermee worden de taakdefinitie met HDInsight, de taak begint en retourneert een *taak* die kunnen worden gebruikt om de status van de taak controleren

* **Wachten-AzureRmHDInsightJob**: Controleer de status van de taak wordt de taakobject. Wacht totdat de taak is voltooid of de wachttijd is overschreden.

* **Get-AzureRmHDInsightJobOutput**: gebruikt voor het ophalen van de uitvoer van de taak

De volgende stappen demonstreren het gebruik van deze cmdlets voor het uitvoeren van een taak in uw cluster HDInsight.

1. Met een editor, de volgende code opslaan als **mapreducejob.ps1**. U moet **CLUSTERNAAM** vervangen door de naam van uw cluster HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Open een nieuwe **Azure PowerShell** -prompt. Mappen naar de locatie van het bestand **mapreducejob.ps1** wijzigen en het gebruik van de volgende opdracht uit om het script uitvoeren:

        .\mapreducejob.ps1
    
    Wanneer u het script uitvoert, wordt u mogelijk gevraagd om te verifiëren bij uw Azure-abonnement. Ook wordt u gevraagd te bieden de accountnaam HTTPS/beheerder en het wachtwoord voor het cluster HDInsight.

3. Als de taak is voltooid, moet u ontvangt uitvoer ongeveer als volgt uit:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Deze uitvoer wordt aangegeven dat de taak is voltooid.

    > [AZURE.NOTE] Als de **ExitCode** een waarde dan 0 is, raadpleegt u [problemen oplossen](#troubleshooting).

    In dit voorbeeld wordt de gedownloade bestanden naar een bestand **uitvoer.txt** ook opgeslagen in de map die u het script uit uitvoert.

###<a name="view-output"></a>Weergave-uitvoer

Open het bestand **uitvoer.txt** in een teksteditor om woorden en telt geproduceerd door de taak weer te geven.

> [AZURE.NOTE] De uitvoerbestanden die van een taak MapReduce zijn onveranderlijke. Dus als u dit voorbeeld opnieuw uitvoeren, moet u de naam van het uitvoerbestand wijzigen.

##<a id="troubleshooting"></a>Problemen oplossen

Als er worden geen gegevens wordt geretourneerd als de taak is voltooid, mogelijk een fout opgetreden tijdens het verwerken. Als u wilt bekijken foutgegevens voor deze taak, de volgende opdracht toevoegen aan het einde van het **mapreducejob.ps1** -bestand, opslaan en vervolgens opnieuw uit te voeren.

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Dit geeft als resultaat de informatie die is geschreven naar STDERR op de server wanneer u de taak hebt uitgevoerd en deze kan helpen bepalen waarom de taak is verbroken.

##<a id="summary"></a>Overzicht

Zoals u ziet, u Azure PowerShell een eenvoudige manier MapReduce taken uitvoeren op een cluster HDInsight, het controleren van de taakstatus en het ophalen van de uitvoer.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce taken in HDInsight:

* [MapReduce op HDInsight Hadoop gebruiken](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)
