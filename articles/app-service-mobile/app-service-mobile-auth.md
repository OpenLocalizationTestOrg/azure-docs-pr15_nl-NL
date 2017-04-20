<properties
    pageTitle="Verificatie en autorisatie in Azure mobiele Apps | Microsoft Azure"
    description="Conceptuele verwijzing en overzicht van de verificatie / autorisatie aanbevelen voor Azure Mobile-Apps"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Verificatie en autorisatie in Azure mobiele Apps

## <a name="what-is-app-service-authentication--authorization"></a>Wat is App Service verificatie / autorisatie?

> [AZURE.NOTE] In dit onderwerp worden gemigreerd naar een geconsolideerde [App Service verificatie / autorisatie](../app-service/app-service-authentication-overview.md) onderwerp Web, Mobile en API-Apps behandelt.

App-Service verificatie / autorisatie is een functie die kan de toepassing Meld u aan gebruikers zonder code wijzigingen nodig op de app-end. Deze biedt een eenvoudige manier om te beveiligen van uw toepassing en werken met gegevens per gebruiker.

App-Service wordt gebruikt voor federatieve identiteit, waarin een 3e derden **identiteitsprovider** ("IDP') worden opgeslagen accounts en wordt geverifieerd door gebruikers, en de toepassing deze identiteit gebruikt in plaats van een eigen. App-Service ondersteunt vijf identiteitsprovider afmelden bij het vak: _Azure Active Directory_, _Facebook_, _Google_, _Microsoft-Account_en _Twitter_. U kunt ook deze ondersteuning voor uw apps uitvouwen door een andere id-provider of uw eigen aangepaste identiteit-oplossing integreren.

Een willekeurig aantal deze identiteitsproviders kunt gebruikmaken van uw app zodat u uw eindgebruikers met opties geven kan voor hoe ze zich aanmeldt.

Als u direct aan de slag wilt, raadpleegt u een van de volgende zelfstudies:

- [Verificatie toevoegen aan uw iOS-app]
- [Verificatie toevoegen aan uw Xamarin.iOS-app]
- [Verificatie toevoegen aan uw Xamarin.Android-app]
- [Verificatie toevoegen aan uw Windows-app]

## <a name="how-authentication-works"></a>De werking van verificatie

Om te verifiëren met een van de identiteitsprovider, moet u eerst de identiteitsprovider moet weten over de toepassing configureren. De identiteitsprovider vervolgens krijgt u id's en geheimen die u terug naar de toepassing opgeeft. Dit voltooit u de vertrouwensrelatie en Hiermee kan de App Service identiteiten weergegeven om deze te valideren.

Deze stappen worden beschreven in de volgende onderwerpen:

- [Het configureren van uw app als u wilt gebruiken van Azure Active Directory-aanmelding]
- [Het configureren van uw app als u wilt gebruiken, Facebook login]
- [Het configureren van uw app als u wilt gebruiken, Google login]
- [Het configureren van uw app als u wilt gebruiken, Microsoft-Account aanmelden]
- [Het configureren van uw app als u wilt gebruiken, Twitter login]

Nadat alles is geconfigureerd op de backend kost, kunt u uw client en meld u aan. Er zijn twee manieren hier:

- Met één regel met code, laat de Mobile-Apps client SDK Meld u aan gebruikers.
- Gebruikmaken van een SDK uitgegeven door een opgegeven identiteitsprovider naar identiteit en vervolgens toegang krijgen tot de App-Service.

>[AZURE.TIP] De meeste toepassingen moeten een provider SDK gebruiken om een meer native-gevoel login ervaring en om te profiteren van ondersteuning voor vernieuwen en andere providerspecifieke voordelen.

### <a name="how-authentication-without-a-provider-sdk-works"></a>De werking van verificatie zonder een provider SDK

Als u niet instellen op een provider SDK wilt, kunt u de Mobile-Apps voor het uitvoeren van de aanmelding voor u toestaan. De Mobile-Apps-client SDK wordt een webweergave om de provider van uw keuze te openen en vul de aanmelden. Af en toe aan blogs en forums ziet u dit waarnaar wordt verwezen als de "server stroom" of "stroom server geleide" sinds de server is de aanmelding beheren en de client SDK nooit de provider token ontvangt.

De code die nodig zijn voor het starten van deze overdracht vindt u in de zelfstudie verificatie voor elk platform. Aan het einde van de stroom, de client SDK heeft een token App Service en het token wordt automatisch gekoppeld aan alle aanvragen voor het back-end.

### <a name="how-authentication-with-a-provider-sdk-works"></a>De werking van verificatie met een provider SDK

Werken met een provider kunt SDK de ervaring aanmelden om te communiceren dichter met het platform OS dat de app wordt uitgevoerd op. Hiermee kunt u ook een token provider en sommige gebruikersgegevens op de client, waardoor u deze eenvoudiger graph API's gebruiken en aanpassen van de gebruikerservaring. Af en toe op blogs en forums ziet u dit genoemd de "client-stroom" of "client omgeleid stroom" sinds code op de client is de aanmelding voor de verwerking en de clientcode toegang heeft tot een token provider.

Nadat een token provider hebt ontvangen, moet deze worden verzonden naar de App-Service voor validatie. Aan het einde van de stroom, de client SDK heeft een token App Service en het token wordt automatisch gekoppeld aan alle aanvragen voor het back-end. De ontwikkelaar kan ook een verwijzing naar de token provider houden desgewenst.

## <a name="how-authorization-works"></a>De werking van autorisatie

App-Service verificatie / autorisatie beschrijft de verschillende mogelijkheden voor **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd**. Voordat u uw code een bepaald aanvraag ontvangt, kunt u de App Service controleren om te zien als het verzoek is geverifieerd en als niet, weigeren en probeert om de gebruiker aanmelden voordat u probeert opnieuw kunt hebben.

Eén optie is hebt niet-geverifieerde aanvragen omleiden naar een van de identiteitsprovider. In een webbrowser, zou dit daadwerkelijk de gebruiker op een nieuwe pagina duren. Echter uw mobiele client niet op deze manier worden omgeleid en niet-geverifieerde antwoorden ontvangt een antwoord HTTP _401 niet gemachtigd_ . De eerste aanvraag die zorgt ervoor dat de klant moet altijd naar het eindpunt login gegeven dit, en breng kunt u oproepen naar een andere API's. Als u probeert te bellen van een andere API voordat u zich aanmeldt, ontvangt de klant een fout.

Als u meer gedetailleerde wilt laten bepalen op welke eindpunten verificatie vereist, kunt u ook kiezen "geen actie (toestaan verzoek)" voor niet-geverifieerde aanvragen. In dit geval worden alle verificatie beslissingen uitgesteld uw programmacode. Hiermee kunt u ook toegang geeft tot specifieke gebruikers op basis van aangepaste autorisatieregels.

## <a name="documentation"></a>Documentatie

De volgende zelfstudies weergeven verificatie toevoegen aan uw mobiele clients met App-Service:

- [Verificatie toevoegen aan uw iOS-app]
- [Verificatie toevoegen aan uw Xamarin.iOS-app]
- [Verificatie toevoegen aan uw Xamarin.Android-app]
- [Verificatie toevoegen aan uw Windows-app]

De volgende zelfstudies weergeven het App-Service om te kunnen verschillende verificatieproviders configureren:

- [Het configureren van uw app als u wilt gebruiken van Azure Active Directory-aanmelding]
- [Het configureren van uw app als u wilt gebruiken, Facebook login]
- [Het configureren van uw app als u wilt gebruiken, Google login]
- [Het configureren van uw app als u wilt gebruiken, Microsoft-Account aanmelden]
- [Het configureren van uw app als u wilt gebruiken, Twitter login]

Als u gebruiken van een systeem identiteit dan die hier wilt, kunt u ook gebruikmaken van de [ondersteuning voor aangepaste verificatie in de .NET-server SDK voorbeeld](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Verificatie toevoegen aan uw iOS-app]: app-service-mobile-ios-get-started-users.md
[Verificatie toevoegen aan uw Xamarin.iOS-app]: app-service-mobile-xamarin-ios-get-started-users.md
[Verificatie toevoegen aan uw Xamarin.Android-app]: app-service-mobile-xamarin-android-get-started-users.md
[Verificatie toevoegen aan uw Windows-app]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Het configureren van uw app als u wilt gebruiken van Azure Active Directory-aanmelding]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Het configureren van uw app als u wilt gebruiken, Facebook login]: app-service-mobile-how-to-configure-facebook-authentication.md
[Het configureren van uw app als u wilt gebruiken, Google login]: app-service-mobile-how-to-configure-google-authentication.md
[Het configureren van uw app als u wilt gebruiken, Microsoft-Account aanmelden]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Het configureren van uw app als u wilt gebruiken, Twitter login]: app-service-mobile-how-to-configure-twitter-authentication.md
