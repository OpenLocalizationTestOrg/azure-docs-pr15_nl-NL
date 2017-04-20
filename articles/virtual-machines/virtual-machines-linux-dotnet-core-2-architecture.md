<properties
   pageTitle="Resources met Azure resourcemanager sjablonen implementeert berekenen | Microsoft Azure"
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

# <a name="application-architecture-with-azure-resource-manager-templates"></a>Toepassingsarchitectuur met Azure resourcemanager sjablonen

Wanneer u een implementatie van Azure resourcemanager ontwikkelt, berekenen vereisten moeten worden toegewezen aan Azure resources en -services. Als een toepassing uit verschillende HTTP-eindpunten, een database en een gegevens in de cache van de service bestaat, de Azure bronnen die host elk van deze onderdelen moet worden gerationaliseerd. De steekproef muziek Store-servicetoepassing bevat bijvoorbeeld een webtoepassing die wordt gehost op een virtuele machine en een SQL-database, die wordt gehost in SQL Azure-database. 

In dit document een gedetailleerd overzicht van hoe de bronnen voor het berekenen van muziek Store zijn geconfigureerd in de steekproef resourcemanager Azure-sjabloon. Alle afhankelijkheden en unieke configuraties zijn gemarkeerd. Voor een optimale ervaring, vooraf een exemplaar van de oplossing aan uw Azure-abonnement en werk samen met de sjabloon Azure resourcemanager implementeren. De volledige sjabloon vindt u hier: [Muziek Store-implementatie op Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="virtual-machine"></a>VM

De muziek Store-servicetoepassing bevat een webtoepassing waar klanten kunnen bladeren en muziek te kopen. Er zijn verschillende Azure services dat de host van webtoepassingen, in dit voorbeeld wordt een virtuele Machine wordt gebruikt. Met de muziek Store voorbeeldsjabloon, een virtuele machine wordt geïmplementeerd, een webserver installeren en de website van muziek Store geïnstalleerd en geconfigureerd. Voor dit artikel is alleen de implementatie van de virtuele machines gedetailleerde. De configuratie van de webserver en de toepassing wordt beschreven in een latere artikel.

Een virtuele machine kunnen worden toegevoegd aan een sjabloon met de wizard Visual Studio Add New Resource of door een geldig JSON invoegen in de sjabloon-implementatie. Wanneer u een virtuele machine implementeert, zijn er ook meerdere verwante resources nodig. Als u Visual Studio voor het maken van de sjabloon, worden deze resources worden gemaakt voor u. Als het bouwen van de sjabloon handmatig, wordt deze resources moeten worden ingevoegd en geconfigureerd.

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [VM JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
      ........<truncated>  
    }
```

Zodra geïmplementeerd, kunnen de eigenschappen van het virtuele machine worden bekeken in de portal van Azure.

![VM](./media/virtual-machines-linux-dotnet-core/vm.png)

## <a name="storage-account"></a>Opslag-Account

Opslag-accounts zijn veel opslagopties en mogelijkheden. Voor de context van Azure virtuele machines bevat een account opslag in de virtuele vaste schijven van de virtuele machine en eventuele aanvullende gegevensschijven. De steekproef muziek Store bevat één opslag account waarin de virtuele harde schijf van elke virtuele machine in de implementatie. 

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [Opslag-Account](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Hiervoor is een opslag-account koppelen aan een virtuele machine binnen de sjabloondeclaratie resourcemanager van de virtuele machine. 

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [virtuele Machine en opslag Account koppeling](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).

```none
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Na implementatie, kan de opslag-account in de portal van Azure worden bekeken.

![Opslag-Account](./media/virtual-machines-linux-dotnet-core/storacct.png)

Klikken in de container opslag account blob, kan het bestand virtuele harde schijf voor elke virtuele machine geïmplementeerd met de sjabloon worden bekeken.

![Virtuele vaste schijven](./media/virtual-machines-linux-dotnet-core/vhd.png)

Zie [Azure Storage documentatie](https://azure.microsoft.com/documentation/services/storage/)voor meer informatie over Azure opslag.

## <a name="virtual-network"></a>Virtual Network

Als een virtuele machine interne netwerken, zoals de mogelijkheid om te communiceren met andere virtuele machines en Azure resources vereist, is een Azure Virtual Network vereist.  Een virtueel netwerk maakt geen de virtuele machine toegankelijk via internet. Openbare connectiviteit vereist een openbare IP-adres, dat is gedetailleerde verderop in deze reeks.

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [Virtual Network en subnetten](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "10.0.0.0/24",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
          }
        }
      }
    ]
  }
}
```

In de portal Azure eruitziet het virtuele netwerk van de volgende afbeelding. Zoals u ziet dat alle virtuele machines geïmplementeerd met de sjabloon zijn gekoppeld aan het virtuele netwerk.

![Virtual Network](./media/virtual-machines-linux-dotnet-core/vnet.png)

## <a name="network-interface"></a>Netwerkinterface

 Een netwerkinterface verbinding wordt gemaakt van een virtuele machine met een virtueel netwerk, meer specifiek aan een subnet die is gedefinieerd in het virtuele netwerk. 
 
 Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [Netwerkinterface](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166).
 
```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceNamePrefix'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'SSH-VM', copyIndex())]"
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
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/SSH-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Elke resource VM bevat een netwerkprofiel. De netwerkinterface is gekoppeld aan de virtuele machine in dit profiel.  

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [VM netwerkprofiel](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350).


```none
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceNamePrefix'), copyindex()))]"
    }
  ]
}
```

In de portal Azure eruitziet de netwerkinterface van de volgende afbeelding. Het interne IP-adres en de koppeling VM kunnen worden weergegeven op het netwerk interface resource.

![Netwerkinterface](./media/virtual-machines-linux-dotnet-core/nic.png)

Zie voor meer informatie over Azure virtuele netwerken, [Azure Virtual Network documentatie](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Azure SQL-Database

Naast een virtuele machine de muziek Store-website hosten, is een Azure SQL-Database als de database muziek Store wilt hosten geïmplementeerd. Het voordeel van het gebruik van Azure SQL-Database hier is dat een tweede reeks virtuele machines niet vereist is en schaal en beschikbaarheid zijn ingebouwd in de service.

Een Azure SQL-database kan worden toegevoegd met de Visual Studio Add New Resource wizard, of door een geldig JSON invoegen in een sjabloon. De SQL Server-resource bevat een gebruikersnaam in te voeren en het wachtwoord waarmee beheerdersrechten op de SQL-exemplaar wordt verleend. Ook wordt een SQL-firewall resource toegevoegd. Standaard zijn de toepassingen die zijn ingesloten in een Azure verbinding maken met de SQL-exemplaar. Als u wilt toestaan van externe toepassing moet zoals een SQL Server Management studio verbinding maken met de SQL-exemplaar, de firewall worden geconfigureerd. Om de video muziek Store is de standaardconfiguratie in orde. 

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [Azure SQL-DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401).


```none
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicStoreSqlName')]",
  "location": "[resourceGroup().location]",

  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('sqlAdminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicStoreSqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Een weergave van de SQL server en MusicStore database zoals gezien in de portal van Azure.

![SQL Server](./media/virtual-machines-linux-dotnet-core/sql.png)

Zie [Azure SQL Database-documentatie](https://azure.microsoft.com/documentation/services/sql-database/)voor meer informatie over het implementeren van Azure SQL-Database.

## <a name="next-step"></a>Volgende stap

<hr>

[Stap 2: toegang en beveiliging in Azure resourcemanager-sjablonen](./virtual-machines-linux-dotnet-core-3-access-security.md)
