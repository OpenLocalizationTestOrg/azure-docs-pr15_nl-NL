<properties 
    pageTitle="Analyseren van gegevens over vertragingen flight met onderdeel op Linux gebaseerde HDInsight | Microsoft Azure" 
    description="Leren werken met component vluchtgegevens op Linux gebaseerde HDInsight analyseren en vervolgens de gegevens exporteren naar Sqoop met SQL-Database." 
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
    ms.date="10/11/2016" 
    ms.author="larryfr"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Gegevens over vertragingen flight analyseren met behulp van component in HDInsight

Meer informatie over het analyseren van gegevens over vertragingen flight component met op Linux gebaseerde HDInsight en vervolgens de gegevens exporteren naar Azure SQL-Database met behulp van Sqoop.

> [AZURE.NOTE] Terwijl de afzonderlijke onderdelen van dit document kunnen worden gebruikt met Windows gebaseerde HDInsight kolomgroepen (Python en component bijvoorbeeld) veel stappen gelden voor Linux gebaseerde clusters. Zie voor stapsgewijze instructies die met een cluster op basis van Windows werkt, [analyseren flight gegevens over vertragingen component in HDInsight gebruiken](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Een HDInsight cluster__. Zie [aan de slag met Hadoop met onderdeel in HDInsight op Linux](hdinsight-hadoop-linux-tutorial-get-started.md) voor stapsgewijze instructies voor het maken van een nieuw Linux gebaseerde HDInsight cluster.

- __Azure SQL-Database__. U kunt een Azure SQL-database als een gegevensopslag bestemming wilt gebruiken. Als u een SQL-Database niet al hebt, raadpleegt u [SQL-Database zelfstudie: een SQL-database maken in minuten](../sql-database/sql-database-get-started.md).

- __Azure CLI__. Als u de CLI Azure niet hebt geïnstalleerd, raadpleegt u [installeren en configureren van de Azure CLI](../xplat-cli-install.md) voor meer informatie.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>De vluchtgegevens downloaden

1. Blader naar de [onderzoeks- en beheer van innovatieve technologie Bureau of transport Statistics][rita-website].
2. Klik op de pagina, selecteert u de volgende waarden:

  	| Naam | Waarde |
  	| ---- | ---- |
  	| Filter jaar | 2013 |
  	| Filteren van de termijn | Januari |
  	| Velden | Jaar, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Schakel alle andere velden |

3. Klik op **downloaden**. 

##<a name="upload-the-data"></a>De gegevens uploaden

1. Gebruik de volgende opdracht uit het zip-bestand uploaden naar het hoofd knooppunt van HDInsight:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    __Bestandsnaam__ vervangen door de naam van het zip-bestand. __Gebruikersnaam__ vervangen door de SSH-aanmelding voor de cluster HDInsight. CLUSTERNAAM vervangen door de naam van de cluster HDInsight.
    
    > [AZURE.NOTE] Als u een wachtwoord om te verifiëren van uw aanmelding SSH gebruikt, wordt u gevraagd het wachtwoord. Als u een openbare sleutel hebt gebruikt, moet u mogelijk gebruikt u de `-i` parameter en geef het pad naar het overeenkomende persoonlijke sleutel. Bijvoorbeeld `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Wanneer de upload is voltooid, maakt u verbinding met het cluster via SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Zie de volgende artikelen voor meer informatie over het gebruik van SSH met Linux gebaseerde HDInsight:
    
    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Zodra u verbinding hebt, gebruikt u de volgende Pak het ZIP-bestand:

        unzip FILENAME.zip
    
    Hiermee wordt een CSV-bestand dat is ongeveer 60MB groot uitgepakt.
    
4. Gebruik van de volgende handelingen uit een nieuwe map maken op WASB (de store gedistribueerde gegevens die worden gebruikt door HDInsight) en kopieert u het bestand:

    hdfs dfs - mkdir -p /tutorials/flightdelays/data hdfs dfs-plaatsen FILENAME.csv/zelfstudies/flightdelays/gegevens /
    
##<a name="create-and-run-the-hiveql"></a>Maken en uitvoeren van de HiveQL

Gebruik de volgende stappen uit om gegevens te importeren uit het CSV-bestand in een component-tabel met de naam __vertragingen__.

1. Gebruik de volgende handelingen uit om te maken en bewerken van een nieuw bestand met de naam __flightdelays.hql__:

        nano flightdelays.hql
        
    Gebruik de volgende handelingen uit als de inhoud van dit bestand:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Gebruik __Ctrl + X__en __Y__ het bestand wilt opslaan.

3. Gebruik de volgende component starten en uitvoeren van het bestand __flightdelays.hql__ :

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] In dit voorbeeld `localhost` wordt gebruikt, aangezien er verbinding is met het hoofd knooppunt van het cluster HDInsight, dat wil zeggen waarop HiveServer2 wordt uitgevoerd.

4. Gebruik de volgende opdracht uit een interactieve Beeline-sessie te openen:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Wanneer u ontvangt het `jdbc:hive2://localhost:10001/>` wordt gevraagd, voert u de volgende gegevens ophalen uit het geïmporteerde vluchtgegevens van een vertraging.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    Hiermee wordt een lijst met steden die geconstateerd weer vertragingen, samen met de gemiddelde vertragingstijd ophalen en opslaan op `/tutorials/flightdelays/output`. Later Sqoop leest de gegevens vanaf deze locatie en exporteren naar Azure SQL-Database.

6. Als u wilt afsluiten Beeline, voert u `!quit` bij de prompt.

## <a name="create-a-sql-database"></a>Een SQL-Database maken

Als u al een SQL-Database, kunt u de naam van de server moet ophalen. U kunt dit vinden in de [Portal van Azure](https://portal.azure.com) __SQL-Databases__selecteert en vervolgens filteren op de naam van de database die u wilt gebruiken. De servernaam wordt weergegeven in de kolom __SERVER__ .

Als u een SQL-Database niet al hebt, gebruikt u de informatie in [SQL-Database zelfstudie: een SQL-database maken in minuten](../sql-database/sql-database-get-started.md) u een account maakt. U moet de naam van de server gebruikt voor de database opslaan.

##<a name="create-a-sql-database-table"></a>Een tabel SQL-Database maken

> [AZURE.NOTE] Er zijn tal van manieren verbinding maken met SQL-Database aan een tabel maken. De volgende stappen gebruiken [FreeTDS](http://www.freetds.org/) vanuit het cluster HDInsight.

1. SSH met verbinding maken met het cluster Linux gebaseerde HDInsight en de volgende stappen uitvoeren vanaf de SSH-sessie.

3. Gebruik de volgende opdracht uit om te FreeTDS installeren:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Zodra FreeTDS is geïnstalleerd, gebruikt u de volgende opdracht verbinding maken met de SQL-Database-server. __Servernaam__ vervangen door de naam van de SQL-Database-server. __AdminLogin__ en __beheerderswachtwoord__ vervangen door de aanmelding voor SQL-Database. __Databasenaam__ vervangen door de naam van de database.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    U ontvangt uitvoer ongeveer als volgt uit:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Aan de `1>` wordt gevraagd, voert u de volgende regels:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Wanneer de `GO` instructie wordt ingevoerd, de vorige beweringen wordt geëvalueerd. Hiermee maakt u een nieuwe tabel met de naam __vertragingen__, met een gegroepeerde index (vereist voor SQL-Database).

    Gebruik de volgende handelingen uit om te bevestigen dat de tabel is gemaakt:

        SELECT * FROM information_schema.tables
        GO

    Hier ziet u uitvoer ongeveer als volgt uit:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Voer `exit` bij de `1>` vragen om het hulpprogramma tsql af te sluiten.
    
##<a name="export-data-with-sqoop"></a>Gegevens exporteren met Sqoop

2. Gebruik de volgende opdracht uit om te bevestigen dat Sqoop uw SQL-Database kunnen zien:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Dit moet retourneren een lijst met databases, met inbegrip van de database die u in de tabel vertragingen in eerder hebt gemaakt.

3. Gebruik de volgende opdracht gegevens exporteren vanuit hivesampletable aan de tabel mobiledata:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    Hiermee wordt aangegeven wanneer Sqoop verbinding maken met SQL-Database, met de database met de tabel vertragingen en gegevens exporteren vanuit de wasbs: / / / zelfstudies/flightdelays/uitvoer (waar we opgeslagen de uitvoer van de query component eerder,) aan de tabel vertragingen.

4. Nadat de opdracht is voltooid, gebruikt u de volgende verbinding maken met de database met TSQL:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Zodra u verbinding hebt, gebruikt u de volgende instructies om te bevestigen dat de gegevens zijn geëxporteerd naar de tabel mobiledata:
    
        SELECT * FROM delays
        GO

    Hier ziet u een lijst met gegevens in de tabel. Type `exit` wanneer u het hulpprogramma tsql afsluit.

##<a id="nextsteps"></a>Volgende stappen

Nu weten u hoe u een bestand uploaden naar Azure-blobopslag, hoe u een tabel component wordt gevuld met behulp van de gegevens van Azure-blobopslag component query's uitvoeren en het gebruik van Sqoop gegevens exporteren vanuit HDFS met een Azure SQL-database. Meer informatie raadpleegt u de volgende artikelen:

* [Aan de slag met HDInsight][hdinsight-get-started]
* [Component gebruiken met HDInsight][hdinsight-use-hive]
* [Oozie gebruiken met HDInsight][hdinsight-use-oozie]
* [Sqoop gebruiken met HDInsight][hdinsight-use-sqoop]
* [Varken met HDInsight gebruiken][hdinsight-use-pig]
* [Ontwikkel Java MapReduce-programma's voor HDInsight][hdinsight-develop-mapreduce]
* [Ontwikkel Python Hadoop streaming programma's voor HDInsight][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
