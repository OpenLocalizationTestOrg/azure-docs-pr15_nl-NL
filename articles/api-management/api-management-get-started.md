<properties
    pageTitle="Uw eerste API in Azure API Management beheren | Microsoft Azure"
    description="Informatie over het maken van API's, bewerkingen toevoegen en aan de slag met API Management."
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
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="manage-your-first-api-in-azure-api-management"></a>Uw eerste API in Azure API Management beheren

## <a name="overview"> </a>Overzicht

Deze handleiding ziet u hoe u snel aan de slag in Azure API te beheren en uw eerste API-gesprek.

## <a name="concepts"> </a>Wat Azure API Management is?

U kunt Azure API Management kunt u een back-end en een volwaardige API-programma op basis van deze starten.

Gebruikelijke scenario's zijn:

* **Mobiele infrastructuur beveiligen** door access gating met API-toetsen, DOS voorkomen aanvallen met behulp van beperken of geavanceerde beveiligingsbeleid voor apparaten zoals JWT token gegevensvalidatie gebruiken.
* **Inschakelen van ISV partner ecosystemen** door aanbod snel partner onboarding via de portal ontwikkelaar en het bouwen van een gevel API loskoppelen van interne implementaties die niet juist die goed partner verbruik zijn.
* **Een interne API-programma uitgevoerd** door te bieden een centrale locatie voor de organisatie om te communiceren over de beschikbaarheid en de meest recente wijzigingen in API's, op basis van de organisatie-accounts, access gating alle op basis van een beveiligde kanaal tussen de gateway API en de backend.


Het systeem bestaat uit de volgende onderdelen:

* De **API gateway** is het eindpunt dat:
  * API belt en stuurt ze naar uw backends accepteert.
  * Controleert of API toetsen, JWT tokens certificaten en andere referenties.
  * Quota voor afdwingt en limieten classificeren.
  * Hiermee transformeert u de API in de browser zonder code wijzigingen.
  * In de cache opgeslagen backend antwoorden waar instellen.
  * Logboeken belt metagegevens voor analytics-toepassing.

* De **publisher-portal** is de administratieve interface waar u uw API-mailprogramma instelt. Gebruik deze naar:
    * Definiëren of API schema importeren.
    * Pakket API's in de producten.
    * Beleid zoals quota of transformaties op de API's instellen.
    * Inzicht krijgen van analytics.
    * Gebruikers beheren.

* De **developer portal** fungeert als de aanwezigheid van de belangrijkste web voor ontwikkelaars, waar ze kunnen:
    * Alleen API-documentatie.
    * Een API via de interactieve beheerconsole uitproberen.
    * Maak een account en u hierop abonneren om API toetsen.
    * Access-analyses op hun eigen gebruik.


## <a name="create-service-instance"> </a>Maken van een exemplaar API Management

>[AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Als u geen account hebt, kunt u een gratis account maken in een paar minuten. Zie [Azure gratis proefversie][]voor meer informatie.

De eerste stap in werken met API Management is het opzetten van een exemplaar van service. Meld u aan bij de [Klassieke Azure-Portal][] en klik op **Nieuw**, **App-Services**, **API Management**, **maken**.

![Nieuw exemplaar van API Management][api-management-create-instance-menu]

Geef een unieke onderliggend domeinnaam wilt gebruiken voor de URL van de service voor **URL**.

Kies het gewenste **abonnement** en **regio** voor uw exemplaar van de service. Nadat u de gewenste opties, klikt u op de knop **volgende** .

![Nieuwe API Management-service][api-management-create-instance-step1]

Typ **Contoso Ltd.** voor de **Naam van organisatie**, en voer uw e-mailadres in het veld **Beheerder E-Mail** .

>[AZURE.NOTE] Deze e-mailadres wordt gebruikt voor meldingen voor het beheer van de API-systeem. Zie [hoe u meldingen en e-mailsjablonen in Azure API Management configureren][]voor meer informatie.

![Nieuwe API Management-service][api-management-create-instance-step2]

API Management service-exemplaren zijn beschikbaar in drie lagen: ontwikkelaars, Standard en Premium. Nieuwe API Management service-exemplaren worden standaard gemaakt in de ontwikkelaars laag. Selecteer de laag standaard of Premium, schakel het selectievakje **Geavanceerde instellingen** in en selecteert u de gewenste laag in het volgende scherm.

>[AZURE.NOTE] De laag ontwikkelaars is voor de ontwikkeling, testen en testfase API-programma's waar beschikbaarheid niet van belang is. In de lagen Standard en Premium, kunt u uw aantal gereserveerde eenheden voor het verwerken van meer verkeer schalen. De standaardweergave en de Premium-lagen bieden uw API Management-service met de grootste van verwerkingskracht en prestaties. U kunt deze zelfstudie uitvoeren met behulp van elke laag. Zie voor meer informatie over API Management lagen, [API Management prijzen][].

Klik op het selectievakje in als u wilt maken van uw exemplaar van de service.

![Nieuwe API Management-service][api-management-instance-created]

Nadat het exemplaar van de service is gemaakt, is de volgende stap te maken of importeren van een API.

## <a name="create-api"> </a>Een API importeren

Een API bestaat uit een reeks bewerkingen die kan worden gestart door een clienttoepassing. API-bewerkingen zijn via proxy met bestaande-webservices.

API's kan worden gemaakt (en bewerkingen kunnen worden toegevoegd) handmatig, of ze kunnen worden geïmporteerd. In deze zelfstudie wordt we de API voor een steekproef Rekenmachine webservice geleverd door Microsoft en die worden gehost op Azure importeren.

>[AZURE.NOTE] Zie voor hulp bij het maken van een API en handmatig toe te voegen bewerkingen, [hoe API's maken](api-management-howto-create-apis.md) en [hoe u bewerkingen tot een API toevoegen](api-management-howto-add-operations.md).

API's zijn geconfigureerd in de publisher-portal, die wordt geopend met de klassieke Azure-Portal. Klik op **beheren** in de klassieke Azure-Portal voor uw API Management-service te bereiken de publisher-portal.

![Publisher-portal][api-management-management-console]

Als u wilt importeren de Rekenmachine API, **API's** in het menu **API beheer** aan de linkerkant op en klik vervolgens op **API importeren**.

![Knop importeren API][api-management-import-api]

De volgende stappen om te configureren de Rekenmachine API uitvoeren:

1. Klik op **Van URL**, **http://calcapi.cloudapp.net/calcapi.json** in het tekstvak **URL van specificatie van het document** , en klik op het keuzerondje **Swagger** .
2. Typ **berekenen** in het tekstvak **achtervoegsel Web API-URL** .
3. Klik in het vak **producten (optioneel)** en kies **Starter**.
4. Klik op **Opslaan** als u wilt importeren de API.

![Nieuwe API toevoegen][api-management-import-new-api]

>[AZURE.NOTE] **API Management** ondersteunt momenteel 1.2 en 2.0-versie van Swagger document voor importeren. Zorg ervoor dat, hoewel [Swagger 2.0-specificatie](http://swagger.io/specification) gedeclareerd die `host`, `basePath`, en `schemes` eigenschappen zijn optioneel, het Swagger 2.0-document **moet** bevatten die eigenschappen; anders won't het krijgen geïmporteerd. 

Nadat de API zijn geïmporteerd, wordt de overzichtspagina voor de API in de publisher-portal weergegeven.

![API-overzicht][api-management-imported-api-summary]

De sectie API heeft meerdere tabbladen. Eenvoudige maatstaven en informatie over de API op het tabblad **Overzicht** weergegeven. Het tabblad [Instellingen](api-management-howto-create-apis.md#configure-api-settings) wordt gebruikt voor de configuratie voor een API weergeven en bewerken. Het tabblad [bewerkingen](api-management-howto-add-operations.md) wordt gebruikt voor het beheren van de API's bewerkingen. Het tabblad **beveiliging** kan worden gebruikt voor het configureren van de gateway-verificatie voor de backend-server met behulp van basisverificatie of [onderlinge certificaatverificatie](api-management-howto-mutual-certificates.md)en voor het configureren van de [gebruikersautorisatie met behulp van OAuth 2.0](api-management-howto-oauth2.md).  Het tabblad **problemen** wordt gebruikt voor het weergeven van problemen gemeld door de ontwikkelaars die uw API's gebruiken. Het tabblad **producten** wordt gebruikt voor het configureren van de producten die deze API bevatten.

Standaard wordt elk exemplaar API Management geleverd met twee steekproeven producten:

-   **Starter**
-   **Onbeperkt**

In deze zelfstudie hebt u de eenvoudige rekenmachine-API toegevoegd aan het product Starter, wanneer de API is geïmporteerd.

Als u wilt bellen een API, moeten eerst ontwikkelaars abonneren op een product waarmee ze toegang hebt tot deze. Ontwikkelaars kunnen u abonneren op producten in de developer portal of beheerders van de producten in de publisher-portal ontwikkelaars kunnen u abonneren. U kunt een beheerder bent sinds u hebt gemaakt het exemplaar van het beheer van de API in de vorige stappen in deze zelfstudie, zodat u bent al geabonneerd op elk product al dan niet standaard.

## <a name="call-operation"> </a>Bellen van een bewerking vanuit de developer portal

Bewerkingen kunnen worden aangeroepen rechtstreeks vanuit de developer portal, die is een handige manier om te bekijken en de bewerkingen van een API testen. In deze zelfstudie stap belt u bewerking van de eenvoudige rekenmachine API's **toevoegen twee gehele getallen** . Klik op **Developer portal** van het menu boven in de publisher-portal.

![Developer portal][api-management-developer-portal-menu]

Klik op **API's** van het bovenste menu en klik vervolgens op **Eenvoudige rekenmachine** als u wilt zien van de beschikbare bewerkingen.

![Developer portal][api-management-developer-portal-calc-api]

Houd rekening met de beschrijvingen van de steekproef en parameters die zijn geïmporteerd samen met de API en de bewerkingen, documentatie voor de ontwikkelaars die worden gebruikt door deze bewerking leveren. Deze beschrijvingen kunnen ook worden toegevoegd wanneer bewerkingen handmatig worden toegevoegd.

Als u wilt bellen met de bewerking van **twee gehele getallen toevoegen** , klikt u op **het uitproberen**.

![Het uitproberen][api-management-developer-portal-calc-api-console]

U kunt Voer enkele waarden in voor de parameters of de standaardwaarden behouden en klik vervolgens op **verzenden**.

![HTTP Get][api-management-invoke-get]

Nadat een bewerking is geactiveerd, wordt de developer portal weergegeven de **antwoordstatus**, het **antwoord kopteksten**en alle **inhoud van de reactie**.

![Antwoord][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Gebruiksanalyses weergeven

Als u wilt bekijken analyses van eenvoudige rekenmachine, Ga terug naar de publisher-portal door te **beheren** selecteren in het menu aan de bovenkant van de developer portal.

![Beheren][api-management-manage-menu]

De standaardweergave voor de publisher-portal is het **Dashboard**, waarin u een overzicht van uw exemplaar van de API-Management.

![Dashboard][api-management-dashboard]

Plaats de muisaanwijzer op de grafiek voor **Eenvoudige rekenmachine** om te zien van de specifieke doelstellingen voor het gebruik van de API voor een bepaalde periode.

>[AZURE.NOTE] Als u niet alle regels in de grafiek ziet, gaat u terug naar de developer portal sommige gesprekken te voeren in de API, wacht even en keert u terug naar het dashboard.

Klik op **Details weergeven** als wilt bekijken van de overzichtspagina voor de API, met inbegrip van een grotere versie van de doelstellingen van de weergegeven.

![Analytics][api-management-mouse-over]

![Overzicht][api-management-api-summary-metrics]

Klik op **analyse** in het menu **API beheer** aan de linkerkant voor gedetailleerde afmetingen en rapporten.

![Overzicht][api-management-analytics-overview]

De sectie **Analytics** heeft de volgende vier tabbladen:

-   Biedt de algemene status- en gebruiksgegevens aan de doelstellingen, evenals de bovenste ontwikkelaars, topproducten bovenste API's en bovenste bewerkingen **in een oogopslag** .
-   **Gebruik** bevat een uitgebreide beschrijving van API oproepen en bandbreedte, inclusief een geografische weergave.
-   **Servicestatus** richt zich op statuscodes, success tarieven, antwoord tijden en API in de cache en service-antwoord tijden.
-   **Activiteit** biedt rapporten die op inzoomen op de specifieke activiteit door ontwikkelaars, product, API en bewerking.

## <a name="next-steps"> </a>Vervolgstappen

- Leer hoe u [uw API met limieten voor de snelheid beveiligen](api-management-howto-product-with-rules.md).

[Azure gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Meldingen en e-mailsjablonen in Azure API Management configureren]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[API Management prijzen]: http://azure.microsoft.com/pricing/details/api-management/

[Azure klassieke Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
