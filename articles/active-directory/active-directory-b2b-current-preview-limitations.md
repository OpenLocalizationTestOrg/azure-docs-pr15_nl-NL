<properties
   pageTitle="Huidige preview beperkingen voor Azure Active Directory B2B samenwerken | Microsoft Azure"
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
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Voorbeeld van de samenwerking Azure AD B2B: huidige voorbeeld beperkingen

- Meervoudige verificatie (MFA) niet ondersteund voor externe gebruikers. Bijvoorbeeld als Contoso MFA heeft, maar Partner organigram niet betekent, klikt u vervolgens Partner organigram gebruikers kunnen niet worden verleend MFA door B2B samenwerking.
- Uitnodigingen voor zijn mogelijk alleen via CSV; afzonderlijke uitnodigingen en API toegang worden niet ondersteund.
- Azure AD globale beheerders kunnen alleen CSV-bestanden te uploaden.
- Uitnodigingen voor consumenten e-mailadressen (zoals hotmail.com, Gmail.com of comcast.net) worden momenteel niet ondersteund.
- Externe gebruikerstoegang tot on-premises implementatie toepassingen niet getest.
- Externe gebruikers worden niet automatisch afgewerkt wanneer de werkelijke gebruiker wordt verwijderd uit de adreslijst.
- Uitnodigingen voor distributielijsten worden niet ondersteund.
- Maximaal 2000 records kan worden geüpload via CSV.

## <a name="related-articles"></a>Verwante artikelen
Onze andere artikelen op Azure AD B2B samenwerking bladeren:

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Werkwijze](active-directory-b2b-how-it-works.md)
- [Gedetailleerd overzicht](active-directory-b2b-detailed-walkthrough.md)
- [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
- [Externe gebruiker token opmaken](active-directory-b2b-references-external-user-token-format.md)
- [Wijzigingen van de externe gebruiker object kenmerken](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
