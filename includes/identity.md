Identiteit beheren is net zo belangrijk in de openbare cloud als het on-premises is. Om u te helpen met dit, ondersteunt Azure verschillende verschillende cloud identiteit technologieën. De opties omvatten deze:

- U kunt Windows Server Active Directory (gewoonlijk alleen AD genoemd) in de cloud met virtuele machines die zijn gemaakt met Azure virtuele machines uitvoeren. Deze benadering is logisch wanneer u Azure gebruikt voor uw on-premises implementatie-datacenter uitbreiden in de cloud.


- U kunt Azure Active Directory gebruiken voor uw gebruikers eenmalige aanmelding tot softwaretoepassingen [als een Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) . Van Microsoft Office 365 gebruikmaakt van deze technologie, bijvoorbeeld en toepassingen op Azure of andere platforms cloud kunnen ook gebruiken.


- Toepassingen die worden uitgevoerd in de cloud of on-premises implementatie kunt Azure Active Directory-toegangsbeheer kunnen gebruikers log in het gebruik van identiteiten uit Facebook, Google, Microsoft en andere identiteitsprovider.


In dit artikel worden alle drie van de volgende opties.

## <a name="table-of-contents"></a>Inhoudsopgave

- [Windows Server Active Directory uitgevoerd in virtuele machines](#adinvm)

- [Gebruik van de Azure Active Directory](#ad)

- [Gebruik van de Azure Active Directory-toegangsbeheer](#ac)


## <a name="adinvm"></a>Windows Server Active Directory uitgevoerd in virtuele machines

Windows Server AD uitgevoerd in Azure virtuele machines lijkt veel op het on-premises implementatie uitvoeren. [Afbeelding 1](#fig1) ziet u een typisch voorbeeld van hoe dit eruitziet.

![Azure Active Directory in de virtuele machines](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Afbeelding 1: Windows Server Active Directory kunt uitvoeren in Azure virtuele machines verbonden met de datacenter van de on-premises implementatie van een organisatie met Azure Virtual Network.

In het voorbeeld hieronder, wordt Windows Server AD uitgevoerd op VMs die zijn gemaakt met behulp van Azure virtuele Machines, van het platform IaaS technologie. Deze VMs en enkele andere zijn gegroepeerd in een virtueel netwerk is verbonden met een on-premises implementatie-datacenter met Azure Virtual Network. Het virtuele netwerk carves uit een groep van cloud virtuele machines die met de on-premises netwerk via een verbinding virtual private network (VPN samenwerken). Hiermee kunt doen eruit deze Azure virtuele machines alleen een ander subnet naar de on-premises implementatie-datacenter. Als de afbeelding wordt weergegeven, worden twee van deze VMs Windows Server AD-domeincontrollers uitgevoerd. De andere virtuele machines in het virtuele netwerk mogelijk toepassingen, zoals SharePoint of in een andere manier wordt gebruikt, zoals voor het ontwikkelen en testen worden uitgevoerd. Het datacenter van de on-premises implementatie wordt ook twee Windows Server AD-domeincontrollers uitgevoerd.

Er zijn verschillende manieren om verbinding te maken van de domeincontrollers in de cloud met die met een on-premises:

- Alle labels maken voor een deel van één Active Directory-domein.

- Afzonderlijke AD domeinen on-premises implementatie maken in de cloud die deel uitmaken van de dezelfde bos.

- Afzonderlijke AD forests maken in de cloud en on-premises en verbind de forests cross-bos vertrouwensrelaties of Windows Server Active Directory Federation Services (AD FS), die ook in virtuele machines op Azure uitvoeren kunt gebruikt.

Ongeacht keuze wordt gemaakt, een beheerder moet ervoor zorgen dat verificatie aanmeldingsaanvragen van on-premises gebruikers gaat u naar cloud domeincontrollers alleen indien nodig, omdat de koppeling naar de cloud is waarschijnlijk langzamer dan on-premises implementatie-netwerken te gebruiken. Een andere factor u rekening moet houden in verbindende cloud en domeincontrollers on-premises implementatie is het verkeer gegenereerd door herhaling. Domeincontrollers in de cloud zijn meestal hun eigen AD-site, waarmee een beheerder om te plannen hoe vaak replicatie is voltooid. Azure kosten voor verkeer verzonden afmelden bij een Azure datacenter, hoewel het niet voor bytes verzonden in, die mogelijk van invloed zijn op van de beheerder replicatie-opties. Het is ook zegt af te wijzen dat terwijl Azure een eigen Domain Name System (DNS)-ondersteuning biedt, deze service die vereist zijn voor Active Directory (zoals ondersteuning voor dynamische DNS- en SRV-records ontbreekt). Reden moet voor het uitvoeren van Windows Server AD op Azure worden gebruikt bij het instellen van uw eigen DNS-servers in de cloud.

Windows Server AD uitgevoerd in Azure VMs kunt zinvol in diverse verschillende situaties. Hier volgen enkele voorbeelden:

- Als u een uitbreiding van uw eigen datacenter Azure virtuele Machines gebruikt, kunt u toepassingen kunt uitvoeren in de cloud die nodig hebt lokale domeincontrollers worden afgehandeld dingen zoals geïntegreerde Windows-verificatie aanvragen of LDAP-query's. SharePoint, bijvoorbeeld de interactie tussen en vaak met Active Directory, en dus het is mogelijk uit te voeren van een SharePoint-farm op Azure met een on-premises adreslijst, domeincontrollers in de cloud instellen wordt aanzienlijk prestaties verbeteren. (Dit is Houd er rekening mee dat dit niet per se vereist is, echter; voldoende toepassingen kan worden uitgevoerd in de cloud met alleen on-premises implementatie domeincontrollers.)

- Stel dat het kantoor in een veel ontbreekt de bronnen om uit te voeren eigen domeincontrollers. Tegenwoordig kunnen de gebruikers moeten worden geverifieerd domein controller aan de andere kant van de wereld - aanmeldingen traag zijn. Active Directory in een Microsoft-datacenter nabij waarop Azure kunt versnellen dit zonder meer servers op het kantoor.

- Een organisatie die gebruikmaakt van Azure voor herstel mogelijk een klein aantal actieve VMs in de cloud, met inbegrip van een domeincontroller onderhouden. Oplossing kan vervolgens worden bereid om uit te vouwen van deze site naar wens over te nemen voor niet-werkende ergens anders.

Er zijn ook andere mogelijkheden. Bijvoorbeeld: u bent niet vereist voor Windows Server AD in de cloud verbinding met een on-premises implementatie-datacenter. Als u wilt uitvoeren van een SharePoint-farm die een bepaalde reeks gebruikers dienden, bijvoorbeeld van wie uitsluitend met cloudgebaseerde identiteiten wilt aanmelden, u kunt een zelfstandige bos maken op Azure. Hoe u deze technologie wilt gebruiken, is afhankelijk van wat uw doelstellingen zijn. (Voor meer gedetailleerde informatie over het gebruik van Windows Server AD met Azure, [raadpleegt u hier](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Gebruik van de Azure Active Directory

Zoals SaaS-toepassingen meer algemene worden, verhogen ze een duidelijke vraag: wat voor soort adreslijstservice moeten deze toepassingen cloudgebaseerde gebruiken? Microsoft antwoord op deze vraag is Azure Active Directory.

Er zijn twee belangrijkste opties voor het gebruik van deze adreslijstservice in de cloud:

- Personen en organisaties die gebruikmaken van alleen SaaS-toepassingen kunnen vertrouwen op Azure Active Directory als hun enige adreslijstservice.

- Organisaties die de Active Directory van Windows Server wordt uitgevoerd kunnen hun on-premises adreslijst verbinden met Azure Active Directory en vervolgens deze gebruiken om aan te geven hun gebruikers eenmalige aanmelding tot SaaS-toepassingen.


[Afbeelding 2](#fig2) ziet u de eerste van de volgende twee opties, waar Azure Active Directory wordt gevormd door alle die nodig is.

![Azure Active Directory in de virtuele machines](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Afbeelding 2: De Azure Active Directory biedt van een organisatie gebruikers eenmalige aanmelding tot SaaS-toepassingen, met inbegrip van Office 365.

Als de afbeelding wordt weergegeven, is Azure AD een meerdere tenant-service. Dit betekent dat veel verschillende organisaties directory informatie over gebruikers op elk van deze op te slaan tegelijk kan worden ondersteund. In dit voorbeeld wordt een gebruiker op organisatie A probeert voor toegang tot een toepassing voor SaaS. Deze toepassing misschien deel uit van Office 365, zoals SharePoint Online, of het mogelijk iets anders: niet-Microsoft-toepassingen kunnen ook deze technologie gebruiken. Omdat Azure AD ondersteunt het SAML 2.0-protocol, is alle die nodig is van een toepassing de mogelijkheid om te communiceren met deze standaard. (In feite toepassingen die gebruikmaken van Azure AD kunnen worden uitgevoerd in een datacenter, niet alleen een Azure datacenter.)

Het proces wordt gestart wanneer de gebruiker toegang heeft tot een toepassing voor SaaS (stap 1). Als u wilt deze toepassing gebruikt, moet de gebruiker een token uitgegeven door Azure AD presenteren.

Deze token bevat informatie die aangeeft van de gebruiker en deze digitaal ondertekend door Azure AD. Als u het token, de gebruiker geverifieerd zichzelf bij Azure AD doordat een gebruikersnaam en wachtwoord (stap 2). Azure AD vervolgens geeft als resultaat het token hij moet (stap 3).

Deze token wordt vervolgens verzonden naar de SaaS-toepassing (stap 4), die van het token handtekening is gevalideerd en gebruikt de inhoud ervan (stap 5). De toepassing wordt meestal de informatie over die het token bevat om te bepalen welke gegevens die de gebruiker is toegestaan naar access en misschien op andere manieren gebruikt.

Als de toepassing meer informatie over de gebruiker dan wat deel van de token uitmaakt nodig, kunt deze dit aanvragen rechtstreeks vanuit Azure AD met de API Azure AD Graph (stap 6). In de oorspronkelijke versie van Azure AD, het directory-schema is heel eenvoudig: deze alleen gebruikers en groepen en relaties tussen deze bevat. Toepassingen kunnen deze gegevens gebruiken voor meer informatie over verbindingen tussen gebruikers. Stel dat een toepassing moet weet wie de manager van deze gebruiker is te beslissen of hij heeft toegang tot een bepaalde hoeveelheid gegevens. Deze vindt meer informatie dit door de query's uitvoeren Azure AD via de API Graph.

De grafiek-API gebruikt een gewone RESTful protocol, waardoor u deze eenvoudig gebruik van de meeste clients, met inbegrip van mobiele apparaten. De API ondersteunt ook het selectievakje extensies gedefinieerd door OData, dingen zoals een querytaal om te laten clients access-gegevens op gebruiksvriendelijker manieren toevoegen. (Zie [Inleiding tot OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf)voor meer informatie over OData.) Omdat de API Graph kunnen worden gebruikt voor meer informatie over relaties tussen gebruikers, kunt deze toepassingen begrijpen van de sociale grafiek die ingesloten in het schema Azure AD voor een bepaalde organisatie (dit is waarom deze het Graph-API heet functie). En als u wilt verifiëren zelf Azure AD voor Graph API aanvragen, een toepassing OAuth 2.0 gebruikt.

Als een organisatie geen in Windows Server Active Directory gebruikt-heeft geen on-premises-servers of domeinen - en uitsluitend gebruikmaakt van cloud-toepassingen die gebruikmaken van Azure AD, wilt gebruik alleen deze map cloud van het bedrijf gebruikers eenmalige aanmelding al deze geven. Nog terwijl dit scenario vaker elke dag wordt, de meeste organisaties nog steeds gebruiken on-premises implementatie domeinen die zijn gemaakt met Windows Server Active Directory. Azure AD heeft een handige rol spelen hier ook, zoals [figuur 3](#fig3) ziet.

![Azure Active Directory in de virtuele Machine](./media/identity/identity_03_AD.png)
<a id="fig3"></a>afbeelding 3: een organisatie Windows Server Active Directory met Azure Active Directory aan SaaS toepassingen verleent aan de gebruikers eenmalige aanmelding kan communiceren.

In dit scenario wordt een gebruiker op organisatie B wenst voor toegang tot een toepassing voor SaaS. Voordat ze dit doet, moeten directory-beheerders van de organisatie een relatie Federatie instellen met Azure AD met AD FS, zoals in de afbeelding wordt getoond. Deze beheerders moet ook gegevenssynchronisatie tussen van de organisatie on-premises implementatie Windows Server AD en Azure AD configureren. Dit wordt gekopieerd gebruiker en groepsgegevens van de on-premises adreslijst te Azure AD. Zoals u ziet in deze sectie geeft: de organisatie is In feite de on-premises adreslijst uitbreiden in de cloud. Windows Server AD en Azure AD op deze manier combineren biedt de organisatie een adreslijstservice die kan worden beheerd als een eenheid, terwijl het nog steeds een footprint van beide on-premises implementatie en in de cloud.

Als u wilt gebruiken Azure AD, de gebruiker meldt zich eerst bij haar on-premises Active Directory-domein zoals u gewend bent (stap 1). Wanneer zij probeert voor toegang tot de SaaS-toepassing (stap 2), levert het proces Federatie Azure AD uitgifte haar een token van deze toepassing (stap 3). (Zie voor meer informatie over de werking van Federatie [op Claims gebaseerde identiteit voor Windows: technologieën en scenario's](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Voordat u, bevat deze token informatie die aangeeft dat de gebruiker en wordt deze digitaal ondertekend door Azure AD. Deze token wordt vervolgens verzonden naar de SaaS-toepassing (stap 4), die van het token handtekening is gevalideerd en gebruikt de inhoud ervan (stap 5). En staat in het vorige scenario, de toepassing de grafiek-API gebruiken kan voor meer informatie over deze gebruiker als SaaS nodig (stap 6).

Tegenwoordig kunnen is Azure AD is niet volledig on-premises implementatie Windows Server AD te vervangen. De cloud-map heeft een veel eenvoudiger schema zoals reeds vermeld, en dingen zoals Groepsbeleid, de mogelijkheid voor het opslaan van informatie over machines en ondersteuning voor LDAP ook ontbreken. (In feite een Windows-computer kan niet worden geconfigureerd als u wilt dat gebruikers zich kunnen aanmelden via niets maar Azure AD - dit niet wordt ondersteund scenario.) In plaats daarvan de eerste doelstellingen van Azure AD opnemen zodat gebruikers toegang bedrijfstoepassingen in de cloud zonder behoud van een afzonderlijke aanmelding en vrijmaken on-premises directory-beheerders van hun on-premises adreslijst handmatig te synchroniseren met elke SaaS-toepassing die gebruikmaakt van hun organisatie. Na verloop van tijd echter verwachten deze cloud-adreslijstservice met een oplossing voor een grotere groep scenario's beschreven.

## <a name="ac"></a>Gebruik van de Azure Active Directory-toegangsbeheer

Identiteit cloudgebaseerde technologieën kunnen worden gebruikt voor het oplossen van tal van problemen. Azure Active Directory kunt geven van een organisatie gebruikers eenmalige aanmelding tot meerdere SaaS-toepassingen, bijvoorbeeld. Maar identiteit technologieën in de cloud kunnen ook worden gebruikt in andere manieren.

Stel, dat een toepassing wil haar gebruikers laten Meld u aan met tokens uitgegeven door meerdere *identiteitsprovider (IdPs)*. Een groot aantal verschillende identiteitsprovider bestaan vandaag, zoals Facebook, Google, Microsoft en anderen, en toepassingen vaak laat gebruikers zich kunnen aanmelden met een van deze identiteiten. Waarom moet een toepassing lastig hoeft te vallen behoudt een eigen lijst met gebruikers en wachtwoorden opnieuw instellen wanneer deze in plaats daarvan kunt vertrouwen op de identiteit die al aanwezig? Bestaande identiteiten accepteren zorgt ervoor dat leven eenvoudiger beide voor gebruikers met een minder gebruikersnaam en wachtwoord te onthouden en voor de personen die de toepassing maken die niet meer nodig hebt voor het behoud van hun eigen lijst met gebruikersnamen en wachtwoorden opnieuw instellen.

Maar terwijl elke identiteitsprovider een soort token problemen, deze tokens niet standaard worden-elke IdP een eigen indeling heeft. Bovendien de informatie in deze tokens ook niet standaard. Er is een toepassing die wil accepteren tokens uitgegeven door, zeg, Facebook, Google en Microsoft geconfronteerd met de uitdaging van unieke code schrijft u omgaat met elk van deze indelingen.

Maar waarom dit doen? Waarom niet maken in plaats daarvan een tussenpersoon die één token indeling met een algemene weergave van identiteit informatie kunt genereren? Deze methode zou eenvoudiger leven voor de ontwikkelaars die maken van toepassingen, aangezien ze nu nodig hebt u omgaat met slechts één soort token. Azure Active Directory-toegangsbeheer doet dit precies, intermediair in de cloud leveren voor het werken met diverse tokens. [Afbeelding 4](#fig4) ziet u hoe dat werkt

![Azure Active Directory in de virtuele Machine](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>afbeelding 4: Azure Active Directory-toegangsbeheer maakt het eenvoudiger voor toepassingen identiteitstokens uitgegeven door verschillende identiteitsprovider accepteren.

Het proces wordt gestart wanneer een gebruiker probeert te krijgen tot de toepassing via een browser. De toepassing wordt omgeleid ze een IdP van haar keuze (en dat de toepassing ook vertrouwensrelaties). Zij wordt geverifieerd zichzelf bij deze IdP, zoals door in te voeren een gebruikersnaam en wachtwoord (stap 1) en de IdP geeft als resultaat een token met informatie over haar (stap 2).

Als de afbeelding wordt weergegeven, ondersteunt beheren in Access een aantal verschillende cloudgebaseerde IdPs, inclusief accounts gemaakt door Google, Yahoo, Facebook, Microsoft (voorheen Windows Live ID) en OpenID providers. Ondersteunt ook identiteiten die zijn gemaakt met behulp van Azure Active Directory en, via federatie met AD FS, Windows Server Active Directory. Het doel is voor de meest gebruikte identiteiten vandaag, ongeacht of ze door IdPs in de cloud of on-premises implementatie uitgegeven bent.

Als een browser van de gebruiker heeft een token IdP van haar gekozen IdP, deze naar te sturen token toegangsbeheer (stap 3). Toegangsbeheer is gevalideerd met het token, ervoor te zorgen dat dit echt is uitgegeven door deze IdP en maakt vervolgens een nieuwe token op basis van de regels die zijn gedefinieerd voor deze toepassing. Zoals Azure Active Directory, toegangsbeheer is een service meerdere tenant, maar de tenants zijn toepassingen in plaats van klant organisaties. Elke toepassing krijgt een eigen naamruimte, zoals in de afbeelding en verschillende regels voor het autorisatie en nog veel meer kunt definiëren.

Deze regels kunnen de beheerder van de van elke toepassing definiëren hoe tokens afgeleid van verschillende IdPs moeten worden omgezet in een token beheren in Access. Bijvoorbeeld als verschillende typen verschillende IdPs gebruikt voor het weergeven van gebruikersnamen, kunnen toegangsbeheer regels transformeren al deze in een algemene gebruikersnaam type. Deze nieuwe token stuurt toegangsbeheer terug naar de browser (stap 4), die u dit naar de toepassing (stap 5 verstuurt). Wanneer deze het toegangsbeheer token heeft, wordt de toepassing gecontroleerd dat deze token echt is uitgegeven door toegangsbeheer, en vervolgens worden de gegevens bevat (stap 6) gebruikt.

Hoewel dit proces iets ingewikkeld lijken mogelijk, deze daadwerkelijk zorgt ervoor dat leven aanzienlijk eenvoudiger voor de maker van de toepassing. In plaats van verwerken diverse tokens die de verschillende informatie bevat, kan de toepassing identiteiten uitgegeven door meerdere identiteitsprovider tijdens het nog steeds ontvangt alleen een enkele token met vertrouwde gegevens accepteren. In plaats van elke toepassing is geconfigureerd om te vertrouwen verschillende IdPs vereist, ook deze vertrouwensrelaties in plaats daarvan worden beheerd door toegangsbeheer: een toepassing nodig alleen deze vertrouwen.

Het verdient aanbeveling af te wijzen dat er niets over toegangsbeheer is gekoppeld aan Windows - deze net zo goed kan worden gebruikt door een Linux-toepassing die alleen Google en Facebook identiteiten geaccepteerd. En hoewel toegangsbeheer deel uit van de Azure Active Directory familie maakt, kunt u zien ervan als een geheel afzonderlijk service dan in de vorige sectie is beschreven. Hoewel beide technologieën met identiteit werken, deze zijn gerelateerd aan heel anders problemen (Hoewel Microsoft heeft gemeld dat dit verwacht te integreren van de twee op een gegeven moment).

Werken met identiteit is belangrijk in bijna elke toepassing. Het doel van toegangsbeheer is zodat u eenvoudiger voor ontwikkelaars toepassingen die identiteiten van diverse identiteitsprovider accepteren maken. Door te plaatsen deze service in de cloud, heeft Microsoft gesteld deze beschikbaar voor elke toepassing uitgevoerd op een willekeurig platform.

##<a name="about-the-author"></a>Over de auteur

David Chappell is hoofdsom van Chappell & Associates [www.davidchappell.com](http://www.davidchappell.com) in San Francisco, Californië.
