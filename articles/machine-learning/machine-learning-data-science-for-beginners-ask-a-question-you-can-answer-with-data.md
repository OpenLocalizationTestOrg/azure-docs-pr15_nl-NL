<properties
   pageTitle="Stel een vraag kunt u beantwoorden met gegevens - opstellen vragen | Microsoft Azure"
   description="Informatie over het opstellen van een vraag in een wetenschappelijke gegevens in de wetenschappelijke gegevens voor Beginners video 3. Een vergelijking van de indeling en regressie vragen bevat."
   keywords="gegevens wetenschappelijke vragen, vragen, regressie vragen, classificatievragen opstellen, scherpe vraag"
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

# <a name="ask-a-question-you-can-answer-with-data"></a>Stel een vraag die u met gegevens beantwoorden kunt

## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Gegevens wetenschappelijke voor Beginners reeks

Informatie over het opstellen van een vraag in een wetenschappelijke gegevens in de wetenschappelijke gegevens voor Beginners video 3. Deze video bevat een vergelijking van vragen voor classificatie en regressie algoritmen.

Als u optimaal gebruikmaken van de reeks, bekijkt u deze alle. [Ga naar de lijst met video 's](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-ask-a-question-you-can-answer-with-data]

## <a name="other-videos-in-this-series"></a>Andere video's in deze reeks

*Gegevens wetenschappelijke voor Beginners* is een snelle introductie over gegevens wetenschappelijke in vijf korte video's.

  * Video 1: [De antwoorden van 5 vragen gegevens wetenschappelijke](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*
  * Video 2: [uw gegevens gereed Is voor gegevens wetenschappelijke?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
  * Video 3: Stel een vraag die u met gegevens beantwoorden kunt
  * Video 4: [Een antwoord met een eenvoudige model voorspellen](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*
  * Video 5: [Werk van anderen om uit te voeren gegevens wetenschappelijke kopiëren](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Transcriptie: Stel een vraag die u met gegevens beantwoorden kunt

Welkom bij de derde video in de reeks "Gegevens wetenschappelijke voor Beginners."  

In dit voorbeeld krijgt u enkele tips voor het opstellen van een vraag die u met gegevens beantwoorden kunt.

U krijgt meer uit deze video als u eerst de twee eerdere video's in deze reeks bekijken: "de wetenschappelijke van de gegevens 5 vragen kan beantwoorden" en "Is uw gegevens gereed is voor gegevens wetenschappelijke?"

## <a name="ask-a-sharp-question"></a>Stel een vraag scherpe

We hebben gesproken over hoe gegevens wetenschappelijk het proces is van het gebruik van namen (ook wel categorieën of labels genoemd) en getallen te voorspellen een antwoord op een vraag. Maar niet elke vraag; Er moet een *scherpe vraag.*

Een vaag vraag hoeft te worden beantwoord met een naam of een getal. Er moet een scherpe vraag.

Stel dat u een magische licht met een genie die wordt elke vraag die u vragen naar waarheid beantwoorden gevonden. Maar het is een mischievous genie en hij wordt probeert iets te zijn answer als vaag en duidelijker is als hij afwezig met ophalen kunt. U wilt vastmaken hem omlaag met een vraag dus luchtdichte dat niet kan hij help maar zien wat u wilt weten.

Als u een vraag stellen vaag, zoals "Wat er gebeurt met mijn voorraad veroorzaken?", de genie kan beantwoorden, "is de prijs verandert". Dat is een ware antwoord, maar het is niet erg handig.

Maar als u dit naar een vraag stellen scherpe, zoals "Wat zijn de verkoopprijs van mijn aandeel volgende week?", de genie kan geen help, maar kunt u een specifiek beantwoord en een verkoopprijs voorspellen.

## <a name="examples-of-your-answer-target-data"></a>Voorbeelden van uw antwoord: afstemmen van gegevens

Nadat u uw vraag opstellen, controleert u of u voorbeelden van het antwoord in uw gegevens hebt.

Als onze vraag "Wat de verkoopprijs van mijn aandeel is volgende week?" we hebt om ervoor te zorgen dat onze gegevens bevatten de Geschiedenis aandelenkoers.

Als u onze vraag is "welke auto in mijn vloot dient eerst mislukt?" we hebt om ervoor te zorgen dat onze gegevens bevatten informatie over eerdere fouten.

![Gegevens - voorbeelden van uw antwoord afstemmen. Een vraag in een wetenschappelijke gegevens opstellen.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-target-data.png)

In deze voorbeelden van de antwoorden worden een doel genoemd. Een doel is wat we probeert te voorspellen over toekomstige gegevenspunten, of u nu een categorie of een getal.

Als u geen doelgegevens hebt, moet u enkele ophalen. Niet mogelijk om uw vraag zonder deze te beantwoorden.

## <a name="reformulate-your-question"></a>Uw vraag reformulate

Soms kunt u uw vraag om een gebruiksvriendelijker antwoord uitgelegd.

De vraag "Is deze gegevens punt A of B?" voorspellen = de categorie (of de naam of het label) van een ander nummer. Om te reageren, gebruiken we een *algoritme van de indeling*.

De vraag "Hoeveel?" of 'Hoe veel?' worden voorspeld een bedrag. We gebruiken een *regressie algoritme*om te reageren.

Als u wilt zien hoe we deze kunt transformeren, eens kijken naar de vraag, "welke nieuwsbericht is het meest interessant voor deze lezer?" Dit wordt gevraagd om een voorspelling van één keuze uit vele mogelijkheden - met andere woorden "Is dit A of B of C of D?" - en een classificatie-algoritme wilt gebruiken.

Maar het is mogelijk dat deze vraag beter te beantwoorden als u uitgelegd als "hoe interessante is elk artikel in deze lijst voor deze lezer?" Nu kunt u geven elk artikel een numerieke score en wordt het gemakkelijk kunt identificeren van het artikel hoogste scores. Dit is een formuleren van de classificatie vraag in een regressie vraag of hoeveel?

![Uw vraag reformulate. Classificatie vraag versus regressie vraag.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-classification-question-vs-regression-question.png)

Hoe u een vraag is een aanwijzing op welke algoritme van de vraag krijgt u een antwoord.

U vindt dat bepaalde gezinnen algoritmen - zoals de knoppen in ons voorbeeld van het verhaal nieuws - nauw zijn gerelateerd. U kunt uw vraag als u wilt gebruiken het algoritme waarmee u het nuttigste antwoord reformulate.

Maar, belangrijkste, vraag die scherpe - de vraag die u met gegevens beantwoorden kunt. En zorg ervoor dat u de juiste gegevens antwoord hebt.

We hebben gesproken over enkele basisprincipes voor een vraag die u kunt beantwoorden met gegevens.

Zorg ervoor dat de andere video's in "gegevens wetenschappelijke voor Beginners' van Microsoft Azure Machine Learning uitchecken.


## <a name="next-steps"></a>Volgende stappen

  * [Probeer een eerste gegevens wetenschappelijk experiment met Machine Learning Studio](machine-learning-create-experiment.md)
  * [Bekijk de inleiding voor Machine Learning op Microsoft Azure](machine-learning-what-is-machine-learning.md)
