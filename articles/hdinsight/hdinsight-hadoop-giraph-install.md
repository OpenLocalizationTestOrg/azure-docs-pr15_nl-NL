<properties
    pageTitle="Installeren en gebruiken van Giraph op Hadoop clusters in HDInsight | Microsoft Azure"
    description="Informatie over het aanpassen van HDInsight cluster met Giraph en het gebruik van Giraph."
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

# <a name="install-and-use-giraph-in-hdinsight"></a>Installeren en gebruiken van Giraph in HDInsight


Informatie over het aanpassen van Windows op basis HDInsight cluster met Giraph met de actie Script en het gebruik van Giraph verwerkingstijd van grote grafieken. Zie voor informatie over het gebruik van Giraph met een cluster Linux gebaseerde [Giraph installeren op HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).
 
U kunt Giraph op een willekeurig type cluster (Hadoop, Storm, HBase, een) op Azure HDInsight installeren met behulp van *Scriptactie*. Een voorbeeldscript Giraph installeren op een cluster HDInsight vindt u in een alleen-lezen opslag van Azure blob bij [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1). Het voorbeeldscript werkt alleen met HDInsight cluster versie 3.1. Zie voor meer informatie over HDInsight cluster versies, [HDInsight cluster versies](hdinsight-component-versioning.md).

**Verwante artikelen**

- [Giraph installeren op HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md)
- [Hadoop maken clusters in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight clusters.
- [Aanpassen HDInsight cluster met scriptactie][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight clusters met de Script-actie.
- [Ontwikkel scriptactie-scripts voor HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Wat is Giraph?

<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> kunt u graph verwerken met behulp van Hadoop uitvoeren en kan worden gebruikt met Azure HDInsight. Grafieken modelleren van relaties tussen objecten, zoals de verbindingen tussen routers in een grote netwerk zoals Internet, of de relaties tussen personen aan de sociale netwerken (ook wel een sociale grafiek genoemd). Verwerking van de grafiek kunt u reden dan ook over de relaties tussen objecten in een grafiek, zoals:

- Identificerende mogelijke vrienden op basis van uw huidige relaties.
- Identificerende de kortste route tussen twee computers in een netwerk.
- Berekening van de pagina rang van webpagina's.


## <a name="install-giraph-using-portal"></a>Installeren met behulp van portal Giraph

1. Beginnen met het maken van een cluster met behulp van de optie **Aangepaste maken** , zoals beschreven op [Hadoop maken clusters in HDInsight](hdinsight-provision-clusters.md#portal).
2. Klik op de pagina **Scriptacties** van de wizard op **scriptactie toevoegen** voor meer informatie over de scriptactie, zoals hieronder wordt weergegeven:

    ![Gebruik scriptactie om aan te passen een cluster] (./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Gebruik scriptactie om aan te passen een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie. Bijvoorbeeld <b>Giraph installeren</b>.</td></tr>
        <tr><td>Script URI</td>
            <td>Geef de id URI (Uniform Resource) aan het script die als u wilt aanpassen van het cluster wordt geactiveerd. Bijvoorbeeld: <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Knooppunttype</td>
            <td>Geef de knooppunten waarop het script aanpassing wordt uitgevoerd. U kunt <b>alle knooppunten</b>, <b>alleen hoofd knooppunten</b>of <b>alleen werknemer knooppunten</b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters, indien nodig door het script. Het script voor het installeren van Giraph hoeft geen parameters, zodat u kunt dit leeg laat.</td></tr>
    </table>

    U kunt meer dan één scriptactie om meerdere onderdelen op het cluster toevoegen. Nadat u de scripts hebt toegevoegd, klikt u op het vinkje om te beginnen met het maken van het cluster.

## <a name="use-giraph"></a>Giraph gebruiken

We het voorbeeld SimpleShortestPathsComputation gebruiken om te laten zien van de eenvoudige <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> uitvoering voor het zoeken van de kortste route tussen objecten in een grafiek. Gebruik van de volgende stappen uit om te uploaden de voorbeeldgegevens en het voorbeeld oppervlak, een taak uitvoeren met behulp van het voorbeeld SimpleShortestPathsComputation en vervolgens de resultaten te bekijken.

1. Upload een voorbeeld van gegevensbestand met Azure-blobopslag. Maak een nieuw bestand met de naam **tiny_graph.txt**op uw lokale computer. De volgende regels moet bevatten:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Het tiny_graph.txt-bestand uploaden naar de primaire opslagruimte voor uw cluster HDInsight. Zie voor instructies over het uploaden van gegevens, de [gegevens voor Hadoop-projecten in HDInsight uploaden](hdinsight-upload-data.md).

    Deze gegevens worden van een relatie tussen objecten in een Services graph, met behulp van de indeling [bron\_-id, bron\_waarde, [[dest\_id], [rand\_waarde],...]]. Elke regel staat een relatie tussen een **bron\_id** object en een of meer **dest\_id** objecten. De **rand\_waarde** (of gewicht) kunnen worden beschouwd als de sterkte of afstand van de verbinding tussen **source_id** en **dest\_id**.

    Getekend, en met de waarde (of gewicht) als de afstand tussen objecten, de bovenstaande gegevens als volgt uitzien:

    ![tiny_graph.txt getekend als cirkels met verschillende afstand tussen regels](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)



4. Voer in het voorbeeld SimpleShortestPathsComputation. Gebruik de volgende Azure PowerShell-cmdlets om uit te voeren in het voorbeeld met behulp van het bestand tiny_graph.txt als invoer. 

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

        $clusterName = "clustername"
        # Giraph examples jar
        $jarFile = "wasbs:///example/jars/giraph-examples.jar"
        # Arguments for this job
        $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                        "-ca", "mapred.job.tracker=headnodehost:9010",
                        "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                        "-vip", "wasbs:///example/data/tiny_graph.txt",
                        "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                        "-op",  "wasbs:///example/output/shortestpaths",
                        "-w", "2"
        # Create the definition
        $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
          -JarFile $jarFile
          -ClassName "org.apache.giraph.GiraphRunner"
          -Arguments $jobArguments

        # Run the job, write output to the Azure PowerShell window
        $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureHDInsightJob -Job $job
        Write-Host "STDERR"
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput

    Klik in het bovenstaande voorbeeld kunt u **clusternaam** vervangen door de naam van uw HDInsight-cluster met Giraph is geïnstalleerd.

5. De resultaten bekijken. Zodra de taak is voltooid, de resultaten worden opgeslagen in twee uitvoerbestanden in de __wasbs: / / / voorbeeld/out/shotestpaths__ map. De bestanden worden __deel-m-00001__ en __deel-m-00002__genoemd. De volgende stappen uitvoeren om te downloaden en weergave van de uitvoer:

        $subscriptionName = "<SubscriptionName>"       # Azure subscription name
        $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
        $containerName = "<ContainerName>"             # Blob storage container name

        # Select the current subscription
        Select-AzureSubscription $subscriptionName

        # Create the Storage account context object
        $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

        # Download the job output to the workstation
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force

    Hiermee wordt de mapstructuur __voorbeeld/uitvoer/shortestpaths__ maken in de huidige map op uw werkstation en de twee uitvoerbestanden naar die locatie downloaden.

    Gebruik de cmdlet __kat__ om de inhoud van de bestanden weer te geven:

        Cat example/output/shortestpaths/part*

    De uitvoer moet worden de volgende strekking weergegeven:


        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    Het voorbeeld is hard gecodeerde beginnen SimpleShortestPathComputation object-ID 1 en de kortste route op andere objecten. Zodat de uitvoer moet worden gelezen als `destination_id distance`, waarbij afstand de waarde (of het gewicht) van de randen afgelegde tussen object-ID 1 en de doel-ID.

    Zomersportevenementen volgt, kunt u de resultaten controleren door de kortste paden reizen tussen 1-ID en alle andere objecten. Houd er rekening mee dat het kortste pad tussen 1-ID en -ID 4 5 is. Dit is de totale afstand tussen <span style="color:orange">ID 1 en 3</span>, en klik vervolgens <span style="color:red">ID 3 en 4</span>.

    ![Tekenen van objecten als cirkels met kortste paden getekend tussen](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Giraph via Aure PowerShell installeren

Zie [HDInsight aanpassen clusters met de Script-actie](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Het voorbeeld wordt gedemonstreerd hoe u een via Azure PowerShell installeert. U moet het script als u wilt gebruiken [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)aanpassen.

## <a name="install-giraph-using-net-sdk"></a>Met .NET SDK Giraph installeren

Zie [HDInsight aanpassen clusters met de Script-actie](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Het voorbeeld wordt gedemonstreerd hoe u met de SDK .NET elektrische installeert. U moet het script als u wilt gebruiken [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)aanpassen.


## <a name="see-also"></a>Zie ook

- [Giraph installeren op HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md)
- [Hadoop maken clusters in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight clusters.
- [Aanpassen HDInsight cluster met scriptactie][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight clusters met de Script-actie.
- [Ontwikkel scriptactie-scripts voor HDInsight](hdinsight-hadoop-script-actions.md).
- [Installeren en gebruiken van een op HDInsight clusters][hdinsight-install-spark]: scriptactie voorbeeld over het installeren van een.
- [R installeren op HDInsight clusters][hdinsight-install-r]: scriptactie voorbeeld over het installeren van R.
- [Solr installeren op HDInsight clusters](hdinsight-hadoop-solr-install.md): scriptactie voorbeeld over het installeren van Solr.



[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: ../powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
