<properties
    pageTitle="Problemen met de Retraining van een Azure Machine Learning klassieke webservice | Microsoft Azure"
    description="Opsporen en corrigeren van veelvoorkomende problemen er wanneer u het model zijn omscholing voor een Azure Machine Learning-webservice."
    services="machine-learning"
    documentationCenter=""
    authors="VDonGlover"
   manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Problemen met de bijscholing van een Azure Machine Learning klassieke webservice

## <a name="retraining-overview"></a>Bijscholing overzicht

Wanneer u een blog experiment als score webservice implementeert is het een statische model. Zodra er nieuwe gegevens beschikbaar of wanneer de consument van de API hun eigen gegevens bevat, moet het model worden retrained. 

Zie voor een volledige stapsgewijze instructies voor het opnieuw opleiden proces voor een klassiek webservice, [Retrain Machine Learning modellen via programmacode](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Bijscholing proces

Wanneer u de webservice retrain moet, moet u enkele extra onderdelen toevoegen:

* Een webservice geïmplementeerd vanuit het Experiment Training. Het experiment moet een module **Web Service uitvoer** is gekoppeld aan de uitvoer van de module **Trein Model** .  

    ![De uitvoer van de service web toevoegen aan het model trein.][image1]

* Een nieuw eindpunt toegevoegd aan uw score webservice.  U kunt het eindpunt programmacode met de voorbeeldcode waarnaar wordt verwezen in de Retrain Machine Learning-modellen via programmacode toevoegen onderwerp of via de portal van Azure klassieke.

Vervolgens kunt u in de steekproef C#-code van de Training-webservice API help-pagina retrain model. Nadat u de resultaten hebt geëvalueerd en tevreden met bent, kunt u het ervaren model scoren webservice met het nieuwe endpoint die u hebt toegevoegd bijwerken.

Met alle delen op hun plaats staan zijn de belangrijkste stappen die u uitvoeren moet om het model retrain als volgt:

1.  Roep de webservice Training: het gesprek is aan de Batch Execution Service (BES), niet de aanvragen antwoord Service (records). U kunt de steekproef C#-code op de pagina API help om het gesprek te starten. 
2.  De waarden voor de *BaseLocation*, *RelativeLocation*en *SasBlobToken*vinden: deze waarden worden geretourneerd door de uitvoer van de oproep door naar de webservice Training. 
      ![de uitvoer van de steekproef opnieuw opleiden en de BaseLocation, RelativeLocation en SasBlobToken waarden weergegeven.][image6]
3.  De toegevoegde eindpunt van de score webservice bijwerken met het nieuwe ervaren model: gebruik van de steekproef code in de Retrain Machine Learning-modellen via programmacode, het nieuwe eindpunt dat u hebt toegevoegd aan het score model met het zojuist ervaren model van de webservice Training bijwerken.

## <a name="common-obstacles"></a>Algemene belemmering

### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Controleer als beschikt u over de juiste PATCH-URL

De URL PATCH die u gebruikt, moet het account dat is gekoppeld aan het nieuwe score eindpunt dat u hebt toegevoegd aan de score webservice. Er zijn een aantal manieren om te verkrijgen van de URL PATCH:

**Optie 1: via programmacode**

De juiste PATCH URL opvragen:

1.  De code van de steekproef [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) uitvoeren.
2.  De waarde *HelpLocation* uit de resultaten van AddEndpoint, en kopieer de URL.

    ![HelpLocation in de uitvoer van de steekproef addEndpoint.][image2]

3.  Plak de URL in een browser om te navigeren naar een pagina met help-koppelingen voor de webservice.
4.  Klik op de koppeling **Bijwerken Resource** om de patch help-pagina te openen.

**Optie 2: De Azure klassieke-portal gebruiken**

1.  Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).
2.  Open het tabblad Machine Learning. 
     ![Machine bijvoorbeeld tabblad.][image4]
3.  Klik op de naam van de werkruimte, klikt u vervolgens **Webservices**.
4.  Klik op de score webservice waarmee die u werkt. (Als u de standaardnaam van de webservice niet hebt wijzigt, wordt deze eindigt op [scoren Exp.].)
5.  Klik op **toevoegen eindpunt**.
6.  Nadat het eindpunt is toegevoegd, klikt u op de naam van het eindpunt. Klik vervolgens op **Resource bijwerken** om de patch Helppagina te openen.

>[AZURE.NOTE] Als u het eindpunt aan de webservice Training in plaats van de blog webservice hebt toegevoegd, ontvangt u het volgende foutbericht weergegeven wanneer u klikt op de koppeling **Bijwerken Resource** : geïnstalleerd maar deze functie wordt niet ondersteund of beschikbaar in deze context. Deze webservice heeft geen resources die kunnen worden bijgewerkt. We verontschuldigen ons voor het ongemak en over het verbeteren van deze werkstroom werkt.

![Nieuw eindpunt dashboard.][image3]

De PATCH help-pagina bevat de PATCH URL die u moet gebruiken en voorbeeldcode die u kunt Roep deze bevat.

![Patch-URL.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Controleer dat u het juiste score eindpunt wilt bijwerken

* Voer de webservice Training niet patch: de bewerking patch moet worden uitgevoerd op de score webservice.
* De standaardeindpunt op webservice kan niet worden hersteld: de bewerking patch moet worden uitgevoerd op het nieuwe score Web service-eindpunt dat u hebt toegevoegd.

U kunt welke webservice waarmee het eindpunt is ingeschakeld door een bezoek de portal van Azure klassieke te verifiëren. 

>[AZURE.NOTE] Zorg ervoor dat u het eindpunt toevoegt aan de blog webservice, niet de webservice Training. Als u zowel een Training en een blog webservice correct hebben geïnstalleerd, ziet u twee verschillende webservices worden vermeld. De blog webservice moet eindigen met "[blog exp.]".

1.  Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).
2.  Open het tabblad Machine Learning. 
     ![Machine learning werkruimte UI.][image4]
3.  Selecteer uw werkruimte.
4.  Klik op **webservices**.
5.  Selecteer uw blog webservice.
6.  Controleer of uw nieuwe eindpunt is toegevoegd aan de webservice.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>De werkruimte die uw webservice in zodat deze in de juiste regio controleren

1.  Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).
2.  Selecteer Machine Learning in het menu.
      ![Machine learning regio UI.][image4]
3.  Controleer of de locatie van uw werkruimte.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png