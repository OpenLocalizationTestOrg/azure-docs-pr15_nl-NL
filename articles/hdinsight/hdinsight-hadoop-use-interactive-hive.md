<properties
    pageTitle="Gebruik interactieve component in HDInsight | Microsoft Azure"
    description="Leren werken met interactieve component (component op LLAP) in HDInsight."
    keywords=""
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Interactieve component in HDInsight (Preview) gebruiken

Interactieve (ook component [Live lang en proces]( https://cwiki.apache.org/confluence/display/Hive/LLAP)) is een nieuwe HDInsight [clustertype]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interactieve component kunt in het cachegeheugen die component query's veel meer interactieve en sneller kunt. Deze nieuwe functie kunt HDInsight van's werelds meeste zodat, flexibele, en open Big Data-oplossing op de cloud met in het geheugen in de cache opgeslagen (via component en een) en geavanceerde analyses via naadloze integratie met R-Services. 

Het cluster interactieve component verschilt van het cluster Hadoop. Deze bevat alleen de component-service. 

> [AZURE.NOTE] MapReduce, varken Sqoop, Oozie en andere services worden, snel van dit clustertype verwijderd.
De component-service in de interactieve component cluster is alleen toegankelijk via de weergave Ambari component, Beeline en ODBC-component. Het is niet toegankelijk via component console, Templeton Azure CLI en Azure PowerShell. 


 


## <a name="create-an-interactive-hive-cluster"></a>Een interactieve component cluster maken

Interactieve component cluster wordt alleen ondersteund op Linux gebaseerde clusters. Zie voor informatie over het maken van HDInsight clusters [maken Linux gebaseerde Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Component van interactieve component uitvoeren

Er zijn verschillende opties hoe kunt u component query's uitvoeren:

- Met de weergave van Ambari component component uitvoeren

    Zie [de weergave component met Hadoop in HDInsight gebruiken]( hdinsight-hadoop-use-hive-ambari-view.md)voor de informatie over het gebruik van de weergave component.

- Component met Beeline uitvoeren

    Zie [Gebruik component met Hadoop in HDInsight met Beeline](hdinsight-hadoop-use-hive-beeline.md)voor de informatie over het gebruik van Beeline op HDInsight.

    U Beeline uit de headnode of een lege randknooppunt gebruiken.  Met Beeline vanaf een lege randknooppunt wordt aanbevolen.  Zie voor informatie over het maken van een HDInsight cluster met een lege edgenode [Gebruik lege rand knooppunten in HDInsight](hdinsight-apps-use-edge-node.md).

- Component met ODBC-component worden uitgevoerd

    Zie [Excel verbinden met een Hadoop met de component ODBC-stuurprogramma](hdinsight-connect-excel-hive-odbc-driver.md)voor de informatie over het gebruik van ODBC-component.

**De verbindingsreeks JDBC zoeken:**

1.  Aanmelden bij Ambari via de volgende URL: https://<ClusterName>. AzureHDInsight.net.
2.  Klik in het linkermenu op **component** .
3.  Klik op het gemarkeerde pictogram om de URL kopiëren:

    ![HDInsight Hadoop interactieve component LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Zie ook
-   [Hadoop maken Linux gebaseerde clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): informatie over het maken van interactieve component clusters in HDInsight.
-   [Gebruik component met Hadoop in HDInsight via Beeline](hdinsight-hadoop-use-hive-beeline.md): informatie over het gebruik van Beeline om in te dienen component query's.
-   [Excel verbinden met een Hadoop met de component ODBC-stuurprogramma](hdinsight-connect-excel-hive-odbc-driver.md): Leer hoe u Excel verbinden met component.
