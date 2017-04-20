<properties
    pageTitle="Uw app verbinden met uw virtueel netwerk via PowerShell"
    description="Instructies over het verbinding maken met en werken met virtuele netwerken via PowerShell"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Uw app verbinden met uw virtueel netwerk via PowerShell #

## <a name="overview"></a>Overzicht ##

U kunt in Azure App-Service, uw app (web, mobile of API) verbinden met een Azure virtuele netwerk (VNet) in uw abonnement. Deze functie heet VNet-integratie. De functie VNet integratie moet niet verwarren met de functie App-omgeving, zodat u een exemplaar van Azure App-Service in uw virtuele netwerk uitvoeren.

De functie Integratie met VNet heeft een gebruikersinterface (UI) in de nieuwe portal die u integreren met virtuele netwerken die zijn geïmplementeerd kunt met behulp van het implementatiemodel klassieke of het implementatiemodel resourcemanager Azure. Als u weten over de functie wilt, raadpleegt u [uw app met een Azure virtuele netwerk integreren](web-sites-integrate-with-vnet.md).

In dit artikel is niet over het gebruik van de gebruikersinterface, maar liever over het inschakelen van integratie via PowerShell. Omdat de opdrachten voor elk implementatiemodel verschillend zijn, heeft dit artikel een sectie voor elk implementatiemodel.  

Voordat u met in dit artikel verdergaat, moet u zorgen dat u hebt:

- De meest recente Azure PowerShell SDK is geïnstalleerd. Hiermee kunt u met het installatieprogramma van de Web-Platform installeren.
- Een app in Azure App-Service uitgevoerd in een standaard- of Premium SKU.

## <a name="classic-virtual-networks"></a>Klassieke virtuele netwerken ##

In deze sectie wordt uitgelegd drie taken voor virtuele netwerken die gebruikmaken van het implementatiemodel klassieke:

1. Uw app verbinden met een reeds bestaande virtuele netwerk dat een gateway heeft en is geconfigureerd voor punt-naar-site connectivity.
1. Uw gegevens van de integratie van virtueel netwerk voor uw app bijwerken.
1. Uw app uw virtuele netwerk verbreken.

### <a name="connect-an-app-to-a-classic-vnet"></a>Een app verbinden met een klassieke VNet ###

Als u wilt een app verbinden door een virtueel netwerk, volgt u deze drie stappen:

1. Naar de web-app declareren dat deze wordt worden deelnemen aan een bepaald virtueel netwerk. De app genereert een certificaat dat bij het virtuele netwerk wordt verleend voor punt-naar-site connectivity.
1. Het certificaat van web-app uploaden naar het virtuele netwerk en vervolgens het punt-naar-site VPN pakket URI worden opgehaald.
1. De web-app virtueel netwerkverbinding met het punt-naar-site-pakket URI bijwerken.

De eerste en de derde stappen worden opgenomen in scripts, maar de tweede stap is een eenmalige, handmatige actie via de portal of toegang tot **ZETTEN** of **PATCH** acties uitvoeren op het eindpunt van de resourcemanager Azure virtuele netwerk vereist. Contact opnemen met Azure-ondersteuning als dit is ingeschakeld. Voordat u begint, zorg dat er een klassieke virtuele netwerk met punt-naar-site connectivity is al ingeschakeld en een gateway geïmplementeerd. Als u wilt maken van de gateway en punt-naar-site-connectiviteit inschakelen, moet u de portal gebruiken, zoals beschreven op het [maken van een gateway VPN][createvpngateway].

Het klassieke virtuele netwerk moet hetzelfde abonnement als uw App serviceplan waarin de app die u met integreert.

##### <a name="set-up-azure-powershell-sdk"></a>Azure PowerShell SDK instellen #####

Open een PowerShell-venster en uw Azure-account en een abonnement instellen met behulp van:

    Login-AzureRmAccount

Deze opdracht wordt geopend een prompt om uw Azure referenties te verkrijgen. Nadat u zich hebt aangemeld, gebruikt u een van de volgende opdrachten voor het selecteren van het abonnement waaraan u wilt gebruiken. Zorg ervoor dat u het abonnement dat uw virtueel netwerk en App serviceplan zich bevinden gebruikt in.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

of

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Variabelen die in dit artikel worden gebruikt #####

Om te vereenvoudigen opdrachten, wordt er een **$Configuration** PowerShell variabele met de specifieke configuratie instellen.

Stel een variabele als volgt in PowerShell met de volgende parameters:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

De locatie van de app moet de locatie zonder spaties. West Amerikaans is bijvoorbeeld westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

Het volgende item is waar het certificaat moet worden toegevoegd. Een schrijfbare pad naar uw lokale computer moet worden. Zorg ervoor dat .cer aan het einde.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Als u wilt zien wat u instellen, typt u **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

De rest van deze sectie wordt ervan uitgegaan dat er een variabele gemaakt als alleen beschreven.

##### <a name="declare-the-virtual-network-to-the-app"></a>Het virtuele netwerk bij de app declareren #####

Gebruik de volgende opdracht uit te geven van de app dat deze dit bepaalde virtuele netwerk gaat gebruiken. Hierdoor wordt de app te genereren nodig certificaten:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Als deze opdracht is geslaagd, moet **$vnet** een variabele **Eigenschappen** in deze hebben. De **Eigenschappen** variabele mag zowel de certificaatvingerafdruk van een en de certificaatgegevens bevatten.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Het certificaat van web-app uploaden naar het virtuele netwerk #####

Een handleiding eenmalige stap is vereist voor elke combinatie van de virtuele netwerk en het abonnement. Dat wil zeggen als u verbinding apps abonnement A Virtual Network A maakt, moet u moet deze stap slechts eenmaal ongeacht hoeveel apps u configureren. Als u een nieuwe app aan een ander virtuele netwerk toevoegt, moet u als volgt opnieuw. De reden hiervoor is dat een reeks certificaten is gegenereerd op het niveau van een abonnement in Azure App-Service, en de set wordt eenmaal voor elke virtuele netwerk dat de apps maakt verbinding met.

De certificaten wordt al zijn ingesteld als u deze stappen hebt uitgevoerd of als u geïntegreerd met hetzelfde virtuele netwerk met behulp van de portal.

De eerste stap is om de .cer-bestand te genereren. De tweede stap is het .cer-bestand uploaden naar uw virtuele netwerk. Als u wilt genereren het .cer-bestand met de API-gesprek in de eerdere stap, voer de volgende opdrachten.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Het certificaat wordt gevonden in de locatie die **$Configuration.GeneratedCertificatePath** bevat.

Als u wilt uploaden handmatig het certificaat, gebruikt u de [Azure-portal] [ azureportal] en **Blader virtuele netwerken (klassieke)** > **VPN-verbindingen** > **punt-naar-site** > **certificaten beheren**. U kunt uw certificaat te uploaden.

##### <a name="get-the-point-to-site-package"></a>Het pakket punt-naar-site ophalen #####

De volgende stap bij het instellen van een virtuele netwerkverbinding op een web-app is voor het ophalen van het pakket punt-naar-site en deze aan uw web-app te bieden.

De volgende sjabloon opslaan in een bestand met de naam van de GetNetworkPackageUri.json ergens op uw computer, bijvoorbeeld C:\Azure\Templates\GetNetworkPackageUri.json.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Invoerparameters instellen:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Bel het script:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


De variabele **$output. Outputs.packageUri** bevat nu het pakket URI voor uw web-app.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Het pakket punt-naar-site uploaden naar uw app #####

De laatste stap is om te creëren van de app met het pakket. Alleen uitvoeren de volgende opdracht:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Als u een bericht wordt gevraagd om te bevestigen dat u een bestaande resource wilt overschrijven, zorg er dan voor dat gebruik hiervan is toegestaan.

Nadat deze opdracht is gemaakt, moet uw app nu worden verbonden met het virtuele netwerk. Om te bevestigen success, gaat u naar uw app-console en typt u het volgende:

    SET WEBSITE_

Als er een omgevingsvariabele WEBSITE_VNETNAME met een waarde die overeenkomt met de naam van het doel virtuele netwerk genoemd, zijn alle configuraties geslaagd.

### <a name="update-classic-vnet-integration-information"></a>Klassieke VNet integratie informatie bijwerken ###

Als u wilt bijwerken of synchroniseren van uw gegevens, herhaalt u gewoon de stappen die u gevolgd wanneer u de integratie in eerste instantie hebt gemaakt. Deze stappen volgen:

1. Gegevens over uw definiëren.
1. Het virtuele netwerk bij de app declareren.
1. Het pakket punt-naar-site krijgen.
1. Het pakket punt-naar-site naar uw app uploaden.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Uw app verbreken door een klassieke VNet ###

Als u wilt verbreken de app, moet u de configuratiegegevens die tijdens de integratie van virtuele netwerken werd ingesteld. Deze informatie wordt gebruikt, is er vervolgens één opdracht uw app met uw netwerk virtuele verbreken.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Resourcemanager virtuele netwerken ##

Resourcemanager virtuele netwerken hebben Azure resourcemanager API's, waarin sommige processen wanneer ze worden vergeleken met klassieke virtuele netwerken vereenvoudigen. We hebben een script waarmee u kunt de volgende taken uitvoeren:

- Een virtueel netwerk resourcemanager maken en uw app integreren.
- Maken van een gateway, punt-naar-site-connectiviteit configureren in een bestaand resourcemanager virtuele netwerk en vervolgens uw app integreren met deze.
- Uw app integreren met een reeds bestaande resourcemanager virtuele netwerk met een gateway en punt-naar-site connectivity ingeschakeld.
- Uw app uw virtuele netwerk verbreken.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Resourcemanager VNet App Service integratie script ###

Kopieer de volgende script en sla deze op een bestand. Als u niet dat het script gebruiken wilt, altijd voor meer informatie over dit om te zien hoe dingen instellen met een virtueel resourcemanager-netwerk.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Sla een kopie van het script. In dit artikel, dit V2VnetAllinOne.ps1 wordt genoemd, maar u kunt een andere naam. Er zijn geen argumenten voor dit script. U hoeft uitvoeren deze. Het eerste wat u doet het script is gevraagd u aan te melden. Nadat u zich hebt aangemeld, wordt het script krijgt meer informatie over uw account en geeft als resultaat een lijst met abonnementen. Het verzoek om uw referenties niet meegerekend, de uitvoering van het eerste script ziet er zo uit:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Demo-abonnement (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) MS-Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Paarse Demo-abonnement (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Kies een optie: 3

    Account: ccompy@microsoft.com omgeving: AzureCloud abonnement: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d Tenant: 722278f-fef1-499f-91ab-2323d011db47

    Voer de resourcegroep van uw App: Voer de naam van uw App hcdemo-rg: v2vnetpowershell wat wilt u doen?

    1) Een nieuwe virtueel netwerk toevoegen aan een App
    2) Een bestaande virtueel netwerk toevoegen aan een App
    3) Een virtueel netwerk verwijdert uit een App

De rest van deze sectie wordt uitgelegd elk van deze drie opties.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Maak een VNet resourcemanager en integreren met deze ###

Als u wilt maken van een nieuw virtuele netwerk dat het resourcemanager implementatie-objectmodel gebruikt en integreren met uw app, selecteert u **1) een nieuwe Virtual Network toevoegen aan een App**. Hiermee wordt u gevraagd voor de naam van het virtuele netwerk. In mijn hoofdletters/kleine letters, zoals u in de volgende instellingen ziet gebruikt ik de naam van de v2pshell.

Het script biedt de informatie over het virtuele netwerk dat wordt gemaakt. Als ik wilt, kan ik een van de waarden wijzigen. In dit voorbeeld wordt uitgevoerd, maar ik heb gemaakt een virtueel netwerk met de volgende instellingen:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Als u wijzigen van de waarden wilt, typt u **j** en breng de wijzigingen. Wanneer u tevreden over de virtuele netwerkinstellingen bent, typt u **N** of drukt u op Enter wanneer u wordt gevraagd over het wijzigen van de instellingen. Vanaf dat moment tot voltooiing, het script kunt u nagaan wat dit aantal ' i's doen tot deze begint te maken van de gateway virtueel netwerk. Deze stap duurt maximaal een uur. Er is geen voortgangsindicator tijdens deze fase, maar het script zal u laten weten wanneer de gateway is gemaakt.

Wanneer het script is voltooid, is deze heet **voltooid**. U hebt nu een resourcemanager virtuele netwerk met de naam en de instellingen die u hebt geselecteerd. Deze nieuwe virtueel netwerk worden ook geïntegreerd met uw app.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Uw app integreren met een reeds bestaande resourcemanager VNet ###

Wanneer u met een reeds bestaande virtueel netwerk, integreren bent als u een virtueel netwerk van resourcemanager die geen gateway of punt-naar-site connectivity opgeeft, wordt het script ingesteld dat. Als de VNET al deze items instellen, wordt het script gaat rechtstreeks naar de app-integratie. Als u wilt dit proces, selecteert u **2) een bestaand virtuele netwerk toevoegen aan een App**.

Deze optie werkt alleen als u een bestaande virtuele netwerk voor resourcemanager, die is hetzelfde als de app-abonnement hebt. Nadat u de optie hebt geselecteerd, wordt weergegeven met een lijst met uw resourcemanager virtuele netwerken.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Kies een optie: 5

Het virtuele netwerk die u integreren wilt met selecteert. Als u al een gateway met punt-naar-site connectivity is ingeschakeld, wordt het script uw app gewoon integreren met uw virtuele netwerk. Als u een gateway niet hebt, moet u de gateway-subnet opgeven. Uw subnet, gateway, moet zich in uw adresruimte virtueel netwerk en deze kan niet in een ander subnet. Als u een virtueel netwerk zonder een gateway hebt en deze stap uitvoert, dingen die er als volgt uit:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

In dit voorbeeld, maar ik heb gemaakt een virtueel netwerkgateway waarvoor de volgende instellingen heeft:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Als u wijzigen op een van deze instellingen wilt, kunt u doen. Anders, druk op Enter en het script maakt u de gateway en uw app als bijlage toevoegen aan uw virtuele netwerk. De aanmaaktijd van een gateway is echter nog steeds een uur, dus controleer of u die Houd er rekening mee. Als alles is voltooid, heet het script **voltooid**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Uw app verbreken door een Resource Manager VNet ###

Uw app verbreken door uw virtuele netwerk niet noteren de gateway of punt-naar-site connectivity uitschakelen. U, nadat alle gebruikt deze voor iets anders. Deze worden ook niet verbroken deze vanuit een andere apps behalve het gedeelte dat u hebt opgegeven. Als u wilt deze actie uitvoeren, selecteert u **3) een virtueel netwerk verwijdert uit een App**. Als u dit doet, worden er ongeveer zo uitziet:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Hoewel het script verwijderen staat, worden het virtuele netwerk niet verwijderd. Deze worden alleen de integratie verwijderd. Nadat u bevestigt dat dit is wat u wilt doen, wordt de opdracht helemaal snel worden verwerkt en wordt uitgelegd **waar** zodra deze is voltooid.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
