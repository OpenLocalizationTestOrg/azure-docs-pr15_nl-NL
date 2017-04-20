<properties
   pageTitle="Resourcemanager sjabloon scenario | Microsoft Azure"
   description="Een stap voor stap Stapsgewijze instructies voor een resource manager sjabloon inrichting van een eenvoudige Azure IaaS architectuur."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# <a name="resource-manager-template-walkthrough"></a>Resourcemanager sjabloon Stapsgewijze instructies

Een van de eerste vragen bij het maken van een sjabloon als volgt "te starten?". Een kunt beginnen met een lege sjabloon, de basisstructuur wordt beschreven in de [sjabloon Authoring artikel](resource-group-authoring-templates.md#template-format)te volgen en de resources en de gewenste parameters en variabelen toevoegen. Een goed alternatief zou starten door te gaan met behulp van de [galerie met quickstart](https://github.com/Azure/azure-quickstart-templates) en zoekt u scenario's met de sjabloon die u probeert te maken. U kunt verschillende sjablonen samenvoegen of bewerk een bestaande aanpassen aan uw eigen specifieke scenario. 

We even verder kijken op een gemeenschappelijke infrastructuur:

* Twee virtuele machines die hetzelfde opslag-account gebruikt, worden in bepaalde beschikbaarheid en klik op hetzelfde subnet van een virtueel netwerk.
* Een enkel NIC en VM IP adres voor elke virtuele machine.
* Een taakverdeling met een regel op poort 80 van taakverdeling

![architectuur](./media/resource-group-overview/arm_arch.png)

In dit onderwerp begeleidt u bij de stappen voor het maken van een sjabloon resourcemanager voor deze infrastructuur van. De laatste sjabloon die u maakt is gebaseerd op een Quickstart sjabloon [2 VMs in een taakverdeling en de regels van taakverdeling](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/)genoemd.

Maar dat is een groot wilt maken in één keer, dus laten we eerst een opslag-account maken en het dashboard implementeren. Nadat u hebt onder de knie de opslag-account maken, wordt u de andere bronnen toevoegen en de infrastructuur voltooien zodat de sjabloon opnieuw te implementeren.

>[AZURE.NOTE] U kunt elk gewenst type editor gebruiken bij het maken van de sjabloon. Visual Studio biedt hulpmiddelen die sjabloon te vereenvoudigen, maar u hoeft niet Visual Studio om te voltooien van deze zelfstudie. Zie voor een zelfstudie over het gebruik van Visual Studio om te maken van een implementatie Web App en SQL-Database [maken en implementeren van Azure resourcegroepen tot en met Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## <a name="create-the-resource-manager-template"></a>De resourcemanager-sjabloon maken

De sjabloon is een JSON-bestand dat alle bronnen u implementeert definieert. Ook kunt u parameters die zijn opgegeven tijdens de implementatie variabelen die gemaakt op basis van andere waarden en expressies en uitvoer van de implementatie definiëren. 

Laten we beginnen met de eenvoudigste sjabloon:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Dit bestand opslaan als **azuredeploy.json** (Houd er rekening mee dat de sjabloon kan elke naam die u wilt hebben, net dat deze moet een json-bestand).

## <a name="create-a-storage-account"></a>Een opslag-account maken
In de sectie **bronnen** toevoegen een object dat u de opslag-account definieert, zoals hieronder wordt weergegeven. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

U mogelijk afvraagt waar deze eigenschappen en waarden afkomstig zijn uit. De eigenschappen **type**, **naam**, **apiVersion**en **locatie** zijn standaard elementen die beschikbaar voor alle typen zijn. U kunt meer informatie over de algemene elementen aan [Resources](resource-group-authoring-templates.md#resources). **de naam** is ingesteld op een waarde die u als locatie voor de die worden gebruikt door de resourcegroep tijdens de implementatie en **locatie doorgeeft** . Besproken hoe u een **type** en **apiVersion** in de onderstaande secties bepalen.

De sectie **Eigenschappen** bevat alle eigenschappen die uniek voor een bepaald brontype zijn. De waarden die u opgeeft in deze sectie wordt precies overeenkomen met de bewerking opslag in de REST API voor het maken van dat resourcetype. Wanneer u een account opslag maakt, moet u een **accountType**opgeven. Zoals u ziet in de [REST API voor het maken van een account opslag in](https://msdn.microsoft.com/library/azure/mt163564.aspx) de sectie eigenschappen van de REST-bewerking bevat ook een eigenschap **accountType** , en de toegestane waarden worden beschreven. In dit voorbeeld het accounttype is ingesteld op **Standard_LRS**, maar u kunt opgeven enig ander criterium aangeven of gebruikers toestaan zich te geven in het accounttype als een parameter.

Nu we springen terug naar de sectie **parameters** , en Zie hoe u de naam van het account opslag definiëren. U kunt meer informatie over het gebruik van parameters op [Parameters](resource-group-authoring-templates.md#parameters). 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Hier kunt u een parameter van het type tekenreeks waarin de naam van de opslag-account wordt ingevoerd gedefinieerd. De waarde voor deze parameter krijgt tijdens de sjabloonimplementatie.

## <a name="deploying-the-template"></a>De sjabloon implementeren
We hebben een volledige sjabloon voor het maken van een nieuw account voor de opslag. Als u het intrekken is, wordt de sjabloon is opgeslagen in **azuredeploy.json** bestand:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Er zijn helemaal een paar manieren om te implementeren van een sjabloon gebruiken, zoals u in het [artikel Resource-implementatie ziet](resource-group-template-deploy.md). Als u wilt de sjabloon via Azure PowerShell implementeert, gebruiken:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Of, als u wilt de sjabloon met Azure CLI implementeert, gebruiken:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

U bent nu de eigenaar van de trots van een opslag-account.

De volgende stappen worden alle resources die zijn vereist voor het implementeren van de architectuur die worden beschreven in het begin van deze zelfstudie toevoegen. U kunt deze resources worden toevoegen in de dezelfde sjabloon die u hebt gewerkt.

## <a name="availability-set"></a>Beschikbaarheid instellen
Na de definitie voor de opslag-account, Voeg een wanneer ingestelde voor de virtuele machines. In dit geval zijn er geen aanvullende eigenschappen vereist, zodat de definitie eenvoudig is. Zie de [REST API voor het maken van een beschikbaarheid instellen](https://msdn.microsoft.com/library/azure/mt163607.aspx) voor de sectie volledige eigenschappen voor het geval u wilt de update domain aantal en foutenstructuuranalyse domein waarden tellen definiëren.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

U ziet dat de **naam** is ingesteld op de waarde van een variabele. Voor deze sjabloon, die de naam van de beschikbaarheid van de set nodig in een paar andere locaties. Eenvoudiger kunt u uw sjabloon handhaven door die waarde eenmaal definiëren en gebruiken op meerdere locaties.

De waarde die u voor **type opgeeft** bevat zowel de provider van de resource en het resourcetype. De resource-provider is **Microsoft.Compute** en het resourcetype wordt **availabilitySets**voor beschikbaarheid sets. U kunt de lijst met beschikbare resource providers opvragen door de volgende PowerShell-opdracht uit te voeren:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Of, als u Azure CLI gebruikt, kunt u de volgende opdracht uitvoeren:
```
    azure provider list
```
Gezien het feit dat in dit onderwerp u met de opslag-accounts, virtuele machines en virtuele netwerken maakt, werkt u met:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Als u wilt zien welke van de resource voor een bepaalde aanbieder, voer de volgende PowerShell-opdracht:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Of, voor Azure CLI, de volgende opdracht retourneert de beschikbare typen in de indeling van JSON en sla deze op een bestand.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

Als een van de typen binnen **Microsoft.Compute**ziet u **availabilitySets** . De volledige naam van het type is **Microsoft.Compute/availabilitySets**. Hier ziet u de naam van het type resource naar een of meer van de informatiebronnen in u sjabloon.

## <a name="public-ip"></a>Openbare IP
Een openbare IP-adres definiëren. Nogmaals, kijkt u naar de [REST API voor openbare IP-adressen](https://msdn.microsoft.com/library/azure/mt163590.aspx) voor de eigenschappen om in te stellen.

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

De toewijzingsmethode is ingesteld op **dynamische** maar kunt u deze instellen op de waarde die u nodig hebt of stel deze een parameterwaarde accepteren. U kunt gebruikers van uw sjabloon om door te geven in een waarde voor het domein naamlabel hebt ingeschakeld.

Nu eens kijken hoe u de **apiVersion**vaststellen naar. De waarde die u opgeeft gewoon komt overeen met de versie van de REST API die u gebruiken wilt bij het maken van de resource. Zo is, kunt u de REST API-documentatie voor dat resourcetype bekijken. Of u het volgende PowerShell-opdracht voor een bepaald type kunt uitvoeren.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Welke geeft als resultaat de volgende waarden:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Als u wilt de API-versies met Azure CLI wordt weergegeven, voert u de dezelfde opdracht **azure provider weergeven** eerder weergegeven.

Wanneer u een nieuwe sjabloon maakt, kiest u de meest recente versie van de API.

## <a name="virtual-network-and-subnet"></a>Virtuele netwerk- en subnet
Maak een virtueel netwerk met één subnet. Bekijk de [REST API voor virtuele netwerken](https://msdn.microsoft.com/library/azure/mt163661.aspx) voor alle eigenschappen om in te stellen.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## <a name="load-balancer"></a>De belasting voor documentconversies
Nu u een externe omlaag taakverdeling maakt. Omdat deze taakverdeling het openbare IP-adres gebruikt, moet u een afhankelijkheid in het openbare IP-adres in de sectie **dependsOn** declareren. Dit betekent dat de taakverdeling wordt niet geïmplementeerd totdat het openbare IP-adres is voltooid implementeren. Zonder deze afhankelijkheid definiëren, ontvangt u een fout omdat resourcemanager probeert te implementeren van de bronnen parallel en probeert om in te stellen van de taakverdeling naar openbare IP-adres dat nog niet bestaat. 

U ook maakt een resourcegroep die backend-mailadres, een aantal regels voor binnenkomende NAT naar RDP in de VMs en een laden taakverdeling regel met een onderzoeken tcp poort 80 in de definitie van deze resource. Afhandeling de [REST API voor de verdeling van belasting](https://msdn.microsoft.com/library/azure/mt163574.aspx) voor alle eigenschappen.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## <a name="network-interface"></a>Netwerkinterface
U maakt 2 netwerkinterfaces, één voor elke VM. In plaats van dubbele items voor de netwerkinterfaces opnemen, kunt u de [functie copyIndex()](resource-group-create-multiple.md) doorlopen in de kopie lus (genoemd nicLoop) en het maken van het getal netwerkinterfaces die zijn gedefinieerd de `numberOfInstances` variabelen. De netwerkinterface, is afhankelijk van het maken van het virtuele netwerk en de taakverdeling. Het subnet gedefinieerd in het virtuele netwerk maken en de belasting voor gelijkmatige verdeling-id wordt gebruikt voor het configureren van de groep laden de verdeling van adressen en de binnenkomende NAT-regels.
Bekijk de [REST API voor netwerkinterfaces](https://msdn.microsoft.com/library/azure/mt163668.aspx) voor alle eigenschappen.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## <a name="virtual-machine"></a>VM
U maakt 2 virtuele machines met copyIndex(), functie, zoals bij het maken van het [Netwerkinterfaces](#network-interface).
De aanmaakdatum VM is afhankelijk van het account opslag netwerk interface en hetzelfde beschikbaarheid instellen. Deze VM wordt gemaakt van de afbeelding van een marketplace, zoals gedefinieerd de `storageProfile` , eigenschap - `imageReference` wordt gebruikt om de afbeelding van publisher, aanbieding, sku en versie te definiëren. Ten slotte is een diagnostische profiel geconfigureerd voor het inschakelen van diagnostische hulpprogramma's voor de VM. 

Als u zoekt de relevante eigenschappen voor een afbeelding marketplace, volgt u de artikelen [Linux VM afbeeldingen selecteren](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) of [Selecteer Windows VM afbeeldingen](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) .

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] Voor afbeeldingen die zijn gepubliceerd door **3e leveranciers**, moet u nog een eigenschap met de naam opgeven `plan`. Een voorbeeld vindt u in [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) uit de galerie quickstart. 

U klaar bent met het definiëren van de bronnen voor de sjabloon.

## <a name="parameters"></a>Parameters

Klik in de sectie parameters definiëren de waarden die kunnen worden opgegeven bij het distribueren van de sjabloon. Alleen parameters definiëren voor waarden die u denkt dat moet worden gewijzigd tijdens de implementatie. U kunt een standaardwaarde opgeven voor een parameter die wordt gebruikt als een tijdens de implementatie is opgegeven. U kunt ook de toegestane waarden definiëren, zoals wordt weergegeven voor de parameter **imageSKU** .

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## <a name="variables"></a>Variabelen

U kunt waarden die worden gebruikt op meer dan één plaats in uw sjabloon en waarden die zijn opgebouwd uit andere expressies of variabelen definiëren in de sectie variabelen. Variabelen worden vaak gebruikt om te vereenvoudigen de syntaxis van de sjabloon.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

U kunt de sjabloon hebt voltooid! U kunt uw sjabloon ten opzichte van de volledige sjabloon in de [galerie met quickstart](https://github.com/Azure/azure-quickstart-templates) onder [2 VMs met de belasting voor documentconversies en de verdeling van regels sjabloon laden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules)vergelijken. De sjabloon wijkt mogelijk iets af op basis van het gebruik van de andere versie getallen. 

U kunt de sjabloon opnieuw implementeren met behulp van de dezelfde opdrachten die u hebt gebruikt bij het distribueren van de opslag-account. U hoeft niet het account opslagruimte verwijderen voordat opnieuw wordt geïmplementeerd omdat resourcemanager wordt overslaan opnieuw maken resources die al bestaat en niet zijn gewijzigd.

## <a name="next-steps"></a>Volgende stappen

- [Azure resourcemanager sjabloon Visualizer (ARMViz)](http://armviz.io/#/) is een prima hulpmiddel om te visualiseren op ARM sjablonen, zodra deze mogelijk te groot is om te begrijpen alleen bij het lezen van de json-bestand.
- Meer informatie over de structuur van een sjabloon, raadpleegt u [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md).
- Zie voor meer informatie over het implementeren van een sjabloon, [Deploy resourcegroep met Azure resourcemanager-sjabloon](resource-group-template-deploy.md)
