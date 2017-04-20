<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Het maken van apps rechtstreeks met behulp van de protocollen die worden ondersteund door Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C: Verificatieprotocollen

Azure Active Directory (Azure AD) B2C biedt identiteit als een service voor uw apps door de ondersteuning van twee industriële standaard protocollen: OpenID verbinding en OAuth 2.0. De service is compatibel met standaarden, maar de twee implementaties van deze protocollen subtiele verschillen kunnen hebben.  De informatie in deze handleiding is u nuttig vindt, als u uw programmacode schrijven door rechtstreeks verzenden en HTTP-aanvragen voor de verwerking, in plaats van met een bibliotheek bron openen. Het is raadzaam dat u deze pagina lezen voordat u op de details van elk specifiek protocol meteen. Maar als u al bekend met Azure AD B2C bent, kunt u rechtstreeks naar [de protocol verwijzing hulplijnen](#protocols)gaan.

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>De basisbeginselen
Elke app met Azure AD B2C moet worden geregistreerd in uw adreslijst B2C in de [portal van Azure](https://portal.azure.com). Het registratieproces app verzamelt en enkele waarden worden toegewezen aan uw app:

- Een **Toepassings-ID** die deze identificeert op uw app.
- Een **URI omleiden** of **pakket-id** die kunnen worden gebruikt om te antwoorden terug naar uw app sturen.
- Een paar andere scenario-specifieke waarden. Informatie over [het registreren van uw toepassing](active-directory-b2c-app-registration.md)voor meer informatie.

Nadat u uw app geregistreerd, wordt deze door te sturen van aanvragen naar het eindpunt v2.0 communiceert met Azure AD:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Vrijwel alle OAuth en verbindt u OpenID overdrachten zijn vier partijen betrokken in de exchange:

![OAuth 2.0 rollen](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- De **autorisatie server** onderdeel is het eindpunt van de v2.0 Azure AD. Het verwerkt veilig gerelateerd aan gebruikersgegevens en toegang. Het ook verwerkt de vertrouwensrelaties tussen de partijen in een stroom. Dit is verantwoordelijk voor het verifiëren van de gebruikers id, verlenen en intrekken van toegang tot bronnen en tokens worden uitgeven. Het is ook bekend als de identiteitsprovider.
- De **eigenaar van de resource** is meestal de eindgebruiker. De feestje die eigenaar is van de gegevens en de kracht en het toestaan van derden voor toegang tot die gegevens of de resource heeft.
- Het **OAuth-client** is uw app. Dit wordt aangegeven door de bijbehorende toepassing-id Het is gewoonlijk de partij die eindgebruikers met samenwerken. Ook vraagt deze tokens van de server autorisatie. De eigenaar van de resource moet de machtiging client voor toegang tot de resource.
- De **server van de resource** is waarop de resource of de gegevens zich bevindt. De server autorisatie veilig verifiëren en het OAuth-client Autoriseer vertrouwde. Er worden ook vruchtdragende access tokens om ervoor te zorgen dat toegang tot een resource kan worden verleend.

## <a name="policies"></a>Beleidsregels
Hoewel, Azure AD B2C beleidsregels zijn de belangrijkste functies van de service. Azure AD B2C breidt de mogelijkheden van de standaard OAuth 2.0 en verbindt u OpenID protocollen door de introductie van beleid. Deze toestaan Azure AD B2C om uit te voeren veel meer dan eenvoudige verificatie en machtiging. Beleidsregels beschrijven volledig consumenten identiteit ervaringen, waaronder aanmelding, aanmelden en profiel bewerken. Beleid kunnen worden gedefinieerd in de gebruikersinterface van een administratieve. Ze kunnen worden uitgevoerd met behulp van een speciaal queryparameter in HTTP-verificatieaanvragen. Beleidsregels zijn niet standaardfuncties van OAuth 2.0 en OpenID verbinding kunt maken, zodat u de tijd voor meer informatie over deze moet maken. Zie [Azure AD B2C Naslaggids voor het beleid](active-directory-b2c-reference-policies.md)voor meer informatie.

## <a name="tokens"></a>Tokens
De implementatie van Azure AD B2C van OAuth 2.0 en verbindt u OpenID wordt uitgebreid gebruikgemaakt van vruchtdragende tokens, inclusief vruchtdragende tokens die worden weergegeven als JSON web tokens (JWTs). Een dragertoken is een lightweight beveiligingstoken die de "vruchtdragende" toegang tot een beveiligde bron verleent. De toonder is een partij die het token kunt presenteren. Azure AD moet een partij eerst worden geverifieerd voordat deze kan een dragertoken ontvangen. Maar als de vereiste stappen worden niet geopend secure de token in overdracht en opslag, kan deze kan worden onderschept en gebruikt door een onbedoelde partij.

Sommige beveiligingstokens ingebouwde functie waarmee u voorkomt onbevoegden dat via deze hebben, maar vruchtdragende tokens ik heb geen deze methode. Zij moeten worden overgebracht in een beveiligd kanaal, zoals transport laag beveiliging (HTTPS). Als een dragertoken buiten een beveiligd kanaal wordt verzonden, een schadelijke partij een aanval man in het midden in het bezit van het token en kunt gebruiken om toegang te krijgen onbevoegd toegang krijgen tot een beveiligde bron. Dezelfde beveiligingsprincipes toepassen wanneer vruchtdragende tokens worden opgeslagen of in cache opslaan voor later gebruik. Altijd Zorg ervoor dat uw app verzendt en vruchtdragende tokens op een veilige manier opgeslagen.

Zie voor aanvullende vruchtdragende token beveiligingsoverwegingen, [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Meer informatie over de verschillende soorten tokens die worden gebruikt in Azure AD B2C zijn beschikbaar in [de token Azure AD-verwijzing](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protocollen

Wanneer u klaar bent om te controleren van bepaalde aanvragen voorbeeld, kunt u beginnen met een van de volgende zelfstudies. Ieder een komt overeen met een bepaalde verificatie-scenario. Als u hulp bij het bepalen welke stroom geschikt voor u is nodig hebt, raadpleegt u [de soorten apps die u kunt maken met behulp van Azure AD B2C](active-directory-b2c-apps.md).

- [Mobiele en systeemeigen toepassingen maken met behulp van OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
- [WebApps maken met OpenID verbinding maken](active-directory-b2c-reference-oidc.md)
