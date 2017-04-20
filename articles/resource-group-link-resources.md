<properties 
    pageTitle="Resources in Azure resourcemanager koppelen | Microsoft Azure" 
    description="Maak een koppeling tussen verwante resources in verschillende resourcegroepen in Azure resourcemanager." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/01/2016" 
    ms.author="tomfitz"/>

# <a name="linking-resources-in-azure-resource-manager"></a>Resources in Azure resourcemanager koppelen

Tijdens de implementatie, kunt u een resource als afhankelijk is van een andere resource markeren, maar die levenscyclus eindigt op implementatie. Na implementatie, moet u er geen aangegeven relatie is tussen afhankelijke bronnen. Resourcemanager bevat een functie die resource koppelen, zodat de permanente relaties definiëren tussen resources genoemd.

Met de resource koppelen, kunt u relaties die resourcegroepen's beslaan van het document. Dat is bijvoorbeeld gebruikelijk als u wilt dat een database maken met een eigen levenscyclus zich bevinden in één resourcegroep en een app waarin de levenscyclus van een andere bevinden zich in een andere resource-groep. De app maakt verbinding met de database, zodat u wilt markeren, een koppeling tussen de app en de database. 

Alle gekoppelde resources moeten behoren tot hetzelfde abonnement. Elke resource kan worden gekoppeld aan 50 andere bronnen. De enige manier om verwante resources query's mogelijk is via de REST API. Als een van de gekoppelde bronnen zijn verwijderd of verplaatst, moet de eigenaar van de koppeling de resterende koppeling opschonen. U bent **niet** gewaarschuwd wanneer een resource die is gekoppeld aan andere bronnen verwijderen.

## <a name="linking-in-templates"></a>Koppelen in sjablonen

U opnemen om te definiëren een koppeling in een sjabloon, een resourcetype waarin de naamruimte van de provider resource zijn gecombineerd en typt u van de resource bron met **/providers/links**. De naam moet de naam van de resource bron bevatten. U opgeeft de resource-id van de resource target. Het volgende voorbeeld wordt een koppeling tussen een website en een opslag-account.

    {
      "type": "Microsoft.Web/sites/providers/links",
      "apiVersion": "2015-01-01",
      "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
      "dependsOn": [ "[variables('siteName')]" ],
      "properties": {
        "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
        "notes": "This web site uses the storage account to store user information."
      }
    }


Zie [Resource koppelingen - sjabloon schema](resource-manager-template-links.md)voor een volledige beschrijving van de sjabloonindeling.

## <a name="linking-with-rest-api"></a>Koppelen met REST API

Als u wilt een koppeling tussen geïmplementeerd resources definiëren, voert u het:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

{Abonnements-id} vervangen door uw abonnements-id. Vervang {-resourcegroep}, {provider-naamruimte, {-brontype} en {bronnaam} met de waarden die de eerste resource in de koppeling identificeren. {Link-naam} vervangen door de naam van de koppeling wilt maken. Gebruik 2015-01-01 voor de api-versie.

In de aanvraag bevat een object dat de tweede bron in de koppeling wordt gedefinieerd:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

Het element eigenschappen bevat de id voor de tweede bron.

U kunt koppelingen zoeken in uw abonnement:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Zie voor meer voorbeelden van het ophalen van informatie over koppelingen, inclusief [Gekoppelde bronnen](https://msdn.microsoft.com/library/azure/mt238499.aspx).

## <a name="next-steps"></a>Volgende stappen

- U kunt ook uw resources met tags indelen. Meer informatie over sociaalnetwerklabels resources, raadpleegt u [werken met tags om u te organiseren van uw resources](resource-group-using-tags.md).
- Zie voor een beschrijving van het maken van sjablonen en de resources worden geïmplementeerd definiëren, [ontwerpfuncties sjablonen](resource-group-authoring-templates.md).
