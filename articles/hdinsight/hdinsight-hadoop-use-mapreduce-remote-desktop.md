<properties
   pageTitle="MapReduce en extern bureaublad met Hadoop in HDInsight | Microsoft Azure"
   description="Informatie over het gebruik van extern bureaublad verbinding maken met Hadoop op HDInsight en MapReduce taken uitvoeren."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>MapReduce in Hadoop op HDInsight met extern bureaublad gebruiken

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In dit artikel leert u hoe u verbinding maken met een Hadoop op HDInsight cluster met behulp van extern bureaublad en vervolgens MapReduce taken uitvoeren met de opdracht Hadoop.

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

* Een cluster op basis van Windows HDInsight (Hadoop op HDInsight)

* Een clientcomputer met Windows 10, Windows 8 of Windows 7

##<a id="connect"></a>Verbinding met extern bureaublad

Extern bureaublad voor het cluster HDInsight inschakelen en klik verbinden volgens de instructies in [verbinding maken met HDInsight clusters RDP gebruiken](hdinsight-administer-use-management-portal.md#rdp).

##<a id="hadoop"></a>Gebruik de opdracht Hadoop

Wanneer u met het bureaublad voor het cluster HDInsight verbonden bent, gebruikt u de volgende stappen aan een taak MapReduce uitvoeren met de opdracht Hadoop:

1. Start de **opdrachtregel Hadoop**vanaf het bureaublad HDInsight. Hiermee opent u een nieuwe opdrachtprompt in de **c:\apps\dist\hadoop-&lt;versienummer >** directory.

    > [AZURE.NOTE] Het versienummer verandert als Hadoop wordt bijgewerkt. De omgevingsvariabele **HADOOP_HOME** kan worden gebruikt om het pad te vinden. Bijvoorbeeld `cd %HADOOP_HOME%` adreslijsten wordt gewijzigd in de directory Hadoop, zonder dat u hoeft te weten het versienummer weergegeven.

2. Gebruik de volgende opdracht uit om met de opdracht **Hadoop** een voorbeeld MapReduce taak uit te voeren:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Hiermee start u de klasse **wordcount** , die deel van het bestand **hadoop-mapreduce-examples.jar** in de huidige map uitmaakt. Als invoer, wordt het document **wasbs://example/data/gutenberg/davinci.txt** gebruikt en uitvoer is opgeslagen op: **wasbs: / / / voorbeeld/gegevens/WordCountOutput**.

    > [AZURE.NOTE] Zie voor meer informatie over deze taak MapReduce en de voorbeeldgegevens <a href="hdinsight-use-mapreduce.md">Gebruik MapReduce in HDInsight Hadoop</a>.

2. De taak genereert details, zoals deze wordt verwerkt, en retourneert informatie ongeveer als volgt uit wanneer de taak voltooid is:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Wanneer de taak voltooid is, gebruikt u de volgende opdracht uit voor een overzicht van de uitvoerbestanden die zijn opgeslagen op **wasbs://example/data/WordCountOutput**:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    Dit moet twee bestanden, **_SUCCESS** en **deel-r-00000**worden weergegeven. Het **onderdeel-r-00000** -bestand bevat de uitvoer voor deze taak.

    > [AZURE.NOTE] Sommige taken MapReduce mogelijk de resultaten splitsen in meerdere **deel-r-###** -bestanden. Als dat het geval is, gebruikt u de ### achtervoegsel om aan te geven van de volgorde van de bestanden.

4. Weergave van de uitvoer, gebruikt u de volgende opdracht:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Er verschijnt een lijst van de woorden die zijn opgenomen in het bestand **wasbs://example/data/gutenberg/davinci.txt** , samen met het aantal keren die elk woord is opgetreden. Hier volgt een voorbeeld van de gegevens die, worden opgeslagen in het bestand:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Overzicht

Zoals u ziet, u de opdracht Hadoop een eenvoudige manier om te MapReduce taken uitvoeren op een cluster HDInsight en vervolgens de taak-uitvoer te bekijken.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce taken in HDInsight:

* [MapReduce op HDInsight Hadoop gebruiken](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)
