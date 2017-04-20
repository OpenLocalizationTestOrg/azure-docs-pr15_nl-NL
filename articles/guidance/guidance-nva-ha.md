<properties
   pageTitle="Virtuele toestellen implementeren in beschikbaarheid | Microsoft Azure"
   description="Het virtuele netwerkapparaten implementeren in beschikbaarheid."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Virtuele toestellen implementeren in beschikbaarheid

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel vindt u een overzicht van een reeks procedures voor de implementatie van virtuele netwerkapparaten (NVAs) op een wijze die ten zeerste beschikbaar. Voordat u doorgaat, moet u weten hoe [de gebruiker gedefinieerde routes (UDR)] [ udr-overview] en [de belasting voor documentconversies] [ lb-overview] werk in Azure wordt aangegeven.

U kunt verschillende NVAs beschikbaar in de Azure marketplace om uit te breiden de functionaliteit van Azure op dezelfde manier die u toestellen in uw on-premises implementatie-datacenter gebruiken. De volgende afbeelding ziet u de implementatie van een voorbeeld van een [enkel NVA] [ nva-scenario] als een toestel firewall. 

![[0]][0]

Hoewel de voorgaande implementatie werkt, maakt u kennis een potentieel risico. Als het virtuele toestel mislukt, wordt geen verkeersstromen. Om dit probleem op te lossen, moet u meerdere NVAs gebruiken. Echter ook waarvoor andere instellingen en resources moet worden gebruikt afhankelijk van uw vereisten.

U kunt een van de volgende oplossingen gebruiken om te implementeren van een uiterst beschikbare NVA-omgeving.

|Oplossing|Voordelen|Overwegingen|
|---|---|---|
|Ingress met laag 7 virtuele toestellen|Alle knooppunten zijn actief|Vereist een NVA die kunt verbindingen beëindigen en SNAT gebruiken<br/>Vereist een aparte set NVAs voor verkeer die afkomstig zijn van Internet en van Azure<br/>Kan alleen worden gebruikt voor verkeer die afkomstig zijn buiten Azure|
|Ingress-Egress met laag 7 virtuele toestellen|Alle knooppunten zijn actief<br/>Aankunnen verkeer afkomstig is van Azure |Vereist een NVA die kunt verbindingen beëindigen en SNAT gebruiken<br/>Vereist een aparte set NVAs voor verkeer die afkomstig zijn van Internet en van Azure|
|PIP-UDR veranderen|Enkele reeks NVAs voor al het verkeer<br/>Al het verkeer (geen limiet op poortregels) kan worden verwerkt.|Actieve-passieve<br/>Een failoverproces vereist|

## <a name="ingress-with-layer-7-virtual-appliances"></a>Ingress met laag 7 virtuele toestellen
U kunt een reeks NVAs achter een Azure taakverdeling bieden connectiviteit met Azure werkbelasting achter een klein aantal servers poorten (zoals HTTP en HTTPS). De volgende afbeelding wordt het beschikbaarheid in dit scenario op het niveau van de NVA gemarkeerd.

![[1]][1]

In dit scenario moet het netwerk virtuele toestel gebruikt om alle verbindingen beëindigen en aan het subnet werklast door te geven. De werklast virtuele machines (VMs) reageer op de NVA dat zij een verzoek om van en verkeersstromen zonder problemen heeft ontvangen. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Ingress-egress met laag 7 virtuele toestellen
U kunt de voorgaande architectuur uitbreiden en toevoegen van een andere set NVAs worden afgehandeld verkeer die afkomstig zijn van Azure moeten worden verwerkt door NVAs, zoals wordt weergegeven in de volgende afbeelding:

![[2]][2]

In dit scenario wordt al het verkeer die afkomstig zijn in Azure wordt aangegeven doorgestuurd naar een interne taakverdeling die laden tussen een andere set NVAs distribueert. Deze NVAs-verkeer met Internet via hun afzonderlijke openbare IP-adressen. 

## <a name="pip-udr-switch"></a>PIP-UDR veranderen
U kunt voorkomen meerdere NVA stapels maken met behulp van twee NVAs in actieve-passieve modus. In dit scenario kunt u het openbare IP-adres (PIP) en de gebruiker gedefinieerde routes (UDRs) schakelen wanneer het actieve knooppunt stopt.  

![[3]][3]

Dit scenario is vergelijkbaar met het één NVA scenario. De enige verschil is dat de PIP en UDRs als u wilt schakelen van verkeer tussen de NVAs moet worden gewijzigd. Deze wijzigingen kunnen handmatig worden gedaan of kunt u ook automatiseren. Als u wilt automatiseren, kunt u een toepassing aan beide NVAs die Hiermee wordt gecontroleerd op de status van het actieve knooppunt implementeren. Nadat het actieve knooppunt niet actief is, wordt uw toepassing kunt wijzigen de PIP en UDRs om de koppeling naar het passieve knooppunt.

Een mogelijke uitvoering van deze oplossing is het gebruik van een [ZooKeeper] [ zookeeper] daemon op de NVAs worden afgehandeld leider verkiezingsuitslagen (bepalen welke knooppunt is het actieve knooppunt). Als een leidinggevende is geselecteerd, wordt aangeroepen tot de Azure REST API de PIP van het knooppunt intrekken en toevoegen aan de leider. Dit moet ook UDRs zodat deze verwijzen naar de nieuwe leider privé IP-adres wijzigen.

## <a name="next-steps"></a>Volgende stappen

- Leer hoe u [een DMZ tussen Azure en uw on-premises implementatie-datacenter implementeert] [ dmz-on-prem] laag tot en met 7 NVAs gebruiken.
- Leer hoe u [een DMZ tussen Azure en Internet implementeert] [ dmz-internet] laag tot en met 7 NVAs gebruiken.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Eén NVA architectuur"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Laag 7 ingress"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "7 ingress- en egress lagen"
[3]: ./media/guidance-nva-ha/active-passive.png "Actieve-passieve cluster"