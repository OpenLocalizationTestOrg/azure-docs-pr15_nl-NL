<properties
   pageTitle="Beeline gebruiken om te werken met onderdeel op HDInsight (Hadoop) | Microsoft Azure"
   description="Informatie over het gebruik van SSH om te verbinden met een Hadoop-cluster in HDInsight en vervolgens de component query's met behulp van Beeline interactief dient. Beeline is een hulpprogramma voor het werken met HiveServer2 via JDBC."
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Component gebruiken met Hadoop in HDInsight via Beeline

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In dit artikel leert u hoe u verbinding maken met een cluster Linux gebaseerde HDInsight en vervolgens de component query's met behulp van het hulpprogramma voor de [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) interactief dient met Secure Shell (SSH).

> [AZURE.NOTE] Beeline maakt JDBC verbinding met component. Zie [verbinding maken met de component op Azure HDInsight met het stuurprogramma JDBC component](hdinsight-connect-hive-jdbc-driver.md)voor meer informatie over het gebruik van JDBC met onderdeel.

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

* Een Linux gebaseerde Hadoop op HDInsight cluster.

* Een SSH-client. Linux, Unix en Mac OS moeten worden geleverd met een SSH-client. Windows-gebruikers, moeten een mailclient gebruikt, zoals [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)downloaden.

##<a id="ssh"></a>Verbinding maken met SSH

Verbinding maken met de FQDN-naam (Fully Qualified Domain Name) van uw cluster HDInsight via de opdracht SSH. De FQDN-naam moet de naam die u vervolgens het cluster, gegeven **. azurehdinsight.net**. Bijvoorbeeld de volgende manier verbinding maken met een cluster met de naam **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Als u een certificaat-toets voor de verificatie SSH opgegeven** toen u het cluster HDInsight hebt gemaakt, mogelijk moet u de locatie van de persoonlijke sleutel op uw clientsysteem opgeven:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Als u een wachtwoord voor de verificatie SSH opgegeven** toen u het cluster HDInsight hebt gemaakt, moet u het wachtwoord wanneer u wordt gevraagd te geven.

Zie voor meer informatie over het gebruik van SSH met HDInsight, [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, OS X, en Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Stopverf (clients op basis van Windows)

Windows beschikt niet over een ingebouwde SSH-client. We raden u aan **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight van Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="beeline"></a>Gebruik de opdracht Beeline

1. Zodra u verbinding hebt, gebruikt u de volgende Beeline starten:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Hiermee wordt de client Beeline begin en verbinding maken met de JDBC-url. Hier `localhost` aangezien HiveServer2 wordt uitgevoerd op beide hoofd knooppunten in het cluster en we Beeline rechtstreeks in de primaire headnode uitvoert wordt gebruikt.
    
    Zodra de opdracht is voltooid, wordt u automatisch komt bij een `jdbc:hive2://localhost:10001/>` prompt.

3. Opdrachten beeline meestal beginnen met een `!` teken, bijvoorbeeld `!help` help weergeven. Echter de `!` vaak kan worden weggelaten. Bijvoorbeeld `help` werkt ook.

    Als u help bekijkt, ziet u `!sql`, die wordt gebruikt HiveQL instructies uitvoeren. Echter HiveQL is dus veelgebruikte dat kunt u de voorgaande weglaat `!sql`. De volgende twee instructies hebt precies dezelfde resultaten; de tabellen die momenteel beschikbaar via de component weergeven:
    
        !sql show tables;
        show tables;
    
    Klik op een nieuw cluster, slechts één tabel moet worden weergegeven: __hivesampletable__.

4. Gebruik de volgende handelingen uit om het schema voor het hivesampletable weer te geven:

        describe hivesampletable;
        
    Hiermee herstelt u de volgende informatie:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Hiermee worden de kolommen in de tabel weergegeven. Terwijl we enkele query's op deze gegevens uitvoeren kan, in plaats daarvan een nieuwe tabel maken u kunt zien hoe gegevens in de component laden en toepassen van een schema.
    
5. Voer de volgende instructies om een nieuwe tabel met de naam **log4jLogs** met behulp van het cluster HDInsight uw beschikking voorbeeldgegevens maken:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies heeft de volgende acties uitvoeren:

    * **DROP TABLE** - verwijdert u de tabel en het gegevensbestand geval de tabel al bestaat.
    * **Externe tabel maken** - Hiermee maakt u een nieuwe 'externe' tabel in component. De definitie van de tabel externe tabellen alleen opgeslagen in component. De gegevens is links op de oorspronkelijke locatie.
    * **Rij-indeling** : Hiermee wordt aan component hoe de gegevens worden opgemaakt. In dit geval worden de velden in elke log gescheiden door een spatie.
    * **Opgeslagen als TEXTFILE locatie** - Hiermee wordt aan Component waar de gegevens is opgeslagen (de map met de voorbeeldgegevens /), en dat deze wordt opgeslagen als tekst.
    * **Selecteer** - Hiermee selecteert u een telling van alle rijen waarbij kolom **t4** de waarde **[fout bevat]**. Dit moet een waarde van **3** retourneren als er drie rijen die deze waarde bevatten.
    * **INPUT__FILE__NAME zoals '%.log'** - Hiermee wordt aan die we alleen gegevens uit bestanden die eindigen ophalen moet op component. log. Normaal gesproken vergt zou u alleen is gegevens met hetzelfde schema in dezelfde map als query's uitvoeren met de component, maar het logboekbestand van dit voorbeeld wordt opgeslagen met andere gegevensindelingen.

    > [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens die moeten worden bijgewerkt door een externe bron, zoals een geautomatiseerde gegevens upload-proces of door een andere MapReduce bewerking, maar altijd wilt component query's voor het gebruik van de meest recente gegevens verwachten.
    >
    > Weghalen van een externe tabel bevat **niet** verwijderen de gegevens, alleen de definitie van de tabel.
    
    De uitvoer van deze opdracht moet er ongeveer als volgt te werk:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Als u wilt afsluiten Beeline, gebruikt u `!quit`.

##<a id="file"></a>Een bestand HiveQL uitvoeren

Beeline kunnen ook worden gebruikt om een bestand met HiveQL-instructies te voeren. Gebruik de volgende stappen naar een bestand maken en voer vervolgens met Beeline.

1. Gebruik de volgende opdracht uit om te maken van een nieuw bestand met de naam __query.hql__:

        nano query.hql
        
2. Zodra de editor wordt geopend, gebruikt u de volgende handelingen uit als de inhoud van het bestand. Deze query maakt een nieuwe 'interne' tabel met de naam **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Deze instructies heeft de volgende acties uitvoeren:

    * **Maken van tabel als niet aanwezig** - Hiermee maakt u een tabel, als deze nog niet bestaat. Aangezien het **externe** trefwoord niet wordt gebruikt, is dit is een interne tabel, die is opgeslagen in de component datawarehouse en volledig wordt beheerd door component.
    * De gegevens **Die zijn opgeslagen als ORC** - opgeslagen in geoptimaliseerd rij kolommen (ORC)-indeling. Dit is een zeer geoptimaliseerde en efficiënt indeling voor het opslaan van de component data.
    * **Invoegen OVERSCHRIJVEN... Selecteer** : Hiermee selecteert u rijen uit de tabel **log4jLogs** die **[fout]**bevatten en vervolgens de gegevens in de tabel **errorLogs** invoegen.
    
    > [AZURE.NOTE] In tegenstelling tot de externe tabellen, worden een interne tabel weghalen verwijderd als u ook de onderliggende gegevens.
    
3. Als u wilt het bestand opslaat, gebruikt u __Ctrl__+__X___, voer vervolgens __j__en ten slotte op __Enter__.

4. Gebruik de volgende handelingen uit in het bestand met Beeline uitvoeren. __HOSTNAME__ vervangen door de naam die eerder verkregen voor de kop knooppunt en het __wachtwoord__ met het wachtwoord voor het account:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] De `-i` parameter Beeline begint, voert u de instructies in het bestand query.hql en blijft aanwezig in Beeline bij de `jdbc:hive2://localhost:10001/>` prompt. U kunt ook uitvoeren voor een bestand met de `-f` parameter keert u terug naar we vaker doen nadat het bestand is verwerkt.

5. Om te bevestigen dat de tabel **errorLogs** is gemaakt, gebruikt u de volgende instructie om te retourneren van alle rijen uit **errorLogs**:

        SELECT * from errorLogs;

    Drie rijen met gegevens moet worden geretourneerd, alle met **[fout]** in de kolom t4:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Meer informatie over het Beeline-connectiviteit

De stappen in dit document gebruik `localhost` verbinding maken met HiveServer2 waarop de headnode cluster. Terwijl u ook de hostnaam kunt of de FQDN-naam van de headnode die extra stappen voor het proces (stappen vereisen om vast te de hostnaam of FQDN). Met `localhost` voldoende is wanneer u een Beeline uit de headnode gebruikt.

Als u een randknooppunt in uw cluster met Beeline is geïnstalleerd, moet u de hostnaam of FQDN-naam van de headnode om verbinding te gebruiken.

Als u Beeline installeren op een client buiten uw cluster hebt, kunt u verbinding maken met de volgende opdracht uit. __CLUSTERNAAM__ vervangen door de naam van uw cluster HDInsight. __Wachtwoord__ vervangen door het wachtwoord voor het beheerdersaccount (HTTP aanmelden).

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Houd er rekening mee dat de parameters/URI anders is dan bij het rechtstreeks op een headnode of van een randknooppunt binnen het cluster uitgevoerd. Dit komt omdat de verbinding met het cluster van internet gebruikmaakt van een openbare gateway dat het verkeer via poort 443 routeert. Ook worden verschillende andere services via de openbare gateway op poort 443, zodat de URI anders is dan bij het rechtstreeks verbinding maken. Wanneer u verbinding maakt via internet, moet u ook de sessie verifiëren door het wachtwoord.

##<a id="summary"></a><a id="nextsteps"></a>Volgende stappen

Zoals u ziet u de opdracht Beeline een eenvoudige manier interactief component query's uitvoeren op een cluster HDInsight.

Voor algemene informatie over component in HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u Tez met onderdeel gebruikt, raadpleegt u de volgende documenten voor foutopsporing informatie:

* [De gebruikersinterface Tez op HDInsight op basis van Windows gebruiken](hdinsight-debug-tez-ui.md)

* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

