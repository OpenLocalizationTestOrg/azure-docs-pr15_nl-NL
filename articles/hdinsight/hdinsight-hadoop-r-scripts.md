<properties
    pageTitle="Gebruik R in HDInsight om aan te passen clusters | Microsoft Azure"
    description="Leer hoe u met de actie Script R installeert en R op HDInsight clusters gebruiken."
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
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van R op HDInsight Hadoop clusters

Informatie over het aanpassen van Windows op basis van HDInsight cluster met R met de actie Script en het gebruik van R op HDInsight clusters. De [premium-laag](https://azure.microsoft.com/pricing/details/hdinsight/) aanbod voor HDInsight bevat R-Server als onderdeel van uw cluster HDInsight. Hierdoor R scripts met MapReduce en een verdeelde berekeningen uitvoeren. Zie [aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md)voor meer informatie. Zie voor informatie over het gebruik van R met een cluster Linux gebaseerde [installeren en gebruiken R op HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
U kunt R op een willekeurig type cluster (Hadoop, Storm, HBase, een) op Azure HDInsight installeren met behulp van *Scriptactie*. Een voorbeeldscript R installeren op een cluster HDInsight vindt u in een alleen-lezen opslag van Azure blob bij [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1). 

**Verwante artikelen**

- [Installeren en gebruiken van R op HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Hadoop maken clusters in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight clusters
- [Aanpassen HDInsight cluster met scriptactie][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight clusters met de actie Script
- [Ontwikkel scriptactie scripts voor HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Wat is R?

Het <a href="http://www.r-project.org/" target="_blank">Project R voor statistische Computing</a> is een geopende bron taal- en -omgeving voor statistische computing. R biedt honderden ingebouwde statistische functies en een eigen programmeertaal die worden gecombineerd aspecten van het functionele en object-georiënteerd programmeren. Ook vindt u hier de uitgebreide mogelijkheden voor grafische. R is de voorkeur programming omgeving voor de meeste professionele statistici en wetenschappers in een groot aantal velden.

R is compatibel met Azure Blob Storage (WASB), zodat de gegevens die zijn opgeslagen er kan worden verwerkt met R op HDInsight.  

## <a name="install-r"></a>R installeren

Een [voorbeeldscript](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) R installeren op een cluster HDInsight vindt u in een alleen-lezen blob in Azure opslag. In deze sectie bevat instructies over het gebruik van het voorbeeldscript tijdens het maken van het cluster met behulp van de Azure-Portal.

> [AZURE.NOTE] Het voorbeeldscript is geïntroduceerd met HDInsight cluster versie 3.1. Zie voor meer informatie over HDInsight cluster versies, [HDInsight cluster versies](hdinsight-component-versioning.md).

1. Wanneer u een cluster HDInsight via de Portal maakt, **Optionele configuratie**op en klik vervolgens op **Scriptacties**.
2. Klik op de pagina **Scriptacties** Voer de volgende waarden:

    ![Gebruik scriptactie om aan te passen een cluster] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Gebruik scriptactie om aan te passen een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie, bijvoorbeeld <b>R installeren</b>.</td></tr>
        <tr><td>Script URI</td>
            <td>Geef de URI aan het script die wordt aangeroepen als u wilt aanpassen van het cluster, bijvoorbeeld <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Knooppunttype</td>
            <td>Geef de knooppunten waarop het script aanpassing wordt uitgevoerd. U kunt <b>Alle knooppunten</b>, <b>alleen hoofd knooppunten</b>of <b>werknemer knooppunten</b> alleen kiezen.
        <tr><td>Parameters</td>
            <td>Geef de parameters, indien nodig door het script. Het script R installeren is echter geen parameters, vereist zodat u kunt dit leeg laat.</td></tr>
    </table>

    U kunt meer dan één scriptactie om meerdere onderdelen op het cluster toevoegen. Nadat u de scripts hebt toegevoegd, klikt u op het vinkje om te beginnen met het cluster crating.

U kunt ook het script R installeren op HDInsight via Azure PowerShell of de HDInsight .NET SDK. Instructies voor deze procedures worden verderop in dit artikel gegeven.

## <a name="run-r-scripts"></a>R-scripts uitvoeren
In deze sectie wordt beschreven hoe een R-script uitvoeren op de Hadoop-cluster met HDInsight.

1. **Maak extern bureaublad verbinding met het cluster**: van de Portal extern bureaublad inschakelen voor het cluster die u hebt gemaakt met R is geïnstalleerd, en maak verbinding met het cluster. Zie [verbinding maken met HDInsight clusters met RDP](hdinsight-administer-use-management-portal.md#rdp)voor instructies.

2. **Open de R-console**: het R installatie plaatst een koppeling naar de R-console op het bureaublad van het hoofd knooppunt. Klik op deze de R-console openen.

3. **De R-script uitvoeren**: het R-script rechtstreeks vanuit de R-console kan worden uitgevoerd door plakken, te selecteren en op ENTER te drukken. Hier volgt een eenvoudig voorbeeld-script dat de getallen 1 tot en met 100 genereert en wordt deze vervolgens vermenigvuldigd met 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

De eerste twee regels bellen de RHadoop-bibliotheken die worden geïnstalleerd met R. De laatste regel worden de resultaten aan de console. De uitvoer ziet er als volgt:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>R via Aure PowerShell installeren

Zie [HDInsight aanpassen clusters met de Script-actie](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Het voorbeeld wordt gedemonstreerd hoe u een via Azure PowerShell installeert. U moet het script als u wilt gebruiken [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)aanpassen.

## <a name="install-r-using-net-sdk"></a>Installeren met behulp van .NET SDK R

Zie [HDInsight aanpassen clusters met de Script-actie](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Het voorbeeld wordt gedemonstreerd hoe u met de SDK .NET elektrische installeert. U moet het script als u wilt gebruiken [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11)aanpassen.


## <a name="see-also"></a>Zie ook

- [Installeren en gebruiken van R op HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Hadoop maken clusters in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight clusters
- [Aanpassen HDInsight cluster met scriptactie][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight clusters met de actie Script
- [Ontwikkel scriptactie scripts voor HDInsight](hdinsight-hadoop-script-actions.md)
- [Installeren en gebruiken van een op HDInsight clusters][hdinsight-install-spark]: scriptactie voorbeeld over het installeren van een
- [Giraph installeren op HDInsight clusters](hdinsight-hadoop-giraph-install.md): scriptactie voorbeeld over het installeren van Giraph
- [Solr installeren op HDInsight clusters](hdinsight-hadoop-solr-install-linux.md): scriptactie voorbeeld over het installeren van Solr.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
