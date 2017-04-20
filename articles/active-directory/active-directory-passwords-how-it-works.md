<properties
    pageTitle="Werkwijze: Azure AD-wachtwoordbeheer | Microsoft Azure"
    description="Meer informatie over de verschillende onderdelen van Azure AD wachtwoordbeheer, inclusief waar gebruikers registreren, opnieuw instellen en hun wachtwoorden, wijzigen en waar beheerders configureren, rapporteren en beheer van de lokale Active Directory wachtwoorden inschakelen."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="how-password-management-works"></a>De werking van wachtwoordbeheer

> [AZURE.IMPORTANT] **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Wachtwoordbeheer in Azure Active Directory bestaat uit verschillende logische onderdelen die hieronder worden beschreven.  Klik op elke koppeling voor meer informatie over dat onderdeel.

- [**Wachtwoord configuratie Gegevensbeheerportal**](#password-management-configuration-portal) – verschillende aspecten van de manier waarop wachtwoorden worden beheerd in hun tenants door te schuiven naar het tabblad van de van hun map configureren in de [Beheerportal van Azure](https://manage.windowsazure.com)kunnen beheerders.
- [**Portal voor registratie van gebruiker**](#user-registration-portal) : gebruikers zichzelf kunnen registreren voor het wachtwoord opnieuw instellen door deze webportal.
- [**User wachtwoord opnieuw instellen Portal**](#user-password-reset-portal) -gebruikers kunnen hun eigen wachtwoorden met een aantal verschillende uitdagingen overeenkomstig de beleidsregels voor het opnieuw instellen van wachtwoorden beheerder worden beheerd in opnieuw instellen
- [**User wachtwoord wijzigen Portal**](#user-password-change-portal) -gebruikers kunnen hun eigen wachtwoord wijzigen op elk gewenst moment door te voeren hun oude wachtwoord en een nieuw wachtwoord met behulp van de webportal van deze selecteren
- [**Wachtwoord Management rapporten**](#password-management-reports) – beheerders kunnen bekijken en analyseren wachtwoord opnieuw instellen en registratie activiteit in hun tenant door te gaan naar de sectie 'Activiteit declareren' van de map 'Declareren' tabblad in de [Beheerportal van Azure](https://manage.windowsazure.com)
- [**Wachtwoord write-backs onderdeel van Azure AD Connect**](#password-writeback-component-of-azure-ad-connect) : beheerders kunnen desgewenst de inschakelen wachtwoord write-backs bij het installeren van Azure AD Connect om in te schakelen beheer van federatieve of wachtwoord gesynchroniseerd gebruikerswachtwoorden vanuit de cloud.

## <a name="password-management-configuration-portal"></a>Wachtwoord-beheerportal configuratie
U kunt wachtwoord informatiebeheerbeleid instellen voor een bepaalde map met behulp van de [Azure Management Portal](https://manage.windowsazure.com) door te gaan naar de sectie **Opnieuw gebruikersbeleid wachtwoord** in van de map **configureren** tabblad configureren.  Op deze configuratiepagina kunt u bepalen veel aspecten van de manier waarop wachtwoorden worden beheerd in uw organisatie, waaronder:

- In- en uitschakelen wachtwoord opnieuw instellen voor alle gebruikers in een map
- Het aantal uitdagingen instellen (één of twee) van een gebruiker moet zijn of haar wachtwoord opnieuw in te doorlopen
- Instellen voor de specifieke soorten uitdagingen die u wilt inschakelen voor gebruikers in uw organisatie uit de onderstaande opties:
 - Mobiele telefoon (een verificatiecode via een tekst of een telefoongesprek)
 - Telefoon op kantoor (een telefoongesprek)
 - Alternatieve e (een verificatiecode via e-mail)
 - Vragen over beveiliging (knowledge gebaseerde verificatie)
- Het aantal vragen stellen een gebruiker moet registreren om te kunnen gebruiken van de vragen verificatiemethode (alleen zichtbaar als beveiliging vragen zijn ingeschakeld)
- Het aantal vragen stellen een gebruiker moet opgeven tijdens opnieuw instellen voor het gebruik van de vragen verificatiemethode (alleen zichtbaar als beveiliging vragen zijn ingeschakeld)
- Gebruik vooraf blik, gelokaliseerde, beveiliging vragen die een gebruiker gebruiken kunt bij het registreren voor het wachtwoord opnieuw instellen (alleen zichtbaar als beveiliging vragen zijn ingeschakeld)
- Het definiëren van de aangepaste beveiliging vragen die een gebruiker gebruiken kunt bij het registreren voor het wachtwoord opnieuw instellen (alleen zichtbaar als beveiliging vragen zijn ingeschakeld)
- Het vereisen van gebruikers te registreren voor het wachtwoord opnieuw instellen wanneer ze naar de toepassing toegang Configuratiescherm op [http://myapps.microsoft.com](http://myapps.microsoft.com)gaan.
- Vereisen gebruikers om te bevestigen opnieuw hun eerder geregistreerde gegevens na een configureerbare aantal dagen hebt doorgegeven (alleen zichtbaar als afgedwongen registratie is ingeschakeld)
- Middel van een aangepaste helpdesk e-mailbericht of de URL die wordt weergegeven aan gebruikers geval ze problemen ondervinden hun wachtwoorden opnieuw instellen
- In- of uitschakelen van de mogelijkheid wachtwoord write-backs (wanneer wachtwoord write-backs is geïmplementeerd met AAD verbinden)
- De status van de wachtwoord write-backs-agent weergeven (wanneer wachtwoord write-backs is geïmplementeerd met AAD verbinden)
- Inschakelt en e-mailmeldingen aan gebruikers hun eigen wachtwoord opnieuw is ingesteld (gevonden in de sectie **meldingen** van de [Azure Management Portal](https://manage.windowsazure.com))
- E-mailmeldingen voor beheerders inschakelt en andere beheerders hun eigen (gevonden in de sectie **meldingen** van de [Azure-beheerportal](https://manage.windowsazure.com) wachtwoorden opnieuw instellen
- Portal huisstijl van het wachtwoord opnieuw in en wachtwoord opnieuw instellen van e-mailberichten met het logo en de naam van uw organisatie met behulp van de functie voor het aanpassen (zijn gevonden in de sectie **Directory-eigenschappen** van de [Azure-beheerportal](https://manage.windowsazure.com) huisstijl tenant

Zie meer informatie over het configureren van wachtwoordbeheer in uw organisatie, [aan de slag: Azure AD-wachtwoordbeheer](active-directory-passwords-getting-started.md).

##<a name="user-registration-portal"></a>Portal voor registratie van gebruiker
Voordat gebruikers gebruikmaken van wachtwoorden opnieuw instellen zijn, moeten de cloud-gebruikersaccounts worden bijgewerkt met de juiste verificatiegegevens om ervoor te zorgen dat deze door het juiste aantal wachtwoord opnieuw instellen uitdagingen gedefinieerd door de beheerder geven kunnen.  Beheerders kunnen ook deze verificatiegegevens van hun gebruiker namens definiëren met behulp van de Azure of Office webportals DirSync / Azure AD Connect of Windows PowerShell.

Als u liever uw gebruikers hun eigen gegevens hebt geregistreerd, wordt ook hebben echter een webpagina die gebruikers gaat u kunnen naar alleen worden aangeboden als deze informatie.  Deze pagina, kunnen gebruikers om op te geven verificatiegegevens volgens het wachtwoord opnieuw instellen van beleidsregels die zijn ingeschakeld in hun organisatie.  Nadat u deze gegevens is geverifieerd, wordt deze opgeslagen in de cloud-gebruikersaccount moet worden gebruikt voor accountherstel op een later tijdstip. Hier ziet u wat de registratie portal lijkt op:

  ![][001]

Zie voor meer informatie [aan de slag: Azure AD-wachtwoordbeheer](active-directory-passwords-getting-started.md) en [Aanbevolen procedures: Azure AD-wachtwoordbeheer](active-directory-passwords-best-practices.md).

##<a name="user-password-reset-portal"></a>Startpagina van gebruikers wachtwoord opnieuw instellen
Zodra u hebt ingeschakeld selfservice-wachtwoord opnieuw instellen van uw organisatie zelf opnieuw instellen wachtwoordbeleid en ervoor gezorgd dat uw gebruikers de juiste contactpersoongegevens in de adreslijst, gebruikers in uw organisatie kunnen hun eigen wachtwoorden automatisch vanaf een webpagina die een account voor werk of School voor het aanmelden (zoals [portal.microsoftonline.com gebruikt](https://portal.microsoftonline.com)) opnieuw. Klik op pagina's zoals de volgende gebruikers zien een **geen toegang tot uw account?** koppeling.

  ![][002]

Klikken op deze koppeling, wordt de portal van selfservice-wachtwoord opnieuw instellen starten.

  ![][003]

Zie voor meer informatie over hoe gebruikers hun eigen wachtwoorden opnieuw [aan de slag: Azure AD-wachtwoordbeheer](active-directory-passwords-getting-started.md).

##<a name="user-password-change-portal"></a>Startpagina van gebruikers wachtwoord wijzigen
Als gebruikers hun eigen wachtwoord wijzigen, kunnen ze dit doen met behulp van de portal voor het wijzigen van wachtwoord op elk gewenst moment.  Gebruikers hebben toegang tot de portal wachtwoord wijzigen via de profielpagina Access Configuratiescherm of te klikken op de koppeling 'wachtwoord wijzigen' uit in Office 365-toepassingen.  In het geval wanneer hun wachtwoorden verlopen, worden gebruikers, ook gevraagd deze automatisch wijzigen als u zich aanmeldt.

  ![][004]

In beide gevallen, als wachtwoord write-backs is ingeschakeld en de gebruiker is een gekoppeld of manier in Wachtwoordsynchronisatie worden deze gewijzigde wachtwoorden opgeslagen in uw lokale Active Directory. Hier ziet u wat het wachtwoord wijzigen portal lijkt op:

  ![][005]

Meer informatie over hoe gebruikers hun eigen lokale Active Directory-wachtwoorden kunnen wijzigen, raadpleegt u [aan de slag: Azure AD-wachtwoordbeheer](active-directory-passwords-getting-started.md).

##<a name="password-management-reports"></a>Wachtwoordbeheer-rapporten
Navigeren naar het tabblad **rapporten** en zoek onder de sectie **Activiteitenlogboeken** , ziet u twee wachtwoordbeheer rapporten: **wachtwoordherstel activiteit** en **registratie activiteit voor wachtwoordherstel**.  Met deze twee rapporten, krijgt u een weergave van gebruikers registreren voor en het gebruik van wachtwoord opnieuw in uw organisatie. Hier ziet u hoe deze rapporten eruit ziet in de [Beheerportal van Azure](https://manage.windowsazure.com):

  ![][006]

Zie voor meer informatie [inzichten ophalen: Azure AD-wachtwoordbeheer rapporten](active-directory-passwords-get-insights.md).

##<a name="password-writeback-component-of-azure-ad-connect"></a>Wachtwoord write-backs onderdeel van Azure AD Connect
Als de wachtwoorden van gebruikers in uw organisatie is afkomstig van uw on-premises omgeving (hetzij via federatie of Wachtwoordsynchronisatie), kunt u de nieuwste versie van Azure AD Connect om in te schakelen voor het bijwerken van deze wachtwoorden rechtstreeks vanuit de cloud installeren.  Dit betekent dat wanneer uw gebruikers vergeet of wilt wijzigen van hun wachtwoord AD, ze dus rechtstreeks vanaf het web doen kunnen.  Hier ziet u waar u het wachtwoord write-backs vinden in de installatiewizard Azure AD Connect:

  ![][007]

Zie voor meer informatie over Azure AD Connect [aan de slag: Azure AD Connect](active-directory-aadconnect.md). Zie voor meer informatie over wachtwoord write-backs, [aan de slag: Azure AD-wachtwoordbeheer](active-directory-passwords-getting-started.md).


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Koppelingen naar wachtwoord opnieuw in documentatie
Hieronder vindt u koppelingen naar alle pagina's documentatie Azure AD wachtwoorden opnieuw instellen:

* **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [**Aan de slag**](active-directory-passwords-getting-started.md) - Leer hoe u kunt u gebruikers opnieuw instellen en hun cloud of on-premises-wachtwoord wijzigen
* [**Aanpassen**](active-directory-passwords-customize.md) - informatie over het aanpassen van het uiterlijk en uiterlijk en gedrag van de service aan de behoeften van uw organisatie
* [**Aanbevolen procedures**](active-directory-passwords-best-practices.md) - Ontdek hoe u snel en effectief wachtwoorden in uw organisatie beheren
* [**Inzicht krijgen**](active-directory-passwords-get-insights.md) - meer informatie over onze geïntegreerde rapportagemogelijkheden
* [**Veelgestelde vragen**](active-directory-passwords-faq.md) - antwoorden op veelgestelde vragen
* [**Problemen oplossen**](active-directory-passwords-troubleshoot.md) - Leer hoe u snel problemen oplossen met de service
* [**Meer informatie**](active-directory-passwords-learn-more.md) - Ga diep op de technische details van de werking van de service



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
