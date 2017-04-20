<properties     
    pageTitle="Azure gegevens Factory - voorbeelden" 
    description="Meer informatie over voorbeelden die worden geleverd biedt met de Azure gegevens Factory-service." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---samples"></a>Azure gegevens Factory - voorbeelden

## <a name="samples-on-github"></a>Voorbeelden van GitHub
De [GitHub Azure-DataFactory bibliotheek](https://github.com/azure/azure-datafactory) bevat verschillende voorbeelden die u helpen snel platform omhoog met Azure gegevens Factory-service (of) wijzigen van de scripts en in eigen toepassing te gebruiken. De map Samples\JSON bevat JSON fragmenten voor veelvoorkomende scenario's.

| Voorbeeld | Beschrijving |
| :----- | :---------- | 
| [ADF Stapsgewijze instructies](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) | In dit voorbeeld biedt een end-to-end Stapsgewijze instructies voor het verwerken van logboekbestanden Azure gegevens Factory gebruiken om te schakelen van gegevens uit logbestanden in inzicht krijgen in. <br/><br/>In dit scenario de pijplijn Data Factory verzamelt steekproef Logboeken, processen en de gegevens uit logboeken bij referentiegegevens cursussen, en transformeert dan de gegevens als u wilt evalueren van de effectiviteit van een marketingcampagne die onlangs is gestart. |
| [Voorbeelden van JSON](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) | In dit voorbeeld bevat voorbeelden van JSON voor veelvoorkomende scenario's. | 
| [HTTP-gegevens Downloader steekproef](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) | In dit voorbeeld showcases downloaden van gegevens uit een HTTP-eindpunt Azure-blobopslag met aangepaste activiteit van .NET. |
| [Cross AppDomain stip netto activiteit steekproef](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | In dit voorbeeld kunt u de auteur van een aangepaste .NET-activiteit die niet is beperkt tot constructie-versies die worden gebruikt door het startpictogram van het ADF (bijvoorbeeld WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, enzovoort). |
| [R-script uitvoeren](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |  In dit voorbeeld bevat de aangepaste gegevens Factory-activiteit die kan worden gebruikt om aan te roepen RScript.exe. In dit voorbeeld werkt alleen met uw eigen (niet op aanvraag) HDInsight cluster die beschikt over R geïnstalleerd op is geïnstalleerd. |
| [Elektrische taken op HDInsight Hadoop cluster roepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) | In dit voorbeeld ziet u hoe u een programma een aanroepen met MapReduce activiteit. Het programma elektrische kopieert alleen gegevens uit één Azure Blob container naar een andere. |
| [Twitter-analyse met behulp van Azure Machine Learning Batch scoren activiteit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) | In dit voorbeeld ziet u het gebruik van AzureMLBatchScoringActivity om aan te roepen een Azure Machine Learning-model waarmee twitter sentiment analyse, scoren, tekstvoorspelling enzovoort. |
| [Met aangepaste activiteit Twitter-analyse](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |  In dit voorbeeld ziet u hoe u met een aangepaste .NET-activiteit roepen een Azure Machine Learning-model waarmee twitter sentiment analyse, scoren, tekstvoorspelling enzovoort. |
| [Met parameters pijpleidingen voor Azure Machine Learning](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) | Het voorbeeld bevat een end-to-end C#-code als u wilt implementeren N pijpleidingen voor scoren en bijscholing elk voorzien van een ander gebied parameter waar de lijst met regio's die is afkomstig zijn uit een bestand parameters.txt, die deel uitmaakt van dit voorbeeld. | 
| [Naslag voor gegevens vernieuwen voor Azure Stream Analytics-taken](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |  In dit voorbeeld ziet hoe u Azure gegevens fabriek en Azure Stream Analytics samen aan de query's uitvoeren met referentiegegevens en configuratie van het vernieuwen voor referentiegegevens volgens een schema gebruiken. |
| [Hybride verkooppijplijn met On-premises Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) | De steekproef gebruikt een on-premises implementatie Hadoop cluster als een berekeningscluster doel voor het uitvoeren van taken in gegevens fabriek, net zoals u andere doelen berekeningscluster zoals een HDInsight op basis van Hadoop cluster in de cloud. |
| [JSON-conversieprogramma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) | Dit hulpmiddel kunt u JSONs converteren van versie vóór 2015-07-01-Preview-versie naar de meest recente of 2015-07-01-preview (standaard). |  
| [I-SQL-voorbeeldbestand invoer](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |  Dit bestand is een voorbeeldbestand die worden gebruikt door een activiteit I-SQL. | 

## <a name="azure-resource-manager-templates"></a>Azure resourcemanager-sjablonen
U kunt de volgende resourcemanager Azure-sjablonen vinden voor gegevens Factory op Github. 

| Sjabloon | Beschrijving |
| -------- | ----------- | 
| [Kopieer uit Azure-blobopslag met Azure SQL-Database](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) | Een factory Azure gegevens maakt implementeren van deze sjabloon met een pijplijn die gegevens opgehaald uit de opgegeven Azure-blobopslag met de SQL Azure-database |    
| [Kopieer uit Salesforce met Azure-blobopslag](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) | Implementeren van deze sjabloon wordt gemaakt van een fabriek Azure-gegevens met een pijplijn die gegevens opgehaald uit de opgegeven Salesforce-account aan de Azure-blobopslag. |    
| [Transformeer gegevens door te component script uitvoeren op een cluster Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) | Implementeren van deze sjabloon wordt gemaakt van een fabriek Azure-gegevens met een pijplijn waarmee gegevens worden omgezet door het component voorbeeldscript uit te voeren op een cluster Azure HDInsight Hadoop. | 


## <a name="samples-in-azure-portal"></a>Voorbeelden in Azure-portal
U kunt de tegel **voorbeeld pijpleidingen** op de startpagina van uw gegevens factory steekproef pijpleidingen en hun bijbehorende entiteiten (gegevenssets en gekoppelde services) in implementeren naar uw gegevens factory. 

1. Maken van een fabriek gegevens of open een bestaande gegevens factory. Zie [aan de slag met Azure gegevens Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#CreateDataFactory) voor stapsgewijze instructies voor het maken van een fabriek gegevens.
2. Klik in het blad **Gegevens FACTORY** voor de fabriek gegevens, op de tegel **voorbeeld pijpleidingen** .

    ![Voorbeeld pijpleidingen tegel](./media/data-factory-samples/SamplePipelinesTile.png)

2. Klik in het blad **steekproef pijpleidingen** op de **steekproef** die u wilt implementeren. 
    
    ![Voorbeeld pijpleidingen blade](./media/data-factory-samples/SampleTile.png)

3. Configuratie-instellingen voor de steekproef opgeven. Bijvoorbeeld uw Azure opslag naam en account accountsleutel, Azure SQL-servernaam, database, gebruikersnaam en wachtwoord, enzovoort. 

    ![Voorbeeld blade](./media/data-factory-samples/SampleBlade.png)

4. Nadat u klaar bent met de configuratieinstellingen opgeven, klikt u op **maken** om te maken/de steekproef pijpleidingen en implementeren gekoppelde services/tabellen die worden gebruikt door de pijpleidingen.
5. U ziet de status van de implementatie op de tegel van de steekproef die u eerder hebt geklikt op het blad **steekproef pijpleidingen** .

    ![Implementatiestatus](./media/data-factory-samples/DeploymentStatus.png)

6. Als u het bericht **implementatie is voltooid** op de tegel voor de steekproef ziet, sluit u het blad **steekproef pijpleidingen** .  
5. Op **Gegevens FACTORY** blade ziet u dat gekoppelde services, gegevenssets en pijpleidingen worden toegevoegd aan uw factory gegevens.  

    ![Gegevens Factory blade](./media/data-factory-samples/DataFactoryBladeAfter.png)
   
## <a name="samples-in-visual-studio"></a>Voorbeelden in Visual Studio

### <a name="prerequisites"></a>Vereisten voor

U moet de volgende manieren te werk op uw computer is geïnstalleerd: 

- Visual Studio 2013 of Visual Studio-2015
- Download Azure SDK voor Visual Studio 2013 of Visual Studio-2015. Navigeer naar de [Pagina voor het downloaden van Azure](https://azure.microsoft.com/downloads/) en op **tegenover 2013** of **tegenover 2015** in de sectie **.NET** .
- Download de meest recente Azure gegevens Factory-invoegtoepassing voor Visual Studio: [tegenover 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) of [tegenover 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Als u Visual Studio 2013 gebruikt, kunt u de invoegtoepassing ook bijwerken door de volgende stappen uit te voeren: klik op **Extra**in het menu -> **Extensions en Updates** -> **Online** -> **Visual Studio-galerie** -> **Microsoft Azure gegevens Factory Tools voor Visual Studio** -> **bijwerken**.

### <a name="use-data-factory-templates"></a>Gegevens Factory sjablonen gebruiken

1. Klik op **bestand** in het menu, wijs **Nieuw**aan en klik op **Project**. 
2. Voer de volgende stappen uit in het dialoogvenster **Nieuw Project** : 
    1. Selecteer **DataFactory** onder **sjablonen**. 
    2. Selecteer **Gegevens Factory sjablonen** in het rechterdeelvenster. 
    3. Voer een **naam** voor het project. 
    4. Selecteer een **locatie** voor het project. 
    5. Klik op **OK**. 

    ![Dialoogvenster Nieuw project](./media/data-factory-samples/vs-new-project-adf-templates.png)
6. Selecteer de voorbeeldsjabloon in de sectie **Use-Case sjablonen** in het dialoogvenster **Gegevens Factory sjablonen** en klik op **volgende**. De volgende stappen begeleiden u tot en met behulp van de sjabloon **Klant profiel** . Stappen zijn identiek voor de andere voorbeelden. 

    ![Dialoogvenster voor gegevens Factory-sjablonen](./media/data-factory-samples/vs-data-factory-templates-dialog.png) 
7. Klik op **volgende** op de pagina **Gegevens Factory basisbeginselen** in het dialoogvenster **Gegevens Factory configuratie** .
8. Klik op de pagina **configureren gegevens factory** volgt u de volgende stappen uit: 
    1. Selecteer **nieuwe gegevens Factory maken**. U kunt ook **Gebruik bestaande gegevens factory**selecteren.
    2. Voer een **naam** voor de fabriek gegevens.
    3. Selecteer het **abonnement Azure** waarin u wilt dat de gegevens fabriek moet worden gemaakt. 
    4. Selecteer de **resourcegroep** voor de fabriek gegevens.
    5. Selecteer de **West Amerikaans**, **Oost Verenigde Staten**, of **Noord Europe** voor de **regio**.
    6. Klik op **volgende**. 
9. Geef een bestaande **Azure SQL-database** en **Azure opslag-account** **configureren gegevens worden opgeslagen** op de pagina (of) maken van database/opslag en op volgende. 
10. Selecteer standaardwaarden **configureren berekenen** op de pagina en klik op **volgende**. 
11. In de pagina **Summary** alle instellingen controleren en klik op **volgende**. 
12. Wacht totdat de implementatie is voltooid en klik op **Voltooien**op de pagina **Status van de implementatie** .
13. Met de rechtermuisknop op project in de Solution Explorer en klik op **publiceren**. 
19. Als u het dialoogvenster **aanmelden bij uw Microsoft-account** ziet, voert u uw referenties voor het account dat Azure abonnement heeft en klik op **aanmelden**.
20. Hier ziet u het volgende dialoogvenster:

    ![Het dialoogvenster publiceren](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Voer de volgende stappen uit in de pagina **data factory genoemd configureren** : 
    1. Bevestig dat **Gebruik bestaande gegevens factory** -optie.
    2. Selecteer de **gegevens factory** u had Selecteer bij gebruik van de sjabloon. 
    6. Klik op **volgende** om te schakelen naar de pagina **Publiceren Items** . (Druk op **TAB** om terug te verplaatsen uit het veld naam als de knop **volgende** is uitgeschakeld). 
23. De pagina **Publiceren Items** Zorg ervoor dat alle gegevens-bedrijven entiteiten zijn geselecteerd en klik op **volgende** om naar de pagina **Summary** te schakelen.     
24. Bekijk het overzicht en klik op **volgende** om de implementatie starten en de **Implementatie-Status**weergeven.
25. **Implementatiestatus** op de pagina ziet u de status van het implementatieproces. Klik op Voltooien wanneer de implementatie klaar is. 

Zie [uw eerste gegevens factory (Visual Studio) maken](data-factory-build-your-first-pipeline-using-vs.md) voor meer informatie over het gebruik van Visual Studio naar gegevens Factory entiteiten van de auteur en publiceren naar Azure.          