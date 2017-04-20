<properties
    pageTitle="Gebruikersmachtigingen voor specifieke testomgeving beleid verlenen | Microsoft Azure"
    description="Leer hoe u gebruikersmachtigingen aan specifieke testomgeving beleidsregels in DevTest Labs op basis van de behoeften van elke gebruiker toewijzen"
    services="devtest-lab,virtual-machines,visual-studio-online"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Gebruikersmachtigingen verlenen voor specifieke testomgeving beleid

## <a name="overview"></a>Overzicht

Dit artikel wordt beschreven hoe u gebruikersmachtigingen verlenen voor een bepaalde testomgeving beleid met PowerShell. Op deze manier kunnen machtigingen worden toegepast op basis van de behoeften van elke gebruiker. U wilt bijvoorbeeld de mogelijkheid om te wijzigen van de beleidsinstellingen VM, maar niet de beleidsregels van de kosten voor een bepaalde gebruiker verlenen.

## <a name="policies-as-resources"></a>Beleidsregels als resources

Zoals in het artikel [Azure Rolgebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md) besproken, kunt RBAC fijnmazige toegangsbeheer van resources voor Azure. RBAC gebruikt, kunt u taken in uw team DevOps scheiden en alleen de hoeveelheid toegang verlenen aan gebruikers die ze nodig hebben om uit te voeren hun taken.

Een beleid is in DevTest Labs, een resourcetype waarmee de actie RBAC **Microsoft.DevTestLab/labs/policySets/policies/**. Elk beleid testomgeving is een resource in het resourcetype, en als bereik aan een rol RBAC kan worden toegewezen.

Bijvoorbeeld om te verlenen gebruikers alleen-lezen/schrijven machtigingen voor het beleid **Toegestaan VM formaten** , maakt u een aangepaste rol die met het **Microsoft.DevTestLab/labs/policySets/policies/ werkt** * actie, en vervolgens de juiste gebruikers toewijzen aan deze aangepaste rol in het bereik van * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Meer informatie over aangepaste rollen in RBAC, raadpleegt u het [aangepaste rollen beheren in access](../active-directory/role-based-access-control-custom-roles.md).

##<a name="creating-a-lab-custom-role-using-powershell"></a>Maken van een testomgeving aangepaste rol via PowerShell
Om te kunnen beginnen, moet u het volgende artikel, waarin wordt uitgelegd hoe u kunt installeren en configureren van de Azure PowerShell-cmdlets lezen: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Nadat u de Azure PowerShell-cmdlets hebt ingesteld, kunt u de volgende taken uitvoeren:

- Lijst van alle bewerkingen/acties voor een resource-provider
- Lijst met acties in een bepaalde rol:
- Een aangepaste rol maken

Het volgende PowerShell-script ziet u voorbeelden van hoe u deze taken uitvoert:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Wanneer u machtigingen toewijst aan een gebruiker voor een specifieke beleid met aangepaste rollen
Als u uw aangepaste rollen hebt gedefinieerd, kunt u ze kunt toewijzen aan gebruikers. Een aangepaste rol aan een gebruiker toewijzen, moet u eerst de **object-id** , dat staat voor die gebruiker aanvragen. Als u wilt dat doet, gebruikt u de cmdlet **Get-AzureRmADUser** .

In het volgende voorbeeld wordt de **object-id** van de gebruiker *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Nadat u de **object-id** voor de gebruiker en de naam van een aangepaste rol hebt, kunt u die rol toewijzen aan de gebruiker met de cmdlet **New-AzureRmRoleAssignment** :

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

In het vorige voorbeeld, wordt het beleid **AllowedVmSizesInLab** gebruikt. U kunt een van de volgende beleid:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt verleende gebruikersmachtigingen aan specifieke testomgeving beleid, Hier volgen enkele volgende stappen:

- [Toegang tot een laboratorium secure](devtest-lab-add-devtest-user.md).

- [Testomgeving beleid instellen](devtest-lab-set-lab-policy.md).

- [Een sjabloon testomgeving maken](devtest-lab-create-template.md).

- [Aangepaste onderdelen maken voor uw VMs](devtest-lab-artifact-author.md).

- [Een VM met onderdelen aan een laboratorium toevoegen](devtest-lab-add-vm-with-artifacts.md).
