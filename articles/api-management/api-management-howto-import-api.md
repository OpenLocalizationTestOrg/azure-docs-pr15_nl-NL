<properties 
    pageTitle="Beheer van de API-basisbegrippen" 
    description="Meer informatie over API's, producten, rollen, groepen en andere belangrijke concepten API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Het importeren van de definitie van een API met bewerkingen in Azure API Management

Beheer van API nieuwe API's kan worden gemaakt en de bewerkingen handmatig worden toegevoegd of de API kan worden geïmporteerd samen met de bewerkingen in één stap.

API's en de bijbehorende bewerkingen kunnen worden geïmporteerd met de volgende indelingen.

-   WADL
-   Swagger

Deze handleiding ziet u hoe een nieuwe API maken en importeren de bewerkingen in één stap. Zie voor informatie over het handmatig maken een API en toe te voegen bewerkingen [hoe API's maken][] en [hoe u bewerkingen tot een API toevoegen][].

## <a name="import-api"> </a>Een API importeren

API's zijn gemaakt en hebt geconfigureerd in de publisher-portal. Voor toegang tot de publisher-portal op **beheren** in de klassieke Azure-Portal voor uw service API Management. Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

![Publisher-portal][api-management-management-console]

**API's** in het menu **API beheer** aan de linkerkant op en klik vervolgens op **API importeren**.

![API voor importeren][api-management-import-apis]

Het venster **Import-API** bevat drie tabbladen die overeenkomen met de drie manieren om te leveren van de API-specificatie.

-   Kunt u de API-specificatie in het vak aangewezen tekst plakken **vanaf het Klembord** .
-   **Uit bestand** , kunt u om te bladeren naar en selecteer het bestand met de API-specificatie.
-   **Van URL** kunt u de URL voor de specificatie voor de API opgeven.

![Indeling voor het importeren API][api-management-import-api-clipboard]

Na het opgeven van de specificatie API, gebruikt u de keuzerondjes aan de rechterkant om aan te geven van de indeling specificatie. De volgende indelingen worden ondersteund.

-   WADL
-   Swagger

Voer vervolgens een **URL van de Web-API achtervoegsel**. Hiermee wordt toegevoegd aan de basis-URL voor uw API management-service. Het grondtal URL geldt voor alle API's die worden gehost op elk exemplaar van een API Management-service. API-beheer onderscheidt API's door hun achtervoegsel en kunnen daarom het achtervoegsel moet uniek zijn voor elke API in een specifieke API management service-exemplaar.

Zodra alle waarden worden ingevoerd, klikt u op **Opslaan** om de API en de bijbehorende bewerkingen te maken. 

>[AZURE.NOTE] Zie een zelfstudie bekijken van het importeren van een eenvoudige rekenmachine API in Swagger-indeling, [beheren uw eerste API in Azure API Management](api-management-get-started.md).

## <a name="export-api"></a> Een API exporteren

Naast het importeren van nieuwe API's, kunt u de definities van uw API's exporteren vanuit de publisher-portal. Als u wilt doen, klikt u op **API exporteren** op het **tabblad Samenvatting** van uw **API**.

![API exporteren][api-management-export-api]

API's kunnen worden geëxporteerd met WADL of Swagger. Selecteer de gewenste indeling, klikt u op **Opslaan**en kies de locatie waar u het bestand wilt opslaan.

![API exportindeling][api-management-export-api-format]

## <a name="next-steps"> </a>Vervolgstappen

Nadat een API wordt gemaakt en de bewerkingen die zijn geïmporteerd, die u kunt bekijken en configureren eventuele aanvullende instellingen, de API toevoegen aan een Product en uw project publiceren zodat deze beschikbaar is voor ontwikkelaars. Zie de volgende handleidingen voor meer informatie.

-   [API-instellingen configureren][]
-   [Het maken en publiceren van een product][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Aan de slag met Azure API Management]: api-management-get-started.md
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance

[Bewerkingen toevoegen aan een API]: api-management-howto-add-operations.md
[Het maken en publiceren van een product]: api-management-howto-add-products.md
[Het maken van API 's]: api-management-howto-create-apis.md
[API-instellingen configureren]: api-management-howto-create-apis.md#configure-api-settings
