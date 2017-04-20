<properties 
   pageTitle="Het gebruik van openbare IP-adressen in een virtueel netwerk"
   description="Meer informatie over het configureren van een virtueel netwerk om te gebruiken van openbare IP-adressen"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="public-ip-address-space-in-a-virtual-network-vnet"></a>Openbare IP-adresruimte in een virtueel netwerk (VNet)

Virtuele netwerken (VNets) kunnen openbare en persoonlijke (RFC 1918 adres geblokkeerd) IP-adres spaties bevatten. Wanneer u een openbare IP-adresbereiken toevoegt, wordt deze verwerkt als onderdeel van de persoonlijke VNet IP-adresruimte die is alleen bereikbaar vanuit de VNet, onderling verbonden VNets, en uw on-premises locatie.

De onderstaande afbeelding ziet een VNet die openbare en persoonlijke IP-adres spaties bevat.

![Openbare IP conceptuele](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## <a name="how-do-i-add-a-public-ip-address-range"></a>Hoe voeg ik een openbare IP-adresbereiken?

U een openbare IP-adresbereiken op dezelfde manier voegt u een persoonlijke IP-adresbereiken; toe toevoegen met behulp van een bestand *netcfg* of door de configuratie toe te voegen in de [portal van Azure](http://portal.azure.com). Wanneer u uw VNet maken of u kunt teruggaan en deze later toevoegen, kunt u een openbare IP-adresbereiken toevoegen. Het volgende voorbeeld ziet u openbare en persoonlijke IP-adres spaties in de dezelfde VNet geconfigureerd.

![Openbare IP-adres in de Portal](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## <a name="are-there-any-limitations"></a>Gelden er beperkingen?

Er zijn een paar IP-adresbereiken die niet zijn toegestaan:

- 224.0.0.0/4 (Multicast)

- 255.255.255.255/32 (Broadcast)

- 127.0.0.0/8 (loopback)

- 169.254.0.0/16 (link-local)

- 168.63.129.16/32 (interne DNS)

## <a name="next-steps"></a>Volgende stappen

[Het beheren van DNS-servers op een VNet](virtual-networks-manage-dns-in-vnet.md)