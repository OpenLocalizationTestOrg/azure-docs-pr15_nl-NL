<properties
   pageTitle="Scenario's en voorbeelden voor het abonnement beheermodel | Microsoft Azure"
   description="Bevat voorbeelden van het implementeren van Azure abonnement beheermodel voor veelvoorkomende scenario's."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Voorbeelden van Azure enterprise scaffold implementeren

In dit onderwerp bevat voorbeelden van hoe een onderneming de aanbevelingen voor een [Azure enterprise scaffold](resource-manager-subscription-governance.md)kunt implementeren. Een fictief bedrijf map Contoso wordt gebruikt om te laten zien aanbevolen procedures voor veelvoorkomende scenario's. 

## <a name="background"></a>Achtergrond

Contoso is een wereldwijd bedrijf dat levering ketting oplossingen voor klanten in alles vanuit een model "Software als een Service" aan een verpakt model biedt geïmplementeerd on-premises implementatie.  Deze uitgroeien software overal ter wereld met aanzienlijk development centers in India, de Verenigde Staten en Canada. 

Het gedeelte ISV van het bedrijf is opgedeeld in verschillende onafhankelijke bedrijfseenheden die producten in een aanzienlijk en grote ondernemingen beheren. Elke eenheid bedrijven heeft een eigen ontwikkelaars, productmanagers en architecten. 

De indeling van de onderneming technologie Services (ETS) bedrijven eenheid biedt gecentraliseerde IT-capaciteit en verschillende datacenters waarbij bedrijfseenheden hun toepassingen host beheert. Samen met de datacenters beheert, de organisatie ETS biedt en beheert gecentraliseerde samenwerking (zoals e-mail en websites) en netwerk/telefonie-services. Ze ook beheren klant is gericht werkbelasting voor kleinere bedrijfseenheden die geen operationele medewerkers hebben. 

In dit onderwerp worden de volgende personas gebruikt:

- Dave is de ETS Azure-beheerder.
- Lisa is directeur van ontwikkeling van Contoso in de eenheid levering ketting bedrijven. 

Contoso moet maken van een lijn-of-business-app en een app klant-omlaag. Zij heeft besloten om uit te voeren van de apps op Azure. Dave leest het onderwerp van het [beschrijvende abonnement beheermodel](resource-manager-subscription-governance.md) en is nu klaar voor de aanbevelingen implementeren. 

## <a name="scenario-1-line-of-business-application"></a>Scenario 1: LOB-toepassing

Contoso is een bron code managementsysteem (BitBucket) moet worden gebruikt door ontwikkelaars kant van de wereld zitten samenstellen.  De toepassing infrastructuur gebruikt als een Service (IaaS) voor het hosten en bestaat uit endwebservers en een database-server. Ontwikkelaars toegang tot servers in hun omgevingen ontwikkeling, maar deze niet nodig hebt voor toegang tot de servers in Azure wordt aangegeven. Contoso ETS wenst toe te staan dat de eigenaar van de toepassing en het team voor het beheren van de toepassing. De toepassing is alleen beschikbaar tijdens het op van Contoso-bedrijfsnetwerk. Dave vereisten voor het instellen van het abonnement van deze toepassing. Het abonnement wordt ook in de toekomst hosten andere software relevant zijn voor ontwikkelaars.  

### <a name="naming-standards--resource-groups"></a>Naming standaarden & resourcegroepen

Dave Hiermee maakt u een abonnement ter ondersteuning van speciale tools voor ontwikkelaars die gebruikt in alle bedrijfseenheden worden. Hij moet maken betekenisvolle namen voor de groepen van het abonnement en resource (voor de toepassing en de netwerken). Hij maakt het volgende abonnement- en resourcekalenders groepen:

| Item | Naam | Beschrijving |
| ---- | ---- | ----------- |
| Abonnement | Contoso ETS DeveloperTools productie | Ondersteuning biedt voor algemene Tools voor ontwikkelaars |
| Resourcegroep | rgBitBucket | Bevat de web-toepassingsserver en database-server |
| Resourcegroep | rgCoreNetworks | Bevat de virtuele netwerken en site-naar-site gateway-verbinding |


### <a name="role-based-access-control"></a>Toegangsbeheer op basis van rollen

Na het maken van zijn abonnement wil Dave om ervoor te zorgen dat de juiste teams en toepassing eigenaren toegang hun resources tot hebben. Dave herkent dat elk team verschillende vereisten heeft. Hij maakt gebruik van de groepen die zijn gesynchroniseerd vanuit van Contoso on-premises Active Directory (AD) met Azure Active Directory en de juiste toegangsniveau voor de teams bevat. 

Dave wordt de volgende rollen voor het abonnement toegewezen: 

| Rol | Toegewezen aan | Beschrijving |
| ---- | ----------- | ----------- |
| [Eigenaar](./active-directory/role-based-access-built-in-roles.md#owner) | ID beheerd met de Contoso AD | Deze-ID wordt beheerd met slechts in tijd (JUST) toegang tot en met het hulpmiddel voor het beheer van de identiteit van van Contoso en zorgt ervoor dat abonnement eigenaar access volledig is gecontroleerd. |
| [Beveiliging Manager](./active-directory/role-based-access-built-in-roles.md#security-manager) | Beveiligings- en risico management afdeling | Deze functie kan gebruikers om te kijken naar Beveiligingscentrum Azure en de status van de resources. |
| [Netwerk Inzender](./active-directory/role-based-access-built-in-roles.md##network-contributor) | Netwerkteam | Deze functie kan van Contoso netwerkteam voor het beheren van de VPN van de Site naar Site en de virtuele netwerken. |
| *Aangepaste rol* | De eigenaar van de toepassing | Dave Hiermee maakt u een rol die biedt de mogelijkheid om te wijzigen van bronnen binnen de resourcegroep. Zie voor meer informatie, [Aangepaste rollen in Azure RBAC](./active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Beleidsregels

Dave heeft de volgende vereisten voor het beheren van resources in het abonnement:

- Omdat de ontwikkeling hulpmiddelen voor ondersteuning voor ontwikkelaars kant van de wereld zitten, wilt hij geen gebruikers resources worden gemaakt in een gebied blokkeren. Echter moet hij weten waar de resources worden gemaakt. 
- Hij is weerspiegeld kosten. Daarom wil hij voorkomen dat de eigenaren van de toepassing onnodig dure virtuele machines worden gemaakt.  
- Omdat deze toepassing ontwikkelaars in de vele bedrijfseenheden fungeert, wil hij melding geven voor elke resource met de eigenaar van het bedrijf per eenheid en -toepassing. Met behulp van de volgende codes kunt ETS in de juiste teams rekening.

Hij maakt het volgende [resourcemanager beleidsregels](resource-manager-policy.md): 

| Veld | Effect | Beschrijving |
| ----- | ------ | ----------- |
| locatie | controlelogboek | Het maken van de resources in elke regio controleren |
| type | weigeren | Het maken van G-serie virtuele machines weigeren |
| labels | weigeren | Toepassing eigenaar tag vereisen |
| labels | weigeren | De tag center kosten vereisen |
| labels | toevoegen | Labelnaam **Business Unit** en tagwaarde **ETS** toevoegen aan alle resources |


### <a name="resource-tags"></a>Resource-labels

Dave begrijpt dat hij moet hebben specifieke informatie over de factuur die u aan te geven van de kostenplaats voor de uitvoering BitBucket. Daarnaast kunnen wil Dave van alle resources die ETS eigenaar is van weten. 

De volgende [codes](resource-group-using-tags.md) voegt hij toe aan de resourcegroepen en resources. 
 
| Labelnaam | Tagwaarde |
| -------- | --------- |
| ApplicationOwner | De naam van de persoon die deze toepassing beheert. |
| CostCenter | De kostenplaats van de groep waartoe het Azure verbruik betaalt. |
| Business Unit | **ETS** (de eenheid voor bedrijven die zijn gekoppeld aan het abonnement) |

### <a name="core-network"></a>Core-netwerk

Het Contoso ETS informatie beveiligings- en risico managementteam reviseert Davids voorgestelde plan de toepassing Azure verplaatsen. Ze willen om ervoor te zorgen dat de toepassing niet wordt blootgesteld aan internet.  Dave, heeft ook ontwikkelaars-apps die in de toekomst wordt verplaatst naar Azure. Deze apps vereisen openbare interfaces.  Als u wilt aan deze vereisten voldoet, biedt hij zowel de interne als de externe virtuele netwerken en een beveiligingsgroep netwerk beperken van toegang.

Hij maakt de volgende bronnen:

| Resourcetype | Naam | Beschrijving |
| ------------- | ---- | ----------- |
| Virtual Network | vnInternal | Zorg dat de toepassing BitBucket gebruikt en via ExpressRoute is verbonden met het bedrijfsnetwerk van Contoso.  Een subnet (sbBitBucket) bevat de toepassing met een specifieke IP-adresruimte. |
| Virtual Network | vnExternal | Beschikbaar voor toekomstige toepassingen waarvoor openbare eindpunten. |
| Beveiligingsgroep netwerk | nsgBitBucket | Zorgt ervoor dat de aanvallen van deze werkzaamheden is geminimaliseerd doordat verbindingen alleen op poort 443 voor het subnet waar de toepassing woont (sbBitBucket). |

### <a name="resource-locks"></a>Resource-vergrendelingen 

Dave herkent dat de verbinding hebben met het bedrijfsnetwerk van Contoso naar het interne virtuele netwerk moet worden beveiligd tegen wayward script of per ongeluk wordt verwijderd. 

Hij maakt het volgende [resource vergrendelen](resource-group-lock-resources.md):

| Vergrendelingstype | Resource | Beschrijving |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnInternal | Voorkomen dat gebruikers het virtuele netwerk of subnetten verwijdert, maar de toevoeging van nieuwe subnetten niet voorkomt. |

### <a name="azure-automation"></a>Azure automatisering 

Dave heeft niets te automatiseren van deze toepassing. Hoewel hij een automatisering Azure-account hebt gemaakt, niet hij het eerste instantie gebruiken. 

### <a name="azure-security-center"></a>Azure Beveiligingscentrum 

Beheer van Contoso IT-service moet snel kunt herkennen en threats verwerken. Ze willen ook voor meer informatie over welke problemen zich kunnen voordoen.  

Om te voldoen aan deze vereisten is voldaan, Dave schakelt het [Beveiligingscentrum Azure](./security-center/security-center-intro.md)en biedt toegang tot de rol Security Manager. 

## <a name="scenario-2-customer-facing-app"></a>Scenario 2: klant is gericht-app

De business-leiderschap in de eenheid levering ketting bedrijven heeft verschillende mogelijkheden om uit te breiden betrokkenheid met de Contoso-klanten met behulp van een loyaliteitskaart geïdentificeerd. Lisa van team moet maken van deze toepassing en besluit dat Azure de mogelijkheid om te voldoen aan de zakelijke behoeften verhogen. Lisa werkt met Dave van ETS voor het configureren van twee abonnementen voor het ontwikkelen en gebruiken van deze toepassing.

### <a name="azure-subscriptions"></a>Azure-abonnementen 

Dave zich aanmelden bij de Portal van Azure Enterprise en ziet dat de levering ketting afdeling al bestaat.  Als dit project het eerste ontwikkelingsproject voor het team levering ketting in Azure wordt aangegeven is, herkent Dave echter dat u een nieuw account voor het ontwikkelteam van Lisa.  Hij wordt gemaakt van het account "R & D" voor haar team en toegang aan Lisa wordt toegewezen. Lisa Logboeken in via de portal Account en maakt u twee abonnementen: één waarin de Ontwikkelingsservers en één waarin de productieservers.  Zij voldoet aan de eerder vastgestelde naming standaarden bij het maken van de volgende abonnementen: 

| Gebruik van abonnement | Naam |
| ---------------- | ---- |
| Ontwikkeling | Supply Chain ResearchDevelopment LoyaltyCard ontwikkeling |
| Productie | Supply Chain bewerkingen LoyaltyCard productie |

### <a name="policies"></a>Beleidsregels

Dave en Lisa bespreken van de toepassing en dat deze toepassing alleen klanten in de regio Noord-Amerikaanse fungeert identificeren.  Lisa en haar teamsites plannen gebruik van de toepassing Service-omgeving en Azure SQL Azure van de toepassing maken. Mogelijk moet ze virtuele machines maken tijdens de ontwikkeling.  Lisa om ervoor te zorgen dat haar ontwikkelaars de resources die zijn vereist voor het verkennen en de bestudering van problemen zonder trekken in ETS hebben. 

Voor het **abonnement van de ontwikkeling**maken ze het volgende beleid:

| Veld | Effect | Beschrijving |
| ----- | ------ | ----------- |
| locatie | controlelogboek | Het maken van de resources in elke regio controleren. |

Ze doen geen afbreuk aan het type sku die een gebruiker in de ontwikkelingsfase bevindt maken kan en labels niet vereist voor alle resourcegroepen en resources.

Voor het **abonnement productie**maken ze de volgende beleidsitems:

| Veld | Effect | Beschrijving |
| ----- | ------ | ----------- |
| locatie | weigeren | Het maken van resources buiten de VS datacenters weigeren. |
| labels | weigeren | Toepassing eigenaar tag vereisen |
| labels | weigeren | Afdeling tag vereisen. | 
| labels | toevoegen | Tag toevoegen aan elke resourcegroep waarmee wordt aangegeven productieomgeving. |

Het type van een gebruiker in productie maken kunt sku beperken niet.

### <a name="resource-tags"></a>Resource-labels 

Dave begrijpt dat hij moet hebben specifieke informatie om aan te geven van de juiste bedrijven groepen voor de facturering en eigendom. Hij Hiermee definieert u labels van de resource voor resourcegroepen en resources. 
 
Labelnaam | Tagwaarde |
| -------- | --------- |
| ApplicationOwner | De naam van de persoon die deze toepassing beheert. |
| Afdeling | De kostenplaats van de groep waartoe het Azure verbruik betaalt. |
| EnvironmentType | **Productie** (Hoewel het abonnement **productie** in de naam bevat, met inbegrip van dit label kan gemakkelijk kunnen worden geïdentificeerd wanneer resources in de portal of op de factuur kijken.) |

### <a name="core-networks"></a>Core-netwerken te gebruiken

Het Contoso ETS informatie beveiligings- en risico managementteam reviseert Davids voorgestelde plan de toepassing Azure verplaatsen. Ze willen om ervoor te zorgen dat de toepassing loyaliteitskaart correct is geïsoleerd en in een netwerk DMZ beveiligd.  Maak een extern virtueel netwerk en een beveiligingsgroep netwerk de toepassing loyaliteitskaart met het bedrijfsnetwerk Contoso isoleren om te voldoen aan deze vereiste, Dave en Lisa.  

Voor het **abonnement van de ontwikkeling**maken ze:

| Resourcetype | Naam | Beschrijving |
| ------------- | ---- | ----------- |
| Virtual Network | vnInternal | De Contoso-loyaliteitskaart ontwikkelomgeving fungeert en via ExpressRoute is verbonden met het bedrijfsnetwerk van Contoso. |

Voor het **abonnement productie**maken ze:

| Resourcetype | Naam | Beschrijving |
| ------------- | ---- | ----------- |
| Virtual Network | vnExternal | De toepassing loyaliteitskaart gehost en is niet verbonden rechtstreeks met ExpressRoute van Contoso. Code is verplaatst via hun systeem broncode rechtstreeks naar de PaaS-services. |
| Beveiligingsgroep netwerk | nsgBitBucket | Zorgt ervoor dat het aanvalsoppervlak van de werklast door alleen toestaan in gebonden communicatie op TCP 443 wordt geminimaliseerd.  Contoso is ook wordt onderzocht met behulp van een Firewall van de toepassing Web voor extra beveiliging. |  

### <a name="resource-locks"></a>Resource-vergrendelingen 

Dave en Lisa verlenen en besluit toe te voegen resource vergrendelingen in enkele van de belangrijkste bronnen in de omgeving om te voorkomen dat per ongeluk wordt verwijderd tijdens een onjuiste code push. 

Ze maken de volgende vergrendelen:

| Vergrendelingstype | Resource | Beschrijving |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnExternal | Voorkomen dat personen de virtual network of subnetten verwijderen. De vergrendeling voorkomt niet dat voor het toevoegen van nieuwe subnetten. |

### <a name="azure-automation"></a>Azure automatisering 

Lisa en haar ontwikkelteam hebt de uitgebreide runbooks voor het beheren van de omgeving van deze toepassing. De runbooks toestaan voor het toevoegen/verwijderen van knooppunten voor de toepassing en andere taken DevOps. 

Als u wilt deze runbooks gebruiken, kunnen [automatisering](./automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure Beveiligingscentrum 

Beheer van Contoso IT-service moet snel kunt herkennen en threats verwerken. Ze willen ook voor meer informatie over welke problemen zich kunnen voordoen.  

Om te voldoen aan deze vereisten is voldaan, kan Dave Azure Beveiligingscentrum. Hij zorgt ervoor dat de Beveiligingscentrum Azure is de bronnen voor controle en toegang tot de teams DevOps en beveiliging biedt. 

## <a name="next-steps"></a>Volgende stappen

- Zie [Aanbevolen procedures voor het maken van Azure resourcemanager sjablonen](resource-manager-template-best-practices.md)voor meer informatie over het maken van sjablonen van de Resource Manager.

*[Karel Kuhnhausen](https://github.com/karlkuhnhausen) bijgedragen aan de in dit onderwerp.*
