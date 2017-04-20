<properties
    pageTitle="Hoe u uw virtuele netwerk voor een siteverzameling Azure RemoteApp plant | Microsoft Azure"
    description="Leer hoe u uw virtuele netwerk voor een siteverzameling Azure RemoteApp plant."
    services="remoteapp"
    documentationCenter="" 
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>Het plannen van uw virtuele netwerk van Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Dit document wordt beschreven hoe u uw Azure virtual network (VNET) en het subnet configureert voor Azure RemoteApp. Als u niet bekend met Azure virtuele netwerken bent, is dit een mogelijkheid waarmee u de infrastructuur van uw netwerk in de cloud virtueel en hybride oplossingen bouwen met Azure en on-premises implementatie resources. U vindt meer informatie over deze [hier](../virtual-network/virtual-networks-overview.md).

Als u definiëren beveiligingsbeleid voor apparaten voor verkeer (binnenkomende en uitgaande) in uw virtuele netwerk waar u Azure RemoteApp implementeert wilt, wordt aangeraden het maken van een afzonderlijk subnet voor Azure RemoteApp van de rest van uw implementaties in het Azure virtuele netwerk. Lees voor meer informatie over het definiëren van beveiligingsbeleid voor apparaten in uw subnet Azure virtuele netwerk [Wat is een netwerk beveiliging groep (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Typen Azure RemoteApp collecties met Azure virtuele netwerken

De volgende afbeeldingen weergeven de twee andere verzameling opties wanneer u wilt gebruiken, een virtueel netwerk.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure RemoteApp cloud siteverzameling met VNET

 ![Azure RemoteApp - Cloud siteverzameling met een VNET](./media/remoteapp-planvpn/ra-cloudvpn.png)

Hiermee wordt een Azure RemoteApp-siteverzameling waar alle bronnen die de sessie RemoteApp-hosts moeten toegang hebben tot zijn geïmplementeerd in Azure wordt aangegeven. Dit steekt nogal in de dezelfde VNET als de RemoteApp VNET of een ander VNET in Azure wordt aangegeven.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Azure RemoteApp hybride siteverzameling met VNET

![Azure RemoteApp - hybride siteverzameling met een VNET](./media/remoteapp-planvpn/ra-hybridvpn.png)

Hiermee wordt een Azure RemoteApp-siteverzameling waar aantal bronnen die de RemoteApp-sessie hosts moeten toegang hebben tot geïmplementeerd on-premises implementatie zijn. De VNET RemoteApp is gekoppeld aan de on-premises netwerk Azure hybride technologieën zoals naar website VPN of Express Route gebruiken.


## <a name="how-the-system-works"></a>De werking van het systeem

Achter de implementeert Azure RemoteApp Azure virtuele machines (met uw geüploade afbeelding) naar het virtuele netwerk subnet die u hebt gekozen tijdens het inrichten. Als u ervoor hebt gekozen voor een siteverzameling hybride, willen we de FQDN-naam van de domeincontroller die u hebt ingevoerd in de werkstroom voor het inrichten met de DNS-server die beschikbaar zijn in het virtuele netwerk oplossen.  
Als u verbinding met een bestaand virtuele netwerk maakt, zorg om weer te geven van de vereiste poorten in uw netwerk-beveiligingsgroepen in uw subnet, Azure RemoteApp. 

Het is raadzaam om dat u een [groot genoeg subnet voor Azure RemoteApp](remoteapp-vnetsizing.md)gebruiken. De grootste ondersteund door Azure virtuele netwerk is /8 (via CIDR subnet definities). Uw subnet moet groot genoeg voor alle Azure RemoteApp VMs tijdens schaal van wanneer meer gebruikers toegang krijgt de apps tot. 

Hier volgen de dingen die u moet inschakelen op uw subnet virtueel netwerk: 

2.  Uitgaand verkeer uit het subnet moet kunnen op poortbereik 10101-10175 communiceren met een van de interne Azure RemoteApp-services.
3.  Uitgaand verkeer moet kunnen van uw subnet, verbinding maken met Azure Storage op poort 443
4.  Hebt u Active Directory die worden gehost in Azure wordt aangegeven, zorg er dan voor dat een VM binnen het subnet virtueel netwerk voor Azure RemoteApp verbinding maken met die domeincontroller is. De DNS-records in het virtuele netwerk moet mogelijk zijn voor het oplossen van de FQDN-naam van deze domeincontroller.


## <a name="virtual-network-with-forced-tunneling"></a>Virtuele netwerk met een afgedwongen tunneling

[U wordt gedwongen tunneling](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) wordt nu ondersteund voor alle nieuwe Azure RemoteApp collecties. We ondersteuning momenteel geen voor de migratie van een bestaande siteverzameling ter ondersteuning van afgedwongen tunneling.  U moet de bestaande collecties met de VNET die u Azure RemoteApp koppelen wilt verwijderen en maak een nieuwe om toegang te krijgen gedwongen tunneling ingeschakeld op uw siteverzamelingen. 
