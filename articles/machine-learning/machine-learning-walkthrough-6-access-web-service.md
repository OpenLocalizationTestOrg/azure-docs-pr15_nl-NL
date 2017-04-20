<properties
    pageTitle="Stap 6: Toegang tot de Machine Learning-webservice | Microsoft Azure"
    description="Stap 6 van de prognose blog oplossing Stapsgewijze instructies: toegang tot een actieve Azure Machine Learning-webservice."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Stapsgewijze instructies stap 6: Toegang tot de Azure Machine Learning-webservice

Dit is de laatste stap van de procedure, [een blog analytics-oplossing in Azure Machine Learning ontwikkelen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Een Machine Learning-werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3.  [Maak een nieuwe experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Trainen en de modellen voor evalueren](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [De webservice implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Toegang tot de webservice**

----------

In de vorige stap in dit scenario geïmplementeerd we een webservice die gebruikmaakt van onze krediet risico tekstvoorspelling model. Nu moeten gebruikers kunnen gegevens verzenden en ontvangen van resultaten. 

De webservice is een Azure-webservice die kunt ontvangen en terug te keren gegevens met behulp van de REST API's op twee manieren:  

-   **Aanvraag en respons** - door de gebruiker een of meer rijen met gegevens van de creditcard verzonden naar de service met behulp van een HTTP-protocol en de service reageert met een of meer sets met resultaten.
-   **Batch Execution** - door de gebruiker een of meer rijen met gegevens van de creditcard in een Azure blob worden opgeslagen en stuurt de blob locatie naar de service. De service wordt bepaald met alle rijen met gegevens in de invoer blob, worden de resultaten in een andere blob opgeslagen en geeft als resultaat de URL van dat onderdeel.  

De snelste en eenvoudigste manier voor toegang tot de webservice is via de Web-App sjablonen die beschikbaar zijn in de [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).
Deze web app-sjablonen kunnen maken op een aangepaste web-app weet uw webservice invoergegevens en wat wordt geretourneerd. U hoeft te is toegang bieden tot uw web-service en gegevens en doet de rest van de sjabloon.

Zie voor meer informatie over het gebruik van de web app-sjablonen, [verbruiken een Azure Machine Learning-webservice met een web app-sjabloon](machine-learning-consume-web-service-with-web-app-template.md).

U kunt ook een maatwerktoepassing op voor toegang tot de webservice starter-code die zijn opgegeven voor u in R, C# en Python programming talen met ontwikkelen.
U vindt de volledige details in [het gebruiken van een Azure Machine Learning-webservice die uit een Machine Learning-experiment is gepubliceerd](machine-learning-consume-web-services.md).
