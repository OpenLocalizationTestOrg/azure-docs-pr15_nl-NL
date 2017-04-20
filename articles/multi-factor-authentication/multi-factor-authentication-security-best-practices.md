<properties 
    pageTitle="Aanbevolen procedures voor het gebruik van Azure MFA voor beveiliging"
    description="Dit document bevat aanbevolen procedures rond het gebruik van Azure MFA met Azure-accounts"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Aanbevolen procedures voor beveiliging voor het gebruik van Azure meervoudige verificatie met Azure AD-accounts

Als het gaat om het verbeteren van uw verificatieproces, is meervoudige verificatie de voorkeur keuze voor de meeste organisaties. Azure meervoudige verificatie MFA () kunnen bedrijven om te voldoen aan de vereisten van hun beveiliging en naleving terwijl een eenvoudige aanmeldervaring voor hun gebruikers. In dit artikel wordt uitgelegd hoe enkele aanbevolen procedures waarmee u rekening houden moet bij het plannen van de goedkeuring van Azure MFA.

## <a name="best-practices-for-azure-multi-factor-authentication-in-the-cloud"></a>Aanbevolen procedures voor het Azure meervoudige verificatie in de cloud
Om te voorzien van al uw gebruikers meervoudige verificatie en voordelen van de uitgebreide functies die Azure meervoudige verificatie biedt nemen, moet u naar het inschakelen van Azure meervoudige verificatie op al uw gebruikers.  Dit is doen met behulp van een van de volgende opties:

- Azure AD Premium of de Enterprise-mobiliteit-Suite
- Meervoudige verificatie-Provider

### <a name="azure-ad-premiumenterprise-mobility-suite"></a>Azure AD Premium/Enterprise mobiliteit Suite

![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

De eerste aanbevolen stap voor gebruik Azure MFA in de cloud met Azure AD Premium of de Enterprise-mobiliteit-Suite is licenties toe te wijzen aan uw gebruikers.  Azure meervoudige verificatie maakt deel uit van deze suites en zo uw organisatie geen hoeft extra voor het uitbreiden van de mogelijkheid meervoudige verificatie voor alle gebruikers.

Meervoudige verificatie instellen wanneer overwegen volgende:

- U hoeft niet te maken van een meervoudige Auth-Provider.  Azure AD Premium en de Enterprise-mobiliteit-Suite wordt geleverd met Azure meervoudige verificatie.  Als u een Auth-Provider kan u dubbele maakt gefactureerd.
- Azure AD Connect is alleen vereist als u uw on-premises Active Directory-omgeving met een Azure AD-directory synchroniseert. Als u alleen een Azure AD-map die niet is gesynchroniseerd met een exemplaar van de on-premises implementatie van Active Directory, hoeft u niet Azure AD Connect.


### <a name="multi-factor-auth-provider"></a>Meervoudige verificatie-Provider

![Meervoudige verificatie-Provider](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Als u nog geen Azure AD Premium of de Enterprise-mobiliteit-Suite klikt u vervolgens is de eerste aanbevolen stap voor gebruik Azure MFA in de cloud om een MFA Auth-Provider te maken. Hoewel MFA standaard beschikbaar voor globale beheerders met Azure Active Directory is, wanneer u MFA voor uw organisatie implementeert moet u de mogelijkheid meervoudige verificatie voor alle gebruikers en moet u doen uitbreiden dat u een meervoudige Auth-Provider moet.
Wanneer u de Auth-Provider selecteert, moet u een map te selecteren en de volgende oorzaken hebben:

- U hoeft niet een Azure AD-directory om een meervoudige Auth-Provider te maken.
- U moet de meervoudige Auth Provider koppelen aan een Azure AD-directory als u wilt uitbreiden meervoudige verificatie naar al uw gebruikers en/of wilt uw globale beheerders kunnen om te profiteren van functies zoals de beheerportal, aangepaste begroetingen en rapporten.
- Zijn alleen vereist in als u uw on-premises Active Directory-omgeving synchroniseert met een Azure AD-directory DirSync of AAD-synchronisatie. Als u alleen een Azure AD-map die niet is gesynchroniseerd met een exemplaar van de on-premises implementatie van Active Directory gebruikt, hoeft u niet DirSync of AAD-synchronisatie.
- Als u Azure AD Premium of de Enterprise-mobiliteit-Suite hebt, hoeft u niet om een meervoudige Auth-provider te maken. U moet alleen een gebruiker een licentie toewijzen en vervolgens kunt u beginnen MFA inschakelen voor gebruikers.
- Zorg dat u het juiste gebruiksmodel voor uw bedrijf (per auth of per gebruiker ingeschakeld), nadat u het gebruiksmodel selecteert u deze niet kan wijzigen.

### <a name="user-account"></a>Gebruikersaccount
Als u MFA inschakelt voor uw gebruikers, gebruikersaccounts kunnen zijn op een van de drie core Staten: uitgeschakeld, ingeschakeld of afgedwongen.
Gebruik de onderstaande richtlijnen om te zorgen dat u gebruikt de meest geschikte optie voor uw implementatie:

- Wanneer de status van de gebruiker is ingesteld op uitgeschakeld, meervoudige verificatie geen gebruikmaakt van die gebruiker. Dit is de standaardstatus.
- Als de status van de gebruiker is ingesteld op ingeschakeld, betekent dit dat de gebruiker is ingeschakeld, maar niet het registratieproces is voltooid. Ze wordt gevraagd om het proces aan de volgende aanmelden te voltooien. Deze instelling heeft geen invloed op niet browser apps. Alle apps nog steeds werkt totdat het registratieproces is voltooid.
- Wanneer de status van de gebruiker is ingesteld op afgedwongen, betekent dit dat de gebruiker mogelijk of zijn mogelijk niet uitgevoerd registratie. Als ze het registratieproces hebt voltooid zijn ze meervoudige verificatie gebruiken. Anders wordt wordt de gebruiker gevraagd om het registratieproces bij volgende aanmelden te voltooien. Deze instelling heeft invloed op niet browser apps. Deze apps werkt pas appwachtwoorden worden gemaakt en gebruikt.
- Gebruik de sjabloon beschikbaar in het artikel [aan de slag met Azure meervoudige verificatie in de cloud](multi-factor-authentication-get-started-cloud.md) melding van gebruiker een e-mailbericht verzenden naar uw gebruikers over MFA adoptie.

### <a name="supportability"></a>Ondersteuning

Aangezien de meeste van de gebruikers die gewend zijn voor het gebruik van alleen wachtwoorden om te verifiëren, is het belangrijk dat uw bedrijf chatstatus weer voor alle gebruikers met betrekking tot dit proces. Deze status kan de kans die gebruikers contact met uw helpdesk voor secundaire met betrekking tot MFA opnemen wordt.
Er zijn echter enkele scenario's MFA tijdelijk uit te schakelen wanneer nodig is. Gebruik de onderstaande richtlijnen voor meer informatie over hoe u omgaat met de scenario's:

- Zorg ervoor dat uw ondersteuningsmedewerkers zijn training greep scenario's waarin de mobiele app of de telefoon geen een melding of telefoongesprek ontvangt en daarom die de gebruiker bevindt zich niet kunt aanmelden. Een eenmalige overslaan-optie voor het toestaan van een gebruiker om te verifiëren van één keer "overslaat" meervoudige verificatie kan worden ingeschakeld. De overslaan is tijdelijk en verloopt na het opgegeven aantal seconden.
- Zo nodig kunt u gebruikmaken van de functie vertrouwde IP-adressen in Azure MFA. Deze functie kan beheerders van een tenant beheerde of federatieve, worden omzeild meervoudige verificatie voor gebruikers die zich wilt uit lokaal intranet van het bedrijf aanmelden. De functies zijn beschikbaar voor Azure AD-tenants die Azure AD Premium, Enterprise mobiliteit Suite of Azure meervoudige verificatie-licentie hebben.


## <a name="best-practices-for-azure-multi-factor-authentication-on-premises"></a>Aanbevolen procedures voor het lokale Azure meervoudige verificatie
Als uw bedrijf besloten om te profiteren van een eigen infrastructuur voor het inschakelen van MFA, zijn deze om te implementeren van een Azure meervoudige verificatieserver on-premises. De onderdelen van MFA Server worden weergegeven in het onderstaande diagram:

![Meervoudige Auth Provider](./media/multi-factor-authentication-security-best-practices/server.png)
*niet standaard geïnstalleerd ** geïnstalleerd maar niet al dan niet standaard is ingeschakeld


Azure meervoudige verificatieserver kan worden gebruikt voor het beveiligen van cloud resources en on-premises implementatie-resources die zijn toegankelijk voor Azure AD-accounts.  Dit kunt echter alleen uitgevoerd via de Federatie.  Dat wil zeggen, moet u AD FS en hebt gekoppeld aan uw Azure AD-tenant.
Server voor meervoudige verificatie instellen wanneer overwegen volgende:

- Als u Active Directory Federation Services, met Azure AD-resources beveiligen en vervolgens de 1e factor verificatie wordt uitgevoerd on-premises AD FS gebruikt en de 2e factor wordt uitgevoerd on-premises door behouden blijft het claimen.
- Het is niet een vereiste dat de Server Azure meervoudige verificatie worden geïnstalleerd op de AD FS-federatie-server, maar de meervoudige verificatie-Adapter voor AD FS moeten worden geïnstalleerd op een Windows Server 2012 R2 met AD FS. U kunt de server installeren op een andere computer, zo lang maken als dit een ondersteunde versie is en de AD FS-adapter afzonderlijk installeren op de AD FS-federatie-server. Zie de procedure hieronder voor informatie over het installeren van de adapter afzonderlijk.
- De installatiewizard meervoudige verificatie AD FS-Adapter Hiermee maakt u een beveiligingsgroep naam PhoneFactor Admins in Active Directory en telt vervolgens de AD FS-serviceaccount van uw service Federatie aan deze groep. Het wordt aanbevolen dat u op uw domeincontroller verifiëren dat de groep PhoneFactor Admins daadwerkelijk wordt gemaakt en dat de AD FS-serviceaccount deel uit van deze groep maakt. Zo nodig de AD FS-serviceaccount toevoegen aan de groep PhoneFactor Admins op uw domeincontroller handmatig.

### <a name="user-portal"></a>User Portal
Deze portal wordt uitgevoerd in een website Internet Information Server (IIS), waarmee de mogelijkheden selfservice en biedt een volledige reeks beheermogelijkheden van de gebruiker. Gebruik de onderstaande richtlijnen voor het configureren van dit onderdeel:

- IIS 6 of hoger is vereist
- ASP.NET-v2.0.507207 moet worden geïnstalleerd en geregistreerd
- Deze server kan worden geïmplementeerd in een netwerk met een omtrek.



### <a name="app-passwords"></a>Appwachtwoorden
Als uw organisatie is gefedereerd met eenmalige aanmelding met Azure AD en u gaat Azure MFA gebruikt, klikt u vervolgens worden op de hoogte van de volgende handelingen uit bij gebruik van appwachtwoorden (Houd er rekening mee dat dit alleen voor federatieve (SSO geldt) wordt gebruikt):

- Het wachtwoord van de App is gecontroleerd door Azure AD en kunnen daarom omzeilt Federatie. Federatie wordt alleen gebruikt bij het instellen van Appwachtwoord.
- Voor federatieve (SSO) wordt de wachtwoorden van gebruikers worden opgeslagen in de organisatie-id. Als de gebruiker het bedrijf verlaat, wordt die info en organisatie-id met DirSync in realtime heeft. Account uitschakelen/verwijdering duurt maximaal 3 uur om te synchroniseren, uitschakelen/verwijdering van Appwachtwoord in Azure AD vertragen.
- On-premises implementatie beheren in Access-Client-instellingen worden niet herkend door de Appwachtwoord
- Geen on-premises implementatie-verificatie logboekregistratie / controle van de mogelijkheid is beschikbaar voor Appwachtwoord
- Meer eindgebruikers education is vereist voor de Microsoft Lync 2013-client.
- Voor bepaalde geavanceerde architectuur ontwerpen kunnen vragen om een combinatie van organisatie-gebruikersnaam en wachtwoorden app gebruiken bij het gebruik van meervoudige verificatie met clients, afhankelijk van waar ze verifiëren. Voor klanten die worden geverifieerd bij een on-premises implementatie-infrastructuur, gebruikt u een organisatie-gebruikersnaam en wachtwoord. Voor klanten die worden geverifieerd bij Azure AD, gebruikt u het appwachtwoord.
- Standaard kunnen gebruikers appwachtwoorden maken als uw bedrijf die vereist of als u toestaan dat gebruikers wilt kunnen appwachtwoord maken in sommige gevallen Zorg ervoor dat de gebruikers van de optie toestaan appwachtwoorden u zich aanmeldt bij browser-toepassingen maken is geselecteerd.

## <a name="additional-considerations"></a>Aanvullende overwegingen
Gebruik de lijst hieronder voor meer informatie over enkele aanvullende overwegingen en aanbevolen procedures voor elk onderdeel dat is geïmplementeerd on-premises implementatie:

Methode|Beschrijving
:------------- | :------------- |
[Active Directory Federation Services](multi-factor-authentication-get-started-adfs.md)|Informatie over het instellen van Azure meervoudige verificatie met AD FS.
[RADIUS-Authenticaton](multi-factor-authentication-get-started-server-radius.md)|  Informatie over het instellen en configureren van de Server Azure-MFA met straal.
[IIS-verificatie](multi-factor-authentication-get-started-server-iis.md)|Informatie over het instellen en configureren van de Azure MFA Server met IIS.
[Windows Authenticaton](multi-factor-authentication-get-started-server-windows.md)|  Informatie over het instellen en configureren van de Azure MFA Server met Windows-verificatie.
[LDAP-verificatie](multi-factor-authentication-get-started-server-ldap.md)|Informatie over het instellen en configureren van de Server Azure-MFA met LDAP-verificatie.
[Extern bureaublad Gateway en Azure meervoudige verificatieserver RADIUS gebruiken](multi-factor-authentication-get-started-server-rdg.md)|  Informatie over het instellen en configureren van de Azure MFA Server met extern bureaublad-Gateway RADIUS gebruiken.
[Synchroniseren met Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md)|Informatie over het instellen en configureren van synchronisatie tussen Active Directory en de Server Azure-MFA.
[De mobiele App-Web-Service van Azure meervoudige verificatie Server implementeren](multi-factor-authentication-get-started-server-webservice.md)|Informatie over het instellen en configureren van de Azure MFA server-webservice.
[Geavanceerde VPN-configuratie met Azure meervoudige verificatie](multi-factor-authentication-advanced-vpn-configurations.md)|Informatie over het configureren van Cisco ASA, Citrix Netscaler en Juniper/Pulse Secure VPN toestellen met LDAP of RADIUS is opgegeven.


## <a name="additional-resources"></a>Aanvullende informatie
In dit artikel worden enkele aanbevolen procedures voor Azure MFA gemarkeerd, maar er zijn andere bronnen die u ook gebruiken kunt bij het plannen van uw implementatie MFA. De onderstaande lijst bevat enkele belangrijke artikelen die u tijdens dit proces helpen kunnen:

- [Rapporten in Azure meervoudige verificatie](multi-factor-authentication-manage-reports.md)
- [Werking voor Azure meervoudige verificatie instellen](multi-factor-authentication-end-user-first-time.md)
- [Veelgestelde vragen over Azure meervoudige verificatie](multi-factor-authentication-faq.md)
