<properties
   pageTitle="Beschikbaarheid en schaal in Azure resourcemanager sjablonen | Microsoft Azure"
   description="Azure virtuele machines DotNet Core zelfstudie"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="availability-and-scale-in-azure-resource-manager-templates"></a>Beschikbaarheid en schaal in resourcemanager Azure-sjablonen

Beschikbaarheid en schaal verwijzen naar beschikbaarheid en de mogelijkheid om te voldoen aan de vraag. Als een toepassing een 99,9% van de tijd moet, moet deze moet een architectuur waarmee voor meerdere gelijktijdige berekeningscluster resources. In plaats van één website, bijvoorbeeld bevat een configuratie met een hoger niveau van de beschikbaarheid van meerdere exemplaren van dezelfde site, met het verdelen van technologie vóór deze. In deze configuratie wordt kan één exemplaar van de toepassing worden genomen omlaag voor onderhoud, terwijl de resterende blijven functioneren. Schaal worden echter verwijst naar een mogelijkheid toepassingen moet fungeren aanvraag. Met een laden kan gebalanceerde-toepassing, toevoegen of verwijderen van exemplaren van de groep een toepassing aan de nieuwe schaal om te voldoen aan de vraag.

In dit document een gedetailleerd overzicht van hoe de implementatie van de steekproef muziek Store is geconfigureerd voor beschikbaarheid en schaal. Alle afhankelijkheden en unieke configuraties zijn gemarkeerd. Voor een optimale ervaring, vooraf een exemplaar van de oplossing aan uw Azure-abonnement en werk samen met de sjabloon Azure resourcemanager implementeren. De volledige sjabloon vindt u hier: [Muziek Store-implementatie op Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="availability-set"></a>Beschikbaarheid instellen

Een beschikbaarheid instellen betrekking logisch hebben op Azure virtuele Machines over fysieke hosts en andere infrastructurele onderdelen zoals power voorraad en fysieke netwerkhardware. Beschikbaarheid sets Zorg ervoor dat tijdens het onderhoud, apparaat mislukt of andere omlaag tijd, niet alle virtuele machines worden gedaan. Een beschikbaarheid instellen kunnen worden toegevoegd aan een resourcemanager Azure-sjabloon met de Visual Studio toevoegen Wizard Nieuwe bron of geldige JSON invoegen in een sjabloon.

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [Beschikbaarheid instellen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

Een beschikbaarheid instellen wordt als een eigenschap van een resource VM gedeclareerd. 

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [koppeling beschikbaarheid instellen met VM](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).


```none
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
De beschikbaarheid instellen, zoals gezien van de Azure-portal. Elke virtuele machine en meer informatie over de configuratie hier gedetailleerde.

![Beschikbaarheid instellen](./media/virtual-machines-linux-dotnet-core/aset.png)

Zie voor gedetailleerde informatie op beschikbaarheid Sets [beheren beschikbaarheid van virtuele machines](./virtual-machines-linux-manage-availability.md). 

## <a name="network-load-balancer"></a>Taakverdeling netwerk

Terwijl een set beschikbaarheid fouttolerantie van toepassing biedt, beschikbaar een taakverdeling veel exemplaren van de toepassing op een adres van één netwerk. Meerdere exemplaren van een toepassing kunnen worden gehost op veel virtuele machines, elkaar verbonden met een taakverdeling. Als de toepassing wordt geopend, stuurt de taakverdeling de binnenkomende aanvraag over het bijgevoegde leden. Een taakverdeling kan worden toegevoegd met de Visual Studio toevoegen Wizard Nieuwe bron of door in te voegen correct opgemaakt als JSON resource in de sjabloon resourcemanager Azure.

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [Taakverdeling netwerk](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

Omdat de toepassing van de steekproef is blootgesteld aan internet met een openbare IP-adres, is dit adres is gekoppeld aan de taakverdeling. 

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager- [koppeling van taakverdeling netwerk met openbare IP-adres](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

In de portal Azure ziet u het overzicht van netwerk laden de verdeling van de koppeling met het openbare IP-adres.

![Taakverdeling netwerk](./media/virtual-machines-linux-dotnet-core/nlb.png)

## <a name="load-balancer-rule"></a>Laden de verdeling van regel

Wanneer u met een taakverdeling, worden regels waarmee wordt bepaald hoe verkeer wordt verdeeld over de beoogde bronnen geconfigureerd. Zorg dat de steekproef muziek Store-toepassing, wordt verkeer binnenkomt op poort 80 van het openbare IP-adres en is verdeeld over poort 80 van alle virtuele machines. 

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [Laden de verdeling van regel](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).


```none
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Een weergave van de regel netwerk laden de verdeling van de portal.

![Regel voor de verdeling van netwerk laden](./media/virtual-machines-linux-dotnet-core/lbrule.png)

## <a name="load-balancer-probe"></a>Laden-test voor de verdeling

Verdeling van de belasting moet ook controleren van elke virtuele machine, zodat alleen op computers worden aanvragen verzonden. Deze controle wordt uitgevoerd door een constant scannen van een vooraf gedefinieerde poort. De muziek Store-implementatie is geconfigureerd voor het zoeken van poort 80 op alle opgenomen virtuele machines. 

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [Laden de verdeling van zoeken](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).


```none
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

De verdeling van laden test gezien van de Azure-portal.

![Netwerk laden verdeling-test](./media/virtual-machines-linux-dotnet-core/lbprobe.png)

## <a name="inbound-nat-rules"></a>Regels voor binnenkomende NAT

Wanneer u met een taakverdeling, moeten de regels in plaats die niet-laden gebalanceerde toegang verlenen tot elke virtuele Machine worden gebracht. Wanneer u een SSH-verbinding maakt met elke virtuele machine, bijvoorbeeld dit verkeer niet moet worden verdeeld, liever een vooraf bepaald pad moet worden geconfigureerd. vooraf bepaalde paden worden geconfigureerd met behulp van een resource NAT-regel voor binnenkomende verbindingen. Gebruik van deze resource kan inkomende communicatie worden toegewezen aan afzonderlijke virtuele Machines. 

Zorg dat de toepassing muziek Store, is een poort beginnend bij 5000 toegewezen aan poort 22 op elke virtuele Machine voor SSH-toegang. De `copyindex()` functie wordt gebruikt om de inkomende poort, verhoogd, zodat de tweede virtuele Machine ontvangt een inkomende poort van 5001, de derde 5002, enzovoort.

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [NAT-regels voor binnenkomende verbindingen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270). 

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

Een voorbeeld inkomende NAT-regel, zoals gezien in de portal van Azure. Een regel voor het SSH NAT is gemaakt voor elke virtuele machine in de implementatie.

![Binnenkomende NAT-regel](./media/virtual-machines-linux-dotnet-core/natrule.png)

Zie [taakverdeling voor Azure infrastructuurservices](./virtual-machines-linux-load-balance.md)voor uitgebreide informatie over de verdeling van de Azure netwerk belasting.

## <a name="deploy-multiple-vms"></a>Meerdere VMs implementeren

Tot slot zijn voor een beschikbaarheid instellen of een taakverdeling effectief, functie, meerdere virtuele machines zijn vereist. Meerdere VMs kunnen worden geïmplementeerd met de functie Azure resourcemanager sjabloon kopiëren. Met de functie kopiëren, is het niet nodig is om te definiëren van een bepaald aantal virtuele Machines, in plaats daarvan deze waarde kan dynamisch kan worden verstrekt op het moment van implementatie. De functie kopiëren gebruikt het aantal exemplaren gemaakt en grepen het juiste aantal virtuele machines en bijbehorende hulpbronnen implementeren.

In de voorbeeldsjabloon muziek Store, een parameter die in een aantal exemplaar wordt gedefinieerd. Dit nummer wordt gebruikt in de sjabloon bij het maken van virtuele machines en verwante resources.

```none
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

Klik op de resource VM krijgt de lus kopiëren een naam en het aantal exemplaren parameter wordt gebruikt om te bepalen hoeveel resulterende exemplaren van de.

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager- [Functie van VM kopiëren](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300). 


```none
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

De huidige iteratie van de kopieerfunctie kan worden geopend met de `copyIndex()` functie. De waarde van de functie kopiëren index kan worden gebruikt naam opgeven voor virtuele machines en andere resources. Als twee instanties van een virtuele machine zijn geïmplementeerd, moeten ze bijvoorbeeld verschillende namen. De `copyIndex()` functie kan worden gebruikt als onderdeel van de naam van de virtuele machine maken van een unieke naam. Een voorbeeld van de `copyindex()` functie die wordt gebruikt voor de naamgeving van toepassing is zichtbaar in de virtuele Machine resource. Hier is de naam van de computer een samenvoeging van de `vmName` parameter, en de `copyIndex()` functie. 

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [De functie Index kopiëren](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319). 


```none
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

De `copyIndex` functie enkele malen wordt gebruikt in de voorbeeldsjabloon muziek Store. Resources en functies die gebruikmaken van `copyIndex` bevat alle specifiek is voor één exemplaar van de virtuele machine zoals netwerk-interface, de verdeling van regels voor laden en een afhankelijk van de functies. 

Zie [meerdere exemplaren van resources in Azure resourcemanager maken](../resource-group-create-multiple.md)voor meer informatie over de functie kopiëren.

## <a name="next-step"></a>Volgende stap

<hr>

[Stap 4 - toepassingsimplementatie waarbij Azure resourcemanager-sjablonen](./virtual-machines-linux-dotnet-core-5-app-deployment.md)