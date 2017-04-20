<properties
    pageTitle="Rolgebaseerd toegangsbeheer (RBAC) met Azure CLI beheren | Microsoft Azure"
    description="Informatie over het beheren van Rolgebaseerd toegangsbeheer (RBAC) met de Azure opdrachtregel-interface door de vermelding van rollen en rol acties en rollen toewijzen aan het abonnement en -toepassing bereiken."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Toegangsbeheer op basis van rollen met de Azure opdrachtregel-interface beheren

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)

U kunt Rolgebaseerd toegangsbeheer RBAC () in de portal van Azure en Azure resourcemanager API gebruiken voor het beheren van toegang tot uw abonnement en resources op een fijnmazige niveau. Met deze functie kunt u de toegang voor gebruikers, groepen of service principes Active Directory verlenen door sommige rollen toewijzen aan deze op een bepaald bereik.

Voordat u de opdrachtregel Azure (CLI) gebruiken kunt voor het beheren van RBAC, hebt u het volgende:

- Azure CLI versie 0.8.8 of hoger. Als u wilt de nieuwste versie installeren en koppelen aan uw Azure-abonnement, Zie [installeren en configureren van de Azure CLI](../xplat-cli-install.md).
- Azure resourcemanager in Azure CLI. Ga naar het [gebruik van de Azure CLI met bronbeheer](../xplat-cli-azure-resource-manager.md) voor meer informatie.

## <a name="list-roles"></a>Lijst weergeven van functies

### <a name="list-all-available-roles"></a>Een lijst met alle beschikbare rollen
U kunt alle beschikbare rollen gebruiken:

        azure role list

Het volgende voorbeeld ziet u de lijst met *alle beschikbare rollen*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Schermafbeelding van de opdrachtregel - lijst azure rol - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Lijst met acties van een rol
U kunt de acties van een rol gebruiken:

    azure role show "<role name>"

Het volgende voorbeeld ziet u de acties van de rollen *Inzender* en *VM Inzender* .

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Schermafbeelding van de opdrachtregel - azure rol weergeven - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>Lijst met access
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Lijst roltoewijzingen effectieve op een resourcegroep
U kunt de roltoewijzingen die aanwezig zijn in een resourcegroep gebruiken:

    azure role assignment list --resource-group <resource group name>

Het volgende voorbeeld ziet de roltoewijzingen in de groep *pharma-verkoop-projecforcast* .

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Opdrachtregel RBAC Azure - azure rol toewijzing sorteren op groep-schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Lijst roltoewijzingen voor een gebruiker
U kunt de roltoewijzingen voor een specifieke gebruiker en de toewijzingen die zijn toegewezen aan groepen van een gebruiker gebruiken:

    azure role assignment list --signInName <user email>

U kunt ook zien roltoewijzingen die worden overgenomen van groepen doordat de opdracht:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Het volgende voorbeeld ziet u de roltoewijzingen die zijn toegewezen aan de *sameert@aaddemo.com* gebruiker. Dit geldt ook voor rollen die rechtstreeks aan de gebruiker zijn toegewezen en functies die worden overgenomen van groepen.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Schermafbeelding van de opdrachtregel - toewijzingslijst azure rol door gebruiker - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>Toegang verlenen
Om toegang te verlenen nadat u hebt aangegeven dat de rol die u wilt toewijzen, gebruiken:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Een rol toewijzen aan groep op het bereik van abonnement
Als u wilt een rol toewijzen aan een groep voor het bereik van abonnement, gebruiken:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Het volgende voorbeeld wordt de functie *lezer* naar *Christine Koch van Team* voor het bereik van *abonnement* .


![RBAC Azure opdrachtregel - azure roltoewijzing maken groep-schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Een rol toewijzen aan een toepassing voor het bereik van abonnement
Als u wilt een rol toewijzen aan een toepassing voor het bereik van abonnement, gebruiken:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Het volgende voorbeeld worden de rol *Inzender* bij een *Azure AD* -toepassing op de geselecteerde abonnement toegewezen.

 ![RBAC Azure opdrachtregel - azure roltoewijzing maken door toepassing](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Een rol toewijzen aan een gebruiker op het bereik van de groep resource
Als u wilt een rol toewijzen aan een gebruiker op het bereik van de groep resource, gebruiken:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Het volgende voorbeeld worden de rol *Inzender VM* naar toegewezen *samert@aaddemo.com* gebruiker op het bereik *Pharma-verkoop-ProjectForcast* resource.

![RBAC Azure opdrachtregel - azure roltoewijzing maken door de gebruiker-schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Een rol aan een groep voor het bereik van de resource toewijzen
Als u wilt een rol toewijzen aan een groep voor het bereik van de resource, gebruiken:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Het volgende voorbeeld worden de rol *Inzender VM* toegewezen aan een *Azure AD* -groep op een *subnet*.

![RBAC Azure opdrachtregel - azure roltoewijzing maken groep-schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>Verwijder toegang
Als u wilt verwijderen van een roltoewijzing, gebruiken:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Het volgende voorbeeld wordt verwijderd van de toewijzing van de rol *Inzender VM* uit de *sammert@aaddemo.com* gebruiker op de resourcegroep *Pharma-verkoop-ProjectForcast* .
De toewijzing van rol verwijdert in het voorbeeld uit een groep op het abonnement.

![Schermafbeelding van de opdrachtregel - azure rol toewijzing verwijderen - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Een aangepaste rol maken
Als u een aangepaste rol hebt gemaakt, wilt gebruiken:

    azure role create --inputfile <file path>

Het volgende voorbeeld wordt een aangepaste rol *VM Operator*genoemd. De aangepaste rol toegang verleent tot alle gelezen bewerkingen van *Microsoft.Compute*, *Microsoft.Storage*en *Microsoft.Network* resource-providers toegang verleent tot begint, opnieuw opstarten en virtuele machines controleren. De aangepaste rol kan worden gebruikt in twee abonnementen. Dit voorbeeld wordt een JSON-bestand als een invoer.

![Schermafbeelding van de JSON - aangepaste rol definition-](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure opdrachtregel - azure rol maken - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Een aangepaste rol wijzigen

Als u wilt wijzigen van een aangepaste rol, gebruikt u eerst de `azure role show` opdracht om te vragen roldefinitie. Controleer vervolgens de gewenste wijzigingen aan het definitiebestand rol. Tot slot gebruiken `azure role set` om op te slaan de roldefinitie van de gewijzigde.

    azure role set --inputfile <file path>

Het volgende voorbeeld wordt de bewerking *Microsoft.Insights/diagnosticSettings/* de **Acties**en een Azure abonnement op de **AssignableScopes** van de aangepaste rol VM Operator.

![JSON - definitie van aangepaste rol - schermafbeelding wijzigen](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Schermafbeelding van de opdrachtregel - azure rol set - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen

Als u wilt verwijderen van een aangepaste rol, gebruikt u eerst de `azure role show` opdracht om te bepalen de **ID** van de rol. Gebruik vervolgens de `azure role delete` opdracht om te verwijderen van de rol door het opgeven van de **ID**.

De aangepaste rol *VM Operator* Hiermee verwijdert u het volgende voorbeeld.

![Schermafbeelding van de opdrachtregel - azure rol verwijderen - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Lijst met aangepaste rollen

U kunt de rollen die beschikbaar voor de toewijzing opgeslagen op een bereik zijn gebruiken de `azure role list` opdracht.

Het volgende voorbeeld worden alle rol die beschikbaar zijn voor de toewijzing in het geselecteerde abonnement.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Schermafbeelding van de opdrachtregel - lijst azure rol - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

In het volgende voorbeeld wordt de aangepaste rol *VM Operator* is niet beschikbaar in het abonnement *Production4* omdat deze abonnement niet wordt vermeld op de **AssignableScopes** van de rol.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Schermafbeelding van de opdrachtregel - azure rol lijst voor aangepaste rollen - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>RBAC onderwerpen
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
