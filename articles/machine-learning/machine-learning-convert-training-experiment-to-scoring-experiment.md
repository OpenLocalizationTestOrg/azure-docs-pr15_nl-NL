<properties
    pageTitle="Een experiment Machine Learning-training converteren naar een blog experiment | Microsoft Azure"
    description="Klik hier voor meer informatie over het converteren van een experiment Machine Learning-training, die wordt gebruikt voor uw blog analytics-model, naar een blog experiment die kan worden geïmplementeerd als een webservice training."
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
    ms.date="08/19/2016"
    ms.author="garye"/>

# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>Een experiment Machine Learning-training converteren naar een blog experiment

Azure Machine Learning kunt u bouwen, testen en bekijk analyses oplossingen implementeren.

Nadat u hebt gemaakt en klik op een *training experimenteren* om te trainen van uw blog analytics-model herhaald en u gebruiken wilt voor het verkrijgen van nieuwe gegevens, moet u voorbereiden en uw experiment voor het scoren stroomlijnen. Vervolgens kunt u dit *blog experiment* implementeren als een Azure webservice zodat gebruikers kunnen gegevens naar uw model verzenden en van uw model voorspellingen ontvangen.

Door te converteren naar een blog experiment, bent u uw ervaren model klaar om te worden geïmplementeerd als een webservice krijgt. Gebruikers van de webservice stuurt invoergegevens naar uw model en uw model stuurt weer de resultaten van de voorspelling. Dus als u naar een blog experiment converteren zult u in gedachten moet houden hoe u uw model moet worden gebruikt door anderen verwachten.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Het proces van het converteren van een experiment training naar een blog experiment omvat drie stappen:

1.  Sla het machine learning-model dat u hebt training en vervang de machine learning-algoritme en [Trein Model] [ train-model] modules met uw opgeslagen ervaren model.
2.  Het experiment naar alleen de modules die nodig zijn voor het scoren knippen. Een experiment training bevat een aantal modules die nodig zijn voor training, maar niet nodig zijn als u het model hebt ervaren en klaar voor gebruik voor het scoren.
3.  Definiëren waar uw experiment u accepteren in gegevens uit de gebruiker van de web-service en welke gegevens worden geretourneerd.

## <a name="set-up-web-service-button"></a>Knop webservice instellen

Nadat u uw experiment (knop**uitvoeren** onder aan het tekenpapier experiment) hebt uitgevoerd, wordt de knop **Webservice instellen** (selecteren de optie **Blog webservice** ) de drie stappen uw experiment training converteren naar een blog experiment voor u uitvoeren:

1.  Uw ervaren model wordt opgeslagen als een module in het gedeelte **Ervaren modellen** van het palet module (aan de linkerkant van het tekenpapier experiment), vervangen door de machine learning-algoritme en [Trein Model] [ train-model] modules met het opgeslagen ervaren model.
2.  Hiermee verwijdert u modules die duidelijk niet nodig zijn. In ons voorbeeld deze groep omvat de [Gesplitste gegevens][split], 2e [Score Model][score-model], en [Model evalueren] [ evaluate-model] modules.
3.  Er wordt gemaakt van Web service invoer en uitvoer modules en op standaardlocaties in uw experiment toevoegt.

Het volgende experiment traint bijvoorbeeld een twee-klasse gestimuleerd beslissing structuur model telling voorbeeldgegevens gebruikt:

![Training experiment][figure1]

De modules in dit experiment met vier verschillende functies uit te voeren:

![Functies van de module][figure2]

Wanneer u dit experiment training naar een blog experiment converteren, enkele van deze modules niet meer nodig zijn of ze een ander doel hebben:

- **Gegevens** - de gegevens in deze gegevensset voorbeeld niet wordt gebruikt tijdens het scoren: de gebruiker van de webservice levert de gegevens die moeten worden van een score voorzien. De metagegevens van deze dataset, zoals gegevenstypen, is echter wel gebruikt door de ervaren model. Zo moet u de gegevensset in de blog experiment houden, zodat deze metagegevens kan bieden.

- **Prep** - afhankelijk van de gegevens die worden verzonden voor het scoren, deze modules al dan niet nodig is voor de inkomende gegevens.

    Bijvoorbeeld, in dit voorbeeld de gegevensset steekproef ontbrekende waarden hebt en deze kolommen die u niet nodig zijn om te trainen van het model bevat. Dus een [Schone ontbrekende gegevens] [ clean-missing-data] module is opgenomen behandelen ontbrekende waarden en een [Kolommen selecteren in de gegevensset] [ select-columns] module is opgenomen die extra kolommen uit de gegevensstroom uitsluiten. Als u weet dat de gegevens die worden verzonden voor het scoren via de webservice hebben geen ontbrekende waarden en klik vervolgens kunt u de [Wissen.Control ontbrekende gegevens] verwijderen[ clean-missing-data] module. Echter sinds de [Kolommen selecteren in de gegevensset] [ select-columns] module helpt u bij het definiëren van de set functies wordt behaald, die module moet blijven.

- **Trein** - zodra het model is getraind, u deze opslaan als een module één ervaren model. U kunt vervolgens deze afzonderlijke modules vervangen met het opgeslagen ervaren model.

- **Score** - wordt In dit voorbeeld de module gesplitste gebruikt de gegevensstroom in een set testgegevens en trainingsgegevens verdelen. Klik in het blog experiment dit niet nodig is en kan worden verwijderd. Op dezelfde manier voor het 2e [Score Model] [ score-model] module en het [Model evalueren] [ evaluate-model] module worden gebruikt voor het vergelijken van resultaten van de testgegevens, zodat deze modules ook niet nodig hebt bij het blog experiment. Het resterende [Score Model] [ score-model] module echter nodig is om te retourneren van het resultaat van een score via de webservice.

Hier ziet u hoe ons voorbeeld eruit ziet nadat u hebt geklikt **Webservice instellen**:

![Geconverteerde blog experiment][figure3]

Mogelijk voldoende voor het voorbereiden van uw experiment worden geïmplementeerd als een webservice. U kunt echter enkele extra werk die specifiek zijn voor uw experiment doen.

### <a name="adjust-input-and-output-modules"></a>Invoer- en uitvoerbereik modules aanpassen

In uw experiment training, moet u een reeks trainingsgegevens gebruikt en vervolgens enkele verwerking om de gegevens in een formulier dat de algoritme van de machine learning nodig hebt. Als de gegevens die u verwacht te ontvangen via de webservice deze verwerking niet nodig hebt, kunt u de **Web-service invoer module** verplaatsen naar een ander knooppunt in uw experiment.

Bijvoorbeeld, al dan niet standaard **Webservice instellen** weer kunt u de module **Web service invoer** boven aan de gegevensstroom van uw, zoals in de bovenstaande afbeelding. Als de invoergegevens deze verwerking niet nodig hebt, kunt klikt u echter handmatig plaatsen de **Web-service invoer** voorbij de modules gegevensverwerking:

![De invoer van de service web verplaatsen][figure4]

De ingevoerde gegevens via de webservice wordt nu doorgegeven rechtstreeks in de module Score Model zonder een voorverwerking.

Standaard wordt **Webservice instellen** op dezelfde manier de module Web service uitvoer onderaan in de gegevensstroom geplaatst. In dit voorbeeld de webservice keert terug naar de gebruiker de uitvoer van het [Model Score] [ score-model] module waarin de vector invoergegevens voltooid plus scores voor het resultaat.

Als u liever een ander - retourneren bijvoorbeeld, alleen de score resultaten en niet de hele vector van invoergegevens- en u kunnen echter invoegen een [Kolommen selecteren in de gegevensset] [ select-columns] module uitsluiten van alle kolommen behalve de score resultaten. U vervolgens de module **Web service uitvoer** verplaatst naar de uitvoer van de [Kolommen selecteren in de gegevensset] [ select-columns] module:

![De uitvoer van de service web verplaatsen][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Toevoegen of verwijderen van extra gegevensverwerking modules

Als er meer modules in uw experiment waarvan u niet meer nodig weet tijdens scoren, kunnen deze worden verwijderd. Omdat we de module **Web service invoer** naar een positie na de gegevensverwerking modules verplaatst hebben, kunnen we bijvoorbeeld de [Wissen.Control ontbrekende gegevens] verwijderen[ clean-missing-data] module van het blog experiment.

Onze blog experiment ziet er nu zo uit:

![Extra module verwijderen][figure6]

### <a name="add-optional-web-service-parameters"></a>Optionele Web Service Parameters toevoegen

In sommige gevallen wilt u mogelijk de gebruiker toestaan van uw web-service te wijzigen het gedrag van modules als de service wordt geopend. *Web Service Parameters* kunt u dit wilt doen.

Een algemeen voorbeeld is instellen van de [Gegevens importeren] [ import-data] module zodat de gebruiker van de webservice gedistribueerde een andere gegevensbron opgeven kunt wanneer de webservice wordt geopend. Of te configureren de [Gegevens exporteren] [ export-data] module zodat een andere bestemming kan worden opgegeven.

U kunt Web Service Parameters definiëren en koppel deze aan een of meer moduleparameters, en kunt u opgeven of ze verplicht of optioneel zijn. De gebruiker van de webservice kan vervolgens waarden voor deze parameters opgeven wanneer de service wordt geraadpleegd en de acties module wordt dienovereenkomstig gewijzigd.

Zie voor meer informatie over Web Service Parameters, [Gebruik Azure Machine Learning Web Service Parameters ] [ webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Het blog experiment te implementeren als een webservice

Nu dat het blog experiment voldoende is voorbereid, kunt u het dashboard implementeren als een Azure webservice. Met de webservice, kunnen gebruikers gegevens verzenden naar uw model en het model de voorspellingen zullen retourneren.

Zie voor meer informatie over het implementatieproces voltooien, [Deploy een Azure Machine Learning-webservice][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
