<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - definiëren strategie voor gegevensbescherming | Microsoft Azure"
    description="U kunt de strategie voor gegevensbescherming voor uw hybride identiteit oplossing om te voldoen aan de vereisten voor bedrijven die u hebt gedefinieerd definiëren."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Strategie voor gegevensbescherming voor uw hybride identiteit oplossing definiëren

In deze taak kunt u de strategie voor gegevensbescherming voor uw hybride identiteit oplossing om te voldoen aan de vereisten van de bedrijven die u hebt gedefinieerd in definiëren:

- [Vereisten voor beveiliging van gegevens bepalen](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
- [Vereisten voor beheer van webinhoud bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
- [Vereisten voor het beheer van access bepalen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
- [Vereisten voor incident antwoord bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Opties voor gegevens beveiliging definiëren
Zoals wordt uitgelegd in [bepalen directory-synchronisatie vereisten](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), zich Microsoft Azure AD kunt synchroniseren met uw Active Directory Domain Services (AD DS) on-premises implementatie. Deze integratie kan organisaties om te profiteren van Azure AD om te verifiëren gebruikersreferenties wanneer ze toegang zoekt tot bedrijfsresources. Dit kan gebeuren voor beide scenario's: gegevens rest on-premises implementatie en in de cloud.  Toegang tot gegevens in Azure AD moet gebruikersverificatie via een beveiliging token-service (STS).

Zodra geverifieerd, de UPN (User Principal Name) wordt gelezen uit het verificatietoken en de gerepliceerde partition en container overeenkomt met het domein van de gebruiker wordt bepaald. Informatie over de bestaan, de status ingeschakeld en de rol van de gebruiker wordt gebruikt door het autorisatiesysteem om te bepalen of de gevraagde toegang tot de doeltenant voor deze gebruiker is gemachtigd in deze sessie. Bepaalde geautoriseerde acties (specifiek, maak gebruiker, wachtwoorden opnieuw instellen) maken van een audittrail die kan worden gebruikt door de tenantbeheerder van een om naleving inspanningen of onderzoeken te beheren.

Zwevend gegevens uit uw on-premises implementatie-datacenter in Azure Storage via een internetverbinding niet altijd mogelijk haalbaar is vanwege het gegevensvolume van de, de beschikbaarheid van de bandbreedte of andere relevante informatie. De [Azure-opslagservice importeren/exporteren](../storage/storage-import-export-service.md) biedt een hardware-optie voor het plaatsen/ophalen van grote hoeveelheden gegevens in blobopslag. Dit kunt u vaste schijven [BitLocker zijn versleuteld](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) rechtstreeks naar een Azure datacenter waar cloud operatoren wordt de inhoud uploaden naar uw account opslag of kunnen ze uw Azure gegevens downloaden naar uw stations om terug te keren naar u verzenden. Alleen versleutelde schijven worden geaccepteerd voor dit proces (via een BitLocker-sleutel gegenereerd door de service zelf tijdens de installatie van de taak). De toets BitLocker geleverd aan Azure afzonderlijk, dus leveren afmelden bij band belangrijke delen.

Aangezien gegevens tijdens overdracht in verschillende scenario's plaatsvinden kan, is het ook relevante informatie vinden die Microsoft Azure [virtual netwerken](https://azure.microsoft.com/documentation/services/virtual-network/) gebruikt om u te isoleren tenants verkeer van elkaar, die gebruikmaakt van eenheden zoals host en Gast niveau firewalls, filteren op IP-pakket poort blokkeren en HTTPS-eindpunten. De meeste interne communicatie van Azure, inclusief infrastructuur-naar-infrastructuur en infrastructuur-naar-klant (on-premises) zijn echter ook versleuteld. Nog een belangrijke mogelijkheid is de communicatie binnen Azure datacenters; Microsoft beheert netwerken te gebruiken om te kunnen waarborgen dat geen VM kunt nabootsen of klik op het IP-adres van een ander afluisteren. TLS/SSL wordt gebruikt bij het openen van Azure Storage of SQL-Databases, of wanneer u verbinding maakt met Cloudservices. In dit geval is de klant-beheerder verantwoordelijk voor het verkrijgen van een TLS/SSL-certificaat en deze aan de tenant-infrastructuur implementeren. Gegevensverkeer verplaatsen tussen virtuele Machines in de dezelfde implementatie of tussen tenants in een enkel implementatie via Microsoft Azure Virtual Network kan worden beveiligd via versleutelde communicatieprotocollen zoals HTTPS, SSL/TLS of anderen.

Afhankelijk van hoe u de vragen in de [vereisten voor de beveiliging van bepalen gegevens](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)hebt beantwoord, moet u het volgende kunnen zijn om te bepalen hoe u wilt uw gegevens beschermen en hoe de hybride identiteit oplossing helpt u op die. De tabel bevat de opties die worden ondersteund door Azure en die beschikbaar voor elk gegevens beveiliging scenario zijn.


| Opties voor gegevens-beveiliging         | Aan de rest in de cloud | Aan de rest on-premises implementatie | Tijdens overdracht |
|---------------------------------|----------------------|---------------------|------------|
| BitLocker station versleuteling      | X                    | X                   |            |
| SQL Server databases versleutelen | X                    | X                   |            |
| VM-naar-VM-versleuteling             |                      |                     | X          |
| SSL/TLS                         |                      |                     | X          |
| VPN                             |                      |                     | X          |


>[AZURE.NOTE]
Lees [naleving door functie](https://azure.microsoft.com/support/trust-center/services/) in het [Vertrouwenscentrum in Microsoft Azure](https://azure.microsoft.com/support/trust-center/) meer informatie over de certificaten die voldoet aan elke Azure-service.
Aangezien de opties voor gegevensbescherming gebruiken een gelaagde methode, vergelijking tussen deze opties zijn niet van toepassing voor deze taak. Zorg ervoor dat u gebruikmaken van alle opties beschikbaar voor elke status die de gegevens.

## <a name="define-content-management-options"></a>Opties voor beheer van webinhoud definiëren
Een voordeel van het gebruik van Azure AD voor het beheren van een hybride identiteit-infrastructuur is dat het proces volledig doorzichtig maken vanuit het oogpunt van de eindgebruiker is. De gebruiker probeert voor toegang tot een gedeelde bron, de resource is verificatie vereist, de gebruiker moet een verificatie verzendt naar Azure AD te verkrijgen van het token en toegang tot de bron. Er gebeurt dit hele proces op de achtergrond, zonder tussenkomst van de gebruiker. Het is ook mogelijk aan een [groep](active-directory-manage-groups.md#getting-started-with-access-management) gebruikers toestemming verlenen zodat zij kunnen bepaalde veelgebruikte acties uitvoeren.

Organisaties die meestal vragen over gegevensprivacy zijn vereisen classificatie van gegevens voor de oplossing. Als de huidige on-premises implementatie-infrastructuur al voor classificatie van gegevens gebruikt is, is het mogelijk om te profiteren van Azure AD als belangrijkste opslagplaats voor gebruikers id. Een algemene hulpmiddel dat dit gebruikte on-premises voor classificatie van gegevens is heet [Gegevens classificatie Toolkit](https://msdn.microsoft.com/library/Hh204743.aspx) voor Windows Server 2012 R2. Dit hulpmiddel kunt identificeren, te classificeren en beschermen van gegevens op bestandsservers in uw persoonlijke cloud. Het is ook mogelijk om te profiteren van de [Automatische indeling van het bestand](https://technet.microsoft.com/library/hh831672.aspx) in Windows Server 2012 om dit te doen.

Als uw organisatie geen classificatie van gegevens op hun plaats staan, maar beveiligen van vertrouwelijke bestanden zonder toe te voegen nieuwe Servers moet on-premises implementatie, kunnen ze Microsoft [Azure Rights Management-Service](https://technet.microsoft.com/library/JJ585026.aspx)gebruiken.  Azure RMS versleuteling identiteit en autorisatie beleidsregels wordt gebruikt om te helpen beveiligen uw bestanden en e-mail en deze werkt op meerdere apparaten, telefoons, tablets en pc's. Omdat Azure RMS een cloudservice is, moet u er hoeft te worden geconfigureerd expliciet vertrouwensrelaties met andere organisaties voordat u beveiligde inhoud met hen delen kunt is. Als ze al een Office 365 of een Azure AD-map, wordt automatisch de samenwerking tussen verschillende organisaties ondersteund. U kunt ook de directorykenmerken Azure RMS een gemeenschappelijke identiteit ondersteuning voor uw lokale Active Directory-accounts moet, met behulp van Azure Active Directory-synchronisatie Services (AAD-synchronisatie) of Azure AD Connect synchroniseren.

Een essentieel onderdeel van het beheer van webinhoud is voor meer informatie over wie toegang heeft tot welke resource, dus de mogelijkheid van een uitgebreide logboekregistratie is belangrijk voor de oplossing. Azure AD biedt logboek met meer dan 30 dagen, waaronder:

- Wijzigingen in het lidmaatschap van de rol (ex: gebruiker toegevoegd aan de rol van globale beheerder)
- Updates van referenties (ex: wachtwoordwijzigingen)
- Domeinbeheer (ex: een aangepast domein, verwijdering van een domein verifiëren)
- Toevoegen of verwijderen van toepassingen
- Gebruikersbeheer (ex: toevoegen, verwijderen, het bijwerken van een gebruiker)
- Toevoegen of verwijderen van licenties

>[AZURE.NOTE]
Lees [Microsoft Azure beveiligings- en controle Log-beheer](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) meer informatie over de mogelijkheden van logboekregistratie in Azure wordt aangegeven.
Afhankelijk van hoe u de vragen in [bepalen beheer van webinhoud vereisten](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)hebt beantwoord, moet u het volgende kunnen zijn om te bepalen hoe u de inhoud moet worden beheerd in uw identiteit hybride oplossing wilt. Hoewel alle opties die staan weergegeven in de tabel 6 kunnen integreren met Azure AD zijn, is het belangrijk om te bepalen welke meer geschikt is voor uw zakelijke behoeften is.

| Opties voor beheer van webinhoud                                                               | Voordelen                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Nadelen                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gecentraliseerde on-premises (Active Directory Rights Management Services)                      | Volledige controle over de infrastructuur van de server die verantwoordelijk is voor de classificatie van de gegevens <br> Ingebouwde mogelijkheid in Windows Server niet nodig voor extra licentie of -abonnement <br> Kan worden geïntegreerd met Azure AD in een scenario voor hybride <br> Ondersteunt informatie rights management (IRM)-mogelijkheden in Microsoft Online services zoals Exchange Online en SharePoint Online, evenals Office 365 <br> On-premises implementatie Microsoft server-producten, zoals Exchange-Server en SharePoint Server bestandsservers waarop Windows Server en bestand classificatie-infrastructuur (FCI) ondersteunt. | Hogere, onderhoud (behouden omhoog met updates, configuratie en mogelijke upgrades), sinds IT eigenaar is van de Server <br> De infrastructuur van een server on-premises implementatie vereisen<br> Doesn'tleverage Azure mogelijkheden native                                     |
| Centrale in de cloud (Azure RMS)                                                     | Eenvoudiger kunt beheren vergeleken met de on-premises implementatie-oplossing <br> Kan worden geïntegreerd met AD DS in een scenario voor hybride <br>  Volledig geïntegreerd met Azure AD <br> Een on-premises servers om te implementeren van de service vereisen niet <br> Ondersteunt het on-premises implementatie Microsoft-serverproducten zoals Exchange-Server, SharePoint Server en bestandsservers waarop Windows Server en bestand indeling, infrastructuur (FCI) <br> IT, volledige controle over hun tenant sleutel met BYOK videomogelijkheden kan hebben.                                                                                    | Uw organisatie moet een cloud abonnement hebben dat ondersteuning biedt voor RMS <br> Uw organisatie moet een Azure AD-directory ter ondersteuning van gebruikersverificatie voor RMS                                                                                  |
| Hybride (Azure RMS geïntegreerd met, On-Premises Active Directory Rights Management Services) | Dit scenario worden de voordelen van beide, gecentraliseerde on-premises implementatie en in de cloud bij elkaar opgeteld.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Uw organisatie moet een cloud abonnement hebben dat ondersteuning biedt voor RMS <br> Uw organisatie moet een Azure AD-directory ter ondersteuning van gebruikersverificatie voor RMS, hebben <br> Een verbinding tussen Azure cloudservice is vereist en on-premises-infrastructuur |

## <a name="define-access-control-options"></a>Opties voor toegangsbeheer definiëren
Bepaalt de functies die beschikbaar zijn in Azure AD is mogelijk om in te schakelen van uw bedrijf gebruik van een identiteit centrale opslagplaats terwijl u gebruikers en partners voor eenmalige aanmelding (SSO) zoals wordt weergegeven in de onderstaande afbeelding door gebruikmaken van de verificatie, autorisatie en toegang:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Centraal beheer en volledig-integratie met andere mappen

Azure Active Directory biedt eenmalige aanmelding tot duizenden SaaS-toepassingen en on-premises implementatie van webtoepassingen. Lees de [lijst met de compatibiliteit van de Azure Active Directory federation: derden Identiteitsproviders die kunnen worden gebruikt voor het implementeren van eenmalige aanmelding](https://msdn.microsoft.com/library/azure/jj679342.aspx) artikel voor meer informatie over de eenmalige aanmelding van derden die zijn getest door Microsoft. Deze mogelijkheid kan organisatie tal van scenario's B2B terwijl de besturing van het beheer identiteits- en toegangsbeheer implementeren. Tijdens de B2B is ontwerpen proces echter moet u de verificatiemethode die worden gebruikt door de partner en controleren of deze methode wordt ondersteund door Azure begrijpen. Dit zijn momenteel ondersteund door Azure AD methoden:

- Beveiliging bevestiging Markup Language (SAML)
- OAuth
- Kerberos
- Tokens
- Certificaten


>[AZURE.NOTE] Lees [Azure Active Directory verificatieprotocollen](https://msdn.microsoft.com/library/azure/dn151124.aspx) voor meer informatie over elke protocol en de mogelijkheden ervan in Azure weten.

De ondersteuning voor Azure AD gebruikt, kunnen mobiele bedrijfstoepassingen werknemers aan te melden bij hun mobiele toepassingen met hun zakelijke Active Directory-referenties mogen de dezelfde eenvoudig Mobile Services-ervaring voor verificatie gebruiken. Met deze functie wordt Azure AD ondersteund als een identiteitsprovider in Mobile-Services samen met met de andere identiteitsproviders die wordt al ondersteund (waaronder Microsoft-Accounts, Facebook-ID, Google-ID en Twitter-ID). Als u de on-premises implementatie-apps vinden op van het bedrijf AD DS van de gebruiker-referenties gebruikt, moet de toegang van partners en gebruikers die afkomstig zijn uit de cloud transparant zijn. U kunt beheren gebruikerswachtwoord voorwaardelijke toegangsbeheer voor webtoepassingen (cloudgebaseerde), web API cloudservices van Microsoft, 3e toepassingen van derden SaaS en systeemeigen (mobiel) clienttoepassingen en de voordelen van beveiliging, controle, rapportage allemaal op één plaats. Het verdient echter dit in een niet-productieomgeving of met een beperkte tijdsduur van gebruikers te valideren.


>[AZURE.TIP] het is belangrijk dat Azure AD geen groepsbeleid zoals AD DS is vermeld. Om te kunnen afdwingen van beleid voor apparaten moet u een oplossing voor het beheer van mobiele apparaat zoals [Intune van Microsoft](https://technet.microsoft.com/library/jj676587.aspx).

Als de gebruiker is geverifieerd met Azure AD, is het belangrijk het toegangsniveau dat de gebruiker heeft deze wordt berekend. Het niveau van de toegang van de gebruiker via een resource zijn kan variëren, Azure AD kunt u een extra beveiligingslaag toevoegen door de toegang tot enkele bronnen beheren, u moet ook rekening mee houden dat de resource zelf ook een eigen toegangsbeheerlijst afzonderlijk, zoals het toegangsbeheer voor bestanden die zich bevinden in een bestandsserver bevatten kan. De onderstaande afbeelding bevat een overzicht van de niveaus van toegangsbeheer dat u kunt in een hybride scenario:

![](./media/hybrid-id-design-considerations/accesscontrol.png)


Elke interactie in het diagram weergegeven in de afbeelding X Hiermee geeft u één besturingselement scenario met cloudtoegang die kan worden beschreven met Azure AD. Hieronder hebt u een beschrijving van elk scenario:

1. voorwaardelijke toegang tot de toepassingen die worden gehost on-premises implementatie: U kunt geregistreerde apparaten gebruiken met clienttoegangsbeleid voor toepassingen die zijn geconfigureerd voor gebruik van AD FS met Windows Server 2012 R2. Zie voor meer informatie over het instellen van voorwaardelijke toegang voor on-premises implementatie, [bij het instellen van On-premises voorwaardelijke toegang tot een Azure Active Directory-apparaatregistratie](active-directory-conditional-access-on-premises-setup.md).
2. toegangsbeheer bij Azure Management Portal: Azure, heeft ook de mogelijkheid om toegang tot de beheerportal te besturen met RBAC (rol gebaseerd toegangsbeheer). Deze methode kunt het bedrijf de hoeveelheid bewerkingen die persoon uitvoeren kunt wanneer hij toegang tot Azure-beheerportal heeft beperken. Met behulp van RBAC toegang tot de portal te beheren, IT-beheerders ca Gemachtigdentoegang met behulp van de volgende manieren voor toegang beheren:

 - Op basis van een groep roltoewijzing: U kunt toegang toewijzen aan Azure AD-groepen die kunnen worden gesynchroniseerd vanuit uw lokale Active Directory. Hiermee kunt u gebruikmaken van de investeringen in bestaande die uw organisatie heeft aangebracht in tooling en processen voor het beheer van groepen. U kunt ook de functie voor het beheer van gedelegeerd group van Azure AD Premium gebruiken.
 - Rollen in Azure ingebouwd middelen: U kunt drie rollen, eigenaar, Inzender en Reader, om ervoor te zorgen dat gebruikers en groepen gemachtigd bent alleen de taken die ze nodig hebben om hun werk te doen.
 - Gedetailleerde toegang tot bronnen: U kunt rollen toewijzen aan gebruikers en groepen voor een bepaald abonnement, resourcegroep of een afzonderlijke Azure resource zoals een website of de database. Op deze manier kunt u ervoor zorgen dat gebruikers hebben toegang tot alle resources die ze nodig hebben en geen toegang tot de resources die ze niet hoeft te beheren.

>[AZURE.NOTE] Lees [Rolgebaseerd toegangsbeheer in Azure](https://azure.microsoft.com/updates/role-based-access-control-in-azure-preview-portal/) om te weten meer informatie over deze mogelijkheid. Voor ontwikkelaars die bouwen van toepassingen en wil het toegangsbeheer voor hen aanpassen, is het ook Azure AD-toepassingsrollen voor autorisatie gebruiken. Bekijk dit [WebApp-RoleClaims-DotNet voorbeeld](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) op het maken van uw app als u wilt deze mogelijkheid gebruiken.

3. voorwaardelijke toegang voor Office 365-toepassingen met Microsoft Intune: IT-beheerders voorwaardelijke apparaat clienttoegangsbeleid als u wilt beveiligen bedrijfsresources, terwijl op hetzelfde moment waardoor de informatiewerkers op compatibele apparaten voor toegang tot de services kunt inrichten. Zie [Voorwaardelijke Clienttoegangsbeleid van het type apparaat voor Office 365-services](active-directory-conditional-access-device-policies.md)voor meer informatie.

4. voorwaardelijke toegang voor Saas apps: [deze functie](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) kunt u voor het configureren van toegangsregels per toepassing meervoudige verificatie en de mogelijkheid toegang voor gebruikers niet op een vertrouwde netwerk blokkeren. U kunt de meervoudige verificatie regels toepassen op alle gebruikers die zijn toegewezen aan de toepassing, of alleen voor gebruikers binnen de opgegeven beveiligingsgroepen. Gebruikers kunnen worden uitgesloten uit de vereiste meervoudige verificatie als ze toegang krijgt tot de toepassing van een IP-adres in in van de organisatie netwerk.

Aangezien de opties voor toegangsbeheer gebruiken een gelaagde benadering, vergelijking tussen deze opties zijn niet van toepassing voor deze taak. Zorg ervoor dat u gebruikmaken van alle opties beschikbaar voor elk scenario waarvoor u toegang tot uw resources te beheren.

## <a name="define-incident-response-options"></a>Opties voor incident antwoorden definiëren
Azure AD kunt helpen IT naar identiteit mogelijke beveiligingsrisico's in de omgeving door te controleren van de gebruiker een activiteit IT kunnen gebruikmaken van Azure AD toegang en gebruik de mogelijkheid om te krijgen inzicht in de integriteit en de beveiliging van de adreslijst van uw organisatie rapporten. Met deze informatie kunt een IT-beheerder beter bepalen waar mogelijke beveiligingsrisico's mogelijk liggen zodat ze voldoende plannen kunnen deze risico's te verminderen.  [Azure AD Premium-abonnement](active-directory-get-started-premium.md) heeft een set beveiligingsrapporten die kunt inschakelen IT om deze informatie te verkrijgen. [Azure AD-rapporten](active-directory-view-access-usage-reports.md) zijn gecategoriseerd zoals hieronder wordt weergegeven:

- **Afwijking rapporten**: aanmelden gebeurtenissen die gevonden moeten afwijkende bevatten. Ons doel is om te bellen u op de hoogte van dergelijke activiteit en kunt u mogelijk te bepalen of een gebeurtenis verdacht is.
- **Rapport van de toepassing Integrated**: biedt inzichten in hoe cloud-toepassingen worden gebruikt in uw organisatie. Azure Active Directory biedt integratie met duizenden cloud-toepassingen.
- **Fout-rapporten**: fouten die optreden kunnen bij de inrichting van accounts naar externe toepassingen aangeven.
- **Gebruiker / regiospecifieke rapporten**: apparaat/teken in activiteitsgegevens voor een specifieke gebruiker weergeven.
- **Logboeken aan de activiteit**: een record van alle gecontroleerde gebeurtenissen bevatten binnen de afgelopen 24 uur, laatste zeven dagen, of laatste 30 dagen, evenals groep activiteit wijzigingen en wachtwoord opnieuw instellen en registratie activiteit.

>[AZURE.TIP]
Een ander rapport waarmee u kunt ook het Incident antwoord team werken aan een aanvraag is het rapport [gebruiker met gestolen referenties](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) .  Dit rapport oppervlakken overeenkomsten tussen deze lijst gestolen referenties en uw tenant.

Andere belangrijke ingebouwd Azure AD die kan worden gebruikt tijdens een incident antwoord onderzoek in rapporten en zijn:

- **Activiteit voor wachtwoordherstel**: de beheerder voorzien van inzichten in hoe actief wachtwoorden opnieuw instellen in de organisatie wordt gebruikt.
- **Wachtwoordherstel registratie activiteit**: biedt inzichten waarin gebruikers hun methoden voor het wachtwoord opnieuw instellen, hebben geregistreerd en welke methoden die zij hebben geselecteerd.
- **Groepsactiviteit**: biedt een overzicht van de wijzigingen aan de groep (ex: gebruikers toegevoegd of verwijderd) die in het deelvenster Access zijn gestart.

Naast de rapportage core videomogelijkheden die beschikbaar zijn in Azure AD Premium die kunnen worden gebruikt tijdens een Incident antwoord onderzoek IT kunnen ook gebruikmaken van controlerapport om informatie te verkrijgen, zoals:

- Wijzigingen in het lidmaatschap van de rol (ex: gebruiker toegevoegd aan de rol van globale beheerder)
- Updates van referenties (ex: wachtwoordwijzigingen)
- Domeinbeheer (ex: een aangepast domein, verwijdering van een domein verifiëren)
- Toevoegen of verwijderen van toepassingen
- Gebruikersbeheer (ex: toevoegen, verwijderen, het bijwerken van een gebruiker)
- Toevoegen of verwijderen van licenties

Aangezien de opties voor incident antwoord gebruiken een gelaagde methode, vergelijking tussen deze opties zijn niet van toepassing voor deze taak. Zorg ervoor dat u van alle opties beschikbaar voor elk scenario waarvoor u gebruikmaken moet Azure AD rapportage mogelijkheid gebruiken als onderdeel van een van uw bedrijf incident antwoord proces.

## <a name="next-steps"></a>Volgende stappen
[Beheertaken voor hybride identiteit bepalen](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)


## <a name="see-also"></a>Zie ook
[Ontwerp overwegingen overzicht](active-directory-hybrid-identity-design-considerations-overview.md)
