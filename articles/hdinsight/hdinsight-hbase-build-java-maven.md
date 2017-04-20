<properties
pageTitle="Een Maven met HBase-toepassing bouwen en implementeren naar Windows gebaseerde HDInsight | Microsoft Azure"
description="Informatie over het gebruik van Apache Maven om te maken van een toepassing Java gebaseerde Apache HBase en klik op het dashboard implementeren naar een cluster op basis van Windows Azure HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"
tags="azure-portal"/>

<tags
ms.service="hdinsight"
ms.workload="big-data"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Maven gebruiken om te maken van Java-toepassingen die gebruikmaken van HBase bij op basis van Windows HDInsight (Hadoop)

Informatie over het maken en te bouwen van een toepassing [Apache HBase](http://hbase.apache.org/) in Java met behulp van Apache Maven. Gebruik vervolgens de toepassing met Azure HDInsight (Hadoop).

[Maven](http://maven.apache.org/) is een software-projectmanagement en inzichtelijk hulpmiddel waarmee u software, documentatie en rapporten voor Java projecten maken. In dit artikel leert u hoe gebruiken om u te maken van een eenvoudige Java-toepassing die worden gemaakt, query's, en een tabel HBase op een cluster Azure HDInsight verwijderd.

> [AZURE.NOTE] De stappen in dit document wordt ervan uitgegaan dat u een cluster HDInsight op basis van Windows gebruikt. Zie voor informatie over het gebruik van een cluster Linux gebaseerde HDInsight [Gebruiken Maven Java-toepassingen die gebruikmaken van HBase met Linux gebaseerde HDInsight maken](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>Vereisten

* [Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 of hoger

* [Maven](http://maven.apache.org/)


* [Een Windows gebaseerde HDInsight cluster met HBase](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] De stappen in dit document is getest met HDInsight cluster versies 3,2 en 3.3. De standaardwaarden die beschikbaar zijn in de voorbeelden zijn bedoeld voor een cluster HDInsight 3.3.

##<a name="create-the-project"></a>Het project maken

1. Vanaf de opdrachtregel in uw ontwikkelomgeving, door mappen te wijzigen naar de locatie waar u wilt maken van het project, bijvoorbeeld `cd code\hdinsight`.

2. Gebruik de opdracht __mvn__ , die is geïnstalleerd met Maven, om te genereren van de steiger voor het project.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Deze opdracht maakt een map in de huidige locatie, met de naam die is opgegeven met de parameter __artifactID__ (**hbaseapp** in dit voorbeeld.) Deze map bevat de volgende items:

    * __pom.XML__: de Project-objectmodel ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) bevat informatie en configuratie van informatie voor het samenstellen van het project.

    * __src__: de map waarin de map __main\java\com\microsoft\examples__ , waar u de toepassing wordt creëert.

3. Verwijder het bestand __src\test\java\com\microsoft\examples\apptest.java__ omdat deze niet in dit voorbeeld wordt gebruikt.

##<a name="update-the-project-object-model"></a>Het objectmodel van het Project bijwerken

1. Bewerk het bestand __pom.xml__ en voeg de volgende code binnen de `<dependencies>` sectie:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Dit gedeelte wordt uitgelegd Maven dat het project __hbase-client__ versie __1.1.2__vereist. Tijdens het compileren, wordt deze afhankelijkheid gedownload van de standaard Maven opslagplaats. U kunt de [Maven centrale opslagplaats zoeken](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) voor meer informatie over deze afhankelijkheid.

    > [AZURE.IMPORTANT] Het versienummer moet overeenkomen met de versie van HBase die wordt geleverd bij uw cluster HDInsight. Gebruik de volgende tabel om het juiste versienummer weergegeven.

  	| HDInsight cluster versie | HBase versie moet worden gebruikt |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3.3 | 1.1.2 |

    Zie [Wat zijn de verschillende Hadoop-onderdelen beschikbaar voor communicatie met HDInsight](hdinsight-component-versioning.md)voor meer informatie over HDInsight versies en onderdelen.

2. Als u een cluster HDInsight 3.3 gebruikt, moet u ook de volgende toevoegen de `<dependencies>` sectie:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Deze afhankelijkheid de phoenix core-onderdelen, die worden gebruikt door Hbase versie wordt geladen 1.1.x.

2. Voeg de volgende code naar het bestand __pom.xml__ . In dit gedeelte moet deel uitmaken van de `<project>...</project>` codes in het bestand, bijvoorbeeld tussen `</dependencies>` en `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
              <resource>
                <directory>${basedir}/conf</directory>
                <filtering>false</filtering>
                <includes>
                  <include>hbase-site.xml</include>
                </includes>
              </resource>
            </resources>
          <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
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
          </plugins>
        </build>

    De `<resources>` sectie configureert een resource (__conf\hbase-site.xml__) dat configuratiegegevens voor HBase bevat.

    > [AZURE.NOTE] U kunt ook de waarden van de systeemconfiguratie via programmacode instellen. Zie de opmerkingen in het volgende voor hoe u dit voorbeeld __CreateTable__ .

    Dit `<plugins>` sectie configureert de [Maven compileerprogramma Plug-in](http://maven.apache.org/plugins/maven-compiler-plugin/) en de [Invoegtoepassing voor Maven arceren](http://maven.apache.org/plugins/maven-shade-plugin/). De invoegtoepassing compileerprogramma wordt gebruikt om samen te stellen van de topologie. De invoegtoepassing tint wordt gebruikt om te voorkomen dat licentie dupliceren in het oppervlak-pakket die is ingebouwd door Maven. De reden waarom die dit wordt gebruikt, is dat de dubbele licentiebestanden ertoe leiden dat een fout tijdens de uitvoering op het cluster HDInsight. Met maven-arceren-invoegtoepassing met de `ApacheLicenseResourceTransformer` implementatie voorkomt dat deze fout.

    De invoegtoepassing voor het arceren van een maven ook genereert een uber oppervlak (of fat oppervlak) waarin alle afhankelijkheden vereist door de toepassing.

3. Sla het bestand __pom.xml__ .

4. Maak een nieuwe map met de naam __conf__ in de adreslijst __hbaseapp__ . In de map __conf__ , door een bestand met de naam __hbase-site.xml__te maken. Gebruik de volgende handelingen uit als de inhoud van het bestand:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
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
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Dit bestand wordt gebruikt om te laden van de configuratie HBase voor een cluster HDInsight.

    > [AZURE.NOTE] Dit is een minimale hbase-site.xml-bestand en bevat de absoluut minimale instellingen voor het cluster HDInsight.

3. Sla het bestand __hbase-site.xml__ .

##<a name="create-the-application"></a>De toepassing maken

1. Ga naar de map __hbaseapp\src\main\java\com\microsoft\examples__ en geef het bestand app.java __CreateTable.java__.

2. Open het bestand __CreateTable.java__ en de bestaande inhoud vervangen door de volgende code:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Dit is de klasse __CreateTable__ , die wordt een tabel met de naam __mensen__ maken en deze te vullen met enkele vooraf gedefinieerde gebruikers.

3. Sla het bestand __CreateTable.java__ .

4. Maak een nieuw bestand met de naam __SearchByEmail.java__in de adreslijst __hbaseapp\src\main\java\com\microsoft\examples__ . Gebruik de volgende code als de inhoud van dit bestand:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    De klasse __SearchByEmail__ kan worden gebruikt op query voor rijen aan e-mailadres. Omdat deze een filter reguliere expressies gebruikt, kunt u een tekenreeks of een reguliere expressie bieden, bij gebruik van de klas.

5. Sla het bestand __SearchByEmail.java__ .

6. Maak een nieuw bestand met de naam __DeleteTable.java__in de adreslijst __hbaseapp\src\main\hava\com\microsoft\examples__ . Gebruik de volgende code als de inhoud van dit bestand:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Deze klasse is voor het opschonen van dit voorbeeld door te schakelen en daar neerzetten in de tabel die is gemaakt door de klasse __CreateTable__ .

7. Sla het bestand __DeleteTable.java__ .

##<a name="build-and-package-the-application"></a>Bouwen en pakket opslaan van de toepassing

1. Open een opdrachtprompt en mappen wijzigen in de map __hbaseapp__ .

2. Gebruik de volgende opdracht uit om te maken van een oppervlak-bestand met de toepassing:

        mvn clean package

    Hiermee worden de onderdelen van een vorige opbouwen, downloads van eventuele afhankelijkheden die nog niet zijn geïnstalleerd, klikt u vervolgens genereert en pakketten van de toepassing.

3. Als de opdracht is voltooid, wordt in de map __hbaseapp\target__ een bestand met de naam __hbaseapp-1.0-SNAPSHOT.jar__bevat.

    > [AZURE.NOTE] Het bestand __hbaseapp-1.0-SNAPSHOT.jar__ is een uber oppervlak (ook wel een fat oppervlak, genoemd) waarin alle afhankelijkheden vereist voor het uitvoeren van de toepassing.

##<a name="upload-the-jar-file-and-start-a-job"></a>Het oppervlak-bestand uploaden en een taak starten

Er zijn tal van manieren naar een bestand uploaden naar uw cluster HDInsight, zoals wordt beschreven in het [uploaden van gegevens voor Hadoop-projecten in HDInsight](hdinsight-upload-data.md). De volgende stappen Azure PowerShell gebruiken.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. Na het installeren en configureren van Azure PowerShell, maakt u een nieuw bestand met de naam __hbase-runner.psm1__. Gebruik de volgende handelingen uit als de inhoud van dit bestand:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Dit bestand bevat twee modules:

    * __Toevoegen-HDInsightFile__ - gebruikt voor het uploaden van bestanden naar HDInsight

    * __Start-HBaseExample__ - gebruikt voor het uitvoeren van de klassen eerder hebt gemaakt

2. Sla het bestand __hbase-runner.psm1__ .

3. Een nieuw Azure PowerShell-venster openen, mappen naar de map __hbaseapp__ wijzigen en voer de volgende opdracht.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Wijzig het pad naar de locatie van het __hbase-runner.psm1__ -bestand eerder hebt gemaakt. Hiermee wordt de module voor deze Azure PowerShell-sessie.

2. Gebruik de volgende opdracht uit om de __hbaseapp-1.0-SNAPSHOT.jar__ te uploaden naar uw cluster HDInsight.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    __Hdinsightclustername__ vervangen door de naam van uw cluster HDInsight. De opdracht uploadt de __hbaseapp-1.0-SNAPSHOT.jar__ naar de locatie van het __voorbeeld/potten__ in de primaire opslagruimte voor uw cluster HDInsight.

3. Nadat de bestanden die zijn geüpload, moet u de volgende code gebruiken om een tabel met de __hbaseapp__te maken:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    __Hdinsightclustername__ vervangen door de naam van uw cluster HDInsight.

    Deze opdracht maakt een nieuwe tabel met de naam __mensen__ in uw cluster HDInsight. Deze opdracht weergegeven niet uitvoer in het consolevenster.

2. Als u wilt zoeken naar items in de tabel, gebruikt u de volgende opdracht:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    __Hdinsightclustername__ vervangen door de naam van uw cluster HDInsight.

    Deze opdracht gebruikt de klasse **SearchByEmail** om te zoeken naar alle rijen waarbij het gezin van de kolom __contactinformation__ en de kolom met __e__ , bevat de tekenreeks __contoso.com__. U kunt de volgende resultaten moet ontvangen:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Met __fabrikam.com__ voor de `-emailRegex` waarde als resultaat de gebruikers aan wie __fabrikam.com__ in het veld e-mailbericht een. Aangezien deze zoekopdracht wordt geïmplementeerd met behulp van een gewone filter op basis van een expressie, u kunt ook invoeren reguliere expressies, zoals __^ r__, welke resulteert posten waarbij het e-mailbericht met de letter 'r begint'.

##<a name="delete-the-table"></a>De tabel verwijderen

Wanneer u klaar bent met het voorbeeld, gebruikt u de volgende opdracht uit de Azure PowerShell-sessie om de tabel van de __personen__ die in dit voorbeeld worden gebruikt te verwijderen:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

__Hdinsightclustername__ vervangen door de naam van uw cluster HDInsight.

##<a name="troubleshooting"></a>Problemen oplossen

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Geen resultaten of onverwachte resultaten bij het gebruik van de begin-HBaseExample

Gebruik de `-showErr` -parameter voor het weergeven van de standaardfout (STDERR) die wordt weergegeven tijdens het uitvoeren van de taak.
