<properties
    pageTitle="Gebruik een Machine Learning-webservice met een web app-sjabloon | Microsoft Azure"
    description="Gebruik een web app-sjabloon in Azure Marketplace een blog webservice in Azure Machine Learning in beslag neemt."
    keywords="webservice, uitoefening, REST API, machine learning"
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
    ms.date="10/10/2016"
    ms.author="garye;raymondl"/>

# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Een Azure Machine Learning-webservice met een web app-sjabloon gebruiken

>[AZURE.NOTE] In dit onderwerp wordt beschreven technieken voor een klassiek webservice. 

Eenmaal u hebt uw blog model ontwikkeld en deze als een Azure webservice met Machine Learning Studio geïmplementeerd, of met hulpprogramma's zoals R of Python, kunt u het operationalized model met een REST API openen.

Er zijn een aantal manieren om te gebruiken van de REST API en toegang tot de webservice. U kunt bijvoorbeeld een toepassing schrijven in C#, R of Python met behulp van de steekproef-code voor u gegenereerd wanneer u de webservice (beschikbaar op de pagina API Help in het dashboard van de web-service in Machine Learning Studio) geïmplementeerd. Of u kunt de steekproef Microsoft Excel-werkmap die voor u gemaakt voor (ook beschikbaar in het dashboard van de web-service in Studio).

Maar de snelste en eenvoudigste manier om te krijgen tot uw webservice is via de Web-App sjablonen die beschikbaar zijn in de [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>De Azure Machine Learning-Web App-sjablonen

Een aangepaste web-app de invoergegevens en de verwachte resultaten van uw webservice kent kunnen maken op het web app-sjablonen beschikbaar in de Azure Marketplace. U hoeft te is de web app toegang geven tot uw web-service en gegevens en doet de rest van de sjabloon.

Twee sjablonen zijn beschikbaar:

- [Azure ML aanvraag en respons Service Web App-sjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Azure ML Batch Execution Service Web App-sjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Elke sjabloon wordt gemaakt van een steekproef ASP.NET-toepassing, met de API-URI en de toets voor de webservice, en deze als een website Azure implementeren. De aanvraag en respons Service (records)-sjabloon maakt u een web-app u één rij met gegevens verzenden naar de webservice kunt om een enkel resultaat. De Batch Execution Service (BES)-sjabloon maakt u een web-app u veel rijen met gegevens kunt om meerdere resultaten verzenden.

Geen kleurcodering is vereist voor het gebruik van deze sjablonen. U gewoon het API-URI en de sleutel en de sjabloon wordt de toepassing voor u gemaakt.

## <a name="how-to-use-the-request-response-service-rrs-template"></a>Het gebruik van de aanvraag en respons Service (records)-sjabloon

Zodra u hebt uw webservice geïmplementeerd, kunt u de onderstaande stappen om de records web app-sjabloon, gebruiken, zoals wordt weergegeven in het volgende diagram volgen.

![Proces voor het records websjabloon gebruiken][image1]

1. Open het tabblad **Webservices** Machine Learning Studio, en open vervolgens de webservice die u wilt openen. De toets wordt vermeld onder **API-sleutel** kopiëren en opslaan.

    ![API-sleutel][image3]

2. Open de **Aanvraag en respons** API Help-pagina. Boven aan de Helppagina bij **aanvragen**, Kopieer de waarde **URI aanvragen** en sla deze. Deze waarde er als volgt uit:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![URI aanvragen][image4]

3. Ga naar de [Azure-portal](https://portal.azure.com), **aanmelden**, klik op **Nieuw**, zoeken naar en selecteer **Azure ML aanvraag en respons Service Web App**en klik op **maken**. 

    - Geef uw web-app een unieke naam. De URL van de web-app worden deze naam gevolgd door `.azurewebsites.net.` bijvoorbeeld`http://carprediction.azurewebsites.net.`

    - Selecteer de Azure-abonnement en -services, waaronder uw webservice wordt uitgevoerd.

    - Klik op **maken**.

    ![WebApp maken][image5]

4. Als Azure is uitgevoerd met het implementeren van de web-app, klikt u op de **URL** op de pagina web app-instellingen in Azure wordt aangegeven of typ de URL in een webbrowser. Bijvoorbeeld:`http://carprediction.azurewebsites.net.`

5. Wanneer de web-app voor het eerst wordt uitgevoerd vraagt deze u naar de **URL van de Post API** en **API-sleutel**.
Voer de waarden die u eerder hebt opgeslagen:
    - **URI aanvragen** van de Help-pagina van de API voor **URL van de API-bericht**
    - **API-sleutel** vanuit het dashboard van de service web voor de **API-sleutel**.

    Klik op **verzenden**.

    ![Voer Post URI en API-sleutel][image6]

6. De web-app de **Configuratie van de Web-App** -pagina met de huidige instellingen van de web-service wordt weergegeven. Hier kunt u wijzigingen aanbrengen in de instellingen die worden gebruikt door de web-app.

    > [AZURE.NOTE] Wanneer deze instellingen alleen verandert, verandert deze voor deze web-app. Deze wijzigen standaardinstellingen van uw web-service niet. Als u de **Beschrijving** hier wijzigen wijzigen niet deze bijvoorbeeld de beschrijving wordt weergegeven op het web servicedashboard in Machine Learning Studio.

    Wanneer u klaar bent, klikt u op **wijzigingen opslaan**en klik vervolgens op **Ga naar de startpagina**.

7. De startpagina kunt u waarden te verzenden naar uw webservice, klikt u op **verzenden**en het resultaat wordt geretourneerd.

Als u terugkeren naar de pagina **configuratie wilt** , gaat u naar de `setting.aspx` pagina van de web-app. Bijvoorbeeld: `http://carprediction.azurewebsites.net/setting.aspx.` wordt u gevraagd de API-sleutel opnieuw invoeren - u nodig hebt die toegang tot de pagina en bijwerken van de instellingen.

U kunt stoppen, start opnieuw op of verwijderen van de web-app in de portal van Azure zoals een andere WebApp. U kunt zo lang maken als deze wordt uitgevoerd Blader naar de startpagina webadres en voer nieuwe waarden.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Het gebruik van de sjabloon Batch Execution Service (BES)

U kunt de BES web app-sjabloon op dezelfde manier als de sjabloon met records, behalve dat de web-app die u maakt, kunt u meerdere rijen met gegevens verzenden en ontvangen van meerdere resultaten.

De resultaten van een webservice voor uitvoering van batch zijn opgeslagen in een container Azure opslag; de invoerwaarden kunnen afkomstig zijn van Azure opslag of een lokaal bestand.
Zo is, moet u een container Azure opslag voor de resultaten van de web-app en moet u uw invoergegevens voorbereiden.

![Proces voor het BES websjabloon gebruiken][image2]

1. Voer dezelfde procedure als u wilt maken van de web-app BES als voor de sjabloon records, behalve:
    - Krijgen de **URI aanvragen** van de pagina **BATCH EXECUTION** API-Help voor de webservice.
    - Ga naar [Azure ML Batch Execution Service Web App-sjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) waarmee u wilt openen van de sjabloon BES op Azure Marketplace en klik op **Web-App maken**.

2. Als u wilt opgeven waar u de resultaten die zijn opgeslagen, voert u de gegevens van de container bestemming op de startpagina van de web-app. Ook kunt u opgeven waar de web-app de invoerwaarden in een lokaal bestand of een container Azure opslag kan ophalen.
Klik op **verzenden**.

    ![Opslaginformatie][image7]

De web-app wordt een pagina met taakstatus weergegeven.
Wanneer de taak is voltooid krijgt u de locatie van de resultaten in Azure-blobopslag. U hebt ook de mogelijkheid om de resultaten downloaden naar een lokaal bestand.

## <a name="for-more-information"></a>Voor meer informatie

Voor meer informatie over...

- u maakt een machine learning-experiment met Machine Learning Studio, raadpleegt u [uw eerste experiment in Azure Machine Learning Studio maken](machine-learning-create-experiment.md)

- het implementeren van machine begrepen experimenteren als een webservice, raadpleegt u [een webservice Azure Machine Learning distribueren](machine-learning-publish-a-machine-learning-web-service.md)

- andere manieren voor toegang tot uw webservice, Zie [hoe u een Azure Machine Learning-webservice gebruiken](machine-learning-consume-web-services.md)


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png
