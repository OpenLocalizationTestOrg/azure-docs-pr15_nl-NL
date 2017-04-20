<properties
    pageTitle="Voorbeelden van Hadoop MapReduce uitvoeren op Linux gebaseerde HDInsight | Microsoft Azure"
    description="Aan de slag met MapReduce voorbeelden met Linux gebaseerde HDInsight. SSH met verbinding maken met het cluster en het gebruik van de opdracht Hadoop steekproef taken uitvoeren."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>In de voorbeelden Hadoop worden uitgevoerd in HDInsight

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Linux gebaseerde HDInsight clusters bieden een set MapReduce voorbeelden die kunnen worden gebruikt om vertrouwd raken met het uitvoeren van Hadoop MapReduce taken. In dit document, moet u meer informatie over de beschikbare voorbeelden en doorlopen enkele van deze uitgevoerd.

##<a name="prerequisites"></a>Vereisten voor

- **Een Azure-abonnement**: Zie [gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

- **A Linux gebaseerde HDInsight cluster**: Zie [aan de slag met Hadoop met onderdeel in HDInsight op Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Een SSH-client**: Zie de volgende artikelen voor informatie over het gebruik van SSH met HDInsight:

    - [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>In de voorbeelden ##

**Locatie**: in de voorbeelden zich bevinden op het cluster HDInsight bij **/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar**

**Inhoud**: in de volgende voorbeelden zijn opgenomen in dit archief van:

- **aggregatewordcount**: een statistische op basis van map/verkleinen programma waarin de woorden in de invoer bestanden geteld
- **aggregatewordhist**: een statistische op basis van map/verkleinen programma waarin wordt berekend van het histogram van de woorden in de invoer bestanden
- **bbp**: een toewijzing/verkleinen programma dat Bailey-Borwein-Plouffe gebruikt om te berekenen van exacte cijfers van Pi
- **dbcount**: een voorbeeld van de taak die de pageview logboeken die zijn opgeslagen telt in een een database
- **distbbp**: een kaart/verkleinen-programma dat gebruikmaakt van een formule BBP-type te berekenen exacte bits van Pi
- **GREP**: een toewijzing/verkleinen programma waarbij de overeenkomsten van een regex in het vak Invoerbereik
- **join**: een taak die een join effecten meer dan gesorteerd, gelijkmatig partitioneren gegevenssets
- **multifilewc**: een taak waarbij de woorden uit verschillende bestanden
- **pentomino**: een toewijzing/verkleinen tegel legdatum programma om te zoeken van oplossingen voor pentomino problemen
- **pi**: een toewijzing/verkleinen programma dat maakt een schatting van Pi met een standaardcoupontermijn Monte Carlo methode
- **randomtextwriter**: een toewijzing/verkleinen programma waarmee gegevens worden geschreven 10 GB willekeurig tekstgegevens per knooppunt
- **randomwriter**: een toewijzing/verkleinen programma waarmee gegevens worden geschreven 10 GB aan willekeurige gegevens per knooppunt
- **secondarysort**: een voorbeeld van een secundaire sorteervolgorde definiëren naar de verkleinen
- **sorteren**: een toewijzing/verkleinen programma dat de gegevens die is geschreven door de willekeurige schrijver worden gesorteerd
- **sudoku**: een sudoku Oplosser
- **teragen**: gegevens voor de terasort genereren
- **terasort**: de terasort uitvoeren
- **teravalidate**: resultaten van terasort controleren
- **WordCount**: een toewijzing/verkleinen programma dat de woorden in de invoer bestanden geteld
- **wordmean**: een kaart/verkleinen programma waarbij de gemiddelde lengte van de woorden in de invoer bestanden
- **wordmedian**: een kaart/verkleinen programma waarbij de mediaan lengte van de woorden in de invoer bestanden
- **wordstandarddeviation**: een toewijzing/verkleinen programma waarbij de standaarddeviatie van de lengte van de woorden in de invoer bestanden

**Broncode**: bron-code voor deze voorbeelden wordt vermeld op de cluster HDInsight bij **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples**

> [AZURE.NOTE] De `2.2.4.9-1` in het pad is de versie van het Platform Hortonworks gegevens voor het cluster HDInsight en kan veranderen als HDInsight wordt bijgewerkt.

## <a name="how-to-run-the-samples"></a>Het uitvoeren van de voorbeelden ##

1. Verbinding maken met HDInsight via SSH zoals is beschreven in de volgende artikelen:

    - [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Vanuit de `username@#######:~$` wordt gevraagd, voert u de volgende opdracht uit voor een overzicht van de voorbeelden:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    Hierdoor wordt de lijst met voorbeeld uit de vorige sectie van dit document gegenereerd.

3. Gebruik de volgende opdracht hulp krijgen van een specifieke steekproef. In dit geval de steekproef **wordcount** :

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    U moet het volgende bericht:

        Usage: wordcount <in> [<in>...] <out>

    Hiermee wordt aangegeven dat u diverse invoer paden voor de brondocumenten kunt geven. Het laatste pad is waar u de uitvoer (het aantal woorden in de brondocumenten) is opgeslagen.

4. Gebruik de volgende handelingen uit om te tellen van alle woorden in de notitieblokken van Leonardo Da Vinci, die als voorbeeldgegevens worden geleverd met uw cluster:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    Invoer voor deze taak wordt van **wasbs:///example/data/gutenberg/davinci.txt**gelezen.

    Uitvoer voor dit voorbeeld is opgeslagen in **wasbs: / / / voorbeeld/gegevens/davinciwordcount**.

    > [AZURE.NOTE] Zoals beschreven in de help voor de steekproef wordcount, kunt u ook meerdere invoer bestanden opgeven. Bijvoorbeeld `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` woorden in zowel davinci.txt als ulysses.txt wilt tellen.

5. Zodra de taak is voltooid, gebruikt u de volgende opdracht uit om weer te geven van de uitvoer:

        hdfs dfs -cat /example/data/davinciwordcount/*

    Hiermee worden alle uitvoerbestanden die worden geproduceerd door de taak samengevoegd en waar ze worden weergegeven. Voor dit voorbeeld van de eenvoudige bestaat slechts één bestand, maar als er zijn meer deze opdracht alle labels doorlopen zou.

    De uitvoer is vergelijkbaar met het volgende:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Elke regel staat een woord en hoe vaak deze opgetreden in de invoergegevens.

## <a name="sudoku"></a>Sudoku

Het voorbeeld Sudoku heeft instructies over het gebruik van enigszins nutteloos; "Bevatten een puzzel op de opdrachtregel."

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) is een logische puzzel bestaat uit negen 3 x 3 rasters. Bepaalde cellen in het raster hebben nummers, terwijl andere leeg zijn, en het doel is om oplossen voor de lege cellen. De bovenstaande koppeling meer informatie over de puzzel heeft, maar het doel van dit voorbeeld is voor het oplossen van voor de lege cellen. Onze invoer moet dus een bestand in de volgende indeling:

- Negen rijen van negen kolommen

- Elke kolom bevatten beide een getal of `?` (waarmee wordt aangegeven een lege cel)

- Cellen worden gescheiden door een spatie

Er is een bepaalde manier om samen te stellen Sudoku puzzels; u kunt een getal in een kolom of rij niet herhalen. Er is een voorbeeld in het HDInsight cluster dat juist is samengesteld. Deze bevindt zich op **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** en bevat de volgende onderdelen:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] De `2.2.4.9-1` gedeelte van het pad mogelijk wijzigen, zoals het cluster HDInsight wordt bijgewerkt.

Als wilt uitvoeren dit tot en met het voorbeeld Sudoku, gebruikt u de volgende opdracht uit:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

De resultaten moeten worden de volgende strekking weergegeven:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>PI (π)

In dit voorbeeld pi worden een statistische (standaardcoupontermijn Monte Carlo) methode voor het schatten van de waarde van pi. Punten geplaatst willekeurig binnen een eenheid vierkante ook vallen binnen een cirkel die zijn aangebracht binnen die vierkant met een kans gelijk is aan het gebied van de cirkel, pi/4. De waarde van pi kan worden geschat met de waarde van 4R, waarbij R de verhouding van het aantal punten die zijn binnen de cirkel op het totale aantal punten die binnen het kwadraat is. Des te groter het voorbeeld van punten gebruikt, hoe beter de schatting is.

De toewijzing voor dit voorbeeld genereert een aantal van punten willekeurig binnen een vierkant per eenheid en klik vervolgens telt het aantal die punten die binnen de cirkel zijn.

De reducer vervolgens worden verwezen geteld op basis van de mappers bij elkaar opgeteld en maakt een schatting van de waarde van pi van de te000126961 4R, waarbij R de verhouding van het aantal punten in de berekening opgenomen binnen de cirkel op het totale aantal punten die binnen het kwadraat is.

Gebruik de volgende opdracht uit te voeren in dit voorbeeld. Hiermee wordt 16 kaarten met 10.000.000 voorbeelden voor het schatten van de waarde van pi gebruikt:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

De waarde die het resultaat van dit komt overeen met **3.14159155000000000000**zijn. Voor verwijzingen wordt gemaakt zijn de eerste 10 decimalen van pi 3.1415926535.

##<a name="10gb-greysort"></a>10GB Greysort

GraySort is een referentie-sortering waarvan metrisch het tarief weer sorteren (TB/minuut) die is tijdens het sorteren van zeer grote hoeveelheden gegevens, meestal een 100TB minimale wordt bereikt.

In dit voorbeeld wordt een bescheiden 10GB van gegevens, zodat deze relatief snel kan worden uitgevoerd. De MapReduce toepassingen ontwikkeld door Owen O'Malley en Arun Murthy die de jaarlijkse algemene ("daytona") terabyte sorteren vergelijkende in 2009 met een tarief van 0.578 TB/min (100 TB in 173 minuten gewonnen) wordt gebruikt. Zie voor meer informatie over dit en andere sorteren benchmarks, de site [Sortbenchmark](http://sortbenchmark.org/) .

In dit voorbeeld wordt drie sets met MapReduce-programma's gebruikt:

- **TeraGen**: een MapReduce-programma dat wordt gegenereerd rijen met gegevens om te sorteren

- **TeraSort**: voorbeelden van de invoergegevens en gebruikmaakt van MapReduce voor de gegevens in een totale volgorde sorteren

    TeraSort is een standaard sortering van MapReduce-functies, behalve voor een aangepaste partitioner die gebruikmaakt van een gesorteerde lijst van N-1 partijen toetsen die de belangrijkste bereik voor elke verkleinen definiëren. Met name, alle toetsen zoals voorbeeld [i-1] < = sleutel < voorbeeld [i] zijn verzonden naar i verkleinen. Dit garanties dat de uitvoer van i verkleinen worden alle kleiner dan de uitvoer van verkleinen i + 1.

- **TeraValidate**: een MapReduce-programma dat is gevalideerd met dat de uitvoer globaal is gesorteerd

    Een toewijzing per bestand wordt gemaakt in de adreslijst uitvoer en elke map zorgt ervoor dat elk sleutel kleiner dan of gelijk is aan de vorige taak is. De functie kaart genereert ook records van de eerste en laatste sleutels van elk bestand en de functie verkleinen zorgt ervoor dat de eerste toets van bestand i groter dan de laatste sleutel van bestand i-1 is. Sprake is van problemen worden gerapporteerd als een uitvoer van het verkleinen met de toetsen die onjuist zijn.

Gebruik de volgende stappen uit om het genereren van gegevens, sorteren, en vervolgens valideren de uitvoer:

1. 10GB van gegevens, die wordt opgeslagen naar van het HDInsight cluster standaard opslagruimte met genereren **wasbs: / / / voorbeeld / / 10GB-sorteren-gegevensinvoer**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    De `-Dmapred.map.tasks` Hadoop hoeveel kaart taken wilt gebruiken voor deze taak worden vermeld. Geef de laatste twee parameters de instructie om de taak te maken van 10GB aan gegevens en op te slaan op **wasbs: / / / voorbeeld / / 10GB-sorteren-gegevensinvoer**.

2. Gebruik de volgende opdracht uit om de gegevens te sorteren:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    De `-Dmapred.reduce.tasks` Hadoop wordt aangegeven hoeveel verkleinen taken voor de taak wilt gebruiken. De laatste twee parameters zijn alleen de locaties invoer- en uitvoerbereik voor gegevens.

3. Gebruik de volgende handelingen uit voor het valideren van de gegevens die zijn gegenereerd door het sorteren:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>Volgende stappen ##

In dit artikel, hebt u geleerd hoe uitvoeren in de voorbeelden wordt geleverd bij de clusters Linux gebaseerde HDInsight. Zie de volgende onderwerpen voor zelfstudies over het gebruik van varken, component en MapReduce met HDInsight:

* [Varken met Hadoop op HDInsight gebruiken][hdinsight-use-pig]
* [Component gebruiken met Hadoop op HDInsight][hdinsight-use-hive]
* [MapReduce gebruiken met Hadoop op HDInsight] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
