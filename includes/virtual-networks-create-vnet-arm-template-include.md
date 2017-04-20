## <a name="download-and-understand-the-arm-template"></a>Downloaden en meer informatie over de ARM-sjabloon

U kunt de bestaande ARM-sjabloon voor het maken van een VNet en twee subnetten uit github downloaden, breng de wijzigingen u mogelijk wilt en deze opnieuw gebruiken. Klik hiervoor de onderstaande stappen uit te voeren.

1. Navigeer naar [de voorbeeldpagina voor de sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Klik op **azuredeploy.json**en klik vervolgens op **ONBEWERKTE**.
3. Sla het bestand naar een een lokale map op uw computer.
4. Als u bekend met ARM sjablonen bent, gaat u verder met stap 7.
5. Open het bestand dat u zojuist hebt opgeslagen en kijkt u naar de inhoud onder **parameters** in regel 5. Op ARM Sjabloonparameters bieden een tijdelijke aanduiding voor waarden die kunnen worden ingevuld tijdens de implementatie.

    | Parameter | Beschrijving |
    |---|---|
    | **locatie** | Azure regio waar de VNet worden gemaakt |
    | **vnetName** | Naam voor de nieuwe VNet |
    | **addressPrefix** | Adresruimte voor de VNet, in CIDR indeling |
    | **subnet1Name** | Naam voor de eerste VNet |
    | **subnet1Prefix** | Blokkering van CIDR voor het eerste subnet |
    | **subnet2Name** | Naam voor de tweede VNet |
    | **subnet2Prefix** | Blokkering van CIDR voor het tweede subnet |

    >[AZURE.IMPORTANT] Op ARM sjablonen onderhouden in github kunnen na verloop van tijd wijzigen. Zorg ervoor dat u de sjabloon controleren voordat u deze gebruikt.
    
6. Controleer de inhoud onder **resources** en u ziet het volgende:

    - **type**. Type resource wordt gemaakt door de sjabloon. In dit geval **Microsoft.Network/virtualNetworks**, waarin vertegenwoordigen een VNet.
    - **de naam**. De naam voor de resource. Zoals u ziet het gebruik van **[parameters('vnetName')]**, wat betekent dat de naam wordt weergegeven als invoer door de gebruiker of een parameterbestand tijdens de implementatie.
    - **Eigenschappen**. Lijst met eigenschappen voor de resource. Deze sjabloon worden de eigenschappen van het adres ruimte en subnet tijdens het maken van VNet.

7. Ga terug naar [de voorbeeldpagina voor de sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. **Azuredeploy-paremeters.json**op en klik vervolgens op **ONBEWERKTE**.
9. Sla het bestand naar een een lokale map op uw computer.
10. Open het bestand dat u zojuist hebt opgeslagen en bewerk de waarden voor de parameters. Gebruik de onderstaande waarden om te implementeren van de VNet die worden beschreven in ons scenario.

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. Sla het bestand.
  