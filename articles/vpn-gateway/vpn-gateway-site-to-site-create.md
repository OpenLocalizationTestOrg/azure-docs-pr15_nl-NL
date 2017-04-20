<properties
   pageTitle="Een virtueel netwerk maken met een site-naar-site Gateway VPN-verbinding met behulp van de Azure klassieke portal | Microsoft Azure"
   description="Een VNet maken met een S2S VPN Gateway-verbinding voor cross-premises en hybride configuraties met het implementatiemodel klassieke."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Een VNet maken met de verbinding van een Site-naar-Site met behulp van de Azure klassieke portal

> [AZURE.SELECTOR]
- [Resourcemanager - Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Resourcemanager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klassieke - klassieke Portal](vpn-gateway-site-to-site-create.md)

In dit artikel begeleidt u bij het maken van een virtueel netwerk en een site-naar-site VPN gateway verbinding met uw on-premises netwerk met het implementatiemodel klassieke en de klassieke portal. Site-naar-Site-verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.

![Diagram van de site-naar-Site] (./media/vpn-gateway-site-to-site-create/site2site.png "site-naar-site")


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Implementatiemodellen en methoden voor het Site-naar-Site-verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

De volgende tabel ziet u de momenteel beschikbaar implementatiemodellen en methoden voor het Site-naar-Site configuraties. Wanneer een artikel met configuratiestappen beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Aanvullende configuraties 

Als u VNets met elkaar verbinden wilt, raadpleegt u [een verbinding VNet-naar-VNet voor het implementatiemodel klassieke configureren](virtual-networks-configure-vnet-to-vnet-connection.md). Als u wilt dat de verbinding van een Site-naar-Site met een VNet die beschikt over een verbinding toevoegen, raadpleegt u [toevoegen een S2S verbinding met een VNet met een VPN-verbinding voor bestaande gateway](vpn-gateway-multi-site.md).
 
## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u begint met de configuratie.

- Een VPN-apparaat en iemand die kan configureert. Zie [informatie over VPN-apparaten](vpn-gateway-about-vpn-devices.md). Als u niet bekend zijn met het configureren van uw apparaat VPN of niet bekend bent met het IP-adres bereiken zich in uw on-premises netwerkconfiguratie, moet u te coördineren met iemand die deze informatie voor u geven kunt.

- Een extern omlaag openbare IP-adres voor uw apparaat VPN. Dit IP-adres kan niet worden gevonden achter een NAT bevindt.

- Een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).


## <a name="CreateVNet"></a>Maak uw virtuele netwerk

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/).

2. Klik in de linkerbenedenhoek van het scherm op **Nieuw**. Klik in het navigatiedeelvenster op **Netwerkservices**en klik vervolgens op **Virtual Network**. Klik op **Aangepaste maken** om te beginnen met de configuratiewizard.

3. Als u wilt uw VNet hebt gemaakt, voert u uw configuratie-instellingen op de volgende pagina's:

## <a name="Details"></a>De detailpagina van virtueel netwerk

Voer de volgende gegevens:

- **Naam**: het virtuele netwerk een naam. Bijvoorbeeld: *EastUSVNet*. U gebruikt deze virtuele netwerknaam wanneer u uw exemplaren VMs en PaaS implementeert, zodat u mogelijk niet wilt maken van de naam te ingewikkeld.
- **Locatie**: de locatie is rechtstreeks zijn gerelateerd aan de fysieke locatie (regio) waar u uw resources (VMs) wilt opslaan. Als u wilt dat de VMs die u met deze virtuele netwerk te bevinden fysiek in *Oost-VS*implementeert, selecteert u bijvoorbeeld die locatie. U kunt het gebied dat is gekoppeld aan uw virtuele netwerk nadat u deze hebt gemaakt niet wijzigen.

## <a name="DNS"></a>DNS-servers en VPN connectivity-pagina

Voer de volgende gegevens en klik vervolgens op de pijl-rechts op de rechterbenedenhoek.

- **DNS-Servers**: Voer de naam van de DNS-server en het IP-adres of Selecteer een eerder geregistreerde DNS-server in het snelmenu te openen. Deze instelling maakt geen een DNS-server. Dit kunt u de DNS-servers die u wilt gebruiken voor naamresolutie voor dit virtuele netwerk opgeven.
- **Site-naar-Site VPN configureren**: Schakel het selectievakje in voor **een VPN-verbinding voor de site-naar-site configureren**.
- **Lokale netwerk**: een lokaal netwerk vertegenwoordigt uw fysieke on-premises locatie. Kunt u een lokale netwerk dat u eerder hebt gemaakt, of u een nieuwe lokale netwerk kunt maken. Als u een lokale netwerk dat u eerder hebt gemaakt, gaat u naar de pagina van de configuratie **Lokale netwerken** en controleer of de VPN apparaat IP-adres (openbaar omlaag IPv4-adres) voor het apparaat VPN klopt.

## <a name="Connectivity"></a>Site-naar-site connectivity-pagina

Als u een nieuwe lokale netwerk maakt, ziet u de pagina **Site-naar-Site Connectivity** . Als u een lokale netwerk dat u eerder hebt gemaakt wilt, wordt deze pagina worden niet weergegeven in de wizard en u kunt op verplaatsen naar de volgende sectie.

Voer de volgende gegevens en klik vervolgens op de pijl-rechts.

-   **Naam**: de naam die u wilt bellen (on-premises implementatie) van uw lokale netwerk site.
-   **VPN apparaat IP-adres**: de openbare omlaag IPv4-adres van uw on-premises implementatie VPN apparaat waarmee u kunt verbinding maken met Azure. Het apparaat VPN kan niet worden gevonden achter een NAT bevindt.
-   **Adresruimte**: zijn het starten van IP- en CIDR (aantal adres). U opgeven het adres range(s) die u wilt worden verzonden via de gateway virtueel netwerk naar uw lokale on-premises locatie. Als een IP-adres binnen de bereiken die u hier opgeeft valt, wordt deze gerouteerd via de gateway virtueel netwerk.
-   **Toevoegen-adresruimte**: als er meerdere adresbereiken dat moet worden verzonden via de gateway virtueel netwerk, geeft u elke extra adresbereiken. U kunt toevoegen of verwijderen van bereiken later op de pagina **Lokale netwerk** .

## <a name="Address"></a>Virtuele netwerk adres spaties pagina

Geef het adresbereik die u wilt gebruiken voor uw virtuele netwerk. Hierna ziet u de dynamische IP-adressen (SPANNINGSDIPS) worden toegewezen aan de VMs en andere rol-exemplaren die u dashboard naar dit virtuele netwerk implementeren.

Het is met name belangrijk om te selecteren van een bereik dat geen overlap heeft met een van de bereiken die worden gebruikt voor uw on-premises netwerk. U moet afgestemd op uw netwerkbeheerder. Uw netwerkbeheerder moet mogelijk kunnen fungeren uit een bereik van IP-adressen van uw lokale netwerk-adresruimte kunt gebruiken voor uw virtuele netwerk.

Voer de volgende gegevens en klik vervolgens op het vinkje op de rechterbenedenhoek om uw netwerk te configureren.

- **Adresruimte**: zijn het IP- en adres aantal starten. Controleer of dat de adres-spaties die u opgeeft elkaar niet overlappen een van de adres-spaties die op uw on-premises netwerk.
- **Subnet toevoegen**: beginnend IP-opnemen en adres Count. Extra subnetten zijn niet verplicht, maar u kunt een afzonderlijk subnet maken voor VMs waarvoor statische SPANNINGSDIPS. Of u kunt uw VMs in een subnet die losstaat van uw andere exemplaren van de rol.
- **Gateway-subnet toevoegen**: klik om toe te voegen van de gateway-subnet. De gateway-subnet wordt alleen gebruikt voor de gateway virtueel netwerk en is vereist voor deze configuratie.

Klik op het selectievakje vanaf de onderkant van de pagina en uw virtuele netwerk maken wordt gestart. Wanneer deze is voltooid, ziet u **dat gemaakt** vermeld onder **Status** op de pagina **netwerken te gebruiken** in de klassieke Azure-Portal. Nadat de VNet is gemaakt, kunt u vervolgens de gateway virtueel netwerk configureren.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="VNetGateway"></a>De gateway virtueel netwerk configureren

Configureer de gateway virtueel netwerk om een beveiligde site op site-verbinding te maken. Zie [een netwerkgateway virtual in de portal van Azure klassieke configureren](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Volgende stappen

Nadat de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie de documentatie [virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) voor meer informatie.
