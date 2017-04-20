<properties
   pageTitle="Externe gebruiker token opmaak voor de Preview-versie van Azure Active Directory B2B samenwerking | Microsoft Azure"
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

# <a name="azure-ad-b2b-collaboration-preview-external-user-token-format"></a>Voorbeeld van de samenwerking Azure AD B2B: externe gebruiker token opmaken

De claims voor een standaard Azure AD token worden beschreven in het artikel [Token ondersteund en claimtypen](active-directory-token-and-claims.md) op azure.microsoft.com.

De claims die een externe gebruiker bij geverifieerde B2B samenwerking verschillen zijn als volgt:<br/>
- **OID:** de object-ID van de resource-tenant<br/>
- **TID**: tenant ID van de resource-tenant<br/>
- **Uitgever**: dit is de resource-tenant<br/>
- **IDP**: dit is de thuis tenant van de gebruiker<br/>
- **AltSecId**: dit is de alternatieve beveiligings-ID, dat wil ondoorzichtig naar u zeggen<br/>

## <a name="related-articles"></a>Verwante artikelen
Onze andere artikelen op Azure AD B2B samenwerking bladeren:

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Werkwijze](active-directory-b2b-how-it-works.md)
- [Gedetailleerd overzicht](active-directory-b2b-detailed-walkthrough.md)
- [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
- [Wijzigingen van de externe gebruiker object kenmerken](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Huidige preview beperkingen](active-directory-b2b-current-preview-limitations.md)
- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
