<properties
   pageTitle="Open poorten en eindpunten naar een Linux VM | Microsoft Azure"
   description="Leer hoe u een poort openen / maken van een eindpunt voor uw Linux VM met het model Azure resource manager implementatie en de CLI Azure"
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
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Het openen van poorten en eindpunten naar een Linux VM in Azure wordt aangegeven
U een poort openen of maak een eindpunt, met een virtuele machine (VM) in Azure wordt aangegeven door een netwerk-filter maken voor een subnet of VM netwerkinterface. U plaats deze filters, die binnenkomende en uitgaande verkeer bepalen, op een netwerk-beveiligingsgroep die is gekoppeld aan de resource die het verkeer ontvangt. Laten we eens gebruikt u een voorbeeld van de web-verkeer is toegestaan op poort 80.

## <a name="quick-commands"></a>Snelle opdrachten
Een beveiligingsgroep netwerk en regels die u nodig hebt met [De CLI Azure](../xplat-cli-install.md) hebt geïnstalleerd en gebruikt resourcemanager modus maken:

```bash
azure config mode arm
```

In de volgende voorbeelden, kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen `myResourceGroup`, `myNetworkSecurityGroup`, en `myVnet`.

Maak de beveiligingsgroep van uw netwerk, correct uw eigen namen en de locatie in te voeren. Het volgende voorbeeld wordt de beveiligingsgroep van een netwerk met de naam `myNetworkSecurityGroup` in de `WestUS` locatie:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Een regel wilt toestaan dat HTTP-verkeer naar uw webserver (of aanpassen voor uw situatie, zoals SSH of access-database connectivity) toevoegen. Het volgende voorbeeld wordt een regel met de naam `myNetworkSecurityGroupRule` toe te staan dat TCP-verkeer is toegestaan op poort 80:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

Koppel de beveiligingsgroep van netwerk met van uw VM netwerkinterface (NIC). Het volgende voorbeeld wordt een bestaande NIC met de naam `myNic` met het netwerk-beveiligingsgroep met de naam `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

U kunt ook de beveiligingsgroep van uw netwerk koppelen met een virtueel netwerk subnet in plaats van alleen de netwerkinterface op een enkele VM. Het volgende voorbeeld wordt een bestaand subnet met de naam `mySubnet` in de `myVnet` virtuele netwerk met het netwerk-beveiligingsgroep met de naam `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Meer informatie over het netwerk beveiligingsgroepen
De snelle opdrachten hier kunnen u aan de slag met verkeer die doorloopt naar uw VM. Beveiligingsgroepen netwerk bieden veel handige functies en granulatie voor toegang tot uw resources beheren. U kunt meer informatie over het [maken van een beveiligingsgroep netwerk en hier ACL regels](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

U kunt netwerk-beveiligingsgroepen en ACL regels definiëren als onderdeel van Azure resourcemanager sjablonen. Lees meer over het [maken van netwerk beveiligingsgroepen met sjablonen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Als u gebruiken poort doorverbinden wilt naar een unieke externe poort toewijzen aan een interne poort op uw VM, gebruikt u een taakverdeling en regels voor vertaling (Network Address Translation). U wilt bijvoorbeeld TCP poort 8080 extern en hebben verkeer naar TCP-poort 80 op een VM wordt gestuurd. U kunt meer informatie over het [maken van een verdeling van de belasting internetgerichte](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld kunt u een eenvoudige regel als u wilt dat HTTP-verkeer is toegestaan gemaakt. Hier vindt u informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:

- [Azure resourcemanager-overzicht](../azure-resource-manager/resource-group-overview.md)
- [Wat is een netwerk beveiliging groep (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure resourcemanager overzicht voor netwerktaakverdelers](../load-balancer2    /load-balancer-arm.md)