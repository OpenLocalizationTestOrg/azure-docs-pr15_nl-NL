<properties
    pageTitle="Azure AD Connect: Gebruiker aanmelden | Microsoft Azure"
    description="Azure AD Connect gebruiker aanmelden voor aangepaste instellingen."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2016"
    ms.author="billmath"/>



# <a name="azure-ad-connect-user-sign-on-options"></a>Azure AD verbinding gebruiker (+) op Opties

Azure AD Connect kan uw gebruikers aanmelden bij zowel de cloud en de on-premises bronnen, met dezelfde wachtwoorden. In dit artikel worden de belangrijkste concepten voor elke identiteitsmodel kunt u kiezen de identiteit die u wilt gebruiken voor uw Meld u aan bij Azure AD.

Als u al bekend met het model van Azure AD-identiteit bent en wilt u meer informatie over een specifieke methode, klikt u op onder op de juiste onderwerp.

* [Wachtwoordsynchronisatie](#password-synchronization)
* [Federatieve eenmalige aanmelding (met ADFS)](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)


## <a name="choosing-a-user-sign-in-method"></a>Een gebruiker aanmelden methode kiezen
Voor de meeste organisaties die alleen wilt inschakelen voor gebruiker zich bij Office 365 aanmeldt, wordt op basis van resources, SaaS-toepassingen en andere Azure AD de optie standaard wachtwoord synchronisatie aanbevolen.
Sommige organisaties, hebben echter bepaalde redenen voor het gebruik van een federatieve (+) op de optie zoals AD FS.  Hierbij:

- Uw organisatie al heeft AD FS of een 3e derden Federatie provider geïmplementeerd
- Uw beveiligingsbeleid verboden synchroniseren ogen van de cloud
- U nodig dat gebruikers naadloze SSO ondervinden (zonder extra wachtwoord wordt gevraagd) wanneer het bedrijfsnetwerk bevinden toegang krijgen tot cloud resources in domein samengevoegd machines
- U vereist aantal specifieke mogelijkheden voor die AD FS is
    - On-premises implementatie meervoudige verificatie via een externe provider of smartcards (meer informatie over providers van derden MFA voor AD FS in Windows Server 2012 R2)
    - Voorzieningen van Active Directory-integratie zoals vloeiende accountvergrendeling of AD wachtwoord en werk uur beleid
    - Voorwaardelijke toegang tot beide on-premises en cloud resources met apparaatregistratie Azure AD-join of beleidsregels voor MDM Intune

### <a name="password-synchronization"></a>Wachtwoordsynchronisatie
Wachtwoordsynchronisatie, hashes van gebruikerswachtwoorden gesynchroniseerd vanuit uw lokale Active Directory met Azure AD.  Wanneer u wachtwoorden zijn gewijzigd of opnieuw in on-premises, wordt de nieuwe wachtwoorden worden gesynchroniseerd direct met Azure AD zodat uw gebruikers altijd hetzelfde wachtwoord voor cloud resources gebruiken kunnen zoals ze on-premises implementatie doen.  De wachtwoorden worden nooit verzonden naar Azure AD en die zijn opgeslagen in Azure AD als gewone tekst.
Wachtwoordsynchronisatie kan worden gebruikt met wachtwoord terugschrijven naar het item zelf Servicewachtwoord opnieuw in Azure AD instellen inschakelen.

<center>![Cloud](./media/active-directory-aadconnect-user-signin/passwordhash.png)</center>

[Meer informatie over Wachtwoordsynchronisatie](active-directory-aadconnectsync-implement-password-synchronization.md)


### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>Gebruik een nieuw of bestaand AD FS in Windows Server 2012 R2-farm Federatie
Met federatieve aanmeldingsgegevens, uw gebruikers kunnen zich aanmelden op bij Azure AD op basis van services met hun wachtwoorden on-premises implementatie en, terwijl u op het bedrijfsnetwerk bevinden, zonder te hoeven hun wachtwoord opnieuw invoeren.  De optie Federatie met AD FS kunt u een nieuwe implementeren of geeft u een bestaande AD FS in Windows Server 2012 R2-farm.  Als u ervoor kiest om op te geven van een bestaande serverfarm, wordt Azure AD Connect de vertrouwensrelatie tussen uw farm en Azure AD zodanig configureren dat uw gebruikers kunnen zich aanmelden op.

<center>![Cloud](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>Als u wilt implementeren Federatie met AD FS in Windows Server 2012 R2, moet u het volgende

Als u een nieuwe farm implementeert:

- Een Windows Server 2012 R2-server voor de federatieserver.
- Een Windows Server 2012 R2-server voor de Web-toepassingsproxy.
- een .pfx-bestand met een SSL-certificaat voor de naam van een beoogde Federatie service, bijvoorbeeld fs.contoso.com.

Als u een nieuwe farm implementeren of een bestaande serverfarm gebruikt:

- Lokale beheerdersreferenties op uw federatieservers.
- Lokale beheerdersreferenties op alle werkgroepen (niet van het domein die zijn gekoppeld) servers waarop u wilt implementeren van de rol Web toepassingsproxy.
- De computer waarop u de wizard uitvoeren, moet mogelijk verbinding maken met andere computers waarop u wilt installeren AD FS of Web-toepassingsproxy via Windows Extern beheer.

[Configuratie van SSO met AD FS](./aad-connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) 

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Meld u aan met een eerdere versie van AD FS of een derde partij oplossing

Als u over het gebruik van een eerdere versie van AD FS (zoals AD FS 2.0) of een externe provider Federatie cloud Aanmeldingsadres al hebt geconfigureerd, kunt u de gebruiker configuratie aanmelden via Azure AD Connect overgeslagen.  Hiermee kunt u de meest recente synchronisatie en andere mogelijkheden van Azure AD Connect terwijl ze nog steeds uw bestaande oplossing voor aanmelding op.

[Azure AD derden Federatie compatibiliteit lijst](./active-directory-aadconnect-federation-compatibility.md)

## <a name="user-sign-in-and-user-principal-name-upn"></a>Aanmelding en UPN (User Principal Name)

### <a name="understanding-user-principal-name"></a>UPN lidmaatschap

In Active Directory is de standaard UPN-achtervoegsel de DNS-naam van het domein waarin gebruikersaccount hebt gemaakt. Dit is de domeinnaam die is geregistreerd als het domein van de onderneming op Internet in de meeste gevallen. U kunt echter meer UPN-achtervoegsels met Active Directory-domeinen en vertrouwensrelaties toevoegen.

De UPN van de gebruiker is van de indeling username@domai. Bijvoorbeeld voor een active directory contoso.com gebruiker John heeft mogelijk UPN john@contoso.com. De UPN van de gebruiker is gebaseerd op RFC 822. Hoewel UPN en e-mail dezelfde indeling delen, wordt de waarde van de UPN voor een gebruiker kan of mogelijk niet gelijk is aan het e-mailadres van de gebruiker.

### <a name="user-principal-name-in-azure-ad"></a>UPN in Azure AD

Azure AD Connect-wizard wordt het userPrincipalName-kenmerk gebruik of kunt u opgeven van het kenmerk (in aangepaste installatie) vanuit on-premises worden gebruikt als de UPN in Azure AD. Dit is de waarde die wordt gebruikt voor het aanmelden bij Azure AD. Als de waarde van de gebruiker UPN-kenmerk niet met een geverifieerd-domein in Azure AD overeen komt, klikt u vervolgens Azure AD wordt vervangen door een standaard. onmicrosoft.com-waarde.

Elke map in Azure Active Directory wordt geleverd met een ingebouwde domeinnaam in het formulier contoso.onmicrosoft.com waarmee u aan de slag met Azure of andere Microsoft-services. U kunt verbeteren en aangepaste domeinen gebruikt aanmeldervaring te vereenvoudigen. Voor informatie over het aangepaste domein meer namen in Azure AD en hoe u een domein te bevestigen [toevoegen de naam van uw aangepaste domein met Azure Active Directory](active-directory-add-domain.md#add-your-custom-domain-name-to-azure-active-directory)

## <a name="azure-ad-sign-in-configuration"></a>Azure AD aanmeldingsproblemen configuratie

### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD-aanmelding-configuratie met Azure AD Connect
Azure AD aanmeldervaring varieert is afhankelijk van of Azure AD mogen overeenkomen met de UPN-achtervoegsel van een gebruiker wordt gesynchroniseerd met een van de aangepaste domeinen in de adreslijst Azure AD geverifieerd. Azure AD Connect bevat hulp bij het tijdens het configureren van Azure AD aanmeldingsproblemen instellingen, zodat de gebruiker aanmeldervaring varieert in de cloud is vergelijkbaar met de on-premises implementatie. Azure AD Connect vindt u de UPN-achtervoegsels gedefinieerd voor de domeinen en probeert aan te melden ze overeenkomen met een aangepast domein in Azure AD en helpt u bij de gewenste actie die moet worden ondernomen.
De aanmeldingspagina van Azure AD lijsten uit de UPN-achtervoegsels gedefinieerd voor de lokale Active Directory en de bijbehorende status ten opzichte van elke achtervoegsel weergegeven. De statuswaarden kunnen bestaan uit een van de hieronder:

| De staat | Beschrijving | Actie nodig |
|:----|:----|:----|
|Geverifieerd| Azure AD Connect zijn aangetroffen dat domein in Azure AD overeenkomende geverifieerd. Voor dit domein alle gebruikers zich kunnen aanmelden met hun on-premises implementatie-referenties| Geen actie nodig is |
|Niet gecontroleerd| Azure AD Connect een overeenkomstige aangepaste domein in Azure AD kan vinden, maar deze niet is geverifieerd. UPN-achtervoegsel van de gebruikers van dit domein wordt gewijzigd naar standaard. onmicrosoft.com achtervoegsel na synchronisatie als het domein niet is geverifieerd. | Controleer of u het aangepaste domein in Azure AD. [Meer informatie](./active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |  
|Niet toegevoegd| Azure AD Connect kan een aangepast domein overeenkomt met de UPN-achtervoegsel niet vinden. UPN-achtervoegsel van gebruikers uit dit domein wordt gewijzigd naar standaard. onmicrosoft.com als het domein niet wordt toegevoegd en verifeid in Azure wordt aangegeven.| Toevoegen en verifiëren van een aangepast domein overeenkomt met de UPN-achtervoegsel [Informatie](./active-directory-add-domain.md)|

Azure AD-aanmeldingspagina bevat de UPN suffix(s) gedefinieerd voor de lokale Active Directory en de bijbehorende aangepaste domein in Azure AD met de huidige verificatiestatus. In de aangepaste installatie kiest, kunt u nu het kenmerk voor UPN op de aanmeldingspagina **Azure AD** selecteren.

![Azure AD-aanmeldingspagina](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

U kunt klikken op de knop vernieuwen kunt u de meest recente status van de aangepaste domeinen opnieuw ophalen van Azure AD.

###<a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>Kenmerk voor UPN selecteren in Azure AD

UserPrincipalName - het kenmerk userPrincipalName is het kenmerk gebruikers worden gebruikt wanneer ze Meld u aan bij Azure AD en Office 365. De domeinen hebt gebruikt, ook bekend als de UPN-achtervoegsel, moeten worden geverifieerd in Azure AD voordat de gebruikers worden gesynchroniseerd. Het wordt ten zeerste aanbevolen dat de standaard-kenmerk userPrincipalName. Als dit kenmerk niet geschikt is en kan niet worden gecontroleerd, dan is het mogelijk wilt selecteren van een ander kenmerk, bijvoorbeeld e-mail, als het kenmerk vasthouden van de aanmeldings-ID. Dit wordt genoemd alternatieve id De waarde van het alternatieve ID-kenmerk moet voldoen aan de standaard RFC822. Een alternatieve ID kan worden gebruikt met zowel eenmalige aanmelding (SSO) wachtwoord als Federatie SSO als de oplossing aanmelden.

>[AZURE.NOTE] Gebruik van een alternatieve ID is niet compatibel met alle werkbelasting in het Office 365. Raadpleeg [Configureren alternatieve aanmeldings-ID](https://technet.microsoft.com/library/dn659436.aspx)voor meer informatie.

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>Verschillende aangepaste domein Staten en invloed op Azure aanmeldervaring
Is het belangrijk om te begrijpen van de relatie tussen de Staten aangepaste domein in uw adreslijst Azure AD en de UPN-achtervoegsels gedefinieerd on-premises implementatie. Laat ons leest u over de verschillende mogelijke Azure aanmeldingsproblemen mogelijkheden wanneer u deze instelt sycnhronization met Azure AD Processtappen verbinden.

De onderstaande informatie, laat ons wordt ervan uitgegaan dat we betrokken zijn bij de UPN-achtervoegsel contoso.com die wordt gebruikt in de on-premises adreslijst als onderdeel van de UPN, bijvoorbeeld user@contoso.com.

###### <a name="express-settings--password-synchronization"></a>Express instellingen / Wachtwoordsynchronisatie
| De staat         | Invloed op de gebruikerservaring Azure aanmelden |
|:-------------:|:----------------------------------------|
| Niet toegevoegd | In dit geval is geen aangepast domein voor contoso.com in Azure AD-adreslijst toegevoegd. Gebruikers die UPN on-premises implementatie met achtervoegsel hebben @contoso.com, worden niet gebruikmaken van hun on-premises implementatie UPN naar aanmelden bij Azure. Ze moeten in plaats daarvan een nieuw UPN gekregen ze Azure AD door toe te voegen van het achtervoegsel voor Azure AD standaardmap gebruiken. Als er gebruikers Azure AD directory azurecontoso.onmicrosoft.com vervolgens de lokale gebruiker zijn gesynchroniseerd bijvoorbeeld user@contoso.com de UPN-krijgtuser@azurecontoso.onmicrosoft.com|
| Niet gecontroleerd | In dit geval zijn er een aangepast domein contoso.com toegevoegd in Azure AD directory, maar deze nog niet is geverifieerd. Als u verdergaan waarvan de synchronisatie van gebruikers zonder het domein te verifiëren, worden klikt u vervolgens de gebruikers toegewezen een nieuw UPN door Azure AD zoals deze hebben gedaan in het scenario 'Niet toegevoegd'.|
| Geverifieerd | In dit geval zijn er een aangepast domein contoso.com al hebt toegevoegd en is gecontroleerd in Azure AD voor de UPN-achtervoegsel. Gebruikers kunnen hun on-premises implementatie UPN, bijvoorbeeld gebruiken user@contoso.com, naar aanmelden bij Azure nadat ze zijn gesynchroniseerd met Azure AD|

###### <a name="ad-fs-federation"></a>AD FS-federatie
U kunt een Federatie maken met de standaardwaarde. onmicrosoft.com-domein in Azure AD of een niet-geverifieerde aangepaste domein in Azure AD. Wanneer u de wizard Azure AD Connect worden uitgevoerd als u een niet-geverifieerde domein maken selecteert krijgt een federatie met vervolgens Azure AD Connect u nodig records moeten worden gemaakt waar de DNS voor het domein wordt gehost. Zie voor meer informatie [hier](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Als u de aanmeldingsproblemen Gebruikersoptie als 'Federatie met AD FS' hebt geselecteerd, hebt u een aangepast domein om door te gaan met het maken van een Federatie in Azure AD. Voor onze discussie betekent dit dat we een aangepast domein contoso.com toegevoegd in Azure AD directory moet hebben.

| De staat         | Invloed op de gebruikerservaring Azure aanmelden |
|:-------------:|:----------------------------------------|
| Niet toegevoegd | Azure AD Connect kan een overeenkomstige aangepaste domein in dit geval niet vinden voor de UPN-achtervoegsel contoso.com in Azure AD-adreslijst. Moet u een aangepast domein contoso.com toevoegen als u nodig gebruikers hebt moeten aanmelden met AD FS met hun on-premises implementatie UPN zoals user@contoso.com.|
| Niet gecontroleerd | In dit geval krijgt Azure AD Connect u juiste meer informatie over hoe u uw domein in een later stadium controleren kunt|
| Geverifieerd | In dit geval kunt u verdergaan met de configuratie zonder verder niets|  

## <a name="changing-user-sign-in-method"></a>Gebruiker aanmelden methode wijzigen

U kunt de gebruiker aanmeldingsproblemen methode wijzigen van Federatie naar Wachtwoordsynchronisatie gebruik van de avaialble taken in Azure AD Connect na de eerste configuratie van Azure AD Connect met de wizard. De wizard Azure AD Connect opnieuw uitvoeren en wordt weergegeven met een lijst met taken die u kunt uitvoeren. Selecteer **gebruiker aanmeldingsproblemen van wijzigen** in de lijst met taken. 

![Wijzigen aanmelding"](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Op de volgende pagina, wordt u gevraagd naar de referenties voor Azure AD opgeven.

![Verbinding maken met Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Selecteer op de pagina **aanmelding** **Wachtwoordsynchronisatie**. Hiermee wijzigt u de adreslijst van federatieve op een beheerde abonnement.

![Verbinding maken met Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin3.png)

>[AZURE.NOTE] Als u alleen een tijdelijke schakeloptie in Wachtwoordsynchronisatie aanbrengt, controleert u of de **gebruikersaccounts niet converteren**. Niet controleren op de optie leidt tot conversie van elke gebruiker federatieve en kan het enkele uren duren.
  
## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).

Meer informatie over [Azure AD Connect: concepten ontwerpen](active-directory-aadconnect-design-concepts.md)


