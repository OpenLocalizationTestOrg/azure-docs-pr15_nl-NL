<properties 
   pageTitle="Netwerk interfaces | Microsoft Azure"
   description="Meer informatie over Azure netwerkinterfaces in Azure resourcemanager."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="network-interfaces"></a>Netwerkinterfaces

Een netwerkinterface (NIC) is de koppeling tussen een virtuele Machine (VM) en de onderliggende software-netwerk. In dit artikel wordt uitgelegd wat een netwerkinterface is en hoe deze worden gebruikt in het implementatiemodel resourcemanager Azure-.

Microsoft adviseert de implementatie van nieuwe resources met het implementatiemodel resourcemanager, maar u kunt ook VMs implementeren met netwerkconnectiviteit in het [klassieke](virtual-network-ip-addresses-overview-classic.md) implementatie-model. Als u bekend met het klassieke model bent, zijn er belangrijke verschillen in VM netwerken in het implementatiemodel resourcemanager. Meer informatie over de verschillen Lees het artikel [VM netwerkproblemen - klassiek](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) .

In Azure wordt aangegeven, een netwerkinterface:

1. Een resource die kan worden gemaakt, verwijderd, is en heeft een eigen configureerbare instellingen.
2. Moet worden verbonden met één subnet in één Azure Virtual Network (VNet) wanneer deze gemaakt. Als u niet bekend met VNets bent, weten ze Lees het artikel [virtuele netwerk overzicht](virtual-networks-overview.md) . De NIC mag worden verbonden met een VNet die zich in de dezelfde Azure [locatie](https://azure.microsoft.com/regions) en het [abonnement](../azure-glossary-cloud-terminology.md#subscription) als de NIC Nadat een NIC is gemaakt, kunt u het subnet dat is verbonden met wijzigen, maar u kunt de VNet verbonden met niet wijzigen.
3. Heeft een naam toegewezen aan deze die niet worden gewijzigd nadat de NIC is gemaakt. De naam moet uniek zijn binnen een Azure [resourcegroep](../azure-resource-manager/resource-group-overview.md#resource-groups), maar niet moet uniek zijn binnen het abonnement, de Azure locatie die deze gemaakt in of de VNet verbonden met. Verschillende NIC worden doorgaans gemaakt in een Azure-abonnement. Het wordt aanbevolen dat u een naamgevingsconventie die wordt het beheren van meerdere NIC gemakkelijker dan het gebruik van de standaardnamen ontwerpen. Zie het artikel [aanbevolen naamgevingsconventies voor Azure resources](../guidance/guidance-naming-conventions.md) voor suggesties.
4. Een VM kan worden gekoppeld, maar alleen kan worden gekoppeld aan een enkele VM die zich in dezelfde locatie als het NIC
5. Heeft een MAC-adres, persistent is gemaakt met de NIC voor zo lang maken als het bijgevoegde voor een VM blijft. Het MAC-adres blijft behouden of de VM opnieuw is opgestart (uit in het besturingssysteem) of gestopt (opgeheven toegewezen) en de slag met de Portal Azure, Azure PowerShell of voor de opdrachtregel Azure. Als deze is losgekoppeld van een VM en die zijn bijgevoegd bij een andere VM, ontvangt de NIC een andere MAC-adres. Als de NIC wordt verwijderd, wordt het MAC-adres is toegewezen aan andere NIC's.
6. Hebben moet één primaire **privé** *IPv4-* statische of dynamische IP-adres toegewezen aan deze.
8. Een openbare IP-adres resource kan bijbehorende hebben toe.
9. Ondersteunt versneld netwerken met één-hoofdsite i/o-virtualization (SR-IOV) voor specifieke VM formaten specifieke versies van het besturingssysteem van Microsoft Windows Server wordt uitgevoerd. Lees het artikel [Accelerated toegang voor een virtuele machine](virtual-network-accelerated-networking-powershell.md) meer informatie over deze functie PREVIEW.
10. Verkeer niet privé IP-adressen die zijn toegewezen aan deze als IP-doorsturen is ingeschakeld voor de NIC kan ontvangen Als een VM bijvoorbeeld firewallsoftware wordt uitgevoerd, worden deze doorgestuurd voor een eigen IP-adressen. De VM moet nog steeds software routering of doorschakelen verkeer kan worden uitgevoerd, maar hiervoor IP-doorsturen moet zijn ingeschakeld voor een NIC
11. Vaak gemaakt in dezelfde resourcegroep als de VM deze gekoppeld aan of de dezelfde VNet die deze gekoppeld aan, hoewel het niet nodig om te worden.

## <a name="vms-with-multiple-network-interfaces"></a>VMs met meerdere netwerkinterfaces

Meerdere NIC's kunnen worden gekoppeld aan een VM, maar wanneer u dit doet, moet u zich realiseren van de volgende opties:  

- De grootte VM moet meerdere NIC's ondersteunen. Lees meer informatie over VM grootten meerdere NIC's ondersteunen, de [grootte van Windows Server VM](../virtual-machines/virtual-machines-windows-sizes.md) of [Linux VM tekengrootten](../virtual-machines/virtual-machines-linux-sizes.md) artikelen.   
- De VM moet worden gemaakt met ten minste twee NIC's. Als de VM is gemaakt met slechts één NIC, zelfs als de grootte VM meer dan één ondersteunt, kunt u aanvullende NIC geen voor VM toevoegen nadat deze gemaakt. Zo lang maken als de VM met ten minste twee NIC is gemaakt, kunt u koppelen extra NIC voor VM nadat deze gemaakt, zolang de grootte VM meer dan twee NIC ondersteunt.  
- Als de VM ten minste drie NIC's die zijn gekoppeld heeft, kunt u de secundaire NIC's (de primaire NIC kan niet worden losgekoppeld) loskoppelen van een VM. U kunt NIC's kan niet loskoppelen als de VM twee heeft of minder NIC gekoppeld.  
- De volgorde van de NIC's uit binnen de VM wordt willekeurig, en ook tussen Azure-infrastructuur updates kan wijzigen. Echter de IP-adressen en de bijbehorende ethernet MAC adressen, ongewijzigd blijft. Stel dat het besturingssysteem geeft aan wat Azure NIC1 als Eth1. Eth1 heeft IP-adres 10.1.0.100 en MAC-adres 00-0D-3A-B0-39-0D. Nadat een Azure infrastructuur bijwerken en start opnieuw op, het besturingssysteem mogelijk Azure NIC1 als Eth2 zien, maar het IP- en MAC-adressen niet hetzelfde zijn als deze zijn als het besturingssysteem geïdentificeerd Azure NIC1 als Eth1. Wanneer een herstart handeling van de klant is, ongewijzigd de volgorde NIC blijft in het besturingssysteem.  
- Als de VM deel van een [beschikbaarheid instellen uitmaakt](../azure-glossary-cloud-terminology.md#availability-set), moeten een één NIC of meerdere NIC alle VMs binnen de beschikbaarheid van de set hebben. Als de VMs meerdere NIC's, het nummer hebt worden de velden is niet verplicht moet hetzelfde, zo lang maken als elke VM ten minste twee netwerkadapters heeft.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het maken van een VM met een enkel NIC Lees het artikel [een VM maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- Informatie over het maken van een VM met meerdere NIC Lees het artikel [een VM met meerdere NIC Deploy](virtual-network-deploy-multinic-arm-ps.md) .
- Informatie over het maken van een NIC met meerdere IP-configuraties Lees het artikel [meerdere IP-adressen voor Azure virtuele machines](virtual-network-multiple-ip-addresses-powershell.md) .
