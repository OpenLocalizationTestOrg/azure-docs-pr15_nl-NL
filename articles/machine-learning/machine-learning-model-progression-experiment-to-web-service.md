<properties
    pageTitle="Hoe een Machine Learning-model beweegt van een experiment met een operationalized webservice | Microsoft Azure"
    description="Een overzicht van het mechanisme van hoe de voortgang van uw Azure Machine Learning-model van een ontwikkel met een operationalized webservice experimenteren."
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


# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Hoe een Machine Learning-model beweegt van een experiment met een operationalized webservice

Azure Machine Learning Studio biedt een interactieve tekenpapier waarmee u ontwikkelen, uitvoeren, testen en doorgaan met het ontwikkelen een ***experimenteren*** dat staat voor een blog analyse-model. Er zijn tal van modules die kunt:

* Invoergegevens in uw experiment
* De gegevens bewerken
* Een model met machine learning algoritmen trainen
* Het model score
* De resultaten evalueren
* Uitvoer definitief waarden

Als u tevreden met uw experiment bent, kunt u deze als een ***klassieke Azure Machine Learning-webservice*** of een ***nieuwe Azure Machine Learning-webservice*** implementeren zodat gebruikers kunnen deze nieuwe gegevens verzenden en achterste resultaten ontvangen.

In dit artikel geven we een overzicht van het mechanisme van hoe de voortgang van uw Machine Learning-model van een ontwikkel met een operationalized webservice experimenteren.

>[AZURE.NOTE] Er zijn andere manieren om te ontwikkelen en implementeren van machine learning-modellen, maar in dit artikel is gericht op hoe u Machine Learning Studio gebruiken. Zie het blogbericht [bouwen en implementeren blog Web Apps gebruiken RStudio en Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx)voor informatie over het maken van een klassieke blog webservice met R.

Azure Machine Learning Studio is ontworpen om te ontwikkelen en een *blog analyse model*implementeert, is het mogelijk te Studio gebruiken voor het ontwikkelen van een experiment die niet zijn opgenomen in een blog analyse-model. Een experiment mogelijk bijvoorbeeld alleen gegevens invoeren, bewerken en klik vervolgens op de resultaten. Net als een experiment blog analyse kunt u dit niet-blog experiment als een webservice implementeren, maar het is een eenvoudiger proces omdat het experiment niet is training of scoren een machine learning-model. Het is niet de gewone Studio op deze manier gebruiken, wordt we deze opnemen in de discussie zodat we een volledige uitleg over de werking van Studio kunt geven.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Ontwikkelen en implementeren van een blog webservice

Hier volgen de fasen die volgt op een normale oplossing wanneer u ontwikkelt en het gebruik van Machine Learning Studio dashboard implementeren:

![Implementatie-mailstroom](media\machine-learning-model-progression-experiment-to-web-service\model-stages-from-experiment-to-web-service.png)

*Afbeelding 1 - fasen van een typisch blog analyse-model*

### <a name="the-training-experiment"></a>Het experiment training

De ***training experimenteren*** is de eerste fase van het ontwikkelen van uw webservice in Machine Learning Studio. Het doel van het experiment training is dat u een locatie om te ontwikkelen, testen, doorgaan met het ontwikkelen en uiteindelijk trainen een machine learning-model. U kunt zelfs trainen meerdere modellen tegelijk als u naar de beste oplossing zoeken, maar wanneer u klaar bent met één training als u een experimenteren u selecteren gaat model en de rest van het experiment verwijderen. Voor een voorbeeld van de ontwikkeling van een blog analyse experimenteren, raadpleegt u [een blog analytics-oplossing voor krediet risicobeoordeling in Azure Machine Learning ontwikkelen](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>Het blog experiment

Wanneer u een ervaren model in uw experiment training hebt, klikt u op **Webservice instellen** en selecteer **Blog webservice** in Machine Learning Studio om het proces van uw experiment training converteren naar een ***blog experiment***start. Het doel van het blog experiment is uw ervaren model gebruiken voor het verkrijgen van nieuwe gegevens, met het doel van uiteindelijk steeds geoperationaliseerd als een Azure-webservice.

Deze conversie klaar is voor u door de volgende stappen:

-   De set modules die wordt gebruikt voor training in een enkel module converteren en opslaan als een ervaren model

-   Alle overbodige modules die niet zijn gerelateerd aan scoren verwijderen

-   Invoer- en uitvoerbereik poorten die worden gebruikt door de uiteindelijke webservice toevoegen

Er zijn mogelijk meer wijzigingen die u aanbrengen wilt in uw blog experiment voorbereiden om te implementeren als een webservice. Als u wilt dat de webservice om alleen een subset van resultaten, kunt u bijvoorbeeld een filteren module voordat u de uitvoerpoort toevoegen.

In dit conversieproces het experiment training niet worden verwijderd. Wanneer het proces voltooid is, hebt u twee tabbladen in Studio: één voor het experiment training en één voor de blog experiment. Deze methode die u wijzigingen in het experiment training aanbrengen kunt voordat u uw webservice implementeren en opnieuw het blog experiment opbouwen. Of u een kopie van het experiment training om een andere regel van experimenten te beginnen kunt opslaan.

>[AZURE.NOTE] Wanneer u klikt op **Blog webservice waarmee** u een automatische starten als u wilt uw experiment training omzetten in een blog experiment en dit ook in de meeste gevallen werkt. Als uw experiment training complexe is (bijvoorbeeld: u hebt meerdere paden voor training die u samenvoegen), misschien wilt u deze conversie handmatig doen. Zie [een experiment Machine Learning training een blog experiment converteren](machine-learning-convert-training-experiment-to-scoring-experiment.md)voor meer informatie.

### <a name="the-web-service"></a>De webservice

Als u tevreden bent dat uw blog experiment klaar is, u uw service als een klassieke webservice implementeren kunt of een nieuwe webservice op basis van Azure resourcemanager. Als u wilt mogelijk met het maken van uw model door deze te implementeren als *klassieke Machine Learning-webservice*, klik op **Webservice implementeren** en selecteer **Webservice implementeren [klassieke]**. Als u wilt implementeren als *nieuwe Machine Learning-webservice*, klik op **Webservice implementeren** en selecteer **Implementeren webservice [Nieuw]**. Gebruikers kunnen nu gegevens naar uw model met de webservice REST API verzenden en ontvangen weer de resultaten. Lees [hoe u het gebruiken van een Azure Machine Learning-webservice die is geïmplementeerd vanuit een Machine Learning-experiment](machine-learning-consume-web-services.md)voor meer informatie.

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Niet-meestal: een niet-blog webservice maken

Als uw experiment bevat geen training een blog analyse model en klik u hoeft niet te maken van een experiment training zowel een score experiment: Er is maar één experiment en kunt u deze implementeren als een webservice. Machine Learning Studio wordt gedetecteerd of uw experiment een blog model bevat door een analyse van de modules die u hebt gebruikt.

Nadat u hebt herhaald op uw experiment en tevreden met deze bent:

1.  Klik op **Webservice instellen** en selecteer **Webservice bijscholing** - invoer- en uitvoerbereik knooppunten worden automatisch toegevoegd

2.  Klik op **uitvoeren**

3. Klik op **Webservice implementeren** en selecteer **Webservice implementeren [klassieke]** of **Implementeren webservice [Nieuw]** afhankelijk van de omgeving waarnaar u wilt implementeren.

Uw webservice wordt nu geïnstalleerd en u kunt openen en deze net als een blog webservice beheren.

## <a name="updating-your-web-service"></a>Bijwerken van uw webservice

Nu u hebt uw experiment als een webservice geïmplementeerd, wat gebeurt er als moet u deze bij te werken?

Die afhankelijk van wat u moet bijwerken:

**U wilt wijzigen van de invoer of uitvoer, of u wilt wijzigen hoe gegevens worden bewerkt door de webservice**

Als u niet van het model wijzigen bent, maar alleen wilt wijzigen hoe gegevens worden verwerkt door de webservice, kunt u het blog experiment bewerken en vervolgens klikt u op **Webservice implementeren** en selecteer **Webservice implementeren [klassieke]** of **Implementeren webservice [Nieuw]** opnieuw. De Web-service is gestopt, de bijgewerkte blog experiment wordt geïmplementeerd en de webservice opnieuw is opgestart.

Hier volgt een voorbeeld: Stel dat uw blog experiment geeft als resultaat de hele rij van invoergegevens met het verwachte resultaat. U ervoor kiezen dat u wilt dat de webservice om terug te keren alleen het resultaat. Zo kunt u een module **Project-kolommen** toevoegen in het blog experiment, rechts voordat de uitvoerpoort kolommen dan het resultaat uitsluiten. Wanneer u op **Webservice implementeren** en selecteer opnieuw **Webservice implementeren [klassieke]** of **Implementeren webservice [Nieuw]** , wordt de webservice bijgewerkt.

**U wilt het model met nieuwe gegevens retrain**

Als u wilt bewaren van uw machine learning-model, maar u wilt deze retrain met nieuwe gegevens, hebt u twee mogelijkheden:

1.  **Het model terwijl de webservice wordt uitgevoerd retrain** - als u wilt uw model terwijl de blog retrain webservice wordt uitgevoerd, kunt u dit doen door enkele wijzigingen in het experiment training zodat u een ***opnieuw opleiden experiment***aan en klik vervolgens kunt u deze implementeert als ** *opnieuw opleiden* webservice**. Zie [Retrain Machine Learning model via een programma](machine-learning-retrain-models-programmatically.md)voor instructies over hoe u dit wilt doen.

2.  **Ga terug naar de oorspronkelijke training experiment en gegevens van verschillende training gebruiken voor het ontwikkelen van uw model** - uw blog experiment is gekoppeld aan de webservice, maar het experiment training rechtstreeks niet op deze manier is gekoppeld. Als u het oorspronkelijke training experiment wijzigen en klik op **Web-Service instellen**, deze wordt er een *nieuwe* blog experimenteren die wanneer geïmplementeerd, wordt er een *nieuwe* webservice. Deze wordt niet alleen de oorspronkelijke webservice bijgewerkt.

    Als u nodig hebt voor het wijzigen van het experiment training, openen en klik op **OpslaanAls** om een kopie te maken. Hiermee wordt het oorspronkelijke training experiment, bekijk experiment behouden en webservice. U kunt nu een nieuwe webservice maken met uw wijzigingen. Nadat u de nieuwe webservice die u nu beslissen of u kunt wilt stoppen met het vorige webservice of te laten samen met de nieuwe sectie hebt geïmplementeerd.

**U wilt een ander model trainen**

Als u wijzigingen aanbrengen aan uw oorspronkelijke blog experiment wilt, zoals het selecteren van een andere machine learning-algoritme probeert een methode voor verschillende training, enzovoort, moet u naar de tweede procedure hierboven beschreven voor uw model bijscholing: open het experiment training, klikt u op **OpslaanAls** om een kopie maken en start u het nieuwe pad van het ontwikkelen van uw model , het blog experiment maken en implementeren van de webservice. Hiermee maakt u een nieuw Web service niet gerelateerde naar de oorspronkelijke - kunt u bepalen welke monitor (of beide blijven uitvoeren).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het proces van het ontwikkelen en experiment:

-   het experiment - [converteren van een experiment Machine Learning training een blog experiment](machine-learning-convert-training-experiment-to-scoring-experiment.md) converteren

-   de webservice - [Deploy een Azure Machine Learning-webservice](machine-learning-publish-a-machine-learning-web-service.md) implementeren

-   het model - [Retrain Machine Learning model via programmacode](machine-learning-retrain-models-programmatically.md) bijscholing

Zie het volgende voor voorbeelden van het hele proces:

-   [Machine learning zelfstudie: uw eerste experiment maken in Azure Machine Learning Studio](machine-learning-create-experiment.md)

-   [Demonstratie: Een blog analytics-oplossing voor de risicobeoordeling krediet in Azure Machine Learning ontwikkelen](machine-learning-walkthrough-develop-predictive-solution.md)