<properties
  pageTitle="Een Cloudservice verbinden met een aangepaste domeincontroller | Microsoft Azure"
  description="Leer hoe u uw web/werknemer rollen verbinden met een aangepast AD-domein met PowerShell en AD-domein-extensie"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Azure Cloud Services-rollen verbinden met een aangepaste AD domeincontroller gehost in Azure wordt aangegeven

Er wordt eerst een virtueel netwerk (VNet) instellen in Azure wordt aangegeven. We wordt een Active Directory-domeincontroller (die worden gehost op een Azure virtuele machines) klikt u vervolgens aan de VNet toegevoegd. Er wordt vervolgens bestaande cloud service rollen toevoegen aan de vooraf gemaakte VNet en ze vervolgens verbinding maken met de domeincontroller.

Voordat we aan de slag, enkele dingen in gedachten moet houden:

1.  In deze zelfstudie wordt PowerShell, dus neem controleert u of er Azure PowerShell geïnstalleerd en wilt verzenden. Als u hulp wilt bij het instellen van Azure PowerShell, raadpleegt u [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

2.  Uw exemplaren AD-domeincontroller en Web/werknemer rol moeten in de VNet.

Volg deze stapsgewijze handleiding en als u problemen ondervindt, beter hieronder. Iemand wordt u terug wilt naar u (Ja, we opmerkingen lezen).

3. Het netwerk waarnaar wordt verwezen door de cloud service <mark>moet</mark> een **klassieke virtueel netwerk**.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

U kunt een virtueel netwerk maken in Azure wordt aangegeven via de portal van Azure klassieke of PowerShell. We PowerShell gebruiken voor deze zelfstudie. Zie [Virtuele netwerk maken](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)een virtueel netwerk met behulp van de Azure klassieke portal maken.

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Een virtuele Machine maken

Als u klaar bent met het instellen van het virtuele netwerk, moet u om een AD-domeincontroller te maken. Voor deze zelfstudie gaat we instellen een AD-domeincontroller op een Azure virtuele machines.

Klik hiertoe door een virtuele machine via PowerShell met de onderstaande opdrachten te maken:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Uw virtuele computer met een domeincontroller verhogen
Als u wilt de virtuele Machine hebt geconfigureerd als een AD-domeincontroller, moet u aanmelden voor VM en configureer deze.

Als u wilt Meld u aan bij de VM, kunt u krijgen van het RDP-bestand via PowerShell, gebruikt u de onderstaande opdrachten.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Nadat u bent aangemeld bij de VM, de installatie uw VM als een AD-domeincontroller volgens de stapsgewijze handleiding over [het instellen van uw klant AD-domeincontroller](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Uw Cloudservice toevoegen aan het virtuele netwerk

Vervolgens moet u uw implementatie cloud-service toevoegen aan de VNet die u zojuist hebt gemaakt. Klik hiertoe uw cscfg cloud-service te wijzigen door de betreffende secties toe te voegen aan uw cscfg Visual Studio of editor van uw keuze.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Vervolgens het cloud services-project bouwen en het dashboard implementeren naar Azure. Als u hulp wilt bij het gebruik van uw cloud services-pakket naar Azure, raadpleegt u [het maken en implementeren een Cloudservice](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Uw web/werknemer rollen verbinden met het domein

Als uw project cloud-service wordt geïmplementeerd op Azure, verbinden met uw rol exemplaren het aangepaste AD-domein met de extensie AD-domein. Als u wilt de extensie AD-domein toevoegen aan uw bestaande cloud services-implementatie en deelnemen aan het aangepaste domein, moet u de volgende opdrachten uitvoeren in PowerShell:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

En dat is het.

U cloud services nu deel van uw aangepaste domeincontroller uitmaakt moeten. Als u meer informatie over de verschillende beschikbare opties voor het configureren van de extensie AD-domein wilt, gebruikt u de PowerShell-help zoals hieronder wordt weergegeven.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
