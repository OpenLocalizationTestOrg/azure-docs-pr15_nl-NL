<properties 
    pageTitle="Resources met resourcemanager vergrendelen | Microsoft Azure" 
    description="Voorkomen dat gebruikers bijwerken of verwijderen van bepaalde resources door een beperking voor alle gebruikers en rollen toe te voegen." 
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
    ms.date="08/15/2016" 
    ms.author="tomfitz"/>

# <a name="lock-resources-with-azure-resource-manager"></a>Resources met Azure resourcemanager vergrendelen

Als beheerder moet u mogelijk een abonnement, resourcegroep of resource om te voorkomen dat andere gebruikers in uw organisatie uit per ongeluk verwijderen of wijzigen van kritieke bronnen vergrendelen. U kunt het niveau van de vergrendelen instellen op **CanNotDelete** of **alleen-lezen**. 

- **CanNotDelete** betekent dat bevoegde gebruikers nog steeds kunnen lezen en wijzigen van een resource, maar ze deze niet verwijderen. 
- **Alleen-lezen** betekent dat bevoegde gebruikers kunnen lezen van een resource, maar ze niet kunnen verwijderen of bewerkingen uitvoeren op is geïnstalleerd. De machtiging voor de resource is beperkt tot de rol van de **lezer** . 

**Alleen-lezen** toepassen kan leiden tot onverwachte resultaten omdat sommige bewerkingen die lijkt lezen bewerkingen daadwerkelijk extra acties vereist. Bijvoorbeeld een **alleen-lezen** -vergrendelen in een account opslag plaatsen voorkomt dat alle gebruikers vermelding van de toetsen. De lijst toetsen bewerking via een POST-aanvraag is verwerkt omdat de resulterende sleutels zijn beschikbaar voor schrijven bewerkingen. Voor een ander voorbeeld verhinderd een **alleen-lezen** -slot brengen door een resource App Service Visual Studio Server Explorer weergeven bestanden voor de resource omdat die interactie schrijftoegang vereist.

In tegenstelling tot Rolgebaseerd toegangsbeheer gebruik u management vergrendelingen te voorzien van een beperking voor alle gebruikers en rollen. Zie voor meer informatie over het instellen van machtigingen voor gebruikers en rollen, [Toegangsbeheer op basis van Azure rol](./active-directory/role-based-access-control-configure.md).

Wanneer u een vergrendelen in een bereik van de bovenliggende toepast, nemen alle onderliggende resources over de dezelfde vergrendelen. Even bronnen die u later toevoegt overnemen de vergrendeling van de bovenliggende site. De meeste beperkingen vergrendelen in de overname van prioriteit.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Wie kan maken of verwijderen van vergrendelingen in uw organisatie

Als u wilt maken of verwijderen van management vergrendelingen, hebt u toegang tot **Microsoft.Authorization/\* ** of **Microsoft.Authorization/locks/\* ** acties. Van de ingebouwde functies, worden alleen de **eigenaar** en de **Gebruiker toegang beheerder** deze acties verleend.

## <a name="creating-a-lock-through-the-portal"></a>Maken van een slot via de portal

[AZURE.INCLUDE [resource-manager-lock-resources](../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Een vergrendelen maken in een sjabloon

Het volgende voorbeeld ziet u een sjabloon die wordt gemaakt van een slot op een opslag-account. Het account opslag waarop u wilt de vergrendeling van toepassing is opgegeven als een parameter. De naam van het hangslot is gemaakt door de naam van de resource met **/Microsoft.Authorization/** en de naam van de vergrendelen, klikt u in dit geval **myLock**aaneen te schakelen.

Het type die hoort bij het resourcetype. Voor de opslag, moet u het type naar "Microsoft.Storage/storageaccounts/providers/locks" instellen.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Maken van een slot met REST API

U kunt geïmplementeerd resources met de [REST API voor management vergrendelingen](https://msdn.microsoft.com/library/azure/mt204563.aspx)vergrendelen. De REST API kunt u het maken en verwijderen van vergrendelingen en informatie ophalen over bestaande vergrendelingen.

Als u wilt maken van een slot, voert u het:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Het bereik mogelijk een abonnement, resourcegroep of resource. De naam van de lock is wat u wilt bellen de vergrendelen. Gebruik voor api-versie: **01-01-2015**.

In de aanvraag bevat een JSON-object waarmee de eigenschappen voor de vergrendelen.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Zie [REST API voor management vergrendelingen](https://msdn.microsoft.com/library/azure/mt204563.aspx)voor voorbeelden.

## <a name="creating-a-lock-with-azure-powershell"></a>Maken van een slot met Azure PowerShell

U kunt geïmplementeerd resources met Azure PowerShell vergrendelen met behulp van de **Nieuw-AzureRmResourceLock** , zoals wordt weergegeven in het volgende voorbeeld.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure PowerShell biedt andere opdrachten voor het werken vergrendelingen, zoals **Set-AzureRmResourceLock** een slot bijwerken en **Verwijderen-AzureRmResourceLock** verwijderen van een slot.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het werken met resource vergrendelingen [Vergrendelen omlaag uw Azure Resources](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- Meer informatie over het logisch indelen van uw bronnen, raadpleegt u [werken met tags om u te organiseren van uw resources](resource-group-using-tags.md)
- Als u wilt wijzigen welke resourcegroep die een resource bevindt zich in de, raadpleegt u [resources aan nieuwe resourcegroep verplaatsen](resource-group-move-resources.md)
- U kunt beperkingen en conventies toepassen op uw abonnement met aangepaste machtigingen. Zie [Beleid voor het beheren van resources en toegang](resource-manager-policy.md)voor meer informatie.
