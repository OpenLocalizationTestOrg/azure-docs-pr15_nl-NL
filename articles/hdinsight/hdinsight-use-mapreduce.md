<properties
   pageTitle="MapReduce met Hadoop op HDInsight | Microsoft Azure"
   description="Informatie over het uitvoeren van MapReduce taken op Hadoop in HDInsight clusters. U kunt eenvoudige word tellen bewerking geïmplementeerd als een taak Java MapReduce uitvoeren."
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

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>MapReduce in Hadoop op HDInsight gebruiken

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In dit artikel leert u hoe MapReduce taken uitvoeren op Hadoop in HDInsight clusters. We uitvoeren eenvoudige word tellen bewerking geïmplementeerd als een taak Java MapReduce.

##<a id="whatis"></a>Wat is MapReduce?

Hadoop MapReduce is een kader software voor het schrijven van taken die grote hoeveelheden gegevens verwerken. Invoergegevens opgesplitst onafhankelijke stukken, die u vervolgens op de knooppunten in uw cluster parallel worden verwerkt. Een taak MapReduce bestaat uit twee functies:

* **Toewijzing**: invoergegevens verbruikt, analyseert deze (meestal met filter- en sorteerbewerkingen) en genereert tupels (sleutel-waardeparen)
* **Reducer**: verbruikt tupels dat door de toewijzing en wordt een samenvatting uitgevoerd waarmee maakt u een kleinere, gecombineerde resultaat van de toewijzing-gegevens

Een voorbeeld van eenvoudige word count MapReduce taak wordt geïllustreerd in het volgende diagram:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

De uitvoer van deze taak is een telling van het aantal keren dat elk woord opgetreden in de tekst die is geanalyseerd.

* De toewijzing wordt elke regel van de invoer tekst als invoer en deze opgesplitst in woorden. Deze genereert een sleutelwaarde paar telkens wanneer een woord van het woord plaatsvindt wordt gevolgd door een 1. De uitvoer is gesorteerd voordat deze naar reducer verzonden.

* De reducer de som van de afzonderlijke aantallen voor elk woord en genereert een paar één sleutelwaarde die het woord gevolgd door de som van de vermeldingen bevat.

MapReduce kan worden geïmplementeerd op een aantal verschillende talen. Java is de meest voorkomende implementatie en wordt gebruikt voor gedemonstreerd in dit document.

### <a name="hadoop-streaming"></a>Hadoop Streaming

Talen of kaders die zijn gebaseerd op Java en de Java Virtual Machine (bijvoorbeeld Scalding of trapsgewijs,) kan direct als een taak MapReduce, vergelijkbaar met een Java-toepassing worden uitgevoerd. Anderen, moeten zoals C# of Python of zelfstandige uitvoerbare bestanden, gebruiken Hadoop streaming.

Hadoop streaming communiceert met de toewijzing en reducer via STDIN en STDOUT - de-toewijzing reducer gegevens van een regel per keer uit STDIN lezen en schrijven van de uitvoer naar STDOUT. Elke regel lezen of dat door de toewijzing en reducer moet zijn in de opmaak van de combinatie van een sleutel/waarde, gescheiden door een tabblad charaacter:

    [key]/t[value]

Zie [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html)voor meer informatie.

Zie de volgende onderwerpen voor voorbeelden van het gebruik van Hadoop streaming met HDInsight:

* [Ontwikkel Python MapReduce taken](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>Over de voorbeeldgegevens

In dit voorbeeld voor voorbeeldgegevens, gebruikt u de notitieblokken van Leonardo Da Vinci, die zijn opgegeven als een tekstdocument in uw cluster HDInsight.

De voorbeeldgegevens wordt opgeslagen in Azure-blobopslag, waarin HDInsight wordt gebruikt als het standaardbestandssysteem voor Hadoop clusters. HDInsight hebben toegang tot bestanden die zijn opgeslagen in Blob storage met behulp van het voorvoegsel **wasb** . Voor toegang tot het bestand sample.log, gebruikt u bijvoorbeeld de volgende syntaxis:

    wasbs:///example/data/gutenberg/davinci.txt

Omdat Azure-blobopslag de standaard-opslag voor HDInsight is, kunt u het bestand ook openen via **/example/data/gutenberg/davinci.txt**.

> [AZURE.NOTE] In de vorige syntaxis **wasbs: / / /** wordt gebruikt voor toegang tot bestanden die zijn opgeslagen in de container standaard opslagruimte voor uw cluster HDInsight. Als u extra opslagruimte accounts opgegeven wanneer u uw cluster deze is ingericht en u wilt toegang tot bestanden die zijn opgeslagen in deze accounts, kunt u de gegevens kunt openen door het opgeven van het accountadres container naam en opslag. Bijvoorbeeld **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a id="job"></a>Informatie over het voorbeeld MapReduce

De taak MapReduce die wordt gebruikt in dit voorbeeld bevindt zich op **wasbs://example/jars/hadoop-mapreduce-examples.jar**en deze wordt geleverd bij uw cluster HDInsight. Dit document bevat een voorbeeld van de word count die u voor **davinci.txt**wordt uitgevoerd.

> [AZURE.NOTE] Klik op HDInsight 2.1 kolomgroepen is de bestandslocatie **wasbs:///example/jars/hadoop-examples.jar**.

Voor een verwijzing naar is de volgende de Java-code voor de taak word tellen MapReduce:

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

      public static class TokenizerMapper
           extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
          StringTokenizer itr = new StringTokenizer(value.toString());
          while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
          }
        }
      }

      public static class IntSumReducer
           extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
                           ) throws IOException, InterruptedException {
          int sum = 0;
          for (IntWritable val : values) {
            sum += val.get();
          }
          result.set(sum);
          context.write(key, result);
        }
      }

      public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
          System.err.println("Usage: wordcount <in> <out>");
          System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
      }
    }

Zie voor instructies voor het schrijven van uw eigen taak MapReduce, [ontwikkelen Java MapReduce-programma's voor HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md).

##<a id="run"></a>De MapReduce uitvoeren

HDInsight kunt HiveQL taken uitvoeren met behulp van verschillende methoden. Gebruik van de volgende tabel om te bepalen welke methode is geschikt voor u en voert u de koppeling voor stapsgewijze instructies.

| **Gebruik deze opdracht**...                                                    | **.. .Zie werkwijze**                                       | .. .door dit **cluster besturingssysteem** | .. .from dit **client-besturingssysteem** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | Gebruik de opdracht Hadoop via **SSH**                  | Linux                                     | Linux, Unix, Mac OS X of Windows        |
| [Omslaan](hdinsight-hadoop-use-mapreduce-curl.md)                     | De taak op afstand verstuurt met behulp van de **REST**               | Linux of Windows                          | Linux, Unix, Mac OS X of Windows        |
| [Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) | De taak op afstand verstuurt met **Windows PowerShell** | Linux of Windows                          | Windows                                  |
| [Extern bureaublad](hdinsight-hadoop-use-mapreduce-remote-desktop)    | Gebruik de opdracht Hadoop via **Extern bureaublad**       | Windows                                   | Windows                                  |

##<a id="nextsteps"></a>Volgende stappen

Maar MapReduce krachtige diagnostische mogelijkheden biedt, u kunt werken lastige in outmodel iets. Er zijn verschillende Java gebaseerde kaders die het gemakkelijker maken om te definiëren MapReduce-toepassingen, evenals technologieën zoals varken en component, waarmee een eenvoudigere manier om te werken met gegevens in HDInsight. Meer informatie raadpleegt u de volgende artikelen:

* [Ontwikkel Java MapReduce-programma's voor HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Ontwikkel Python streaming MapReduce-programma's voor HDInsight](hdinsight-hadoop-streaming-python.md)

* [Gebruiker MapReduce taken met Apache Hadoop op HDInsight ontwikkelen](hdinsight-hadoop-mapreduce-scalding.md)

* [Component gebruiken met HDInsight][hdinsight-use-hive]

* [Varken met HDInsight gebruiken][hdinsight-use-pig]

* [Voorbeelden van de HDInsight uitvoeren][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
