<properties
    pageTitle="Rolgebaseerd toegangsbeheer met de REST API beheren"
    description="Rolgebaseerd toegangsbeheer met de REST API beheren"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>Rolgebaseerd toegangsbeheer met de REST API beheren

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)

Rolgebaseerd Access besturingselement RBAC () in de Portal van Azure en Azure resourcemanager API kunt u toegang tot uw abonnement en resources op een fijnmazige niveau beheren. Met deze functie kunt u de toegang voor gebruikers, groepen of service principes Active Directory verlenen door sommige rollen toewijzen aan deze op een bepaald bereik.

## <a name="list-all-role-assignments"></a>Een lijst met alle roltoewijzingen

Een lijst met alle roltoewijzingen aan het opgegeven bereik en subscopes.

Aan de lijst roltoewijzingen, hebt u toegang tot `Microsoft.Authorization/roleAssignments/read` bewerking op het bereik. De ingebouwde rollen toegangsrechten voor deze bewerking. Zie [Toegangsbeheer voor Azure Role-Based](role-based-access-control-configure.md)voor meer informatie over roltoewijzingen en beheren van access voor Azure resources.

### <a name="request"></a>Aanvragen

Gebruik de methode **ophalen** met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

Controleer de waarden van de volgende om aan te passen van uw aanvraag kunt invullen binnen de URI:

1. *{Bereik}* vervangen door het bereik waarvan u de roltoewijzingen lijst wilt maken. De volgende voorbeelden wordt het opgeven van het bereik voor verschillende niveaus:

  - Abonnement: /subscriptions/ {abonnements-id}  
  - Resourcegroep: /subscriptions/ {abonnements-id} / resourceGroups/myresourcegroup1  
  - Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Api-versie}* vervangen door 2015-07-01.

3. *{Filter}* vervangen door de voorwaarde die u wilt toepassen om de lijst van de toewijzing rol te filteren:

  - Lijst roltoewijzingen voor alleen het opgegeven bereik geen inclusief de roltoewijzingen bij subscopes:`atScope()`    
  - Lijst roltoewijzingen voor een specifieke gebruiker, groep of toepassing:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - Roltoewijzingen voor een specifieke gebruiker, inclusief overgenomen van groepen in een lijst met |`assignedTo('{objectId of user}')`

### <a name="response"></a>Antwoord

Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Informatie over een roltoewijzing

Informatie over een enkel roltoewijzing opgegeven door de id van de toewijzing rol krijgt.

Als u informatie over een roltoewijzing, hebt u toegang tot `Microsoft.Authorization/roleAssignments/read` bewerking. De ingebouwde rollen toegangsrechten voor deze bewerking. Zie [Toegangsbeheer voor Azure Role-Based](role-based-access-control-configure.md)voor meer informatie over roltoewijzingen en beheren van access voor Azure resources.

### <a name="request"></a>Aanvragen

Gebruik de methode **ophalen** met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Controleer de waarden van de volgende om aan te passen van uw aanvraag kunt invullen binnen de URI:

1. *{Bereik}* vervangen door het bereik waarvan u de roltoewijzingen lijst wilt maken. De volgende voorbeelden wordt het opgeven van het bereik voor verschillende niveaus:

  - Abonnement: /subscriptions/ {abonnements-id}  
  - Resourcegroep: /subscriptions/ {abonnements-id} / resourceGroups/myresourcegroup1  
  - Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rol-toewijzing-id}* vervangen door de GUID-id van de toewijzing van rol.

3. *{Api-versie}* vervangen door 2015-07-01.

### <a name="response"></a>Antwoord

Statuscode: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Een roltoewijzing maken

De roltoewijzing van een bij het opgegeven bereik voor de opgegeven principal toekennen van de opgegeven rol maken.

Als u wilt een roltoewijzing maken, moet u toegang hebt `Microsoft.Authorization/roleAssignments/write` bewerking. Van de ingebouwde functies, zijn alleen de *eigenaar* en de *Gebruiker toegang beheerder* toegang tot deze bewerking. Zie [Toegangsbeheer voor Azure Role-Based](role-based-access-control-configure.md)voor meer informatie over roltoewijzingen en beheren van access voor Azure resources.

### <a name="request"></a>Aanvragen

Gebruik de methode **plaatsen** met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Controleer de waarden van de volgende om aan te passen van uw aanvraag kunt invullen binnen de URI:

1. *{Bereik}* vervangen door het bereik waarop u wilt de roltoewijzingen maken. Als u een roltoewijzing op een bereik van de bovenliggende site maakt, nemen alle onderliggende bereiken de dezelfde roltoewijzing. De volgende voorbeelden wordt het opgeven van het bereik voor verschillende niveaus:

  - Abonnement: /subscriptions/ {abonnements-id}  
  - Resourcegroep: /subscriptions/ {abonnements-id} / resourceGroups/myresourcegroup1   
  - Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rol-toewijzing-id}* vervangen door een nieuwe GUID, dat de GUID-id van de functietoewijzing voor de nieuwe wordt.

3. *{Api-versie}* vervangen door 2015-07-01.

Voor het hoofdgedeelte van de aanvraag, geef de waarden in de volgende indeling:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| De elementnaam van het     | Vereist | Type   | Beschrijving |
|------------------|----------|--------|-------------|
| roleDefinitionId | Ja      | Tekenreeks | De id van de rol. De opmaak van de identificatie luidt als volgt:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | Ja      | Tekenreeks | object-id van de Azure AD-hoofdsom waarvoor de rol is toegewezen (gebruiker, groep of service principal). |

### <a name="response"></a>Antwoord

Statuscode: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Een roltoewijzing verwijderen

Verwijder een rol toewijzing opgeslagen op het opgegeven bereik.

Als u wilt verwijderen van een roltoewijzing, hebt u toegang tot de `Microsoft.Authorization/roleAssignments/delete` bewerking. Van de ingebouwde functies, zijn alleen de *eigenaar* en de *Gebruiker toegang beheerder* toegang tot deze bewerking. Zie [Toegangsbeheer voor Azure Role-Based](role-based-access-control-configure.md)voor meer informatie over roltoewijzingen en beheren van access voor Azure resources.

### <a name="request"></a>Aanvragen

Gebruik de methode **verwijderen** met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Controleer de waarden van de volgende om aan te passen van uw aanvraag kunt invullen binnen de URI:

1. *{Bereik}* vervangen door het bereik waarop u wilt de roltoewijzingen maken. De volgende voorbeelden wordt het opgeven van het bereik voor verschillende niveaus:

  - Abonnement: /subscriptions/ {abonnements-id}  
  - Resourcegroep: /subscriptions/ {abonnements-id} / resourceGroups/myresourcegroup1  
  - Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rol-toewijzing-id}* vervangen door de rol toewijzing id GUID.

3. *{Api-versie}* vervangen door 2015-07-01.

### <a name="response"></a>Antwoord

Statuscode: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Een lijst met alle rollen

Bevat alle functies die beschikbaar voor de toewijzing opgeslagen op het opgegeven bereik zijn.

Aan lijst rollen, hebt u toegang tot `Microsoft.Authorization/roleDefinitions/read` bewerking op het bereik. De ingebouwde rollen toegangsrechten voor deze bewerking. Zie [Toegangsbeheer voor Azure Role-Based](role-based-access-control-configure.md)voor meer informatie over roltoewijzingen en beheren van access voor Azure resources.

### <a name="request"></a>Aanvragen

Gebruik de methode **ophalen** met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

Controleer de waarden van de volgende om aan te passen van uw aanvraag kunt invullen binnen de URI:

1. *{Bereik}* vervangen door het bereik waarvan u een lijst van de rollen wilt maken. De volgende voorbeelden wordt het opgeven van het bereik voor verschillende niveaus:

  - Abonnement: /subscriptions/ {abonnements-id}  
  - Resourcegroep: /subscriptions/ {abonnements-id} / resourceGroups/myresourcegroup1  
  - Resource-/subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Api-versie}* vervangen door 2015-07-01.

3. *{Filter}* vervangen door de voorwaarde die u wilt toepassen om de lijst met rollen te filteren:

  - Lijst met beschikbare rollen voor toewijzing aan het opgegeven bereik en een van de onderliggende bereiken:`atScopeAndBelow()`
  - Zoeken naar een rol waarbij de exacte weergegeven naam: `roleName%20eq%20'{role-display-name}'`. Gebruik het formulier URL-codering van de exacte weergavenaam van de rol. Bijvoorbeeld:`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Antwoord

Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Informatie over een rol

Informatie ophalen over één functie opgegeven rol definitie-id. Als u informatie over het gebruik van de weergavenaam één functie, raadpleegt u [een lijst met alle rollen](role-based-access-control-manage-access-rest.md#list-all-roles).

Als u informatie over een rol, hebt u toegang tot `Microsoft.Authorization/roleDefinitions/read` bewerking. De ingebouwde rollen toegangsrechten voor deze bewerking. Zie [Toegangsbeheer voor Azure Role-Based](role-based-access-control-configure.md)voor meer informatie over roltoewijzingen en beheren van access voor Azure resources.

### <a name="request"></a>Aanvragen

Gebruik de methode **ophalen** met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Controleer de waarden van de volgende om aan te passen van uw aanvraag kunt invullen binnen de URI:

1. *{Bereik}* vervangen door het bereik waarvan u de roltoewijzingen lijst wilt maken. De volgende voorbeelden wordt het opgeven van het bereik voor verschillende niveaus:

  - Abonnement: /subscriptions/ {abonnements-id}  
  - Resourcegroep: /subscriptions/ {abonnements-id} / resourceGroups/myresourcegroup1  
  - Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rol-definition-id}* vervangen door de GUID-id van de roldefinitie.

3. *{Api-versie}* vervangen door 2015-07-01.

### <a name="response"></a>Antwoord

Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Een aangepaste rol maken
Maak een aangepaste rol.

Als u wilt een aangepaste rol hebt gemaakt, moet u toegang hebt `Microsoft.Authorization/roleDefinitions/write` betrekking heeft op alle de `AssignableScopes`. Van de ingebouwde functies, zijn alleen de *eigenaar* en de *Gebruiker toegang beheerder* toegang tot deze bewerking. Zie [Toegangsbeheer voor Azure Role-Based](role-based-access-control-configure.md)voor meer informatie over roltoewijzingen en beheren van access voor Azure resources.

### <a name="request"></a>Aanvragen

Gebruik de methode **plaatsen** met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Controleer de waarden van de volgende om aan te passen van uw aanvraag kunt invullen binnen de URI:

1. *{Bereik}* vervangen door de eerste *AssignableScope* van de aangepaste rol. De volgende voorbeelden wordt het opgeven van het bereik voor verschillende niveaus.

  - Abonnement: /subscriptions/ {abonnements-id}  
  - Resourcegroep: /subscriptions/ {abonnements-id} / resourceGroups/myresourcegroup1  
  - Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rol-definition-id}* vervangen door een nieuwe GUID, dat de GUID-id van de nieuwe aangepaste rol wordt.

3. *{Api-versie}* vervangen door 2015-07-01.

Voor het hoofdgedeelte van de aanvraag, geef de waarden in de volgende indeling:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| De elementnaam van het | Vereist | Type | Beschrijving |
|--------------|----------|------|-------------|
| naam         | Ja | Tekenreeks   | GUID-id van de aangepaste rol.    |
| properties.roleName               | Ja | Tekenreeks   | De naam van de aangepaste rol weer te geven. Maximale grootte 128 tekens.                        |
| Properties.Description            | Nee  | Tekenreeks   | Beschrijving van de aangepaste rol. Maximale grootte 1024 tekens.                                               |
| Properties.type                   | Ja | Tekenreeks   | Ingesteld op "CustomRole."                                         |
| Properties.permissions.Actions    | Ja | String] | Een matrix van actie tekenreeksen met de bewerkingen die zijn verleend door de aangepaste rol opgeven.             |
| properties.permissions.notActions | Nee  | String] | Een matrix van actie tekenreeksen met de bewerkingen die u wilt uitsluiten van de bewerkingen die zijn verleend door de aangepaste rol opgeven. |
| properties.assignableScopes       | Ja | String] | Een matrix van bereiken waarin de aangepaste rol kan worden gebruikt.   |

### <a name="response"></a>Antwoord

Statuscode: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Een aangepaste rol bijwerken

Een aangepaste rol wijzigen.

Als u wilt wijzigen van een aangepaste rol, moet u toegang hebt `Microsoft.Authorization/roleDefinitions/write` betrekking heeft op alle de `AssignableScopes`. Van de ingebouwde functies, zijn alleen de *eigenaar* en de *Gebruiker toegang beheerder* toegang tot deze bewerking. Zie [Toegangsbeheer voor Azure Role-Based](role-based-access-control-configure.md)voor meer informatie over roltoewijzingen en beheren van access voor Azure resources.

### <a name="request"></a>Aanvragen

Gebruik de methode **plaatsen** met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Controleer de waarden van de volgende om aan te passen van uw aanvraag kunt invullen binnen de URI:

1. *{Bereik}* vervangen door de eerste *AssignableScope* van de aangepaste rol. De volgende voorbeelden wordt het opgeven van het bereik voor verschillende niveaus:

  - Abonnement: /subscriptions/ {abonnements-id}  
  - Resourcegroep: /subscriptions/ {abonnements-id} / resourceGroups/myresourcegroup1  
  - Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rol-definition-id}* vervangen door de GUID-id van de aangepaste rol.

3. *{Api-versie}* vervangen door 2015-07-01.

Voor het hoofdgedeelte van de aanvraag, geef de waarden in de volgende indeling:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| De elementnaam van het | Vereist | Type | Beschrijving |
|--------------|----------|------|-------------|
| naam         | Ja      | Tekenreeks | GUID-id van de aangepaste rol. |
| properties.roleName | Ja | Tekenreeks | De naam van de bijgewerkte aangepaste rol weer te geven. |
| Properties.Description | Nee | Tekenreeks | Beschrijving van de bijgewerkte aangepaste rol. |
| Properties.type | Ja | Tekenreeks | Ingesteld op "CustomRole." |
| Properties.permissions.Actions | Ja | String] | Een matrix van actie tekenreeksen met de bewerkingen waaraan de bijgewerkte aangepaste rol toegang verleent opgeven. |
| properties.permissions.notActions | Nee | String] | Een matrix van actie tekenreeksen met de bewerkingen uitsluiten van de bewerkingen die de bijgewerkte aangepaste rol verleent opgeven. |
| properties.assignableScopes | Ja | String] | Een matrix van bereiken waarin de bijgewerkte aangepaste rol kan worden gebruikt. |

### <a name="response"></a>Antwoord

Statuscode: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen

Een aangepaste rol verwijderen.

Als u wilt verwijderen van een aangepaste rol, hebt u toegang tot `Microsoft.Authorization/roleDefinitions/delete` betrekking heeft op alle de `AssignableScopes`. Van de ingebouwde functies, zijn alleen de *eigenaar* en de *Gebruiker toegang beheerder* toegang tot deze bewerking. Zie [Toegangsbeheer voor Azure Role-Based](role-based-access-control-configure.md)voor meer informatie over roltoewijzingen en beheren van access voor Azure resources.

### <a name="request"></a>Aanvragen

Gebruik de methode **verwijderen** met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Controleer de waarden van de volgende om aan te passen van uw aanvraag kunt invullen binnen de URI:

1. *{Bereik}* vervangen door het bereik waarop u wilt verwijderen van de roldefinitie. De volgende voorbeelden wordt het opgeven van het bereik voor verschillende niveaus:

  - Abonnement: /subscriptions/ {abonnements-id}  
  - Resourcegroep: /subscriptions/ {abonnements-id} / resourceGroups/myresourcegroup1  
  - Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rol-definition-id}* vervangen door de GUID rol definitie-id van de aangepaste rol.

3. *{Api-versie}* vervangen door 2015-07-01.

### <a name="response"></a>Antwoord

Statuscode: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
