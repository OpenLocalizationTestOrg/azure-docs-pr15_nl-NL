<properties
   pageTitle="Python onderdelen gebruiken in een topologie Storm op HDinsight | Microsoft Azure"
   description="Leer hoe u Python onderdelen van kunt gebruiken met Apache Storm op Azure HDInsight. Leert u hoe u Python onderdelen van beide een Java gebaseerd en Clojure gebaseerd Storm topologie."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Ontwikkel Apache Storm topologieën Python op HDInsight gebruiken

Apache Storm ondersteunt meerdere talen, zelfs zodat u onderdelen van verschillende talen in één topologie combineren. In dit document leert u hoe u Python onderdelen gebruiken in uw Java en Clojure gebaseerde Storm topologieën op HDInsight.

##<a name="prerequisites"></a>Vereisten voor

* Python 2,7 of hoger

* Java JDK 1.7 of hoger

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>Storm meertalige ondersteuning

Storm is ontworpen voor gebruik met geschreven met behulp van elke programmeertaal, maar hiervoor is vereist dat de onderdelen meer informatie over het werken met de [definitie van Thrift voor Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift)onderdelen. Een module is voor Python opgegeven als onderdeel van het project Apache Storm waarmee u eenvoudig samenwerken met Storm. U kunt deze module op [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py)vinden.

Aangezien Apache Storm is een Java-proces die wordt uitgevoerd op de Java Virtual Machine (JVM), worden onderdelen in een andere taal zijn geschreven als subprocessen uitgevoerd. De Storm bits uitgevoerd in de JVM communiceert met deze subprocessen JSON-berichten die zijn verzonden via stdin/stdout gebruiken. Meer informatie over de communicatie tussen onderdelen vindt u in de documentatie [Multi-lang Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) .

###<a name="the-storm-module"></a>De module Storm

De storm-module (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py), biedt de bits nodig Python onderdelen die met Storm werken maken.

Hier vindt u zaken als `storm.emit` uitzenden tupels, en `storm.logInfo` schrijven naar de logboeken. Ik zou raden u aan het onderwerp via dit bestand worden gelezen en begrijpt wat deze bevat.

##<a name="challenges"></a>Uitdagingen

Gebruik van de module __storm.py__ , kunt u Python spouts die gegevens en bouten waarmee gegevens, worden verwerkt in beslag neemt maar de algehele Storm topologie definitie die kabels communicatie tussen onderdelen nog steeds is geschreven met Java of Clojure maken. Als u Java gebruikt, moet u Daarnaast kunnen ook Java-onderdelen die als een interface voor de onderdelen Python fungeren maken.

Ook, aangezien Storm clusters in een gedistribueerde manier uitvoeren, moet u ervoor zorgen dat alle modules die zijn vereist voor uw Python-onderdelen beschikbaar op alle werknemer knooppunten in het cluster zijn. Storm niet zelf een eenvoudige manier om dit te doen voor multi-lang resources: u hebt alle afhankelijkheden opnemen als onderdeel van het oppervlak-bestand voor de topologie of afhankelijkheden handmatig op elk knooppunt werknemer in het cluster installeren.

###<a name="java-vs-clojure-topology-definition"></a>Java versus Clojure topologie definitie

Van de twee methoden voor het definiëren van een topologie, is Clojure de eenvoudigste/zuiverst mogelijk rechtstreeks referenc python onderdelen in de definitie van de topologie. Voor Java gebaseerde topologie definities, moet u ook Java-onderdelen die dingen zoals de velden in de tupels declareren geretourneerd door de onderdelen Python verwerken definiëren.

Beide methoden worden beschreven in dit document, samen met voorbeeldprojecten.

##<a name="python-components-with-a-java-topology"></a>Python onderdelen met een Java-topologie

> [AZURE.NOTE] In dit voorbeeld is beschikbaar op [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) in de adreslijst __JavaTopology__ . Dit is een Maven op basis van project. Als u niet bekend met Maven bent, raadpleegt u [ontwikkelen Java gebaseerde topologieën met Apache Storm op HDInsight](hdinsight-storm-develop-java-topology.md) voor meer informatie over het maken van een project Maven voor een Storm topologie.

Een Java gebaseerde topologie die gebruikmaakt van Python (of andere onderdelen van de taal JVM) wordt in eerste instantie weergegeven Java componenten; gebruiken maar als u wilt in elk van de Java spouts/bouten opzoeken, ziet u de code die er ongeveer als volgt uit:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

Dit is waar Java roept Python en wordt het script waarin de werkelijke bout logica wordt uitgevoerd. De Java-spouts/bouten (voor dit voorbeeld) declareren de velden in de tupel die wordt door de onderliggende Python-component worden verzonden.

De werkelijke Python-bestanden worden opgeslagen in de `/multilang/resources` directory in dit voorbeeld. De `/multilang` directory wordt verwezen in de __pom.xml__:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${basedir} / multilang</directory>
    </resource>
</resources>

Deze groep omvat alle bestanden in de `/multilang` map in het oppervlak die wordt worden opgebouwd uit dit project.

> [AZURE.IMPORTANT] Opmerking die alleen Hiermee wordt de `/multilang` directory en niet `/multilang/resources`. Storm verwacht niet JVM resources in een `resources` directory, zodat deze intern al wordt gezocht. Onderdelen plaatsen in deze map kunt u alleen verwijzing door de naam van de Java-code. Bijvoorbeeld `super("python", "countbolt.py");`. Een andere manier om na te denken ervan is dat Storm ziet het `resources` map als de hoofdmap (/) bij het openen van multi-lang resources.
>
> Voor dit voorbeeldproject de `storm.py` module is opgenomen in de `/multilang/resources` directory.

###<a name="build-and-run-the-project"></a>Maken en uitvoeren van het project

Als u wilt dit project lokaal uitvoeren, moet u alleen de volgende opdracht uit Maven gebruiken om maken en uitvoeren in de lokale modus:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

Gebruik ctrl + c om het proces beëindigen.

Als u wilt implementeren het project met een HDInsight cluster Apache Storm, gebruik de volgende stappen uit:

1. Een oppervlak uber maken:

        mvn package

    Hiermee maakt u een bestand met de naam __WordCount--1.0-SNAPSHOT.jar__ in de `/target` directory voor dit project.

2. Het oppervlak-bestand uploaden naar het Hadoop-cluster met een van de volgende methoden:

    * Voor __Linux gebaseerde__ HDInsight clusters: Gebruik `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` het oppervlak-bestand kopiëren naar het cluster, gebruikersnaam met uw gebruikersnaam in te voeren SSH en CLUSTERNAAM vervangen door de naam van het cluster HDInsight.

        Als het bestand is voltooid, verbinding maken met het cluster via SSH en de topologie gebruiken`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * Voor __Windows gebaseerde__ HDInsight clusters: verbinding maken met het Dashboard Storm door te gaan naar HTTPS://CLUSTERNAME.azurehdinsight.net/ in uw browser. CLUSTERNAAM vervangen door de naam van uw HDInsight cluster en geef de beheerdersnaam en het wachtwoord wanneer u wordt gevraagd.

        Toe aan het formulier, de volgende acties uitvoeren:

        * __Jar-bestand__: Selecteer __Bladeren__en selecteer vervolgens het bestand __WordCount-1.0-SNAPSHOT.jar__
        * __Klassenaam__: invoeren`com.microsoft.example.WordCount`
        * __Aanvullende maximumconcentratie__: Voer een beschrijvende naam zoals `wordcount` om aan te geven van de topologie

        Selecteer ten slotte __indienen__ bij het starten van de topologie.

> [AZURE.NOTE] Wanneer is gestart, de topologie van een Storm wordt uitgevoerd totdat gestopt (gedode.) Als u wilt stoppen met de topologie, gebruikt u een de `storm kill TOPOLOGYNAME` command vanaf de opdrachtregel (SSH sessie aan een cluster Linux bijvoorbeeld) of met behulp van de gebruikersinterface Storm, selecteert u de topologie en selecteer vervolgens de knop __verwijderen__ .

##<a name="python-components-with-a-clojure-topology"></a>Python onderdelen met een topologie Clojure

> [AZURE.NOTE] In dit voorbeeld is beschikbaar op [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) in de adreslijst __ClojureTopology__ .

Deze topologie is gemaakt met behulp van [Leiningen](http://leiningen.org) om een nieuwe Clojure project te [maken](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project). Daarna zijn de volgende wijzigingen aan in het scaffolded project aangebracht:

* __project.CLJ__: afhankelijkheden toegevoegd voor Storm en uitsluitingen voor items die mogelijk een probleem wanneer geïmplementeerd op de server HDInsight.
* __resources/resources__: Leiningen Hiermee maakt u een standaard `resources` directory, maar de bestanden die zijn opgeslagen hier weergegeven aan in de hoofdmap van het oppervlak-bestand dat is gemaakt op basis van dit project wordt toegevoegd en Storm bestanden in een submap met de naam verwacht `resources`. Zodat een submap is toegevoegd en de Python-bestanden worden opgeslagen in `resources/resources`. Tijdens de uitvoering, wordt dit verwerkt als de hoofdmap (/) voor toegang tot Python onderdelen.
* __src/WordCount/Core.CLJ__: dit bestand bevat de definitie van de topologie en uit het bestand __project.clj__ wordt verwezen. Zie voor meer informatie over het gebruik van Clojure definiëren van de topologie van een Storm [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html).

###<a name="build-and-run-the-project"></a>Maken en uitvoeren van het project

__Maken en uitvoeren van het project lokaal__, gebruikt u de volgende opdracht:

    lein clean, run

Als u wilt stoppen met de topologie, gebruikt u __Ctrl + C__.

__Voor een uberjar bouwen en implementeren naar HDInsight__, gebruikt u de volgende stappen:

1. Maak een uberjar met de topologie en afhankelijkheden vereist:

        lein uberjar

    Hiermee maakt u een nieuw bestand met de naam `wordcount-1.0-SNAPSHOT.jar` in de `target\uberjar+uberjar` directory.
    
2. Gebruik een van de volgende manieren kunt implementeren en uitvoeren van de topologie aan een cluster HDInsight:

    * __Linux gebaseerde HDInsight__
    
        1. Kopieer het bestand naar het gebruik van hoofd knooppunt HDInsight cluster `scp`. Bijvoorbeeld:
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            GEBRUIKERSNAAM vervangen door een gebruiker SSH voor uw cluster en CLUSTERNAAM met de naam van uw cluster HDInsight.
            
        2. Wanneer het bestand is gekopieerd naar het cluster, via u SSH verbinding maken met het cluster en de taak verstuurt. Zie voor informatie over het gebruik van SSH met HDInsight een van de volgende opties:
        
            * [SSH gebruiken met Linux gebaseerde HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [SSH gebruiken met Linux gebaseerde HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. Zodra u verbinding hebt, gebruikt u de volgende starten van de topologie:
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __Op basis van Windows HDInsight__
    
        1. Verbinding maken met het Dashboard Storm door te gaan naar HTTPS://CLUSTERNAME.azurehdinsight.net/ in uw browser. CLUSTERNAAM vervangen door de naam van uw HDInsight cluster en geef de beheerdersnaam en het wachtwoord wanneer u wordt gevraagd.

        2. Toe aan het formulier, de volgende acties uitvoeren:

            * __Jar-bestand__: Selecteer __Bladeren__en selecteer vervolgens het bestand __wordcount-1.0-SNAPSHOT.jar__
            * __Klassenaam__: invoeren`wordcount.core`
            * __Aanvullende maximumconcentratie__: Voer een beschrijvende naam zoals `wordcount` om aan te geven van de topologie

            Selecteer ten slotte __indienen__ bij het starten van de topologie.

> [AZURE.NOTE] Wanneer is gestart, de topologie van een Storm wordt uitgevoerd totdat gestopt (gedode.) Als u wilt stoppen met de topologie, gebruikt u een de `storm kill TOPOLOGYNAME` command vanaf de opdrachtregel (SSH sessie aan een cluster Linux) of met behulp van de gebruikersinterface Storm, selecteert u de topologie en selecteer vervolgens de knop __verwijderen__ .

##<a name="next-steps"></a>Volgende stappen

In dit document, hebt u geleerd hoe Python onderdelen van de topologie van een Storm gebruiken. Raadpleeg de volgende documenten voor andere manieren om te Python gebruiken met HDInsight:

* [Het gebruik van Python voor streaming MapReduce taken](hdinsight-hadoop-streaming-python.md)
* [Het gebruik van Python gebruiker gedefinieerde functies (UDF) in varken en component](hdinsight-python.md)
