<properties
   pageTitle="Maak een VM Linux met meerdere NIC | Microsoft Azure"
   description="Informatie over het maken van een Linux VM met meerdere NIC's die zijn bijgevoegd bij het gebruik van de Azure CLI of resourcemanager sjablonen."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Een VM Linux maken met meerdere NIC 's
U kunt een virtuele machine (VM) maken in Azure wordt aangegeven met meerdere virtueel netwerk-interfaces (NIC) gekoppeld. Een gebruikelijk zou zijn om met verschillende subnetten voor front-end en back-end-connectiviteit of een specifiek voor een oplossing controleren of back-netwerk. In dit artikel vindt snelle opdrachten voor het maken van een VM met meerdere NIC's die zijn gekoppeld. Voor gedetailleerde informatie, waaronder over het maken van meerdere NIC binnen uw eigen scripts we vaker doen, Zie voor meer informatie over het [implementeren van meerdere NIC's VMs](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Verschillende [grootten VM](virtual-machines-linux-sizes.md) een verschillend aantal NIC wordt ondersteund, dus het formaat van uw VM dienovereenkomstig gewijzigd.

>[AZURE.WARNING] Wanneer u een VM maken - u NIC niet aan een bestaande VM toevoegen, moet u meerdere NIC's koppelen. U kunt [een VM op basis van de oorspronkelijke virtuele schijven maken](virtual-machines-linux-copy-vm.md) en meerdere NIC maken terwijl u de VM implementeren.

## <a name="quick-commands"></a>Snelle opdrachten
Zorg ervoor dat u hebt de [Azure CLI](../xplat-cli-install.md) aangemeld en het gebruik van resourcemanager modus:

```bash
azure config mode arm
```

In de volgende voorbeelden, kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `myVM`.

Maak eerst een resourcegroep. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `WestUS` locatie:

```bash
azure group create myResourceGroup -l WestUS
```

Maak een account opslagruimte voor uw VMs. Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Een virtueel netwerk om uw VMs aan verbinding te maken. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet` met het voorvoegsel van een adres van `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Maak twee virtueel netwerksubnetten - één telefoonnummer voor de front-verkeer is toegestaan en één voor back-enddatabase verkeer. Het volgende voorbeeld wordt twee subnetten, met de naam `mySubnetFrontEnd` en `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Maak twee NIC's, één NIC subnet, de front- en één NIC koppelen aan het subnet back-enddatabase. Het volgende voorbeeld wordt twee NIC's, met de naam `myNic1` en `myNic2`, en deze is gekoppeld aan uw subnetten:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Tot slot maken uw VM, koppelen van de twee netwerkadapters die u eerder hebt gemaakt. Het volgende voorbeeld wordt een VM met de naam `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Meerdere NIC met Azure CLI maken
Als u een VM gebruik van de Azure CLI eerder hebt gemaakt, moeten de snelle opdrachten vertrouwd zijn. Het proces is dezelfde om één NIC of meerdere NIC's te maken. Hier vindt u meer informatie over het [implementeren van meerdere NIC's met de CLI Azure](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), inclusief het proces van het maken van alle NIC doorlopen scripts.

Het volgende voorbeeld wordt twee NIC's, met de naam `myNic1` en `myNic2`, met één NIC verbinding maken met elk subnet:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Maakt u gewoonlijk ook een [Beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) of [de belasting voor documentconversies](../load-balancer/load-balancer-overview.md) om te beheren en verkeer verdelen over uw VMs. De opdrachten kunt u opnieuw, zijn op hetzelfde tijdens het werken met meerdere NIC's. Het volgende voorbeeld wordt de beveiligingsgroep van een netwerk met de naam `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

Afhankelijk van uw NIC's over het gebruik van de beveiligingsgroep van netwerk `azure network nic set`. Het volgende voorbeeld wordt `myNic1` en `myNic2` met `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Wanneer u de VM maakt, opgeven u nu meerdere NIC. Liever met `--nic-name` om een enkele NIC, in plaats daarvan u `--nic-names` en geef een door komma's gescheiden lijst met NIC's. U moet ook Wees voorzichtig wanneer u de VM-grootte selecteren. Limieten voor het totale aantal NIC's die u aan een VM toevoegen kunt zijn. Lees meer over [Linux VM grootte](virtual-machines-linux-sizes.md). Het volgende voorbeeld ziet u hoe u meerdere NIC's en klikt u vervolgens een VM-grootte die ondersteuning biedt voor gebruik van meerdere NIC opgeeft (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Meerdere NIC met resourcemanager sjablonen maken
Declaratieve JSON-bestanden Azure resourcemanager sjablonen gebruiken om te definiëren van uw omgeving. Hier vindt u een [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Resourcemanager sjablonen kunnen u meerdere exemplaren van een resource tijdens de implementatie, zoals het maken van meerdere NIC's maken. U *kopiëren* gebruiken om op te geven hoe vaak maken:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Meer informatie over het [maken van meerdere exemplaren via *kopiëren*](../resource-group-create-multiple.md). 

U kunt ook een `copyIndex()` vervolgens een nummer toevoegen aan de naam van een resource, waarmee u maken `myNic1`, `myNic2`, enz. Hier volgt een voorbeeld van de indexwaarde toe te voegen:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Een volledig voorbeeld van het [maken van meerdere NIC resourcemanager sjablonen gebruiken](../virtual-network/virtual-network-deploy-multinic-arm-template.md), kunt u lezen.

## <a name="next-steps"></a>Volgende stappen
Zorg ervoor dat moet worden gereviseerd [Linux VM grootte](virtual-machines-linux-sizes.md) als u probeert te maken van een VM met meerdere NIC's. Let op het maximum aantal NIC elke grootte VM ondersteunt. 

Vergeet niet dat u extra NIC niet toevoegen aan een bestaande VM, moet u de NIC's maken wanneer u de VM implementeert. Wees voorzichtig bij het plannen van uw implementaties om ervoor te zorgen dat u de vereiste netwerkverbinding vanaf het begin hebt.