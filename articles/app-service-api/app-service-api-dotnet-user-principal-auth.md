<properties
    pageTitle="Gebruikersverificatie voor API-Apps in Azure App Service | Microsoft Azure"
    description="Informatie over het beveiligen van een API-app in de App-Service van Azure doordat alleen toegang tot geverifieerde gebruikers."
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

# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Gebruikersverificatie voor API-Apps in Azure App-Service

## <a name="overview"></a>Overzicht

In dit artikel leest hoe u een app Azure-API beveiligen zodat alleen geverifieerde gebruikers deze kunnen bellen. Het artikel wordt ervan uitgegaan dat u het [overzicht van Azure-Service voor App-verificatie](../app-service/app-service-authentication-overview.md)hebt gelezen.

U leert:

* Klik hier voor meer informatie over het configureren van een verificatieprovider, met details van Azure Active Directory (Azure AD).
* Klik hier voor meer informatie over het gebruiken van een beveiligde API-app met behulp van de [Active Directory verificatie bibliotheek (ADAL) voor JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

Het artikel bevat twee secties:

* De sectie [gebruikersverificatie in Azure App-Service configureren](#authconfig) in het algemeen wordt uitgelegd hoe u gebruikersverificatie configureren voor een API-app en geldt ook voor alle kaders worden ondersteund door de App-Service, inclusief .NET, Node.js en Java.

* Beginnen met het gedeelte [doorlopende de zelfstudies .NET-API-Apps](#tutorialstart) , de hulplijnen artikel u via het configureren van een toepassing voor de steekproef met een .NET back-end en een voorgrond AngularJS eindigen, zodat de site gebruikmaakt van Azure Active Directory voor gebruikersverificatie. 

## <a id="authconfig"></a>Het configureren van gebruikersverificatie in Azure App-Service

Deze sectie bevat algemene instructies die van toepassing zijn op alle API-Apps. Voor bepaalde stappen voor de toepassing van de steekproef naar doen lijst-.NET, gaat u naar [de zelfstudies .NET introductie doorlopende](#tutorialstart).

1. Ga naar het blad **Instellingen** van de API-app die u wilt beveiligen, zoek de sectie **functies** en klik vervolgens op in de [portal van Azure](https://portal.azure.com/) **verificatie / autorisatie**.

    ![Azure-portal verificatie](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In de **verificatie / autorisatie** blade, klikt u **op**.

4. Selecteer een van de opties in de vervolgkeuzelijst **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** .

    * Als u wilt dat alleen geverifieerde oproepen naar uw API-app hebt bereikt, kiest u een van de opties **Meld u aan met...** . Deze optie kunt u de app API beveiligen zonder het schrijven van een code, die wordt uitgevoerd in deze.

    * Als u alle gesprekken wilt laten uw API-app hebt bereikt, kiest u **aanvraag toestaan (geen actie)**. U kunt deze optie niet-geverifieerde bellers met een keuze van verificatieproviders verwijzen. Met deze optie moet u programmacode om af te handelen autorisatie schrijven.

    Zie [verificatie en autorisatie voor API-Apps in Azure App-Service](app-service-api-authentication.md#multiple-protection-options)voor meer informatie.

5. Selecteer een of meer van de **Verificatieproviders**.

    De afbeelding ziet u keuzes waarbij alle Bellers kunnen worden geverifieerd door Azure AD.
 
    ![Azure portal verificatie blade](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

    Als u een verificatieprovider kiest, wordt in de portal wordt een blade configuratie weergegeven voor de provider. 

    Lees [hoe u het configureren van uw App-servicetoepassing gebruik van de Azure Active Directory-aanmelding](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)voor gedetailleerde instructies waarin wordt uitgelegd hoe u de verificatie-provider configuratie bladen configureert. (De koppeling naar een artikel over Azure AD gaat, maar het artikel zelf bevat tabbladen die zijn gekoppeld aan artikelen voor de andere verificatieproviders.)

7. Wanneer u klaar bent met het blad verificatie provider configuratie, klik op **OK**.

7. In de **verificatie / autorisatie** blade, klikt u op **Opslaan**.

Als dit klaar is, wordt alle API-oproepen in App Service geverifieerd voordat ze de API-app hebt bereikt. De verificatieservices werken op dezelfde manier voor alle talen die service App worden ondersteund, inclusief .NET, Node.js en Java. 

Als u geverifieerde API-oproepen, bevat de beller van de verificatieprovider 2.0 OAuth-dragertoken in de koptekst autorisatie van HTTP-aanvragen. Het token kan worden verkregen met behulp van de verificatieprovider SDK.

## <a id="tutorialstart"></a>De zelfstudies .NET API Apps doorlopende

Als u de Node.js of Java zelfstudies voor API-apps volgt, gaat u verder met het volgende artikel, [service principal verificatie voor API-apps](app-service-api-dotnet-service-principal-auth.md). 

Als u de reeks .NET voor API-apps volgen en de voorbeeldtoepassing al werkt zoals u werd gevraagd in de [eerste](app-service-api-dotnet-get-started.md) en [tweede](app-service-api-cors-consume-javascript.md) zelfstudies, gaat u verder met de sectie [verificatie in App Service en Azure AD instellen](#azureauth) .

Als u deze zelfstudie volgen wilt zonder tussenkomst van de eerste en tweede zelfstudies, doet u de volgende stappen waarin wordt uitgelegd hoe u aan de slag met behulp van een geautomatiseerde proces om te implementeren van de steekproef-toepassing.

>[AZURE.NOTE] De volgende stappen u aan de naar hetzelfde beginpunt als wanneer u de eerste twee zelfstudies, met één uitzondering hebt: Visual Studio al weet niet welke WebApp of API-app, die elk project wordt geïmplementeerd in. Dat betekent dat u hebt geen exacte instructies in deze zelfstudie waarin wordt uitgelegd hoe implementeren naar de juiste doelen. Als u niet bekend bent met uitzoeken hoe u de implementatiestappen op uw eigen, is het beter de reeks uit de [eerste zelfstudie](app-service-api-dotnet-get-started.md) dan om te beginnen met dit proces geautomatiseerd volgen.

1. Zorg ervoor dat u alle vereisten weergegeven in de [eerste zelfstudie](app-service-api-dotnet-get-started.md)hebt. Deze zelfstudies verificatie naast de vereisten die wordt weergegeven, is ervan uitgegaan dat u hebt gewerkt met App Service WebApps en API-apps in Visual Studio en de Azure-portal.

2. Klik op de knop **distribueren naar Azure** in de [takenlijst steekproef opslagplaats Leesmij-bestand](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) voor het implementeren van de API-apps en de web-app. Maak een notitie van de Azure resourcegroep die wordt gemaakt, zoals u deze kunt later WebApp en API app namen opzoeken.
 
3. Downloaden of de [takenlijst steekproef opslagplaats](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) als u de code die u met lokaal in Visual Studio werken moet klonen.

## <a id="azureauth"></a>Verificatie in App Service en Azure AD instellen

U hebt nu de toepassing uitvoert in Azure App Service zonder dat gebruikers worden geverifieerd. In deze sectie kunt u verificatie toevoegen door de volgende taken kunnen uitvoeren:

* App Service Azure Active Directory (Azure AD) is verificatie vereist voor het bellen van de middelste laag API-app configureren.
* Maak een Azure AD-toepassing.
* De toepassing Azure AD de dragertoken na aanmelding verzenden naar de front-end van de AngularJS configureren. 

Als u problemen ondervindt bij de Zelfstudievideo aanwijzingen, raadpleegt u de sectie [Probleemoplossing](#troubleshooting) aan het einde van de zelfstudie. 
 
### <a name="configure-authentication-for-the-middle-tier-api-app"></a>Verificatie configureren voor de middelste laag API-app

1. In de [portal van Azure](https://portal.azure.com/), gaat u naar het blad **Instellingen** van de API-app die u hebt gemaakt voor het project ToDoListAPI, zoek de sectie **functies** en klik vervolgens op **verificatie / autorisatie**.

    ![Azure-portal verificatie](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In de **verificatie / autorisatie** blade, klikt u **op**.

4. Selecteer **aanmelden met de Azure Active Directory**in de vervolgkeuzelijst **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** .

    Deze optie zorgt ervoor dat er geen aanvragen unauathenticated de API-app wordt bereikt. 

5. Klik onder **Verificatieproviders**op **Azure Active Directory**.

    ![Azure portal verificatie blade](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Klik in het blad **Azure Active Directory-instellingen** op **Express**

    ![Azure portal verificatie Express optie](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    Met de optie **snel** kunt App-Service automatisch een Azure AD-toepassing maken in uw Azure AD- [tenant](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    U hoeft te maken van een tenant, omdat elke Azure-account een automatisch heeft.

7. Klik onder **beheer van de modus**op **Nieuwe AD-App maken** als dit nog niet geselecteerd en ziet u de waarde die zich in het tekstvak **Maken App** . u kunt deze AAD toepassing later in de portal van Azure klassieke opzoeken.

    ![Azure-portal Azure AD-instellingen](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

    Azure wordt automatisch een Azure AD-toepassing maken met de naam van de opgegeven in uw Azure AD-tenant. Standaard de Azure AD-toepassing heet hetzelfde als de API-app. Als u liever, kunt u een andere naam invoeren.
 
7. Klik op **OK**.

7. In de **verificatie / autorisatie** blade, klikt u op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Alleen gebruikers in uw Azure AD-tenant kunnen nu de API-app te bellen.

### <a name="optional-test-the-api-app"></a>Optioneel: Test de API-app

1. Ga naar de URL van de API-app in een browser: het blad **API-app** in de portal van Azure, klik op de koppeling onder **URL**.  

    U wordt omgeleid naar een aanmeldingsvenster omdat er niet-geverifieerde aanvragen zijn niet toegestaan tot de API-app.

    Als uw browser gaat naar de pagina 'is gemaakt', de browser al worden geregistreerd op--in dat geval open een InPrivate- of Incognito venster en Ga naar de URL van de API-app.

2. Meld u aan met referenties voor een gebruiker in uw Azure AD-tenant.

    Wanneer u bent aangemeld, wordt de 'is gemaakt' pagina wordt weergegeven in de browser.

9. Sluit de browser.

### <a name="configure-the-azure-ad-application"></a>De Azure AD-toepassing configureren

Wanneer u Azure AD-verificatie hebt geconfigureerd, wordt in App Service een Azure AD-toepassing voor u gemaakt. Toepassing is al dan niet standaard de nieuwe Azure AD geconfigureerd voor het dragertoken naar de URL van de API-app. In deze sectie configureren van de Azure AD-toepassing te leveren van de dragertoken naar de AngularJS front-end in plaats van rechtstreeks naar de middelste laag API-app. De front-end van de AngularJS stuurt de token naar de app API wanneer deze de API-app u belt.

>[AZURE.NOTE] Het proces bewaren als eenvoudig mogelijk, deze zelfstudie gebruikt per één Azure AD trapsgewijs-toepassing voor de front-end van zowel het midden API-app. Er is een andere optie twee Azure AD-toepassingen gebruiken. U moet in dat geval de front-end Azure AD-toepassing toestemming geven voor het bellen van de middelste laag Azure AD-toepassing. Deze methode meerdere toepassingen, krijgt u meer gedetailleerde controle over machtigingen voor elke laag.

11. Ga in de [klassieke Azure-portal](https://manage.windowsazure.com/)naar **Azure Active Directory**.

    U moet de klassieke portal gebruiken omdat bepaalde Azure Active Directory-instellingen die u nodig toegang tot hebt nog niet beschikbaar in de huidige Azure-portal.

12. Klik op het tabblad **map** op uw AAD-tenant.

    ![Azure AD in klassieke-portal](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. Klik op **toepassingen > toepassingen mijn bedrijf eigenaar is van**, en klik vervolgens op het vinkje.

    Mogelijk moet u ook de pagina die u wilt zien van de nieuwe toepassing te vernieuwen.

15. Klik op de naam van degene die Azure voor u gemaakt wanneer u verificatie ingeschakeld voor uw app API in de lijst met toepassingen.

    ![Tabblad met Azure AD-toepassingen](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. Klik op **configureren**.

    ![Azure AD-configureren-tabblad](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. **Eenmalige aanmelding URL** naar de URL voor uw AngularJS WebApp, niet volgspaties slash instellen.

    Bijvoorbeeld: https://todolistangular.azurewebsites.net

    ![URL-aanmelding](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. **Antwoord-URL** naar de URL voor uw web-app, niet volgspaties slash instellen.

    Bijvoorbeeld: https://todolistsangular.azurewebsites.net

16. Klik op **Opslaan**.

    ![Antwoord URL](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. Klik onder aan de pagina, op **manifest beheren > downloaden manifest**.

    ![Manifest downloaden](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. Download het bestand naar een locatie waar u deze kunt bewerken.

16. Zoek in het gedownloade bestand manifest naar de `oauth2AllowImplicitFlow` eigenschap. Wijzig de waarde van deze eigenschap van `false` naar `true`, en sla het bestand.

    Deze instelling is vereist voor toegang via een JavaScript-toepassing voor één pagina. U kunt de 2.0 Oauth-dragertoken moeten worden geretourneerd in de URL-fragment.

16. Klik op **manifest beheren > Upload manifest**, en upload het bestand dat u in de vorige stap hebt bijgewerkt.

    ![Manifest uploaden](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. Kopieer de waarde van de **Client-ID** en ergens die kunt u deze later kunt opslaan.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Het project ToDoListAngular voor verificatie configureren

In deze sectie wijzigen u de front-end van de AngularJS zodat de site gebruikmaakt van Active Directory verificatie bibliotheek (ADAL) voor JS waar u een dragertoken voor de gebruiker is aangemeld van Azure AD. De code bevat de token in HTTP-aanvragen verzonden naar de middelste laag, zoals wordt weergegeven in het volgende diagram. 

![Diagram van gebruiker verificatie](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

De volgende wijzigingen aanbrengen aan bestanden in het project ToDoListAngular.

1. Open het bestand *index.html* .

2. Verwijder de opmerkingen bij de regels die verwijzen naar de Active Directory verificatie bibliotheek (ADAL) voor JS-scripts.

        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>

1. Open het bestand *app/scripts/app.js* .

2. Opmerkingen toevoegen uit de blokkering van de code die zijn gemarkeerd voor "zonder verificatie" en verwijder de opmerkingen bij de blokkering van de code die zijn gemarkeerd voor "met verificatie".

    Deze wijziging wordt verwezen naar de ADAL JS-verificatie-provider en levert configuratiewaarden die deze. In de volgende stappen stelt u de configuratiewaarden voor uw API-app en Azure AD-toepassing.

8. In de code die Hiermee stelt u de `endpoints` variabele, de API-URL naar de URL van de app API instellen die u hebt gemaakt voor het project ToDoListAPI en de Azure AD toepassings-ID ingesteld op de client-ID die u hebt gekopieerd in de portal van Azure klassieke.

    De code is nu vergelijkbaar met het volgende voorbeeld.

        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. In de oproep door naar `adalProvider.init`, ingestelde `tenant` naar de naam van de tenant en `clientId` dezelfde waarde waarnaar u in de vorige stap hebt gebruikt.

    De code is nu vergelijkbaar met het volgende voorbeeld.

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

    Deze wijzigingen naar `app.js` opgeven of het bellen code en de genoemd API in de dezelfde Azure AD-toepassing zijn.

1. Open het bestand *app/scripts/homeCtrl.js* .

2. Opmerkingen toevoegen uit de blokkering van de code die zijn gemarkeerd voor "zonder verificatie" en verwijder de opmerkingen bij de blokkering van de code die zijn gemarkeerd voor "met verificatie".

1. Open het bestand *app/scripts/indexCtrl.js* .

2. Opmerkingen toevoegen uit de blokkering van de code die zijn gemarkeerd voor "zonder verificatie" en verwijder de opmerkingen bij de blokkering van de code die zijn gemarkeerd voor "met verificatie".

### <a name="deploy-the-todolistangular-project-to-azure"></a>Het project ToDoListAngular implementeren naar Azure

8. Met de rechtermuisknop op het project ToDoListAngular in **Solution Explorer**en klik vervolgens op **publiceren**.

9. Klik op **publiceren**.

    Visual Studio wordt geïmplementeerd van het project en wordt een browser op basis van de web-app-URL wordt geopend. Hier ziet een 403 foutpagina, dat wil zeggen normale voor een poging om te gaan naar een basis Web API-URL in een browser.

    U moet altijd een wijziging aanbrengt in de middelste laag API-app voordat u de toepassing kunt testen.

10. Sluit de browser.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Het project ToDoListAPI voor verificatie configureren

Momenteel het project ToDoListAPI stuurt "*" Als de `owner` te ToDoListDataAPI waarde. In deze sectie wijzigt u de code als u wilt verzenden, de ID van de gebruiker is aangemeld.

De volgende wijzigingen aanbrengen in het project ToDoListAPI.

1. Open het bestand *Controllers/ToDoListController.cs* en verwijder de opmerkingen bij de regel in elke methode actie die wordt ingesteld `owner` naar de Azure AD `NameIdentifier` waarde claimen. Bijvoorbeeld:

        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

    **Belangrijk**: Verwijder de code in opmerkingen niet bij de `ToDoListDataAPI` methode; u kunt dit doen later voor de service principal verificatie zelfstudie.

### <a name="deploy-the-todolistapi-project-to-azure"></a>Het project ToDoListAPI implementeren naar Azure

8. Met de rechtermuisknop op het project ToDoListAPI in **Solution Explorer**en klik vervolgens op **publiceren**.

9. Klik op **publiceren**.

    Visual Studio wordt geïmplementeerd van het project en wordt een browser op basis van de API-app-URL wordt geopend.

10. Sluit de browser.

### <a name="test-the-application"></a>De toepassing testen

9. Ga naar de URL van de web-app, **met behulp van HTTPS niet HTTP**.

8. Klik op het tabblad **Takenlijst** .

    U wordt gevraagd of u aan te melden.

9. Meld u aan met de referenties van een gebruiker in uw AAD-tenant.

10. De pagina **Takenlijst** wordt weergegeven.

    ![Takenlijst pagina](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

    Geen taakitems worden weergegeven, omdat u tot nu alle zijn eigenaar "*". Nu de middelste laag aanvraagt items voor de gebruiker is aangemeld en geen nog zijn gemaakt.

11. Voeg nieuwe taakitems om te bevestigen dat de toepassing werkt.

12. In een ander browservenster, gaat u naar de URL van de gebruikersinterface Swagger voor de ToDoListDataAPI API-app en klikt u op **takenlijst van > krijgen**. Voer een sterretje voor de `owner` parameter, en klik vervolgens op **het afwezigheidsbericht uitproberen**.

    Het antwoord wordt weergegeven dat de nieuwe taakitems de werkelijke hebben Azure AD-gebruikers-ID in de eigenschap eigenaar.

    ![Eigenaar-ID in JSON-antwoord](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## <a name="building-the-projects-from-scratch"></a>Samenstellen van de projecten maken

De twee Web API-projecten zijn gemaakt met de project-sjabloon van **Azure API-App** en de controller uit standaard waarden vervangen door een takenlijst van controller. 

Zie voor informatie over het maken van een toepassing van AngularJS één pagina met Web API 2 back-enddatabase [handen op testomgeving: maken van een één toepassing pagina WACHTWOORDVERIFICATIE met ASP.NET Web API en Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Zie de volgende bronnen voor informatie over het toevoegen van Azure AD-verificatiecode:

* [AngularJS één pagina Apps gebruiken met Azure AD beveiligen](../active-directory/active-directory-devquickstarts-angular.md).
* [Inleiding tot ADAL JS v1](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Problemen oplossen

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Zorg ervoor dat u en niet door elkaar ToDoListAPI (middelste laag) ToDoListDataAPI (gegevenslaag). Bijvoorbeeld verifiëren dat u verificatie toegevoegd aan de middelste laag API-app niet de gegevenslaag. 
* Zorg ervoor dat de broncode AngularJS verwijst naar de URL van de app middelste laag API (ToDoListAPI, niet ToDoListDataAPI) en de juiste Azure AD-client-ID. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe App Service verificatie gebruiken voor een API-app en hoe u de app API bellen via de ADAL JS-bibliotheek. In de volgende zelfstudie leert u hoe u [beveiligde toegang tot de API-app voor service-naar-service scenario's](app-service-api-dotnet-service-principal-auth.md).

