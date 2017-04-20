
<properties
   pageTitle="Azure richtlijnen | patronen en procedures | Microsoft Azure"
   description="Azure verwijzing architecturen"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Azure verwijzing architecturen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_Deze inhoud is in de actieve ontwikkelingsfase bevindt. Is het handig vandaag, zodat we beschikbaar te voor preview maken zijn. We waarde uw feedback._

Onze architecturen verwijzing zijn gerangschikt op scenario met meerdere gerelateerde architecturen gegroepeerd.
Elke afzonderlijke architectuur biedt aanbevolen procedures en aanbevolen stappen, evenals een uitvoerbare onderdeel die de aanbevelingen.
Veel van de architectuur zijn geleidelijke; samenstellen van boven aan de voorgaande architecturen die minder vereisten hebben.

## <a name="designing-your-infrastructure-for-resiliency"></a>Ontwerpen van uw infrastructuur voor tolerantie

Deze reeks begint met aanbevolen procedures voor een optimale VM configuratie en culminates in een meerdere landen/regio-implementatie waarbij failover.

- [Een Windows VM uitvoert op Azure](guidance-compute-single-vm.md)
- [Een VM Linux op Azure uitgevoerd](guidance-compute-single-vm-linux.md)
- [Meerdere VMs voor schaalbaarheid en beschikbaarheid uitgevoerd](guidance-compute-multi-vm.md)
- [Windows VMs uitgevoerd voor een architectuur N lagen](guidance-compute-n-tier-vm.md)
- [Actieve Linux VMs voor een architectuur N lagen](guidance-compute-n-tier-vm-linux.md)
- [Met het Windows VMs in meerdere gebieden voor beschikbaarheid](guidance-compute-multiple-datacenters.md)
- [Linux VMs uitgevoerd in meerdere gebieden voor beschikbaarheid](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>Uw on-premises netwerk verbinden met Azure

Deze reeks wordt gestart door aan te tonen de manieren aan uw bestaande netwerk verbinden met Azure. Vervolgens wordt de uitgebreid bedekt vereisten voor beschikbaarheid en beveiliging.

- [Uitvoering van een hybride netwerkarchitectuur met Azure en on-premises VPN](guidance-hybrid-network-vpn.md)
- [Een hybride netwerkarchitectuur met Azure ExpressRoute implementeren](guidance-hybrid-network-expressroute.md)
- [Een netwerkarchitectuur ten zeerste beschikbaar hybride implementeren](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>Uw netwerk hybride beveiligen

Deze reeks behandelt bewezen procedures voor het maken van DMZ in Azure op beveiligde verbindingen die afkomstig zijn uit uw on-premises implementatie-datacenter en Internet.

- [Een DMZ tussen Azure en uw on-premises implementatie-datacenter implementeren](guidance-iaas-ra-secure-vnet-hybrid.md)
- [Een DMZ tussen Azure en Internet implementeren](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Identiteit services leveren

Deze reeks wordt gestart door te demonstreren dat het gebruik van Azure Active Directory te leveren gebruikersverificatie in Azure wordt aangegeven. Vervolgens wordt de uitgebreid complexe scenario's voor de infrastructuur van uw Hiermee uitbreiden naar Azure en het gebruik van ADFS voor doorschakelen naar gemachtigden bedekt.

- [Implementatie van Azure Active Directory](./guidance-identity-aad.md)
- [Active Directory-adreslijstservices (Hiermee) uitbreiden naar Azure](./guidance-identity-adds-extend-domain.md)
- [Maken van een Active Directory Directory Services (Hiermee) resource bos in Azure wordt aangegeven](./guidance-identity-adds-resource-forest.md)
- [Implementatie van Azure Active Directory Federation Services (ADFS)](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Uitbreidt scalable webtoepassing met Azure PaaS

Deze reeks behandelt aanbevelingen voor het bouwen van scalable en ten zeerste beschikbaar Webapps. 

- [Eenvoudige webtoepassing](guidance-web-apps-basic.md)
- [Verbeteren van de schaalbaarheid in een webtoepassing](guidance-web-apps-scalability.md)
- [Webtoepassing met hoge beschikbaarheid](guidance-web-apps-multi-region.md)
