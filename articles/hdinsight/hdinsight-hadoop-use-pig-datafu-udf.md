<properties
pageTitle="DataFu gebruiken met varken op HDInsight"
description="DataFu is een verzameling bibliotheken voor gebruik met Hadoop. Leer hoe u DataFu met varken kunt gebruiken op uw cluster HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>DataFu gebruiken met varken op HDInsight

DataFu is een verzameling bron openen bibliotheken voor gebruik met Hadoop. In dit document leert u hoe u DataFu gebruiken op uw cluster HDInsight en hoe u met varken DataFu gebruiker gedefinieerde functies (UDF).

##<a name="prerequisites"></a>Vereisten voor

* Een Azure-abonnement.

* Een cluster Azure HDInsight (Linux of Windows op basis)

* Een eenvoudige bekend zijn bij het [gebruik van varken op HDInsight](hdinsight-use-pig.md)

##<a name="install-datafu-on-linux-based-hdinsight"></a>DataFu op Linux gebaseerde HDInsight installeren

> [AZURE.NOTE] DataFu is geïnstalleerd op Linux gebaseerde clusters versie 3,3 en hoger en klik op Windows gebaseerde clusters. Dit is niet geïnstalleerd op Linux gebaseerde clusters eerder dan 3.3.
>
> Als u een versie Linux gebaseerde cluster 3,3 of hoger, of een cluster op basis van Windows gebruikt, kunt u dit gedeelte overslaan.

DataFu kunnen worden gedownload en geïnstalleerd uit de bibliotheek Maven. Gebruik de volgende stappen DataFu toevoegen aan uw cluster HDInsight:

1. Verbinding maken met uw HDInsight Linux gebaseerde cluster via SSH. Zie een van de volgende documenten voor meer informatie over het gebruik van SSH met HDInsight:

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. Gebruik van de volgende opdracht uit om de DataFu oppervlak-bestand met het hulpprogramma wget te downloaden of kopieer en plak de koppeling in uw browser om de download te starten.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Upload het bestand vervolgens naar de standaard-opslag voor uw cluster HDInsight. Dit zorgt ervoor dat het bestand beschikbaar voor alle knooppunten in het cluster en het bestand wordt in opslagruimte blijven, zelfs als u verwijderen en opnieuw maken van het cluster.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] In het bovenstaande voorbeeld wordt het oppervlak in `wasbs:///example/jars` omdat deze map al in de clusteropslag bestaat. U kunt een locatie die u wilt dat op HDInsight clusteropslag kunt gebruiken.

##<a name="use-datafu-with-pig"></a>DataFu gebruiken met varken

De stappen in deze sectie wordt ervan uitgegaan dat u bekend bent met het gebruik van varken op HDInsight en geef alleen de beweringen varken Latijns, niet de stapsgewijze instructies voor het gebruiken met het cluster. Zie voor meer informatie over het gebruik van varken met HDInsight [Gebruik varken met HDInsight](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] Wanneer u een DataFu van varken gebruikt op een cluster Linux gebaseerde HDInsight, moet u eerst het oppervlak-bestand met de volgende varken Latijns-instructie registreren:
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> DataFu is geregistreerd al dan niet standaard op Windows gebaseerde HDInsight clusters.

U wordt meestal een alias voor DataFu functies definiëren. Bijvoorbeeld:

    DEFINE SHA datafu.pig.hash.SHA();
    
Hiermee definieert u een alias benoemde `SHA` voor de functie hashing SHA. U kunt dit vervolgens gebruiken in een varken Latijns-script om te genereren van een hash voor de invoergegevens. Bijvoorbeeld de volgende Hiermee vervangt u de namen in de invoergegevens door een hashwaarde:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Als dit wordt gebruikt met de volgende invoergegevens:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
De volgende uitvoer wordt gegenereerd:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Volgende stappen

Zie de volgende documenten voor meer informatie over DataFu of varken:

* [Apache DataFu varken handleiding](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
