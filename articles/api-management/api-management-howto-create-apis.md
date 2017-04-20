<properties 
    pageTitle="API's maken in Azure API Management" 
    description="Informatie over het maken en API's configureren in Azure API Management." 
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

# <a name="how-to-create-apis-in-azure-api-management"></a>API's maken in Azure API Management

Een API in API Management vertegenwoordigt een reeks bewerkingen die kunnen worden aangeroepen door clienttoepassingen. Nieuwe API's zijn gemaakt in de publisher-portal en klikt u vervolgens de gewenste bewerkingen worden toegevoegd. Zodra de bewerkingen zijn toegevoegd, wordt de API is toegevoegd aan een product en kan worden gepubliceerd. Als een API is gepubliceerd, kunt u deze geabonneerd op en gebruikt door ontwikkelaars.

Deze handleiding ziet u de eerste stap in het proces: het maken en configureren van een nieuwe API in API-Management. Zie [hoe u bewerkingen tot een API toevoegen][] en [hoe maken en publiceren van een product][]voor meer informatie over het toevoegen van bewerkingen en publiceren van een product.

## <a name="create-new-api"> </a>Maken van een nieuwe API

API's zijn gemaakt en hebt geconfigureerd in de publisher-portal. Voor toegang tot de publisher-portal op **beheren** in de klassieke Azure-Portal voor uw service API Management.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

**API's** in het menu **API beheer** aan de linkerkant op en klik vervolgens op **API toevoegen**.

![API maken][api-management-create-api]

Gebruik het venster **nieuwe API toevoegen** voor het configureren van de nieuwe API.

![Nieuwe API toevoegen][api-management-add-new-api]

De volgende velden worden gebruikt voor het configureren van de nieuwe API.

-   **De naam van het web API** biedt een unieke en beschrijvende naam voor de API. Deze wordt weergegeven in de portals ontwikkelaars en publisher.
-   **De URL van webservice** verwijst naar de uitvoering van de API HTTP-service. API management stuurt aanvragen voor dit adres.
-   **Web API-URL achtervoegsel** wordt toegevoegd aan de basis-URL voor de API management-service. Het grondtal URL geldt voor alle API's die worden gehost door een exemplaar van de service API Management. API-beheer onderscheidt API's door hun achtervoegsel en kunnen daarom het achtervoegsel moet uniek zijn voor elke API voor een bepaald publisher.
-   **Web API URL-schema** wordt bepaald welke protocollen kunnen worden gebruikt voor toegang tot de API. HTTPs al dan niet standaard is opgegeven.
-   Als u wilt deze nieuwe API (optioneel) hebt toegevoegd aan een product, klik op de **producten (optioneel)** -omlaag en kies een product. Deze stap kan meerdere keren herhaald de API toevoegen aan meerdere producten.

Nadat de gewenste waarden zijn geconfigureerd, klikt u op **Opslaan**. Nadat de nieuwe API is gemaakt, wordt de overzichtspagina voor de API in de publisher-portal weergegeven.

![API-overzicht][api-management-api-summary]

## <a name="configure-api-settings"> </a>Configureren API-instellingen

U kunt het tabblad **Instellingen** gebruiken om te controleren en bewerken van de configuratie voor een API. **De naam van het web API**, **URL webservice**en **Web API-URL achtervoegsel** zijn ingesteld wanneer de API wordt gemaakt en kan worden gewijzigd hier. **Beschrijving** biedt een optionele beschrijving en **Web API URL-schema** bepaalt welke protocollen kunnen worden gebruikt voor toegang tot de API.

![API-instellingen][api-management-api-settings]

Als u wilt gateway-verificatie voor de uitvoering van de API backend-service configureren, selecteer het tabblad **beveiliging** . De **met referenties** vervolgkeuzelijst kan worden gebruikt voor het configureren van **HTTP eenvoudige** of **certificaten** verificatie. Als u wilt gebruiken HTTP-basisverificatie, voert u gewoon de gewenste referenties. Zie voor informatie over het gebruik van verificatie via clientcertificaat [het beveiligen van back-enddatabase services met verificatie via clientcertificaat in Azure API Management][].

Het tabblad **beveiliging** kan ook worden gebruikt voor het configureren van de **gebruikersautorisatie** met OAuth 2.0. Lees [hoe u het Autoriseer ontwikkelaars accounts OAuth 2.0 in Azure API Management gebruiken][]voor meer informatie.

![Eenvoudige verificatie-instellingen][api-management-api-settings-credentials]

Klik op **Opslaan** om op te slaan wijzigingen die u in de API-instellingen aanbrengt.

## <a name="next-steps"> </a>Vervolgstappen

Nadat een API is gemaakt en de instellingen die is geconfigureerd, de volgende stappen zijn de bewerkingen toevoegen aan de API, de API toevoegen aan een product en uw project publiceren zodat deze beschikbaar voor ontwikkelaars is. Zie de volgende artikelen voor meer informatie.

-   [Bewerkingen toevoegen aan een API][]
-   [Het maken en publiceren van een product][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[Bewerkingen toevoegen aan een API]: api-management-howto-add-operations.md
[Het maken en publiceren van een product]: api-management-howto-add-products.md

[Aan de slag met Azure API Management]: api-management-get-started.md
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance
[Back-enddatabase services client met verificatie via clientcertificaat in Azure API Management beveiligen]: api-management-howto-mutual-certificates.md
[Hoe u Autoriseer ontwikkelaars accounts OAuth 2.0 gebruiken in Azure API Management]: api-management-howto-oauth2.md