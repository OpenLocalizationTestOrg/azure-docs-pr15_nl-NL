<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="De typen toepassingen die u in de Azure Active Directory B2C kunt maken."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory-B2C: Typen toepassingen

Azure Active Directory (Azure AD) B2C ondersteunt verificatie voor een groot aantal moderne app architecturen. Alle labels zijn gebaseerd op de standaard industriestandaardprotocollen [OAuth 2.0](active-directory-b2c-reference-protocols.md) of [OpenID verbinding maken](active-directory-b2c-reference-protocols.md). In dit document een korte beschrijving van de soorten apps die u kunt maken, onafhankelijk van de taal of platform u liever. Ook kunt u inzicht krijgen in de algemene scenario's voordat u [beginnen met het samenstellen van toepassingen](active-directory-b2c-overview.md#getting-started).

## <a name="the-basics"></a>De basisbeginselen
Elke app met Azure AD B2C moet worden geregistreerd in uw [B2C directory](active-directory-b2c-get-started.md) via de [Portal van Azure](https://portal.azure.com/). Het registratieproces app verzamelt en enkele waarden worden toegewezen aan uw app:

- Een **Toepassings-ID** die deze identificeert op uw app.
- Een **URI omleiden** die kunnen worden gebruikt om te antwoorden terug naar uw app sturen.
- Alle andere scenario / regiospecifieke-waarden. Informatie voor meer informatie over hoe u [een app registreert](active-directory-b2c-app-registration.md).

Nadat de app is geregistreerd, wordt deze door te sturen van aanvragen naar het eindpunt van Azure AD v2.0 communiceert met Azure AD:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Elk verzoek om die wordt verzonden naar Azure AD B2C Hiermee geeft u een **beleid**. Een beleid bepaalt het gedrag van Azure AD. U kunt ook deze eindpunten gebruiken om te maken van een zeer aanpasbare reeks gebruikerservaringen. Algemene beleid zijn aanmelding, aanmelden en beleidsregels profiel bewerken. Als u niet bekend met beleidsregels bent, moet u het Azure AD B2C [extensible beleid framework](active-directory-b2c-reference-policies.md) Lees voordat u verdergaat.

De interactie tussen elke app en een eindpunt v2.0, voert u een patroon met vergelijkbare op hoog niveau:

1. De app wordt u omgeleid zodat de gebruiker naar het eindpunt v2.0 uitvoeren van een [beleid](active-directory-b2c-reference-policies.md).
2. De gebruiker voltooit het beleid volgens de definitie van het beleid.
4. De app ontvangt een soort beveiligingstoken van het eindpunt v2.0.
5. De app wordt het beveiligingstoken gebruikt voor toegang tot beveiligde informatie of een beveiligde bron.
6. De resource-server is gevalideerd met het beveiligingstoken om te bevestigen dat toegang kan worden verleend.
7. De app worden regelmatig vernieuwd voor het beveiligingstoken.

<!-- TODO: Need a page for libraries to link to -->
Deze stappen kunnen afwijken enigszins op basis van het type app die u maakt. Bron openen bibliotheken kunnen adres van de details van u.

## <a name="web-apps"></a>WebApps
Web-apps voor (inclusief .NET, PHP, Java, Ruby, Python en Node.js) die worden gehost op een server en via een browser ondersteunt Azure AD B2C [OpenID verbinding maken](active-directory-b2c-reference-protocols.md) voor alle gebruikerservaringen. Hierbij aanmelden, aanmelding, en beheer van gebruikersprofielen. In de implementatie van Azure AD B2C van OpenID Connect Start uw web-app de mogelijkheden van deze gebruiker verificatieaanvragen door naar te verzenden Azure AD. Het resultaat van de aanvraag is een `id_token`. Deze beveiligingstoken staat voor de gebruikers id. Deze bevat ook informatie over de gebruiker in de vorm van claims:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Meer informatie over de verschillende typen tokens en claims die beschikbaar zijn in een app in de [B2C token verwijzen naar](active-directory-b2c-reference-tokens.md).

In een WebApp duurt elke uitvoering van een [beleid](active-directory-b2c-reference-policies.md) deze algemene stappen:

![Afbeelding van de zwembanen Web App](./media/active-directory-b2c-apps/webapp.png)

Validatie van de `id_token` met behulp van een openbare ondertekend sleutel die is ontvangen van Azure AD is voldoende om de identiteit van de gebruiker te verifiëren. Hiermee stelt u een sessiecookie die kan worden gebruikt om de gebruiker op aanvragen van de volgende pagina te identificeren.

Dit scenario in actie, bekijkt u een van de web-app aanmelden codevoorbeelden in onze [dat aan de slag sectie](active-directory-b2c-overview.md#getting-started).

Naast het vergemakkelijken eenvoudige aanmeldingsproblemen, een server WebApp mogelijk ook moeten toegang hebben tot een back-enddatabase-webservice. In dit geval kunt de web-app uitvoeren van een iets anders [OpenID verbinding-mailstroom](active-directory-b2c-reference-oidc.md) en tokens met behulp van autorisatiecodes aanschaffen en tokens vernieuwen. Dit scenario wordt weergegeven in de volgende [sectie van de Web-API's](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Web-API 's
U kunt Azure AD B2C secure webservices zoals van uw app RESTful web API. Web API's kunt OAuth 2.0 hun om gegevens te beveiligen, door te verifiëren binnenkomende HTTP-aanvragen tokens gebruiken. De beller van een web API wordt een token in de koptekst van de verificatie van een HTTP-aanvraag:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Het web API kunt het token gebruiken om te controleren of de identiteit van de beller API en informatie over de beller extraheren uit claims die in het token worden gecodeerd. Meer informatie over de verschillende typen tokens en claims die beschikbaar zijn in een app in het [overzicht van Azure AD B2C token](active-directory-b2c-reference-tokens.md).

> [AZURE.NOTE]
    Azure AD B2C ondersteunt momenteel alleen web API's die worden gebruikt door hun eigen bekende clients. Uw volledige app bevatten bijvoorbeeld een iOS-app, een Android-app en een back-enddatabase web API. Deze architectuur wordt volledig ondersteund. Toestaan een partner mailclient gebruikt, zoals een andere app van iOS, voor toegang tot hetzelfde web die API wordt momenteel niet ondersteund. Alle onderdelen van de volledige app moet delen een één toepassing-ID.

Een web API kunt tokens ontvangen van vele soorten clients, inclusief WebApps, desktop en mobiele apps, apps van één pagina, serverzijde daemons en andere web API's. Hier volgt een voorbeeld van de volledige stroom voor een web-app die een web API-oproepen:

![Afbeelding van de Web-App Web API zwembanen](./media/active-directory-b2c-apps/webapi.png)

Meer informatie over autorisatiecodes, vernieuwen tokens en de stappen voor het ophalen van tokens, lees meer over het [OAuth 2.0-protocol](active-directory-b2c-reference-oauth-code.md).

Als u wilt weten hoe u een web API met behulp van Azure AD B2C beveiligt, raadpleegt u het web API-zelfstudies in onze [aan de slag sectie](active-directory-b2c-overview.md#getting-started).

## <a name="mobile-and-native-apps"></a>Mobile en systeemeigen apps
Apps die zijn geïnstalleerd op apparaten, zoals een mobiele en bureaublad-apps, moeten vaak toegang hebben tot de back-enddatabase services of web API's namens gebruikers. U kunt aangepaste identiteit management ervaringen toevoegen aan uw interne apps en veilig gesprek backend-services via Azure AD B2C en de [OAuth 2.0 autorisatie code stroom](active-directory-b2c-reference-oauth-code.md).  

In deze stroom, de app wordt uitgevoerd [beleidsregels](active-directory-b2c-reference-policies.md) en ontvangt een `authorization_code` van Azure AD nadat de gebruiker het beleid is voltooid. De `authorization_code` vertegenwoordigt van de app-machtigingen om te bellen van back-enddatabase services namens de gebruiker die momenteel is aangemeld. De app kunt vervolgens uitwisselen de `authorization_code` op de achtergrond voor een `id_token` en een `refresh_token`.  De app de beschikking over de `id_token` om te verifiëren op een back-enddatabase web API in HTTP-aanvragen. Zo kunt u de `refresh_token` om een nieuwe `id_token` waarop een oudere verloopt.

> [AZURE.NOTE]
    Azure AD B2C ondersteunt momenteel alleen tokens die worden gebruikt voor toegang tot een back-enddatabase webservice van de app. Uw volledige app bevatten bijvoorbeeld een iOS-app, een Android-app en een back-enddatabase web API. Deze architectuur wordt volledig ondersteund. Uw iOS-app voor toegang tot een partner web API met OAuth 2.0 access tokens toestaan is momenteel niet ondersteund. Alle onderdelen van de volledige app moet delen een één toepassing-ID.

![Afbeelding van de zwembanen systeemeigen-App](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Huidige beperkingen
Azure AD B2C ondersteunt de volgende soorten apps niet momenteel, maar ze zijn op de roadmapy. Extra beperkingen en beperkingen die betrekking hebben op Azure AD B2C worden beschreven in [beperkingen](active-directory-b2c-limitations.md).

### <a name="single-page-apps-javascript"></a>Eén pagina apps (JavaScript)
Veel moderne apps hebben een front-end van één pagina app hoofdzakelijk in JavaScript geschreven. Ze vaak een kader zoals AngularJS, Ember.js of Durandal gebruiken. De algemeen beschikbaar Azure AD-service ondersteuning biedt voor deze apps met behulp van de impliciete OAuth 2.0-stroom. Deze stroom is echter niet nog beschikbaar zijn in Azure AD B2C.

### <a name="daemonsserver-side-apps"></a>Daemons/servers apps
Apps die langdurige processen bevatten of die worden toegepast zonder de aanwezigheid van een gebruiker moeten ook toegang krijgen tot beveiligde resources zoals web API's. Deze apps kunnen verifiëren en tokens krijgen met behulp van de app-identiteit (in plaats van een gedelegeerd gebruikersidentiteit) en met behulp van de OAuth 2.0-client referenties stroom.

Deze stroom wordt momenteel niet ondersteund door Azure AD B2C. Deze apps kunnen tokens krijgen alleen nadat de stroom van een interactieve gebruiker heeft plaatsgevonden.

### <a name="web-api-chains-on-behalf-of-flow"></a>Web API koppelt (op-namens-van-mailstroom)
Veel architecturen bevatten een web API die vereist zijn voor het bellen van een ander volgende web API, waarbij beide zijn beveiligd met Azure AD B2C. Dit scenario geldt in systeemeigen clients waarvoor een Web API-back-enddatabase. Dit vervolgens oproepen een Microsoft-onlineservice zoals de API Azure AD Graph.

Dit scenario gekoppelde web API kan worden ondersteund met behulp van het OAuth 2.0 JWT vruchtdragende referentie verlenen, ook bekend als de stroom op-namens-of.  De stroom op-namens-of is echter niet momenteel geïmplementeerd in de Azure AD-B2C.
