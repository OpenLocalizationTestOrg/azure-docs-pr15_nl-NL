<properties
    pageTitle="Azure Active Directory B2C: Overzicht van | Microsoft Azure"
    description="Ontwikkelen van toepassingen van consumenten-omlaag met Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory-B2C: Aanmelden en meld u aan consumenten in uw toepassingen

Azure Active Directory-B2C is een oplossing voor uitgebreide cloud Identiteitsbeheer voor uw web consumenten-omlaag en mobiele toepassingen. Dit is een zeer beschikbaar globale service die wordt aangepast aan honderden miljoenen consumenten identiteiten. Gebaseerd op een veilige enterprise-grade-platform en blijft Azure Active Directory B2C uw toepassingen, uw bedrijf en uw consumenten beveiligd.

In het verleden, zou softwareontwikkelaars die wilt registreren en meld u aan consumenten in hun toepassingen hun eigen code hebt geschreven. En zou hebben gebruikt on-premises implementatie-databases of systemen voor de opslag van gebruikersnamen en wachtwoorden opnieuw instellen. Azure Active Directory-B2C biedt ontwikkelaars een betere manier consumenten identiteitsbeheer integreren in hun toepassingen met behulp van een beveiligde, gestandaardiseerde platform en een uitgebreide set extensible beleid. Wanneer u Azure Active Directory B2C gebruikt, kunnen uw consumenten voor uw toepassingen aanmelden met behulp van hun bestaande accounts van sociale (Facebook, Google, Amazon, LinkedIn) of door te maken van nieuwe referenties (e-mailadres en wachtwoord of gebruikersnaam en wachtwoord); verwijst naar de laatste "lokale accounts."

## <a name="get-started"></a>Aan de slag

Maken van een toepassing die consumenten registreren en aanmelden, u accepteert moet eerst de toepassing registreren met een Azure Active Directory B2C tenant. Uw eigen tenant krijgen via de stappen in [een tenant Azure AD B2C maken](active-directory-b2c-get-started.md).

U kunt uw toepassing ten opzichte van de Azure Active Directory B2C-service schrijven door te kiezen om protocolberichten te verzenden rechtstreeks met [OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) of [Open ID verbinding](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), of met behulp van onze bibliotheken waarmee het werk voor u. Uw favoriete platform kunt kiezen in de volgende tabel en aan de slag.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Wat is er nieuw

Controleer terug hier vaak meer informatie over toekomstige wijzigingen in de Azure Active Directory B2C. We wordt ook over updates tweet met behulp van @AzureAD.

- Informatie over onze [extensible beleid framework](active-directory-b2c-reference-policies.md) en de soorten beleidsregels waarmee u kunt maken en gebruiken in uw toepassingen.
- Voeg een bladwijzer onze [cloudserviceblog](https://blogs.msdn.microsoft.com/azureadb2c/) voor meldingen op secundaire serviceproblemen, updates, status en beperkingen. Gaat u verder met het [dashboard van Azure status](https://azure.microsoft.com/status/) ook controleren.
- Huidige [servicebeperkingen, beperkingen, en voorwaarden](active-directory-b2c-limitations.md).
- Ten slotte, een [voorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) met Azure AD B2C & ASP.NET Core.

## <a name="how-to-articles"></a>Artikelen

Leer hoe u specifieke functies van de Azure Active Directory B2C gebruikt:

- Configureer [Facebook](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md) [Microsoft-account](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md)en [LinkedIn](active-directory-b2c-setup-li-app.md) -accounts voor gebruik in uw toepassingen consumenten-omlaag.
- [Aangepaste kenmerken gebruiken voor het verzamelen van informatie over uw consumenten](active-directory-b2c-reference-custom-attr.md).
- [Azure meervoudige verificatie inschakelen in uw toepassingen consumenten-omlaag](active-directory-b2c-reference-mfa.md).
- [Selfservice-wachtwoord opnieuw instellen voor uw gebruikers instellen](active-directory-b2c-reference-sspr.md).
- [Pas het uiterlijk van registreren, aanmelden, en de andere pagina's van consumenten-omlaag](active-directory-b2c-reference-ui-customization.md) die worden verwerkt in Azure Active Directory B2C.
- [Gebruik de Azure Active Directory Graph API via programmacode maken, lezen, bijwerken, en verwijderen van consumenten](active-directory-b2c-devquickstarts-graph-dotnet.md) in uw tenant Azure Active Directory B2C.

## <a name="next-steps"></a>Volgende stappen

Deze koppelingen zijn handig voor het verkennen van de service in diepteas:

- Zie de [Azure Active Directory B2C prijsinformatie](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
- U kunt hulp krijgen op stapel overloop met behulp van de [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) of [adal](http://stackoverflow.com/questions/tagged/adal) tags.
- Stuur ons uw gedachten met behulp van de [Gebruiker Voice](https://feedback.azure.com/forums/169401-azure-active-directory/)--we horen graag ze! Gebruik van de uitdrukking ' AzureADB2C: "de titel van uw bericht zodat wij dit kunnen vinden.
- Bekijk de [Azure AD B2C Protocol verwijzing](active-directory-b2c-reference-protocols.md).
- Bekijk de [Azure AD B2C Token verwijzing](active-directory-b2c-reference-tokens.md).
- Lees de [Veelgestelde vragen B2C van Azure Active Directory](active-directory-b2c-faqs.md).
- [Bestand ondersteuning aanvragen voor Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u aan meldingen ontvangen van wanneer-incidenten plaatsgevonden door te bezoeken van [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneren op advies beveiligingsmeldingen.
