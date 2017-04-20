<properties
   pageTitle="Voorbeelden van de Azure Active Directory-Code | Microsoft Azure"
   description="Een index van Azure Active Directory-code steekproeven, geordend op scenario."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Voorbeelden van de Azure Active Directory-Code

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Verificatie en autorisatie toevoegt aan uw webtoepassingen en web API's kunt u Microsoft Azure Active Directory (Azure AD). In deze sectie een koppeling naar codevoorbeelden waarin u kunt zien hoe het werkt en codefragmenten die u in uw toepassingen gebruiken kunt. Klik op de pagina van de steekproef code vindt u gedetailleerde gelezen-mij onderwerpen die met vereisten, installatie en configuratie helpen. En de code is toegelicht aan de hand waarvan u de kritieke secties begrijpen.

Als u wilt weten over het eenvoudige scenario voor elk Voorbeeldtype, raadpleegt u verificatie scenario's voor Azure AD.

Bijdragen aan onze voorbeelden op GitHub: [Microsoft Azure Active Directory voorbeelden en documentatie](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Webbrowser aan webtoepassing.

Deze voorbeelden wordt een webtoepassing die wordt u omgeleid zodat de browser van de gebruiker ze aanmelden bij Azure AD schrijven.



| Taal/Platform | Voorbeeld | Beschrijving
| ----------------- | ------ | -----------
| C# / .NET | [WebApp-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | OpenID Connect (ASP.Net OpenID verbinding OWIN middleware) gebruikt om gebruikers uit een Azure AD-tenant te verifiëren.
| C# / .NET | [WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Een meerdere tenant .NET MVC webtoepassing die OpenID verbinding (ASP.Net OpenID verbinding OWIN middleware) gebruikt om gebruikers te verifiëren uit meerdere Azure AD-tenants.
| C# / .NET | [WebApp-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | WS-Federatie (ASP.Net WS-Federation OWIN middleware) gebruiken om gebruikers te verifiëren vanuit een Azure AD-tenant.






## <a name="single-page-application-spa"></a>Eén pagina toepassing (beveiligd-WACHTWOORDVERIFICATIE)

In dit voorbeeld ziet hoe u een toepassing één pagina is beveiligd met Azure AD schrijven.  

| Taal/Platform | Voorbeeld | Beschrijving
| ----------------- | ------ | -----------
| JavaScript, C# / .NET | [SinglePageApp-DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | Gebruik ADAL voor JavaScript en Azure AD om een enkele pagina AngularJS gebaseerde app geïmplementeerd met een ASP.NET web API back-end secure.


## <a name="native-application-to-web-api"></a>De toepassing waarmee Web API

Deze codevoorbeelden wordt het maken van systeemeigen clienttoepassingen die web API's die zijn beveiligd met Azure AD bellen. Ze [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) en [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)gebruiken.

| Taal/Platform | Voorbeeld | Beschrijving
| ----------------- | ------ | -----------
| JavaScript | [NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | Gebruik de ADAL-invoegtoepassing voor Apache Cordova maken van een app Apache Cordova die een web API-oproepen en Azure AD voor verificatie wordt gebruikt.
| C# / .NET | [NativeClient-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | Een .NET WPF-toepassing die een web API-die oproepen is beveiligd met behulp van Azure AD.
| C# / .NET | [NativeClient-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Een Windows Store-toepassing die een web API-die oproepen is beveiligd met Azure AD.
| C# / .NET | [NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | Een Windows Store-toepassing een meerdere tenant web API die is beveiligd met Azure AD bellen.
| C# / .NET | [WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Een systeemeigen clienttoepassing die een web API, waarmee een token uitvoeren namens de oorspronkelijke gebruiker krijgt en gebruikt vervolgens het token om te bellen van een ander web API-oproepen.
| C# / .NET | [NativeClient-WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Een Windows Store-servicetoepassing voor Windows Phone 8.1 die een web API-die oproepen is beveiligd met Azure AD.
| ObjC | [NativeClient-iOS](https://github.com/Azure-Samples/active-directory-ios) | Een iOS-toepassing die een web API-die oproepen is Azure AD voor verificatie vereist.
| C# / .NET | [WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Een native clienttoepassing die logica voor het verwerken van een token JWT in een web API, in plaats van OWIN middleware bevat.
| C# / Xamarin | [NativeClient-Xamarin-Android](https://github.com/Azure-Samples/active-directory-xamarin-android) | Een Xamarin binding naar de systeemeigen Azure AD verificatie bibliotheek (ADAL) voor de Android-bibliotheek.
| C# / Xamarin | [NativeClient-Xamarin-iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Een Xamarin binding naar de systeemeigen Azure AD verificatie bibliotheek (ADAL) voor iOS.
| C# / Xamarin | [NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Een project Xamarin die is bedoeld voor vijf platforms en een web API-oproepen die is beveiligd met Azure AD.
| C# / .NET | [NativeClient Headless DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | Een systeemeigen-toepassing waarin niet-interactieve verificatie wordt uitgevoerd en een web API-oproepen die is beveiligd met Azure AD.



## <a name="web-application-to-web-api"></a>Webtoepassing Web API

Voorbeelden van deze code [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) weergeven hoe gebruiken om te maken van webtoepassingen die bellen web API's die zijn beveiligd met Azure AD.

| Taal/Platform | Voorbeeld | Beschrijving
| ----------------- | ------ | -----------
| C# / .NET | [WebApp-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Een web API met ondertekend in machtigingen van de gebruiker belt.
|  C# / .NET | [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Een web API met machtigingen van de toepassing bellen.
| C# / .NET | [WebApp-WebAPI-OAuth2-UserIdentity-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | Toevoegen aan een bestaande webtoepassing autorisatie met [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) zodat deze een web API kunt bellen.
| JavaScript | [WebAPI-Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Stel een REST API-service die geïntegreerd met Azure AD voor de API-beveiliging. Een server Node.js met een Web-API bevat.
| C# / .NET | [WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Een meerdere tenant MVC webtoepassing die OpenID verbinding (ASP.Net OpenID verbinding OWIN middleware) gebruikt om gebruikers te verifiëren uit een Azure AD-tenant. Een autorisatiecode wordt de grafiek-API roepen.

## <a name="server-or-daemon-application-to-web-api"></a>Server of demontoepassing naar Web API

Deze codevoorbeelden wordt het maken van een toepassing daemon of server waarvoor u resources uit een web API met behulp van [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) en [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Taal/Platform | Voorbeeld | Beschrijving
| ----------------- | ------ | -----------
| C# / .NET | [Daemon-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | Een consoletoepassing voor de roept een web API. De client-referentie is een wachtwoord.
| C# / .NET | [Daemon-CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | Een consoletoepassing die een web API-oproepen. De client-referentie is een certificaat.


## <a name="calling-azure-ad-graph-api"></a>Azure AD Graph API bellen

Een voorbeeld van deze code hoe toepassingen maken die de API Azure AD Graph directorygegevens lezen en schrijven als u wilt bellen.

| Taal/Platform | Voorbeeld | Beschrijving
| ----------------- | ------ | -----------
| Java | [Java-GraphAPI-WebApp](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Een webtoepassing die de grafiek-API voor toegang tot gegevens van Azure AD-map.
| PHP | [WebApp-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Een webtoepassing die de grafiek-API voor toegang tot gegevens van Azure AD-map.
| C# / .NET | [WebApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Een webtoepassing die de grafiek-API voor toegang tot gegevens van Azure AD-map.
| C# / .NET | [ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | Deze app console ziet u veelvoorkomende lezen en schrijven oproepen tot de API Graph en ziet u hoe u uitvoeren van de gebruiker licentie is toegewezen en bijwerken van een gebruiker miniatuurfoto en koppelingen.
| C# / .NET | [ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | Een consoletoepassing die wordt gebruikt de differentiële query in de grafiek-API om periodieke wijzigingen in gebruikersobjecten in een Azure AD-tenant.
| C# / .NET | [WebApp-GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | Een toepassing MVC gebruikt Graph API-query's te genereren van een eenvoudige bedrijf organigram weergegeven.
| PHP | [WebApp-GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | Een PHP-toepassing die de grafiek-API om te registreren extensie en klik vervolgens lezen, bijwerken en verwijderen van waarden in het kenmerk extensie belt.


## <a name="authorization"></a>Autorisatie

Deze codevoorbeelden wordt het gebruik van Azure AD voor autorisatie.

| Taal/Platform | Voorbeeld | Beschrijving
| ----------------- | ------ | -----------
| C# / .NET | [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Rolgebaseerd toegangsbeheer (RBAC) op claims van Azure Active Directory-groep gebruiken in een toepassing die is geïntegreerd met Azure AD uitvoeren.
| C# / .NET | [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Rolgebaseerd toegangsbeheer (RBAC) op rollen van Azure Active Directory-toepassing gebruiken in een toepassing die is geïntegreerd met Azure AD uitvoeren.


## <a name="legacy-walkthroughs"></a>Oudere Walkthroughs

Deze walkthroughs enigszins oudere technologie wilt gebruiken, maar nog steeds mogelijk interessant zijn.

| Taal/Platform | Voorbeeld | Beschrijving
| ----------------- | ------ | -----------
| C# / .NET | [Rol- en ACL gebaseerde autorisatie in een Microsoft Azure AD-toepassing](http://go.microsoft.com/fwlink/?LinkId=331694) | Rollen gebaseerde verificatie (RBAC) en ACL gebaseerde verificatie in een toepassing die is geïntegreerd met Azure AD uitvoeren.
| C# / .NET |  [AAL - Windows Store-app voor de REST service - verificatie](http://go.microsoft.com/fwlink/?LinkId=330605) |  Gebruik [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) (voorheen AAL) voor Windows Store Beta gebruiker verificatie mogelijkheden toevoegen aan een Windows Store-app.
| C# / .NET | [ADAL - systeemeigen App REST service - verificatie met AAD via een Browser-dialoogvenster](http://go.microsoft.com/fwlink/?LinkId=259814) |  Gebruik [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) gebruiker verificatie mogelijkheden toevoegen aan een WPF-client.
| C# / .NET | [ADAL - systeemeigen App REST service - verificatie met ACS via een Browser-dialoogvenster](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  Gebruik [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) en [Access Control Service 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) gebruiker verificatie mogelijkheden toevoegen aan een WPF-client.
| C# / .NET | [ADAL - Server naar Server-verificatie](http://go.microsoft.com/fwlink/?LinkId=259816) | Gebruik [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md) voor het beveiligen van serviceaanvragen van een server kant-proces op een MVC4 Web API REST-service.
| C# / .NET | [Eenmalige aanmelding toevoegen aan uw webtoepassing met Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Een toepassing .NET om uit te voeren web eenmalige aanmelding ten opzichte van het telefoonboek van uw Azure AD-enterprise configureren.
| C# / .NET | [Meerdere Tenant webtoepassingen met Azure AD ontwikkelen](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Met Azure AD kunt toevoegen aan de eenmalige aanmelding en een directory access mogelijkheden van één .NET-toepassing voor gebruik door meerdere organisaties.
JAVA | [Java-steekproef-App voor Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkId=263969) | Gebruik de API Graph naar access directory-gegevens van Azure AD.
PHP | [PHP steekproef-App voor Azure AD Graph API](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Gebruik de API Graph naar access directory-gegevens van Azure AD.
| C# / .NET | [Voorbeeld-App voor Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkID=262648) | Gebruik de API Graph naar access directory-gegevens van Azure AD.
| C# / .NET | [Voorbeeld-App voor differentiële Query Azure AD-grafiek](http://go.microsoft.com/fwlink/?LinkId=275398) | De differentiële query in de grafiek-API gebruiken om periodieke wijzigingen in gebruikersobjecten in een Azure AD-tenant.
| C# / .NET | [Voorbeeld-App voor het integreren van meerdere Tenant Cloud-toepassing voor Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) | Een toepassing voor meerdere tenant integreren met Azure AD.
| C# / .NET | [Een Windows Store-toepassing en de REST-webservice met Azure AD beveiligen](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Een eenvoudige API-webresource en een Windows Store-clienttoepassing met Azure AD maken en de [Azure AD verificatie bibliotheek (ADAL)](active-directory-authentication-libraries.md).
| C# / .NET| [In de grafiek API gebruiken om query's in Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Een Microsoft .NET-toepassing voor het gebruik van de API Azure AD Graph toegang tot gegevens uit een adreslijst Azure AD-tenant configureren.

## <a name="see-also"></a>Zie ook

##### <a name="other-resources"></a>Overige informatiebronnen

[Handleiding voor ontwikkelaars van de Azure Active Directory](active-directory-developers-guide.md)

[Azure AD Graph conceptuele API- en verwijzingsfunctie](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Help-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

