<properties
   pageTitle="Hoe u meerdere VPN-gateway naar website verbindingen toevoegt aan een virtueel netwerk voor de Resource Manager implementatiemodel met behulp van de Azure portal | Microsoft Azure"
   description="Meerdere site S2S verbindingen toevoegen aan een VPN-gateway met een bestaande verbinding"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>



# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Een Site op Site-verbinding met een VNet met een VPN-verbinding voor bestaande gateway toevoegen

> [AZURE.SELECTOR]
- [Resourcemanager - Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klassieke - PowerShell](vpn-gateway-multi-site.md)

In dit artikel begeleidt u bij met behulp van de Azure portal Site-naar-Site (S2S)-verbindingen toevoegen aan een VPN-gateway met een bestaande verbinding. Dit type verbinding wordt vaak een siteconfiguratie 'meerdere ' genoemd. 

In dit artikel kunt u een S2S-verbinding met een VNet waarop al een S2S verbinding, punt-naar-Site verbinding of VNet-naar-VNet verbinding toevoegen. Er gelden enkele beperkingen bij het toevoegen van verbindingen. Controleer de [voordat u begint](#before) sectie in dit artikel om te verifiëren voordat u de configuratie begint. 

In dit artikel is van toepassing op VNets gemaakt met behulp van het implementatiemodel resourcemanager die een gateway RouteBased VPN hebben. Deze stappen worden niet toegepast op ExpressRoute/naar website naast elkaar bestaande verbinding configuraties. Zie [ExpressRoute/S2S naast elkaar bestaande verbindingen](../expressroute/expressroute-howto-coexist-resource-manager.md) voor informatie over naast elkaar bestaande verbindingen.

### <a name="deployment-models-and-methods"></a>Implementatiemodellen en methoden

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

We in deze tabel bijwerken als nieuwe artikelen en aanvullende hulpmiddelen voor beschikbaar komen voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 


## <a name="before"></a>Voordat u begint

Controleer of u de volgende items:

- U maakt niet een ExpressRoute/S2S naast elkaar bestaande verbinding.
- U hebt een virtueel netwerk die is gemaakt met het implementatiemodel resourcemanager met een bestaande verbinding.
- De gateway virtueel netwerk voor uw VNet is RouteBased. Als u een gateway PolicyBased VPN hebt, moet u de gateway virtueel netwerk verwijderen en een nieuwe gateway VPN als RoutBased maken.
- Geen van de adresbereiken overlappen naar een of meer van de VNets die deze VNet verbinding met maakt.
- U hebt compatibele VPN apparaat en iemand die kan configureert u deze. Zie [informatie over VPN-apparaten](vpn-gateway-about-vpn-devices.md). Als u niet bekend zijn met het configureren van uw apparaat VPN of niet bekend bent met het IP-adres bereiken zich in uw on-premises netwerkconfiguratie, moet u te coördineren met iemand die deze informatie voor u geven kunt.
- Er wordt een extern omlaag openbare IP-adres voor uw apparaat VPN. Dit IP-adres kan niet worden gevonden achter een NAT bevindt.


## <a name="part1"></a>Deel 1: een verbinding configureren

1. Via een browser, Ga naar de [portal van Azure](http://portal.azure.com) en, indien nodig, meld u aan met uw Azure-account.
2. Klik op **alle resources** en zoek uw **virtueel netwerkgateway** uit de lijst met resources en klikt u erop.
3. Klik op het blad **virtuele netwerkgateway** op **verbindingen**.

    ![Verbindingen blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>

4. Klik op het blad **verbindingen** , op **+ toevoegen**.

    ![Verbindingsknop toevoegen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>

5. Vul de volgende velden op het blad **verbinding toevoegen** :
    - **Naam:** De naam die u wilt verlenen tot de site die u de verbinding met maakt.
    - **Verbindingstype:** Selecteer **Site-naar-site (IPsec)**.

    ![Verbinding blade toevoegen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>Deel 2: Voeg een gateway lokale netwerk

1. Klik op **LAN gateway** ***kiezen een gateway lokale netwerk***. Hiermee opent u het blad **kiezen lokale netwerkgateway** .

    ![Kies lokale netwerkgateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Klik op **Nieuw** om het blad **maken lokale netwerkgateway** .

    ![Lokale netwerk gateway blade maken](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>

3. Vul de volgende velden op het blad **maken lokale netwerkgateway** :
    - **Naam:** De naam die u wilt verlenen tot de bron van de gateway lokale netwerk.
    - **IP-adres:** Het openbare IP-adres van de VPN-apparaat op de site die u wilt verbinden.
    - **Adresruimte:** De adresruimte die u wilt worden gerouteerd naar de nieuwe site in de lokale netwerk.
4. Klik op **OK** op het blad **maken lokale netwerkgateway** de wijzigingen wilt opslaan.

## <a name="part3"></a>Deel 3: de gedeelde sleutel toevoegen en de verbinding maken

1. Klik op het blad **verbinding toevoegen** sleutel toevoegen met de gedeelde die u wilt gebruiken om uw verbinding te maken. U kunt krijgen van de gedeelde toets van uw apparaat VPN, of er een hier maken en configureer uw apparaat VPN als u wilt dezelfde gedeelde sleutel gebruiken. Het belangrijkste is dat de toetsen precies hetzelfde zijn.

    ![Gedeelde sleutel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Klik onderaan in het blad op **OK** om de verbinding te maken.

## <a name="part4"></a>Deel 4: Controleer de VPN-verbinding

U kunt controleren of uw VPN-verbinding in de portal of via PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## <a name="next-steps"></a>Volgende stappen

- Nadat de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie de virtuele machines [leerpad](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) voor meer informatie.
