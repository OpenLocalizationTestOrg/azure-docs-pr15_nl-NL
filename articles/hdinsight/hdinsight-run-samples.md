<properties
    pageTitle="In de voorbeelden Hadoop worden uitgevoerd in HDInsight | Microsoft Azure"
    description="Aan de slag met de Azure HDInsight-service met de voorbeelden. PowerShell-scripts die MapReduce programma's op gegevens clusters uitvoeren gebruiken."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>

#<a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Voorbeelden van Hadoop MapReduce uitvoeren in Windows gebaseerde HDInsight

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Een reeks voorbeelden worden gegeven om optimaal gebruik begonnen actieve MapReduce taken op Hadoop clusters met Azure HDInsight. Deze voorbeelden zijn er beschikbaar op elk van de HDInsight beheerde clusters die u maakt. Deze voorbeelden uitvoeren maakt u vertrouwd met het gebruik van Azure PowerShell-cmdlets voor het uitvoeren van taken op Hadoop clusters.

- [**Aantal woorden**][hdinsight-sample-wordcount]: telt de word-vermeldingen in een tekstbestand.
- [**C#-streaming woorden tellen**][hdinsight-sample-csharp-streaming]: telt de word-vermeldingen in een tekstbestand met de streaming Hadoop-interface.
- [**Schatting van de pi**][hdinsight-sample-pi-estimator]: gebruik een statistische (standaardcoupontermijn Monte Carlo) methode voor het schatten van de waarde van pi.
- [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: een algemene GraySort uitvoert op een 10 GB-bestand met behulp van HDInsight. Er zijn drie taken uitvoeren: Teragen om de gegevens, Terasort om de gegevens te sorteren en Teravalidate om te bevestigen dat de gegevens correct zijn gesorteerd te genereren.

>[AZURE.NOTE] De broncode vindt u in de bijlage. 

Veel aanvullende documentatie bestaat op het web voor Hadoop-gerelateerde technologieën, zoals Java gebaseerde MapReduce hoeft te programmeren en streaming en documentatie over de cmdlets die worden gebruikt in Windows PowerShell uitvoeren van scripts. Zie voor meer informatie over deze bronnen:

- [Ontwikkel Java MapReduce-programma's voor Hadoop in HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
- [Hadoop-taken in HDInsight indienen](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Inleiding tot Azure HDInsight][hdinsight-introduction]

Tegenwoordig een groot aantal personen Kies component en varken MapReduce.  Zie voor meer informatie:

- [Component in HDInsight gebruiken](hdinsight-use-hive.md)
- [Varken in HDInsight gebruiken](hdinsight-use-pig.md)
 
**Vereisten**:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **een cluster HDInsight**. Zie voor instructies over de verschillende manieren waarin dergelijke clusters kunnen worden gemaakt, [Hadoop maken clusters in HDInsight](hdinsight-provision-clusters.md).
- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="hdinsight-sample-wordcount"></a>Aantal - Java voor Word 

Als u wilt een project MapReduce indient, moet u eerst de definitie van een MapReduce maken. In de taakdefinitie van de geeft u het bestand MapReduce oppervlak en de locatie van het bestand oppervlak, dat wil **wasbs:///example/jars/hadoop-mapreduce-examples.jar**, naam van de klasse en de argumenten zeggen.  Het programma dat wordcount MapReduce heeft twee argumenten: het bronbestand dat wordt gebruikt om te tellen woorden en de locatie voor uitvoer.

De broncode vindt u in de [bijlage A](#apendix-a---the-word-count-MapReduce-program-in-java).

Voor de procedure van het ontwikkelen van een Java MapReduce-programma, Zie - [ontwikkelen Java MapReduce programma's voor Hadoop in HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
 
**Een taak van word telling MapReduce indienen**

1. Open **Windows filter wissen**. Zie voor instructies [installeren en configureren van Azure PowerShell][powershell-install-configure].
2. Plak de volgende PowerShell-script:

        $subscriptionName = "<Azure Subscription Name>"
        $resourceGroupName = "<Resource Group Name>"
        $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name
        
        Select-AzureRmSubscription -SubscriptionName $subscriptionName
        
        # Define the MapReduce job
        $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "wordcount" `
                                    -Arguments "wasbs:///example/data/gutenberg/davinci.txt", "wasbs:///example/data/WordCountOutput1"
        
        # Submit the job and wait for job completion
        $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:" 
        $mrJob = Start-AzureRmHDInsightJob `
                            -ResourceGroupName $resourceGroupName `
                            -ClusterName $clusterName `
                            -HttpCredential $cred `
                            -JobDefinition $mrJobDefinition 
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -JobId $mrJob.JobId 
        
        # Get the job output
        $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
        $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
        $defaultStorageContainer = $cluster.DefaultStorageContainer
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -DefaultStorageAccountName $defaultStorageAccount `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultContainer $defaultStorageContainer  `
            -JobId $mrJob.JobId `
            -DisplayOutputType StandardError

        # Download the job output to the workstation
        $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
        Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force
        
        # Display the output file
        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"

    De taak MapReduce levert een bestand met de naam *deel-r-00000*, waarin de woorden en de aantallen. Het script gebruikt de opdracht **findstr** voor een overzicht van alle woorden die *'daar'*bevat.

3. Stelt u de eerste 3 variabelen en het script uitvoeren.

## <a name="hdinsight-sample-csharp-streaming"></a>Aantal - C#-streaming voor Word

Hadoop biedt een streaming API naar MapReduce, waarmee u schrijven kaart en functies in andere talen dan Java te verkleinen.

> [AZURE.NOTE] De stappen in deze zelfstudie gelden alleen voor Windows gebaseerde HDInsight clusters. Zie voor een voorbeeld van streaming voor HDInsight Linux gebaseerde clusters, [Python ontwikkelen streaming programma's voor HDInsight](hdinsight-hadoop-streaming-python.md).

In het voorbeeld wordt de toewijzing en de reducer zijn uitvoerbare bestanden die de invoer van [stdin] gelezen[ stdin-stdout-stderr] (per regel) en de uitvoer naar [stdout]verzenden[stdin-stdout-stderr]. Het programma telt alle woorden in de tekst.

Wanneer een uitvoerbaar bestand voor **mappers**is opgegeven, wordt elke taak toewijzing het uitvoerbare bestand als een afzonderlijk proces gestart wanneer de toewijzing wordt geïnitialiseerd. Als de taak toewijzing wordt uitgevoerd, zet u de invoer in lijnen en -feeds van de regels aan de [stdin] [ stdin-stdout-stderr] van het proces.

De toewijzing verzamelt ondertussen de regel-georiënteerd uitvoer van de stdout van het proces. Elke regel wordt geconverteerd naar een combinatie sleutelwaarde /, die worden verzameld als de uitvoer van de toewijzing. Standaard het voorvoegsel van een regel naar het eerste teken van tabblad is de sleutel en de rest van de regel (met uitzondering van het tabteken) is de waarde. Als er geen tabteken in de regel, hele regel beschouwd als de sleutel en de waarde null is.

Wanneer een uitvoerbaar bestand voor **verkleiningstoestellen**is opgegeven, wordt elke taak reducer het uitvoerbare bestand als een afzonderlijk proces gestart wanneer de reducer wordt geïnitialiseerd. Als de taak reducer wordt uitgevoerd, deze de paren sleutel/invoerwaarden naar lijnen converteert en deze-feeds de regels aan de [stdin] [ stdin-stdout-stderr] van het proces.

Ondertussen de reducer de uitvoer lijn-georiënteerd verzamelt van [stdout] [ stdin-stdout-stderr] van het proces. Elke regel wordt geconverteerd naar een paar sleutelwaarde /, die worden verzameld als de uitvoer van de reducer. Standaard het voorvoegsel van een regel naar het eerste teken van tabblad is de sleutel en de rest van de regel (exclusief het tabteken) is de waarde.

Zie voor meer informatie over de interface Hadoop-Streaming, [Hadoop Streaming] [hadoop-streaming].

**Om in te dienen een streaming word tellen taak C#**

- Volg de procedure in [woorden tellen - Java](#word-count-java)en de definitie van de functie vervangen door het volgende:

        $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                                -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                                -Mapper "cat.exe" `
                                -Reducer "wc.exe" `
                                -InputPath "/example/data/gutenberg/davinci.txt" `
                                -OutputPath "/example/data/StreamingOutput/wc.txt"  


    Het uitvoerbestand zijn:
    
        example/data/StreamingOutput/wc.txt/part-00000      
                                
## <a name="hdinsight-sample-pi-estimator"></a>Schatting van PI

De schatting van de pi gebruikmaakt van een statistische (standaardcoupontermijn Monte Carlo) methode voor het schatten van de waarde van pi. Punten geplaatst willekeurig binnen een eenheid vierkante ook vallen binnen een cirkel die zijn aangebracht binnen die vierkant met een kans gelijk is aan het gebied van de cirkel, pi/4. De waarde van pi kan worden geschat met de waarde van 4R, waarbij R de verhouding van het aantal punten die zijn binnen de cirkel op het totale aantal punten die binnen het kwadraat is. Des te groter het voorbeeld van punten gebruikt, hoe beter de schatting is.

Het script is opgegeven voor dit voorbeeld dient een Hadoop oppervlak taak en is ingesteld op Omhoog uitvoeren met een waarde 16 kaarten, elk van die is vereist voor het berekenen van 10 miljoen voorbeeld wordt verwezen door de betreffende parameterwaarden. Deze parameterwaarden kunnen worden gewijzigd om te verbeteren de geschatte waarde van pi. Voor verwijzing zijn de eerste 10 decimalen van pi 3.1415926535.

**Een taak van de schatting van pi indienen**

- Volg de procedure in [woorden tellen - Java](#word-count-java)en de definitie van de functie vervangen door het volgende:

        $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "pi" `
                                    -Arguments "16", "10000000"

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB Graysort

In dit voorbeeld wordt een bescheiden 10GB van gegevens, zodat deze relatief snel kan worden uitgevoerd. De MapReduce toepassingen ontwikkeld door Owen O'Malley en Arun Murthy die de jaarlijkse algemene ("daytona") terabyte sorteren vergelijkende in 2009 met een tarief van 0.578 TB/min (100 TB in 173 minuten gewonnen) wordt gebruikt. Zie voor meer informatie over dit en andere sorteren benchmarks, de site [Sortbenchmark](http://sortbenchmark.org/) .

In dit voorbeeld wordt drie sets met MapReduce-programma's gebruikt:

1. **TeraGen** is een MapReduce-programma dat u gebruiken kunt om te genereren van de rijen met gegevens wilt sorteren.
2. **TeraSort** voorbeelden van de invoergegevens en MapReduce gebruikt de gegevens in een totale volgorde sorteren. TeraSort is een standaard sortering van MapReduce-functies, behalve voor een aangepaste partitioner die gebruikmaakt van een gesorteerde lijst van N-1 partijen toetsen die de belangrijkste bereik voor elke verkleinen definiëren. Met name, alle toetsen zoals voorbeeld [i-1] < = sleutel < voorbeeld [i] zijn verzonden naar i verkleinen. Dit garanties dat de uitvoer van i verkleinen worden alle kleiner dan de uitvoer van verkleinen i + 1.
3. **TeraValidate** is een MapReduce-programma dat is gevalideerd met dat de uitvoer globaal is gesorteerd. Een toewijzing per bestand wordt gemaakt in de adreslijst uitvoer en elke map zorgt ervoor dat elk sleutel kleiner dan of gelijk is aan de vorige taak is. De functie kaart genereert ook records van de eerste en laatste sleutels van elk bestand en de functie verkleinen zorgt ervoor dat de eerste toets van bestand i groter dan de laatste sleutel van bestand i-1 is. Sprake is van problemen worden gerapporteerd als een uitvoer van het verkleinen met de toetsen die onjuist zijn.

De invoer- en uitvoerbereik-indeling die wordt gebruikt door alle drie de toepassingen, gelezen en geschreven van de tekstbestanden in de juiste indeling. De uitvoer van het verkleinen heeft replicatie is ingesteld op 1, in plaats van de standaard 3, omdat de prijsvraag vergelijkende niet vereist is voor dat de uitvoergegevens kunnen meerdere knooppunten worden gerepliceerd.

Drie taken is vereist door de steekproef, elke overeenkomt met een van de MapReduce-programma's die worden beschreven in de inleiding:

1. Genereer de gegevens voor het sorteren door de **TeraGen** MapReduce taak uit te voeren.
2. De gegevens sorteren door de **TeraSort** MapReduce taak uit te voeren.
3. Bevestig dat de gegevens correct zijn gesorteerd door de **TeraValidate** MapReduce taak uit te voeren.

**De taken indienen**

- Volg de procedure in [woorden tellen - Java](#word-count-java)en gebruik de volgende definities van timerserviceopdrachten:

    $teragen = nieuw AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - klassenaam "teragen" '-argumenten "-Dmapred.map.tasks=50", "100000000", "/ voorbeeld / / 10 GB-sorteren-invoer van gegevens"
    
    $terasort = nieuw AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - klassenaam "terasort" '-argumenten "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/ voorbeeld / / 10 GB-sorteren-invoer van gegevens", "/ voorbeeld/gegevens/10 GB-sorteren-uitvoer"
    
    $teravalidate = nieuw AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - klassenaam "teravalidate" '-argumenten "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/ voorbeeld/gegevens/10 GB-sorteren-uitvoer", "/ voorbeeld/gegevens/10 GB-sorteren-valideren"


##<a name="next-steps"></a>Volgende stappen 

In dit artikel en de artikelen in elk van de voorbeelden, hebt u geleerd hoe uitvoeren in de voorbeelden wordt geleverd bij de clusters HDInsight via Azure PowerShell. Zie de volgende onderwerpen voor zelfstudies over het gebruik van varken, component en MapReduce met HDInsight:

* [Aan de slag met Hadoop met onderdeel in HDInsight te analyseren mobiele telefoon gebruiken][hdinsight-get-started]
* [Varken met Hadoop op HDInsight gebruiken][hdinsight-use-pig]
* [Component gebruiken met Hadoop op HDInsight][hdinsight-use-hive]
* [Hadoop-taken in HDInsight indienen] [hdinsight-submit-jobs]
* [Azure HDInsight SDK-documentatie][hdinsight-sdk-documentation]
* [Fouten opsporen in Hadoop in HDInsight: foutberichten] [hdinsight-errors]


## <a name="appendix-a---the-word-count-source-code"></a>Bijlage A - de Word telling-broncode

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


## <a name="appendix-b---the-word-count-streaming-source-code"></a>Bijlage B - het aantal woorden streaming-broncode

Het programma MapReduce gebruikt de toepassing cat.exe als een toewijzing-interface om te streamen van de tekst in de console en de toepassing wc.exe als de interface verkleinen om het aantal woorden die afkomstig zijn uit een document te tellen. De toewijzing en de reducer tekens, per regel, de standaard invoer stream (stdin) lezen en schrijven naar de standaard uitvoer stream (stdout).

    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



De toewijzing-code in het bestand cat.cs gebruikt een [StreamReader] [ streamreader] object te lezen van de tekens van de binnenkomende stream naar de console, die schrijft vervolgens de stream naar de standaard uitvoer stream met de statische [Console.Writeline] [ console-writeline] methode.


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


De code reducer in het bestand wc.cs gebruikt een [StreamReader] [ streamreader] object te lezen tekens uit de standaard invoer stream die zijn uitvoer door de cat.exe-toewijzing. Als dit de tekens met de [Console.Writeline] leest[ console-writeline] methode telt deze de woorden door te tellen, spaties en einde van regel tekens aan het einde van elk woord. Deze vervolgens het totaal schrijft naar de standaard uitvoer stream met de [Console.Writeline] [ console-writeline] methode.





## <a name="appendix-c---the-pi-estimator-source-code"></a>Bijlage C - de Pi schatting-broncode

De schatting van de pi Java-code die met de functies van de toewijzing en reducer is beschikbaar voor controle hieronder. Het programma toewijzing genereert een opgegeven aantal punten geplaatst willekeurig binnen een vierkant per eenheid en vervolgens telt het aantal die punten die binnen de cirkel zijn. Het programma reducer worden verwezen geteld op basis van de mappers bij elkaar opgeteld en vervolgens maakt een schatting van de waarde van pi van de te000126961 4R, waarbij R de verhouding van het aantal punten in de berekening opgenomen binnen de cirkel op het totale aantal punten die binnen het kwadraat is.

    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }
     
## <a name="appendix-d---the-10gb-graysort-source-code"></a>Bijlage D - de 10gb graysort-broncode

De code voor het programma TeraSort MapReduce wordt voor controle in deze sectie gepresenteerd.


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }
    }














 




[hdinsight-errors]: hdinsight-debug-jobs.md

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md


[powershell-install-configure]: ../powershell-install-configure.md

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
