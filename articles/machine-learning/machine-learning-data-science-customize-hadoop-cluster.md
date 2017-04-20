<properties 
    pageTitle="Hadoop clusters aanpassen voor het Team gegevens wetenschap proces | Microsoft Azure" 
    description="Populaire Python modules dat beschikbaar is in aangepaste Azure HDInsight Hadoop clusters."
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="hangzh;bradsev" />

# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Azure HDInsight Hadoop clusters voor het Team gegevens wetenschap proces aanpassen 

In dit artikel wordt beschreven hoe een cluster HDInsight Hadoop aanpassen door het installeren van de 64-bits Anaconda (Python 2.7) op elk knooppunt wanneer het cluster als een HDInsight-service is ingericht. Ook ziet u hoe u toegang tot de headnode om in te dienen aangepaste taken aan het cluster. Deze aanpassingen maakt veel populaire Python modules die van Anaconda gemakkelijk beschikbaar maken voor gebruik in door de gebruiker gedefinieerde functies (UDF) die zijn bedoeld uitmaken deel voor de verwerking van component records in het cluster. Zie voor instructies over de procedures in dit scenario gebruikt, [hoe om in te dienen component query's](machine-learning-data-science-move-hive-tables.md#submit).

Het menu onder koppelingen naar onderwerpen over het instellen van de verschillende gegevens wetenschappelijke omgevingen die worden gebruikt door het [Team gegevens wetenschap proces (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]


## <a name="customize"></a>Azure HDInsight Hadoop Cluster aanpassen

Een aangepaste HDInsight Hadoop-cluster maken, gebruikers moeten aanmelden bij [**Klassieke Portal van Azure**](https://manage.windowsazure.com/), klikt u op **Nieuw** in de hoek linksonder en vervolgens Selecteer DATA SERVICES -> HDINSIGHT -> **Aangepaste maken** om het venster **Cluster Details** weer te geven. 

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

De naam van het cluster moet worden gemaakt op pagina 1 van de configuratie van de invoer en standaardwaarden voor de andere velden accepteren. Klik op de pijl om te gaan naar de volgende configuratiepagina. 

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Op de configuratiepagina 2, invoer van het aantal **Knooppunten van gegevens**, selecteert u het **Land/virtuele netwerk**en selecteert u de grootte van het **Knooppunt van hoofd** - en het **Knooppunt van gegevens**. Klik op de pijl om te gaan naar de volgende configuratiepagina.

>[AZURE.NOTE] Het **Land/virtuele netwerk** mag niet hetzelfde zijn als het gebied van het account opslag die u wilt worden gebruikt voor het cluster HDInsight Hadoop. Anders in vierde pagina van de configuratie weergegeven het opslag-account dat de gebruikers wilt gebruiken niet op de vervolgkeuzelijst van de **Naam van het ACCOUNT**.

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Geef een gebruikersnaam en wachtwoord voor het cluster HDInsight Hadoop op configuratiepagina 3. **Niet doen** , selecteert u de _Enter de component/Oozie Metastore_. Klik vervolgens op de pijl om te gaan naar de volgende configuratiepagina. 

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Geef de naam van de account opslag, de standaard-container van het cluster HDInsight Hadoop op pagina 4. Als gebruikers _standaardcontainer maken_ in de vervolgkeuzelijst **STANDAARDCONTAINER selecteert** , kunt u een container met dezelfde naam als het cluster wordt gemaakt. Klik op de pijl om terug te gaan naar de laatste configuratiepagina.

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Op de laatste pagina in de configuratie **Scriptacties** , klikt u op knop **scriptactie toevoegen** en de tekstvelden vullen met de volgende waarden.
 
* **Naam** - een willekeurige tekenreeks als de naam van deze scriptactie in. 
* **TYPE knooppunt** - Selecteer **alle knooppunten**. 
* **SCRIPT URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *publicscripts* is een openbare container in opslag-account 
    * *getgoing* die we gebruiken om bestanden van de PowerShell-script om te vergemakkelijken gebruikers kunnen worden geopend in Azure te delen. 
* **PARAMETERS** - (verlaten leeg)

Ten slotte, klik op het selectievakje het maken van het aangepaste HDInsight Hadoop-cluster starten. 

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Toegang tot het knooppunt hoofd van Hadoop Cluster

Gebruikers moeten externe toegang tot het Hadoop-cluster in Azure inschakelen voordat ze toegang heeft tot het hoofd knooppunt van de Hadoop-cluster via RDP. 

1. Meld u aan bij de [**Klassieke Portal van Azure**](https://manage.windowsazure.com/) **HDInsight** Selecteer aan de linkerkant, selecteer uw cluster Hadoop in de lijst met clusters, klikt u op het tabblad **configuratie** en klik vervolgens op het pictogram **Externe inschakelen** onder aan de pagina.
    
    ![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)

2. Klik in het venster **Extern bureaublad configureren** de velden gebruikersnaam en wachtwoord invoeren en selecteer de vervaldatum voor externe toegang. Klik vervolgens op het vinkje voor de externe toegang tot het hoofd knooppunt van de Hadoop-cluster.

    ![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)
    
>[AZURE.NOTE] De gebruikersnaam en wachtwoord voor de externe toegang zijn niet de gebruikersnaam en wachtwoord dat u gebruikt wanneer u het cluster Hadoop gemaakt. Hierna ziet u een afzonderlijke set referenties. De vervaldatum van de externe toegang heeft ook, moeten binnen zeven dagen vanaf de huidige datum.

Nadat externe toegang is ingeschakeld, klikt u op **verbinding maken met** onderaan op de pagina op afstand in het hoofd knooppunt. U zich aanmeldt bij het hoofd knooppunt van de cluster Hadoop invoeren van de referenties voor de RAS-gebruiker die u eerder hebt opgegeven.

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

De volgende stappen in het proces van geavanceerde analyses zijn toegewezen in de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) en eventueel stappen die gegevens verplaatsen naar HDInsight, verwerken en probeer het voorbereiding op leren van de gegevens met Azure Machine Learning.

Lees [hoe u het indienen van component query's](machine-learning-data-science-move-hive-tables.md#submit) voor instructies voor het openen van de Python modules die van Anaconda uit het hoofd knooppunt van de cluster in de gebruiker gedefinieerde functies (UDF) die worden gebruikt uitmaken deel voor het verwerken van component records die zijn opgeslagen in het cluster.

 
