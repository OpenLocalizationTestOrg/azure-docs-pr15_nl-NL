<properties
   pageTitle="MapReduce en SSH verbinding met het Hadoop in HDInsight | Microsoft Azure"
   description="Informatie over het gebruik van SSH MapReduce taken met Hadoop op HDInsight uitvoeren."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>MapReduce gebruiken met Hadoop op HDInsight via SSH

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In dit artikel leert u hoe u verbinding maken met een Hadoop op HDInsight cluster en vervolgens dient MapReduce taken met behulp van Hadoop-opdrachten met Secure Shell (SSH).

> [AZURE.NOTE] Als u al vertrouwd te raken met Linux gebaseerde Hadoop-servers bent, maar u hebt geen ervaring met HDInsight, raadpleegt u [Linux gebaseerde HDInsight tips](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

* Een cluster Linux gebaseerde HDInsight (Hadoop op HDInsight)

* Een SSH-client. Linux, Unix en Mac-besturingssystemen moeten worden geleverd met een SSH-client. Windows-gebruikers, moeten een mailclient gebruikt, zoals [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)downloaden.

##<a id="ssh"></a>Verbinding maken met SSH

Verbinding maken met de FQDN-naam (Fully Qualified Domain Name) van uw cluster HDInsight via de opdracht SSH. De FQDN-naam moet de naam die u hebt opgegeven het cluster, gevolgd door **. azurehdinsight.net**. Bijvoorbeeld de volgende manier verbinding maken met een cluster met de naam **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Als u de sleutel van een certificaat voor SSH verificatie opgegeven** wanneer u het cluster HDInsight hebt gemaakt, mogelijk moet u de locatie van de persoonlijke sleutel opgeven op uw clientsysteem, bijvoorbeeld:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Als u een wachtwoord voor de verificatie SSH opgegeven** wanneer u het cluster HDInsight hebt gemaakt, moet u het wachtwoord wanneer u wordt gevraagd te geven.

Zie voor meer informatie over het gebruik van SSH met HDInsight, [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, OS X, en Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>Stopverf (Windows-clients)

Windows beschikt niet over een ingebouwde SSH-client. We raden u aan **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight van Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hadoop"></a>Hadoop-opdrachten gebruiken

1. Nadat u met het cluster HDInsight verbonden bent, gebruikt u de volgende opdracht uit **Hadoop** om een taak MapReduce te starten:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Hiermee wordt de klasse **wordcount** , die deel van het bestand **hadoop-mapreduce-examples.jar uitmaakt** gestart. Als invoer, wordt het document **wasbs://example/data/gutenberg/davinci.txt** gebruikt en uitvoer is opgeslagen op **wasbs: / / / voorbeeld/gegevens/WordCountOutput**.

    > [AZURE.NOTE] Zie voor meer informatie over deze taak MapReduce en de voorbeeldgegevens [Gebruik MapReduce in Hadoop op HDInsight](hdinsight-use-mapreduce.md).

2. De taak genereert details, zoals wordt verwerkt, en retourneert informatie ongeveer als volgt uit wanneer de taak is voltooid:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Wanneer de taak is voltooid, gebruikt u de volgende opdracht uit voor een overzicht van de uitvoerbestanden die zijn opgeslagen op **wasbs://example/data/WordCountOutput**:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    Dit moet twee bestanden, **_SUCCESS** en **deel-r-00000**worden weergegeven. Het **onderdeel-r-00000** -bestand bevat de uitvoer voor deze taak.

    > [AZURE.NOTE] Sommige taken MapReduce mogelijk de resultaten splitsen in meerdere **deel-r-###** -bestanden. Als dat het geval is, gebruikt u de ### achtervoegsel om aan te geven van de volgorde van de bestanden.

4. Weergave van de uitvoer, gebruikt u de volgende opdracht:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Er verschijnt een lijst van de woorden die zijn opgenomen in het bestand **wasbs://example/data/gutenberg/davinci.txt** en het aantal keren die elk woord is opgetreden. Hier volgt een voorbeeld van de gegevens die, worden opgeslagen in het bestand:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Overzicht

Zoals u ziet, bieden Hadoop-opdrachten een eenvoudige manier om MapReduce taken uitvoeren in een cluster HDInsight en vervolgens de taak-uitvoer te bekijken.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce taken in HDInsight:

* [MapReduce op HDInsight Hadoop gebruiken](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)
