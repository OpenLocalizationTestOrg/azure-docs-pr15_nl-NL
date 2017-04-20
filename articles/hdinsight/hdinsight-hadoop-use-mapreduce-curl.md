<properties
   pageTitle="Gebruik MapReduce en krul met Hadoop in HDInsight | Microsoft Azure"
   description="Leer hoe u extern MapReduce taken met een Hadoop uitvoeren op HDInsight krul gebruiken."
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

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>MapReduce taken uitvoeren met Hadoop op HDInsight met krul

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In dit document leert u hoe u met krul MapReduce taken worden uitgevoerd op een Hadoop op HDInsight cluster.

Krul wordt gebruikt om te laten zien hoe u kunt communiceren met HDInsight via onbewerkte HTTP-aanvragen MapReduce taken uitvoeren. Dit werkt met behulp van de WebHCat REST API (voorheen bekend als Templeton) die door uw cluster HDInsight.

> [AZURE.NOTE] Als u al vertrouwd te raken met Linux gebaseerde Hadoop-servers bent, maar u hebt geen ervaring met HDInsight, raadpleegt u [Wat u moet weten over Linux gebaseerde Hadoop op HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

* Een Hadoop op HDInsight cluster (Linux of op basis van Windows)

* [Omslaan](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>MapReduce taken met krul uitvoeren

> [AZURE.NOTE] Wanneer u krul of een andere REST-communicatie met WebHCat gebruikt, moet u de aanvragen verifiëren door het HDInsight cluster beheerder-gebruikersnaam en wachtwoord te geven. U moet de naam van het cluster ook gebruiken als onderdeel van de URI die wordt gebruikt voor het versturen van aanvragen op de server.
>
> Voor de opdrachten in deze sectie, kunt u **gebruikersnaam** vervangen door de gebruiker worden geverifieerd bij de cluster en het **wachtwoord** door het wachtwoord voor het gebruikersaccount. **CLUSTERNAAM** vervangen door de naam van uw cluster.
>
> De REST API is beveiligd met behulp van de [standaard-verificatie](http://en.wikipedia.org/wiki/Basic_access_authentication). U moet altijd aanvragen maken met behulp van HTTPS om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server.

1. Gebruik de volgende opdracht uit om te bevestigen dat u verbinding met uw cluster HDInsight maken kunt vanaf een opdrachtregel:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    U moet verschijnt een bericht als volgt uit:

        {"status":"ok","version":"v1"}

    De parameters in deze opdracht gebruikt zijn er als volgt uit:

    * **-u**: dit is de gebruikersnaam en wachtwoord gebruikt om te verifiëren van de aanvraag
    * **-G**: geeft aan dat dit een GET-aanvraag

    Het begin van de URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen.

2. Als u wilt een taak MapReduce stuurt, gebruikt u de volgende opdracht:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    Het einde van de URI (/ mapreduce/oppervlak) worden vermeld WebHCat dat dit verzoek om een taak MapReduce van een klasse in een oppervlak-bestand wordt gestart. De parameters in deze opdracht gebruikt zijn er als volgt uit:

    * **-d**: `-G` niet wordt gebruikt, zodat het verzoek standaard de methode POST. `-d`Hiermee geeft u de gegevenswaarden die worden verzonden met het verzoek.

        * **User.name**: de gebruiker van wie de opdracht wordt uitgevoerd
        * **oppervlak**: de locatie van het oppervlak-bestand met de klas om te worden uitgevoerd
        * **klasse**: de klasse die de logica MapReduce bevat
        * **argument**: de argumenten worden doorgegeven aan de taak MapReduce. in dit geval, het invoer tekstbestand en de adreslijst die worden gebruikt voor de uitvoer

    Een taak-ID die kan worden gebruikt om te controleren van de status van de taak moet worden geretourneerd door deze opdracht:

        {"id":"job_1415651640909_0026"}

3. Als u wilt controleren van de status van de taak, gebruikt u de volgende opdracht uit. De **taak-id** vervangen door de waarde als resultaat gegeven in de vorige stap. Als de retourwaarde is bijvoorbeeld `{"id":"job_1415651640909_0026"}`, en vervolgens de taak-id zou `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Als de taak voltooid is, wordt de status worden 'geslaagd'.

    > [AZURE.NOTE] Deze aanvraag krul geeft als resultaat een JSON-document met informatie over de taak. jq wordt gebruikt om op te halen alleen de waarde van de status.

4. Wanneer de status van de taak heeft gewijzigd in **is voltooid**, kunt u de resultaten van de taak ophalen van Azure-blobopslag. De `statusdir` parameter die wordt doorgegeven aan de query bevat de locatie van het uitvoerbestand; in dit geval **wasbs: / / / voorbeeld/krul**. Dit adres slaat de uitvoer van de taak in de map **voorbeeld/krul** in de standaard-opslag container door uw cluster HDInsight gebruikt.

U kunt de lijst en deze bestanden downloaden met behulp van de [Azure CLI](../xplat-cli-install.md). Als u bijvoorbeeld aan lijst met bestanden in het **voorbeeld/krul**, gebruik de volgende opdracht uit:

    azure storage blob list <container-name> example/curl

Een bestand te downloaden gebruik het volgende:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Moet u de opslagaccountnaam waarin de blob met behulp van de `-a` en `-k` parameters of set de **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS\_sleutel** omgevingsvariabelen. Zie [het uploaden van gegevens met HDInsight](hdinsight-upload-data.md) voor meer informatie.

##<a id="summary"></a>Overzicht

Zoals u in dit document, kunt u onbewerkte HTTP-aanvraag om te worden uitgevoerd, controleren en de resultaten van component taken bekijken in uw cluster HDInsight.

Voor meer informatie over de REST-interface die in dit artikel wordt gebruikt, raadpleegt u [WebHCat verwijzing](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce taken in HDInsight:

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)
