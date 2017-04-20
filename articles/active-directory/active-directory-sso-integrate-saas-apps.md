<properties
    pageTitle="Eenmalige aanmelding Azure Active Directory integreren met SaaS apps |  Microsoft Azure"
    description="Eenmalige aanmelding verificatie en gecentraliseerde toegangsbeheer met SaaS-apps in Azure Active Directory van gebruikers inschakelen. Een overzicht van het integreren van Azure Active Directory aan SaaS-apps."
    services="active-directory"
      keywords="Azure AD integreren met SaaS apps"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Eenmalige aanmelding Azure Active Directory integreren met SaaS apps  

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-enterprise-apps-manage-sso.md)
- [Azure klassieke portal](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Als u wilt beginnen instellen van eenmalige aanmelding voor een app die u in uw organisatie bent brengen, wordt u een bestaande map in Azure Active Directory (Azure AD). U kunt een Azure AD-map die u via Microsoft Azure-, Office 365- of Windows Intune krijgen kunt gebruiken. Als u twee of meer van de volgende hebt, raadpleegt u [uw Azure AD-directory beheren](active-directory-administer.md) om te bepalen naar welke gebruiken.

## <a name="authentication"></a>Verificatie

Voor toepassingen ondersteunen die de SAML 2.0, WS-Federatie, of OpenID verbinding protocollen, Azure Active Directory worden gebruikt voor het ondertekenen van certificaten tot stand brengen van vertrouwensrelaties. Zie voor meer informatie over dit [beheren certificaten voor federatieve eenmalige aanmelding](active-directory-sso-certs.md).

Voor toepassingen die ondersteuning bieden voor alleen HTML op formulieren gebaseerde aanmeldingsproblemen, Azure Active Directory gebruikmaakt van 'wachtwoord vaulting' tot stand brengen van vertrouwensrelaties. Hiermee worden de gebruikers in uw organisatie kunnen automatisch worden aangemeld bij een toepassing voor SaaS door Azure AD met de gegevens van de gebruiker van de configuratietoepassing SaaS. Azure AD verzamelt en gegevens van de gebruikersaccount en de gerelateerde wachtwoord veilig worden opgeslagen. Zie voor meer informatie [op basis van wachtwoorden eenmalige aanmelding](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Autorisatie

Een ingerichte account kan een gebruiker zijn gemachtigd om te gebruiken van een toepassing nadat ze via eenmalige aanmelding hebt geverifieerd. Inrichting van de gebruiker kan worden uitgevoerd handmatig of in sommige gevallen u kunt toevoegen en verwijderen van gebruikersgegevens uit de SaaS-app op basis van wijzigingen hebt aangebracht in Azure Active Directory. Zie voor meer informatie over het gebruik van bestaande Azure AD-connectors voor het inrichten van geautomatiseerde [automatisch gebruiker inrichting en verwijderen van gegevens voor SaaS-toepassingen](active-directory-saas-app-provisioning.md).

Anders, u kunt handmatig gebruikersgegevens toevoegen aan een app, of gebruik andere inrichten oplossingen die beschikbaar in de marketplace zijn.

## <a name="access"></a>Access

Azure AD biedt aanpasbare op verschillende manieren om te implementeren toepassingen voor eindgebruikers in uw organisatie. U bent niet vergrendeld in een bepaalde implementatie of access-oplossing. U kunt [de oplossing die het meest geschikt](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Aanvullende overwegingen voor toepassingen al gebruik in

Instellen van eenmalige op voor een toepassing die al in uw organisatie gebruikmaakt van is een ander proces van het proces van het maken van nieuwe accounts voor een nieuwe toepassing. Er zijn een paar voorbereidende stappen, met inbegrip van: de identiteit van de gebruikers in de toepassing Azure AD identiteiten toewijzen en informatie over hoe gebruikers ervaart aanmelden bij een toepassing nadat deze is geïntegreerd.

> [AZURE.NOTE] Als u wilt SSO instellen voor een bestaande toepassing, moet u beschikken over globale beheerdersrechten in beide Azure AD en de SaaS-toepassing.

### <a name="mapping-user-accounts"></a>Gebruikersaccounts toewijzen

De identiteit van een gebruiker heeft meestal een unieke id die mogelijk een e-mailadres of UPN (User Principal Name). U moet koppeling (kaart) toepassings-id van elke gebruiker naar de bijbehorende Azure AD-identiteit. Er zijn verschillende manieren voor het uitvoeren van dit afhankelijk van hoe de vereiste van de verificatie van uw toepassing.

Zie [aanpassen claims uitgegeven in het SAML-token](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) en [aanpassen kenmerktoewijzingen voor het inrichten van](active-directory-saas-customizing-attribute-mappings.md)voor meer informatie over het toewijzen van toepassing identiteiten met Azure AD identiteiten.

### <a name="understanding-the-users-log-in-experience"></a>Informatie over het aanmelden ervaring van de gebruiker

Wanneer u eenmalige aanmelding voor een toepassing die al in gebruik is integreert, is het Houd er rekening mee dat de gebruikerservaring worden beïnvloed. Voor alle toepassingen starten gebruikers met hun Azure AD-referenties aan te melden. Dit kan ook worden dat ze een ander portal gebruiken moeten voor toegang tot de toepassingen.

Eenmalige aanmelding voor sommige toepassingen kan worden uitgevoerd op van de toepassing sign in interface, maar voor andere toepassingen, moet de gebruiker leest u over een centrale portal zoals ([Mijn Apps](http://myapps.microsoft.com) of [Office365](http://portal.office.com/myapps)) aan te melden. Meer informatie over de verschillende soorten eenmalige aanmelding en hun gebruikerservaringen in [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

Een andere waardevolle resource is *Suppressing gebruiker toestemming* in het artikel [Guiding ontwikkelaars](active-directory-applications-guiding-developers-for-lob-applications.md) .

## <a name="next-steps"></a>Volgende stappen


Azure AD vindt SaaS-apps die u in de App-galerie vinden voor een aantal [zelfstudies over het integreren van SaaS apps](active-directory-saas-tutorial-list.md).

Als de app is niet in de App-galerie, kunt u [toe te voegen aan de galerie met Azure AD App als een maatwerktoepassing op](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Er is veel meer informatie over alle deze problemen in de bibliotheek Azure.com, beginnend met [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Zie ook

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
