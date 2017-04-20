<properties
   pageTitle="Resourcemanager sjabloon voor opslag | Microsoft Azure"
   description="Ziet u het schema resourcemanager voor het implementeren van opslag accounts tot en met een sjabloon."
   services="azure-resource-manager,storage"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="storage-account-template-schema"></a>Schema voor opslag-account

Hiermee maakt u een opslag-account.

## <a name="schema-format"></a>Schema-indeling

Als wilt een opslag-account maken, voegt u het volgende schema naar de sectie bronnen van de sjabloon.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Waarden

De volgende tabellen worden de waarden die u nodig hebt om in te stellen in het schema beschreven.

| Naam | Waarde |
| ---- | ---- |
| type | Opsommen<br />Vereist<br />**Microsoft.Storage/storageAccounts**<br /><br />Het resourcetype om te maken. |
| apiVersion | Opsommen<br />Vereist<br />**2015-06-15** of **2015-05-01-preview**<br /><br />De API-versie te gebruiken voor het maken van de resource. | 
| naam | Tekenreeks<br />Vereist<br />Tussen 3 en 24 tekens, alleen getallen en kleine letters.<br /><br />De naam van het account opslag maken. De naam moet uniek zijn voor alle Azure. Overweeg het gebruik van de functie [uniqueString](resource-group-template-functions.md#uniquestring) met uw naamgevingsconventie, zoals wordt weergegeven in het onderstaande voorbeeld. |
| locatie | Tekenreeks<br />Vereist<br />Een gebied die ondersteuning biedt voor opslag-accounts. Om te bepalen geldige regio's, Zie [regio's die worden ondersteund](resource-manager-supported-services.md#supported-regions).<br /><br />Het gebied voor het hosten van het account opslag. |
| Eigenschappen | Object<br />Vereist<br />[Eigenschappen van object](#properties)<br /><br />Een object dat aangeeft van het type account dat u opslag wilt maken. |

<a id="properties" />
### <a name="properties-object"></a>Eigenschappen van object

| Naam | Waarde |
| ---- | ---- | 
| accountType | Tekenreeks<br />Vereist<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**of **Premium_LRS**<br /><br />Het type account opslag. De toegestane waarden overeenkomen met de standaard lokaal overtollige, standaard Zone overtollige standaard geografische-redundante, standaard leestoegang geografische-redundante en Premium lokaal overtollige. Zie voor informatie over het soort account, [Azure Storage herhaling](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Voorbeelden

Het volgende voorbeeld implementeert een standaard lokaal overtollige opslag-account met een unieke naam op basis van de resource groeps-id.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>QuickStart sjablonen

Zijn er veel quickstart sjablonen die een account opslagruimte bevatten. De volgende sjablonen illustreren enkele veelvoorkomende scenario's:

- [Een standaard-opslag-Account maken](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Eenvoudige implementatie van een Windows-VM](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Eenvoudige implementatie van een Linux-VM](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [Een profiel CDN, een CDN-eindpunt met een Account opslag als origin maken](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [Maak een hoge Availabilty SharePoint-Farm met 9 VMs via de Powershell DSC-extensie](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [Eenvoudige implementatie van 5 knooppunt secure Service stof Cluster met af die zijn ingeschakeld](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [Een virtuele Machine maken op basis van de afbeelding van een Windows met 4 lege gegevensschijven](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Volgende stappen

- Zie [Inleiding tot Microsoft Azure Storage](./storage/storage-introduction.md)voor algemene informatie over opslag.
- Sjablonen die een nieuw account voor de opslag gebruiken met een virtuele Machine, Zie bijvoorbeeld [Deploy eenvoudige Linux VM](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) of [een eenvoudige Windows VM Deploy](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).
