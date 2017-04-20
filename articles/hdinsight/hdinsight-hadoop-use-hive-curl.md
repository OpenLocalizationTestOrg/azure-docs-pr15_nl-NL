<properties
   pageTitle="Hadoop-component gebruiken met krul in HDInsight | Microsoft Azure"
   description="Leer hoe u extern indienen varken taken met behulp van krul HDInsight."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>Component query's uitvoeren met Hadoop in HDInsight met krul

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In dit document leert u hoe u met krul component query's uitvoeren op een Hadoop op Azure HDInsight cluster.

Krul wordt gebruikt om te laten zien hoe u met HDInsight kunt werken met behulp van onbewerkte HTTP-verzoeken om te worden uitgevoerd, controleren en de resultaten van query's component ophalen. Dit werkt met behulp van de WebHCat REST API (voorheen bekend als Templeton) die door uw cluster HDInsight.

> [AZURE.NOTE] Als u al bekend bent met het gebruik van Linux gebaseerde Hadoop-servers, maar nog niet eerder met HDInsight, raadpleegt u [Wat u moet weten over Hadoop op Linux gebaseerde HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

* Een Hadoop op HDInsight cluster (Linux of op basis van Windows)

* [Omslaan](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Component query's uitvoeren met behulp van krul

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

    Het begin van de URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, wordt niet hetzelfde zijn voor alle aanvragen. Het pad, **/status/status**, geeft aan dat het verzoek om terug te keren status WebHCat (ook wel bekend als Templeton) voor de server. U kunt ook de versie van component aanvragen met behulp van de volgende opdracht uit:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    Dit moet het resultaat van de volgende strekking:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Gebruik de volgende handelingen uit om een nieuwe tabel met de naam **log4jLogs**te maken:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    De parameters in deze opdracht gebruikt zijn er als volgt uit:

    * **-d** - Aangezien `-G` niet wordt gebruikt, het verzoek standaard de methode POST. `-d`Hiermee geeft u de gegevenswaarden die worden verzonden met het verzoek.

        * **user.name** - de gebruiker die de opdracht wordt uitgevoerd.

        * **uitvoeren** - instructies voor het HiveQL uit te voeren.

        * **statusdir** - de map die de status voor deze taak wilt opslaan.

    Deze instructies heeft de volgende acties uitvoeren:

    * **DROP TABLE** - wordt de tabel en het gegevensbestand verwijderd als de tabel al bestaat.

    * **Externe tabel maken** - Hiermee maakt u een nieuwe 'externe' tabel in component. Alleen de definitie van de tabel opslaan externe tabellen in component. De gegevens is links op de oorspronkelijke locatie.

        > [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens die moeten worden bijgewerkt door een externe bron, zoals een geautomatiseerde gegevens upload-proces of door een andere MapReduce bewerking, maar altijd wilt component query's voor het gebruik van de meest recente gegevens verwachten.
        >
        > Weghalen van een externe tabel bevat **niet** verwijderen de gegevens, alleen de definitie van de tabel.

    * **Rij-indeling** : Hiermee wordt aan component hoe de gegevens worden opgemaakt. In dit geval worden de velden in elke log gescheiden door een spatie.

    * **Opgeslagen als TEXTFILE locatie** - worden vermeld component waar de gegevens is opgeslagen (de map met de voorbeeldgegevens /), en dat deze wordt opgeslagen als tekst.

    * **Selecteer** - Hiermee selecteert u een telling van alle rijen waarbij kolom **t4** de waarde **[fout bevat]**. Dit moet een waarde van **3** retourneren als er drie rijen die deze waarde bevatten.

    > [AZURE.NOTE] U ziet dat de afstand tussen HiveQL-instructies worden vervangen door de `+` teken gebruikt in combinatie met krul. Tussen aanhalingstekens waarden met een spatie, zoals het scheidingsteken, niet mag worden vervangen door `+`.

    * **INPUT__FILE__NAME zoals '% 25.log'** - deze limieten voor de zoekopdracht tot alleen bestanden die eindigen op gebruiken. log. Als dit niet aanwezig is, probeert component te zoeken in alle bestanden in deze map en submappen, inclusief bestanden die niet overeenkomen met de kolom schema gedefinieerd voor deze tabel.

    > [AZURE.NOTE] Let erop dat % 25 is de URL codering formulier van %, zodat de werkelijke voorwaarde is `like '%.log'`. De % heeft met URL moet worden gecodeerd, zoals deze wordt behandeld als een speciaal teken in URL's.

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

6. Gebruik de volgende instructies om een nieuwe 'interne' tabel met de naam **errorLogs**te maken:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Deze instructies heeft de volgende acties uitvoeren:

    * **Maken van tabel als niet aanwezig** - Hiermee maakt u een tabel, als deze nog niet bestaat. Aangezien het **externe** trefwoord niet wordt gebruikt, is dit is een interne tabel, die is opgeslagen in de component datawarehouse en volledig wordt beheerd door component.

        > [AZURE.NOTE] In tegenstelling tot de externe tabellen, worden een interne tabel weghalen verwijderd als u ook de onderliggende gegevens.

    * De gegevens **Die zijn opgeslagen als ORC** - opgeslagen in geoptimaliseerd rij kolommen (ORC)-indeling. Dit is een zeer geoptimaliseerde en efficiënt indeling voor het opslaan van de component data.
    * **Invoegen OVERSCHRIJVEN... Selecteer** : Hiermee selecteert u rijen uit de tabel **log4jLogs** die **[fout]**bevatten en vervolgens de gegevens in de tabel **errorLogs** invoegen.
    * **Selecteer** - Hiermee selecteert u alle rijen van de nieuwe **errorLogs** -tabel.

7. Gebruik de taak-ID die is geretourneerd naar Controleer de status van de taak. Wanneer deze is voltooid, via Azure CLI voor Mac, Linux en Windows zoals wordt beschreven eerder downloaden en de resultaten bekijken. De uitvoer moet bevatten drie lijnen, die allemaal **[fout]**bevatten.


##<a id="summary"></a>Overzicht

Zoals u in dit document, kunt u een onbewerkte HTTP-verzoek om te worden uitgevoerd, controleren en de resultaten van component taken weergeven op uw cluster HDInsight.

Voor meer informatie over de REST-interface in dit artikel gebruikt, raadpleegt u <a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">WebHCat verwijzing</a>.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over component met HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u Tez met onderdeel gebruikt, raadpleegt u de volgende documenten voor foutopsporing informatie:

* [De gebruikersinterface Tez op HDInsight op basis van Windows gebruiken](hdinsight-debug-tez-ui.md)

* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](hdinsight-debug-ambari-tez-view.md)

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


