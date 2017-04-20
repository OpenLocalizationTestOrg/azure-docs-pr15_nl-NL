<properties
    pageTitle="Wat is HBase in HDInsight? | Microsoft Azure"
    description="Een inleiding tot Apache HBase in HDInsight, een database NoSQL combineren Hadoop. Meer informatie over het gebruik gevallen en HBase in vergelijking met andere Hadoop-clusters."
    keywords="bigtable, nosql, wat is hbase"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Wat is HBase in HDInsight: A NoSQL database waarmee u BigTable-achtige mogelijkheden Hadoop

Apache HBase is een open source, NoSQL-database die is gebouwd op Hadoop en sterk op Google BigTable. HBase biedt RAM en sterke consistentie voor grote hoeveelheden gegevens in een schemaless database geordend op kolom gezinnen ongestructureerde en semistructured.

Gegevens worden opgeslagen in de rijen van een tabel en gegevens binnen een rij is gegroepeerd op kolom familie. HBase is een schemaless database in de zin dat de kolommen noch het type gegevens die zijn opgeslagen in deze moet vóór het gebruik van deze velden worden gedefinieerd. De openen-broncode kan lineair petabytes van gegevens op duizenden knooppunten verwerken. Deze vertrouwen op overbodige, batchbestand en andere functies die worden verstrekt door gedistribueerde toepassingen in de Hadoop-systeem.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Hoe wordt HBase geïmplementeerd in Azure HDInsight?

HDInsight HBase wordt aangeboden als een beheerde cluster die is geïntegreerd in de Azure-omgeving. De clusters zijn geconfigureerd voor het opslaan van gegevens rechtstreeks in Azure-blobopslag, met lage latentie en verbeterde elasticiteit in opties voor aanpassing prestaties en kosten. Hiermee worden klanten interactieve websites die met grote gegevenssets werken, services sensor en telemetrielogboek gegevensopslag uit miljoenen eindpunten maken en deze gegevens analyseren met Hadoop-taken te maken. HBase en Hadoop zijn goede uitgangspunten voor groot adp in Azure wordt aangegeven; met name kan worden ingeschakeld realtime-toepassingen om te werken met grote gegevenssets.

De schaal architectuur van HBase te leveren automatische sharding van tabellen, sterke consistentie voor lezen en schrijven en automatische overname maakt gebruik van de uitvoering HDInsight. Biedt betere prestaties door in het geheugen caching voor lezen en hoge gegevensdoorvoer streaming voor schrijven. De inrichting van virtueel netwerk is ook beschikbaar voor HDInsight HBase. Zie voor meer informatie [inrichten HDInsight clusters op Azure Virtual Network] [hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Hoe worden de gegevens in HDInsight HBase beheerd?

Gegevens kunnen worden beheerd in HBase met behulp van de `create`, `get`, `put`, en `scan` opdrachten uit de shell HBase. Gegevens worden geschreven met de database met behulp van `put` en lezen met behulp van `get`. De `scan` opdracht wordt gebruikt voor het verkrijgen van gegevens uit meerdere rijen in een tabel. Gegevens kunnen ook worden beheerd met de HBase C#-API, waarmee een clientbibliotheek boven aan de HBase REST API. Een database HBase kan ook worden opgezocht met behulp van component. Zie voor een inleiding tot deze programming modellen, [aan de slag met HBase met Hadoop in HDInsight][hbase-get-started]. CO-processors zijn ook beschikbaar, waarmee de verwerking van gegevens in de knooppunten waarop de database wordt gehost.


## <a name="scenarios-use-cases-for-hbase"></a>Scenario's: Gebruik dozen voor HBase
De canonieke use-case voor welke BigTable (en dus ook HBase) is gemaakt is zoeken op het web. Zoekprogramma's bouwen indexen die termen toewijzen aan de webpagina's waarin deze. Maar er zijn andere gebruik vaak die HBase geschikt is voor, verscheidene die in deze sectie zijn gespecificeerde.

- Sleutelwaarden store

    HBase kunnen worden gebruikt als een archief sleutelwaarden en is geschikt voor het bericht systemen beheren. Facebook HBase voor hun berichtensysteem, en is ideaal voor het opslaan en beheren van internetcommunicatie. HBase WebTable gebruikt om te zoeken naar en tabellen die zijn opgehaald uit de webpagina's beheren.

- Sensorgegevens

    HBase is handig voor het vastleggen van gegevens die worden verzameld, stapsgewijs uit verschillende bronnen. Dit geldt ook voor sociale analytics, tijdreeks, interactieve dashboards up-to-date met trends en items te houden en controle log systemen beheren. Voorbeelden hiervan zijn Bloomberg bedrijf terminal en de tijd reeks Database openen (OpenTSDB), die worden opgeslagen en biedt toegang tot aan de doelstellingen die over de status van server-systemen zijn verzameld.

- Realtime query

    [Phoenix](http://phoenix.apache.org/) is een SQL-query-engine voor Apache HBase. Dit wordt geopend als een JDBC-stuurprogramma en maakt de query's uitvoeren en HBase tabellen beheren met behulp van SQL.

- HBase als een platform

    Toepassingen kunnen worden uitgevoerd boven aan de HBase door deze te gebruiken als een gegevensopslag. Voorbeelden hiervan zijn Phoenix, OpenTSDB Kiji en Titan. Toepassingen kunnen ook integreren met HBase. Voorbeelden hiervan zijn component, varken Solr, Storm, Flume, Impala, een, bevatten en meer details.


##<a name="next-steps"></a>Volgende stappen

- [Aan de slag met HBase met Hadoop in HDInsight][hbase-get-started]
- [HDInsight clusters op Azure Virtual Network inrichten] [hbase-provision-vnet]
- [Replicatie van HBase op HDInsight configureren](hdinsight-hbase-geo-replication.md)
- [Twitter sentiment met HBase in HDInsight analyseren][hbase-twitter-sentiment]
- [Maven gebruiken om te maken van Java-toepassingen die gebruikmaken van HBase bij HDInsight (Hadoop)][hbase-build-java-maven]

##<a name="see-also"></a>Zie ook

- [Apache HBase](https://hbase.apache.org/)
- [Bigtable: Een verdeelde opslagsysteem voor gestructureerde gegevens](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
