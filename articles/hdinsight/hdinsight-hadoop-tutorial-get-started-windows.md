<properties
   pageTitle="Hadoop zelfstudie: aan de slag met Hadoop op Windows | Microsoft Azure"
   description="Aan de slag met Hadoop in HDInsight. Informatie over het maken van Hadoop clusters in Windows, component query's uitvoeren op gegevens en analyseren uitvoer in Excel."
   keywords="hadoop zelfstudie hadoop in windows, hadoop cluster, meer hadoop, component query"
   services="hdinsight"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="03/07/2016"
   ms.author="nitinme"/>


# <a name="hadoop-tutorial-get-started-using-hadoop-in-hdinsight-on-windows"></a>Hadoop zelfstudie: aan de slag met Hadoop in HDInsight in Windows

> [AZURE.SELECTOR]
- [Linux gebaseerde](../hdinsight-hadoop-linux-tutorial-get-started.md)
- [Op basis van Windows](../hdinsight-hadoop-tutorial-get-started-windows.md)

Als u meer Hadoop in Windows te gaan met HDInsight, ziet deze zelfstudie u hoe u een component-query uitvoeren op ongestructureerde gegevens in een Hadoop-cluster en vervolgens de resultaten in Microsoft Excel analyseren.

>[AZURE.NOTE] De informatie in dit document hoort bij op basis van Windows HDInsight clusters. Zie voor informatie over Linux gebaseerde clusters [Hadoop zelfstudie: aan de slag met Linux gebaseerde Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

Wordt ervan uitgegaan dat u een grote ongestructureerde gegevensverzameling hebt en u wilt uitvoeren van een query component erop te extraheren enkele nuttige informatie. Dat is precies wat u wilt doen in deze zelfstudie. Hier ziet u hoe u dit wilt bereiken:

   !["Hadoop zelfstudie: Maak een account; maken van een cluster Hadoop. verzenden van een query component; gegevens analyseren in Excel.][image-hdi-getstarted-flow]

Bekijk een demovideo van deze zelfstudie voor meer informatie over Hadoop op HDInsight:

![Video van een eerste Hadoop-zelfstudie: een query component op een cluster Hadoop indienen en analyseren resultaten in Excel.][img-hdi-getstarted-video]

**[Bekijk de Hadoop-zelfstudie voor HDInsight op YouTube](https://www.youtube.com/watch?v=Y4aNjnoeaHA&list=PLDrz-Fkcb9WWdY-Yp6D4fTC1ll_3lU-QS)**

Microsoft biedt in combinatie met de algemene beschikbaarheid van Azure HDInsight, ook HDInsight Emulator voor Azure, voorheen bekend als *Microsoft HDInsight Developer Preview*. De Emulator is bedoeld voor ontwikkelaars scenario's en biedt alleen ondersteuning voor één knooppunt implementaties. Zie voor informatie over het gebruik van HDInsight Emulator [Aan de slag met de HDInsight Emulator][hdinsight-emulator].

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie voor Hadoop in Windows, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **A Werkstationcomputer** met Office 2013 Professional Plus, Office 365 Pro Plus, zelfstandige versie van Excel 2013 of Office 2010 Professional Plus.

### <a name="access-control-requirements"></a>Vereisten voor het beheer van Access

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-hadoop-clusters"></a>Hadoop clusters maken

Wanneer u een cluster maakt, maakt u Azure berekeningscluster resources met Hadoop en verwante toepassingen. In dit gedeelte maakt u een HDInsight versie 3,2 cluster. U kunt ook Hadoop clusters voor andere versies maken. Zie voor instructies [maken HDInsight clusters met aangepaste opties][hdinsight-provision]. Zie voor informatie over HDInsight versies en hun serviceovereenkomsten [HDInsight onderdeel versiebeheer](hdinsight-component-versioning.md).


**Een Hadoop-cluster maken**

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com/).
2. Klik op **Nieuw**, klik op **Gegevens analyse**en klik vervolgens op **HDInsight**. De portal wordt een **Nieuw HDInsight Cluster** blade geopend.

    ![Een nieuw cluster in de Portal Azure maken] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.1.png "Een nieuw cluster in de Portal Azure maken")

3. Typ of Selecteer de volgende opties:

    ![ENTER clusternaam en type] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.2.png "ENTER clusternaam en type")
    
  	|Veldnaam| Waarde|
  	|----------|------|
  	|De naam van cluster| Een unieke naam voor het cluster identificeren|
  	|Clustertype| Selecteer **Hadoop** voor deze zelfstudie. |
  	|Cluster-besturingssysteem| Selecteer **Windows Server 2012 R2 Datacenter** voor deze zelfstudie.|
  	|HDInsight-versie| Selecteer de meest recente versie voor deze zelfstudie.|
  	|Abonnement| Selecteer het Azure abonnement dat wordt gebruikt voor het cluster.|
  	|Resourcegroep | Selecteer een bestaande Azure resourcegroep of maak een nieuwe resourcegroep. Een eenvoudige HDInsight cluster bevat een cluster en het standaardaccount voor de opslag.  U kunt de twee in een resourcegroep voor eenvoudig beheer groeperen.|
  	|Referenties| Voer het cluster login gebruikersnaam en wachtwoord. Cluster van Windows op basis kunt 2 gebruikersaccounts hebben.  De gebruiker cluster (of HTTP-gebruiker) wordt gebruikt voor het beheren van het cluster en taken.  U kunt desgewenst maken met een extern bureaublad (RDP) gebruikersaccount naar externe verbinding maken met het cluster. Als u extern bureaublad inschakelt, maakt u het gebruikersaccount RDP.|
  	|Gegevensbron| Klik op Nieuw om een nieuwe standaard Azure opslag-account maken. Naam van het cluster als de standaardnaam van de container gebruiken. Elk cluster HDinsight heeft een standaard Blob container van een Azure opslag-accounts.  De locatie van het standaardaccount Azure opslag bepaalt de locatie van het cluster HDInsight.|
  	|Knooppunt prijzen van lagen| 1 of 2 werknemer knooppunten met de werknemer knooppunt en hoofd standaardmededeling laag prijzen voor deze zelfstudie gebruiken.|
  	|Optionele configuratie| In dit gedeelte overslaan.|

9. Zorg ervoor dat **vastmaken aan Startboard** is geselecteerd en klik vervolgens op **maken**op het blad **Nieuwe HDInsight Cluster** . Hiermee maakt u het cluster en een tegel voor deze toevoegen aan de Startboard van uw Azure-Portal. Het pictogram wordt aangegeven dat het cluster maakt en de HDInsight als pictogram wilt weergeven wanneer maken is voltooid wordt gewijzigd.

  	| Tijdens het maken van | Maken is voltooid |
  	| ------------------ | --------------------- |
  	| ![Indicator maken voor startboard](./media/hdinsight-hadoop-tutorial-get-started-windows/provisioning.png) | ![Cluster tegel gemaakt](./media/hdinsight-hadoop-tutorial-get-started-windows/provisioned.png) |

    > [AZURE.NOTE] Het duurt enige tijd voor het cluster moet worden gemaakt, meestal ongeveer 15 minuten. De tegel op het Startboard of het fragment **meldingen** aan de linkerkant van de pagina gebruiken om te controleren of het maakproces.

10. Zodra de aanmaakdatum is voltooid, klikt u op de tegel voor het cluster uit de Startboard aan het blad cluster starten.


## <a name="run-a-hive-query-from-the-portal"></a>Component query's uitvoeren in de portal
Nu dat u een cluster HDInsight hebt gemaakt, wordt de volgende stap is een taak onderdeel als u wilt een component voorbeeldtabel query uit te voeren. We gebruiken *hivesampletable*, waarbij HDInsight clusters wordt geleverd. De tabel bevat gegevens over fabrikanten van mobiele apparaten, platforms en -modellen. Een query component op deze tabel gegevens voor mobiele apparaten door een specifieke fabrikant opgehaald.

> [AZURE.NOTE] Voor .NET versie 2.5 of hoger, wordt geleverd met de SDK Azure HDInsight Tools voor Visual Studio. Met behulp van de hulpmiddelen in Visual Studio, kunt u verbinding maken met HDInsight cluster, component tabellen maken en component query's uitvoeren. Zie voor meer informatie [aan de slag met HDInsight Hadoop Tools for Visual Studio][1].

**Een taak onderdeel uitvoeren vanuit het dashboard cluster**

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com/).
2. Klik op **Door alles bladeren** en klik vervolgens op **HDInsight Clusters** om een lijst met kolomgroepen inclusief het cluster die u zojuist hebt gemaakt in de vorige sectie.
3. Klik op de naam van het cluster die u wilt gebruiken het onderdeel taak uit te voeren en klik vervolgens op **Dashboard** aan de bovenkant van het blad.
4. Een webpagina wordt geopend in de tab van een andere browser. Voer het Hadoop-gebruikersaccount en wachtwoord. De standaardnaam van de gebruiker is **beheerder**; het wachtwoord is wat u bij het maken van het cluster hebt ingevoerd.
5. Vanuit het dashboard, klikt u op het tabblad **Editor component** . De volgende webpagina wordt geopend.

    ![Tabblad van de component Editor in het dashboard van de cluster HDInsight.][img-hdi-dashboard]

    Zijn er meerdere tabbladen aan de bovenkant van de pagina. Het standaardtabblad **Component Editor**en de andere tabbladen **Werkervaring** en **Bestandsbrowser**werkt. Met behulp van het dashboard, kunt u indienen component query's, Hadoop taak logboeken controleren en blader bestanden in de opslag.

    > [AZURE.NOTE] De URL van de webpagina wordt * &lt;clusternaam&gt;. azurehdinsight.net*. Dus in plaats van het dashboard van de portal te openen, kunt u openen het dashboard vanuit een webbrowser met behulp van de URL.

6. Voer op het tabblad **Editor component** voor **De naam van de Query**, **HTC20**.  De naam van de query is de titel van de taak. Klik in het Querydeelvenster voert u de query component zoals wordt weergegeven in de afbeelding:

    ![Component query ingevoerd in het Querydeelvenster van de component-Editor.][img-hdi-dashboard-query-select]

4. Klik op **verzenden**. Duurt het even om de resultaten terug te gaan. Het scherm vernieuwd om 30 seconden. U kunt ook klikken op **vernieuwen** als u wilt vernieuwen van het scherm.

    ![Resultaten van een query component in vermeld onder aan het dashboard cluster.][img-hdi-dashboard-query-select-result]

5. Nadat de status geeft aan dat de taak is voltooid, klikt u op de naam van de query in het scherm om de uitvoer weer te geven. Maak een notitie van **Taak starten Time (UTC)**. Moet u deze later.

    ![Taak begintijd weergegeven op het tabblad Overzicht van functies van het dashboard van de cluster HDInsight.][img-hdi-dashboard-query-select-result-output]

    De pagina ziet u ook de **Uitvoer van de taak** en de **Functie Log**. U hebt ook de mogelijkheid om te downloaden van het uitvoerbestand (\_stdout) en het logboekbestand \(_stderr).


**Om te bladeren naar het uitvoerbestand**

1. Klik op **Bestandsbrowser**op het dashboard cluster.
2. Klik op de naam van uw opslag-account, klikt u op de containernaam van de (dit is hetzelfde als de clusternaam van uw) en klik vervolgens op **gebruiker**.
3. Klik op **beheerder** en klik op de GUID die het laatst gewijzigd (iets nadat de taak begintijd die u eerder hebt genoteerd) heeft. Kopieer deze GUID. Dit moet u in het volgende gedeelte.


    ![De query component uitvoerbestand die GUID vermeld in het tabblad bestandsbrowser.][img-hdi-dashboard-query-browse-output]


##<a name="connect-to-microsoft-business-intelligence-tools-for-excel"></a>Verbinding maken met hulpprogramma's voor Microsoft business intelligence voor Excel

De Power Query-invoegtoepassing voor Microsoft Excel kunt u de taak-uitvoer uit HDInsight importeren in Excel, waar hulpprogramma's voor Microsoft business intelligence kunnen worden gebruikt om de resultaten verder te analyseren.

U moet Excel 2013 of 2010 is geïnstalleerd om te voltooien van dit onderdeel van de zelfstudie hebben.

**Microsoft Power Query voor Excel downloaden**

- Microsoft Power Query voor Microsoft Excel vanuit het [Microsoft Downloadcentrum](http://www.microsoft.com/download/details.aspx?id=39379) downloaden en installeren.

**HDInsight gegevens te importeren**

1. Open Excel en maak een nieuwe werkmap.
3. Klik op het menu **Power Query** op **Uit een andere bron**en klik vervolgens op **Uit Azure HDInsight**.

    ![Excel PowerQuery importeren menu openen voor Azure HDInsight.][image-hdi-gettingstarted-powerquery-importdata]

3. Voer de **Naam van het Account** van de Azure-blobopslag-account dat is gekoppeld aan uw cluster en klik vervolgens op **OK**. (Dit is het opslag-account dat u eerder in deze zelfstudie hebt gemaakt).
4. Voer de **Accountsleutel** voor het Azure-blobopslag-account en klik vervolgens op **Opslaan**.
5. Dubbelklik in het rechterdeelvenster op de naam van de blob. Standaard is de naam van de blob hetzelfde als de naam van het cluster.

6. Zoek **stdout** in de kolom **naam** . Controleer of de GUID in de bijbehorende **Mappad** kolom overeenkomt met de GUID die u eerder hebt gekopieerd. Worden er een overeenkomst voorgesteld dat de uitvoergegevens hoort bij de taak die u hebt verzonden. Klik op **binaire** in de kolom links van **stdout**.

    ![De gegevensuitvoer door GUID zoeken in de lijst met inhoud.][image-hdi-gettingstarted-powerquery-importdata2]

9. Klik op **sluiten en laden** in de linkerbovenhoek de component taak uitvoeren in Excel te importeren.

##<a name="run-samples"></a>Voorbeelden van uitvoeren

HDInsight cluster biedt een query-console die bevat een galerie aan de slag om uit te voeren voorbeelden rechtstreeks vanuit de portal. U kunt de voorbeelden gebruiken om te leren werken met HDInsight door enkele eenvoudige scenario's doorlopen. Deze voorbeelden wordt geleverd met alle vereiste elementen, zoals de gegevens te analyseren en de query's uitvoeren op de gegevens. Zie voor meer informatie over de voorbeelden in de galerie aan de slag, [Meer Hadoop in HDInsight via de galerie met HDInsight aan de slag](hdinsight-learn-hadoop-use-sample-gallery.md).

**Naar het uitvoeren van de steekproef**

1. Klik op de startboard Azure-Portal op de tegel voor het cluster die u zojuist hebt gemaakt.
 
2. Klik op het nieuwe cluster blad, op **Dashboard**. Wanneer u wordt gevraagd, voert u de beheerdersgebruikersnaam en wachtwoord voor het cluster.

    ![Cluster dashboard starten] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.Cluster.Dashboard.png "Cluster dashboard starten")
 
3. Klik op het tabblad **Aan de slag galerie** van de webpagina dat wordt geopend, en klik vervolgens onder de categorie **oplossingen met voorbeeldgegevens** op de steekproef die u wilt uitvoeren. Volg de instructies op de pagina met webonderdelen om te voltooien van de steekproef. De volgende tabel bevat een paar voorbeelden en vindt u meer informatie over welke elk monster bevat.

Voorbeeld | Wat doet dit?
------ | ---------------
[Sensor gegevensanalyse][hdinsight-sensor-data-sample] | Leer hoe u HDInsight gebruiken voor het verwerken van historische gegevens die is gemaakt met de verwarming, ventilatie en airconditioning (Aircoschema)-systemen systemen die niet kunnen naar betrouwbaar voor het behoud van een set temperatuur identificeren.
[Website logboekanalyse][hdinsight-weblogs-sample] | Informatie over het gebruiken van HDInsight te analyseren website logboekbestanden om inzicht te krijgen in de frequentie van bezoeken naar de website in een dag van externe websites en een overzicht van de website van fouten die de gebruikers ondervinden.
[Twitter trendanalyse](hdinsight-analyze-twitter-data.md) | Informatie over het gebruik van HDInsight analyseren van trends in Twitter.

##<a name="delete-the-cluster"></a>Het cluster verwijderen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="next-steps"></a>Volgende stappen
In deze zelfstudie Hadoop hebt u geleerd hoe een Hadoop-cluster maken in Windows in HDInsight, component query's uitvoeren op gegevens en importeren de resultaten in Excel, waar zij verdere kunnen worden verwerkt en grafisch met bedrijfsinformatiehulpprogramma's worden weergegeven. Meer informatie raadpleegt u de volgende zelfstudies:

- [Aan de slag met HDInsight Hadoop Tools voor Visual Studio][1]
- [Aan de slag met de HDInsight Emulator][hdinsight-emulator]
- [Azure-blobopslag gebruiken met HDInsight][hdinsight-storage]
- [HDInsight via PowerShell beheren][hdinsight-admin-powershell]
- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
- [MapReduce gebruiken met HDInsight][hdinsight-use-mapreduce]
- [Component gebruiken met HDInsight][hdinsight-use-hive]
- [Varken met HDInsight gebruiken][hdinsight-use-pig]
- [Oozie gebruiken met HDInsight][hdinsight-use-oozie]
- [Ontwikkel Java MapReduce-programma's voor HDInsight][hdinsight-develop-mapreduce]


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-versions]: hdinsight-component-versioning.md


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-emulator]: hdinsight-hadoop-emulator-get-started.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hadoop-hdinsight-intro]: hdinsight-hadoop-introduction.md
[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://go.microsoft.com/fwlink/?LinkId=510084
[apache-hive]: http://go.microsoft.com/fwlink/?LinkId=510085
[apache-mapreduce]: http://go.microsoft.com/fwlink/?LinkId=510086
[apache-hdfs]: http://go.microsoft.com/fwlink/?LinkId=510087
[hdinsight-hbase-custom-provision]: hdinsight-hbase-tutorial-get-started.md


[powershell-download]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell-install-configure]: powershell-install-configure.md
[powershell-open]: powershell-install-configure.md#step-1-install


[img-hdi-dashboard]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.png
[img-hdi-dashboard-query-select]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.browse.output.png

[img-hdi-getstarted-video]: ./media/hdinsight-hadoop-tutorial-get-started-windows/hdi-get-started-video.png


[image-hdi-storageaccount-quickcreate]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.StorageAccount.QuickCreate.png
[image-hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.ClusterStatus.png
[image-hdi-quickcreatecluster]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.QuickCreateCluster.png
[image-hdi-getstarted-flow]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GetStartedFlow.png

[image-hdi-gettingstarted-powerquery-importdata]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData.png
[image-hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData2.png
 
