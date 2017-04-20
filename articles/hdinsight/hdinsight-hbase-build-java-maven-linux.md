<properties
    pageTitle="Een HBase-toepassing met Maven en Java maken en klik vervolgens implementeren naar Linux gebaseerde HDInsight | Microsoft Azure"
    description="Informatie over het gebruik van Apache Maven om te maken van een toepassing Java gebaseerde Apache HBase en klik op het dashboard implementeren naar Linux gebaseerde HDInsight in de cloud Azure."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Maven gebruiken om te maken van Java-toepassingen die gebruikmaken van HBase bij op basis van Linux HDInsight (Hadoop)

Informatie over het maken en te bouwen van een toepassing [Apache HBase](http://hbase.apache.org/) in Java met behulp van Apache Maven. Gebruik vervolgens de toepassing met een cluster Linux gebaseerde HDInsight.

[Maven](http://maven.apache.org/) is een software-projectmanagement en inzichtelijk hulpmiddel waarmee u software, documentatie en rapporten voor Java projecten maken. In dit artikel wordt uitgelegd hoe u dit als u wilt maken van een eenvoudige Java-toepassing die wordt gemaakt, query's gebruikt en een tabel HBase op een cluster Linux gebaseerde HDInsight verwijderd.

> [AZURE.NOTE] De stappen in dit document wordt ervan uitgegaan dat u een cluster Linux gebaseerde HDInsight gebruikt. Zie voor informatie over het gebruik van een cluster HDInsight op basis van Windows [Maven Java-toepassingen die gebruikmaken van HBase met HDInsight op basis van Windows gebruiken](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Vereisten

* [Java-platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 of hoger

* [Maven](http://maven.apache.org/)

* [Een Linux gebaseerde Azure HDInsight cluster met HBase](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] De stappen in dit document is getest met HDInsight cluster versies 3,2, 3.3 en 3.4. De standaardwaarden die beschikbaar zijn in de voorbeelden zijn bedoeld voor een cluster HDInsight 3.4.

* **Bekend zijn met SSH en SCP**. Zie de volgende onderwerpen voor meer informatie over het gebruik van SSH en SCP met HDInsight:

    * **Linux, Unix of OS X-clients**: Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, OS X of Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-clients**: Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>Het project maken

1. Vanaf de opdrachtregel in uw ontwikkelomgeving, door mappen te wijzigen naar de locatie waar u wilt maken van het project, bijvoorbeeld `cd code/hdinsight`.

2. Gebruik de opdracht __mvn__ , die is geïnstalleerd met Maven, om te genereren van de steiger voor het project.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Hierdoor wordt een nieuwe map in de huidige map, gemaakt met de naam die is opgegeven met de parameter __artifactID__ (**hbaseapp** in dit voorbeeld.) Deze map bevat de volgende items:

    * __pom.XML__: de Project-objectmodel ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) bevat informatie en configuratie van informatie voor het samenstellen van het project.

    * __src__: de map waarin de map __Hoofdgegeven/java/com/microsoft/voorbeelden__ , waar u de toepassing wordt creëert.

3. Verwijder het bestand __src/test/java/com/microsoft/examples/apptest.java__ omdat deze niet in dit voorbeeld worden gebruikt.

##<a name="update-the-project-object-model"></a>Het objectmodel van het Project bijwerken

1. Bewerk het bestand __pom.xml__ en voeg de volgende code binnen de `<dependencies>` sectie:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Hiermee wordt aangegeven Maven dat het project __hbase-client__ versie __1.1.2__vereist. Tijdens het compileren, wordt dit gedownload van de standaard Maven opslagplaats. U kunt de [Maven centrale opslagplaats zoeken](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) voor meer informatie over deze afhankelijkheid.

    > [AZURE.IMPORTANT] Het versienummer moet overeenkomen met de versie van HBase die wordt geleverd bij uw cluster HDInsight. Gebruik de volgende tabel om het juiste versienummer.

  	| HDInsight cluster versie | HBase versie moet worden gebruikt |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3.3 en 3.4 | 1.1.2 |

    Zie [Wat zijn de verschillende Hadoop-onderdelen beschikbaar voor communicatie met HDInsight](hdinsight-component-versioning.md)voor meer informatie over HDInsight versies en onderdelen.

2. Als u een 3.3 HDInsight of 3,4 cluster gebruikt, moet u ook de volgende toevoegen de `<dependencies>` sectie:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Hiermee opent de phoenix core-onderdelen, die nodig zijn met Hbase versie 1.1.x.

2. Voeg de volgende code naar het bestand __pom.xml__ . Dit moet binnen de `<project>...</project>` codes in het bestand, bijvoorbeeld tussen `</dependencies>` en `</project>`.

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

    Hiermee wordt een resource (__conf/hbase-site.xml__), die configuratiegegevens voor HBase bevat.

    > [AZURE.NOTE] U kunt ook de waarden van de systeemconfiguratie via programmacode instellen. Zie de opmerkingen in het volgende voor hoe u dit voorbeeld __CreateTable__ .

    Hiermee configureert ook de [Maven compileerprogramma Plug-in](http://maven.apache.org/plugins/maven-compiler-plugin/) en de [Invoegtoepassing voor Maven arceren](http://maven.apache.org/plugins/maven-shade-plugin/). De invoegtoepassing compileerprogramma wordt gebruikt om samen te stellen van de topologie. De invoegtoepassing tint wordt gebruikt om te voorkomen dat licentie dupliceren in het oppervlak-pakket die is ingebouwd door Maven. De reden waarom die dit wordt gebruikt, is dat de dubbele licentiebestanden ertoe leiden dat een fout tijdens de uitvoering op het cluster HDInsight. Met maven-arceren-invoegtoepassing met de `ApacheLicenseResourceTransformer` implementatie voorkomt dat deze fout.

    De invoegtoepassing voor het arceren van een maven ook genereert een uber oppervlak (of fat oppervlak,) waarin alle afhankelijkheden vereist door de toepassing.

3. Sla het bestand __pom.xml__ .

4. Maak een nieuwe map met de naam __conf__ in de adreslijst __hbaseapp__ . Dit wordt gebruikt voor het opslaan van configuratiegegevens voor verbinding maakt met HBase.

5. Gebruik de volgende opdracht uit de HBase-configuratie van de server HDInsight kopiëren naar de map __conf__ . **Gebruikersnaam** vervangen het de naam van uw aanmelding SSH. **CLUSTERNAAM** vervangen door de naam van uw HDInsight cluster:

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Als u een wachtwoord voor uw account SSH gebruikt, wordt u gevraagd het wachtwoord in te voeren. Als u een SSH-toets met het account gebruikt, moet u mogelijk gebruikt u de `-i` -parameter voor het opgeven van het pad naar het bestand. Het volgende voorbeeld wordt de persoonlijke sleutel uit laden `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>De toepassing maken

1. Ga naar de map __hbaseapp/src/Hoofdgegeven/java/com/microsoft/voorbeelden__ en geef het bestand app.java __CreateTable.java__.

2. Open het bestand __CreateTable.java__ en de bestaande inhoud vervangen door het volgende:

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
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

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

4. Maak een nieuw bestand met de naam __SearchByEmail.java__in de adreslijst __hbaseapp/src/Hoofdgegeven/java/com/microsoft/voorbeelden__ . Gebruik de volgende handelingen uit als de inhoud van dit bestand:

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

6. Maak een nieuw bestand met de naam __DeleteTable.java__in de adreslijst __hbaseapp/src/Hoofdgegeven/hava/com/microsoft/voorbeelden__ . Gebruik de volgende handelingen uit als de inhoud van dit bestand:

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

2. Gebruik de volgende opdracht uit een oppervlak-bestand met de toepassing maken uit de adreslijst __hbaseapp__ :

        mvn clean package

    Hiermee worden de onderdelen van een vorige opbouwen, downloads eventuele afhankelijkheden die nog niet zijn geïnstalleerd, klikt u vervolgens genereert en pakketten van de toepassing.

3. Als de opdracht is voltooid, wordt de ____ hbaseapp/doelmap een bestand met de naam __hbaseapp-1.0-SNAPSHOT.jar__bevatten.

    > [AZURE.NOTE] Het bestand __hbaseapp-1.0-SNAPSHOT.jar__ is een uber oppervlak (ook wel een fat oppervlak, genoemd) waarin alle afhankelijkheden vereist voor het uitvoeren van de toepassing.

##<a name="upload-the-jar-file-and-run-jobs"></a>Het oppervlak-bestand uploaden en taken uitvoeren

1. Gebruik de volgende handelingen uit om het oppervlak te uploaden naar het cluster HDInsight. **Gebruikersnaam** vervangen het de naam van uw aanmelding SSH. **CLUSTERNAAM** vervangen door de naam van uw HDInsight cluster:

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Hiermee wordt het bestand uploaden naar de basismap voor uw gebruikersaccount SSH.

    > [AZURE.NOTE] Als u een wachtwoord voor uw account SSH gebruikt, wordt u gevraagd het wachtwoord in te voeren. Als u een SSH-toets met het account gebruikt, moet u mogelijk gebruikt u de `-i` -parameter voor het opgeven van het pad naar het bestand. Het volgende voorbeeld wordt de persoonlijke sleutel uit laden `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. Gebruik SSH verbinding maken met de cluster HDInsight. **Gebruikersnaam** vervangen het de naam van uw aanmelding SSH. **CLUSTERNAAM** vervangen door de naam van uw HDInsight cluster:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Als u een wachtwoord voor uw account SSH gebruikt, wordt u gevraagd het wachtwoord in te voeren. Als u een SSH-toets met het account gebruikt, moet u mogelijk gebruikt u de `-i` -parameter voor het opgeven van het pad naar het bestand. Het volgende voorbeeld wordt de persoonlijke sleutel uit laden `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Zodra u verbinding hebt, gebruikt u de volgende handelingen uit om een nieuwe HBase-tabel met de Java-toepassing te maken:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    Hiermee maakt u een nieuwe HBase-tabel met de naam __personen__, en gevuld met gegevens.

4. Gebruik vervolgens de volgende handelingen uit om te zoeken naar e-mailadressen die zijn opgeslagen in de tabel:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    U kunt de volgende resultaten moet ontvangen:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>De tabel verwijderen

Wanneer u klaar bent met het voorbeeld, gebruikt u de volgende opdracht uit de Azure PowerShell-sessie om de tabel van de __personen__ die in dit voorbeeld worden gebruikt te verwijderen:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

