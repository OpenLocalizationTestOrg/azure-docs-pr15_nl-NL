<properties
   pageTitle="Gereserveerd IP | Microsoft Azure"
   description="Meer informatie over gereserveerde IP-adressen en hoe u ze kunt beheren"
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

# <a name="reserved-ip-overview"></a>Gereserveerde IP-overzicht
IP-adressen in Azure wordt aangegeven in twee categorieën vallen: dynamische en gereserveerde. Openbare IP-adressen die worden beheerd door Azure zijn standaard dynamisch. Dat betekent dat het IP-adres voor een bepaald cloudservice (VIP gebruikt) of voor toegang tot een rol of VM exemplaar rechtstreeks (ILPIP) te bezoeken, kunnen wijzigen wanneer resources afgesloten worden of opgeheven.

Als u wilt voorkomen dat IP-adressen wijzigen, kunt u een IP-adres reserveren. Gereserveerde IP-adressen kan alleen worden gebruikt als een VIP, ervoor te zorgen dat het IP-adres voor de cloudservice wordt niet hetzelfde zijn zelfs wanneer resources afgesloten worden of opgeheven. Bovendien kunt u gebruikt als een VIP naar een gereserveerde IP-adres bestaande dynamische IP-adressen converteren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe u een statische openbare IP-adres met het [resourcemanager implementatiemodel](virtual-network-ip-addresses-overview-arm.md)reserveren.

Zorg ervoor dat u meer informatie over de werking van [IP-adressen](virtual-network-ip-addresses-overview-classic.md) in Azure wordt aangegeven.

## <a name="when-do-i-need-a-reserved-ip"></a>Wanneer heb ik een gereserveerde IP nodig?
- **Gewenste om ervoor te zorgen dat de IP in uw abonnement is gereserveerd**. Als u reserveren een IP-adres dat niet uit uw abonnement in elk geval worden uitgebracht wilt, moet u een gereserveerde openbare IP-adres.  
- **U wilt dat uw IP om te blijven met uw cloudservice zelfs op gestopt of opgeheven staat (VMs)**. Als u uw service zijn toegankelijk via een IP-adres dat niet wordt gewijzigd, wilt zelfs wanneer VMs in de cloudservice stoppen of opgeheven.
- **Gewenste om ervoor te zorgen dat uitgaand verkeer van Azure een overzichtelijk IP-adres gebruikt**. U moet mogelijk uw firewall on-premises implementatie is geconfigureerd dat alleen verkeer is toegestaan van specifieke IP-adressen. Door te reserveren een IP, moet u weet het bron-IP-adres en hoeft te bijwerken van uw firewallregels vanwege een IP-wijzigen.

## <a name="faq"></a>FAQ
1. Kan ik een gereserveerde IP-adres gebruiken voor alle Azure-services?  
  - Gereserveerde IP-adressen kan alleen worden gebruikt voor VMs en cloud service exemplaar rollen weergegeven via een VIP.
1. Hoeveel gereserveerde IP-adressen kan ik hebben?  
  - Alle Azure abonnementen zijn op dit moment kan geautoriseerd gebruik van 20 gereserveerde IP-adressen. U kunt echter extra gereserveerde IP-adressen aanvragen. Zie de pagina [abonnement en Service limieten](../azure-subscription-service-limits.md) voor meer informatie.
1. Is er een boete voor gereserveerde IP-adressen?
  - Zie [Gereserveerde IP-adres prijzen Details](http://go.microsoft.com/fwlink/?LinkID=398482) voor prijsinformatie.
1. Hoe ik een IP-adres reserveren?
  - U kunt PowerShell of de [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx) Reserveer een IP-adres in een bepaalde regio. Dit gereserveerde IP-adres is gekoppeld aan uw abonnement. U kunt niet een IP-adres reserveren met behulp van de beheerportal.
1. Kan ik gebruiken dit met affiniteit groep op basis van VNets?
  - Gereserveerde IP-adressen worden alleen ondersteund in regionale VNets. Deze wordt niet ondersteund voor VNets die zijn gekoppeld aan affiniteit groepen. Zie voor meer informatie over het koppelen van een VNet met een regio of een groep affiniteit [over regionale VNets en affiniteit groepen](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>Gereserveerde VIP's beheren

Voordat u gereserveerde IP-adressen gebruiken kunt, moet u deze toevoegen aan uw abonnement. Als u wilt maken een gereserveerde IP-adres van de groep openbare IP-adressen beschikbaar op de locatie van de *Central US* , voer de volgende PowerShell-opdracht:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Zoals u ziet, maar dat u geen opgeven wat IP wordt gereserveerd. Voer de volgende PowerShell-opdracht om weer te geven welke IP-adressen zijn gereserveerd in uw abonnement, en ziet u de waarden voor *ReservedIPName* en *adres*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Nadat een IP is gereserveerd, blijft deze aan uw abonnement gekoppeld totdat u deze verwijderen. Als u wilt verwijderen de gereserveerde IP hierboven, voer de volgende PowerShell-opdracht:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>Het reserveren van het IP-adres van een bestaande cloudservice

U kunt het IP-adres van een bestaande cloudservice reserveren door toe te voegen van de parameter *- Servicenaam* . Als u wilt reserveren het IP-adres van een cloudservice *TestService* op de locatie van de *Central US* , voer de volgende PowerShell-opdracht:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>Het koppelen van een gereserveerde IP naar een nieuwe cloudservice
De onderstaande script Hiermee maakt u een nieuwe gereserveerde IP-adres en vervolgens koppelt u deze naar een nieuwe cloudservice *TestService*met de naam.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Wanneer u een gereserveerde IP-adres voor gebruik met een cloudservice maakt, moet u nog steeds te verwijzen naar de VM met behulp van *VIP:&lt;poortnummer >* voor binnenkomende communicatie. Een IP reserveren betekent niet dat u direct verbinding kunt maken met de VM. De gereserveerde IP wordt toegewezen aan de cloudservice die de VM is geïmplementeerd in. Als u rechtstreeks een VM door IP verbinding wilt maken, hebt u voor het configureren van een openbare IP exemplaar niveau. Een exemplaar niveau openbare IP is een type van openbare IP-(ook wel een ILPIP genoemd) en die rechtstreeks naar uw VM is toegewezen. Dit kan niet worden gereserveerd. Zie [Exemplaar niveau openbare IP-(ILPIP)](virtual-networks-instance-level-public-ip.md) voor meer informatie.

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>Hoe u een gereserveerde IP-adres van een lopende implementatie verwijderen
Als u wilt verwijderen de gereserveerde IP toegevoegd aan de nieuwe service in de bovenstaande script hebt gemaakt, voert u de volgende PowerShell-opdracht:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Een gereserveerde IP-adres worden van een lopende implementatie niet verwijderd de reservering uit uw abonnement. Deze gewoon maakt de IP moet worden gebruikt door een andere resource in uw abonnement.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>Hoe u kunt een gereserveerde IP aan de implementatie van een lopende koppelen
De onderstaande script Hiermee maakt u een nieuwe cloudservice met *TestService2* met een nieuwe VM *TestVM2*met de naam de naam en klikt u vervolgens koppelt de bestaande gereserveerde IP met de naam *MyReservedIP* naar de cloudservice.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Het koppelen van een gereserveerde IP naar een cloudservice met behulp van een service-configuratiebestand
U kunt ook een gereserveerde IP naar een cloudservice koppelen met behulp van een service-configuratiebestand (CSCFG). Het onderstaande voorbeeld-xml ziet u hoe u een cloudservice als u wilt gebruiken, een gereserveerde VIP benoemde *MyReservedIP*configureren:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Volgende stappen

- Begrijpen hoe [IP-adressen](virtual-network-ip-addresses-overview-classic.md) werkt in het implementatiemodel klassieke.

- Meer informatie over [gereserveerde privé IP-adressen](virtual-networks-reserved-private-ip.md).

- Meer informatie over [exemplaar niveau openbare IP-(ILPIP)-adressen](virtual-networks-instance-level-public-ip.md).
