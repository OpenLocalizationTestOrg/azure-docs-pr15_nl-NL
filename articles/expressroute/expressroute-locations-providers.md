<properties
   pageTitle="ExpressRoute locaties | Microsoft Azure"
   description="Dit artikel bevat een gedetailleerd overzicht van locaties waar services worden aangeboden en hoe u verbinding maken met Azure regio's."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute partners en peering locaties

De tabellen in dit artikel vindt informatie over ExpressRoute connectivity providers, ExpressRoute geografische spreiding, Microsoft-cloudservices ondersteund via ExpressRoute en ExpressRoute systeemintegrators (SIs).

## <a name="partners"></a>ExpressRoute connectivity providers

ExpressRoute wordt ondersteund in alle regio's van Azure locaties. De volgende map bevat een lijst met Azure regio's en ExpressRoute locaties. ExpressRoute locaties verwijzen naar die waar Microsoft collega's met verschillende serviceproviders.

![Locatie-kaart][0]

Hebt u toegang tot Azure services in alle regio's binnen een geopolitieke gebied als er verbinding is met ten minste één ExpressRoute locatie binnen de geopolitieke regio. De volgende tabel bevat een overzicht van een Azure regio's naar ExpressRoute locaties in een geopolitieke regio.

|**Geopolitieke regio**|**Azure regio 's**|**ExpressRoute locaties**|
|---|---|---|
|**Noord-Amerika**|Oost-Amerikaanse, West Amerikaans, Oost Amerikaans 2, Central US, Zuid-Central US, centraal Noord-Amerikaanse, Canada centraal, Canada Oost|Amsterdam, Chicago, Dallas, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington domeincontroller, Montreal +, Quebec stad +, Amsterdam|
|**Zuid-Amerika**|Brazilië Zuid|Sao Paulo|
|**Europa**|Noord-Europa, West Europa, Verenigd Koninkrijk West, Verenigd Koninkrijk Zuid|Amsterdam, Dublin, Londen, Newport(Wales) +, Parijs|
|**Azië**|Oost-Aziatische, Zuidoost-Azië|Hongkong, Singapore|
|**Japan**|Japanse West Japan Oost|Osaka, Tokyo|
|**Australië**|Australië Zuidoost, Australië Oost|Melbourne, Sydney|
|**India**|India West, India centraal, India Zuid|Chennai, Mumbai|



De onderstaande tabel wordt aandacht besteed aan regio's en geopolitieke grenzen voor nationale wolken.

|**Geopolitieke regio**|**Azure regio 's**|**ExpressRoute locaties**|
|---|---|---|---|
|**Amerikaanse overheid cloud**|Amerikaans beurs Iowa, V.S. beurs Virginia|Arnhem, Dallas, New York, Washington domeincontroller|
|**China**|China noorden, China Oost|Beijing, Shanghai|
|**Duitsland**|Duitsland centraal, Duitsland Oost|Berlijn, Frankfurt|


Connectiviteit tussen geopolitieke regio's wordt niet ondersteund in de standaard ExpressRoute SKU. Moet u de invoegtoepassing ExpressRoute premium ter ondersteuning van globale verbindingen inschakelen. Connectiviteit met nationale cloud omgevingen wordt niet ondersteund. U kunt werken met uw provider connectivity als deze nodig.


## <a name="connectivity-provider-locations"></a>Connectiviteit provider locaties

> [AZURE.SELECTOR]
[Locaties per Provider](expressroute-locations.md#connectivity-provider-locations)
[Providers door locatie](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>Productie Azure
| **Locatie**  | **Serviceproviders** |
|---------------|-----------------------|
| **Amsterdam** | Aryaka netwerken, AT & T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Internet oplossingen - Cloud verbinding kunt maken, Interxion, niveau 3 communicatie, oranje, Tata communicatie, TeleCity groep Telenor, Verizon |
| **Assen** | Equinix |
| **Chennai** | Tata communicatie |
| **Chicago** | AT & T NetBond, Comcast, Equinix, niveau 3 communicatie, Zayo groep |
| **Den Bosch** | AT & T NetBond, Cologix, Equinix, niveau 3 communicatie Megaport |
| **Dublin** | Colt, Telecity groep |
| **Hongkong** | British Telecom, China Telecom globale, Equinix, Megaport, oranje, PCCW globale beperkt, Tata communicatie Verizon |
| **Londen** | AT & T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud verbinding kunt maken, Interxion, Jisc, niveau 3 communicatie, MTN, NTT communicatie, oranje, Tata communicatie, Telecity groep Telenor, Verizon, Vodafone |
| **Las Vegas** | Niveau 3 communicatie +, Megaport
| **Los Angeles** | CoreSite, Equinix, Megaport, NTT, Zayo groep |
| **Melbourne** | AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New York** | Equinix, Megaport, Zayo groep |
| **Newport(Wales) +** | Volgende generatie gegevens + |
| **Montreal** | Cologix + |
| **Mumbai** | Tata communicatie |
| **Osaka** | Equinix, Internet initiatief Japan Inc. - IIJ, NTT communicatie Softbank |
| **Parijs** | Interxion, Equinix + |
| **Sao Paulo** | Equinix, Telefonica |
| **Seattle** | Equinix, niveau 3 communicatie Megaport |
| **Silicon Valley** | Aryaka-netwerken te gebruiken, AT & T NetBond, British Telecom, CenturyLink +, Comcast, Equinix, niveau 3 communicatie, oranje, Tata communicatie, Verizon, Zayo groep |
| **Singapore** | Aryaka-netwerken te gebruiken, AT & T NetBond, British Telecom, Equinix, InterCloud, Megaport, oranje, SingTel, Tata communicatie, Verizon |
| **Sydney** | AARNet, AT & T NetBond, British Telecom, Equinix, Megaport, NEXTDC, oranje, Telstra Corporation, Verizon |
| **Tokyo** | Aryaka-netwerken te gebruiken, British Telecom, Colt, Equinix, Internet initiatief Japan Inc. - IIJ, NTT communicatie, Softbank, Verizon |
| **Amsterdam** | Cologix, Equinix, Zayo groep |
| **Washington domeincontroller** | Aryaka-netwerken te gebruiken, AT & T NetBond, British Telecom, Comcast, Equinix, InterCloud, niveau 3 communicatie, Megaport, oranje, Tata communicatie, Verizon, Zayo groep |

 **+**Hiermee wordt aangegeven binnenkort beschikbaar

### <a name="national-cloud-environments"></a>Nationale cloud omgevingen

#### <a name="us-government-cloud"></a>Amerikaanse overheid cloud

| **Locatie**  |**Serviceproviders** |
|---------------|--------------------|
| **Chicago** | AT & T NetBond, Equinix, niveau 3 communicatie Verizon |
| **Den Bosch** |  Equinix, Verizon + |
| **New York** | Equinix, niveau 3 communicatie +,-Verizon |
| **Washington domeincontroller** | AT & T NetBond, Equinix, niveau 3 communicatie Verizon |

#### <a name="china"></a>China

| **Locatie**  | **Serviceproviders** |
|---------------|-----------------------|
| **Peking** | China Telecom |
| **Shanghai** |  China Telecom |
Meer informatie raadpleegt u [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>Duitsland

| **Locatie**  | **Serviceproviders** |
|---------------|-----------------------|
| **Berlijn** | Colt +, e-bescherming |
| **Frankfurt** | Colt, Equinix, Interxion |

## <a name="nonpartners"></a>Verbindingen via serviceproviders niet wordt vermeld

Als uw provider connectivity niet in de vorige gedeelten staat, kunt u nog steeds een verbinding maken.

- Neem contact op met uw provider connectivity om te zien als ze zijn verbonden met een van de uitwisseling in de tabel hierboven. U kunt de volgende koppelingen om meer informatie over services van exchange-providers verzamelen controleren. Verschillende connectivity providers zijn al verbonden met Ethernet-mailwisselingen.

    - [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Uw connectivity-provider Breid uw naar de peering locatie van keuze hebt.
    - Er zeker van te dat uw provider connectivity uw connectivity ten zeerste beschikbaar zodanig dat er geen potentiële risico zijn.
- Bestel een circuitlijnen ExpressRoute met de exchange als uw provider connectivity verbinding maken met Microsoft.
    - Volg de stappen in [een circuitlijnen ExpressRoute maken](expressroute-howto-circuit-classic.md) voor het instellen van connectivity.

|**Locatie**|**Exchange**|**Connectiviteit Providers**|
|-------------|------------|-------------------------|
| **New York** | Equinix | Lightower |
| **Seattle** | Equinix | Alaska communicatie |
| **Silicon Valley** | Equinix | XO communicatie |
| **Singapore** | Equinix | 1CLOUDSTAR |
| **Washington domeincontroller** | Equinix | Lightower |

## <a name="expressroute-system-integrators"></a>ExpressRoute systeemintegrators

Privé connectiviteit met aan uw wensen voldoet inschakelen kan lastig zijn, op basis van de schaal van uw netwerk. U kunt werken met een van de systeemintegrators vermeld in de volgende tabel om u te helpen met onboarding naar ExpressRoute.

|**Continent**|**Systeemintegrators**|
|-------------|---------------------|
| **Azië** | Avanade Inc., OneAs1a|
| **Europa** | Avanade Inc., Dotnet oplossingen|
| **ONS** | Avanade Inc., Equinix Professional Services, Perficient, Project leiderschap|

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)voor meer informatie over ExpressRoute.
- Zorg ervoor dat alle voorwaarden wordt voldaan. Zie [ExpressRoute vereisten](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Locatie-kaart"
