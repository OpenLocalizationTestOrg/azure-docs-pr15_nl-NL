
<properties
   pageTitle="Een volledige Linux-omgeving met de CLI Azure | Microsoft Azure"
   description="Opslag, een VM Linux, een virtueel netwerk en subnet, een taakverdeling, een NIC, een openbare IP-adres en een beveiligingsgroep netwerk, alle helemaal omhoog met behulp van de Azure CLI maken."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Een volledige Linux-omgeving maken met behulp van de Azure CLI

In dit artikel maken we een eenvoudig netwerk met een taakverdeling en een paar VMs die handig voor het ontwikkelen en eenvoudige computing zijn. We begeleiden in het proces door opdracht totdat u twee werktijd, beveiligde Linux VMs hebt-waarmee u verbinding vanuit een willekeurige plaats op Internet maken kunt. U kunt vervolgens op verplaatsen naar meer complexe netwerken en omgevingen.

Daarbij, u meer informatie over de afhankelijkheidshiërarchie dat het implementatiemodel resourcemanager wordt uitgelegd, en over hoeveel power deze bevat. Nadat u hoe het systeem is gemaakt ziet, kunt u deze veel sneller opnieuw met behulp van [Azure resourcemanager sjablonen](../resource-group-authoring-templates.md). Ook nadat u hoe de onderdelen van uw omgeving in elkaar passen leert, makkelijker maken van sjablonen als u wilt automatiseren.

De omgeving bevat:

- Twee VMs binnen een set beschikbaarheid.
- Een taakverdeling met een regel taakverdeling op poort 80.
- Groep (NSG) beveiligingsregels uw VM beschermen tegen ongewenste verkeer netwerk.

![Overzicht van de Basic-omgeving](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Als u wilt deze aangepaste omgeving hebt gemaakt, moet u de meest recente [Azure CLI](../xplat-cli-install.md) in resourcemanager-modus (`azure config mode arm`). Moet u ook een JSON parseren van hulpmiddel. In dit voorbeeld wordt [jq](https://stedolan.github.io/jq/)gebruikt.

## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende informatie in de sectie snel het grondtal de opdrachten voor het uploaden van een VM naar Azure. Meer gedetailleerde informatie en context voor elke stap u in de rest van het document vindt, begin [hier](#detailed-walkthrough).

Zorg ervoor dat u hebt [De CLI Azure](../xplat-cli-install.md) aangemeld en het gebruik van resourcemanager modus:

```bash
azure config mode arm
```

In de volgende voorbeelden, kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Parameternamen voorbeeld opnemen `myResourceGroup`, `mystorageaccount`, en `myVM`.

Maak de resourcegroep. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `westeurope` locatie:

```bash
azure group create -n myResourceGroup -l westeurope
```

Controleer of de resourcegroep met behulp van de JSON-parser:

```bash
azure group show myResourceGroup --json | jq '.'
```

Maak de opslag-account. Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount` (de opslagaccountnaam moet uniek zijn, dus Geef uw eigen unieke naam):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Controleer of de opslag-account met behulp van de JSON-parser:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Het virtuele netwerk maken. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Een subnet maken. Het volgende voorbeeld wordt een subnet met de naam `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Controleer of het virtuele netwerk en subnet met behulp van de JSON-parser:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Maak een openbare IP-adres. Het volgende voorbeeld wordt een openbare IP-adres met de naam `myPublicIP` met de DNS-naam van `mypublicdns` (de naam van de DNS-moet uniek zijn, dus Geef uw eigen unieke naam):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Maak de taakverdeling. Het volgende voorbeeld wordt een taakverdeling met de naam `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Een front IP-toepassingen voor de taakverdeling maken en het openbare IP koppelen. Het volgende voorbeeld wordt een front IP-toepassingen met de naam `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

Maak de back-enddatabase IP-toepassingen voor de taakverdeling. Het volgende voorbeeld wordt een back-enddatabase IP-toepassingen met de naam `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

SSH inkomende netwerk adres NAT (Translation) regels maken voor de taakverdeling. Het volgende voorbeeld wordt twee laden de verdeling van regels, `myLoadBalancerRuleSSH1` en `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Maken van het web inkomende NAT regels voor de taakverdeling. Het volgende voorbeeld wordt een laden de verdeling van regel met de naam`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Maak de belasting voor gelijkmatige verdeling systeemstatus-test. Het volgende voorbeeld wordt een TCP-test met de naam `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Controleer of de taakverdeling, IP-toepassingen en NAT regels met behulp van de JSON-parser:

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Maak de eerste netwerkadapter (NIC). Vervang de `#####-###-###` secties met uw eigen Azure abonnement-ID. Uw abonnement ID wordt vermeld in de uitvoer van `jq` bij het onderzoek van de bronnen die u maakt. U kunt ook uw abonnements-ID met bekijken `azure account list`. 

Het volgende voorbeeld wordt een NIC met de naam `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

De tweede NIC maken Het volgende voorbeeld wordt een NIC met de naam `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Controleer of u de twee netwerkadapters met behulp van de JSON-parser:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Maak de beveiligingsgroep van het netwerk. Het volgende voorbeeld wordt de beveiligingsgroep van een netwerk met de naam `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Voeg twee regels voor binnenkomende verbindingen voor de beveiligingsgroep van het netwerk. Het volgende voorbeeld wordt twee regels, `myNetworkSecurityGroupRuleSSH` en `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Controleer of de beveiligingsgroep van netwerk en regels voor binnenkomende verbindingen met behulp van de JSON-parser:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

De beveiligingsgroep van netwerk verbinden met de twee netwerkadapters:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

De beschikbaarheid van de set maken. Het volgende voorbeeld wordt een set met de naam beschikbaarheid `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

De eerste Linux VM maken. Het volgende voorbeeld wordt een VM met de naam `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

De tweede Linux VM maken. Het volgende voorbeeld wordt een VM met de naam `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

De JSON-parser gebruiken om te controleren of alles die is gemaakt:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Uw nieuwe omgeving exporteren naar een sjabloon om snel nieuwe exemplaren opnieuw te maken:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht
De gedetailleerde stappen die volgt wordt uitgelegd wat elke opdracht doet terwijl u uw omgeving bouwen. Deze concepten zijn handig als u uw eigen aangepaste omgevingen voor ontwikkel- of samenstelt.

Zorg ervoor dat u hebt [De CLI Azure](../xplat-cli-install.md) aangemeld en het gebruik van resourcemanager modus:

```bash
azure config mode arm
```

In de volgende voorbeelden, kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Parameternamen voorbeeld opnemen `myResourceGroup`, `mystorageaccount`, en `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Resourcegroepen maken en kies implementatie locaties

Azure resourcegroepen zijn logische implementatie entiteiten met configuratiegegevens en metagegevens om het inschakelen van het logische beheer van de resource implementaties. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `westeurope` locatie:

```bash
azure group create --name myResourceGroup --location westeurope
```

Uitvoer:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Een opslag-account maken

Opslag accounts moet u voor uw schijven VM en voor schijven aanvullende gegevens die u wilt toevoegen. U maken opslag-accounts vrijwel direct nadat u resourcegroepen maken.

Hier we gebruiken de `azure storage account create` opdracht, doorgeven van de locatie van het account, de resourcegroep die wordt bepaald, en het type opslagondersteuning dat u wilt. Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Uitvoer:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Naar het bestuderen van onze resourcegroep met behulp van de `azure group show` opdracht, laten we eens gebruik het hulpprogramma [jq](https://stedolan.github.io/jq/) samen met de `--json` Azure CLI optie. (U kunt **jsawk** of een taal-bibliotheek die u wilt de JSON parseren gebruiken).

```bash
azure group show myResourceGroup --json | jq '.'
```

Uitvoer:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Als u wilt onderzoeken het opslag-account met behulp van de CLI, moet u eerst de accountnamen en toetsen instellen. De naam van de opslag-account in het volgende voorbeeld vervangen door een naam die u kiest:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Vervolgens kunt u de informatie van uw opslagruimte eenvoudig weergeven:

```bash
azure storage container list
```

Uitvoer:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en subnet maken

Vervolgens gaat u moet maken van een virtueel netwerk uitgevoerd in Azure en een subnet waarin u uw VMs kunt maken. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet` met de `192.168.0.0/16` adresvoorvoegsel:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Uitvoer:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Klik nogmaals document met de optie--json van `azure group show` en **jq** om te zien hoe we onze resources bouwen. Nu een `storageAccounts` resource en een `virtualNetworks` resource.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Uitvoer:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Nu maken een subnet in de `myVnet` virtueel netwerk waarin de VMs zijn geïmplementeerd. We gebruiken de `azure network vnet subnet create` command, samen met de resources die we al hebt gemaakt: de `myResourceGroup` resourcegroep en de `myVnet` virtueel netwerk. Voeg in het volgende voorbeeld wordt het subnet met de naam `mySubnet` met het adres subnetprefix `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Uitvoer:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Omdat het subnet logisch binnen het virtuele netwerk is, zoekt we de informatie subnet met de opdracht van een iets anders. De opdracht we gebruiken is `azure network vnet show`, maar we gaat u verder met de JSON-uitvoer bestuderen met behulp van **jq**.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Uitvoer:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Een openbare IP-adres (PIP) maken

Nu het openbare IP-adres (PIP) die we aan uw taakverdeling toewijzen maken. Dit kunt u verbinding maken met uw VMs van Internet via de `azure network public-ip create` opdracht. Omdat het standaarde-mailadres dynamisch is, we een benoemde DNS-vermelding maken in het domein **cloudapp.azure.com** met behulp van de `--domain-name-label` optie. Het volgende voorbeeld wordt een openbare IP-adres met de naam `myPublicIP` met de DNS-naam van `mypublicdns`. Als de DNS-naam moet uniek zijn, bieden u uw eigen unieke naam voor de DNS:

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Uitvoer:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Het openbare IP-adres is ook een resource worden op het hoogste niveau, zodat u deze met kan zien `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Uitvoer:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Kunt u meer informatie over resource, zoals de FQDN-naam (Fully Qualified Domain Name) van het subdomein, met behulp van de volledige onderzoeken `azure network public-ip show` opdracht. Het openbare IP-adres resource logisch is toegewezen, maar een specifiek adres nog niet is toegewezen. Als u een IP-adres, gaat u naar de gewenste een taakverdeling, die we nog niet hebt gemaakt.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Uitvoer:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Een taakverdeling en IP-groepen maken
Wanneer u een taakverdeling maakt, kunt u verkeer verdelen over meerdere VMs. Het biedt ook redundantie in uw toepassing door meerdere VMs die op aanvragen van de gebruikers in het geval van onderhoud of dik laadtijd reageren uit te voeren. Het volgende voorbeeld wordt een taakverdeling met de naam `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Uitvoer:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Onze taakverdeling is vrij leeg, zodat we maken van sommige pools IP. We wilt maken van twee IP-toepassingen voor de verdeling van onze belasting - één voor de front-end en één voor de back-end. De front-IP-toepassingen is iedereen zichtbaar. Het is ook de locatie waarnaar toewijzen we de PIP die we eerder hebt gemaakt. Vervolgens gebruiken we de back-enddatabase van toepassingen als een locatie voor onze VMs verbinding maken met. Op die manier het verkeer door de verdeling van de belasting naar de VMs kan stromen.

Eerst onze front IP-groep maken. Het volgende voorbeeld wordt een front toepassingen met de naam `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Uitvoer:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Houd er rekening mee hoe we gebruikt de `--public-ip-name` veranderen door te geven de `myPublicIP` die we eerder hebt gemaakt. Het toewijzen van het openbare IP-adres aan de taakverdeling, kunt u uw VMs bereiken via Internet.

Vervolgens maken onze tweede IP-toepassingen, ditmaal voor onze verkeer back-enddatabase. Het volgende voorbeeld wordt een back-enddatabase toepassingen met de naam `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Uitvoer:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

U ziet hoe onze taakverdeling doet door op te zoeken met `azure network lb show` en dat de JSON-uitvoer onderzoeken:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Uitvoer:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Taakverdeling NAT-regels maken
Als u verkeer die doorloopt tot en met onze taakverdeling, moeten we netwerkadres NAT (Translation) regels maken die binnenkomende of uitgaande acties bepalen. U kunt opgeven van het protocol gebruiken en vervolgens externe poorten toewijzen aan interne poorten naar wens. Voor onze omgeving, laten we enkele regels waarmee SSH tot en met onze taakverdeling naar onze VMs maken. We instellen TCP-poorten 4222 en 4223 om te verwijzen naar TCP-poorten 22 op onze VMs (die we later maken). Het volgende voorbeeld wordt een regel met de naam `myLoadBalancerRuleSSH1` moet worden toegewezen TCP-poorten 4222 poort 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Uitvoer:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Herhaal de procedure voor de tweede regel van de NAT voor SSH. Het volgende voorbeeld wordt een regel met de naam `myLoadBalancerRuleSSH2` moet worden toegewezen TCP-poorten 4223 poort 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Laten we ook verdergaan en een NAT regel maken voor TCP-poort 80 voor Internet-verkeer, de regel tot onze IP-toepassingen installeren. Als we de regel met IP-groep, in plaats van het installeren van de regel moet onze VMs afzonderlijk, aansluiten kunnen we toevoegen of verwijderen van VMs uit de groep IP. Verdeling van de belasting wordt de stroom van verkeer automatisch aangepast. Het volgende voorbeeld wordt een regel met de naam `myLoadBalancerRuleWeb` moet worden toegewezen TCP-poort 80 poort 80:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Uitvoer:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Maken van een laden de verdeling van systeemstatus-test

Een Servicestatus onderzoeken regelmatig controles in voor de VMs achter onze taakverdeling om te controleren of ze besturingsomgeving en reageren op vergaderverzoeken, zoals gedefinieerd. Als dit niet het geval is, wordt ze bent verwijderd uit de bewerking om ervoor te zorgen dat gebruikers worden niet wordt doorgestuurd naar deze. U kunt aangepaste controles voor de systeemstatus-test, samen met de intervallen en time-out waarden definiëren. Zie voor meer informatie over gezondheid sondes, [controleert van taakverdeling](../load-balancer/load-balancer-custom-probe-overview.md). Het volgende voorbeeld wordt een TCP systeemstatus voortgangstekst benoemde `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Uitvoer:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Hier opgegeven we een interval van 15 seconden voor de controles van onze status. We kunnen maximaal vier sondes (één minuut) voordat de taakverdeling acht of de host niet meer werkt mist.

## <a name="verify-the-load-balancer"></a>Controleer of de taakverdeling
Nu is de configuratie van de verdeling van laden voltooid. Hier volgen de stappen die u hebt gemaakt:

1. U hebt gemaakt eerst een taakverdeling.
2. Vervolgens een front IP-groep hebt gemaakt en toegewezen een openbare IP-adres.
3. Vervolgens kunt u een back-enddatabase IP-toepassingen die VMs kunnen worden verbonden met gemaakt.
4. Daarna kunt u NAT-regels die SSH naar de VMs voor beheer, samen met een regel waarmee TCP-poort 80 voor onze WebApp toestaan gemaakt.
5. Ten slotte kunt u een test systeemstatus geregeld wordt gecontroleerd of de VMs toegevoegd. Deze status-test zorgt ervoor dat gebruikers geen proberen voor toegang tot een VM die niet langer is werking of inhoud.

Laten we bekijken hoe uw taakverdeling eruitziet nu:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Uitvoer:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Een NIC voor gebruik met de Linux VM maken

NIC's zijn via een programma beschikbaar omdat u regels toepassen op het gebruik ervan kunt. U kunt ook meer dan één hebben. In de volgende `azure network nic create` opdracht, u de NIC naar de laden back-enddatabase IP-groep maken en koppelen aan de NAT-regel toe te staan SSH verkeer.
 
Vervang de `#####-###-###` secties met uw eigen Azure abonnement-ID. Uw abonnement ID wordt vermeld in de uitvoer van `jq` bij het onderzoek van de bronnen die u maakt. U kunt ook uw abonnements-ID met bekijken `azure account list`. 

Het volgende voorbeeld wordt een NIC met de naam `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Uitvoer:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

U kunt de details weergeven door rechtstreeks onderzoeken van de resource. U de resource bestuderen met behulp van de `azure network nic show` opdracht:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Uitvoer:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Nu maken we de tweede NIC, verbinding te opnieuw naar onze back-enddatabase IP-groep. Deze regel van de tweede tijd NAT toestemming geeft dat SSH-verkeer is toegestaan. Het volgende voorbeeld wordt een NIC met de naam `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Een beveiligingsgroep netwerk en regels maken

Nu maken we een beveiligingsgroep netwerk en de regels voor binnenkomende verbindingen die voor de toegang tot de NIC Een beveiligingsgroep netwerk kunnen worden toegepast op een NIC of subnet. U moet u regels om te bepalen de stroom van verkeer en afmelden bij uw VMs definiëren. Het volgende voorbeeld wordt de beveiligingsgroep van een netwerk met de naam `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Laten we de binnenkomende regel voor het NSG toe te staan dat binnenkomende verbindingen op poort 22 (voor ondersteuning van SSH) toevoegen. Het volgende voorbeeld wordt een regel met de naam `myNetworkSecurityGroupRuleSSH` toe te staan dat TCP-poorten 22:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Nu we toevoegen de binnenkomende regel voor het NSG toe te staan dat binnenkomende verbindingen op poort 80 (voor ondersteuning van web-verkeer is toegestaan). Het volgende voorbeeld wordt een regel met de naam `myNetworkSecurityGroupRuleHTTP` toe te staan dat TCP-poort 80:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] De binnenkomende regel is een filter voor binnenkomende netwerkverbindingen. In dit voorbeeld wordt de NSG aan de virtuele NIC VMs, dat wil zeggen dat elk verzoek tot poort 22 wordt doorgegeven aan de NIC op onze VM binden. Deze binnenkomende regel over een netwerkverbinding, en niet over een eindpunt, wat het is over in klassieke implementaties. Als u wilt een poort opent, moet u laten de `--source-port-range` ingesteld op '\*' (de standaardwaarde) inkomende aanmeldingsaanvragen van **eventuele** aanvragen van een poort accepteren. Poorten zijn meestal dynamisch.

## <a name="bind-to-the-nic"></a>Binding maken met de NIC

De NSG binden aan de NIC's. Moeten we onze NIC contact met ons netwerk-beveiligingsgroep. Beide opdrachten aansluiten op beide van onze NIC uitvoeren:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Een set beschikbaarheid maken
Beschikbaarheid sets help verspreiding uw VMs over foutenstructuuranalyse domeinen en de upgrade domeinen. Laten we een beschikbaarheid instellen voor uw VMs maken. Het volgende voorbeeld wordt een set met de naam beschikbaarheid `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Foutenstructuuranalyse domeinen definiëren een groepering van virtuele machines die een gemeenschappelijke power bron- en netwerk schakeloptie delen. Standaard worden de virtuele machines die zijn geconfigureerd binnen uw beschikbaarheid instellen voor maximaal drie foutenstructuuranalyse domeinen gescheiden. Het idee is een hardwareprobleem in een van deze domeinen foutenstructuuranalyse heeft geen invloed op elke VM waarop uw app wordt uitgevoerd. VMs verdeeld Azure automatisch over de domeinen van de fout wanneer deze in een set beschikbaarheid te plaatsen.

De upgrade domeinen geven aan groepen van virtuele machines en onderliggende fysieke hardware die kan worden gestart op hetzelfde moment. De volgorde waarin de upgrade domeinen opnieuw worden gestart inhoud mogelijk niet opeenvolgende tijdens gepland onderhoud, maar slechts één upgrade opnieuw is opgestart tegelijk. Klik nogmaals verdeelt Azure automatisch uw VMs over de upgrade domeinen wanneer ze op een site beschikbaarheid plaatsen.

Meer informatie over het [beheren van de beschikbaarheid van VMs](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>De VMs Linux maken

U kunt de opslag- en bronnen ter ondersteuning van Internet toegankelijke VMs hebt gemaakt. Nu we die VMs kunt maken en ze zijn beveiligd met een SSH-sleutel die geen wachtwoord. In dit geval gaan we een Ubuntu VM op basis van de meest recente LTS maken. We deze afbeelding-informatie zoeken via `azure vm image list`, zoals wordt beschreven in [het zoeken van Azure VM afbeeldingen](virtual-machines-linux-cli-ps-findimage.md).

We een afbeelding met de opdracht geselecteerd `azure vm image list westeurope canonical | grep LTS`. In dit geval we gebruiken `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Voor het laatste veld, geven we `latest` zodat in de toekomst wordt altijd de meest recente versie. (De tekenreeks die we gebruiken is `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Deze stap is vertrouwde voor iedereen die al is gemaakt door een ssh rsa openbare en persoonlijke sleutel koppelen op Linux of Mac met behulp van **ssh-keygen-t rsa -b 2048**. Als u hoeft niet alle paren certificaat sleutels uw `~/.ssh` directory, kunt u deze maken:

- Automatisch, met behulp van de `azure vm create --generate-ssh-keys` optie.
- Handmatig, met behulp van [de instructies voor het zelf maken](virtual-machines-linux-mac-create-ssh-keys.md).

U kunt ook de `--admin-password` methode om te verifiëren van uw verbindingen SSH nadat de VM is gemaakt. Deze methode is meestal minder veilig.

Maak de VM doordat alle onze bronnen en informatie samen met de `azure vm create` opdracht:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Uitvoer:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

U kunt verbinding maken met uw VM direct via uw standaard SSH sleutels. Zorg ervoor dat u de juiste poort opgeeft, aangezien we tot en met de taakverdeling geven. (Voor onze eerste VM we de NAT regel instellen voor poort 4222 doorsturen naar onze VM):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Uitvoer:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Verdergaan en uw tweede VM op dezelfde manier maken:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

En u kunt nu de `azure vm show myResourceGroup myVM1` opdracht om te bekijken wat u hebt gemaakt. U voert op dit moment uw VMs Ubuntu achter een taakverdeling in Azure wordt aangegeven dat u in kunt zich alleen met een combinatie van uw SSH sleutel (omdat wachtwoorden zijn uitgeschakeld). U kunt installeren nginx of httpd, een WebApp implementeren, en het verkeer stroom door de verdeling van de belasting aan beide van de VMs.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Uitvoer:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>De omgeving exporteren als een sjabloon
Nu dat u hebt gemaakt om deze omgeving, wat gebeurt er als u wilt maken van een extra ontwikkelomgeving met dezelfde parameters of een productieomgeving die overeenkomt met het? Resourcemanager JSON-sjablonen die alle parameters voor uw omgeving definiëren gebruikt. U bouwen volledige omgevingen door te verwijzen naar deze JSON-sjabloon. U kunt [handmatig JSON sjablonen maken](../resource-group-authoring-templates.md) of een bestaande omgeving voor het maken van de JSON-sjabloon voor u exporteren:

```bash
azure group export --name myResourceGroup
```

Hiermee maakt u deze opdracht de `myResourceGroup.json` bestand in uw huidige werkmap. Wanneer u een omgeving met deze sjabloon maakt, wordt u gevraagd om alle resourcenamen, inclusief de namen voor de taakverdeling, netwerkinterfaces of VMs. U kunt deze namen in uw sjabloonbestand vullen door toe te voegen de `-p` of `--includeParameterDefaultValue` -parameter voor de `azure group export` opdracht die eerder is aangegeven. De JSON-sjabloon als u wilt opgeven van de resourcenamen, of [een parameters.json-bestand maken](../resource-group-authoring-templates.md#parameters) waarmee de resourcenamen bewerken.

Een omgeving van uw sjabloon maken:

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

U wilt mogelijk Lees [meer over het implementeren van sjablonen](../resource-group-template-deploy-cli.md). Meer informatie over hoe u stapsgewijs omgevingen bijwerken, gebruikt u het parameterbestand, en sjablonen openen vanuit een opslaglocatie van één.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om te werken met meerdere netwerken onderdelen en VMs beginnen. Uw toepassing bouwen met behulp van de belangrijkste onderdelen kennis hier kunt u in dit Voorbeeldomgeving.
