<properties
   pageTitle="Hadoop Sqoop gebruiken met krul in HDInsight | Microsoft Azure"
   description="Leer hoe u extern indienen Sqoop taken met behulp van krul HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Sqoop taken uitvoeren met Hadoop in HDInsight met krul

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van krul Sqoop taken uitvoeren op een Hadoop-cluster in HDInsight.

Krul wordt gebruikt om te laten zien hoe u met HDInsight kunt werken met behulp van onbewerkte HTTP-verzoeken om te worden uitgevoerd, controleren en de resultaten van Sqoop taken ophalen. Dit werkt met behulp van de WebHCat REST API (voorheen bekend als Templeton) die door uw cluster HDInsight.

> [AZURE.NOTE] Als u al bekend bent met het gebruik van Linux gebaseerde Hadoop-servers, maar nog niet eerder met HDInsight, raadpleegt u [informatie over het gebruik van HDInsight op Linux](hdinsight-hadoop-linux-information.md).

##<a name="prerequisites"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

* Een Hadoop op HDInsight cluster (Linux of op basis van Windows)

* [Omslaan](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Sqoop taken indienen via krul

> [AZURE.NOTE] Wanneer u krul of een andere REST-communicatie met WebHCat, moet u de aanvragen verifiëren doordat de gebruikersnaam en wachtwoord voor de beheerder van de cluster HDInsight. U moet de naam van het cluster ook gebruiken als onderdeel van de id URI (Uniform Resource) gebruikt voor het versturen van aanvragen op de server.
>
> Voor de opdrachten in deze sectie, vervangen door de gebruiker om te verifiëren met het cluster **gebruikersnaam** en **wachtwoord** vervangen door het wachtwoord voor het gebruikersaccount. **CLUSTERNAAM** vervangen door de naam van uw cluster.
>
> De REST API is via [Basisverificatie](http://en.wikipedia.org/wiki/Basic_access_authentication)beveiligd. U moet altijd aanvragen maken met behulp van Secure HTTP (HTTPS) om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server.

1. Gebruik de volgende opdracht uit om te bevestigen dat u verbinding met uw cluster HDInsight maken kunt vanaf de opdrachtregel:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    U moet verschijnt een bericht als volgt uit:

        {"status":"ok","version":"v1"}

    De parameters in deze opdracht gebruikt zijn er als volgt uit:

    * **-u** - de gebruikersnaam en wachtwoord gebruikt om te verifiëren van het verzoek.
    * **-G** - geeft aan dat dit een GET-aanvraag.

    Het begin van de URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, wordt niet hetzelfde zijn voor alle aanvragen. Het pad, **/status/status**, geeft aan dat het verzoek om terug te keren status WebHCat (ook wel bekend als Templeton) voor de server. 

2. Gebruik de volgende handelingen uit om in te dienen van een taak sqoop:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    De parameters in deze opdracht gebruikt zijn er als volgt uit:

    * **-d** - Aangezien `-G` niet wordt gebruikt, het verzoek standaard de methode POST. `-d`Hiermee geeft u de gegevenswaarden die worden verzonden met het verzoek.

        * **user.name** - de gebruiker die de opdracht wordt uitgevoerd.

        * **opdracht** - het Sqoop opdracht wordt uitgevoerd.

        * **statusdir** - de map die de status voor deze taak wilt opslaan.

    Een taak-ID die kan worden gebruikt om te controleren van de status van de taak moet worden geretourneerd door deze opdracht.

        {"id":"job_1415651640909_0026"}

3. Als u wilt controleren van de status van de taak, gebruikt u de volgende opdracht uit. **Taak-id** vervangen door de waarde als resultaat gegeven in de vorige stap. Als de retourwaarde is bijvoorbeeld `{"id":"job_1415651640909_0026"}`, en vervolgens de **taak-id** zou `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Als de taak is voltooid, zijn de status **is voltooid**.

    > [AZURE.NOTE] Deze aanvraag krul geeft als resultaat een notatie voor JavaScript-Object (JSON)-document met informatie over de taak. jq wordt gebruikt om op te halen alleen de waarde van de status.

4. Nadat u de status van de taak heeft gewijzigd in **is voltooid**, kunt u de resultaten van de taak ophalen van Azure-blobopslag. De `statusdir` parameter doorgegeven aan de query bevat de locatie van het uitvoerbestand; in dit geval **wasbs: / / / voorbeeld/krul**. Dit adres worden de resultaten van de taak in de map **voorbeeld/krul** op de standaard-opslag container door uw cluster HDInsight gebruikt.

    U kunt de lijst en deze bestanden downloaden met behulp van de [Azure CLI](../xplat-cli-install.md). Als u bijvoorbeeld aan lijst met bestanden in **voorbeeld/krul**, gebruik de volgende opdracht uit:

        azure storage blob list <container-name> example/curl

    Een bestand te downloaden gebruik het volgende:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] U moet de opslagaccountnaam waarin de blob met behulp van opgeven de `-a` en `-k` parameters of set de **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS\_sleutel** omgevingsvariabelen. Zie < een href = "hdinsight-upload-data.md" target = "_blank" voor meer informatie.

##<a name="limitations"></a>Beperkingen

* Bulksgewijs exporteren - HDInsight met Linux gebaseerde, de Sqoop verbindingslijn die wordt gebruikt om gegevens te exporteren naar Microsoft SQL Server of Azure SQL-Database ondersteunt momenteel niet bulksgewijs invoegen.

* Batchen - met Linux gebaseerde HDInsight, bij gebruik van de `-batch` overschakelt bij het uitvoeren van wordt ingevoegd, Sqoop meerdere wordt ingevoegd in plaats van de invoegbewerkingen batchen wordt uitgevoerd.

##<a name="summary"></a>Overzicht

Zoals u in dit document, kunt u een onbewerkte HTTP-verzoek om te worden uitgevoerd, controleren en de resultaten van Sqoop taken weergeven op uw cluster HDInsight.

Zie voor meer informatie over de REST-interface in dit artikel gebruikt, de <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">handleiding Sqoop REST API</a>.

##<a name="next-steps"></a>Volgende stappen

Voor algemene informatie over component met HDInsight:

* [Sqoop gebruiken met Hadoop op HDInsight](hdinsight-use-sqoop.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


