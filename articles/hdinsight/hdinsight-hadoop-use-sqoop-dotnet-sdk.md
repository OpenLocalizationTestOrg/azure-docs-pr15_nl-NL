<properties
    pageTitle="Hadoop Sqoop gebruiken in HDInsight | Microsoft Azure"
    description="Informatie over het gebruik van HDInsight .NET SDK uitvoeren Sqoop importeren en exporteren tussen een Hadoop-cluster en een Azure SQL-database."
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
   ms.date="09/14/2016"
    ms.author="jgao"/>

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Met .NET SDK voor Hadoop in HDInsight Sqoop-taken uitvoeren

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van HDInsight .NET SDK Sqoop taken uitvoeren in HDInsight importeren en exporteren tussen HDInsight cluster en Azure SQL-database of SQL Server-database.

> [AZURE.NOTE] De stappen in dit artikel kunnen worden gebruikt met een van beide een Windows- of Linux gebaseerde HDInsight cluster. deze stappen echter werken alleen vanuit een Windows-client. Gebruik de tabkiezer boven aan dit artikel om te kiezen andere methoden.

###<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **A Hadoop cluster in HDInsight**. Zie [cluster maken en SQL-database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Met .NET SDK Sqoop uitvoeren

De HDInsight .NET SDK biedt .NET-client-bibliotheken, waardoor het eenvoudiger om te werken met HDInsight clusters van .NET. In dit gedeelte maakt u een C#-console-toepassing de hivesampletable exporteren naar de SQL-Database-tabel die u eerder in deze zelfstudie hebt gemaakt.

**Een taak Sqoop indienen**

1. Maak een C#-console-toepassing in Visual Studio.
2. Voer de volgende Nuget-opdracht om te importeren van het pakket van de Visual Studio Package Manager-Console.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. Gebruik de volgende code in het bestand Program.cs:

        using System.Collections.Generic;
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
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Druk op **F5** om het programma te starten. 

##<a name="limitations"></a>Beperkingen

* Bulksgewijs exporteren - HDInsight met Linux gebaseerde, de Sqoop verbindingslijn die wordt gebruikt om gegevens te exporteren naar Microsoft SQL Server of Azure SQL-Database ondersteunt momenteel niet bulksgewijs invoegen.

* Batchen - met Linux gebaseerde HDInsight, bij gebruik van de `-batch` overschakelt bij het uitvoeren van wordt ingevoegd, Sqoop meerdere wordt ingevoegd in plaats van de invoegbewerkingen batchen wordt uitgevoerd.

##<a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe Sqoop gebruiken. Meer informatie raadpleegt u:

- [Gebruik Oozie met HDInsight](hdinsight-use-oozie.md): gebruik Sqoop actie in een werkstroom Oozie.
- [Analyseren flight gegevens over vertragingen met HDInsight](hdinsight-analyze-flight-delay-data.md): component gebruiken om te analyseren flight uitstellen gegevens en gebruikt u Sqoop gegevens exporteren naar een Azure SQL-database.
- [Gegevens met HDInsight uploaden](hdinsight-upload-data.md): zoeken naar andere methoden voor het uploaden van gegevens naar HDInsight/Azure-blobopslag.


