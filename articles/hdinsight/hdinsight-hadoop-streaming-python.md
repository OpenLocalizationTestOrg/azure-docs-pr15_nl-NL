<properties
   pageTitle="Ontwikkel Python MapReduce taken met HDInsight | Microsoft Azure"
   description="Informatie over het maken en uitvoeren van taken Python MapReduce op HDInsight Linux gebaseerde clusters."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Ontwikkel Python streaming programma's voor HDInsight

Hadoop biedt een streaming API voor MapReduce waarmee u kunt de kaart te schrijven en functies in andere talen dan Java te verkleinen. In dit artikel leert u hoe u met Python MapReduce bewerkingen uitvoeren.

> [AZURE.NOTE] Terwijl de code Python in dit document kan worden gebruikt met een cluster HDInsight op basis van Windows, zijn de stappen in dit document specifiek voor Linux gebaseerde clusters.

In dit artikel is gebaseerd op informatie en voorbeelden die zijn gepubliceerd door Michael Noll bij het [schrijven van een Hadoop MapReduce-programma in Python](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/).

##<a name="prerequisites"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

* Een Hadoop Linux gebaseerde op HDInsight cluster

* Een teksteditor

    > [AZURE.IMPORTANT] De teksteditor moet LF gebruiken als het einde van de regel. Als u deze CRLF gebruikt, hierdoor wordt fouten bij het uitvoeren van de taak MapReduce op HDInsight Linux gebaseerde clusters. Als u niet zeker weet, voert u de optionele stap in het gedeelte [Uitvoeren MapReduce](#run-mapreduce) eventuele CRLF converteren naar LF.

* Voor Windows-clients, stopverf en PSCP. Deze hulpprogramma's zijn beschikbaar via de <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">Pagina voor het downloaden van stopverf</a>.

##<a name="word-count"></a>Woorden tellen

In dit voorbeeld voert u een eenvoudige woorden tellen met behulp van een toewijzing en reducer. De toewijzing zinnen opgesplitst in afzonderlijke woorden en de reducer de woorden is samengevoegd en telt als u wilt de uitvoer produceren.

Het volgende stroomdiagram ziet u wat er gebeurt tijdens de kaart en fasen te verkleinen.

![afbeelding van map verkleinen](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>Waarom Python?

Python is een algemene, op hoog niveau programmeertaal waarmee u kunt express concepten in programmacoderegels minder dan vele andere talen. Onlangs heeft werd populaire met gegevens wetenschappers als een taal prototypen omdat de geïnterpreteerd aard, dynamische te typen en de syntaxis van de elegante eenvoudiger geschikt voor snelle ontwikkeling van toepassingen.

Python is op alle HDInsight clusters geïnstalleerd.

##<a name="streaming-mapreduce"></a>Streaming MapReduce

Hadoop kunt u opgeven van een bestand met de kaart en logica die wordt gebruikt door een taak te verkleinen. De specifieke vereisten voor de kaart en logica te verkleinen zijn:

* **Invoerbereik**: de kaart en verkleinen onderdelen STDIN invoergegevens moeten lezen.

* **Uitvoer**: de kaart en verkleinen onderdelen STDOUT uitvoergegevens moeten schrijven.

* **Opmaak van gegevens**: de gegevens verbruikt en geproduceerd moet de combinatie van een sleutel/waarde, gescheiden door een tab.

Python kunt gemakkelijk deze vereisten is voldaan met behulp van de module **sys** te lezen STDIN en af te drukken STDOUT met behulp van **afdrukken** . De resterende taak is alleen de gegevens met een tab Opmaak (`\t`) teken tussen de sleutel en waarde.

##<a name="create-the-mapper-and-reducer"></a>De toewijzing en reducer maken

De toewijzing en reducer zijn tekstbestanden, in dit geval **mapper.py** en **reducer.py** (u kunt deze wissen welke wat is). U kunt deze met de editor van uw keuze te maken.

###<a name="mapperpy"></a>Mapper.PY

Een nieuw bestand met de naam **mapper.py** maken en gebruiken van de volgende code als de inhoud:

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

Neem even de tijd tot en met de code lezen, zodat u kan begrijpen hoe het werkt.

###<a name="reducerpy"></a>Reducer.PY

Een nieuw bestand met de naam **reducer.py** maken en gebruiken van de volgende code als de inhoud:

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>Upload de bestanden

Zowel **mapper.py** als **reducer.py** moeten zich op het hoofd knooppunt van de cluster voordat we kan worden uitgevoerd. De eenvoudigste manier om ze te uploaden is het gebruik van **scp** (**pscp** als u een Windows-client gebruikt).

Gebruik van de client, in dezelfde map als **mapper.py** en **reducer.py**, de volgende opdracht uit. **Gebruikersnaam** vervangen door een gebruiker SSH en **clusternaam** met de naam van uw cluster.

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

Hierdoor wordt de bestanden uit het lokale systeem naar het hoofd knooppunt gekopieerd.

> [AZURE.NOTE] Als u een wachtwoord beveiligen van uw SSH-account gebruikt, wordt u gevraagd het wachtwoord. Als u een sleutel SSH gebruikt, moet u mogelijk gebruiken de `-i` parameter en het pad naar de persoonlijke sleutel, bijvoorbeeld `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

##<a name="run-mapreduce"></a>MapReduce uitvoeren

1. Verbinding maken met het cluster met behulp van SSH:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Als u een wachtwoord beveiligen van uw SSH-account gebruikt, wordt u gevraagd het wachtwoord. Als u een sleutel SSH gebruikt, moet u mogelijk gebruiken de `-i` parameter en het pad naar de persoonlijke sleutel, bijvoorbeeld `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`.

2. (Optioneel) Als u een teksteditor die CRLF gebruikt als de regel die eindigt gebruikt bij het maken van de bestanden mapper.py en reducer.py of niet weet wat uw editor lijn-eindigt wordt gebruikt, gebruikt u de volgende opdrachten voor het converteren van exemplaren van CRLF in mapper.py en reducer.py naar LF.

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. Gebruik de volgende opdracht uit om de taak MapReduce te starten.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    Deze opdracht heeft de volgende argumenten:

    * **hadoop-streaming.jar**: gebruikt bij het uitvoeren van streaming MapReduce bewerkingen. Deze interfaces Hadoop met de externe MapReduce-code die u opgeeft.

    * **-bestanden**: Hiermee wordt aan Hadoop die de opgegeven bestanden nodig zijn voor deze taak MapReduce en ze moeten worden gekopieerd naar alle werknemer knooppunten.

    * **-toewijzing**: Hiermee wordt aan Hadoop welk bestand wilt gebruiken als de toewijzing.

    * **-reducer**: Hiermee wordt aan Hadoop welk bestand als de reducer moet worden gebruikt.

    * **-invoer**: bestand die moeten worden geteld woorden uit.

    * **-uitvoer**: de map die de uitvoer naar worden geschreven.

        > [AZURE.NOTE] Deze map wordt gemaakt door de taak.

U moet een reeks **INFO** -instructies als het starten van de taak wordt weergegeven en ten slotte raadpleegt u de **kaart** en **verkleinen** bewerking weergegeven als percentages.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

U ontvangt statusinformatie over de taak wanneer deze is voltooid.

##<a name="view-the-output"></a>De uitvoer weergeven

Wanneer de taak voltooid is, gebruikt u de volgende opdracht uit om weer te geven van de uitvoer:

    hdfs dfs -text /example/wordcountout/part-00000

Dit moet een lijst weergeven van woorden en hoe vaak het woord opgetreden. Hier volgt een voorbeeld van de uitvoergegevens:

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u streaming MapRedcue taken met HDInsight, gebruikt u de volgende koppelingen naar andere manieren om te werken met Azure HDInsight verkennen.

* [Component gebruiken met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [Gebruik MapReduce taken met HDInsight](hdinsight-use-mapreduce.md)
