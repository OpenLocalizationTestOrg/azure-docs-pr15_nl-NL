<properties
    pageTitle="Technische functie en de selectie in Azure Machine Learning aanbevelen | Microsoft Azure"
    description="Dit artikel wordt uitgelegd de doelstellingen van de functieselectie en functie engineering en worden voorbeelden gegeven van hun rol in het proces gegevensuitbreiding van machine learning."
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
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Technische functie en de selectie in Azure Machine Learning aanbevelen

In dit onderwerp wordt uitgelegd dat de doelstellingen van de functie engineering en -onderdelen selecteren in het proces gegevensuitbreiding van machine learning. Deze ziet u wat deze processen gebruikmaakt van met behulp van voorbeelden verstrekt door Azure Machine Learning Studio.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

De trainingsgegevens in machine learning u kunt vaak verbeteren door de selectie of de extractie van functies van de onbewerkte gegevens die worden verzameld. Een voorbeeld van een ontworpen functie in de context van leren hoe u de afbeeldingen van handgeschreven tekens classificeren is een bits dichtheid kaart gemaakt op basis van de gegevens van de verdeling onbewerkte bits. Dit overzicht kunt de randen van de tekens efficiënter dan de onbewerkte verdeling zoeken.

Ontworpen en geselecteerde functies vergroot de efficiëntie van het trainingsproces probeert te extraheren van de belangrijkste gegevens in de gegevens. Ze ook verbeteren de kracht van deze modellen de invoergegevens nauwkeurig classificeren en resultaten van belang meer krachtig voorspellen. Technische functie en de selectie kunnen ook zodat het onderwijs meer rekenkundig tractable combineren. Dit gebeurt door te klikken en vervolgens waardoor het aantal functies die nodig zijn om te kalibreren of trainen een model verbeteren. Wiskundig spreekt, zijn de functies die zijn geselecteerd voor het model trainen een minimum aantal onafhankelijke variabelen die de patronen in de gegevens te leggen en vervolgens succes voorspellen resultaten.

De technische functie en de selectie van functies één deel uitmaakt van een groter proces, dat meestal uit vier stappen bestaat:

* Gegevens verzamelen
* Gegevensuitbreiding
* Model constructie
* Achteraf worden verwerkt

Technische functie en de selectie vormen de stap gegevens de verbetering van machine learning. Drie aspecten van dit proces worden onderscheiden voor ons doel:

* **Oude gegevensverwerking**: dit proces wordt geprobeerd om ervoor te zorgen dat de verzamelde gegevens schoon en consistent is. Het bevat taken zoals integratie van meerdere gegevenssets, afhandeling ontbrekende, afhandeling inconsistente gegevens en converteren van gegevenstypen.
* **Technische functie**: dit proces probeert aanvullende relevante functies vanuit de bestaande onbewerkte functies in de gegevens maken en om de blog power op de algoritme van learning te verbeteren.
* **Functieselectie**: dit proces Hiermee selecteert u de belangrijkste subset van de functies van de oorspronkelijke gegevens verkleinen van de dimensionaliteit van het probleem training.

Dit onderwerp behandelt alleen de functie engineering en de functie selectie aspecten van het proces van de schaduweffecten gegevens. Zie [voorverwerking gegevens in Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/)voor meer informatie over de stap gegevens de vooraf verwerking.


## <a name="creating-features-from-your-data--feature-engineering"></a>Maken van functies uit uw gegevens--aanbevelen engineering

De trainingsgegevens bestaat uit een matrix die bestaat uit voorbeelden (records of opmerkingen die zijn opgeslagen in rijen), die elk een reeks functies (variabelen of velden die zijn opgeslagen in kolommen heeft). De functies die zijn opgegeven in de de opzet verwachting om aan te geven van de patronen in de gegevens. Hoewel veel van de velden onbewerkte gegevens kunnen rechtstreeks worden opgenomen in de geselecteerde functieset van kon u een model trainen, moeten aanvullende ontworpen functies vaak worden gemaakt op basis van de functies in de onbewerkte gegevens voor het genereren van een gegevensverzameling uitgebreide training.

Welk soort functies moet worden gemaakt voor het verbeteren van de gegevensverzameling wanneer een model training? Ontworpen functies waarmee u de training kunt bevatten informatie waarmee de patronen in de gegevens beter te onderscheiden. U verwacht dat de nieuwe functies om aanvullende gegevens die niet duidelijk is geregistreerd of eenvoudig zichtbaar in de oorspronkelijke of bestaande functieset, maar dit proces is iets van een illustratie. Geluids- en productief beslissingen vereist vaak sommige expertise domein.

Bij het starten met Azure Machine Learning, het gemakkelijkst om te leren dit proces concrete invulling te geven met behulp van de voorbeelden in Machine Learning Studio. Twee voorbeelden worden hier weergegeven:

* Een voorbeeld regressie ([voorspelling van het aantal fietsen huur](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) in een gecontroleerde experiment waar de doelwaarden bekend
* Een voorbeeld van tekst-mining regelclassificatie [Functie Hashing] gebruiken[feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Voorbeeld 1: Tijdelijke functies voor een regressiemodel toevoegen ###

U kunt zien hoe u functies voor een taak regressie engineering, gaat u gebruiken de experiment "aanvraag prognoses van fietsen" in Azure Machine Learning Studio. Het doel van dit experiment is om te voorspellen van de aanvraag voor de fietsen, dat wil zeggen het aantal fietsen huur binnen een bepaalde maand, dag of uur. De gegevensset **fietsen huur UCI gegevensverzameling** wordt gebruikt als de onbewerkte invoergegevens.

Deze gegevensset is gebaseerd op reële gegevens uit het kapitaal Bikeshare bedrijf dat een netwerk van de huur fietsen in in de Verenigde Staten Washington domeincontroller onderhoudt. De gegevensset staan voor het aantal fietsen huur binnen een bepaalde uur na een dag, uit 2011 bij 2012 en erin 17379 rijen en kolommen 17. De functieset onbewerkte bevat weer voorwaarden (temperatuur, vochtigheid, windsnelheid) en het type van de dag (feestdagen of weekdag). Het veld te voorspellen is **ct**, een telling die de huur fietsen binnen een bepaalde uur vertegenwoordigt en die een bereik van 1 tot 977.

Als u wilt samenstellen effectieve functies in de trainingsgegevens, worden de vier regressie-modellen worden gemaakt met behulp van dezelfde algoritme, maar met vier verschillende training gegevensgroepen. De vier gegevensgroepen de dezelfde onbewerkte invoergegevens vertegenwoordigen, maar met een toeneemt aantal functies instellen. Deze functies zijn gegroepeerd in vier categorieën:

1. A = weer + feestdagen + weekdag + weekend functies voor de voorspelde dag
2. B = aantal fietsen die zijn gehuurd in elk van de vorige 12 uur
3. C = aantal fietsen die zijn gehuurd in elk van de vorige 12 dagen aan hetzelfde uur
4. D = aantal fietsen die in elk van de vorige 12 weken aan hetzelfde uur en dezelfde dag zijn gehuurd

Naast de functie set A, al in de oorspronkelijke onbewerkte gegevens bestaat, worden de andere functies drie sets gemaakt via de functie engineering proces. Functieset B schermopnames de recente vraag naar de fietsen. Functie instellen C schermopnames de aanvraag voor bikes aan een bepaald uur. Functieset D schermopnames aanvraag voor bikes op bepaalde uur en bepaalde dag van de week. Elk van de vier training gegevensgroepen bevat functiesets A, A + B, A + B + C en A + B + C + D, respectievelijk.

In het experiment Azure Machine Learning, worden deze vier training gegevenssets gevormd via vier vertakkingen van de vooraf verwerkte invoer gegevensset. Behalve voor de meest linkse tak, die elk van deze vertakkingen bevat een [R-Script uitvoeren] [ execute-r-script] module waarin een set functies (functie wordt ingesteld B, C en D) kleurencombinaties respectievelijk is gebouwd en is toegevoegd aan de geïmporteerde gegevensset. De volgende afbeelding ziet het R-script gebruikt om te maken van functieset B in de tweede links tak.

![Een functieset maken](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

De volgende tabel bevat een overzicht van de vergelijking van de prestatieresultaten van de vier modellen. De beste resultaten worden weergegeven door functies A + B + C. Houd er rekening mee dat het tarief fout afneemt wanneer er extra functiesets zijn opgenomen in de trainingsgegevens. Hiermee wordt gecontroleerd onze vermoeden dat de functiesets B en C vindt u extra relevante informatie voor de taak regressie. Het toevoegen van het instellen van de functie D lijkt niet compatibel te leveren eventuele aanvullende wilt verkleinen in het tarief fout.

![Prestatieresultaten van vergelijken](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Voorbeeld 2: Maken van functies in tekst mining  

Technische functie wordt sterk uiteen in taken met betrekking tot mining voor tekst, zoals de document-indeling en sentiment analyse toegepast. Als u classificeren van documenten in verschillende categorieën wilt, is een typisch verondersteld bijvoorbeeld dat de woorden of zinnen die zijn opgenomen in één documentcategorie minder kunnen optreden in een ander documentcategorie zijn. De frequentie van het woord of woordgroep verdeling is met andere woorden, kunnen om aan te geven ander documentcategorieën. In tekst mining-toepassingen, is de functie engineering proces nodig om u te maken van de functies die betrekking hebben op woord of woordgroep frequentie personen omdat afzonderlijke onderdelen van de inhoud van een tekstvak gewoonlijk als de ingevoerde gegevens gebruikt.

Als u wilt bereiken deze taak, wordt een techniek die genoemd *functie hashing* toegepast om het efficiënt willekeurige tekstfuncties omzetten in indexen. In plaats van elke functie tekst (woorden of woordgroepen) koppelen aan een bepaalde index, deze methode werkt met behulp van een formule hash tot de functies en met behulp van hun hashwaarden als indexen rechtstreeks.

In Azure Machine Learning, er is een [Functie Hashing] [ feature-hashing] module die Hiermee maakt u deze functies woord of woordgroep. De volgende afbeelding ziet u een voorbeeld van het gebruik van deze module. De invoer gegevensverzameling bevat twee kolommen: de classificatie van het adresboek getal van 1 naar 5 en de werkelijke inhoud controleren. Het doel van deze [Functie Hashing] [ feature-hashing] module is om op te halen, nieuwe functies waarmee de frequentie exemplaar van het bijbehorende woorden of woordgroepen binnen de recensie bepaalde adresboek weergeven. Als u wilt deze module gebruiken, moet u de volgende stappen uit:

1. Selecteer de kolom met de invoer tekst (**Kol2** in dit voorbeeld).
2. *Hashing bitsize* ingesteld op 8, wat betekent dat 2 ^ 8 = 256 functies worden gemaakt. Het woord of woordgroep in de tekst wordt vervolgens naar 256 indexen worden opgedeeld. De parameter *Hashing bitsize* bereik van 1 tot 31. Als de parameter is ingesteld op een hoog getal, hebben de woorden of woordgroepen minder waarschijnlijk in dezelfde index worden hashing.
3. Stel de parameter *N-g* naar 2. Dit de frequentie exemplaar van unigrams (een functie van elk woord) en bigrams (een functie voor elk paar aangrenzende woorden) opgehaald uit de ingevoerde tekst. De parameter *N-g* bereik van 0 tot 10 is, geeft het maximumaantal opeenvolgende woorden in een functie wilt opnemen.  

![Functie hashing module](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

De volgende afbeelding ziet hoe deze nieuwe functies eruit.

![Functie hashing voorbeeld](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Filterfuncties uit uw gegevens---onderdelen selecteren  ##

*Selectie van de functie* is een proces dat meestal wordt toegepast op de bouw van training gegevenssets voor blog modellering taken zoals classificatie of regressie taken. Het doel is om een subset van de functies van de oorspronkelijke gegevensset die Hiermee reduceert u de afmetingen met behulp van een minimum aantal functies om aan te geven van de maximale hoeveelheid variantie in de gegevens te selecteren. Deze een subset van de functies bevat de enige functies wilt opnemen als u wilt het model trainen. Functieselectie fungeert voornamelijk gebruikt:

* Functieselectie vaak verhogen classificatie nauwkeurigheid doordat niet relevant, overbodige of sterke correlatie bestaat tussen functies.
* Functieselectie wordt het aantal functies, waardoor het model trainingsproces efficiënter kleiner. Dit is vooral voor cursisten die echter duur in trainen zoals ondersteuning vector machines.

Hoewel de Functieselectie zoekt naar Beperk het aantal functies in de gegevensset gebruikt om te trainen van het model, het is niet meestal wordt verwezen door de term *dimensionaliteit wilt verkleinen.* Functie selectiemethoden extraheren een subset van de oorspronkelijke functies in de gegevens niet wijzigen.  Dimensionaliteit wilt verkleinen methoden gebruiken ontworpen functies waarmee u kunnen de oorspronkelijke functies transformeren en deze zo te wijzigen. Voorbeelden van dimensionaliteit wilt verkleinen methoden zijn principal onderdeel analyse, canonieke correlatieanalyse en enkelvoud waarde uitgevouwen.

Een breed worden toegepast categorie van de functie selectiemethoden in een gecontroleerde context is filter gebaseerde onderdelen selecteren. Vóór evaluatie van de correlatie tussen elke functie en het kenmerk target, worden in deze methoden een statistische eenheid een score toewijzen aan elke functie toepassen. De functies zijn vervolgens gerangschikt op de score, waarin u kunt de drempel voor bewaren of verwijderen van een specifieke functie instellen. Voorbeelden van de statistische eenheden gebruikt in de volgende manieren zijn Pearson-correlatiecoëfficiënt, onderlinge informatie en de chi-kwadraatverdeling test.

Azure Machine Learning Studio biedt modules voor onderdelen selecteren. In de volgende afbeelding ziet deze modules [Filter gebaseerde Functieselectie] opnemen[ filter-based-feature-selection] en [Fisher lineaire Discriminant analyse][fisher-linear-discriminant-analysis].

![Voorbeeld van de functie selectie](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)


Bijvoorbeeld, gebruikt u de [Functieselectie op basis van een Filter] [ filter-based-feature-selection] module met de tekst mining voorbeeld eerder beschreven. Stel dat u wilt maken van een regressiemodel nadat een reeks 256 functies is gemaakt via de [Functie Hashing] [ feature-hashing] module en dat de variabele antwoord **Kol1** en vertegenwoordigt een boek Raadpleeg classificatie getal van 1 tot en met 5. Stel de **functie scoren methode** op **Pearson-correlatiecoëfficiënt**, **doelkolom** **Kol1**en **aantal gewenste functies** tot **50**. De module [Filter gebaseerde onderdelen selecteren] [ filter-based-feature-selection] vervolgens genereert een gegevensverzameling met 50 functies gebruiken samen met het kenmerk target **Kol1**. De volgende afbeelding ziet de stroom van dit experiment en de invoerparameters weergegeven.

![Voorbeeld van de functie selectie](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

De volgende afbeelding ziet de resulterende gegevensverzamelingen. Elke functie is gebaseerd behaald bij de Pearson-correlatiecoëfficiënt tussen zichzelf en door het kenmerk target **Kol1**. De functies met bovenste scores worden bewaard.

![Filter gebaseerde selectie gegevens functiesets](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

De volgende afbeelding ziet de bijbehorende scores van de geselecteerde onderdelen.

![Geselecteerde functie scores](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Met deze [Functieselectie op basis van een Filter] toepassen[ filter-based-feature-selection] module 50 van 256 functies zijn geselecteerd, omdat ze de meeste functies verband met de variabele **Kol1** op basis van de score methode **Pearson-correlatiecoëfficiënt**doel hebben.

## <a name="conclusion"></a>Sluiten
Technische functie en de Functieselectie zijn in twee stappen worden uitgevoerd als u wilt de trainingsgegevens van de voorbereiden bij het maken van een machine learning-model. Normaal gesproken vergt functie engineering eerst om te genereren aanvullende functies wordt toegepast en klikt u vervolgens stap voor het selecteren van de functie wordt uitgevoerd om te voorkomen niet relevant, overbodige of zeer gecorreleerde functies.

Het is niet altijd noodzakelijkerwijs om uit te voeren functies engineering of functie selecteren. Opgeven of het nodig is, is afhankelijk van de gegevens die u hebt of verzamelen, het algoritme die u kiest en het doel van het experiment.


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
