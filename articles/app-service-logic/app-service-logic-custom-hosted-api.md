<properties
    pageTitle="Een aangepaste API in de logica apps bellen"
    description="Uw aangepaste API die worden gehost op App-Service met logica apps gebruiken"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>Uw aangepaste API die worden gehost op App-Service met logica apps gebruiken

Hoewel logica apps bevat een uitgebreide set 40 + verbindingslijnen voor een groot aantal services, is het raadzaam om te bellen in uw eigen aangepaste API die uw eigen code kan worden uitgevoerd. Een van de makkelijkste en meest scalable manieren voor het hosten van uw eigen aangepaste web-API's is op het gebruik van App-Service. In dit artikel wordt uitgelegd hoe u inbellen bij een willekeurig web API gehost in een App Service API-app, in de browser of mobiele app.

Voor informatie over het samenstellen van API's als een trigger of actie binnen logica apps, raadpleegt u [in dit artikel](app-service-logic-create-api-app.md).

## <a name="deploy-your-web-app"></a>Implementeer de WebApp

Eerst moet u uw API te implementeren als een Web-App in de App-Service. De volgende instructies begeleidende eenvoudige implementatie: [een ASP.NET-web-app maken](../app-service-web/web-sites-dotnet-get-started.md).  Terwijl u in elke API via een app logica bellen kunt, voor de beste ervaring raden dat u Swagger metagegevens eenvoudig integreren met logica apps acties toevoegen.  U vindt meer informatie over het [toevoegen van swagger](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui).

### <a name="api-settings"></a>API-instellingen

In de volgorde voor de logica apps designer parseren van uw Swagger, is het belangrijk dat u CORS inschakelen en stel de eigenschappen APIDefinition van uw web-app.  Dit is heel eenvoudig om in te stellen van de Azure-Portal.  Gewoon openen van het blad instellingen van uw Web-App, en stel de 'API definitie' naar de URL van uw swagger.json-bestand (dit is meestal https://{name}.azurewebsites.net/swagger/docs/v1) onder de sectie API en voeg een CORS beleid voor ' *' om te staan voor aanmeldingsaanvragen van de logica apps Designer.

## <a name="calling-into-the-api"></a>Inbellen bij de API

Wanneer u binnen de logica apps-portal, als u CORS hebt ingesteld en de definitie van de API-eigenschappen u kunnen eenvoudig moet kunt u aangepaste API acties binnen uw flow toevoegen.  U kunt selecteren in de ontwerpfunctie naar websites van uw abonnement als u wilt de websites weergeven met de URL van een swagger gedefinieerd.  U kunt ook de HTTP + Swagger actie aan te wijzen aan een swagger en een lijst met beschikbare acties en invoeritems.  Tot slot kunt u altijd een verzoek om de actie HTTP gebruiken om te bellen van een API, zelfs wanneer die niet heeft of laten zien van een document swagger maken.

Als u beveiligen van uw API wilt, klik zijn er een aantal verschillende manieren om te doen:

1. Geen codewijziging vereist - Azure Active Directory kan worden gebruikt voor het beveiligen van uw API zonder codewijzigingen of opnieuw installeren.
1. Eenvoudige Auth, AAD Auth of certificaat Auth afdwingen in de code van uw API.

## <a name="securing-calls-to-your-api-without-a-code-change"></a>Oproepen naar uw API zonder een codewijziging beveiligen

In dit gedeelte maakt u twee Azure Active Directory-toepassingen – één voor uw app logica en één voor uw Web-App.  U kunt oproepen naar uw Web-App met de service-hoofdsom die is gekoppeld aan de AAD-toepassing voor de app logica (cliënt-id en geheim) verifiëren. Ten slotte de toepassing opneemt id in de definitie van uw logica-app.

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>Deel 1: Het instellen van een toepassings-id voor de logica-app

Dit is wat de logica-app wordt gebruikt om te verifiëren met active directory. U alleen *hoeft* te doen dit één keer voor uw adreslijst. Bijvoorbeeld, kunt u dezelfde identiteit gebruikt voor al uw apps logica, hoewel u ook unieke identiteiten per logica-app maken mogelijk als u wilt. U kunt als volgt in de gebruikersinterface of PowerShell gebruiken.

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>De identiteit van de groep met behulp van de Azure klassieke portal maken

1. Ga naar [Active directory in de klassieke Azure-portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) en selecteer de map die u voor uw Web-App gebruikt
2. Klik op het tabblad **toepassingen**
3. Klik op **toevoegen** in de opdrachtenbalk onder aan de pagina
4. Geef uw identiteit een naam, klikt u op de pijl-rechts
5. In een unieke tekenreeks die zijn opgemaakt als een domein in de twee tekstvakken plaatsen en klik op het vinkje
6. Klik op het tabblad **configureren** van deze toepassing
7. Kopieer de **Client-ID**, deze wordt gebruikt in uw app logica
8. Klik in de sectie **toetsen** klikt u op **duur selecteren** en kiest u 1 jaar of 2 jaar
9. Klik op de knop **Opslaan** onder aan het scherm (mogelijk moet wacht een paar seconden)
10. Nu moet u de toets kopiëren in het vak. Dit wordt ook gebruikt in uw app logica

#### <a name="create-the-application-identity-using-powershell"></a>De identiteit van de groep via PowerShell maken
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Zorg ervoor dat u de **Tenant-ID**, de **Toepassings-ID** en het wachtwoord dat u gebruikt kopiëren

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>Deel 2: Uw Web-App voor een identiteit van de app AAD beveiligen

Als uw Web-app al wordt geïmplementeerd kunt u deze alleen inschakelen in de portal. Anders kunt u autorisatie inschakelen voor een deel van de implementatie van Azure Resource manager.

#### <a name="enable-authorization-in-the-azure-portal"></a>Autorisatie in de Portal van Azure inschakelen

1. Ga naar de Web-app en klik op de **Instellingen** in de opdrachtenbalk.
2. Klik op **Autorisatie/verificatie**.
3. **Inschakelen.**

Een toepassing is op dit moment automatisch voor u gemaakt. U moet van deze toepassing cliënt-ID voor deel 3, zodat u moet:

1. Ga naar [Active directory in de klassieke Azure-portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) en selecteer de map.
2. Zoeken naar de app in het zoekvak
3. Klik erop in de lijst
4. Klik op het tabblad **configureren**
5. U ziet nu de **Client-ID**

#### <a name="deploying-your-web-app-using-an-arm-template"></a>Uw Web-App met een sjabloon van ARM implementeren

Eerst moet u een toepassing voor uw Web-app maken. Dit moet verschillen van de toepassing die wordt gebruikt voor uw app logica. Begin met het volgen van de bovenstaande stappen in deel 1, maar nu voor het gebruik van de **startpagina van** en **IdentifierUris** de werkelijke https://-**URL** van uw Web-app.

>[AZURE.NOTE]Wanneer u de toepassing voor uw Web-app maakt, moet u de [Azure klassieke portal methode](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory), zoals de PowerShell-commandlet niet van de vereiste machtigingen instellen voor gebruikers aan te melden bij een website is ingesteld.

Eenmaal hebt u de klant-ID en tenant-ID, neem de volgende handelingen uit als een resource sub van de Web-app in de implementatiesjabloon:

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Als u wilt uitvoeren van een distributie automatisch die een lege WebApp en logica app samen met AAD implementeert, klikt u op de volgende knop:

[![Implementeren naar Azure](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Zie [logica app oproepen in aangepaste API die worden gehost op App-Service en beschermd door AAD](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json)voor de sjabloon voltooid.


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>Deel 3: De sectie autorisatie in de app logica vullen

In de sectie **autorisatie** van de **HTTP** -actie:`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Element | Beschrijving |
|---------|-------------|
| type | Type verificatie. Voor ActiveDirectoryOAuth verificatie is de waarde ActiveDirectoryOAuth. |
| tenant | De tenant-id die wordt gebruikt om aan te geven van de AD-tenant. |
| publiek | Vereist. De resource die u verbinding met maakt. |
| clientID | De client-id voor de Azure AD-toepassing. |
| geheim | Vereist. Geheim van de client die het token aanvraagt. |

De bovenstaande sjabloon al volgt instellen, maar als u de app logica rechtstreeks maakt, moet u de volledige autorisatie sectie opnemen.

## <a name="securing-your-api-in-code"></a>De API in code beveiligen

### <a name="certificate-auth"></a>Certificaat auth

U kunt clientcertificaten gebruiken voor het valideren van de verzoeken voor oproepen naar uw Web-app. Zie [Hoe naar configureren TLS onderlinge verificatie voor Web App](../app-service-web/app-service-web-configure-tls-mutual-auth.md) voor het instellen van uw code.

In de sectie *autorisatie* u moet bevatten: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Element | Beschrijving |
|---------|-------------|
| type | Vereist. Type verificatie. De waarde moet voor SSL-clientcertificaten, ClientCertificate. |
| PFX | Vereist. Base64-codering inhoud van het PFX-bestand. |
| wachtwoord | Vereist. Wachtwoord voor toegang tot het PFX-bestand. |

### <a name="basic-auth"></a>Eenvoudige auth

U kunt basisverificatie (bijvoorbeeld gebruikersnaam en wachtwoord) gebruiken voor het valideren van de verzoeken voor oproepen. Eenvoudige auth is een algemene patroon en u dit kunt doen in een taal die u in uw app maakt.

In de sectie *autorisatie* u dient: `{"type": "basic","username": "test","password": "test"}`.

| Element | Beschrijving |
|---------|-------------|
| type | Vereist. Type verificatie. De waarde moet voor basisverificatie Basic. |
| gebruikersnaam | Vereist. UserName om te verifiëren. |
| wachtwoord | Vereist. Wachtwoord om te verifiëren. |

### <a name="handle-aad-auth-in-code"></a>Greep AAD auth in code

De verificatie van de Azure Active Directory die u in staat in de Portal stellen wordt standaard niet fijnmazige autorisatie. Zo wordt uw API aan een specifieke gebruiker of de app, maar alleen om een bepaald tenant niet worden vergrendeld.

Als u wilt de API beperken tot alleen de logica app, bijvoorbeeld in code, kunt u de kop die bevat de JWT en Controleer wie de beller, weigert verzoeken die niet overeenkomen met kunt ophalen.

Geavanceerde mogelijkheden, als u wilt deze helemaal in uw eigen code implementeren, en niet gebruikmaken van de Portal-functie, kunt u lezen in dit artikel: [Active Directory gebruiken voor verificatie in Azure App-Service](../app-service-web/web-sites-authentication-authorization.md).

Nog steeds moet u de bovenstaande stappen voor het maken van een toepassings-id voor de logica-app en dat de API aan te roepen gebruiken.
