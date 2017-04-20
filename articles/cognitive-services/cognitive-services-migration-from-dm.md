
<properties
    pageTitle="Migreren naar Azure cognitieve Services aanbevelingen API uit de aanbevelingen DataMarket-API | Microsoft Azure"
    description="Azure machine learning-aanbevelingen--migreren naar aanbevelingen cognitieve service"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Migreren naar Azure cognitieve Services aanbevelingen API uit de aanbevelingen DataMarket-API
In dit artikel leest u hoe u het migreren van de [Microsoft DataMarket aanbevelingen API](https://datamarket.azure.com/dataset/amla/recommendations) naar de [Microsoft Azure cognitieve Services aanbevelingen API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api).

De DataMarket aanbevelingen API accepteert nieuwe klanten 31 December 2016, meer en 28 februari 2017 TCP/IP.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Hoe start ik de Azure cognitieve Services aanbevelingen-API gebruiken?

Ga als volgt te werk als u wilt migreren naar de API voor aanbevelingen van cognitieve Services:

1.  Als u een Azure-abonnement u [zich registreren](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) voor een nog geen hebt. 

1.  Stapsgewijze instructies krijgen van de [Handleiding aan de slag](cognitive-services-recommendations-quick-start.md) om te registreren voor de API voor aanbevelingen van cognitieve Services en via programmacode. 

1.  Ga naar de [cognitieve Services aanbevelingen API lossen pagina](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) voor meer informatie over de service en documentatie vinden.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Ik heb de gebruikersinterface aanbevelingen gebruikt om mijn modellen te maken. Is er een vergelijkbaar hulpprogramma voor de API voor aanbevelingen van cognitieve Services?

Absoluut! De URL voor de nieuwe [Gebruikersinterface aanbevelingen](http://recommendations-portal.azurewebsites.net/) is http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] Uw referenties DataMarket werken hier niet. Registreren voor een abonnement in de Portal Azure, en vind de Accountsleutel nodig hebt voor de nieuwe [Aanbevelingen UI](http://recommendations-portal.azurewebsites.net/).
Als u niet een sleutel-Account hebt, raadpleegt u de taak 1 van de [Handleiding aan de slag](cognitive-services-recommendations-quick-start.md).

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>De nieuwe API-indeling is hetzelfde als de DataMarket aanbevelingen-API?

De API ondersteunt de hetzelfde scenario's en proces loopt als deze scenario's die worden ondersteund in de DataMarket-versie, maar de werkelijke API-interface om te voldoen aan de [Microsoft REST API richtlijnen](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)is bijgewerkt. De API's zijn consistenter en werk beter met hulpprogramma's voor die ondersteuning Swagger.

Als u wilt weten over elk van de API's, raadpleegt u de [API explorer](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db).
Gebruik de *Probeer* deze knop als u wilt testen van een API-aanroep. De opmaak van de bestanden die worden gebruikt door de aanbevelingen-API (catalogus en het gebruik van bestanden) is niet gewijzigd, zodat u de dezelfde bestanden en/of infrastructuur die u hebt gemaakt om te genereren die bestanden kunt blijven gebruiken.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Wat zijn enkele nieuwe functies in de API voor aanbevelingen van cognitieve Services?

We hebben in de afgelopen twee maanden, nieuwe mogelijkheden voor de cognitieve Services aanbevelingen API uitgebracht:
-   Aanbevelingen UI voor training en testen van modellen zonder te hoeven schrijven van één regel met code
-   Batch score bieden u in één keer duizenden aanbevelingen
-   Aan de doelstellingen ondersteuning op query de kwaliteit van aanbevelingen modellen maken
-   Ondersteuning voor bedrijfsregels
-   Mogelijkheid om te opsommen en gebruik en catalogus bestanden downloaden
-   Volgorde opbouwen ondersteuning om query's in de kwaliteit van de functies van het item in een model aanbevelingen
-   Toegevoegde mogelijkheid om te zoeken naar een product in de catalogus

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Wanneer stopt Microsoft ondersteuning van de DataMarket aanbevelingen API?

[Aanbevelingen API op DataMarket](https://datamarket.azure.com/dataset/amla/recommendations) accepteert geen nieuwe klanten na 31 December 2016, en door 28 februari 2017 volledig TCP/IP. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Wat gebeurt er als er geen de gegevens die ik nodig om mijn modellen in de API voor aanbevelingen van cognitieve Services opnieuw te maken?

We wilt zorgen dat zo eenvoudig mogelijk voor u. We kunnen helpen u uw oude modellen van uw DataMarket-account te gaan naar uw nieuwe Azure cognitieve Services aanbevelingen API-abonnement. Contact met ons opnemen op [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Vragen we u de DataMarket abonnements-ID en uw cognitieve Services abonnements-ID, voordat we de modellen voor aan uw nieuwe account koppelen.
