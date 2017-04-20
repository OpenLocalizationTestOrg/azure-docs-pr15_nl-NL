<properties
    pageTitle="Kleurtoon gebruiken met Hadoop op HDInsight Linux clusters | Microsoft Azure"
    description="Informatie over het installeren en gebruiken van tint met Hadoop clusters op HDInsight Linux."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van kleurtoon op HDInsight Hadoop clusters

Leer hoe u de kleurtoon op HDInsight Linux clusters installeren en gebruiken van tunneling de verzoeken om om te leiden kleurtoon.

## <a name="what-is-hue"></a>Wat is tint?

Tint is een verzameling webtoepassingen gebruikt om te communiceren met een Hadoop-cluster. U kunt kleurtoon Blader de opslag die is gekoppeld aan een Hadoop-cluster (WASB, in het geval van HDInsight clusters), uitvoeren component taken en varken scripts, enzovoort. De volgende onderdelen zijn beschikbaar voor communicatie met tint installaties op een cluster HDInsight Hadoop.

* Bijenwas component Editor
* Varken
* Metastore manager
* Oozie
* FileBrowser (dat gesprekken voert WASB standaardcontainer)
* Taak-Browser

> [AZURE.WARNING] Onderdelen van het cluster HDInsight volledig worden ondersteund en Microsoft Support helpt isoleren en oplossen van problemen met deze onderdelen.
>
> Aangepaste onderdelen ontvangen commercieel redelijk ondersteuning om u te helpen u verder problemen met het probleem. Dit kan leiden tot het probleem oplossen of vraag of u kunt voeren beschikbare kanalen voor de bron openen technologieën waar uitgebreide expertise voor deze technologie is gevonden. Bijvoorbeeld, zijn er veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache projecten tevens projectsites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Installeren kleurtoon scriptacties gebruiken

De volgende scriptactie kan worden gebruikt tint installeren op een cluster Linux gebaseerde HDInsight.
https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-HUE-uber-v02.sh
    
In deze sectie bevat instructies over het gebruik van het script bij de inrichting van het cluster met behulp van de Azure-Portal. 

> [AZURE.NOTE] Azure PowerShell, de CLI Azure, de HDInsight .NET SDK of Azure resourcemanager sjablonen kunnen ook worden gebruikt om toe te passen scriptacties. U kunt ook scriptacties toepassen op clusters is gebeurd. Zie [aanpassen HDInsight clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md)voor meer informatie.

1. Start de inrichting van een cluster met behulp van de stappen in de [voorziening HDInsight clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), maar inrichting niet voltooien.

    > [AZURE.NOTE] Als u wilt installeren tint op HDInsight clusters, de grootte van de aanbevolen headnode is ten minste A4 (8-cores, 14 GB geheugen vereist).

2. Selecteer **Scriptacties**op het blad **Optionele configuratie** en geef de informatie, zoals hieronder wordt weergegeven:

    ![Opgeven script actieparameters voor tint] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Opgeven script actieparameters voor tint")

    * __Naam__: Voer een beschrijvende naam voor de scriptactie.
    * __SCRIPT URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __Hoofd__: Schakel deze optie
    * __Werknemer__: laat dit vak leeg.
    * __ZOOKEEPER__: laat dit vak leeg.
    * __PARAMETERS__: laat dit vak leeg.

3. Gebruik de knop **selecteren** de configuratie opslaan onderaan in de **Script-acties**. Gebruik ten slotte de knop **selecteren** onderaan in het blad **Optionele configuratie** optionele configuratie informatie wilt opslaan.

4. Ga verder met het cluster inrichting zoals is beschreven in de [voorziening HDInsight clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="use-hue-with-hdinsight-clusters"></a>Tint gebruiken met HDInsight clusters

SSH Tunneling is de enige manier toegang hebt tot kleurtoon op het cluster wanneer deze wordt uitgevoerd. Tunneling via SSH, kan het verkeer naar het Ga rechtstreeks naar het headnode van het cluster waarop tint wordt uitgevoerd. Nadat het cluster is voltooid inrichting, gebruikt u de volgende stappen tint gebruiken op een cluster HDInsight Linux.

1. Gebruik van de informatie in [Gebruik SSH tunnel naar Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, en andere web van UI openen](hdinsight-linux-ambari-ssh-tunnel.md) om te maken van een tunnel SSH van uw clientsysteem naar het cluster HDInsight en configureer uw webbrowser als u wilt de tunnel SSH gebruiken als proxy.

2. Zodra u hebt een tunnel SSH gemaakt en geconfigureerd van uw browser naar proxy-verkeer is toegestaan door de werkmap, moet u de hostnaam van het primaire hoofd knooppunt vinden. U kunt dit doen door verbinding te maken met het cluster met SSH op poort 22. Bijvoorbeeld `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` waarbij __gebruikersnaam__ uw gebruikersnaam SSH is en __CLUSTERNAAM__ is de naam van uw cluster.

    Zie de volgende documenten voor meer informatie over het gebruik van SSH:

    * [SSH gebruiken met Linux gebaseerde HDInsight van een client Linux, Unix of Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH gebruiken met Linux gebaseerde HDInsight vanuit een Windows-client](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Zodra u verbinding hebt, gebruikt u de volgende opdracht uit de FQDN-naam van de primaire headnode verkrijgen:

        hostname -f

    Hiermee herstelt u een naam ongeveer als volgt uit:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    Dit is de hostnaam van de primaire headnode waarin de tint-website zich bevindt.

2. Via de browser naar de kleurtoon-portal op http://HOSTNAME:8888 openen. HOSTNAME vervangen door de naam die u in de vorige stap hebt aangeschaft.

    > [AZURE.NOTE] Wanneer u zich voor de eerste keer aanmeldt, wordt u gevraagd om een account aan te melden bij de portal kleurtoon te maken. De referenties die u hier opgeeft, worden beperkt tot de portal en niet zijn gerelateerd aan de beheerder of SSH gebruikersreferenties u hebt opgegeven terwijl inrichten het cluster.

    ![Meld u aan bij de portal tint] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Geef de referenties voor tint-portal")

### <a name="run-a-hive-query"></a>Component query's uitvoeren

1. In de portal tint **Query Editors**op en klik vervolgens op **component** om de component-editor te openen.

    ![Component gebruiken] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Component gebruiken")

2. Klik op het tabblad **helpen** onder **Database**, ziet u **hivesampletable**. Dit is een voorbeeldtabel die wordt geleverd bij alle Hadoop clusters op HDInsight. Voer een voorbeeldquery in het rechterdeelvenster en raadpleegt u de uitvoer op het tabblad **resultaten** in het venster onder, zoals wordt weergegeven in de schermopname.

    ![Query uitvoeren component] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Query uitvoeren component")

    U kunt ook het tabblad **grafiek** gebruiken om een visuele weergave van het resultaat weer te geven.

### <a name="browse-the-cluster-storage"></a>Bladeren in de clusteropslag

1. In de portal tint klikt u in de rechterbovenhoek van de menubalk op **Bestandsbrowser** .

2. Standaard is de bestandsbrowser wordt geopend in de map **/user/myuser** . Klik op de schuine streep rechts voordat u de lijst met gebruikers in het pad naar het Ga naar de hoofdsite van de Azure opslag container die is gekoppeld aan het cluster.

    ![Gebruik bestandsbrowser] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Gebruik bestandsbrowser")

3. Klik met de rechtermuisknop op een bestand of map om de beschikbare bewerkingen weer te geven. Gebruik de knop **uploaden** in de rechterhoek bestanden uploaden naar de huidige map. Gebruik de knop **Nieuw** om nieuwe bestanden of mappen te maken.

> [AZURE.NOTE] De kleurtoon bestandsbrowser kunt alleen de inhoud van de standaard-container die is gekoppeld aan het cluster HDInsight weergeven. Geen extra opslagruimte accounts/containers die u mogelijk hebt verbonden met het cluster worden niet toegankelijk via de bestandsbrowser. Echter zijn de extra containers die is gekoppeld aan het cluster altijd toegankelijk is voor de component-taken. Als u de opdracht Voer bijvoorbeeld `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` in de component-editor, ziet u de inhoud van extra containers ook. In deze opdracht is **newcontainer** niet de standaard-container die is gekoppeld aan een cluster.

## <a name="important-considerations"></a>Belangrijke overwegingen

1. Het script gebruikt om te installeren kleurtoon alleen op de primaire headnode van het cluster geïnstalleerd.

2. Meerdere Hadoop-services (HDFS, garens, MR2, Oozie) worden tijdens de installatie opnieuw gestart voor het bijwerken van de configuratie. Nadat het script is voltooid kleurtoon installeren, is het duurt enige tijd voor andere Hadoop-services te starten. Dit mogelijk trager kleurtoon van in eerste instantie. Nadat alle services worden gestart, wordt kleurtoon volledig functioneel zijn.

3.  Kleurtoon worden niet-begrepen Tez taken, de huidige standaardinstelling voor component. Als u MapReduce gebruiken als de component execution engine wilt, update voor het script om de volgende opdracht in het script gebruiken:

        set hive.execution.engine=mr;

4.  U kunt een scenario waarbij uw services worden uitgevoerd op de primaire headnode terwijl de Manager van de Resource kan worden uitgevoerd op de secundaire hebben met Linux kolomgroepen. Dit scenario kan leiden tot fouten (Zie hieronder) wanneer u details weergeven van taken uitvoeren op het cluster met kleurtoon. U kunt echter de taakdetails weergeven wanneer de taak is voltooid.

    ![Portal fout tint] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Kleurtoon portal fout")

    Dit is vanwege een bekend probleem. Als een tijdelijke Ambari zo aanpassen dat de actieve resourcemanager ook op de primaire headnode wordt uitgevoerd.

5.  Kleurtoon begrijpt WebHDFS terwijl HDInsight clusters use Azure Storage gebruiken `wasbs://`. Zo is, wordt de aangepast script gebruikt met scriptactie WebWasb, dat wil een WebHDFS-compatibele service zeggen voor het bespreken naar WASB geïnstalleerd. Ja, hoewel de portal tint zegt HDFS op plaatsen (zoals wanneer u de muisaanwijzer op het **Bestandsbrowser**plaatst), moet dit worden beschouwd als WASB.


## <a name="next-steps"></a>Volgende stappen

- [Giraph op HDInsight clusters installeren](hdinsight-hadoop-giraph-install-linux.md). Gebruik cluster aanpassing Giraph installeren op HDInsight Hadoop clusters. Giraph kunt u graph verwerking met Hadoop uitvoeren en deze kan worden gebruikt met Azure HDInsight.

- [Solr op HDInsight clusters installeren](hdinsight-hadoop-solr-install-linux.md). Gebruik cluster aanpassing Solr installeren op HDInsight Hadoop clusters. Solr kunt u krachtige zoekbewerkingen uitvoeren op gegevens in cache.

- [R op HDInsight clusters installeren](hdinsight-hadoop-r-scripts-linux.md). Gebruik cluster aanpassing R installeren op HDInsight Hadoop clusters. R is een open source taal- en -omgeving voor statistische computing. Deze bevat honderden ingebouwde statistische functies en een eigen programmeertaal die worden gecombineerd aspecten van het functionele en object-georiënteerd programmeren. Ook vindt u hier de uitgebreide mogelijkheden voor grafische.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
