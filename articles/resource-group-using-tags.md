<properties
    pageTitle="Markeringen gebruiken om te organiseren van uw Azure resources | Microsoft Azure"
    description="Ziet u het toepassen van tags wilt indelen van bronnen voor de facturering en beheren."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="tomfitz"/>


# <a name="using-tags-to-organize-your-azure-resources"></a>Markeringen gebruiken om te organiseren van uw Azure resources

Resourcemanager kunt u resources logisch indelen door toe te passen tags. De codes bestaan uit de sleutel/waardeparen die resources identificeren met eigenschappen die u definieert. Als u wilt markeren resources behoren tot dezelfde categorie, kunt u dezelfde tag toepassen door deze resources.

Wanneer u resources met een bepaald label bekijkt, ziet u resources uit uw resourcegroepen. U bent niet beperkt tot alleen de resources in dezelfde resourcegroep, waarmee u uw resources in een manier die onafhankelijk is van de implementatie-relaties organiseren. Labels is handig als u wilt organiseren van resources voor facturering of management.

Elk naamkaartje die u aan een resource of resourcegroep toevoegen wordt automatisch toegevoegd aan de taxonomie abonnement hele. U kunt ook de taxonomie als vooraf voor uw abonnement op tagnamen en waarden die u gebruiken wilt als resources in de toekomst zijn gelabeld invullen.

Elke resource of resourcegroep mag maximaal 15 tags bevatten. De naam van de code is beperkt tot 512 tekens en de tagwaarde is beperkt tot 256 tekens.

> [AZURE.NOTE] U kunt alleen tags toepassen op resources die ondersteuning bieden voor resourcemanager bewerkingen. Als u een virtuele Machine, Virtual Network of opslag via het implementatiemodel klassieke gemaakt (zoals via de portal klassieke), u niet een tag toepassen op de desbetreffende resource. Om te ondersteunen labelen, implementeer deze opnieuw deze resources tot en met bronbeheer. Alle andere bronnen ondersteuning voor labelen.

## <a name="templates"></a>Sjablonen

Als u wilt een melding geven voor een resource tijdens de implementatie, gewoon het element **labels** toevoegen aan de resource die u implementeert en geef de tagnaam en waarde. De tagnaam en waarde hoeft niet te vooraf bestaat niet in uw abonnement. U kunt maximaal 15 tags opgeven voor elke resource.

Het volgende voorbeeld ziet u een account opslagruimte met een tag.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Resourcemanager ondersteunt momenteel geen verwerking van een object voor de tagnamen en waarden. Een object voor de tag-waarden in plaats daarvan doorgeeft, maar moet nog steeds u de namen van de tag, zoals wordt weergegeven in het volgende voorbeeld.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## <a name="portal"></a>Portal

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>PowerShell

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>REST API

De portal en PowerShell gebruiken allebei de [Resourcemanager REST API](https://msdn.microsoft.com/library/azure/dn848368.aspx) achter de schermen. Als u integreren in een andere omgeving labelen wilt, kunt u labels krijgen met een ophalen van de resource-id en de set labels bijwerken met een PATCH-oproep.


## <a name="tags-and-billing"></a>Labels en facturering

Voor ondersteunde services, kunt u markeringen gebruiken om uw factuur gegevens te groeperen. Bijvoorbeeld, kunt u problemen definiëren en tags wilt organiseren van het gebruik van de facturering voor virtuele machines toepassen [virtuele Machines geïntegreerd met Azure Resource Manager](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) . Als u meerdere VMs voor andere organisaties worden uitgevoerd, kunt u de labels aan het gebruik van de groep door kostenplaats.  
U kunt ook codes gebruiken voor het categoriseren van kosten door runtimeomgeving. zoals de facturering gebruik voor VMs uitgevoerd in productieomgeving.

Hier vindt u informatie over tags tot en met de [Azure Resourcegebruik en RateCard API's](billing-usage-rate-card-overview.md) of het bestand waarvoor gebruik door komma's gescheiden waarden (CSV). U downloaden het bestand gebruik van de [portal van Azure-accounts](https://account.windowsazure.com/) of [EA-portal](https://ea.azure.com). Zie [inzicht in uw verbruik van Microsoft Azure krijgen](billing-usage-rate-card-overview.md)voor meer informatie over toegang tot het factureringsgegevens. Zie [Azure facturering REST API-naslaggids](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)voor REST API-bewerkingen.

Wanneer u het CSV-gebruik voor de services die ondersteuning bieden voor labels bij de facturering downloadt, is de tags worden weergegeven in de kolom **Tags** . Zie [informatie over uw factuur voor Microsoft Azure](billing/billing-understand-your-bill.md)voor meer informatie.

![Zie tags in facturering](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Volgende stappen

- U kunt beperkingen en conventies toepassen op uw abonnement met aangepaste machtigingen. Het beleid die u definieert kan vereisen dat alle resources een waarde voor een bepaald label hebben. Zie [Beleid voor het beheren van resources en toegang](resource-manager-policy.md)voor meer informatie.
- Zie voor een introductie over het gebruik van Azure PowerShell bij het implementeren van resources, [Azure PowerShell gebruiken met Azure Resource Manager](./powershell-azure-resource-manager.md).
- Zie [de CLI Azure voor Mac, Linux, en Windows met Azure Resource Management gebruiken](./xplat-cli-azure-resource-manager.md)voor een introductie over het gebruik van Azure CLI bij het implementeren van resources.
- Zie voor een inleiding over het gebruik van de portal [met behulp van de Azure portal als u wilt uw Azure resources beheren](./azure-portal/resource-group-portal.md)  
