<properties 
   pageTitle="Het klassieke virtuele netwerken koppelen aan resourcemanager virtuele netwerken in de portal | Microsoft Azure"
   description="Informatie over het maken van een VPN-verbinding tussen klassieke VNets en resourcemanager VNets VPN Gateway en de portal gebruiken"
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
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-in-the-portal"></a>Verbinding maken virtuele netwerken van andere implementatiemodellen in de portal

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)


Azure momenteel heeft twee management modellen: klassieke en Resource Manager (RM). Als u Azure hebt gebruikt voor het enige tijd, hebt u waarschijnlijk Azure VMs en exemplaar rollen in een klassieke VNet uitgevoerd. Uw nieuwere VMs en exemplaren van de rol kunnen worden uitgevoerd in een VNet in resourcemanager gemaakt. In dit artikel begeleidt u bij het klassieke VNets verbinden met resourcemanager VNets toe te staan dat de bronnen in de implementatiemodellen voor afzonderlijke via een gateway-verbinding met elkaar communiceren. 

U kunt een verbinding maken tussen VNets die in verschillende-abonnementen en in verschillende regio's zijn. U kunt ook verbinding maakt VNets die al verbindingen met on-premises implementatie-netwerken te gebruiken hebt, zolang de gateway die ze zijn geconfigureerd met dynamische of route gebaseerde is. Zie de [VNet-naar-VNet Veelgestelde vragen](#faq) aan het einde van dit artikel voor meer informatie over VNet-naar-VNet verbindingen.

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Implementatiemodellen en methoden voor VNet-naar-VNet verbindingen

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

We de volgende tabel als nieuwe artikelen bijwerken en aanvullende hulpmiddelen voor beschikbaar komen voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks naar deze uit de tabel.<br><br>

**VNet-naar-VNet**

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Voordat u begint

De volgende stappen begeleid u bij de instellingen die nodig is om een dynamische of route gebaseerde gateway configureren voor elke VNet en een VPN-verbinding tussen de gateways te maken. Deze configuratie biedt geen ondersteuning voor statische of beleid gebaseerde gateways. 

In dit artikel gebruiken we de klassieke-portal, Azure-portal en PowerShell. Dit is momenteel niet mogelijk te maken van deze configuratie met alleen de Azure-portal.

### <a name="prerequisites"></a>Vereisten voor

 - Beide VNets zijn al gemaakt.
 - De adresbereiken voor de VNets niet met elkaar overlappen of laat deze overlappen met een van de bereiken voor andere verbindingen die de gateways kunnen niet worden verbonden met.
 - U kunt de meest recente PowerShell-cmdlets hebt geïnstalleerd (1.0.2 of later). Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie. Zorg ervoor dat u de Service Management (SMI) en de cmdlets Resource Manager (RM) installeren. 

### <a name="values"></a>Voorbeeldinstellingen

U kunt de voorbeeldinstellingen gebruiken als verwijzing.

**Klassieke VNet-instellingen**

De naam van de VNet = ClassicVNet <br>
Locatie = West US <br>
Virtual Network adres spaties 10.0.0.0/24 = <br>
Subnet-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
De naam van het lokale netwerk = RMVNetLocal <br>

**Resourcemanager VNet-instellingen**

De naam van de VNet = RMVNet <br>
Resourcegroep RG1 = <br>
Virtual Network IP-adres spaties 192.168.0.0/16 = <br>
Subnet-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Locatie = Oost US <br>
Gateway-virtuele netwerknaam = RMGateway <br>
Het openbare IP-gatewaynaam = gwpip <br>
Gateway-type = VPN <br>
Type VPN = Route gebaseerde <br>
Lokale netwerkgateway = ClassicVNetLocal <br>

## <a name="createsmgw"></a>Sectie 1: Klassieke VNet-instellingen configureren

In dit gedeelte maken we het lokale netwerk en de gateway voor uw klassieke VNet. De instructies in dit gedeelte gebruikt u de klassieke portal. De portal van Azure biedt momenteel geen alle instellingen die betrekking op een klassieke VNet hebben.

### <a name="part-1---create-a-new-local-network"></a>Deel 1: Maak een nieuwe lokale netwerk

Open het [klassieke portal](https://manage.windowsazure.com) en meld u aan met uw Azure-account.

1. Klik in de linkerbenedenhoek van het scherm, op **Nieuw** > **Netwerkservices** > **Virtual Network** > **toevoegen lokale netwerk**.

2. Typ een naam voor de RM VNet u verbinding wilt maken in het venster **Geef de details van uw lokale netwerk** . Typ in het vak **VPN apparaat IP-adres (optioneel)** een geldige openbare IP-adres. Dit is alleen een tijdelijke plaatsaanduiding. U wijzigen dit IP-adres later. Klik op de rechterbenedenhoek van het venster, op de pijlknop.
 
3. Typ het netwerk voorvoegsel en CIDR blokkeren op de pagina **Geef de adresruimte** in het vak **Beginnen IP-** tekst voor de Resource Manager VNet u verbinding wilt maken. Deze instelling wordt gebruikt om op te geven van de adresruimte om te sturen naar de VNet RM.

### <a name="part-2---associate-the-local-network-to-your-vnet"></a>Deel 2: het lokale netwerk aan uw VNet koppelen

1. **Virtuele netwerken** boven aan de pagina om te schakelen naar het scherm virtuele netwerken op en klik op als u wilt uw klassieke VNet selecteren. Klik op de pagina voor uw VNet op **configureren** om te navigeren naar de configuratiepagina.

2. Onder de sectie **naar website connectivity** verbinding, schakelt u het selectievakje **verbinding maken met het lokale netwerk** . Selecteer het lokale netwerk dat u hebt gemaakt. Als er meerdere lokale netwerken die u hebt gemaakt, moet u om te selecteren die u hebt gemaakt voor uw VNet resourcemanager in de vervolgkeuzelijst.

3. Klik op **Opslaan** onder aan de pagina.

### <a name="part-3---create-the-gateway"></a>Deel 3 – de gateway maken

1. Na het opslaan van de instellingen, klikt u op **Dashboard** boven aan de pagina om te wijzigen in de dashboardpagina. Vanaf de onderkant van de dashboardpagina, **Gateway maken**Klik op **Dynamische routering**. Klik op **Ja** om te beginnen met het maken van de gateway. Een gateway dynamische routering is vereist voor deze configuratie.

2. Wacht totdat de gateway bij naar worden gemaakt. Dit kan soms 45 minuten of langer duren.

### <a name="ip"></a>Deel 4 - weergave het openbare IP-adres van gateway

Nadat de gateway is gemaakt, kunt u het IP-adres van de gateway bekijken op de pagina **Dashboard** . Dit is het openbare IP-adres van de gateway. Noteer of kopieer het openbare IP-adres. U deze gebruiken in de volgende stappen wanneer u het lokale netwerk voor uw configuratie resourcemanager VNet maken.


## <a name="creatermgw"></a>Deel 2: Resourcemanager VNet-instellingen configureren

In dit gedeelte maken we de gateway virtueel netwerk en het lokale netwerk voor uw VNet resourcemanager. Begint niet de volgende stappen tot nadat u het openbare IP-adres voor de klassieke VNet gateway hebt opgehaald.

De schermafbeeldingen die worden als voorbeelden gegeven. Zorg ervoor dat de waarden vervangen door uw eigen. Als u deze configuratie als oefening maakt, raadpleegt u deze [waarden](#values).


### <a name="part-1---create-a-gateway-subnet"></a>Deel 1 – een subnet gateway maken

Voordat u verbinding met uw netwerk virtuele een gateway, moet u eerst de gateway subnet maken voor het virtuele netwerk waarnaar u een verbinding wilt maken. Een gateway-subnet met CIDR tellen van /28 of groter maken (/ 27/26, enz.)

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

Via een browser, Ga naar de [Azure-portal](http://portal.azure.com) en meld u aan met uw Azure-account.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Deel 2: Maak een virtueel netwerkgateway


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### <a name="part-3---create-a-local-network-gateway"></a>Deel 3 – een gateway lokale netwerk maken

De gateway van het lokale netwerk wordt meestal verwijst naar uw on-premises locatie. Krijgt Azure welk IP-adres bereiken naar route naar de locatie en het openbare IP-adres van het apparaat voor die locatie. Echter in dit geval verwijst deze naar de adresbereiken en het openbare IP-adres dat is gekoppeld aan uw klassieke VNet en virtueel netwerkgateway.

Geef de gateway lokale netwerk een naam waarmee Azure gebruikt om naar deze verwijzen te op. U kunt de gateway van uw lokale netwerk kunt maken, terwijl uw gateway virtueel netwerk wordt gemaakt. Voor deze configuratie gebruikt u het openbare IP-adres dat is toegewezen aan uw klassieke VNet gateway in de [vorige sectie](#ip).

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### <a name="part-4---copy-the-public-ip-address"></a>Deel 4 - kopie het openbare IP-adres

Zodra de gateway virtueel netwerk is voltooid maken, kopieert u het openbare IP-adres dat is gekoppeld aan de gateway. U deze gebruiken wanneer u de LAN-instellingen voor uw klassieke VNet configureren. 


## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Sectie 3: Het lokale netwerk voor de klassieke VNet wijzigen

Open de [klassieke portal](https://manage.windowsazure.com).

2. Klik in de klassieke portal Schuif omlaag aan de linkerkant en klikt u op **netwerken**. Klik op de **Lokale netwerken** boven aan de pagina op de pagina **netwerken te gebruiken** . 

3. Klik op het lokale netwerk dat u in deel 1 hebt geconfigureerd. Onderaan op de pagina, klikt u op **bewerken**.

4. Klik op de pagina **Geef de details van uw lokale netwerk** kunt u het IP-adres van tijdelijke aanduiding vervangen door het openbare IP-adres voor de gateway resourcemanager die u in de vorige sectie hebt gemaakt. Klik op de pijl om te gaan naar de volgende sectie. Controleer of de **Adresruimte** klopt en klik vervolgens op het vinkje om de wijzigingen te accepteren.

## <a name="connect"></a>Sectie 4: De verbinding maken

In deze sectie, wordt de verbinding maken tussen de VNets. De stappen hiervoor moet PowerShell. U kunt deze verbinding kunt maken in een van de portals. Controleer of u hebt gedownload en geïnstalleerd de klassieke (SMI) en de Resource Manager (RM) PowerShell-cmdlets.


1. Meld u aan bij uw Azure-account in de PowerShell-console. De volgende cmdlet wordt u gevraagd de aanmeldingsreferenties voor uw Azure-Account. Na het aanmelden, worden uw accountinstellingen zodat ze beschikbaar voor Azure PowerShell zijn gedownload.

        Login-AzureRmAccount 

    Een lijst met uw Azure abonnementen krijgen als u meer dan één abonnement hebt.

        Get-AzureRmSubscription

    Geef het abonnement waaraan u wilt gebruiken. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. U de klassieke PowerShell-cmdlets gebruiken in uw Azure-Account toevoegen. Hiervoor kunt u de volgende opdracht uit:

        Add-AzureAccount

3. Uw gedeelde sleutel instellen door in het onderstaande voorbeeld uit te voeren. In dit voorbeeld `-VNetName` is de naam van de klassieke VNet en `-LocalNetworkSiteName` is de naam die u hebt opgegeven voor het lokale netwerk wanneer u deze in de klassieke portal hebt geconfigureerd. De `-SharedKey` is een waarde die u kunt genereren en opgeven. De waarde die u hier opgeeft, moet de dezelfde waarde die u in de volgende stap, opgeeft wanneer u de verbinding maakt.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. De VPN-verbinding maken door de volgende opdrachten uit te voeren:
    
    **Stel de variabelen**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **De verbinding maken**<br> Houd er rekening mee dat het `-ConnectionType` IPsec, niet Vnet2Vnet is. In dit voorbeeld `-Name` is de naam die u wilt bellen van de verbinding. In het onderstaande voorbeeld wordt gemaakt van een verbinding met de naam '*rm naar klassieke verbinding*'.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="verify-your-connection"></a>Controleer uw verbinding

U kunt uw verbinding controleren met behulp van de klassieke-portal, de Azure portal of PowerShell. U kunt de volgende stappen uit om te controleren of de verbinding. De waarden vervangen door uw eigen.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)] 

## <a name="faq"></a>Veelgestelde vragen over VNet-naar-VNet

De details van de veelgestelde vragen voor meer informatie over VNet-naar-VNet verbindingen weergeven.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


