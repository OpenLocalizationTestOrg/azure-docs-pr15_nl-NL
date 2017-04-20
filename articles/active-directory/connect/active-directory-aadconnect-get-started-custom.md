<properties
    pageTitle="Azure AD Connect: Aangepaste installatie | Microsoft Azure"
    description="In dit document een gedetailleerd overzicht van de opties voor aangepaste installatie voor Azure AD Connect. Gebruik deze instructies voor het installeren van Active Directory via Azure AD Connect."
    services="active-directory"
    keywords="wat Azure AD Connect is, installeert u Active Directory, vereiste onderdelen voor Azure AD"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="custom-installation-of-azure-ad-connect"></a>Aangepaste installatie van Azure AD Connect
Azure AD Connect **aangepaste instellingen** wordt gebruikt wanneer u meer opties voor de installatie wilt gebruiken. Deze worden gebruikt als er meerdere forests of als u wilt configureren, optionele functies niet worden besproken in de aangepaste installatie. Deze worden gebruikt in alle gevallen waarin de optie [**Snelle installatie**](active-directory-aadconnect-get-started-express.md) niet voldoet aan uw implementatie of de topologie.

Voordat u begint met het installeren van Azure AD Connect, zorg [Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) downloaden en voltooid de oude vereiste stappen in [Azure AD Connect: Hardware en vereisten voor](../active-directory-aadconnect-prerequisites.md). Ook voor zorgen dat u hebt accounts beschikbaar, zoals wordt beschreven in [Azure AD Connect-accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md)vereist.

Als aangepaste instellingen niet overeenkomen met uw topologie, bijvoorbeeld om af te upgraden DirSync, [gerelateerd documentatie](#related-documentation) voor andere scenario's.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Aangepaste instellingen installatie van Azure AD Connect

### <a name="express-settings"></a>Express-instellingen
Op deze pagina, klikt u op **aanpassen** om een aangepaste instellingen-installatie te starten.

### <a name="install-required-components"></a>Vereiste onderdelen installeren
Wanneer u de Synchronisatieservices installeert, laat u de sectie optionele configuratie niet inschakelt en Azure AD Connect ingesteld alles automatisch. Een exemplaar van SQL Server 2012 Express LocalDB ingesteld, de juiste groepen maken en machtigingen toewijzen. Als u wijzigen van de standaardwaarden wilt, kunt u de volgende tabel voor meer informatie over de optionele configuratie-opties die beschikbaar zijn.

![Vereiste onderdelen](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

Optionele configuratie  | Beschrijving
------------- | -------------
Een bestaande SQL-Server gebruiken | Kunt u de naam van de SQL Server en de exemplaarnaam opgeven. Kies deze optie als u al een databaseserver die u wilt gebruiken. Voer de naam van het exemplaar gevolgd door een komma en poort getal in de **Exemplaarnaam** als uw SQL-Server geen bladeren is ingeschakeld.
Een bestaande serviceaccount gebruiken | Azure AD Connect maakt standaard een lokale serviceaccount voor de Synchronisatieservices gebruiken. Het wachtwoord wordt automatisch gegenereerde en onbekend bij de installatie van Azure AD Connect persoon. Als u een externe SQL-server gebruiken of gebruik een proxy waarvoor verificatie is vereist, moet u een service-account in het domein en het wachtwoord kent. Voer in dat geval de serviceaccount gebruiken. Controleer of de gebruiker die de installatie uitvoert is een SA in SQL zodat een aanmelding voor de serviceaccount kan worden gemaakt. Zie [Azure AD Connect-accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation)
Synchronisatie van aangepaste groepen opgeven | Azure AD Connect maakt standaard vier groepen lokaal op de server wanneer de Synchronisatieservices zijn geïnstalleerd. Deze groepen zijn: beheerdersgroep, operatoren groep bladeren groep en de groep wachtwoord opnieuw instellen. U kunt uw eigen groepen hier opgeven. De groepen moeten lokaal op de server en in het domein kunnen worden gevonden.

### <a name="user-sign-in"></a>Aanmelding
Na de installatie van de vereiste onderdelen, wordt u gevraagd uw gebruikers eenmalige aanmelding methode selecteren. De volgende tabel bevat een korte beschrijving van de beschikbare opties. Voor een volledige beschrijving van de methoden aanmelden, raadpleegt u [aanmelding](../active-directory-aadconnect-user-signin.md).

![Gebruiker aanmelden](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

Optie voor eenmalige aanmelding | Beschrijving
------------- | -------------
Wachtwoordsynchronisatie | Gebruikers kunnen aanmelden bij Microsoft cloudservices, zoals Office 365, met hetzelfde wachtwoord die ze in hun on-premises netwerk gebruiken. De wachtwoorden van gebruikers zijn gesynchroniseerd met Azure AD als een wachtwoordhash en verificatie wordt weergegeven in de cloud. Zie [Wachtwoordsynchronisatie](../active-directory-aadconnectsync-implement-password-synchronization.md) voor meer informatie.
Federatie met AD FS | Gebruikers kunnen aanmelden bij Microsoft cloudservices, zoals Office 365, met hetzelfde wachtwoord die ze in hun on-premises netwerk gebruiken.  De gebruikers worden omgeleid naar hun on-premises implementatie AD FS exemplaar als u zich aanmeldt en verificatie plaatsvindt on-premises implementatie.
Niet is geconfigureerd | Geen onderdeel is geïnstalleerd en geconfigureerd. Kies deze optie als u al een 3e federatieserver van derden of een andere bestaande oplossing op hun plaats staan.

### <a name="connect-to-azure-ad"></a>Verbinding maken met Azure AD
Voer op het Verbind met Azure AD-scherm, een globale beheerder-account en wachtwoord. Als u op de vorige pagina **Federatie met AD FS** hebt geselecteerd, niet Meld u aan met een account in een domein dat u van plan bent om in te schakelen voor Federatie. Een aanbeveling is het gebruik van een account in het standaard **onmicrosoft.com** -domein, waarbij het telefoonboek van uw Azure AD wordt geleverd.

Dit account wordt alleen gebruikt voor het maken van een serviceaccount in Azure AD en wordt niet gebruikt nadat de wizard is voltooid.  
![Gebruiker aanmelden](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Als uw account van de globale beheerder MFA ingeschakeld heeft, moet u het wachtwoord nogmaals in het pop-aanmelding opgeven en de uitdaging MFA te voltooien. Waar u mogelijk een leveren een verificatiecode of een telefoongesprek starten.  
![Gebruiker MFA aanmelden](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

De hoofdbeheerderaccount kan ook [Bevoegdheden identiteitsbeheer](../active-directory-privileged-identity-management-getting-started.md) ingeschakeld bevatten.

Als u een foutbericht weergegeven en problemen met verbinding hebt, raadpleegt u [problemen oplossen met de verbinding oplossen](../active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-the-section-sync"></a>Pagina's onder de sectie synchroniseren

### <a name="connect-your-directories"></a>Verbinding maken met uw mappen
Als u wilt verbinden met uw Active Directory Domain Services, vereisten Azure AD Connect voor de referenties van een account met machtigingen. U kunt het domeinonderdeel in NetBios of FQDN, dat wil zeggen FABRIKAM\syncuser of fabrikam.com\syncuser invoeren. Dit account kan een gewone gebruikersaccount zijn, omdat deze alleen de standaard-leesmachtigingen nodig. Echter, afhankelijk van uw scenario moet u mogelijk meer machtigingen. Zie [Azure AD verbinding maken met Accounts en machtigingen](../active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account) voor meer informatie

![Verbinding maken met Directory](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure AD aanmeldingsproblemen configuratie
Deze pagina kunt u de UPN-domeinen die aanwezig zijn in on-premises implementatie bekijken AD DS en die zijn geverifieerd in Azure AD. Deze pagina kunt u het wilt gebruiken voor het userPrincipalName-kenmerk configureren.

![Niet-geverifieerde domeinen](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Bekijk elk domein gemarkeerd **Niet toegevoegd** en wordt **Niet gecontroleerd**. Zorg ervoor dat deze domeinen die u gebruikt in Azure AD zijn geverifieerd. Klik op het symbool vernieuwen wanneer u uw domeinen hebt gecontroleerd. Zie voor meer informatie, [toevoegen en controleer of het domein](../active-directory-add-domain.md)

**UserPrincipalName** - met het kenmerk userPrincipalName is de gebruikers van het kenmerk gebruiken wanneer ze zich aanmeldt bij Azure AD en Office 365. De domeinen hebt gebruikt, ook bekend als de UPN-achtervoegsel, moeten worden geverifieerd in Azure AD voordat de gebruikers worden gesynchroniseerd. Microsoft raadt aan de standaard-kenmerk userPrincipalName behouden. Als dit kenmerk niet geschikt is en kan niet worden gecontroleerd, dan is het mogelijk om te selecteren van een ander kenmerk. U kunt bijvoorbeeld e-mail selecteert als het kenmerk vasthouden van de aanmeldings-ID. Met behulp van een ander kenmerk dan userPrincipalName heet **Alternatieve ID**. De waarde van het alternatieve ID-kenmerk moet voldoen aan de standaard RFC822. Een alternatieve ID kan worden gebruikt met zowel Wachtwoordsynchronisatie als Federatie.

>[AZURE.WARNING]
Gebruik van een alternatieve ID is niet compatibel met alle werkbelasting in het Office 365. Raadpleeg [Configureren alternatieve aanmeldings-ID](https://technet.microsoft.com/library/dn659436.aspx)voor meer informatie.

### <a name="domain-and-ou-filtering"></a>Domein en organisatie-eenheid filteren
Standaard worden alle domeinen en organisatie-eenheden gesynchroniseerd. Als er zijn enkele domeinen of organisatie-eenheden u niet wilt synchroniseren met Azure AD, kunt u deze domeinen en organisatie-eenheden bijvoorbeeld deselecteren.  
![DomainOU filteren](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png) is deze pagina in de wizard Configuratie van filteren op basis van het domein. Zie [filteren op basis van een domein](../active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering)voor meer informatie.

Het is ook mogelijk dat enkele domeinen die niet bereikbaar vanwege firewall beperkingen zijn. Deze domeinen zijn standaard uitgeschakeld en er een waarschuwing.  
![Niet bereikbaar domeinen](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Als u deze waarschuwing ziet, zorgen dat deze domeinen daadwerkelijk niet bereikbaar zijn en de waarschuwing wordt verwacht.

### <a name="uniquely-identifying-your-users"></a>Een unieke aanduiding voor uw gebruikers
De overeenkomende over forests functie kunt u opgeven hoe gebruikers uit uw AD DS-forests in Azure AD worden weergegeven. Een gebruiker kan op slechts één keer tussen alle forests worden weergegeven of een combinatie van ingeschakeld en uitgeschakeld accounts hebt. De gebruiker kan ook worden weergegeven als een contactpersoon in sommige forests.

![Unieke](./media/active-directory-aadconnect-get-started-custom/unique.png)

Instelling | Beschrijving
------------- | -------------
[Gebruikers worden slechts één keer aangegeven in alle forests](../active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | Alle gebruikers worden gemaakt als afzonderlijke objecten in Azure AD. De objecten zijn geen lid van de metaverse.
[E-mail-kenmerk](../active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | Deze optie deelneemt aan gebruikers en contactpersonen als het e-kenmerk hetzelfde resultaat in verschillende forests heeft. Gebruik deze optie wanneer uw contactpersonen zijn gemaakt met GALSync.
[ObjectSID en msExchangeMasterAccountSID / msRTCSIP-OriginatorSid](../active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | Deze optie deelneemt aan een ingeschakelde gebruiker in een bos account met een uitgeschakelde gebruiker in een resource bos. Deze configuratie wordt in Exchange, een gekoppelde Postvak genoemd. Deze optie kan ook worden gebruikt als u alleen Lync gebruiken en Exchange niet aanwezig in de bos resource is.
sAMAccountName als MailNickName | Deze optie deelneemt aan de vergadering op kenmerken waar wordt naar verwachting dat de aanmeldings-ID voor de gebruiker kan worden gevonden.
Een specifiek kenmerk | Deze optie kunt u uw eigen kenmerk selecteren. **Beperking:** Zorg ervoor dat een kenmerk dat al u in de metaverse vindt kiezen. Als u een aangepaste kenmerk (niet in de metaverse) kiest, wordt de wizard niet voltooien.

**Bron anker** - met het kenmerk sourceAnchor is een kenmerk dat is onveranderlijke tijdens de levensduur van een gebruikersobject. De primaire sleutel koppelen van de on-premises implementatie-gebruiker door de gebruiker in Azure AD is. Aangezien het kenmerk kan niet worden gewijzigd, moet u plannen voor een goede kenmerk dat u wilt gebruiken. Een goede is objectGUID. Dit kenmerk is niet gewijzigd, tenzij de gebruikersaccount tussen forests/domeinen is verplaatst. In een omgeving met meerdere bos is waar u accounts tussen forests verplaatsen, een ander kenmerk moet worden gebruikt, zoals een kenmerk met het veld Werknemer-id. Voorkom kenmerken die u wilt wijzigen wanneer een persoon marries of toewijzingen wijzigen. U kunt niet gebruiken kenmerken met een @-sign, zodat e-mail en userPrincipalName kunnen niet worden gebruikt. Het kenmerk is ook hoofdlettergevoelig zodat u een object verplaatsen tussen forests, zorg er bij de hoofdletters/kleine letters behouden. Binaire kenmerken worden base64-codering, maar andere kenmerktypen blijven in de draadloos staat. Dit kenmerk is in Federatie scenario's en sommige Azure AD-interfaces, ook bekend als immutableID. Meer informatie over het bron-anker vindt u in het [ontwerpconcepten](../active-directory-aadconnect-design-concepts.md#sourceAnchor).

### <a name="sync-filtering-based-on-groups"></a>Synchronisatie filteren op basis van groepen
Functie groepen filteren, kunt u slechts een klein aantal objecten synchroniseren voor een pilot. Als u wilt gebruiken voor deze functie, door een groep te maken voor dit doel in uw lokale Active Directory. Gebruikers en groepen die moeten worden gesynchroniseerd vervolgens toevoegen aan Azure AD als directe leden. U kunt later toevoegen en verwijderen van gebruikers aan deze groep voor de lijst met objecten die aanwezig in Azure AD zijn moet onderhouden. Alle objecten die u wilt synchroniseren, moet een rechtstreeks lid van de groep. Gebruikers, groepen, contactpersonen en computers/apparaten moeten direct leden zijn. Geneste groepslidmaatschap is niet opgelost. Wanneer u een groep toevoegen als een lid alleen de groep zelf wordt toegevoegd en niet de leden.

![Synchronisatie filteren](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
Deze functie is alleen bedoeld voor de ondersteuning van een pilot. Gebruik deze niet in een volledige productie-implementatie.

In een volledige productie-implementatie, is dit het verstandig om moeilijk bij te houden van een groep met alle objecten die u wilt synchroniseren. In plaats daarvan moet u een van de manieren gebruiken in [configureren filteren](../active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Optionele onderdelen
Dit scherm kunt u de optionele functies voor uw specifieke scenario's selecteren.

![Optionele onderdelen](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
Als u momenteel DirSync of actieve Azure AD-synchronisatie hebt, hebt u een van de functies write-backs in Azure AD Connect niet geactiveerd.

Optionele onderdelen | Beschrijving
------------------- | -------------
Hybride implementatie van Exchange | De hybride implementatie van Exchange-functie kan voor het samenwerken bestaan van Exchange-postvakken beide on-premises implementatie en in Office 365. Azure AD Connect wordt gesynchroniseerd door een specifieke reeks [kenmerken](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) van Azure AD weer aan uw on-premises adreslijst.
Azure AD-app en kenmerk filteren | Doordat Azure AD-app en kenmerk filteren, kan de set gesynchroniseerde kenmerken worden aangepast. Deze optie worden twee meer pagina's met configuratie opgeteld bij de wizard. Zie [Azure AD-app en kenmerk filteren](#azure-ad-app-and-attribute-filtering)voor meer informatie.
Wachtwoordsynchronisatie | Als u een Federatie geselecteerd als de oplossing aanmelden, kunt u deze optie inschakelen. Wachtwoordsynchronisatie kan vervolgens worden gebruikt als de optie van een back-up. Zie voor meer informatie, [Wachtwoordsynchronisatie](../active-directory-aadconnectsync-implement-password-synchronization.md).
Wachtwoord write-backs | Wachtwoordwijzigingen die afkomstig zijn uit Azure AD is doordat wachtwoord write-backs terug geschreven naar uw on-premises adreslijst. Zie [aan de slag met wachtwoordbeheer](../active-directory-passwords-getting-started.md)voor meer informatie.
Groep write-backs | Als u de functie voor **Office 365-groepen** gebruikt, hebt u kunt deze groepen weergegeven in uw lokale Active Directory. Deze optie is alleen beschikbaar als u Exchange aanwezig zijn in uw lokale Active Directory hebt. Zie voor meer informatie, [groep write-backs](../active-directory-aadconnect-feature-preview.md#group-writeback).
Apparaat write-backs | Kunt u write-backs apparaatobjecten in Azure AD naar uw lokale Active Directory voor voorwaardelijke toegang scenario's. Zie [inschakelen apparaat write-backs in Azure AD Connect](../active-directory-aadconnect-feature-device-writeback.md)voor meer informatie.
Adreslijstsynchronisatie extensie kenmerk | Kenmerken die zijn opgegeven zijn doordat adreslijstsynchronisatie extensies kenmerk gesynchroniseerd met Azure AD. Zie [Directory extensies](../active-directory-aadconnectsync-feature-directory-extensions.md)voor meer informatie.

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD-app en kenmerk filteren
Als u beperken welke kenmerken wilt voor synchronisatie met Azure AD, klikt u vervolgens starten door te selecteren voor welke services u gebruikt. Als u wijzigingen in de configuratie op deze pagina maakt, wordt een nieuwe service heeft expliciet worden geselecteerd door de installatiewizard opnieuw.

![Optionele onderdelen Apps](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Deze pagina ziet op basis van de services in de vorige stap hebt geselecteerd, u alle kenmerken die worden gesynchroniseerd. Deze lijst is een combinatie van alle objecttypen wordt gesynchroniseerd. Als er enkele bepaalde kenmerken die u wilt niet gesynchroniseerd zijn, kunt u deze kenmerken bijvoorbeeld deselecteren.

![Optionele onderdelen kenmerken](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
Functionaliteit kan van invloed zijn op kenmerken verwijderen. Zie voor aanbevolen procedures en aanbevelingen, [kenmerken worden gesynchroniseerd](../active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).

### <a name="directory-extension-attribute-sync"></a>Adreslijstsynchronisatie extensie kenmerk
U kunt het schema in Azure AD uitbreiden met aangepaste kenmerken die zijn toegevoegd door uw organisatie of andere kenmerken in Active Directory. Deze functie wilt gebruiken, selecteer u **Directory extensie kenmerk synchroniseren** op de pagina **Optionele onderdelen** . U kunt meer kenmerken synchroniseren op deze pagina selecteren.

![Directory-extensies](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Zie [Directory extensies](../active-directory-aadconnectsync-feature-directory-extensions.md)voor meer informatie.

## <a name="configuring-federation-with-ad-fs"></a>Federatie met AD FS configureren
AD FS configureren met Azure AD Connect is eenvoudig met een paar muisklikken. Het volgende is voordat u de configuratie vereist.

- Een Windows Server 2012 R2-server voor de federatieserver met extern beheer ingeschakeld
- Een Windows Server 2012 R2-server voor de Web Application-proxyserver met extern beheer ingeschakeld
- Een SSL-certificaat voor de naam van de service Federatie die u wilt gebruiken (bijvoorbeeld sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>Minimumvereisten van de AD FS-configuratie
Als u wilt uw AD FS-farm met Azure AD Connect hebt geconfigureerd, Controleer dat WinRM is ingeschakeld op de externe servers. Daarnaast, leest u over de vereiste poorten is vermeld in de [tabel 3 - Azure AD Connect en Federatie Servers/WAP](../active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap).

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Maak een nieuwe AD FS-farm of gebruik van een bestaande AD FS-farm
U kunt een bestaand AD FS-farm of u kunt kiezen om een nieuwe AD FS-farm te maken. Als u een nieuw account te maken, moet u de SSL-certificaat te bieden. Als het SSL-certificaat met een wachtwoord is beveiligd, wordt u gevraagd om het wachtwoord.

![AD FS-Farm](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Als u besluit om het gebruik van een bestaande AD FS-farm, komt u rechtstreeks naar het configureren van de vertrouwensrelatie tussen AD FS en Azure AD-scherm.

### <a name="specify-the-ad-fs-servers"></a>Geef de AD FS-servers
Voer de servers die u AD FS wilt op installeren. U kunt een of meer servers op basis van uw planning behoeften capaciteit toevoegen. Deelnemen aan alle servers met Active Directory voordat u deze configuratie uitvoeren. Het is raadzaam een één AD FS-server voor testen en pilot implementaties installeren. Voeg vervolgens en implementeren van meer servers uw schaal behoeften door Azure AD Connect opnieuw na de eerste configuratie uit te voeren.

>[AZURE.NOTE]
Zorg ervoor dat alle servers zijn gekoppeld aan een AD-domein voordat u deze configuratie doet.

![AD FS-Servers](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Geef de Web Application-proxyservers
Voer de servers die u wilt instellen als uw proxyservers webtoepassing. De webserver voor de proxy van toepassing is geïmplementeerd in uw DMZ (extranet omlaag) en verificatie aanmeldingsaanvragen van het extranet ondersteunt. U kunt een of meer servers op basis van uw planning behoeften capaciteit toevoegen. Het is raadzaam om één toepassing proxy webserver voor testen en pilot implementaties installeren. Voeg vervolgens en implementeren van meer servers uw schaal behoeften door Azure AD Connect opnieuw na de eerste configuratie uit te voeren. Het is raadzaam om met een equivalent aantal proxyservers om te voldoen aan de verificatie van het intranet.

>[AZURE.NOTE]
<li> Als het account dat u gebruikt een lokale beheerder op de AD FS-servers is, wordt u gevraagd voor beheerdersreferenties.</li>
<li> Zorg ervoor dat er HTTP-/ HTTPS verbinding tussen de Azure AD Connect-server en de Web Application-proxyserver is voordat u deze stap uitvoert.</li>
<li> Zorg ervoor dat er HTTP-/ HTTPS-verbinding tussen de webserver-toepassing en de AD FS-server toe te staan dat verificatieaanvragen voor mailstroom via is.</li>

![WebApp](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

U wordt gevraagd om in te voeren referenties zodat de webserver van de toepassing kan een beveiligde verbinding met de AD FS-server maken. Deze referenties moeten een lokale beheerder op de AD FS-server.

![Proxy](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Geef de serviceaccount voor de AD FS-service
De AD FS-service vereist een domein-account om te verifiëren van gebruikers en gebruikersgegevens opzoeken in Active Directory. Twee soorten serviceaccounts kan worden ondersteund:

- **Groep beheerde serviceaccount** - geïntroduceerd in Active Directory Domain Services met Windows Server 2012. Dit type account biedt services, zoals AD FS, één account zonder dat u nodig hebt bij het accountwachtwoord regelmatig. Gebruik deze optie als u al Windows Server 2012-domeincontrollers in het domein dat uw AD FS-servers behoort.
- **Domein-gebruikersaccount** - dit type account moet u een wachtwoord en het wachtwoord regelmatig bij te werken wanneer het wachtwoord gewijzigd of verloopt. Gebruik deze optie alleen wanneer u geen Windows Server 2012-domeincontrollers in het domein dat uw AD FS-servers behoort.

Als u de groep beheerde serviceaccount hebt geselecteerd en deze functie nooit in Active Directory gebruikt is, wordt u gevraagd om Enterprise beheerdersreferenties. Deze referenties worden gebruikt voor het starten van de belangrijkste store en inschakelen van de functie in Active Directory.

![AD FS-serviceaccount](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Selecteer het Azure AD-domein dat u wilt een Federatie vormen
Deze configuratie wordt gebruikt voor het instellen van de relatie federatie tussen AD FS en Azure AD. Dit probleem beveiligingstokens naar Azure AD AD FS configureert en Azure AD om te vertrouwen de tokens afgeleid van dit specifieke exemplaar van de AD FS configureert. Deze pagina kunt u alleen één domein configureren in de eerste installatie. U kunt meer domeinen later configureren door Azure AD Connect opnieuw uit te voeren.

![Azure AD-domein](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Verifieer het domein Azure AD is geselecteerd voor Federatie
Wanneer u het domein is federatief selecteert, vindt Azure AD Connect u benodigde informatie om een niet-geverifieerde domein te bevestigen. Zie [toevoegen en controleer of het domein](../active-directory-add-domain.md) voor het gebruik van deze gegevens.

![Azure AD-domein](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
AD Connect wordt geprobeerd het domein te verifiëren in de fase configureren. Als u nog configureren zonder dat u de benodigde DNS-records toevoegt, is de wizard niet voltooien van de configuratie.

## <a name="configure-and-verify-pages"></a>Configureer en controleer of pagina 's
De configuratie gebeurt op deze pagina.

>[AZURE.NOTE]
Voordat u installatie verdergaat en als u Federatie hebt geconfigureerd, zorg dat u [naamresolutie voor federatieservers](../active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers)hebt geconfigureerd.

![Klaar om te configureren](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Tijdelijke modus
Het is mogelijk om een nieuwe synchronisatieserver parallel met tijdelijke modus in te stellen. Dit wordt alleen ondersteund als u wilt dat één synchronisatieserver exporteren naar een map in de cloud. Maar als u wilt verplaatsen van een andere server, bijvoorbeeld een actieve DirSync, klikt u vervolgens kunt u inschakelen Azure AD Connect in de modus waarin u tijdelijk opslaat. Wanneer ingeschakeld, wordt de synchronisatie-engine importeren en gegevens als normale synchroniseren, maar deze exporteert niet alles naar Azure AD of AD. De functies wachtwoord synchroniseren en het wachtwoord write-backs zijn uitgeschakeld in de modus waarin u tijdelijk opslaat.

![Tijdelijke modus](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

In de modus tijdelijk opslaan, is het mogelijk te vereiste wijzigingen aanbrengen in de synchronisatie-engine en bekijk wat wordt worden geëxporteerd. Wanneer de configuratie er goed uitziet, voer de installatiewizard opnieuw en tijdelijk opslaan-modus uitschakelen. Gegevens worden nu geëxporteerd naar Azure AD van deze server. Zorg ervoor dat de andere server op hetzelfde moment dus slechts één server is actief exporteren uit te schakelen.

Zie [tijdelijke modus](../active-directory-aadconnectsync-operations.md#staging-mode)voor meer informatie.

### <a name="verify-your-federation-configuration"></a>Controleer de configuratie Federatie
Azure AD Connect wordt gecontroleerd met de DNS-instellingen voor u wanneer u op de knop controleren klikt.

![Voltooien](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Controleer of](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Bovendien de volgende verificatie-stappen uitvoeren:

- U kunt aanmelden via een browser van een domein gekoppelde computer op het intranet valideren: verbinding maken met https://myapps.microsoft.com en controleer of het aanmelden met uw account is aangemeld. Het ingebouwde AD DS-beheerdersaccount wordt niet gesynchroniseerd en kan niet worden gebruikt voor verificatie.
- Valideren dat u vanaf een apparaat uit het extranet kunt aanmelden. Klik op een computer thuis of een mobiel apparaat, verbinding maken met https://myapps.microsoft.com en uw referenties.
- Valideer de aanmeldingsproblemen rich client. Verbinding maken met https://testconnectivity.microsoft.com, kies het tabblad **Office 365** en kies de **Office 365 eenmalige aanmelding Test**.

## <a name="next-steps"></a>Volgende stappen
Nadat de installatie is voltooid, meld u af en meld u opnieuw aan bij Windows voordat u synchronisatie servicebeheer of synchronisatie regel Editor gebruiken.

Nu dat u Azure AD Connect geïnstalleerd hebt kunt u [controleren of de installatie en licenties toewijzen](../active-directory-aadconnect-whats-next.md).

Meer informatie over deze functies, die zijn ingeschakeld met de installatie: [voorkomen dat per ongeluk wordt verwijderd](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) en [Azure AD verbinding status](../active-directory-aadconnect-health-sync.md).

Meer informatie over deze algemene onderwerpen: [scheduler en hoe u synchronisatie activeren](../active-directory-aadconnectsync-feature-scheduler.md).

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Gerelateerde documentatie

Onderwerp |  
--------- | ---------
Azure AD Connect-overzicht | [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](../active-directory-aadconnect.md)
Installeren met Express-instellingen | [Installatie van Azure AD Connect Express](active-directory-aadconnect-get-started-express.md)
Upgrade van DirSync | [Een upgrade uitvoert vanuit Azure AD-synchronisatie (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Accounts die worden gebruikt voor de installatie | [Meer informatie over Azure AD Connect-accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md)
