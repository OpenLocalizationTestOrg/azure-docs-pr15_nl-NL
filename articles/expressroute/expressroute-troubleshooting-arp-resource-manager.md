<properties 
   pageTitle="ExpressRoute Troubleshooting Guide - ARP tabellen ophalen | Microsoft Azure"
   description="Deze pagina bevat instructies voor het ARP tabellen voor een circuitlijnen ExpressRoute"
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

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>ExpressRoute Troubleshooting guide - ARP aan de tabellen in het implementatiemodel resourcemanager

> [AZURE.SELECTOR]
[PowerShell - resourcemanager](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - klassiek](expressroute-troubleshooting-arp-classic.md)

In dit artikel begeleidt u bij de stappen voor meer informatie over de tabellen ARP voor uw circuitlijnen ExpressRoute. 

>[AZURE.IMPORTANT] In dit document is bedoeld om u te helpen u automatisch opsporen en oplossen van problemen met eenvoudige. Het is niet bedoeld ter vervanging voor ondersteuning van Microsoft. Als u niet voor het gebruiken van de onderstaande richtlijnen om het probleem oplossen, moet u een ondersteuningsticket openen met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Tabellen resolutie ARP (Protocol) en ARP adres
Resolutie ARP (Address Protocol) is een laag 2-protocol dat is gedefinieerd in [RFC 826](https://tools.ietf.org/html/rfc826). ARP wordt gebruikt om de Ethernet-adres (MAC-adres) met een IP-adres.

De tabel ARP biedt een toewijzing van de IPv4-adres en MAC-adres voor een bepaalde peering. De tabel ARP voor een ExpressRoute circuitlijnen peering bevat de volgende informatie voor elke interface (primaire en secundaire)

1. Toewijzing van on-premises implementatie router interface IP-adres aan het MAC-adres
2. Toewijzing van ExpressRoute router interface IP-adres aan het MAC-adres
3. Leeftijd van de toewijzing

ARP tabellen kunt valideren layer 2-configuratie en probleemoplossing eenvoudige 2 verbindingsproblemen lagen. 

Voorbeeld ARP tabel: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


De volgende sectie bevat informatie over hoe u de tabellen ARP is zichtbaar voor de ExpressRoute rand routers kunt weergeven. 

## <a name="prerequisites-for-learning-arp-tables"></a>Vereisten voor het leren ARP tabellen

Zorg ervoor dat u het volgende hebt voordat u verder voortgang

 - Een geldig ExpressRoute circuitlijnen geconfigureerd met ten minste één peering. De circuitlijnen moet volledig worden geconfigureerd door de provider connectivity. U (of uw provider connectivity) moet zijn geconfigureerd ten minste één van de peerings (Azure privé, Azure openbaar en Microsoft) op deze circuitlijnen.
 - IP-adresbereiken gebruikt voor het configureren van de peerings (Azure privé, Azure openbaar en Microsoft). Bekijk de IP-adres toewijzing voorbeelden in de [ExpressRoute routeren vereisten pagina](expressroute-routing.md) om te begrijpen hoe IP-adressen zijn toegewezen aan interfaces aan uw kant- en aan de kant ExpressRoute. Aan de hand van de [configuratiepagina peering van ExpressRoute](expressroute-howto-routing-arm.md)krijgt u informatie over de peering configuratie.
 - Gegevens uit uw netwerken team / connectivity provider op de MAC-adressen van interfaces gebruikt met deze IP-adressen.
 - U hebt de meest recente PowerShell-module voor Azure (versie 1,50 of hoger).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Het ARP ophalen tabellen voor uw circuitlijnen ExpressRoute
Deze sectie bevat instructies over hoe u de tabellen ARP per peering via PowerShell kunt weergeven. U of uw provider connectivity moet hebt geconfigureerd de peering voordat naar verder vordert. Elke circuitlijnen heeft twee paden (primaire en secundaire). U kunt de tabel ARP voor elk pad onafhankelijk controleren.

### <a name="arp-tables-for-azure-private-peering"></a>ARP tabellen voor Azure privé peering
De volgende cmdlet biedt het ARP tabellen voor Azure privé peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Voorbeeld van uitvoer hieronder vindt u een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP tabellen voor Azure openbare peering
De volgende cmdlet biedt het ARP tabellen voor Azure openbare peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Voorbeeld van uitvoer hieronder vindt u een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP tabellen voor Microsoft peering
De volgende cmdlet biedt het ARP tabellen voor Microsoft peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Voorbeeld van uitvoer hieronder vindt u een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Hoe u deze gegevens gebruiken
De tabel ARP van een peering kan worden gebruikt om te bepalen valideren 2-laagconfiguratie en de verbindingen. Deze sectie bevat een overzicht van hoe ARP tabellen eruit onder verschillende scenario's zien zullen.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>ARP tabel tijdens een circuitlijnen operationele status (verwachte status)

 - De tabel ARP hebben een fragment voor de on-premises implementatie in combinatie met een geldig IP-adres en MAC-adres en een vergelijkbare vermelding voor de Microsoft-kant. 
 - De laatste decimale waarden van het on-premises implementatie IP-adres is altijd een oneven getal.
 - De laatste decimale waarden van het Microsoft IP-adres zijn altijd een even getal.
 - Hetzelfde MAC-adres wordt weergegeven op de Microsoft-kant voor alle 3 peerings (primaire / secundaire). 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP tabel wanneer on-premises implementatie / connectivity provider kant staat problemen

 - Slechts één vermelding wordt weergegeven in de tabel ARP. Hier ziet de toewijzing tussen de MAC-adres en het IP-adres gebruikt in de Microsoft-kant. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Open een verzoek voor ondersteuning bij uw provider connectivity voor foutopsporing van dergelijke problemen. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP tabel wanneer Microsoft kant problemen heeft

 - U ziet niet een ARP-tabel weergegeven voor een peering als er problemen bij Microsoft zijn. 
 -  Open een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Opgeven dat er een probleem met layer 2 connectivity. 

## <a name="next-steps"></a>Volgende stappen

 - Layer 3-configuraties voor uw circuitlijnen ExpressRoute valideren
     - Route samenvatting om te bepalen de status van BGP sessies ophalen 
     - Route-tabellen om te bepalen welke voorvoegsels voor eenheden zijn aangekondigd over ExpressRoute ophalen
 - Valideer de overdracht van gegevens aan de hand van de bytes in / uit
 - Open een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u nog steeds problemen ondervindt.
