<properties
    pageTitle="Een nieuwe webservice met de Machine Learning Management PowerShell-cmdlets Retrain | Microsoft Azure"
    description="Leer hoe u via een programma een model retrain en bijwerken van de webservice als u wilt het zojuist ervaren model in Azure Machine Learning met de Machine Learning Management PowerShell-cmdlets gebruiken."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Een nieuwe webservice met de Machine Learning Management PowerShell-cmdlets Retrain

Wanneer u een nieuwe webservice retrain, kunt u de definitie van de blog webservice om te verwijzen naar het nieuwe ervaren model bijwerken.  

## <a name="prerequisites"></a>Vereisten voor

U moet hebt ingesteld een experiment training en een blog experiment zoals wordt weergegeven in het [Retrain Machine Learning model via programmacode](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] Het blog experiment moet worden geïmplementeerd als een (nieuw) op basis van Azure Resource Manager-machine learning-webservice. 
 
Zie voor meer informatie over de implementatie-webservices, [Deploy een Azure Machine Learning-webservice](machine-learning-publish-a-machine-learning-web-service.md).

Dit proces is vereist dat u de Cmdlets van Azure Machine Learning hebt geïnstalleerd. Zie de [Cmdlets van Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt767952.aspx) -verwijzing op MSDN voor informatie over het installeren van de Machine Learning-cmdlets.

De volgende gegevens gekopieerd van de opnieuw opleiden uitvoer:

* BaseLocation
* RelativeLocation

De stappen die u neemt zijn:

1.  Meld u aan bij uw resourcemanager Azure-account.
2.  Definitie van de webservice ophalen
3.  Definitie van de webservice als JSON exporteren
4.  De verwijzing naar de ilearner label in de JSON bijwerken.
5.  De JSON importeren in de definitie van een Web-Service
6.  De webservice bijwerken met nieuwe definitie van de Web-Service

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Meld u aan bij uw account resourcemanager van Azure

U moet eerst aanmelden bij uw Azure-account vanuit de PowerShell-omgeving met de cmdlet [Toevoegen-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition"></a>Definitie van de webservice ophalen

Vervolgens de webservice krijgen door de ondersteuning voor de cmdlet [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) . De definitie van de Web-Service is een interne weergave van het ervaren model van de webservice en is niet direct kan worden gewijzigd. Zorg ervoor dat u definitie van de webservice voor uw blog experiment en niet uw training experiment ophaalt.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Om te bepalen de naam van de resource van een bestaande webservice, voert u de cmdlet Get-AzureRmMlWebService zonder parameters de webservices in uw abonnement wordt weergegeven. Zoek de webservice en kijkt u naar de web-service-ID. De naam van de resourcegroep is het vierde element in de ID direct na het element *resourceGroups* . In het volgende voorbeeld is de naam van de resource-groep standaard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

U kunt ook om te bepalen de naam van de resource van een bestaande webservice, meld u aan bij de portal van Microsoft Azure Machine Learning-webservices. Selecteer de webservice. De naam van de resource-groep is het vijfde element van de URL van de webservice, direct na het element *resourceGroups* . In het volgende voorbeeld is de naam van de resource-groep standaard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Definitie van de webservice als JSON exporteren

De definitie aan het model ervaren gebruik van de zojuist ervaren Model, moet u de cmdlet [Exporteren-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) exporteren naar een JSON-bestandsindeling.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>De verwijzing naar de ilearner label in de JSON bijwerken.

Klik in de activa, zoek het [ervaren model], de *uri* -waarde in het knooppunt *locationInfo* bijwerken met de URI van de blob ilearner. De URI wordt gegenereerd door het *BaseLocation* en de *RelativeLocation* uit de resultaten van het gesprek bijscholing BES te combineren.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
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

## <a name="import-the-json-into-a-web-service-definition"></a>De JSON importeren in de definitie van een Web-Service

Naar het gewijzigde JSON-bestand converteren naar de definitie van een Web-Service die u gebruiken kunt bij het Experiment Predicative, moet u de cmdlet [Importeren-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) gebruiken.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>De webservice bijwerken met nieuwe definitie van de Web-Service

Tot slot kunt u [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet het blog experiment bijwerken.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Overzicht

De Machine Learning PowerShell-cmdlets voor management gebruikt, kunt u het ervaren model van een blog webservice inschakelen van scenario's zoals bijwerken:

* Periodiek model bijscholing met nieuwe gegevens.
* Verdeling van een model voor klanten met het doel van deze retrain het model met hun eigen gegevens toe.
