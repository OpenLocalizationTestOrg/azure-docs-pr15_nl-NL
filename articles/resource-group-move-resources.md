<properties 
    pageTitle="Resources verplaatsen naar nieuwe resourcegroep | Microsoft Azure" 
    description="Azure Resource Manager gebruiken voor het verplaatsen van resources naar een nieuwe resourcegroep of -abonnement." 
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
    ms.date="10/21/2016" 
    ms.author="tomfitz"/>

# <a name="move-resources-to-new-resource-group-or-subscription"></a>Resources verplaatsen naar nieuwe resourcegroep-abonnement

In dit onderwerp ziet u hoe resources verplaatsen naar een nieuw abonnement of een nieuwe resourcegroep in hetzelfde abonnement. U kunt de portal, PowerShell, Azure CLI of de REST API resource verplaatsen. De bewerkingen verplaatsen in dit onderwerp zijn beschikbaar voor u zonder een hulp van de Azure ondersteuning.

Meestal verplaatsen u resources wanneer u dat besluit:

- Voor billing doeleinden moet een resource in een ander abonnement van wonen.
- Een resource deelt niet langer de levenscyclus van dezelfde als de resources die het eerder met is gegroepeerd. U wilt verplaatsen naar een nieuwe resourcegroep zodat u kunt deze resource afzonderlijk beheren in de andere bronnen.

Wanneer u resources verplaatst, worden zowel de bronnengroep en de doelgroep vergrendeld tijdens de bewerking. Schrijven en verwijderen op de groepen worden geblokkeerd totdat de overstap is voltooid.

U kunt de locatie van de resource niet wijzigen. Door een resource alleen verplaatst u deze naar een nieuwe resourcegroep. De nieuwe resourcegroep mogelijk een andere locatie, maar die niet de locatie van de resource wordt gewijzigd.

> [AZURE.NOTE] In dit artikel wordt beschreven hoe resources in een bestaande Azure-account aanbieden verplaatsen. Als u wijzigen van uw aanbod wilt (zoals een upgrade van pay-as-you-go vooraf betalen) terwijl u werkt met uw bestaande resources Azure-account, raadpleegt u [overschakelen naar een ander voorstel uw Azure-abonnement](billing-how-to-switch-azure-offer.md). 

## <a name="checklist-before-moving-resources"></a>Controlelijst voordat u verdergaat resources

Er zijn enkele belangrijke stappen uitvoeren voordat u verdergaat van een resource. Door te verifiëren deze voorwaarden, kunt u fouten te voorkomen.

1. De service moet de mogelijkheid om te verplaatsen van resources inschakelen. Raadpleeg de lijst hieronder voor informatie over welke [services kunnen zwevend bronnen](#services-that-enable-move).
1. De bron- en doeltabellen abonnementen moeten bestaan binnen dezelfde [Active Directory-tenant](./active-directory/active-directory-howto-tenant.md). Als u wilt verplaatsen naar een nieuwe tenant, belt u ondersteuning.
2. Het abonnement bestemming moet zijn geregistreerd voor de resource-provider van de resource wordt verplaatst. Als dit niet het geval is, wordt er een foutbericht met de mededeling dat het **abonnement is niet geregistreerd voor een resourcetype**. U kunt dit probleem tegenkomen bij het verplaatsen van een resource aan een nieuw abonnement, maar dat nooit abonnement dat type resource gebruikt is. Als u wilt weten hoe u de registratiestatus van de controleren en resource providers hebt geregistreerd, raadpleegt u de [Resource providers en typen](../resource-manager-supported-services.md#resource-providers-and-types).
4. Als u App Service app verplaatst, kunt u de [App servicebeperkingen](#app-service-limitations)hebt bekeken.
4. Als u resources die zijn gekoppeld aan herstel Services verplaatst, kunt u de [beperkingen bij het herstelproces is Services](#recovery-services-limitations) hebt bekeken
5. Als u resources die zijn geïmplementeerd via klassieke objectmodel verplaatst, kunt u [klassieke implementatie beperkingen](#classic-deployment-limitations)hebt bekeken.

## <a name="when-to-call-support"></a>Wanneer u ondersteuning bellen

U kunt de meeste bronnen via de selfservice-bewerkingen die wordt weergegeven in dit onderwerp verplaatsen. De bewerkingen selfservice om te gebruiken:

- Resourcemanager bronnen verplaatsen.
- Klassieke bronnen op basis van de [beperkingen bij het klassieke implementatie](#classic-deployment-limitations)verplaatsen. 

Bel de ondersteuning als u wilt:

- Verplaats uw resources naar een nieuwe Azure-account (en de Active Directory-tenant).
- Klassieke resources verplaatsen maar problemen hebt met de beperkingen.

## <a name="services-that-enable-move"></a>Services waarmee verplaatsen

Nu zijn de services die verplaatsen naar een nieuwe resourcegroep en een abonnement in staat stellen:

- API-beheer
- App Service-apps (WebApps) - zien [App servicebeperkingen](#app-service-limitations)
- Automatisering
- Batch
- CDN
- Cloud Services - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
- Gegevens Factory
- DNS
- DocumentDB
- HDInsight clusters
- IoT Hubs
- Belangrijke kluis 
- Mediaservices
- Mobiele betrokkenheid
- Melding Hubs
- Operationele inzichten
- Bestand Vgx Cache.
- Planner
- Zoeken
- Service Bus
- Opslag
- Opslag (klassieke) - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
- SQL-databaseserver - de database en server moet zich bevinden in dezelfde resourcegroep. Wanneer u een SQL server hebt verplaatst, worden ook alle bijbehorende databases verplaatst.
- Virtuele Machines - echter doet geen ondersteuning voor verplaatsen naar een nieuw abonnement als de certificaten zijn opgeslagen in een kluis-toets
- Virtuele Machines (klassieke) - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
- Virtuele netwerken

## <a name="services-that-do-not-enable-move"></a>Services waarmee niet verplaatsen

De services die momenteel niet inschakelt verplaatsen van een resource zijn:

- Toepassingsgateway
- Toepassing inzichten
- Route Express
- Herstel Services kluis - ook niet de berekeningscluster-, netwerk- en resources die zijn gekoppeld aan de kluis herstel Services verplaatsen, Zie [herstel Services beperkingen](#recovery-services-limitations).
- Virtuele Machines met certificaat dat is opgeslagen in de sleutel kluis
- Virtuele Machines schaal Sets
- Virtuele netwerken (klassieke) - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
- VPN Gateway

## <a name="app-service-limitations"></a>App-servicebeperkingen

Wanneer u met de App Service-apps werkt, kunt u alleen een App serviceplan niet verplaatsen. Als u wilt verplaatsen App Service-apps, zijn uw opties:

- Het App-abonnement en alle andere resources uit de App-Service in die resourcegroep verplaatsen naar een nieuwe resourcegroep die nog niet App Service resources. Deze vereiste betekent dat u zelfs de App Service bronnen die niet zijn gekoppeld aan het abonnement dat App moet verplaatsen. 
- De apps verplaatsen naar een andere resource-groep, maar alle App Service-abonnementen behouden in de oorspronkelijke resourcegroep.

Als uw oorspronkelijke resourcegroep ook een resource van toepassing inzichten bevat, kunt u deze resource niet verplaatsen, omdat de toepassing inzichten de verplaatsing niet momenteel inschakelen. Als u de resource van toepassing inzichten opnemen wanneer u App Service apps verplaatst, wordt de hele verplaatsing mislukt. De toepassing inzichten en App serviceplan hoeft echter niet te bevinden zich in dezelfde resourcegroep als de app voor de app naar behoren.

Stel dat uw resourcegroep bevat:

- **Web-a** die is gekoppeld aan **een abonnement** en **app-inzichten-a**
- **web-b** die gekoppeld aan het **abonnement-b** en **app-inzichten-b is**

Uw opties zijn:

- Verplaatsen **web-a**, **plan-a**, **web-b**en **plan-b**
- Verplaatsen **web-a** en **web-b**
- Verplaatsen **web-a**
- **Web-b** verplaatsen

Alle andere combinaties gebruikmaakt van op een resourcetype die niet verplaatsen (toepassing inzichten) verplaatsen of verlaten achter een resourcetype die achter kan niet worden gelaten bij het verplaatsen van een App Service-abonnement (een type resource App Service).

Als uw web-app bevindt zich in een andere resourcegroep dan de App Service-abonnement, maar u wilt verplaatsen op een nieuwe resourcegroep, moet u de verplaatsen in twee stappen uitvoeren. Bijvoorbeeld:

- **Web-a** bevindt zich in de **web-groep**
- **plan-a** bevindt zich in de **abonnement-groep**
- Gewenste **web-a** en **plan-a** zich bevinden binnen **gecombineerd-groep**

U doet dit verplaatsen door bewerkingen twee afzonderlijke verplaatsen in de volgende volgorde:

1. Verplaats de **web-a** aan **abonnement-groep**
2. Verplaatsen **web-a** en **plan-a** aan **gecombineerd-groep**.

U kunt een App-Service-certificaat verplaatsen naar een nieuwe resourcegroep abonnement zonder problemen. Als uw web-app een SSL-certificaat dat u extern hebt gekocht en geüpload naar de app bevat, moet u het certificaat verwijderen voordat u verdergaat de web-app. U kunt bijvoorbeeld de volgende stappen uitvoeren:

1. De geüploade certificaat verwijderen uit de WebApp
2. Verplaats de web-app
3. Het certificaat uploaden naar de WebApp

## <a name="recovery-services-limitations"></a>Herstel Services beperkingen

Verplaats is niet ingeschakeld voor de opslag, netwerk, of resources die worden gebruikt voor het instellen van herstel met Azure Site herstel berekenen. 

Stel dat u hebt ingesteld met herhaling lokale computers met een opslag-account (Storage1) en wilt de beveiligde machine bovenkomen na failover naar Azure als een virtuele machine (VM1) die zijn bijgevoegd bij een virtueel netwerk (Network1). U kunt een van deze Azure resources - Storage1, VM1 en Network1 - niet verplaatsen via resourcegroepen binnen hetzelfde abonnement of via abonnementen.

## <a name="classic-deployment-limitations"></a>Klassieke implementatie beperkingen

De opties voor het verplaatsen van resources die zijn geïmplementeerd via het klassieke objectmodel zijn afhankelijk van of u de resources in een abonnement of aan een nieuw abonnement wilt verplaatsen. 

### <a name="same-subscription"></a>Hetzelfde abonnement

Wanneer u kunt resources uit één resourcegroep verplaatst naar een andere resourcegroep binnen hetzelfde abonnement, gelden de volgende beperkingen:

- Virtuele netwerken (klassieke) kunnen niet worden verplaatst.
- Virtuele machines (klassieke) moet worden verplaatst met de cloudservice. 
- Cloudservice kan alleen worden verplaatst wanneer de verplaatsing alle virtuele machines bevat.
- Slechts één cloudservice kan tegelijkertijd worden verplaatst.
- Slechts één opslag-account (klassieke) kan tegelijkertijd worden verplaatst.
- Opslag-account (klassieke) kan niet worden verplaatst in dezelfde bewerking met een virtuele machine of een cloudservice.

Klassieke om bronnen te verplaatsen naar een nieuwe resourcegroep binnen hetzelfde abonnement, de bewerkingen van de standaard verplaatsen naar de [beheerportal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli)of [REST API](#use-rest-api)te gebruiken. U bewerkingen gebruiken terwijl u gebruikt voor het verplaatsen van resourcemanager resources.

### <a name="new-subscription"></a>Nieuw abonnement

Wanneer u resources verplaatst naar een nieuw abonnement, gelden de volgende beperkingen:

- Alle klassieke resources in het abonnement dat moeten worden verplaatst in dezelfde bewerking.
- Het doel-abonnement mag geen andere klassieke bronnen.
- Onderweg kan alleen worden opgevraagd via een afzonderlijke REST API voor klassieke verplaatst. De standaard resourcemanager verplaatsen opdrachten werken niet bij het verplaatsen van klassieke resources aan een nieuw abonnement.

Klassieke om bronnen te verplaatsen naar een nieuw abonnement, moet u andere bewerkingen die specifiek voor klassieke resources zijn. Voer de volgende stappen klassieke om bronnen te verplaatsen naar een nieuw abonnement.

1. Controleer als het bron-abonnement aan het verplaatsen van een cross-abonnement deelnemen kunt. Gebruik de volgende bewerking:

         POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
    
     Klik in het hoofdgedeelte van de aanvraag, zijn:

         {
           "role": "source"
         }

     Het antwoord voor de bewerking gegevensvalidatie heeft de volgende indeling:

         {
           "status": "{status}",
           "reasons": [
             "reason1",
             "reason2"
           ]
         }

2. Controleer als de waarde van doelabonnement aan het verplaatsen van een cross-abonnement deelnemen kunt. Gebruik de volgende bewerking:

         POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01

     Klik in het hoofdgedeelte van de aanvraag, zijn:

         {
           "role": "target"
         }

     Het antwoord zich in dezelfde opmaak als het valideren van het bron-abonnementen.

3. Als beide abonnementen gevalideerd worden, verplaatst u alle klassieke resources uit een abonnement op een ander abonnement met de volgende bewerking:

         POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01

    Klik in het hoofdgedeelte van de aanvraag, zijn:

         {
           "target": "/subscriptions/{target-subscription-id}"
         }

De bewerking kan uitvoeren voor enkele minuten. 

## <a name="use-portal"></a>Portal gebruiken

Selecteer de resourcegroep met deze bronnen om bronnen te verplaatsen naar een nieuwe resourcegroep in het **hetzelfde abonnement**, en selecteer vervolgens de knop **verplaatsen** .

![resources verplaatsen](./media/resource-group-move-resources/edit-rg-icon.png)

Of, om bronnen te verplaatsen naar een **Nieuw abonnement**, selecteer de resourcegroep met deze bronnen en selecteer vervolgens het pictogram edit abonnement.

![resources verplaatsen](./media/resource-group-move-resources/change-subscription.png)

Selecteer de resources om te gaan en de resourcegroep bestemming. Bevestig dat u moet scripts voor deze resources bijwerken en klik op **OK**. Als u het pictogram van het abonnement bewerken in de vorige stap hebt geselecteerd, moet u ook het doelabonnement.

![doel selecteren](./media/resource-group-move-resources/select-destination.png)

In **meldingen**ziet u dat de verplaatsing wordt uitgevoerd.

![verplaatsen status weergeven](./media/resource-group-move-resources/show-status.png)

Wanneer deze is voltooid, wordt u gewaarschuwd van het resultaat.

![verplaatsen resultaat weergeven](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>PowerShell gebruiken

Bestaande om bronnen te verplaatsen naar een ander resourcegroep of -abonnement, gebruikt u de opdracht **Verplaatsen-AzureRmResource** .

Het eerste voorbeeld ziet hoe u een bron naar een nieuwe resourcegroep verplaatsen.

    $resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId

Het tweede voorbeeld ziet hoe u meerdere resources verplaatsen naar een nieuwe resourcegroep.

    $webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
    $plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId

Als u wilt verplaatsen naar een nieuw abonnement, moet u een waarde voor de parameter **DestinationSubscriptionId** bevatten.

U wordt gevraagd om te bevestigen dat u wilt verplaatsen van de opgegeven resources.

    Confirm
    Are you sure you want to move these resources to the resource group
    '/subscriptions/{guid}/resourceGroups/newRG' the resources:

    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

## <a name="use-azure-cli"></a>Azure CLI gebruiken

Bestaande om bronnen te verplaatsen naar een ander resourcegroep of -abonnement, gebruikt u de opdracht **azure resource verplaatsen** . U moet de resource-id's van de bronnen om te verplaatsen. U kunt uw resource-id's met de volgende opdracht uit:

    azure resource list -g sourceGroup --json

Welke geeft als resultaat de volgende indeling:

    [
      {
        "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
        "name": "storagedemo",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "southcentralus",
        "tags": {},
        "kind": "Storage",
        "sku": {
          "name": "Standard_RAGRS",
          "tier": "Standard"
        }
      }
    ]

Het volgende voorbeeld ziet u hoe u een account opslag verplaatsen naar een nieuwe resourcegroep. Met de parameter **-i** , vindt u dat een lijst door komma's gescheiden van de resource-id bevindt zich verplaatsen.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
    
U wordt gevraagd om te bevestigen dat u wilt verplaatsen van de opgegeven resource.

## <a name="use-rest-api"></a>REST API gebruiken

Bestaande om bronnen te verplaatsen naar een ander resourcegroep of -abonnement, voert u het:

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

In het hoofdgedeelte van de aanvraag geeft u de doelgroep voor de resource en de resources te gaan. Zie [resources verplaatsen](https://msdn.microsoft.com/library/azure/mt218710.aspx)voor meer informatie over de verplaatsing van de REST.


## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over PowerShell-cmdlets voor het beheren van uw abonnement, [Azure PowerShell gebruiken met bronbeheer](powershell-azure-resource-manager.md).
- Meer informatie over Azure CLI opdrachten voor het beheren van uw abonnement, Zie [gebruik van de Azure CLI met bronbeheer](xplat-cli-azure-resource-manager.md).
- Zie voor meer informatie over de functies van de portal voor het beheren van uw abonnement, [met behulp van de Azure portal voor het beheren van resources](./azure-portal/resource-group-portal.md).
- Meer informatie over het toepassen van een logische organisatie op uw resources, raadpleegt u [werken met tags om u te organiseren van uw resources](resource-group-using-tags.md).
