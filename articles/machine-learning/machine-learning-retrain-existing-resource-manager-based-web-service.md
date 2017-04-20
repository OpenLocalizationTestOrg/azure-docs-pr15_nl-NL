<properties
    pageTitle="Een bestaande blog webservice Retrain | Microsoft Azure"
    description="Leer hoe u een model retrain en bijwerken van de webservice als u wilt gebruiken het zojuist ervaren model in Azure Machine Learning."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Een bestaande blog webservice Retrain

In dit document omschrijving van het opnieuw opleiden proces voor het volgende scenario:

* U hebt een experiment training en een blog experiment die u hebt geïmplementeerd als een geoperationaliseerd webservice.
* Er nieuwe gegevens dat u wilt dat uw blog webservice gebruiken om te voeren de scoren.

Beginnen met uw bestaande webservice en experimenten, moet u als volgt te werk:

1. Werk het model.
    1. Wijzig uw experiment training om te staan voor web service invoer en uitvoer.
    2. Het experiment training te implementeren als een opnieuw opleiden webservice.
    3. Gebruik van het training experiment Batch Execution Service (BES) om het model retrain.
2. De Azure Machine Learning PowerShell-cmdlets gebruiken bij het blog experiment.
    1.  Meld u aan bij uw resourcemanager Azure-account.
    2.  Definitie van de webservice krijgen.
    3.  Definitie van de webservice als JSON exporteren.
    4.  De verwijzing naar de ilearner label in de JSON bijwerken.
    5.  De JSON importeren in de definitie van een web-service.
    6.  Werk de webservice met een nieuwe definitie van de web-service.

## <a name="deploy-the-training-experiment"></a>Het experiment training implementeren

Als u wilt het experiment training als opnieuw opleiden webservice implementeren, moet u web service invoer en uitvoer toevoegen aan het model. Door te verbinden met een module *Web Service uitvoer* van het experiment * [Trein Model] [ train-model] * module, dat u het experiment training om te produceren, een nieuwe ervaren model dat u in uw blog experiment gebruiken kunt inschakelen. Als u een module *Evalueren Model* hebt, kunt u ook web service uitvoer om de evaluatieresultaten als uitvoer koppelen.

Uw experiment training bijwerken:

1. Een *Web Service invoer* module verbinden met uw invoer van gegevens (bijvoorbeeld een module *Wissen.Control ontbrekende gegevens* ). Meestal wilt u ervoor zorgen dat uw invoergegevens worden verwerkt op dezelfde manier als de trainingsgegevens van uw oorspronkelijke.
2. Een module *Web Service uitvoer* verbinden met de uitvoer van de module *Trein Model* .
3. Als u een module *Evalueren Model* hebt en u wilt uitvoeren van de evaluatieresultaten, verbinden met een module *Web Service uitvoer* de uitvoer van de module *Model evalueren* .

Voer uw experiment.

Vervolgens moet u het experiment training implementeren als een webservice die een ervaren model en model evaluatieresultaten oplevert.  

De onderkant van het papier experiment, op **Webservice instellen**en selecteer vervolgens **Implementeren webservice [Nieuw]**. De portal Azure Machine Learning-webservices wordt geopend naar de pagina **Webservice implementeren** . Typ een naam voor uw webservice, kiest u een betalingsschema en klik vervolgens op **Deploy**. U kunt alleen de Batch Execution-methode gebruiken voor het maken van ervaren modellen.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Het model met nieuwe gegevens met behulp van BES Retrain

In dit voorbeeld gebruiken we C# maken van de toepassing opnieuw opleiden. U kunt ook Python of R steekproef-code voor het uitvoeren van deze taak.

De opnieuw opleiden API's bellen:

1. Maak een C#-console-toepassing in Visual Studio (**Nieuw** > **Project** > **Windows-bureaublad** > **Console-toepassing**).
2.  Meld u aan bij de portal Machine Learning-webservices.
3.  Klik op de webservice waarmee u werkt.
2.  Klik op **gebruiken**.
3.  Klik onder aan de pagina **verbruiken** in de sectie **Voorbeeldcode** op **Batch**.
5.  De steekproef C#-code voor Batchuitvoering Kopieer en plak deze in het bestand Program.cs. Zorg ervoor dat de naamruimte intact blijft.

Het pakket NuGet Microsoft.AspNet.WebApi.Client, zoals opgegeven in de opmerkingen toevoegen. De verwijzing naar Microsoft.WindowsAzure.Storage.dll wilt toevoegen, moet u mogelijk eerst de [clientbibliotheek voor Azure Storage services](https://www.nuget.org/packages/WindowsAzure.Storage)installeren.

De volgende schermafbeelding ziet u de pagina **verbruiken** in de portal Azure Machine Learning-webservices.

![Pagina gebruiken][1]

### <a name="update-the-apikey-declaration"></a>De declaratie apikey bijwerken

Zoek de declaratie **apikey** :

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Zoek de primaire sleutel in de sectie **eenvoudige verbruik info** van de pagina **verbruiken** en kopiëren naar de declaratie **apikey** .

### <a name="update-the-azure-storage-information"></a>De opslag van Azure-gegevens bijwerken

De code van de steekproef BES uploads van een bestand vanaf een lokaal station (bijvoorbeeld ' C:\temp\CensusIpnput.csv') met Azure Storage, verwerkt en de resultaten gegevens terug worden geschreven met Azure Storage.  

Als u de gegevens van Azure Storage bijwerken, moet u de opslagaccountnaam, sleutel en container informatie ophalen voor uw account opslag van de Azure klassieke portal en vervolgens update de correspondi na het uitvoeren van uw experiment, de resulterende werkstroom moet er ongeveer als volgt te werk:

![Resulterende werkstroom nadat uitvoeren][4]NG waarden in de code.

1. Meld u aan bij de portal van Azure klassieke.
1. Klik in de kolom linker navigatiegedeelte op **opslag**.
1. Selecteer in de lijst met accounts van opslag, een voor de opslag van het retrained model.
1. Klik onder aan de pagina, op **Toegangstoetsen beheren**.
1. Kopiëren en de **Primaire sleutel van Access** opslaan en sluit het dialoogvenster.
1. Aan de bovenkant van de pagina, klikt u op **Containers**.
1. Selecteer een bestaande container, of een nieuw account te maken en sla de naam.

Zoek de *StorageAccountName*, *StorageAccountKey*en *StorageContainerName* declaraties en de waarden die u hebt opgeslagen in de klassieke portal bijwerken.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Ook moet u ervoor zorgen dat de invoer bestand beschikbaar is op de locatie die u in de code opgeeft.

### <a name="specify-the-output-location"></a>De uitvoerlocatie van de opgeven

Wanneer u de uitvoerlocatie in de nettolading aanvragen opgeeft, de extensie van het bestand dat is opgegeven in *RelativeLocation* moet worden opgegeven als `ilearner`. Zie het volgende voorbeeld:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

De volgende is een voorbeeld van opnieuw opleiden uitvoer: ![bijscholing uitvoer][6]

## <a name="evaluate-the-retraining-results"></a>De opnieuw opleiden resultaten evalueren

Wanneer u de toepassing uitvoert, wordt de uitvoer bevat de URL en gedeelde handtekeningen toegangstoken die nodig zijn voor toegang tot de evaluatieresultaten.

U kunt de prestatieresultaten van het retrained model weergeven door het combineren van de *BaseLocation*, *RelativeLocation*en *SasBlobToken* uit het resultaat voor *output2* (zoals weergegeven in de voorgaande opnieuw opleiden uitvoer afbeelding) en de volledige URL plakken in de adresbalk van de browser.  

Bekijk de resultaten om te bepalen of het zojuist ervaren model goed genoeg voor het vervangen van het bestaande bestand uitvoert.

Kopieer de *BaseLocation*, *RelativeLocation*en *SasBlobToken* uit het resultaat.

## <a name="retrain-the-web-service"></a>De webservice Retrain

Wanneer u een nieuwe webservice retrain, kunt u de definitie van de blog webservice om te verwijzen naar het nieuwe ervaren model bijwerken. De definitie van de web-service is een interne weergave van het ervaren model van de webservice en is niet direct kan worden gewijzigd. Zorg ervoor dat u definitie van de webservice voor uw blog experiment en niet uw training experiment ophaalt.

## <a name="sign-in-to-azure-resource-manager"></a>Meld u aan naar Azure resourcemanager

U moet eerst aanmelden bij uw Azure-account vanuit de PowerShell-omgeving met behulp van de cmdlet [Toevoegen-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition-object"></a>Het object Web servicedefinitie ophalen

Vervolgens krijgen de definitie van de Web-Service-object door de ondersteuning voor de cmdlet [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) .

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Om te bepalen de naam van de resource van een bestaande webservice, voert u de cmdlet Get-AzureRmMlWebService zonder parameters de webservices in uw abonnement wordt weergegeven. Zoek de webservice en kijkt u naar de web-service-ID. De naam van de resourcegroep is het vierde element in de ID direct na het element *resourceGroups* . In het volgende voorbeeld is de naam van de resource-groep standaard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

U kunt ook om te bepalen de naam van de resource van een bestaande webservice, meld u aan bij de portal Azure Machine Learning-webservices. Selecteer de webservice. De naam van de resource-groep is het vijfde element van de URL van de webservice, direct na het element *resourceGroups* . In het volgende voorbeeld is de naam van de resource-groep standaard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>De definitie van de Web-Service-object als JSON exporteren

Als u wilt wijzigen van de definitie van het ervaren model met het zojuist ervaren objectmodel, moet u eerst de cmdlet [Exporteren-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) exporteren naar een indeling van JSON-bestand gebruiken.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>De verwijzing naar de blob ilearner bijwerken

Klik in de activa, zoek het [ervaren model], de *uri* -waarde in het knooppunt *locationInfo* bijwerken met de URI van de blob ilearner. De URI wordt gegenereerd door het *BaseLocation* en de *RelativeLocation* uit de resultaten van het gesprek bijscholing BES te combineren.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>De JSON importeren in een object definitie van de Web-Service

De cmdlet [Importeren-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) moet u de gewijzigde JSON-bestand terug omzetten in een object definitie van de Web-Service die u gebruiken kunt bij het predicative experiment.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>De webservice bijwerken

Ten slotte, gebruikt u de cmdlet [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) het blog experiment bijwerken.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
