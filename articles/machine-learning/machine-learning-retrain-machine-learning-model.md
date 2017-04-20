<properties
    pageTitle="Een Machine Learning-Model Retrain | Microsoft Azure"
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
    ms.date="10/10/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-machine-learning-model"></a>Een Machine Learning-Model Retrain

Als onderdeel van het proces van het uitoefening van machine learning-modellen in Azure Machine Learning, is uw model training en opgeslagen. U vervolgens deze gebruiken om te maken van een predicative webservice. De webservice kan vervolgens worden gebruikt in websites, dashboards en mobiele apps. 

Modellen die u samenstelt met Machine Learning zijn meestal niet statische. Zodra er nieuwe gegevens beschikbaar of wanneer de consument van de API hun eigen gegevens bevat moet het model worden retrained. 

Bijscholing kan vaak optreden. Met de functie programma bijscholing API, kunt u via programmacode retrain het model, waarbij de bijscholing-API's en de webservice bijwerken met het zojuist ervaren model. 

In dit document omschrijving van het proces opnieuw opleiden en ziet u hoe u de bijscholing-API's gebruiken.

## <a name="why-retrain-defining-the-problem"></a>Waarom retrain: het probleem definiëren  

Als onderdeel van de machine learning-trainingsproces, is een model ervaren met een reeks gegevens. Modellen die u samenstelt met Machine Learning zijn meestal niet statische. Zodra er nieuwe gegevens beschikbaar of wanneer de consument van de API hun eigen gegevens bevat moet het model worden retrained.

In deze situaties is een programma API een handige manier toe te staan dat u of de consument van uw API's te maken van een client die u kunt een eenmalige of gewone worden opgeteld, retrain het model met hun eigen gegevens. Ze kunnen vervolgens de resultaten van bijscholing evalueren en bijwerken van de Web-service API als u wilt gebruiken het zojuist ervaren model.

>[AZURE.NOTE] Als u een bestaande Training Experiment en nieuwe webservice hebt, wilt u mogelijk Retrain uitchecken met een bestaande blog webservice in plaats van de stapsgewijze instructies die worden genoemd in de volgende sectie volgen.

## <a name="end-to-end-workflow"></a>End-to-end-werkstroom 

Het proces omvat de volgende onderdelen: A Training Experiment en een blog Experiment gepubliceerd als een webservice. Als u wilt inschakelen bijscholing van een ervaren model, moet het Experiment Training worden gepubliceerd als een webservice met de uitvoer van een ervaren model. Hiermee worden API toegang tot het objectmodel voor herscholing. 

De volgende stappen gelden voor zowel nieuw en klassieke Web services:

De eerste blog webservice maken:

* Maken van een experiment training
* Een experiment blog web maken
* Een blog webservice implementeren

De webservice Retrain:

* Training experiment om te staan voor bijscholing bijwerken
* De opnieuw opleiden webservice implementeren
* Gebruik de Batch Execution Service-code aan het model retrain

Voor stapsgewijze instructies voor de voorgaande stappen, raadpleegt u [Retrain Machine Learning model via programmacode](machine-learning-retrain-models-programmatically.md).

Als u een klassieke webservice geïmplementeerd:

* Maak een nieuwe eindpunt op de blog webservice
* Lees het PATCH URL en de code
* Gebruik van de URL PATCH naar het nieuwe Endpoint wijst u het retrained model 

Zie [een klassieke webservice Retrain](machine-learning-retrain-a-classic-web-service.md)voor stapsgewijze instructies voor de voorgaande stappen.

Als u een klassieke webservice bijscholing problemen ondervindt, raadpleegt u [problemen met de bijscholing van een Azure Machine Learning klassieke webservice](machine-learning-troubleshooting-retraining-models.md).

Als u een nieuwe webservice geïmplementeerd:

* Meld u aan bij uw account resourcemanager van Azure
* Definitie van de webservice ophalen
* Definitie van de webservice als JSON exporteren
* Bijwerken van de verwijzing naar de `ilearner` blob in de JSON
* De JSON importeren in de definitie van een Web-Service
* De webservice bijwerken met nieuwe definitie van de Web-Service

Voor stapsgewijze instructies voor de voorgaande stappen, raadpleegt u [een nieuwe webservice met de Machine Learning Management PowerShell-cmdlets Retrain](machine-learning-retrain-new-web-service-using-powershell.md).

Het proces voor het instellen van omscholing voor een klassiek webservice omvat de volgende stappen:

![Overzicht van het proces bijscholing][1]

Het proces voor het instellen van omscholing voor een nieuwe webservice omvat de volgende stappen:

![Overzicht van het proces bijscholing][7]

## <a name="other-resources"></a>Overige informatiebronnen

- [Bijscholing en bijwerken van Azure Machine Learning-modellen met Azure gegevens Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
- [Veel Machine Learning-modellen en web service eindpunten maken van een experiment via PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md)
- De video [AML bijscholing modellen gebruikt API's](https://www.youtube.com/watch?v=wwjglA8xllg) ziet u hoe u retrain Machine Learning-modellen die zijn gemaakt in Azure Machine Learning gebruik van de bijscholing API's en PowerShell.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

