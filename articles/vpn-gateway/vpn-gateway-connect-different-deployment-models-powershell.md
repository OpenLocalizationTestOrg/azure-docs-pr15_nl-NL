<properties 
   pageTitle="Het klassieke virtuele netwerken koppelen aan resourcemanager virtuele netwerken via PowerShell | Microsoft Azure"
   description="Informatie over het maken van een VPN-verbinding tussen klassieke VNets en resourcemanager VNets gebruik van VPN Gateway en PowerShell"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Verbinding maken virtuele netwerken van andere implementatiemodellen via PowerShell

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)

Azure momenteel heeft twee management modellen: klassieke en Resource Manager (RM). Als u Azure hebt gebruikt voor het enige tijd, hebt u waarschijnlijk Azure VMs en exemplaar rollen in een klassieke VNet uitgevoerd. Uw nieuwere VMs en exemplaren van de rol kunnen worden uitgevoerd in een VNet in resourcemanager gemaakt. In dit artikel begeleidt u bij het klassieke VNets verbinden met resourcemanager VNets toe te staan dat de bronnen in de implementatiemodellen voor afzonderlijke via een gateway-verbinding met elkaar communiceren.

U kunt een verbinding maken tussen VNets die in verschillende-abonnementen en in verschillende regio's zijn. U kunt ook verbinding maakt VNets die al verbindingen met on-premises implementatie-netwerken te gebruiken hebt, zolang de gateway die ze zijn geconfigureerd met dynamische of route gebaseerde is. Zie de [VNet-naar-VNet Veelgestelde vragen](#faq) aan het einde van dit artikel voor meer informatie over VNet-naar-VNet verbindingen.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Implementatiemodellen en methoden om verbinding te maken van VNets in verschillende implementatiemodellen

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

We de volgende tabel als nieuwe artikelen bijwerken en aanvullende hulpmiddelen voor beschikbaar komen voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks naar deze uit de tabel.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="before-beginning"></a>Voordat u begint

De volgende stappen begeleid u bij de instellingen die nodig is om een dynamische of route gebaseerde gateway configureren voor elke VNet en een VPN-verbinding tussen de gateways te maken. Deze configuratie biedt geen ondersteuning voor statische of beleid gebaseerde gateways.

### <a name="prerequisites"></a>Vereisten voor

 - Beide VNets zijn al gemaakt.
 - De adresbereiken voor de VNets niet met elkaar overlappen of laat deze overlappen met een van de bereiken voor andere verbindingen die de gateways kunnen niet worden verbonden met.
 - U kunt de meest recente PowerShell-cmdlets hebt geïnstalleerd (1.0.2 of later). Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie. Zorg ervoor dat u de Service Management (SMI) en de cmdlets Resource Manager (RM) installeren. 

### <a name="exampleref"></a>Voorbeeldinstellingen

U kunt de voorbeeldinstellingen gebruiken als een verwijzing.

**Klassieke VNet-instellingen**

De naam van de VNet = ClassicVNet <br>
Locatie = West US <br>
Virtual Network adres spaties 10.0.0.0/24 = <br>
Subnet-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
De naam van het lokale netwerk = RMVNetLocal <br>
GatewayType = DynamicRouting

**Resourcemanager VNet-instellingen**

De naam van de VNet = RMVNet <br>
Resourcegroep RG1 = <br>
Virtual Network IP-adres spaties 192.168.0.0/16 = <br>
Subnet-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Locatie = Oost US <br>
Het openbare IP-gatewaynaam = gwpip <br>
Lokale netwerkgateway = ClassicVNetLocal <br>
Virtuele netwerkgateway-sleutel = RMGateway <br>
Gateway IP-adressen configuratie = gwipconfig


## <a name="createsmgw"></a>Sectie 1 - het klassieke VNet configureren

### <a name="part-1---download-your-network-configuration-file"></a>Deel 1 – Download het configuratiebestand netwerk

1. Meld u aan bij uw Azure-account in de PowerShell-console met verhoogde rechten. De volgende cmdlet wordt u gevraagd de aanmeldingsreferenties voor uw Azure-Account. Na het aanmelden, downloaden het uw accountinstellingen, zodat ze beschikbaar voor Azure PowerShell zijn. U gebruikt de SMI PowerShell-cmdlets om dit deel van de configuratie te voltooien.

        Add-AzureAccount

2. Exporteer het configuratiebestand Azure netwerk door de volgende opdracht uit te voeren. U kunt de locatie van het bestand exporteren naar een andere locatie zo nodig kunt wijzigen. U het bestand bewerken en klik vervolgens op Azure importeren.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

3. Open het .xml-bestand dat u hebt gedownload om deze te bewerken. Zie het [Netwerk configuratieschema](https://msdn.microsoft.com/library/jj157100.aspx)voor een voorbeeld van het configuratiebestand netwerk.
 

### <a name="part-2--verify-the-gateway-subnet"></a>Deel 2 - Controleer of het subnet gateway

In het element **VirtualNetworkSites** door een gateway-subnet aan uw VNet toevoegen als een al niet is gemaakt. Als u werkt met het netwerk configuratiebestand, het subnet gateway moet de naam 'GatewaySubnet' of Azure niet herkend en deze gebruiken als een gateway-subnet.

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Voorbeeld:**
    
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### <a name="part-3---add-the-local-network-site"></a>Deel 3 – het lokale netwerk-site toevoegen

De lokale netwerk-site die u hebt toegevoegd staat voor het RM VNet waaraan u een verbinding wilt maken. U moet toevoegen een element **LocalNetworkSites** naar het bestand als er een nog niet bestaat. Op dit moment kan de VPNGatewayAddress in de configuratie zijn van een geldige openbare IP-adres omdat we de gateway nog niet hebt gemaakt voor de Resource Manager VNet. Als we de gateway hebt gemaakt, wordt vervangen door deze IP-adres van de tijdelijke aanduiding voor het juiste openbare IP-adres dat is toegewezen aan de gateway RM.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Deel 4 – de VNet koppelen aan het lokale netwerk-site

In dit gedeelte opgeven we de lokale netwerk-site die u wilt verbinden met de VNet naar. In dit geval is de VNet resourcemanager die u eerder wordt verwezen. Zorg ervoor dat de namen die overeenkomen met. Deze stap maakt geen een gateway. Hiermee geeft u het lokale netwerk dat de gateway wordt verbinden.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Deel 5: sla het bestand en uploaden

Sla het bestand en klik op Azure importeren door de volgende opdracht uit te voeren. Zorg ervoor dat u het bestandspad zo nodig is voor uw omgeving wijzigen.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

U moet iets te zien zoals dit resultaat weergegeven dat het importeren is voltooid.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Deel 6 – maken van de gateway 

U kunt de gateway VNet maken met behulp van de klassieke-portal, of via PowerShell.

Gebruik het volgende voorbeeld om te maken van een dynamische routeren gateway:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

U kunt de status van de gateway controleren met behulp van de `Get-AzureVNetGateway` cmdlet.


## <a name="creatermgw"></a>Deel 2: De gateway RM VNet configureren

Als u wilt een VPN-gateway maken voor de VNet RM, volgt u de volgende instructies. Begint niet de stappen tot nadat u het openbare IP-adres voor de klassieke VNet gateway hebt opgehaald. 

1. **Aanmelden bij uw Azure-account** in de PowerShell-console. De volgende cmdlet wordt u gevraagd de aanmeldingsreferenties voor uw Azure-Account. Na het aanmelden, worden uw accountinstellingen zodat ze beschikbaar voor Azure PowerShell zijn gedownload.

        Login-AzureRmAccount 

    Een lijst met uw Azure abonnementen krijgen als u meer dan één abonnement hebt.

        Get-AzureRmSubscription

    Geef het abonnement waaraan u wilt gebruiken. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Een gateway lokale netwerk maken**. In een virtueel netwerk, worden de gateway lokale netwerk meestal verwijst naar uw on-premises locatie. In dit geval verwijst de gateway lokale netwerk naar uw klassieke VNet. Geef deze een naam waarmee Azure kunt raadplegen en het adres ruimte voorvoegsel ook opgeven. Azure gebruikt het IP-adresvoorvoegsel dat u wilt aangeven welke te verzenden naar uw on-premises locatie opgeven. Als u de gegevens hier later aanpassen wilt voordat u maakt de gateway, kunt u de waarden wijzigen en opnieuw uitvoeren van de steekproef.<br><br>
    - `-Name`is de naam die u wilt toewijzen om te verwijzen naar de gateway lokale netwerk.<br>
    - `-AddressPrefix`de adres-ruimte is voor uw klassieke VNet.<br>
    - `-GatewayIpAddress`is het openbare IP-adres van de klassieke VNet gateway. Zorg ervoor dat u in het onderstaande voorbeeld zodat het juiste IP-adres wijzigen.
    
            New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
            -Location "West US" -AddressPrefix "10.0.0.0/24" `
            -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. **Aanvragen van een openbare IP-adres** moet worden toegewezen aan de gateway virtueel netwerk voor de Resource Manager VNet. U kunt geen het IP-adres dat u wilt gebruiken. Het IP-adres is dynamisch toegewezen aan de gateway virtueel netwerk. Dit houdt echter niet in dat het IP-adres wordt gewijzigd. Alleen de wijzigingen virtueel netwerk gateway IP-adres is wanneer de gateway is verwijderd en opnieuw gemaakt. Deze wordt niet gewijzigd in het formaat wijzigen, opnieuw instellen of andere interne onderhoud/upgrades van de gateway.<br>In deze stap instellen we ook een variabele die wordt gebruikt in een latere stap.


        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

4. **Verifiëren dat uw virtuele netwerk een gateway-subnet heeft**. Als u geen gateway-subnet bestaat, toevoegen. Zorg ervoor dat de gateway-subnet heet *GatewaySubnet*.

5. **Het subnet ophalen** voor de gateway wordt gebruikt door de volgende opdracht uit te voeren. In deze stap instellen we ook een variabele moet worden gebruikt in de volgende stap.
    - `-Name`is de naam van uw VNet resourcemanager.
    - `-ResourceGroupName`is de resourcegroep waaraan de VNet is gekoppeld. De gateway-subnet moet al bestaan voor deze VNet en moet de naam *GatewaySubnet* werkt alleen naar behoren.


            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **De gateway IP-adressen configuratie maken**. De gatewayconfiguratie bepaalt het subnet en het openbare IP-adres gebruiken. In het onderstaande voorbeeld gebruiken om de gatewayconfiguratie te maken.<br>In deze stap de `-SubnetId` en `-PublicIpAddressId` parameters moeten worden doorgegeven de eigenschap id uit het subnet en IP-adres objecten, respectievelijk. U kunt een eenvoudige tekenreeks niet gebruiken. Deze variabelen worden ingesteld in de stap voor het aanvragen van een openbare IP-adres en de stap het subnet terughalen.

        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
    
7. **De resourcemanager virtueel netwerkgateway maken** door de volgende opdracht uit te voeren. De `-VpnType` *RouteBased*moet zijn. Kan duren, 45 minuten of meer voor deze taak is voltooid.

        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased

8. **Kopieer het openbare IP-adres** eenmaal de gateway VPN is gemaakt. U deze gebruiken wanneer u de LAN-instellingen voor uw klassieke VNet configureren. U kunt de volgende cmdlet gebruiken om op te halen het openbare IP-adres. Het openbare IP-adres wordt weergegeven in de return als *IP-adres*.

        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Sectie 3: Het lokale netwerk voor de klassieke VNet wijzigen

U kunt deze stap op basis van het netwerk configuratiebestand exporteren, te bewerken, en klik vervolgens op te slaan en het bestand te importeren terug naar Azure doen. Of, kunt u deze instelling in de klassieke portal. 

### <a name="to-modify-in-the-portal"></a>Om te wijzigen in de portal

1. Open de [klassieke portal](https://manage.windowsazure.com).

2. Klik in de klassieke portal Schuif omlaag aan de linkerkant en klikt u op **netwerken**. Klik op de **Lokale netwerken** boven aan de pagina op de pagina **netwerken te gebruiken** . 

3. Klik op de pagina **Lokale netwerken** op het lokale netwerk dat u geconfigureerd in deel 1, en Ga naar het einde van de pagina en klik op **bewerken**.

4. Klik op de pagina **Geef de details van uw lokale netwerk** kunt u het IP-adres van tijdelijke aanduiding vervangen door het openbare IP-adres voor de gateway resourcemanager die u in de vorige sectie hebt gemaakt. Klik op de pijl om te gaan naar de volgende sectie. Controleer of de **Adresruimte** klopt en klik vervolgens op het vinkje om de wijzigingen te accepteren.

### <a name="to-modify-using-the-network-configuration-file"></a>Voor het wijzigen van het netwerk configuratiebestand

1. Exporteer het configuratiebestand netwerk.
2. In een teksteditor, wijzigt u het IP-adres van de VPN gateway.

        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Uw wijzigingen op te slaan en importeer het bewerkte bestand terug naar Azure.


## <a name="connect"></a>Sectie 4: Een verbinding maken tussen de gateways

Een verbinding tussen de gateways maken, moet u PowerShell. Mogelijk moet u uw Azure-Account als u wilt gebruiken het klassieke PowerShell-cmdlets toevoegen. Hiervoor kunt u de volgende cmdlet: 
        
    Add-AzureAccount

1. **Uw gedeelde sleutel instellen** door de volgende opdracht uit te voeren. In dit voorbeeld `-VNetName` is de naam van de klassieke VNet en `-LocalNetworkSiteName` is de naam die u hebt opgegeven voor het lokale netwerk wanneer u deze in de klassieke portal hebt geconfigureerd. De `-SharedKey` is een waarde die u kunt genereren en opgeven. De waarde die u hier opgeeft, moet de dezelfde waarde die u in de volgende stap, opgeeft wanneer u de verbinding maakt. Het rendement voor dit voorbeeld moet worden weergegeven **Status: geslaagd**.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. De VPN-verbinding maken door de volgende opdrachten uit te voeren.

    **Stel de variabelen**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **De verbinding maken**<br> Houd er rekening mee dat het `-ConnectionType` IPsec, niet Vnet2Vnet is.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. U kunt de verbinding weergeven in een van beide portal of met behulp van de `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet.

        Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>Veelgestelde vragen over VNet-naar-VNet

De details van de veelgestelde vragen voor meer informatie over VNet-naar-VNet verbindingen weergeven.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


