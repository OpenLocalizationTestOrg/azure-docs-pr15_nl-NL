<properties
    pageTitle="Aanbevelingen met Mahout en HDInsight Linux gebaseerde genereren | Microsoft Azure"
    description="Informatie over het gebruik van de Apache Mahout-machine learning-bibliotheek te genereren film aanbevelingen met Linux gebaseerde HDInsight (Hadoop)."
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
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Film aanbevelingen genereren met behulp van Apache Mahout met Linux gebaseerde Hadoop in HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Informatie over het gebruik van de [Apache Mahout](http://mahout.apache.org) -machine learning-bibliotheek met Azure HDInsight film aanbevelingen genereren.

Mahout is een [machine learning] [ ml] bibliotheek voor Apache Hadoop. Mahout bevat algoritmen voor het verwerken van gegevens, zoals filteren, indelen, en cluster. In dit artikel, kunt u een aanbeveling-engine wilt gebruiken voor het genereren van de film aanbevelingen die zijn gebaseerd op films die uw vrienden hebt gezien.

> [AZURE.NOTE] De stappen in dit document moet een Hadoop Linux gebaseerde op HDInsight cluster. Zie voor informatie over het gebruik van Mahout met een cluster op basis van Windows [genereren film aanbevelingen met behulp van Apache Mahout met Windows gebaseerde Hadoop in HDInsight](hdinsight-mahout.md)

##<a name="prerequisites"></a>Vereisten voor

* Een Linux gebaseerde Hadoop op HDInsight cluster. Zie voor informatie over het maken van een [aan de slag met Linux gebaseerde Hadoop in HDInsight][getstarted]

##<a name="mahout-versioning"></a>Mahout versiebeheer

Zie voor meer informatie over de versie van Mahout wordt geleverd bij uw cluster HDInsight [HDInsight versies en Hadoop-onderdelen](hdinsight-component-versioning.md).

> [AZURE.WARNING] Het is mogelijk om een andere versie van Mahout te uploaden naar het cluster HDInsight, alleen onderdelen van het cluster HDInsight volledig worden ondersteund en Microsoft Support helpt isoleren en oplossen van problemen met deze onderdelen.
>
> Aangepaste onderdelen ontvangen commercieel redelijk ondersteuning waarmee u kunt het probleem verder kunt oplossen. Dit kan leiden tot het probleem oplossen of vraag of u kunt voeren beschikbare kanalen voor de bron openen technologieën waar uitgebreide expertise voor deze technologie is gevonden. Bijvoorbeeld, zijn er veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache projecten tevens projectsites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/), [een](http://spark.apache.org/).

##<a name="recommendations"></a>Lidmaatschap aanbevelingen

Een van de functies die is verstrekt door Mahout is een engine aanbeveling. Deze engine accepteert gegevens in de notatie van `userID`, `itemId`, en `prefValue` (gebruikers van de voorkeur voor het item). Mahout kunt vervolgens uitvoeren voor collega exemplaar analyse om te bepalen: _gebruikers die een voorkeur voor een item heeft ook een voorkeur voor deze andere items bevatten_. Mahout bepaalt vervolgens gebruikers met vind ik leuk-item-voorkeuren, kunnen worden gebruikt om aanbevelingen.

Hier volgt een zeer eenvoudige voorbeeld met films:

* __CO exemplaar__: Jan, Lisa en Stefan leuk vindt _ster oorlogen_ _Terug ontvangt in het imperium_en _retourneren van het Jedi_. Mahout bepaalt dat gebruikers die een van deze films ook tevreden bent over de andere twee.

* __CO exemplaar__: Bob en Lisa ook leuk vindt _Het Phantom Menace_, _aanval van de klonen_en _Revenge van de Sith_. Mahout bepaalt dat gebruikers die de vorige drie films ook leuk vindt tevreden bent over deze drie.

* __Overeenkomsten aanbeveling__: omdat Jan de eerste drie films leuk vindt, Mahout analyseert films die anderen met soortgelijke voorkeuren leuk vindt, maar Jan heeft niet die u wilt controleren (leuk vindt/geclassificeerd). In dit geval aanbevolen Mahout _Het Phantom Menace_, _aanval van de klonen_en _Revenge van de Sith_.

###<a name="understanding-the-data"></a>Informatie over de gegevens

Gemakkelijk [GroupLens onderzoek] [ movielens] classificatie-gegevens voor films in een indeling die compatibel is met Mahout bevat. Deze gegevens zijn beschikbaar op van uw cluster standaard opslagruimte met `/HdiSamples/HdiSamples/MahoutMovieData`.

Er zijn twee bestanden, `moviedb.txt` (informatie over de films) en `user-ratings.txt`. De gebruiker-ratings.txt-bestand wordt gebruikt tijdens analyse, terwijl moviedb.txt wordt gebruikt om aan te bieden van gebruikersvriendelijke tekst gegevens bij het weergeven van de resultaten van de analyse.

De gegevens in de gebruiker-ratings.txt heeft een structuur van `userID`, `movieID`, `userRating`, en `timestamp`, waarin wordt uitgelegd ons hoe ten zeerste prioriteitsniveau van een film in elke gebruiker. Hier volgt een voorbeeld van de gegevens:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>De analyse uitvoeren

Gebruik de volgende opdracht uit de aanbeveling taak uit te voeren:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] De taak kan enkele minuten duren om uit te voeren, en meerdere MapReduce taken kunnen uitvoeren.

##<a name="view-the-output"></a>De uitvoer weergeven

1. Zodra de taak is voltooid, gebruikt u de volgende opdracht uit om weer te geven van de gegenereerde uitvoer:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    De uitvoer wordt als volgt weergegeven:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    De eerste kolom is de `userID`. De waarden in ' [' en ']' zijn `movieId`:`recommendationScore`.

2. De uitvoer, samen met de moviedb.txt, kunt u meer beschrijvende gebruikersgegevens weergeven. Eerst moeten we de bestanden kopiëren lokaal met de volgende opdrachten:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    Hiermee wordt de uitvoergegevens kopiëren naar een bestand met de naam **recommendations.txt** in de huidige map, samen met de film-gegevensbestanden.

3. Gebruik de volgende opdracht uit naar een nieuwe Python script maken waarmee film namen voor de gegevens in de uitvoer aanbevelingen wordt opzoeken:

        nano show_recommendations.py

    Wanneer de editor wordt geopend, gebruikt u de volgende handelingen uit als de inhoud van het bestand:

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Druk op **CTRL + X**, **Y**en ten slotte **Enter** om de gegevens te slaan.

3. Gebruik de volgende opdracht uit om het bestand uitvoerbare:

        chmod +x show_recommendations.py

4. Voer het script Python. Het volgende wordt ervan uitgegaan dat u in de map waar alle bestanden die zijn gedownload:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    Hiermee wordt de aanbevelingen voor gebruikers-ID 4 gegenereerd bekijken.

    * De **gebruiker-ratings.txt** -bestand wordt gebruikt voor het ophalen van films die de gebruiker heeft beoordeeld
    * De **moviedb.txt** -bestand wordt gebruikt voor het ophalen van de namen van de films
    * De **recommendations.txt** wordt gebruikt voor het ophalen van de film aanbevelingen voor deze gebruiker

    De uitvoer van deze opdracht is ongeveer als volgt uit:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Tijdelijke gegevens verwijderen

Mahout taken verwijderd tijdelijke gegevens die zijn gemaakt tijdens het verwerken van de taak niet. De `--tempDir` parameter wordt opgegeven in de taak voorbeeld te isoleren van de tijdelijke bestanden in een specifieke pad voor het gemakkelijk verwijderen. De temp om bestanden te verwijderen, gebruikt u de volgende opdracht:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Als u de opdracht opnieuw uitvoeren wilt, moet u ook de uitvoermap verwijderen. Gebruik de volgende manieren te werk om deze map te verwijderen:
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u Mahout gebruikt, kunt u andere manieren werken met gegevens in HDInsight ontdekken:

* [Component met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight](hdinsight-use-pig.md)
* [MapReduce met HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
