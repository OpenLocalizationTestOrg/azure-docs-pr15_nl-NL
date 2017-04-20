<properties
    pageTitle="Een SQL Server virtuele Machine maken in Azure PowerShell (resourcemanager) | Microsoft Azure"
    description="Biedt stappen en PowerShell-scripts voor het maken van een VM Azure met SQL Server VM galerijafbeeldingen."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Inrichten van een SQL Server-VM via Azure PowerShell (resourcemanager)

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>Overzicht

Deze zelfstudie ziet u hoe u een enkel Azure virtuele machine met het **Resourcemanager Azure** -implementatie-model met Azure PowerShell-cmdlets maken. We gaan één virtuele machines met een enkele schijf uit een afbeelding in de SQL-galerie maken in deze zelfstudie. We gaan nieuwe providers voor de opslag-, netwerk- en berekeningscluster resources die worden gebruikt door de virtuele machine maken. Als u bestaande providers voor een van deze resources hebt, kunt u in plaats daarvan deze providers gebruiken.

Als u de lightversie van dit onderwerp nodig hebt, raadpleegt u [inrichten een SQL Server-VM met Azure PowerShell klassieke](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Vereisten voor

Voor deze zelfstudie hebt u het volgende nodig:

- Een Azure-account en abonnementen voordat u begint. Als u niet hebt, registreren voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- [Azure PowerShell)](../powershell-install-configure.md), minimale versie van 1.4.0 of hoger (deze zelfstudie geschreven met versie 1.5.0).
    - Typ het **Get-Module Azure-ListAvailable**wilt ophalen uw versie.

## <a name="configure-your-subscription"></a>Uw abonnement configureren

Open Windows PowerShell en toegang tot uw Azure-account maken door de volgende cmdlet uit te voeren. U krijgt een aanmeldingsscherm uw referenties invoeren. Gebruik de dezelfde e-mailadres en wachtwoord waarmee u zich aanmelden bij de portal van Azure.

    Add-AzureRmAccount

Na het succes aanmelden ziet u enkele informatie op scherm dat de SubscriptionId waarmee u zich aanmeldde bevat. Dit is het abonnement waarin de bronnen voor deze zelfstudie wordt gemaakt tenzij u in een ander abonnement wijzigt. Als u meerdere SubscriptionIds hebt, voert u de volgende cmdlet om terug te keren van een lijst met al uw SubscriptionIds:

    Get-AzureRmSubscription

Als u wilt wijzigen in een andere SubscriptionID, moet u de volgende cmdlet uitvoeren met de gewenste SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Afbeelding van de variabelen definiëren

Om te vereenvoudigen bruikbaarheid en het begrip van de voltooide script van deze zelfstudie, wordt begin met het definiëren van een aantal variabelen. Wijzig de parameterwaarden als u naar eigen inzicht, maar pas op voor het geven van namen aan beperkingen die betrekking hebben op de naam van lengte berekend en speciale tekens bij het wijzigen van de waarden die zijn ingevoerd.

### <a name="location-and-resource-group"></a>Locatie en een resourcegroep
Twee variabelen gebruiken om te definiëren het gegevensgebied en de resourcegroep waarin u de andere bronnen voor de virtuele machine wilt maken.

Wijzig desgewenst en vervolgens de volgende cmdlets uit om deze variabelen geïnitialiseerd uitvoeren.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Eigenschappen van opslag

Gebruik de volgende variabelen om de opslag-account en het type opslag moet worden gebruikt door de virtuele machine te definiëren.

Wijzig desgewenst en vervolgens de volgende cmdlet als u wilt deze variabelen geïnitialiseerd uitvoeren. Houd er rekening mee dat in dit voorbeeld gebruiken we [Premium opslag](../storage/storage-premium-storage.md), die voor productie werkbelasting geschikt is. Zie voor meer informatie over deze richtlijnen en andere aanbevelingen, [prestaties aanbevolen procedures voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Netwerkeigenschappen van het

Gebruik de volgende variabelen om te definiëren de netwerkinterface, de methode voor TCP/IP-toewijzing, de virtuele netwerknaam, de subnetnaam van de virtuele, het bereik van IP-adressen voor het virtuele netwerk, het bereik van IP-adressen voor het subnet en het label openbare domein naam moet worden gebruikt door het netwerk in de virtuele machine.

Wijzig desgewenst en vervolgens de volgende cmdlet als u wilt deze variabelen geïnitialiseerd uitvoeren.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>VM eigenschappen

Gebruik de volgende variabelen om te definiëren de naam van de virtuele machine, de naam van de computer, de grootte VM en de naam van de schijf besturingssysteem voor de virtuele machine.

Wijzig desgewenst en vervolgens de volgende cmdlet als u wilt deze variabelen geïnitialiseerd uitvoeren.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Afbeeldingseigenschappen

Gebruik de volgende variabelen definiëren van de afbeelding wilt gebruiken voor de virtuele machine. In dit voorbeeld wordt de afbeelding van de SQL Server-2016 Enterprise gebruikt.

Wijzig desgewenst en vervolgens de volgende cmdlet als u wilt deze variabelen geïnitialiseerd uitvoeren.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Houd er rekening mee dat u een volledige lijst met SQL Server-abonnementen voor de afbeelding met de opdracht Get-AzureRmVMImageOffer kunt krijgen:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

En ziet u de beschikbaar voor een aanbod met de opdracht Get-AzureRmVMImageSku SKU's. De volgende opdracht ziet u dat alle SKU's beschikbaar voor de **SQL2014SP1-WS2012R2** bieden.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Met het implementatiemodel resourcemanager is het eerste object dat u maakt de resourcegroep. We gebruiken de cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) een Azure resourcegroep en de bijbehorende bronnen maken met de naam van de resource-groep en locatie die zijn gedefinieerd door de variabelen die u eerder geïnitialiseerd.

Voer de volgende cmdlet om uw nieuwe resourcegroep te maken.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Een opslag-account maken

De virtuele machine vereist opslag resources voor de schijf besturingssysteem gebruikt en voor de SQL Server-gegevens en log-bestanden. We gaan volgt de procedure waarmee één schijf maken voor beide. U kunt extra schijven later met behulp van de cmdlet [Toevoegen-Azure schijf](https://msdn.microsoft.com/library/azure/dn495252.aspx) te plaatsen van de SQL Server-gegevens en logboekbestanden op speciale schijven koppelen. We gebruiken de cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) maken van een standaard-opslag-account in uw nieuwe resourcegroep en met de opslagaccountnaam, opslag Sku naam en locatie gedefinieerd met behulp van de variabelen die u eerder geïnitialiseerd.

Voer de volgende cmdlet om uw nieuwe opslag-account maken.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Netwerk resources maken

De virtuele machine is een aantal mogelijkheden voor netwerkconnectiviteit vereist.

- Elke virtuele machine moet een virtueel netwerk.
- Een virtueel netwerk moet ten minste één subnet gedefinieerd.
- Een netwerkinterface moet worden gedefinieerd met een openbare of een persoonlijk IP-adres.

### <a name="create-a-virtual-network-subnet-configuration"></a>Een virtuele netwerkconfiguratie subnet maken

We zijn verzonden door te maken van een subnet is geconfigureerd voor onze virtuele netwerk. We gaan voor training, een standaard-subnet met de cmdlet [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) maken. We maakt onze virtuele netwerkconfiguratie subnet met de naam en het adres voorvoegsel van subnet gedefinieerd met behulp van de variabelen die u eerder geïnitialiseerd.

>[AZURE.NOTE] U kunt aanvullende eigenschappen van het virtuele subnet netwerkconfiguratie via deze cmdlet definiëren, maar die buiten het bereik van deze zelfstudie is.

De volgende cmdlet als u wilt maken van uw virtuele subnetconfiguratie uitvoeren.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

We gaan vervolgens onze virtueel netwerk met de cmdlet [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) maken. We onze virtueel netwerk tijdens uw nieuwe resourcegroep wilt maken, met de naam, locatie en adres voorvoegsel gedefinieerd met behulp van de variabelen die u eerder geïnitialiseerd en het gebruik van de subnetconfiguratie die u in de vorige stap hebt gedefinieerd.

Voer de volgende cmdlet om uw virtuele netwerk te maken.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Het openbare IP-adres maken

Nu dat we onze virtueel netwerk gedefinieerd hebben, moeten we een IP-adres voor connectiviteit met de virtuele machine configureren. Voor deze zelfstudie gaan we een openbare IP-adres met dynamische IP-adressen voor internetconnectiviteit maken. We gebruiken de cmdlet [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) maken van het openbare IP-adres in de resourcegroep prevously gemaakt en met de naam, locatie, toewijzingsmethode en DNS-domein naamlabel gedefinieerd met behulp van de variabelen die u eerder geïnitialiseerd.

>[AZURE.NOTE] U kunt aanvullende eigenschappen van het openbare IP-adres met deze cmdlet definiëren, maar die buiten het bereik van deze eerste zelfstudie is. U kunt ook een privé-adres of een adres maken met een statisch adres, maar dat is ook buiten het bereik van deze zelfstudie.

Voer de volgende cmdlet als u wilt maken van uw openbare IP-adres.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>De netwerkinterface maken

We gaan nu de netwerkinterface die worden gebruikt door onze VM maken. De cmdlet [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) gebruiken we onze netwerkinterface maken in de resourcegroep eerdere en met de naam, locatie, subnet en openbare IP-adres eerder gedefinieerde gemaakt.

Voer de volgende cmdlet om de netwerkinterface van uw te maken.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Een object VM configureren

Nu we hebben opslag- en resources die zijn gedefinieerd, we zijn klaar om te berekenen bronnen definiëren voor de virtuele machine. We wordt opgeven van het formaat van de virtuele machine en verschillende eigenschappen van het besturingssysteem, geef de netwerkinterface die we eerder hebt gemaakt, blobopslag definiëren en geef vervolgens de schijf besturingssysteem voor onze zelfstudie.

### <a name="create-the-vm-object"></a>Het object VM maken

We zijn verzonden door het opgeven van de grootte VM. Voor deze zelfstudie geeft we een DS13. We gebruiken de cmdlet [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) een configureerbare VM-object maken met de naam en de grootte die zijn gedefinieerd met behulp van de variabelen die u eerder geïnitialiseerd.

De volgende cmdlet als u wilt maken van het object VM uitvoeren.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Een referentie-object waarin de naam en het wachtwoord voor de lokale beheerder-referenties maken

Voordat we de eigenschappen van het besturingssysteem voor de virtuele machine instellen kunt, moeten we de referenties voor de lokale beheerdersaccount als een beveiligde tekenreeks. Hiervoor gebruiken we de cmdlet [Get-referentie](https://technet.microsoft.com/library/hh849815.aspx) .

De volgende cmdlet uitvoeren en typ de naam en het wachtwoord voor het lokale beheerdersaccount in de virtuele Windows-computer wilt gebruiken in het venster Windows PowerShell referentie.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>De eigenschappen van het besturingssysteem voor de virtuele machine instellen

We kunnen nu de VM besturingssysteem-eigenschappen instellen. We gebruiken de cmdlet [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) wilt instellen van het type besturingssysteem als Windows, vereisen de [VM agent](virtual-machines-windows-classic-agents-and-extensions.md) worden geïnstalleerd, opgeven dat de cmdlet mogelijk automatisch bijwerken maakt en de naam van de virtuele machine, de naam van de computer en het gebruik van de variabelen die u eerder geïnitialiseerd referenties instellen.

De volgende cmdlet om in te stellen van de eigenschappen van het besturingssysteem voor uw virtuele computer worden uitgevoerd.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>De netwerkinterface toevoegen aan de virtuele machine

Vervolgens wordt we de netwerkinterface die we hebben gemaakt eerder toevoegen aan de virtuele machine. We gebruiken de cmdlet [Toevoegen-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) om toe te voegen van de netwerkinterface met het netwerk interface-variabele die u eerder hebt gedefinieerd.

De volgende cmdlet om in te stellen van de netwerkinterface voor uw virtuele computer worden uitgevoerd.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>De opslaglocatie blob voor de schijf moet worden gebruikt door de virtuele machine instellen

We Stel vervolgens de opslaglocatie blob voor de schijf moet worden gebruikt door de virtuele machine met de variabelen die u eerder hebt gedefinieerd.

De volgende cmdlet om in te stellen van de opslaglocatie van blob uitvoeren.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Het besturingssysteem schijfeigenschappen voor de virtuele machine instellen

Vervolgens we wordt het besturingssysteem schijfeigenschappen instellen voor de virtuele machine. We gebruiken de cmdlet [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) om op te geven dat het besturingssysteem voor de virtuele machine wordt afkomstig van een afbeelding zijn, om in te stellen in cache opslaan als u wilt lezen alleen (omdat SQL Server wordt geïnstalleerd op dezelfde schijf) en de naam van de virtuele machine en het besturingssysteem schijf gedefinieerd met behulp van de variabelen die we eerder gedefinieerd te definiëren.

De volgende cmdlet instellen van het besturingssysteem schijfeigenschappen voor uw virtuele computer worden uitgevoerd.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>De afbeelding platform voor de virtuele machine opgeven

Onze laatste configuratiestap, is de afbeelding platform voor onze VM opgeven. Voor training gebruiken we de meest recente SQL Server-2016 CTP-afbeelding. We gebruiken de cmdlet [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) in deze afbeelding gebruiken, zoals gedefinieerd door de variabelen die u eerder hebt gedefinieerd.

De volgende cmdlet om op te geven van de afbeelding platform voor uw virtuele computer worden uitgevoerd.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>De SQL-VM maken

Nu u klaar bent met de volgende configuratiestappen uit, bent u gereed voor het maken van de virtuele machine. We gebruiken de cmdlet [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) maken van de virtuele machine met de variabelen die we hebt gedefinieerd.

De volgende cmdlet als u wilt maken van uw virtuele computer worden uitgevoerd.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

De virtuele machine wordt gemaakt. Zoals u ziet dat een standaard-opslag-account voor opstarten diagnostische gegevens is gemaakt, omdat het opgegeven opslag-account voor de VM schijf een premium-opslag-account is.

U kunt deze computer nu weergeven in de Portal Azure om [het openbare IP-adres en de volledig gekwalificeerde domeinnaam](virtual-machines-windows-portal-sql-server-provision.md#Connect)weer te geven.  

## <a name="example-script"></a>Voorbeeldscript

Het volgende script bevat het volledige PowerShell-script voor deze zelfstudie. Het wordt ervan uitgegaan dat u al ingesteld het Azure abonnement hebt voor gebruik met de opdrachten **Toevoegen-AzureRmAccount** en **Selecteer AzureRmSubscription** .


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Volgende stappen
Nadat de virtuele machine is gemaakt, bent u nu verbinding maken met de virtuele machine en configuratie van RDP-connectiviteit. Zie [verbinding maken naar een SQL Server virtuele Machine op Azure (resourcemanager)](virtual-machines-windows-sql-connect.md)voor meer informatie.
