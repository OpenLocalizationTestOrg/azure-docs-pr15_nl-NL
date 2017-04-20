<properties 
   pageTitle="Active Directory-verificatie en resourcemanager | Microsoft Azure"
   description="Een ontwikkelaar van de handleiding voor verificatie met het Azure resourcemanager API en de Active Directory voor een app integreren met andere Azure abonnementen."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="dugill;tomfitz" />

# <a name="how-to-use-azure-active-directory-and-resource-manager-to-manage-a-customers-resources"></a>Het gebruik van Azure Active Directory en resourcemanager bronnen van een klant beheren

## <a name="introduction"></a>Inleiding

Als u een softwareontwikkelaar die vereist zijn voor het maken van een app die worden beheerd van klant Azure resources, dit onderwerp ziet u hoe u met de Azure resourcemanager API's geverifieerd en toegang krijgen tot bronnen in de overige abonnementen. 

Uw app hebben toegang tot de API resourcemanager op aantal manieren oplossen:

1. **Gebruiker + app access**: voor apps die toegang resources namens een aangemelde gebruiker tot. Deze methode werkt voor apps, zoals WebApps en opdrachtregel hulpmiddelen voor die betrekking op alleen "interactieve management" Azure resources hebben.
1. **App - lezentoegang**: voor apps die daemon services and geplande taken uit te voeren. De identiteit van de app is directe toegang geven tot de resources. Deze methode werkt voor apps die op de lange termijn "offline toegang' tot Azure nodig.

In dit onderwerp biedt stapsgewijze instructies voor het maken van een app die zowel deze autorisatie methoden in dienst. Het uitvoeren van elke stap met REST API of C# worden weergegeven. De volledige MVC ASP.NET-aanvraag is beschikbaar op [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

De code voor dit onderwerp wordt uitgevoerd als een web-app die u bij [http://vipswapper.azurewebsites.net/cloudsense proberen kunt](http://vipswapper.azurewebsites.net/cloudsense). 

## <a name="what-the-web-app-does"></a>Werking van de web-app

De web-app:

1. Tekens in een Azure-gebruiker.
2. De gebruiker de web-app om toegang te verlenen aan resourcemanager wordt gevraagd.
3. Krijgt de gebruiker + app-toegangstoken voor toegang tot resourcemanager.
4. Token (uit stap 3) wordt gebruikt om te bellen resourcemanager en van de app service principal toewijzen aan een rol bij het abonnement, waarbij de lange termijn toegang krijgt voor een app aan het abonnement.
5. App-lezentoegang token krijgt.
6. Token (uit stap 5) wordt gebruikt voor het beheren van resources in het abonnement via resourcemanager.

Hier ziet u de stroom end-to-end van de webtoepassing.

![Resourcemanager verificatie-mailstroom](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Als een gebruiker, kunt u de abonnements-id opgeven voor het abonnement dat u wilt gebruiken:

![abonnements-id opgeven](./media/resource-manager-api-authentication/sample-ux-1.png)

Selecteer het account wilt gebruiken voor logboekregistratie in.

![Selecteer account](./media/resource-manager-api-authentication/sample-ux-2.png)

Uw referenties opgeven.

![referenties opgeven](./media/resource-manager-api-authentication/sample-ux-3.png)

De app toegang verlenen tot uw Azure abonnementen:
 
![Toegang verlenen](./media/resource-manager-api-authentication/sample-ux-4.png)
 
Uw verbonden abonnementen beheren:

![Verbinding maken met abonnement](./media/resource-manager-api-authentication/sample-ux-7.png)


## <a name="register-application"></a>Register-toepassing

Voordat u kleurcodering begint, registreren uw web-app met Azure Active Directory (AD). De app-registratie Hiermee maakt u een centrale identiteit voor de app in Azure AD. Basisinformatie over het gebruik van de toepassing zoals OAuth cliënt-ID, antwoord URL's en referenties die in uw toepassing wordt gebruikt om te verifiëren en toegang tot Azure resourcemanager-API's bevat. De app-registratie records ook de verschillende gedelegeerde machtigingen die voor uw toepassing nodig bij het openen van Microsoft APIs namens de gebruiker. 

Omdat uw app toegang krijgt andere abonnementen tot, moet u deze als een toepassing voor multi-tenant configureren. Om te worden gevalideerd, vindt u een domein dat is gekoppeld aan uw Active Directory. Als u wilt zien van de domeinen die is gekoppeld aan uw Active Directory, meld u aan bij de [klassieke portal](https://manage.windowsazure.com). Selecteer uw Active Directory en selecteer vervolgens **domeinen**.

Het volgende voorbeeld ziet u hoe u de app registreert met behulp van Azure PowerShell. U moet de nieuwste versie (augustus 2016) van Azure PowerShell voor deze opdracht om te werken. 

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
    
Meld u als de AD-toepassing, moet u de toepassing-id en wachtwoord. Als u wilt de toepassings-id die wordt geretourneerd uit de vorige opdracht wordt weergegeven, gebruiken:

    $app.ApplicationId

Het volgende voorbeeld ziet u hoe u de app registreert met behulp van Azure CLI. 

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

De resultaten zijn de toepassings-id, die u nodig hebt bij het verifiëren als de toepassing.

### <a name="optional-configuration---certificate-credential"></a>Optionele configuratie - certificaat referentie

Azure AD ondersteunt ook certificaat referenties voor toepassingen: u een zelfondertekend certificaat maken, blijven de persoonlijke sleutel en de openbare sleutel toevoegen aan de registratie van uw Azure AD-toepassingen. Voor verificatie, uw toepassing een kleine nettolading verzendt naar Azure AD ondertekend met uw persoonlijke sleutel en Azure AD is gevalideerd met de handtekening met de openbare sleutel die u hebt geregistreerd.

Zie [Azure PowerShell gebruiken om een service hoofdsom voor toegang tot resources te maken](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) of [Gebruik Azure CLI een service hoofdsom voor toegang tot resources maken](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate)voor informatie over het maken van een AD-app met een certificaat.

## <a name="get-tenant-id-from-subscription-id"></a>Id van de tenant bij abonnements-id ophalen

Als u een token die kan worden gebruikt om te bellen resourcemanager, moet uw toepassing kent van de tenant-ID van de Azure AD-tenant waarop het Azure abonnement. Waarschijnlijk uw gebruikers hun id abonnement kent, maar mogelijk niet weten ze hun tenant id's voor Active Directory. Als u de gebruikersnaam van de tenant, vraag de gebruiker voor de abonnements-id. Bieden die abonnements-id bij het verzenden van een verzoek om over het abonnement:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Het verzoek mislukt omdat de gebruiker niet nog heeft aangemeld, maar u de tenant-id uit het antwoord ophalen kunt. In deze uitzondering, moet u de tenant-id ophalen uit de waarde van de koptekst antwoord voor **WWW-verificatie**. U ziet deze implementatie in de methode [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) .

## <a name="get-user--app-access-token"></a>Gebruiker + app-toegangstoken ophalen

Uw toepassing wordt de gebruiker naar Azure AD met een OAuth 2.0 Autoriseer aanvragen - voor de verificatie van de referenties van de gebruiker en een autorisatiecode terug te gaan. Uw toepassing gebruikt de autorisatiecode toegang kunt krijgen een token voor de Resource-Manager. De methode [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) maakt het verzoek autorisatie.

In dit onderwerp ziet u de REST API-verzoeken om de gebruiker te verifiëren. U kunt ook helper bibliotheken gebruiken om uit te voeren verificatie in uw code. Zie voor meer informatie over deze bibliotheken, [Azure Active Directory verificatie bibliotheken](./active-directory/active-directory-authentication-libraries.md). Zie voor hulp bij het integreren van identiteitsbeheer in een toepassing, [handleiding voor ontwikkelaars van de Azure Active Directory](./active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Auth aanvraag (OAuth 2.0)

Een Open-ID verbinding maken met/OAuth2.0 Autoriseer aanvraag verstrekken naar het eindpunt Azure AD autoriseren:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in het onderwerp van het [verzoek om een vergunning-code](./active-directory/active-directory-protocols-oauth-code.md#request-an-authorization-code) .

Het volgende voorbeeld ziet u hoe OAuth2.0 autorisatie aanvragen:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD wordt geverifieerd door de gebruiker en, indien nodig, wordt gevraagd om gebruikers te machtigen bij de app. Dit geeft als resultaat de autorisatiecode naar de URL van het beantwoorden van uw toepassing. Afhankelijk van de gevraagde response_mode, Azure AD ofwel stuurt terug de gegevens in queryreeks of als post-gegevens.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Auth aanvraag (Open-ID verbinding maken)

Als u wilt niet alleen de toegang tot Azure resourcemanager namens de gebruiker, maar kan de gebruiker te melden bij uw-toepassing via hun Azure AD-account, een Open-ID verbinding Autoriseer aanvraag verstrekken. Met de Open-ID verbinding kunt maken ontvangt uw toepassing ook een id_token van Azure AD die uw app gebruiken kunt in de gebruiker zich aan te melden.

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in het onderwerp [Verzend het verzoek aanmelden](./active-directory/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) .

Een voorbeeld openen-ID verbinding maken met verzoek om luidt als volgt:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD wordt geverifieerd door de gebruiker en, indien nodig, wordt gevraagd om gebruikers te machtigen bij de app. Dit geeft als resultaat de autorisatiecode naar de URL van het beantwoorden van uw toepassing. Afhankelijk van de gevraagde response_mode, Azure AD ofwel stuurt terug de gegevens in queryreeks of als post-gegevens.

Een voorbeeld antwoord openen-ID-verbinding is:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Token aanvraag (OAuth2.0 Code verlenen Flow)

Nu uw toepassing heeft de autorisatiecode ontvangen van Azure AD, is het tijd hebt om de toegang token voor Azure resourcemanager.  Posten op een OAuth2.0 Code verlenen Token aanvragen naar het Azure AD Token eindpunt: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in het onderwerp [de autorisatiecode gebruiken](./active-directory/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) .

Het volgende voorbeeld ziet een verzoek om code verlenen token met wachtwoord referentie:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Tijdens het werken met certificaat referenties, maak een JSON Web Token (JWT) en u aanmelden (RSA SHA256) met de persoonlijke sleutel van van uw toepassing certificaat referentie. De claimtypen voor het token worden weergegeven in [JWT token claims](./active-directory/active-directory-protocols-oauth-code.md#jwt-token-claims). Voor een verwijzing naar de [code Active Directory Auth bibliotheek (.NET)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) aan te melden Client bevestiging JWT tokens te bekijken.

Zie de [specificatie openen ID-verbinding](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) voor meer informatie over clientverificatie. 

Het volgende voorbeeld ziet een verzoek om code verlenen token met certificaat referentie:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
    
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012
    
    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Een voorbeeld-antwoord voor code verlenen token: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Code verlenen token antwoord verwerken

Een positief token antwoord bevat (gebruiker + app) toegangstoken voor Azure resourcemanager. Uw toepassing wordt dit toegangstoken gebruikt voor toegang tot resourcemanager namens de gebruiker. De levensduur van access tokens uitgegeven door Azure AD is één uur. Het is niet waarschijnlijk dat uw webtoepassing vernieuwen moet (gebruiker + app) toegangstoken. Als deze verlengen van het toegangstoken moet, gebruikt u de vernieuwen token die uw toepassing in de token reactie krijgt. Posten op een OAuth2.0 Token aanvragen naar het Azure AD Token eindpunt: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

De parameters voor gebruik met de aanvraag vernieuwen worden beschreven in [het toegangstoken vernieuwen](./active-directory/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

Het volgende voorbeeld ziet het gebruik van het vernieuwen token:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Hoewel vernieuwen tokens kunnen worden gebruikt om de nieuwe access tokens voor Azure Resource Manager verkrijgen, zijn zij niet geschikt voor offline toegang tot uw toepassing. De levensduur van de tokens vernieuwen is beperkt en vernieuwen tokens zijn afhankelijk van de gebruiker. Als de gebruiker de organisatie verlaat, verliest de toepassing het token vernieuwen met access. Deze methode is niet geschikt voor toepassingen die worden gebruikt door teams hun Azure bronnen beheren.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Als de gebruiker toegang tot abonnement kunt geven controleren

De toepassing nu heeft een token Azure resourcemanager namens de gebruiker toegang. De volgende stap is het abonnement van uw app verbinden. Nadat u verbinding maakt, uw app deze abonnementen kunt beheren zelfs wanneer de gebruiker niet aanwezig (langdurige offlinetoegang). 

Voor elk abonnement om verbinding te, belt u de [resourcemanager lijstmachtigingen, sitemachtigingen](https://msdn.microsoft.com/library/azure/dn906889.aspx) API om te bepalen of de gebruiker toegangsrechten voor management voor het abonnement.

De methode [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) van de app van de steekproef ASP.NET MVC implementeert dit gesprek.

Het volgende voorbeeld ziet over het aanvragen van een gebruiker machtigingen voor een abonnement. 83cfe939-2402-4581-b761-4f59b0a041e4 is de id van het abonnement.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Een voorbeeld van het antwoord op gebruikersmachtigingen ophalen op abonnement luidt als volgt:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

De machtigingen API retourneert meerdere machtigingen. Elke machtiging bestaat uit de toegestane acties (acties) en zijn niet toegestaan acties (notactions). Als u een actie aanwezig is in de lijst toegestane acties van een machtiging en niet aanwezig is in de lijst notactions met deze machtiging, wordt de gebruiker mag handeling uit te voeren. **Microsoft.Authorization/RoleAssignments/Write** is de actie dat die toegang rights management verleent. Uw toepassing moet parseren van het resultaat van de machtigingen om een overeenkomst regex op deze tekenreeks actie in de acties en notactions van elke machtiging te vinden.

## <a name="get-app-only-access-token"></a>App-lezentoegang token ophalen

U weet nu als de gebruiker toegang tot het Azure abonnement kunt geven. De volgende stappen zijn:

1. De juiste RBAC rol toewijzen aan de identiteit van de toepassing op het abonnement.
2. De access-toewijzing valideren query's uitvoeren voor de machtigingen van de toepassing op het abonnement of door te resourcemanager token gebruikt alleen-app te openen.
1. De verbinding opnemen in de gegevensstructuur toepassingen "verbonden abonnementen" - persistent maken van de id van het abonnement.

Laten we even naar dichter bij de eerste stap. Als u wilt de juiste RBAC rol toewijzen aan de identiteit van de toepassing, moet u het volgende bepalen:

- De object-id van de identiteit van de toepassing in van de gebruiker Azure Active Directory
- De id van de RBAC rol die uw toepassing is op het abonnement dat is vereist

Wanneer uw aanvraag wordt geverifieerd door een gebruiker van een Azure AD, wordt een service principal object voor uw toepassing in die Azure AD gemaakt. Azure kunt RBAC rollen wilt toewijzen aan service principes directe toegang tot de bijbehorende toepassingen op Azure resources verleent. Deze actie is precies wat we wilt doen. Query de API Azure AD Graph om te bepalen de id van de service hoofdsom van uw toepassing in de ondertekend gebruiker bevindt zich Azure AD.

U alleen een toegangstoken voor Azure resourcemanager - moet u een nieuwe toegangstoken de API Azure AD grafiek aan te roepen. Elke toepassing in Azure AD daartoe is gemachtigd om query's in een eigen service principal object, zodat een alleen-app-toegangstoken voldoende is.

<a id="app-azure-ad-graph">
### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>App-lezentoegang token krijgen voor Azure AD Graph-API

Om te verifiëren van uw app en een token tot Azure AD Graph API, moet u het verzoek voor een token van een Client referentie verlenen OAuth2.0 stroom naar Azure AD token eindpunt (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/Token**).

De methode [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) van de toepassing van de steekproef ASP.net MVC krijgt een app-lezentoegang token voor Graph API met behulp van de bibliotheek voor verificatie van Active Directory voor .NET.

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in het [verzoek om een Access-Token](./active-directory/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) -onderwerp.

Een voorbeeld-verzoek voor de client referentie verlenen token: 

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Een voorbeeld-antwoord voor client referentie verlenen token: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Object-id van toepassing service hoofdsom gebruiker Azure AD ophalen

Nu de app alleen-lezen toegangstoken gebruiken om de [principes van Azure AD Graph-Service](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API om te bepalen de Object-Id van de toepassing service principal in de adreslijst.

De methode [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) van de toepassing van de steekproef ASP.net MVC implementeert dit gesprek.

Het volgende voorbeeld ziet over het aanvragen van een toepassing service principal. a0448380-c346-4f9f-b897-c18733de9394 is de client-id van de toepassing.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Het volgende voorbeeld ziet u een antwoord op de aanvraag voor een toepassing service principal 

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC rol id ophalen

Als u wilt de juiste RBAC rol toewijzen aan uw service principal, moet u de id van de rol van Azure RBAC bepalen.

De juiste RBAC rol voor uw toepassing:

- Als uw toepassing alleen het abonnement, bewaakt zonder wijzigingen, moet alleen reader machtigingen voor het abonnement. De rol van de **lezer** toewijzen.
- Als uw toepassing Azure het abonnement, entiteiten maken/wijzigen/verwijderen beheert, moet een van de machtigingen Inzender.
  - Als u wilt beheren op een bepaald type resource, toewijzen de rollen resource / regiospecifieke Inzender (virtuele Machine Inzender, virtuele netwerk Inzender, opslag Account Inzender, enz.)
  - Als u wilt beheren op elk gewenst resourcetype, toewijzen aan de rol **Inzender** .

De roltoewijzing voor uw toepassing is zichtbaar voor gebruikers, dus selecteer de kleinste vereist bevoegdheid.

Bel de [roldefinitie van de resourcemanager API](https://msdn.microsoft.com/library/azure/dn906879.aspx) om een lijst met alle rollen van Azure RBAC en zoeken en vervolgens het resultaat naar de gewenste roldefinitie zoeken op naam doorlopen.

De methode [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) van de app van de steekproef ASP.net MVC implementeert dit gesprek.

Het volgende voorbeeld van de aanvraag ziet u hoe u Azure RBAC rol-id. 09cbd307-aa71-4aca-b346-5f253e6e3ebb is de id van het abonnement.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Het antwoord heeft de volgende indeling: 

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

U hoeft niet te bellen van deze API op doorlopend. Nadat u de bekende GUID van de roldefinitie hebt vastgesteld, kunt u de rol definitie-id als kunt maken:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Hier volgen de bekende GUID's van veelgebruikte ingebouwde rollen:

| Rol | GUID |
| ----- | ------ |
| Lezer | acdd72a7-3385-48ef-bd42-f606fba81ae7
| Inzender | b24988ac-6180-42a0-ab88-20f7382dd24c
| VM Inzender | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| Virtual Network Inzender | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Opslag Account Inzender | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Website Inzender | de139f84-1756-47ae-9be6-808fbbe84772
| Web abonnement Inzender | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| SQL Server Inzender | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| SQL DB Inzender | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec

### <a name="assign-rbac-role-to-application"></a>RBAC rol toewijzen aan toepassing

Hebt u alles wat die u de juiste RBAC rol toewijzen aan uw service principal moet met behulp van de [Resource Manager roltoewijzing maken](https://msdn.microsoft.com/library/azure/dn906887.aspx) API.

De methode [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) van de app van de steekproef ASP.net MVC implementeert dit gesprek.

Een voorbeeld-aanvraag RBAC rol toewijzen aan toepassing: 

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

In de aanvraag, worden de volgende waarden gebruikt:

| GUID | Beschrijving |
| ------ | --------- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb | de id van het abonnement
| c3097b31-7309-4c59-b4e3-770f8406bad2 | de object-id van de service hoofdsom van de toepassing
| acdd72a7-3385-48ef-bd42-f606fba81ae7 | de id van de rol van de lezer
| 4f87261d-2816-465d-8311-70a27558df4c | een nieuwe guid gemaakt voor de nieuwe roltoewijzing

Het antwoord heeft de volgende indeling: 

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>App-lezentoegang token voor Azure Resource Manager verkrijgen

Valideren van deze app heeft het gewenste toegang tot het abonnement, het uitvoeren van een taak testen op het abonnement dat met een token app alleen-lezen.

Als u een app-lezentoegang token, volgt u de instructies uit sectie [app - lezentoegang token voor Azure AD Graph API ophalen](#app-azure-ad-graph), met een andere waarde voor de parameter resource: 

    https://management.core.windows.net/

De methode [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) van de toepassing van de steekproef ASP.NET MVC krijgt een app-lezentoegang token resourcemanager voor Azure Active Directory-verificatie Library met voor .net.

#### <a name="get-applications-permissions-on-subscription"></a>Machtigingen van toepassing op abonnement verkrijgen

Als u wilt controleren dat uw toepassing de gewenste toegang op een Azure-abonnement heeft, kan u ook de [Resourcemanager machtigingen](https://msdn.microsoft.com/library/azure/dn906889.aspx) API bellen. Deze methode is vergelijkbaar met hoe u bepaald of de gebruiker toegangsbeheer rechten voor het abonnement heeft. Schakel ditmaal belt u echter de machtigingen API met het kenmerk alleen-app toegangstoken die u in de vorige stap hebt ontvangen.

De methode [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) van de app van de steekproef ASP.NET MVC implementeert dit gesprek.

## <a name="manage-connected-subscriptions"></a>Verbonden abonnementen beheren

Wanneer de juiste RBAC rol is toegewezen aan van de toepassing service principal het abonnement, kunt uw toepassing blijven monitoring/beheren met behulp van app-lezentoegang tokens voor Azure resourcemanager.

Als de eigenaar van een abonnement wordt verwijderd van uw toepassing roltoewijzing via de klassieke portal of de opdrachtregel hulpmiddelen, is niet langer uw toepassing abonnement toegang tot. U moet in dat geval de gebruiker die de verbinding met het abonnement dat is plakken van buiten de toepassing een melding en geeft u een optie om de verbinding 'herstellen'. 'Herstellen' zou gewoon opnieuw maken voor de toewijzing van de rol die offline is verwijderd.

Net zoals u de gebruiker abonnementen verbinden met uw toepassing hebt ingeschakeld, moet u de gebruiker abonnementen te verbreken toestaan. Verbinding verbreken betekent dat de toewijzing van de rol van de toepassing service principal met het abonnement verwijderen uit een access management oogpunt. (Optioneel) een staat in de toepassing voor het abonnement mogelijk te worden verwijderd. Er kunnen alleen gebruikers met toegangsmachtiging voor sitebeheer op het abonnement het abonnement verbreken.

De [methode RevokeRoleFromServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) van de app van de steekproef ASP.net MVC implementeert dit gesprek.

Dat is het - gebruikers kunnen nu eenvoudig verbinding maken en beheren van hun Azure abonnementen met uw toepassing.

