<properties
   pageTitle="Twitter-trending onderwerpen met Apache Storm op HDInsight | Microsoft Azure"
   description="Leer hoe u Trident gebruiken om te maken van de topologie van een Apache Storm die trending onderwerpen op Twitter op basis van hashtags bepaalt."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Twitter trending onderwerpen met Apache Storm op HDInsight bepalen

Leer hoe u Trident gebruiken om te maken van de topologie van een Storm die trending onderwerpen (hash tags) op Twitter bepaalt.

Trident is een hoog niveau abstractie hulpprogramma's zoals joins, aggregaties, groeperen, functies en filters waarmee u. Daarnaast kunnen telt Trident primitieven voor het uitvoeren van statuscontrole, incrementele verwerking. In dit voorbeeld wordt gedemonstreerd hoe u een topologie met een aangepaste spout, functie en diverse ingebouwde functies die worden verstrekt door Trident kunt samenstellen.

> [AZURE.NOTE] In dit voorbeeld is intensief gebaseerd op het voorbeeld [Trident Storm](https://github.com/jalonsoramos/trident-storm) door Juan Alonso.

##<a name="requirements"></a>Vereisten

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java en de JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a>

* <a href="http://git-scm.com/" target="_blank">Cijfer</a>

* Een Twitter-account voor ontwikkelaars

##<a name="download-the-project"></a>Downloaden van het project

Gebruik de volgende code aan het project lokaal klonen.

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>Topologie

De topologie voor dit voorbeeld is er als volgt uit:

![topologie](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] Dit is een vereenvoudigde weergave van de topologie. Meerdere exemplaren van de onderdelen worden verdeeld over de knooppunten in het cluster.

De Trident-streepjescode die door de topologie implementeert is als volgt:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Deze code, gebeurt het volgende:

1. Hiermee maakt u een nieuwe stroom van de spout. De spout tweets opgehaald uit Twitter en ze filters voor specifieke trefwoorden (liefde, muziek en restaurant in dit voorbeeld).

2. HashtagExtractor, een aangepaste functie wordt gebruikt voor het uitpakken hash tags uit elke tweet. Deze worden verstrekt aan de stream.

3. De stream is gegroepeerd op hash tag en doorgegeven aan een-lezer. Deze aggregator Hiermee maakt u een telling van het aantal keren dat elk naamkaartje hash is opgetreden. Deze gegevens blijft behouden in het geheugen. Een nieuwe stream geeft ten slotte met de tag hash en het aantal.

4. Omdat we alleen geïnteresseerd in het meest populaire hash labels voor een bepaalde reeks tweets bent, wordt de constructie **FirstN** om alleen de bovenste 10 waarden, op basis van het aantalveld terug te toegepast.

> [AZURE.NOTE] Dan de spout en HashtagExtractor gebruiken we ingebouwde Trident-functionaliteit.
>
> Zie voor informatie over de werking van de ingebouwde <a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">pakket storm.trident.operation.builtin</a>.
>
> Zie de volgende onderwerpen voor Trident statuswaarden implementaties dan MemoryMapState:
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">Storm Trident elastische zoeken</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">Trident-bestand Vgx.</a>

###<a name="the-spout"></a>De spout

De spout, **TwitterSpout**, gebruikt <a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a> tweets ophalen uit Twitter. Een filter wordt gemaakt (liefde, muziek en restaurant in dit voorbeeld) en de binnenkomende tweets (status) die overeenkomen met het filter in een gekoppelde blokkeren wachtrij worden opgeslagen. (Zie <a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">Class LinkedBlockingQueue</a>voor meer informatie.) Ten slotte zijn items die verzameld uit de wachtrij en dat aan de topologie.

###<a name="the-hashtagextractor"></a>De HashtagExtractor

Als u wilt extraheren hash labels, <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">getHashtagEntities</a> gebruikt voor het ophalen van alle hash tags die zijn opgenomen in de tweet. Deze worden vervolgens geeft aan de stream.

##<a name="enable-twitter"></a>Twitter inschakelen

Gebruik de volgende stappen voor het registreren van een nieuwe Twitter-toepassing en de consumenten en toegang token informatie nodig zijn om te lezen Twitter verkrijgen:

1. Ga naar <a href="https://apps.twitter.com" target="_blank">Twitter-Apps</a> en klik op de knop **nieuwe app maken** . Wanneer het formulier invullen, laat u het veld **URL terugbellen** leeg.

2. Wanneer de app is gemaakt, klikt u op het tabblad **toetsen en Access Tokens** .

3. Kopieer de informatie **Consumentsleutel** en **Consumentgeheim** .

4. Onderaan op de pagina, selecteert u **Mijn toegangstoken maken** als er geen tokens bestaat. Wanneer de tokens zijn gemaakt, kunt u de **Toegangstoken** en **Token geheim van Access** -gegevens kopiëren.

5. In het **TwitterSpoutTopology** project dat u eerder hebt gekopieerd, opent u het bestand **resources/twitter4j.properties** , voegt u de informatie die u verzameld in de vorige stappen en sla het bestand.

##<a name="build-the-topology"></a>De topologie maken

Gebruik de volgende code voor het maken van het project:

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>De topologie testen

Gebruik de volgende opdracht uit om te testen van de topologie lokaal:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Nadat de topologie wordt gestart, ziet u gegevens voor foutopsporing de hash met labels en telt verzonden door de topologie. De uitvoer moet worden de volgende strekking weergegeven:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>Volgende stappen

Nu dat u de topologie lokaal getest hebt, Ontdek hoe u het implementeren van de topologie: [distribueren en beheren van Apache Storm topologieën op HDInsight](hdinsight-storm-deploy-monitor-topology.md).

U is mogelijk ook geïnteresseerd in de volgende onderwerpen voor Storm:

* [Ontwikkel Java topologieën voor Storm op HDInsight Maven gebruiken](hdinsight-storm-develop-java-topology.md)

* [Ontwikkel C# topologieën voor Storm op HDInsight gebruik van Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Voor meer voorbeelden Storm voor HDinsight:

* [Voorbeeld topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md)
