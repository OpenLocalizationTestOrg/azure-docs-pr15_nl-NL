## <a name="traffic-manager-profile"></a>Verkeer Manager profiel

Verkeer manager en bijbehorende onderliggende eindpunt resource kunt de DNS-zoekresultaten omleiden naar een eindpunten in Azure wordt aangegeven en buiten Azure. De verdeling van dergelijke verkeer valt onder methoden voor het doorsturen beleid. Verkeer-beheer kunt ook eindpunt systeemstatus kunnen worden bewaakt en verkeer correct wordt gewijzigd op basis van de status van een eindpunt. 

| Eigenschap | Beschrijving |
|---|---|
|**trafficRoutingMethod**| mogelijke waarden zijn *prestaties*, *gewogen*en *prioriteit* | 
| **dnsConfig** | FQDN-naam voor het profiel | 
| **Protocol** | cmdlets voor controle protocol, zijn mogelijke waarden *HTTP* en *HTTPS*|
| **Poort** | poort voor controle |  
| **Pad** | cmdlets voor controle pad |
| **Eindpunten** |  container voor eindpunt resources | 

### <a name="endpoint"></a>Eindpunt 

Een eindpunt is een onderliggende bron van een profiel verkeer Manager. Staat dit voor een service of web-eindpunt aan welke gebruiker verkeer is verdeeld op basis van de geconfigureerde beleid in de bron verkeer Manager profiel. 

| Eigenschap | Beschrijving | 
|---|---| 
| **Type** |  het type van het eindpunt, mogelijke waarden zijn, *wijst u Azure einde*, *Externe eindpunt*en *Genest eindpunt* | 
| **targetResourceId** |  openbare IP-adres van een service of web-eindpunt. Dit is een Azure of externe eindpunt. | 
| **Gewicht** | eindpunt gewicht gebruikt bij verkeer beheren. | 
| **Prioriteit** | prioriteit van het eindpunt, waarmee een actie failover definiëren |

Voorbeeld van verkeer Manager in de indeling van Json: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>Aanvullende informatie

Lees [REST API-documentatie voor verkeer Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) voor meer informatie.
