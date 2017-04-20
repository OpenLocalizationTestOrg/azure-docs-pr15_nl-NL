<properties
    pageTitle="Azure Active Directory B2C: Veelgestelde vragen | Microsoft Azure"
    description="Veelgestelde vragen over Azure Active Directory B2C"
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
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>Azure Active Directory B2C: veelgestelde vragen

Deze pagina vindt u antwoorden op veelgestelde vragen over de B2C Azure Active Directory (Azure AD). Gecontroleerd regelmatig op updates.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Kan ik Azure AD B2C functies gebruiken in mijn bestaande, op basis van een werknemer Azure AD-tenant?

Momenteel kunnen niet Azure AD B2C functies worden ingeschakeld in uw bestaande Azure AD-tenant. Het is raadzaam dat u een aparte tenant Azure AD B2C als functies wilt gebruiken voor het beheren van uw consumenten maken.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Kan ik Azure AD B2C gebruiken om te leveren sociale login (Facebook en Google +) in Office 365?

Azure AD B2C kunnen niet worden gebruikt met Microsoft Office 365. Deze kan niet in het algemeen, worden gebruikt voor verificatie naar SaaS apps (Office 365, Salesforce, werkdag, enzovoort). Het identiteits-en access biedt alleen voor consumenten gerichte web en mobiele toepassingen en is niet van toepassing op werknemer of partner scenario's.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Wat zijn lokale accounts in Azure AD B2C? Hoe zijn ze anders uit dan accounts voor werk- of schoolaccount in Azure AD?

In een Azure AD-tenant, elke gebruiker in de tenant (behalve de gebruikers met bestaande Microsoft-accounts) zich aan met een e-mailadres van het formulier `<xyz>@<tenant domain>`, waarbij `<tenant domain>` is een van de geverifieerde domeinen in de tenant of de eerste `<...>.onmicrosoft.com` domein. Dit type account is een account voor werk- of schoolaccount.

In een tenant Azure AD B2C meeste apps wilt dat de gebruiker aan te melden met een willekeurige e-mailadres (bijvoorbeeld joe@comcast.net, bob@gmail.com, sarah@contoso.com, of jim@live.com). Dit type account is een lokale account. Vandaag, wordt ook ondersteund willekeurige gebruikersnamen (gewoon tekenreeksen) als lokale accounts (bijvoorbeeld Jan, Stefan, sarah of jim). U kunt een van deze twee typen van de lokale account kiezen in de service Azure AD B2C.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Welke sociale identiteitsprovider ondersteund nu? Welke wilt u in de toekomst wordt ondersteund?

Wordt Facebook, Google + LinkedIn en Amazon momenteel ondersteund. We wordt ondersteuning voor andere populaire sociale identiteitsprovider op basis van de vraag toegevoegd.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Kan ik bereiken als u wilt meer informatie over consumenten Verzamel van verschillende sociale identiteitsprovider configureren?

Nee, maar deze functie is op onze routekaart. De standaard-bereiken gebruikt voor onze ondersteunde set sociale identiteitsproviders zijn:

- Facebook: e-mailbericht
- Google +: e-mailbericht
- Microsoft-account: openid e-mailprofiel
- Amazon: profiel
- LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Heeft mijn toepassing moet worden uitgevoerd op Azure voor het werken met Azure AD B2C?

Nee, kunt u uw toepassing overal (in de cloud of on-premises) hosten. Alle deze nodig hebt om te communiceren met Azure AD B2C is de mogelijkheid om te verzenden en ontvangen van HTTP-aanvragen op openbaar toegankelijke eindpunten.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Ik heb meerdere Azure AD B2C-Tenants. Hoe beheer ik ze op de Portal Azure?

Elke B2C van Azure AD-tenant heeft een eigen blade B2C-functies van de Azure-portal. Zie [Azure AD B2C: uw toepassing registreren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) voor meer informatie over hoe u kunt navigeren naar een specifieke tenant B2C functies blade op de Azure-portal. Schakelen tussen Azure AD B2C mappen op de Azure-portal, wordt uw B2C functies blade openen in de meeste browsers niet behouden.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Hoe kan ik verificatie-e-mailberichten aanpassen (de inhoud en de "van:" veld) door Azure AD B2C verzonden?

U kunt de [bestaande huisstijl bedrijf-functie](../active-directory/active-directory-add-company-branding.md) gebruiken om aan te passen van de inhoud van de verificatie-e-mailberichten. Specifieke taken kunnen deze twee elementen van het e-mailbericht worden aangepast:

- **Banner-Logo**: weergegeven in de rechterbenedenhoek.
- **Achtergrondkleur**: bovenaan.

    ![Schermafbeelding van een aangepaste verificatie-e-mailadres](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Het e-mailhandtekening bevat de B2C-tenant-naam die u hebt opgegeven toen u eerst de tenant B2C hebt gemaakt. U kunt de naam van deze instructies kunt wijzigen:

- Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/) als de beheerder van abonnement.
- Ga naar uw B2C-tenant.
- Klik op het tabblad **configureren** .
- Het veld **naam** onder de sectie **Eigenschappen van map** wijzigen.
- Klik op **Opslaan** onder aan de pagina.

Er is momenteel geen manier om de "van: ' op het e-mailbericht. Als u geïnteresseerd in deze mogelijkheid en bent in de hoofdtekst van het e-mailbericht voor de verificatie volledig aan te passen, stemmen voor de functie op [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails).

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Hoe kan ik migreren mijn bestaande gebruikersnamen en wachtwoorden, profielen van mijn database naar Azure AD B2C?

U kunt de API Azure AD Graph uw Migratiehulpmiddel schrijven. Zie de [steekproef Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) voor meer informatie. We bieden verschillende opties voor de migratie en hulpmiddelen voor out-van-het-box in de toekomst.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Welke wachtwoordbeleid wordt gebruikt voor lokale accounts in Azure AD B2C?

Het beleid voor het wachtwoord van Azure AD B2C voor lokale accounts is gebaseerd op het beleid voor Azure AD. Azure AD B2C bevindt zich registreren, aanmelding of aanmelden en het wachtwoord opnieuw instellen van beleidsregels voor gebruik "sterk" Wachtwoordsterkte en niet verlopen wachtwoorden. Lees het [Azure AD-wachtwoordbeleid](https://msdn.microsoft.com/library/azure/jj943764.aspx) voor meer informatie.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Kan ik Azure AD Connect gebruiken om te migreren van consumenten identiteiten die zijn opgeslagen op mijn lokale Active Directory naar Azure AD B2C?

Nee, Azure AD Connect is niet ontworpen voor gebruik met Azure AD B2C. We bieden verschillende opties voor de migratie en hulpmiddelen voor out-van-het-box in de toekomst.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure AD B2C met werkt zoals Microsoft Dynamics CRM-systemen?

Momenteel niet. Integratie van deze systemen ligt op onze routekaart.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Azure AD B2C heeft werken met SharePoint on-premises implementatie 2016 of eerder?

Momenteel niet. Azure AD B2C heeft geen ondersteuning voor SAML 1.1 tokens die portals en e-commerce-toepassingen die zijn gebaseerd op SharePoint on-premises implementatie nodig. Houd er rekening mee dat Azure AD B2C niet is bedoeld voor de SharePoint extern delen van de partner scenario; Zie [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) in plaats daarvan.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Moet ik Azure AD B2C of B2B gebruiken voor de externe identiteiten beheren?

Lees dit artikel over [externe identiteiten](../active-directory/active-directory-b2b-compare-external-identities.md) voor meer informatie over de juiste functies toepassen op uw externe identiteit scenario's.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Welke rapporteren en controle functies Azure AD B2C biedt? Zijn ze in Azure AD Premium?

Nee, Azure AD B2C ondersteunt geen dezelfde set rapporten als Azure AD Premium. Azure AD B2C brengt eenvoudige rapportage en API's snel controle.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Kan ik de gebruikersinterface van pagina's served door Azure AD B2C localize? Welke talen worden ondersteund?

Azure AD B2C is op dit moment geoptimaliseerd voor Engels het alleen. We wilt lokalisatie functies zo snel mogelijk implementeren.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Kan ik mijn eigen URL's gebruiken op mijn aanmeldings- en aanmeldingsproblemen-pagina's die worden verwerkt in Azure AD B2C? Bijvoorbeeld, kan ik de URL van login.microsoftonline.com naar login.contoso.com wijzigen?

Momenteel niet. Deze functie is op onze routekaart. Houd er ook rekening mee dat verifiëren van uw domein op het tabblad **domeinen** van de tenant van de Azure klassieke portal wordt dit niet.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Hoe verwijder ik mijn B2C van Azure AD-tenant?

Volg deze stappen om uw tenant Azure AD B2C verwijderen:

- Volg deze stappen om te [Navigeren naar het blad B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure-portal.
- Ga naar de bladen **toepassingen**, **identiteitsprovider** en **alle beleidsregels** en de vermeldingen in elk van deze verwijderen.
- Nu Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/) als de beheerder van abonnement. (Dit is de dezelfde werk- of schoolaccount of hetzelfde Microsoft-account dat u gebruikt om u te registreren voor Azure).
- Ga naar de Active Directory-extensie aan de linkerkant en klik op uw tenant B2C.
- Klik op het tabblad **gebruikers** .
- Selecteer elke gebruiker in inschakelen (uitsluiten de gebruiker die u bent aangemeld als, dat wil zeggen het abonnement beheerder). Klik op **verwijderen** onder aan de pagina en klik op **Ja** wanneer u wordt gevraagd.
- Klik op het tabblad **toepassingen** .
- Selecteer **toepassingen mijn bedrijf eigenaar is** in het veld vervolgkeuzelijst **weergeven** en klik op het vinkje.
- Hier ziet u een toepassing **b2c-extensies-app** onderstaande genoemd. Klik op **verwijderen** onder aan de pagina en klik op **Ja** wanneer u wordt gevraagd.
- Ga naar de Active Directory-extensie opnieuw en selecteert u uw B2C-tenant.
- Klik op **verwijderen** onder aan de pagina. Volg de instructies op het scherm om het proces voltooien.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Kan ik Azure AD B2C als onderdeel van Enterprise mobiliteit Suite krijgen?

Nee, Azure AD B2C een pay-as-you-go Azure-service is en geen deel uitmaakt van Enterprise mobiliteit Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Hoe meld ik problemen met Azure AD B2C?

Zie [ondersteuning aanvragen voor Azure Active Directory B2C-bestand](active-directory-b2c-support.md).

## <a name="more-information"></a>Meer informatie

U kunt ook handig te huidige [servicebeperkingen, beperkingen, en beperkingen](active-directory-b2c-limitations.md).
