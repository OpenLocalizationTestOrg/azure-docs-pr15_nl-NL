<properties 
   pageTitle="Een virtueel netwerk verbinden met meerdere sites met VPN Gateway en PowerShell | Microsoft Azure"
   description="In dit artikel wordt u begeleid meerdere lokale on-premises implementatie sites verbinden door een virtueel netwerk met behulp van een Gateway VPN voor het implementatiemodel klassieke."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Een Site op Site-verbinding met een VNet met een VPN-verbinding voor bestaande gateway toevoegen

> [AZURE.SELECTOR]
- [Resourcemanager - Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klassieke - PowerShell](vpn-gateway-multi-site.md)

In dit artikel begeleidt u bij het PowerShell gebruiken voor de Site-naar-Site (S2S)-verbindingen toevoegen aan een VPN-gateway met een bestaande verbinding. Dit type verbinding wordt vaak een siteconfiguratie 'meerdere ' genoemd. 

In dit artikel is van toepassing op virtuele netwerken die zijn gemaakt met behulp van het implementatiemodel klassieke (ook wel bekend als servicebeheer). Deze stappen worden niet toegepast op ExpressRoute/naar website naast elkaar bestaande verbinding configuraties. Zie [ExpressRoute/S2S naast elkaar bestaande verbindingen](../expressroute/expressroute-howto-coexist-classic.md) voor informatie over naast elkaar bestaande verbindingen.

### <a name="deployment-models-and-methods"></a>Implementatiemodellen en methoden

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

We in deze tabel bijwerken als nieuwe artikelen en aanvullende hulpmiddelen voor beschikbaar komen voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>Over het verbinding maken

U kunt meerdere on-premises implementatie sites verbinden met één virtuele netwerk. Dit is bijzonder aantrekkelijk voor het ontwikkelen van oplossingen voorstelt hybride-cloud. Maakt een verbinding met meerdere site met de netwerkgateway Azure virtuele is vergelijkbaar met het maken van andere verbindingen naar website. Ja, kunt u een bestaande Azure VPN gateway, zolang de gateway dynamische is (route-basis).

Als u een statische gateway verbonden met uw netwerk virtuele al hebt, kunt u het type gateway naar een dynamische wijzigen zonder dat u nodig hebt om het virtuele netwerk om te kunnen uitgevoerd op meerdere locaties opnieuw te maken. Voordat u wijzigt de routering type, zorg dat uw on-premises implementatie VPN gateway route gebaseerde VPN configuraties ondersteunt. 

![diagram met meerdere trainingssite] (./media/vpn-gateway-multi-site/multisite.png "meerdere sites")

## <a name="points-to-consider"></a>Overwegingen

**U kunt niet gebruikmaken van de Azure klassieke Portal om deze virtuele netwerk te wijzigen.** Voor deze release moet u wijzigingen aanbrengen in het netwerk configuratiebestand in plaats van met behulp van de Azure klassieke Portal. Als u wijzigingen in de klassieke Azure-Portal aanbrengt, wordt ze uw meerdere site-Verwijzingsinstellingen voor dit virtuele netwerk overschrijven. 

U moet vertrouwd heel met het netwerk configuratiebestand op het moment dat u klaar bent met de procedure meerdere locaties. Echter als er meerdere personen Bezig met configureren van uw netwerk, moet u om ervoor te zorgen dat iedereen op de hoogte van deze beperking. Dit betekent niet dat u de klassieke Azure-Portal helemaal niet gebruiken. U kunt deze kunt gebruiken voor de rest, met uitzondering van configuratiewijzigingen aanbrengt in deze bepaalde virtueel netwerk.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u de configuratie, Controleer of u het volgende:

- Een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).

- Compatibele VPN hardware voor elk on-premises implementatie locatie. Schakel [Over VPN apparaten voor Virtual netwerkconnectiviteit](vpn-gateway-about-vpn-devices.md) om te controleren of het apparaat dat u wilt gebruiken is iets die bekend is compatibel.

- Een extern omlaag openbare IPv4-IP-adres voor elk apparaat VPN. Het IP-adres kan niet worden gevonden achter een NAT bevindt. Dit is vereiste.

- U moet de meest recente versie van de Azure PowerShell-cmdlets installeren. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.

- Iemand die wordt getoond hoe u goed configureren van de VPN hardware. U kunt niet gebruikmaken van de VPN automatisch gegenereerde scripts in de klassieke Azure-Portal voor het configureren van uw VPN-apparaten. Dit betekent dat u moet wel sterke begrijpen hoe u uw apparaat VPN configureren of werken met een persoon die heeft.

- Het IP-adresbereiken die u gebruiken voor uw virtuele netwerk wilt (als u dit nog niet hebt gemaakt). 

- Het IP-adresbereiken voor elk van de lokale netwerk-sites die u een verbinding met maken kunt. U moet om ervoor te zorgen dat het IP-adres bereiken voor elk van de lokale netwerk-sites die u wilt verbinden met elkaar niet overlappen. Anders weigert de klassieke Azure-beheerportal of de REST API de configuratie die u wilt uploaden. 

    Hebt u twee lokale netwerksites dat beide het IP-adres bereik 10.2.3.0/24 bevatten en u een pakket met een doeladres 10.2.3.3 hebt, wouldn't Azure bijvoorbeeld weet in welke site u verzenden van het pakket wilt, omdat de adresbereiken overlappen. Als u wilt voorkomen dat routeren problemen, Azure niet is toegestaan voor het uploaden van een configuratiebestand met overlappende bereiken.



## <a name="1-create-a-site-to-site-vpn"></a>1. een VPN-verbinding voor de Site-naar-Site maken

Als u al een VPN-verbinding van het Site-naar-Site met een dynamische routeren gateway geweldig! U kunt doorgaan met het [exporteren van de virtuele netwerk configuratie-instellingen](#export). Als dat niet zo is, als volgt te werk:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Als u al een Site-naar-Site virtueel netwerk, maar er een statische (beleid-basis) routeren gateway:

1. Wijzig het type van de gateway in het dynamische routering. Een VPN-verbinding met meerdere locaties vereist een dynamische (ook wel bekend als route-basis) routeren gateway. Als u wilt wijzigen van het type gateway, moet u eerst de bestaande gateway verwijderen en vervolgens een nieuwe record maken. Zie voor instructies voor [het wijzigen van de VPN routeren type voor de gateway](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  

2. Uw nieuwe gateway configureren en maak uw tunnel VPN. Zie [configureren een VPN-Gateway in de klassieke Azure-Portal](vpn-gateway-configure-vpn-gateway-mp.md)voor instructies. Eerst uw gateway type wijzigen dynamische routering. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Als u geen virtuele netwerk met een Site-naar-Site:

1. Maken van uw website aan virtuele netwerk door gebruik van deze instructies: [een virtueel netwerk met een VPN-verbinding voor Site-naar-Site in de klassieke Azure-Portal maken](vpn-gateway-site-to-site-create.md).  

2. Een dynamische routeren gateway deze instructies configureren: [een VPN-Gateway configureren](vpn-gateway-configure-vpn-gateway-mp.md). Zorg ervoor dat u Selecteer **dynamische routering** voor hun gateway.

## <a name="export"></a>2. het netwerk configuratie-bestand exporteren 

Exporteer het configuratiebestand netwerk. Het bestand dat u exporteren wordt gebruikt voor het configureren van uw nieuwe meerdere site-instellingen. Als u de instructies voor het exporteren van een bestand nodig hebt, raadpleegt u de sectie in het artikel: [het maken van een VNet met een netwerk configuratie-bestand in de Portal Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. het configuratiebestand netwerk openen

Open het netwerk configuratiebestand die u hebt gedownload in de laatste stap. Gebruik een XML-editor waarmee u tevreden bent. Het bestand moet er ongeveer als volgt uit:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. meerdere verwijzingen naar toevoegen

Wanneer u toevoegt of naslaginformatie site verwijdert, wordt u configuratiewijzigingen aanbrengen in de ConnectionsToLocalNetwork/LocalNetworkSiteRef. Een nieuwe lokale site verwijzing triggers Azure maken van een nieuwe tunnel toevoegen. In het onderstaande voorbeeld wordt de netwerkconfiguratie is van een verbinding met één site. Sla het bestand wanneer u klaar bent met het aanbrengen van wijzigingen.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. het netwerk configuratie-bestand importeren

Het netwerk configuratie-bestand importeren. Wanneer u dit bestand met de wijzigingen importeert, wordt de nieuwe tunnels toegevoegd. De tunnels wordt gebruikt in de dynamische gateway die u eerder hebt gemaakt. Als u de instructies voor het importeren van het bestand nodig hebt, raadpleegt u de sectie in het artikel: [het maken van een VNet met een netwerk configuratie-bestand in de Portal Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. toetsen downloaden

Als uw nieuwe tunnels zijn toegevoegd, gebruikt u de PowerShell-cmdlet `Get-AzureVNetGatewayKey` om de IPsec/IKE vooraf gedeelde sleutels voor elke tunnel.

Bijvoorbeeld:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Als u liever, kunt u ook de *Krijgen virtuele netwerk Gateway Shared Key* REST API gebruiken om de vooraf-gedeelde sleutels.

## <a name="7-verify-your-connections"></a>7. Controleer uw verbindingen

Controleer de status van meerdere site tunnel. Nadat de sleutels voor elke tunnel is gedownload, wilt u controleren of verbindingen. Gebruik `Get-AzureVnetConnection` om te zien virtueel netwerk tunnel, zoals wordt weergegeven in het onderstaande voorbeeld. VNet1 is de naam van de VNet.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Volgende stappen

Meer informatie over VPN Gateways, raadpleegt u [Over VPN Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md).

