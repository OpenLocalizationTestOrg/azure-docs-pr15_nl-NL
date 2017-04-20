<properties 
   pageTitle="Configureren afgedwongen tunneling voor Site-naar-Site verbindingen met het implementatiemodel klassieke | Microsoft Azure"
   description="Hoe u omleiden of 'afdwingen' alle Internet verkeer terug naar uw on-premises locatie."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Afgedwongen tunneling met het implementatiemodel klassieke configureren

> [AZURE.SELECTOR]
- [PowerShell - klassiek](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - resourcemanager](vpn-gateway-forced-tunneling-rm.md)

Afgedwongen tunneling kunt u omleiden of "dwingen" alle Internet verkeer terug naar uw on-premises locatie via een VPN-Site-naar-Site tunnel voor controle en controle. Dit is een beveiligingsvereiste kritieke voor de meeste enterprise IT beleid. 

Zonder afgedwongen tunneling, wordt Internet verkeer uit uw VMs in Azure wordt aangegeven altijd doorlopen van Azure netwerkinfrastructuur rechtstreeks af met Internet, zonder de optie toe te staan dat u wilt controleren of het verkeer wilt controleren. Onbevoegde toegang tot Internet kan mogelijk leiden tot openbaarmaking of andere soorten inbreuk op beveiliging.

In dit artikel wordt u begeleid configureren gedwongen tunneling voor virtuele netwerken die zijn gemaakt met behulp van het implementatiemodel klassieke. 

**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Implementatiemodellen en hulpprogramma's voor afgedwongen tunneling**

Een afgedwongen tunneling verbinding kan worden geconfigureerd voor zowel het implementatiemodel klassieke en het implementatiemodel resourcemanager. Zie de volgende tabel voor meer informatie. We bijwerken in deze tabel zodra nieuwe artikelen, nieuwe implementatiemodellen en extra's beschikbaar zijn voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks naar deze uit de tabel.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Vereisten en overwegingen

Afgedwongen tunneling in Azure wordt aangegeven is via virtuele netwerk door de gebruiker gedefinieerde routes (UDR) geconfigureerd. Verkeer omleiden naar een on-premises site wordt uitgedrukt als een standaard-Route naar de gateway Azure VPN. De volgende sectie bevat de huidige beperking van de routering tabel en routes voor een virtueel Azure-netwerk:


-  Elk subnet virtueel netwerk heeft een ingebouwde, systeem routeren tabel. De systeem-mailroutering tabel heeft de volgende drie groepen van routes:

    - **Lokale VNet stuurt:** Rechtstreeks naar de bestemming VMs in hetzelfde virtuele netwerk
    
    - **Op lokale routes:** Met de gateway Azure VPN
    
    - **Standaardroute:** Rechtstreeks met Internet. Pakketten bestemd is voor de privé IP-adressen niet de vorige twee routes vallen gaan verloren.


-  Met de versie van de gebruiker gedefinieerde routes, kunt u een routeren tabel als u wilt toevoegen van een standaard-route maken en vervolgens de routering tabel koppelen aan uw subnetten VNet om in te schakelen afgedwongen tunneling op deze subnetten.

- U moet een 'standaard-site instellen "tussen de lokale sites cross lokale verbinding hebt met het virtuele netwerk.

- Afgedwongen tunneling moet worden gekoppeld aan een VNet waarop een dynamische routeren VPN gateway (niet in een statische gateway).
 
- U wordt gedwongen tunneling ExpressRoute niet via deze methode is geconfigureerd, maar in plaats daarvan is ingeschakeld door een standaard-route via de ExpressRoute BGP peering sessies advertenties. Zie de [Documentatie van ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) voor meer informatie.



## <a name="configuration-overview"></a>Configuratieoverzicht

Klik in het volgende voorbeeld wordt tunnel de Frontend subnet is niet verplicht. De werkbelasting in het subnet Frontend kunnen blijven accepteren en reageren rechtstreeks op klantaanvragen van Internet. De middelgrote en Backend-subnetten gedwongen tunnel. Een uitgaande verbindingen vanuit deze twee subnetten met Internet wordt gedwongen of teruggeleid naar de site van een on-premises implementatie via een de tunnel S2S VPN.

Hiermee kunt u te beperken en internettoegang uit uw virtuele machines controleren of cloud services in Azure wordt aangegeven, terwijl u naar het inschakelen van uw service met meerdere niveaus architectuur die is vereist. Ook kunt u toepassen afgedwongen tunnel naar de hele virtuele netwerken als er geen werkbelasting internetgerichte in uw virtuele netwerken.


![U wordt gedwongen Tunneling](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u begint met de configuratie.

- Een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).

- Een geconfigureerde virtueel netwerk. 

- De meest recente versie van de Azure PowerShell-cmdlets. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.


## <a name="configure-forced-tunneling"></a>Afgedwongen tunneling configureren

De volgende procedure kunt u opgeven afgedwongen tunneling voor een virtueel netwerk. De volgende configuratiestappen uit overeenkomen met het configuratiebestand VNet-netwerk.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

In dit voorbeeld het virtuele netwerk 'MultiTier-VNet' heeft drie subnetten: *Frontend*, *Midtier*en *Backend* subnetten, met vier cross lokale verbindingen: *DefaultSiteHQ*en drie *vertakkingen*. 

De stappen wordt de *DefaultSiteHQ* instellen als de verbinding van de site standaard voor u wordt gedwongen tunneling en configureren van de Midtier en Backend-subnetten gebruik gedwongen tunneling.


1. Maak een routeren tabel. Gebruik de volgende cmdlet om uw routetabel te maken.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Een standaard-route toevoegen aan de routering tabel. 

    Het volgende voorbeeld wordt een standaard-route aan de routering tabel gemaakt in stap 1. Opmerking die de route alleen ondersteund, is het voorvoegsel bestemming van '0.0.0.0/0' naar de volgende 'VPNGateway' hop.
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Koppel de routering tabel naar de subnetten. 

    Nadat een routeren tabel gemaakt en een route toegevoegd, gebruikt u het volgende voorbeeld toevoegen of de tabel aan een subnet VNet koppelen. Het voorbeeld wordt de routetabel "MyRouteTable" aan de Midtier en Backend-subnetten van VNet MultiTier-VNet.

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Toewijzen aan een standaard-site voor u wordt gedwongen tunneling. 

    In de vorige stap, de cmdlet-voorbeeldscripts de routering tabel gemaakt en dat is gekoppeld de routetabel met twee van de subnetten VNet. De resterende stap is een lokale site tussen de verbindingen met meerdere locaties van het virtuele netwerk selecteren als de standaard-site of tunnel.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>Aanvullende PowerShell-cmdlets

### <a name="to-delete-a-route-table"></a>Een tabel verwijderen

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>Voor een overzicht van een tabel

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>Een route verwijderen uit een tabel

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>Een route verwijderen uit een subnet

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Voor een overzicht van de tabel die is gekoppeld aan een subnet
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Een standaard-site verwijderen uit een gateway VNet VPN

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>






