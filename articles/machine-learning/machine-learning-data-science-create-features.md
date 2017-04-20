<properties
    pageTitle="Technische functie in het proces van Cortana Analytics aanbevelen | Microsoft Azure" 
    description="Dit artikel wordt uitgelegd de doelstellingen van de functie engineering en worden voorbeelden gegeven van de rol in het proces voor het verbeteren van gegevens van machine learning."
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


# <a name="feature-engineering-in-the-cortana-analytics-process"></a>Technische functie functie in het proces van Cortana Analytics 

In dit onderwerp wordt uitgelegd van de doelstellingen van de functie engineering en worden voorbeelden gegeven van de rol in het proces voor het verbeteren van gegevens van machine learning. De voorbeelden gebruikt om te illustreren dit proces worden getekend van Azure Machine Learning Studio. 

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

In dit **menu** koppelingen naar onderwerpen waarin wordt uitgelegd hoe u functies voor gegevens maken in verschillende omgevingen. Deze taak is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Technische pogingen om uit te breiden de blog kracht van algoritmen leren door te maken van functies van onbewerkte gegevens die helpen vergemakkelijken het onderwijs aanbevelen. De technische functie en de selectie van functies maakt u één deel uit van de TDSP die worden beschreven in de [Wat is het proces Team gegevens wetenschappelijke?](data-science-process-overview.md) Zijn de onderdelen van de stap **ontwikkelen onderdelen** van de TDSP functie engineering en selectie. 

* **technische functie**: dit proces probeert aanvullende relevante functies vanuit de bestaande onbewerkte functies in de gegevens maken en om te verhogen van de blog macht van het algoritme leren gebruiken.

* **Functieselectie**: dit proces Hiermee selecteert u de belangrijkste subset van de functies van de oorspronkelijke gegevens in een poging verkleinen van de dimensionaliteit van het probleem training.

Normaal gesproken **functie engineering** eerst wordt toegepast om te genereren aanvullende functies en vervolgens de stap voor het **selecteren van de functie** wordt uitgevoerd om te voorkomen niet relevant, overbodige of zeer gecorreleerde functies.

De trainingsgegevens in machine learning u kunt vaak verbeteren door extractie van functies van de onbewerkte gegevens die worden verzameld. Een voorbeeld van een ontworpen functie in de context van leren hoe u de afbeeldingen van handgeschreven tekens classificeren is het maken van een stapje dichtheid kaart die zijn gemaakt op basis van de gegevens van de verdeling onbewerkte bits. Dit overzicht kunt de randen van de tekens efficiënter te gebruiken om de onbewerkte verdeling direct zoeken.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="creating-features-from-your-data---feature-engineering"></a>Maken van functies uit uw gegevens - aanbevelen Engineering

De trainingsgegevens bestaat uit een matrix die bestaat uit voorbeelden (records of opmerkingen die zijn opgeslagen in rijen), die elk een reeks functies (variabelen of velden die zijn opgeslagen in kolommen heeft). De functies die zijn opgegeven in de de opzet verwachting om aan te geven van de patronen in de gegevens. Hoewel veel van de velden onbewerkte gegevens kunnen rechtstreeks worden opgenomen in de geselecteerde functieset van kon u een model trainen, is het vaak het geval dat aanvullende (ontworpen) functies worden gemaakt op basis van de functies in de onbewerkte gegevens moeten naar een uitgebreide training dataset genereren.

Welk soort functies moet worden gemaakt voor het verbeteren van de gegevensset wanneer een model training? Ontworpen functies waarmee u de training kunt bevatten informatie waarmee de patronen in de gegevens beter te onderscheiden. We verwachten de nieuwe functies om aanvullende gegevens die niet duidelijk genomen of eenvoudig zichtbaar in de oorspronkelijke of bestaande functieset. Maar dit proces is iets van een illustratie. Geluids- en productief beslissingen vereist vaak sommige expertise domein.

Bij het starten met Azure Machine Learning, het gemakkelijkst om te leren dit proces concrete invulling te geven met voorbeelden in de Studio. Twee voorbeelden worden hier weergegeven:

* Een voorbeeld van de regressie [voorspelling van het aantal fietsen huur](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) van een gecontroleerde experiment waar de doelwaarden bekend zijn
* Een mining classificatie tekstvoorbeeld [Functie Hashing](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) gebruiken

### <a name="example-1-adding-temporal-features-for-regression-model"></a>Voorbeeld 1: Tijdelijke functies toe te voegen voor regressiemodel ###

Laten we gebruiken de experiment "aanvraag prognoses van fietsen" in Azure Machine Learning Studio om te laten zien hoe u functies voor een taak regressie-engineering toepassen. Het doel van dit experiment is om te voorspellen van de aanvraag voor de fietsen, dat wil zeggen het aantal fietsen huur binnen een bepaalde maand/dag/uur. De gegevensset "fietsen huur UCI gegevensset" wordt gebruikt als de onbewerkte invoergegevens. Deze dataset is gebaseerd op reële gegevens uit het kapitaal Bikeshare bedrijf dat een netwerk van de huur fietsen in in de Verenigde Staten Washington domeincontroller onderhoudt. De gegevensset staat voor het aantal fietsen huur binnen een bepaalde uur na een dag in de jaar 2011 en jaar 2012 en 17379 rijen en 17 kolommen bevat. De functieset onbewerkte bevat weer voorwaarden (temperatuur/vochtigheid/windsnelheid) en het type van de dag (feestdagen/weekdag). Het veld te voorspellen is 'CT', een telling die de huur fietsen binnen een bepaalde uur vertegenwoordigt en welke kan variëren van 1 tot 977 bereiken.

Met het doel van het bouwen van effectieve functies in de trainingsgegevens, vier regressie modellen worden gemaakt via dezelfde algoritme, maar met vier verschillende training gegevenssets. De vier gegevenssets de dezelfde onbewerkte invoergegevens vertegenwoordigen, maar met een toeneemt aantal functies instellen. Deze functies zijn gegroepeerd in vier categorieën:

1. A = weer + feestdagen + weekdag + weekend functies voor de voorspelde dag
2. B = aantal fietsen die zijn gehuurd in elk van de vorige 12 uur
3. C = aantal fietsen die zijn gehuurd in elk van de vorige 12 dagen aan hetzelfde uur
4. D = aantal fietsen die in elk van de vorige 12 weken aan hetzelfde uur en dezelfde dag zijn gehuurd

Naast de functie set A, al bestaat in de oorspronkelijke onbewerkte gegevens, worden de andere functies drie sets gemaakt via de functie engineering proces. Functieset B schermopnames zeer recent aanvraag voor de fietsen. Functie instellen C schermopnames de aanvraag voor bikes aan een bepaald uur. Functieset D schermopnames aanvraag voor bikes op bepaalde uur en bepaalde dag van de week. De vier training gegevenssets die elke bevat respectievelijk functie set A, A + B, A + B + C en A + B + C + D.

In het experiment Azure Machine Learning, worden deze vier training gegevenssets gevormd via vier vertakkingen van de vooraf verwerkte invoer gegevensset. Een module [Execute R Script](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) , waarin een set functies (functie ingesteld B, C en D) kleurencombinaties zijn, behalve de linkerkant bevat de meeste tak, elk van deze vertakkingen respectievelijk opgesteld en toegevoegd aan de geïmporteerde gegevensset. De volgende afbeelding ziet het R-script gebruikt om te maken van functieset B in de tweede links tak.

![functies maken](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

De vergelijking van de prestatieresultaten van de vier modellen in de volgende tabel worden samengevat. De beste resultaten worden weergegeven door functies A + B + C. Opmerking het tarief fout wordt verlaagd wanneer extra functieset zijn opgenomen in de trainingsgegevens. Er wordt gecontroleerd onze vermoeden dat de functieset van B, C vindt u extra relevante informatie voor de taak regressie. Maar voor het toevoegen van de functie D lijkt niet compatibel te bieden eventuele aanvullende wilt verkleinen in het tarief fout.

![resultaat-vergelijking](./media/machine-learning-data-science-create-features/result1.png)

### <a name="example2"></a>Voorbeeld 2: Maken van functies in tekst Mining  

Technische functie wordt sterk uiteen in taken met betrekking tot mining voor tekst, zoals de document-indeling en sentiment analyse toegepast. Als we classificeren van documenten in verschillende categorieën wilt, is een typisch verondersteld bijvoorbeeld dat de word/zinnen opgenomen in één document categorie minder kunnen optreden in een ander document categorie zijn. In een andere woorden kan de frequentie van de verdeling woorden/woordgroepen om aan te geven ander documentcategorieën. In tekst mining-toepassingen, omdat afzonderlijke onderdelen van de inhoud van een tekstvak gewoonlijk als de ingevoerde gegevens gebruikt, is de functie engineering proces nodig om te maken van de functies die betrekking hebben op personen woord of woordgroep frequentie.

Als u wilt bereiken deze taak, wordt een techniek die genoemd **functie hashing** toegepast om het efficiënt willekeurige tekstfuncties omzetten in indexen. In plaats van elke functie tekst (woorden/zinnen) koppelen aan een bepaalde index, deze methode werkt door een functie hash toepassen op de functies en hun hashwaarden rechtstreeks als indexen gebruiken.

In Azure Machine Learning, is een [Functie Hashing](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) module die wordt gemaakt van dit woord of woordgroep functies op handige wijze. Volgende afbeelding ziet u een voorbeeld van het gebruik van deze module. De invoer gegevensset bestaat uit twee kolommen: de classificatie van het adresboek getal van 1 tot en met 5, en de inhoud van de werkelijke controleren. Het doel van deze [Functie Hashing](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) module is voor het ophalen van een reeks nieuwe videofuncties die weergeven de frequentie exemplaar van de corresponderende woord(en) / r-zin(nen) binnen het bepaalde adresboek bekijken. Als u wilt deze module hebt gebruikt, moeten we de volgende stappen uit:

* Selecteer eerst de kolom met de ingevoerde tekst ("Kol2' in dit voorbeeld).
* Tweede, instellen 'Hashing bitsize' tot en met 8, hetgeen betekent dat 2 ^ 8 = 256 functies wordt gemaakt. De word-fase in alle tekst wordt worden hashing naar 256 indexen. De parameter "Hashing bitsize" bereik van 1 tot 31. De woorden / r-zin(nen) hebben minder waarschijnlijk in dezelfde index worden hashing als ervan kan worden van een hogere waarde instellen.
* Derde, stel de parameter "N-g" naar 2. Hiermee wordt de frequentie exemplaar van unigrams (een functie van elk woord) en bigrams (een functie voor elk paar aangrenzende woorden) van de invoer tekst. De parameter "N-g" bereik van 0 tot en met 10 is, geeft het maximumaantal opeenvolgende woorden in een functie wilt opnemen.  

![Module "Aanbevelen Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

De volgende afbeelding ziet u wat deze nieuw uiterlijk functie zoals.

![Voorbeeld "Aanbevelen Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing2.png)


## <a name="conclusion"></a>Sluiten

Ontworpen en geselecteerde functies vergroot de efficiëntie van het trainingsproces voor dat probeert te extraheren van de belangrijkste gegevens in de gegevens. Ze ook verbeteren de kracht van deze modellen de invoergegevens nauwkeurig classificeren en resultaten van belang meer krachtig voorspellen. Technische functie en de selectie kunnen ook zodat het onderwijs meer rekenkundig tractable combineren. Dit gebeurt door te klikken en vervolgens waardoor het aantal functies die nodig zijn om te kalibreren of trainen een model verbeteren. Wiskundig spreekt, zijn de functies die zijn geselecteerd voor het model trainen een minimum aantal onafhankelijke variabelen die de patronen in de gegevens te leggen en vervolgens succes voorspellen resultaten.

Houd er rekening mee dat het is niet altijd per se om uit te voeren functies engineering of functie selecteren. Opgeven of het nodig is of niet, is afhankelijk van de gegevens die we hebben of verzamelen, de algoritme van de die we kiezen en het doel van het experiment.
 
