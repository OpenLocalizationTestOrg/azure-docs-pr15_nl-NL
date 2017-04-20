<properties 
    pageTitle="Het beheren van Azure zoeken via Powershell-scripts | Microsoft Azure | De zoekservice gehoste cloud" 
    description="De zoekservice van Azure met PowerShell-scripts beheren. Maken of een Azure-zoekservice bijwerken en beheren van Azure zoeken beheerder toetsen" 
    services="search" 
    documentationCenter="" 
    authors="seansaleh" 
    manager="mblythe" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="powershell" 
    ms.date="08/15/2016" 
    ms.author="seasa"/>

# <a name="manage-your-azure-search-service-with-powershell"></a>De zoekservice van Azure met PowerShell beheren
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST API](search-get-started-management-api.md)

In dit onderwerp worden de PowerShell-opdrachten om uit te voeren veel van de beheertaken voor zoeken van Azure-services. We doorlopen maken van een search-service, schalen en beheren van de API toetsen.
Deze opdrachten parallelle de opties beschikbaar in de [Azure zoeken Management REST API](http://msdn.microsoft.com/library/dn832684.aspx).

## <a name="prerequisites"></a>Vereisten voor
 
- U moet Azure PowerShell 1,0 of hoger hebben. Zie voor instructies [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).
- U moet zijn aangemeld bij uw Azure-abonnement in PowerShell zoals hieronder beschreven.

U moet eerst, meld u aan bij Azure met deze opdracht:

    Login-AzureRmAccount

Geef het e-mailadres van uw Azure-account en het wachtwoord in het dialoogvenster Microsoft Azure aanmelden.

U kunt ook [login niet-interactief met een service principal](../resource-group-authenticate-service-principal.md).

Als u meerdere Azure abonnementen hebt, moet u uw Azure abonnement instellen. Een overzicht van uw huidige abonnement, moet u deze opdracht uitvoeren.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Als u wilt opgeven van het abonnement, moet u de volgende opdracht uitvoeren. Klik in het volgende voorbeeld wordt de naam van het abonnement is `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Opdrachten om u te helpen aan de slag

    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1
    
    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
    
    # View your resource
    $resource
    
    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key
    
    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
        
    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource
    
    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource
    
## <a name="next-steps"></a>Volgende stappen
    
Nu dat uw service wordt gemaakt, kunt u de volgende stappen uitvoeren: een [index](search-what-is-an-index.md), [query een index](search-query-overview.md)maken en ten slotte kunt maken en beheren van uw eigen search-servicetoepassing met Azure zoeken.

- [Een index Azure zoeken maken in de portal van Azure](search-create-index-portal.md)

- [Query een Azure zoekindex Search Explorer gebruiken in de portal van Azure](search-explorer.md)

- [Een indexering om gegevens te laden van andere services instellen](search-indexer-overview.md)

- [Het gebruik van Azure zoeken in .NET](search-howto-dotnet-sdk.md)

- [Uw zoekopdracht Azure-verkeer analyseren](search-traffic-analytics.md)
