<properties
   pageTitle="Routeren vereisten voor ExpressRoute | Microsoft Azure"
   description="Deze pagina vindt uitgebreide vereisten voor het configureren en beheren van routering voor ExpressRoute circuits."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>Vereisten voor het routeren van ExpressRoute  

Als u wilt verbinden met Microsoft cloudservices ExpressRoute gebruiken, moet u instellen en beheren-mailroutering. Sommige connectivity-providers bieden deze instellen en beheren als een beheerde service-mailroutering. Neem contact op met uw provider connectivity om te zien als ze deze service bieden. Zo niet, kunt u moet voldoen aan de volgende vereisten. 

Raadpleeg het artikel [Circuits en routeren domeinen](expressroute-circuit-peerings.md) voor een beschrijving van de routering sessies die worden ingesteld moeten in om verbinding te vereenvoudigen.

>[AZURE.NOTE] Microsoft biedt geen ondersteuning voor elke router redundantie protocollen (bijvoorbeeld HSRP, VRRP) voor beschikbaarheid configuraties. We afhankelijk een paar overtollige BGP-sessies per peering beschikbaarheid.

## <a name="ip-addresses-used-for-peerings"></a>IP-adressen die wordt gebruikt voor peerings

U moet een paar blokken IP-adressen voor het configureren van routering tussen uw netwerk en van Microsoft Enterprise rand (MSEEs) routers reserveren. In dit gedeelte vindt u een lijst met vereisten en beschrijft de regels met betrekking tot hoe deze IP-adressen moeten worden opgehaald en gebruikt.

### <a name="ip-addresses-used-for-azure-private-peering"></a>IP-adressen die wordt gebruikt voor Azure privé peering

U kunt privé IP-adressen of openbare IP-adressen voor het configureren van de peerings. De adresbereik dat is gebruikt voor het configureren van routes moet niet overlappen met adresbereiken gebruikt om te maken van virtuele netwerken in Azure wordt aangegeven. 

 - U moet een /29 reserveren subnet of twee /30 subnetten voor routeren interfaces.
 - De subnetten die wordt gebruikt voor de routering kunnen zijn privé IP-adressen of openbare IP-adressen.
 - De subnetten mag niet conflicteren met het bereik gereserveerd door de klant voor gebruik in de Microsoft-cloud.
 - Als een /29 subnet wordt gebruikt, wordt deze gesplitst in twee /30 subnetten. 
     - De eerste /30 subnet worden gebruikt voor de primaire koppeling en de tweede/30 subnet wordt gebruikt voor de secundaire koppeling.
     - Voor elk van de /30 subnetten, moet u het eerste IP-adres van de /30 subnet op uw router. Microsoft gebruikt de tweede IP-adres van de /30 subnet voor het instellen van een sessie BGP.
     - U moet beide BGP sessies voor onze [beschikbaarheid SLA](https://azure.microsoft.com/support/legal/sla/) geldig instellen.  

#### <a name="example-for-private-peering"></a>Voorbeeld voor persoonlijke peering

Als u besluit om a.b.c.d/29 gebruiken voor het instellen van de peering, wordt deze gesplitst in twee /30 subnetten. Klik in het onderstaande voorbeeld gaan we hoe het subnet a.b.c.d/29 worden gebruikt. 

a.b.c.d/29 worden gesplitst a.b.c.d/30 en a.b.c.d+4/30 en doorgegeven naar Microsoft via de inrichten API's. U zult a.b.c.d+1 als de VRF IP gebruiken voor de primaire PE en Microsoft a.b.c.d+2 gebruikt als de VRF IP voor de primaire MSEE. U zult a.b.c.d+5 als de VRF IP gebruiken voor de secundaire PE en Microsoft a.b.c.d+6 wordt gebruikt als de VRF IP voor de secundaire MSEE.

Houd rekening met een zaak waarin u 192.168.100.128/29 voor het instellen van het privé peering selecteren. 192.168.100.128/29 bevat adressen uit 192.168.100.128 192.168.100.135, waaronder:

- 192.168.100.128/30 worden toegewezen aan link1, met provider met 192.168.100.129 en Microsoft 192.168.100.130 gebruiken.
- 192.168.100.132/30 worden toegewezen aan Koppeling2, met provider met 192.168.100.133 en Microsoft 192.168.100.134 gebruiken.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>IP-adressen die worden gebruikt voor Azure openbare en Microsoft peering

Voor het instellen van de sessies BGP moet u openbare IP-adressen dat u eigenaar bent. Microsoft moet mogelijk om te controleren of de eigenaar van het IP-adressen via Internet registervermeldingen Routering en Internet routering registervermeldingen. 

- Hebt u een unieke/29 subnet of twee /30 subnetten voor het instellen van het BGP peering voor elke peering per ExpressRoute circuit (als u meer dan één hebt). 
- Als een /29 subnet wordt gebruikt, wordt deze gesplitst in twee /30 subnetten. 
    - De eerste /30 subnet worden gebruikt voor de primaire koppeling en de tweede/30 subnet wordt gebruikt voor de secundaire koppeling.
    - Voor elk van de /30 subnetten, moet u het eerste IP-adres van de /30 subnet op uw router. Microsoft gebruikt de tweede IP-adres van de /30 subnet voor het instellen van een sessie BGP.
    - U moet beide BGP sessies voor onze [beschikbaarheid SLA](https://azure.microsoft.com/support/legal/sla/) geldig instellen.

## <a name="public-ip-address-requirement"></a>Openbare IP-adres vereiste 

### <a name="private-peering"></a>Persoonlijke Peering 

U kunt openbaar of privé IPv4-adressen voor privé peering gebruiken. Wij bieden end-to-end moeten worden geïsoleerd van uw verkeer zodat overlappende adressen met andere klanten niet mogelijk is voor het geval privé peering is. Deze adressen zijn niet aangekondigd met Internet. 

### <a name="public-peering"></a>Openbare Peering

Het Azure openbare peering pad kunt u verbinding maken met alle services gehost in Azure wordt aangegeven via de openbare IP-adressen. Hierbij services weergegeven in de [Veelgestelde vragen over ExpessRoute](expressroute-faqs.md) en alle services die worden gehost door ISV's op Microsoft Azure. Connectiviteit met Microsoft Azure-services op openbare peering wordt altijd vanaf uw netwerk bij het Microsoft-netwerk gestart. U moet openbare IP-adressen gebruiken voor het verkeer naar Microsoft-netwerk.

### <a name="microsoft-peering"></a>Microsoft Peering

Het Microsoft peering pad kunt u verbinding maken met Microsoft-cloudservices die niet worden ondersteund door het Azure openbare peering pad. De lijst met services bevat Office 365-services, zoals Exchange Online, SharePoint Online, Skype voor bedrijven en CRM Online. Microsoft ondersteuning biedt voor bidirectionele verbinding aan de Microsoft-peering. Microsoft-cloudservices verkeer moet geldige openbare IPv4-adressen gebruiken voordat ze deelnemen aan het Microsoft-netwerk.

Zorg ervoor dat uw IP-adres en als getal zijn geregistreerd voor u in een van de onderstaande registervermeldingen.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] Openbare IP-adressen aangekondigd naar Microsoft via ExpressRoute moeten niet aangekondigd met Internet. Dit kan connectiviteit met andere services van Microsoft verbreken. Openbare IP-adressen die worden gebruikt door servers in uw netwerk die met O365 eindpunten in Microsoft communiceren kunnen echter aangekondigd via ExpressRoute. 

## <a name="dynamic-route-exchange"></a>Dynamische route exchange

Routeren exchange zijn via eBGP-protocol. EBGP-sessies zijn geconfigureerd tussen de MSEEs en uw routers. Verificatie van BGP sessies is niet vereist. Indien nodig, kan een MD5-hash kan worden geconfigureerd. Zie de [Routering van configureren](expressroute-howto-routing-classic.md) en de [inrichting van Circuitlijnen werkstromen en circuitlijnen Staten](expressroute-workflows.md) voor informatie over het configureren van BGP sessies.

## <a name="autonomous-system-numbers"></a>Zelfstandige systeem getallen

Microsoft gebruikt als 12076 voor openbare, Azure Azure privé en Microsoft peering. We hebben ASN's gereserveerd in 65515 naar 65520 voor interne gebruik. 16 zowel 32-bits als getallen worden ondersteund.

Er zijn geen vereisten rond gegevens doorverbinden symmetrie. De paden doorsturen en afzender mogelijk verschillende router paren bladeren. Identieke routes moeten over meerdere circuitlijnen paren die deel uitmaken van u aangekondigd van beide zijden. Route aan de doelstellingen moeten zijn niet identiek zijn.

## <a name="route-aggregation-and-prefix-limits"></a>Limieten voor aggregatie en voorvoegsel routeren

Ondersteunen we tot 4000 voorvoegsels voor eenheden aangekondigd met ons via het Azure privé peering. Dit kan worden verhoogd maximaal 10.000 voorvoegsels voor eenheden als de ExpressRoute premium-invoegtoepassing is ingeschakeld. We accepteren maximaal 200 voorvoegsels per BGP sessie voor openbare Azure en Microsoft peering. 

Als het aantal voorvoegsels voor eenheden groter is dan de limiet van de sessie BGP verwijderd. We worden standaardroutes naar alleen de persoonlijke peering koppeling geaccepteerd. Provider moet uitfilteren en standaardroute en privé IP-adressen (RFC 1918) van de openbare Azure en peering paden van Microsoft. 

## <a name="transit-routing-and-cross-region-routing"></a>De overdracht Routering en cross-regio routering

ExpressRoute kan niet worden geconfigureerd als overdracht routers. U moet zijn afhankelijk van uw connectivity-provider voor de overdracht routeren services.

## <a name="advertising-default-routes"></a>Reclame standaardroutes

Standaardroutes mogen alleen op Azure privé peering sessies. In dat geval wordt we routeert alle verkeer van de bijbehorende virtuele netwerken naar uw netwerk. Standaardroutes advertenties in privé peering resulteert in het pad internet van Azure wordt geblokkeerd. U moet zijn afhankelijk van uw bedrijf rand voor het routeren van verkeer van en tot internet voor services die worden gehost in Azure wordt aangegeven. 

 Als u wilt inschakelen connectiviteit met andere Azure mailservices en -infrastructuurservices, moet u ervoor zorgen dat een van de volgende items op hun plaats staan is:

 - Azure openbare peering is ingesteld voor het routeren verkeer naar openbare eindpunten
 - Kunt u de gebruiker gedefinieerde routering internetconnectiviteit voor elke subnet vereisen internetconnectiviteit toestaan.
 
>[AZURE.NOTE] Standaardroutes advertenties worden afgebroken Windows en andere VM licentieactivering. Volg de instructies [hier](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) om dit te omzeilen.

## <a name="support-for-bgp-communities-preview"></a>Ondersteuning voor BGP community's (Preview)


Deze sectie bevat een overzicht van hoe BGP community's worden gebruikt met ExpressRoute. Microsoft zal routes in de openbare en Microsoft peering paden met routes gelabeld met het juiste Communitywaarden gebruiken. Waarom u dit doet en de details op Communitywaarden worden hieronder beschreven. Microsoft, echter niet van toepassing geen gelabeld naar routes aangekondigd naar Microsoft Communitywaarden.

Als u verbinding via ExpressRoute op elke één peering plek binnen een geopolitieke gebied aan Microsoft maakt, hebt u toegang tot alle Microsoft cloudservices in alle regio's binnen de geopolitieke grens. 

Als u met Microsoft in Amsterdam tot en met ExpressRoute verbonden, hebt u bijvoorbeeld toegang tot alle Microsoft cloudservices gehost in Noord-Europa en West Europa. 

Verwijzen naar de pagina [ExpressRoute partners en peering locaties](expressroute-locations.md) voor een gedetailleerde lijst van geopolitieke regio's, gekoppeld Azure regio's en bijbehorende ExpressRoute peering locaties.

U kunt meer dan één ExpressRoute circuitlijnen per geopolitieke regio kopen. Meerdere verbindingen hebt, kunt u grote voordelen op beschikbaarheid vanwege geografische redundantie. In gevallen waarin u meerdere ExpressRoute circuits hebt, ontvangt u dezelfde reeks voorvoegsels voor eenheden aangekondigd van Microsoft op de openbare peering en Microsoft peering paden. Dit betekent dat u hebt meerdere paden vanuit uw netwerk in Microsoft. Hierdoor mogelijk sub optimale routeren beslissingen worden aangebracht in uw netwerk. Hierdoor kunnen optreden sub optimale connectivity ervaringen met andere services. 

Microsoft zal voorvoegsels voor eenheden via openbare peering markeren en Microsoft peering met de juiste BGP Communitywaarden die aangeeft dat de regio de voorvoegsels hebt ondergebracht. U kunt de Communitywaarden naar het juiste routeren besluiten om te bieden [optimale zoekresultaten omleiden naar klanten](expressroute-optimize-routing.md)te rekenen.

| **Geopolitieke regio** | **Microsoft Azure regio** | **Waarde van de Gemeenschap BGP** |
|---|---|---|
| **Noord-Amerika** |    |  |
|    | Oost-VS | 12076:51004 |
|    | Oost-Amerikaanse 2 | 12076:51005 |
|    | West VS | 12076:51006 |
|    | West Amerikaans 2 | 12076:51026 |
|    | West Centraal VS | 12076:51027 |
|    | Centraal Noord-Amerikaanse | 12076:51007 |
|    | Zuid-Central US | 12076:51008 |
|    | Central US | 12076:51009 |
|    | Canada centraal | 12076:51020 |
|    | Canada Oost | 12076:51021 |
| **Zuid-Amerika** |  |  |
|    | Brazilië Zuid | 12076:51014 |
| **Europa** |    |  |
|    | Noord-Europa | 12076:51003 |
|    | West Europa | 12076:51002 |
| **Azië en stille** |    |   |
|    | Oost-Azië | 12076:51010 |
|    | Zuidoost-Azië | 12076:51011 |
| **Japan** |     |   |
|    | Japan Oost | 12076:51012 |
|    | Japan West | 12076:51013 |
| **Australië** |    |   | 
|    | Australië Oost | 12076:51015 |
|    | Australië Zuidoost | 12076:51016 |
| **India** |    |   |
|    | India Zuid | 12076:51019 |
|    | India West | 12076:51018 |
|    | India centraal | 12076:51017 |

Alle routes aangekondigd van Microsoft zal zijn gemarkeerd met de juiste community-waarde. 

>[AZURE.IMPORTANT] Globale voorvoegsels voor eenheden wordt zijn gemarkeerd met een juiste community-waarde en wordt aangekondigd alleen wanneer ExpressRoute premium-invoegtoepassing is ingeschakeld.


Naast het bovenstaande, op basis van de service waartoe ze behoren voorvoegsels voor eenheden wordt ook labelen door Microsoft. Dit geldt alleen voor de Microsoft-peering. De onderstaande tabel bevat een toewijzing van waarde waarnaar BGP-community-service.

| **Service** | **Waarde van de Gemeenschap BGP** |
|---|---|
| **Exchange** | 12076:5010 |
| **SharePoint** | 12076:5020 |
| **Skype voor bedrijven** | 12076:5030 |
| **CRM Online** | 12076:5040 |
| **Andere Office 365-Services** | 12076:5100 |

>[AZURE.NOTE] Microsoft heeft geen BGP community-waarden die u op de routes aangekondigd naar Microsoft instelt niet worden uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

- Configureer uw ExpressRoute verbinding.

    - [Een circuitlijnen ExpressRoute voor het implementatiemodel klassieke maken](expressroute-howto-circuit-classic.md) of [maken en wijzigen van een ExpressRoute circuitlijnen met Azure Resource Manager](expressroute-howto-circuit-arm.md)
    - [Configureren voor het implementatiemodel klassieke routering](expressroute-howto-routing-classic.md) of [omleiding voor het implementatiemodel resourcemanager configureren](expressroute-howto-routing-arm.md)
    - [Koppeling een klassieke VNet naar een circuitlijnen ExpressRoute](expressroute-howto-linkvnet-classic.md) of [koppeling een resourcemanager VNet naar een circuitlijnen ExpressRoute](expressroute-howto-linkvnet-arm.md)


