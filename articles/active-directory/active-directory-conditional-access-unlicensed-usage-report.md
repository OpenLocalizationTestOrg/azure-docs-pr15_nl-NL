<properties
    pageTitle="Zonder licentie gebruiksrapport | Microsoft Azure"
    description="De zonder licentie gebruik rapport kunt die u vaststelt dat gebruikers zonder licentie dat gebruikt dat is betaald Azure AD-functies."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="unlicensed-usage-report"></a>Gebruiksrapport zonder licentie

De zonder licentie gebruik rapport kunt die u vaststelt dat gebruikers zonder licentie dat gebruikt dat is betaald Azure AD-functies. Hiermee kunt u nog beter maken gebruik van de licenties die u hebt gekocht en om te identificeren die u weet wanneer u mogelijk extra licenties nodig heb. 

Het rapport weergeven actieve gebruik van de betaalde functies in de afgelopen 30 dagen 

## <a name="report-structure"></a>Rapport-structuur
 
| Kolomnaam          |    Beschrijving |
| :--                  | :--         |
| Gebruikers zonder licentie      |    Naam van de gebruiker |
| Functie              | De naam van de functie. Bijvoorbeeld: voorwaardelijke toegang |
| Toepassing geopend | De naam van de toepassing die wordt geopend met de functie. Bijvoorbeeld: Office 365 SharePoint Online |

 
> [AZURE.NOTE] Als een gebruikersaccount is verwijderd de kolom 'Gebruiker zonder licentie' wordt gevuld met een ID, zoals 1003000090D8B285


## <a name="conditional-access-feature"></a>Voorwaardelijke access-functie

Gebruikers zonder licentie worden, gemarkeerd wanneer ze toegang een service waarop voorwaardelijke-beleid toegepast tot als ze een Azure AD Premium-licentie niet hebben. 

Dit geldt voor MFA / locatie beleidsregels, evenals apparaat beleid die Intune gebruiken.
 

## <a name="see-also"></a>Zie ook

- [Apps voorwaardelijke Access gebruiken met Office 365 en andere Azure Active Directory worden verbonden](active-directory-conditional-access.md)
- [Aan de slag met voorwaardelijke toegang tot Azure AD](active-directory-conditional-access-azuread-connected-apps.md) 


