<properties
    pageTitle="Fout: onvoldoende geheugen (OOM) - component instellingen | Microsoft Azure"
    description="Verhelp een fout onvoldoende geheugen (OOM) uit een query component in Hadoop in HDInsight. De klant-scenario is een query meerdere veel grote tabellen."
    keywords="Afmelden bij de instellingen van geheugen fout, OOM, component"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>Een afwezigheidsbericht geheugen (OOM)-fout op te lossen met component geheugeninstellingen in Hadoop in Azure HDInsight

Een van de algemene problemen onze klanten nominale is treedt er een afwezigheidsbericht geheugen (OOM) fout bij het gebruik van component. In dit artikel worden de scenario van een klant en de component-instellingen die het is raadzaam om het probleem te verhelpen.

## <a name="scenario-hive-query-across-large-tables"></a>Scenario: Component query over grote tabellen

Een klant uitvoert de query hieronder component.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Sommige nuances bevatten van deze query:

* T 1 is een alias aan een grote tabel, Tabel1, waarvoor een groot aantal kolomtypen tekenreeks.
* Andere tabellen zijn niet die groot, maar wel een groot aantal kolommen.
* Alle tabellen zijn deelnemen aan elkaar op in sommige gevallen met meerdere kolommen in TABLE1 en anderen.

Wanneer de query met component op MapReduce op een 24 knooppunt A3 cluster wordt uitgevoerd door de klant, wordt de query uitgevoerd in zit 26 minuten. De klant zoals gezien van de volgende waarschuwingen weergegeven wanneer de query is uitgevoerd met component op MapReduce:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Omdat de query uitgevoerd in zit 26 minuten, de klant deze waarschuwingen genegeerd en in plaats daarvan de slag kunt richten op het verbeteren van deze verder van query-prestaties.

De klant geraadpleegd [optimaliseren component query's voor Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md)en besloten te Tez execution engine gebruiken. Als u dezelfde query is uitgevoerd met de instelling Tez is ingeschakeld de query voor 15 minuten hebt uitgevoerd en vervolgens heeft de volgende fout:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

De klant vervolgens besloten te gebruiken een groter VM (dat wil zeggen D12) een groter VM denkt meer opslagruimte ruimte zou hebben. Zelfs dan bleef de klant om de fout weer te geven. De klant bereikt af aan het team HDInsight voor hulp bij dit probleem foutopsporing.

## <a name="debug-the-out-of-memory-oom-error"></a>Fouten opsporen in de fout niet geheugen (OOM)

Onze ondersteuning en technische teams gevonden samen op een van de problemen veroorzaken de fout niet geheugen (OOM) is een [bekend probleem dat wordt beschreven in de JIRA Apache](https://issues.apache.org/jira/browse/HIVE-8306). Van de beschrijving in het JIRA:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

We bevestigd dat **hive.auto.convert.join.noconditionaltask** daadwerkelijk is ingesteld op **true** herkent onder Component site.xml bestand:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

Op basis van de waarschuwing en de JIRA, is onze hypothese dat kaart deelnemen aan de oorzaak van de fout Java opslagruimte ruimte OOM is. Zodat we grondigere naar dit probleem spitte.

Aan de container Tez daadwerkelijk wanneer Tez execution-engine is gebruikt voor de opslagruimte hoeveel ruimte wordt gebruikt zoals in het blogbericht [Hadoop garens geheugeninstellingen in HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx). Zie de afbeelding onder met een beschrijving van het geheugen van de container Tez.

![Tez container geheugen diagram: onvoldoende geheugen beschikbaar OOM component](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


Het blogbericht geeft al aan de volgende twee geheugeninstellingen het geheugen container voor de opslagruimte definiëren: **hive.tez.container.size** en **hive.tez.java.opts**. Uit ervaring betekent de uitzondering OOM niet dat de container is te klein. Het betekent dit dat de Java opslagruimte (hive.tez.java.opts) is te klein. Zodat wanneer u OOM ziet, u proberen kunt om uit te breiden **hive.tez.java.opts**. Indien nodig moet u mogelijk **hive.tez.container.size**vergroten. De instelling **java.opts** moet ongeveer 80% van **container.size**.

> [AZURE.NOTE]  De instelling **hive.tez.java.opts** moet altijd groter zijn dan **hive.tez.container.size**.

Aangezien een machine D12 met 28GB geheugen vereist, besloten we een container grootte van 10GB (10240MB) en wijst u 80% naar java.opts. Dit is gedaan op de component-console met de instelling hieronder:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Op basis van deze instellingen, de query is uitgevoerd in onder tien minuten.

## <a name="conclusion-oom-errors-and-container-size"></a>Sluiten: De OOM fouten en de grootte van de container

Treedt er een fout OOM betekent niet dat u dat de grootte van de container te klein is. U moet in plaats daarvan de geheugeninstellingen configureren zodat het formaat van de opslagruimte opgehoogd en ten minste 80% is van de container geheugengrootte.
