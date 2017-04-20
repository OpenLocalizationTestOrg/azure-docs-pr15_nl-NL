<properties
   pageTitle="Een VPN-punt-naar-Site gateway-verbinding met een Azure virtuele netwerk met behulp van de Azure portal configureren | Microsoft Azure"
   description="Veilige wijze te verbinden met uw Azure virtuele netwerk door een VPN-punt-naar-Site gateway-verbinding met behulp van de Azure portal te maken."
   services="vpn-gateway"
   documentationCenter="na"
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
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Een punt-naar-Site-verbinding met een VNet met behulp van de Azure portal configureren

> [AZURE.SELECTOR]
- [Resourcemanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resourcemanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassieke - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

In dit artikel begeleidt u tijdens het maken van een VNet met een punt-naar-Site verbinding in de klassieke implementatiemodel met behulp van de Azure portal. Een punt-naar-Site (P2S)-configuratie kunt u een beveiligde verbinding maken vanaf een afzonderlijke clientcomputer met een virtueel netwerk. Een verbinding P2S is handig als u verbinden met uw VNet vanaf een externe locatie wilt, zoals thuis of een telefonische vergadering. Of, als u slechts een paar clients die u moeten verbinding maken met een virtueel netwerk.

Punt-naar-Site verbindingen hoeven niet een VPN-apparaat of een openbare IP-adres om te werken. Een VPN-verbinding tot stand is gebracht door te starten van de verbinding van de clientcomputer. Zie voor meer informatie over verbindingen punt-naar-Site, de [VPN Gateway-Veelgestelde vragen](vpn-gateway-vpn-faq.md#point-to-site-connections) en [Over VPN Gateway](vpn-gateway-about-vpngateways.md#point-to-site).

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Implementatiemodellen en methoden voor P2S verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

De volgende tabel ziet u de twee implementatiemodellen en beschikbaar implementatiemethoden voor het P2S configuraties. Wanneer een artikel met configuratiestappen beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Eenvoudige werkstroom 

![Punt-naar--diagram trainingssite] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punt-naar-site")


De volgende secties begeleiden u via de stappen voor het maken van een beveiligde verbinding punt-naar-Site met een virtueel netwerk. 

1. Een virtueel netwerk en een VPN-gateway maken
2. Certificaten genereren
3. Upload het .cer-bestand
4. Het pakket VPN-client configuratie genereren
5. De clientcomputer configureren
6. Verbinding maken met Azure

### <a name="example-settings"></a>Voorbeeldinstellingen

U kunt het volgende voorbeeldinstellingen:

- **Naam: VNet1**
- **Adresruimte: 192.168.0.0/16**
- **De subnetnaam van de: FrontEnd**
- **Subnet adresbereiken: 192.168.1.0/24**
- **Abonnement:** Als u meer dan één abonnement hebt, controleert u of u het juiste account gebruikt.
- **Resourcegroep: TestRG**
- **Locatie: Oost-Amerikaanse**
- **Verbindingstype: punt-naar-site**
- **Client-adresruimte: 172.16.201.0/24**. VPN-clients die verbinding maken met de VNet via deze verbinding punt-naar-Site ontvangen een IP-adres van de opgegeven toepassingen.
- **GatewaySubnet: 192.168.200.0/24**. De Gateway-subnet, moet de naam "GatewaySubnet" gebruiken.
- **Grootte:** Selecteer de gateway SKU die u wilt gebruiken.
- **Routeren Type: dynamische**

## <a name="vnetvpn"></a>Sectie 1 - een virtueel netwerk en een VPN-gateway maken

### <a name="createvnet"></a>Deel 1: Een virtueel netwerk maken

Als u nog niet een virtueel netwerk, een maken. Schermafbeeldingen worden als voorbeelden gegeven. Zorg ervoor dat de waarden vervangen door uw eigen. Als u wilt een VNet maken met behulp van de Azure-portal, gebruikt u de volgende stappen uit: 

1. Via een browser, Ga naar de [portal van Azure](http://portal.azure.com) en, indien nodig, meld u aan met uw Azure-account.

2. Klik op **Nieuw**. Typ in het **vak Zoeken de marketplace** , "Virtual Network". Zoek **Virtual Network** in de resulterende lijst en klik op om het blad **Virtual Network** .

    ![Zoeken naar virtueel netwerk blade] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Zoeken naar virtueel netwerk blade")

3. Selecteer **klassieke**onderaan in het blad Virtual Network, klikt u in de lijst **Selecteer een implementatiemodel** en klik op **maken**.

    ![Selecteer implementatiemodel] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Selecteer implementatiemodel")

4. Klik op het blad **virtueel netwerk maken** door de VNet-instellingen te configureren. In dit blade voegt u uw eerste adresruimte en een bereik met één subnet. Als u klaar bent met het maken van de VNet, kunt u teruggaan en extra subnetten en adres spaties toevoegen.

    ![Maken virtueel netwerk blade] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Maken virtueel netwerk blade")

5. Controleer of het **abonnement** is de juiste is. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.

6. Klik op **Resource groeperen** en selecteer een bestaande resourcegroep of maak een nieuwe record door een naam voor de nieuwe resourcegroep te typen. Als u een nieuwe groep maakt, Geef een naam de resourcegroep op basis van de geplande configuratiewaarden. Voor meer informatie over resourcegroepen, gaat u naar [Azure resourcemanager overzicht](azure-resource-manager/resource-group-overview.md#resource-groups).

7. Selecteer vervolgens de **locatie** -instellingen voor uw VNet. De locatie bepaalt waar de resources die u dashboard naar deze VNet implementeren zich bevinden.

8. Selecteer **vastmaken aan dashboard** als u wel uw VNet terugvinden op het dashboard wilt, en klik vervolgens op **maken**.
    
    ![Vastmaken aan het dashboard] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "Vastmaken aan het dashboard")


9. Nadat u hebt geklikt maken, ziet u een tegel op het dashboard die de voortgang van uw VNet worden doorgevoerd. De tegel verandert wanneer de VNet wordt gemaakt.

    ![Tegel van virtueel netwerk maken] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Tegel van virtueel netwerk maken")

10. Nadat u uw virtuele netwerk hebt gemaakt, kunt u het IP-adres van een DNS-server kunt toevoegen om te kunnen naamresolutie verwerken. De instellingen voor uw netwerk op virtuele openen, klikt u op DNS-servers en toevoegen van het IP-adres van de DNS-server die u wilt gebruiken. Deze instelling worden geen gemaakt voor een nieuwe DNS-server. Zorg ervoor dat u een DNS-server waarop uw resources met communiceren kunnen toevoegen.

Nadat uw virtuele netwerk is gemaakt, ziet u **dat gemaakt** vermeld onder **Status** op de pagina netwerken te gebruiken in de portal van Azure klassieke.

### <a name="gateway"></a>Deel 2: Gateway subnet en een dynamische routeren gateway maken

In deze stap maakt u een gateway-subnet en een dynamische routeren gateway. Klik in de portal Azure voor het implementatiemodel klassieke kunt maken van de gateway-subnet en de gateway worden uitgevoerd via de dezelfde configuratie bladen.

1. Ga naar het virtuele netwerk waarvoor u wilt maken van een gateway in de portal.

2. Klik op het blad voor uw virtuele netwerk, klikt u op het blad **Overzicht** , klikt u in de sectie VPN verbindingen op **Gateway**.

    ![Klik hier om te maken van een gateway] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Klik hier om te maken van een gateway")


3. Selecteer op het blad **Nieuwe VPN-verbinding** **punt-naar-site**.

    ![Verbindingstype P2S] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "Verbindingstype P2S")

4. Voeg het IP-adresbereiken voor **Client-adresruimte**. Dit is het bereik van waaruit de VPN-clients een IP-adres ontvangt, wanneer u verbinding maakt. Verwijder het bereik automatisch gevulde en toevoegen aan uw eigen.

    ![Client-adresruimte] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Client-adresruimte")

5. Schakel het selectievakje **direct gateway maken** .

    ![Direct gateway maken] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Direct gateway maken")

6. Klik op **optioneel gatewayconfiguratie** als u wilt openen van het blad **de configuratie van de Gateway** .

    ![Klik op optioneel gatewayconfiguratie] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Klik op optioneel gatewayconfiguratie")

7. Klik op **Instellingen Subnet configureren vereist** om toe te voegen van de **gateway subnet**. Het is mogelijk te maken van een gateway-subnet kleinst /29, wordt u aangeraden die u maakt een grotere subnet waarin meer adressen door ten minste /28 of /27 te selecteren. Hierdoor wordt voor voldoende adressen zijn voor mogelijke extra configuraties kunt u in de toekomst.

    >[AZURE.IMPORTANT] Tijdens het werken met gateway subnetten, kunt u geen koppelen aan een netwerk-beveiligingsgroep (NSG) naar het subnet gateway. Koppelen aan een netwerk-beveiligingsgroep aan dit subnet, is het mogelijk dat uw VPN gateway niet meer werkt zoals verwacht. Zie voor meer informatie over beveiligingsgroepen netwerk, [Wat is een beveiligingsgroep netwerk?](../articles/virtual-network/virtual-networks-nsg.md)

    ![GatewaySubnet toevoegen] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "GatewaySubnet toevoegen")

8. Selecteer de **grootte**van de gateway. Dit is de gateway SKU die u gebruiken wilt voor het maken van de gateway virtueel netwerk. Klik in de portal is de standaard-SKU **eenvoudige**. Zie voor meer informatie over gateway SKU's, [Over VPN Gateway-instellingen](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![grootte van de gateway](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)

9. Selecteer het **Type routering** voor de gateway. P2S configuraties vragen om een **dynamische** routeren type. Klik op **OK** wanneer u klaar bent met het configureren van deze blade.

    ![Routeren type configureren] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Routeren type configureren")

10. Klik op het blad **Nieuwe VPN-verbinding** op **OK** onderaan in het blad om te beginnen met het maken van de gateway virtueel netwerk. Dit kan maximaal 45 minuten duren. 


## <a name="generatecerts"></a>Sectie 2 - genereren van certificaten

Certificaten worden gebruikt door Azure om VPN clients voor punt-naar-Site VPN's te verifiëren. U exporteren openbare certificaatgegevens (niet in de persoonlijke sleutel) als een Base-64 encoded X.509 .cer-bestand van een basis-certificaat dat is gegenereerd door een bedrijfsoplossing certificaat of hoofd zelfondertekend certificaat. U het certificaat voor openbare gegevens importeren uit het certificaat hoofdsite naar Azure. Daarnaast moet u een clientcertificaat vanuit de hoofdsite-certificaat voor clients genereren. Elke client die verbinding maken met het virtuele netwerk met behulp van een verbinding P2S wil moet een clientcertificaat geïnstalleerd dat is gegenereerd vanuit de hoofdsite certificaat hebben.

### <a name="cer"></a>Deel 1: De .cer-bestand naar de hoofdsite certificaat verkrijgen


Als u een bedrijfsoplossing gebruikt, kunt u uw bestaande keten certificaten. Als u geen serviceabonnement enterprise CA oplossing gebruikt, kunt u een hoofdmap zelfondertekend certificaat maken. Eén methode voor het maken van een zelfondertekend certificaat is makecert.

- Als u een enterprise certificatensysteem gebruikt, wijken de .cer-bestand naar de hoofdsite-certificaat dat u wilt gebruiken. 

- Als u een bedrijfsoplossing certificaat niet gebruikt, moet u een zelfondertekend basis-certificaat te genereren. Voor Windows 10 stappen, kunt u verwijzen naar het [werken met zelfondertekende basiscertificaten voor configuraties punt-naar-Site](vpn-gateway-certificates-point-to-site.md).

1. Als u een .cer-bestand van een certificaat, open **certmgr.msc** en zoek naar de hoofdmap. Met de rechtermuisknop op de hoofdmap zelfondertekend certificaat, klikt u op **alle taken**en klik vervolgens op **exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.

2. Klik in de Wizard op **volgende**, selecteert u **Nee, de persoonlijke sleutel niet exporteren**en klik vervolgens op **volgende**.

3. Klik op de pagina **Bestandsindeling voor Export** Selecteer **Base-64 encoded X.509 (. CER).** Klik vervolgens op **volgende**. 

4. Klik op het **bestand te exporteren**, **Blader** naar de locatie waarnaar u wilt exporteren van het certificaat. Geef het certificaatbestand voor de **bestandsnaam**. Klik vervolgens op **volgende**.

5. Klik op **Voltooien** als het certificaat wilt exporteren.


### <a name="genclientcert"></a>Deel 2: Een clientcertificaat genereren

Kunt u een unieke certificaat genereren voor elke klant die verbinding maken of kunt u hetzelfde certificaat op meerdere klanten gebruiken. Het voordeel van unieke clientcertificaten genereren is de mogelijkheid één certificaat intrekken, indien nodig. Anders als iedereen het dezelfde clientcertificaat gebruikt en u vindt dat u moet het certificaat voor één client intrekken, moet u genereren en installeren van nieuwe certificaten voor alle clients die dat certificaat gebruiken om te verifiëren.

- Als u een bedrijfsoplossing certificaat gebruikt, een clientcertificaat met de indeling van de algemene waarde genereren 'name@yourdomain.com', in plaats van de notatie 'domeinnaam\gebruikersnaam'. 

- Als u een zelfondertekend certificaat gebruikt, raadpleegt u [werken met zelfondertekende basiscertificaten voor punt-naar-Site configuraties](vpn-gateway-certificates-point-to-site.md) om een clientcertificaat te genereren.

### <a name="exportclientcert"></a>Deel 3: Exporteren de clientcertificaat

Een clientcertificaat installeren op elke computer waarop u wilt verbinden met het virtuele netwerk. Een clientcertificaat is vereist voor verificatie. U kunt automatiseren met het installeren van het clientcertificaat of kunt u dit handmatig installeren. De volgende stappen begeleiden u tot en met exporteren en het clientcertificaat handmatig installeren.

1. Als u wilt exporteren een clientcertificaat, kunt u *certmgr.msc*. Met de rechtermuisknop op de clientcertificaat dat u wilt exporteren, klikt u op **alle taken**en klik vervolgens op **exporteren**.
2. Exporteer het clientcertificaat met de persoonlijke sleutel. Dit is een *.pfx* -bestand. Zorg ervoor dat opnemen of het wachtwoord dat u hebt ingesteld voor dit certificaat (key) onthouden.

## <a name="upload"></a>Sectie 3 - Upload de hoofdmap certificaat .cer-bestand

Nadat de gateway is gemaakt, kunt u de .cer-bestand voor een vertrouwde basiscertificeringsinstantie-certificaat uploaden naar Azure. U kunt bestanden voor maximaal 20 basiscertificaten uploaden. U uploaden niet de persoonlijke sleutel voor de hoofdmap-certificaat naar Azure. Zodra de .cer-bestand wordt geüpload, wordt deze door Azure gebruikt om clients die verbinding met het virtuele netwerk maken te verifiëren.

1. In de sectie die **VPN-verbindingen** van het blad voor uw VNet, klikt u op de afbeelding **clients** op de **punt-naar-site openen VPN-verbinding** blade.

    ![Clients] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Clients")

2. Klik op de verbinding **punt-naar-site** blade, klikt u op **certificaten beheren** om te openen van het blad **certificaten** .<br>

    ![Certificaten blade](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>


3. Klik op het blad **certificaten** op **uploaden** om te openen van het blad **certificaat uploaden** .<br>

    ![Certificaten blade uploaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>


4. Klik op de afbeelding van de map om het .cer-bestand te zoeken. Selecteer het bestand en klik op **OK**. Vernieuw de pagina om te zien van de geüploade certificaat op het blad **certificaten** .

    ![Certificaat uploaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>
    

## <a name="vpnclientconfig"></a>Sectie 4 – het pakket VPN-client configuratie genereren

Als u wilt verbinden met het virtuele netwerk, moet u ook een VPN-client configureren. De clientcomputer vereist een clientcertificaat en het juiste VPN-client configuratie-pakket om te koppelen.

Het pakket VPN-client bevat configuratiegegevens om te configureren van de VPN-clientsoftware die is ingebouwd in Windows. Het pakket extra software niet geïnstalleerd. De instellingen zijn specifiek voor het virtuele netwerk waarmee u verbinding wilt maken. Voor de lijst met client-besturingssystemen die worden ondersteund, raadpleegt u de verbindingen [punt-naar-Site](vpn-gateway-vpn-faq.md#point-to-site-connections) gedeelte van de VPN Gateway Veelgestelde vragen. 

### <a name="to-generate-the-vpn-client-configuration-package"></a>Het pakket VPN-client configuratie genereren

1. Klik in de portal Azure in het blad **Overzicht** voor uw VNet, in een **VPN-verbindingen**, klikt u op de client-afbeelding op de **punt-naar-site openen VPN-verbinding** blade.
2. Aan de bovenkant van het **punt-naar-site VPN verbinding** blade, klik op het downloadpakket die overeenkomt met het besturingssysteem van client waarop deze worden geïnstalleerd:

 - Voor 64-bits-clients, selecteert u **VPN-Client (64-bits)**.
 - Voor 32-bits-clients, selecteert u **VPN-Client (32-bits)**.

    ![Configuratie voor VPN-client downloaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>


3. Hier ziet u een bericht dat Azure van de VPN-client configuratie Inpakken voor het virtuele netwerk genereren. Het pakket wordt gegenereerd na een paar minuten, en een bericht wordt weergegeven op uw lokale computer dat het pakket is gedownload. Sla het pakketbestand configuratie. Hiermee installeert u op elke clientcomputer die verbinding maken met het virtuele netwerk door gebruik te P2S.


## <a name="clientconfiguration"></a>Sectie 5: de clientcomputer configureren

### <a name="part-1-install-the-client-certificate"></a>Deel 1: Installeer het clientcertificaat

Elke clientcomputer moet een clientcertificaat om te verifiëren. Tijdens de installatie van het clientcertificaat, moet u het wachtwoord dat is gemaakt wanneer dit certificaat is geëxporteerd.

1. De .pfx-bestand kopiëren naar de clientcomputer.
2. Dubbelklik op de .pfx-bestand om het te installeren. Locatie voor de installatie niet wijzigen.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Deel 2: Installeer het pakket VPN-client configuratie

U kunt hetzelfde VPN-client configuratiepakket op elke clientcomputer, mits de versie komt overeen met de architectuur voor de client.

1. Het configuratiebestand lokaal kopiëren naar de computer die u wilt verbinden met uw netwerk virtuele en dubbelklik op het .exe-bestand. 

2. Wanneer het pakket heeft geïnstalleerd, kunt u de VPN-verbinding starten. Het configuratiepakket is niet ondertekend door Microsoft. U wilt ondertekenen van het gebruik van uw organisatie ondertekend service-pakket of uzelf met [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)ondertekenen. Is het OK als het pakket gebruiken zonder aan te melden. Echter als het pakket is niet aangemeld, een waarschuwing weergegeven wanneer u het pakket installeert.

3. Ga naar **Netwerkinstellingen** op de clientcomputer en klik op **VPN**. Hier ziet u de verbinding wordt vermeld. Deze bevat de naam van het virtuele netwerk dat deze wordt verbonden en ziet er ongeveer als volgt: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "VNet VPN-client")

## <a name="connect"></a>Sectie-6 - verbinding maken met Azure

### <a name="connect-to-your-vnet"></a>Verbinding maken met uw VNet

1. Als u wilt verbinden met uw VNet, klikt u op de clientcomputer, navigeer naar een VPN-verbindingen en zoek naar de VPN-verbinding die u hebt gemaakt. Dit is dezelfde naam als het virtuele netwerk genoemd. Klik op **verbinding maken**. Een pop-mogelijk het bericht weergegeven die verwijst naar met behulp van het certificaat. Als dit gebeurt, klikt u op **Doorgaan** om meer bevoegdheden gebruiken. 

2. Klik op **verbinding maken** om te starten van de verbinding op de statuspagina van **verbinding** . Als u een **Certificaat selecteren** -venster ziet, controleert u of het clientcertificaat met het besturingselement dat u wilt gebruiken om verbinding te. Als dit niet het geval is, gebruikt u de pijl omlaag om het juiste certificaat te selecteren en klik vervolgens op **OK**.

    ![Verbinding maken] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "VPN-clientverbinding")

3. Moet nu de verbinding tot stand worden gebracht.

    ![Established verbinding] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Verbinding maken")

### <a name="verify-the-vpn-connection"></a>Controleer de VPN-verbinding

1. Open een opdrachtprompt met verhoogde bevoegdheid om te bevestigen dat uw VPN-verbinding actief is, en *ipconfig/alle*uitvoeren.
2. De resultaten bekijken. Zoals u ziet het IP-adres dat u hebt ontvangen een van de adressen in de punt-naar-Site connectivity adresbereik dat u hebt opgegeven tijdens het maken van uw VNet. Het resultaat moet iets ongeveer als volgt:

Voorbeeld:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Volgende stappen

U kunt virtuele machines toevoegen aan uw virtuele netwerk. Lees [hoe u een aangepaste virtuele machine maken](../virtual-machines/virtual-machines-windows-classic-createportal.md).