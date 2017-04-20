<properties
    pageTitle="Ontwikkel Java MapReduce-programma's voor Linux gebaseerde HDInsight | Microsoft Azure"
    description="Informatie over het ontwikkelen van Java MapReduce-programma's en deze implementeert met Linux gebaseerde HDInsight."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>Ontwikkel Java MapReduce-programma's voor Hadoop op HDInsight Linux

Deze documenten begeleidt u bij het gebruik van Apache Maven voor een MapReduce-toepassing bouwen en implementeren en uitvoeren op een Hadoop Linux gebaseerde op HDInsight cluster.

##<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 of hoger (of een equivalent, zoals OpenJDK)

- [Apache Maven](http://maven.apache.org/)

- **Een Azure-abonnement**

- **Azure CLI**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

De volgende omgevingsvariabelen kunnen worden ingesteld tijdens de installatie van Java en de JDK. U moet het selectievakje die ze bestaat en dat ze de juiste waarden voor uw systeem bevatten.

* **JAVA_HOME** - verwijzen naar de map waarin de Java runtime-omgeving (JRE) is geïnstalleerd. Bijvoorbeeld in een OS X-, Unix- of Linux-systeem, moet er een waarde die vergelijkbaar is met `/usr/lib/jvm/java-7-oracle`. Klik in Windows heeft deze een waarde die vergelijkbaar is met`c:\Program Files (x86)\Java\jre1.7`

* **Pad** - moet bevatten de volgende paden:

    * **JAVA_HOME** (of het overeenkomstige pad)

    * **JAVA_HOME\bin** (of het overeenkomstige pad)

    * De map waarin Maven is geïnstalleerd

##<a name="create-a-new-maven-project"></a>Een nieuw Maven project maken

1. Wijzigen van een sessie, of de opdrachtregel in uw ontwikkelomgeving, mappen naar de locatie die u wilt opslaan dit project.

3. Gebruik de opdracht __mvn__ , die is geïnstalleerd met Maven, om te genereren van de steiger voor het project.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Hiermee wordt een nieuwe map maken in de huidige map, met de naam die is opgegeven met de parameter __artifactID__ (**wordcountjava** in dit voorbeeld.) Deze map bevat de volgende items:

    * __pom.xml__ - [Project Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) met de gegevens en configuratie details voor het samenstellen van het project.

    * __src__ - de map waarin de map __Hoofdgegeven/java/organigram/apache/hadoop/voorbeelden__ , waar u de toepassing wordt creëert.

3. Verwijder het bestand __src/test/java/org/apache/hadoop/examples/apptest.java__ , zoals het wordt niet in dit voorbeeld worden gebruikt.

##<a name="add-dependencies"></a>Afhankelijkheden toevoegen

1. Bewerk het bestand __pom.xml__ en voeg de volgende binnen de `<dependencies>` sectie:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    Hiermee wordt aangegeven Maven dat het project is vereist voor de bibliotheken (vermeld binnen &lt;artifactId\>) met een specifieke versie (vermeld binnen &lt;versie\>). Tijdens het compileren, wordt dit gedownload van de standaard Maven opslagplaats. U kunt de [Maven opslagplaats zoeken](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) om meer weer te gebruiken.

    De `<scope>provided</scope>` worden vermeld Maven dat deze afhankelijkheden niet moeten worden verpakt met de toepassing, zoals ze worden geleverd door het cluster HDInsight tijdens runtime.

2. De volgende naar het bestand __pom.xml__ toevoegen. Dit moet binnen de `<project>...</project>` tags in het bestand. bijvoorbeeld, tussen `</dependencies>` en `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                </execution>
              </executions>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    De invoegtoepassing voor eerste Hiermee configureert u de [Invoegtoepassing voor Maven arceren](http://maven.apache.org/plugins/maven-shade-plugin/), die wordt gebruikt voor het maken van een uberjar (ook wel een fatjar genoemd), waarin afhankelijkheden vereist door de toepassing. Daarnaast wordt dupliceren van licenties binnen het pakket oppervlak problemen op sommige systemen veroorzaken kan voorkomen.

    De tweede invoegtoepassing configureert het compileerprogramma Maven, die wordt gebruikt voor het instellen van de versie van Java vereist door deze toepassing naar de versie die worden gebruikt op het cluster HDInsight.

3. Sla het bestand __pom.xml__ .

##<a name="create-the-mapreduce-application"></a>De toepassing MapReduce maken

1. Ga naar de map __wordcountjava/src/Hoofdgegeven/java/organigram/apache/hadoop/voorbeelden__ en geef het bestand __App.java__ __WordCount.java__.

2. Open het bestand __WordCount.java__ in een teksteditor en de inhoud vervangen door het volgende:

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

    U ziet de pakketnaam **org.apache.hadoop.examples** en de naam van de klasse is **WordCount**. U kunt deze namen worden gebruikt wanneer u de taak MapReduce verstuurt.

3. Sla het bestand.

##<a name="build-the-application"></a>De toepassing bouwen

1. Wijzigen naar de map __wordcountjava__ , als u nog niet er.

2. Gebruik de volgende opdracht uit om te maken van een oppervlak-bestand met de toepassing:

        mvn clean package

    Dit wordt de onderdelen van een vorige opbouwen schonen, eventuele afhankelijkheden die nog niet zijn geïnstalleerd, maken en de toepassing inpakken downloaden.

3. Nadat de opdracht is voltooid, wordt de ____ wordcountjava/doelmap een bestand met de naam __wordcountjava-1.0-SNAPSHOT.jar__bevatten.

    > [AZURE.NOTE] Het bestand __wordcountjava-1.0-SNAPSHOT.jar__ is een uberjar, waarin niet alleen de WordCount taak, maar ook afhankelijkheden die de taak moet worden gedurende runtime.


##<a id="upload"></a>Het oppervlak uploaden

Gebruik de volgende opdracht uit het oppervlak-bestand uploaden naar de headnode HDInsight:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Hierdoor wordt de bestanden uit het lokale systeem naar het hoofd knooppunt gekopieerd.

> [AZURE.NOTE] Als u een wachtwoord beveiligen van uw SSH-account gebruikt, wordt u gevraagd het wachtwoord. Als u een sleutel SSH gebruikt, moet u mogelijk gebruiken de `-i` parameter en het pad naar de persoonlijke sleutel. Bijvoorbeeld `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="run"></a>De taak MapReduce uitvoeren

1. Verbinding maken met HDInsight via SSH zoals is beschreven in de volgende artikelen:

    - [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Gebruik de volgende opdracht uit de toepassing MapReduce uit te voeren in de sessie SSH:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    Hiermee wordt de toepassing WordCount MapReduce gebruiken om te tellen van de woorden in het bestand davinci.txt en de resultaten op te slaan __wasbs: / / / voorbeeld/gegevens/wordcountout__. De invoer bestand en de uitvoer worden naar de standaard-opslag voor het cluster opgeslagen.

3. Zodra de taak is voltooid, gebruikt u de volgende handelingen uit om de resultaten te bekijken:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    U kunt een lijst met woorden en aantallen, met de volgende strekking waarden moet ontvangen:

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>Volgende stappen

In dit document, hebt u geleerd hoe een taak Java MapReduce ontwikkelen. Zie het volgende documenten controleren op andere manieren om te werken met HDInsight.

- [Component gebruiken met HDInsight][hdinsight-use-hive]
- [Varken met HDInsight gebruiken][hdinsight-use-pig]
- [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

Zie ook het [Java Developer Center](https://azure.microsoft.com/develop/java/)voor meer informatie.

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

