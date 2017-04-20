<properties
   pageTitle="Azure Active Directory v2.0 verificatie bibliotheken | Microsoft Azure"
   description="Clientbibliotheken compatibele en server middleware-bibliotheken, en gerelateerde bibliotheek, bron en voorbeelden koppelingen, voor het eindpunt van de v2.0 Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0 verificatie-bibliotheken
Het eindpunt van de v2.0 Azure Active Directory (Azure AD) ondersteunt de standaard-OAuth 2.0 als OpenID verbinding 1.0-protocol. U kunt verschillende bibliotheken van Microsoft en andere organisaties gebruiken met het eindpunt v2.0.

Als u een toepassing die gebruikmaakt van het eindpunt v2.0 samenstelt, is raadzaam dat u bibliotheken die worden geschreven door protocol domein experts die volgen van een methodologie beveiliging ontwikkeling levenscyclus (SDL), zoals [het gevolgd door Microsoft]gebruikt[Microsoft-SDL]. Als u naar hand-code ondersteuning voor de protocollen, raden u SDL methodologie volgen en aandacht besteden aan de beveiligingsoverwegingen in de specificaties standaarden voor elk protocol.

## <a name="types-of-libraries"></a>Bibliotheektypen

Azure AD v2.0 werkt met twee soorten bibliotheken:

- **Client-bibliotheken**. Systeemeigen clients en servers kunt u de clientbibliotheken gebruiken om toegangstokens voor het bellen van een resource, zoals Microsoft Graph.
- **Server middleware-bibliotheken**. WebApps server middleware bibliotheken voor aanmelding gebruiken Web-API's gebruik server middleware bibliotheken voor het valideren van tokens die zijn verzonden door systeemeigen klanten of andere servers bevinden.

## <a name="library-support"></a>Ondersteuning voor de bibliotheek
Omdat u een standaarden-compatibele bibliotheek kiezen kunt wanneer u het eindpunt v2.0 gebruikt, is het belangrijk te weten waar u ondersteuning. Voor problemen en functieverzoeken verzenden in de bibliotheek code, contact op met de eigenaar van de bibliotheek. Neem contact op met Microsoft voor problemen en functieverzoeken verzenden in de service aan de clientzijde protocol-implementatie.

Bibliotheken afkomstig zijn in twee categorieën voor ondersteuning:

- **Microsoft ondersteunde**. Microsoft biedt correcties voor deze bibliotheken en SDL heeft gedaan innen op deze bibliotheken.
- **Compatibele**. Microsoft heeft deze bibliotheken getest in eenvoudige scenario's en bevestigd dat ze met het eindpunt v2.0 werken. Microsoft biedt geen oplossingen voor deze bibliotheken en een controle door een van deze bibliotheken niet heeft gedaan. Problemen bij en functieverzoeken verzenden moeten worden doorgestuurd naar van de bibliotheek open source project.

Zie de volgende secties in dit artikel voor een lijst met bibliotheken die met het eindpunt v2.0 werken.

## <a name="microsoft-supported-client-libraries"></a>Microsoft ondersteunde clientbibliotheken
| Platform| De bibliotheeknaam van de| Downloaden | Broncode | Voorbeeld |
| :-: | :-: | :-: | :-: | :-: |
| .NET, Windows Store, Xamarin | Verificatie van Microsoft-bibliotheek (MSAL) voor .NET | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [MSAL voor .NET (GitHub)][ClientLib-NET-Repo] | [Voorbeeld van Windows-bureaubladclient systeemeigen][ClientLib-NET-Sample] |
| Node.js | Microsoft Azure Active Directory Passport.js-invoegtoepassing | [Passport Azure AD (npm)][ClientLib-Node-Lib] | [Passport Azure AD (GitHub)][ClientLib-Node-Repo] | Binnenkort beschikbaar |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft ondersteunde server middleware-bibliotheken
| Platform| De bibliotheeknaam van de| Downloaden | Broncode | Voorbeeld |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID verbinding Middleware voor ASP.NET | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana Project (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Voorbeeld van de Web-app][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | OWIN OAuth vruchtdragende Middleware voor ASP.NET | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana Project (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Web API-voorbeeld][ServerLib-Net4-Owin-Oauth-Sample] |
| .NET core | OWIN OpenID Middleware voor .NET Core verbinden | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [ASP.NET-beveiliging (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Voorbeeld van de Web-app][ServerLib-NetCore-Owin-Oidc-Sample] |
| .NET core | OWIN OAuth vruchtdragende Middleware voor .NET Core | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [ASP.NET-beveiliging (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | Binnenkort beschikbaar |
| Node.js | Microsoft Azure Active Directory Passport.js invoegtoepassing | [Passport Azure AD (npm)][ServerLib-Node-Lib] | [Passport Azure AD (GitHub)][ServerLib-Node-Repo] | [Voorbeeld van de Web-app][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Compatibele clientbibliotheken
| Platform| De bibliotheeknaam van de | Geteste versie | Broncode | Voorbeeld |
| :-: | :-: | :-: | :-: | :-: |
| Android-apparaat | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Systeemeigen app-voorbeeld](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Systeemeigen app-voorbeeld](active-directory-v2-devquickstarts-ios.md)|
| JavaScript | [Hello.js](https://adodson.com/hello.js/) | 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | Binnenkort beschikbaar |
| Python-kolf | [Kolf-OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [Kolf-OAuthlib](https://github.com/lepture/flask-oauthlib) | Binnenkort beschikbaar |
| Ruby | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>omniauth-oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | Binnenkort beschikbaar |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Compatibele server middleware-bibliotheken
Binnenkort beschikbaar

## <a name="related-content"></a>Verwante onderwerpen
Zie [Azure AD-app-model v2.0 overzicht]voor meer informatie over het eindpunt van de v2.0 Azure AD,[AAD-App-Model-V2-Overview].

Als u wilt help ons te verfijnen en de inhoud van vorm, gebruik de functie Disqus opmerkingen aan het einde van dit artikel om feedback te geven.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
