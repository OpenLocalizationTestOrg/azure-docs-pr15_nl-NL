<properties
   pageTitle="Bibliotheken van Azure Active Directory-verificatie | Microsoft Azure"
   description="De Azure AD verificatie bibliotheek (ADAL) kan client softwareontwikkelaars eenvoudig om gebruikers te verifiëren naar cloud of lokale Active Directory (AD) en moet u toegangstokens voor het beveiligen van API-oproepen."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Bibliotheken van Azure Active Directory-verificatie

De verificatie van Azure AD bibliotheek (ADAL) kan client toepassingsontwikkelaars eenvoudig om gebruikers te verifiëren naar cloud of lokale Active Directory (AD) en moet u toegangstokens voor het beveiligen van API-oproepen. ADAL bevat veel functies die verificatie tabelmaakquery eenvoudiger voor ontwikkelaars, zoals asynchroon ondersteuning, een configureerbare token cache die toegangstokens en vernieuwen tokens, automatisch token vernieuwen wanneer een toegangstoken verloopt en een token vernieuwen beschikbaar is, worden opgeslagen en nog veel meer. Door het grootste deel van de complexiteit afhandelen, ADAL helpen de focus van een ontwikkelaar op bedrijfslogica in hun-toepassing en eenvoudig resources verzamelen zonder dat ze een expert op beveiliging.

ADAL is beschikbaar op allerlei platforms.

|Platform|Pakketnaam|Client/Server|Downloaden|Broncode|Documentatie en voorbeelden|
|---|---|---|---|---|---|
|.NET-client, Windows Store, UWP, Xamarin iOS en Android|Active Directory-verificatie bibliotheek (ADAL) voor .NET v3 |Client|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL voor .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Documentatie](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET-client, Windows Store, Windows Phone 8.1 |Active Directory-verificatie bibliotheek (ADAL) voor .NET v2 |Client|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL voor .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Documentatie](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|JavaScript|Active Directory-verificatie bibliotheek (ADAL) voor JavaScript|Client|[ADAL voor JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL voor JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Voorbeeld: [SinglePageApp-DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|OS X, iOS|Active Directory-verificatie bibliotheek (ADAL) voor het doel-C|Client|[ADAL voor doelstelling-C (CocoaPods)](http://cocoadocs.org/docsets/ADAL/)|[ADAL voor doelstelling-C (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Voorbeeld: [NativeClient-iOS (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android-apparaat|Active Directory-verificatie bibliotheek (ADAL) voor Android|Client|[ADAL voor Android (de centrale opslagplaats)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL voor Android (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Voorbeeld: [NativeClient-Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|Active Directory-verificatie bibliotheek (ADAL) voor Node.js|Client|[ADAL voor Node.js (npm)](https://www.npmjs.com/package/adal-node)|[ADAL voor Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Voorbeeld: [WebAPI-Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Microsoft Azure Active Directory Passport verificatie middleware voor knooppunt|Client|[Azure Active Directory-Passport voor Node.js (npm)](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directory voor Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Active Directory-verificatie bibliotheek (ADAL) voor Java|Client|[ADAL voor Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL voor Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|Identiteit Protocol extensies voor Microsoft .NET Framework 4.5|Server|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Azure AD identiteit model extensies voor .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|JSON Web Token-Handler voor de Microsoft .net Framework 4.5|Server|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Azure AD identiteit model extensies voor .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|OWIN middleware waarmee een toepassing voor het gebruik van Microsoft technologie voor verificatie.|Server|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|OWIN middleware waarmee een toepassing voor het gebruik van OpenIDConnect voor verificatie.|Server|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Voorbeeld: [WebApp-OpenIDConnecty-DotNet (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|OWIN middleware waarmee een toepassing voor het gebruik van WS-Federation voor verificatie.|Server|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Voorbeeld: [WebApp-WSFederation-DotNet (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Scenario 's

Hier vindt u drie veelvoorkomende scenario's waarin ADAL kan worden gebruikt voor verificatie.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Verificatie van gebruikers van een clienttoepassing naar een externe bron

In dit scenario heeft een ontwikkelaar een mailclient gebruikt, zoals een WPF-toepassing, die vereist zijn voor toegang tot een externe bron beveiligd met Azure AD, zoals een web API. Hij heeft een Azure-abonnement, weet hoe u de volgende web API roepen en kent van de Azure AD-tenant die het web API wordt gebruikt. Hierdoor kunt hij ADAL verificatie met Azure Active Directory, door het delegeren van volledig de verificatie-ervaring naar ADAL of door expliciet gebruikersreferenties voor de verwerking om te gebruiken. ADAL maakt het gemakkelijk te verifiëren van de gebruiker, een toegangstoken en vernieuwen token ophalen van Azure AD en brengt u aan met het toegangstoken aanvraagt op het web API.

Zie [Systeemeigen WPF clienttoepassing Web API](https://github.com/azureadsamples/nativeclient-dotnet)voor een voorbeeld waarop u dit scenario Azure AD-verificatie.

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Verificatie van een servertoepassing naar een externe bron

In dit scenario heeft een ontwikkelaar een toepassing wordt uitgevoerd op een server die vereist zijn voor toegang tot een externe bron beveiligd met Azure AD, zoals een web API. Hij heeft een Azure-abonnement, weet hoe u de volgende service roepen en kent van dat de Azure AD-tenant de web-API wordt gebruikt. Hierdoor kunt hij ADAL verificatie met Azure AD om door te vergemakkelijken expliciet voor de verwerking van de toepassing referenties gebruiken. ADAL maken vraagt deze heel gemakkelijk een token ophalen van Azure AD met behulp van de toepassing client referentie en brengt u aan met die token op het web API. ADAL verwerkt ook de levensduur van het toegangstoken beheren door caching van deze en deze zo nodig te vernieuwen. Zie [Console-toepassing tot Web API](https://github.com/AzureADSamples/Daemon-DotNet)voor een voorbeeld waarop u dit scenario.

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Verificatie van een servertoepassing namens een gebruiker voor toegang tot een externe bron

In dit scenario heeft een ontwikkelaar een toepassing wordt uitgevoerd op een server die vereist zijn voor toegang tot een externe bron beveiligd met Azure AD, zoals een web API. De aanvraag moet ook namens een gebruiker in Azure AD worden aangebracht. Hij heeft een Azure-abonnement, weet hoe u het volgende web API roepen en kent van de Azure AD tenant van de service wordt gebruikt. Als de gebruiker aan de webtoepassing is geverifieerd, kan de toepassing een autorisatiecode krijgen voor de gebruiker van Azure AD. De webtoepassing kunt ADAL verkrijgen van een toegangstoken en token namens een gebruiker met de autorisatie-code en client-referenties die zijn gekoppeld aan de toepassing van Azure AD vernieuwen. Als u de webtoepassing hebt in het bezit van het toegangstoken, kan het web API aanroepen totdat het token verloopt. Als het token verloopt, kunt ADAL als u een nieuwe toegangstoken met behulp van het vernieuwen token die eerder is ontvangen door de webtoepassing gebruiken.


## <a name="see-also"></a>Zie ook

[Handleiding voor ontwikkelaars van de Azure Active Directory](active-directory-developers-guide.md)

[Verificatie-scenario's voor Azure Active directory](active-directory-authentication-scenarios.md)

[Voorbeelden van Azure Active Directory-code](active-directory-code-samples.md)
