<properties 
   pageTitle="Exemplaar niveau openbare IP-(ILPIP) | Microsoft Azure"
   description="Wat zijn ILPIP (PIP) en hoe u ze kunt beheren"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="instance-level-public-ip-overview"></a>Exemplaar niveau openbare IP-overzicht
Een exemplaar niveau openbare IP-(ILPIP) is een openbare IP-adres zijn dat u toewijzen kunt rechtstreeks aan uw exemplaar van de rol van VM of, in plaats van de cloudservice die uw rol of VM exemplaar zich bevinden in. Dit kost niet de plaats van de VIP (virtual IP) die zijn toegewezen aan uw cloudservice. In plaats daarvan is een extra IP-adres die u kunt rechtstreeks verbinden met uw rol of VM exemplaar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](virtual-network-ip-addresses-overview-arm.md). 

Zorg ervoor dat u meer informatie over de werking van [IP-adressen](virtual-network-ip-addresses-overview-classic.md) in Azure wordt aangegeven.

>[AZURE.NOTE] In het verleden, worden een ILPIP is een PIP, staat voor het openbare IP genoemd. 

![Verschil tussen ILPIP en VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Zoals u in de afbeelding 1, de cloudservice wordt geopend met een VIP, terwijl de afzonderlijke VMs gewend zijn toegankelijk met VIP:&lt;poortnummer&gt;. Een ILPIP aan een specifieke VM toewijst, kan deze VM worden geopend rechtstreeks met dat IP-adres.

Wanneer u een cloudservice in Azure wordt aangegeven maakt, worden corresponderende A DNS-records automatisch gemaakt voor toegang tot de service via een volledig gekwalificeerde domeinnaam (FQDN) in plaats van de werkelijke VIP. Dezelfde stappen zijn de gevolgen voor ILPIP, toestaan van toegang tot de rol van of VM exemplaar door FQDN in plaats van de ILPIP. Bijvoorbeeld als u een cloudservice met de naam *contosoadservice*maken en u een web-functie met de naam *contosoweb* met twee instanties configureren, Azure registreert de volgende A-records voor de exemplaren:

- contosoweb\_IN_0.contosoadservice.cloudapp.net
- contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] U kunt slechts één ILPIP voor elk exemplaar VM of rol toewijzen. U kunt maximaal 5 ILPIP van per abonnement kunt gebruiken. ILPIP is op dit moment niet ondersteund voor multi-NIC VMs.

## <a name="why-should-i-request-an-ilpip"></a>Waarom moet ik een ILPIP aanvragen?
Als u kunnen verbinding maken met uw exemplaar van de rol van VM of door een IP-adres dat is toegewezen aan deze wilt, in plaats van met de cloud service VIP:&lt;poortnummer&gt;, een ILPIP aanvragen voor uw VM of uw exemplaar van de rol.
- **Passieve FTP** - doordat een ILPIP op uw VM, u kunt ontvangen verkeer op vrijwel elk poort niet hebt u om een eindpunt verkeer ontvangen te openen. Hiermee worden scenario's zoals passieve FTP waarvan de poorten dynamisch zijn geselecteerd.
- **Uitgaande IP** - uitgaand verkeer die afkomstig zijn van de VM uitvalt met de ILPIP als de bron en dit deze identificeert op de VM naar externe items.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Over het aanvragen van een ILPIP tijdens het maken van VM
De onderstaande PowerShell-script Hiermee maakt u een nieuwe cloudservice *FTPService*, met de naam en vervolgens opgehaald van een afbeelding uit Azure, en maakt een VM *FTPInstance* met de opgehaalde afbeelding met de naam, Hiermee stelt u de VM gebruik van een ILPIP en wordt de VM toegevoegd aan de nieuwe service:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Hoe ILPIP gegevens ophalen voor een VM
Als u de informatie ILPIP voor VM die zijn gemaakt met de bovenstaande script, voer de volgende PowerShell-opdracht en houd rekening met de waarden voor *PublicIPAddress* en *PublicIPName*:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>Hoe een ILPIP verwijdert uit een VM
Als u wilt de ILPIP toegevoegd aan de VM in het bovenstaande script verwijderen, voert u de volgende PowerShell-opdracht:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Hoe u een ILPIP toevoegt aan een bestaande VM
Als u wilt een ILPIP hebt toegevoegd aan de VM gemaakt met behulp van het bovenstaande script, voert u de volgende opdracht uit:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>Het koppelen van een ILPIP voor een VM met behulp van een service-configuratiebestand
U kunt ook een ILPIP voor een VM koppelen met behulp van een service-configuratiebestand (CSCFG). Het onderstaande voorbeeld-xml ziet u hoe u een cloudservice als u wilt gebruiken, een ILPIP *MyPublicIP* benoemde voor een exemplaar van de rol configureren: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Volgende stappen

- Begrijpen hoe [IP-adressen](virtual-network-ip-addresses-overview-classic.md) werkt in het implementatiemodel klassieke.

- Meer informatie over [gereserveerde IP-adressen](virtual-networks-reserved-public-ip.md).
 