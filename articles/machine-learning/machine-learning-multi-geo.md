<properties
   pageTitle="Multi-geografische Help-documentatie | Microsoft Azure"
   description="Leer hoe u een werkruimte maken en publiceren van een webservice in een andere uit de Zuid centraal Verenigde Staten (SCUS) Azure gebied Azure regio."
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="jhubbard"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="tedway; neerajkh"/>

# <a name="multi-geo-help-documentation"></a>Multi-geografische Help-documentatie

In dit artikel wordt beschreven hoe u kunt een werkruimte maken en publiceren van een webservice in andere Azure regio's.

## <a name="create-a-workspace"></a>Een werkruimte maken

1. Meld u aan bij de Portal van Azure klassieke.

2.  Klik op **+ Nieuw** > **GEGEVENSSERVICES** > **MACHINE LEARNING** > **snelle maken**.  Selecteer een andere regio, zoals **Zuidoost-Azië**onder **locatie** .
![Afbeelding van Help-multi-geografische 1][1]
3. Selecteer de werkruimte en klik vervolgens op **aanmelden bij ML Studio**.
![Afbeelding van Help-multi-geografische 2][2]

4. U hebt nu een werkruimte in een andere regio die u op dezelfde manier als een andere werkruimte kunt. Als u wilt schakelen tussen uw werkruimten, kijkt u in de rechterbovenhoek van het scherm. Klik op de vervolgkeuzelijst, selecteer het gebied en selecteer vervolgens de werkruimte. Alles bevindt zich in de regio van de werkruimte; bijvoorbeeld, worden al uw webservices gemaakt op basis van een werkruimte in de werkruimte bevindt zich in dezelfde regio.
![Help voor multi-geografische afbeelding 3][3]

## <a name="open-an-experiment-from-gallery"></a>Open een experiment uit galerie

Als u een experiment uit galerie opent, kunt u ook welke regio die u wilt kopiëren van het experiment om te selecteren.

![Afbeelding van Help-multi-geografische 4][4a]

## <a name="web-service-management"></a>Webservice beheren

U kunt via programmacode webservices, zoals voor omschakeling beheren door het land / regiospecifieke-adres te gebruiken:
- https://asiasoutheast.Management.azureml.NET
- https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Punten onthouden

1.  U kunt alleen experimenten tussen werkruimten die deel uitmaakt van dezelfde regio deze manier kopiëren. Als u wilt kopiëren experiment over werkruimten in verschillende regio's, kunt u de [PowerShell](http://aka.ms/amlps) -commandlet [*Kopie-AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) gebruiken voor het uitvoeren van die. Een andere tijdelijke oplossing is het experiment in galerie publiceren in de modus voor niet-vermelde, opent u het in de werkruimte uit de andere regio.
2.  Werkruimten voor één regio worden alleen door de kiezer regio tegelijk worden weergegeven. In de toekomst is mogelijk om een volledige lijst met werkruimten die u toegang tot in alle regio's tegelijkertijd hebt weer te geven.  
3.  Een gratis werkruimte of gastaccounts (anoniem) werkruimte worden gemaakt en ingesloten in een centrale Zuid-Amerikaanse In de toekomst is mogelijk gratis/Gast Access-werkruimten maken in de regio die u kiest.  
4.  Webservices geïmplementeerd vanuit een werkruimte in Zuidoost-Azië wordt ook worden gehost in Zuidoost-Azië. In de toekomst is mogelijk dat de flexibiliteit van experimenten maken in één regio en eindpunten van een webservice in verschillende gebieden implementeert gegenereerd.  

## <a name="more-information"></a>Meer informatie

Stel een vraag op het [Azure Machine Learning-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
