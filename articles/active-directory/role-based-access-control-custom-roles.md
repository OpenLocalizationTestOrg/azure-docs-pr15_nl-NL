<properties
    pageTitle="Aangepaste rollen in Azure RBAC | Microsoft Azure"
    description="Informatie over het definiëren van aangepaste rollen met toegangsbeheer Azure Role-Based voor nauwkeuriger identiteitsbeheer in uw Azure-abonnement."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="kgremban"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/25/2016"
    ms.author="kgremban"/>


# <a name="custom-roles-in-azure-rbac"></a>Aangepaste rollen in Azure RBAC


Maak een aangepaste rol in Azure Role-Based Access besturingselement RBAC (), als geen van de ingebouwde rollen uw behoeften voor specifieke toegang. Aangepaste rollen kunnen worden gemaakt met behulp van [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure opdrachtregel-Interface](role-based-access-control-manage-access-azure-cli.md) (CLI) en de [REST API](role-based-access-control-manage-access-rest.md). Net als ingebouwde functies, kunnen aangepaste rollen worden toegewezen aan gebruikers, groepen en toepassingen op abonnement, resourcegroep en bereiken van de resource. Aangepaste rollen worden opgeslagen in een Azure AD-tenant en kunnen worden gedeeld met alle abonnementen die die tenant als de Azure AD-map voor de subsciption gebruiken.

Hier volgt een voorbeeld van een aangepaste rol voor controle en opnieuw starten van virtuele machines:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Acties
De eigenschap **Acties** van een aangepaste rol Hiermee worden de Azure bewerkingen waaraan de rol toegang verleent. Dit is een verzameling bewerking tekenreeksen die beveiligbare bewerkingen van Azure resource providers identificeren. Tekenreeksen met de bewerking die jokertekens bevatten (\*) toegang verlenen tot alle bewerkingen die overeenkomen met de bewerking tekenreeks. Bijvoorbeeld:

-   `*/read`verleent toegang tot uw bewerkingen voor alle typen van alle providers van Azure resource lezen.
-   `Microsoft.Network/*/read`verleent toegang tot uw bewerkingen voor alle resourcetypen in de provider van de resource Microsoft.Network Azure lezen.
-   `Microsoft.Compute/virtualMachines/*`verleent de toegang tot alle bewerkingen van virtuele machines en de onderliggende resourcetypen.
-   `Microsoft.Web/sites/restart/Action`verleent toegang tot websites opnieuw.

Gebruik `Get-AzureRmProviderOperation` (in PowerShell) of `azure provider operations show` (in Azure CLI) lijst activiteiten van Azure resource providers. U kunt ook deze opdrachten om te controleren of een tekenreeks bewerking geldig is en om uit te vouwen jokertekens bewerking tekenreeksen.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Schermafbeelding van de PowerShell - Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT bewerking OperationName](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI schermafbeelding - azure provider bewerkingen weergeven ' Microsoft.Compute/virtualMachines/\*/Action " ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
De eigenschap **NotActions** gebruikt als de set bewerkingen die u wilt toestaan eenvoudiger is gedefinieerd door beperkte bewerkingen uit te sluiten. De toegang wordt verleend door een aangepaste rol wordt berekend door af te trekken van de **NotActions** bewerkingen uit de **Acties** bewerkingen.

> [AZURE.NOTE] Als een gebruiker is toegewezen een rol die een bewerking in **NotActions**sluit en een tweede rol die toegang tot dezelfde bewerking verleent is toegewezen, wordt de gebruiker voor deze bewerking wordt toegestaan. **NotActions** is niet een regel voor weigeren, maar deze is gewoon een handige manier om te maken van een reeks toegestane bewerkingen wanneer specifieke bewerkingen moeten worden uitgesloten.

## <a name="assignablescopes"></a>AssignableScopes
De eigenschap **AssignableScopes** van de aangepaste rol Hiermee geeft u de bereiken (abonnementen, resourcegroepen of resources) waarbinnen de aangepaste rol beschikbaar voor de toewijzing is. U kunt de aangepaste rol beschikbaar maken voor toewijzing in alleen de abonnementen of resourcegroepen die het nodig hebben en niet onbelangrijke e-mail gebruikerservaring voor de rest van de abonnementen of resourcegroepen.

Voorbeelden van geldige toegewezen bereiken zijn:

-   "/ abonnementen/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ abonnementen/e91d47c4-76f3-4271-a796-21b4ecfe3624" - ervoor zorgt dat de rol beschikbaar voor de toewijzing in twee abonnementen.
-   "/ abonnementen/c276fc76-9cd4-44c9-99a7-4fd71546436e" - ervoor zorgt dat de rol beschikbaar voor de toewijzing in een één abonnement.
-  "/ abonnementen/c276fc76 / 9cd4 / 44c9 / 99a7 / 4fd71546436e/resourceGroups/netwerk" - Hiermee wordt de rol beschikbaar gemaakt voor de toewijzing alleen in de resourcegroep netwerk.

> [AZURE.NOTE] Moet u ten minste één abonnement, resourcegroep of resource-ID.

## <a name="custom-roles-access-control"></a>Aangepaste rollen toegangsbeheer
De eigenschap **AssignableScopes** van de aangepaste rol bepaalt ook wie kan bekijken, wijzigen en verwijderen van de rol.

- Wie kan een aangepaste rol maken?
    Eigenaren (en beheerders van de gebruiker toegang) van abonnementen, resourcegroepen en resources kunnen aangepaste rollen maken voor gebruik in deze bereiken.
    De gebruiker de rol maken moet uitvoeren `Microsoft.Authorization/roleDefinition/write` bewerking op alle **AssignableScopes** van de rol.

- Wie kan een aangepaste rol wijzigen?
    Eigenaren (en beheerders van de gebruiker toegang) van abonnementen, resourcegroepen en bronnen aangepaste rollen in deze bereiken kunnen wijzigen. Gebruikers moeten kunnen om uit te voeren de `Microsoft.Authorization/roleDefinition/write` bewerking op alle **AssignableScopes** van een aangepaste rol.

- Wie kan aangepaste rollen bekijken?
    Alle ingebouwde rollen in Azure RBAC toestaan weergeven van de rollen die beschikbaar voor de toewijzing zijn. Gebruikers kunnen uitvoeren de `Microsoft.Authorization/roleDefinition/read` bewerking op een bereik de RBAC rollen die beschikbaar voor de toewijzing opgeslagen op dat bereik zijn kunt weergeven.

## <a name="see-also"></a>Zie ook
- [Rol gebaseerd toegangsbeheer](role-based-access-control-configure.md): aan de slag met RBAC in de portal van Azure.
- Meer informatie over het beheren van toegang met:
    - [PowerShell](role-based-access-control-manage-access-powershell.md)
    - [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
    - [REST API](role-based-access-control-manage-access-rest.md)
- [Ingebouwde rollen](role-based-access-built-in-roles.md): informatie over de rollen die worden standaard in RBAC geleverd.
