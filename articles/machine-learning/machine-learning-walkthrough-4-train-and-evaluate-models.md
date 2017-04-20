<properties
    pageTitle="Stap 4: Trainen en evalueren van de blog analytische modellen | Microsoft Azure"
    description="Stap 4 van de prognose blog oplossing Stapsgewijze instructies: trein, score en meerdere modellen in Azure Machine Learning Studio evalueren."
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
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Stapsgewijze instructies stap 4: Trainen en de blog analytische modellen evalueren

Dit onderwerp krijgt u de vierde stap van de procedure, [een blog analytics-oplossing in Azure Machine Learning ontwikkelen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Een Machine Learning-werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3.  [Maak een nieuwe experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  **Trainen en de modellen voor evalueren**
5.  [De webservice implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Toegang tot de webservice](machine-learning-walkthrough-6-access-web-service.md)

----------

Een van de voordelen van het gebruik van Azure Machine Learning Studio voor het maken van machine learning-modellen is de mogelijkheid om te proberen meer dan één type model op een moment in een experiment en vergelijk de resultaten. Dit soort experimenten kunt u de beste oplossing te vinden voor uw probleem.

Klik in het experiment dat we in dit scenario ontwikkelt, we twee verschillende soorten modellen maken en hun score resultaten om te bepalen welke algoritme we wilt gebruiken in ons definitief experiment vergelijken.  

Er zijn verschillende modellen die we kan kiezen uit. Als u wilt zien van de beschikbare modellen, vouw het knooppunt **Machine Learning** in het palet module en vouwt u **Geïnitialiseerd Model** en de knooppunten eronder. Voor de toepassing van dit experiment wordt wordt we selecteert u de ondersteuning Vector Machine (SVM) en de twee-klasse verhoogd beslissingsbomen modules.    

> [AZURE.TIP] Als u hulp bij het bepalen welke Machine Learning algoritme van de beste past bij het specifieke probleem dat u wilt oplossen, raadpleegt u [het algoritmen voor Microsoft Azure Machine Learning kiezen](machine-learning-algorithm-choice.md).

##<a name="train-the-models"></a>De modellen voor trainen
Eerst de boomstructuur gestimuleerd beslissing instellen:  

1.  Zoek de [Twee-klasse verhoogd beslissingsstructuur] [ two-class-boosted-decision-tree] module in het palet module en sleep deze naar het canvas.
2.  Zoeken naar de [Trein Model] [ train-model] module, sleep deze naar het tekenpapier en vervolgens de uitvoer van de module van de structuurweergave gestimuleerd beslissing verbinden met de links invoer poort ("ongetrainde model') van de [Trein Model] [ train-model] module.
    
    De [Twee-klasse verhoogd beslissingsstructuur] [ two-class-boosted-decision-tree] module geïnitialiseerd de algemene model, en het [Model trainen] [ train-model] trainingsgegevens gebruikt om te trainen van het model. 
     
3.  Verbinding maken met de linker-uitvoer ('resultaat gegevensset") van de linker- [R-Script uitvoeren] [ execute-r-script] module rechts input poort ("gegevensset") van de [Trein Model] [ train-model] module.

    > [AZURE.TIP] We hebben geen nodig twee van de invoer en een van de uitvoer van het [R-Script uitvoeren] [ execute-r-script] -module voor dit experiment, zodat we deze niet-vastgemaakte laat. 

4.  Selecteer het [Model trein] [ train-model] module. Klik in het deelvenster **Eigenschappen** klikt u op **kolom Gegevenskiezer starten**, selecteert u de **Alle typen** in de vervolgkeuzelijst omlaag onder **Beschikbare kolommen** en voert "creditering risico' in het tekstvak. Selecteer **Alle typen** in de vervolgkeuzelijst onder de **Geselecteerde kolommen**. Selecteer "Creditering risico" en klik op de gemarkeerde pijlknop om deze te verplaatsen naar de **Geselecteerde kolommen**. 
5.  Klik op **Opslaan**.


Dit gedeelte van het experiment er nu ongeveer zo uitziet:  

![Een model training][1]

Vervolgens stelt in het model SVM.  

Eerste, uitleg over SVM. Gestimuleerd beslissingsbomen werken ook met de functies van elk type. Aangezien de module SVM een lineaire classificatie genereert, heeft het model die wordt gegenereerd echter de beste testfout wanneer alle numerieke functies dezelfde schaal hebben. Als u wilt converteren alle numerieke functies op dezelfde schaal, gebruiken we een "Tanh"-transformatie (met de [Gegevens normaliseren] [ normalize-data] module.) Hiermee transformeert onze getallen in het bereik [0,1]. Tekenreeks-functies worden geconverteerd door de module SVM naar bestaan functies en vervolgens naar binaire 0-1-functies, zodat we niet hoeven te transformeren handmatig tekenreeks functies. Bovendien we niet wilt transformeren van de creditcard risico kolom (kolom 21) - numeriek is, maar is de waarde die we bent het model te voorspellen zijn, training, zodat moeten we dit ongewijzigd laten.  

Ga als volgt te werk als u het model SVM instelt:

1.  Zoek de [Twee-klasse ondersteuning Vector Machine] [ two-class-support-vector-machine] module in het palet module en sleep deze naar het canvas.
2.  Met de rechtermuisknop op de [Trein Model] [ train-model] module, selecteer **kopiëren**, en klik vervolgens met de rechtermuisknop op het tekenpapier en selecteer **Plakken**. De kopie van het [Model trein] [ train-model] module heeft dezelfde kolomselectie als de oorspronkelijke.
3.  De uitvoer van de module SVM verbinden met de links invoer poort ("ongetrainde model') van de tweede [Trein Model] [ train-model] module.
4.  De [Gegevens normaliseren] zoeken[ normalize-data] module en sleep deze naar het canvas.
5.  De invoer van deze module verbinden met de links uitvoer van het links [R-Script uitvoeren] [ execute-r-script] module (Let erop dat de uitvoerpoort van een module kan niet worden verbonden met meer dan één andere module).
6.  Verbinding maken met de links uitvoerpoort ("getransformeerd gegevensset") van de [Gegevens normaliseren] [ normalize-data] module rechts input poort ("gegevensset") van de tweede [Trein Model] [ train-model] module.
7.  In het deelvenster **Eigenschappen** voor de [Gegevens normaliseren] [ normalize-data] module, selecteer **Tanh** voor de parameter **transformatiemethode** .
8.  Klik op **kolom Gegevenskiezer starten**"Geen kolommen" selecteren voor **Begint met**, schakelt u **opnemen** in de eerste vervolgkeuzelijst, selecteer **kolomtype** in de tweede vervolgkeuzelijst en **numerieke** selecteren in de derde vervolgkeuzelijst. Hiermee geeft u alle numerieke kolommen (en alleen numerieke) zijn getransformeerd.
9.  Klik op het plusteken (+) rechts van de rij - Hiermee maakt u een rij van vervolgkeuzelijsten. Selecteer **uitsluiten** in de eerste vervolgkeuzelijst **kolomnamen** in de tweede vervolgkeuzelijst en klik op het tekstvak en selecteer "creditering risico' in de lijst met kolommen. Hiermee wordt aangegeven dat de kolom krediet moet worden genegeerd (we moet u dit wilt doen omdat deze kolom numerieke is en dus anders zou worden getransformeerd).
10. Klik op **OK**.  


De [Gegevens normaliseren] [ normalize-data] module nu is ingesteld op het uitvoeren van een transformatie Tanh op alle numerieke kolommen, behalve voor de kolom kredietrisico.  

Dit gedeelte van onze experiment ziet er nu ongeveer als volgt te werk:  

![Het tweede model training][2]  

##<a name="score-and-evaluate-the-models"></a>Score en de modellen voor evalueren

We gebruiken de testen gegevens die door de [Gesplitste gegevens] is gescheiden[ split] module voor het verkrijgen van onze ervaren-modellen. Vervolgens kunnen we de resultaten van de twee modellen om te zien welke betere resultaten gegenereerd vergelijken.  

1.  Zoek het [Score Model] [ score-model] module en sleep deze naar het canvas.
2.  Verbinding maken met het [Model trein] [ train-model] module die is gekoppeld aan de [Twee-klasse verhoogd beslissingsstructuur] [ two-class-boosted-decision-tree] module links input poort van het [Model Score] [ score-model] module.
3.  Verbinding maken met de juiste invoer poort van het [Model Score] [ score-model] module aan de linkerkant uitvoer van het juiste [R-Script uitvoeren] [ execute-r-script] module.

    Het [Model Score] [ score-model] module kunt nu de creditcard gegevens uit de testen gegevens, voeren via het model en de voorspellingen het model wordt gegenereerd met de kolom van de werkelijke krediet risico in de testen gegevens vergelijken.

4.  Kopieer en plak het [Score Model] [ score-model] module wilt maken van een tweede exemplaar of sleept u een nieuwe module op het tekenpapier.
5.  Verbinding maken met de links invoer poort van deze module aan het model SVM (dat wil zeggen verbinding maken met de uitvoerpoort van het [Model trein] [ train-model] module die gekoppeld aan de [Twee-klasse ondersteuning Vector Machine] [ two-class-support-vector-machine] module).
6.  We hebben de dezelfde transformatie naar de testgegevens doen omdat we hebben gedaan met de trainingsgegevens voor het model SVM. Dus kopieer en plak de [Gegevens normaliseren] [ normalize-data] module maken van een tweede exemplaar en maak verbinding met de links uitvoer van het juiste [R-Script uitvoeren] [ execute-r-script] module.
7.  Verbinding maken met de juiste invoer poort van het [Model Score] [ score-model] module aan de linkerkant uitvoer van de [Gegevens normaliseren] [ normalize-data] module.  

Als u wilt evalueren de twee score resultaten, gebruiken we een [Model evalueren] [ evaluate-model] module.  

1.  Het [Model evalueren] vinden[ evaluate-model] module en sleep deze naar het canvas.
2.  De links invoer poort verbinden met de uitvoerpoort van het [Model Score] [ score-model] module die is gekoppeld aan het model van de structuurweergave gestimuleerd beslissing.
3.  Verbinding maken met de juiste invoer poort aan het andere [Score Model] [ score-model] module.  

Als u wilt het experiment uitvoeren, klikt u op de knop **uitvoeren** onder het tekenpapier. Het kan enkele minuten duren. Een draaiende-indicator in elke module blijkt dat deze wordt uitgevoerd, waarna een groen vinkje wanneer de module is voltooid. Wanneer alle modules ingeschakeld hebt, is het experiment uitgevoerd.

Het experiment ziet er nu ongeveer zo uitziet:  

![Evaluatie van beide modellen][3]

Als u wilt controleren of de resultaten, klikt u op de uitvoerpoort van het [Model evalueren] [ evaluate-model] module en selecteer **visualiseren**.  

Het [Model evalueren] [ evaluate-model] module genereert een paar bochten en aan de doelstellingen waarmee u kunt de resultaten van de twee scored modellen vergelijken. U kunt de resultaten als ontvanger Operator kenmerk (ROC) bochten, precisie/intrekken bochten of Lift bochten weergeven. Aanvullende gegevens worden weergegeven, bevat een matrix verwarring, cumulatieve waarden voor het gebied onder de kromme (AUC) en andere aan de doelstellingen. U kunt de drempelwaarde wijzigen met de schuifregelaar naar links of rechts en Zie hoe dit van invloed is op het instellen van de doelstellingen.  

Aan de rechterkant van de grafiek, klikt u op **Scored dataset** of **Scored gegevensset om te vergelijken** de bijbehorende curve markeren en de bijbehorende aan de doelstellingen hieronder weergegeven. Klik in de legenda voor de bochten "Behaald gegevensset" overeenkomt met de links invoer poort van het [Model evalueren] [ evaluate-model] module - in onze hoofdletters/kleine letters, is dit de boomstructuur gestimuleerd beslissing. "Behaald gegevensset om te vergelijken" komt overeen met de juiste invoer poort - het model SVM in onze hoofdletters/kleine letters. Wanneer u op een van deze labels, wordt de curve voor dat model wordt gemarkeerd en de bijbehorende cijfers bevatten, zoals in de volgende afbeelding.  

![ROC bochten voor modellen][4]

Met deze waarden gecontroleerd, kunt u aangeven welk model zich het dichtst bij zodat u de resultaten die u zoekt. U kunt teruggaan en doorgaan met het ontwikkelen van uw experiment door waarden in de verschillende modellen te wijzigen. 

> [AZURE.TIP] Telkens wanneer u een record van die iteratie het experiment uitvoert wordt in de geschiedenis van uitvoeren bewaard. U kunt deze iteraties weergeven en terugkeren naar elk van deze door te klikken op **Uitvoeren geschiedenis weergeven** onder het tekenpapier. U kunt ook klikken op **Voorafgaande uitvoeren** in het deelvenster **Eigenschappen** om terug te keren naar de voorgaande iteratie het thema dat u hebt geopend.
> 
U kunt een kopie van elk iteratie van uw experiment aanbrengen door te klikken op **Opslaan als** onder het tekenpapier. Eigenschappen voor het **Overzicht** en **Beschrijving** van het experiment van gebruiken om een record van wat u hebt geprobeerd in uw iteraties experiment.

>  Zie [beheren experimenteren iteraties in Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md)voor meer informatie.  


----------

**Volgende: [Deploy de webservice](machine-learning-walkthrough-5-publish-web-service.md)**

[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train1.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train2.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train3.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train4.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
