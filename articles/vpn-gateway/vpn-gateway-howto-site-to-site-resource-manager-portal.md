<properties
   pageTitle="Een virtueel netwerk maken met een Site op Site-VPN-verbinding met Azure resourcemanager en de Portal Azure | Microsoft Azure"
   description="Het maken van VNet met het implementatiemodel resourcemanager en maak verbinding met uw lokale lokale netwerk via een VPN-S2S gateway-verbinding."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Een VNet maken met de verbinding van een Site-naar-Site met behulp van de Azure portal

> [AZURE.SELECTOR]
- [Resourcemanager - Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Resourcemanager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klassieke - klassieke Portal](vpn-gateway-site-to-site-create.md)


In dit artikel begeleidt u bij het maken van een virtueel netwerk en een Site-naar-Site VPN gateway-verbinding voor uw on-premises netwerk met het implementatiemodel resourcemanager Azure-en de Azure-portal. Site-naar-Site-verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.

![Diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Implementatiemodellen en methoden voor het Site-naar-Site-verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

De volgende tabel ziet u de momenteel beschikbaar implementatiemodellen en methoden voor het Site-naar-Site configuraties. Wanneer een artikel met configuratiestappen beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Aanvullende configuraties 

Als u wilt VNets met elkaar verbinden, maar niet een verbinding met een on-premises locatie maakt, raadpleegt u [een verbinding VNet-naar-VNet configureren](vpn-gateway-vnet-vnet-rm-ps.md). Als u wilt dat de verbinding van een Site-naar-Site met een VNet die beschikt over een verbinding toevoegen, raadpleegt u [toevoegen een S2S verbinding met een VNet met een VPN-verbinding voor bestaande gateway](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u begint met de configuratie:

- Een VPN-apparaat en iemand die kan configureert. Zie [informatie over VPN-apparaten](vpn-gateway-about-vpn-devices.md). Als u niet bekend zijn met het configureren van uw apparaat VPN of niet bekend bent met het IP-adres bereiken zich in uw on-premises netwerkconfiguratie, moet u te coördineren met iemand die deze informatie voor u geven kunt.

- Een extern omlaag openbare IP-adres voor uw apparaat VPN. Dit IP-adres kan niet worden gevonden achter een NAT bevindt.
    
- Een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>Configuratie van voorbeeldwaarden voor deze oefening


Wanneer u deze stappen als een oefening, kunt u de configuratie voorbeeldwaarden kunt gebruiken:

- **VNet naam:** TestVNet1
- **Adresruimte:** 10.11.0.0/16 en 10.12.0.0/16
- **Subnetten:**
    - FrontEnd: 10.11.0.0/24
    - Back-end: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- **Resourcegroep:** TestRG1
- **Locatie:** Oost-VS
- **DNS-Server:** 8.8.8.8
- **Gateway-sleutel:** VNet1GW
- **Openbare IP:** VNet1GWIP
- **VPN Type:** Route gebaseerde
- **Verbindingstype:** Site-naar-site (IPsec)
- **Gateway Type:** VPN
- **Lokale netwerk Gateway-sleutel:** Site2
- **Verbindingsnaam:** VNet1toSite2


## <a name="CreatVNet"></a>1. een virtueel netwerk maken 

Als u al een VNet hebt, controleert u of de instellingen compatibel is met het ontwerp van uw VPN gateway. Speciale aandacht besteden aan eventuele subnetten die laat deze met andere netwerken overlappen mogelijk. Als er overlappende subnetten, werkt de verbinding niet goed. Als uw VNet is geconfigureerd met de juiste instellingen, kunt u de stappen in de sectie [een DNS-server opgeven](#dns) beginnen.

### <a name="to-create-a-virtual-network"></a>Een virtueel netwerk maken

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. extra adresruimte en subnetten toevoegen

U kunt extra adresruimte en subnetten toevoegen aan uw VNet zodra deze is gemaakt.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. een DNS-server opgeven

### <a name="to-specify-a-dns-server"></a>Een DNS-server opgeven

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. een subnet gateway maken

Voordat u verbinding met uw netwerk virtuele een gateway, moet u eerst de gateway subnet maken voor het virtuele netwerk waarnaar u een verbinding wilt maken. Indien mogelijk, is het raadzaam te maken van een gateway subnet met een CIDR blok /28 of /27 kan alleen worden aangeboden voldoende IP-adressen zodat aanvullende toekomstige configuratievereisten.

Als u deze configuratie als oefening maakt, raadpleegt u deze [waarden](#values) bij het maken van uw subnet, gateway.

### <a name="to-create-a-gateway-subnet"></a>Een gateway-subnet maken


[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. een gateway virtueel netwerk maken

Als u deze configuratie als oefening maakt, kunt u verwijzen naar de [configuratie voorbeeldwaarden](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Een gateway virtueel netwerk maken

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. een gateway lokale netwerk maken

De gateway van het lokale netwerk verwijst naar uw on-premises locatie. Geef de gateway lokale netwerk een naam waarmee Azure gebruikt om naar deze verwijzen te op. 

Als u deze configuratie als oefening maakt, kunt u verwijzen naar de [configuratie voorbeeldwaarden](#values).

### <a name="to-create-a-local-network-gateway"></a>Een gateway lokale netwerk maken

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. uw apparaat VPN configureren

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. een Site-naar-Site VPN-verbinding maken

Maak de Site-naar-Site VPN-verbinding tussen uw gateway virtueel netwerk en het apparaat VPN. Zorg ervoor dat de waarden vervangen door uw eigen. De gedeelde sleutel moet overeenkomen met de waarde die u voor de configuratie van uw VPN-apparaten gebruikt. 

Controleer voordat u begint in deze sectie, of dat uw virtuele netwerkgateway en het lokale netwerkgateways gemaakt hebt. Als u deze configuratie als oefening maakt, raadpleegt u deze [waarden](#values) bij het maken van de verbinding.

### <a name="to-create-the-vpn-connection"></a>De VPN-verbinding maken


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. Controleer de VPN-verbinding

U kunt controleren of uw VPN-verbinding in de portal of via PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen

- Nadat de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie de virtuele machines [leerpad](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) voor meer informatie.

- Voor informatie over BGP, raadpleegt u de [BGP-overzicht](vpn-gateway-bgp-overview.md) en [hoe u BGP configureert](vpn-gateway-bgp-resource-manager-ps.md).
