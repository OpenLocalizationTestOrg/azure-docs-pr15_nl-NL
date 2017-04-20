<properties
    pageTitle="Beheren Service Bus met PowerShell | Microsoft Azure"
    description="Service Bus met PowerShell-scripts beheren"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>Service Bus met PowerShell beheren

## <a name="overview"></a>Overzicht

Microsoft Azure PowerShell is een uitvoeren van scripts-omgeving waarin u gebruiken kunt om te bepalen en automatiseren de implementatie en het beheer van uw werkbelasting in Azure wordt aangegeven. In dit artikel wordt beschreven hoe PowerShell gebruiken voor het inrichten van en beheren van Service Bus entiteiten zoals naamruimten, wachtrijen en gebeurtenis Hubs met een lokale Azure PowerShell-console.

## <a name="prerequisites"></a>Vereisten voor

Voordat u dit artikel, moet u de volgende vereisten hebben:

- Een Azure-abonnement. Azure is een platform op basis van abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [Koopt][], [Lid biedt][]of [Gratis proefversie][].

- Een computer met Azure PowerShell. Zie voor instructies [installeren en configureren van Azure PowerShell][].

- Een algemene begrip van de PowerShell-scripts, NuGet-pakketten en .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Een verwijzing naar de vergadering .NET voor Service Bus inclusief

Een beperkt aantal PowerShell-cmdlets zijn beschikbaar voor het beheren van Service Bus. Als u wilt inrichten entiteiten die niet worden weergegeven via de bestaande cmdlets, kunt u de .NET-client voor Service Bus in de [Service Bus NuGet-pakket][].

Controleer eerst of dat het script de **Microsoft.ServiceBus.dll** assemblies, waardoor is geïnstalleerd met het pakket NuGet kunt vinden. Het script kunt u deze stappen uitvoeren om te worden flexibele:

1. Bepaalt het pad waarop deze is geactiveerd.
2. Het pad worden doorlopen, totdat er een map met de naam wordt gevonden `packages`. Deze map wordt gemaakt tijdens de installatie van NuGet-pakketten.
3. Recursief zoekopdrachten de `packages` map voor een samenvoeging met de naam **Microsoft.ServiceBus.dll**.
4. Verwijst naar de vergadering zodat de typen beschikbaar voor later gebruik zijn.

Hier ziet u hoe deze stappen zijn geïmplementeerd in een PowerShell-script:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Een Service Bus naamruimte inrichten

Twee PowerShell-cmdlets ondersteunen Service Bus naamruimte bewerkingen. In plaats van de .NET SDK API's, kunt u [Get-AzureSBNamespace][] en [Nieuw-AzureSBNamespace][].

In dit voorbeeld wordt een paar lokale variabelen in het script; `$Namespace` and `$Location`.

- `$Namespace`is de naam van de Service Bus naamruimte die we wilt werken.
- `$Location`Geeft aan wat het datacenter waarin het script de naamruimte bepalingen.
- `$CurrentNamespace`slaat de naamruimte van de verwijzing die het script haalt (of wordt gemaakt).

In een werkelijke script `$Namespace` en `$Location` kan worden gebruikt als parameters.

In dit gedeelte van het script kunt u de volgende taken uitvoeren:

1. Hiermee kunt u proberen om op te halen een naamruimte Service Bus met de opgegeven naam.
2. Als de naamruimte wordt gevonden, wordt deze rapporten wat is gevonden.
3. Als de naamruimte niet wordt gevonden, wordt gemaakt van de naamruimte en haalt u de zojuist gemaakte naamruimte.

    ```
    $Namespace = "MyServiceBusNS"
    $Location = "West US"
    
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
    
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Als u wilt inrichten van andere entiteiten Service Bus, een exemplaar van de klas [NamespaceManager][] te maken uit de SDK.
Een autorisatieregel voor het die wordt gebruikt voor een verbindingsreeks ophalen kunt u de cmdlet [Get-AzureSBAuthorizationRule][] . Slaan we een verwijzing naar de `NamespaceManager` exemplaar de `$NamespaceManager` variabele. We gebruiken `$NamespaceManager` verderop in het script voor het inrichten van andere entiteiten.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Andere entiteiten Service Bus inrichting

Om te kunnen inrichten van andere entiteiten, zoals wachtrijen, onderwerpen en gebeurtenis Hubs, gebruikt u de [.NET-API voor Service Bus][]. In dit artikel is alleen gericht op gebeurtenis Hubs, maar de stappen voor andere entiteiten zijn identiek. Bovendien de meer gedetailleerde voorbeelden, waaronder andere entiteiten, aan het einde van dit artikel wordt verwezen.

In dit gedeelte van het script Hiermee maakt u vier meer lokale variabelen. Deze variabelen worden gebruikt voor het starten van een `EventHubDescription` object. Het script kunt u de volgende taken uitvoeren:

1. Met de `NamespaceManager` object en controleert u als de Hub gebeurtenis die wordt aangeduid met `$Path` bestaat.
2. Als deze niet bestaat, maakt u een `EventHubDescription` en geven die u wilt de `NamespaceManager` class `CreateEventHubIfNotExists` methode.
3. Nadat u hebt vastgesteld dat de Hub gebeurtenis beschikbaar is, maakt u een consumenten groep met `ConsumerGroupDescription` en `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }
        
    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Een naamruimte migreren naar een ander abonnement van Azure

De volgende reeks opdrachten verplaatst een naamruimte van Azure-abonnementen naar de andere. Als u wilt deze bewerking uitvoeren, de naamruimte al moet actief zijn en de gebruiker de PowerShell-opdrachten moet een beheerder op zowel de bronsite en doelsites abonnementen.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een basisoverzicht opgegeven voor het inrichten van Service Bus entiteiten via PowerShell. Iets dat u kunt doen met de .NET-client-bibliotheken, u kunt ook doen in een PowerShell-script.

Er zijn meer gedetailleerde voorbeelden beschikbaar op deze blogberichten:

- [Het maken van Service Bus wachtrijen, onderwerpen en abonnementen met een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Het maken van een Service Bus Namespace en een gebeurtenis Hub met een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Sommige kant-en-scripts zijn ook beschikbaar voor downloaden:
- [Service Bus PowerShell-Scripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Aankoop-opties]: http://azure.microsoft.com/pricing/purchase-options/
[Lid aanbiedingen]: http://azure.microsoft.com/pricing/member-offers/
[Gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/
[Installeren en configureren van Azure PowerShell]: ../powershell-install-configure.md
[Service Bus NuGet pakket]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Nieuwe AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.NET-API voor Service Bus]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
