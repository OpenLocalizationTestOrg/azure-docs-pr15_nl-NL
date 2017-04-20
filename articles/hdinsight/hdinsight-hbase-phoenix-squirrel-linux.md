<properties 
   pageTitle="Gebruik Apache Phoenix en eekhoorn in HDInsight | Microsoft Azure" 
   description="Leer hoe u Apache Phoenix in HDInsight gebruikt en hoe installeren en configureren van eekhoorn op uw werkstation verbinding maken met een HBase cluster in HDInsight." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Apache Phoenix gebruiken met HBase Linux gebaseerde clusters in HDInsight  

Informatie over het gebruik van [Apache Phoenix](http://phoenix.apache.org/) in HDInsight en het gebruik van SQLLine. Zie voor meer informatie over Phoenix, [Phoenix in 15 minuten of minder](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Zie voor de grammatica Phoenix [Phoenix grammatica](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Zie voor de versie-informatie voor het Phoenix in HDInsight, [Wat is er nieuw in de Hadoop cluster versies geleverd door HDInsight?] [hdinsight-versions].

##<a name="use-sqlline"></a>SQLLine gebruiken
[SQLLine](http://sqlline.sourceforge.net/) is een opdrachtregelhulpprogramma SQL uitvoeren. 

###<a name="prerequisites"></a>Vereisten voor
Voordat u SQLLine gebruiken kunt, moet u hebt het volgende:

- **A HBase cluster in HDInsight**. Voor meer informatie over inrichten HBase cluster, raadpleegt u [aan de slag met Apache HBase in HDInsight][hdinsight-hbase-get-started].
- **Verbinding maken met het cluster HBase via het externe bureaublad protocol**. Zie voor instructies [beheren Hadoop clusters in HDInsight via de Portal van Azure-klassieke][hdinsight-manage-portal].


Wanneer u verbinding met een cluster HBase maakt, moet u verbinding maakt met een van de zoohouder. Elk cluster HDInsight heeft 3 zoohouder. 

**Om vast te stellen de hostnaam Zookeeper**

1. Ambari openen door te bladeren naar **https://<ClusterName>. azurehdinsight.net**.
2. Voer de HTTP (cluster)-gebruikersnaam en wachtwoord in om aan te melden.
3. Klik op **ZooKeeper** in het linkermenu. Er wordt 3 **ZooKeeper Server** vermeld.
4. Klik op een van de **Server ZooKeeper** weergegeven. Zoek de **Hostname**op het deelvenster Vergaderoverzicht. Dit is vergelijkbaar met *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Gebruik van SQLLine**

1. Verbinding maken met het cluster via SSH. Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix, of OS X](hdinsight-hadoop-linux-use-ssh-unix.md) of [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md) afhankelijk van de clientcomputer OS voor instructies.

2. Voer de volgende opdrachten om uit te voeren SQLLine uit SSH:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Voer de volgende opdrachten aan een tabel HBase maken en sommige gegevens invoegen:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

Zie [SQLLine handmatig](http://sqlline.sourceforge.net/#manual) en [Phoenix grammatica](http://phoenix.apache.org/language/index.html)voor meer informatie.


 
##<a name="next-steps"></a>Volgende stappen
In dit artikel, hebt u geleerd hoe Apache Phoenix gebruiken in HDInsight.  Zie voor meer informatie

- [Overzicht van de HDInsight HBase][hdinsight-hbase-overview]: HBase is een Apache, open source, NoSQL database gebouwd op Hadoop met RAM en sterke consistentie voor grote hoeveelheden gegevens ongestructureerde en semistructured.
- [HBase clusters op Azure Virtual Network inrichten][hdinsight-hbase-provision-vnet]: Dankzij virtuele HBase clusters worden geïmplementeerd op hetzelfde virtuele netwerk als uw toepassingen zodat toepassingen met HBase rechtstreeks communiceren kunnen.
- [HBase configureren herhaling in HDInsight](hdinsight-hbase-geo-replication.md): meer informatie over het configureren van HBase herhaling over twee Azure datacenters. 
- [Twitter sentiment met HBase in HDInsight analyseren][hbase-twitter-sentiment]: Bekijk hoe u realtime [sentiment analyses](http://en.wikipedia.org/wiki/Sentiment_analysis) van grote gegevens met behulp van HBase in een Hadoop-cluster in HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
