<properties
    pageTitle="Toepassingen met Azure Active Directory beheren | Microsoft Azure"
    description="Dit artikel de voordelen van de Azure Active Directory integratie met on-premises implementatie, cloud en SaaS-toepassingen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>Toepassingen met Azure Active Directory beheren

Bedrijven hebben dan de werkelijke werkstroom of de inhoud, twee basisvereisten voor alle toepassingen:

1. Als u wilt vergroten productiviteit, moeten toepassingen gemakkelijk te vinden en openen

2. Schakel beveiligings- en governance door de organisatie nodig heeft beheer en supervisie op wie kan en daadwerkelijk is toegang tot elke toepassing

In de wereld van cloud-toepassingen kan dit best worden bereikt met identiteit aan een besturingselement voor "*WIE kan wat doen*".

In het computing terminologie:

- *Wie* is bekend als *identiteit* - met het beheer van gebruikers en groepen

- *Wat* is bekend als *toegangsbeheer* – het beheer van toegang tot beveiligde bronnen

Beide onderdelen samen worden genoemd *identiteits- en Access Management (IAM)*, die is gedefinieerd door de groep [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) als "*de beveiliging discipline waarmee de juiste personen voor toegang tot de juiste resources aan de rechterkant tijden de redenen waarom*".

OK, dus wat is het probleem? Als IAM *niet beheerd* op één locatie met een geïntegreerde oplossing is:

- Identiteit beheerders hebben afzonderlijk maken en bijwerken van gebruikersaccounts in alle toepassingen afzonderlijk, een activiteit in een overtollige en tijd in beslag nemen.

- Gebruikers hebben te onthouden meerdere referenties voor toegang tot de toepassingen die ze nodig hebben om te werken met. Hierdoor kunnen vaak gebruikers Noteer hun wachtwoorden of andere oplossingen voor het beheer van wachtwoord waarin maakt u kennis met andere gegevens beveiligingsrisico's gebruiken.

- Overtollige tijd in beslag nemen activiteiten Verminder de hoeveelheid tijd gebruikers en beheerders werken zakelijke activiteiten die van uw bedrijf onder lijn vergroten.

Ja, wat in het algemeen voorkomt dat organisaties van het goedkeuren van geïntegreerde IAM oplossingen?

- Meest technische oplossingen zijn gebaseerd op de platforms voor software die moeten worden geïmplementeerd en aangepast door elke organisatie voor hun eigen toepassingen.

- Cloud-toepassingen zijn vaak vastgesteld met een hogere snelheid dan IT-organisatie met bestaande IAM-oplossingen integreren kunt.

- Beveiligings- en tooling monitoring vereisen extra aanpassing en integratie met uitgebreide E2E-scenario's bereiken.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory geïntegreerd met toepassingen

Azure Active Directory is volledig identiteit van de Microsoft als de serviceoplossing van een (IDaaS) die:

- Hiermee IAM als een cloudservice 

- Biedt centraal toegangsbeheer, eenmalige aanmelding (SSO) en rapportage 

- Ondersteunt geïntegreerd toegangsbeheer voor [duizenden toepassingen](https://azure.microsoft.com/marketplace/active-directory/) in de galerie van toepassing, zoals Salesforce, Google Apps, vak, Concur en meer. 


Met Azure Active Directory, alle toepassingen die u publiceert voor uw partners en klanten (bedrijven of consumenten) hebben dezelfde identiteit en toegang tot beheerfuncties.<br> Hiermee kunt u de operationele kosten aanzienlijk te reduceren.

Wat gebeurt er als u wilt implementeren van een toepassing die nog niet wordt vermeld in de galerie toepassing? Dit is minder tijd in beslag dan eenmalige aanmelding configureren voor toepassingen uit de galerie met toepassing, biedt Azure AD u een wizard waarmee u met de configuratie.

De waarde van Azure AD voorrang heeft op "alleen" cloud-toepassingen. U kunt dit ook gebruiken met on-premises implementatie-toepassingen via een beveiligde externe toegang. Met veilige externe toegang, kunt u voorkomen de dat u een VPN's of andere traditionele RAS management implementaties.

Azure AD biedt met centraal toegangsbeheer en eenmalige aanmelding (SSO) voor al uw toepassingen, de belangrijkste gegevens beveiligings- en productiviteit mogelijke oplossing.

- Gebruikers hebben toegang tot meerdere toepassingen met één aanmeldingsgegevens meer tijd verlenen aan inkomsten genereren of zakelijk bewerkingen activiteiten klaar.

- Identiteit beheerders kunnen toegang tot toepassingen op één plaats beheren.

Het voordeel voor uw bedrijf en voor de gebruiker is duidelijk. Laten we eens op de voordelen voor de beheerder van een identiteit en de organisatie.

## <a name="integrated-application-benefits"></a>Voordelen van de toepassing Integrated

Het proces SSO heeft twee stappen:

- Verificatie, het proces voor het valideren van de gebruikers id.

- Autorisatie, het besluit om inschakelen of blok toegang tot een resource aan een access-beleid.

Wanneer u met Azure AD-toepassingen beheren en inschakelen van eenmalige aanmelding:

- Verificatie wordt uitgevoerd op de on-premises implementatie (bijvoorbeeld AD) of Azure AD-account van de gebruiker.

- Autorisatie wordt uitgevoerd op het Azure AD toewijzingsvelden en beveiliging beleid zorgen voor consistente eindgebruikerservaring en het zodat u kunt toewijzen, locaties en MFA voorwaarden toevoegen op een toepassing, ongeacht de interne mogelijkheden.

Het belangrijk om te begrijpen dat de manier waarop de vergunning wordt gepubliceerd op de doeltoepassing varieert afhankelijk van hoe de toepassing is geïntegreerd met Azure AD.

- **Toepassingen die vooraf zijn geïntegreerd door-provider** Hierna ziet u toepassingen is gebouwd rechtstreeks op Azure AD en te vertrouwen op deze voor de uitgebreide mogelijkheden voor identiteits- en access, zoals Office 365 en Azure. Toegang tot deze toepassingen is via directory-gegevens en token uitgifte ingeschakeld.

- **Toepassingen die vooraf zijn geïntegreerd van Microsoft en aangepaste toepassingen** Hierna ziet u onafhankelijke cloud-toepassingen die afhankelijk zijn van een map interne toepassing en kunnen werken onafhankelijk van Azure AD. Toegang tot deze toepassingen is ingeschakeld door een toepassing specifieke referentie toegewezen aan een toepassing-account. Afhankelijk van de toepassing-functies, de referentie mogelijk een Federatie token of de gebruikersnaam en wachtwoord voor een account die eerder is ingericht in de toepassing.

- **On-premises-toepassingen** Toepassingen die zijn gepubliceerd via de Azure AD-toepassingsproxy hoofdzakelijk toegang bieden tot on-premises implementatie-toepassingen. Deze toepassingen vertrouwen op een centraal op premises directory zoals Windows Server Active Directory. Toegang tot deze toepassingen is ingeschakeld door de proxy activeert voor het leveren van de toepassingsinhoud voor de eindgebruiker terwijl behouden blijft de on-premises implementatie aanmelding vereiste.

Bijvoorbeeld als een gebruiker deelneemt aan uw organisatie, moet u een account voor de gebruiker in Azure AD voor de primaire bewerkingen aanmelding maken. Als deze gebruiker vereist is voor toegang tot een beheerde toepassing zoals Salesforce, moet u ook een account voor deze gebruiker in Salesforce maken en dit koppelen aan de Azure account om er SSO werken. Wanneer de gebruiker de organisatie verlaat, is het raadzaam de Azure AD-account verwijderen en alle tegenhanger accounts in de IAM worden opgeslagen met de toepassingen die de gebruiker heeft toegang tot.

## <a name="access-detection"></a>Access-detectie

In moderne ondernemingen zijn de IT-afdelingen vaak niet op de hoogte van alle cloud-toepassingen die worden gebruikt. In combinatie met de Cloud-App Discovery biedt Azure AD u een oplossing voor deze toepassingen detecteren.

## <a name="account-management"></a>Accountbeheer

Traditioneel accounts niet in de verschillende toepassingen beheren is een handmatig proces dat wordt uitgevoerd door IT of ondersteuning persoonlijke in de organisatie. Azure AD automatische volledig accountbeheer op alle servicetoepassingen provider geïntegreerd en deze toepassingen die vooraf zijn geïntegreerd met door Microsoft ondersteunende geautomatiseerde gebruiker inrichting of SAML JIT.

## <a name="automated-user-provisioning"></a>De inrichting van geautomatiseerde gebruiker

Sommige toepassingen bieden automatiseringsinterfaces voor het maken en verwijderen (of deactiveren) accounts. Als een provider een interface biedt, wordt deze overgenomen door Azure AD. Dit Hiermee reduceert u uw operationele kosten omdat beheertaken automatisch plaats en verbetert de beveiliging van uw omgeving omdat deze de kans van toegang door onbevoegden wordt kleiner.

## <a name="access-management"></a>Toegangsbeheer

U kunt toegang tot de toepassingen die gebruikmaken van afzonderlijke of regel basis van hoeveelheid werk toewijzingen met Azure AD beheren. U kunt ook delegeren toegang tot beheer om de juiste personen in de organisatie ervoor zorgen dat de beste supervisie en de belasting op helpdesk.

## <a name="on-premises-applications"></a>On-premises implementatie-toepassingen

De ingebouwde in toepassing kunt u uw on-premises implementatie-toepassingen aan uw gebruikers met resultaat publiceren met proxy zowel consistente ervaring met moderne cloud-toepassing en de voordelen van Azure AD-cmdlets voor controle, rapportage en beveiliging mogelijkheden toegang.

## <a name="reporting-and-monitoring"></a>Rapportage en monitoring

Azure AD biedt u vooraf geïntegreerde rapporteren en functies waarmee u kunt weten wie toegang tot toepassingen en heeft wanneer ze deze daadwerkelijk gebruikt.

## <a name="related-capabilities"></a>Gerelateerde functies

U kunt uw toepassingen met gedetailleerde clienttoegangsbeleid en vooraf geïntegreerde MFA beveiligen met Azure AD. Voor meer dat informatie over Azure MFA raadpleegt u [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Aan de slag

Als u wilt beginnen toepassingen integreren met Azure AD, raadpleegt u de [integratie van Azure Active Directory met toepassingen die aan de slag](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Zie ook

[Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
