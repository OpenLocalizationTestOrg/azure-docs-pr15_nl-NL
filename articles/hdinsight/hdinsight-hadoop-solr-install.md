<properties
    pageTitle="Gebruik van scriptactie Solr installeren op Hadoop cluster | Microsoft Azure"
    description="Informatie over het aanpassen van HDInsight cluster met Solr met de actie Script."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van Solr op HDInsight Hadoop clusters

Leer hoe u Windows op basis HDInsight cluster met Solr met de actie Script aanpast en hoe u gegevens zoeken met Solr. Zie voor informatie over het gebruik van Solr met een cluster Linux gebaseerde [installeren en gebruiken Solr op HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-solr-install-linux.md).
 
U kunt Solr op een willekeurig type cluster (Hadoop, Storm, HBase, een) op Azure HDInsight installeren met behulp van *Scriptactie*. Een voorbeeldscript Solr installeren op een cluster HDInsight vindt u in een alleen-lezen opslag van Azure blob bij [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1). 

Het voorbeeldscript werkt alleen met HDInsight cluster versie 3.1. Zie voor meer informatie over HDInsight cluster versies, [HDInsight cluster versies](hdinsight-component-versioning.md).

Het voorbeeldscript gebruikt in dit onderwerp wordt gemaakt van een cluster Solr op basis van Windows met een specifieke configuratie. Als u het cluster Solr configureren met verschillende siteverzamelingen, shards, schema's, replica's, enzovoort wilt, moet u aanpassen het script en Solr binaire bestanden.

**Verwante artikelen**

- [Installeren en gebruiken van Solr op HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Hadoop maken clusters in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight clusters.
- [Aanpassen HDInsight cluster met scriptactie][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight clusters met de Script-actie.
- [Ontwikkel scriptactie-scripts voor HDInsight](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Wat is Solr?

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> is een enterprise search-platform waarmee u krachtige zoeken in de volledige tekst van gegevens. Terwijl Hadoop kunt opslaan en beheren van grote hoeveelheden gegevens, biedt Apache Solr de zoekmogelijkheden snel de gegevens worden opgehaald. 

## <a name="install-solr-using-portal"></a>Installeren met behulp van portal Solr

1. Beginnen met het maken van een cluster met behulp van de optie **Aangepaste maken** , zoals beschreven op [Hadoop maken clusters in HDInsight](hdinsight-provision-clusters.md#portal).
2. Klik op de pagina **Scriptacties** van de wizard op **scriptactie toevoegen** voor meer informatie over de scriptactie, zoals hieronder wordt weergegeven:

    ![Gebruik scriptactie om aan te passen een cluster] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Gebruik scriptactie om aan te passen een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie. Bijvoorbeeld <b>Solr installeren</b>.</td></tr>
        <tr><td>Script URI</td>
            <td>Geef de id URI (Uniform Resource) aan het script die als u wilt aanpassen van het cluster wordt geactiveerd. Bijvoorbeeld: <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Knooppunttype</td>
            <td>Geef de knooppunten waarop het script aanpassing wordt uitgevoerd. U kunt <b>alle knooppunten</b>, <b>alleen hoofd knooppunten</b>of <b>alleen werknemer knooppunten</b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters, indien nodig door het script. Het script voor het installeren van Solr hoeft geen parameters, zodat u kunt dit leeg laat.</td></tr>
    </table>

    U kunt meer dan één scriptactie om meerdere onderdelen op het cluster toevoegen. Nadat u de scripts hebt toegevoegd, klikt u op het vinkje om te beginnen met het maken van het cluster.


## <a name="use-solr"></a>Solr gebruiken

U moet beginnen met het indexeren Solr met enkele gegevensbestanden. U kunt vervolgens Solr zoekquery's uitvoeren op de geïndexeerde gegevens. De volgende stappen als u wilt Solr gebruiken in een cluster HDInsight uitvoeren:

1. **Gebruik Remote Desktop Protocol (RDP) op afstand in het cluster HDInsight met Solr is geïnstalleerd**. Schakel in de portal Azure extern bureaublad voor het cluster die u hebt gemaakt met Solr geïnstalleerd en vervolgens remote in het cluster. Zie [verbinding maken met HDInsight clusters met RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)voor instructies.

2. **Index Solr door het uploaden van gegevensbestanden**. Wanneer u Solr indexeert, plaatst u documenten in deze die u nodig hebt mogelijk om op te zoeken. Als u wilt indexeren Solr, RDP gebruikt om externe naar het cluster, Ga naar het bureaublad, opent u de opdrachtregel Hadoop en Ga naar **C:\apps\dist\solr-4.7.2\example\exampledocs**. Voer de volgende opdracht:

        java -jar post.jar solr.xml monitor.xml

    Klik op de console ziet u het volgende resultaat:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Het hulpprogramma post.jar indexeert Solr met twee steekproeven documenten, **solr.xml** en **monitor.xml**. Het hulpprogramma post.jar en de voorbeelddocumenten zijn beschikbaar voor communicatie met Solr installatie.

3. **Gebruik het dashboard Solr zoeken in de geïndexeerde documenten**. Open Internet Explorer in de RDP-sessie met het cluster HDInsight, en het dashboard Solr bij starten **http://headnodehost:8983/solr / #/**. Selecteer **collection1**in het linkerdeelvenster, in de vervolgkeuzelijst **Core Gegevenskiezer** en binnen die, klikt u op **Query**. Als u bijvoorbeeld om te selecteren en alle documenten in Solr retourneren, bevatten de volgende waarden:

    * Voer in het tekstvak **q** ** \*:**\*. Hiermee herstelt u alle documenten die zijn geïndexeerd in Solr. Als u zoeken naar een specifieke tekenreeks binnen de documenten wilt, kunt u die tekenreeks hier invoeren.
    
    * Selecteer in het tekstvak **wt** de uitvoerindeling. Standaard is **json**. Klik op **Query uitvoeren**.

    ![Gebruik scriptactie om aan te passen een cluster] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Query's op Solr dashboard uitvoeren")
    
    De uitvoer geeft als resultaat de twee documenten die wordt gebruikt voor het indexeren Solr. De uitvoer er ongeveer als volgt te werk:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Aanbevolen: Back-up van de geïndexeerde gegevens uit Solr met Azure-blobopslag die is gekoppeld aan het cluster HDInsight**. Als een goede gewoonte, moet u back-up van de geïndexeerde gegevens van knooppunten Solr naar Azure-blobopslag. Doe het volgende doen:

    1. Open Internet Explorer de sessie RDP en wijst u de volgende URL:

            http://localhost:8983/solr/replication?command=backup

        Hier ziet u een antwoord als volgt:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. Ga in de externe sessie naar {SOLR_HOME}\{siteverzameling} \data. Dit moet voor het cluster gemaakt via het voorbeeldscript, **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. Op deze locatie, ziet u een momentopnamemap die zijn gemaakt met een naam die lijkt op * *momentopname.* tijdstempel***.

    3. De map met momentopnamen ZIP en uploadt dit naar Azure-blobopslag. Navigeer naar de locatie van de momentopnamemap met behulp van de volgende opdracht uit vanaf de opdrachtregel Hadoop:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        Deze opdracht kopieert de momentopname naar /example/data/onder de container binnen de standaard opslag-account dat is gekoppeld aan het cluster.

## <a name="install-solr-using-aure-powershell"></a>Solr via Aure PowerShell installeren

Zie [HDInsight aanpassen clusters met de Script-actie](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Het voorbeeld wordt gedemonstreerd hoe u een via Azure PowerShell installeert. U moet het script als u wilt gebruiken [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)aanpassen.

## <a name="install-solr-using-net-sdk"></a>Met .NET SDK Solr installeren

Zie [HDInsight aanpassen clusters met de Script-actie](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Het voorbeeld wordt gedemonstreerd hoe u met de SDK .NET elektrische installeert. U moet het script als u wilt gebruiken [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)aanpassen.



## <a name="see-also"></a>Zie ook

- [Installeren en gebruiken van Solr op HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Hadoop maken clusters in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight clusters.
- [Aanpassen HDInsight cluster met scriptactie][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight clusters met de Script-actie.
- [Ontwikkel scriptactie-scripts voor HDInsight](hdinsight-hadoop-script-actions.md).
- [Installeren en gebruiken van een op HDInsight clusters][hdinsight-install-spark]: scriptactie voorbeeld over het installeren van een.
- [R installeren op HDInsight clusters][hdinsight-install-r]: scriptactie voorbeeld over het installeren van R.
- [Giraph installeren op HDInsight clusters](hdinsight-hadoop-giraph-install.md): scriptactie voorbeeld over het installeren van Giraph.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
