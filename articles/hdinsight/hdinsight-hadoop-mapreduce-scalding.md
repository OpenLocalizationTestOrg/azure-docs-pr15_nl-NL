<properties
 pageTitle="Taken van de gebruiker MapReduce met Maven ontwikkelen | Microsoft Azure"
 description="Informatie over het gebruik van Maven kunt maken van een project MapReduce gebruiker, en vervolgens te implementeren en de taak een Hadoop op HDInsight cluster wordt uitgevoerd."
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
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>Gebruiker MapReduce taken met Apache Hadoop op HDInsight ontwikkelen

Gebruiker is een Scala-bibliotheek waarin kunt u heel gemakkelijk Hadoop MapReduce taken maken. Deze optie biedt een beknopt syntaxis, evenals de naadloze integratie met Scala.

In dit document, leert u hoe u Maven gebruiken om te maken van een eenvoudige word tellen MapReduce taak in Scalding geschreven. Vervolgens leert u hoe u kunt implementeren en uitvoeren van deze taak op een cluster HDInsight.

## <a name="prerequisites"></a>Vereisten voor

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Hadoop op HDInsight cluster op basis van een Windows- of Linux**. Zie [inrichten Linux gebaseerde Hadoop op HDInsight](hdinsight-hadoop-provision-linux-clusters.md) of [inrichten Windows gebaseerde Hadoop op HDInsight](hdinsight-provision-clusters.md) voor meer informatie.

* **[Maven](http://maven.apache.org/)**

* **[Java-platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 of hoger**

## <a name="create-and-build-the-project"></a>Maken en te bouwen van het project

1. Gebruik de volgende opdracht uit om een nieuwe Maven project te maken:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    Deze opdracht maakt u een nieuwe map met de naam **scaldingwordcount**, en de steiger voor een toepassing Scala maken.

2. In de map **scaldingwordcount** , opent u het bestand **pom.xml** en de inhoud vervangen door het volgende:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
                    <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                        <exclude>META-INF/*.SF</exclude>
                        <exclude>META-INF/*.DSA</exclude>
                        <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Dit bestand wordt beschreven voor het project, afhankelijkheden en Plug-ins. Hier volgen de belangrijke items:

    * **maven.compiler.Source** en **maven.compiler.target**: Hiermee stelt u de Java-versie voor dit project

    * **opslagplaatsen**: de opslagplaatsen met afhankelijkheidsbestanden die worden gebruikt door dit project

    * **gebruiker core_2.11** en **hadoop core**: dit project afhankelijk is van zowel Scalding als Hadoop core-pakketten

    * **maven-scala-invoegtoepassing**: Plug scala toepassingen compileren

    * **maven-arceren-invoegtoepassing**: Plug maken (fat) potten gearceerd. Deze invoegtoepassing is van toepassing filters en transformaties; specificially:

        * **filters**: de filters zijn toegepast wijzigen de metagegevens in het oppervlak-bestand wordt geleverd bij. Om te voorkomen dat handtekeningen uitzonderingen gedurende runtime, verschillende handtekeningbestanden die geleverd bij afhankelijkheden worden kunnen uitgesloten.

        * **executions**: de configuratie van fase execution geeft de klasse **com.twitter.scalding.Tool** als de belangrijkste klasse voor het pakket. Zonder deze moet u com.twitter.scalding.Tool, alsmede de klasse met de toepassingslogica wanneer u de uitvoert met de opdracht hadoop opgeven.

3. Verwijder de map **src/testen** , terwijl u niet met tests in dit voorbeeld maken zal.

4. Open het bestand **src/main/scala/com/microsoft/example/App.scala** en de inhoud vervangen door het volgende:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    Dit implementeert een eenvoudige word aantal taak.

5. Opslaan en sluit de bestanden.

6. Gebruik de volgende opdracht uit de adreslijst **scaldingwordcount** voor bouwen en pakket opslaan van de toepassing:

        mvn package

    Zodra deze taak is voltooid, kan het pakket met de toepassing WordCount worden gevonden op **target/scaldingwordcount-1.0-SNAPSHOT.jar**.

## <a name="run-the-job-on-a-linux-based-cluster"></a>De taak een cluster Linux gebaseerde wordt uitgevoerd

> [AZURE.NOTE] De volgende stappen SSH en de opdracht Hadoop gebruiken. Zie voor andere methoden voor het uitvoeren van taken MapReduce, [Gebruik MapReduce in Hadoop op HDInsight](hdinsight-use-mapreduce.md).

1. Gebruik de volgende opdracht uit het pakket uploaden naar uw cluster HDInsight:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    Hierdoor wordt de bestanden uit het lokale systeem naar het hoofd knooppunt gekopieerd.

    > [AZURE.NOTE] Als u een wachtwoord beveiligen van uw SSH-account gebruikt, wordt u gevraagd het wachtwoord. Als u een sleutel SSH gebruikt, moet u mogelijk gebruiken de `-i` parameter en het pad naar de persoonlijke sleutel. Bijvoorbeeld:`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Met de volgende opdracht verbinding maken met het hoofd knooppunt:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Als u een wachtwoord beveiligen van uw SSH-account gebruikt, wordt u gevraagd het wachtwoord. Als u een sleutel SSH gebruikt, moet u mogelijk gebruiken de `-i` parameter en het pad naar de persoonlijke sleutel. Bijvoorbeeld:`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Zodra u verbinding hebt met het hoofd knooppunt, gebruikt u de volgende opdracht uit de word aantal taak uit te voeren

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    Hiermee voert de WordCount klasse die u eerder hebt geïmplementeerd. `--hdfs`Hiermee geeft u de taak HDFS gebruiken. `--input`Hiermee geeft u het bestand ingevoerde tekst, terwijl `--output` Hiermee geeft u de uitvoerlocatie.

4. Nadat de taak is voltooid, gebruikt u de volgende handelingen uit om weer te geven van de uitvoer.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Hierdoor worden gegevens van de volgende strekking weergegeven:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>De taak een cluster op basis van Windows wordt uitgevoerd

De volgende stappen uit de Windows PowerShell gebruiken. Zie voor andere methoden voor het uitvoeren van taken MapReduce, [Gebruik MapReduce in Hadoop op HDInsight](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Start Azure PowerShell en aanmelden bij uw Azure-account. De opdracht na het opgeven van uw referenties, geeft als resultaat informatie over uw account.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Als u meerdere abonnementen hebt, vindt u de abonnements-id die u wilt gebruiken voor implementatie.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] U kunt `Get-AzureRMSubscription` om te zien van alle abonnementen die is gekoppeld aan uw account, waaronder de abonnements-Id voor elke batch.

4. Gebruik het volgende script om te uploaden en de taak WordCount uitvoeren. Vervang `CLUSTERNAME` met de naam van uw HDInsight cluster en zorg ervoor dat `$fileToUpload` is het juiste pad naar het bestand __scaldingwordcount-1.0-SNAPSHOT.jar__ .

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Wanneer u het script uitvoert, wordt u gevraagd om in te voeren de admin-gebruikersnaam en wachtwoord voor uw cluster HDInsight. Eventuele fouten die optreden terwijl de taak wordt uitgevoerd worden geregistreerd bij de console.
     
6. Zodra de taak is voltooid, wordt de uitvoer worden gedownload naar de __uitvoer.txt__ in de huidige map. Gebruik de volgende opdracht uit om de resultaten weer te geven.

        cat output.txt

    Het bestand moet de volgende strekking waarden bevatten:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u met Scalding MapReduce taken voor HDInsight maken, gebruikt u de volgende koppelingen naar andere manieren om te werken met Azure HDInsight verkennen.

* [Component gebruiken met HDInsight](hdinsight-use-hive.md)

* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)

* [Gebruik MapReduce taken met HDInsight](hdinsight-use-mapreduce.md)
