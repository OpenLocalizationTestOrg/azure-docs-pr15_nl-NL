<properties
    pageTitle="Migreren naar resourcemanager met PowerShell | Microsoft Azure"
    description="In dit artikel begeleidt bij de migratie platform ondersteund IaaS resources van klassiek naar Azure resourcemanager met behulp van Azure PowerShell-opdrachten"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Migreren IaaS resources van klassiek naar Azure resourcemanager met behulp van Azure PowerShell

Volgende stappen uit hoe u Azure PowerShell-opdrachten gebruiken om te migreren van infrastructuur als een (IaaS) serviceresources uit het implementatiemodel klassieke aan het implementatiemodel resourcemanager Azure. 

Als u wilt, kunt u ook resources migreren met behulp van de [Azure opdrachtregelinterface (Azure)](virtual-machines-linux-cli-migration-classic-resource-manager.md).

- Achtergrondinformatie over ondersteunde migratie scenario's, raadpleegt u [Platform ondersteund migratie van IaaS resources uit klassieke naar Azure resourcemanager](virtual-machines-windows-migration-classic-resource-manager.md). 
- Zie [technische uitgebreide kennismaking op platform ondersteund migratie van klassieke naar Azure resourcemanager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)voor gedetailleerde instructies en migratie Stapsgewijze instructies.

## <a name="step-1-plan-for-migration"></a>Stap 1: Plan voor migratie

Hier volgen enkele aanbevolen procedures die wordt aangeraden terwijl u migreren IaaS resources uit klassieke naar resourcemanager evalueren:

- Lees de [ondersteunde en niet-ondersteunde functies en configuraties](virtual-machines-windows-migration-classic-resource-manager.md). Hebt u virtuele machines die niet-ondersteunde configuraties of functies gebruiken, wordt u aangeraden wachten op de configuratie/ondersteuningsfunctie nog aangekondigd. U kunt ook als deze aan uw behoeften voldoet, deze functie verwijderen of verplaatsen uit die configuratie om in te schakelen van de migratie.
-   Als u scripts die uw infrastructuur en toepassingen vandaag implementeren hebben geautomatiseerde, kunt u instellingen voor een soortgelijke maken met behulp van deze scripts voor migratie. U kunt ook de steekproef omgevingen instellen met behulp van de Azure-portal.

>[AZURE.IMPORTANT]Virtuele netwerkgateways (naar website, Azure ExpressRoute, toepassingsgateway, punt-naar-site) worden momenteel niet ondersteund voor de migratie van klassiek naar resourcemanager. Als u wilt migreren van een klassieke virtuele netwerk met een gateway, de gateway te verwijderen voordat u een doorvoerbewerking als u wilt verplaatsen van het netwerk (u kunt de voorbereiden-stap uitvoeren zonder te verwijderen van de gateway). Sluit de gateway in Azure resourcemanager nadat u de migratie hebt voltooid.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Stap 2: Installeer de meest recente versie van Azure PowerShell

Er zijn twee belangrijkste opties voor het installeren van Azure PowerShell: [PowerShell galerie](https://www.powershellgallery.com/profiles/azure-sdk/) of [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps). WebPI ontvangt maandelijkse updates. Galerie met PowerShell ontvangt updates op doorlopend. In dit artikel is gebaseerd op Azure PowerShell versie 2.1.0.

Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor installatie-instructies.

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>Stap 3: Uw abonnement instellen en registreren voor migratie

Start een PowerShell-prompt. Migratie, moet u uw omgeving voor beide klassieke instellen en resourcemanager.

Meld u aan bij uw account voor het model resourcemanager.

```powershell
    Login-AzureRmAccount
```

De beschikbare abonnementen ophalen met behulp van de volgende opdracht uit:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Stel uw Azure-abonnement voor de huidige sessie. In het volgende voorbeeld wordt de naam van het standaard-abonnement op **Mijn Azure-abonnement**. Vervang de naam van het voorbeeld-abonnement door uw eigen. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Registratie is een eenmalige stap, maar u moet deze stap één keer voordat u probeert migratie. Zonder te registreren ziet u het volgende foutbericht weergegeven: 

>   *BadRequest: Abonnement is niet geregistreerd voor migratie.* 

Met de provider van de resource migratie registreren met behulp van de volgende opdracht uit:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Wacht vijf minuten voor de registratie om te voltooien. U kunt de status van de goedkeuringswerkstroom controleren met behulp van de volgende opdracht uit:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Zorg ervoor dat RegistrationState `Registered` voordat u verdergaat. 

Meld u nu aan bij uw account voor het klassieke model.

```powershell
    Add-AzureAccount
```

De beschikbare abonnementen ophalen met behulp van de volgende opdracht uit:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Stel uw Azure-abonnement voor de huidige sessie. In het volgende voorbeeld wordt de standaardabonnement op **Mijn Azure-abonnement**. Vervang de naam van het voorbeeld-abonnement door uw eigen. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Stap 4: Controleer of er voldoende cores Azure resourcemanager virtuele Machine in de Azure regio van de huidige distributie of VNET

U kunt de volgende PowerShell-opdracht gebruiken om te controleren van de huidige aantal kernen die u in Azure resourcemanager hebt. Meer informatie over core quota, raadpleegt u [limieten en de resourcemanager Azure](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

In dit voorbeeld wordt de beschikbaarheid in de regio **West Amerikaans** gecontroleerd. Vervang de naam van het voorbeeld regio door uw eigen. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Stap 5: De opdrachten voor het migreren van uw resources IaaS uitvoeren

>[AZURE.NOTE] Alle bewerkingen die hier worden beschreven, zijn idempotency is ingeschakeld. Als u problemen dan een niet-ondersteunde functie of een configuratiefout ondervindt, is het raadzaam dat u opnieuw de voorbereiden proberen, of bewerking afgebroken. Het platform geprobeerd vervolgens de actie opnieuw.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Migreren van virtuele machines in een cloudservice (niet in een virtueel netwerk)

De lijst met cloudservices verkrijgen met behulp van de volgende opdracht en kies vervolgens de cloudservice die u wilt migreren. Als de VMs in de cloudservice in een virtueel netwerk of als ze web of werknemer rollen hebben, retourneert de opdracht een foutbericht wordt weergegeven.

```powershell
    Get-AzureService | ft Servicename
```

De implementatie-naam voor de cloudservice verkrijgen. In dit voorbeeld is de servicenaam van de **Mijn Service**. Vervang de naam van de service voorbeeld met de servicenaam van uw eigen. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

De virtuele machines in de cloudservice voor migratie voorbereiden. U hebt twee opties waaruit u kunt kiezen.

* **Optie 1. De VMs migreren naar een virtueel platform gemaakt-netwerk**

    Eerst controleren of u kunt de cloudservice met de volgende opdrachten migreren:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    De bovenstaande opdracht geeft eventuele waarschuwingen en fouten die migratie blokkeren. Als validatie geslaagd is, kunt klikt u op verplaatsen naar de stap **voorbereiden** :

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **Optie 2. Migreren naar een bestaand virtual netwerk in het implementatiemodel resourcemanager**

    In het volgende voorbeeld wordt de naam van de resource-groep op **myResourceGroup**, de netwerknaam van de virtuele naar **myVirtualNetwork** en de subnetnaam van de naar **mySubNet**. Vervang de namen in het voorbeeld door de namen van uw eigen resources.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Eerst controleren of u de cloudservice met de volgende opdracht kunt migreren:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    De bovenstaande opdracht geeft eventuele waarschuwingen en fouten die migratie blokkeren. Als validatie geslaagd is, kunt klikt u vervolgens u doorgaan met de volgende stap uit voorbereiden:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Nadat de bewerking voorbereiden is gemaakt met een van de voorgaande opties, indexquery de migratiestatus van de VMs. Zorg ervoor dat ze afkomstig zijn de `Prepared` staat.

In het volgende voorbeeld wordt de naam van de VM op **myVM**. Vervang de Voorbeeldnaam met de naam van uw eigen VM.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Controleer de configuratie voor de voorbereide resources via PowerShell of via de portal van Azure. Als u niet klaar voor migratie bent en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht uit:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Als de voorbereide configuratie tevreden bent, kunt u naar voren verplaatsen en de resources doorvoeren met behulp van de volgende opdracht uit:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Virtuele machines in een virtueel netwerk migreren

Als u wilt migreren virtuele machines in een virtueel netwerk, moet u het netwerk migreren. De virtuele machines migreren automatisch aan het netwerk. Kies het virtuele netwerk dat u wilt migreren. 

In het volgende voorbeeld wordt de virtuele netwerknaam op **myVnet**. Vervang de naam van de virtuele netwerk voorbeeld met uw eigen. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Als het virtuele netwerk of werknemer rollen of VMs met niet-ondersteunde configuraties bevat, krijgt u een foutbericht gegevensvalidatie.

Eerst controleren of u het virtuele netwerk migreren kunt met behulp van de volgende opdracht uit:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

De bovenstaande opdracht geeft eventuele waarschuwingen en fouten die migratie blokkeren. Als validatie geslaagd is, kunt klikt u vervolgens u doorgaan met de volgende stap uit voorbereiden:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Controleer de configuratie voor de voorbereide virtuele machines via Azure PowerShell of via de portal van Azure. Als u niet klaar voor migratie bent en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht uit:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Als de voorbereide configuratie tevreden bent, kunt u naar voren verplaatsen en de resources doorvoeren met behulp van de volgende opdracht uit:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>Migreren van een opslag-account

Wanneer u klaar bent met de virtuele machines gemigreerd, wordt aangeraden u de opslag-accounts migreren.

Elk account opslag-migratie voorbereiden met behulp van de volgende opdracht uit. In dit voorbeeld is de opslagaccountnaam **myStorageAccount**. Vervang de Voorbeeldnaam met de naam van uw eigen opslag-account. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Controleer de configuratie voor het account voorbereide opslag via Azure PowerShell of via de portal van Azure. Als u niet klaar voor migratie bent en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht uit:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Als de voorbereide configuratie tevreden bent, kunt u naar voren verplaatsen en de resources doorvoeren met behulp van de volgende opdracht uit:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over migratie, raadpleegt u [Platform ondersteund migratie van IaaS resources uit klassieke naar Azure resourcemanager](virtual-machines-windows-migration-classic-resource-manager.md).
- Als u wilt migreren extra netwerk resources aan resourcemanager via Azure PowerShell, dezelfde stappen te gebruiken met [Verplaatsen-AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Verplaatsen-AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)en [Verplaatsen-AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx).
- Zie voor open source scripts die kunt u migreren van Azure resources van klassieke naar resourcemanager, [Hulpmiddelen voor Community's voor de migratie naar Azure resourcemanager migratie](virtual-machines-windows-migration-scripts.md)
