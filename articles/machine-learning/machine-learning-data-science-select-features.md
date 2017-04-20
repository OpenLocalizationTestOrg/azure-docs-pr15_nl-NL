<properties
    pageTitle="Selectie in het Team gegevens wetenschap proces aanbevelen | Microsoft Azure" 
    description="Dit artikel wordt uitgelegd het doel van de functieselectie en worden voorbeelden gegeven van hun rol in het proces voor het verbeteren van gegevens van machine learning."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Onderdelen selecteren in het Team gegevens wetenschap proces (TDSP)

In dit artikel wordt uitgelegd van de doelstellingen van de functieselectie en worden voorbeelden gegeven van de rol in het proces voor het verbeteren van gegevens van machine learning. In deze voorbeelden worden getekend van Azure Machine Learning Studio. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


In dit onderwerp wordt uitgelegd van het doel van de functieselectie en worden voorbeelden gegeven van de rol in het proces voor het verbeteren van gegevens van machine learning. In deze voorbeelden worden getekend van Azure Machine Learning Studio. 

De technische functie en de selectie van functies maakt u één deel uit van de TDSP die worden beschreven in de [Wat is het proces Team gegevens wetenschappelijke?](data-science-process-overview.md). Zijn de onderdelen van de stap **ontwikkelen onderdelen** van de TDSP functie engineering en selectie.

* **technische functie**: dit proces probeert aanvullende relevante functies vanuit de bestaande onbewerkte functies in de gegevens maken en om de blog power op de algoritme van learning te verbeteren.

* **Functieselectie**: dit proces Hiermee selecteert u de belangrijkste subset van de functies van de oorspronkelijke gegevens in een poging verkleinen van de dimensionaliteit van het probleem training.

Normaal gesproken **functie engineering** eerst wordt toegepast om te genereren aanvullende functies en vervolgens de stap voor het **selecteren van de functie** wordt uitgevoerd om te voorkomen niet relevant, overbodige of zeer gecorreleerde functies.


## <a name="filtering-features-from-your-data---feature-selection"></a>Filterfuncties uit uw gegevens - onderdelen selecteren 

Functieselectie is een proces dat meestal voor de bouw van training gegevenssets voor blog modellering taken zoals classificatie of regressie taken wordt toegepast. Het doel is om een subset van de functies van de oorspronkelijke gegevensset die de afmetingen beperken met behulp van een minimum aantal functies om aan te geven van de maximale hoeveelheid variantie in de gegevens te selecteren. In dit een subset van de functies zijn vervolgens alleen functies op te nemen aan het model trainen. Functieselectie fungeert voornamelijk gebruikt.

* Eerst Functieselectie vaak verhogen classificatie nauwkeurigheid doordat niet relevant, overbodige of sterke correlatie bestaat tussen functies.
* Tweede, wordt het aantal functies waardoor model trainingsproces efficiënter kleiner. Dit is vooral voor cursisten die echter duur in trainen zoals ondersteuning vector machines.

Hoewel de Functieselectie willen Beperk het aantal onderdelen in de gegevensset gebruikt om te trainen van het model, het is niet meestal wordt verwezen door de term "dimensionaliteit wilt verkleinen". Functie selectiemethoden extraheren een subset van de oorspronkelijke functies in de gegevens niet wijzigen.  Dimensionaliteit wilt verkleinen methoden gebruiken ontworpen functies waarmee u kunnen de oorspronkelijke functies transformeren en deze zo te wijzigen. Voorbeelden van dimensionaliteit wilt verkleinen methoden zijn hoofdsom onderdeel analyse, canonieke correlatieanalyse en enkelvoud waarde uitgevouwen.

Onder andere wordt één breed worden toegepast categorie van de functie selectiemethoden in een gecontroleerde context 'Selectiefilter Functieselectie' genoemd. Vóór evaluatie van de correlatie tussen elke functie en het kenmerk target, worden in deze methoden een statistische eenheid een score toewijzen aan elke functie toepassen. De functies zijn vervolgens op de score, die kunnen worden gebruikt om u te helpen bij het instellen van de drempel voor bewaren of verwijderen van een specifieke functie gerangschikt. Voorbeelden van de statistische eenheden gebruikt in de volgende manieren zijn persoon correlatie, onderlinge informatie en de test van de Chi-kwadraat.

Azure Machine Learning Studio zijn opgegeven voor de Functieselectie modules. In de volgende afbeelding ziet deze modules [Filter gebaseerde Functieselectie] opnemen[ filter-based-feature-selection] en [Fisher lineaire Discriminant analyse][fisher-linear-discriminant-analysis].

![Voorbeeld van de functie selectie](./media/machine-learning-data-science-select-features/feature-Selection.png)


Houd rekening met, bijvoorbeeld het gebruik van de [Functieselectie Filter gebaseerde] [ filter-based-feature-selection] module. Voor het gemak blijven we gebruiken de tekst mining voorbeeld hierboven beschreven. Stel dat we wilt maken van een regressiemodel nadat een reeks 256 functies zijn gemaakt door de [Functie Hashing] [ feature-hashing] module en dat de antwoord-variabele is het "Kol1" en vertegenwoordigt een boek Raadpleeg classificaties getal van 1 tot en met 5. Door in te stellen "Aanbevelen score methode" moeten "Pearson correlatie", "doelkolom" 'Kol1', en de 'getal van de gewenste functies' op 50. Vervolgens de module [Filter gebaseerde onderdelen selecteren] [ filter-based-feature-selection] een gegevensset met 50 functies gebruiken samen met het kenmerk target "Kol1" zullen produceren. De volgende afbeelding ziet de stroom van dit experiment en de invoerparameters die hierboven wordt beschreven.

![Voorbeeld van de functie selectie](./media/machine-learning-data-science-select-features/feature-Selection1.png)

De volgende afbeelding ziet de resulterende gegevenssets. Elke functie is gebaseerd behaald bij de Pearson-correlatiecoëfficiënt tussen zichzelf en door het kenmerk target "Kol1". De functies met bovenste scores worden bewaard.

![Voorbeeld van de functie selectie](./media/machine-learning-data-science-select-features/feature-Selection2.png)

De bijbehorende scores van de geselecteerde onderdelen worden weergegeven in de volgende afbeelding.

![Voorbeeld van de functie selectie](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Met deze [Functieselectie op basis van een Filter] toepassen[ filter-based-feature-selection] module 50 van 256 functies zijn geselecteerd, omdat ze niet de meest gecorreleerde functies met de doel-variabele 'Kol1', op basis van de score 'Pearson-correlatiecoëfficiënt'-methode.

## <a name="conclusion"></a>Sluiten
Technische functie en de Functieselectie zijn twee meest ontworpen en de efficiëntie van het trainingsproces inhoudt pogingen kunt ophalen van de belangrijkste informatie in de gegevens voor geselecteerde onderdelen verhogen. Ze ook verbeteren de kracht van deze modellen de invoergegevens nauwkeurig classificeren en resultaten van belang meer krachtig voorspellen. Technische functie en de selectie kunnen ook zodat het onderwijs meer rekenkundig tractable combineren. Dit gebeurt door te klikken en vervolgens waardoor het aantal functies die nodig zijn om te kalibreren of trainen een model verbeteren. Wiskundig spreekt, zijn de functies die zijn geselecteerd voor het model trainen een minimum aantal onafhankelijke variabelen die de patronen in de gegevens te leggen en vervolgens succes voorspellen resultaten.

Houd er rekening mee dat het is niet altijd per se om uit te voeren functies engineering of functie selecteren. Opgeven of het nodig is of niet, is afhankelijk van de gegevens die we hebben of verzamelen, de algoritme van de die we kiezen en het doel van het experiment.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
 
