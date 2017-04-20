## <a name="virtual-network"></a>Virtual Network
Virtuele informatiebronnen netwerken (VNET) en subnetten definiëren van de rand van een waardepapier voor werkbelasting uitgevoerd in Azure wordt aangegeven. Een VNet wordt gekenmerkt door een verzameling adres spaties, gedefinieerd als CIDR blokken. 

>[AZURE.NOTE] Netwerkbeheerders bent bekend met CIDR-notatie. Als u niet bekend met CIDR, [meer informatie over deze bent](http://whatismyipaddress.com/cidr).

![VNet met meerdere subnetten](./media/resource-groups-networking/Figure4.png)

VNets bevatten de volgende eigenschappen.

|Eigenschap|Beschrijving|Voorbeeldwaarden|
|---|---|---|
|**addressSpace**|Verzameling adresvoorvoegsels waaruit de VNet in CIDR notatie|192.168.0.0/16|
|**subnetten**|Verzameling subnetten waaruit de VNet|Zie de onderstaande [subnetten](#Subnets) .|
|**IP-adres**|IP-adres is toegewezen aan een object. Dit is een alleen-lezen-eigenschap.|104.42.233.77|

### <a name="subnets"></a>Subnetten
Een subnet is een resource onderliggende van een VNet en helpt bij het definiëren segmenten van adres spaties binnen een tekstblok CIDR, met IP-adres voorvoegsels voor eenheden. NIC's kunnen worden toegevoegd aan subnetten en verbonden met VMs, een connectiviteit voor verschillende werkbelasting.

Subnetten bevatten de volgende eigenschappen. 

|Eigenschap|Beschrijving|Voorbeeldwaarden|
|---|---|---|
|**addressPrefix**|Één adresvoorvoegsel dat het subnet in CIDR notatie|192.168.1.0/24|
|**networkSecurityGroup**|NSG toegepast op het subnet|Zie [NSGs](#Network-Security-Group)|
|**routeTable**|Routetabel die zijn toegepast op het subnet|Zie [UDR](#Route-table)|
|**ipConfigurations**|Verzameling IP-configruation objecten die worden gebruikt door NIC's die zijn verbonden met het subnet|Zie [UDR](#Route-table)|


Voorbeeld VNet in de indeling van JSON:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Aanvullende informatie

- Meer informatie over [VNet](../articles/virtual-network/virtual-networks-overview.md)krijgen.
- Lees de [REST API-documentatie](https://msdn.microsoft.com/library/azure/mt163650.aspx) voor VNets.
- Lees de [REST API-documentatie](https://msdn.microsoft.com/library/azure/mt163618.aspx) voor subnetten.