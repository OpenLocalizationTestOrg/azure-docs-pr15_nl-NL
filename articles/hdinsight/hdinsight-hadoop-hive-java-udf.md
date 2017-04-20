<properties
pageTitle="Een Java gebruiker gedefinieerde functie (UDF) gebruiken met component in HDInsight | Microsoft Azure"
description="Informatie over het maken en gebruiken van een Java gebruiker gedefinieerde functie (UDF) uit component in HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Gebruik een Java UDF met onderdeel in HDInsight

Component is handig voor het werken met gegevens in HDInsight, maar soms moet u een meer algemeen doel taal. Component kunt u de gebruiker gedefinieerde functies (UDF) met een verscheidenheid aan programming talen maken. In dit document leert u hoe u een UDF Java uit component gebruiken.

## <a name="requirements"></a>Vereisten

* Een Azure-abonnement

* Een cluster HDInsight (Windows of Linux-gebaseerde)

    > [AZURE.NOTE] De meeste stappen in dit document werkt op beide clustertypen; Er zijn echter de stappen die worden gebruikt voor de gecompileerd UDF uploaden naar het cluster en uitvoeren van deze specifiek voor Linux gebaseerde clusters. Koppelingen worden naar informatie die kan worden gebruikt met Windows gebaseerde clusters gegeven.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 of hoger (of een equivalent, zoals OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Een teksteditor of Java IDE

    > [AZURE.IMPORTANT] Als u een Linux gebaseerde HDInsight-server gebruikt, maar de Python-bestanden op een Windows-client maakt, moet u een editor gebruiken die gebruikmaakt van LF als een lijn die eindigen. Als u niet zeker of uw editor LF of CRLF gebruikt weet, raadpleegt u de sectie [Probleemoplossing](#troubleshooting) voor stapsgewijze instructies over het verwijderen van het CR-teken met hulpprogramma's op het cluster HDInsight.

## <a name="create-an-example-udf"></a>Een voorbeeld UDF maken

1. Vanaf de opdrachtregel, gebruikt u de volgende handelingen uit om een nieuwe Maven project te maken:

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Als u PowerShell gebruikt, moet u plaatst u aanhalingstekens rond de parameters. Bijvoorbeeld `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Hiermee maakt u een nieuwe map met de naam __exampleudf__, die het project Maven bevat.

2. Wanneer u het project hebt gemaakt, verwijder de __exampleudf/src/testen__ -map die is gemaakt als onderdeel van het project. niet gegevens worden worden gebruikt voor dit voorbeeld.

3. Open de __exampleudf/pom.xml__en het bestaande `<dependencies>` vermelding met de volgende handelingen uit:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Deze vermeldingen Geef de versie van Hadoop en component wordt geleverd bij HDInsight 3.3 en 3,4 clusters. Hier vindt u informatie over de versies van Hadoop en component HDInsight uw beschikking uit het document [HDInsight onderdeel versiebeheer](hdinsight-component-versioning.md) .

    Toevoegen een `<build>` sectie voordat u de `</project>` regel aan het einde van het bestand. In deze sectie moet de volgende elementen bevatten:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
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
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    
    Deze vermeldingen definiëren het maken van het project. Name de versie van Java die gebruikmaakt van het project en het maken van een uberjar voor implementatie aan het cluster.

    Sla het bestand als de wijzigingen zijn aangebracht.

4. Wijzig de naam van __exampleudf/src/main/java/com/microsoft/examples/App.java__ __ExampleUDF.java__en open het bestand in de editor.

5. De inhoud van het bestand __ExampleUDF.java__ vervangen door de volgende opties, klikt u vervolgens het bestand opslaat.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    Dit implementeert een UDF die een tekenreekswaarde geaccepteerd en geeft als resultaat een kleine letters versie van de tekenreeks.

## <a name="build-and-install-the-udf"></a>Bouwen en de UDF installeren

1. Gebruik de volgende opdracht uit om te compileren en de UDF inpakken:

        mvn compile package

    Dit wordt maken en vervolgens de UDF inpakken in __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__.

2. Gebruik de `scp` opdracht om het bestand kopiëren naar het cluster HDInsight.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    Vervang __myuser__ met het gebruikersaccount SSH voor uw cluster. __Mijncluster__ vervangen door de naam van de cluster. Als u een wachtwoord hebt gebruikt om de SSH-account te beveiligen, wordt u gevraagd het wachtwoord in te voeren. Als u een certificaat hebt gebruikt, moet u mogelijk gebruikt u de `-i` -parameter voor het opgeven van het bestand met persoonlijke sleutel.

3. Verbinding maken met het cluster via SSH. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Zie de volgende documenten voor meer informatie over het gebruik van SSH met HDInsight.

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

4. Uit de sessie SSH het oppervlak-bestand naar HDInsight opslag te kopiëren.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>De UDF uit component gebruiken

1. Gebruik de volgende handelingen uit de client Beeline starten vanuit de sessie SSH.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Deze opdracht wordt ervan uitgegaan dat u de standaardinstelling van __beheerder__ voor de aanmeldingsaccount voor uw cluster gebruikt.

2. Zodra u bij komt de `jdbc:hive2://localhost:10001/>` wordt gevraagd, voert u de volgende handelingen uit als u wilt de UDF aan component toevoegen en deze weer te geven als een functie.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. Gebruik de UDF waarden opgehaald uit een tabel naar kleine letters tekenreeksen om te zetten.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    Hiermee wordt het apparaatplatform (Android, Windows, iOS, enz.) Selecteer in de tabel, de tekenreeks omzetten in kleine letters en geef ze weer. De uitvoer, wordt de volgende strekking weergegeven.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Volgende stappen

Voor andere manieren om te werken met component, raadpleegt u [Gebruik component met HDInsight](hdinsight-use-hive.md).

Voor meer informatie over Hive User-Defined functies, Zie [component operatoren en door de gebruiker gedefinieerde functies](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) het gedeelte van de wiki component bij apache.org.
