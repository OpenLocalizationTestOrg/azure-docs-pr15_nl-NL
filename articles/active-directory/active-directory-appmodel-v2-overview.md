<properties
    pageTitle="Overzicht van v2.0 eindpunt | Microsoft Azure"
    description="Een inleiding tot het bouwen van apps gebruiken met zowel Microsoft-Account en Azure Active Directory-aanmelding."
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
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Aanmelden bij Microsoft-Account en Azure AD-gebruikers in een één-app

In het verleden, is een app-ontwikkelaar die ondersteuning bieden voor zowel Microsoft-accounts en Azure Active Directory wilt integreren met twee afzonderlijke systemen vereist.  We hebt nu een nieuwe verificatie API versie waarmee u aan te melden gebruikers met beide typen accounts met het systeem Azure AD geïntroduceerd.  Dit geconvergeerde verificatiesysteem, wordt **het eindpunt v2.0**genoemd.  Met het eindpunt v2.0 kunt een eenvoudige integratie u een doelgroep die zich uitstrekt over miljoenen gebruikers met persoonlijke en werk-, school-accounts hebt bereikt.

Apps die gebruikmaken van het eindpunt v2.0 kunnen ook in beslag nemen REST API's via de [Microsoft Graph](https://graph.microsoft.io) en [Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) met een van beide type account.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Aan de slag
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Uw favoriete platform kunt kiezen uit de volgende lijst om een app met behulp van onze bron openen bibliotheken kaders te bouwen.  U kunt ook kunt u de documentatie van onze OAuth 2.0 & OpenID verbinding protocol om te verzenden en ontvangen van protocolberichten rechtstreeks zonder een auth-bibliotheek.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Wat is er nieuw
De informatie die hier worden nuttig begrijpen wat is en wat is niet mogelijk is met het eindpunt v2.0.

- Meer informatie over de [apps die u met het eindpunt v2.0 maken kunt typen](active-directory-v2-flows.md).
- Meer informatie over de [beperkingen, beperkingen, en beperkingen](active-directory-v2-limitations.md) aan het eindpunt v2.0.
- Er is ondersteuning voor [beheerder beperkte bereiken](active-directory-v2-scopes.md) en de [OAuth2 client referenties verlenen](active-directory-v2-protocols-oauth-client-creds.md)onlangs toegevoegd.  Ze uitprobeert!

## <a name="reference"></a>Verwijzing
Deze koppelingen zijn handig voor het verkennen van het platform alles:

- Opbouwen 2016: [aan de slag met Microsoft identiteiten: Enterprise Grade aanmelden voor uw Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- U kunt hulp krijgen van de stapel overloop met behulp van de [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) of [adal](http://stackoverflow.com/questions/tagged/adal) tags.
- [v2.0 Protocol verwijzing](active-directory-v2-protocols.md)
- [v2.0 Token verwijzing](active-directory-v2-tokens.md)
- [naslaginformatie over v2.0-bibliotheek](active-directory-v2-libraries.md)
- [Bereiken en toestemming in het eindpunt v2.0](active-directory-v2-scopes.md)
- [De Microsoft-App registratie-Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Office 365 REST API verwijzing](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [De Microsoft Graph](https://graph.microsoft.io)