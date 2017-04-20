<properties
    pageTitle="Service principal verificatie voor API-Apps in Azure App Service | Microsoft Azure"
    description="Informatie over het beveiligen van een API-app in Azure App-Service voor service-naar-service scenario's."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016" 
    ms.author="rachelap"/>

# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Service principal verificatie voor API-Apps in Azure App-Service

## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe u App Service verificatie voor *interne* toegang tot de API-apps. Een interne scenario is waar u een API-app die u wilt worden verbruikbare alleen door uw eigen toepassingscode. Het beste kunt implementeren in dit scenario in App Service is Azure AD gebruiken om te beveiligen van de genoemd API-app. U bellen de beveiligde API-app met een dragertoken die u van Azure AD doordat toepassings-id (service principal) referenties. Zie de sectie **Service-naar-service verificatie** van het [overzicht van Azure-Service voor App-verificatie](../app-service/app-service-authentication-overview.md#service-to-service-authentication)voor alternatieven voor het gebruik van Azure AD.

In dit artikel leert u het:

* Het gebruik van Azure Active Directory (Azure AD) om te voorkomen dat een API-app niet-geverifieerde toegang.
* Klik hier voor meer informatie over het gebruiken van een beveiligde API-app vanaf een API-app, in de browser of mobiele app via Azure AD-service hoofdsom (app identiteit) referenties. Zie [werken met uw aangepaste API die worden gehost op App-Service met logica apps](../app-service-logic/app-service-logic-custom-hosted-api.md)voor informatie over het gebruiken van een app logica.
* Hoe om ervoor te zorgen dat de beveiligde API-app in een browser kan niet worden aangeroepen door aangemelde gebruikers.
* Hoe om ervoor te zorgen dat de beveiligde API-app kan alleen worden aangeroepen door een specifieke Azure AD hoofdsom service.

Het artikel bevat twee secties:

* De sectie [service principal verificatie in Azure App-Service configureren](#authconfig) in het algemeen wordt uitgelegd hoe u verificatie configureren voor een API-app en hoe u de beveiligde API-app in beslag nemen. In deze sectie geldt gelijkmatig voor alle kaders worden ondersteund door de App-Service, inclusief .NET, Node.js en Java.

* Beginnen met de sectie [de zelfstudies .NET introductie doorlopende](#tutorialstart) , begeleidt de zelfstudie u bij een 'interne access'-scenario voor het voorbeeld van een .NET toepassing uitgevoerd in de App-Service configureren. 

## <a id="authconfig"></a>De hoofdsom verificatie service configureren in Azure App-Service

Deze sectie bevat algemene instructies die van toepassing zijn op alle API-Apps. Voor bepaalde stappen voor de toepassing van de steekproef naar doen lijst-.NET, gaat u naar [de reeks .NET API Apps doorlopende](#tutorialstart).

1. In de [portal van Azure](https://portal.azure.com/), gaat u naar het blad **Instellingen** van de API-app die u wilt beveiligen, zoek de sectie **functies** en klik op **verificatie / autorisatie**.

    ![Verificatie/autorisatie in Azure-portal](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In de **verificatie / autorisatie** blade, klikt u **op**.

4. Selecteer **aanmelden met de Azure Active Directory** in de vervolgkeuzelijst **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** .

5. Selecteer onder **Verificatieproviders** **Azure Active Directory**.

    ![Verificatie blade in Azure-portal](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Het blad **Azure Active Directory-instellingen** als u wilt maken een nieuw configureren Azure AD-toepassing, of gebruik een bestaande Azure AD-toepassing als u al een die u wilt gebruiken.

    Interne scenario's is meestal gebruikmaakt van een API-app voor het bellen van een API-app. U kunt afzonderlijke Azure AD-toepassingen voor elke API-app of slechts één Azure AD-toepassing.

    Zie [het configureren van uw App-servicetoepassing gebruik van de Azure Active Directory-aanmelding](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)voor gedetailleerde instructies voor deze blade.

7. Wanneer u klaar bent met het blad verificatie provider configuratie, klik op **OK**.

7. In de **verificatie / autorisatie** blade, klikt u op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Als dit klaar is, App-Service alleen aanvragen kunnen van bellers in de geconfigureerde Azure AD-tenant. Geen code verificatie of machtiging is vereist in de beveiligde API-app. De dragertoken wordt doorgegeven aan de app API samen met meest gebruikte claims in HTTP-headers en vindt u die gegevens in de code voor het valideren van dat aanvragen afkomstig van een bepaalde beller, zoals een service principal zijn.

Deze functionaliteit verificatie werkt op dezelfde manier voor alle talen die service App worden ondersteund, inclusief .NET, Node.js en Java. 

#### <a name="how-to-consume-the-protected-api-app"></a>Hoe u de beveiligde API-app gebruiken

De beller moet voorzien van een Azure AD-dragertoken API-oproepen. Als u een dragertoken met service principal referenties, wordt de beller gebruikt Active Directory-verificatie Library (ADAL voor [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)of [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). Als u een token, biedt de code die u ADAL belt ADAL de volgende informatie:

* De naam van uw Azure AD-tenant.
* De client-ID en geheim (app toets) in de client van de Azure AD-app die is gekoppeld aan de beller.
* De client-ID van de Azure AD-toepassing die is gekoppeld aan de beveiligde API-app. (Als slechts één Azure AD-toepassing wordt gebruikt, dit is dezelfde client-ID als de relatie voor de beller.)

Deze waarden zijn beschikbaar in de Azure AD-pagina's van de [Azure klassieke portal](https://manage.windowsazure.com/).

Zodra het token is aangeschaft, bevat de beller deze met HTTP-aanvragen in de koptekst autorisatie.  App-Service is gevalideerd met het token en kunnen de aanvragen de beveiligde API-app hebt bereikt.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>Hoe u de app API beveiligen tegen toegang door gebruikers in dezelfde tenant

Vruchtdragende tokens voor gebruikers in dezelfde tenant worden beschouwd als geldige voor de beveiligde API-app.  Als u wilt om ervoor te zorgen dat alleen de hoofdsom van een service de beveiligde API-app kunt bellen, kunt u de code toevoegen in de beveiligde API-app voor het valideren van de volgende claims van het token:

* `appid`moet u de client-ID van de Azure AD-toepassing die is gekoppeld aan de beller. 
* `oid`(`objectidentifier`) moet de principal-ID van de beller. 

App-Service biedt ook de `objectidentifier` claimen in de kop van de X-MS-CLIENT-PRINCIPAL-ID.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>Hoe u de app API beschermen tegen toegang verkrijgen met webbrowser

Als u gegevens in de code in de beveiligde API-app niet valideren en als u een afzonderlijke Azure AD-toepassing voor de beveiligde API-app, Controleer of URL van de reactie van de toepassing van de Azure AD is niet hetzelfde als de API-app basis-URL. Als de URL antwoord rechtstreeks naar de beveiligde API-app gegevenspunten, kan een gebruiker in de dezelfde Azure AD-tenant Blader naar de API-app, meld u aan en de API kunt bellen.

## <a id="tutorialstart"></a>De reeks .NET API Apps doorlopende

Als u de reeks Node.js of Java voor API-apps volgt, gaat u verder naar de sectie van de [volgende stappen](#next-steps) . 

De rest van dit artikel blijft de reeks .NET-API-Apps en wordt ervan uitgegaan dat u de [gebruiker verificatie zelfstudie](app-service-api-dotnet-user-principal-auth.md) hebt voltooid en de steekproef hebt uitgevoerd in Azure wordt aangegeven met gebruikersverificatie ingeschakeld.

## <a name="set-up-authentication-in-azure"></a>Verificatie in Azure instellen

In deze sectie u App Service zodanig configureren dat de enige HTTP-aanvragen Hiermee kunt de gegevens laag API-app hebt bereikt zijn de velden die u geldige Azure hebt AD vruchtdragende tokens. 

In de volgende sectie configureert u de middelste laag API-app om de referenties voor verzenden naar Azure AD, een dragertoken terug te gaan en de dragertoken verzenden naar de gegevens laag API-app. Dit proces wordt geïllustreerd in het diagram.

![Service verificatie-diagram](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Als u problemen ondervindt bij de Zelfstudievideo aanwijzingen, raadpleegt u de sectie [Probleemoplossing](#troubleshooting) aan het einde van de zelfstudie. 

1. Ga naar het blad **Instellingen** van de API-app die u hebt gemaakt voor de ToDoListDataAPI (gegevenslaag) API-app in de [portal van Azure](https://portal.azure.com/), en klik vervolgens op **Instellingen**.

2. Zoek de sectie **functies** in het blad **Instellingen** en klik op **verificatie / autorisatie**.

    ![Verificatie/autorisatie in Azure-portal](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In de **verificatie / autorisatie** blade, klikt u **op**.

4. Selecteer **aanmelden met de Azure Active Directory**in de vervolgkeuzelijst **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** .

    Dit is de instelling waardoor App-Service om ervoor te zorgen dat alleen aanvragen bereik de API-app geverifieerde. Voor aanvragen met geldige vruchtdragende tokens, App Service de tokens langs worden doorgegeven aan de API-app en vult HTTP-headers met veelgebruikte claims die gegevens gemakkelijker om beschikbaar te stellen aan uw code.

5. Klik onder **Verificatieproviders**op **Azure Active Directory**.

    ![Verificatie blade in Azure-portal](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Klik in het blad **Azure Active Directory-instellingen** op **Express**.

    Met de **Express** maken optie Azure automatisch een AAD-toepassing in uw Azure AD- [tenant](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    U hoeft te maken van een tenant, omdat elke Azure-account een automatisch heeft.

7. Klik op de **Nieuwe AD-App maken** onder **Management modus**, als dit nog niet geselecteerd.

    De portal wordt het invoervak **App maken** met een standaardwaarde. Standaard de Azure AD-toepassing heet hetzelfde als de API-app. Als u liever, kunt u een andere naam invoeren.
    
    ![Azure AD-instellingen](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)

    **Opmerking**: als alternatief, kunt u een enkel Azure AD toepassing voor de bellen API-app en de beveiligde API-app. Als u ervoor deze alternatieve kiest, moet u niet de optie **Nieuwe AD-App maken** hier omdat u al een Azure AD-toepassing eerder in deze gebruiker verificatie zelfstudie hebt gemaakt. Voor deze zelfstudie gebruikt u scheiden van Azure AD-toepassingen voor de bellen API-app en de beveiligde API-app.

8. Noteer de waarde die is in het invoervak **App maken** ; u kunt deze AAD toepassing later in de portal van Azure klassieke opzoeken.

7. Klik op **OK**.

10. In de **verificatie / autorisatie** blade, klikt u op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)

    App-Service Hiermee maakt u een Azure Active Directory-toepassing met **eenmalige aanmelding URL** en **Beantwoorden URL** automatisch ingesteld op de URL van uw API-app. De laatste waarde kan gebruikers in uw tenant AAD aanmelden en toegang tot de API-app.

### <a name="verify-that-the-api-app-is-protected"></a>Controleer of dat de API-app is beveiligd

1. Ga naar de URL van de API-app in een browser: het blad **API-app** in de portal van Azure, klik op de koppeling onder **URL**. 

    U wordt omgeleid naar een aanmeldingsvenster omdat er niet-geverifieerde aanvragen zijn niet toegestaan tot de API-app. 

    Als uw browser naar de gebruikersinterface Swagger gaat, al uw browser mogelijk geregistreerd op--in dat geval open een InPrivate- of Incognito venster en Ga naar de URL van de gebruikersinterface Swagger.

18. Aanmelden met de referenties van een gebruiker in uw AAD-tenant.

    Wanneer u bent aangemeld, wordt de 'is gemaakt' pagina wordt weergegeven in de browser.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Het project ToDoListAPI als u wilt aanschaffen en verzend het Azure AD-token configureren

In deze sectie kunt u de volgende taken uitvoeren:

* Code in de middelste laag API-app dat Azure AD-toepassing referenties gebruikt bij het aanschaffen van een token en stuur dit met HTTP-aanvragen naar de gegevens laag API-app toevoegen.
* De referenties die u nodig hebt krijgen van Azure AD.
* Voer de referenties in Azure App Service runtime omgevingsinstellingen in de middelste laag API-app. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Het project ToDoListAPI als u wilt aanschaffen en verzend het Azure AD-token configureren

De volgende wijzigingen aanbrengen in het project ToDoListAPI in Visual Studio.

1. Verwijder de opmerkingen bij alle code in het bestand *ServicePrincipal.cs* .

    Dit is de code waarin ADAL voor .NET voor het ophalen van de Azure AD-dragertoken wordt gebruikt.  Configuratiewaarden van verschillende die u hebt ingesteld in de runtimeomgeving Azure later wordt gebruikt. Hier ziet u de code: 

        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
        
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
        
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }

    **Notitie:** Deze code vereist de ADAL voor .NET NuGet-pakket (Microsoft.IdentityModel.Clients.ActiveDirectory), die al in het project is geïnstalleerd. Als u dit project helemaal maakt, moet u dit pakket installeren. Dit pakket wordt niet automatisch geïnstalleerd door de API app nieuw project-sjabloon.

2. In *Controllers/ToDoListController*, verwijder de opmerkingen bij de code in de `NewDataAPIClient` methode die het token aan HTTP worden toegevoegd in de koptekst autorisatie aanvraagt.

        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);

3. Het project ToDoListAPI implementeren. (Met de rechtermuisknop op het project en klik vervolgens op **publiceren > publiceren**.)

    Visual Studio wordt geïmplementeerd van het project en wordt een browser op basis van de web-app-URL wordt geopend. Hier ziet een 403 foutpagina, dat wil zeggen normale voor een poging om te gaan naar een basis Web API-URL in een browser.

4. Sluit de browser.

### <a name="get-azure-ad-configuration-values"></a>Azure AD configuratiewaarden ophalen

11. Ga in de [klassieke Azure-portal](https://manage.windowsazure.com/)naar **Azure Active Directory**.

12. Klik op het tabblad **map** op uw AAD-tenant.

14. Klik op **toepassingen > toepassingen mijn bedrijf eigenaar is van**, en klik vervolgens op het vinkje.

15. Klik in de lijst met toepassingen, op de naam van degene die Azure voor u gemaakt wanneer u verificatie voor de ToDoListDataAPI (gegevenslaag) API-app hebt ingeschakeld.

16. Klik op het tabblad **configureren** .

5. Kopieer de waarde van de **Client-ID** en ergens die kunt u deze later kunt opslaan. 

8. In de portal van Azure klassieke gaat u terug naar de lijst met **Mijn bedrijf eigenaar is van toepassingen**en klik op de AAD-toepassing die u hebt gemaakt voor de middelste laag ToDoListAPI API-app (de tekst die u in de eerdere zelfstudie hebt gemaakt, niet de tekst die u in deze zelfstudie hebt gemaakt).

16. Klik op het tabblad **configureren** .

5. Kopieer de waarde van de **Client-ID** en ergens die kunt u deze later kunt opslaan.

6. Selecteer onder **toetsen** **1 jaar** uit de vervolgkeuzelijst **Selecteer duur** .

6. Klik op **Opslaan**.

    ![App-sleutel genereren](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. De sleutelwaarde kopiëren en deze ergens die kunt u deze later kunt opslaan.

    ![Nieuwe app-code kopiëren](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>Azure AD-instellingen configureren in de middelste laag API-app runtime-omgeving

1. Ga naar de [Azure-portal](https://portal.azure.com/)en ga vervolgens naar het blad **API-App** voor de API-app waarop het project TodoListAPI (middelste laag).

2. Klik op **Instellingen > Toepassingsinstellingen**.

3. In de sectie **instellingen van de App** toevoegen de volgende sleutels en waarden:

  	| **Toets** | IDA: certificeringsinstantie |
  	|---|---|
  	| **Waarde** | https://login.microsoftonline.com/ {uw naam Azure AD-tenant} |
  	| **Voorbeeld** | https://login.microsoftonline.com/contoso.onmicrosoft.com |

  	| **Toets** | IDA: ClientId |
  	|---|---|
  	| **Waarde** | Cliënt-ID van de bellen toepassing (middelste laag - ToDoListAPI) |
  	| **Voorbeeld** | 960adec2-b74a-484a-960adec2-b74a-484a |

  	| **Toets** | IDA: ClientSecret |
  	|---|---|
  	| **Waarde** | App-toets van de bellen toepassing (middelste laag - ToDoListAPI) |
  	| **Voorbeeld** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

  	| **Toets** | IDA: Resource |
  	|---|---|
  	| **Waarde** | Cliënt-ID van de toepassing genoemd (gegevenslaag - ToDoListDataAPI) |
  	| **Voorbeeld** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

    **Opmerking**: voor `ida:Resource`, zorg ervoor dat u de naam van de toepassing **cliënt-ID** en niet de **App-ID-URI**.

    `ida:ClientId`en `ida:Resource` zijn verschillende waarden voor deze zelfstudie omdat u gebruikt scheiden Azure AD-applicaations voor de middelste laag en van gegevens. Als u één Azure AD-toepassing voor de bellen API-app en de beveiligde API-app, gebruikt u dezelfde waarde in beide `ida:ClientId` en `ida:Resource`.

    De code wordt ConfigurationManager gebruikt om deze waarden, zodat ze kunnen worden opgeslagen in het projectbestand Web.config of in de Azure runtime-omgeving. Terwijl een ASP.NET-toepassing in Azure App-Service wordt uitgevoerd, worden de instellingen van Web.config omgevingsinstellingen automatisch opheffen. Omgevingsinstellingen zijn in het algemeen een [veiligere manier gevoelige gegevens vergeleken met een Web.config-bestand wilt opslaan](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

6. Klik op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>De toepassing testen

1. In een browser gaat u naar de HTTPS-URL van de AngularJS front-end WebApp.

2. Klik op het tabblad van de **Takenlijst** en log in met de referenties voor een gebruiker in uw Azure AD-tenant. 

4. Voeg taakitems om te bevestigen dat de toepassing werkt.

    ![Takenlijst pagina](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

    Als de toepassing niet werkt zoals verwacht, controleert u alle instellingen die u hebt ingevoerd in de portal van Azure. Als geen van de instellingen correct weergegeven, raadpleegt u de sectie [Probleemoplossing](#troubleshooting) verderop in deze zelfstudie.

## <a name="protect-the-api-app-from-browser-access"></a>De app API beschermen tegen toegang verkrijgen met webbrowser

Voor deze zelfstudie u een afzonderlijke gemaakt Azure AD-toepassing voor de app API ToDoListDataAPI (gegevenslaag). Zoals u hebt geleerd, wanneer de App Service Hiermee maakt u een AAD-toepassing, wordt de toepassing AAD geconfigureerd in zodanig dat een gebruiker om te gaan naar de URL van de API-app in een browser en meld u aan. Dat betekent dat is het mogelijk dat voor een eindgebruiker in uw Azure AD-tenant, niet alleen een service principal, voor toegang tot de API. 

Als u voorkomen dat toegang verkrijgen met webbrowser wilt zonder het schrijven van een code, die in de beveiligde API-app, kunt u de **URL van het antwoord** in de toepassing AAD wijzigen zodat deze van de API-app basis-URL verschilt. 

### <a name="disable-browser-access"></a>Toegang verkrijgen met webbrowser uitschakelen

1. In de klassieke portal **configureren** tabblad voor de AAD-toepassing die is gemaakt voor de TodoListService door de waarde in het veld **Antwoord URL** te wijzigen zodat deze een geldige URL, maar niet de API-app-URL is.
 
2. Klik op **Opslaan**.

### <a name="verify-browser-access-no-longer-works"></a>Controleer of werkt niet meer toegang verkrijgen met webbrowser

U geverifieerd eerder dat u naar de URL van de app API van een browser gaat door met de referenties van een individuele gebruiker zich te melden. In deze sectie controleert u dat dit niet meer mogelijk is. 

1. In een nieuw browservenster, Ga naar de URL van de API-app opnieuw.

2. Meld u aan wanneer u daarom wordt gevraagd.

3. Aanmelding is uitgevoerd, maar leidt tot een foutpagina.

    U kunt de AAD-app hebt geconfigureerd, zodat gebruikers in de tenant AAD niet kunnen aanmelden en toegang de API in een browser tot. U kunt nog steeds toegang tot de API-app met behulp van een service principal token, waarin u kunt controleren of door te gaan naar de URL van de WebApp en meer taakitems toe te voegen.

## <a name="restrict-access-to-a-particular-service-principal"></a>Toegang tot een bepaalde service principal beperken  

Rechts nu een beller die een token kunt ophalen voor een gebruiker of service hoofdsom in uw Azure AD-tenant de TodoListDataAPI (gegevenslaag) API-app kunt bellen. Het is raadzaam om ervoor te zorgen dat de gegevens laag API-app alleen oproepen uit de TodoListAPI (middelste laag) API-app en alleen uit een bepaalde service principal accepteert. 

U kunt deze beperkingen toevoegen door toe te voegen code voor het valideren van de `appid` en `objectidentifier` vorderingen op binnenkomende oproepen.

Voor deze zelfstudie plaatst u de code die is gevalideerd met app-ID en service principal-ID rechtstreeks in uw controller-acties.  Alternatieven zijn voor het gebruik van een aangepaste `Authorize` kenmerk of als u deze validatie in uw opstarten sequences (bijvoorbeeld OWIN middleware). Zie voor een voorbeeld van de laatste, [in dit voorbeeld-toepassing](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

De volgende wijzigingen aanbrengen aan het project TodoListDataAPI.

2. Open het bestand *Controllers/TodoListController.cs* .

3. Verwijder de opmerkingen bij de regels die ingesteld `trustedCallerClientId` en `trustedCallerServicePrincipalId`.

        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];

4. Verwijder de opmerkingen bij de code in de methode CheckCallerId. Deze methode wordt aangeroepen aan het begin van elke methode actie in de controller uit. 

        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }

5. Implementeer deze opnieuw in het project ToDoListDataAPI Azure App-Service.

6. Ga naar de AngularJS front-end WebApp HTTPS-URL in uw browser en klik op het tabblad **Takenlijst** op de introductiepagina.

    De toepassing werkt niet omdat oproepen naar de back-end worden verbroken. De nieuwe code wordt gecontroleerd werkelijke toepassings-id en objectidentifier, maar deze de juiste waarden te controleren als deze tegen nog niet hebt. De browser ontwikkelaars hulpmiddelen voor Console rapporten dat een fout HTTP 401 wordt de server is geretourneerd.

    ![Fout in ontwikkelaars hulpprogramma's voor Console](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)

    In de volgende stappen configureert u de verwachte waarden.

8. Azure AD PowerShell gebruikt, de waarde van de service-hoofdsom krijgen voor de Azure AD-toepassing die u hebt gemaakt voor het project TodoListWebApp.

    een. Zie voor instructies over het installeren van Azure PowerShell en verbinding maken met uw abonnement, [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md).

    b. Als u een lijst met service principes, uitvoeren de `Login-AzureRmAccount` opdracht en kiest u vervolgens de `Get-AzureRmADServicePrincipal` opdracht.

    c. De object-id voor de service hoofdsom van de toepassing TodoListAPI zoeken en sla deze op een locatie die u later kunt kopiëren uit.

7. Ga naar het blad API-app voor de API-app die u het project ToDoListDataAPI geïmplementeerd in de portal Azure.

9. Klik op **Instellingen > Toepassingsinstellingen**.

3. In de sectie **instellingen van de App** toevoegen de volgende sleutels en waarden:

  	| **Toets** | TODO:TrustedCallerServicePrincipalId |
  	|---|---|
  	| **Waarde** | Service principal-id van het aanroepen van toepassing |
  	| **Voorbeeld** | 4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |

  	| **Toets** | TODO:TrustedCallerClientId |
  	|---|---|
  	| **Waarde** | Cliënt-ID van het aanroepen van toepassing - hebt gekopieerd uit de TodoListAPI Azure AD-toepassing |
  	| **Voorbeeld** | 960adec2-b74a-484a-960adec2-b74a-484a |

6. Klik op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)

6. Ga terug naar de web-app-URL in uw browser en klik op het tabblad **Takenlijst** op de introductiepagina.

    Nu de toepassing werkt zoals verwacht omdat de app vertrouwde beller-ID en service principal-ID zijn de verwachte waarden.

    ![Takenlijst pagina](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>Samenstellen van de projecten maken

De twee Web API-projecten zijn gemaakt met de project-sjabloon van **Azure API-App** en de controller uit standaard waarden vervangen door een takenlijst van controller. Voor het verkrijgen van Azure AD-service principal tokens in het project ToDoListAPI, is het [Active Directory verificatie bibliotheek (ADAL) voor .NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet-pakket geïnstalleerd.
 
Zie voor informatie over het maken van een toepassing van AngularJS één pagina met Web API back-enddatabase zoals ToDoListAngular [handen op testomgeving: maken van een één toepassing pagina WACHTWOORDVERIFICATIE met ASP.NET Web API en Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Zie voor informatie over het toevoegen van Azure AD-verificatiecode [Beveiligen AngularJS één pagina-Apps gebruiken met Azure AD](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Problemen oplossen

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Zorg ervoor dat u en niet door elkaar ToDoListAPI (middelste laag) ToDoListDataAPI (gegevenslaag). Bijvoorbeeld: toevoegen u in deze zelfstudie verificatie aan de gegevens laag API-app, **maar de app-toets moet afkomstig zijn van de Azure AD-toepassing die u hebt gemaakt voor de middelste laag API-app**.

## <a name="next-steps"></a>Volgende stappen

Dit is de laatste zelfstudie in de reeks API-Apps. 

Zie de volgende bronnen voor meer informatie over Azure Active Directory.

* [Azure AD ontwikkelaars handleiding](http://aka.ms/aaddev)
* [Azure AD-scenario 's](http://aka.ms/aadscenarios)
* [Azure AD-voorbeelden](http://aka.ms/aadsamples)

    De steekproef [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) lijkt op wat wordt weergegeven in deze zelfstudie, maar zonder App Service-verificatie.

Zie voor informatie over andere manieren om te implementeren Visual Studio-projecten in API apps met behulp van Visual Studio of door te [automatiseren implementatie](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) van een [systeem voor bronbeheer gebruikt](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), [het implementeren van een app Azure App-Service](../app-service-web/web-sites-deploy.md).
