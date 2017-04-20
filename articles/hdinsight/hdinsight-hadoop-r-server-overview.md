<properties
    pageTitle="Wat is R op HDInsight? Inleiding tot R-Server op HDInsight (preview) | Microsoft Azure"
    description="Wat is R-Server op HDInsight (preview) en het gebruik van R-Server voor het maken van toepassingen voor groot gegevensanalyse."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>Overzicht van R-Server op HDInsight \(preview\)

Met Microsoft Azure HDInsight Premium, Microsoft R Server is nu beschikbaar als optie wanneer u HDInsight clusters in Azure wordt aangegeven maakt. Deze nieuwe functionaliteit biedt gegevens wetenschappers, statistici en R programmeurs met bellen op toegang tot scalable, distributed methoden voor het analytics op HDInsight.

Clusters kunnen worden waarvan de projecten en taken waaraan u bezig bent het formaat en wanneer deze niet meer nodig zijn verwijderd. Aangezien ze deel uit van Azure HDInsight bent, is deze clusters worden geleverd met ondersteuning voor enterprise-level 24 x 7, een SLA van 99,9% beschikbaarheid en de flexibiliteit integreren met andere onderdelen in het Azure-systeem.

>[AZURE.NOTE] R-Server is alleen beschikbaar met HDInsight Premium. HDInsight Premium is momenteel alleen beschikbaar voor Hadoop en een clusters. Ja, momenteel kunt u R Server alleen voor gebruik met Hadoop en een clusters op HDInsight. Zie voor meer informatie [Wat zijn de verschillende niveaus en Hadoop-onderdelen beschikbaar voor communicatie met HDInsight?](hdinsight-component-versioning.md).

R-Server op HDInsight biedt de nieuwste mogelijkheden van analysegegevens R gebaseerde aan grote gegevenssets die met Azure-blobopslag worden geladen. Aangezien R-Server is gebaseerd op bron openen R, kunnen de R-toepassingen die u samenstelt gebruikmaken van een van de bron openen R-pakketten 8000 +, evenals de routines in ScaleR, Microsofts groot gegevens analytics-pakket dat is inbegrepen in R-Server.

Het randknooppunt van de Premium-clusters is een handige locatie verbinding maken met de cluster en uw R-scripts worden uitgevoerd. Met een randknooppunt hebt u de optie van het uitvoeren van ScaleR parallelized verdeelde functies over de cores van de randserver knooppunt. U hebt ook de mogelijkheid om uit te voeren ze op de knooppunten van het cluster met behulp van ScaleR Hadoop kaart verkleinen of een contexten berekenen.

De modellen of voorspellingen dat resultaat uit analyses kan worden gedownload voor gebruik on-premises implementatie. Ze kunnen ook worden geoperationaliseerd ergens anders in Azure wordt aangegeven, zoals via een [Azure Machine Learning Studio](http://studio.azureml.net) - [webservice](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Aan de slag met R op HDInsight

Als u wilt opnemen R-Server in een cluster HDInsight, moet u een Hadoop- of een cluster maken met de optie Premium wanneer u een cluster maken met behulp van de Azure-portal. Beide clustertypen gebruik dezelfde configuratie, waaronder R-Server op de gegevensknooppunten van een cluster en een randknooppunt als een zone aantekening van analysegegevens R-servers zijn gebaseerd. Zie [Aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md) voor een uitgebreide overzicht over het maken van een cluster.

## <a name="learn-about-data-storage-options"></a>Meer informatie over opslagopties van gegevens

Standaard-opslag voor HDInsight clusters is met het bestandssysteem HDFS is toegewezen aan een container blob-blobopslag. Dit zorgt ervoor dat welke gegevens is geüpload naar de clusteropslag of naar clusteropslag in de loop van analyse geschreven, bestaat uit de permanente. U kunt het hulpprogramma [AzCopy](../storage/storage-use-azcopy.md) gebruiken om gegevens te kopiëren naar en vanuit het blob.

Naast blobopslag, hebt u de optie van het gebruik van [Azure Lake gegevensopslag](https://azure.microsoft.com/services/data-lake-store/) met uw cluster. Als u gegevens Lake gebruikt, kunt klikt u vervolgens u zowel blobopslag en gegevens Lake voor HDFS opslag.

U kunt ook [Azure-bestanden](../storage/storage-how-to-use-files-linux.md) gebruiken als een opslagoptie voor de voor gebruik op het randknooppunt. Azure-bestanden kunt u een bestandsshare die is gemaakt in Azure Storage om het bestandssysteem Linux te koppelen. Zie voor meer informatie over opties voor het opslaan van gegevens voor R-Server op HDInsight cluster, [opslagopties voor R-Server op HDInsight clusters](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>-R-Server op het cluster

Nadat u een cluster met R-Server hebt gemaakt, kunt u de R-console op het randknooppunt van het cluster via SSH/stopverf verbinden. U kunt ook verbinding maakt via een browser als u de optionele RStudio Server IDE installeert op het randknooppunt. Zie voor meer informatie over het installeren van RStudio Server [RStudio Server installeren op HDInsight clusters](hdinsight-hadoop-r-server-install-r-studio.md).   

## <a name="develop-and-run-r-scripts"></a>Ontwikkelen en uitvoeren van scripts R

De R-scripts u maken en uitvoeren kunnen een van de 8000 + bron openen R-pakketten naast de parallelized en verdeelde routines in de bibliotheek ScaleR gebruiken. In het algemeen script dat R Server wordt uitgevoerd op het randknooppunt uitgevoerd binnen de interpreter R op dat knooppunt. De uitzondering is deze stappen waarmee een ScaleR correct met een berekeningscluster context die wordt ingesteld Hadoop kaart verminderen (RxHadoopMR) of een (RxSpark).

In dat geval de functie wordt uitgevoerd op een wijze verdeeld over de gegevens die (taak) knooppunten van het cluster die zijn gekoppeld aan de gegevens waarnaar wordt verwezen. Zie [context opties voor R-Server op HDInsight Premium berekenen](hdinsight-hadoop-r-server-compute-contexts.md)voor meer informatie over de verschillende berekeningscluster context opties.

## <a name="operationalize-a-model"></a>Een model mogelijk te maken

Als uw gegevensmodellering voltooid is, kunt u het model zodat voorspellingen voor nieuwe gegevens zowel in Azure en on-premises mogelijk te maken. Dit proces wordt scores genoemd. Hier volgen enkele voorbeelden.

### <a name="score-in-hdinsight"></a>Score in HDInsight

Als u wilt score in HDInsight, een R-functie die uw model belt zodat voorspellingen voor een nieuw gegevensbestand dat u hebt geladen bij uw account opslag te schrijven. Sla de voorspellingen terug naar het opslag-account. U kunt de lopende op aanvraag uitvoeren op het randknooppunt van het cluster of met behulp van een geplande taak.  

### <a name="score-in-azure-machine-learning"></a>Score in Azure Machine Learning

Als u wilt score met behulp van een Azure Machine Learning-webservice, gebruik de bron openen Azure Machine Learning-R-pakket bekend als [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) publiceren van uw model als een Azure webservice. Uitkomt is dit pakket vooraf geïnstalleerd op het randknooppunt. Vervolgens de faciliteiten van Machine Learning gebruiken om te maken van een gebruikersinterface voor de webservice en vervolgens de webservice bellen naar wens voor het scoren.

Als u deze optie kiest, moet u de ScaleR model-objecten converteren naar equivalente open source modelobjecten voor gebruik met de webservice. U kunt dit doen met behulp van ScaleR dwangbewerking functies, zoals `as.randomForest()` voor ensemble gebaseerde-modellen.


### <a name="score-on-premises"></a>Score on-premises implementatie

Als u wilt score on-premises implementatie na het maken van uw model, kunt u serialiseren van het model in R, downloadt u het, terugconverteren deze en vervolgens gebruiken voor nieuwe gegevens scores. Scoort nieuwe gegevens kunt u met behulp van de methode die eerder is beschreven in [score in HDInsight](#scoring-in-hdinsight) of met behulp van [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Voor het behoud van het cluster

### <a name="install-and-maintain-r-packages"></a>Installeren en voor het behoud van R-pakketten

De R-pakketten die u gebruikt de meeste moet op het randknooppunt aangezien het grootste deel van uw R-scripts er kan worden uitgevoerd. Als u wilt installeren extra R-pakketten op het randknooppunt, kunt u de gebruikelijke `install.packages()` methode in R.

In de meeste gevallen moet u geen extra R-pakketten installeren op de gegevensknooppunten als u alleen routines uit de diatheek ScaleR gehele cluster. Echter, moet u wellicht extra pakketten ter ondersteuning van gebruik van **rxExec** of **RxDataStep** kan worden uitgevoerd op de gegevensknooppunten.

In dat geval moeten de extra pakketten door gebruik te maken van een scriptactie worden opgegeven nadat u het cluster hebt gemaakt. Zie [maken van een HDInsight cluster met R-Server](hdinsight-hadoop-r-server-get-started.md)voor meer informatie.   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Hadoop-kaart verkleinen geheugeninstellingen wijzigen

Een cluster kan worden gewijzigd als u wilt wijzigen van de hoeveelheid geheugen die beschikbaar is voor R-Server tijdens het uitvoeren van een taak verkleinen van de kaart. Als u wilt wijzigen van een cluster, gebruikt u de Apache Ambari gebruikersinterface die beschikbaar is via het Azure portal blad voor uw cluster. Zie voor instructies over toegang tot de Ambari UI voor uw cluster [beheren HDInsight clusters met de gebruikersinterface van de Web Ambari](hdinsight-hadoop-manage-ambari.md).

Het is ook mogelijk om te wijzigen van de hoeveelheid geheugen die beschikbaar is voor R-Server met behulp van Hadoop schakelopties in de oproep door naar **RxHadoopMR** als volgt:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>De schaal van uw cluster aanpassen

Een bestaand cluster kan worden vergroot omhoog of omlaag via de portal. Kunt u de extra functie die u nodig voor grotere verwerkingstaken hebt mogelijk toegang door de schaal of kunt u de schaal terug een cluster als deze niet actief is. Zie voor instructies over hoe u de schaal van een cluster [clusters HDInsight beheren](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Voor het behoud van het systeem

Onderhoud wordt uitgevoerd op de onderliggende Linux VMs in een cluster HDInsight kantooruren OS patches en andere updates toe te passen. Meestal is onderhoud klaar om 3:30 AM (gebaseerd op de lokale tijd voor de VM) elke maandag en donderdag. Updates worden zodanig dat ze niet van invloed op meer dan een kwartaal van het cluster tegelijkertijd zijn uitgevoerd.  

Aangezien de hoofd knooppunten overtollige zijn en niet alle gegevensknooppunten ondervindt, vertragen alle taken die worden uitgevoerd tijdens deze periode. Ze moeten nog steeds worden uitgevoerd tot voltooiing, echter. Een aangepaste software of lokale gegevens die u hebt blijft behouden over deze gebeurtenissen onderhoud tenzij een volledige uitval optreedt die moet worden opgebouwd cluster.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Meer informatie over IDE opties voor R-Server op een cluster HDInsight

Het knooppunt Linux rand van een cluster HDInsight Premium is de aantekening-zone voor R gebaseerde analyse. Nadat u verbinding maakt met het cluster, kunt u de console-interface met R-Server starten door te typen **R** bij de opdrachtprompt Linux. Gebruik van de console-interface is verbeterd als u een teksteditor R ontwikkelen van scripts worden uitgevoerd in een ander venster en knippen en plakken van secties van het script in de R-console naar wens.

Een nog fraaiere hulpmiddel voor de ontwikkeling van uw R-script is de IDE R gebaseerde voor gebruik op het bureaublad, zoals Microsoft aangekondigd onlangs [R Tools voor Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS). Dit is een reeks hulpmiddelen voor de bureaublad- en server van [RStudio](https://www.rstudio.com/products/rstudio-server/). U kunt ook de Walware Eclips gebaseerde [StatET](http://www.walware.de/goto/statet)gebruiken.

Er is een andere optie voor het installeren van een IDE op het Linux randknooppunt zelf.  Een populaire keuze is [RStudio Server](https://www.rstudio.com/products/rstudio-server/)met een browser gebaseerde IDE waarmee door externe clients wordt gebruikt. RStudio Server installeren op het randknooppunt van een cluster HDInsight Premium beschikt u over een volledige IDE ervaring voor de ontwikkeling en R scripts met R-Server op het cluster worden uitgevoerd. Dit kan productiever aanzienlijk dan de R-console.  Als u gebruiken RStudio Server wilt, raadpleegt u [RStudio Server installeren op HDInsight clusters](hdinsight-hadoop-r-server-install-r-studio.md).

## <a name="learn-about-pricing"></a>Meer informatie over de prijzen

De kosten die zijn gekoppeld aan een HDInsight Premium cluster met R-Server worden op dezelfde manier gestructureerd aan de kosten voor de standaard HDInsight clusters. Ze zijn gebaseerd op de grootte van de onderliggende VMs over de naam, gegevens en rand knooppunten, met het toevoegen van een opwaartse core-uurs voor Premium. Zie voor meer informatie over HDInsight Premium prijzen, inclusief prijzen tijdens Public Preview en de beschikbaarheid van een gratis proefabonnement van 30 dagen, [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Volgende stappen

Volg de onderstaande koppelingen voor meer informatie over het gebruik van de Server R met HDInsight clusters.

- [Aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md)

- [RStudio Server toevoegen aan HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)

- [Context-opties voor R-Server op HDInsight (preview) berekenen](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure opslagopties voor R-Server op HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
