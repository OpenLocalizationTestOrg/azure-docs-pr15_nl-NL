<properties 
    pageTitle="Wat is Azure Machine Learning Studio? | Microsoft Azure"
    description="Overzicht van Azure ML Studio, een slepen en neerzetten hulpmiddel voor het samenstellen van snel modellen van een bibliotheek kant-en-klare van algoritmen en modules."
    keywords="Azure machine learning-, azure ml, ml studio"
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
    ms.topic="get-started-article"
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="what-is-azure-machine-learning-studio"></a>Wat is Azure Machine Learning Studio?

Microsoft Azure Machine Learning Studio is een gezamenlijke, slepen en neerzetten hulpprogramma die u gebruiken kunt om te bouwen, testen en bekijk analyses oplossingen van uw gegevens implementeren. Machine Learning Studio worden modellen gepubliceerd als webservices die eenvoudig kunnen worden gebruikt door aangepaste apps of BI-functies zoals Excel.

Machine Learning Studio is waar gegevens wetenschappelijke, bekijk analyses cloud resources en uw gegevens elkaar tegenkomen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>De interactieve Machine Learning Studio-werkruimte

Voor het ontwikkelen van een model blog analyse, u meestal gebruik van gegevens uit een of meer bronnen, transformeren verschillende gegevens bewerken en statistische functies met deze gegevens analyseren en een reeks resultaten genereren. Ontwikkeling van een model als volgt is een iteratieve proces. Zoals u kunt de verschillende functies en hun parameters wijzigen door uw resultaten samenkomen totdat u tevreden bent dat er een ervaren, effectieve model.

**Azure Machine Learning Studio** biedt een interactieve, visuele werkruimte zodat u kunt eenvoudig te bouwen, testen en doorgaan met het ontwikkelen van een blog analyse-model. U slepen en neerzetten ***gegevenssets*** en analyse ***modules*** op een interactieve tekenpapier, om een ***experimenteren***, die u in de Machine Learning Studio uitvoert formulier ze verbinding te maken. Als u wilt doorgaan met het ontwikkelen van het modelontwerp van uw, het experiment bewerken, een kopie desgewenst opslaan en opnieuw uitvoeren. Wanneer u klaar bent, kunt u uw ***training experimenteren*** converteren naar een ***blog experiment***en vervolgens publiceren als een ***webservice*** zodat uw model kan worden geopend door anderen.

>[AZURE.TIP] Als u wilt downloaden en afdrukken van een diagram waarmee u een overzicht van de mogelijkheden van Machine Learning Studio, raadpleegt u [overzichtsdiagram van Azure Machine Learning Studio mogelijkheden](machine-learning-studio-overview-diagram.md).

Er is geen hoeft te programmeren vereist, net visueel verbinding gegevenssets en modules om uw blog analyse model te maken.

![Azure ML Studio diagram: experimenten maken, het lezen van gegevens voor veel bronnen, het schrijven van gegevens van een score voorzien, modellen schrijven.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Aan de slag met Machine Learning Studio

Wanneer u eerst [Machine Learning Studio](https://studio.azureml.net) ziet u **de introductiepagina** . Hier kunt u documentatie, video's, webinars weergeven en andere waardevolle bronnen vinden.

Er zijn drie tabbladen aan de bovenkant: **Start** (waar u begint), **Studio**en **Galerie**.

### <a name="studio"></a>Studio

Klik op het tabblad **Studio** en wordt u gevraagd aan te melden met uw Microsoft-account of uw account voor werk- of schoolaccount. Zodra aangemeld, ziet u de volgende tabbladen aan de linkerkant:

- **Projecten** - verzamelingen van experimenten, gegevenssets notitieblokken en andere resources dat staat voor één project
- **EXPERIMENTEN** - experimenten die zijn gemaakt, uitvoeren, en als concepten worden opgeslagen
- **WEB SERVICES** - webservices die u hebt geïmplementeerd vanuit uw experimenten
- **NOTITIEBLOKKEN** - Jupyter notitieblokken die u hebt gemaakt
- **GEGEVENSSETS** - gegevenssets die u hebt geüpload naar Studio
- **Ervaren modellen** - modellen die u hebt ervaren in proeven en opgeslagen in Studio
- **Instellingen** - een verzameling instellingen die u gebruiken kunt voor het configureren van uw account en resources.

### <a name="gallery"></a>Galerie

Klik op het tabblad **Galerie** en u moet worden gemaakt in de galerie met Cortana Intelligence. De galerie is een plaats waar een community van gegevens wetenschappers en ontwikkelaars oplossingen die zijn gemaakt met behulp van de onderdelen van de Cortana Intelligence-Suite kunt delen.

Zie voor meer informatie over de galerie, [delen en oplossingen in de galerie met Cortana Intelligence ontdekken](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Onderdelen van een experiment

Een experiment bestaat uit gegevenssets die gegevens voor analytical modules, die u met elkaar verbinden om een blog analyse model te maken. Specifiek, heeft een geldige experiment deze kenmerken:

- Het experiment heeft ten minste één gegevensset en één module
- Gegevenssets kan niet worden verbonden alleen met modules
- Modules kunnen niet worden verbonden met gegevenssets of andere modules
- Alle invoer poorten voor modules moeten sommige verbinding met de gegevensstroom
- Alle vereiste parameters voor elke module moeten worden ingesteld

U kunt een experiment helemaal maken of kunt u een bestaande steekproef experiment gebruiken als een sjabloon. Zie [Gebruik steekproef experimenten nieuwe experimenten maken](machine-learning-sample-experiments.md)voor meer informatie.

Zie voor een voorbeeld van het maken van een eenvoudige experiment [maken een eenvoudige experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md).

Zie voor gedetailleerde stapsgewijze instructies voor het maken van een blog analytics-oplossing, [een blog-oplossing met Azure Machine Learning ontwikkelen](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Gegevenssets

Een gegevensset is gegevens die is geüpload naar Machine Learning Studio zodat deze kan worden gebruikt in het proces modellering. Een getal van de steekproef gegevenssets zijn opgenomen in Machine Learning Studio om te experimenteren met en u kunt meer gegevenssets kunt uploaden. Hier volgen enkele voorbeelden van opgenomen gegevenssets:

- **MPG gegevens voor verschillende auto** - mijl per gallon (MPG) waarden voor auto die wordt aangeduid met aantal flessen, paardenkracht, enzovoort.
- **En kanker gegevens** - en kanker diagnose gegevens.
- **Bos geactiveerd gegevens** - bos fire grootte in de regio Noord-Oost Portugal.

Het opzetten van een experiment kunt u kiezen uit de lijst met beschikbare gegevenssets aan de linkerkant van het tekenpapier.

Zie [de steekproef gegevenssets in Azure Machine Learning Studio gebruiken](machine-learning-use-sample-datasets.md)voor een lijst met steekproef gegevenssets opgenomen in Machine Learning Studio.

### <a name="modules"></a>Modules

Een module is een algoritme die u op uw gegevens uitvoeren kunt. Machine Learning Studio heeft een aantal modules die variëren van ingress gegevensfuncties training, scoren en processen voor gegevensvalidatie. Hier volgen enkele voorbeelden van opgenomen modules:

- [Converteren naar ARFF] [ convert-to-arff] -converteert een serieel .NET-gegevensset naar kenmerk relatie bestand opmaken (ARFF).
- [Elementaire statistieken berekenen] [ elementary-statistics] -berekent elementaire statistieken zoals gemiddelde, standaarddeviatie, enzovoort.
- [Lineaire regressie] [ linear-regression] -Hiermee maakt u een online kleurovergang afkomst gebaseerde lineaire regressie-model.
- [Model score] [ score-model] -scoort een ervaren classificatie of regressie-model.

Het opzetten van een experiment kunt u kiezen uit de lijst met beschikbare modules aan de linkerkant van het tekenpapier.  

Een module wellicht een set met parameters die u gebruiken kunt voor het configureren van interne algoritmen van de module. Wanneer u een module op het canvas selecteert, worden in het deelvenster **Eigenschappen** aan de rechterkant van het tekenpapier van de module parameters weergegeven. U kunt de parameters in dat deelvenster om af te stellen uw model kunt wijzigen.

Zie voor sommige navigeren in het grote bibliotheek van machine learning algoritmen beschikbaar help, [het algoritmen voor Microsoft Azure Machine Learning kiezen](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Een blog analytics-webservice implementeren

Zodra uw blog analytics-model klaar is, kunt u deze als een webservice rechtstreeks vanuit Machine Learning Studio implementeren. Zie voor meer informatie over dit proces, [Deploy een Azure Machine Learning-webservice](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
