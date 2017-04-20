<properties
   pageTitle="Resourcemanager sjabloon voor de resource vergrendelingen | Microsoft Azure"
   description="Ziet u het schema resourcemanager voor het implementeren van resource vergrendelingen tot en met een sjabloon."
   services="azure-resource-manager"
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="resource-locks-template-schema"></a>Resource vergrendelingen sjabloon schema

Hiermee maakt u een slot aan een resource en de onderliggende resources.

## <a name="schema-format"></a>Schema-indeling

Als wilt maken van een slot, voegt u het volgende schema naar de sectie bronnen van de sjabloon.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Waarden

De volgende tabellen worden de waarden die u nodig hebt om in te stellen in het schema beschreven.

| Naam | Vereist | Beschrijving |
| ---- | -------- | ----------- |
| type | Ja | Het resourcetype om te maken.<br /><br />Voor resources:<br />**{naamruimte} / {Typ} / providers/vergrendelingen**<br /><br/>Voor resourcegroepen:<br />**Microsoft.Authorization/locks** |
| apiVersion | Ja | De API-versie te gebruiken voor het maken van de resource.<br /><br />Gebruik:<br />**01-01-2015**<br /><br /> |
| naam | Ja | Een waarde die de resource vergrendelen en een naam voor de vergrendelen. Kan maximaal 64 tekens, en niet kunt bevatten <>, %, &,?, of een stuurcodes.<br /><br />Voor resources:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Voor resourcegroepen:<br />**{lockname}** |
| dependsOn | Nee | Een lijst door komma's gescheiden met een resource namen of unieke id's voor een resource.<br /><br />De verzameling resources die zijn afhankelijk van de dit vergrendelen. Als de bron die u zijn vergrendelen wordt geïmplementeerd in dezelfde sjabloon, moet u de naam van deze resource opnemen in dit element kunt u zorgen dat de resource eerst wordt geïmplementeerd. | 
| Eigenschappen | Ja | Een object waarmee het type van vergrendelen en notities over de vergrendelen.<br /><br />Zie [Eigenschappen object](#properties-object). |  

### <a name="properties-object"></a>Eigenschappen van object

| Naam | Vereist | Beschrijving |
| ---- | -------- | ----------- |
| niveau   | Ja | Het type vergrendelen toe te passen op het bereik.<br /><br />**CannotDelete** - gebruikers kunnen wijzigen van de resource, maar deze niet verwijderen.<br />**Alleen-lezen** - gebruikers kunnen lezen van een resource, maar ze niet kunnen verwijderen of bewerkingen uitvoeren op is geïnstalleerd. |
| notities   | Nee | Beschrijving van de vergrendeling. Mag maximaal 512 tekens lang zijn. |


## <a name="how-to-use-the-lock-resource"></a>Het gebruik van de resource vergrendelen

U kunt deze resource toevoegen aan uw sjabloon om te voorkomen dat bepaalde acties op een resource. Het hangslot is van toepassing op alle gebruikers en groepen.

Als u wilt maken of verwijderen van management vergrendelingen, moet u toegang tot **Microsoft.Authorization/** * of * *Microsoft.Authorization/locks/* ** Acties. Van de ingebouwde functies, alleen **eigenaar** en **gebruiker toegang beheerder ** daarvoor deze acties. Zie voor informatie over Rolgebaseerd toegangsbeheer [Azure Rolgebaseerd toegangsbeheer](./active-directory/role-based-access-control-configure.md).

De vergrendeling wordt toegepast op de opgegeven resource en alle onderliggende bronnen.

U kunt een vergrendelen met de PowerShell-opdracht **Verwijderen-AzureRmResourceLock** of met de [verwijderbewerking](https://msdn.microsoft.com/library/azure/mt204562.aspx) van de REST API verwijderen.

## <a name="examples"></a>Voorbeelden

Het volgende voorbeeld wordt een slot niet kunt verwijderen naar een web-app.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

Een slot niet kunt verwijderen geldt in het volgende voorbeeld voor de resourcegroep.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over de sjabloonstructuur van de, [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md).
- Zie voor meer informatie over vergrendelingen, [vergrendelen resources met Azure resourcemanager](resource-group-lock-resources.md).
