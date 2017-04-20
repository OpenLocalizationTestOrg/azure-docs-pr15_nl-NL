<properties
    pageTitle="Delen van accounts met Azure AD |  Microsoft Azure"
    description="Hierin wordt beschreven hoe Azure Active Directory kunnen organisaties accounts voor on-premises implementatie-apps en consumenten cloudservices veilig te delen."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Accounts delen met Azure AD

## <a name="overview"></a>Overzicht
Soms moeten organisaties een enkele gebruikersnaam en wachtwoord gebruiken voor meerdere personen. Dit gebeurt meestal in twee gevallen:

- Bij het openen van toepassingen die een unieke aanmelden en een wachtwoord voor elke gebruiker vereisen, of on-premises implementatie apps of consumenten cloud services (bijvoorbeeld zakelijke sociale media-accounts).
- Bij het maken van meerdere gebruikers omgevingen. U wellicht een lokale account die verhoogde bevoegdheden en kan worden gebruikt om de configuratie, beheer en herstel activiteiten (bijvoorbeeld de lokale "globale beheerder" account voor Office 365) of het hoofdniveau in Salesforce core.

Deze accounts zou traditioneel worden gedeeld door de referenties (gebruikersnaam en wachtwoord) naar de juiste personen distribueren of ze opslaan in een gedeelde locatie waar meerdere vertrouwde agenten ze kunnen openen.

Het traditionele delen model heeft verschillende nadelen:

- Toegang bieden tot nieuwe toepassingen, moet u distributie van referenties voor iedereen die toegang nodig heeft.
- Elke gedeelde toepassing kan een eigen unieke set gedeelde referenties, zodat gebruikers te onthouden meerdere sets met referenties vragen. Gebruikers hebben om u te veel aanmeldingsreferenties onthouden, verhoogt het risico dat ze worden gebruik te maken van risicogevoelig gevonden procedures. (bijvoorbeeld opschrijven wachtwoorden).
- U kunt geen zien wie toegang heeft tot een toepassing.
- U kunt geen zien wie heeft *toegang tot* een toepassing.
- Wanneer u nodig hebt toegang tot een toepassing te verwijderen, moet u de referenties bijwerken en deze opnieuw distribueren naar iedereen die toegang tot die toepassing nodig heeft.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory-account delen

Azure AD vindt u een nieuwe aanpak over het gebruik van gedeelde accounts waarmee deze nadelen.

De beheerder van de Azure AD configureert welke toepassingen die een gebruiker toegang tot heeft met het deelvenster Access en kiezen van het type aanbevolen voor eenmalige aanmelding geschikt zijn voor de toepassing. Een van deze typen, *op basis van wachtwoorden eenmalige aanmelding*, kunt Azure AD fungeren als een soort "makelaar" tijdens de aanmelding voor deze app.

Gebruikers melden zich in één keer met hun organisatie-account. Dit is het account waarmee ze regelmatig gebruiken voor toegang tot hun bureaublad of een e-mailbericht. Ze kunnen ontdekken en toegang tot alleen de toepassingen die ze zijn toegewezen. Met gedeelde accounts, kunt deze lijst met toepassingen een willekeurig aantal gedeelde referenties opnemen. De gebruiker hoeft geen te onthouden of noteer de verschillende rekeningen die zij kunnen gebruiken.

Gedeelde accounts niet alleen supervisie vergroten en bruikbaarheid te verbeteren, ze ook beveiliging vergroten. Gebruikers met machtigingen om de referenties gebruiken het gedeelde wachtwoord niet ziet, maar liever krijgen machtigingen om het wachtwoord gebruiken als onderdeel van een stroom geregiseerde verificatie. Met sommige wachtwoord SSO-toepassingen, hebt u bovendien de optie om Azure AD regelmatig rollover (update) het wachtwoord voor het gebruik van wachtwoorden vermeld grote, complexe, de accountbeveiliging met groter wordende. De beheerder kan eenvoudig verlenen of toegang tot een toepassing intrekken en Daarnaast moet u weten wie toegang heeft tot het account en wie deze hebt geopend in het verleden.

Azure AD ondersteunt gedeelde accounts voor elk Enterprise mobiliteit Suite (EMS), Premium of eenvoudige gelicentieerde gebruikers, klikt u in alle typen wachtwoord één Meld u aan op toepassingen. U kunt accounts voor een van de duizenden vooraf geïntegreerde toepassingen in de galerie toepassing delen en uw eigen wachtwoord verificatie-toepassing met [aangepaste SSO-apps](active-directory-sso-integrate-saas-apps.md)kunt toevoegen.

Azure AD-functies waarmee account delen zijn:

- [Eenmalige aanmelding wachtwoord](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Wachtwoord eenmalige aanmelding-agent
- [Toewijzing aan een groep](active-directory-accessmanagement-self-service-group-management.md)
- Aangepaste wachtwoord apps
- [App dashboard/gebruiksrapporten](active-directory-passwords-get-insights.md)
- Eindgebruikers access portals
- [App-proxy](active-directory-application-proxy-get-started.md)
- [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Een account delen
Azure AD gebruiken om te delen van een account die u nodig hebt:

- Een toepassing [app-galerie](https://azure.microsoft.com/marketplace/active-directory/) of een [maatwerktoepassing](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx) toevoegen
- De toepassing voor het wachtwoord eenmalige aanmelding (SSO) configureren
- Gebruik van de [groep op basis van toewijzing](active-directory-accessmanagement-group-saasapps.md) en selecteert u de optie voor het invoeren van een gedeelde referentie
- Optioneel: in sommige toepassingen, zoals Facebook, Twitter of LinkedIn, kunt u de optie inschakelen voor [Azure AD geautomatiseerde wachtwoord album-over](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

U kunt ook uw gedeelde account beter beveiligen met meervoudige verificatie MFA () (meer informatie over het [beveiligen toepassingen met Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) en kunt u de mogelijkheid om te beheren wie toegang heeft tot de toepassing [Azure AD zelf](active-directory-accessmanagement-self-service-group-management.md) groep beheren met delegeren.

## <a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Apps gebruiken met voorwaardelijke toegang beveiligen](active-directory-conditional-access.md)
- [Selfservice-groep management/SSAA](active-directory-accessmanagement-self-service-group-management.md)
