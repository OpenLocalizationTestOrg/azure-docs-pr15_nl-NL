<properties
   pageTitle="Wijzigingen van een externe gebruiker-object-kenmerken, voor Azure Active Directory B2B samenwerking preview | Microsoft Azure"
   description="Azure Active Directory-B2B ondersteunt uw relaties intern doordat zakenpartners selectief toegang krijgen tot uw zakelijke toepassingen"
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
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Voorbeeld van de samenwerking Azure AD B2B: wijzigingen van de externe gebruiker object kenmerken

Elke gebruiker in een Azure AD-directory wordt voorgesteld door een gebruikersobject. Het gebruikersobject in Azure AD betrokken kenmerkwijzigingen in verschillende fasen van de samenwerking B2B uitnodigen-inwisselen stroom. De gebruiker object dat staat voor de gebruiker partner in de adreslijst heeft kenmerken die wijzigen aan inwisselen tijd, wanneer de partner-gebruiker de koppeling in de e-mail uitnodigen op. Name:

- De kenmerken **SignInName** en **AltSecId** zijn gevuld
- Het kenmerk **om** wijzigingen uit de User Principal Name (user_fabrikam.com#EXT#@contoso.com) op de naam aanmelden(user@fabrikam.com)

Het bijhouden van de volgende kenmerken in Azure AD, kunt u problemen met al dan niet een partner-gebruiker heeft de uitnodiging voor de samenwerking B2B hebt ingewisseld.

## <a name="related-articles"></a>Verwante artikelen
Onze andere artikelen op Azure AD B2B samenwerking bladeren:

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Werkwijze](active-directory-b2b-how-it-works.md)
- [Gedetailleerd overzicht](active-directory-b2b-detailed-walkthrough.md)
- [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
- [Externe gebruiker token opmaken](active-directory-b2b-references-external-user-token-format.md)
- [Huidige preview beperkingen](active-directory-b2b-current-preview-limitations.md)
- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
