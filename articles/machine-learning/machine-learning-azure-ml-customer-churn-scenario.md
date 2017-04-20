<properties
    pageTitle="Analyse van klanten te produceren met Machine Learning | Microsoft Azure"
    description="Casestudy van de ontwikkeling van een geïntegreerde model voor het analyseren en scoren klant lospeuteren"
    services="machine-learning"
    documentationCenter=""
    authors="jeannt"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="jeannt"/>

# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Klant te produceren analyseren met behulp van Azure Machine Learning

##<a name="overview"></a>Overzicht
In dit onderwerp vindt u de uitvoering van een verwijzing van een klant lospeuteren analyse project die is ingebouwd met behulp van Azure Machine Learning Studio. Deze worden gekoppeld algemene modellen zuinigste om het probleem van industriële klant lospeuteren besproken. We ook de nauwkeurigheid van modellen die zijn gemaakt met Machine Learning meten en we aanwijzingen voor verdere ontwikkeling beoordelen.  

### <a name="acknowledgements"></a>Bevestigingen

Dit experiment is ontwikkeld en getest door Serge Berger, hoofdsom gegevens wetenschappelijk bij Microsoft en Roger Barga, voorheen Product Manager voor Microsoft Azure Machine Learning. Het documentatieteam van Azure dank hun expertise bevestigt en ze Bedankt voor het delen van dit artikel.

>[AZURE.NOTE] De gegevens die worden gebruikt voor dit experiment is niet openbaar beschikbaar. Zie voor een voorbeeld van het maken van een machine learning-model voor lospeuteren analyse,: [Telco te produceren model sjabloon](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) in de [Galerie met Cortana bedrijfsinformatie](http://gallery.cortanaintelligence.com/)


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="the-problem-of-customer-churn"></a>Het probleem van de klant lospeuteren
Bedrijven in de onbepaalde consumenten en in alle enterprise sectoren hebben te handelen lospeuteren. Soms lospeuteren is overtollige en beleidsbeslissingen beïnvloedt. De traditionele oplossing is om te voorspellen investeringsneiging van hoog churners en hun behoeften via een concierge-service, marketingcampagnes, of door toe te passen speciale dispensaties. Deze benaderingen kunnen variëren uit industrie bedrijfstak en zelfs naar een bepaalde consumenten cluster binnen één industrie (bijvoorbeeld telecommunicatie).

De algemene factor is dat bedrijven moeten deze speciale klant bewaarbeleid inspanningen minimaliseren. Een natuurlijke methodologie zou dus aan elke klant met de kans van lospeuteren score en de bovenste N kleuren die zijn gerelateerd. De belangrijkste klanten mogelijk zijn de meest winstgevende als; bijvoorbeeld nog fraaiere scenario's, een functie winst in dienst is tijdens de selectie van kandidaten voor speciale dispensatie. De volgende punten zijn echter slechts een deel van de compleet strategie voor het omgaan met lospeuteren. Bedrijven hebben ook rekening account risico (en de bijbehorende risicotolerantie), wordt het niveau en de kosten van de tussenkomst en aannemelijke klant Segmentatie.  

##<a name="industry-outlook-and-approaches"></a>Industriële outlook en methoden
Geavanceerde afhandeling van lospeuteren is een teken van een volwaardige industrie. Het klassieke voorbeeld is de bedrijfstak telecommunicatie waar abonnees bekend vaak overschakelen van één provider naar de andere. Deze vrij lospeuteren is van belang voornaamste. Bovendien hebt providers cumulatief aanzienlijk knowledge over *stuurprogramma's te produceren*, die de factoren die klanten station om te schakelen.

Keuze handset of het apparaat is bijvoorbeeld een bekende stuurprogramma van lospeuteren in het bedrijf van de mobiele telefoon. Hierdoor wordt een populaire beleid is aan de prijs van een handset subsidize voor nieuwe abonnees en opladen in een volledige prijs aan bestaande klanten voor een upgrade. In het verleden, heeft dit beleid geleid tot klanten hopping van één provider naar een andere om een nieuwe korting, die op zijn beurt providers om te verfijnen hun strategieën heeft gevraagd.

Hoge volatiliteit in handset aanbiedingen is een factor die zeer snel ongeldig modellen van lospeuteren die zijn gebaseerd op de huidige handset-modellen. Mobiele telefoons worden Daarnaast kunnen niet alleen telecommunicatie apparaten; Deze zijn ook wijze-instructies (Houd rekening met de iPhone) en deze sociale variabelen zich buiten het bereik van gewone telecommunicatie gegevenssets.

Het resultaat voor modellering is dat u niet kunt van een geluid beleid ontwerpen gewoon doordat bekende redenen om lospeuteren. Een doorlopende modellering strategie, inclusief klassieke modellen die kwantificeer bestaan variabelen (zoals beslissingsbomen), is in feite **verplicht**.

Met grote gegevenssets in hun klanten, uitvoert organisaties analyses van grote gegevens (met name lospeuteren detectie op basis van grote gegevens) als een effectieve benadering van het probleem. U vindt meer informatie over de aanpak groot gegevens van het probleem van lospeuteren in de aanbevelingen op ETL sectie.  

##<a name="methodology-to-model-customer-churn"></a>Methode moeten model klant lospeuteren
Een algemene probleemoplossing proces voor het oplossen van klant lospeuteren wordt afgebeeld in afbeeldingen 1-3:  

1.  Een risicomodel kunt u overwegen de invloed van acties op kans en risico's.
2.  Een model tussenkomst kunt u overwegen hoe het niveau van tussenkomst kan invloed hebben op de kans van lospeuteren en het bedrag van de klant verlooptijd (CLV).
3.  Deze analyse gepaard met een kwalitatieve analyse die is doorverwezen naar een proactief marketingcampagne die is bedoeld voor klantsegmenten om de optimale aanbieding.  

![][1]

Deze methode doorsturen ogende is de beste manier wilt behandelen lospeuteren, maar deze wordt geleverd met complexiteit: we hebben een met meerdere modellen archetype en doelcellen afhankelijkheden tussen de modellen voor ontwikkelen. De interactie tussen modellen kan worden encapsulated zoals wordt weergegeven in het volgende diagram:  

![][2]

*Afbeelding 4: Geïntegreerd met meerdere modellen archetype*  

Interactie tussen de modellen is heel belangrijk als we zijn voor het leveren van een compleet aanpak naar klant bewaarbeleid. Elk model vermindert noodzakelijkerwijs na verloop van tijd; de architectuur is dus een impliciete lus (vergelijkbaar met de archetype instellen door de standaard in SCHERPE-DM datamining, [***3***]).  

De algehele cyclus van risico-beslissing-marketing segmentatie/uitgevouwen is nog steeds een generalized structuur die van toepassing op veel zakelijke problemen is. Lospeuteren analyse is gewoon een sterke vertegenwoordiger van deze groep van problemen, omdat deze de kenmerken van een probleem met complexe bedrijven die niet zijn toegestaan met een eenvoudige blog oplossing vertoont. De sociale aspecten van de moderne aanpak te produceren zijn met name niet gemarkeerd in de methode, maar de sociale aspecten worden opgenomen in de archetype modellering, zoals in een model.  

Een interessante toevoeging hier is analyses van grote gegevens. De telecommunicatie en de detailhandel bedrijven vandaag uitgebreide gegevens over hun klanten verzamelen en we eenvoudig kunt voorzien dat dat u met meerdere modellen connectivity in een algemene trend, nieuwe trends zoals de dingen die van Internet en de overal apparaten, waardoor bedrijven verandert te gebruiken op meerdere lagen smart-oplossingen gegeven.  

 
##<a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>De archetype modellering implementeren in Machine Learning Studio
Gegeven het probleem dat hierboven wordt beschreven, wat is de beste manier om een geïntegreerde modellering en scoren methode te implementeren? In dit gedeelte wordt gedemonstreerd hoe we dit gerealiseerd met behulp van Azure Machine Learning Studio.  

De met meerdere modellen benadering is een verplichte bij het ontwerpen van een globale archetype voor lospeuteren. Zelfs het score (blog) deel van de methode moet met meerdere modellen.  

In het volgende diagram ziet u het prototype die u hebt gemaakt, waarin vier score algoritmen in Machine Learning Studio lospeuteren voorspellen in dienst. De reden voor het gebruik van een met meerdere modellen benadering is niet alleen voor het maken van een ensemble classificatie om uit te breiden nauwkeurigheid, maar ook ter bescherming tegen te veel aanpassen en om duidelijke Functieselectie te verbeteren.  

![][3]

*Afbeelding 5: Prototype van een benadering modeling lospeuteren*  

De volgende secties vindt meer informatie over het model dat wordt geïmplementeerd via Machine Learning Studio scoren prototype.  

###<a name="data-selection-and-preparation"></a>Gegevens selecteren en voorbereiden
De gegevens worden gebruikt om de modellen te maken en score klanten is afkomstig van een verticale CRM-oplossing met de gegevens afgeschermd om klant privacy te beschermen. De gegevens bevatten informatie over 8000 abonnementen in de VS en drie bronnen worden gecombineerd: inrichting gegevens (metagegevens abonnement), activiteitsgegevens (gebruik van het systeem) en gegevens van de klant-ondersteuning. De gegevens bevat geen elk bedrijf gerelateerde gegevens over de klanten; bijvoorbeeld, bevat deze geen loyale metagegevens of creditnota scores.  

Voor eenvoudig, zijn ETL en processen opschonen van gegevens buiten het bereik omdat we wordt ervan uitgegaan dat gegevens voorbereiden al heeft plaatsgevonden ergens anders.   

Functieselectie voor modellering is gebaseerd op voorlopige significantie scoren van de set variabelen, opgenomen in het proces dat de module willekeurig bos gebruikt. We berekend voor de uitvoering in Machine Learning Studio, het gemiddelde, mediaan en bereiken voor vertegenwoordiger functies. We hebben toegevoegd, bijvoorbeeld aggregaties voor de kwalitatief gegevens, zoals de laagste en hoogste waarde voor de gebruikersactiviteit.    

We ook vastgelegd tijdelijke informatie voor de meest recente zes maanden. We gegevens voor één jaar geanalyseerd en we tot stand gebracht zelfs als er significantie trends was, wordt het effect op lospeuteren enorm na zes maanden afgenomen.  

De belangrijkste komma is dat het hele proces, inclusief ETL,-onderdelen selecteren, en de modeling is geïmplementeerd in Machine Learning Studio gegevensbronnen in Microsoft Azure gebruiken.   

De volgende diagrammen illustreren de gegevens die is gebruikt.  

![][4]

*Afbeelding 6: Fragment van een gegevensbron (afgeschermd)*  

![][5]


*Afbeelding 7: Functies die zijn opgehaald uit de gegevensbron*
 
> Houd er rekening mee dat deze gegevens privé is en daarom het model en gegevens kunnen niet worden gedeeld.
> Zie echter voor een soortgelijke model met openbaar gegevens, in dit voorbeeld experimenteren in de [Galerie met Cortana Intelligence](http://gallery.cortanaintelligence.com/): [Telco klant te produceren](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Meer informatie over hoe u een lospeuteren analyse model met Cortana Intelligence Suite kunt implementeren, ook aangeraden [in deze video](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) door hoofd programma Manager Westerse Hyong Tok. 
> 

###<a name="algorithms-used-in-the-prototype"></a>Algoritmen die in het prototype worden gebruikt

We de volgende vier machine learning algoritmen gebruikt om u te maken van het prototype (geen aanpassing):  

1.  Logistische regressie (LR)
2.  Gestimuleerd beslissingsstructuur (BT)
3.  Gemiddelde perceptron (Toegangspunt)
4.  Ondersteuning voor vector machine (SVM)  


In het volgende diagram ziet u een deel van het ontwerpvlak experiment, waarin de volgorde waarin de modellen zijn gemaakt wordt aangegeven:  

![][6]  


*Afbeelding 8: Modellen maken in Machine Learning Studio*  

###<a name="scoring-methods"></a>Score methoden
We een score voorzien van de vier modellen met behulp van een gegevensset gelabelde training.  

We ook de score gegevensset aan een vergelijkbare model dat is gemaakt met de bureaublad-editie van SA's Enterprise mijnwerkers 12 ingediend. We de nauwkeurigheid van het model SA's en alle vier Machine Learning Studio-modellen gemeten.  

##<a name="results"></a>Resultaten
In dit gedeelte presenteren we onze resultaten over de nauwkeurigheid van de modellen, op basis van de score gegevensset.  

###<a name="accuracy-and-precision-of-scoring"></a>Nauwkeurigheid en precisie scores
In het algemeen wordt de implementatie in Azure Machine Learning bevindt zich achter SA's in de nauwkeurigheid van ongeveer 10-15% (gebied onder Curve of AUC).  

De belangrijkste meetwaarde in lospeuteren is echter het tarief weer dat misclassification: dat wil zeggen van de bovenste N churners als voorspelde door de classificatie, welke van deze daadwerkelijk hebt **niet** lospeuteren, en nog speciale behandeling ontvangen? In het volgende diagram worden vergeleken dit tarief misclassification voor alle modellen:  

![][7]


*Afbeelding 9: Passau prototype gebied onder curve*

###<a name="using-auc-to-compare-results"></a>AUC gebruiken om resultaten te vergelijken
Gebied onder Curve (AUC) is een meting die een algemene maateenheid voor *Afscheidbaarheid* tussen de verdeling van scores voor positieve en negatieve populatie voorstelt. Dit is vergelijkbaar met de traditionele ontvanger Operator kenmerk (ROC)-grafiek, maar één belangrijk verschil is dat de meetwaarde AUC hoeft u niet de drempelwaarde kiezen. In plaats daarvan deze bevat een overzicht van de resultaten op **alle** mogelijke opties. Daarentegen de traditionele ROC graph geeft het tarief weer positieve op de verticale as en het tarief weer dat ONWAAR positieve op de horizontale as en de drempelwaarde voor classificatie varieert.   

AUC wordt meestal gebruikt als een maateenheid voor zegt voor verschillende algoritmen (of andere systemen voor) omdat hiermee modellen moet worden vergeleken met behulp van hun AUC waarden. Dit is een populaire manier in sectoren zoals meteorologie en biosciences. AUC staat dus voor een populaire hulpmiddel voor het beoordelen van classificatie prestaties.  

###<a name="comparing-misclassification-rates"></a>Vergelijking van misclassification tarieven
We vergeleken de misclassification rente op de betreffende gegevensset met behulp van de CRM-gegevens van ongeveer 8000 abonnementen.  

-   Het tarief weer dat misclassification SA's is 10-15%.
-   Het tarief weer dat misclassification Machine Learning Studio is 15-20% voor de churners boven 200-300.  

In de branche telecommunicatie is het belangrijk om alleen klanten die de hoogste risico hebben aan door te bieden ze een service concierge of andere speciale behandeling te produceren. De implementatie Machine Learning Studio realiseert daartoe resultaten op hetzelfde niveau staat het model SA's.  

Op overeenkomstige wijze, is nauwkeurigheid belangrijker dan de precisie omdat we belangstelling hebben voor voornamelijk correct classificeren mogelijke churners.  

In het volgende diagram van Wikipedia ziet u de relatie in een afbeelding levendige, eenvoudig te begrijpen:  

![][8]

*Afbeelding 10: Verhouding tussen de nauwkeurigheid en precisie van formules*

###<a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Nauwkeurigheid en precisie resultaten voor gestimuleerd beslissing boomstructuur  

Het volgende diagram wordt de onbewerkte resultaten van scoren met behulp van het prototype Machine Learning voor de gestimuleerd beslissing structuur-model, wat gebeurt er met de meest nauwkeurig tussen de modellen voor vier weergegeven:  

![][9]

*Afbeelding 11: Gestimuleerd beslissing structuur model kenmerken*

##<a name="performance-comparison"></a>Prestaties-vergelijking
We vergeleken de snelheid waarmee gegevens is behaald met het gebruik van de Machine Learning Studio-modellen en een vergelijkbare model dat is gemaakt met de bureaublad-editie van SA's Enterprise mijnwerkers 12.1.  

De volgende tabel worden de prestaties van de algoritmen:  

*Tabel 1. Algemene prestaties (nauwkeurigheid) van de algoritmen*

| LR|BT|TOEGANGSPUNT|SVM|
|---|---|---|---|
|Gemiddelde Model|Het beste Model|Attenderen|Gemiddelde Model|

De modellen die zijn ingesloten in een Machine Learning Studio ver-loop SA's met 15-25% voor de snelheid van worden uitgevoerd, maar de nauwkeurigheid grotendeels op nominale is.  

##<a name="discussion-and-recommendations"></a>Discussie en aanbevelingen
In de branche, de telecommunicatie verschillende procedures is ontstaan als u wilt analyseren lospeuteren, waaronder:  

-   Afgeleid van de doelstellingen voor vier fundamentele categorieën:
    -   **Entiteit (bijvoorbeeld een abonnement hebt)**. Basisinformatie over het abonnement en/of de klant die is het onderwerp van lospeuteren inrichten.
    -   **Activiteit**. Alle mogelijke gebruiksinformatie die zijn gerelateerd aan de entiteit, bijvoorbeeld het aantal aanmeldingen aanvragen.
    -   **Klantondersteuning**. Verzamel informatie van de klant Ondersteuningslogboeken om aan te geven of het abonnement problemen of interacties met de klantenondersteuning hadden.
    -   **Concurrentieanalyse en zakelijke gegevens**. Verkrijgen van mogelijke informatie over de klant (bijvoorbeeld kunnen zijn niet beschikbaar of moeilijk om bij te houden).
-   Gebruik urgentie station functie selectie. Dit betekent dat de boomstructuur gestimuleerd beslissing altijd een ordertoezegging aanpak is.  

Het gebruik van deze vier categorieën Hiermee maakt u de indruk dat een eenvoudige *deterministic* aanpak, op basis van indexen gevormd op redelijk factoren per categorie, voldoende moet om te bepalen voor klanten risico voor lospeuteren. Helaas, hoewel dit begrip aannemelijke lijkt, is het een onwaar lidmaatschap. De reden is dat lospeuteren een tijdelijke effect is en de factoren die bijdragen als u wilt produceren meestal tijdelijke Staten hebben. Wat u rekening moet houden vandaag verlaten van een klant leidt mogelijk anders morgen en deze zeker worden verschillende zes maanden vanaf nu. Daarom een *probabilistische* model is noodzakelijk.  

Deze belangrijke waarnemingen wordt vaak vergeten in bedrijf die in het algemeen in analytics liever een business intelligence-georiënteerd aanpak, voornamelijk omdat dit is een eenvoudiger verkopen en toegang verleent ingewikkelde automatisering.  

De toegezegd Self-service gebruiksanalyses met behulp van Machine Learning Studio is echter dat de vier gegevenscategorieën, ingedeeld in de afdeling een zeer handige bron voor machine leren over lospeuteren geworden.  

Een andere interessante functie binnenkort in Azure Machine Learning is de mogelijkheid om een aangepaste module toevoegen aan de bibliotheek van vooraf gedefinieerde modules die al beschikbaar zijn. Deze mogelijkheid, maakt in feite een verkoopkans selecteren van bibliotheken en sjablonen maken voor verticale markten. Het is een belangrijk kenmerk van Azure Machine Learning op de markt.  

We hopen dat om verder in dit onderwerp in de toekomst, met name zijn gerelateerd aan analyses van grote gegevens.
  
##<a name="conclusion"></a>Sluiten
Dit artikel wordt beschreven verstandig bevonden om het algemene probleem van de klant lospeuteren met behulp van een algemene kader. We beschouwd als een prototype voor het scoren modellen en deze met behulp van Azure Machine Learning geïmplementeerd. Wij beoordeeld ten slotte de nauwkeurigheid en prestaties van de oplossing prototype met betrekking tot vergelijkbare algoritmen in SA's.  

**Voor meer informatie:**  

Dit artikel heeft geholpen u? Geef ons uw feedback. Vertel ons op een schaal van 1 (slecht) tot en met 5 (bronnen met uitstekende), hoe beoordeelt u dit artikel en waarom hebt u gegeven om deze classificatie? Bijvoorbeeld:  

-   Bent u beoordeling deze hoog vanwege ondervindt goede voorbeelden, bronnen met uitstekende schermopnamen, schakel het selectievakje schrijven, of een andere reden waarom?
-   Bent u beoordeling deze lage vanwege slechte voorbeelden, bij benadering schermopnamen of onduidelijk schrijven?  

Deze feedback kunnen we de kwaliteit van white papers over die we los te verbeteren.   

[Feedback verzenden](mailto:sqlfback@microsoft.com).
 
##<a name="references"></a>Verwijzingen
Bekijk analyses van [1] : voorbij de informatiebeheer voorspellingen, West McKnight, juli/augustus 2011, p.18-20.  

[2] Wikipedia-artikel: [nauwkeurigheid en precisie van formules](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [SCHERPE-DM 1.0: stapsgewijze datamining Guide](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [big Data Marketing: uw klanten effectiever deelnemen en waarde station](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco te produceren model sjabloon](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) in de [Galerie met Cortana bedrijfsinformatie](http://gallery.cortanaintelligence.com/) 
 
##<a name="appendix"></a>Bijlage

![][10]

*Figuur 12: Momentopname van een presentatie op lospeuteren prototype*


[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
