<properties
    pageTitle="Het maken van de tekst analytics-modellen in Azure Machine Learning Studio | Microsoft Azure"
    description="Tekst in Azure Machine Learning Studio met modules voor voorverwerking van tekst, N-g of functie hashing analytics-modellen maken"
    services="machine-learning"
    documentationCenter=""
    authors="rastala"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="roastala" />


#<a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Het maken van de tekst analytics-modellen in Azure Machine Learning Studio

U kunt Azure Machine Learning bouwen en tekst analytics-modellen mogelijk te maken. Deze modellen kunt u, bijvoorbeeld document classificatie of sentiment analyse problemen oplossen.

In een experiment tekst analyses zou u meestal:

 1. Wissen en vooraf verwerken tekst gegevensset
 2. Numerieke functie vectoren extraheren uit vooraf verwerkte tekst
 3. Trein classificatie of regressie model
 4. Score en het model valideren
 5. Het model implementeert productie

In deze zelfstudie leert u deze stappen als we begeleiden tot en met een sentiment analyse model met Amazon adresboek beoordelingen gegevensset (Zie dit artikel onderzoek "biografieën, Bollywood, giek-vakken en Blenders: domein aanpassing voor Sentiment classificatie ' door John Blitzer, markeren Dredze en Fernando Pereira; Koppeling van rekenkundige taalkundige (ACL), 2007.) Deze dataset bestaat uit controleren scores (1-2 of 4 en 5) en de tekst van een vrije vorm. Het doel is om te controleren score voorspellen: laag (1 - 2) of van hoog (4 en 5).

U kunt experimenten waarvoor in deze zelfstudie aan de galerie met Cortana bedrijfsinformatie kunt vinden:

[Voorspellen adresboek beoordelingen] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Voorspellen adresboek beoordelingen - blog Experiment] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Stap 1: Wissen en vooraf verwerken tekst gegevensset

We eerst het experiment verdelen van de controle-scores in bestaan gerangschikte verzamelingen de onder- en bovendrempel aan het opstellen van het probleem als twee-klasse classificatie. We gebruiken [metagegevens bewerken] (https://msdn.microsoft.com/library/azure/dn905986.aspx) en [groep bestaan Values] (https://msdn.microsoft.com/library/azure/dn906014.aspx) modules.

![Label maken](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Vervolgens opschonen van de tekst gebruik [tekst vooraf verwerken] (https://msdn.microsoft.com/library/azure/mt762915.aspx)-module. Het wassen Hiermee reduceert u het geluid in de gegevensset, kunt u informatie over de belangrijkste functies en de nauwkeurigheid van het definitieve model te verbeteren. We verwijderen stopwords - veelgebruikte woorden zoals 'de' of 'een' - en getallen, speciale tekens, gedupliceerde tekens, e-mailadressen en URL's. We ook de tekst naar kleine letters, de woorden lemmatize converteren en detecteren zin grenzen die vervolgens worden aangegeven met "|||" symbool in vooraf verwerkte tekst.

![Vooraf verwerken van tekst](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Wat gebeurt er als u wilt een aangepaste lijst met stopwords gebruiken? U kunt in doorgeven als optionele invoer. U kunt ook aangepaste C# syntaxis normale expressie gebruiken voor het vervangen van subreeksen en woorden verwijderen door spraak: zelfstandige naamwoorden, woorden of bijvoeglijke naamwoorden gedefinieerd.

Nadat de voorverwerking voltooid is, wordt het splitsen van de gegevens in de trein en sets testen.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Stap 2: Numerieke functie vectoren extraheren uit vooraf verwerkte tekst

Als u wilt een model voor tekstgegevens samenstelt, moet u meestal vrije vorm tekst converteren naar numerieke functie vectoren. In dit voorbeeld gebruiken we [extraheren N g functies uit een tekst]-module (https://msdn.microsoft.com/library/azure/mt762916.aspx) om de tekstgegevens in deze indeling te transformeren. Deze module wordt een kolom van woorden witruimte gescheiden en berekent een woordenlijst van woorden of N-g van woorden, dat wordt weergegeven in uw gegevensset. Vervolgens telt deze het aantal keren voor elk word of N g, wordt weergegeven in elke record en maakt een functie vectoren van die worden geteld. In deze zelfstudie stellen we N g grootte op 2, zodat onze vectoren functie enkele woorden en combinaties van de twee volgende woorden opnemen.

![N-g extraheren](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

We TF toepassen * IDF (Term frequentie Inverse Document frequentie) weging naar N g worden geteld. Deze methode voegt de dikte van de woorden die vaak worden weergegeven in één record, maar niet vaak voorkomen in de hele gegevensset. Andere opties binary, TF, opnemen en grafische wegen.

Deze tekstfuncties hebben vaak hoog dimensionaliteit. Als uw corpus 100.000 unieke woorden heeft, zou uw functie ruimte bijvoorbeeld 100.000 dimensies of meer als N-g worden gebruikt. De module extraheren N g functies kunt u een reeks opties verkleinen van de dimensionaliteit. U kunt kiezen voor woorden die korte of lange, ook niet vaak voorkomende of te veelgebruikte aanzienlijk blog waarde uitsluiten. In deze zelfstudie uitsluiten we N-g die worden weergegeven in minder dan 5 records of in meer dan 80% van records.

Bovendien kunt u functies selecteren om te selecteren van alleen de functies die het meest gecorreleerde met het doel-tekstvoorspelling zijn. We Functieselectie van de chi-kwadraatverdeling gebruiken om 1000 onderdelen te selecteren. U kunt de woordenlijst van geselecteerde woorden of N-g weergeven door te klikken op de juiste uitvoer van uitpakken N-g module.

Als het alternatieve gaat over het gebruik van functies van extraheren N g, kunt u de functie Hashing module gebruiken. Houd er rekening mee Hoewel die [functie Hashing] (https://msdn.microsoft.com/library/azure/dn906018.aspx) geen ingebouwde functie selectie mogelijkheden, of TF heeft * IDF wegen.

## <a name="step-3-train-classification-or-regression-model"></a>Stap 3: Trainen classificatie of regressie-model

Nu zijn de tekst naar kolommen numerieke functie getransformeerd. De gegevensset bevat nog steeds tekenreeks kolommen uit de vorige fases, zodat we kolommen selecteren in de gegevensset uitsluiten ze gebruiken.

We vervolgens [twee-klasse logistische regressie] (https://msdn.microsoft.com/library/azure/dn905994.aspx) om te voorspellen ons doel gebruiken: hoge of lage controleren score. Op dit moment zijn het tekst analytics-probleem een probleem normale classificatie getransformeerd. U kunt de hulpmiddelen die beschikbaar zijn in Azure Machine Learning het model te verbeteren. U kunt bijvoorbeeld experimenteren met verschillende classificaties om vast te stellen hoe nauwkeurige resultaten ze geven, of gebruik hyperparameter optimaliseren om de nauwkeurigheid te verbeteren.

![Trein en Score](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Stap 4: Score en het model valideren

Hoe wilt u het ervaren model valideren? We dit ten opzichte van de gegevensset test score en de nauwkeurigheid evalueren. Echter de woordenlijst van N-g en het gewicht van de gegevensset training weet hoe u het model. Daarom, moeten we die woordenlijst en deze lijndikten gebruiken bij het ophalen van de functies van testgegevens, in plaats van de woordenlijst opnieuw maken. Daarom we extraheren N g functies module toevoegen aan de score tak van het experiment, sluit de uitvoer begrippen op training tak, en stel de woordenlijst-modus op alleen-lezen. We ook uitschakelen het filteren van N-g op frequentie door in te stellen van de minimum 1 exemplaar en maximaal 100%, en de Functieselectie uitschakelen.

Nadat de kolom in testgegevens zijn getransformeerd naar functie voor numerieke kolommen, uitsluiten we de kolommen tekenreeks van de vorige stadia zoals in training tak. We gebruiken Score Model module voorspellingen en Model evalueren module de nauwkeurigheid wordt berekend.

## <a name="step-5-deploy-the-model-to-production"></a>Stap 5: Het model implementeert productie

Het model is bijna klaar om te worden geïmplementeerd op productie. Wanneer geïmplementeerd als webservice, deze tekenreeks van de tekst die als invoer en retourneren een voorspelling "hoge" of "lage." De geleerde N g begrippen gebruikt om de tekst tot functies en ervaren logistische regressiemodel om een voorspelling van deze functies te transformeren. 

Als u wilt het blog experiment hebt ingesteld, opslaan we eerst uit in de woordenlijst N g als gegevensset en het model ervaren logistische regressie de tak training van het experiment. We Sla vervolgens het maken van een experiment graph voor blog experiment met 'OpslaanAls' experiment. We verwijderen de module gesplitste gegevens en de tak training uit het experiment. We vervolgens verbinden met de eerder opgeslagen N g woordenlijst en het model extraheren N g functies en Score Model modules, respectievelijk. We verwijderen de module evalueren Model ook.

We kolommen selecteren in de module gegevensset voordat u de module vooraf verwerken tekst verwijderen van de labelkolom invoegen, en de optie "Score kolom toevoegen aan de gegevensset" in Score Module deselecteren. Op deze manier de webservice vraagt niet om het label probeert te voorspellen en doet niet echo functies van de invoer in antwoord.

![Bekijk Experiment](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Nu hebben we een experiment die kan worden gepubliceerd als een webservice en aangeroepen met behulp van de aanvraag en respons of batch execution API's.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over tekst analytics modules uit [MSDN-documentatie] (https://msdn.microsoft.com/library/azure/dn905886.aspx).
