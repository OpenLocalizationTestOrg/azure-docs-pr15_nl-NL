<properties 
    pageTitle="Het configureren van Virtual Network ondersteuning voor een Premium Azure bestand Vgx. Cache | Microsoft Azure" 
    description="Meer informatie over het maken en beheren van Virtual Network ondersteuning voor uw Premium laag Azure bestand Vgx. Cache-sessies" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Het virtuele netwerkondersteuning configureren voor een Cache Premium Azure bestand Vgx.
Azure bestand Vgx. Cache heeft verschillende cache aanbiedingen waarmee flexibiliteit bij de keuze van cachegrootte en functies, waaronder de nieuwe Premium laag.

Laag premiumfuncties van de Azure bestand Vgx. Cache opnemen cluster, permanente en virtuele netwerkondersteuning (VNet). Een VNet is een privé-netwerk in de cloud. Wanneer een bestand Vgx. Cache van Azure-exemplaar is geconfigureerd met een VNet, is niet openbaar toegankelijk en kan alleen worden geopend vanuit virtuele machines en toepassingen binnen de VNet. In dit artikel wordt beschreven hoe virtuele netwerkondersteuning voor een exemplaar van de Azure bestand Vgx. Cache premium configureren.

>[AZURE.NOTE] Azure bestand Vgx. Cache ondersteunt beide klassieke en ARM VNets.

Zie [Inleiding tot de laag Azure bestand Vgx. Cache Premium](cache-premium-tier-intro.md)voor informatie over andere functies van de cache premium.

## <a name="why-vnet"></a>Waarom VNet?
Implementatie van [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/) biedt verbeterde beveiliging en moeten worden geïsoleerd voor de Cache Azure bestand Vgx., evenals subnetten, beleid voor het beheren van access en andere functies verder beperken van toegang aan Azure bestand Vgx. Cache.

## <a name="virtual-network-support"></a>Virtuele netwerkondersteuning
Virtuele netwerkondersteuning (VNet) is geconfigureerd op het **Nieuwe bestand Vgx. Cache** blad tijdens het maken van de cache. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Zodra u een laag prijzen premium hebt geselecteerd, kunt u Azure bestand Vgx. Cache VNet integratie configureren door te selecteren van een VNet die zich in dezelfde abonnement en locatie als de cache. Als u een nieuwe VNet, eerst maken door de stappen in [een virtueel netwerk met behulp van de Azure portal maken](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of [een virtueel netwerk (klassieke) maken met behulp van de Azure-portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md) en Ga terug naar het **Nieuwe bestand Vgx. Cache** -blad maken en configureren van de premium-cache.

Als u wilt configureren de VNet voor uw nieuwe cache, **Virtual Network** Klik op het **Nieuwe bestand Vgx. Cache** blad en selecteert u de gewenste VNet in de vervolgkeuzelijst.

![Virtuele netwerk][redis-cache-vnet]

Selecteer het gewenste subnet in de vervolgkeuzelijst **Subnet** en geef het gewenste **statisch IP-adres**. Als u een klassieke VNet werkt met het veld **statisch IP-adres** is optioneel en als u geen opgeeft, wordt een van de geselecteerde subnet gekozen.

>[AZURE.IMPORTANT] Wanneer u een Azure bestand Vgx. Cache implementeert naar een VNet ARM, wordt de cache moet zich in een speciale subnet die geen andere bronnen, behalve voor Azure bestand Vgx. Cache exemplaren bevat. Als u een poging een Azure bestand Vgx. Cache implementeren naar een VNet ARM aan een subnet met andere bronnen, de implementatie is mislukt.

![Virtuele netwerk][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] De eerste vier adressen in een subnet zijn gereserveerd en kunnen niet worden gebruikt. Zie voor meer informatie [welke beperkingen voor het werken met IP-adressen binnen deze subnetten?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Nadat de cache is gemaakt, kunt u de configuratie voor de VNet weergeven door te klikken op **Virtual Network** van het blad **Instellingen** .

![Virtuele netwerk][redis-cache-vnet-info]


Als u wilt verbinden met uw exemplaar van de cache Azure bestand Vgx. Wanneer u een VNet gebruikt, geef de hostnaam van de cache in de verbindingstekenreeks zoals wordt weergegeven in het volgende voorbeeld.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Cache VNet Veelgestelde vragen over Azure bestand Vgx.

De volgende lijst bevat antwoorden op veelgestelde vragen over de schaal van Azure bestand Vgx. Cache.

-   [Wat zijn enkele veelvoorkomende problemen onjuiste configuratie met Azure bestand Vgx. Cache en VNets?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-   [Kan ik VNets gebruiken met een standaard- of eenvoudige cache?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-   [Waarom voor het maken van een bestand Vgx. cache niet in sommige subnetten, maar niet met anderen?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-   [Werk alle onderdelen van de cache wanneer een cache in een VNET hostingprovider?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Wat zijn enkele veelvoorkomende problemen onjuiste configuratie met Azure bestand Vgx. Cache en VNets?

Wanneer Azure bestand Vgx. Cache wordt gehost in een VNet, wordt de poorten in de volgende tabel worden gebruikt. Als deze poorten zijn geblokkeerd, is het mogelijk dat de cache niet goed werkt. Met een of meer van deze poorten geblokkeerd is een probleem met de meest voorkomende onjuiste configuratie als u Azure bestand Vgx. Cache in een VNet.

| Poorten     | Richting        | Transportprotocol | Doel                                                                           | Externe IP                           |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| 80, 443     | Uitgaande         | TCP                | Bestand Vgx afhankelijkheden op Azure opslag/PKI (Internet).                                | *                                   |
| 53          | Uitgaande         | TCP/UDP            | Bestand Vgx afhankelijkheden op DNS-(Internet/VNet).                                         | *                                   |
| 6379, 6380  | Binnenkomende verbindingen          | TCP                | Clientcommunicatie met het bestand Vgx. Azure van taakverdeling                               | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 8443        | Inkomende/uitgaande poort | TCP                | Implementatie details voor het bestand Vgx.                                                   | VIRTUAL_NETWORK                     |
| 8500        | Binnenkomende verbindingen          | TCP/UDP            | Azure taakverdeling                                                              | AZURE_LOADBALANCER                  |
| 10221-10231 | Inkomende/uitgaande poort | TCP                | Implementatie details voor het bestand Vgx. (kunt externe eindpunt beperken tot VIRTUAL_NETWORK) | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 13000-13999 | Binnenkomende verbindingen          | TCP                | Clientcommunicatie met bestand Vgx. kolomgroepen Azure van taakverdeling                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 15000-15999 | Binnenkomende verbindingen          | TCP                | Clientcommunicatie met bestand Vgx. kolomgroepen Azure van taakverdeling                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 16001       | Binnenkomende verbindingen          | TCP/UDP            | Azure taakverdeling                                                              | AZURE_LOADBALANCER                  |
| 20226       | Binnenkomende + uitgaande | TCP                | Implementatie details voor Clusters bestand Vgx.                                          | VIRTUAL_NETWORK                     |


Zijn er vereisten voor netwerk-connectiviteit voor Azure bestand Vgx. Cache die mogelijk niet in eerste instantie worden voldaan in een virtueel netwerk. Azure bestand Vgx. Cache vereist alle van de volgende items goed werken als in een virtueel netwerk gebruikt.

-  Uitgaande netwerkverbinding met Azure Storage eindpunten overal ter wereld. Dit geldt ook voor eindpunten bevinden in hetzelfde gebied, als het exemplaar van de Azure bestand Vgx. Cache, alsmede opslag eindpunten bevinden in **andere** Azure regio's. Azure opslag eindpunten oplossen onder de domeinen van de volgende DNS-: *table.core.windows.net*, *blob.core.windows.net* *queue.core.windows.net*en *file.core.windows.net*. 
-  Uitgaande netwerkconnectiviteit *ocsp.msocsp.com*, *mscrl.microsoft.com* en *crl.microsoft.com*. Deze verbinding nodig ter ondersteuning van SSL-functionaliteit.
-  De DNS-configuratie voor het virtuele netwerk moet mogelijk het oplossen van alle eindpunten en domeinen die worden genoemd in de eerdere wordt verwezen. Deze DNS-vereisten kunnen worden voldaan doordat een geldige DNS-infrastructuur is geconfigureerd en onderhouden voor het virtuele netwerk.



### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Kan ik VNets gebruiken met een standaard- of eenvoudige cache?

VNets kan alleen worden gebruikt met premium cache.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Waarom voor het maken van een bestand Vgx. cache niet in sommige subnetten, maar niet met anderen?

Als u een Azure bestand Vgx. Cache voor een VNet ARM implementeert, wordt de cache moet zich in een speciale subnet die geen andere resourcetype bevat. Als u een poging een Azure bestand Vgx. Cache implementeren naar een subnet ARM VNet met andere bronnen, de implementatie is mislukt. Voordat u een nieuwe cache voor het bestand Vgx. kunt maken, moet u de bestaande bronnen binnen het subnet verwijderen.

U kunt meerdere soorten resources dashboard implementeren naar een klassieke VNet zo lang maken als er voldoende IP-adressen die beschikbaar.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Werk alle onderdelen van de cache wanneer een cache in een VNET hostingprovider?

Als de cache deel van een VNET uitmaakt, toegang alleen-clients in het VNET de cache. Als werkt de volgende functies van de cache-management niet op dit moment.

-   Bestand Vgx. Console - omdat het bestand Vgx. Console gebruikmaakt van het bestand Vgx. cli.exe-client die worden gehost op VMs die geen deel uitmaken van uw VNET, deze kan geen verbinding met de cache.


## <a name="use-expressroute-with-azure-redis-cache"></a>ExpressRoute gebruiken met Cache Azure bestand Vgx.

Klanten kunnen verbinden met een circuitlijnen [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) hun virtuele netwerkinfrastructuur, dus hun on-premises netwerk uitbreiden naar Azure. 

Standaard wordt een nieuw gemaakte ExpressRoute circuitlijnen een standaard-route waarmee uitgaande verbinding met Internet. Met deze configuratie kunnen clienttoepassingen verbinding maken met andere Azure eindpunten inclusief Azure bestand Vgx. Cache.

Een algemene klant-configuratie is echter hun eigen standaardroute (0.0.0.0/0) waardoor uitgaande internetverkeer naar de on-premises implementatie in plaats daarvan flow definiëren. Deze verkeersstroom eindemarkeringen altijd connectiviteit met Azure bestand Vgx. Cache omdat de uitgaand verkeer een van beide geblokkeerde on-premises wordt of NAT naar een niet-herkenbaar reeks adressen die niet langer met verschillende Azure eindpunten werken zou doen.

De oplossing is het definiëren van een (of meer) de gebruiker gedefinieerde routes (UDRs) op het subnet waarin de Cache voor het bestand Vgx. van Azure. Een UDR definieert subnet / regiospecifieke routes die wordt in plaats van de standaard-route van kracht.

Indien mogelijk wordt het gebruik van de volgende configuratie aanbevolen:

- De configuratie ExpressRoute promotie 0.0.0.0/0 van en al dan niet standaard tunnels dwingen alle uitgaand verkeer on-premises implementatie.
- De UDR toegepast op het subnet met de Cache Azure bestand Vgx. Hiermee definieert u 0.0.0.0/0 met een volgende hop type Internet (een voorbeeld hiervan is verder naar beneden in dit artikel).

Het gecombineerde effect van deze stappen is dat het subnetniveau van de UDR voorrang op de ExpressRoute gedwongen tunneling, zodat uitgaande internettoegang uit de Cache voor het bestand Vgx. van Azure.

Hoewel u verbinding maakt met een exemplaar van de Azure bestand Vgx. Cache vanuit een on-premises-toepassing via ExpressRoute is niet een veelvoorkomend gebruiksscenario vanwege Prestatieoverwegingen (voor de beste prestaties Azure bestand Vgx. Cache clients moeten in dezelfde gebied, als de Cache Azure bestand Vgx.) in dit scenario die de uitgaande netwerkpad langs interne zakelijke proxy's kan niet worden verzonden, die niet dwingen tunnel naar on-premises implementatie. Het effectieve NAT-adres van een uitgaand verkeer hierdoor uit de Cache Azure bestand Vgx. worden gewijzigd. Het NAT-adres van een Azure bestand Vgx. Cache wijzigt uitgaand netwerkverkeer van instantie connectivity fouten op-veel van de bovenstaande eindpunten. Hierdoor mislukte pogingen van Azure bestand Vgx. Cache maken.

**Belangrijk:**  De routes gedefinieerd in een UDR **moet** worden specifieke voorrang op eventuele routes aangekondigd door de configuratie ExpressRoute. In het volgende voorbeeld wordt de adresbereiken globaal 0.0.0.0/0 gebruikt, en als zodanig kunt potentieel worden per ongeluk overschreven door route advertenties met meer informatie over-adresbereiken.

**Heel belangrijk:**  Azure bestand Vgx. Cache is niet ondersteund met ExpressRoute configuraties die **onjuist cross – reclame routes van de openbare peering pad naar het persoonlijk peering pad**. ExpressRoute configuraties waarvoor openbare peering is geconfigureerd, ontvangt route advertenties om een groot aantal Microsoft Azure IP-adresbereiken van Microsoft. Als deze adresbereiken onjuist cross-aangekondigd op het privé peering pad zijn, is het eindresultaat dat alle uitgaande netwerkpakketten van het exemplaar van de Azure bestand Vgx. Cache subnet onjuist dwingen tunnel tot netwerkinfrastructuur voor on-premises implementatie van een klant zijn. In dit netwerk-mailstroom eindemarkeringen Azure bestand Vgx. Cache. De oplossing voor dit probleem is cross-reclame routes van de openbare peering pad naar het persoonlijk peering pad stoppen.

Achtergrondinformatie over de gebruiker gedefinieerde routes is beschikbaar in dit [Overzicht](../virtual-network/virtual-networks-udr-overview.md). 

Zie voor meer informatie over ExpressRoute, [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md)

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van meer premiumfuncties van cache.

-   [Inleiding tot de laag Premium van Azure Cache bestand Vgx.](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

