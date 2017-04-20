<properties
    pageTitle="Taken voor het voorbereiden van gegevens voor verbeterde machine learning | Microsoft Azure"
    description="Vooraf verwerken en gegevens voorbereiden machine learning opschonen."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Taken voor het voorbereiden van gegevens voor verbeterde machine learning

Voorverwerking en opschonen van gegevens zijn belangrijke taken die meestal moeten worden uitgevoerd voordat gegevensset effectief kan worden gebruikt voor machine learning. Onbewerkte gegevens is vaak ruis en onbetrouwbare en waarden ontbreekt. Met behulp van dergelijke gegevens voor modellering kan misleidende resultaten opleveren. Deze taken deel uitmaken van het Team gegevens wetenschap proces (TDSP) en volgt u meestal eerste verkennen van een gegevensset gebruikt om te detecteren en plannen van de oude verwerking vereist. Zie voor meer gedetailleerde instructies over het proces TDSP, de stappen in het [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Oude verwerking en opschonen van taken, zoals de taak van de gegevens verkennen, kunnen worden uitgevoerd in een groot aantal omgevingen, zoals SQL of component of Azure Machine Learning Studio en met verschillende hulpmiddelen en talen, zoals R of Python, afhankelijk van waar uw gegevens worden opgeslagen en waarop deze is opgemaakt. Aangezien TDSP iteratieve van aard, kunnen deze taken plaatsvinden op verschillende stappen in de werkstroom van het proces.

In dit artikel maakt u kennis met verschillende gegevensverwerking concepten en taken die kunnen worden uitgevoerd voor of na de gegevens in Azure Machine Learning ingesting.

Zie voor een voorbeeld van gegevens verkennen en oude verwerking klaar binnen Azure Machine Learning studio, de video [voorverwerking gegevens in Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) .


## <a name="why-pre-process-and-clean-data"></a>Waarom vooraf worden verwerkt en opschonen van gegevens?

Echte wereld gegevens uit verschillende bronnen zijn verzameld en processen en deze onregelmatigheden of beschadigde gegevens verlies van de kwaliteit van de gegevensset kunnen bevatten. De normale kwaliteitsproblemen die zich voordoen zijn:

* **Onvolledig**: gegevens kenmerken of met ontbrekende waarden ontbreekt.
* **Noisy**: gegevens bevat foutieve records of uitschieters.
* **Inconsistente**: gegevens bevatten conflicterende records of afwijkingen.

Kwaliteitsgegevens is vereist voor de blog modellen kwaliteit. Als u wilt voorkomen "ongewenste in ongewenste out" en kwaliteit van de gegevens te verbeteren en daarom model prestaties, is het noodzakelijk om te leiden van een scherm van de servicestatus gegevens om te vroeg gegevensproblemen opsporen en besluit op de bijbehorende gegevensverwerking en opschonen stappen.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Wat zijn enkele typische gegevens systeemstatus schermen die worden gebruikt?

We kunt de algemene kwaliteit van gegevens controleren door te schakelen:

* Het aantal **records**.
* Het aantal **kenmerken** (of **functies**).
* Het kenmerk **gegevenstypen** (nominale, rangtelwoord of doorlopend).
* Het aantal **waarden ontbreken**.
* **-Opmaak** van de gegevens.
    * Als de gegevens zich in TSV- of CSV-, controleert u dat de kolom scheidingstekens en lijn scheidingstekens altijd correct scheidt u kolommen en lijnen.
    * Als de gegevens zich in de HTML- of XML-indeling, controleert u of de gegevens is juist samengesteld op basis van de desbetreffende standaarden.
    * Parseren van ook mogelijk om te kunnen gestructureerde gegevens ophalen van gedeeltelijk gestructureerd of ongestructureerde gegevens.
* **Inconsistente gegevensrecords**. Controleer het bereik van waarden zijn toegestaan. bijvoorbeeld als de gegevens studenten Cijfergemiddelde, Controleer bevatten of het Cijfergemiddelde in de reeks is, zeg 0 ~ 4.

Wanneer u problemen met de gegevens hebt gevonden, **processing stappen** nodig zijn die vaak heeft betrekking op het opruimen ontbrekende waarden, gegevens normaliseren, afzondering, tekst verwerkt als u wilt verwijderen en/of ingesloten tekens die invloed kunnen zijn op gegevensuitlijning vervangen, gemengde gegevenstypen gemeenschappelijke velden en anderen.

**Azure Machine Learning verbruikt juist opgemaakte tabelgegevens**.  Als de gegevens zich al in de tabelweergave, kan oude gegevensverwerking rechtstreeks met Azure Machine Learning in de Machine Learning-Studio worden uitgevoerd.  Als gegevens niet in de tabelweergave, zeg in XML-bestand is, is parseren mogelijk nodig om te kunnen de gegevens omzetten in tabelvorm.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Wat zijn enkele van de belangrijkste taken in oude gegevensverwerking?

* **Opschonen van gegevens**: invullen of ontbrekende waarden, detecteren in en ruis en uitschieters verwijderen.
* **Gegevenstransformatie**: gegevens om te beperken dimensies en ruis normaliseren.
* **Gegevens wilt verkleinen**: voorbeeld van de records met gegevens of kenmerken voor gemakkelijker gegevensverwerking.
* **Gegevens afzondering**: doorlopend kenmerken converteren naar bestaan kenmerken voor gebruiksgemak met bepaalde machine learning-methoden.
* **Tekst opschonen**: Verwijder ingesloten tekens die mogelijk gegevens in de uitlijning, bijvoorbeeld ingesloten tabs in een door tabs gescheiden gegevensbestand, ingesloten voor nieuwe regels die mogelijk verbreken records, enzovoort.

De volgende secties bevatten informatie over enkele stappen gegevensverwerking.

## <a name="how-to-deal-with-missing-values"></a>Het omgaan met ontbrekende waarden?

Als u wilt omgaan met ontbrekende waarden, is het beste eerst het probleem identificeren de reden voor de ontbrekende waarden beter wordt verwerkt. Normale ontbrekende waarde afhandeling methoden zijn:

* **Verwijdering**: records met ontbrekende waarden verwijderen
* **Pop-vervanging**: ontbrekende waarden vervangen door een pop-waarde: bijvoorbeeld, _Onbekend_ voor bestaan of 0 voor numerieke waarden.
* **Vervanging betekenen**: als de ontbrekende gegevens numerieke is, vervangt u de ontbrekende waarden door het gemiddelde.
* **Veelgebruikte vervangen**: als de ontbrekende gegevens bestaan, de ontbrekende waarden vervangen met de meest voorkomende item
* **Regressie vervanging**: een regressiemethode gebruiken om te ontbrekende waarden vervangen door waarden opgelost.  

## <a name="how-to-normalize-data"></a>Hoe gegevens normaliseren?

Gegevens normaliseren aangepast opnieuw numerieke waarden naar een opgegeven bereik. Populaire gegevens normaliseren methoden opnemen:

* **Min Max normaliseren**: Lineair transformeren van de gegevens naar een bereik, zeg tussen 0 en 1, waar de minimumwaarde wordt aangepast aan 0 en maximale waarde op 1.
* **Z-score normaliseren**: gegevens op basis van het gemiddelde en standaarddeviatie schalen: het verschil tussen de gegevens en het gemiddelde wordt gedeeld door de standaarddeviatie.
* **Decimaal schaalbaarheid**: de gegevens met het verplaatsen van het decimaalteken van de kenmerkwaarde wilt verkleinen.  

## <a name="how-to-discretize-data"></a>Hoe worden onderscheiden van gegevens?

Gegevens kunnen worden discretized door te continue waarden converteren naar nominale kenmerken of intervallen. Enkele manieren hiervoor zijn:

* **Gelijke breedte Binning**: het bereik van alle mogelijke waarden van een kenmerk verdeeld over N groepen van dezelfde grootte en de waarden die niet in een opslaglocatie met de bin-nummer toewijzen.
* **Gelijk-hoogte Binning**: het bereik van alle mogelijke waarden van een kenmerk verdeeld over N groepen, elk met hetzelfde aantal exemplaren, en vervolgens de waarden die niet in een opslaglocatie met een getal dat de opslaglocatie toewijzen.  

## <a name="how-to-reduce-data"></a>Hoe gegevens verminderen?

Er zijn verschillende methoden om te verkleinen van gegevens voor de afhandeling van de gegevens gemakkelijker. Afhankelijk van de gegevensgrootte en het domein, kunnen de volgende manieren worden toegepast:

* **Record steekproeven**: voorbeeld van de gegevensrecords en kiest u alleen de vertegenwoordiger subset van de gegevens.
* **Kenmerk steekproeven**: slechts een subset van de belangrijkste kenmerken van de gegevens selecteren.  
* **Aggregatie**: de gegevens in groepen verdelen en bewaren van de getallen voor elke groep. De dagelijkse inkomsten-nummers van een ketting restaurant tijdens de afgelopen 20 jaar kan bijvoorbeeld worden samengevoegd naar maandelijkse inkomsten om de grootte van de gegevens te reduceren.  

## <a name="how-to-clean-text-data"></a>Hoe op te schonen tekstgegevens?

**Tekstvelden in tabelgegevens** kunnen tekens die van invloed zijn op de grenzen van de uitlijning en/of de record van de kolommen bevatten. Voor bijvoorbeeld tabs in een door tabs gescheiden bestand oorzaak kolom uitlijning ingesloten en ingesloten verbreken tekens voor nieuwe regels record lijnen. Tekstcodering afhandeling van de verkeerde tekst bij het schrijven/lezen tekst leidt tot gegevensverlies, worden onbedoeld invoering van gelezen tekens, bijvoorbeeld nulwaarden, en mogelijk ook invloed tekst parseren. Pas op dat u parseren en bewerken zijn mogelijk nodig om op te schonen tekstvelden voor juiste uitlijning en/of uitpakken gestructureerde gegevens uit tekst ongestructureerde of gedeeltelijk gestructureerde gegevens.

**Gegevens verkennen** biedt een vroege weergave in de gegevens. Een aantal gegevensproblemen kan worden ongedekte tijdens deze stap en bijbehorende methoden kunnen worden toegepast om deze problemen op te lossen.  Het is belangrijk om te vragen zoals wat de bron van het probleem is en hoe het probleem mogelijk zijn ingevoerd. Hiermee ook kunt u bepalen op de gegevensverwerking stappen die worden genomen moeten om ze kunt oplossen. Het soort inzichten een wil afgeleid van de gegevens kan ook worden gebruikt om de prioriteit van de hoeveelheid gegevensverwerking te.

## <a name="references"></a>Verwijzingen

>*Datamining: concepten en technieken*, derde Edition, Morgan Kaufmann, 2011, Jiawei Han, Micheline Kamber en Jian Pei
