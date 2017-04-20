<properties
    pageTitle="Azure AD v2.0 protocollen | Microsoft Azure"
    description="Een handleiding voor protocollen die worden ondersteund door het eindpunt van de v2.0 Azure AD."
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

# <a name="v20-protocols---oauth-20--openid-connect"></a>v2.0 protocollen - OAuth 2.0 & OpenID verbinding maken

Het eindpunt v2.0 kunt Azure AD voor identiteit-als-een-service met industriële standaard protocollen, OpenID verbinding en OAuth 2.0 gebruiken.  Terwijl de service compatibel met standaarden is, kunnen er subtiele verschillen tussen de twee implementaties van deze protocollen.  De informatie hier is handig als u wilt uw programmacode schrijven door te sturen rechtstreeks & HTTP-aanvragen of gebruik een 3e feestje aan komt voor de verwerking openen Bronbibliotheek, in plaats van met een van onze bibliotheken bron openen.
<!-- TODO: Need link to libraries above -->

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="the-basics"></a>De basisbeginselen
Vrijwel alle OAuth & OpenID verbinding overdrachten zijn er vier partijen betrokken in de exchange:

![OAuth 2.0 rollen](../media/active-directory-v2-flows/protocols_roles.png)

- De **Autorisatie Server** onderdeel is het eindpunt v2.0.  Dit is verantwoordelijk voor zorgen dat de gebruikers id, verlenen en intrekken van toegang tot bronnen en tokens worden uitgeven.  Het is ook bekend als de identiteitsprovider - niets te maken met gegevens van de gebruiker, hun access en de vertrouwensrelaties tussen partijen in een stroom veilig worden verwerkt.
- De **Eigenaar van de Resource** is meestal de eindgebruiker.  Dit is de partij die eigenaar is van de gegevens en de kracht en het toestaan van derden voor toegang tot die gegevens of de resource heeft.
- Het **OAuth-Client** is uw app, die wordt aangeduid met de toepassings-id.  Het is gewoonlijk degene die de eindgebruiker met samenwerkt en deze tokens van de server autorisatie vraagt.  De client moet worden gemachtigd voor toegang tot de resource door de eigenaar van de resource.
- De **Server van de Resource** is waarop de resource of de gegevens zich bevindt.  Deze vertrouwensrelaties van de Server autorisatie veilig verificatie en machtiging van het OAuth-Client en vruchtdragende access_tokens gebruikt om ervoor te zorgen dat toegang tot een resource kan worden verleend.


## <a name="app-registration"></a>Registratie-App
Elke app waarin het eindpunt v2.0 moet worden geregistreerd bij [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) voordat u deze kunt werken met OAuth of OpenID verbinding maken.  Het registratieproces app verzamelen & enkele waarden toewijzen aan uw app:

- Een **Toepassings-Id** die deze identificeert op uw app
- Een **URI omleiden** of **Pakket-id** die kunnen worden gebruikt om te antwoorden terug naar uw app sturen
- Een paar andere scenario-specifieke waarden.

Informatie voor meer informatie over hoe u [een app registreert](active-directory-v2-app-registration.md).

## <a name="endpoints"></a>Eindpunten
Zodra geregistreerd, wordt de status van de app communiceert met Azure AD door te sturen van aanvragen naar het eindpunt v2.0:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Waar de `{tenant}` een van de vier verschillende waarden kunt uitvoeren:

| Waarde | Beschrijving |
| ----------------------- | ------------------------------- |
| `common` | Kunnen gebruikers met zowel persoonlijk Microsoft-accounts en werk-, school-accounts van Azure Active Directory u zich aanmeldt bij de toepassing. |
| `organizations` | Alleen gebruikers met een werk-, school-accounts staat van Azure Active Directory u zich aanmeldt bij de toepassing. |
| `consumers` | Alleen gebruikers met een persoonlijk Microsoft-accounts (MSA) u zich aanmeldt bij de toepassing kunt. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`of`contoso.onmicrosoft.com` | Kunnen alleen gebruikers met werk-, school-accounts van een bepaalde Azure Active Directory-tenant u zich aanmeldt bij de toepassing.  De domeinnaam van het beschrijvende van de Azure AD-tenant of van de tenant guid id kan worden gebruikt.  |

Kies een bepaalde app type hieronder voor meer informatie over het werken met deze eindpunten.

## <a name="tokens"></a>Tokens
De uitvoering v2.0 van OAuth 2.0 en verbindt u OpenID benutten uitgebreide vruchtdragende tokens, inclusief vruchtdragende tokens weergegeven als JWTs. Een dragertoken is een lightweight beveiligingstoken die de "vruchtdragende" toegang tot een beveiligde bron verleent. In deze zin is de "toonder" een partij die het token kunt presenteren. Hoewel een partij moet eerst worden geverifieerd bij Azure AD voor het ontvangen van de dragertoken, als de vereiste stappen zijn niet die u hebt gemaakt voor het beveiligen van de token in overdracht en opslag, kunt u er onderschept en die wordt gebruikt door een onbedoelde feest. Sommige beveiligingstokens beschikken over een ingebouwde methode voor het voorkomen dat onbevoegden ze te gebruiken, wordt aan toonder tokens geen deze methode hebt en moeten worden overgebracht in een beveiligd kanaal zoals transport layer beveiliging (HTTPS). Als een dragertoken wordt verzonden in het wissen, kan een man in de middelste aanval worden gebruikt door een schadelijke partij bij het aanschaffen van het token en deze gebruiken voor een onbevoegd toegang krijgen tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer wilt opslaan of caching van vruchtdragende tokens voor later gebruik. Altijd Zorg ervoor dat uw app verzendt en vruchtdragende tokens op een veilige manier opgeslagen. Zie voor meer beveiligingsoverwegingen op vruchtdragende tokens, [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Meer informatie over de verschillende soorten tokens die worden gebruikt in het eindpunt v2.0 is beschikbaar in [de v2.0 eindpunt token verwijzing](active-directory-v2-tokens.md).

## <a name="protocols"></a>Protocollen

Als u klaar om bepaalde aanvragen voorbeeld weer te geven bent, aan de slag met een van de onderstaande zelfstudies.  Ieder een komt overeen met een bepaalde verificatie-scenario.  Als u hulp vaststellen van de juiste stroom voor u nodig hebt, raadpleegt u [de soorten apps die u met de v2.0 maken kunt](active-directory-v2-flows.md).

- [Mobile en de bijbehorende toepassing met OAuth 2.0 maken](active-directory-v2-protocols-oauth-code.md)
- [Maken van Web-Apps gebruiken met Open-ID verbinding maken](active-directory-v2-protocols-oidc.md)
- [Eén pagina-Apps gebruiken met de impliciete OAuth 2.0-stroom maken](active-directory-v2-protocols-implicit.md)
- [Opbouwen Daemons of processen op de Server met de referenties van de OAuth 2.0-Client Flow](active-directory-v2-protocols-oauth-client-creds.md)
- Tokens krijgen in een Web-API met het OAuth 2.0 namens Flow (binnenkort)

<!-- - Get tokens using a username & password with the OAuth 2.0 Resource Owner Password Credentials Flow (coming soon) --> 
