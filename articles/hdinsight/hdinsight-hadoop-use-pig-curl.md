<properties
   pageTitle="Gebruik Hadoop varken met krul in HDInsight | Microsoft Azure"
   description="Informatie over het gebruik van krul varken Latijns taken uitvoeren op een Hadoop-cluster in Azure HDInsight."
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

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Varken taken uitvoeren met Hadoop op HDInsight met behulp van krul

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

In dit document leert u hoe u met krul varken Latijns taken uitvoeren op een cluster Azure HDInsight. De programmeertaal varken Latijns kunt u beschrijven transformaties die zijn toegepast op de invoergegevens om de gewenste uitvoer te produceren.

Krul wordt gebruikt om te laten zien hoe u met HDInsight kunt werken met behulp van onbewerkte HTTP-verzoeken om te worden uitgevoerd, controleren en de resultaten van varken taken ophalen. Dit werkt met behulp van de WebHCat REST API (voorheen bekend als Templeton) die is verstrekt door uw cluster HDInsight.

> [AZURE.NOTE] Als u al bekend bent met het gebruik van Linux gebaseerde Hadoop-servers, maar nog niet eerder met HDInsight, raadpleegt u [Linux gebaseerde HDInsight Tips](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

* Een cluster Azure HDInsight (Hadoop op HDInsight) (Linux gebaseerde of op basis van Windows)

* [Omslaan](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Varken taken uitvoeren via krul

> [AZURE.NOTE] Wanneer u krul of een andere REST-communicatie met WebHCat, moet u de aanvragen verifiëren doordat de beheerder-gebruikersnaam en wachtwoord voor het cluster HDInsight. U moet de naam van het cluster ook gebruiken als onderdeel van de id URI (Uniform Resource) die wordt gebruikt voor het versturen van aanvragen op de server.
>
> Voor de opdrachten in deze sectie, vervangen door de gebruiker om te verifiëren met het cluster **gebruikersnaam** en **wachtwoord** vervangen door het wachtwoord voor het gebruikersaccount. **CLUSTERNAAM** vervangen door de naam van uw cluster.
>
> De REST API is via een [standaard-verificatie](http://en.wikipedia.org/wiki/Basic_access_authentication)beveiligd. U moet altijd aanvragen maken met behulp van Secure HTTP (HTTPS) om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server.

1. Gebruik de volgende opdracht uit om te bevestigen dat u verbinding met uw cluster HDInsight maken kunt vanaf de opdrachtregel:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    U moet verschijnt een bericht als volgt uit:

        {"status":"ok","version":"v1"}

    De parameters in deze opdracht gebruikt zijn er als volgt uit:

    * **-u**: de gebruikersnaam en wachtwoord gebruikt om te verifiëren van de aanvraag
    * **-G**: geeft aan dat dit een GET-aanvraag

    Het begin van de URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, wordt niet hetzelfde zijn voor alle aanvragen. Het pad, **/status/status**, geeft aan dat het verzoek om terug te keren de status van WebHCat (ook wel bekend als Templeton) voor de server.

2. Gebruik de volgende code aan een taak varken Latijns naar het cluster stuurt:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    De parameters in deze opdracht gebruikt zijn er als volgt uit:

    * **-d**: omdat `-G` niet wordt gebruikt, het verzoek standaard de methode POST. `-d`Hiermee geeft u de gegevenswaarden die worden verzonden met het verzoek.

        * **User.name**: de gebruiker van wie de opdracht wordt uitgevoerd
        * **uitvoeren**: het varken Latijns-instructies uitvoeren
        * **statusdir**: de map die de status voor deze taak naar worden geschreven

    > [AZURE.NOTE] U ziet dat de spaties in varken Latijns-instructies worden vervangen door de `+` teken gebruikt in combinatie met krul.

    Een taak-ID die kan worden gebruikt om de status van de taak, bijvoorbeeld controleren moet worden geretourneerd door deze opdracht:

        {"id":"job_1415651640909_0026"}

3. Als u wilt controleren van de status van de taak, gebruikt u de volgende opdracht uit. **Taak-id** vervangen door de waarde als resultaat gegeven in de vorige stap. Als de retourwaarde is bijvoorbeeld `{"id":"job_1415651640909_0026"}`, en vervolgens de **taak-id** zou `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Als de taak is voltooid, zijn de status **is voltooid**.

    > [AZURE.NOTE] Deze aanvraag krul geeft als resultaat een notatie voor JavaScript-Object (JSON)-document met informatie over de taak en jq wordt gebruikt om op te halen alleen de waarde van de status.

##<a id="results"></a>Resultaten weergeven

Wanneer de status van de taak heeft gewijzigd in **is voltooid**, kunt u de resultaten van de taak ophalen van Azure-blobopslag. De `statusdir` parameter doorgegeven aan de query bevat de locatie van het uitvoerbestand; in dit geval **wasbs: / / / voorbeeld/pigcurl**. Dit adres slaat de uitvoer van de taak in de map **voorbeeld/pigcurl** in de standaard-opslag container door uw cluster HDInsight gebruikt.

U kunt de lijst en deze bestanden downloaden met behulp van de [Azure CLI](../xplat-cli-install.md). Als u bijvoorbeeld aan lijst met bestanden in **voorbeeld/pigcurl**, gebruik de volgende opdracht uit:

    azure storage blob list <container-name> example/pigcurl

Een bestand te downloaden gebruik het volgende:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Moet u de opslagaccountnaam waarin de blob met behulp van de `-a` en `-k` parameters of set de **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS\_sleutel** omgevingsvariabelen.

##<a id="summary"></a>Overzicht

Zoals u in dit document, kunt u een onbewerkte HTTP-verzoek om te worden uitgevoerd, controleren en de resultaten van varken taken weergeven op uw cluster HDInsight.

Voor meer informatie over de REST-interface in dit artikel gebruikt, raadpleegt u [WebHCat verwijzing](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over varken op HDInsight:

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
