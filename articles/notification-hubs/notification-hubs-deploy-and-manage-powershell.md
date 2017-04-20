<properties 
    pageTitle="Implementeren en beheren melding Hubs via PowerShell" 
    description="Het maken en beheren van melding Hubs via PowerShell voor automatisering" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Implementeren en beheren melding Hubs via PowerShell

##<a name="overview"></a>Overzicht

In dit artikel leest u het gebruik van maken en beheren Azure melding Hubs via PowerShell. De volgende algemene automatiseringstaken worden weergegeven in dit onderwerp.

+ Een melding Hub maken
+ Referenties instellen

Als u ook een nieuwe service bus naamruimte voor uw hubs melding maken moet, raadpleegt u [Service Bus beheren met PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Meldingen Hubs beheren wordt niet ondersteund door de wordt geleverd bij Azure PowerShell-cmdlets. De beste manier van PowerShell is om te verwijzen naar de vergadering Microsoft.Azure.NotificationHubs.dll. De vergadering is verdeeld met het [pakket Microsoft Azure melding Hubs NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).


## <a name="prerequisites"></a>Vereisten voor

Voordat u in dit artikel begint, hebt u het volgende:

- Een Azure-abonnement. Azure is een platform op basis van abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [Koopt], [Lid biedt]of [Gratis proefversie].

- Een computer met Azure PowerShell. Zie voor instructies [installeren en configureren van Azure PowerShell].

- Een algemene begrip van de PowerShell-scripts, NuGet-pakketten en .NET Framework.


## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Een verwijzing naar de vergadering .NET voor Service Bus inclusief

Beheer van Azure melding Hubs nog niet opgenomen in de PowerShell-cmdlets in Azure PowerShell. Als u wilt inrichten melding hubs, kunt u de .NET-client die beschikbaar zijn in het [pakket Microsoft Azure melding Hubs NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Controleer eerst of dat uw script de **Microsoft.Azure.NotificationHubs.dll** assemblies, waardoor is geïnstalleerd als een pakket NuGet in een project Visual Studio kunt vinden. Het script kunt u deze stappen uitvoeren om te worden flexibele:

1. Bepaalt het pad waarop deze is geactiveerd.
2. Het pad worden doorlopen, totdat er een map met de naam wordt gevonden `packages`. Deze map wordt gemaakt tijdens de installatie van NuGet-pakketten voor Visual Studio-projecten.
3. Recursief zoekopdrachten de `packages` map voor een samenvoeging met de naam **Microsoft.Azure.NotificationHubs.dll**.
4. Verwijst naar de vergadering zodat de typen beschikbaar voor later gebruik zijn.

Hier ziet u hoe deze stappen zijn geïmplementeerd in een PowerShell-script:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>De klas NamespaceManager maken

Als u wilt inrichten melding Hubs, maakt u een exemplaar van de klas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) uit de SDK. 

U kunt de cmdlet [Get-AzureSBAuthorizationRule] is inbegrepen in Azure PowerShell gebruiken om op te halen van een autorisatieregel voor het die wordt gebruikt voor een verbindingsreeks. Slaan we een verwijzing naar de `NamespaceManager` exemplaar de `$NamespaceManager` variabele. We gebruiken `$NamespaceManager` voor het inrichten van de hub van een melding.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Een nieuwe melding Hub inrichting 

Als u wilt een nieuwe melding hub inrichten, gebruikt u de [.NET-API voor melding Hubs].

In dit gedeelte van het script moet u vier lokale variabelen instellen. 

1. `$Namespace`: Stel dit op de naam van de naamruimte waar u wilt maken van een melding-hub.
2. `$Path`: Dit pad naar de naam van de nieuwe melding hub instellen.  Bijvoorbeeld: "MyHub".    
3. `$WnsPackageSid`: Dit aan het pakket beveiligings-id voor u ingesteld Windows-App vanuit het [Windows ontwikkelaar Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Dit naar de geheime sleutel voor u ingesteld Windows-App vanuit het [Windows ontwikkelaar Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Deze variabelen worden gebruikt om verbinding maken met uw naamruimte en een nieuwe melding Hub geconfigureerd voor het afhandelen van meldingen van Windows melding Services (WNS) met WNS referenties voor een Windows-App te maken. Voor informatie over het verkrijgen van het pakket beveiligings-id en geheime sleutel ziet, is de zelfstudie [Aan de slag met melding Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) . 

+ Het script-fragment gebruikt de `NamespaceManager` object om te controleren om te zien als de Hub van de melding die wordt aangeduid met `$Path` bestaat.

+ Als dit niet bestaat nog, maakt het script een `NotificationHubDescription` met WNS referenties en op te geven die u wilt de `NamespaceManager` class `CreateNotificationHub` methode.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Aanvullende informatie

- [Service Bus met PowerShell beheren](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Het maken van Service Bus wachtrijen, onderwerpen en abonnementen met een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Het maken van een Service Bus Namespace en een gebeurtenis Hub met een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Sommige kant-en-scripts zijn ook beschikbaar voor downloaden:
- [Service Bus PowerShell-Scripts](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[Aankoop-opties]: http://azure.microsoft.com/pricing/purchase-options/
[Lid aanbiedingen]: http://azure.microsoft.com/pricing/member-offers/
[Gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/
[Installeren en configureren van Azure PowerShell]: ../powershell-install-configure.md
[.NET-API voor melding Hubs]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 
