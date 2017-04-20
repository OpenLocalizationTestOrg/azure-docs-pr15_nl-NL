<properties
    pageTitle="Model resultaten in Machine Learning interpreteren | Microsoft Azure"
    description="Het kiezen van de optimale parameter instellen voor een algoritme gebruikt en score model uitvoer visualiseren."
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
    ms.date="09/12/2016"
    ms.author="bradsev" />


# <a name="interpret-model-results-in-azure-machine-learning"></a>Model resultaten in Azure Machine Learning interpreteren

In dit onderwerp wordt uitgelegd hoe visualiseren en tekstvoorspelling resultaten in Azure Machine Learning Studio interpreteren. Nadat u hebt een model training en klaar voorspellingen bevindt ("het model behaald"), moet u begrijpen en interpreteren van het resultaat van de voorspelling.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Er zijn vier belangrijke soorten machine learning-modellen in Azure Machine Learning:

* Classificatie
* Cluster
* Regressie
* Recommender-systemen

De modules gebruikt voor tekstvoorspelling boven aan deze modellen zijn:

* [Model score] [ score-model] -module voor classificatie en regressie
* [Toewijzen aan Clusters] [ assign-to-clusters] -module voor cluster
* [Score Matchbox Recommender] [ score-matchbox-recommender] voor aanbeveling systems

In dit document wordt uitgelegd hoe u het interpreteren van tekstvoorspelling resultaten voor elk van deze modules. Zie voor een overzicht van deze modules, [het kiezen van parameters Optimaliseer uw algoritmen in Azure Machine Learning](machine-learning-algorithm-parameters-optimize.md).

In dit onderwerp adressen tekstvoorspelling interpretatie, maar niet de evaluatie van het model. Zie voor meer informatie over het evalueren van uw model [model prestaties in Azure Machine Learning evalueren](machine-learning-evaluate-model-performance.md).

Als u nog niet eerder met Azure Machine Learning en hulp bij het maken van een eenvoudige experiment aan de slag, raadpleegt u [een eenvoudige experiment in Azure Machine Learning Studio maken](machine-learning-create-experiment.md) in Azure Machine Learning Studio.

## <a name="classification"></a>Classificatie ##
Er zijn twee subcategorieën van classificatie problemen:

* Problemen met slechts twee klassen (twee-klasse of binaire indeling)
* Problemen met meer dan twee klassen (meerdere class classificatie)

Azure Machine Learning heeft verschillende modules te handelen elk van deze diagramtypen classificatie, maar de methoden voor het interpreteren van de resultaten van hun tekstvoorspelling zijn vergelijkbaar.

### <a name="two-class-classification"></a>Twee-klasse classificatie###
**Voorbeeld experiment**

Een voorbeeld van een probleem classificatie van twee-klasse is de classificatie van Pascaline bloemen. De taak is om te classificeren Pascaline bloemen op basis van hun functies. De gegevensset Pascaline welke zijn bedoeld in Azure Machine Learning is een subset van de populaire [Pascaline gegevensverzameling](http://en.wikipedia.org/wiki/Iris_flower_data_set) bevat exemplaren van slechts twee bloem soorten (klassen 0 en 1). Er zijn vier functies voor elke bloem (sepal lengte, sepal breedte Bloemblad lengte en Bloemblad breedte).

![Schermafbeelding van Pascaline experiment](./media/machine-learning-interpret-model-results/1.png)

Afbeelding 1. Pascaline twee-klasse classificatie probleem experiment

Een experiment is uitgevoerd om dit probleem op te lossen zoals wordt weergegeven in de afbeelding 1. Een model van de structuurweergave gestimuleerd beslissing twee-klasse is training en van een score voorzien. Nu kunt u de resultaten van de voorspelling van het [Score Model] visualiseren[ score-model] module door te klikken op de uitvoerpoort van het [Model Score] [ score-model] module en vervolgens te klikken op **visualiseren**.

![Score model module](./media/machine-learning-interpret-model-results/1_1.png)

Hiermee opent u de score resultaten zoals wordt weergegeven in de afbeelding 2.

![Resultaten van Pascaline twee-klasse classificatie experiment](./media/machine-learning-interpret-model-results/2.png)

Afbeelding 2. Het resultaat van een score-model in twee-klasse classificatie visualiseren

**Resultaat interpretatie**

Er zijn zes kolommen in de resultatentabel. Kolommen aan de linkerkant vier zijn vier functies. De rechts twee kolommen, Labels behaald en behaald kansen, zijn de resultaten van de voorspelling. De kolom van een score voorzien kansen ziet u de kans dat een bloem hoort bij de positieve klasse (Class 1). Het eerste getal in de kolom (0.028571) betekent dat er is bijvoorbeeld 0.028571 kans dat de eerste bloem deel uitmaakt van klasse 1. De kolom van een score voorzien Labels ziet u de voorspelde klasse voor elke bloem. Dit is gebaseerd op de kolom van een score voorzien kansen. Als de scored kans van een bloem groter dan 0,5 is, is deze voorspeld als Class 1. Dit is anders voorspeld als klasse 0.

**Webpublicatie service**

Nadat de resultaten tekstvoorspelling zijn begrepen en beoordeeld geluid, kan het experiment worden gepubliceerd als een webservice zodat u kunt het dashboard in diverse toepassingen implementeren en noem deze om te verkrijgen class voorspellingen op een nieuwe Pascaline bloem. Zie meer informatie over het wijzigen van een experiment training in een score experiment en publiceren als een webservice, [publiceren de Azure Machine Learning-webservice](machine-learning-walkthrough-5-publish-web-service.md). Deze procedure biedt u een score experiment, zoals weergegeven in afbeelding 3.

![Schermafbeelding van het scoren experiment](./media/machine-learning-interpret-model-results/3.png)

Afbeelding 3. Het Pascaline twee-klasse classificatie probleem experiment scoren

U moet nu de invoer en uitvoer voor de webservice instellen. De invoer is de juiste invoer poort van [Score Model][score-model], namelijk de bloem Pascaline invoer functies. De keuze van de uitvoer is afhankelijk van of u geïnteresseerd in de voorspelde klas (label behaald), de scored kans of beide bent. In dit voorbeeld wordt ervan uitgegaan dat u geïnteresseerd in beide bent. De gewenste om uitvoerkolommen te selecteren, gebruikt u een [Kolommen selecteren in gegevensverzameling] [ select-columns] module. Klik op [Kolommen selecteren in gegevensverzameling][select-columns], klikt u op **kolom Gegevenskiezer starten**en selecteer **Behaald Labels** en **Kansen van een score voorzien**. Na het instellen van de uitvoerpoort van [Kolommen selecteren in gegevensverzameling] [ select-columns] en deze opnieuw uitvoert, moet u het score experiment publiceren als een webservice door te klikken op **WEBSERVICE publiceren**. Het uiteindelijke experiment ziet er als afbeelding 4 uit.

![Het Pascaline twee-klasse classificatie experiment](./media/machine-learning-interpret-model-results/4.png)

Afbeelding 4. Definitief score experiment van een probleem van de twee-klasse classificatie Pascaline

Nadat u de webservice uitvoeren en voer de waarden van enkele functie van een testexemplaar, retourneert het resultaat twee getallen. Het eerste getal is het scored label en het tweede is de kans op scored. Deze bloem wordt voorspeld als Class 1 met 0.9655 kans.

![Test interpreteren score model](./media/machine-learning-interpret-model-results/4_1.png)

![Score testresultaten](./media/machine-learning-interpret-model-results/5.png)

Afbeelding 5. Resultaat van de Web-service van Pascaline twee-klasse classificatie

### <a name="multi-class-classification"></a>Meerdere class classificatie
**Voorbeeld experiment**

In dit experiment voert u een taak letter-opname als een voorbeeld van multiclass classificatie. De classificatie probeert te voorspellen van een bepaalde letter (klasse) op basis van bepaalde handgeschreven kenmerkwaarden opgehaald uit de afbeeldingen handgeschreven.

![Voorbeeld van de herkenning brief](./media/machine-learning-interpret-model-results/5_1.png)

In de trainingsgegevens, zijn er 16 functies die zijn opgehaald uit de letter handgeschreven afbeeldingen. De 26 letters formulier onze 26 klassen. Afbeelding 6 bevat een experiment die een model multiclass classificatie voor letter opname trainen en voorspellen op dezelfde functie instellen op een toets gegevensverzameling.

![Letter herkenning multiclass classificatie experiment](./media/machine-learning-interpret-model-results/6.png)

Afbeelding 6. Letter herkenning multiclass classificatie probleem experiment

De resultaten van het [Model Score] zomersportevenementen[ score-model] module door te klikken op de uitvoerpoort van [Score Model] [ score-model] module en vervolgens te klikken op **visualiseren**, ziet u de inhoud, zoals wordt weergegeven in de afbeelding 7.

![Score model resultaten](./media/machine-learning-interpret-model-results/7.png)

Afbeelding 7. Score model resultaten in een indeling met meerdere class visualiseren

**Resultaat interpretatie**

Kolommen aan de linkerkant 16 vertegenwoordigen de waarden van de functie van de set testen. De kolommen met namen zoals behaald kansen voor Class "XX" NET zijn zoals de kolom van een score voorzien kansen in de twee-klasse hoofdletters/kleine letters. Ze de kans weergegeven die de bijbehorende vermelding in een bepaalde categorie valt. Bijvoorbeeld voor het eerste item is er 0.003571 kans dat dit is een 'A,' 0.000451 kans dat dit is een "B", enzovoort. De laatste kolom (Labels van een score voorzien) is hetzelfde als de Labels van een score voorzien in de twee-klasse hoofdletters/kleine letters. De klasse met de grootste scored kans selecteren als de voorspelde klasse van de corresponderende invoer. Voor het eerste item is het scored label bijvoorbeeld "F" omdat er de grootste kans moeten een "F" (0.916995).

**Webpublicatie service**

U kunt ook het scored label voor elk item en de kans van de scored label openen. De eenvoudige logica is om te vinden van de grootste kans tussen alle scored kansen. Hiervoor moet u het [R-Script uitvoeren] gebruiken[ execute-r-script] module. De R-code wordt weergegeven in figuur 8 en het resultaat van het experiment wordt weergegeven in afbeelding 9.

![Voorbeeld van de code R](./media/machine-learning-interpret-model-results/8.png)

Figuur 8. R-code voor het extraheren van de Labels van een score voorzien en de bijbehorende waarschijnlijkheid van de etiketten

![Experiment resultaat](./media/machine-learning-interpret-model-results/9.png)

Afbeelding 9. Definitief score experiment van het probleem met letter-herkenning multiclass classificatie

Nadat u publiceren en uitvoeren van de webservice en voer waarden in sommige invoer functie, het uiterlijk van de geretourneerde resultaat zoals afbeelding 10. Deze brief handgeschreven, met de opgehaalde 16 functies, is een "T" met 0.9715 kans worden voorspeld.

![Test interpreteren score module](./media/machine-learning-interpret-model-results/9_1.png)

![Testresultaat](./media/machine-learning-interpret-model-results/10.png)

Afbeelding 10. Resultaat van de Web-service van multiclass classificatie

## <a name="regression"></a>Regressie

Regressie problemen zijn anders dan classificatie problemen. In een probleem classificatie, waarmee u wilt voorspellen aparte klassen, zoals welke class een bloem Pascaline behoort. Maar zoals u in het volgende voorbeeld van een probleem regressie ziet, waarmee u wilt een doorlopend variabele, zoals de prijs van een auto voorspellen.

**Voorbeeld experiment**

Gebruik auto prijs tekstvoorspelling als uw voorbeeld voor regressie. U probeert te voorspellen van de prijs van een auto op basis van de functies, waaronder aanbrengen, brandstoftype hoofdtekst en station wiel. Het experiment wordt weergegeven in de afbeelding 11.

![Auto prijs regressie experiment](./media/machine-learning-interpret-model-results/11.png)

Afbeelding 11. Auto prijs regressie probleem experiment

Zomersportevenementen van het [Model Score] [ score-model] module, het resultaat lijkt op figuur 12.

![Resultaten voor auto prijs tekstvoorspelling probleem scoren](./media/machine-learning-interpret-model-results/12.png)

Figuur 12. Score resultaat voor het probleem van de voorspelling auto prijs

**Resultaat interpretatie**

Labels van een score voorzien is de resultaatkolom in deze score resultaat. De getallen zijn de voorspelde prijs voor elk auto.

**Webpublicatie service**

U kunt het experiment regressie in een webservice publiceren en noem deze voor auto prijs tekstvoorspelling op dezelfde manier als in de twee-klasse classificatie use-case.

![Experiment scoren voor auto prijs regressie probleem](./media/machine-learning-interpret-model-results/13.png)

Afbeelding 13. Scoren experiment van een auto prijs regressie probleem

Met de webservice, lijkt het geretourneerde resultaat op figuur 14. De voorspelde prijs voor deze auto is $15,085.52.

![Interpreteren score module testen](./media/machine-learning-interpret-model-results/13_1.png)

![Resultaten van de module scoren](./media/machine-learning-interpret-model-results/14.png)

Figuur 14. Web service resultaat van een auto prijs regressie probleem

## <a name="clustering"></a>Cluster

**Voorbeeld experiment**

Laten we de gegevensset Pascaline opnieuw gebruiken voor het maken van een experiment clustering. Hier kunt u filteren van de etiketten class in de gegevensset zodat deze alleen functies heeft en kan worden gebruikt voor het cluster. In dit Pascaline use-case, geef het aantal twee tijdens het trainingsproces, wat betekent dat u wilt de bloemen cluster in de twee klassen. Het experiment wordt weergegeven in figuur 15.

![Pascaline clustering probleem experimenteren](./media/machine-learning-interpret-model-results/15.png)

Figuur 15. Pascaline clustering probleem experimenteren

Cluster verschilt van de classificatie in dat de gegevensset training geen grond-waarheid etiketten door zelf. Cluster groepen de exemplaren van de gegevensverzameling training in afzonderlijke clusters. Tijdens het trainingsproces ziet u het model de vermeldingen door zal het leren van de verschillen tussen de functies. Daarna kan het ervaren model worden gebruikt om te verder classificeren toekomstige. Er zijn twee gedeelten van het resultaat dat we binnen een clustering probleem belangstelling hebben. Het eerste onderdeel is de gegevensset training labels en de tweede classificeert een nieuwe gegevensgroep met het ervaren model.

Het eerste deel van het resultaat kunt visualiseren door te klikken op de uitvoerpoort links van [Trein cluster Model] [ train-clustering-model] en vervolgens te klikken op **visualiseren**. De visualisatie wordt weergegeven in de afbeelding 16.

![Cluster resultaat](./media/machine-learning-interpret-model-results/16.png)

Afbeelding 16. Resultaat voor de gegevensgroep training cluster visualiseren

Het resultaat van het tweede onderdeel cluster van nieuwe items met het ervaren clustering model, wordt weergegeven in de afbeelding 17.

![Resultaat cluster visualiseren](./media/machine-learning-interpret-model-results/17.png)

Afbeelding 17. Resultaat op een nieuwe gegevensgroep cluster visualiseren

**Resultaat interpretatie**

Hoewel de resultaten van de twee onderdelen uit verschillende experiment fasen voortvloeien, worden ze uitzien en op dezelfde manier worden geïnterpreteerd. De eerste vier kolommen zijn functies. De laatste kolom, toewijzingen, is het resultaat van de voorspelling. De items die zijn toegewezen hetzelfde getal zijn voorspeld worden in hetzelfde cluster, dat wil zeggen dat ze overeenkomsten op een bepaalde manier (dit experiment maakt gebruik van de standaard Euclidean afstand meetwaarde) delen. Omdat u het aantal 2 worden opgegeven, worden de items in de toewijzingen gelabeld 0 of 1.

**Webpublicatie service**

U kunt het clustering experiment in een webservice publiceren en noem deze voor cluster voorspellingen hetzelfde als in de indeling met twee-klasse use-case.

![Experiment scoren voor Pascaline clustering probleem](./media/machine-learning-interpret-model-results/18.png)

Figuur 18. Scoren experiment van een Pascaline clustering probleem

Nadat u hebt uitgevoerd van de webservice, wordt het geretourneerde resultaat lijkt op figuur 19. Deze bloem wordt voorspeld in cluster 0.

![Test score module interpreteren](./media/machine-learning-interpret-model-results/18_1.png)

![Score module resultaat](./media/machine-learning-interpret-model-results/19.png)

Figuur 19. Resultaat van de Web-service van Pascaline twee-klasse classificatie

## <a name="recommender-system"></a>Recommender-systeem
**Voorbeeld experiment**

Voor recommender systemen, kunt u het probleem van de aanbeveling restaurant gebruiken als voorbeeld: u kunt restaurants aanbevelen voor klanten die op basis van hun classificatie geschiedenis. De invoergegevens bestaat uit drie delen:

* Restaurant classificaties van klanten
* Gegevens van de functie klant
* Gegevens van restaurant-functie

Er zijn verschillende dingen die we met de [Trein Matchbox Recommender doen kunt] [ train-matchbox-recommender] module Azure Machine weten:

- Classificaties voor een bepaalde gebruiker en het item voorspellen
- Items aan een bepaalde gebruiker aangeraden
- Gebruikers die zijn gerelateerd aan een bepaalde gebruiker zoeken
- Items met betrekking tot een bepaald item zoeken

U kunt kiezen wat u wilt doen door in de vier opties in het menu **Recommender tekstvoorspelling type** te selecteren. Hier kunt u alle vier scenario's doorlopen.

![Matchbox recommender](./media/machine-learning-interpret-model-results/19_1.png)

Een typische Azure Machine Learning-experiment voor een systeem recommender ziet er als figuur 20 uit. Zie voor informatie over het gebruik van deze recommender systeemmodules [trein matchbox recommender] [ train-matchbox-recommender] en [Score matchbox recommender][score-matchbox-recommender].

![Recommender systeem experiment](./media/machine-learning-interpret-model-results/20.png)

20 van de afbeelding. Recommender systeem experiment

**Resultaat interpretatie**

**Classificaties voor een bepaalde gebruiker en het item voorspellen**

Als u **Classificatie tekstvoorspelling** onder **Recommender tekstvoorspelling type**, vraagt u het systeem recommender te voorspellen de classificatie voor een bepaalde gebruiker en het item. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer figuur 21 eruitziet.

![Resultaat van het systeem recommender--beoordeling tekstvoorspelling score](./media/machine-learning-interpret-model-results/21.png)

Figuur 21. Het resultaat score van het systeem recommender--beoordeling tekstvoorspelling visualiseren

De eerste twee kolommen zijn de paren gebruiker-item is verstrekt door de invoergegevens. De derde kolom is de voorspelde classificatie van een gebruiker voor een bepaald item. Klik in de eerste rij de klant U1048 bijvoorbeeld naar tarief restaurant 135026 voorspeld door 2.

**Items aan een bepaalde gebruiker aangeraden**

U selecteert u **Item aanbeveling** onder **Recommender tekstvoorspelling type**en vraagt u het systeem recommender aanbevelen items naar een bepaalde gebruiker. De laatste parameter om te kiezen in dit scenario wordt *aanbevolen item selectie*. De optie **Uit het prioriteitsniveau van Items (voor model evaluatie)** is hoofdzakelijk voor model evaluatie tijdens het training. Voor deze fase tekstvoorspelling Kies we **Uit alle Items**. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer figuur 22 eruitziet.

![Score resultaat van recommender systeem--item aanbeveling](./media/machine-learning-interpret-model-results/22.png)

Afbeelding van 22. Score resultaat van het systeem recommender--item aanbeveling visualiseren

De eerste van de zes kolommen staat voor het opgegeven gebruikers-id's aanbevelen items, zoals verstrekt door de invoergegevens. De andere vijf kolommen vertegenwoordigen de items aanbevolen aan de gebruiker in aflopende volgorde van relevantie te verkrijgen. In de eerste rij, bijvoorbeeld, is het meest aanbevolen restaurant voor klant U1048 134986, gevolgd door 135018, 134975 135021 en 132862.

**Gebruikers die zijn gerelateerd aan een bepaalde gebruiker zoeken**

Als u **Verwante gebruikers** onder **Recommender tekstvoorspelling type**, vraagt u het systeem recommender om gerelateerde gebruikers aan een bepaalde gebruiker te zoeken. Gerelateerde gebruikers zijn de gebruikers die een soortgelijke voorkeuren hebben. De laatste parameter om te kiezen in dit scenario is *gerelateerde gebruikersselectie*. De optie **Uit dat geclassificeerd Items van de gebruikers (voor model evaluatie)** is hoofdzakelijk voor model evaluatie tijdens het training. Kies **Uit alle gebruikers** voor deze fase tekstvoorspelling. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer figuur 23 eruitziet.

![Score resultaat van recommender systeem--gerelateerde gebruikers](./media/machine-learning-interpret-model-results/23.png)

Figuur 23. Score resultaten van het systeem recommender--gerelateerde gebruikers visualiseren

De eerste van de zes kolommen ziet u de opgegeven gebruiker dat id 's nodig zijn om te zoeken naar gerelateerde gebruikers, zoals verstrekt door invoergegevens. De vijf kolommen opslaan de voorspelde gerelateerde gebruikers van de gebruiker in aflopende volgorde van relevantie te verkrijgen. In de eerste rij, bijvoorbeeld, is de meest relevante klant voor klant U1048 U1051, gevolgd door U1066, U1044 U1017 en U1072.

**Items met betrekking tot een bepaald item zoeken**

Als u **Verwante Items** onder **Recommender tekstvoorspelling type**, vraagt u het systeem recommender om gerelateerde items aan een bepaald item te zoeken. Verwante items zijn de items die meestal worden aangegeven door de gebruiker hetzelfde leuk te vinden. De laatste parameter om te kiezen in dit scenario is *gerelateerd item selectie*. De optie **Uit het prioriteitsniveau van Items (voor model evaluatie)** is hoofdzakelijk voor model evaluatie tijdens het training. We kiezen **Uit alle Items** voor deze fase tekstvoorspelling. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer figuur 24 eruitziet.

![Score resultaat van recommender systeem--verwante items](./media/machine-learning-interpret-model-results/24.png)

Figuur 24. Visualiseren score resultaten van het systeem recommender--verwante items

De eerste van de zes kolommen staat voor het opgegeven item-id's nodig om te vinden van verwante items, zoals verstrekt door de invoergegevens. De vijf kolommen opslaan de voorspelde gerelateerde items van het item in aflopende volgorde met relevantie te verkrijgen. In de eerste rij, bijvoorbeeld, is het meest relevant item voor item 135026 135074, gevolgd door 135035, 132875 135055 en 134992.

**Webpublicatie service**

Het proces van het publiceren van deze experimenten als webservices om voorspellingen lijkt voor elk van de vier scenario's. Hier naartoe het tweede scenario (aanbevolen items naar een bepaalde gebruiker) als voorbeeld. U kunt de procedure met de andere drie volgen.

Het systeem ervaren recommender als een ervaren model op te slaan en de ingevoerde gegevens naar een enkele gebruiker-ID-kolom filteren verzoek, kunt u het experiment zoals in de afbeelding 25 aansluiten en publiceren als een webservice.

![Scoren experiment van het probleem van de aanbeveling restaurant](./media/machine-learning-interpret-model-results/25.png)

Afbeelding 25. Scoren experiment van het probleem van de aanbeveling restaurant

Met de webservice, lijkt het geretourneerde resultaat op afbeelding 26. De vijf aanbevolen restaurants voor gebruiker U1048 zijn 134986, 135018 134975, 135021 en 132862.

![Voorbeeld van de systeemservice recommender](./media/machine-learning-interpret-model-results/25_1.png)

![Voorbeeldresultaten experiment](./media/machine-learning-interpret-model-results/26.png)

Afbeelding 26. Resultaat van de Web-service van het probleem met restaurant aanbeveling


<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
