<properties
    pageTitle="PowerShell-module voor Machine Learning | Microsoft Azure"
    description="De PowerShell-module voor Azure Machine Learning is beschikbaar in openbare preview-modus. PowerShell gebruiken om te maken en beheren van werkruimten, experimenten, webservices en meer."
    keywords="experimenteren, lineaire regressie, machine learning algoritmen, machine learning zelfstudie, bekijk modellering technieken, gegevens wetenschappelijk experiment"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>PowerShell-module voor Microsoft Azure Machine Learning

De PowerShell-module voor Azure Machine Learning is een krachtige hulpprogramma waarmee u Windows PowerShell gebruiken voor het beheren van werkruimten, experimenten, gegevenssets, web serivces en meer.

U kunt de documentatie weergeven en downloaden van de module, samen met de volledige broncode, bij [https://aka.ms/amlps](https://aka.ms/amlps). 

## <a name="what-is-the-machine-learning-powershell-module"></a>Wat is de Machine Learning PowerShell-module?

De Machine Learning PowerShell-module is een. NET gebaseerde DLL-module waarmee u volledig beheer Azure Machine Learning-werkruimten, experimenten gegevenssets, webservices en eindpunten van een webservice vanaf Windows PowerShell. Samen met de module, kunt u de volledige broncode waaronder een duidelijke gescheiden [C#-API-laag](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs)downloaden. Dit betekent kunt u verwijzen naar dit dll-bestand uit uw eigen .NET-project en beheren van Azure Machine Learning tot en met .NET-code. Bovendien afhankelijk de DLL van de onderliggende REST API's die u van rechtstreeks vanuit uw favoriete-client gebruikmaken kunt.

## <a name="what-can-i-do-with-the-powershell-module"></a>Wat kan ik doen met de PowerShell-module?

Hier volgen enkele van de taken die u met deze PowerShell-module uitvoeren kunt. Bekijk de [volledige documentatie](https://aka.ms/amlps) voor deze en veel meer functies.

- Een nieuwe werkruimte met behulp van een certificaat management ([Nieuw-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace)) inrichten
- Exporteren en importeren van een JSON-bestand dat staat voor een experiment graph ([Exporteren-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) en [Importeren-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
- Uitvoeren van een experiment ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
- Maken van een webservice afmelden bij een blog experiment ([Nieuw-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Een nieuw eindpunt te maken van een gepubliceerde webservice ([Toevoegen-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Roepen een records en/of BES web service-eindpunt ([Roep-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) en [Roep-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Hier volgt een snel voorbeeld van PowerShell gebruiken voor het uitvoeren van een bestaande experiment:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Zie voor een gebruiksvoorbeeld verder in dit artikel over het gebruik van de PowerShell-module om een zeer aangevraagd taak te automatiseren: [maken veel Machine Learning-modellen en web service eindpunten van een experiment via PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Hoe ga ik aan de slag?

Om te beginnen met Machine Learning PowerShell downloaden van het [vrijgeven van pakket](https://github.com/hning86/azuremlps/releases) van GitHub en volg de [instructies voor de installatie](https://github.com/hning86/azuremlps/blob/master/README.md). U moet de DLL gedownload/bestanden de blokkering opheffen en vervolgens te importeren in uw omgeving PowerShell. Grootste deel van de cmdlets vereisen dat u opgeeft dat de ID van de werkruimte, de werkruimte Autorisatietoken en de Azure regio waarop de werkruimte zich bevindt. De eenvoudigste manier om aan te bieden deze is via een standaard config.json-bestand, waarvoor nader in de installatie-instructies. Uiteraard kunt kunt u ook de structuur cijfer klonen en de code lokaal gebruik van Visual Studio wijzigen/samenstellen.

## <a name="next-steps"></a>Volgende stappen

De PowerShell-module blijft worden verbeterd en uitgevouwen tijdens deze periode preview. Gaten houden de [Cortana Intelligence en Machine Learning-Blog](https://blogs.technet.microsoft.com/machinelearning/) voor meer informatie en nieuws.
