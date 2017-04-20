<properties
   pageTitle="Gids voor probleemoplossing voor ExpressRoute: ophalen ARP tabellen | Microsoft Azure"
   description="Deze pagina bevat instructies voor het ophalen van de ARP tabellen voor een circuitlijnen ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>Gids voor probleemoplossing voor ExpressRoute: ARP van tabellen in het implementatiemodel klassieke ophalen

> [AZURE.SELECTOR]
[PowerShell - resourcemanager](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - klassiek](expressroute-troubleshooting-arp-classic.md)

In dit artikel begeleidt u bij de stappen voor het ophalen van de tabellen resolutie ARP (Address Protocol) voor uw circuitlijnen Azure ExpressRoute.

>[AZURE.IMPORTANT] In dit document is bedoeld om u te helpen u automatisch opsporen en oplossen van problemen met eenvoudige. Het is niet bedoeld ter vervanging voor ondersteuning van Microsoft. Als u niet kunt het probleem oplossen door de volgende richtlijnen, opent u een verzoek voor ondersteuning met [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Tabellen resolutie ARP (Protocol) en ARP adres
ARP is een Layer 2-protocol die gedefinieerd in [RFC 826](https://tools.ietf.org/html/rfc826). ARP wordt gebruikt om een Ethernet-adres (MAC) toewijzen aan een IP-adres.

Een tabel ARP vindt u een toewijzing van de IPv4-adres en MAC-adres voor een bepaalde peering. De tabel ARP voor een ExpressRoute circuitlijnen peering biedt de volgende informatie voor elke interface (primaire en secundaire):

1. Toewijzing van een on-premises implementatie router interface IP-adres op een MAC-adres
2. Toewijzing van een ExpressRoute router interface IP-adres op een MAC-adres
3. De leeftijd van de toewijzing

ARP tabellen kunnen helpen met het valideren van laag 2-configuratie en bij het oplossen van eenvoudige Layer 2-verbindingsproblemen.

Hier volgt een voorbeeld van een tabel ARP:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Het volgende gedeelte vindt u informatie over het weergeven van de ARP-tabellen die door de ExpressRoute rand routers worden bekeken.

## <a name="prerequisites-for-using-arp-tables"></a>Vereisten voor het gebruik van ARP-tabellen

Zorgen dat u het volgende voordat u verdergaat:

 - Een geldig ExpressRoute circuitlijnen die geconfigureerd met ten minste één peering. De circuitlijnen moet volledig worden geconfigureerd door de provider connectivity. U (of uw provider connectivity) moet ten minste één van de peerings (Azure privé, Azure openbare of Microsoft) op configureren deze circuitlijnen.

 - IP-adresbereiken die worden gebruikt voor het configureren van de peerings (Azure privé, Azure openbare en Microsoft). Bekijk de IP-adres toewijzing voorbeelden in de [ExpressRoute routeren vereisten pagina](expressroute-routing.md) om te begrijpen hoe IP-adressen zijn toegewezen aan interfaces op uw aise en aan de kant ExpressRoute. Aan de hand van de [configuratiepagina peering van ExpressRoute](expressroute-howto-routing-classic.md)krijgt u informatie over de peering configuratie.

 - Gegevens uit uw netwerken team of connectivity provider over de MAC-adressen van de interfaces die worden gebruikt met deze IP-adressen.

 - De meest recente Windows PowerShell-module voor Azure (versie 1,50 of hoger).

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP tabellen voor uw circuitlijnen ExpressRoute
In deze sectie bevat instructies over het weergeven van de tabellen ARP voor elk type peering via PowerShell. Voordat u verder gaat, wordt u of uw provider connectivity moet voor het configureren van de peering. Elke circuitlijnen heeft twee paden (primaire en secundaire). U kunt de tabel ARP voor elk pad onafhankelijk controleren.

### <a name="arp-tables-for-azure-private-peering"></a>ARP tabellen voor Azure privé peering
De volgende cmdlet biedt het ARP tabellen voor Azure privé peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Hieronder volgt een voorbeeld van uitvoer voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP tabellen voor Azure openbare peering:
De volgende cmdlet biedt het ARP tabellen voor Azure openbare peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Hieronder volgt een voorbeeld van uitvoer voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Hieronder volgt een voorbeeld van uitvoer voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP tabellen voor Microsoft peering
De volgende cmdlet biedt het ARP tabellen voor Microsoft peering:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Voorbeeld van uitvoer hieronder vindt u een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Hoe u deze gegevens gebruiken
De tabel ARP van een peering kan worden gebruikt voor het valideren van Layer 2-configuratie en de verbindingen. Deze sectie bevat een overzicht van hoe ARP tabellen in verschillende scenario's zoeken.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>ARP tabel wanneer een circuitlijnen zich in een modus (verwacht bevindt)

 - De tabel ARP heeft een vermelding voor de on-premises implementatie in combinatie met een geldig IP- en MAC-adres en een vergelijkbare vermelding voor de Microsoft-kant.
 - De laatste decimale waarden van het on-premises implementatie IP-adres is altijd een oneven getal.
 - De laatste decimale waarden van het Microsoft-IP-adres is altijd een even getal.
 - Hetzelfde adres MAC aan de rechterkant van het Microsoft voor alle drie peerings (primaire/secundaire) wordt weergegeven.


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>ARP tabel wanneer deze on-premises implementatie of wanneer de kant connectivity-provider problemen heeft

 Slechts één vermelding wordt weergegeven in de tabel ARP. De toewijzing tussen het MAC-adres en het IP-adres dat wordt gebruikt bij Microsoft worden weergegeven.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Als er een probleem als volgt, opent u een verzoek voor ondersteuning bij uw provider connectivity oplossen.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>ARP tabel wanneer de Microsoft-kant problemen heeft

 - U ziet niet een ARP-tabel weergegeven voor een peering als er problemen bij Microsoft zijn.
 -  Open een verzoek voor ondersteuning met [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Opgeven dat er een probleem met Layer 2 connectivity.

## <a name="next-steps"></a>Volgende stappen

 - Valideer de Layer 3-configuraties voor uw circuitlijnen ExpressRoute:
     - Krijg een route samenvatting om de status van BGP sessies te bepalen.
     - Een tabel om te bepalen welke voorvoegsels voor eenheden zijn aangekondigd over ExpressRoute krijgen.
 - Valideer de overdracht van gegevens aan de hand van bytes in-en uitfaden.
 - Open een verzoek voor ondersteuning met [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u nog steeds problemen ondervindt.
