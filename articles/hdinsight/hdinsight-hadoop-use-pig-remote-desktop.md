<properties
   pageTitle="Hadoop varken gebruiken met extern bureaublad in HDInsight | Microsoft Azure"
   description="Leer hoe u de opdracht varken met varken Latijns-instructies uitvoeren vanuit een verbinding met extern bureaublad met een Hadoop op basis van een Windows-cluster in HDInsight."
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

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Varken taken uitvoeren vanaf een verbinding met extern bureaublad

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dit document bevat stapsgewijze instructies voor het gebruik van de opdracht varken varken Latijns-instructies uitvoeren van een verbinding met extern bureaublad aan een cluster HDInsight op basis van Windows. Varken Latijns kunt u MapReduce toepassingen maken door te beschrijven gegevenstransformaties, in plaats van toewijzen en functies te verkleinen.

In dit document, leert u hoe u

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende.

* Een cluster op basis van Windows HDInsight (Hadoop op HDInsight)

* Een clientcomputer met Windows 10, Windows 8 of Windows 7

##<a id="connect"></a>Verbinding met extern bureaublad

Extern bureaublad voor het cluster HDInsight inschakelen en klik verbinden volgens de instructies in [verbinding maken met HDInsight clusters RDP gebruiken](hdinsight-administer-use-management-portal.md#rdp).

##<a id="pig"></a>Gebruik de opdracht varken

2. Nadat u een verbinding met extern bureaublad hebt, start u de **opdrachtregel Hadoop** via het pictogram op het bureaublad.

2. Gebruik de volgende handelingen uit om te beginnen van de opdracht varken:

        %pig_home%\bin\pig

    U krijgt een `grunt>` prompt.

3. Voer de volgende instructie:

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Deze opdracht laadt de inhoud van het bestand sample.log in het bestand LOGBOEKEN. U kunt de inhoud van het bestand weergeven met behulp van de volgende opdracht uit:

        DUMP LOGS;

4. De gegevens transformeren door een reguliere expressie alleen het niveau voor logboekregistratie extraheren uit elke record toe te voegen:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    U kunt **DUMP** gebruiken om weer te geven van de gegevens na de transformatie. In dit geval `DUMP LEVELS;`.

5. Verdergaan met het toepassen van transformaties met behulp van de volgende beweringen. Gebruik `DUMP` om weer te geven van het resultaat van de transformatie na elke stap.

    <table>
    <tr>
    <th>Instructie</th><th>Beschrijving</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER niveaus door LOGLEVEL niet null is.</td><td>Hiermee verwijdert u rijen met een null-waarde voor het niveau voor logboekregistratie en slaat de resultaten in FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = groep FILTEREDLEVELS door LOGLEVEL;</td><td>De rijen op niveau voor logboekregistratie van groepen en slaat de resultaten in GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREQUENTIE foreach = GROUPEDLEVELS groep genereren als LOGLEVEL, tellen (FILTEREDLEVELS. LOGLEVEL) zoals tellen;</td><td>Hiermee maakt u een nieuwe set gegevens bevat elke unieke log niveau waarde en hoe vaak deze plaatsvindt. Dit is opgeslagen in de frequentie</td>
    </tr>
    <tr>
    <td>RESULTAAT = volgorde frequentie door tellen desc;</td><td>Logboekniveaus orders door tellen (aflopend), worden opgeslagen en tot resultaat</td>
    </tr>
    </table>

6. U kunt ook de resultaten van een transformatie opslaan met behulp van de `STORE` instructie. Bijvoorbeeld de volgende opdracht Hiermee slaat de `RESULT` naar de map **/example/data/pigout** in de container standaard opslagruimte voor uw cluster:

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] De gegevens worden opgeslagen in de opgegeven map in de bestanden **deel nnnnn**. Als de map al bestaat, ontvangt u een foutbericht wordt weergegeven.

7. Voer de volgende instructie de prompt knorvis om af te sluiten.

        QUIT;

###<a name="pig-latin-batch-files"></a>Varken Latijns batch-bestanden

U kunt ook de opdracht varken gebruiken om uit te voeren varken Latijns die deel van een bestand uitmaakt.

3. Na de prompt knorvis afgemeld, open **Kladblok** en maak een nieuw bestand met de naam **pigbatch.pig** in de map **% PIG_HOME %** .

4. Typ of plak de volgende regels in het bestand **pigbatch.pig** en sla deze:

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Gebruik de volgende handelingen uit in het **pigbatch.pig** -bestand met de opdracht varken uitvoeren.

        pig %PIG_HOME%\pigbatch.pig

    Als de batchtaak is voltooid, ziet u de volgende uitvoer, die hetzelfde zijn moet als wanneer u gebruikt `DUMP RESULT;` in de vorige stappen:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Overzicht

Zoals u ziet, wordt de opdracht varken kunt u interactief MapReduce bewerkingen uitvoeren of varken Latijns-taken die zijn opgeslagen in een batchbestand uitvoeren.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over varken in HDInsight:

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
