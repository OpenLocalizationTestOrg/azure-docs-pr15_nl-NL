<properties
   pageTitle="Identiteitsbeheer voor Multitenant toepassingen | Microsoft Azure"
   description="Aanbevolen procedures voor verificatie, autorisatie en identiteit management in multitenant-apps."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>Identiteitsbeheer voor multitenant toepassingen in Microsoft Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Deze reeks beschreven aanbevolen procedures voor multitenancy, wanneer u met behulp van Azure AD voor verificatie en -identiteit management.

Wanneer u een toepassing voor de multitenant maakt, is een van de eerste uitdagingen gebruikersidentiteiten beheren, omdat elke gebruiker nu een tenant behoort. Bijvoorbeeld:

- Gebruikers melden met hun organisatie-referenties.
- Gebruikers moeten toegang hebben tot de gegevens maar niet de gegevens die hoort bij andere tenants van hun organisatie.
- Een organisatie kunt registreren voor de toepassing en vervolgens toepassingsrollen toewijzen aan de leden.

Azure Active Directory (Azure AD) heeft enkele handige functies die ondersteuning bieden voor alle volgende scenario's beschreven.

Bij deze reeks artikelen, ook die u hebt gemaakt voor een volledige, [volledige implementatie] [ tailspin] van een multitenant-app. De artikelen doorgevoerd wat u geleerd bij het samenstellen van de toepassing. Zie [de toepassing enquêtes uitgevoerd](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md)om te beginnen met de toepassing.

Hier volgt de lijst met artikelen in deze reeks:

- [Inleiding tot identiteitsbeheer voor multitenant toepassingen](guidance-multitenant-identity-intro.md)
- [Over de toepassing de enquêtes](guidance-multitenant-identity-tailspin.md)
- [Verificatie met behulp van Azure AD en verbindt u OpenID](guidance-multitenant-identity-authenticate.md)
- [Werken met claimen gebaseerde identiteiten](guidance-multitenant-identity-claims.md)
- [Aanmelden bij en onboarding tenant](guidance-multitenant-identity-signup.md)
- [Toepassingsrollen](guidance-multitenant-identity-app-roles.md)
- [Op basis van rollen en op basis van een resource autorisatie](guidance-multitenant-identity-authorize.md)
- [Een back-end-web API beveiligen](guidance-multitenant-identity-web-api.md)
- [In de cache OAuth2 access tokens](guidance-multitenant-identity-token-cache.md)
- [Met de klant AD FS voor multitenant apps in Azure samenbrengen](guidance-multitenant-identity-adfs.md)
- [Gebruik van de client bevestiging access tokens ophalen van Azure AD](guidance-multitenant-identity-client-assertion.md)
- [Gebruik van de sleutel kluis om te beveiligen van toepassing geheimen](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
