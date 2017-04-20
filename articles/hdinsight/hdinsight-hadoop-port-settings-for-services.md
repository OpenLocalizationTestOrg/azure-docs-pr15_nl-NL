<properties
pageTitle="Poorten die worden gebruikt door HDInsight | Azure"
description="Een lijst met poorten die worden gebruikt door Hadoop services worden uitgevoerd op HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>Poorten en -URI's die worden gebruikt door HDInsight

Dit document bevat een lijst met de poorten die worden gebruikt door Hadoop services worden uitgevoerd op HDInsight Linux gebaseerde clusters. Ook wordt aandacht besteed aan poorten die verbinding maken met de cluster via SSH worden gebruikt.

## <a name="public-ports-vs-non-public-ports"></a>Openbare poorten versus niet-openbare-poorten

Linux gebaseerde HDInsight clusters beschrijft slechts drie poorten openbaar op internet. 22, 23 en 443. Deze worden gebruikt voor veilig toegang tot het cluster met SSH en services die via het beveiligde HTTPS-protocol.

Intern HDInsight door verschillende Azure virtuele Machines (de knooppunten binnen het cluster,) is geïmplementeerd op een Azure Virtual Network uitgevoerd. Van binnen het virtuele netwerk, zijn er poorten niet beschikbaar zijn via internet. Bijvoorbeeld als u verbinding met een van de kop knooppunten via SSH maakt, vanaf het hoofd knooppunt u kunt vervolgens rechtstreeks toegang tot services worden uitgevoerd op knooppunten.

> [AZURE.IMPORTANT] Wanneer u een cluster HDInsight maakt als u een virtueel Azure-netwerk niet als een configuratieoptie opgeeft, wordt er een gemaakt; echter u niet deelnemen aan andere computers (zoals andere Azure virtuele Machines of uw clientcomputer ontwikkeling,) als volgt automatisch gemaakt virtueel netwerk. 

Als u wilt deelnemen aan extra machines bij het virtuele netwerk, moet u eerst het virtuele netwerk maken en geef deze bij het maken van uw cluster HDInsight. Zie voor meer informatie [mogelijkheden voor HDInsight uitbreiden met behulp van een Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Openbare poorten

Alle knooppunten in een cluster HDInsight zich bevinden in een Azure Virtual Network en niet rechtstreeks toegankelijk zijn vanuit internet. Een openbare gateway biedt internettoegang tot de volgende poorten, die gebruikt in alle HDInsight clustertypen worden.

| Service | Poort | Protocol | Beschrijving |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22 | SSH | Clients verbinding maakt sshd op de primaire headnode. Zie [gebruiken SSH met Linux gebaseerde HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| sshd | 22 | SSH | Clients verbinding maakt sshd op het randknooppunt (alleen HDInsight Premium). Zie [aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md) |
| sshd | 23 | SSH | Clients verbinding maakt sshd op de secundaire headnode. Zie [gebruiken SSH met Linux gebaseerde HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443 | HTTPS | Ambari web UI. Zie [HDInsight beheren met behulp van de gebruikersinterface van de Web Ambari](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443 | HTTPS | Ambari REST API. Zie [HDInsight beheren met de Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443 | HTTPS | HCatalog REST API. Zie [component met krul gebruikt](hdinsight-hadoop-use-pig-curl.md), [gebruikt u varken met krul](hdinsight-hadoop-use-pig-curl.md), [MapReduce met krul](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443 | ODBC | Maakt verbinding met de component ODBC gebruiken. Zie [Excel verbinden met HDInsight via het Microsoft ODBC-stuurprogramma](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443 | JDBC | Maakt verbinding met de component JDBC gebruiken. Zie [verbinding maken met de component op HDInsight met het stuurprogramma JDBC component](hdinsight-connect-hive-jdbc-driver.md) |

De volgende zijn beschikbaar voor specifieke clustertypen:

| Service | Poort | Protocol |Clustertype | Beschrijving |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443 | HTTPS | HBase | HBase REST API. Zie [aan de slag met HBase](hdinsight-hbase-tutorial-get-started-linux.md) |
| Hier | 443 | HTTPS |  Elektrische | Een REST API. Overzicht van [een indienen projecten op afstand met hier](hdinsight-apache-spark-livy-rest-interface.md) |
| Storm | 443 | HTTPS | Storm | Storm web UI. Zie [distribueren en beheren van Storm topologieën op HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>Verificatie

Alle services die op internet openbaar worden aangeboden moeten worden geverifieerd:

| Poort | Referenties |
| ---- | ----------- |
| 22 of 23 | De gebruikersreferenties SSH is opgegeven tijdens het cluster maken |
| 443 | De aanmeldingsnaam (standaard:-beheerder) en het wachtwoord die zijn ingesteld tijdens het maken van cluster |

## <a name="non-public-ports"></a>Niet-openbaar-poorten

> [AZURE.NOTE] Sommige services zijn alleen beschikbaar op bepaalde clustertypen. Bijvoorbeeld is HBase alleen beschikbaar op HBase clustertypen.

### <a name="hdfs-ports"></a>HDFS-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode web UI | Hoofd knooppunten | 30070 | HTTPS | Web UI huidige status weergeven |
| NameNode metagegevensservice | hoofd knooppunten | 8020 | IPC | Bestand systeem metagegevens 
| DataNode | Alle werknemer knooppunten | 30075 | HTTPS | Web UI status bekijken, Logboeken, enzovoort. |
| DataNode | Alle werknemer knooppunten | 30010 | &nbsp; | Overdracht van gegevens |
| DataNode | Alle werknemer knooppunten | 30020 | IPC | Metagegevens bewerkingen |
| Secundaire NameNode | Hoofd knooppunten | 50090 | HTTP | Controlepunt voor NameNode metagegevens |

### <a name="yarn-ports"></a>GARENS-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| Resourcemanager web UI | Hoofd knooppunten | 8088 | HTTP | Web UI voor resourcemanager |
| Resourcemanager web UI | Hoofd knooppunten | 8090 | HTTPS | Web UI voor resourcemanager |
| Resourcemanager beheerder interface | hoofd knooppunten | 8141 | IPC | Voor de toepassing ingediende items (component, component server, varken, enz.) |
| Resourcemanager scheduler | hoofd knooppunten | 8030 | HTTP | Administratieve interface |
| Resourcemanager Toepassingsinterface | hoofd knooppunten | 8050 | HTTP |Adres van de interface van de manager toepassingen |
| NodeManager | Alle werknemer knooppunten | 30050 | &nbsp; | Het adres van de beheerder van de container |
| NodeManager web UI | Alle werknemer knooppunten | 30060 | HTTP | Resource manager interface |
| Adres van de tijdlijn | Hoofd knooppunten | 10200 | RPC | De tijdlijn service RPC-service. |
| Tijdlijn web UI | Hoofd knooppunten | 8181 | HTTP | De tijdlijn service web UI |

### <a name="hive-ports"></a>Component-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Hoofd knooppunten | 10001 | Thrift | Service voor via programmacode verbinding maakt met de component (Thrift/JDBC) |
| HiveServer | Hoofd knooppunten | 10000 | Thrift | Service voor via programmacode verbinding maakt met de component (Thrift/JDBC) |
| Component Metastore | Hoofd knooppunten | 9083 | Thrift | Service voor via programmacode verbinding maakt met component metagegevens (Thrift/JDBC) |

### <a name="webhcat-ports"></a>WebHCat-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat server | Hoofd knooppunten | 30111 | HTTP | Web API boven aan de HCatalog en andere services Hadoop |

### <a name="mapreduce-ports"></a>MapReduce-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Hoofd knooppunten | 19888 | HTTP | MapReduce JobHistory web UI |
| JobHistory | Hoofd knooppunten | 10020 | &nbsp; | MapReduce JobHistory server |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Hiermee kunt u overdrachten tussenliggende Map naar het aanvragen van een verkleiningstoestellen |

### <a name="oozie"></a>Oozie

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie server | Hoofd knooppunten | 11000 | HTTP | URL van Oozie-service |
| Oozie server | Hoofd knooppunten | 11001 | HTTP | Poort voor Oozie-beheerder |

### <a name="ambari-metrics"></a>Aan de doelstellingen Ambari

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| Tijdlijn (geschiedenis van toepassing) | Hoofd knooppunten | 6188 | HTTP | De tijdlijn service web UI |
| Tijdlijn (geschiedenis van toepassing) | Hoofd knooppunten | 30200 | RPC | De tijdlijn service web UI |

### <a name="hbase-ports"></a>HBase-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Hoofd knooppunten | 16000 | &nbsp; | &nbsp; |
| HMaster info Web UI | Hoofd knooppunten | 16010 | HTTP | De poort voor het web HBase outmodel UI |
| Regio-server | Alle werknemer knooppunten | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | De poort waarmee clients verbinding maken met ZooKeeper |

### <a name="kafka-ports"></a>Kafka-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| ------- | ------- | ---- | -------- | ----------- |
| Makelaar  | Werknemer knooppunten | 9092 | [Kafka via-Protocol](http://kafka.apache.org/protocol.html) | Wordt gebruikt voor clientcommunicatie van de |
| &nbsp; | Zookeeper knooppunten | 2181 | &nbsp; | De poort waarmee clients verbinding maken met Zookeeper |
