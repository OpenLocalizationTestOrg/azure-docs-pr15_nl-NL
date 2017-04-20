<properties
   pageTitle="Open poorten voor een VM met behulp van de Azure portal | Microsoft Azure"
   description="Leer hoe u een poort openen / maken van een eindpunt voor uw Windows-VM met het model resource manager implementatie in de Portal van Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>Poorten voor een VM in Azure wordt aangegeven met behulp van de Azure portal openen
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Snelle opdrachten
U kunt ook [deze stappen via Azure PowerShell uitvoeren](virtual-machines-windows-nsg-quickstart-powershell.md).

U maakt eerst de beveiligingsgroep van uw netwerk. Selecteer een resourcegroep in de portal, klik op **toevoegen**, en vervolgens zoekt en selecteert u 'Netwerk beveiligingsgroep':

![Een beveiligingsgroep netwerk toevoegen](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Voer een naam voor de beveiligingsgroep van uw netwerk, selecteert u of een resourcegroep maken en selecteer een locatie. Klik op **maken** wanneer u klaar bent:

![Maak een beveiligingsgroep netwerk](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Selecteer uw nieuwe netwerk-beveiligingsgroep. Selecteer 'inkomende beveiligingsregels' en klik op de knop **toevoegen** als een regel wilt maken:

![Een binnenkomende regel toevoegen](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Geef een naam voor de nieuwe regel. Poort 80 wordt al standaard ingevoerd. Deze blade is waar u de bron, protocol en bestemming wijzigen wilt bij het toevoegen van extra regels aan de beveiligingsgroep van uw netwerk. Klik op **OK** om de regel te maken:

![Een binnenkomende regel maken](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

De laatste stap is het koppelen van de beveiligingsgroep van uw netwerk met een subnet of een specifieke netwerkinterface. Laten we de beveiligingsgroep van netwerk met een subnet koppelen Selecteer 'Subnetten' en klik vervolgens op **koppelen**:

![Een beveiligingsgroep netwerk koppelen aan een subnet](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Selecteer uw virtuele netwerk en selecteer vervolgens het juiste subnet:

![Een beveiligingsgroep netwerk koppelen aan virtuele netwerken](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

U hebt nu een beveiligingsgroep netwerk, een inkomende regel gemaakt waarmee staat verkeer op poort 80 en deze die is gekoppeld aan een subnet gemaakt. Een VMs die u verbinding met dat subnet maakt zijn op poort 80 bereikbaar.


## <a name="more-information-on-network-security-groups"></a>Meer informatie over het netwerk beveiligingsgroepen
De snelle opdrachten hier kunnen u aan de slag met verkeer die doorloopt naar uw VM. Beveiligingsgroepen netwerk bieden veel handige functies en granulatie voor toegang tot uw resources beheren. U kunt meer informatie over het [maken van een beveiligingsgroep netwerk en hier ACL regels](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

U kunt netwerk-beveiligingsgroepen en ACL regels definiëren als onderdeel van Azure resourcemanager sjablonen. Lees meer over het [maken van netwerk beveiligingsgroepen met sjablonen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Als u gebruiken poort doorverbinden wilt naar een unieke externe poort toewijzen aan een interne poort op uw VM, gebruikt u een taakverdeling en regels voor vertaling (Network Address Translation). U wilt bijvoorbeeld TCP poort 8080 extern en hebben verkeer naar TCP-poort 80 op een VM wordt gestuurd. U kunt meer informatie over het [maken van een verdeling van de belasting internetgerichte](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld kunt u een eenvoudige regel als u wilt dat HTTP-verkeer is toegestaan gemaakt. Hier vindt u informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:

- [Azure resourcemanager-overzicht](../azure-resource-manager/resource-group-overview.md)
- [Wat is een netwerk beveiliging groep (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure resourcemanager overzicht voor netwerktaakverdelers](../load-balancer/load-balancer-arm.md)
