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

| **Serviceprovider**  |**Microsoft Azure** | **Office 365 en CRM Online** | **Locaties** |
|-----------------------|--------------------|----------------|---------------|
| **AARNet** | Ondersteund | Ondersteund | Melbourne, Sydney |
| **[Aryaka-netwerken te gebruiken]( http://www.aryaka.com/)** | Ondersteund | Ondersteund | Amsterdam, Silicon Valley, Singapore, Tokyo, Washington domeincontroller |
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Ondersteund | Ondersteund | Amsterdam, Chicago, Dallas, Londen, Silicon Valley, Singapore, Sydney, Washington domeincontroller |
| **[British Telecom]( http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** | Ondersteund | Ondersteund | Amsterdam, Hongkong, Londen, Silicon Valley, Singapore, Sydney, Tokyo, Washington domeincontroller |
|**CenturyLink** | Binnenkort beschikbaar | Binnenkort beschikbaar| Silicon Valley |
|**China Telecom globale** | Ondersteund | Niet ondersteund | Hongkong |
|**[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** | Ondersteund | Binnenkort beschikbaar | Dallas, Montreal +, Amsterdam |
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)**  |  Ondersteund | Ondersteund | Amsterdam, Dublin, Londen, Tokyo |
| **Comcast** | Ondersteund | Ondersteund | Arnhem, Silicon Valley, Washington domeincontroller |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** | Ondersteund | Ondersteund | Los Angeles | 
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Ondersteund | Ondersteund | Amsterdam, Amsterdam, Chicago, Dallas, Hongkong, Londen, Los Angeles, Melbourne, New York, Osaka, Parijs +, Sao Paulo, Seattle, Silicon Valley, Singapore, Sydney, Tokyo, Amsterdam, Washington domeincontroller |
| **euNetworks** |  Ondersteund | Ondersteund | Amsterdam |
| **GÉANT** | Ondersteund | Ondersteund | Amsterdam |
| **[Internet initiatief Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |  Ondersteund | Ondersteund | Osaka, Tokyo |
| **[InterCloud]( https://www.intercloud.com/)** | Ondersteund | Ondersteund | Amsterdam, Londen, Singapore, Washington domeincontroller |
| **Oplossingen voor Internet - Cloud verbinding maken** | Ondersteund | Ondersteund | Amsterdam, Londen |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)**  | Ondersteund | Ondersteund | Amsterdam, Londen, Parijs |
| **Jisc** | Ondersteund | Ondersteund | Londen | 
| **[Communicatie met niveau 3]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Ondersteund | Ondersteund | Amsterdam, Chicago, Dallas, Las Vegas +, Londen, Seattle, Silicon Valley, Washington domeincontroller |
| **Megaport** | Ondersteund | Ondersteund | Dallas, Hong Kong Las Vegas, Los Angeles, Melbourne, New York, Seattle, Singapore, Sydney, Washington domeincontroller |
| **MTN** | Ondersteund | Ondersteund | Londen |
| **Volgende generatie gegevens** | Binnenkort beschikbaar | Binnenkort beschikbaar | Newport(Wales) + |
| **NEXTDC** | Ondersteund | Ondersteund | Melbourne, Sydney |
| **NTT communicatie** | Ondersteund | Ondersteund | Londen, Los Angeles, Osaka, Tokyo |
| **[Oranje]( http://www.orange-business.com/en/products/business-vpn-galerie)** | Ondersteund | Ondersteund | Amsterdam, Hongkong, Londen, Silicon Valley, Singapore, Sydney, Washington domeincontroller |
| **Globale PCCW beperkt** | Ondersteund | Ondersteund | Hongkong |
| **[SingTel]( http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |  Ondersteund | Ondersteund | Singapore |
| **Softbank** | Ondersteund | Ondersteund | Osaka, Tokyo | 
| **[Tata communicatie](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** | Ondersteund | Ondersteund | Amsterdam, Chennai, Hongkong, Londen, Mumbai, Silicon Valley, Singapore, Washington domeincontroller |
| **[Groep TeleCity]( http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** | Ondersteund | Ondersteund | Amsterdam, Dublin, Londen |
| **Telefonica** | Ondersteund | Ondersteund | Sao Paulo |
| **Telenor** | Ondersteund | Ondersteund | Amsterdam, Londen |
| **[Telstra Corporation]( http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** | Ondersteund | Ondersteund | Melbourne, Sydney |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** | Ondersteund | Ondersteund | Amsterdam, Hongkong, Londen, Silicon Valley, Singapore, Sydney, Tokyo, Washington domeincontroller |
| **Vodafone** | Ondersteund | Niet ondersteund | Londen | 
| **[Groep Zayo]( http://www.zayo.com/solutions/industries/connect-to-cloud-data-centers/cloud-connectivity/microsoft-expressroute/)** | Ondersteund | Ondersteund | Arnhem, Los Angeles, New York, Silicon Valley, Amsterdam, Washington domeincontroller |

 **+**Hiermee wordt aangegeven binnenkort beschikbaar

### <a name="national-cloud-environments"></a>Nationale cloud omgevingen

#### <a name="us-government-cloud"></a>Amerikaanse overheid cloud

| **Serviceprovider**  |**Microsoft Azure** | **Office 365** | **Locaties** |
|-----------------------|--------------------|----------------|---------------|
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Ondersteund | Ondersteund | Chicago, Washington domeincontroller |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Ondersteund | Ondersteund | Arnhem, Dallas, New York, Washington domeincontroller |
| **[Communicatie met niveau 3]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Ondersteund | Ondersteund | Chicago, New York +, Washington domeincontroller |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** | Ondersteund | Ondersteund | Arnhem, Den Bosch +, New York, Washington domeincontroller |

#### <a name="china"></a>China

| **Serviceprovider**  |**Microsoft Azure** | **Office 365** | **Locaties** |
|-----------------------|--------------------|----------------|---------------|
| **China Telecom** | Ondersteund | Niet ondersteund | Beijing, Shanghai|
Meer informatie raadpleegt u [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/).

#### <a name="germany"></a>Duitsland

| **Serviceprovider**  |**Microsoft Azure** | **Office 365** | **Locaties** |
|-----------------------|--------------------|----------------|---------------|
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** | Ondersteund | Niet ondersteund | Berlijn +, Frankfurt|
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Ondersteund | Niet ondersteund | Frankfurt|
| **e-bescherming** | Ondersteund | Niet ondersteund | Berlijn|
| **Interxion** | Ondersteund | Niet ondersteund | Frankfurt|

## <a name="nonpartners"></a>Verbindingen via serviceproviders niet wordt vermeld

Als uw provider connectivity niet in de vorige gedeelten staat, kunt u nog steeds een verbinding maken.

- Neem contact op met uw provider connectivity om te zien als ze zijn verbonden met een van de uitwisseling in de tabel hierboven. U kunt de volgende koppelingen om meer informatie over services van exchange-providers verzamelen controleren. Verschillende connectivity providers zijn al verbonden met Ethernet-mailwisselingen.

    - [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Uw connectivity-provider Breid uw naar de peering locatie van keuze hebt.
    - Er zeker van te dat uw provider connectivity uw connectivity ten zeerste beschikbaar zodanig dat er geen potentiële risico zijn.
- Bestel een circuitlijnen ExpressRoute met de exchange als uw provider connectivity verbinding maken met Microsoft.
    - Volg de stappen in [een circuitlijnen ExpressRoute maken](expressroute-howto-circuit-classic.md) voor het instellen van connectivity.

|**Connectivity-provider**|**Exchange**|**Locaties**|
|---|---|---|
|**[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)**|Equinix|Singapore|
|**Alaska communicatie**|Equinix|Seattle|
|**[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure )**|Equinix|New York, Washington domeincontroller|
|**[XO communicatie](http://www.xo.com/)**|Equinix|Silicon Valley|


## <a name="expressroute-system-integrators"></a>ExpressRoute systeemintegrators

Privé connectiviteit met aan uw wensen voldoet inschakelen kan lastig zijn, op basis van de schaal van uw netwerk. U kunt werken met een van de systeemintegrators vermeld in de volgende tabel om u te helpen met onboarding naar ExpressRoute.

|**Systeem integrator**|**Continent**|
|---|---|
|**[Avanade Inc.](http://www.avanade.com/)**| Azië, Europa, VS |
|**[DotNet oplossingen](http://www.dotnetsolutions.co.uk/)**| Europa |
|**[Equinix Professional Services](http://www.equinix.com/services/consulting/)**|ONS|
|**[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Azië |
|**[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | ONS |
|**[Project-leiderschap](http://www.projectleadership.net/azure)** | ONS |

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)voor meer informatie over ExpressRoute.
- Zorg ervoor dat alle voorwaarden wordt voldaan. Zie [ExpressRoute vereisten](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Locatie-kaart"
