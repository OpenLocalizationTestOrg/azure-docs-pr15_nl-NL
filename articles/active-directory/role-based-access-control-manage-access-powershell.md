<properties
    pageTitle="Rolgebaseerd toegangsbeheer (RBAC) met Azure PowerShell beheren | Microsoft Azure"
    description="Het beheren van RBAC met Azure PowerShell, met inbegrip van de vermelding van de rollen, rollen toewijzen en verwijderen van roltoewijzingen."
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

# <a name="manage-role-based-access-control-with-azure-powershell"></a>Rolgebaseerd toegangsbeheer met Azure PowerShell beheren

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)


U kunt Rolgebaseerd toegangsbeheer RBAC () in de portal van Azure en Azure Resource Management API gebruiken voor het beheren van toegang aan uw abonnement op een fijnmazige niveau. Met deze functie kunt u de toegang voor gebruikers, groepen of service principes Active Directory verlenen door sommige rollen toewijzen aan deze op een bepaald bereik.

Voordat u PowerShell gebruiken kunt voor het beheren van RBAC, hebt u het volgende:

- Azure PowerShell versie 0.8.8 of hoger. Installeer de meest recente versie en koppelen aan uw Azure-abonnement: Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

- Azure resourcemanager cmdlets. Installeer de [resourcemanager Azure-cmdlets](https://msdn.microsoft.com/library/mt125356.aspx) in PowerShell.

## <a name="list-roles"></a>Lijst weergeven van functies

### <a name="list-all-available-roles"></a>Een lijst met alle beschikbare rollen
Aan lijst RBAC rollen die beschikbaar zijn voor de toewijzing en te controleren van de bewerkingen waartoe ze toegang, gebruikt u `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![Schermafbeelding van de PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Lijst met acties van een rol
U kunt de acties voor een specifieke rol gebruiken `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![Schermafbeelding van de PowerShell-Get AzureRmRoleDefinition voor een specifieke rol - RBAC](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Bekijken wie toegang heeft
Aan de lijst RBAC access toewijzingen, voert u `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Roltoewijzingen lijst op een bepaald bereik
Hier ziet u alle access-toewijzingen voor een opgegeven abonnement, resourcegroep of resource. Bijvoorbeeld als u wilt zien van alle actieve toewijzingen voor een resourcegroep, gebruikt u `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![Schermafbeelding van de PowerShell-Get AzureRmRoleAssignment voor een resourcegroep - RBAC](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Lijst weergeven van functies die zijn toegewezen aan een gebruiker
U kunt de rollen die zijn toegewezen aan een specifieke gebruiker en de rollen die zijn toegewezen aan de groepen waartoe de gebruiker behoort gebruiken `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![Schermafbeelding van de PowerShell-Get AzureRmRoleAssignment voor een gebruiker - RBAC](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Lijst klassieke service-beheerder en coadmin roltoewijzingen
Aan de lijst met access toewijzingen voor de beheerder van de klassieke abonnement en coadministrators, gebruiken:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Toegang verlenen
### <a name="search-for-object-ids"></a>Zoeken naar object-id 's
Als u wilt een rol toewijzen, moet u zowel het object (gebruiker, groep of toepassing) en het bereik identificeren.

Als u niet dat de abonnements-ID weet, kunt u deze vinden in het blad **abonnementen** op de Azure-portal. Als u wilt weten hoe u de query voor de abonnements-ID, Zie [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) op MSDN.

Als u de object-ID voor een Azure AD-groep, gebruikt u:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Als u de object-ID voor een Azure AD-service principal of toepassing, gebruikt u:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Een rol toewijzen aan een toepassing voor het bereik van abonnement
Als u wilt verlenen van toegang tot een toepassing voor het bereik van abonnement, gebruiken:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![Schermafbeelding van de PowerShell nieuwe AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Een rol toewijzen aan een gebruiker op het bereik van de groep resource
Om toegang te verlenen aan een gebruiker voor het bereik van de groep resource, gebruiken:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![Schermafbeelding van de PowerShell nieuwe AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Een rol aan een groep voor het bereik van de resource toewijzen
Om toegang te verlenen aan een groep voor het bereik van de resource, gebruiken:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![Schermafbeelding van de PowerShell nieuwe AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Verwijder toegang
Als u wilt verwijderen toegang voor gebruikers, groepen en -toepassingen, gebruiken:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![Schermafbeelding van de PowerShell-verwijderen AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Een aangepaste rol maken
Als u wilt maken van een aangepaste rol, gebruikt u de `New-AzureRmRoleDefinition` opdracht.

Wanneer u een aangepaste rol via PowerShell maakt, moet u beginnen met een van de [ingebouwde functies](role-based-access-built-in-roles.md). De kenmerken om toe te voegen van de *Acties*, *notActions*of *bereiken* die u wilt bewerken, en sla de wijzigingen als een nieuwe rol.

Het volgende voorbeeld begint met de rol *Inzender VM* en die wordt gebruikt om te maken van een aangepaste rol *VM Operator*genoemd. De nieuwe rol toegang verleent tot alle gelezen bewerkingen van *Microsoft.Compute*, *Microsoft.Storage*en *Microsoft.Network* resource-providers toegang verleent tot begint, start opnieuw en virtuele machines controleren. De aangepaste rol kan worden gebruikt in twee abonnementen.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![Schermafbeelding van de PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>Een aangepaste rol wijzigen
Als u wilt wijzigen van een aangepaste rol, gebruikt u eerst de `Get-AzureRmRoleDefinition` opdracht om te vragen de roldefinitie. Controleer vervolgens de gewenste wijzigingen aan de roldefinitie van de. Ten slotte, gebruikt u de `Set-AzureRmRoleDefinition` opdracht om op te slaan de roldefinitie van de gewijzigde.

Het volgende voorbeeld wordt de `Microsoft.Insights/diagnosticSettings/*` bewerking aan de aangepaste rol *VM Operator* .

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![Schermafbeelding van de PowerShell-Set AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Het volgende voorbeeld wordt een Azure-abonnement aan de toegewezen bereiken van de aangepaste rol *VM Operator* .

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![Schermafbeelding van de PowerShell-Set AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen

Als u wilt verwijderen van een aangepaste rol, gebruikt u de `Remove-AzureRmRoleDefinition` opdracht.

De aangepaste rol *VM Operator* Hiermee verwijdert u het volgende voorbeeld.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![Schermafbeelding van de PowerShell-verwijderen AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Lijst met aangepaste rollen
U kunt de rollen die beschikbaar voor de toewijzing opgeslagen op een bereik zijn gebruiken de `Get-AzureRmRoleDefinition` opdracht.

Het volgende voorbeeld worden alle functies die beschikbaar voor de toewijzing in het geselecteerde abonnement zijn.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![Schermafbeelding van de PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

In het volgende voorbeeld wordt de aangepaste rol *VM Operator* is niet beschikbaar in het abonnement *Production4* omdat deze abonnement niet wordt vermeld op de **AssignableScopes** van de rol.

![Schermafbeelding van de PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Zie ook
- [Via Azure PowerShell met Azure resourcemanager](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
