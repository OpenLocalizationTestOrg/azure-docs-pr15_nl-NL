<properties
    pageTitle="Een SQL Server virtuele Machine maken in Azure PowerShell (klassieke) | Microsoft Azure"
    description="Biedt stappen en PowerShell-scripts voor het maken van een VM Azure met SQL Server VM galerijafbeeldingen. In dit onderwerp wordt gebruikt voor de implementatie van klassieke modus."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Inrichten van een SQL Server-VM via Azure PowerShell (klassieke)

## <a name="overview"></a>Overzicht

In dit artikel vindt u stappen voor het maken van een SQL Server-VM in Azure wordt aangegeven via de PowerShell-cmdlets.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie [inrichten een SQL Server-VM Azure PowerShell Resource Manager gebruiken](virtual-machines-windows-ps-sql-create.md)voor de Resource Manager-versie van dit onderwerp.

### <a name="install-and-configure-powershell"></a>Installeren en configureren van PowerShell:

1. Als u niet een Azure-account hebt, bezoekt u de [gratis proefabonnement van Azure](https://azure.microsoft.com/pricing/free-trial/).

2. [Download en installeer de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md).

3. Windows PowerShell starten en verbindt u deze aan uw Azure-abonnement met de opdracht **Toevoegen-AzureAccount** .

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>Uw doel Azure regio bepalen

Uw SQL Server-VM wordt in een cloudservice die zich een specifiek gebied van de Azure bevindt worden gehost. De volgende stappen helpen u om te bepalen de regio, opslag-account en service die wordt gebruikt voor de rest van de zelfstudie cloud.

1. Hiermee bepaalt u de datacenter dat u gebruiken wilt voor het hosten van uw SQL Server-VM. De volgende PowerShell-opdrachten worden de beschikbare regio's weergegeven in detail met een overzicht aan het einde.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Nadat u uw voorkeur locatie hebt geïdentificeerd, stel een variabele met de naam **$dcLocation** regio.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>Uw abonnement en opslag-account instellen

1. Hiermee bepaalt u het Azure abonnement dat u voor de nieuwe virtuele machine gebruiken wilt.

        (Get-AzureSubscription).SubscriptionName

1. Uw doel Azure abonnement toewijzen aan de variabele **$subscr** . Stel dit als uw huidige Azure-abonnement.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Schakel vervolgens voor bestaande opslag-accounts. Het volgende script worden alle opslag accounts bestaat niet in uw regio door u gekozen:

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Als u een nieuw account voor de opslag vereist, moet u eerst een alle kleine-opslagaccountnaam maken met de opdracht Nieuw AzureStorageAccount zoals in het volgende voorbeeld: **Nieuw AzureStorageAccount - StorageAccountName "<storage account name>"-locatie $dcLocation**

1. De naam van het doel-opslag-account toewijzen aan de **$staccount**. Gebruik vervolgens de **Set-AzureSubscription** voor het instellen van het abonnement en de huidige opslag-account.

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>Selecteer de afbeelding van een SQL Server-VM

1. Bekijk de lijst met beschikbare virtuele machines van SQL Server-afbeeldingen in de galerie. Deze alle afbeeldingen hebben een **ImageFamily** -eigenschap die met 'SQL begint'. De volgende query wordt de afbeelding familie beschikbaar voor uw met SQL Server vooraf geïnstalleerd weergeeft.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Wanneer u de VM afbeelding familie, kan er meerdere gepubliceerde afbeeldingen in deze reeks. Gebruik het volgende script om de meest recente gepubliceerde VM afbeelding naam voor uw gezin geselecteerde afbeelding (zoals **SQL Server 2016 RTM Enterprise op Windows Server 2012 R2**) te vinden:

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>De virtuele machine maken

Ten slotte de virtuele machine maken met PowerShell:

1. Maak een cloudservice als host voor de nieuwe VM. Houd er rekening mee dat het is ook mogelijk in plaats daarvan een bestaande cloudservice gebruiken. Maak een nieuwe variabele **$svcname** met de korte naam van de cloudservice.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Geef de naam van de virtuele machine en een grootte. Zie voor meer informatie over VM grootte [VM grootten voor Azure](virtual-machines-windows-sizes.md).

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Geef het lokale beheerdersaccount en wachtwoord.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. De volgende script uitvoeren om de virtuele machine maken.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Zie de sectie **uw opdrachtset samenstellen** in [Azure PowerShell gebruiken om te maken en vooraf Windows gebaseerde virtuele Machines configureren](virtual-machines-windows-classic-create-powershell.md)voor aanvullende uitleg en configuratieopties.

## <a name="example-powershell-script"></a>Voorbeeld van de PowerShell-script

Het volgende script biedt en voorbeeld van een voltooid script die Hiermee maakt u een **SQL Server 2016 RTM Enterprise op Windows Server 2012 R2** virtuele machine. Als u dit script gebruikt, moet u de eerste variabelen op basis van de vorige stappen in dit onderwerp aanpassen.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Verbinding met extern bureaublad

1. Maak de. RDP-bestanden in de huidige document gebruikersmap aan deze virtuele machines installatie te voltooien starten:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. In de map documenten, start u het RDP-bestand. Verbinding maken met de beheerder-gebruikersnaam en wachtwoord eerder (bijvoorbeeld als uw gebruikersnaam VMAdmin is, "\VMAdmin" opgeven als de gebruiker en geef het wachtwoord).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Voltooi de configuratie van de SQL Server-Machine voor externe toegang

Nadat u hebt aangemeld bij de computer met extern bureaublad, configureren op basis van de instructies in de [stappen voor het configureren van SQL Server-connectiviteit in een VM Azure](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)SQL-Server.

## <a name="next-steps"></a>Volgende stappen

U vindt aanvullende instructies voor het inrichten van virtuele machines met PowerShell in de [virtuele machines documentatie](virtual-machines-windows-classic-create-powershell.md). Zie voor extra scripts die betrekking hebben op SQL Server en Premium opslagruimte, [Gebruik Azure Premium opslagruimte met SQL Server op virtuele Machines](virtual-machines-windows-classic-sql-server-premium-storage.md).

In veel gevallen wordt de volgende stap is om te migreren van uw databases naar deze nieuwe SQL Server-VM. Zie het [migreren van een Database met SQL Server op een VM Azure](virtual-machines-windows-migrate-sql.md)voor database migratie instructies.

Als u ook geïnteresseerd bent in met behulp van de Azure portal SQL virtuele Machines maken, raadpleegt u de [inrichting van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md). Houd er rekening mee dat met de aanbevolen resourcemanager model, in plaats van het klassieke model gebruikt in dit onderwerp PowerShell VMs Hiermee maakt u de zelfstudie die u bij de portal begeleidt.

Naast deze resources, is het raadzaam dat u [andere onderwerpen over met SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)bekijken.
