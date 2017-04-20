<properties
   pageTitle="Resource-Provider overzicht | Microsoft Azure"
   description="Meer informatie over de nieuwe Resource netwerkprovider in Azure resourcemanager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="network-resource-provider"></a>Resource-netwerkprovider
Een basis-nodig in het succes van vandaag, is de mogelijkheid om te maken en beheren van grote schaal netwerk op toepassingen op een manier agile, flexibele, beveiligde en herhaald. Azure Resource Manager (ARM) kunt u toepassingen, zoals een één verzameling resources in resourcegroepen maken. Deze resources worden beheerd door verschillende resource providers onder ARM.

Azure resourcemanager, is afhankelijk van andere resource providers voor toegang tot uw resources. Er zijn drie belangrijkste resource providers: netwerk, opslag en berekeningscluster. In dit document wordt beschreven hoe de kenmerken en voordelen van de Resource netwerkprovider, waaronder:

- **Metagegevens** : u kunt gegevens toevoegen aan resources-labels gebruiken. Deze tags kunnen worden gebruikt voor het bijhouden van Resourcegebruik via resourcegroepen en abonnementen.
- **Betere controle van uw netwerk** - netwerk resources losjes zijn gekoppeld en u kunt deze op een uitgebreider manier bepalen. Dit betekent dat u hebt meer flexibiliteit bij het beheren van de netwerken resources.
- **Sneller configuratie** - omdat netwerk resources losjes zijn gekoppeld, u kunt maken en goedkeuringen netwerk resources parallel. Dit is aanzienlijk verminderd configuratietijd.
- **Rol gebaseerd toegangsbeheer** - RBAC biedt standaardrollen, met specifieke beveiligingsbereik, naast het maken van aangepaste rollen voor secure management.
- **Eenvoudiger beheer en implementatie** - is gemakkelijker te implementeren en beheren van toepassingen aangezien kunt kunt u een hele toepassing stapel als één set van resources in een resourcegroep. En sneller implementeert, aangezien u alleen het leveren van een sjabloon JSON nettolading kunt implementeren.
- **Snelle aanpassing** - kunt u declaratieve stijl sjablonen gebruiken om in te schakelen herhaald en snelle aanpassing van implementaties.
- **Herhaald aanpassing** - kunt u declaratieve stijl sjablonen gebruiken om in te schakelen herhaald en snelle aanpassing van implementaties.
- **Management interfaces** - kunt u een van de volgende interfaces voor het beheren van uw bronnen:
    - REST API gebaseerd
    - PowerShell
    - .NET SDK
    - Node.JS SDK
    - Java SDK
    - Azure CLI
    - Voorbeeld-Portal
    - Op ARM sjabloontaal

## <a name="network-resources"></a>Netwerk resources
U kunt nu netwerk resources belooft beheren in plaats van dat ze allemaal beheerd via een resource één berekening (een virtuele machine). Dit zorgt ervoor dat een hogere mate van flexibiliteit en flexibiliteit bij het schrijven van de infrastructuur van een complexe en grote schaal in een resourcegroep.

Een conceptuele weergave van een steekproef-implementatie waarbij een toepassing meerlagige wordt hieronder weergegeven. Elke resource die u, zoals NIC, openbare IP-adressen en VMs ziet, kan onafhankelijk worden beheerd.

![Model met netwerk](./media/resource-groups-networking/Figure2.png)

Elke resource bevat een gemeenschappelijke set met eigenschappen en hun afzonderlijke eigenschap. De algemene eigenschappen zijn:

|Eigenschap|Beschrijving|Voorbeeldwaarden|
|---|---|---|
|**naam**|Unieke resourcenaam. Elk resourcetype heeft een eigen naming beperkingen.|PIP01, VM01, NIC01|
|**locatie**|Azure regio waarin de resource zich bevindt|westus, eastus|
|**ID**|Unieke identificatie van de URI gebaseerd|/Subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

U kunt de afzonderlijke eigenschappen van resources in de onderstaande secties controleren.

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Management interfaces
U kunt uw Azure netwerken resources met verschillende interfaces beheren. In dit document gaan we in op kabel van die interfaces: REST API en sjablonen.

### <a name="rest-api"></a>REST API
Zoals eerder is vermeld, kunnen netwerk resources worden beheerd via tal van interfaces, inclusief REST API, .NET SDK, Node.JS SDK, Java SDK, PowerShell, CLI, Azure-Portal en sjablonen.

De Rest API voldoen aan de specificatie HTTP 1.1-protocol. De algemene URI-structuur van de API is hieronder:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

En de parameters accolades vertegenwoordigen de volgende elementen:

- **abonnement-id** - uw Azure abonnements-id.
- **resource-provider-naamruimte** - naamruimte voor de provider die wordt gebruikt. De waarde voor de resource netwerkprovider is *Microsoft.Network*.
- **regio-naam** - de regionaam van de Azure

De volgende methoden voor HTTP ondersteund bij het maken van oproepen naar de REST API:

- **Plaatsen** - gebruikt om te maken van een resource van een bepaald type, de eigenschap van een resource wijzigen of een koppeling tussen resources wijzigen.
- **KRIJG** - gebruikt voor het ophalen van gegevens voor een ingerichte resource.
- **Verwijderen** - gebruikt om te verwijderen van een bestaande bron.

Zowel de vergaderverzoeken en antwoorden voldoen aan een indeling van JSON nettolading. Zie [Azure Resource Management API's](https://msdn.microsoft.com/library/azure/dn948464.aspx)voor meer informatie.

### <a name="arm-template-language"></a>Op ARM sjabloontaal
Naast het beheren van resources imperatively (via API's of SDK), kunt u ook een declaratieve programming stijl maken en beheren van netwerk resources met behulp van de taal van de sjabloon ARM gebruiken.

De weergave van een voorbeeld van een sjabloon vindt u hieronder:

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

De sjabloon is hoofdzakelijk een JSON-beschrijving van de resources en de exemplaarwaarden is toegevoegd via parameters. In het onderstaande voorbeeld kan worden gebruikt om te maken van een virtueel netwerk met 2 subnetten.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Hebt u de optie aan de betreffende parameterwaarden handmatig te bieden bij gebruik van een sjabloon of kunt u een parameterbestand. Het volgende voorbeeld ziet u een mogelijke reeks parameterwaarden moet worden gebruikt met de bovenstaande sjabloon:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


De belangrijkste voordelen van het gebruik van sjablonen zijn:

- U kunt een complexe infrastructuur in een resourcegroep in een declaratieve stijl maken. De integratie van het maken van de bronnen, inclusief afhankelijkheid beheer wordt uitgevoerd door ARM.
- De infrastructuur kan worden gemaakt in een herhaald manier tussen de verschillende regio's en in een gebied gewoon door parameters te wijzigen.
- De stijl declaratieve leidt tot kortere overlappingstijd in de sjablonen bouwen en implementeren van de infrastructuur.

Zie [Azure quickstart sjablonen](https://github.com/Azure/azure-quickstart-templates)voor voorbeeldsjablonen.

Zie voor meer informatie over de taal van de sjabloon ARM, [Azure resourcemanager sjabloon taal](../resource-group-authoring-templates.md).

De bovenstaande voorbeeldsjabloon wordt gebruikt voor het virtuele netwerk en subnetresources. Er zijn andere netwerkbronnen die u gebruiken kunt, zoals hieronder wordt weergegeven:

### <a name="using-a-template"></a>Een sjabloon gebruiken

U kunt services implementeren naar Azure van een sjabloon via PowerShell, AzureCLI, of door een klik om toe te implementeren van GitHub te voeren. Uitvoeren als u wilt implementeren services van een sjabloon in GitHub, de volgende stappen uit:

1. Open het bestand template3 uit GitHub. Open bijvoorbeeld [virtuele netwerk met twee subnetten](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Klik op **Deploy naar Azure**, en klik vervolgens op aanmelden bij de Azure-portal met uw referenties.
3. Controleer of de sjabloon en klik vervolgens op **Opslaan**.
4. Klik op **bewerken van parameters** en selecteer een locatie, zoals *West Amerikaans*, voor de vnet en subnetten.
5. Zo nodig de parameters **ADDRESSPREFIX** en **SUBNETPREFIX** wijzigen en klik vervolgens op **OK**.
6. Klik op **Selecteer een resourcegroep** en klik vervolgens op in de resourcegroep die u wilt de vnet en subnetten aan toevoegen. U kunt ook een nieuwe resourcegroep maken door te klikken **of nieuwe maken**.
3. Klik op **maken**. Zoals u ziet de tegel voor het weergeven van **de implementatie van de inrichting van sjabloon**. Nadat de implementatie klaar is, ziet u een scherm dat vergelijkbaar is met onderstaande.

![Voorbeeld van sjabloonimplementatie](./media/resource-groups-networking/Figure6.png)


## <a name="next-steps"></a>Volgende stappen

[Azure resourcemanager sjabloon taal](../resource-group-authoring-templates.md)

[Azure netwerken – veelgebruikte sjablonen](https://github.com/Azure/azure-quickstart-templates)

[Resource-Provider berekenen](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

[Azure resourcemanager-overzicht](../azure-resource-manager/resource-group-overview.md)
