<properties 
    pageTitle="Hoe u Autoriseer ontwikkelaars accounts OAuth 2.0 gebruiken in Azure API Management" 
    description="Leer hoe u gebruikers OAuth 2.0 gebruiken in API Management machtigen." 
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

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Hoe u Autoriseer ontwikkelaars accounts OAuth 2.0 gebruiken in Azure API Management

Veel API's ondersteunen [OAuth 2.0](http://oauth.net/2/) als u wilt beveiligen van de API en zorg ervoor dat alleen geldige gebruikers toegang hebben, en ze alleen toegang tot resources waaraan ze hebt recht. Pas Azure API van interactieve ontwikkelaars beheerconsole met deze API's gebruiken, kunt de service u uw exemplaar van de service om deze te werken met uw OAuth 2.0 ingeschakeld API configureren.

## <a name="prerequisites"> </a>Vereisten

Deze handleiding ziet u hoe u uw exemplaar van de service API Management om deze te gebruiken OAuth 2.0 autorisatie voor ontwikkelaars-accounts configureren, maar wordt niet uitgelegd hoe u een OAuth 2.0-provider configureren. De configuratie voor elke OAuth 2.0-provider is niet hetzelfde, hoewel de stappen lijken, en de vereiste onderdelen van de informatie die wordt gebruikt bij het configureren van OAuth 2.0 in uw exemplaar van de service API Management hetzelfde zijn. In dit onderwerp ziet u voorbeelden van Azure Active Directory gebruikt als een OAuth 2.0-provider.

>[AZURE.NOTE] Zie voor meer informatie over het configureren van OAuth 2.0 met Azure Active Directory, de steekproef [WebApp-GraphAPI-DotNet][] .

## <a name="step1"> </a>Een OAuth 2.0 autorisatie-server in API Management configureren

Als u wilt beginnen, klikt u op **beheren** in de klassieke Azure-Portal voor uw service API Management. Hiermee gaat u naar de publisher-API-beheerportal.

![Publisher-portal][api-management-management-console]

>[AZURE.NOTE] Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

Klik op **beveiliging** in het menu **API beheer** aan de linkerkant op **OAuth 2.0**en klik vervolgens op **toevoegen autorisatie server**.

![OAuth 2.0][api-management-oauth2]

Nadat u hebt geklikt **toevoegen autorisatie server**, wordt het nieuwe autorisatie serverformulier weergegeven.

![Nieuwe server][api-management-oauth2-server-1]

Geef een naam en een optionele beschrijving in de velden **naam** en **Beschrijving** . 

>[AZURE.NOTE] Deze velden worden gebruikt om aan te geven van de server OAuth 2.0 autorisatie binnen het huidige exemplaar van de API Management-service en bijbehorende waarden niet afkomstig zijn van de OAuth 2.0-server.

Voer de **URL voor de aanmeldingspagina van Client registratie**. Deze pagina is waar gebruikers kunnen maken en beheren van hun-accounts en varieert afhankelijk van de OAuth 2.0-provider gebruikt. De **URL voor de aanmeldingspagina van Client registratie** verwijst naar de pagina waarmee gebruikers kunnen maken en configureren van hun eigen accounts voor OAuth 2.0-providers die ondersteuning bieden voor gebruikersbeheer accounts. Sommige organisaties geen configureren en gebruiken van deze functionaliteit, zelfs als de provider OAuth 2.0 wordt ondersteund. Als uw provider OAuth 2.0 geen Gebruikersbeheer accounts geconfigureerd, voert u een tijdelijke aanduiding voor URL hier zoals de URL van uw bedrijf of een URL, zoals `https://placeholder.contoso.com`.

Het volgende gedeelte van het formulier bevat de **code autorisatie verlenen typen**, **autorisatie eindpunt URL**en **autorisatie verzoek methode** -instellingen.

![Nieuwe server][api-management-oauth2-server-2]

Geef de **autorisatiecode verlenen typen** door te schakelen van de gewenste typen. **Autorisatiecode** is opgegeven al dan niet standaard.

Voer de **URL van de autorisatie-eindpunt**. Voor Azure Active Directory, deze URL is vergelijkbaar met de volgende URL, waar `<client_id>` wordt vervangen door de client-id die uw toepassing op de server OAuth 2.0 aangeeft.

    https://login.windows.net/<client_id>/oauth2/authorize

De **autorisatie verzoek methode** geeft aan hoe het verzoek autorisatie wordt verzonden naar de OAuth 2.0-server. Standaard is **ophalen** geselecteerd.

Het volgende gedeelte is waar de **URL voor Token-eindpunt**, **verificatiemethoden Client** **toegangstoken methode verzenden**en **standaardbereik** zijn opgegeven.

![Nieuwe server][api-management-oauth2-server-3]

Voor een Azure Active Directory OAuth 2.0-server, de **URL-eindpunt Token** hebben de volgende indeling, waar `<APPID>` heeft de notatie van `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

De standaardinstelling voor de **Client verificatiemethoden** is **eenvoudige**en **toegangstoken verzenden methode** **autorisatie koptekst**. Deze waarden worden geconfigureerd in deze sectie van het formulier, samen met de **standaardbereik**.

De sectie **referenties Client** bevat de **Client-ID** en de **geheim Client**, die zijn verkregen tijdens het maken en configuratie van uw OAuth 2.0-server. Zodra de **Client-ID** en **geheim Client** zijn opgegeven, wordt de **redirect_uri** voor de **autorisatiecode** wordt gegenereerd. Deze URI wordt gebruikt voor het configureren van de URL beantwoorden in de configuratie van uw OAuth 2.0-server.

![Nieuwe server][api-management-oauth2-server-4]

Als **autorisatiecode verlenen typen** is ingesteld op **Resource eigenaarswachtwoord**, wordt de sectie **Resource eigenaar wachtwoordreferenties** gebruikt om op te geven van de referenties; anders kunt u deze leeg laten.

![Nieuwe server][api-management-oauth2-server-5]

Als u het formulier hebt voltooid, klikt u op **Opslaan** om op te slaan de configuratie van de API Management OAuth 2.0 autorisatie-server. Zodra de configuratie van de server is opgeslagen, kunt u API's voor het gebruik van deze configuratie kunt configureren, zoals wordt weergegeven in het volgende gedeelte.

## <a name="step2"> </a>Een API voor het gebruik van de machtiging van OAuth 2.0-gebruikers configureren

**API's** in het menu **API beheer** aan de linkerkant op, klik op de naam van de gewenste API, klikt u op **beveiliging**en schakel vervolgens het selectievakje voor **OAuth 2.0**.

![Gebruikersautorisatie voor][api-management-user-authorization]

Selecteer de gewenste **autorisatie server** in de vervolgkeuzelijst en klik op **Opslaan**.

![Gebruikersautorisatie voor][api-management-user-authorization-save]

## <a name="step3"> </a>Testen van de machtiging van het OAuth 2.0-gebruikers in de Developer Portal

Zodra u hebt uw OAuth 2.0 autorisatie server geconfigureerd en uw API als u wilt gebruiken die server geconfigureerd, kunt u deze kunt testen door te gaan naar de Developer Portal en een API oproepen.  Klik op **Developer portal** in het bovenste rechts menu.

![Developer portal][api-management-developer-portal-menu]

**API's** in het bovenste menu en selecteer **Echo API**.

![Echo-API][api-management-apis-echo-api]

>[AZURE.NOTE] Als u slechts één API geconfigureerde of zichtbaar is voor uw account hebt, gaat klikt op API's u rechtstreeks naar de bewerkingen voor die API.

Selecteer de bewerking voor de **Resource ophalen** op **Geopende Console**en selecteer vervolgens **autorisatiecode** in de vervolgkeuzelijst.

![Geopende console][api-management-open-console]

Wanneer **autorisatiecode** is geselecteerd, wordt een pop-upvenster weergegeven met het formulier aanmeldingsproblemen van de OAuth 2.0-provider. In dit voorbeeld krijgt u het formulier aanmeldingsproblemen van Azure Active Directory.

>[AZURE.NOTE] Als er pop-ups uitgeschakeld, moet u deze inschakelen door de browser. Nadat u deze hebt ingeschakeld, worden selecteren op **autorisatiecode** opnieuw en het formulier aanmeldingsproblemen weergegeven.

![Aanmelden][api-management-oauth2-signin]

Nadat u zich hebt aangemeld, de **aanvraag kopteksten** zijn gevuld met een `Authorization : Bearer` koptekst waarmee het verzoek.

![Koptekst token aanvragen][api-management-request-header-token]

U kunt nu de gewenste waarden voor de resterende parameters configureren en verzend het verzoek. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende video en de bijbehorende [artikel](api-management-howto-protect-backend-with-aad.md)voor meer informatie over het gebruik van OAuth 2.0 en API-beheer.

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Aan de slag met Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

