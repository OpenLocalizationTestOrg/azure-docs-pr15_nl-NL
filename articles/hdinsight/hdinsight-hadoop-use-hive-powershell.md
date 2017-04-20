<properties
   pageTitle="Hadoop-component gebruiken met PowerShell in HDInsight | Microsoft Azure"
   description="PowerShell gebruiken om te component query's uitvoeren in Hadoop op HDInsight."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>Component query's via PowerShell uitvoeren

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell in de modus Azure resourcegroep component query's uitvoeren in een Hadoop op HDInsight cluster.

> [AZURE.NOTE] In dit document biedt geen een gedetailleerde beschrijving van wat het HiveQL-instructies die worden gebruikt in de voorbeelden doen. Voor informatie over de HiveQL die in dit voorbeeld wordt gebruikt, Zie [Gebruik component met Hadoop op HDInsight](hdinsight-use-hive.md).


**Vereisten voor**

U voltooit de stappen in dit artikel, moet u het volgende.

- **Een cluster Azure HDInsight (Hadoop op HDInsight) (Windows- of Linux-gebaseerde)**
- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Component query's via Azure PowerShell uitvoeren

Azure PowerShell biedt *cmdlets* waarmee u kunt op afstand component query's uitvoeren op HDInsight. Dit gebeurt intern, met behulp van oproepen van de REST naar [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (voorheen genoemd Templeton) op de cluster HDInsight.

De volgende cmdlets worden gebruikt wanneer component query's op een externe HDInsight cluster uitgevoerd:

* **Toevoegen-AzureRmAccount**: verifieert Azure PowerShell aan uw Azure-abonnement

* **Nieuw-AzureRmHDInsightHiveJobDefinition**: Hiermee maakt u een nieuwe *taakdefinitie* met behulp van de opgegeven HiveQL beweringen

* **Start-AzureRmHDInsightJob**: Hiermee worden de taakdefinitie met HDInsight, de taak begint en retourneert een *taak* die kunnen worden gebruikt om de status van de taak controleren

* **Wachten-AzureRmHDInsightJob**: Controleer de status van de taak wordt de taakobject. Deze wacht totdat de taak is voltooid of de wachttijd is overschreden.

* **Get-AzureRmHDInsightJobOutput**: gebruikt voor het ophalen van de uitvoer van de taak

* **Roep AzureRmHDInsightHiveJob**: gebruikt voor het uitvoeren van HiveQL-instructies. Hiermee wordt de query blokkeren is voltooid en geeft vervolgens de resultaten

* **Gebruik AzureRmHDInsightCluster**: Hiermee stelt u de huidige cluster wilt gebruiken voor de opdracht **Roep-AzureRmHDInsightHiveJob**

De volgende stappen uit te zien hoe het gebruik van deze cmdlets voor het uitvoeren van een taak in uw cluster HDInsight:

1. Met een editor, de volgende code opslaan als **hivejob.ps1**. U moet **CLUSTERNAAM** vervangen door de naam van uw cluster HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Open een nieuwe **Azure PowerShell** -prompt. Mappen naar de locatie van het bestand **hivejob.ps1** wijzigen en het gebruik van de volgende opdracht uit om het script uitvoeren:

        .\hivejob.ps1

    Wanneer het script wordt uitgevoerd, wordt u gevraagd om in te voeren van het account HTTPS/beheerdersreferenties voor uw cluster. U kunt ook gevraagd aanmelden bij uw Azure-abonnement.
    
7. Als de taak is voltooid, moeten worden geretourneerd informatie ongeveer als volgt uit:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Zoals eerder is vermeld, kan **Roep-component** worden gebruikt voor een query uitvoeren en wacht totdat het antwoord. Gebruik van de volgende opdrachten en **CLUSTERNAAM** vervangen door de naam van uw cluster:

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    De uitvoer ziet er als volgt uit:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] Voor langere HiveQL-query's, kunt u het **Hier-tekenreeksen** met de Azure PowerShell-cmdlet of de HiveQL scriptbestanden. Het codefragment van de volgende ziet hoe u gebruik de cmdlet **Roep-component** om het uitvoeren van een HiveQL script-bestand. De HiveQL script-bestand moet worden geüpload naar wasbs: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > Zie voor meer informatie over **Hier-tekenreeksen** <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Gebruik Windows PowerShell hier-tekenreeksen</a>.

##<a name="troubleshooting"></a>Problemen oplossen

Als er worden geen gegevens wordt geretourneerd als de taak is voltooid, mogelijk een fout opgetreden tijdens het verwerken. Als u wilt bekijken foutgegevens voor deze taak, het volgende toevoegen aan het einde van het bestand **hivejob.ps1** , opslaan en vervolgens opnieuw uit te voeren.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Dit geeft als resultaat de informatie die naar STDERR op de server is geschreven wanneer u de taak hebt uitgevoerd en deze kan helpen bepalen waarom de taak is verbroken.

##<a name="summary"></a>Overzicht

Zoals u ziet, u Azure PowerShell een eenvoudige manier component query's uitvoeren in een cluster HDInsight, het controleren van de taakstatus en het ophalen van de uitvoer.

##<a name="next-steps"></a>Volgende stappen

Voor algemene informatie over component in HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
