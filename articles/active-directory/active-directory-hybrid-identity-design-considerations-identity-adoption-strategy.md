<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - definiëren een hybride identiteit aanpassingsstrategie voor | Microsoft Azure"
    description="Met voorwaardelijke toegangsbeheer controles Azure Active Directory de specifieke voorwaarden die u bij het verifiëren van de gebruiker en vóór het toestaan van toegang tot de toepassing kiezen. Zodra deze voorwaarden is voldaan, wordt de gebruiker is geverifieerd, en toegang hebben tot de toepassing."
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


# <a name="define-a-hybrid-identity-adoption-strategy"></a>Een hybride identiteit aanpassingsstrategie voor definiëren

In deze taak kunt u de hybride identiteit aanpassingsstrategie voor voor uw hybride identiteit oplossing om te voldoen aan de vereisten voor bedrijven die werden besproken in definiëren:

- [Zakelijke behoeften bepalen](active-directory-hybrid-identity-design-considerations-business-needs.md)
- [Vereisten voor directory-synchronisatie bepalen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
- [Vereisten voor meervoudige verificatie bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Zakelijke behoeften bepalen
Vereisten voor de eerste taak adressen bepalen van het bedrijf organisaties.  Dit is zeer brede en bereik kneep kan optreden als u niet dat u daarbij.  Houd het eenvoudig in het begin maar vergeet voor het plannen van een ontwerp die wordt uitgevoerd en wijzigen in de toekomst bevorderen.  Ongeacht of het ontwerp van een eenvoudige of een uiterst complexe, is de Azure Active Directory het Microsoft Identity-platform die ondersteuning biedt voor Office 365, Microsoft Online Services en cloud op toepassingen.

## <a name="define-an-integration-strategy"></a>Een Integratiestrategie definiëren
Microsoft heeft drie belangrijkste integratie-scenario's die cloud identiteiten, gesynchroniseerde identiteiten en federatieve identiteiten zijn.  Het is raadzaam op vast te stellen op een van deze integratiestrategieën.  De strategie die u kunnen variëren en beslissingen bij het kiezen van een kan onder andere bestaan, welk type gebruikerservaring die u opgeven wilt, hebt u enkele van de bestaande infrastructuur al in-place en wat is het meest efficiënte kiezen.  
 
![](./media/hybrid-id-design-considerations/integration-scenarios.png)

Er zijn de scenario's gedefinieerd in de bovenstaande afbeelding:

- **De identiteit van de cloud**: dit identiteiten die uitsluitend in de cloud aanwezig zijn.  In het geval van Azure AD zou deze specifiek in uw adreslijst Azure AD staan.
- **Gesynchroniseerde**: dit identiteiten die on-premises implementatie aanwezig zijn in de cloud.  Azure AD Connect gebruikt, worden deze gebruikers gemaakt of die zijn gekoppeld met bestaande Azure AD-accounts.  Wachtwoordhash van de gebruiker wordt gesynchroniseerd vanuit de on-premises omgeving in de cloud in wat een wachtwoordhash wordt genoemd.  Wanneer gebruik gesynchroniseerd is de enige belemmering als een gebruiker is uitgeschakeld in de on-premises omgeving, kan het tot 3 uur voor de status van die account moet verschijnen in Azure AD duren.  Dit is vanwege het tijdsinterval van synchronisatie.
- **Federatieve**: deze identiteiten bestaat beide on-premises implementatie en in de cloud.  Azure AD Connect gebruikt, worden deze gebruikers gemaakt of die zijn gekoppeld met bestaande Azure AD-accounts.  
 
>[AZURE.NOTE]
Voor meer informatie over de synchronisatie Lees opties [integreren met Azure Active Directory identiteiten on-premises implementatie](active-directory-aadconnect.md).

De volgende tabel helpt bij het bepalen van de voor- en nadelen van elk van de volgende methoden:

| Strategie         | Voordelen                                                                                                                                                                                                                                                  | Nadelen                                                                                                                                                                                                                                                                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **De identiteit van de cloud** | Eenvoudiger kunt beheren voor kleine organisatie. <br> Niets te installeren op de lokale geen extra hardware nodig<br>Eenvoudig uitgeschakeld als de gebruiker het bedrijf verlaat                                                                                                   | Gebruikers moeten aanmelden bij het openen van werkbelasting in de cloud <br> Wachtwoorden al dan niet hetzelfde is voor de cloud en on-premises identiteiten                                                                                                                                                                                                                     |
| **Gesynchroniseerd**     | On-premises implementatie wachtwoord wordt geverifieerd beide on-premises implementatie en cloud mappen <br>Eenvoudiger kunt beheren voor kleine, middelgrote of grote organisaties <br>Gebruikers kunnen hebben eenmalige aanmelding (SSO) voor enkele bronnen <br> Microsoft voorkeur methode voor synchronisatie <br> Eenvoudiger kunt beheren | Sommige klanten misschien niet graag hun mappen synchroniseren met de cloud einddatum van specifieke bedrijf gerechtelijke                                                                                                                                                                                                                                                  |
| **Federatieve**        | Gebruikers kunnen hebben eenmalige aanmelding (SSO) <br>Als een gebruiker wordt beëindigd of verlaat, het account kan, direct kan worden uitgeschakeld en access ingetrokken,<br> Ondersteunt geavanceerde scenario's die niet kan worden uitgevoerd met gesynchroniseerd                                           | Meer stappen voor het instellen en configureren <br> Hogere onderhoud <br> Extra hardware nodig voor de STS-infrastructuur <br> Extra hardware voor het installeren van de federatieserver nodig. Extra software is vereist als AD FS wordt gebruikt <br> Uitgebreide instellingen voor eenmalige aanmelding vereisen <br> Kritiek punt risico als de federatieserver niet actief is, gebruikers niet mogelijk om te verifiëren |

### <a name="client-experience"></a>Clientervaring
De strategie die u gebruikt, wordt de gebruikerservaring aanmeldingsproblemen dicteren.  De volgende tabel vindt u met informatie over wat de gebruikers hun aanmeldervaring moeten kunnen verwachten.  Houd er rekening mee dat niet alle federatieve identiteitsprovider SSO in alle scenario's ondersteunt.

**Domein-die zijn gekoppeld en particuliere netwerktoepassingen**:
 

|                              | Gesynchroniseerde identiteit      | Federatieve identiteit                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Webbrowsers                 | Op formulieren gebaseerde verificatie | eenmalige, soms vereist voor het leveren van de organisatie-ID |
| Outlook                      | Vragen om referenties     | Vragen om referenties                                       |
| Skype voor bedrijven (Lync)    | Vragen om referenties     | eenmalige aanmelding voor Lync, wordt u gevraagd of u de referenties voor Exchange   |
| SkyDrive Pro                 | Vragen om referenties     | eenmalige aanmelding                                               |
| Office Pro Plus abonnement | Vragen om referenties     | eenmalige aanmelding                                               |

**Extern of niet-vertrouwde bronnen**:

|                              | Gesynchroniseerde identiteit      | Federatieve identiteit                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Webbrowsers                 | Op formulieren gebaseerde verificatie |  Op formulieren gebaseerde verificatie |
| Outlook, Skype voor bedrijven (Lync), Skydrive Pro, Office-abonnement| Vragen om referenties     | Vragen om referenties                                       |
| Exchange ActiveSync    | Vragen om referenties     | eenmalige aanmelding voor Lync, wordt u gevraagd of u de referenties voor Exchange   |
| Mobiele apps                 | Vragen om referenties     | Vragen om referenties                                               |

Als u hebt vastgesteld uit 1 dat er een 3e derden IdP of worden gaan om te bieden Federatie met Azure AD gebruiken voor een taak, moet u rekening houden met de volgende ondersteunde mogelijkheden:
- Een SAML 2.0-provider dat compatibel is voor het SP-Lite profiel kunt ondersteuning voor verificatie Azure AD en bijbehorende toepassingen
- Ondersteunt passieve verificatie, wat auth OWA, SPO, enzovoort vergemakkelijkt.
- Exchange Online-clients kunnen via de SAML 2.0 Enhanced Client profiel (ECP) worden ondersteund

Ook moet u rekening houden met welke mogelijkheden niet meer beschikbaar:

- Zonder ondersteuning voor WS-vertrouwen/identiteitsfederatie, worden alle andere actieve clients afgebroken
 - Dat betekent dat er geen Lync-client, OneDrive-client, Office-abonnement, Office Mobile voordat u Office 2016
- Overgang van Office naar passieve verificatie gebruikt, kan deze ter ondersteuning van puur SAML 2.0 IdPs, maar ondersteuning nog steeds niet op basis van de client-door-client


>[AZURE.NOTE]
Lees het artikel http://aka.ms/ssoproviders voor de meest bijgewerkte lijst.

## <a name="define-synchronization-strategy"></a>Synchronisatie strategie definiëren
In deze taak u de hulpmiddelen die wordt gebruikt definieert voor het synchroniseren van de organisatie on-premises implementatie gegevens naar de cloud en wat u moet gebruiken topologie.  Omdat de meeste organisaties Active Directory gebruikt, vindt u informatie over het gebruik van Azure AD Connect naar het adres van de bovenstaande vragen in sommige details.  Voor omgevingen waarvoor geen Active Directory, vindt u informatie over FIM 2010 R2 of MIM 2016 gebruiken om u te helpen deze strategie.  Echter LDAP-mappen, dus is afhankelijk van uw tijdlijn biedt ondersteuning voor toekomstige versies van Azure AD Connect, deze informatie mogelijk is om u te helpen.

###<a name="synchronization-tools"></a>Hulpmiddelen voor synchronisatie
Jaar, hebben verschillende hulpprogramma's voor synchronisatie bestaat en die wordt gebruikt voor verschillende scenario's.  Azure AD Connect is momenteel het Ga naar programma van uw keuze voor alle ondersteunde scenario's.  AAD-synchronisatie en DirSync zijn ook nog steeds rond en zelfs mogelijk aanwezig zijn in uw omgeving nu. 

>[AZURE.NOTE]
Voor de meest recente informatie over de ondersteunde mogelijkheden van elke functie, door [Directory-integratie hulpmiddelen voor vergelijking](active-directory-hybrid-identity-design-considerations-tools-comparison.md) artikel te lezen.  

### <a name="supported-topologies"></a>Ondersteunde topologieën
Wanneer u een strategie synchronisatie definieert, kunnen de topologie die wordt gebruikt, moet worden bepaald. U afhankelijk van de informatie die is vastgesteld in stap 2 kunt bepalen welke topologie het juiste account te gebruiken. De één bos, één Azure AD topologie is de meest gebruikte en bestaat uit een enkel Active Directory-bos en één exemplaar van Azure AD.  Hiermee dient te worden gebruikt in een meeste scenario's beschreven en wordt de verwachte topologie wanneer u een installatie Azure AD verbinding Express gebruikt zoals in de onderstaande afbeelding.
 
![](./media/hybrid-id-design-considerations/single-forest.png)Enkel bos Scenario dat is zeer gebruikelijk voor grote en zelfs kleine organisaties hebben meerdere forests, zoals wordt weergegeven in de afbeelding 5.

>[AZURE.NOTE]
Voor meer informatie over de verschillende lokale en Azure AD-topologieën met Azure AD Connect Lees synchronisatie het artikel [topologieën voor Azure AD Connect](active-directory-aadconnect-topologies.md).


![](./media/hybrid-id-design-considerations/multi-forest.png) 

Scenario voor meerdere bos

Als dit de hoofdletters/kleine letters en vervolgens de topologie meerdere / forest-enkel Azure AD moet worden beschouwd als de volgende items voldaan wordt:

- Gebruikers hebben alleen 1 identiteit tussen alle forests: de sectie van de onderstaande unieke id gebruikers dit uitgebreider beschreven.
- De gebruiker geverifieerd bij het bos waarin hun identiteit zich bevindt
- UPN en bron anker (onveranderlijke id) vandaan deze bos
- Alle forests zijn toegankelijk voor Azure AD Connect: dit betekent dat deze niet hoeft te worden domein die zijn gekoppeld en kan worden geplaatst in een DMZ als dit dit vergemakkelijkt.
- Gebruikers hebben slechts één postvak
- De bos die als host postvak van een gebruiker fungeert heeft de beste gegevenskwaliteit voor kenmerken zichtbaar in de Exchange algemene adreslijst lijst (GAL)
- Als er geen postvak op de gebruiker is, kan klikt u vervolgens een bos worden gebruikt deze waarden bijdragen
- Als u een gekoppelde postvak hebt, klikt u vervolgens is er ook een ander account in een ander domein gebruikt om aan te melden.

>[AZURE.NOTE]
Objecten die zich in beide on-premises en in de cloud zijn "verbonden" via een unieke id. In de context van Directory-synchronisatie, worden deze unieke id wordt de SourceAnchor genoemd. In de context van eenmalige aanmelding, dit de ImmutableId genoemd. [Ontwerpconcepten voor Azure AD Connect](active-directory-aadconnect-design-concepts.md#sourceanchor) voor meer overwegingen met betrekking tot het gebruik van SourceAnchor.

Als de bovenstaande niet van toepassing zijn en u meer dan één actieve account of meer dan één postvak hebt, wordt Azure AD Connect Kies een en de andere negeren.  Als u postvakken, maar geen andere account hebt gekoppeld, wordt deze accounts niet worden geëxporteerd naar Azure AD en die gebruiker worden niet een lid van een groep.  Dit is afwijkt van hoe dit is in het verleden met DirSync en opzettelijk beter ondersteuning is deze meerdere bos scenario's. Een scenario voor meerdere bos wordt weergegeven in de onderstaande afbeelding.
 
![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 
 
**Meerdere bos scenario voor meerdere Azure AD**

Het wordt aanbevolen dat slechts één map in Azure AD voor een organisatie, maar dit wordt toegestaan deze een 1:1-relatie tussen een Azure AD Connect-synchronisatie-server en een Azure AD-directory wordt bewaard.  Voor elk exemplaar van Azure AD moet u een installatie van Azure AD Connect.  Daarnaast Azure AD inherent aan het ontwerp wordt geïsoleerd en gebruikers in één exemplaar van Azure AD niet mogelijk om gebruikers in een ander exemplaar weer te geven.

Het is mogelijk en één exemplaar van de on-premises implementatie van Active Directory verbinden met meerdere Azure AD-mappen zoals wordt weergegeven in de onderstaande afbeelding wordt ondersteund:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 
 

**Scenario voor het filteren van enkele verzameling**

Om te kunnen als volgt moeten worden voldaan:

- Azure AD Connect synchroniseren servers moeten worden geconfigureerd voor het filteren van zodat ze elke een wederzijds set gegroepeerde objecten hebben.  Dit uitgevoerd, bijvoorbeeld door een bereik van elke server aan een bepaald domein of organisatie-eenheid instellen.
- Een DNS-domein kan alleen worden geregistreerd in één Azure AD directory zodat de UPN van de gebruikers in de on-premises implementatie moet AD afzonderlijke naamruimten gebruiken.
- Gebruikers in één exemplaar van Azure AD is alleen mogelijk om gebruikers hun exemplaar weer te geven.  Ze is niet mogelijk om gebruikers in de andere processen weer te geven
- Alleen een van de Azure AD-mappen hybride implementatie van Exchange met het lokale inschakelen kunt AD
- Onderlinge exclusiviteit geldt ook voor terugschrijven.  Dit zorgt ervoor dat sommige terugschrijven-functies niet worden ondersteund met deze topologie, omdat deze wordt ervan uitgegaan een enkel on-premises implementatie-configuratie dat.  Deze groep omvat:
 - Terugschrijven groeperen met standaardinstellingen is geconfigureerd
 - Apparaat terugschrijven


Let op dat het volgende wordt niet ondersteund en niet moet worden gekozen als een implementatie:

- Deze wordt niet ondersteund als u wilt dat meerdere Azure AD Connect synchronisatie servers verbinding maakt met dezelfde Azure AD map, zelfs als ze zijn geconfigureerd voor synchronisatie van elkaar wederzijds uitsluiten set object
- Deze wordt niet ondersteund als u wilt synchroniseren van de gebruiker zich meerdere Azure AD-mappen. 
- Dit wordt ook niet ondersteund als u wilt maken van een configuratie wijzigen zodat gebruikers in één Azure AD wilt weergeven als contactpersonen in een andere Azure AD-map. 
- Het is ook niet-ondersteunde voor het wijzigen van Azure AD Connect-synchronisatie verbinding maakt met meerdere Azure AD-mappen.
- Azure AD-mappen zijn standaard zo geïsoleerd. Het is niet-ondersteunde wijzigen van de configuratie van Azure AD Connect synchroniseren met het lezen van gegevens uit een andere Azure AD-map in een poging tot het bouwen van een GAL veelvoorkomende en geïntegreerd tussen de mappen. Wordt deze niet ondersteund als u wilt gebruikers exporteren als contactpersonen naar een andere lokale AD die Azure AD Connect sync gebruiken.


>[AZURE.NOTE]
Als uw organisatie wordt beperkt computers in uw netwerk geen verbinding kan maken met Internet, in dit artikel worden de eindpunten (FQDN, IPv4 en IPv6-adresbereiken) dat u moet opnemen in uw uitgaande toestaan lijsten en Internet Explorer vertrouwde websites van de client van computers om ervoor te zorgen uw computers kunnen Office 365 kunt gebruiken. Lees meer [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)voor meer informatie.

## <a name="define-multi-factor-authentication-strategy"></a>Meervoudige verificatie bepalen
In deze taak definieert u de meervoudige verificatie strategie gebruiken.  Azure meervoudige verificatie wordt geleverd in twee verschillende versie.  Een is een cloudgebaseerde en de andere on-premises implementatie op basis van de Server Azure-MFA.  Op basis van de evaluatie hierboven u kunt u bepalen welke oplossing is het juiste account voor uw strategie.  Gebruik de onderstaande tabel om te bepalen welke meest geschikte optie voor ontwerp van uw bedrijf-beveiliging behoefte:

Ontwerpopties voor meervoudige:

| Activa beveiligen                                               | MFA in de cloud | MFA on-premises |
|---------------------------------------------------------------|------------------|----------------|
| Microsoft-apps                                                | Ja              | Ja            |
| SaaS apps in de app-galerie                                  | Ja              | Ja            |
| IIS-toepassingen die zijn gepubliceerd via Azure AD-App-Proxy         | Ja              | Ja            |
| IIS-toepassingen die niet zijn gepubliceerd via de Proxy Azure AD-App | geen               | Ja            |
| Externe toegang als VPN, RDG                                     | geen               | Ja            |

Hoewel u mogelijk hebt vereffend aan een oplossing voor uw strategie, moet u nog steeds gebruiken van de evaluatie van boven op waar uw gebruikers zich bevinden.  Hierdoor kunnen de oplossing als u wilt wijzigen.  Gebruik de onderstaande tabel om u te helpen u dit bepalen:

| Gebruikerslocatie                                                       | Optie voorkeur ontwerp                 |
|---------------------------------------------------------------------|-----------------------------------------|
| Azure Active Directory                                              | Multi-FactorAuthentication in de cloud |
| Azure AD en on-premises Federatie met AD FS met AD             | Beide                                    |
| Azure AD en on-premises AD met Azure AD Connect niet Wachtwoordsynchronisatie | Beide                                    |
| Azure AD en on-premises met Azure AD Connect met Wachtwoordsynchronisatie  | Beide                                    |
| On-premises AD                                                      | Meervoudige verificatie-Server      |

>[AZURE.NOTE]
U moet er ook voor zorgen dat de functies die vereist voor het ontwerp van uw zijn worden ondersteund door de optie voor het ontwerp van meervoudige verificatie die u hebt geselecteerd.  Lees voor meer informatie [kiezen de beveiligingsoplossing meervoudige voor u](../multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).

## <a name="multi-factor-auth-provider"></a>Meervoudige verificatie-Provider
Meervoudige verificatie is standaard beschikbaar voor globale beheerders met een Azure Active Directory-tenant. Echter als u wilt uitbreiden meervoudige verificatie naar al uw gebruikers en/of uw globale beheerders wilt kunnen advantage-functies zoals de beheerportal, aangepaste begroetingen en rapporten maken, moet klikt u aanschaffen en meervoudige verificatieprovider configureren.

>[AZURE.NOTE]
U moet er ook voor zorgen dat de functies die vereist voor het ontwerp van uw zijn worden ondersteund door de optie voor het ontwerp van meervoudige verificatie die u hebt geselecteerd. 

##<a name="next-steps"></a>Volgende stappen
[Vereisten voor beveiliging van gegevens bepalen](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Zie ook
[Ontwerp overwegingen overzicht](active-directory-hybrid-identity-design-considerations-overview.md)
