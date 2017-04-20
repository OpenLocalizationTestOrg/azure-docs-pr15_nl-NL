<properties
    pageTitle="Het eindpunt van Azure AD v2.0 | Microsoft Azure"
    description="Een vergelijking tussen de oorspronkelijke Azure AD en de eindpunten v2.0."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="whats-different-about-the-v20-endpoint"></a>Wat is er anders over het eindpunt v2.0?

Als u bekend met Azure Active Directory bent of apps hebt geïntegreerd met Azure AD in het verleden, is het mogelijk dat er enkele verschillen in het v2.0-eindpunt dat u niet zou verwachten.  In dit document ziet u deze verschillen voor uw lidmaatschap.

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft-accounts en Azure AD-accounts
het eindpunt v2.0 kunnen ontwikkelaars schrijven apps die accepteren aanmeldingsproblemen vanuit zowel Microsoft-Accounts en Azure AD-accounts met een enkel auth-eindpunt.  Hiermee geeft u de mogelijkheid om te schrijven van uw app volledig account-agnostic; het kan zijn ignorant van het type account dat de gebruiker zich aanmeldt.  Uiteraard kunt u *kunt* uw app op de hoogte van het type account wordt gebruikt in een bepaalde sessie aanbrengen, maar u niet hoeft te.

Bijvoorbeeld, als uw app de [Microsoft Graph belt](https://graph.microsoft.io), zijn enkele extra functionaliteit en de gegevens beschikbaar voor ondernemingsgebruikers, zoals de SharePoint-sites of Directory-gegevens.  Voor veel acties, zoals het [lezen van een gebruiker e-mail](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), de code kan wel worden geschreven precies hetzelfde voor zowel Microsoft-Accounts en Azure AD-accounts.  

Uw app integreren met Microsoft-Accounts en Azure AD-accounts is nu een eenvoudig proces.  U kunt één enkele reeks eindpunten, één bibliotheek en de registratie van een één app gebruiken voor toegang tot de consumenten en de enterprise-wereld.  Meer informatie over het eindpunt v2.0, lees dan [het overzicht](active-directory-appmodel-v2-overview.md).


## <a name="new-app-registration-portal"></a>Nieuwe app-portal voor registratie
het eindpunt v2.0 kan alleen worden geregistreerd in een nieuwe locatie: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Dit is de portal waar kunt u de Id van een groep, het uiterlijk van de aanmeldingspagina van uw app en meer aanpassen.  Alles wat u nodig hebt voor toegang tot de portal is een account van Microsoft, mogelijk gemaakt - persoonlijke of werk/school-account.  

We kunnen nog meer functionaliteit toevoegen aan de Portal van de registratie van deze App na verloop van tijd.  De bedoeling is dat deze portal wordt de nieuwe locatie waar u kunt gaan om iets en alles dat u hoeft te doen met uw Microsoft-apps te beheren.


## <a name="one-app-id-for-all-platforms"></a>Een app Id voor alle platforms
In de oorspronkelijke Azure Active Directory-service, kunt u diverse andere apps voor één project hebt geregistreerd.  U moest afzonderlijke app registraties gebruiken voor uw systeemeigen clients en de WebApps:

![Registratie van oude toepassingen UI](../media/active-directory-v2-flows/old_app_registration.PNG)

Bijvoorbeeld als u een website zowel een iOS-app hebt gemaakt, moest u deze afzonderlijk te registreren met twee verschillende toepassings-id's.  Als u een website en een back-end web api had, kunt u elk als een afzonderlijke app geregistreerd in Azure AD.  Als u had een iOS-app en een Android-app, u ook mogelijk hebt geregistreerd twee verschillende apps.  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

U hoeft is nu een registratie één app en een enkel toepassings-Id voor elk van uw projecten.  U kunt verschillende "platforms' toevoegen aan een elk project en geef de juiste gegevens voor elk platform die u hebt toegevoegd.  U kunt zo veel apps als u tevreden bent met afhankelijk van uw vereisten, maar voor de meeste gevallen moet slechts één toepassings-Id nodig zijn natuurlijk kunt maken.

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

Ons doel is dat wordt dit tot een meer vereenvoudigd app beheer en ontwerpen leiden en maken van een meer gecombineerde weergave van een enkel project dat u aan kunnen werken.


## <a name="scopes-not-resources"></a>Bereiken, niet resources
Een app kunt in de oorspronkelijke Azure AD-service, gedragen zich als een **resource**of een geadresseerde van tokens.  Een resource kunt definiëren een aantal **bereiken** of **oAuth2Permissions** die wordt begrepen, zodat client apps aanvragen tokens aan deze resource voor een bepaalde reeks bereiken.  Houd rekening met de API Azure AD-grafiek als een voorbeeld van een resource:

- Resource-id, of `AppID URI`:`https://graph.windows.net/`
- Bereiken of `OAuth2Permissions`: `Directory.Read`, `Directory.Write`, enz.  

Dit alles geldt voor de het eindpunt v2.0.  Een app kan nog steeds zich gedragen als resource bereiken definiëren en worden die wordt aangeduid met een URI.  Client-apps kunnen nog steeds toegang tot deze bereiken aanvragen.  Echter heeft de manier waarop waarin een client die machtigingen aanvraagt gewijzigd.  In het verleden, een 2.0 OAuth Autoriseer aanvraag om deel te Azure AD mogelijk hebt vroeger:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Wanneer de parameter **resource** welke resource aangegeven vraagt de client-app autorisatie voor.  Azure AD berekend de machtigingen die vereist door de app op basis van statische configuratie in de Portal Azure en uitgegeven tokens dienovereenkomstig gewijzigd.  Nu Autoriseer de dezelfde OAuth 2.0 verzoek ziet ernaar uit dat:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

waarbij de parameter **scope** wordt aangegeven welke resource en machtigingen de app autorisatie voor vraagt. Er is nog steeds de gewenste resource zeer aanwezig zijn in het verzoek: deze gewoon wordt omsloten in elk van de waarden van de parameter scope.  Gebruik van de parameter scope op deze manier kunt het eindpunt v2.0 moeten beter in overeenstemming met de OAuth 2.0-specificatie en beter uitlijnen met algemene industrie procedures.  Bovendien kunt apps om uit te voeren, [incrementele toestemming](#incremental-and-dynamic-consent), die wordt beschreven in het volgende gedeelte.

## <a name="incremental-and-dynamic-consent"></a>Incrementele en dynamische toestemming
Apps geregistreerd in het algemeen beschikbaar Azure AD service nodig om op te geven van de vereiste OAuth 2.0-machtigingen in de Portal Azure tijdens het maken van een app:

![Machtigingen voor registratie UI](../media/active-directory-v2-flows/app_reg_permissions.PNG)

Welke machtigingen van een app die vereist zijn **statisch**geconfigureerd.  Terwijl dit de configuratie van de app in de Portal Azure voorkomt toegestaan en en de code nette eenvoudige blijven, daaraan enkele problemen voor ontwikkelaars:

- Een app moest weten alle machtigingen die heb dit ooit nodig bij de aanmaaktijd van een app.  Machtigingen toevoegen na verloop van tijd is een moeilijk proces.
- Een app moest alles van de bronnen voor die dit ooit tijd vooraf ook toegang weet.  Het is moeilijk te maken van de apps die toegang krijgen een willekeurig aantal resources tot kunnen.
- Een app moest aanvragen van alle machtigingen die heb dit ooit nodig na het eerste aanmelden van de gebruiker.  In sommige gevallen dit onder leiding van een aan een lijst met machtigingen die eindgebruikers uit het goedkeuren van de toegang van de app op de eerste aanmeldingsproblemen afgeraden erg lang.

Met het eindpunt v2.0, kunt u de machtigingen die uw app moet **dynamisch**, gedurende runtime, tijdens normaal gebruik van de app.  Hiervoor kunt u de bereiken uw app op elk gewenst moment tijdig moet door ze in de `scope` parameter van een autorisatieaanvraag:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

De bovenstaande aanvragen machtiging voor de app van de gebruiker van een Azure AD directory om gegevens te lezen, alsmede gegevens naar de map schrijven.  Als de gebruiker heeft ingestemd met deze machtigingen in het verleden voor deze bepaalde app, worden ze worden gewoon hun referenties invoeren en zijn aangemeld bij de app.  Als de gebruiker niet heeft ingestemd met een van deze machtigingen, vraag het eindpunt v2.0 de gebruiker voor toestemming voor deze machtigingen.  Meer informatie, u kunt meer lezen over [machtigingen, toestemming te geven, en bereiken](active-directory-v2-scopes.md).

Een app voor toegang dynamisch via zodat de `scope` parameter geeft u volledige controle over uw gebruikerservaring.  Als u wilt, kunt u frontload uw toestemming ervaring en vraagt u alle machtigingen in één eerste autorisatie verzoek.  Of als een groot aantal machtigingen vereist is voor uw app, kunt u kiezen voor het verzamelen van deze machtigingen van de gebruiker stap voor stap als zij proberen het gebruik van bepaalde functies van de app na verloop van tijd.

## <a name="well-known-scopes"></a>Bekende bereiken

#### <a name="offline-access"></a>Offline toegang
het eindpunt v2.0 kan vragen om het gebruik van een nieuwe bekende machtiging voor apps - de `offline_access` bereik.  Alle apps moet deze machtiging ook zelf aanvragen als ze nodig hebben toegang tot bronnen in de naam van een gebruiker voor een langere tijd, zelfs wanneer de gebruiker kan niet worden actief gebruik van de app.  De `offline_access` bereik wordt weergegeven aan de gebruiker in de dialoogvensters toestemming als 'Toegang tot uw gegevens offline', die de gebruiker moet accepteren.  Aanvragen van de `offline_access` machtiging wordt uw web-app moet ontvangen OAuth 2.0 refresh_tokens van het eindpunt v2.0 inschakelen.  Refresh_tokens zijn lange levensduur en kunnen worden uitgewisseld voor nieuwe OAuth 2.0 access_tokens voor langere van access.  

Als uw app vraagt niet om de `offline_access` reikwijdte, ontvangt deze geen refresh_tokens.  Dit betekent dat wanneer u een authorization_code in de [OAuth 2.0 autorisatie code stroom](active-directory-v2-protocols.md#oauth2-authorization-code-flow)inwisselen, alleen ontvangt u terug een access_token uit de `/token` eindpunt.  Die access_token blijven geldig voor een korte periode (meestal één uur), maar verloopt uiteindelijk.  AT die tijdstip, uw app moet de gebruiker omleiden terug naar de `/authorize` eindpunt om op te halen van een nieuwe authorization_code.  Tijdens deze omleiding, de gebruiker kan of niet moet hun referenties opnieuw invoeren of mogelijk opnieuw toestemming voor machtigingen, afhankelijk van het het type app.

Meer informatie over het OAuth 2.0-, refresh_tokens en access_tokens, raadpleegt u de [v2.0 protocol verwijzing](active-directory-v2-protocols.md).

#### <a name="openid-profile--email"></a>OpenID, profiel en e-mail

In de oorspronkelijke Azure Active Directory-service, zou de belangrijkste OpenID verbinding aanmeldingsproblemen stroom een enorme hoeveelheid informatie over de gebruiker in de resulterende id_token bieden.  De claims in een id_token kunnen opnemen van de gebruiker naam, voorkeur gebruikersnaam, e-mailadres, object-ID en meer.

We zijn nu de gegevens beperken dat de `openid` bereik biedt uw app toegang tot.  Het bereik 'openid' wordt alleen kunt u uw app aan te melden van de gebruiker in en ontvangen van een app-specifieke-id voor de gebruiker.  Als u ophalen van persoonlijke gegevens worden verzameld over de gebruiker in uw app wilt, moet uw app aanvullende machtigingen aanvragen van de gebruiker.  We zijn Kennismaking met twee nieuwe bereiken – de `email` en `profile` bereiken – waarmee u kunt doen.

De `email` bereik is zeer eenvoudige, maar het biedt uw app toegang tot de primaire e-mailadres van de gebruiker via de `email` claimen in de id_token.  De `profile` bereik biedt uw app toegang tot alle andere basisinformatie over de gebruiker – hun naam, de voorkeur gebruikersnaam, object-ID, enzovoort.

Hiermee kunt u uw app op een wijze minimale vrijgeven code: u kunt alleen vraag de gebruiker voor de reeks gegevens dat uw app is vereist om de werk te doen.  Voor meer informatie over deze bereiken, raadpleegt u [de v2.0 bereik verwijzing](active-directory-v2-scopes.md). 

## <a name="token-claims"></a>Token Claims

De claims in tokens uitgegeven door het eindpunt v2.0 worden niet gelijk aan tokens uitgegeven door de algemeen beschikbaar eindpunten Azure AD - apps migreren naar de nieuwe service moeten niet wordt ervan uitgegaan dat een bepaalde claim in id_tokens of access_tokens aanwezig.   Tokens uitgegeven door het eindpunt v2.0 compatibel zijn met het OAuth 2.0 en verbindt u OpenID specificaties, maar mogelijk verschillende semantiek dan de algemeen beschikbaar hiervoor voert Azure AD-service.

Meer informatie over het specifieke claims weergegeven in v2.0 tokens, raadpleegt u [v2.0 token verwijzing](active-directory-v2-tokens.md).

## <a name="limitations"></a>Beperkingen
Er gelden enkele beperkingen naar rekening moet houden bij gebruik van de komma v2.0.  Raadpleeg de [v2.0 beperkingen bij het document](active-directory-v2-limitations.md) om te zien als een van deze beperkingen op uw bepaalde scenario toepassen.
