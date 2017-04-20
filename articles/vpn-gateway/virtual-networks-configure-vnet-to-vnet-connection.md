<properties
   pageTitle="Een verbinding VNet-naar-VNet voor het implementatiemodel klassieke configureren | Microsoft Azure"
   description="Hoe u verbinding maakt Azure virtuele netwerken samenwerken met PowerShell en de Azure klassieke portal."
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
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# <a name="configure-a-vnet-to-vnet-connection-for-the-classic-deployment-model"></a>Een verbinding VNet-naar-VNet voor het implementatiemodel klassieke configureren

> [AZURE.SELECTOR]
- [Resourcemanager - Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Resourcemanager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klassieke - klassieke Portal](virtual-networks-configure-vnet-to-vnet-connection.md)



In dit artikel begeleidt u bij de stappen voor het maken en verbindt u virtuele netwerken samenwerken met het klassieke implementatiemodel (ook wel bekend als servicebeheer). De volgende stappen gebruiken de Azure klassieke portal om de VNets en gateways en PowerShell configureren van de VNet-naar-VNet-verbinding te maken. U kunt de verbinding niet configureren in de portal.

![VNet aan VNet Connectivity-Diagram](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)


### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Implementatiemodellen en methoden voor VNet-naar-VNet verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

De volgende tabel ziet u de momenteel beschikbaar implementatiemodellen en methoden voor het VNet-naar-VNet configuraties. Wanneer een artikel met configuratiestappen beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Over VNet-naar-VNet verbindingen

Een virtueel netwerk verbinden met een ander virtuele netwerk is (VNet VNet) vergelijkbaar met een virtueel netwerk verbinden met een on-premises site-locatie. Beide typen connectivity gebruiken een VPN-gateway te leveren van een beveiligde tunnel met IPsec/IKE. 

De VNets die u verbinding maakt kan zijn in verschillende-abonnementen en verschillende regio's. U kunt VNet naar VNet communicatie met meerdere locaties configuraties combineren. Hiermee kunt u stand brengen van netwerktopologieën die cross-premises-connectiviteit met onderling virtuele netwerkconnectiviteit combineren.


### <a name="why-connect-virtual-networks"></a>Waarom verbinding virtuele netwerken?

U kunt verbinding maken met virtuele netwerken voor de volgende oorzaken hebben:

- **Cross geografische regio-redundantie en geografische-aanwezigheid**
    - U kunt instellen uw eigen geografische herhaling of de synchronisatie met beveiligde verbindingen zonder te gaan via internetgerichte eindpunten.
    - Met Azure taakverdeling en Microsoft of derden clusteringtechnologie, kunt u maximaal beschikbare werkbelasting met geografische redundantie instellen tussen meerdere Azure regio's. Een belangrijke voorbeeld is voor het instellen van SQL altijd op met de beschikbaarheid van de groepen spreiden over meerdere Azure regio's.

- **Regionale meerlagige toepassingen met sterke moeten worden geïsoleerd grens**
    - Binnen hetzelfde regio, kunt u toepassingen met meerdere niveaus instellen met meerdere VNets verbonden samen met sterke moeten worden geïsoleerd en beveiligde communicatie met tussen laag.

- **Cross tussen organisatie communicatie in Azure-abonnement**
    - Als u meerdere Azure abonnementen hebt, kunt u de werkbelasting uit andere abonnementen elkaar verbinden veilig tussen virtuele netwerken.
    - Voor ondernemingen of serviceproviders, kunt u de gehele organisatie communicatie met beveiligde VPN-technologie binnen Azure inschakelen.

### <a name="vnet-to-vnet-faq-for-classic-vnets"></a>VNet-naar-VNet Veelgestelde vragen over klassieke VNets

- De virtuele netwerken kunnen zich in dezelfde of een andere abonnementen.

- De virtuele netwerken kunnen zijn in dezelfde of een andere Azure regio's (locaties).

- Een cloudservice of een eindpunt van taakverdeling kan geen bestrijkt virtuele netwerken, zelfs als ze met elkaar zijn verbonden.

- Meerdere virtuele netwerken aan elkaar koppelen, geen een VPN-apparaten vereist.

- VNet-naar-VNet ondersteunt verbindende Azure virtuele netwerken. Dit ondersteunt verbindende virtuele machines of cloud services die niet zijn geïmplementeerd in een virtueel netwerk.

- VNet-naar-VNet vereist dynamische routeren gateways. Azure statische routeren gateways worden niet ondersteund.

- Virtuele netwerkconnectiviteit kan tegelijkertijd met meerdere locaties VPN's worden gebruikt. Er is een maximum 10 VPN tunnel voor een virtueel netwerk VPN gateway verbinding maken met andere virtuele netwerken of on-premises implementatie-sites.

- De adresruimte van de virtuele netwerken en on-premises lokale netwerksites moeten niet overlappen. Overlappende adres spaties, zal het maken van virtuele netwerken of bestanden zijn geüpload netcfg configuratie mislukt.

- Overtollige tunnels tussen twee virtuele netwerken worden niet ondersteund.

- Alle VPN tunnels voor de VNet, inclusief P2S VPN's, deel de beschikbare bandbreedte voor de gateway VPN en de beschikbaarheid VPN op dezelfde gateway SLA in Azure wordt aangegeven.

- VNet-naar-VNet verkeer passeert af van de Azure backbone.


## <a name="step1"></a>Stap 1: uw IP-adresbereiken plannen

Het is belangrijk te bepalen de bereiken die u bij het configureren van uw virtuele netwerken wilt gebruiken. Voor deze configuratie, moet u ervoor zorgen dat geen VNet bereiken elkaar overlappen met elkaar, of met een van de lokale netwerken die ze verbinding met maken.

De volgende tabel ziet u een voorbeeld van het definiëren van uw VNets. Gebruik de bereiken als uitgangspunt alleen. Noteer de bereiken voor uw virtuele netwerken. Volgende stappen moet u deze informatie.

**Voorbeeldinstellingen**

|Virtual Network  |-Adresruimte               |Regio      |Een verbinding met lokale netwerk-site|
|:----------------|:---------------------------|:-----------|:-----------------------------|
|VNet1            |VNet1 (10.1.0.0/16)         |Amerikaans West     |VNet2Local (10.2.0.0/16)      |
|VNet2            |VNet2 (10.2.0.0/16)         |Japan Oost  |VNet1Local (10.1.0.0/16)      |
  
## <a name="step-2---create-vnet1"></a>Stap 2: VNet1 maken

In deze stap maken we VNet1. Als u een van de voorbeelden, moet u uw eigen waarden vervangen. Als uw VNet al bestaat, hoeft u niet te doen van deze stap. Maar u moet controleren of de IP-adresbereiken niet overlapt met het bereik voor de tweede VNet, of met een van de andere VNets waaraan u een verbinding wilt maken.

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com). In dit artikel gebruiken we de klassieke portal omdat enkele van de vereiste configuratie-instellingen nog niet beschikbaar in de portal van Azure.

2. Klik in de linkerbenedenhoek van het scherm op **Nieuw** > **Netwerkservices** > **Virtual Network** > **Maken van aangepaste** naar het begin van de configuratiewizard. Terwijl u door de wizard kunt bladeren, moet u de opgegeven waarden toevoegen aan elke pagina.

### <a name="virtual-network-details"></a>Virtual Network Details

Voer de volgende gegevens op de pagina Details van virtuele netwerk:

  ![Virtual Network Details](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **De naam van** -de naam van uw virtuele netwerk. Bijvoorbeeld: VNet1.
  - **Locatie** – wanneer u een virtueel netwerk maakt, u koppelen aan een Azure locatie (regio). Als u wilt dat uw VMs die zijn geïmplementeerd in uw virtuele netwerk te bevinden fysiek in West VS, selecteert u bijvoorbeeld die locatie. U kunt de locatie die is gekoppeld aan uw virtuele netwerk nadat u deze hebt gemaakt niet wijzigen.

### <a name="dns-servers-and-vpn-connectivity"></a>DNS-Servers en VPN-verbinding

Voer de volgende gegevens op de pagina DNS-Servers en VPN-verbinding en klik vervolgens op de pijl-rechts op de rechterbenedenhoek.

  ![DNS-Servers en VPN-verbinding](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  

- **DNS-Servers** - Voer de naam van de DNS-server en IP-adres of Selecteer een eerder geregistreerde DNS-server in de vervolgkeuzelijst. Deze instelling maakt geen een DNS-server. Dit kunt u de DNS-servers die u wilt gebruiken voor naamresolutie voor dit virtuele netwerk opgeven. Als u bevatten naamresolutie tussen uw virtuele netwerken wilt, hebt u voor het configureren van uw eigen DNS-server, in plaats van met de van die is verstrekt door Azure naamresolutie.
- Niet, selecteer een van de selectievakjes in voor P2S of S2S connectivity. Klik op de pijl in de rechterbenedenhoek om naar het volgende scherm te gaan.

### <a name="virtual-network-address-spaces"></a>Virtual Network adres spaties

Geef de adresbereiken die u wilt gebruiken voor uw virtuele netwerk op de pagina virtuele netwerk adres spaties. Hierna ziet u de dynamische IP-adressen (SPANNINGSDIPS) worden toegewezen aan de VMs en andere rol-exemplaren die u dashboard naar dit virtuele netwerk implementeren. 

Als u een VNet die u ook een verbinding met uw on-premises netwerk hebt maakt, is het vooral belangrijk is voor een bereik selecteren dat geen overlap heeft met een van de bereiken die worden gebruikt voor uw on-premises netwerk. In dat geval moet u afgestemd op uw netwerkbeheerder. Uw netwerkbeheerder moet mogelijk kunnen fungeren uit een bereik van IP-adressen van uw lokale netwerk-adresruimte kunt gebruiken voor uw VNet.

  ![Virtuele netwerk adres spaties pagina](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  - **Adresruimte** - inclusief IP starten en adres tellen. Controleer of dat de adres-spaties die u opgeeft elkaar niet overlappen met een van de adres-spaties die op uw on-premises netwerk. In dit voorbeeld gebruiken we 10.1.0.0/16 voor VNet1.
  - **Toevoegen subnet** - inclusief IP starten en adres tellen. Extra subnetten zijn niet verplicht, maar u kunt een afzonderlijk subnet maken voor VMs waarvoor statische SPANNINGSDIPS. Of u kunt uw VMs in een subnet die losstaat van uw andere exemplaren van de rol.
 
**Klik op het vinkje** op de rechterbenedenhoek van de pagina en uw virtuele netwerken begint te maken. Wanneer deze is voltooid, ziet u 'Gemaakt' weergegeven onder Status op de pagina netwerken te gebruiken.

## <a name="step-3---create-vnet2"></a>Stap 3: VNet2 maken

Vervolgens Herhaal de bovenstaande stappen voor het maken van een andere VNet. In de volgende stappen wordt u verbinding maken met de twee VNets. U kunt ook verwijzen naar de [voorbeeldinstellingen](#step1) in stap 1. Als uw VNet al bestaat, hoeft u niet te doen van deze stap. Echter u nodig hebt om te bevestigen dat de IP-adresbereiken elkaar niet overlappen met een van de andere VNets of on-premises netwerken waarmee u verbinding wilt maken.

## <a name="step-4---add-the-local-network-sites"></a>Stap 4: het lokale netwerk-sites toevoegen

Wanneer u een configuratie VNet-naar-VNet maakt, moet u lokale netwerksites, die worden weergegeven in de **Lokale netwerken** pagina van de portal configureren. De instellingen in elke site lokale netwerk Azure gebruikt om te bepalen hoe om te leiden verkeer tussen de VNets. U bepalen de naam die u wilt gebruiken om te verwijzen naar de site van elke lokale netwerk. Het is raadzaam een beschrijvende, gebruikt bij het selecteren van de waarde in een vervolgkeuzelijst in de volgende stappen.

Bijvoorbeeld VNet1 verbinding maakt met de site van een lokale netwerk die u maakt met de naam 'VNet2Local'. De instellingen voor VNet2Local bevatten de adresvoorvoegsels voor VNet2 en een openbare IP-adres van de gateway VNet2. VNet2 verbinding maakt met een lokale netwerk-site u maakt met de naam 'VNet1Local' met de adresvoorvoegsels voor VNet1 en het openbare IP-adres voor de gateway VNet1.

### <a name="localnet"></a>De site van het lokale netwerk VNet1Local toevoegen

1. Klik in de linkerbenedenhoek van het scherm op **Nieuw** > **Netwerkservices** > **Virtual Network** > **Lokale netwerk toevoegen**.

2. Klik op de pagina **details van uw lokale netwerk opgeven** voor **de naam**, voer een naam die u gebruiken wilt voor het netwerk waarmee u verbinding wilt maken. In dit voorbeeld kunt u 'VNet1Local' om te verwijzen naar de IP-adresbereiken en de gateway voor VNet1.

3. Geef een geldige openbare IP-adres voor **VPN apparaat IP-adres (optioneel)**. Meestal gebruikt u het werkelijke extern IP-adres voor een VPN-apparaat. Voor VNet-naar-VNet configuraties gebruikt u het openbare IP-adres dat is toegewezen aan de gateway voor uw VNet. Maar, gezien het feit dat u de gateway nog niet hebt gemaakt, kunt u een geldig openbare IP-adres als een tijdelijke aanduiding. Niet laat dit leeg: dit is niet optioneel voor deze configuratie. In een latere stap, Ga terug naar deze instellingen en configureert u deze met de bijbehorende gateway IP-adressen zodra Azure genereert. Klik op de pijl om naar het volgende scherm te gaan.

4. Voer op **de pagina adres opgeven**, het IP-adres bereik en adres aantal voor VNet1. Dit moet altijd exact overeen met het bereik dat is geconfigureerd voor VNet1. Azure gebruikt het IP-adresbereiken die u opgeeft om het verkeer bedoeld voor VNet1 te routeren. Klik op het vinkje om u te maken van het lokale netwerk.

### <a name="add-the-local-network-site-vnet2local"></a>De site van het lokale netwerk VNet2Local toevoegen

Gebruik de bovenstaande stappen voor het maken van de site van het lokale netwerk "VNet2Local". U kunt verwijzen naar de waarden in de [voorbeeldinstellingen](#step1) in stap 1, indien nodig.

### <a name="configure-each-vnet-to-point-to-a-local-network"></a>Elke VNet zodat deze verwijzen naar een lokaal netwerk configureren

Elke VNet moet verwijzen naar de desbetreffende lokale netwerk dat u wilt doorsturen verkeer naar. 

#### <a name="for-vnet1"></a>Voor VNet1

1. Navigeer naar de pagina **configureren** voor virtuele netwerk **VNet1**. 
2. Onder site-naar-site connectivity, selecteert u 'Verbinding maken met het lokale netwerk' en selecteer **VNet2Local** als het lokale netwerk in de vervolgkeuzelijst. 
3. Instellingen opslaan.

#### <a name="for-vnet2"></a>Voor VNet2

1. Navigeer naar de pagina **configureren** voor virtuele netwerk **VNet2**. 
2. Onder site-naar-site-connectiviteit, selecteert u 'Verbinding maken met het lokale netwerk' en selecteer **VNet1Local** in de vervolgkeuzelijst als het lokale netwerk. 
3. Instellingen opslaan.

## <a name="step-5---configure-a-gateway-for-each-vnet"></a>Stap 5: een gateway configureren voor elke VNet

Een gateway dynamische routering voor elke virtuele netwerk configureren. Deze configuratie biedt geen ondersteuning voor statische routering gateways. Als u VNets die eerder zijn geconfigureerd en die al gateways dynamische routering hebt gebruikt, moet u niet volgt deze stap. Als uw gateways statische routering, die u wilt verwijderen en maak deze opnieuw als dynamische routering gateways. Als u een gateway verwijdert, wordt het openbare IP-adres dat is toegewezen aan deze wordt uitgebracht, en moet u terug te gaan en te configureren, een van uw lokale netwerken en een VPN-apparaten met de nieuwe openbare IP-adres voor de nieuwe gateway.

1. Klik op de pagina **netwerken** controleren dat de statuskolom voor het virtuele netwerk **gemaakt is**.

2. Klik in de kolom **naam** op de naam van uw virtuele netwerk. In dit voorbeeld gebruiken we "VNet1".

3. Klik op de pagina **Dashboard** , zoals u ziet dat dit VNet geen een gateway nog geconfigureerd. Hier ziet u deze status wijzigen, zoals u Doorloop de stappen voor het configureren van de gateway.

4. Klik onder aan de pagina, op **Gateway maken** en **Dynamische routering**. Wanneer het systeem u om te bevestigen vraagt dat u wilt dat de gateway hebt gemaakt, klikt u op Ja.

    ![Gateway-type](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  

5. Als uw gateway maakt, zoals u ziet de gateway-afbeelding op de pagina wordt gewijzigd in geel en "Gateway maken" wordt gemeld dat. Het duurt meestal ongeveer 30 minuten voor de gateway te maken.

6. Herhaal dezelfde stappen voor VNet2. U kunt de eerste VNet gateway om te voltooien voordat u begint met het maken van de gateway voor uw andere VNet niet nodig hebt.

7. Wanneer de status van de gateway in 'Verbinding maken' verandert, is het openbare IP-adres voor elke gateway zichtbaar zijn in het Dashboard. Noteer het IP-adres dat overeenkomt met naar elke VNet, niet als u wilt ze verwisselt bezighoudt. Dit zijn de IP-adressen die worden gebruikt bij het bewerken van uw tijdelijke aanduiding voor IP-adressen van het apparaat VPN voor elke lokale netwerk.

## <a name="step-6---edit-the-local-network"></a>Stap 6: het lokale netwerk bewerken

1. Klik op de pagina **Lokale netwerken** klikt u op de naam van de naam van het lokale netwerk dat u wilt bewerken, klik op **bewerken** onder aan de pagina. Voor **VPN apparaat IP-adres**, het IP-adres van de gateway die met de VNet overeenkomt worden ingevoerd. Bijvoorbeeld voor VNet1Local, plaatst u het gateway IP-adres is toegewezen aan VNet1. Klik vervolgens op de pijl onder aan de pagina.

2. Klik op het selectievakje in de rechterbenedenhoek zonder wijzigingen op de pagina **Geef de adresruimte** .

## <a name="step-7---create-the-vpn-connection"></a>Stap 7: de VPN-verbinding maken

Wanneer de vorige stappen zijn voltooid, wordt de IPsec/IKE vooraf gedeelde sleutels wilt instellen en de verbinding maken. Deze reeks stappen PowerShell gebruikt en kan niet worden geconfigureerd in de portal. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de Azure PowerShell-cmdlets. Zorg ervoor dat de nieuwste versie van de Service Management (SMI)-cmdlets downloaden. 

1. Open Windows PowerShell en meld u aan.

        Add-AzureAccount

2. Selecteer het abonnement dat uw VNets bevinden zich in.

        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"

3. Maak de verbindingen. In de voorbeelden, zoals u ziet dat de gedeelde sleutel precies hetzelfde is. De gedeelde sleutel moet altijd overeenkomen met.


    VNet1 naar VNet2 verbinding

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    VNet2 naar VNet1 verbinding

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

4. Wacht totdat de verbindingen worden geïnitialiseerd. Zodra de gateway is geïnitialiseerd, is de gateway ziet eruit als de volgende afbeelding.

    ![Status van gateway - verbonden](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  

    [AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="next-steps"></a>Volgende stappen

U kunt virtuele machines toevoegen aan uw virtuele netwerken. Zie de [documentatie van virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) voor meer informatie.



[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 
