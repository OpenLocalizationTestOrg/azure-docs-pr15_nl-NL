<properties
   pageTitle="Overzicht van Azure identiteit management beveiliging | Microsoft Azure"
   description=" Microsoft identiteits- en toegangsbeheer management oplossingen help IT toegang tot toepassingen en bronnen over het zakelijke datacenter en beveiligen in de cloud, extra niveaus van gegevensvalidatie zoals meervoudige verificatie en voorwaardelijke clienttoegangsbeleid inschakelen. Dit artikel bevat een overzicht van de core Azure beveiligingsfuncties die met identiteitsbeheer helpen. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-identity-management-security-overview"></a>Azure identiteit management beveiliging-overzicht

Microsoft identiteits- en toegangsbeheer management oplossingen help IT toegang tot toepassingen en bronnen over het zakelijke datacenter en beveiligen in de cloud, extra niveaus van gegevensvalidatie zoals meervoudige verificatie en voorwaardelijke clienttoegangsbeleid inschakelen. Verdachte activiteiten tot en met geavanceerde beveiliging rapportage, controle en waarschuwingen helpt Verklein mogelijke beveiligingskwesties controleren. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) biedt eenmalige aanmelding tot duizenden cloud (SaaS)-apps en toegang tot uw WebApps die u hebt uitgevoerd van on-premises implementatie.

Voordelen van de beveiliging van Azure Active Directory (AD) omvatten de mogelijkheid te:

- Maken en beheren van de identiteit van een één voor elke gebruiker in uw bedrijf hybride synchroon houden van gebruikers, groepen en apparaten
- Eenmalige aanmelding toegang bieden tot uw toepassingen zoals duizenden vooraf geïntegreerde SaaS-apps
- Beveiliging in access toepassing inschakelen met het afdwingen van meervoudige verificatie op basis van regels beide on-premises en cloud toepassingen
- Beveiligde toegang tot on-premises implementatie-webtoepassingen via Azure AD-toepassingsproxy inrichten

Het doel van dit artikel is voor een overzicht van de core Azure beveiligingsfuncties die met identiteitsbeheer helpen. We bieden ook koppelingen naar artikelen met details van elke functie geeft zodat u kunt meer informatie.  

Het artikel ligt de nadruk op de volgende core Azure identiteit beheermogelijkheden:

- Eenmalige aanmelding
- Reverse-proxy
- Meervoudige verificatie
- Beveiliging controle, waarschuwingen en machine learning gebaseerde rapporten
- Consumenten-identiteit en toegang beheren
- Apparaatregistratie
- Bevoegdheden identiteitsbeheer
- Identiteitsbeveiliging
- Hybride identiteitsbeheer

## <a name="single-sign-on"></a>Eenmalige aanmelding

Eenmalige aanmelding (SSO) houdt toegang tot alle toepassingen en bronnen die u doen en grote ondernemingen moet, door te melden bij het gebruik van slechts één keer Eén gebruikersaccount. Wanneer aangemeld, kunt u alle gewenste zonder verificatie-toepassingen openen (bijvoorbeeld een wachtwoord typt) een tweede maal.

Als een service (SaaS)-toepassingen zoals Office 365, vak en Salesforce voor eindgebruikers productiviteit, zijn afhankelijk van de software veel organisaties. In het verleden, IT-afdeling nodig afzonderlijk maken en bijwerken van gebruikersaccounts in elke SaaS-toepassing en gebruikers hebt u er een wachtwoord voor elke toepassing SaaS onthoudt.

Azure AD doorloopt lokale Active Directory in de cloud, zodat gebruikers kunnen hun primaire organisatieaccount kunt niet alleen aanmelden bij hun apparaten domein behoren en bedrijf van resources, maar ook alle web en SaaS-toepassingen die nodig zijn voor hun taak.

Niet alleen gebruikers geen hebt voor het beheren van meerdere sets met gebruikersnamen en wachtwoorden, toepassing toegang kan worden automatisch ingerichte of maak ingerichte op basis van organisatie-groepen en hun status als werknemer. Azure AD maakt u kennis met beveiliging en toegang tot besturingselementen voor beheermodel waarmee u kunt de toegang van gebruikers centraal beheren SaaS webtoepassingen.

Meer informatie:

- [Overzicht van eenmalige aanmelding](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
- [Eenmalige aanmelding Azure Active Directory integreren met SaaS apps](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Reverse-proxy

Azure AD-toepassingsproxy kunt u publiceren on-premises implementatie-toepassingen, zoals [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) -sites, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx)en [IIS](http://www.iis.net/)-op basis van apps binnen uw privé-netwerk en biedt beveiligde toegang voor gebruikers buiten uw netwerk. Toepassingsproxy biedt externe toegang en eenmalige aanmelding (SSO) voor veel soorten on-premises implementatie-webtoepassingen met de duizenden SaaS-toepassingen die ondersteuning biedt voor Azure AD. Werknemers kunnen Meld u aan bij uw apps vanuit start op hun eigen apparaten en verifiëren via deze proxy cloudgebaseerde.

Meer informatie:

- [Azure AD-toepassingsproxy inschakelen](../active-directory/active-directory-application-proxy-enable.md)
- [Toepassingen met Azure AD-toepassingsproxy publiceren](../active-directory/active-directory-application-proxy-publish.md)
- [Enkel eenmalige aanmelding met toepassingsproxy](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Werken met voorwaardelijke toegang](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Meervoudige verificatie
Azure meervoudige verificatie (MFA) is een methode verificatie dat nodig is van het gebruik van meer dan één verificatiemethode en een kritieke tweede laag van beveiliging toegevoegd aan de gebruiker aanmeldingen en transacties. MFA helpt beschermen toegang tot gegevens en toepassingen tijdens de vergadering gebruiker aanvraag voor een eenvoudig proces aanmelden. Levert sterke verificatie via een bereik van opties voor verificatie, telefoongesprek, SMS-bericht of mobiele app melding of verificatie-code en 3e partijen OAuth tokens.

Meer informatie:

- [Meervoudige verificatie](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Wat Azure meervoudige verificatie is?](../multi-factor-authentication/multi-factor-authentication.md)
- [De werking van Azure meervoudige verificatie](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Beveiliging controle, waarschuwingen en machine learning gebaseerde rapporten

Beveiliging cmdlets voor controle en waarschuwingen en machine learning gebaseerde rapporten die inconsistente access patronen aangeven kunt u beveiligen van uw bedrijf. U kunt toegang tot Azure Active Directory- en gebruiksrapporten krijgen inzicht in de integriteit en de beveiliging van de adreslijst van uw organisatie. Met deze informatie kunt een directory-beheerder beter bepalen waar mogelijke beveiligingsrisico's mogelijk liggen zodat ze voldoende plannen kunnen deze risico's te verminderen.

Klik in de klassieke Azure portal zijn rapporten gecategoriseerd op de volgende manieren:

- Afwijking rapporten – bevatten aanmelden gebeurtenissen die we afwijkende worden gevonden. Ons doel is om te bellen u op de hoogte van dergelijke activiteit en kunt u mogelijk te bepalen of een gebeurtenis verdacht is.
- Geïntegreerde toepassing rapporten – bieden inzichten in hoe cloud-toepassingen worden gebruikt in uw organisatie. Azure Active Directory biedt integratie met duizenden cloud-toepassingen.
- Foutenrapporten – fouten die kunnen optreden bij de inrichting van accounts naar externe toepassingen aangeven.
- Rapporten van de gebruiker gedefinieerde – weergegeven apparaat/aanmelden in activiteitsgegevens voor een specifieke gebruiker.
- Een record van alle gecontroleerde gebeurtenissen bevatten activiteitenlogboeken – binnen de afgelopen 24 uur, laatste zeven dagen, of laatste 30 dagen, evenals groep activiteit wijzigingen en wachtwoord opnieuw instellen en registratie activiteit.

Meer informatie:

- [Uw toegang en gebruik rapporten weergeven](../active-directory/active-directory-view-access-usage-reports.md)
- [Aan de slag met Azure Active Directory-rapportage](../active-directory/active-directory-reporting-getting-started.md)
- [Azure Active Directory-handleiding voor rapportage](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Consumenten-identiteit en toegang beheren

Azure Active Directory-B2C is een zeer beschikbaar, globale, identiteit management service voor consumenten gerichte toepassingen die wordt aangepast aan honderden miljoenen identiteiten. Dit kan worden geïntegreerd in mobile- en web platforms. Uw consumenten kunnen aanmelden bij al uw toepassingen via aanpasbare ervaringen met behulp van hun bestaande accounts van sociale of door te maken van nieuwe referenties.

In het verleden, zou softwareontwikkelaars die wilt registreren en meld u aan consumenten in hun toepassingen hun eigen code hebt geschreven. En zou hebben gebruikt on-premises implementatie-databases of systemen voor de opslag van gebruikersnamen en wachtwoorden opnieuw instellen. Azure Active Directory-B2C biedt uw organisatie een betere manier op consumenten identiteitsbeheer integreren met toepassingen met behulp van een beveiligde, gestandaardiseerde platform en een grote verzameling extensible beleidsregels.

Wanneer u Azure Active Directory B2C gebruikt, kunnen uw consumenten registreren voor uw toepassingen met behulp van hun bestaande accounts van sociale (Facebook, Google, Amazon, LinkedIn) of door te maken van nieuwe referenties (e-mailadres en wachtwoord of gebruikersnaam en wachtwoord).

Meer informatie:

- [Wat is Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
- [Voorbeeld van Azure Active Directory-B2C: Meld u aan omhoog en meld u aan consumenten in uw toepassingen](../active-directory-b2c/active-directory-b2c-overview.md)
- [Voorbeeld van de Azure Active Directory-B2C: Typen toepassingen](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Apparaatregistratie

Azure AD-apparaatregistratie is de basis voor apparaat gebaseerde [voorwaardelijke toegang](../active-directory/active-directory-conditional-access-on-premises-setup.md) scenario's. Wanneer een apparaat is geregistreerd, biedt Azure Active Directory-apparaatregistratie het apparaat met een identiteit die wordt gebruikt voor het verifiëren van het apparaat wanneer de gebruiker zich aanmeldt. Het apparaat dat geverifieerde en de kenmerken van het apparaat, kunnen vervolgens worden gebruikt om af te dwingen voorwaardelijke-beleid voor toepassingen die worden gehost in de cloud en on-premises implementatie.

Wanneer gecombineerd met een mobiel apparaat management (MDM)-oplossing zoals Intune, worden de kenmerken van het apparaat in Azure Active Directory worden bijgewerkt met aanvullende informatie over het apparaat. Hiermee kunt u voorwaardelijke toegangsregels die toegang vanaf apparaten om te voldoen aan de standaarden voor beveiliging en naleving afdwingen maken.

Meer informatie:

- [Aan de slag met Azure Active Directory-apparaatregistratie](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [On-premises implementatie voorwaardelijke toegang tot een Azure Active Directory-apparaatregistratie instellen](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Automatische apparaatregistratie met Windows Azure Active Directory voor Windows domein behoren apparaten](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Bevoegdheden identiteitsbeheer
Azure Active Directory (AD) bevoegdheden identiteitsbeheer kunt u beheren, instellen, en uw bevoegdheden identiteiten en toegang tot uw resources in Azure AD, evenals andere Microsoft online services zoals Office 365 of Microsoft Intune.

Soms moeten gebruikers verrichten bevoegdheden in resources van Azure- of Office 365 of andere SaaS-apps. Dit betekent vaak organisaties hebben deze permanente bevoegdheden om toegang te geven in Azure AD. Dit is een groeiende beveiligingsrisico voor resources cloud gehoste omdat organisaties voldoende niet bewaken wat die gebruikers mee bezig zijn met hun beheerdersbevoegdheden. Ook als een gebruikersaccount met bevoegdheden toegang is is gehackt, die één niet naleven kan van invloed zijn op hun algehele cloud-beveiliging. Azure AD rechten identiteitsbeheer kunt u dit risico oplossen.

Azure AD rechten identiteitsbeheer kunt u:

- Zie welke gebruikers zijn Azure AD-beheerders
- Op aanvraag, "just in time" beheerderstoegang tot Microsoft Online Services, zoals Office 365 en Intune inschakelen
- Rapporten over de beheerder van access geschiedenis en wijzigingen in beheerder toewijzingen ophalen
- Meldingen over de toegang tot een bevoegdheden rol ontvangen

Meer informatie:

- [Azure AD-rechten identiteitsbeheer](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Rollen in Azure AD bevoegdheden identiteitsbeheer](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure AD bevoegdheden identiteitsbeheer: Hoe toevoegen of verwijderen van een gebruikersrol](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Identiteitsbeveiliging
Azure AD-identiteit beveiliging is een beveiligingsservice waarmee een geconsolideerde weergave in de risico's en potentiële problemen invloed zijn op de identiteit van uw organisatie. Identiteitsbeveiliging maakt gebruik van de bestaande Azure Active Directory van afwijking detectiemogelijkheden (beschikbaar via Azure AD afwijkende activiteitsrapporten), en maakt u kennis met nieuwe risico gebeurtenistypen die afwijkingen in realtime kunnen detecteren.

Meer informatie:

- [Beveiliging van Azure Active Directory-identiteit](../active-directory/active-directory-identityprotection.md)
- [Kanaal 9: Azure AD en identiteit weergeven: identiteit beveiliging Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hybride identiteitsbeheer

Aanpak van Microsoft voor identiteit betrekking hebben op on-premises implementatie en de cloud, een enkele gebruikers-id voor verificatie en autorisatie voor alle resources, ongeacht de locatie te maken.

Meer informatie:

- [Whitepaper hybride-identiteit](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Active Directory-teamblog](https://blogs.technet.microsoft.com/ad/)
