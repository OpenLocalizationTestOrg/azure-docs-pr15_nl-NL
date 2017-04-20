<properties
    pageTitle="Installatie R op Linux gebaseerde HDInsight | Microsoft Azure"
    description="Informatie over het installeren en gebruiken van R om aan te passen op basis van Linux Hadoop clusters."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van R op HDInsight Hadoop clusters

Met behulp van **Scriptactie** cluster aanpassen, kunt u R installeren op een willekeurig type cluster in Hadoop op HDInsight. Hiermee worden gegevens wetenschappers en analisten R gebruiken om te implementeren van de krachtige MapReduce/garens programming framework verwerkingstijd van grote hoeveelheden gegevens op Hadoop-clusters die zijn geïmplementeerd in HDInsight.

> [AZURE.IMPORTANT] De [premium-laag](https://azure.microsoft.com/pricing/details/hdinsight/) aanbod voor HDInsight bevat R-Server als onderdeel van uw cluster HDInsight. Hierdoor R scripts met MapReduce en een verdeelde berekeningen uitvoeren. Zie [aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md)voor meer informatie. 


## <a name="what-is-r"></a>Wat is R?

Het <a href="http://www.r-project.org/" target="_blank">Project R voor statistische Computing</a> is een geopende bron taal- en -omgeving voor statistische computing. R biedt honderden ingebouwde statistische functies en een eigen programmeertaal die worden gecombineerd aspecten van het functionele en object-georiënteerd programmeren. Ook vindt u hier de uitgebreide mogelijkheden voor grafische. R is de voorkeur programming omgeving voor de meeste professionele statistici en wetenschappers in een groot aantal velden.

R-scripts kunnen worden uitgevoerd op Hadoop clusters in HDInsight die zijn aangepast scriptactie wanneer gemaakt met het installeren van de R-omgeving. R is compatibel met Azure Blob Storage (WASB), zodat de gegevens die zijn opgeslagen er kan worden verwerkt met R op HDInsight.

## <a name="what-the-script-does"></a>Werking van het script

De scriptactie gebruikt om te R installeren op uw cluster HDInsight installeert de volgende Ubuntu-pakketten, die een eenvoudige R-installatie:

* [r-base](http://packages.ubuntu.com/precise/r-base): basis GNU-R-pakket
* [r-base-ontwikkelaar](http://packages.ubuntu.com/precise/r-base-dev): Auxilliary GNU-R-pakketten

De volgende RHadoop-pakketten zijn ook geïnstalleerd, dat integratie met MapReduce en HDFS bieden:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): Hiermee kunnen ontwikkelaars een R Hadoop MapReduce gebruiken
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): Hiermee kunnen ontwikkelaars een R Hadoop HDFS (WASB voor HDInsight) gebruiken

Bovendien worden de volgende R-pakketten geïnstalleerd:

| R-pakket | Wat deze bevat |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Een laag niveau R aan Java-interface. |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | R- en C++-integratie. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | Objecten R serialiseren/terugconverteren naar JSON |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Functies voor bitsgewijze bewerkingen voor geheel getal vectoren. |
| [overzicht](https://cran.r-project.org/web/packages/digest/index.html) | Maak coderingshash geklaard R objecten. |
| [functionele](https://cran.r-project.org/web/packages/functional/index.html) | Currypoeders, opstellen en andere functies hoger volgorde |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Flexibele herstructureren en gegevens. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Eenvoudige en consistente aanvullende inhoud voor algemene tekenreeksbewerkingen. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Hulpprogramma's voor splitsen, toepassen en combineren van gegevens. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Hulpmiddelen voor het verplaatsen van venster Statistiek GIF-bestand, Base64, ROC AUC, enzovoort. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Benaderen tekenreeks overeenkomende en afstand functies tekenreeks. |

## <a name="install-r-using-script-actions"></a>Installeren R scriptacties gebruiken

De volgende scriptactie wordt gebruikt voor R installeren op een cluster HDInsight. https://hdiconfigactions.BLOB.Core.Windows.NET/linuxrconfigactionv01/r-Installer-v01.sh
    
In deze sectie bevat instructies over het gebruik van het script bij het maken van een nieuw cluster met behulp van de Azure portal. 

> [AZURE.NOTE] Azure PowerShell, de CLI Azure, de HDInsight .NET SDK of Azure resourcemanager sjablonen kunnen ook worden gebruikt om toe te passen scriptacties. U kunt ook scriptacties toepassen op clusters is gebeurd. Zie [aanpassen HDInsight clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md)voor meer informatie.

1. Start de inrichting van een cluster met behulp van de stappen in de [voorziening Linux gebaseerde HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md#portal), maar inrichting niet voltooien.

2. Selecteer **Scriptacties**op het blad **Optionele configuratie** en de onderstaande informatie opgeven:

    * __Naam__: Voer een beschrijvende naam voor de scriptactie.
    * __SCRIPT URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __Hoofd__: Schakel deze optie
    * __Werknemer__: Schakel deze optie
    * __ZOOKEEPER__: Schakel deze optie om op het knooppunt Zookeeper te installeren.
    * __PARAMETERS__: laat dit veld leeg

3. Gebruik de knop **selecteren** de configuratie opslaan onderaan in de **Script-acties**. Gebruik ten slotte de knop **selecteren** onderaan in het blad **Optionele configuratie** optionele configuratie informatie wilt opslaan.

4. Ga verder met het cluster inrichting zoals is beschreven in [HDInsight inrichten Linux gebaseerde clusters](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="run-r-scripts"></a>R-scripts uitvoeren

Nadat het cluster is voltooid inrichting, gebruikt u de volgende stappen R gebruiken om een bewerking MapReduce op het cluster.

1. Verbinding maken met het HDInsight cluster SSH gebruiken:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Zie de volgende onderwerpen voor meer informatie over het gebruik van SSH met HDInsight:

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Vanuit de `username@hn0-CLUSTERNAME:~$` wordt gevraagd, voert u de volgende opdracht uit een interactieve R-sessie te starten:

        R

3. Voer de volgende R-programma. Hiermee wordt de getallen 1 tot en met 100 gegenereerd en wordt deze vervolgens vermenigvuldigd met 2.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    De eerste regel oproepen de RHadoop bibliotheek rmr2, die wordt gebruikt voor MapReduce bewerkingen.

    De tweede regel genereert waarden 1-100, en slaat u deze naar de Hadoop bestandssysteem via `to.dfs`.

    De derde regel maakt een MapReduce-proces met de functionaliteit van rmr2 en verwerkt. Hier ziet u meerdere regels schuift dan, zoals de verwerking begint.

4. Gebruik vervolgens de volgende handelingen uit om het pad dat de uitvoer MapReduce is opgeslagen op tijdelijk weer te geven:

        print(calc())

    Dit moet er ongeveer uitzien als `/tmp/file5f615d870ad2`. Weergave van de werkelijke uitvoer, gebruikt u de volgende:

        print(from.dfs(calc))

    De uitvoer ziet er als volgt:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. Naar afsluiten R, typ het volgende:

        q()


## <a name="next-steps"></a>Volgende stappen

- [Installeren en gebruiken kleurtoon op HDInsight clusters](hdinsight-hadoop-hue-linux.md). Kleurtoon is web UI waarmee u gemakkelijk maken, uitvoeren en opslaan varken en component taken, evenals bladeren de standaard-opslag voor uw HDInsight cluster.

- [Giraph op HDInsight clusters installeren](hdinsight-hadoop-giraph-install.md). Gebruik cluster aanpassing Giraph installeren op HDInsight Hadoop clusters. Giraph kunt u graph verwerking met Hadoop uitvoeren en deze kan worden gebruikt met Azure HDInsight.

- [Solr op HDInsight clusters installeren](hdinsight-hadoop-solr-install.md). Gebruik cluster aanpassing Solr installeren op HDInsight Hadoop clusters. Solr kunt u krachtige zoekbewerkingen uitvoeren op gegevens in cache.

- [Kleurtoon op HDInsight clusters installeren](hdinsight-hadoop-hue-linux.md). Gebruik cluster aanpassing Kleurtoon op HDInsight Hadoop clusters installeren. Kleurtoon is een verzameling webtoepassingen gebruikt om te communiceren met een Hadoop-cluster.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
