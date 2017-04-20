<properties
    pageTitle="MapReduce taken met HDInsight .NET SDK | Microsoft Azure"
    description="Leer hoe u MapReduce taken met Azure HDInsight Hadoop HDInsight .NET SDK gebruiken."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
   ms.date="10/28/2016"
    ms.author="jgao"/>

# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>MapReduce taken met HDInsight .NET SDK uitvoeren

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Leer hoe u MapReduce taken met HDInsight .NET SDK indienen. HDInsight clusters worden geleverd met een oppervlak-bestand met enkele voorbeelden MapReduce. Het oppervlak-bestand is */example/jars/hadoop-mapreduce-examples.jar*.  Een van de voorbeelden is *wordcount*. U een C#-console-toepassing om in te dienen van een taak wordcount ontwikkelen.  De taak leest het bestand */example/data/gutenberg/davinci.txt* en Hiermee kunt u de resultaten naar */example/data/davinciwordcount*.  Als u de toepassing opnieuw uit te voeren wilt, moet u de uitvoermap opschonen.

> [AZURE.NOTE] De stappen in dit artikel moeten worden uitgevoerd vanuit een Windows-client. Gebruik de tabkiezer weergegeven boven aan het artikel voor informatie over het gebruik van een Linux, OS X of Unix-client voor gebruik met component.

##<a name="prerequisites"></a>Vereisten voor

Voordat u in dit artikel begint, hebt u het volgende:

- **A Hadoop cluster in HDInsight**. Zie [aan de slag met Linux gebaseerde Hadoop in HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012-2013-2015**.

##<a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>MapReduce taken met HDInsight .NET SDK indienen

De HDInsight .NET SDK biedt .NET-client-bibliotheken, waardoor het eenvoudiger om te werken met HDInsight clusters van .NET. 

**Taken**

1. Maak een C#-console-toepassing in Visual Studio.
2. Voer de volgende opdracht uit vanaf de beheerconsole Nuget Package Manager.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. Gebruik de volgende code:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitMRJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };

                    var paras = new MapReduceJobSubmissionParameters
                    {
                        //Content = "wordcount  ",
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the MR job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Druk op **F5** om de toepassing te starten.


## <a name="next-steps"></a>Volgende stappen

U kunt op verschillende manieren om te maken van een cluster HDInsight hebt geleerd in dit artikel. Meer informatie raadpleegt u de volgende artikelen:

- Voor het maken van een cluster en een component uptaak, Zie [aan de slag met Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
- Zie voor het maken van HDInsight clusters, [Hadoop maken Linux gebaseerde clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
- Zie voor het beheren van HDInsight clusters [beheren Hadoop clusters in HDInsight](hdinsight-administer-use-management-portal.md).
- Zie voor de HDInsight .NET SDK leren, [HDInsight .NET SDK verwijzing](https://msdn.microsoft.com/library/mt271028.aspx).
- Voor niet-interactieve verifiëren bij Azure, raadpleegt u [niet-interactieve verificatie .NET HDInsight toepassingen maken](hdinsight-create-non-interactive-authentication-dotnet-applications.md).




