<properties
   pageTitle="Richtlijnen voor toepassingen huisstijl | Microsoft Azure"
   description="Een uitgebreide handleiding voor ontwikkelaars-georiënteerd bronnen voor Azure Active Directory"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# <a name="branding-guidelines-for-applications"></a>Richtlijnen voor toepassingen huisstijl


In dit onderwerp wordt beschreven hoe de bestaande huisstijl richtlijnen die u gebruiken moet bij het ontwikkelen van toepassingen met Azure Active Directory (Azure AD). Deze richtlijnen helpen uw klanten sturen wanneer ze gebruiken hun werk- of schoolaccount-account willen, beheerd in Azure AD of persoonlijk account voor aanmelding en aanmelden bij uw toepassing.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Persoonlijke accounts versus werk- of schoolaccounts van Microsoft

Microsoft beheert twee soorten gebruikersaccounts:

- **Persoonlijke accounts** (voorheen Windows Live ID). Deze accounts vertegenwoordigen van de relatie tussen *afzonderlijke* gebruikers en Microsoft en zijn gebruikt voor toegang tot consumentenapparaten en services van Microsoft. Deze accounts zijn bedoeld voor eigen gebruik.

- **Accounts voor werk- of schoolaccount.** Deze accounts worden beheerd door Microsoft namens organisaties die gebruikmaken van Azure Active Directory. Deze accounts worden gebruikt voor het aanmelden bij Office 365 en andere bedrijven-services van Microsoft.

Microsoft werk- of schoolaccount accounts meestal aan eindgebruikers (medewerkers, studenten, federale werknemers) door hun organisaties (bedrijf, school, government Bureau toegewezen zijn). Deze accounts zijn onder de knie rechtstreeks in de cloud, in het platform Azure AD of gesynchroniseerd met Azure AD vanuit een on-premises adreslijst, zoals Windows Server Active Directory. Microsoft is de *beheerder* van de accounts voor werk of school, maar de accounts zijn eigendom en wordt beheerd door de organisatie.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Verwijzen naar Azure AD-accounts in uw toepassing

Microsoft niet eindgebruikers van de Azure of de namen van de huisstijl Active Directory en geen van beide worden, moet u.

- Nadat gebruikers bent aangemeld, moet u de naam en het logo zoveel mogelijk van de organisatie. Dit is beter dan met algemene voorwaarden zoals "uw organisatie".

- Wanneer gebruikers niet bent aangemeld, raadpleegt u op hun rekeningen als "werk of schoolaccounts ' en het gebruik van het Microsoft-logo overbrengen dat deze accounts worden beheerd door Microsoft. Niet termen gebruiken, zoals 'enterprise-account', 'bedrijven-account' of "bedrijfsaccount" die gebruiker verwarring maakt.

## <a name="user-account-pictogram"></a>Pictogram voor gebruiker-account
In een eerdere versie van deze richtlijnen, is het raadzaam een pictogram "blauwe badge" gebruiken. Afhankelijk van de gebruiker en ontwikkelaars feedback nu wordt aangeraden het gebruik van het Microsoft-logo in plaats daarvan. Hierdoor kunnen begrijpen dat ze het account dat ze gebruiken met Office 365 of een ander op Microsoft business-services te melden bij uw app opnieuw kunnen gebruiken.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Aanmelden en aanmelden met Azure AD

Uw app kan afzonderlijke paden voor aanmelding en aanmeldingsproblemen presenteren en de volgende secties bevatten visuele instructies voor beide scenario's.

**Als uw app eindgebruiker teken ondersteunt omhoog (bijvoorbeeld gratis proefversie of freemium model)**: kunt u een knop **aanmelden** waarmee gebruikers toegang hebben tot uw app met hun werkaccount of hun persoonlijke account weergeven. Azure AD wordt gevraagd toestemming of de eerste keer dat ze toegang uw app tot weergegeven.

**Als uw app vereist is voor machtigingen die alleen beheerders kunnen toestemming, of als uw app vereist is voor organisatie-licenties**: U moet beheerder acquisition gebruiker aanmelden scheiden. De **knop 'deze app krijgt'** leiden beheerders als u wilt Meld u aan en vraag of ze kunnen toestemming namens gebruikers in hun organisatie. Dit is het voordeel van eindgebruikers toestemming aanwijzingen voor uw app onderdrukken.

## <a name="visual-guidance-for-app-acquisition"></a>Visuele instructies voor de aanschaf van de app

De koppeling "de app downloaden" moet de gebruiker omleiden naar de toegang van de Azure AD-verlenen (Autoriseer) pagina, zodat de beheerder van een organisatie te machtigen uw app toegang heeft tot van hun organisatie gegevens die wordt gehost door Microsoft. Meer informatie over hoe u toegang worden besproken in het artikel [Toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md) .

Nadat u beheerders toestemming naar uw app, kunt deze toe te voegen aan de gebruikers Office 365-app startprogramma voor apps-ervaring (toegankelijk vanaf de wafel en vanaf [https://portal.office.com/myapps](https://portal.office.com/myapps)). Als u reclame maken voor deze mogelijkheid wilt, kunt u termen, bijvoorbeeld "Deze app aan uw organisatie toevoegen" en een knop als volgt weergeven:

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/add-to-my-org.png)

Echter, is het raadzaam dat u schrijft verklarende tekst in plaats van te vertrouwen op knoppen. Bijvoorbeeld:
> *Als u al gebruikt voor Office 365 of andere bedrijven-service van Microsoft, kunt u gewoon < your_app_name > toegang tot de gegevens van uw organisatie te verlenen. Hierdoor wordt uw gebruikers toegang hebben tot < your_app_name > met hun bestaande werk-accounts.*


## <a name="visual-guidance-for-sign-in"></a>Visuele instructies voor aanmelden
Uw app een (+) in de knop waarmee gebruikers worden omgeleid naar het eindpunt aanmeldingsproblemen die overeenkomt met het protocol dat u kunt integreren met Azure AD moet worden weergegeven. De volgende sectie bevat informatie op hoe die knop eruit moet.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Pictogram en "Meld u aan met Microsoft"
Dit is de koppeling van het Microsoft-logo en de bepalingen van de "Aanmelden in met Microsoft" dat Azure AD onder andere identiteitsproviders een unieke aanduiding uw app kan ondersteunen. Als er niet voldoende ruimte voor 'Aanmelden in met Microsoft', is het ok naar moet u deze "Aanmelden".

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-light.png)

U kunt ook een donker kleurenschema gebruiken voor de knoppen.

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Richtlijnen voor effectieve en niet moet doen met huisstijl

**Gebruik 'werk of school-account' in combinatie met de knop 'Aanmelden in met Microsoft' te leveren aanvullende uitleg waarmee eindgebruikers herkend of ze deze kunnen gebruiken.** Gebruik **geen** andere termen zoals 'enterprise-account', 'bedrijven-account' of "bedrijfsaccount."

Gebruik **geen** "Office 365-ID" of "Azure-ID". Office 365 is ook de naam van een consument aanbod van Microsoft Azure AD niet wordt gebruikt voor verificatie.

**Niet** de instellingen van het Microsoft-logo.

**Niet** blootgesteld eindgebruikers tot de merken Azure of Active Directory. Het is echter wel ok voor het gebruik van deze termen met ontwikkelaars, IT-professionals en beheerders.

## <a name="navigation-dos-and-donts"></a>Tips voor navigatie

**Beschikken** over een manier om gebruikers afmelden en overschakelen naar een andere gebruikersaccount. Terwijl de meeste mensen hebben een één persoonlijk account vanuit Microsoft/Facebook/Google/Twitter, zijn er vaak personen gekoppeld aan meer dan één organisatie. Ondersteuning voor meerdere gebruikers zijn aangemeld bij is binnenkort beschikbaar.
