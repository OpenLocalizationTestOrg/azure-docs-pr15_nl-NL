<properties
    pageTitle="Azure Machine Learning-webservices: De implementatie en verbruik | Microsoft Azure"
    description="Bronnen voor de implementatie en webservices verbruikt."
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
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure Machine Learning-webservices: De implementatie en verbruik

U kunt Azure Machine Learning gebruiken om te implementeren machine learning werkstromen en modellen als webservices. Deze webservices kunnen vervolgens worden gebruikt om te bellen van de machine learning-modellen vanuit toepassingen via Internet moet voorspellingen in realtime of in de batchmodus. Omdat de webservices RESTful, kunt u ze bellen van verschillende programming talen en platforms, zoals .NET en Java, en van toepassingen, zoals Excel.

De volgende gedeelten wordt koppelingen naar walkthroughs, code en documentatie waarmee u aan de slag.

## <a name="deploy-a-web-service"></a>Een webservice implementeren

### <a name="with-azure-machine-learning-studio"></a>Met Azure Machine Learning-Studio

Machine Learning Studio en de portal van Microsoft Azure Machine Learning-webservices kunt u implementeren en beheren van een webservice-code te schrijven.

De volgende koppelingen bieden algemene informatie over het implementeren van een nieuwe webservice:

* Zie voor een overzicht over het implementeren van een nieuwe webservice die gebaseerd op Azure resourcemanager, [Deploy een nieuwe webservice](machine-learning-webservice-deploy-a-web-service.md).
* Zie voor stapsgewijze instructies over het implementeren van een webservice, [Deploy een Azure Machine Learning-webservice](machine-learning-publish-a-machine-learning-web-service.md).
* Zie voor volledige stapsgewijze instructies over het maken en implementeren van een webservice, [scenario stap 1: een Machine Learning-werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md).
* Voor specifieke voorbeelden die een webservice implementeren, raadpleegt u:

    * [Stapsgewijze instructies stap 5: De Azure Machine Learning-webservice implementeren](machine-learning-walkthrough-5-publish-web-service.md)
    * [Het implementeren van een webservice aan meerdere landen](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Met web services resource provider API's (Azure resourcemanager-API's)

De provider voor de resource van Azure Machine Learning voor webservices kunt distribueren en beheren van webservices met behulp van de REST API-oproepen. Zie de verwijzing [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) op MSDN voor meer informatie.

### <a name="with-powershell-cmdlets"></a>Met de PowerShell-cmdlets

Azure Machine Learning resource-provider voor webservices kunt distribueren en beheren van webservices met behulp van de PowerShell-cmdlets.

Als u wilt de cmdlets gebruiken, moet u eerst aanmelden bij uw Azure-account vanuit de PowerShell-omgeving met behulp van de cmdlet [Toevoegen-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) . Als u niet bekend met het bent bellen van de PowerShell-opdrachten die zijn gebaseerd op resourcemanager, raadpleegt u [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Als u wilt exporteren uw blog experiment, gebruikt u [deze code steekproef](https://github.com/ritwik20/AzureML-WebServices). Nadat u de .exe-bestand van de code hebt gemaakt, kunt u typen:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

De toepassing wordt uitgevoerd, maakt een web service JSON-sjabloon. Als u de sjabloon wilt implementeren van een webservice, moet u de volgende informatie toevoegen:

* Opslagaccountnaam en van toetsen

    U kunt de opslagaccountnaam en van toetsen krijgen van de [Azure-portal](https://portal.azure.com/) of de [Azure klassieke portal](http://manage.windowsazure.com/).
* Resourcebeperkingen abonnement-ID

    U kunt de abonnement-ID krijgen van de portal [Azure Machine Learning-webservices](https://services.azureml.net) door aanmelden bij en te klikken op de abonnementsnaam van een.

Deze toevoegen aan de JSON-sjabloon als onderliggende elementen van het knooppunt *Eigenschappen* op hetzelfde niveau als het knooppunt *MachineLearningWorkspace* .

Hier volgt een voorbeeld:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Zie de volgende artikelen en voorbeelden van code voor meer informatie:

* [Azure Machine Learning-Cmdlets]( https://msdn.microsoft.com/library/azure/mt767952.aspx) verwijzing op MSDN
* Voorbeeld [scenario](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) op GitHub

## <a name="consume-the-web-services"></a>De webservices

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Van de Azure Machine Learning-webservices UI (testen)

U kunt uw webservice in de portal Azure Machine Learning-webservices testen. Dit geldt ook voor het testen van de aanvraag en respons-service (records) en Batch Execution service (BES)-interfaces.

* [Een nieuwe webservice implementeren](machine-learning-webservice-deploy-a-web-service.md)
* [Een webservice Azure Machine Learning implementeren](machine-learning-publish-a-machine-learning-web-service.md)
* [Stapsgewijze instructies stap 5: De Azure Machine Learning-webservice implementeren](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Uit Excel

U kunt een Excel-sjabloon waarbij de webservice downloaden:

* [Gebruik een Azure Machine Learning-webservice uit Excel](machine-learning-consuming-from-excel.md)
* [Excel-invoegtoepassing voor Azure Machine Learning-webservices](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>Vanaf een REST-client

Azure Machine Learning-webservices zijn RESTful API's. U kunt deze API's uit verschillende platforms, zoals .NET, Python, R, Java, enzovoort, gebruiken. De pagina **verbruiken** voor uw webservice op de [portal van Microsoft Azure Machine Learning-webservices](https://services.azureml.net) heeft steekproef-code waarmee u aan de slag kunt. Lees [hoe u het gebruiken van een Azure Machine Learning-webservice die is geïmplementeerd vanuit een Machine Learning-experiment](machine-learning-consume-web-services.md)voor meer informatie.

