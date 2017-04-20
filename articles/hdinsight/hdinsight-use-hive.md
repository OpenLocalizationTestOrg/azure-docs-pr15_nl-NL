<properties
    pageTitle="Informatie over component en het gebruik van HiveQL | Microsoft Azure"
    description="Meer informatie over Apache component en hoe u dit product gebruiken met Hadoop in HDInsight. Kies hoe u uw taak component uitvoeren en HiveQL gebruiken om te analyseren van een steekproef Apache log4j-bestand."
    keywords="hiveql, wat is component"
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
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Component en HiveQL gebruiken met Hadoop in HDInsight te analyseren van een steekproef Apache log4j-bestand

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


In deze zelfstudie hebt u meer informatie over het gebruik van Apache component in Hadoop op HDInsight en kiezen hoe u uw component taak uit te voeren. Ook leert u over HiveQL en hoe u een voorbeeldbestand van Apache log4j analyseren.

##<a id="why"></a>Wat is de component en waarom deze gebruiken?
[Apache component](http://hive.apache.org/) is een systeem voor het BTW-records van gegevens voor Hadoop, waarmee gegevens samenvatting, query's uitvoeren en analyseren van gegevens met behulp van HiveQL (een querytaal dat vergelijkbaar is met SQL). Component kan worden gebruikt om interactief uw gegevens verkennen of herbruikbare batch taken verwerkt.

Component kunt u de projectstructuur van het op grotendeels ongestructureerde gegevens. Nadat u de structuur definieert, kunt u component om query's die gegevens zonder kennis van Java of MapReduce. **HiveQL** (de component query language), kunt u met de instructies die vergelijkbaar met T-SQL zijn-query's schrijft.

Component begrijpt het werken met gestructureerde en gedeeltelijk gestructureerde gegevens, zoals tekstbestanden waarin de velden worden gescheiden door specifieke tekens. Component ondersteunt ook aangepaste **serialisatiefunctie/deserializers (SerDe)** voor complexe of onregelmatig gestructureerde gegevens. Zie voor meer informatie [het gebruik van een aangepaste JSON SerDe met HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Door gebruiker gedefinieerde functies (UDF)

Component kan ook worden verlengd door **gebruiker gedefinieerde functies (UDF)**. Een UDF kunt u functionaliteit of -logica die is niet gemakkelijk gebaseerd implementeren in HiveQL. Zie de volgende onderwerpen voor een voorbeeld van het gebruik van UDF's met onderdeel:

* [Gebruik een Java-gebruiker gedefinieerde functie met onderdeel](hdinsight-hadoop-hive-java-udf.md)

* [Gebruik van Python met component en varken in HDInsight](hdinsight-python.md)

* [Gebruik C# met component en varken in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Het toevoegen van een aangepaste component UDF met HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Voorbeeld van aangepaste component UDF datum/tijdnotaties converteren naar component tijdstempel](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Interne tabellen tegenover externe tabellen component

Er zijn enkele dingen die u moet weten over de component interne tabel en de externe tabel:

- De opdracht **Tabel maken** Hiermee maakt u een interne tabel. Het gegevensbestand moet zich bevinden in de standaardcontainer.
- De opdracht **Tabel maken** het gegevensbestand verplaatst naar de /hive/warehouse/<TableName> map.
- De opdracht **Externe tabel maken** Hiermee maakt u een externe tabel. Het gegevensbestand kan zich bevinden buiten de standaardcontainer.
- De opdracht **Externe tabel maken** het gegevensbestand niet wordt verplaatst.
- De opdracht **Maken met externe tabel** mag niet alle mappen op de locatie. Dit is de reden waarom de zelfstudie een kopie van het bestand sample.log wordt.

Zie voor meer informatie [HDInsight: component interne en externe tabellen inleiding][cindygross-hive-tables].


##<a id="data"></a>Over de voorbeeldgegevens, een Apache log4j-bestand

Dit voorbeeld wordt een voorbeeldbestand *log4j* , die is opgeslagen op **/example/data/sample.log** in uw blob storage container. Elke log in het bestand bestaat uit een lijn van velden met een `[LOG LEVEL]` veld toe aan het weergeven van het type en de ernst, bijvoorbeeld:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

In het vorige voorbeeld is het met logboekniveau fout.

> [AZURE.NOTE] U kunt ook een bestand log4j genereren met behulp van het hulpmiddel [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) logboekregistratie en vervolgens dat bestand uploaden naar de container blob. Zie [Gegevens uploaden naar HDInsight](hdinsight-upload-data.md) voor instructies. Zie voor meer informatie over hoe Azure-blobopslag wordt gebruikt met HDInsight, [Gebruik Azure-blobopslag met HDInsight](hdinsight-hadoop-use-blob-storage.md).

De voorbeeldgegevens wordt opgeslagen in Azure-blobopslag, waarin HDInsight wordt gebruikt als het standaard-bestandssysteem. HDInsight hebben toegang tot bestanden die zijn opgeslagen in BLOB's met behulp van het voorvoegsel **wasb** . Voor toegang tot het bestand sample.log, gebruikt u bijvoorbeeld de volgende syntaxis:

    wasbs:///example/data/sample.log

Omdat Azure-blobopslag de standaard-opslag voor HDInsight is, kunt u het bestand ook openen via **/example/data/sample.log** uit HiveQL.

> [AZURE.NOTE] De syntaxis van de, **wasbs: / / /**, wordt gebruikt voor toegang tot bestanden die zijn opgeslagen in de container standaard opslagruimte voor uw cluster HDInsight. Als u extra opslagruimte accounts opgegeven wanneer u uw cluster deze is ingericht en u wilt toegang tot bestanden die zijn opgeslagen in deze accounts, opent u de gegevens door het opgeven van de container en opslag account-mailadres, bijvoorbeeld **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a id="job"></a>Voorbeeld taak: projecteren kolommen op gescheiden gegevens

De volgende beweringen voor HiveQL wordt project voor het kolommen op gescheiden gegevens die zijn opgeslagen in de **wasbs: / / / / voorbeeldgegevens** directory:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

In het vorige voorbeeld uitvoeren de beweringen HiveQL de volgende acties:

* __hive.execution.engine=tez; instellen__: Hiermee stelt u de uitvoeringsengine Tez gebruikt. Tez gebruiken in plaats van MapReduce, kan een stijging queryprestaties bieden. Zie voor meer informatie over Tez, de sectie [Gebruik Apache Tez voor verbeterde prestaties](#usetez) .

    > [AZURE.NOTE] Deze instructie is alleen vereist bij gebruik van een cluster HDInsight op basis van Windows. Tez is de standaard execution engine voor Linux gebaseerde HDInsight.

* **DROP TABLE**: Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.
* **Externe tabel maken**: maakt een nieuwe **externe** tabel in component. De definitie van de tabel in externe tabellen alleen worden opgeslagen in component; de gegevens is links in de oorspronkelijke locatie en in de oorspronkelijke indeling.
* **Rij-indeling**: Hiermee wordt aan component hoe de gegevens worden opgemaakt. In dit geval worden de velden in elke log gescheiden door een spatie.
* **Opgeslagen als TEXTFILE locatie**: Hiermee wordt aan Component waar de gegevens is opgeslagen (de map met de voorbeeldgegevens /) en wordt deze opgeslagen als tekst. De gegevens kan worden in één bestand of verdeeld over meerdere bestanden binnen de adreslijst.
* **Selecteer**: Hiermee selecteert u een telling van alle rijen waarvan de kolom **t4** de waarde **[fout bevat]**. Dit moet een waarde van **3** retourneren omdat er zijn drie rijen met deze waarde.
* **INPUT__FILE__NAME zoals '%.log'** - Hiermee wordt aan die we alleen gegevens uit bestanden die eindigen ophalen moet op component. log. Hiermee beperkt u de zoekopdracht naar het bestand sample.log die de gegevens bevat, en behoudt uit het retourneren van gegevens uit andere voorbeeld gegevensbestanden die niet overeenkomen met het schema dat zoals gedefinieerd.

> [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens moeten worden bijgewerkt door een externe bron, zoals een geautomatiseerde gegevens uploadproces, of door een andere MapReduce bewerking verwachten en u wilt dat altijd component query's om de meest recente gegevens te gebruiken.
>
> Weghalen van een externe tabel bevat **niet** de gegevens verwijderd, worden alleen de definitie van de tabel.

Na het maken van de externe tabel, worden de volgende beweringen gebruikt om een **interne** tabel te maken.

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Deze instructies heeft de volgende acties uitvoeren:

* **Maak tabel als niet bestaat**: Hiermee maakt u een tabel, als deze nog niet bestaat. Omdat het **externe** trefwoord niet wordt gebruikt, is dit is een interne tabel, die is opgeslagen in de component datawarehouse en volledig wordt beheerd door component.
* **Opgeslagen als ORC**: de gegevens zijn opgeslagen in de indeling geoptimaliseerd rij kolommen (ORC). Dit is een zeer geoptimaliseerde en efficiënt indeling voor het opslaan van de component data.
* **Invoegen OVERSCHRIJVEN... Selecteer**: Hiermee selecteert u rijen uit de tabel **log4jLogs** **[fout]**bevat en voegt vervolgens de gegevens in de tabel **errorLogs** .

> [AZURE.NOTE] In tegenstelling tot de externe tabellen verwijderd een interne tabel weghalen ook de onderliggende gegevens.

##<a id="usetez"></a>Apache Tez gebruiken voor verbeterde prestaties

[Apache Tez](http://tez.apache.org) is een kader waarmee gegevens intensief toepassingen, zoals component veel efficiënter uitvoeren bij het op schaal. In de meest recente versie van HDInsight, component ondersteunt op Tez uitgevoerd. Tez is standaard voor HDInsight Linux gebaseerde clusters ingeschakeld.

> [AZURE.NOTE] Tez is momenteel standaard uitgeschakeld voor Windows gebaseerde HDInsight clusters en moet zijn ingeschakeld. Om te profiteren van Tez, moet de volgende waarde voor een query component worden ingesteld:
>
> ```set hive.execution.engine=tez;```
>
>Dit kan op basis van de per query worden verzonden door het te plaatsen aan het begin van uw query. U kunt ook deze moet zijn op al dan niet standaard op een cluster door de configuratiewaarde bij het maken van het cluster instellen. U vindt meer informatie in de [Inrichting van HDInsight Clusters](hdinsight-provision-clusters.md).

De [component aan Tez Ontwerpdocumenten](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) bevatten een aantal meer informatie over de opties voor implementatie en afstemmen configuraties.

Hulp bij foutopsporing taken hebt uitgevoerd met Tez, HDInsight biedt de volgende web UI waarmee u kunt de details van Tez taken bekijken:

* [De gebruikersinterface Tez op HDInsight op basis van Windows gebruiken](hdinsight-debug-tez-ui.md)

* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](hdinsight-debug-ambari-tez-view.md)

##<a id="run"></a>Kiezen hoe u de taak HiveQL uit te voeren

HDInsight HiveQL taken met een verscheidenheid aan methoden kan worden uitgevoerd. Gebruik van de volgende tabel om te bepalen welke methode is geschikt voor u en voert u de koppeling voor stapsgewijze instructies.

| **Gebruik deze optie** als u wilt dat...                                                     | .. .an **interactieve** shell | ... **batchbestand** | .. .door dit **cluster besturingssysteem** | .. .from dit **client-besturingssysteem** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Component weergeven](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Elke gewenste (op basis van browser) |
| [De opdracht beeline (vanuit een sessie SSH)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X of Windows        |
| [De opdracht component (vanuit een sessie SSH)](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X of Windows        |
| [Omslaan](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux of Windows                          | Linux, Unix, Mac OS X of Windows        |
| [Query-console](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | Elke gewenste (op basis van browser)                            |
| [HDInsight tools voor Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux of Windows                          | Windows                                  |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux of Windows                          | Windows                                  |
| [Extern bureaublad](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Component taken op Azure HDInsight uitgevoerd met lokale SQL Server Integration Services

U kunt ook SQL Server Integration Services (SSIS) gebruiken om een taak onderdeel te voeren. Het Azure-functiepakket voor SSIS biedt de volgende onderdelen die met component taken op HDInsight werken.


- [Azure HDInsight-component taak][hivetask]
- [Azure abonnement Connection Manager][connectionmanager]


Meer informatie over het Azure-functiepakket voor SSIS [hier][ssispack].


##<a id="nextsteps"></a>Volgende stappen

U hebt geleerd wat component is en hoe u dit product gebruiken met Hadoop in HDInsight, gebruikt u de volgende koppelingen naar andere manieren om te werken met Azure HDInsight verkennen.


- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
- [Varken met HDInsight gebruiken][hdinsight-use-pig]
- [Sqoop gebruiken met HDInsight](hdinsight-use-sqoop.md)
- [Oozie gebruiken met HDInsight](hdinsight-use-oozie.md)
- [Gebruik MapReduce taken met HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
