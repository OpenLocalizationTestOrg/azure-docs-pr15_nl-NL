<properties
   pageTitle="Hadoop varken gebruiken in HDInsight | Microsoft Azure"
   description="Informatie over het gebruiken van varken met Hadoop op HDInsight."
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
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>Varken met Hadoop op HDInsight gebruiken

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Apache varken](http://pig.apache.org/) is een platform voor het maken van programma's voor Hadoop met behulp van een procedurele taal *varken Latijns*genoemd. Varken is een alternatief voor Java voor *MapReduce* oplossingen te maken en deze is opgenomen in Azure HDInsight.

In dit artikel leert u hoe u varken met HDInsight kunt gebruiken.

##<a id="why"></a>Waarom varken gebruiken?

Een van de uitdagingen van het verwerken van gegevens met behulp van MapReduce in Hadoop implementeert uw logica verwerking met behulp van alleen een kaart en een functie verkleinen. Voor complexe verwerking, hoeft u vaak te verwerking splitsen in meerdere MapReduce bewerkingen die zijn samengevoegd tot het gewenste resultaat.

In plaats van dat u alleen tekens gebruiken en functies beperken, kunt varken u verwerking definiëren als een reeks transformaties die de gegevens doorloopt tot en met om de gewenste uitvoer te produceren.

De taal varken Latijns kunt u de gegevensstroom uit onbewerkte invoer door een of meer transformaties, om de gewenste uitvoer te produceren beschrijven. Varken Latijns-programma's volgen deze algemene patroon:

- **Laden**: gegevens worden bewerkt in het bestandssysteem lezen
- **Transformeren**: de gegevens bewerken
- **Dump of store**: gegevens uitvoeren naar het scherm of opslaan om deze verwerking

Varken Latijns ondersteunt ook gebruiker gedefinieerde functies (UDF), waarmee u aan te roepen externe onderdelen die logica die is moeilijk te model in varken Latijns implementeren.

Zie voor meer informatie over varken Latijns [varken Latijns verwijzing handmatig 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) en [varken Latijns verwijzing handmatig 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Raadpleeg de volgende documenten voor een voorbeeld van het gebruik van UDF's met varken:

* [Gebruik DataFu met varken in HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu is een verzameling handig UDF's door Apache onderhouden

* [Python gebruiken met varken en component in HDInsight](hdinsight-python.md)

* [Gebruik C# met component en varken in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>Over de voorbeeldgegevens

Dit voorbeeld wordt een voorbeeldbestand *log4j* , die is opgeslagen op **/example/data/sample.log** in uw blob storage container. Elke log in het bestand bestaat uit een lijn van velden met een `[LOG LEVEL]` veld toe aan het weergeven van het type en de ernst, bijvoorbeeld:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

In het vorige voorbeeld is het met logboekniveau fout.

> [AZURE.NOTE] U kunt ook een bestand log4j genereren met behulp van het hulpmiddel [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) logboekregistratie en vervolgens naar dat bestand uploaden naar uw blob. Zie [Gegevens uploaden naar HDInsight](hdinsight-upload-data.md) voor instructies. Zie voor meer informatie over hoe BLOB's in de opslagruimte van de Azure HDInsight machtigingen gebruikt, [Gebruik Azure-blobopslag met HDInsight](hdinsight-hadoop-use-blob-storage.md).

De voorbeeldgegevens wordt opgeslagen in Azure-blobopslag, waarin HDInsight wordt gebruikt als het standaardbestandssysteem voor Hadoop clusters. HDInsight hebben toegang tot bestanden die zijn opgeslagen in BLOB's met behulp van het voorvoegsel **wasb** . Voor toegang tot het bestand sample.log, gebruikt u bijvoorbeeld de volgende syntaxis:

    wasbs:///example/data/sample.log

Omdat WASB de standaard-opslag voor HDInsight is, kunt u het bestand ook openen via **/example/data/sample.log** uit varken Latijns.

> [AZURE.NOTE] De syntaxis van de, **wasbs: / / /**, wordt gebruikt voor toegang tot bestanden die zijn opgeslagen in de container standaard opslagruimte voor uw cluster HDInsight. Als u extra opslagruimte accounts opgegeven wanneer u uw cluster deze is ingericht en u wilt toegang tot bestanden die zijn opgeslagen in deze accounts, opent u de gegevens door het opgeven van de container en opslag account-mailadres, bijvoorbeeld: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a id="job"></a>Over de taak van de steekproef

De volgende varken Latijns-taak laadt het bestand **sample.log** uit de standaard-opslag voor uw cluster HDInsight. En vervolgens een reeks transformaties die resulteren in een telling van het aantal keren dat elk logboekniveau opgetreden in de invoergegevens uitvoeren. De resultaten zijn in een teksteditor weergeven in STDOUT.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

De volgende afbeelding ziet u een overzicht van wat elke transformatie tot de gegevens betekent.

![Grafische weergave van de transformaties][image-hdi-pig-data-transformation]

##<a id="run"></a>De taak varken Latijns uitvoeren

HDInsight kunt varken Latijns taken uitvoeren met behulp van verschillende methoden. Gebruik van de volgende tabel om te bepalen welke methode is geschikt voor u en voert u de koppeling voor stapsgewijze instructies.

| **Gebruik deze optie** als u wilt dat...                                   | .. .an **interactieve** shell | ... **batchbestand** | .. .door dit **cluster besturingssysteem** | .. .from dit **client-besturingssysteem** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X of Windows        |
| [Omslaan](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux of Windows                          | Linux, Unix, Mac OS X of Windows        |
| [.NET-SDK voor Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux of Windows                          | Windows (voor nu)                        |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux of Windows                          | Windows                                  |
| [Extern bureaublad](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | Windows                                   | Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Varken taken op Azure HDInsight uitgevoerd met lokale SQL Server Integration Services

U kunt ook SQL Server Integration Services (SSIS) gebruiken om een taak varken te voeren. Het Azure-functiepakket voor SSIS biedt de volgende onderdelen die met varken taken op HDInsight werken.


- [Azure HDInsight varken taak][pigtask]
- [Azure abonnement Connection Manager][connectionmanager]


Meer informatie over het Azure-functiepakket voor SSIS [hier][ssispack].


##<a id="nextsteps"></a>Volgende stappen

U hebt geleerd hoe u varken met HDInsight, gebruikt u de volgende koppelingen naar andere manieren om te werken met Azure HDInsight verkennen.

* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
* [Component gebruiken met HDInsight][hdinsight-use-hive]
* [Sqoop gebruiken met HDInsight](hdinsight-use-sqoop.md)
* [Oozie gebruiken met HDInsight](hdinsight-use-oozie.md)
* [Gebruik MapReduce taken met HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png
