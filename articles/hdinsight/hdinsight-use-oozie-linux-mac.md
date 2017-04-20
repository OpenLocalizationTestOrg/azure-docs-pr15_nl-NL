<properties
    pageTitle="Hadoop Oozie werkstromen gebruiken in Linux gebaseerde HDInsight | Microsoft Azure"
    description="Gebruik Hadoop Oozie in Linux gebaseerde HDInsight. Informatie over het definiëren van een werkstroom Oozie en dien een taak Oozie."
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


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-hdinsight"></a>Oozie gebruiken met Hadoop definiëren en uitvoeren van een werkstroom op Linux gebaseerde HDInsight

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Informatie over het gebruik van Apache Oozie om te definiëren van een werkstroom die gebruikmaakt van component en Sqoop en voer vervolgens de werkstroom op een cluster Linux gebaseerde HDInsight.

Apache Oozie is een werkstroom/afhankelijk-systeem die worden beheerd in Hadoop-taken. Het is geïntegreerd met de stapel Hadoop, en Hadoop taken voor Apache MapReduce, Apache varken Apache component en Apache Sqoop wordt ondersteund. Ook kunnen worden gebruikt om taken die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn te plannen

> [AZURE.NOTE] Een andere optie voor het definiëren van werkstromen met HDInsight is Factory van Azure-gegevens. Meer informatie over Azure gegevens Factory, raadpleegt u [Gebruik varken en component met gegevens Factory][azure-data-factory-pig-hive].

##<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**: Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI**: Zie [installeren en configureren van de Azure CLI](../xplat-cli-install.md)
    
    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- **Een HDInsight cluster**: Zie [Aan de slag met HDInsight op Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Een Azure SQL-database**: Hiermee worden gemaakt met de stappen in dit document

##<a name="example-workflow"></a>Voorbeeldwerkstroom

De werkstroom die u implementeren wilt volgens de instructies in dit document bevat twee acties. Acties zijn definities voor taken, zoals component, Sqoop, MapReduce of andere proces uitgevoerd:

![Werkstroomdiagram][img-workflow-diagram]

1. Een component actie wordt uitgevoerd voor een script HiveQL records extraheren uit de **hivesampletable** wordt geleverd bij HDInsight. Elke rij met gegevens worden een bezoek van een specifieke mobiel apparaat. De recordindeling ziet er ongeveer als volgt uit:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    De component script gebruikt in dit document telt het totale bezoeken voor elk platform (zoals Android of iPhone), worden opgeslagen en de telt in een nieuwe tabel in de component.

    Zie voor meer informatie over component, [Gebruik component met HDInsight][hdinsight-use-hive].

2.  Een actie Sqoop Hiermee exporteert u de inhoud van de nieuwe tabel in de component aan een tabel in een Azure SQL-database. Zie voor meer informatie over Sqoop, [Gebruik Hadoop Sqoop met HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Zie voor ondersteunde Oozie versies op HDInsight clusters [Wat is er nieuw in de Hadoop cluster versies geleverd door HDInsight?] [hdinsight-versions].

##<a name="create-the-working-directory"></a>De werkmap maken

Oozie verwacht resources die zijn vereist voor een taak moet worden opgeslagen in dezelfde map. In dit voorbeeld wordt **wasbs: / / / zelfstudies/useoozie**. Gebruik de volgende opdracht deze map en de gegevensmap waarin de nieuwe component-tabel gemaakt door deze werkstroom wordt ingevoerd maken:

    hdfs dfs -mkdir -p /tutorials/useoozie/data

> [AZURE.NOTE] De `-p` parameter veroorzaakt alle mappen in het pad naar het worden gemaakt als ze nog niet bestaan. **De gegevensmap** worden gebruikt voor het opslaan van gegevens die worden gebruikt door het script **useooziewf.hql** .

Ook de volgende opdracht zorgt ervoor dat Oozie zich als uw gebruikersaccount voordoen kan wanneer u zich component en Sqoop taken uitvoeren. **Gebruikersnaam** vervangen door uw aanmeldingsnaam:

    sudo adduser USERNAME users

Als er een foutbericht dat de gebruiker al lid zijn van gebruikers is, kunt u deze gewoon negeren.

##<a name="add-a-database-driver"></a>Een databasestuurprogramma toevoegen

Aangezien deze werkstroom worden Sqoop gebruikt om gegevens te exporteren met SQL-Database, moet u een kopie van het stuurprogramma voor JDBC gebruikt om te communiceren met SQL-Database opgeven. Gebruik de volgende opdracht uit om deze te kopiëren naar de werkmap:

    hdfs dfs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/

Als de werkstroom voor het andere bronnen, zoals een oppervlak met een toepassing voor de MapReduce hebt gebruikt, moet u zou deze ook toevoegen.

##<a name="define-the-hive-query"></a>De query component definiëren

Gebruik de volgende stappen naar een HiveQL script maken waarmee een query die wordt gebruikt in een werkstroom Oozie verderop in dit document definieert.

1. Met SSH verbinding maken met het cluster Linux gebaseerde HDInsight:

    * **Linux, Unix of OS X-clients**: Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, OS X of Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-clients**: Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Gebruik de volgende opdracht uit om een nieuw bestand te maken:

        nano useooziewf.hql

1. Zodra de nano-editor wordt geopend, gebruikt u de volgende handelingen uit als de inhoud van het bestand:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;

    Er zijn twee variabelen in het script gebruikt:

    - **${hiveTableName}**: bevat de naam van de tabel moet worden gemaakt
    - **${hiveDataFolder}**: de locatie voor het opslaan van de bestanden voor de tabel bevat

    De werkstroom definitiebestand (workflow.xml in deze zelfstudie) deze waarden worden doorgegeven aan dit script HiveQL tijdens runtime.

2. Druk op Ctrl-X om af te sluiten van de editor. Wanneer u wordt gevraagd, selecteert u het bestand wilt opslaan, **Y** en het gebruik van **Enter** om de bestandsnaam **useooziewf.hql** gebruiken.

3. Gebruik de volgende opdrachten **useooziewf.hql** kopiëren naar **wasbs:///tutorials/useoozie/useooziewf.hql**:

        hdfs dfs -copyFromLocal useooziewf.hql /tutorials/useoozie/useooziewf.hql

    Deze opdrachten Sla het bestand **useooziewf.hql** op de opslag van Azure-account dat is gekoppeld aan dit cluster waarin het bestand behouden blijft, zelfs als het cluster wordt verwijderd. Hiermee kunt u geld besparen door te clusters verwijderen wanneer ze worden niet gebruikt, behoud van uw taken en werkstromen.

##<a name="define-the-workflow"></a>De workflow definiëren

Oozie werkstromen definities zijn in hPDL (een XML-proces Definition Language) geschreven. Gebruik de volgende stappen uit om te definiëren van de werkstroom:

1. Gebruik de volgende-instructie maken en bewerken van een nieuw bestand:

        nano workflow.xml

1. Zodra de nano-editor wordt geopend, voert u de volgende handelingen uit als de inhoud van het bestand:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>
            <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.job.queue.name</name>
                    <value>${queueName}</value>
                </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
            </action>
            <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.compress.map.output</name>
                    <value>true</value>
                </property>
                </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveDataFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\t"</arg>
                <archive>sqljdbc41.jar</archive>
                </sqoop>
            <ok to="end"/>
            <error to="fail"/>
            </action>
            <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>
            <end name="end"/>
        </workflow-app>

    Er zijn twee acties die zijn gedefinieerd in de werkstroom:

    - **RunHiveScript**: dit is de startactie en wordt uitgevoerd de **useooziewf.hql** component script

    - **RunSqoopExport**: dit Hiermee exporteert u de gegevens die zijn gemaakt op basis van het script component met SQL-Database met Sqoop. Alleen wordt uitgevoerd als de actie **RunHiveScript** geslaagd is.

        > [AZURE.NOTE] Zie voor meer informatie over Oozie werkstroom en gebruiken van werkstroomacties [Apache Oozie 4.0 documentatie] [ apache-oozie-400] (voor HDInsight versie 3.0) of [Apache Oozie 3.3.2 documentatie] [ apache-oozie-332] (voor HDInsight versie 2.1).

    De werkstroom heeft dat verschillende items, zoals `${jobTracker}`, die wordt vervangen door de waarden die u in de taakdefinitie verderop in dit document.

    Bedenk ook de `<archive>sqljdbc4.jar</arcive>` vermelding in de sectie Sqoop. Hiermee wordt aangegeven wanneer Oozie dit archief beschikbaar maken voor Sqoop wanneer deze actie wordt uitgevoerd.

2. Gebruik Ctrl-X, en vervolgens **j** en op **Enter** op het bestand wilt opslaan.

3. Gebruik de volgende opdracht uit de **workflow.xml** -bestand kopiëren naar **wasbs:///tutorials/useoozie/workflow.xml**:

        hdfs dfs -copyFromLocal workflow.xml /tutorials/useoozie/workflow.xml

##<a name="create-the-database"></a>De database maken

Volg de stappen in het document [een SQL-Database maken](../sql-database/sql-database-get-started.md) om een nieuwe database te maken. Wanneer u de database maakt, gebruikt u __oozietest__ als de databasenaam. Maak een notitie van de naam die wordt gebruikt voor de database-server, ook als dit nodig in het volgende gedeelte.

###<a name="create-the-table"></a>De tabel maken

> [AZURE.NOTE] Er zijn tal van manieren verbinding maken met SQL-Database aan een tabel maken. De volgende stappen gebruiken [FreeTDS](http://www.freetds.org/) vanuit het cluster HDInsight.

3. Gebruik de volgende opdracht uit om te FreeTDS installeren op de cluster HDInsight:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Eenmaal FreeTDS is geïnstalleerd, gebruikt u de volgende opdracht verbinding maken met de SQL-Database-server die u eerder hebt gemaakt:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest

    U ontvangt uitvoer ongeveer als volgt uit:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

5. Aan de `1>` wordt gevraagd, voert u de volgende regels:

        CREATE TABLE [dbo].[mobiledata](
        [deviceplatform] [nvarchar](50),
        [count] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
        GO

    Wanneer de `GO` instructie wordt ingevoerd, de vorige beweringen wordt geëvalueerd. Hiermee maakt een nieuwe tabel met de naam **mobiledata** die door Sqoop naar worden geschreven.

    Gebruik de volgende handelingen uit om te bevestigen dat de tabel is gemaakt:

        SELECT * FROM information_schema.tables
        GO

    Hier ziet u uitvoer ongeveer als volgt uit:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo     mobiledata      BASE TABLE

8. Voer `exit` bij de `1>` vragen om het hulpprogramma tsql af te sluiten.

##<a name="create-the-job-definition"></a>De taakdefinitie maken

De taakdefinitie wordt beschreven waar vind ik de workflow.xml, evenals andere bestanden die worden gebruikt door de werkstroom (zoals useooziewf.hql.) Ook Hiermee definieert u de waarden voor eigenschappen binnen de werkstroom gebruikt en de bijbehorende bestanden.

1. Volg de volgende opdracht uit om het volledige adres WASB naar de standaard-opslag. Deze wordt gebruikt in het configuratiebestand in even:

        sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml

    Dit moet gegevens worden de volgende strekking geretourneerd:

        <name>fs.defaultFS</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>

    Sla de **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** waarde, zoals deze wordt gebruikt in de volgende stappen.

2. Volg de volgende opdracht uit om FQDN-naam van de headnode cluster. Hiermee wordt gebruikt voor het adres van uw JobTracker voor het cluster. Deze wordt gebruikt in het configuratiebestand in even:

        hostname -f

    Hiermee herstelt u informatie ongeveer als volgt uit:

        hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net

    De poort voor de JobTracker is 8050, zodat het volledige adres gebruiken voor de JobTracker **hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050**.

1. Gebruik de volgende handelingen uit om de configuratie van Oozie taak definitie te maken:

        nano job.xml

2. Zodra de nano-editor wordt geopend, gebruikt u de volgende handelingen uit als de inhoud van het bestand:

        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

          <property>
            <name>nameNode</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
          </property>

          <property>
            <name>jobTracker</name>
            <value>JOBTRACKERADDRESS</value>
          </property>

          <property>
            <name>queueName</name>
            <value>default</value>
          </property>

          <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
          </property>

          <property>
            <name>hiveScript</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
          </property>

          <property>
            <name>hiveTableName</name>
            <value>mobilecount</value>
          </property>

          <property>
            <name>hiveDataFolder</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
          </property>

          <property>
            <name>sqlDatabaseConnectionString</name>
            <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
          </property>

          <property>
            <name>sqlDatabaseTableName</name>
            <value>mobiledata</value>
          </property>

          <property>
            <name>user.name</name>
            <value>YourName</value>
          </property>

          <property>
            <name>oozie.wf.application.path</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
          </property>
        </configuration>

    * Vervang alle exemplaren van **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** met de waarde die u eerder hebt ontvangen.

    > [AZURE.WARNING] Met de container en opslag-account als onderdeel van het pad, moet u het volledige pad van de WASB gebruiken. Met de korte opmaak (wasbs: / / /), wordt de actie RunHiveScript mislukt wanneer de taak is gestart.

    * **JOBTRACKERADDRESS** vervangen door het JobTracker/ResourceManager-adres dat u eerder hebt ontvangen.

    * **Uwnaam** vervangen door uw aanmeldingsnaam voor het cluster HDInsight.

    * **Servernaam**, **adminLogin**en **beheerderswachtwoord** vervangen door de gegevens voor uw Azure SQL-Database.

    De meeste informatie in dit bestand wordt gebruikt om te vullen de waarden in de workflow.xml of ooziewf.hql-bestanden (zoals ${nameNode}.)

    > [AZURE.NOTE] Waar vind ik het bestand workflow.xml Hiermee definieert u de vermelding **oozie.wf.application.path** die de werkstroom bevat deze taak is uitgevoerd.

2. Gebruik Ctrl-X, en vervolgens **j** en op **Enter** op het bestand wilt opslaan.

##<a name="submit-and-manage-the-job"></a>Formuliergegevens verzenden en beheren van de taak

De volgende stappen gebruikt de opdracht Oozie indienen en beheren van Oozie werkstromen op het cluster. De opdracht Oozie is een gebruiksvriendelijke interface via de [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [AZURE.IMPORTANT] Wanneer u met de opdracht Oozie, moet u de FQDN-naam voor de headnode HDInsight. Deze FQDN is alleen toegankelijk zijn vanuit het cluster, of als het cluster zich op een virtueel netwerk van Azure op andere computers in hetzelfde netwerk.

1. Gebruik de volgende handelingen uit voor de URL voor de service Oozie:

        sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml

    Hiermee herstelt u een waarde ongeveer als volgt uit:

        <name>oozie.base.url</name>
        <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>

    Het gedeelte **http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie** is de URL voor gebruik met de opdracht Oozie.

2. Gebruik de volgende handelingen uit om te maken van een omgevingsvariabele voor de URL, zodat u niet hoeft te typen voor elke opdracht:

        export OOZIE_URL=http://HOSTNAMEt:11000/oozie

    Vervang de URL door het bereik dat u eerder hebt ontvangen.

3. Gebruik de volgende handelingen uit om in te dienen de taak:

        oozie job -config job.xml -submit

    Hiermee wordt de taakgegevens geladen vanuit **job.xml** en verstuurt dit naar Oozie, maar wordt niet uitgevoerd.

    Zodra de opdracht is voltooid, moeten de ID van de taak worden geretourneerd. Bijvoorbeeld `0000005-150622124850154-oozie-oozi-W`. Hiermee worden gebruikt voor het beheren van de taak.

4. Bekijk de status van de taak met de volgende opdracht. Voer in de taak-ID die het resultaat van de vorige opdracht:

        oozie job -info <JOBID>

    Hiermee herstelt u informatie ongeveer als volgt uit.

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasbs:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    Deze taak heeft een status van `PREP`, waarmee wordt aangegeven dat deze is verzonden, maar is nog niet gestart.

4. Gebruik de volgende handelingen uit om de taak te starten:

        oozie job -start JOBID

    Als u de status na deze opdracht controleren, dient actief en informatie voor de acties in de taak wordt geretourneerd.

5. Nadat de taak voltooid is, kunt u dat de gegevens is gegenereerd en geëxporteerd naar de tabel SQL-Database met behulp van de volgende opdrachten kunt controleren:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest

    Aan de `1>` wordt gevraagd, voert u de volgende handelingen uit:

        SELECT * FROM mobiledata
        GO

    U ontvangt informatie ongeveer als volgt uit:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Zie voor meer informatie over de opdracht Oozie, [Oozie opdrachtregel hulpmiddel](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

##<a name="oozie-rest-api"></a>Oozie REST API

De Oozie REST API kunt u uw eigen hulpmiddelen die met Oozie werken maken. Hier volgen HDInsight specifieke informatie over het gebruik van de Oozie REST API:

* **URI**: de REST API zijn toegankelijk vanaf buiten het cluster op`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **Verificatie**: U moet worden geverifieerd tot de API met HTTP-clusteraccount (admin), en hetzelfde wachtwoord. Bijvoorbeeld:

        curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions

Zie voor meer informatie over het gebruik van de Oozie REST API [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

##<a name="oozie-web-ui"></a>Oozie Web UI

De gebruikersinterface van de Web Oozie biedt een webweergave in de status van Oozie taken aan het cluster. Dit kunt u taakstatus, de taakdefinitie, configuratie, een grafiek van de acties weergeven in de taak en logboeken voor de taak. U kunt ook details voor acties binnen een project weergeven.

Voor toegang tot de gebruikersinterface van de Web Oozie, gebruikt u de volgende stappen uit:

1. Maak een tunnel SSH aan het cluster HDInsight. Zie [Gebruik SSH tunnel naar Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, en andere web van UI openen](hdinsight-linux-ambari-ssh-tunnel.md)voor informatie over hoe u dit wilt doen.

2. Nadat een tunnel is gemaakt, opent u het web Ambari UI in uw webbrowser. De URI voor de site Ambari is **https://CLUSTERNAME.azurehdinsight.net**. **CLUSTERNAAM** vervangen door de naam van uw cluster Linux gebaseerde HDInsight.

3. Selecteren vanaf de linkerkant van de pagina, **Oozie**, en vervolgens **Snelkoppelingen**en ten slotte **Oozie Web UI**.

    ![afbeelding van de menu 's](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. De gebruikersinterface van de Web Oozie standaard werkstroomtaken uitgevoerd weergeven. Alle werkstroomtaken, selecteert u **Alle taken**.

    ![Alle taken die worden weergegeven](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Selecteer een taak om meer informatie over de taak te bekijken.

    ![Taak info](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Op het tabblad taak Info ziet u eenvoudige taakgegevens, evenals de afzonderlijke acties binnen de taak. Met de tabbladen aan de bovenkant kunt u de definitie van de taak, taakconfiguratie, access het project-logboek weergeven of bekijken van een doorgestuurd acyclische Graph (DAG) van de taak.

    * **Taak Log**: Selecteer de knop **GetLogs** om alle logboeken voor de taak of het veld **Voer zoekfilter** gebruiken om te filteren Logboeken

        ![Taak log](./media/hdinsight-use-oozie-linux-mac/joblog.png)

    * **JobDAG**: de DAG is een grafisch overzicht van de gegevens uit die u hebt gemaakt via de werkstroom

        ![Taak DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Informatie voor de actie krijgt selecteren een van de acties op het tabblad **Taak Info** . Bijvoorbeeld, selecteert u de actie **RunHiveScript** .

    ![Actie info](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Hier ziet u details voor de actie, inclusief een koppeling naar de **Console-URL**, die kan worden gebruikt om JobTracker informatie voor de taak te bekijken.

##<a name="scheduling-jobs"></a>U taken plant

De coördinator kunt u opgeven van een begin, einde en exemplaar frequentie voor taken, zodat deze kunnen worden gepland voor bepaalde tijden.

Als u wilt een schema voor de werkstroom definiëren, gebruikt u de volgende stappen uit:

1. Gebruik de volgende handelingen uit om te maken van een nieuw bestand met de naam **coordinator.xml**:

        nano coordinator.xml

    Gebruik de volgende handelingen uit als de inhoud van het bestand:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
          <action>
            <workflow>
              <app-path>${workflowPath}</app-path>
            </workflow>
          </action>
        </coordinator-app>

    Opmerking Dit waarin `${...}` variabelen die wordt vervangen door waarden in de taakdefinitie. Zijn de variabelen:

    * **${coordFrequency}**: tijd tussen de sessies van de taak uitvoeren
    * **${coordStart}**: de begintijd van de taak
    * **${coordEnd}**: de eindtijd van de taak
    * **${coordTimezone}**: coördinator taken zijn in een vaste tijdzone geen zomertijd (meestal weergegeven met behulp van UTC). Deze tijdzone wordt verwezen als de "Oozie verwerking tijdzoneverschil"
    * **${wfPath}**: het pad naar de workflow.xml

2. Gebruik Ctrl-X, en vervolgens **j** en op **Enter** op het bestand wilt opslaan.

3. Gebruik de volgende manieren te werk om deze te kopiëren naar de werkmap voor deze taak:

        hadoop fs -copyFromLocal coordinator.xml /tutorials/useoozie/coordinator.xml

4. Gebruik de volgende handelingen uit om het bestand **job.xml** te wijzigen:

        nano job.xml

    De volgende wijzigingen aanbrengen:

    * Wijziging `<name>oozie.wf.application.path</name>` naar `<name>oozie.coord.application.path</name>`. Hiermee wordt aangegeven wanneer Oozie het bestand coördinator uitvoeren in plaats van de werkstroom-bestand

    * De volgende die sets een variabele gebruikt in de coordinator.xml om te verwijzen naar de locatie van de workflow.xml toevoegen:

            <property>
              <name>workflowPath</name>
              <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
            </property>

        De waarden voor **mycontainer** en **mystorageaccount** vervangen door de waarden in andere vermeldingen in het bestand job.xml gebruikt.

    * Het volgende voorbeeld, waarin de begin, einde en frequentie wilt gebruiken voor het bestand coordinator.xml definiëren toevoegen:

            <property>
              <name>coordStart</name>
              <value>2015-06-25T12:00Z</value>
            </property>

            <property>
              <name>coordEnd</name>
              <value>2015-06-27T12:00Z</value>
            </property>

            <property>
              <name>coordFrequency</name>
              <value>1440</value>
            </property>

            <property>
              <name>coordTimezone</name>
              <value>UTC</value>
            </property>

        Deze de begintijd tot 12:00 uur 25e juni 2015, de eindtijd voor juni 27th 2015 verlengt, en het interval voor het uitvoeren van deze taak op dagelijks instellen (de frequentie is in minuten, dus 24 uur x 60 minuten = 1440 minuten.) Ten slotte is het tijdzoneverschil ingesteld op UTC.

5. Gebruik Ctrl-X, en vervolgens **j** en op **Enter** op het bestand wilt opslaan.

6. Als u wilt de taak uitvoert, gebruikt u de volgende opdracht:

        oozie job -config job.xml -run

    Hiermee wordt indienen en start de taak.

7. Als u de gebruikersinterface van de Web Oozie bezoekt en selecteer het tabblad **Coördinator taken** , moet u informatie ongeveer als volgt uit:

    ![tabblad coördinator-taken](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    Noteer de vermelding van de **Volgende intreding** ; Dit is wanneer u de taak volgende wordt uitgevoerd.

8. Net als bij de eerdere werkstroomtaak, de vermelding van de taak selecteren in de webversie van UI alleen informatie weergegeven over de taak:

    ![Coördinator taak info](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    Houd er rekening mee dat dit alleen geslaagd uitgevoerd van de taak, geen afzonderlijke acties binnen de geplande werkstroom weergegeven. Om te zien die, selecteer een van de **actie** -items. Informatie die vergelijkbaar is met die voor de eerdere werkstroomtaak opgehaald wordt weergegeven.

    ![Actie info](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

##<a name="troubleshooting"></a>Problemen oplossen

Bij het oplossen van problemen met de Oozie, de Oozie UI is heel handig zijn als deze kunt u gemakkelijk weergeven beide logboeken Oozie, evenals koppelingen naar JobTracker logboeken voor MapReduce taken zoals component query's. In het algemeen, moet het patroon voor probleemoplossing:

1. U kunt u de taak in Oozie Web gebruikersinterface weergeven.

2. Als er een fout of mislukt voor een specifieke actie, selecteert u de actie om te zien als het veld **Foutbericht** vindt u meer informatie over de fout.

3. Als deze functie beschikbaar is, gebruikt u de URL van de actie om weer te geven nog meer informatie (zoals JobTracker zich aanmeldt,) voor de actie.

Hier volgen specifieke fouten die kunnen optreden en hoe u ze kunt oplossen.

###<a name="ja009-cannot-initialize-cluster"></a>JA009: Kan niet worden cluster geïnitialiseerd

**Symptomen**: de taakstatus wordt gewijzigd in **buiten werking**. Details voor de taak wordt de status RunHiveScript weergegeven als **START_MANUAL**. De actie selecteren, worden het volgende foutbericht weergegeven:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Oorzaak**: het WASB-adressen die worden gebruikt in het bestand **job.xml** niet bevatten de opslagruimte container of opslagaccountnaam. De notatie van de adressen WASB moet `wasbs://containername@storageaccountname.blob.core.windows.net`.

**Resolutie**: de WASB-adressen die worden gebruikt door de taak wijzigen.

###<a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a>JA002: Oozie is niet toegestaan nabootsen &lt;gebruiker >

**Symptomen**: de taakstatus wordt gewijzigd in **buiten werking**. Details voor de taak wordt de status RunHiveScript weergegeven als **START_MANUAL**. De actie selecteren, worden het volgende foutbericht weergegeven:

    JA002: User: oozie is not allowed to impersonate <USER>

**Oorzaak**: huidige machtigingsinstellingen Oozie nabootsen van de opgegeven gebruikersaccount niet toestaan.

**Resolutie**: Oozie is kunnen gebruikers in de groep **gebruikers** nabootsen. Gebruik de `groups USERNAME` om de groepen die het gebruikersaccount deel uit van maakt weer te geven. Als de gebruiker niet een lid van de groep **gebruikers is** , gebruikt u de volgende opdracht uit de gebruiker toevoegen aan de groep:

    sudo adduser USERNAME users

> [AZURE.NOTE] Het kan enkele minuten duren voordat HDInsight herkent dat de gebruiker is toegevoegd aan de groep.

###<a name="launcher-error-sqoop"></a>Startpictogram voor fout (Sqoop)

**Symptomen**: de taakstatus wordt gewijzigd in **KILLED**. Meer informatie voor de taak wordt de status RunSqoopExport als **fout**weergegeven. De actie selecteren, worden het volgende foutbericht weergegeven:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Oorzaak**: Sqoop kan het databasestuurprogramma is vereist voor toegang tot de database te laden.

**Resolutie**: wanneer u met Sqoop van een taak Oozie, moet u het databasestuurprogramma met de andere resources (zoals de workflow.xml) worden gebruikt door de taak opnemen.

U moet ook verwijzen naar het archief met het databasestuurprogramma uit de `<sqoop>...</sqoop>` gedeelte van de workflow.xml.

Bijvoorbeeld voor het project in dit document gebruikt u de volgende stappen uit:

1. Kopieer het bestand sqljdbc4.1.jar naar de map /tutorials/useoozie:

         hadoop fs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar

2. Wijzigen van de workflow.xml om toe te voegen van de volgende handelingen uit op een nieuwe regel bovenstaande `</sqoop>`:

        <archive>sqljdbc41.jar</archive>

##<a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u een Oozie workflow definiëren en hoe een taak Oozie uit te voeren. Meer informatie over het werken met HDInsight, raadpleegt u de volgende artikelen:

- [Op tijd gebaseerde Oozie coördinator gebruiken met HDInsight][hdinsight-oozie-coordinator-time]
- [Gegevens voor Hadoop-projecten in HDInsight uploaden][hdinsight-upload-data]
- [Sqoop gebruiken met Hadoop in HDInsight][hdinsight-use-sqoop]
- [Component gebruiken met Hadoop op HDInsight][hdinsight-use-hive]
- [Varken met Hadoop op HDInsight gebruiken][hdinsight-use-pig]
- [Ontwikkel Java MapReduce-programma's voor HDInsight][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]: storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
