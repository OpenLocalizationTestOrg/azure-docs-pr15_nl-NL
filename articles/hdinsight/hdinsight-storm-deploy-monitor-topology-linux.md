<properties
   pageTitle="Implementeren en te beheren Apache Storm topologieën op Linux gebaseerde HDInsight | Microsoft Azure"
   description="Leer hoe u implementeren, controleren en beheren van Apache Storm topologieën met het Dashboard Storm op Linux gebaseerde HDInsight. Gebruik Hadoop tools voor Visual Studio."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Implementeren en Apache Storm topologieën op Linux gebaseerde HDInsight beheren

Informatie over de basisbeginselen van beheren en controleren van Storm topologieën waarop Linux gebaseerde Storm op HDInsight clusters in dit document.

> [AZURE.IMPORTANT] De stappen in dit artikel moet een Storm Linux gebaseerde op HDInsight cluster. Zie voor informatie over implementeert en de controle topologieën op Windows gebaseerde HDInsight [distribueren en beheren van Apache Storm topologieën op Windows gebaseerde HDInsight](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>Vereisten voor

* **A Linux gebaseerde Storm op HDInsight cluster**: Zie [aan de slag met Apache Storm op HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) voor stapsgewijze instructies voor het maken van een cluster

* **Bekend zijn met SSH en SCP**: Zie voor meer informatie over het gebruik van SSH en SCP met HDInsight, de volgende handelingen uit:

    * **Linux, Unix of OS X-clients**: Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, OS X of Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-clients**: Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

* **Een SCP-client**: dit is opgegeven met alle Linux, Unix en OS X-systemen. Voor Windows-clients raden PSCP, die beschikbaar zijn vanaf de [downloadpagina stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)is.

## <a name="start-a-storm-topology"></a>De topologie van een Storm starten

### <a name="using-ssh-and-the-storm-command"></a>SSH en de opdracht Storm

1. Gebruik SSH verbinding maken met de cluster HDInsight. **Gebruikersnaam** vervangen het de naam van uw aanmelding SSH. **CLUSTERNAAM** vervangen door de naam van uw HDInsight cluster:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Zie de volgende documenten voor meer informatie over het gebruik van SSH verbinding maken met uw cluster HDInsight:
    
    * **Linux, Unix of OS X-clients**: Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, OS X of Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Windows-clients**: Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Gebruik de volgende opdracht uit een topologie starten:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    Hiermee start u de topologie van de WordCount voorbeeld op het cluster. Er wordt willekeurig zinnen genereren en tellen van elk woord in de zinnen.

    > [AZURE.NOTE] Bij het verzenden van topologie aan het cluster, moet u eerst het oppervlak-bestand met het cluster vóór het gebruik van kopiëren de `storm` opdracht. Dit kan worden uitgevoerd met de `scp` command vanuit de client waar het bestand zich bevindt. Bijvoorbeeld:`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Het voorbeeld WordCount en andere voorbeelden van de starter storm staan al op uw cluster bij `/usr/hdp/current/storm-client/contrib/storm-starter/`.

### <a name="programmatically"></a>Via programmacode

U kunt een topologie via programmacode implementeren naar Storm op HDInsight door te communiceren met de Nimbus-service in uw cluster gehost. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-storm-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) biedt een voorbeeld Java-toepassing die laat hoe u zien kunt implementeren en een topologie via de Nimbus-service starten.

## <a name="monitor-and-manage-using-the-storm-command"></a>Bewaken en beheren met de opdracht storm

De `storm` hulpprogramma kunt u werken met topologieën vanaf de opdrachtregel worden uitgevoerd. Hier volgt een lijst met veelgebruikte opdrachten. Gebruik `storm -h` voor een volledige lijst met opdrachten.

### <a name="list-topologies"></a>Lijst topologieën

Gebruik de volgende opdracht uit voor een overzicht van alle gestart topologieën:

    storm list
    
Hiermee herstelt u informatie ongeveer als volgt uit:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Deactiveer en opnieuw activeren

Het deactiveren van een topologie onderbreekt deze totdat deze worden beëindigd of opnieuw geactiveerd. Gebruik de volgende handelingen uit om te deactiveren en opnieuw activeren:

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Een actieve topologie beëindigen

Storm topologieën, één keer wordt gestart, blijft uitgevoerd totdat gestopt. Als u wilt stoppen met een topologie, gebruikt u de volgende opdracht:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Vastleggen

Opnieuw een topologie, kan het systeem voor het bijwerken van de evenwijdigheid van de topologie. Als u het formaat van het cluster om toe te voegen meer notities hebt gewijzigd, opnieuw wordt kunt bijvoorbeeld een actieve topologie om te gebruiken van de nieuwe knooppunten.

> [AZURE.WARNING] Opnieuw een topologie eerst de topologie, deactiveert en vervolgens opnieuw werknemers gelijkmatig verdeeld over de cluster tot slot en geeft vervolgens de topologie naar de status die deze verkeerde voordat opnieuw is opgetreden. Dus als de topologie actief is, deze worden actieve opnieuw. Als deze is gedeactiveerd, blijft het gedeactiveerd.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Controleren en beheren met behulp van de gebruikersinterface Storm

De gebruikersinterface Storm een webservice-interface biedt voor het werken met topologieën uitgevoerd en is opgenomen in uw cluster HDInsight. Als u wilt weergeven in de gebruikersinterface Storm, gebruikt u een webbrowser om te openen __https://CLUSTERNAME.azurehdinsight.net/stormui__, waar __CLUSTERNAAM__ is de naam van uw cluster.

> [AZURE.NOTE] Als u wordt gevraagd een gebruikersnaam en wachtwoord op te geven, voert u de beheerder van het cluster (admin) en het wachtwoord dat u wanneer gebruikt u het cluster maakt.


### <a name="main-page"></a>Hoofdpagina

De hoofdpagina van de gebruikersinterface Storm biedt de volgende informatie:

* **Cluster samenvatting**: basisinformatie over het cluster Storm.

* **Topologie overzicht**: een lijst met actieve topologieën. Gebruik de koppelingen in deze sectie meer informatie over specifieke topologieën kunnen bekijken.

* **Toezichthouder samenvatting**: informatie over de toezichthouder Storm.

* **Configuratie van nimbus**: Nimbus configuratie voor het cluster.

### <a name="topology-summary"></a>Topologie samenvatting

Selecteren van een koppeling in de sectie **topologie samenvatting** bevat de volgende informatie over de topologie:

* **Topologie samenvatting**: basisinformatie over de topologie.

* **Topologie acties**: Management acties die u voor de topologie uitvoeren kunt.

  * **Activeren**: cv's verwerking van een gedeactiveerd topologie.

  * **Deactiveren**: een actieve topologie onderbreekt.

  * **Vastleggen**: Hiermee past u de evenwijdigheid van de topologie. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u lopende topologieën vastleggen. Hierdoor wordt de topologie om aan te passen parallellisme ter voor het betere of beperktere aantal knooppunten in het cluster.

    Zie <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">informatie over de evenwijdigheid van de topologie van een Storm</a>voor meer informatie.

  * **Beëindigen**: beëindigt de topologie van een Storm na de opgegeven time-out.

* **Topologie stat**: statistieken over de topologie. Gebruik de koppelingen in de kolom van het **venster** voor het instellen van de tijdsperiode voor de resterende items op de pagina.

* **Spouts**: de spouts die worden gebruikt door de topologie. Gebruik de koppelingen in deze sectie meer informatie over specifieke spouts kunnen bekijken.

* **Bolts**: de bouten die worden gebruikt door de topologie. Gebruik de koppelingen in deze sectie meer informatie over specifieke bouten kunnen bekijken.

* **Configuratie van de topologie**: de configuratie van de geselecteerde topologie.

### <a name="spout-and-bolt-summary"></a>Spout en bout overzicht

Een spout selecteren in de secties **Spouts** of **Bolts** , wordt de volgende informatie over het geselecteerde item weergegeven:

* **Onderdeel samenvatting**: basisinformatie over de spout of bout.

* **Spout/bout stat**: statistieken over de spout of bout. Gebruik de koppelingen in de kolom van het **venster** voor het instellen van de tijdsperiode voor de resterende items op de pagina.

* **Invoer stat** (alleen bout): informatie over de invoer-streams dat door de bout.

* **Uitvoer stat**: informatie over de streams dat door dit spout of bout.

* **Executors**: informatie over de instanties van het spout of bout. Selecteer het item **poort** voor een specifieke executor een logboek diagnostische gegevens geproduceerd voor dit exemplaar te bekijken.

* **Fouten**: een foutgegevens hiervoor spout of bout.

## <a name="rest-api"></a>REST API

De gebruikersinterface Storm is gebouwd met de REST API, zodat u uitvoeren kunt, dezelfde management en functionaliteit bewaken met behulp van de REST API. U kunt de REST API gebruiken om te maken van aangepaste hulpmiddelen voor het beheren en controleren van Storm topologieën.

Zie [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html)voor meer informatie. De volgende informatie hoort bij het gebruik van de REST API met Apache Storm op HDInsight.

> [AZURE.IMPORTANT] De Storm REST API is niet openbaar beschikbaar via internet en moet worden geopend met een tunnel SSH het knooppunt HDInsight cluster hoofd. Voor informatie over het maken en gebruiken van een tunnel SSH, Zie [Gebruik SSH tunnel naar Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, en andere web van UI openen](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Base URI

De basis-URI voor de REST API op HDInsight Linux gebaseerde clusters is beschikbaar op het hoofd knooppunt in **https://HEADNODEFQDN:8744/api/v1/**; echter de domeinnaam van het hoofd knooppunt wordt gegenereerd tijdens het maken van cluster en is niet statische.

U kunt de volledig gekwalificeerde domeinnaam (FQDN) voor het hoofd knooppunt op verschillende manieren vinden:

* __Vanuit een SSH sessie__: Gebruik de opdracht `headnode -f` uit een sessie SSH aan het cluster.

* __Van Ambari Internet__: __Services__ vanaf de bovenkant van de pagina selecteert en vervolgens __Storm__. Selecteer op het tabblad __Overzicht__ __Storm UI-Server__. De FQDN-naam van het knooppunt dat de Storm gebruikersinterface en de REST API wordt uitgevoerd op zich aan het begin van de pagina.

* __Uit Ambari REST API__: Gebruik de opdracht `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` om informatie over het knooppunt dat de Storm UI en de REST API worden uitgevoerd op te halen. __Wachtwoord__ vervangen door de admin-wachtwoord voor het cluster. __CLUSTERNAAM__ vervangen door de naam van de cluster. In het antwoord bevat het fragment "hostnaam" de FQDN-naam van het knooppunt.

### <a name="authentication"></a>Verificatie

Aanvragen voor de REST API moeten **Basisverificatie**, gebruiken zodat u de naam van de beheerder cluster HDInsight en het wachtwoord gebruiken.

> [AZURE.NOTE] Omdat basisverificatie is verzonden via uitschakelen voor tekst, moet u **altijd** gebruik HTTPS communicatie met het cluster beveiligen.

### <a name="return-values"></a>Retourwaarden

Informatie die wordt geretourneerd uit de REST API mogelijk alleen best uit in de cluster of virtuele machines op hetzelfde Azure virtuele netwerk als het cluster. Bijvoorbeeld de FQDN-naam (Fully Qualified) als resultaat gegeven voor Zookeeper-servers worden niet toegankelijk zijn vanuit Internet.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe om te implementeren en topologieën controleren met behulp van het Dashboard Storm wordt uitgelegd hoe u [ontwikkelen Java gebaseerde topologieën Maven gebruiken](hdinsight-storm-develop-java-topology.md).

Zie voor een lijst met meer voorbeeld topologieën, [voorbeeld topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md).
