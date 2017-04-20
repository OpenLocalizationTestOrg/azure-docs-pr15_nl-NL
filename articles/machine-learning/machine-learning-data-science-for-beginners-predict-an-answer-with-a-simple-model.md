<properties
   pageTitle="Een antwoord met een eenvoudige model - regressiemodel voorspellen | Microsoft Azure"
   description="Het maken van een eenvoudige regressiemodel te voorspellen wat een prijs in de wetenschappelijke gegevens voor Beginners video 4. Een lineaire regressie met doelgegevens bevat."                                  
   keywords="een model, eenvoudige model, prijs tekstvoorspelling, eenvoudige regressiemodel maken"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>

# <a name="predict-an-answer-with-a-simple-model"></a>Een antwoord met een eenvoudige model voorspellen

## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Gegevens wetenschappelijke voor Beginners reeks

Informatie over het maken van een eenvoudige regressiemodel te voorspellen wat de prijs van een ruit in wetenschappelijke gegevens voor Beginners video 4. We wordt een regressiemodel met gegevens van de doellijst tekenen.

Als u optimaal gebruikmaken van de reeks, bekijkt u deze alle. [Ga naar de lijst met video 's](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-predict-an-answer-with-a-simple-model]

## <a name="other-videos-in-this-series"></a>Andere video's in deze reeks

*Gegevens wetenschappelijke voor Beginners* is een snelle introductie over gegevens wetenschappelijke in vijf korte video's.

  * Video 1: [De antwoorden van 5 vragen gegevens wetenschappelijke](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*
  * Video 2: [uw gegevens gereed Is voor gegevens wetenschappelijke?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
  * Video 3: [Stel een vraag die u kunt beantwoorden met gegevens](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*
  * Video 4: Een antwoord met een eenvoudige model voorspellen
  * Video 5: [Werk van anderen om uit te voeren gegevens wetenschappelijke kopiëren](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Transcriptie: Een antwoord met een eenvoudige model voorspellen

Welkom bij de vierde video in de "gegevens wetenschappelijke voor Beginners" reeks. In dit voorbeeld, we maken van een eenvoudige model en breng een voorspelling.

Een *model* is een eenvoudigere verhaal over onze gegevens. Ik ziet u wat ik betekenen.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Verzamelen relevante, nauwkeurig, verbonden, voldoende gegevens

Stel dat ik wil winkelen voor een ruit. Ik heb een belsignaal die tot mijn oma met een instelling voor een ruit 1.35 karaat behoorden, maar ik wil een idee van hoe het kost ophalen. Ik een Kladblok- en Penknoppen nemen bij de winkel juwelen en ik Noteer de prijs van alle de ruiten in de hoofdletters/kleine letters en hoeveel ze wegen in carats. Beginnen met het eerste ruitje - de 1.01 carats en $7,366.

Nu ik gaat doorlopen en deze stap herhalen voor alle andere ruiten in de winkel.

![Kolommen met gegevens van de ruit](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

U ziet dat onze lijst met twee kolommen. Elke kolom heeft een ander kenmerk,-dikte in carats en prijs- en elke rij is één gegevenspunt met een enkel ruit.

We hebben een klein daadwerkelijk hebt gemaakt gegevens Stel hier - een tabel. U ziet dat ze voldoen aan onze criteria voor kwaliteit:

* De gegevens is **relevante** - gewicht is groeiende gerelateerd aan de prijs
* **Deze klopt** - we de prijzen die we Noteer goed gecontroleerd
* Het is **verbonden** : Er zijn geen spaties in een van deze kolommen
* En, zoals we ziet, is **genoeg** gegevens om onze vraag te beantwoorden

## <a name="ask-a-sharp-question"></a>Stel een vraag scherpe

Nu we onze vraag wordt vormen in een scherpe manier: "hoeveel kost het kopen van een ruit 1.35 karaat?"

Onze lijst geen een ruit 1.35 karaat in, zodat we de rest van ons gegevens gebruiken om een antwoord op de vraag hebt.

## <a name="plot-the-existing-data"></a>De bestaande gegevens uitzetten

Het eerste wat dat we doen is een horizontale lijn getal genoemd as, als u wilt de lijndikten in een grafiek tekenen. Het bereik van de lijndikten is 0 tot en met 2, zodat we een lijn die betrekking heeft op dat bereik en zet maatstreepjes voor elke halve karaat wordt tekenen.

Vervolgens gaat we een verticale as als u wilt opnemen van de prijs en maak verbinding met de dikte van horizontale as tekenen. Dit is in eenheden van dollars. Nu hebben we een reeks coördinaten assen.

![Gewicht en prijs assen](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

We gaan nu uitvoeren van deze gegevens en u deze omzetten in een *spreidings uitzetten*. Dit is een uitstekende manier om numerieke gegevenssets visualiseren.

Voor het eerste gegevenspunt krijgen we een verticale lijn op 1.01 carats. Klik, wordt een horizontale lijn op $7,366 krijgen. Waar ze overeenkomen met, tekent we een punt. Hiermee wordt onze eerste ruit.

Nu we leest u over elke ruit in deze lijst voorkomt en hetzelfde te doen. Als we tot en met, dit is wat we ophalen: een reeks stippen, één voor elke ruit.

![Spreiding tekenen](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Het model door de gegevenspunten tekenen

Als u de puntjes en squint bekijken, de verzameling ziet er nu zo een fat, bij benadering lijn. We kunnen maken van onze markering en tekent een rechte lijn door de werkmap.

Een lijn wilt tekenen, we een *model*gemaakt. Beschouw als de echte wereld duurt en een eenvoudig getekende-versie van deze te maken. Nu de getekende is er mis: de regel niet gaat u door de gegevenspunten. Maar het is een handige vereenvoudigen.

![Lineaire regressielijn](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Het feit dat alle de puntjes gaan niet precies door de regel is het OK. Gegevens wetenschappers wordt uitgelegd dat dit door op te zeggen dat er het model - die de regel - en klikt u vervolgens elke stip heeft enkele *ruis* of de *variantie* die is gekoppeld aan dit. Er is de onderliggende perfect relatie en is de ingaan, echte wereld die worden toegevoegd ruis en onzekerheid.

Omdat we probeert om de vraag te beantwoorden *hoeveel?* heet dit een *regressie*. En omdat we een rechte lijn gebruikt, is het een *lineaire regressie*.

## <a name="use-the-model-to-find-the-answer"></a>Het model gebruiken om het antwoord te vinden

Nu we een model hebben en we onze vraag vragen: hoeveel kost een ruit 1.35 karaat?

Als u wilt beantwoorden onze vraag, we krijgen van 1.35 carats en een verticale lijn tekenen. Waar deze de modelregel kruist krijgen we een horizontale lijn aan de euro-as. Dit via rechts onder 10.000. Giek! Dat is het antwoord: een ruit 1.35 karaat kosten ongeveer $10.000.

![Zoek het antwoord op het model](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Een betrouwbaarheidsinterval maken

Het is vanzelfsprekend meer wilt weten hoe nauwkeurig deze tekstvoorspelling zijn. Het is handig om te weten of het ruitje 1.35 karaat zeer dicht bij $10.000 wordt, of veel hoger of lager. Als u wilt weten welke, tekent u laten we een envelop rond de regressielijn dat meestal de puntjes bevat. Deze envelop heet onze *betrouwbaarheidsinterval*: we heel achter dat prijzen binnen deze envelop vallen, omdat in de afgelopen optimaal gebruik van deze hebben. We kunnen twee meer horizontale lijnen tekenen uit waar de lijn 1.35 karaat boven en de onderkant van de envelop kruist.

![Betrouwbaarheidsinterval](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Nu we iets over onze betrouwbaarheidsinterval zeggen kunnen: We kunt vertrouwen Stel de prijs van een ruit 1.35 karaat is bijna $10.000 -, maar het is mogelijk zo laag $ 8000 man en kan het zijn zo hoog fl 12.000.

## <a name="were-done-with-no-math-or-computers"></a>We klaar bent, zonder wiskundige of computers

We hebben gedaan welke gegevens wetenschappers uitbetaald moet doen en we hebben gedaan NET door te tekenen:

* We een vraag gesteld dat we met gegevens beantwoorden kan
* We een *model* met *lineaire regressie* ingebouwd
* We een *tekstvoorspelling*hebt aangebracht, met een *betrouwbaarheidsinterval*

En we niet wiskundige of computers gebruiken om te doen.

Nu als we gehad meer informatie, graag...

* het knippen van het ruitje
* kleurvariaties (hoe dicht het ruitje is om te worden witte)
* het aantal opgenomen bedrijven in het ruitje

.. .waarna we had meer kolommen. In dat geval wordt de wiskundige handig. Als er meer dan twee kolommen, is moeilijk te tekenen punten op papier. De wiskundige kunt u deze regel of dit vlak met de gegevens van zeer aangepast passen.

Ook als in plaats van alleen een klein aantal ruiten, hebben we twee duizend of twee miljoen en vervolgens kunt u die kunnen worden geopend veel sneller doen met een computer.

Tegenwoordig kunnen wij hebben gesproken hierover lineaire regressie en we een voorspelling gebruik van gegevens die zijn aangebracht.

Zorg ervoor dat de andere video's in "gegevens wetenschappelijke voor Beginners' van Microsoft Azure Machine Learning uitchecken.



## <a name="next-steps"></a>Volgende stappen

  * [Probeer een eerste gegevens wetenschappelijk experiment met Machine Learning Studio](machine-learning-create-experiment.md)
  * [Bekijk de inleiding voor Machine Learning op Microsoft Azure](machine-learning-what-is-machine-learning.md)
