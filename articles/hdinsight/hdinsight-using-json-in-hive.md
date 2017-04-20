<properties
   pageTitle="Analyseren en proces JSON documenten met onderdeel in HDInsight | Microsoft Azure"
   description="Meer informatie over het gebruiken van JSON-documenten en deze analyseren met component in HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Proces en analyseren JSON-documenten component in HDInsight

Informatie over het proces en analyseren JSON-bestanden component in HDInsight. Het volgende JSON-document worden gebruikt in deze zelfstudie

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Het bestand kan worden gevonden op wasbs://processjson@hditutorialdata.blob.core.windows.net/. Zie voor meer informatie over het gebruik van Azure-blobopslag met HDInsight [Gebruik HDFS-compatibele Azure-blobopslag met Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md). Als u wilt, kunt u het bestand kopiëren naar de standaard-container van uw cluster.

In deze zelfstudie gebruikt u de component-console.  Zie [Gebruik component met Hadoop op HDInsight met extern bureaublad](hdinsight-hadoop-use-hive-remote-desktop.md)voor instructies van het openen van de component-console.

##<a name="flatten-json-documents"></a>JSON documenten samenvoegen

De methoden in het volgende gedeelte is vereist dat de JSON-document in één rij. U moet dus samenvoegen dat de JSON-document met een tekenreeks. Als uw document JSON is al ze zijn teruggebracht, kunt u deze stap overslaan en Ga rechtstreeks naar het volgende gedeelte op JSON analyseren van gegevens.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Het logboekbestand van JSON bevindt zich op **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. De tabel *StudentsRaw* component verwijst naar het onbewerkte niet afgeplatte JSON-document.

De tabel *StudentsOneLine* component wordt de opslag van de gegevens in het bestandssysteem van HDInsight standaard onder het pad */json/leerlingen/studenten /* .

De instructie INSERT vullen de StudentOneLine-tabel met de afgeplatte JSON-gegevens.

De SELECT-instructie terug 1 rij alleen.

Hier ziet u de uitvoer van de SELECT-instructie:

![Samenvoegen van de JSON-document.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>JSON-documenten in component analyseren

Component biedt drie verschillende methoden voor het uitvoeren van query's op documenten JSON:

- Gebruik aan de\_JSON\_OBJECT UDF (de gebruiker gedefinieerde functie)
- de UDF JSON_TUPLE gebruiken
- aangepaste SerDe gebruiken
- Schrijf dat u eigenaar bent van UDF Python of andere talen gebruikt. Zie [dit artikel] [ hdinsight-python] over het uitvoeren van uw eigen code Python met onderdeel.

### <a name="use-the-getjsonobject-udf"></a>Gebruik aan de\_JSON_OBJECT UDF
Component bevat een ingebouwde UDF [krijgen json-object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) dat de JSON query's uitvoeren tijdens runtime kunt uitvoeren. Deze methode heeft twee argumenten – de naam van de tabel en de methodenaam van de die het afgeplatte JSON-document en de JSON-veld dat moet worden geparseerd. Bekijk een voorbeeld om te kijken hoe deze UDF werkt.

Voornaam en achternaam voor elke leerling/student

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Hier ziet de uitvoer wanneer deze query uitgevoerd op consolevenster.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Er gelden enkele beperkingen van de UDF get-json_object.

- Omdat voor elk veld in de query opnieuw parseren van de query, heeft dit gevolgen voor de prestaties.
- KRIJG\_JSON_OBJECT() geeft als resultaat die de tekenreeks van een matrix. Als u wilt converteren dit naar een matrix component, moet u reguliere expressies gebruiken bij het vervangen van vierkante haken ' [' en ']' en vervolgens ook bellen splitsing om toegang te krijgen van de matrix.


Daarom de wiki component raadt json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Gebruik de UDF JSON_TUPLE

Een andere UDF verstrekt door Component heet [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) waarmee beter dan [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Deze methode een set toetsen en een tekenreeks JSON duurt en geeft als resultaat een tupel van waarden met een functie. De volgende query geeft als resultaat de student-id en de kwaliteit van de JSON-document:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

De uitvoer van dit script in de component-console:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_TUPEL worden de syntaxis van de [zijkanten weergave](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) gebruikt in Component waarmee json\_tupel een virtuele tabel maken met het toepassen van de functie UDT op elke rij van de oorspronkelijke tabel.  Complexe JSONs worden ook lastig vanwege herhaald gebruik van de zijkanten weergave. Bovendien kan niet JSON_TUPLE geneste JSONs verwerken.


###<a name="use-custom-serde"></a>Aangepaste SerDe gebruiken

SerDe is de beste keuze voor parseren van geneste JSON-documenten, kunt u het schema JSON definiëren en gebruiken van het schema parseren van documenten. In deze zelfstudie gebruikt u een van de populaire SerDe die is ontwikkeld door [rcongiu](https://github.com/rcongiu).

**De aangepaste SerDe gebruiken:**

1. Installeer [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). De Windows X64-versie van de JDK kiezen als u wilt gebruiken het Windows-implementatie van HDInsight

    >[AZURE.WARNING] JDK 1,8 werkt niet met deze SerDe.

    Nadat de installatie is voltooid, een nieuwe gebruikersomgevingsvariabele toevoegen

    1. Open de **weergave Geavanceerde systeeminstellingen** in het scherm van Windows.
    2. Klik op **omgevingsvariabelen**.  
    3. Voeg dat een nieuwe **JAVA_HOME** omgevingsvariabele verwijst naar **C:\Program Files\Java\jdk1.7.0_55** of afhankelijk van waar uw JDK is geïnstalleerd.

    ![Juiste config waarden in te stellen voor JDK][image-hdi-hivejson-jdk]

2. [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip) installeren

    De map bin toevoegen aan uw pad door te gaan naar het besturingselement Panel--> bewerken de variabelen systeem voor uw account Environment variabelen. De volgende schermafbeelding ziet u hoe u dit wilt doen.

    ![Bij het instellen van Maven][image-hdi-hivejson-maven]

3. Het project van [Component-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github site klonen. U kunt dit doen door te klikken op de knop 'Downloaden Zip', zoals wordt weergegeven in de onderstaande schermafbeelding.

    ![Klonen van het project][image-hdi-hivejson-serde]

4: Ga naar de map waar u deze Inpakken en type mvn 'pakket' hebt gedownload. Dit moet de benodigde oppervlak-bestanden die u vervolgens via naar het cluster kopiëren kunt maken.

5: Ga naar de doelmap onder de hoofdmap waar u het pakket hebt gedownload. Het json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar bestand uploaden naar hoofd-knooppunt van uw cluster. Ik dit meestal onder de component binaire map opgeslagen: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin of een vergelijkbare.

6: Typ in de prompt component 'toevoegen oppervlak /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar'. Omdat in mijn geval wordt het oppervlak zich in de map C:\apps\dist\hive-0.13.x\bin, kan ik het oppervlak met de naam rechtstreeks toevoegen zoals hieronder wordt weergegeven:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

U bent nu klaar voor gebruik van de SerDe uitvoeren van query's ten opzichte van de JSON-document.

De volgende instructie Maak een tabel met een gedefinieerd schema

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Voor een overzicht van de voornaam en achternaam van de student

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Hier ziet het resultaat van de component-console.

![SerDe Query 1][image-hdi-hivejson-serde_query1]

Voor het berekenen van de som van scores van de JSON-document

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

De bovenstaande query gebruikt [zijkanten weergave uitlichten](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF om uit te vouwen van de matrix van scores zodat ze kunnen worden opgeteld.

Hier ziet u de uitvoer van de component-console.

![SerDe Query 2][image-hdi-hivejson-serde_query2]

Om te zoeken welke onderwerpen die een opgegeven student meer dan 80 punten heeft behaald selecteren  
      JT. StudentClassCollection.ClassId uit json_table jt zijkanten weergave uitlichten (jt. StudentClassCollection.Score) siteverzameling als score waar score > 80;

De bovenstaande query resulteert in een matrix component in tegenstelling tot get\_json\_object dat geeft een tekenreeks als resultaat.

![SerDe Query 3][image-hdi-hivejson-serde_query3]

Als u skil wilt onjuiste JSON, en vervolgens zoals wordt beschreven in de [wikipagina](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) van deze SerDe u die bereiken kunt door te typen van de volgende code:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Overzicht
Het type JSON-operator in component die u kiest is Kortom, afhankelijk van uw scenario. Als u een eenvoudige JSON-document hebt en u slechts één veld hoeft opzoeken op – kunt u gebruik aan van de component UDF\_json\_object. Als u meer dan één toetsen hebt opzoeken op kunt u json_tuple gebruiken. Als u een geneste document hebt, kunt u de JSON-SerDe moet gebruiken.

Zie voor andere verwante artikelen

- [Component en HiveQL gebruiken met Hadoop in HDInsight te analyseren van een steekproef Apache log4j-bestand](hdinsight-use-hive.md)
- [Gegevens over vertragingen flight analyseren met behulp van component in HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Gegevens te analyseren Twitter met component in HDInsight](hdinsight-analyze-twitter-data.md)
- [Een Hadoop-taak met DocumentDB en HDInsight uitvoeren](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
