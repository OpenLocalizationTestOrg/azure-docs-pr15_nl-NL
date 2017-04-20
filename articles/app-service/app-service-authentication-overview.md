<properties
    pageTitle="Verificatie en autorisatie in Azure App Service | Microsoft Azure"
    description="Conceptuele verwijzing en overzicht van de verificatie / autorisatie aanbevelen voor Azure App-Service"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>Verificatie en autorisatie in Azure App-Service

## <a name="what-is-app-service-authentication--authorization"></a>Wat is App Service verificatie / autorisatie?

App-Service verificatie / autorisatie is een functie waarmee u een manier om uw gebruikers aanmelden, zodat u niet hoeft te wijzigen van de code op de app-end-toepassing. Deze biedt een eenvoudige manier om te beveiligen van uw toepassing en werken met gegevens per gebruiker.

App-Service wordt gebruikt voor federatieve identiteit, waarin een identiteitsprovider van derden worden opgeslagen accounts en gebruikers worden geverifieerd. De toepassing, is afhankelijk van wat u van de provider identiteit informatie zodat de app niet hebt die gegevens zelf wilt opslaan. App-Service ondersteunt vijf identiteitsprovider afmelden bij het vak: Azure Active Directory, Facebook, Google, Microsoft-Account en Twitter. Uw app kunt u een willekeurig aantal deze identiteitsprovider gebruiken aan uw gebruikers met opties voor hoe ze zich aanmelden. Als u wilt de ingebouwde ondersteuning uitvouwen, kunt u een andere id-provider of [uw eigen aangepaste identiteit-oplossing]integreren[custom-auth].

Als u direct aan de slag wilt, raadpleegt u een van de volgende zelfstudies:

- [Verificatie toevoegen aan uw iOS-app] [ iOS] (of [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms]of [Cordova])
- [Gebruikersverificatie voor API-Apps in Azure App-Service][apia-user]
- [Aan de slag met Azure-Service voor App - deel 2][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>De werking van verificatie in App Service

Om te verifiëren via een van de identiteitsprovider, moet u eerst de identiteitsprovider moet weten over de toepassing configureren. Het id-provider, klikt u vervolgens vindt u id's en geheimen waarover u voor App-Service beschikt. Dit is de vertrouwensrelatie voltooid zodat App Service gebruiker beweringen, zoals verificatietokens, van de identiteitsprovider kunt valideren.

Als u zich aanmeldt een gebruiker met behulp van een van deze providers, moet de gebruiker worden omgeleid naar een eindpunt dat gebruikers zich voor die provider. Als klanten een webbrowser gebruikt, kunt u de App-Service automatisch alle niet-geverifieerde gebruikers verwijzen naar het eindpunt dat u zich aanmeldt gebruikers kunt hebben. Anders moet u uw klanten sturen `{your App Service base URL}/.auth/login/<provider>`, waarbij `<provider>` is een van de volgende waarden: aad, facebook, google, microsoft of twitter. Mobile en API-scenario's worden in secties verderop in dit artikel beschreven.

Gebruikers die met uw toepassing via een webbrowser werken hebben een cookie instellen zodat deze geverifieerde blijven kunnen terwijl ze uw toepassing bladeren. Voor andere clienttypen, zoals uw mobiele telefoon, een JSON web token (JWT), die moeten worden gepresenteerd de `X-ZUMO-AUTH` koptekst wordt uitgevoerd naar de klant. De Mobile-Apps-client SDK afhandelen dit voor u. U kunt ook een Azure Active Directory identiteit token of toegangstoken mogelijk rechtstreeks opgenomen in de `Authorization` koptekst als [dragertoken](https://tools.ietf.org/html/rfc6750).

App Service worden gevalideerd eventuele cookie of token die uw toepassing problemen om gebruikers te verifiëren. Als u wilt beperken wie toegang heeft tot uw toepassing, raadpleegt u de sectie [autorisatie](#authorization) verderop in dit artikel.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobiele verificatie met een provider SDK

Nadat alles is geconfigureerd op de backend kost, kunt u mobiele clients aan te melden met App Service wijzigen. Er zijn twee manieren hier:

- Gebruik een SDK die een opgegeven identiteitsprovider publiceert om de identiteit en vervolgens toegang krijgen tot de App-Service.
- Een ononderbroken lijn met code gebruiken zodat de Mobile-Apps-client SDK gebruikers zich kan aanmelden.

>[AZURE.TIP] De meeste toepassingen moeten een provider SDK gebruiken voor het krijgen van de stijl van een consistenter wanneer gebruikers zich aanmeldt, vernieuwen ondersteuning gebruiken, en andere voordelen die de provider aangeeft.

Wanneer u een provider SDK gebruikt, kunnen gebruikers zich kunnen aanmelden bij een ervaring die meer hecht geïntegreerd met het besturingssysteem dat de app wordt uitgevoerd op. Hiermee kunt u ook een token provider en sommige gebruikersgegevens op de client, waardoor u deze eenvoudiger graph API's gebruiken en aanpassen van de gebruikerservaring. Af en toe op blogs en forums ziet u dit genoemd de "client-stroom" of "stroom client omgeleid" omdat code op de client zich aanmeldt gebruikers en de clientcode toegang heeft tot een token provider.

Als een token provider staat, moet deze worden verzonden naar de App-Service voor validatie. Wanneer het token is gevalideerd met App-Service, App Service Hiermee maakt u een nieuwe App Service token die wordt geretourneerd naar de klant. De Mobile-Apps-client SDK heeft methoden voor het beheren van deze exchange en het token automatisch aan alle aanvragen voor de toepassing-end koppelen. Ontwikkelaars kunnen ook een verwijzing naar de provider token behouden desgewenst.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobiele verificatie zonder een provider SDK

Als u niet instellen op een provider SDK wilt, kunt u de functie Mobile-Apps van Azure App-Service aan te melden voor u. De Mobile-Apps-client SDK wordt een webweergave om de provider van uw keuze te openen en meld u aan de gebruiker. Af en toe op blogs en forums ziet u dit genoemd de "server stroom" of "stroom server geleide" omdat de server worden beheerd door het proces dat zich aanmeldt gebruikers en de client SDK nooit de provider token ontvangt.

Code om te beginnen deze stroom wordt opgenomen in de zelfstudie verificatie voor elk platform. Aan het einde van de stroom, de client SDK heeft een token App Service en het token wordt automatisch gekoppeld aan alle aanvragen voor de toepassing-end.

### <a name="service-to-service-authentication"></a>Verificatie van de service-naar-service

Hoewel u gebruikers toegang tot uw toepassing geven kunt, kunt u ook een andere toepassing uw eigen API aan te roepen vertrouwen. Bijvoorbeeld, kan er één WebApp een API bellen in een andere WebApp. In dit scenario u referenties voor een serviceaccount in plaats van gebruikersreferenties gebruiken om een token. Een serviceaccount wordt ook wel een *service principal* in Azure Active Directory jargon en verificatie met behulp van dergelijke account wordt ook wel een scenario voor de service-naar-service.

>[AZURE.IMPORTANT] Omdat mobiele apps uitgevoerd op klant-apparaten, doet u met mobiele toepassingen _niet_ tellen als toepassingen vertrouwde en niet een service principal stroom moet gebruiken. In plaats daarvan moeten ze de stroom van een gebruiker die eerder is gedetailleerde gebruiken.

Voor service-naar-service-scenario's kunt App Service uw toepassing beveiligen met behulp van Azure Active Directory. De bellen toepassing moet alleen bieden een Azure Active Directory-service principal Autorisatietoken die worden opgehaald door de klant-ID en geheime van Azure Active Directory-client. Een voorbeeld van dit scenario waarin ASP.NET-API apps wordt beschreven in de zelfstudie, [Service principal verificatie voor API-Apps][apia-service].

Als u App Service verificatie wilt u omgaat met een scenario voor de service-naar-service gebruikt, kunt u certificaten of basisverificatie. Voor informatie over clientcertificaten in Azure wordt aangegeven, Zie [Hoe naar configureren TLS onderlinge verificatie voor Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Zie voor informatie over basisverificatie in ASP.NET [Verificatie Filters in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Verificatie in een service-account van een App Service logica app aan een API-app is een speciale geval gedetailleerde in [werken met uw aangepaste API die worden gehost op App-Service met logica apps](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="authorization"></a>De werking van machtiging in App Service

Hebt u volledige controle over de aanvragen die uw toepassing kan openen. App-Service verificatie / autorisatie kan worden geconfigureerd met een van de volgende problemen:

- Toestaan dat alleen geverifieerde aanvragen voor uw toepassing hebt bereikt.

    Als een browser een anonieme aanvraag ontvangt, worden App Service omgeleid naar een pagina voor de identiteitsprovider die u kiest, zodat gebruikers zich kunnen aanmelden. Als het verzoek afkomstig zijn van een mobiel apparaat, is het geretourneerde antwoord een reactie HTTP _401 niet gemachtigd_ .

    Met deze optie moet u niet een verificatiecode helemaal te schrijven in uw app. Als u betere autorisatie nodig, is informatie over de gebruiker beschikbaar voor uw code.

- Alle aanvragen toestaan om bereiken van uw toepassing, maar geven verificatie-informatie in de koptekst van de HTTP geverifieerde aanvragen valideren.

    Deze optie past omleiding autorisatiebeslissingen uw programmacode. Deze biedt meer flexibiliteit bij het afhandelen van anonieme aanvragen, maar u moet programmacode schrijven.

- Toestaan dat alle aanvragen voor uw toepassing hebt bereikt, en geen actie ondernemen in verificatie-informatie in de aanvragen.

    In dit geval, de verificatie / autorisatie-functie is uitgeschakeld. De taken van verificatie en machtiging zijn geheel tot aan uw toepassingscode.

De vorige problemen worden bepaald door de optie **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** in de portal van Azure. Als u kiest u *providernaam *Meld u aan met ** **, alle aanvragen moeten worden geverifieerd.** Toestaan dat verzoek (geen actie) ** past u de beschikking autorisatie aan uw code omleiding maar nog steeds biedt verificatie-informatie. Als u uw code afhandelen, alles hebt wilt, kunt u de verificatie uitschakelen / autorisatie functie.

## <a name="working-with-user-identities-in-your-application"></a>Werken met de identiteit van de gebruikers in uw toepassing

App Service geeft sommige gebruikersgegevens aan uw toepassing met behulp van speciale kopteksten. Externe aanvragen verboden deze kop en wordt alleen presenteren als ingesteld door het App-Service verificatie / autorisatie. Sommige voorbeeld kopteksten zijn:

* X-MS-CLIENT-HOOFDSOM-NAME
* X-MS-CLIENT-PRINCIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Code die is geschreven in een taal of framework kunt krijgen van de benodigde informatie van deze headers. ASP.NET-4.6-apps voor de **ClaimsPrincipal** automatisch ingesteld met de juiste waarden.

Uw toepassing krijgt ook extra Gebruikersdetails via een HTTP GET op de `/.auth/me` eindpunt van de toepassing. Een geldige token die is inbegrepen in de aanvraag retourneert een nettolading JSON met meer informatie over de provider die wordt gebruikt, de onderliggende provider token en enkele andere gebruikersgegevens. De Mobile-Apps-server SDK's bieden methoden voor gebruik met deze gegevens. Zie [het gebruik van de Azure mobiele Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)en [werken met de .NET-endserver SDK voor Azure Mobile-Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info)voor meer informatie.

## <a name="documentation-and-additional-resources"></a>Documentatie en aanvullende bronnen

### <a name="identity-providers"></a>Identiteitsproviders
De volgende zelfstudies weergeven het App-Service voor het gebruik van verschillende verificatieproviders configureren:

- [Het configureren van uw app als u wilt gebruiken van Azure Active Directory-aanmelding][AAD]
- [Het configureren van uw app als u wilt gebruiken, Facebook login][Facebook]
- [Het configureren van uw app als u wilt gebruiken, Google login][Google]
- [Het configureren van uw app als u wilt gebruiken, Microsoft-Account aanmelden][MSA]
- [Het configureren van uw app als u wilt gebruiken, Twitter login][Twitter]

Als u een identiteit systeem dan die hier gebruiken wilt, kunt u ook gebruiken het [voorbeeld van aangepaste verificatie-ondersteuning in de mobiele Apps .NET-server SDK][custom-auth], die kan worden gebruikt in WebApps, mobiele apps of API-apps.

### <a name="web-applications"></a>Webtoepassingen
De volgende zelfstudies weergeven verificatie toevoegen aan een webtoepassing:

- [Aan de slag met Azure-Service voor App - deel 2][web-getstarted]

### <a name="mobile-applications"></a>Mobiele toepassingen
De volgende zelfstudies weergeven hoe u verificatie toevoegt aan uw mobiele clients met behulp van de stroom server doorgestuurd:

- [Verificatie toevoegen aan uw iOS-app][iOS]
- [Verificatie naar uw Android-app toevoegen] [Android]
- [Verificatie van uw Windows-app toevoegen] [Windows]
- [Verificatie toevoegen aan uw Xamarin.iOS-app] [Xamarin.iOS]
- [Verificatie toevoegen aan uw Xamarin.Android-app] [Xamarin.Android]
- [Verificatie toevoegen aan uw Xamarin.Forms-app] [Xamarin.Forms]
- [Verificatie van uw app Cordova toevoegen] [Cordova]

Als u wilt gebruiken van de client omgeleid stroom voor Azure Active Directory, gebruikt u de volgende bronnen:

- [Gebruik de Active Directory-verificatie-bibliotheek voor iOS][ADAL-iOS]
- [Gebruik de Active Directory-verificatie-bibliotheek voor Android][ADAL-Android]
- [Gebruik de Active Directory-verificatie-bibliotheek voor Windows en Xamarin][ADAL-dotnet]

Gebruik de volgende bronnen als u wilt de stroom client omgeleid voor Facebook gebruiken:

- [De Facebook-SDK gebruiken voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Als u wilt gebruiken van de client omgeleid stroom voor Twitter, gebruikt u de volgende bronnen:

- [Gebruik Twitter-stof voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Gebruik de volgende bronnen als u wilt de stroom client omgeleid voor Google gebruiken:

- [Gebruik de Google aanmeldingsproblemen SDK voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API-toepassingen
De volgende zelfstudies weergeven het beveiligen van uw apps API:

- [Gebruikersverificatie voor API-Apps in Azure App-Service][apia-user]
- [Service principal verificatie voor API-Apps in Azure App-Service][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android-apparaat]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
