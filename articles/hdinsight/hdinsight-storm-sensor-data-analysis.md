<properties
   pageTitle="Sensorgegevens analyseren met Apache Storm en HBase | Microsoft Azure"
   description="Leer hoe u verbinding maken met Apache Storm met een virtueel netwerk. Gebruik van Storm met HBase sensor om gegevens te verwerken van de hub van een gebeurtenis en deze visualiseren met D3.js."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Sensorgegevens analyseren met Apache Storm, gebeurtenis Hub en HBase in HDInsight (Hadoop) 

Informatie over het gebruik van Apache Storm aan HDInsight sensor om gegevens te verwerken van Azure gebeurtenis Hub, opslaan in Apache HBase op HDInsight en deze visualiseren met behulp van D3.js wordt uitgevoerd met een Azure-Web-App.

De resourcemanager Azure-sjabloon gebruikt in dit document laat zien hoe meerdere Azure resources in een resourcegroep maken. Specifiek, deze Hiermee maakt u een virtueel Azure-netwerk, twee HDInsight clusters (Storm en HBase) en een Azure-Web-App. Een node.js-implementatie van een webdashboard realtime wordt automatisch naar de web-app geïmplementeerd.

> [AZURE.NOTE] De informatie in dit document en het voorbeeld, is getest met Linux gebaseerde HDInsight 3.3 en 3,4 cluster versies.

## <a name="prerequisites"></a>Vereisten voor

* Een Azure-abonnement. Zie [Azure krijgen gratis proefversie](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

    > [AZURE.IMPORTANT] U hoeft niet een bestaand HDInsight cluster; de stappen in dit document maakt de volgende bronnen:
    >
    > * Een Azure Virtual Network
    > * Een Storm op HDInsight cluster (Linux gebaseerde 2 werknemer knooppunten)
    > * Een HBase op HDInsight cluster (Linux gebaseerde 2 werknemer knooppunten)
    > * Een Web-App van Azure waarop het webdashboard van het

* [Node.js](http://nodejs.org/): Hiermee wordt gebruikt om een voorbeeld van het webdashboard lokaal op uw ontwikkelomgeving te.

* [Java en de 1.7 JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html): gebruikt voor het ontwikkelen van de topologie Storm.

* [Maven](http://maven.apache.org/what-is-maven.html): gebruikt om te maken en compilatie van het project.

* [Cijfer](http://git-scm.com/): gebruikt om te downloaden van het project van GitHub.

* Een __SSH__ -client: verbinding maken met de HDInsight Linux gebaseerde clusters gebruikt. Zie de volgende documenten voor meer informatie over het gebruik van SSH met HDInsight.

    * [SSH gebruiken met HDInsight vanuit een Windows-client](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [SSH gebruiken met HDInsight van een client Linux, Unix of Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] U moet ook toegang hebben tot de `scp` opdracht, die wordt gebruikt om bestanden tussen uw lokale ontwikkelomgeving en het gebruik van SSH HDInsight-cluster te kopiëren.

## <a name="architecture"></a>Architectuur

![Architectuurdiagram](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

In dit voorbeeld bevat de volgende onderdelen:

* **Azure gebeurtenis Hubs**: gegevens bevat die worden verzameld van sensoren. In dit voorbeeld wordt een toepassing geleverd die de gegevens hebt gegenereerd.

* **Storm op HDInsight**: verschaft realtime verwerking van gegevens van gebeurtenis Hub.

* **HBase op HDInsight**: biedt een permanente NoSQL gegevensopslag voor gegevens nadat deze is verwerkt stormenderhand.

* **Azure virtuele netwerkservice**: beveiligde communicatie tussen de Storm op HDInsight en HBase op HDInsight clusters kunt.

    > [AZURE.NOTE] Een virtueel netwerk is vereist voor het gebruik van de Java HBase client-API, als deze niet wordt weergegeven via de openbare gateway voor HBase clusters. HBase en Storm clusters in hetzelfde virtuele netwerk te installeren, kunt het cluster Storm (of een ander op het virtuele netwerk) direct toegang tot HBase client-API gebruiken.

* **De website van het dashboard**: een voorbeelddashboard die gegevens in realtime grafieken.

    * De website is geïmplementeerd in Node.js, zodat deze kan worden uitgevoerd op de client-besturingssystemen voor het testen van of deze kan worden geïmplementeerd op Azure-Websites.

    * [Socket.IO](http://socket.io/) wordt gebruikt voor realtime communicatie tussen de topologie Storm en de website.

        > [AZURE.NOTE] Dit is een gedetailleerd implementatie. U kunt een communicatie-framework, zoals onbewerkte WebSockets of SignalR gebruiken.

    * [D3.js](http://d3js.org/) wordt gebruikt om de gegevens die wordt verzonden naar de website in een grafiek.

> [AZURE.IMPORTANT] Twee clusters zijn vereist, zoals het er is geen ondersteunde methode voor het maken van één HDInsight cluster voor zowel Storm en HBase.

De topologie leest gegevens van gebeurtenis Hub met behulp van de klasse [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) en schrijft gegevens naar HBase met de klasse [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) . Communicatie met de website wordt uitgevoerd door middel van [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java).

Hierna volgt een diagram van de topologie.

![topologiediagram](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] Dit is een zeer vereenvoudigde weergave van de topologie. Tijdens de uitvoering, wordt een exemplaar van elk onderdeel voor elke partition gemaakt voor de gebeurtenis Hub die wordt gelezen. Deze exemplaren zijn verdeeld over de knooppunten in het cluster en gegevens is doorgestuurd ertussen als volgt:
>
> * Gegevens uit de spout de parser is verdeeld.
> * Gegevens uit de parser naar het Dashboard en HBase is gegroepeerd op apparaat-ID, zodat berichten van hetzelfde apparaat flow altijd op één onderdeel.

### <a name="topology-components"></a>Zoektopologieonderdelen

* **EventHub Spout**: de spout is opgegeven als onderdeel van Apache Storm versie 0.10.0 en hoger.

    > [AZURE.NOTE] De gebeurtenis Hubs spout gebruikt in dit voorbeeld is een Storm op HDInsight cluster versie 3.3 of 3.4 vereist. Zie voor informatie over het gebruik van gebeurtenis Hubs met een oudere versie van Storm op HDInsight [procesgebeurtenissen van Azure gebeurtenis Hubs met Storm op HDInsight](hdinsight-storm-develop-java-event-hub-topology.md).

* **ParserBolt.java**: de gegevens die worden verstrekt door de spout is onbewerkte JSON en af en toe meer dan één gebeurtenis per keer wordt gegenereerd. Deze bout laat zien hoe de gegevens die is verstrekt door de spout lezen en verzenden naar een nieuwe stroom als een tupel die meerdere velden bevat.

* **DashboardBolt.java**: dit geeft aan hoe u van de bibliotheek van de client Socket.io voor Java gegevens te verzenden realtime naar de webdashboard.

Dit voorbeeld wordt het kader [lichtstroom](https://storm.apache.org/releases/0.10.0/flux.html) , zodat de definitie van de topologie deel van YAML-bestanden uitmaakt. Er zijn twee:

* __geen hbase.yaml__ - dit bestand bij het testen van de topologie in uw ontwikkelomgeving gebruiken. Niet gebruikt HBase onderdelen, omdat u geen toegang de API HBase Java van buiten het virtuele netwerk dat het cluster worden opgeslagen tot in.

* __met hbase.yaml__ - dit bestand bij het distribueren van de topologie aan het cluster Storm gebruiken. HBase onderdelen gebruikt de functie omdat deze wordt uitgevoerd in hetzelfde virtuele netwerk als het cluster HBase.

## <a name="prepare-your-environment"></a>Voorbereiden van uw omgeving

Voordat u dit voorbeeld gebruiken, moet u een Azure gebeurtenis-Hub, waarin de topologie Storm van leest maken.

### <a name="configure-event-hub"></a>Gebeurtenis-Hub configureren

Gebeurtenis-Hub is de gegevensbron voor dit voorbeeld. Gebruik de volgende stappen uit om te maken van een nieuwe gebeurtenis Hub.

1. Selecteer in de [portal van Azure](https://portal.azure.com) **+ Nieuw** -> __Internet van zaken__ -> __Gebeurtenis Hubs__.

2. Klik op het blad __Namespace maken__ , moet u de volgende taken uitvoeren:

    1. Voer een __naam__ voor de naamruimte.
    2. Selecteer een prijzen laag. __Eenvoudige__ is voldoende voor dit voorbeeld.
    3. Selecteer het Azure __abonnement__ te gebruiken.
    4. Selecteer een bestaande resourcegroep of een nieuw account te maken.
    5. Selecteer de __locatie__ voor de Hub van de gebeurtenis.
    6. Selecteer __vastmaken aan dashboard__en klik vervolgens op __maken__.

3. Wanneer het maakproces is voltooid, wordt het blad gebeurtenis Hubs voor de naamruimte weergegeven. Selecteer __+ toevoegen gebeurtenis Hub__hier. Klik op het blad __Gebeurtenis Hub maken__ , voer een naam in van __sensordata__ en selecteer __maken__. Laat de andere velden bij de standaardwaarden.

4. Selecteer in het blad gebeurtenis Hubs voor de naamruimte, __Gebeurtenis Hubs__. Selecteer het item __sensordata__ .

5. Selecteer in het blad voor de sensordata gebeurtenis-Hub, __clienttoegangsbeleid gedeeld__. Gebruik de koppeling __+ toevoegen__ om toe te voegen van de volgende beleidsitems:


  	| De beleidsnaam van het | Op claims |
  	| ----- | ----- |
  	| apparaten | Verzenden |
  	| storm | Luisteren |

5. Selecteer beide beleidsregels en noteert u de __Primaire__ -sleutelwaarde. Moet u de waarde voor beide beleidsregels in toekomstige stappen.

## <a name="download-and-configure-the-project"></a>Download en configureren van het project

Gebruik de volgende handelingen uit om te downloaden van het project van GitHub.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Nadat de opdracht is voltooid, hebt u de structuur van de volgende map:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
            src/ - the Java bolts
            dev.properties - contains configuration values for your environment
        dashboard/nodejs/ - this is the node.js web dashboard
        SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] In dit document niet naar volledige details van de code die is opgenomen in dit voorbeeld; de code is echter volledig toegelicht.

Open het bestand **hdinsight-eventhub-example/TemperatureMonitor/dev.properties** en uw gegevens gebeurtenis Hub toevoegen aan de volgende regels:

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] In dit voorbeeld wordt ervan uitgegaan dat u __storm__ gebruikt als de naam van het beleid dat een claim __beluisteren__ heeft en dat uw Hub gebeurtenis __sensordata__naam.

 Sla het bestand nadat u deze informatie hebt toegevoegd.

## <a name="compile-and-test-locally"></a>Compileren en lokaal te testen

Voordat u testen, moet u het dashboard om te bekijken van de uitvoer van de topologie en gegevens worden opgeslagen in de Hub gebeurtenis genereren starten.

> [AZURE.IMPORTANT] Het onderdeel HBase van deze topologie is niet actief terwijl testen lokaal, als de Java-API voor het cluster HBase niet toegankelijk zijn vanuit buiten het Azure virtuele netwerk waarmee de clusters bevat.

### <a name="start-the-web-application"></a>Start de webtoepassing

1. Open een nieuwe opdrachtprompt of terminal en mappen wijzigen in het **hdinsight-eventhub-/ voorbeelddashboard**en het gebruik van de volgende opdracht voor het installeren van de afhankelijkheden die nodig zijn voor de webtoepassing:

        npm install

2. Gebruik de volgende opdracht uit de webtoepassing te starten:

        node server.js

    Hier ziet u een bericht ongeveer als volgt uit:

        Server listening at port 3000

2. Open een webbrowser en voer **http://localhost:3000 /** als het adres. Hier ziet u een pagina ongeveer als volgt uit:

    ![webdashboard](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    Deze opdrachtprompt of terminal open laten. Na het testen, gebruik u Ctrl + C om te stoppen de webserver.

### <a name="start-generating-data"></a>Beginnen met het genereren van gegevens

> [AZURE.NOTE] De stappen in deze sectie gebruik Node.js zodat ze kunnen worden gebruikt op een willekeurig platform. Zie de map **SendEvents** voor andere voorbeelden taal.

1. Open een nieuwe opdrachtprompt, shell of terminal en mappen wijzigen in **hdinsight-eventhub-voorbeeld/SendEvents/nodejs**en het gebruik van de volgende opdracht uit om de afhankelijkheden die nodig zijn voor de toepassing te installeren:

        npm install

2. Open het bestand **app.js** in een teksteditor en voer de gebeurtenis Hub-gegevens die u eerder hebt aangeschaft:

        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
    
    > [AZURE.NOTE] In dit voorbeeld wordt ervan uitgegaan dat u __sensordata__ als de naam van de gebeurtenis Hub en __apparaten__ hebt gebruikt als de naam van het beleid dat een claim __verzenden heeft__ .

2. Gebruik de volgende opdracht uit om in te voegen van nieuwe items in de Hub gebeurtenis:

        node app.js

    Hier ziet u meerdere regels van uitvoer die de gegevens die zijn verzonden naar gebeurtenis Hub bevatten. Deze ziet er ongeveer als volgt uit:

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>De topologie starten

2. Open een nieuwe opdrachtprompt, shell of terminal en wijzigt u mappen naar __Hdinsight-eventhub-voorbeeld/TemperatureMonitor__, en gebruik vervolgens de volgende opdracht uit de topologie starten:

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    Als u PowerShell gebruikt, gebruikt in plaats daarvan het volgende:

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] Als u zich op een Unix-Linux/OS X-systeem en [Storm in uw ontwikkelomgeving](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html)geïnstalleerd, kunt u in plaats daarvan de volgende opdrachten gebruiken:
    >
    > `mvn compile package`
    > `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

    Hiermee wordt de topologie gedefinieerd in het bestand __geen hbase.yaml__ in de lokale modus gestart. De waarden in het bestand __dev.properties__ bieden de verbindingsgegevens voor gebeurtenis Hubs. Eenmaal gestart, de topologie vermeldingen van gebeurtenis Hub leest en verzendt deze aan het dashboard uitgevoerd op uw lokale computer. Hier ziet u regels in het webdashboard, dat u de volgende strekking weergegeven:

    ![dashboard met gegevens](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. Terwijl het dashboard actief is, gebruikt u de `node app.js` command uit de vorige stap nieuwe gegevens te verzenden naar gebeurtenis Hubs. Omdat de temperatuur-waarden worden willekeurig gegenereerd, wordt de grafiek moet bijwerken zodat grote wijzigingen in de temperatuur.

    > [AZURE.NOTE] U moet zich in de map __Hdinsight-eventhub-voorbeeld/SendEvents/Nodejs__ bij gebruik van de `node app.js` opdracht.

3. Na het bevestigt dat dit werkt, stoppen de topologie met Ctrl + C. Ctrl + C kunt u ook de lokale webserver stoppen.

## <a name="create-a-storm-and-hbase-cluster"></a>Een cluster Storm en HBase maken

Om de topologie worden uitgevoerd op HDInsight en de bout HBase inschakelen, moet u een nieuwe Storm cluster en HBase cluster maken. De stappen in deze sectie met een [Azure resourcemanager sjabloon](../resource-group-template-deploy.md) kunt maken een nieuw Azure Virtual Network en een Storm en HBase cluster in de virtuele netwerk. De sjabloon ook Hiermee maakt u een Azure-Web-App en een kopie van het dashboard implementeren erin.

> [AZURE.NOTE] Een virtueel netwerk wordt gebruikt, zodat de topologie uitgevoerd op de cluster Storm rechtstreeks kan communiceren met het HBase cluster de HBase Java-API gebruiken.

De resourcemanager sjabloon gebruikt in dit document bevindt zich in een container openbare blob bij __https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__.

1. Klik op de volgende knop om te melden bij Azure en open de sjabloon resourcemanager in de portal van Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende gegevens van het blad **Parameters** :

    ![HDInsight parameters](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**: deze waarde wordt gebruikt als de naam voor de Storm en HBase clusters. Bijvoorbeeld maakt __hdi__ invoeren een Storm cluster __storm-hdi__ met de naam en een HBase cluster __hbase-hdi__met de naam.
    * __CLUSTERLOGINUSERNAME__: de beheerder-gebruikersnaam voor de clusters Storm en HBase.
    * __CLUSTERLOGINPASSWORD__: de gebruiker beheerderswachtwoord voor de clusters Storm en HBase.
    * __SSHUSERNAME__: het SSH gebruiker te maken voor de clusters Storm en HBase.
    * __SSHPASSWORD__: het wachtwoord voor de gebruiker SSH voor de clusters Storm en HBase.
    * __Locatie__: het gebied dat de clusters wordt gemaakt in.
    
    Klik op __OK__ als u wilt opslaan van de parameters.
    
3. De sectie __resourcegroep__ gebruiken om een nieuwe resourcegroep maken of selecteren van een bestaande eigenschap.

4. Selecteer in het vervolgkeuzemenu __Resource groep locatie__ dezelfde locatie als u hebt geselecteerd voor de parameter __locatie__ .

5. Selecteer de __juridische voorwaarden__en selecteer __maken__.

6. Tot slot __vastmaken aan dashboard__ controleren en selecteer vervolgens __maken__. Het duurt ongeveer 20 minuten clusters maken.

Wanneer de resources zijn gemaakt, wordt u omgeleid naar een blade voor de resourcegroep met de clusters en webdashboard.

![Resource-groep blade voor de vnet en clusters](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] U ziet dat de namen van de clusters HDInsight __storm-BASENAME__ en __hbase-BASENAME__, waarbij BASENAME is de naam die u aan de sjabloon. U gebruikt deze namen in de volgende stappen wanneer u verbinding maakt met de clusters. Bedenk ook dat de naam van de dashboard-site __basename-dashboard is__. Gebruikt u dit later bij het weergeven van het dashboard.

## <a name="configure-the-dashboard-bolt"></a>De bout Dashboard configureren

Om te kunnen gegevens verzendt naar het dashboard is geïmplementeerd als een web-app, moet u de volgende regel in het bestand __dev.properties__ wijzigen:

    dashboard.uri: http://localhost:3000

Wijziging `http://localhost:3000` naar `http://BASENAME-dashboard.azurewebsites.net` en sla het bestand. __BASENAME__ vervangen door de naam van de die u in de vorige stap hebt opgegeven. U kunt ook de resourcegroep eerder hebt gemaakt om te selecteren van het dashboard en de URL te bekijken.

## <a name="create-the-hbase-table"></a>De tabel HBase maken

Om te kunnen gegevens opslaan in HBase, moeten we eerst een tabel maken. U doorgaans wilt vooraf maken resources die nog moet worden Storm ernaar te schrijven, als het maken van resources uit binnen een topologie Storm kunnen resulteren in meerdere, verdeelde kopieën van de code die bij het maken van dezelfde resource. De resources buiten de topologie maken en alleen Storm gebruiken voor lezen/schrijven en analyses.

1. Gebruik SSH verbinding maken met de HBase cluster met SSH gebruiker en hetzelfde wachtwoord die u hebt verstrekt aan de sjabloon gemaakt. Als u verbinding maken met bijvoorbeeld de `ssh` opdracht, gebruikt u de volgende syntaxis:

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    In deze opdracht kunt u __gebruikersnaam__ vervangen door de SSH-gebruikersnaam die u hebt opgegeven toen het cluster en __BASENAME__ maken met de naam van de opgegeven. Wanneer u wordt gevraagd, voer het wachtwoord voor de gebruiker SSH.

2. Start de shell HBase op de sessie SSH.

        hbase shell
    
    Nadat de shell is geladen, ziet u een `hbase(main):001:0>` prompt.

3. Voer de volgende opdracht uit om een tabel om op te slaan de sensorgegevens te maken uit de shell HBase:

        create 'SensorData', 'cf'

4. Controleer of dat de tabel is gemaakt met behulp van de volgende opdracht uit:

        scan 'SensorData'
        
    Hiermee geeft informatie vergelijkbaar met het volgende voorbeeld, die aangeeft dat er 0 rijen in de tabel.
    
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Voer de volgende handelingen uit als u wilt de shell HBase afsluiten:

        exit

## <a name="configure-the-hbase-bolt"></a>De bout HBase configureren

Als u wilt schrijven naar HBase uit het cluster Storm, moet u de configuratiedetails van uw cluster HBase de bout HBase bieden. De eenvoudigste manier hiervoor is het downloaden van de __hbase-site.xml__ uit het cluster en deze opnemen in uw project. U moet ook opmerkingen bij verschillende afhankelijkheden in het bestand __pom.xml__ , waarin de storm-hbase onderdeel en de vereiste afhankelijkheden laden.

> [AZURE.IMPORTANT] U moet ook het storm-hbase.jar-bestand op uw Storm op HDInsight cluster 3.3 of 3.4 cluster; downloaden in deze versie is gecompileerd als u wilt werken met HBase 1.1.x, die wordt gebruikt voor HBase op HDInsight 3.3 en 3,4 clusters. Als u een onderdeel storm-hbase elders gebruikt, kan deze worden gecompileerd ten opzichte van een oudere versie van HBase.

### <a name="download-the-hbase-sitexml"></a>De hbase-site.xml downloaden

Gebruik SCP naar het bestand __hbase-site.xml__ downloaden via het cluster in een opdrachtprompt. In het volgende voorbeeld, kunt u __gebruikersnaam__ vervangen door de SSH-gebruiker die u hebt opgegeven toen het cluster en __BASENAME__ maken met de naam van de die u eerder hebt opgegeven. Wanneer u wordt gevraagd, voer het wachtwoord voor de gebruiker SSH. Vervang de `/path/to/TemperatureMonitor/resources/hbase-site.xml` door het pad naar dit bestand in het project TemperatureMonitor.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

Hiermee wordt de __hbase-site.xml__ gedownload naar het opgegeven pad.

### <a name="download-and-install-the-storm-hbase-component"></a>Download en installeer het onderdeel storm-hbase

1. Gebruik in een opdrachtprompt SCP naar het bestand __storm-hbase.jar__ downloaden via het cluster Storm. In het volgende voorbeeld, kunt u __gebruikersnaam__ vervangen door de SSH-gebruiker die u hebt opgegeven toen het cluster en __BASENAME__ maken met de naam van de die u eerder hebt opgegeven. Wanneer u wordt gevraagd, voer het wachtwoord voor de gebruiker SSH.

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    Hiermee wordt een bestand met de naam downloaden `storm-hbase-####.jar`, waarbij ### het versienummer van Storm voor dit cluster. Noteer dit nummer, zoals het later wordt gebruikt.

2. Gebruik de volgende opdracht voor het installeren van dit onderdeel in de lokale Maven-bibliotheek op uw ontwikkelomgeving. Hiermee worden Maven om te zoeken van het pakket bij het compileren van het project. Vervang __####__ met het versienummer is opgenomen in de bestandsnaam.

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    Als u PowerShell gebruikt, gebruikt u de volgende opdracht uit:

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### <a name="enable-the-storm-hbase-component-in-the-project"></a>Inschakelen van het onderdeel storm-hbase in het project

1. Open het bestand __TemperatureMonitor/pom.xml__ en verwijder de volgende regels:

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] Alleen deze twee regels; verwijderen een van de lijnen tussen deze niet verwijderen.
    
    Hiermee worden verschillende onderdelen die nodig zijn bij communicatie met gebruik van de bout hbase HBase.

2. Zoek naar de volgende regels en vervang __####__ met het versienummer van de storm-hbase-bestand dat u eerder hebt gedownload.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] Het versienummer moet overeenkomen met de versie die u hebt gebruikt bij het installeren van het onderdeel naar de lokale Maven-bibliotheek, zoals Maven deze gegevens gebruikt laden van het onderdeel bij het maken van het project.

2. Sla het bestand __pom.xml__ .

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Bouwen, pakket en implementeren van de oplossing met HDInsight

Gebruik de volgende stappen uit om te implementeren van de topologie Storm aan het cluster storm in uw ontwikkelomgeving.

1. Gebruik de volgende opdracht uit te voeren een nieuwe build een oppervlak-pakket maken van uw project uit de adreslijst __TemperatureMonitor__ :

        mvn clean compile package

    Hiermee maakt u een bestand met de naam **TemperatureMonitor-1.0-SNAPSHOT.jar** in **de doelmap van uw project** .

2. Gebruik scp om het __TemperatureMonitor-1.0-SNAPSHOT.jar__ -bestand uploaden naar uw cluster Storm. In het volgende voorbeeld, kunt u __gebruikersnaam__ vervangen door de SSH-gebruiker die u hebt opgegeven toen het cluster en __BASENAME__ maken met de naam van de die u eerder hebt opgegeven. Wanneer u wordt gevraagd, voer het wachtwoord voor de gebruiker SSH.

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] Het kan enkele minuten duren voor het uploaden van het bestand, zoals deze verschillende MB worden.

    Denk scp voor het uploaden van het bestand __dev.properties__ , als dit document bevat de informatie die wordt gebruikt om te verbinden met gebeurtenis Hubs en het dashboard.

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. Nadat de bestanden die zijn geüpload, moet u een verbinding maken met het cluster via SSH.

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. Gebruik de volgende opdracht uit te starten van de topologie van de sessie SSH.

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    Hiermee wordt de topologie met de definitie van de topologie in het bestand __met hbase.yaml__ en de configuratiewaarden in het bestand __dev.properties__ gestart.

3. Nadat de topologie is gestart, opent u een browser naar de website die u hebt gepubliceerd op Azure, gebruikt u de `node app.js` opdracht gegevens te verzenden naar gebeurtenis Hub. Hier ziet u de webdashboard bijwerken om de gegevens weer te geven.

    ![dashboard](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>Gegevens van de HBase weergeven

Nadat u de gegevens over het gebruik van de topologie hebt ingediend `node app.js`, gebruikt u de volgende stappen uit om te verbinden met HBase en verifieer dat de gegevens is geschreven aan de tabel die u eerder hebt gemaakt.

1. Gebruik SSH verbinding maken met de cluster HBase.

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. Start de shell HBase op de sessie SSH.

        hbase shell
    
    Nadat de shell is geladen, ziet u een `hbase(main):001:0>` prompt.

2. Weergave rijen uit de tabel:

        scan 'SensorData'
        
    Dit moet worden geretourneerd informatie vergelijkbaar met de volgende opties, die aangeeft dat er 0 rijen in de tabel.
    
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds

    > [AZURE.NOTE] Deze scanbewerking retourneert alleen een maximum van 10 rijen uit de tabel.

## <a name="delete-your-clusters"></a>Clusters verwijderen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Verwijderen als u wilt verwijderen in één keer de clusters, opslag- en WebApp, de resourcegroep dat ze bevat.

## <a name="next-steps"></a>Volgende stappen

U hebt nu geleerd hoe u gegevens van de gebeurtenis Hubs lezen, opslaan in HBase en de gegevens op een externe-dashboard met behulp van Socket.io en D3.js visualiseren met Storm.

* Zie voor meer voorbeelden van Storm topologieën met HDInsight:

    * [Voorbeeld topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md)

* Zie voor meer informatie over Apache Storm, de [Apache Storm](https://storm.incubator.apache.org/) -site.

* Zie voor meer informatie over HBase op HDInsight, de [HBase met HDInsight overzicht](hdinsight-hbase-overview.md).

* Zie voor meer informatie over Socket.io, de site [socket.io](http://socket.io/) .

* Zie voor meer informatie over D3.js, [D3.js - gegevens basis van hoeveelheid werk documenten](http://d3js.org/).

* Zie voor informatie over het maken van topologieën in Java [ontwikkelen Java topologieën voor Apache Storm op HDInsight](hdinsight-storm-develop-java-topology.md).

* Zie voor informatie over het maken van topologieën in .NET [ontwikkelen C# topologieën voor Apache Storm op HDInsight gebruik van Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
