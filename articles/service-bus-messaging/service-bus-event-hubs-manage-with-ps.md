<properties
    pageTitle="PowerShell gebruiken voor het beheren van Service Bus en gebeurtenis Hubs resources | Microsoft Azure"
    description="PowerShell gebruiken voor het maken en beheren van Service Bus en gebeurtenis Hubs resources"
    services="service-bus,event-hubs"
    documentationCenter=".NET"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="use-powershell-to-manage-service-bus-and-event-hubs-resources"></a>PowerShell gebruiken voor het beheren van Service Bus en gebeurtenis Hubs resources

Microsoft Azure PowerShell is een uitvoeren van scripts-omgeving waarin u kunt de implementatie en het beheer van Azure services te automatiseren. In dit artikel wordt beschreven hoe PowerShell gebruiken voor het inrichten van en beheren van Service Bus entiteiten zoals naamruimten, wachtrijen en gebeurtenis Hubs met een lokale Azure PowerShell-console.

## <a name="prerequisites"></a>Vereisten voor

Voordat u begint, hebt u het volgende nodig:

- Een Azure-abonnement. Azure is een platform op basis van abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [kopen opties][], [lid biedt][]of [gratis-account][].

- Een computer met Azure PowerShell. Zie voor instructies [installeren en configureren van Azure PowerShell][].

- Een algemene begrip van de PowerShell-scripts, NuGet-pakketten en .NET Framework.

## <a name="include-a-reference-to-the-net-assembly-for-service-bus"></a>Een verwijzing naar de vergadering .NET voor Service Bus opnemen

Een beperkt aantal PowerShell-cmdlets zijn beschikbaar voor het beheren van Service Bus. Als u wilt inrichten entiteiten die niet worden weergegeven via de bestaande cmdlets, kunt u de .NET-client voor Service Bus uit binnen PowerShell door te verwijzen naar de [Service Bus NuGet-pakket].

Controleer eerst of dat het script de **Microsoft.ServiceBus.dll** assemblies, waardoor is geïnstalleerd met het pakket NuGet kunt vinden. Het script kunt u deze stappen uitvoeren om te worden flexibele:

1. Bepaalt het pad van waaruit deze is geactiveerd.
2. Het pad worden doorlopen, totdat er een map met de naam wordt gevonden `packages`. Deze map wordt gemaakt tijdens de installatie van NuGet-pakketten.
3. Recursief zoekopdrachten de `packages` map voor een samenvoeging met de naam **Microsoft.ServiceBus.dll**.
4. Verwijst naar de vergadering zodat de typen beschikbaar voor later gebruik zijn.

Hier ziet u hoe deze stappen zijn geïmplementeerd in een PowerShell-script:

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}

```

## <a name="provision-a-service-bus-namespace"></a>Een Service Bus naamruimte inrichten

Tijdens het werken met Service Bus naamruimten, zijn er twee cmdlets die u in plaats van de .NET SDK gebruiken kunt: [Get-AzureSBNamespace][] en [Nieuw-AzureSBNamespace][].

In dit voorbeeld wordt een paar lokale variabelen in het script; `$Namespace` and `$Location`.

- `$Namespace`is de naam het van de Service Bus naamruimte die we wilt werken.
- `$Location`Geeft aan wat het datacenter waarin wordt we de naamruimte inrichten.
- `$CurrentNamespace`slaat de naamruimte van de verwijzing die we ophalen (of maken).

In een werkelijke script `$Namespace` en `$Location` kan worden gebruikt als parameters.

In dit gedeelte van het script gebeurt het volgende:

1. Hiermee kunt u proberen om op te halen een naamruimte Service Bus met de opgegeven naam.
2. Als de naamruimte wordt gevonden, wordt deze rapporten wat is gevonden.
3. Als de naamruimte niet wordt gevonden, wordt gemaakt van de naamruimte en haalt u de zojuist gemaakte naamruimte.

    ``` powershell

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
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
Als u wilt inrichten van andere entiteiten Service Bus, maakt u een exemplaar van de `NamespaceManager` object uit de SDK. Een autorisatieregel voor het die wordt gebruikt voor een verbindingsreeks ophalen kunt u de cmdlet [Get-AzureSBAuthorizationRule][] . In dit voorbeeld bevat een verwijzing naar de `NamespaceManager` exemplaar de `$NamespaceManager` variabele. Het script later gebruikt `$NamespaceManager` voor het inrichten van andere entiteiten.

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## <a name="provisioning-other-service-bus-entities"></a>Andere entiteiten Service Bus inrichting

Als u wilt inrichten van andere entiteiten, zoals wachtrijen, onderwerpen en gebeurtenis Hubs, kunt u de [.NET-API voor Service Bus][]. Meer gedetailleerde voorbeelden, waaronder andere entiteiten, wordt aan het einde van dit artikel verwezen.

### <a name="create-an-event-hub"></a>De Hub van een gebeurtenis maken

In dit gedeelte van het script Hiermee maakt u vier meer lokale variabelen. Deze variabelen worden gebruikt voor het starten van een `EventHubDescription` object. Het script gebeurt het volgende:

1. Met de `NamespaceManager` object en controleert u als de Hub gebeurtenis die wordt aangeduid met `$Path` bestaat.
2. Als deze niet bestaat, maakt u een `EventHubDescription` en geven die u wilt de `NamespaceManager` class `CreateEventHubIfNotExists` methode.
3. Nadat u hebt vastgesteld dat de Hub gebeurtenis beschikbaar is, maakt u een consumenten groep met `ConsumerGroupDescription` en `NamespaceManager`.

    ``` powershell

    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"

    # Check if the Event Hub already exists
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

### <a name="create-a-queue"></a>Een wachtrij maken

Als u wilt een wachtrij of onderwerp maakt, moet u een controle van de naamruimte zoals in de vorige sectie uitvoeren.

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### <a name="create-a-topic"></a>Een onderwerp maken

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## <a name="next-steps"></a>Volgende stappen

In dit artikel voorzien u een eenvoudige werkstroom voor het inrichten van Service Bus entiteiten via PowerShell. Hoewel er een beperkt aantal PowerShell-cmdlets beschikbaar zijn voor het beheren van Service Bus kunt messaging entiteiten, door te verwijzen naar de vergadering Microsoft.ServiceBus.dll vrijwel elke willekeurige bewerking u uitvoeren met de .NET-client-bibliotheken die u kunt ook in een PowerShell-script uitvoeren.

Er zijn meer gedetailleerde voorbeelden beschikbaar in deze blogberichten:

- [Het maken van Service Bus wachtrijen, onderwerpen en abonnementen met een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Het maken van een Service Bus Namespace en een gebeurtenis Hub met een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Sommige kant-en-scripts zijn ook beschikbaar voor downloaden:

- [Service Bus PowerShell-Scripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[aankoop-opties]: http://azure.microsoft.com/pricing/purchase-options/
[lid biedt]: http://azure.microsoft.com/pricing/member-offers/
[gratis-account]: http://azure.microsoft.com/pricing/free-trial/
[Service Bus NuGet pakket]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Nieuwe AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.NET-API voor Service Bus]: https://msdn.microsoft.com/en-us/library/azure/mt419900.aspx
[Installeren en configureren van Azure PowerShell]: ../powershell-install-configure.md
