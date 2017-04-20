<properties 
    pageTitle="Machine learning publiceren webservice met Azure Marketplace | Microsoft Azure" 
    description="Hoe uw Azure Machine Learning-webservice publiceren naar de Azure Marketplace" 
    services="machine-learning" 
    documentationCenter="" 
    authors="BharathS" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="bharaths"/>

# <a name="publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>Azure Machine Learning-webservice publiceren naar de Azure Marketplace 

De Azure Marketplace biedt de mogelijkheid om te publiceren van Azure Machine Learning-webservices als betaald of services voor verbruik door externe klanten vrij te geven. Dit artikel bevat een overzicht van dat proces met koppelingen naar richtlijnen om u te helpen. Met behulp van dit proces, kunt u uw webservices beschikbaar zijn voor andere ontwikkelaars gebruiken in hun toepassingen maken.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Overzicht van het publicatieproces 

Hier volgen de stappen voor het publiceren van een webservice Azure Machine Learning met Azure Marketplace:

1. Maken en publiceren van een Machine Learning aanvraag en respons-service (records)
2. De service implementeren naar productie en de API-sleutel en OData-eindpunt-informatie opvragen.
3. Gebruik van de URL van de gepubliceerde webservice publiceren naar [Azure Marketplace (Datamarket)](https://publish.windowsazure.com/workspace/) 
4. Zodra verzonden, wordt uw aanbod beoordeeld en moet worden goedgekeurd voordat uw klanten kunt starten aankoop hiervan. Het publicatieproces kan een paar dagen duren. 

## <a name="walk-through"></a>Doorloop
###<a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Stap 1: Maken en publiceren van een Machine Learning aanvraag en respons-service (records)###
 Als u nog niet hebt gedaan dit, gaat u naar bij deze [doorlopen](machine-learning-walkthrough-5-publish-web-service.md).

###<a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Stap 2: De service implementeren naar productie en de API-sleutel en OData-eindpunt informatie te verkrijgen###
1. Selecteer de optie **MACHINE LEARNING** in de linkernavigatiebalk in de [Klassieke Azure-Portal](http://manage.windowsazure.com)en selecteer uw werkruimte. 

2. Klik op het tabblad **WEB SERVICES** en selecteer de webservice die u wilt publiceren naar de marketplace.

    ![Azure Marketplace][workspace]

3. Selecteer het eindpunt dat u wilt de marketplace gebruiken. Als u geen extra eindpunten hebt gemaakt, kunt u de **standaard** -eindpunt selecteren.

4. Nadat u op het eindpunt hebt geklikt, is mogelijk om de **API-sleutel**weer te geven. Moet u dit stuk informatie later in stap 3, Controleer dus een kopie ervan.

    ![Azure Marketplace][apikey]

5. Klik op de methode **Aanvraag en respons** , klikt u op dit punt dat we ondersteunen geen publicerende batch execution services met de marketplace. Dat gaat u naar de API help-pagina voor de aanvraag en respons.

6. Kopieer het **Adres van de OData-eindpunt**, moet u deze informatie later in stap 3.

    ![Azure Marketplace][odata]




de service implementeren in bedrijf.



###<a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Stap 3: De URL van de gepubliceerde webservice gebruiken om te publiceren met Azure Marketplace (DataMarket)###

1.  Navigeer naar [Azure Marketplace (Datamarket)](http://datamarket.azure.com/home) 
2.  Klik op de koppeling **publiceren** boven aan de pagina. Hiermee gaat u naar de [Microsoft Azure Portal voor publiceren](https://publish.windowsazure.com)
3.  Klik op de sectie **uitgevers** om u te registreren als een uitgever.
4.  Wanneer u een nieuwe aanbieding maakt, selecteert u **Gegevensservices**en klik op **maken een nieuwe gegevens-Service**. 
 
    ![Azure Marketplace][image1]

    <br />


5.  Klik onder **abonnementen** vindt u informatie over uw aanbod, inclusief een prijzen-abonnement. Bepaal als u een gratis of betaalde service biedt. Als u wilt ophalen dat is betaald, bieden betalingsgegevens zoals uw bank en belasting.

6.  Klik onder **Marketing** bevatten informatie over uw aanbod, zoals de titel en beschrijving voor de aanbieding.

7.  Onder **prijzen** kunt u de prijs instellen voor uw plannen voor specifieke landen of laat het systeem "autoprice" uw aanbod.

8. Klik op het tabblad **Gegevensservice** **Webservice** als **Gegevensbron**.

    ![Azure Marketplace][image2]

9.  De URL en API-sleutel voor de service voor web krijgen van de klassieke Azure-Portal, zoals beschreven in stap 2 hierboven.

10. Plak het adres van de OData-eindpunt in het tekstvak **URL Service** in het dialoogvenster Marketplace-gegevensservice-instelling.

11. Voor **verificatie**, kiest u **kop** als de **Verificatie kleurenschema**.

    - Voer "Autorisatie" voor de **kopnaam**.
    - Voor de **Waarde van de koptekst**, typ "Vruchtdragende" (zonder de aanhalingstekens), klikt u op **de SPATIEBALK** en plak de API-sleutel.
    - Schakel het selectievakje **deze Service is OData** .
    - Klik op **Verbinding testen** om de verbinding te testen.

12. Zorg ervoor **Dat machine Learning** is geselecteerd onder **categorieën**.

13. Wanneer u klaar bent met invoeren van de metagegevens over uw aanbod, klik op **publiceren**en klik vervolgens **op tijdelijke Push**. Nu wordt u geïnformeerd over eventuele resterende problemen die u nodig hebt om op te lossen.

14. Als u zeker bent voltooiing van alle resterende problemen, klikt u op op **goedkeuring naar productie te zenden aanvragen**. Het publicatieproces kan een paar dagen duren. 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 
