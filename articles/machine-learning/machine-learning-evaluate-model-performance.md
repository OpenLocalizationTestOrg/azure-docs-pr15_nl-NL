<properties 
    pageTitle="Model prestaties in Machine Learning evalueren | Microsoft Azure" 
    description="Wordt uitgelegd hoe u het model prestaties in Azure Machine Learning evalueren." 
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
    ms.author="bradsev;garye" />


# <a name="how-to-evaluate-model-performance-in-azure-machine-learning"></a>Hoe u het model prestaties in Azure Machine Learning evalueren

In dit onderwerp laat zien hoe de prestaties van een model in Azure Machine Learning Studio wordt berekend en biedt een korte beschrijving van de doelstellingen beschikbaar voor deze taak. Drie veelvoorkomende scenario's met gecontroleerde learning worden weergegeven: 

* regressie
* binaire classificatie 
* multiclass classificatie

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Evaluatie van de prestaties van een model is een van de fasen core in het proces van de wetenschappelijke gegevens. Hiermee wordt aangegeven hoe geslaagd het score (voorspellingen) van een gegevensset is door een ervaren model. 

Azure Machine Learning ondersteunt model evaluatie tot en met twee van de belangrijkste machine learning-modules: [Evalueren Model] [ evaluate-model] en [Model kruislings gevalideerd][cross-validate-model]. Deze modules kunnen u zien hoe uw model uitvoert in een getal van de doelstellingen die vaak worden gebruikt in machine learning en statistieken worden aangepast.

##<a name="evaluation-vs-cross-validation"></a>Evaluatie versus Cross gegevensvalidatie##
Evaluatie en cross gegevensvalidatie zijn standaard manieren om de prestaties van uw model te meten. Beide evaluatie aan de doelstellingen die u kunt controleren of vergelijken met die van andere modellen genereren.

[Model evalueren] [ evaluate-model] een scored gegevensset verwacht als invoer (of 2 in geval u wilt vergelijken van de prestaties van 2 verschillende modellen). Dit betekent dat u moet uw model met het [Model trainen] trainen[ train-model] module en aanbrengen voorspellingen op sommige gegevensset met het [Model Score] [ score-model] module, voordat u de resultaten kunt evalueren. De evaluatie is het op basis van de scored etiketten/waarschijnlijkheid samen met de waar etiketten, die uitgevoerd door het [Model Score worden] [ score-model] module.

U kunt ook kunt u cross validatie om uit te voeren van een aantal bewerkingen trein score evalueren (10 vouwen) automatisch op verschillende subsets van de invoergegevens. De invoergegevens opgesplitst 10 onderdelen, waar is een gereserveerd voor het testen van, en de andere 9 voor training. Dit proces wordt herhaald 10 en de doelstellingen van de evaluatie gemiddelde. Dit helpt bij het bepalen hoe u ook een model zou generalize naar nieuwe gegevenssets. Het [Model kruislings gevalideerd] [ cross-validate-model] module worden in een ongetrainde model en sommige gelabelde gegevensset en Hiermee kunt u de evaluatieresultaten van elk van de 10 vouwen, naast de gemiddelde resultaten.

In de volgende secties we maken van eenvoudige regressie en classificatie-modellen en hun prestaties, evalueren met zowel het [Model evalueren] [ evaluate-model] en het [Model kruislings gevalideerd] [ cross-validate-model] modules.

##<a name="evaluating-a-regression-model"></a>Evaluatie van een regressiemodel##
Wordt ervan uitgegaan dat we wilt voorspellen een auto prijs met bepaalde functies zoals afmetingen, paardenkracht engine specificaties en dergelijke. Dit is een probleem typische regressie, waarbij de doel-variabele (*prijs*) een doorlopend numerieke waarde is. We kunt een enkelvoudige lineaire regressiemodel dat, gegeven van de functie waarden van een bepaalde auto, de prijs van die auto kunt voorspellen plaatsen. Dit regressiemodel kan worden gebruikt voor het verkrijgen van dezelfde dataset die we training op. Als we de voorspelde prijzen voor alle de auto's hebt, kunt we de prestaties van het model door te kijken de voorspellingen hoeveel van de werkelijke prijzen gemiddeld afwijken evalueren. We gebruiken de *auto prijs gegevens (onbewerkte) gegevensset* beschikbaar in de sectie **Gegevenssets opgeslagen** in Azure Machine Learning Studio ter illustratie.
 
###<a name="creating-the-experiment"></a>Maken van het Experiment###
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

- Auto prijsgegevens (onbewerkte)
- [Lineaire regressie][linear-regression]
- [Trein Model][train-model]
- [Score-Model][score-model]
- [Model evalueren][evaluate-model]


Verbinding maken met de poorten, zoals hieronder wordt weergegeven in afbeelding 1 en stelt u de kolom van het Label van het [Model trein] [ train-model] module *prijs*.
 
![Evaluatie van een regressiemodel](media/machine-learning-evaluate-model-performance/1.png)

Afbeelding 1. Evaluatie van een regressiemodel.

###<a name="inspecting-the-evaluation-results"></a>De evaluatieresultaten controleren###
Na het uitvoeren van het experiment, kunt u klikken op de uitvoerpoort van het [Model evalueren] [ evaluate-model] module en selecteer *visualiseren* om de evaluatieresultaten te zien. Beschikbaar voor regressie-modellen op de doelstellingen van de evaluatie zijn: *Absolute Error bedoelt*, *Hoofdsite bedoelt Absolute Error* *Relatieve, Absolute Error*, *Relatieve kwadraat fout*en de *Coëfficiënt van bepaling*.

De term "fout" hier het verschil aangeeft tussen de voorspelde waarde en de werkelijke waarde. De absolute waarde of het kwadraat van dit verschil wordt meestal berekend om vast te leggen van de totale grootte van fout in alle exemplaren, zoals het verschil tussen de voorspelde en waar waarde negatieve in sommige gevallen zijn. De doelstellingen van de fout meten de blog prestaties van een regressiemodel tussen de afwijking van het gemiddelde van de voorspellingen van de waarden waar. Lagere foutwaarden betekent dat het model is nauwkeuriger bij het maken van voorspellingen. Een algemene error-meetwaarde 0 betekent dat het model exact de gegevens past.

De correlatiecoëfficiënt, dat wil ook bekend als R zeggen kwadraat, is ook de gebruikelijke manier voor het meten van hoe u ook het model de gegevens past. Dit kan worden geïnterpreteerd als de verhouding van variatie uitleg door het model. Een hogere verhouding is in dit geval beter waarbij 1 wordt aangegeven past perfect.
 
![Aan de doelstellingen van evaluatie in lineaire regressie](media/machine-learning-evaluate-model-performance/2.png)

Afbeelding 2. Lineaire regressie evaluatie van de doelstellingen.

###<a name="using-cross-validation"></a>Met kruisverwijzingen gegevensvalidatie###
Zoals eerder is vermeld, u kunt uitvoeren herhaalde training, scoren en evaluaties automatisch met het [Model kruislings gevalideerd] [ cross-validate-model] module. U hoeft alleen in dit geval is een gegevensset, een ongetrainde model en een [Model valideren van meerdere] [ cross-validate-model] module (Zie onderstaande afbeelding). U moet de labelkolom instellen op *prijs* in het [Model kruislings gevalideerd] [ cross-validate-model] eigenschappen van de module.

![Een regressiemodel cross-valideren](media/machine-learning-evaluate-model-performance/3.png)

Afbeelding 3. Cross-een regressiemodel valideren.

Na het uitvoeren van het experiment, kunt u de evaluatieresultaten controleren door te klikken op de juiste uitvoerpoort van het [Model kruislings gevalideerd] [ cross-validate-model] module. Hierdoor krijgt een gedetailleerd overzicht van de doelstellingen voor elke iteratie (gevouwen) en de gemiddelde resultaten van elk van de doelstellingen (figuur 4).
 
![Cross-validatie resultaten van een regressiemodel](media/machine-learning-evaluate-model-performance/4.png)

Afbeelding 4. Cross-validatie resultaten van een regressiemodel.

##<a name="evaluating-a-binary-classification-model"></a>Evaluatie van een Model binaire classificatie##
In een scenario voor binaire indeling heeft de doel-variabele slechts twee mogelijke resultaten, bijvoorbeeld: {0, 1} of {ONWAAR, waar}, {negatieve, positieve}. Wordt ervan uitgegaan dat u krijgt een gegevensset van volwassen werknemers met enkele demografische en indiensttreding variabelen en dat u wordt gevraagd om te voorspellen van het niveau van de inkomsten, een binaire variabele met de waarden {"< 50K =", "> 50K"}. Met andere woorden, de negatieve klasse geeft de werknemers die kleiner dan of gelijk is aan 50K per jaar en de positieve klasse geeft alle andere werknemers. Zoals in het scenario regressie zou doen we een model trainen, score sommige gegevens en evalueren van de resultaten. Het belangrijkste verschil hier is de keuze van Azure Machine Learning berekent aan de doelstellingen en uitvoer. Om te illustreren de inkomsten niveau tekstvoorspelling scenario, gebruiken we de [volwassen](http://archive.ics.uci.edu/ml/datasets/Adult) gegevensset maken van een experiment Azure Machine Learning en de prestaties van een twee-klasse logistische regressie-model, een veelgebruikte binaire classificatie evalueren.

###<a name="creating-the-experiment"></a>Maken van het Experiment###
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

- Volwassen telling inkomsten binaire indeling gegevensset
- [Twee-klasse logistische regressie][two-class-logistic-regression]
- [Trein Model][train-model]
- [Score-Model][score-model]
- [Model evalueren][evaluate-model]

Verbinding maken met de poorten, zoals hieronder wordt weergegeven in afbeelding 5 en stelt u de kolom van het Label van het [Model trein] [ train-model] module *inkomsten*.

![Evaluatie van een Model binaire classificatie](media/machine-learning-evaluate-model-performance/5.png)

Afbeelding 5. Evaluatie van een Model binaire indeling.

###<a name="inspecting-the-evaluation-results"></a>De evaluatieresultaten controleren###
Na het uitvoeren van het experiment, kunt u klikken op de uitvoerpoort van het [Model evalueren] [ evaluate-model] module en selecteer *visualiseren* om te zien van de evaluatieresultaten (afbeelding 7). Beschikbaar voor binaire classificatie-modellen op de doelstellingen van de evaluatie zijn: *nauwkeurigheid*, *precisie* *intrekken*, *F1 Score*en *AUC*. De module uitvoer daarnaast een verwarring-matrix met het aantal waar positieve, ONWAAR negatieven, onrechte, en waar negatieven, evenals *ROC*, *Precisie/intrekken*en *til* bochten.

Nauwkeurigheid is het aantal exemplaren juist is ingedeeld. Het is gewoonlijk de eerste meetwaarde die u bekijkt om te bepalen of een classificatie. Wanneer de testgegevens is echter niet in balans (waar grootste deel van de exemplaren die deel uitmaakt van een van de klassen), of als u liever in de prestaties van beide van de klassen, vastleggen nauwkeurigheid niet echt de effectiviteit van een classificatie. In het niveau classificatie inkomsten scenario wordt ervan uitgegaan dat u wilt testen op sommige gegevens waarbij 99% van de exemplaren staan voor personen die kleiner dan of gelijk is aan 50K per jaar verdienen. Het is mogelijk om een 0.99 nauwkeurigheid door de klas voorspellen "< 50K =" naar alle exemplaren. De classificatie lijkt in dit geval worden goed bezig algehele, maar in feite niet aan een van de hoog inkomen personen (de 1%) correct classificeren.

Om die reden is het handig zijn om te berekenen van extra statistieken die specifiekere aspecten van de evaluatie vastleggen. Voordat u overschakelt op de details van deze doelstellingen, is het belangrijk om te begrijpen van de matrix verwarring van een evaluatie met binaire indeling. De etiketten class in de trainingsset kunnen voor alleen 2 mogelijke waarden die we meestal naar verwijzen uitvoeren als positief of negatief. De positieve en negatieve exemplaren die een classificatie voorspellen = correct worden waar positieve (TP) en waar ontkenningen (TN), respectievelijk genoemd. De verkeerde ingedeeld exemplaren worden op dezelfde manier onrechte (inch) en ONWAAR ontkenningen (FN) genoemd. De matrix verwarring is gewoon een tabel met het aantal exemplaren die onder elk van deze 4 categorieën vallen. Azure Machine Learning wordt automatisch bepaald welke van de twee klassen in de gegevensset de positieve klasse is. Als de labels van de klas zijn Booleaanse waarde of gehele getallen, wordt de 'waar' of '1' gelabelde exemplaren de positieve klas zijn toegewezen. Als de etiketten tekenreeksen zijn, zoals in het geval van de gegevensset inkomsten, de etiketten worden alfabetisch en het eerste niveau moeten de negatieve klas terwijl het tweede niveau de positieve klasse is wordt gekozen.

![Binaire indeling verwarring Matrix](media/machine-learning-evaluate-model-performance/6a.png)

Afbeelding 6. Binaire indeling verwarring Matrix.

Teruggaan naar het probleem van de classificatie inkomsten, zou willen we vraag verschillende evaluatievragen die help ons te weet wat de prestaties van de classificatie die wordt gebruikt. Een zeer natuurlijke vraag is: ' afmelden bij de personen aan wie het model voorspeld om te worden jaarlijks > 50 K (TP + inch), hoeveel zijn geclassificeerd correct (TP)?' Deze vraag kan worden beantwoord door te kijken de **precisie** van het model, namelijk de verhouding van positieve die correct zijn ingedeeld: TP/(TP+FP). Vraag is ' afmelden bij alle hoog jaarlijks werknemers met inkomsten > 50 k (TP + FN), hoeveel de classificatie classificeren correct (TP) ". Dit is daadwerkelijk de die **intrekken**, of het tarief weer dat waar positieve: TP/(TP+FN) van de classificatie. U mogelijk ziet dat er een duidelijke verhouding tussen precisie van formules en intrekken. Bijvoorbeeld, gegeven een relatief gebalanceerde gegevensset, een classificatie die voorspellen = hoofdzakelijk positief exemplaren, moet een hoge intrekken, maar een liever lage precisie zoals veel van de negatieve exemplaren zou worden verkeerd geclassificeerd met een groot aantal onrechte resultaat. Als u wilt zien van de tekening van hoe de doelstellingen van deze twee variëren, kunt u op de precisie/INTREKKEN curve in de uitvoer van evaluatie resultatenpagina (linksboven deel uit van de afbeelding 7).

![Binaire indeling evaluatieresultaten](media/machine-learning-evaluate-model-performance/7.png) afbeelding 7. Resultaten van de evaluatie binaire indeling.

Een andere gerelateerde metrisch die wordt vaak gebruikt is **F1 Score**, die zowel precisie van formules en intrekken rekening houdt. Dit is het harmonische gemiddelde van de doelstellingen van deze 2 en wordt berekend als zodanig: F1 = 2 (intrekken precisie x) / (precisie + intrekken). F1 score is een goede manier om samen te vatten van de evaluatie een enkel getal, maar het is altijd een goede gewoonte om te kijken naar zowel precisie van formules en intrekken samen om inzicht te krijgen in de werking van een classificatie.

Bovendien kan een controleren het tarief weer dat waar positieve versus het ONWAAR positieve tarief in de kromme **Ontvanger besturingsomgeving kenmerk (ROC)** en de bijbehorende **Gebied onder de Curve (AUC)** -waarde. Hoe dichter deze curve is naar de linkerbovenhoek verschijnt, hoe beter de prestaties van de classificatie (die is het optimaliseren van het tarief weer dat waar positieve terwijl het tarief weer dat ONWAAR positieve geminimaliseerd). Bochten die lijken op de diagonaal van de tekening, de resultaten van classificaties die doorgaans voorspellingen die lijken op willekeurig raden.

###<a name="using-cross-validation"></a>Met kruisverwijzingen gegevensvalidatie###
Zoals in het voorbeeld regressie, kunnen we cross validatie herhaaldelijk training, score en verschillende subsets van de gegevens automatisch evalueren uitvoeren. Daarnaast kunt u gebruiken het [Model kruislings gevalideerd] [ cross-validate-model] module, een model ongetrainde logistische regressie en een gegevensset. De labelkolom moet zijn ingesteld op *inkomsten* in het [Model kruislings gevalideerd] [ cross-validate-model] eigenschappen van de module. Na het experiment uitgevoerd en te klikken op de juiste uitvoerpoort van het [Model kruislings gevalideerd] [ cross-validate-model] module, kunnen we de binaire indeling metrieke waarden voor elke gevouwen, Daarnaast zien aan het gemiddelde en de standaarddeviatie van de verschillende. 
 
![Een Model binaire indeling cross-valideren](media/machine-learning-evaluate-model-performance/8.png)

Figuur 8. Cross-een binaire classificatie-Model valideren.

![Cross-validatie resultaten van een binaire classificatie](media/machine-learning-evaluate-model-performance/9.png)

Afbeelding 9. Cross-validatie de resultaten van een binaire classificatie.

##<a name="evaluating-a-multiclass-classification-model"></a>Evaluatie van een Model Multiclass classificatie##
In dit experiment gebruiken we de populaire [Pascaline](http://archive.ics.uci.edu/ml/datasets/Iris "Pascaline") gegevensset die exemplaren van 3 verschillende typen (klassen) van de plant Pascaline bevat. Er zijn 4 functie waarden (sepal lengte/breedte en Bloemblad lengte/breedte) voor elk exemplaar. In de vorige experimenten we training en het gebruik van de dezelfde gegevenssets modellen getest. Hier we gebruiken de [Gesplitste gegevens] [ split] module 2 subsets van de gegevens maken, trainen op de eerste, en score en klik op het tweede evalueren. De gegevensset Pascaline algemeen beschikbaar is in de [UCI Machine Learning-bibliotheek](http://archive.ics.uci.edu/ml/index.html)en kan worden gedownload met een [Importgegevens] [ import-data] module.

###<a name="creating-the-experiment"></a>Maken van het Experiment###
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

- [Gegevens importeren][import-data]
- [Multiclass beslissing bos][multiclass-decision-forest]
- [Gesplitste gegevens][split]
- [Trein Model][train-model]
- [Score-Model][score-model]
- [Model evalueren][evaluate-model]

Verbinding maken met de poorten zoals hieronder wordt weergegeven in afbeelding 10.

Stel de kolomindex Label van het [Model trein] [ train-model] module tot en met 5. De gegevensset heeft geen veldnamenrij, maar we weten dat de labels van de klas in de vijfde kolom zijn.

Klik op de [Gegevens importeren] [ import-data] module en stel de eigenschap *gegevensbron* op *Web-URL via HTTP*en de *URL* voor http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Stel het deel van de exemplaren die moet worden gebruikt voor training in de [Gesplitste gegevens] [ split] module (0,7 bijvoorbeeld).
 
![Evaluatie van een Multiclass classificatie](media/machine-learning-evaluate-model-performance/10.png)

Afbeelding 10. Evaluatie van een Multiclass classificatie

###<a name="inspecting-the-evaluation-results"></a>De evaluatieresultaten controleren###
Het experiment uitvoeren en klik op de uitvoerpoort van [Evalueren Model][evaluate-model]. De evaluatieresultaten worden weergegeven in de vorm van een matrix verwarring in dit geval. De matrix geeft de werkelijke waarden versus voorspelde exemplaren voor alle 3 klassen.
 
![Multiclass classificatie evaluatieresultaten](media/machine-learning-evaluate-model-performance/11.png)

Afbeelding 11. Resultaten van de evaluatie multiclass classificatie.

###<a name="using-cross-validation"></a>Met kruisverwijzingen gegevensvalidatie###
Zoals eerder is vermeld, u kunt uitvoeren herhaalde training, scoren en evaluaties automatisch met het [Model kruislings gevalideerd] [ cross-validate-model] module. U moet een gegevensset, een ongetrainde model en een [Model valideren van meerdere] [ cross-validate-model] module (Zie onderstaande afbeelding). U moet opnieuw instellen van de labelkolom van het [Model kruislings gevalideerd] [ cross-validate-model] module (kolomindex 5 in dit geval). Nadat het experiment uitgevoerd en op te klikken rechts uitvoer poort van het [Model kruislings gevalideerd][cross-validate-model], controleert u de metrische waarden voor elke gevouwen, evenals de standaarddeviatie van de gemiddelde en de standaardversie. Het hier weergegeven aan de doelstellingen zijn de vergelijkbaar met de die zijn besproken in de binaire indeling hoofdletters/kleine letters. Bedenk wel dat in multiclass indeling, de waar positieve/gebruikgemaakt van negatieven en ONWAAR positieve/negatieven computing wordt uitgevoerd door tellen op basis van het per class, aangezien er geen algemene positief of negatief class. Bijvoorbeeld bij het berekenen van de precisie van formules of intrekken van de klasse 'Pascaline-setosa', wordt uitgegaan dat dit de positieve klasse en alle andere negatief is.
 
![Een Model Multiclass classificatie cross-valideren](media/machine-learning-evaluate-model-performance/12.png)

Figuur 12. Cross-een Multiclass classificatie-Model valideren.


![Cross-validatie resultaten van een Model Multiclass classificatie](media/machine-learning-evaluate-model-performance/13.png)

Afbeelding 13. Cross-validatie resultaten van een Multiclass classificatie-Model.


<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/
 
