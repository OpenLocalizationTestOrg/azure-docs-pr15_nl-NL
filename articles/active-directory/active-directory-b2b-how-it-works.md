<properties
   pageTitle="Voorbeeld van de samenwerking Azure AD B2B: hoe dit werkt | Microsoft Azure"
   description="Wordt beschreven hoe Azure Active Directory B2B samenwerking uw relaties intern ondersteunt doordat zakenpartners selectief toegang krijgen tot uw zakelijke toepassingen"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Voorbeeld van de samenwerking Azure AD B2B: hoe het werkt
Azure AD B2B samenwerking is gebaseerd op een van de uitnodiging en model inwisselen. U vindt u de e-mailadressen van de partijen die u wilt werken, samen met de toepassingen die u wilt gebruiken. Azure AD doorgestuurd de uitnodiging voor een e-mailbericht met een koppeling. De partner-gebruiker volgt op de koppeling en wordt gevraagd aan te melden met hun Azure AD-account of registreer voor een nieuwe Azure AD-account.

1. Uw beheerder nodigt partner gebruikers door het uploaden van [een gestructureerde CSV-bestand](active-directory-b2b-references-csv-file-format.md) met behulp van de Azure portal.
2. De portal verzendt uitnodigen e-mailberichten naar deze partner-gebruikers.
3. De partner-gebruikers op de koppeling in het e-mailbericht en aanmelden met hun werk-referenties (als ze al in Azure AD), of te registreren als Azure AD B2B samenwerking gebruiker wordt gevraagd.
4. Partner gebruikers worden omgeleid naar de toepassing die zij zijn uitgenodigd, waar ze hebt nu toegang.

## <a name="directory-operations"></a>Directorybewerkingen
Gebruikers van de partner bestaat niet in uw Azure AD als externe gebruikers. Dit betekent dat uw beheerder kunt inrichten van licenties, groepslidmaatschap toewijzen en verder toegang verlenen tot zakelijke apps via de portal van Azure of Azure PowerShell net zoals met voor gebruikers in uw bedrijf.

Terwijl een betaald Azure AD abonnement (Basic of Premium) is niet nodig te gebruiken van Azure AD B2B, tenants die een betaald hebben abonnement Azure AD (Basic of Premium) ontvangen de volgende voordelen:

 - Beheerders kunnen groepen toewijzen aan apps, eenvoudiger beheer van uitgenodigd gebruikerstoegang voorzien.
 - Beheerder tenant huisstijl wordt gebruikt om de uitnodiging voor de e-mailberichten van een huisstijl voorzien en aflossingsprijs ervaring, leveren meer context aan uw partner gebruikers zijn uitgenodigd.

## <a name="related-articles"></a>Verwante artikelen
 Onze andere artikelen op Azure AD B2B samenwerking bladeren

 - [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Gedetailleerd overzicht](active-directory-b2b-detailed-walkthrough.md)
 - [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
 - [Externe gebruiker token opmaken](active-directory-b2b-references-external-user-token-format.md)
 - [Wijzigingen van de externe gebruiker object kenmerken](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Huidige preview beperkingen](active-directory-b2b-current-preview-limitations.md)
 - [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
