<properties 
   pageTitle="Een VPN-punt-naar-Site gateway-verbinding met virtuele netwerk met het implementatiemodel resourcemanager-en de portal van Azure configureren | Microsoft Azure"
   description="Veilige wijze te verbinden met uw Azure virtuele netwerk door een VPN-punt-naar-Site gateway-verbinding met resourcemanager en de Azure-portal te maken."
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
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Een punt-naar-Site-verbinding met een VNet met behulp van de Portal Azure configureren

> [AZURE.SELECTOR]
- [Resourcemanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resourcemanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassieke - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Een punt-naar-Site (P2S)-configuratie kunt u een beveiligde verbinding maken vanaf een afzonderlijke clientcomputer met een virtueel netwerk. Een verbinding P2S is handig als u wilt verbinden met uw VNet vanaf een externe locatie, zoals thuis of een telefonische vergadering, of als er slechts een paar clients die u moeten verbinding maken met een virtueel netwerk. 

Punt-naar-Site verbindingen hoeven niet een VPN-apparaat of een openbare IP-adres om te werken. Een VPN-verbinding tot stand is gebracht door te starten van de verbinding van de clientcomputer. Zie voor meer informatie over verbindingen punt-naar-Site, de [VPN Gateway-Veelgestelde vragen](vpn-gateway-vpn-faq.md#point-to-site-connections) en de [Planning en ontwerp](vpn-gateway-plan-design.md).

In dit artikel begeleidt u tijdens het maken van een VNet met de verbinding van een punt-naar-Site in het resourcemanager implementatiemodel met behulp van de Azure portal.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Implementatiemodellen en methoden voor P2S verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

De volgende tabel ziet u de twee implementatiemodellen en beschikbaar implementatiemethoden voor het P2S configuraties. Wanneer een artikel met configuratiestappen beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Eenvoudige werkstroom 

![Punt-naar--diagram trainingssite] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punt-naar-site")

### <a name="example"></a>Voorbeeldwaarden

- **Naam: VNet1**
- **Adresruimte: 192.168.0.0/16**<br>In dit voorbeeld gebruiken we slechts één-adresruimte. U kunt meer dan één adresruimte hebben voor uw VNet.
- **De subnetnaam van de: FrontEnd**
- **Subnet adresbereiken: 192.168.1.0/24**
- **Abonnement:** Als u meer dan één abonnement hebt, controleert u of u het juiste account gebruikt.
- **Resourcegroep: TestRG**
- **Locatie: Oost-Amerikaanse**
- **GatewaySubnet: 192.168.200.0/24**
- **Gateway-virtuele netwerknaam: VNet1GW**
- **Gateway-type: VPN**
- **Type VPN: Route gebaseerde**
- **Openbare IP-adres: VNet1GWpip**
- **Verbindingstype: punt-naar-site**
- **Adres van client-toepassingen: 172.16.201.0/24**<br>VPN-clients die verbinding maken met de VNet via deze verbinding punt-naar-Site ontvangen een IP-adres van de groep client adressen.

## <a name="before-beginning"></a>Voordat u begint

- Controleer of u een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).
    
## <a name="createvnet"></a>Deel 1: Maak een virtueel netwerk

Als u deze configuratie als oefening maakt, kunt u verwijzen naar de [voorbeeldwaarden](#example).

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2. extra adresruimte en subnetten toevoegen

U kunt extra adresruimte en subnetten toevoegen aan uw VNet zodra deze is gemaakt.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3. een subnet gateway maken

Voordat u verbinding met uw netwerk virtuele een gateway, moet u eerst de gateway subnet maken voor het virtuele netwerk waarnaar u een verbinding wilt maken. Indien mogelijk, is het raadzaam te maken van een gateway subnet met een CIDR blok /28 of /27 kan alleen worden aangeboden voldoende IP-adressen zodat aanvullende toekomstige configuratievereisten.

De schermafbeeldingen in deze sectie worden weergegeven als een voorbeeld van de verwijzing. Zorg ervoor dat het bereik voor het adres van GatewaySubnet die overeenkomt met de vereiste waarden voor de configuratie gebruiken.

#### <a name="to-create-a-gateway-subnet"></a>Een gateway-subnet maken

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="dns"></a>4. Geef een DNS-server (optioneel)

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Deel 2: Maak een virtueel netwerkgateway

Punt-naar-site verbindingen vereisen de volgende instellingen:

- Gateway-type: VPN
- Type VPN: Route gebaseerde

### <a name="to-create-a-virtual-network-gateway"></a>Een gateway virtueel netwerk maken

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Deel 3 – genereren van certificaten

Certificaten worden gebruikt door Azure om VPN clients voor punt-naar-Site VPN's te verifiëren. U exporteren openbare certificaatgegevens (niet in de persoonlijke sleutel) als een Base-64 encoded X.509 .cer-bestand van een basis-certificaat dat is gegenereerd door een bedrijfsoplossing certificaat of hoofd zelfondertekend certificaat. U het certificaat voor openbare gegevens importeren uit het certificaat hoofdsite naar Azure. Daarnaast moet u een clientcertificaat vanuit de hoofdsite-certificaat voor clients genereren. Elke client die verbinding maken met het virtuele netwerk met behulp van een verbinding P2S wil moet een clientcertificaat geïnstalleerd dat is gegenereerd vanuit de hoofdsite certificaat hebben.

### <a name="getcer"></a>1. de .cer-bestand naar de hoofdsite certificaat verkrijgen

Als u een bedrijfsoplossing gebruikt, kunt u uw bestaande keten certificaten. Als u geen serviceabonnement enterprise CA oplossing gebruikt, kunt u een hoofdmap zelfondertekend certificaat maken. Eén methode voor het maken van een zelfondertekend certificaat is makecert.

- Als u een enterprise certificatensysteem gebruikt, wijken de .cer-bestand naar de hoofdsite-certificaat dat u wilt gebruiken. 

- Als u een bedrijfsoplossing certificaat niet gebruikt, moet u een zelfondertekend basis-certificaat te genereren. Voor Windows 10 stappen, kunt u verwijzen naar het [werken met zelfondertekende basiscertificaten voor configuraties punt-naar-Site](vpn-gateway-certificates-point-to-site.md).

1. Als u een .cer-bestand van een certificaat, open **certmgr.msc** en zoek naar de hoofdmap. Met de rechtermuisknop op de hoofdmap zelfondertekend certificaat, klikt u op **alle taken**en klik vervolgens op **exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.

2. Klik in de Wizard op **volgende**, selecteert u **Nee, de persoonlijke sleutel niet exporteren**en klik vervolgens op **volgende**.

3. Klik op de pagina **Bestandsindeling voor Export** Selecteer **Base-64 encoded X.509 (. CER).** Klik vervolgens op **volgende**. 

4. Klik op het **bestand te exporteren**, **Blader** naar de locatie waarnaar u wilt exporteren van het certificaat. Geef het certificaatbestand voor de **bestandsnaam**. Klik vervolgens op **volgende**.

5. Klik op **Voltooien** als het certificaat wilt exporteren.

### <a name="generateclientcert"></a>2. een clientcertificaat genereren

Kunt u een unieke certificaat genereren voor elke klant die verbinding maken of kunt u hetzelfde certificaat op meerdere klanten gebruiken. Het voordeel van unieke clientcertificaten genereren is de mogelijkheid één certificaat intrekken, indien nodig. Anders als iedereen het dezelfde clientcertificaat gebruikt en u vindt dat u moet het certificaat voor één client intrekken, moet u genereren en installeren van nieuwe certificaten voor alle clients die dat certificaat gebruiken om te verifiëren.

- Als u een bedrijfsoplossing certificaat gebruikt, een clientcertificaat met de indeling van de algemene waarde genereren 'name@yourdomain.com', in plaats van de notatie 'domeinnaam\gebruikersnaam'. 

- Als u een zelfondertekend certificaat gebruikt, raadpleegt u [werken met zelfondertekende basiscertificaten voor punt-naar-Site configuraties](vpn-gateway-certificates-point-to-site.md) om een clientcertificaat te genereren.

### <a name="exportclientcert"></a>3. het clientcertificaat exporteren

Een clientcertificaat is vereist voor verificatie. Na het clientcertificaat worden gegenereerd, door deze te exporteren. Het clientcertificaat dat u exporteren wordt later geïnstalleerd op elke clientcomputer.

1. Als u wilt exporteren een clientcertificaat, kunt u *certmgr.msc*. Met de rechtermuisknop op de clientcertificaat dat u wilt exporteren, klikt u op **alle taken**en klik vervolgens op **exporteren**.
2. Exporteer het clientcertificaat met de persoonlijke sleutel. Dit is een *.pfx* -bestand. Zorg ervoor dat opnemen of het wachtwoord dat u hebt ingesteld voor dit certificaat (key) onthouden.

## <a name="addresspool"></a>Deel 4: Voeg de groep client adressen

1. Nadat de gateway virtueel netwerk is gemaakt, gaat u naar de sectie **Instellingen** van het blad van de gateway virtueel netwerk. Klik in het gedeelte **Instellingen** op **punt-naar-site configuratie** openen van het blad **configuratie** .

    ![Wijs site blade] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "Wijs site blade")

2. **Groep adressen** is de groep met IP-adressen waaruit clients die verbinding maken met een IP-adres, ontvangt. De groep adressen toevoegen en klik vervolgens op **Opslaan**.

    ![adres van client-toepassingen] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "adres van client-toepassingen")

## <a name="uploadfile"></a>Deel 5 – de hoofdmap certificaat .cer-bestand uploaden

Nadat de gateway is gemaakt, kunt u de .cer-bestand voor een vertrouwde basiscertificeringsinstantie-certificaat uploaden naar Azure. U kunt bestanden voor maximaal 20 basiscertificaten uploaden. U uploaden niet de persoonlijke sleutel voor de hoofdmap-certificaat naar Azure. Zodra de .cer-bestand wordt geüpload, wordt deze door Azure gebruikt om clients die verbinding met het virtuele netwerk maken te verifiëren.

1. Navigeer naar de configuratie **punt-naar-site** blade. U toevoegen de .cer-bestanden in de sectie **hoofdsite certificaat** van deze blade.

    ![Wijs site blade] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "Wijs site blade")

2. Zorg ervoor dat u het certificaat hoofdsite geëxporteerd, zoals een Base-64 encoded X.509 (.cer)-bestand. Moet u deze exporteren in deze indeling, zodat u het certificaat met teksteditor openen kunt.
3. Open het certificaat met een teksteditor, zoals Kladblok. Kopieer alleen in de volgende sectie.

    ![certificaatgegevens] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "certificaatgegevens")

4. Plak de certificaatgegevens in het gedeelte van het **Certificaat in openbare gegevens** van de portal. De naam van het certificaat in de ruimte **naam** plaatsen en klik vervolgens op **Opslaan**. U kunt maximaal 20 vertrouwde basiscertificaten toevoegen.

    ![certificaat uploaden] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "certificaat uploaden")

## <a name="clientconfig"></a>Deel 6 - Download en installeer het pakket VPN-client configuratie

Clients verbinding maken met Azure met P2S moeten hebben zowel een clientcertificaat en een VPN-client configuratiepakket is geïnstalleerd. VPN-client configuratie-pakketten zijn beschikbaar voor Windows-clients. 

Het pakket VPN-client bevat informatie configureren van de VPN-client-software die is ingebouwd in Windows. De configuratie hoort bij de VPN waarmee u verbinding wilt maken. Het pakket extra software niet geïnstalleerd. Raadpleeg de [VPN Gateway-Veelgestelde vragen](vpn-gateway-vpn-faq.md#point-to-site-connections) voor meer informatie.

1. Klik op de configuratie **punt-naar-site** blade, klik op **downloaden VPN-client** als u wilt openen van het blad **downloaden VPN-client** .

    ![VPN-client downloaden] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "VPN-client downloaden")

2. Selecteer het juiste pakket voor de client en klik op **downloaden**. Selecteer **AMD64**voor 64-bits-clients. Selecteer **x86**voor 32-bits-clients.

3. Het pakket installeren op de clientcomputer. Als er een pop-up SmartScreen-, klik op **meer info**, vervolgens **toch uitvoeren** om te kunnen installeren van het pakket.

4. Ga naar **Netwerkinstellingen** op de clientcomputer en klik op **VPN**. Hier ziet u de verbinding wordt vermeld. Dit wordt de naam van het virtuele netwerk dat deze verbinding met en Hiermee wordt gezocht zoals in dit voorbeeld weergegeven: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN-client")

## <a name="installclientcert"></a>Deel 7 - installatie het clientcertificaat

Elke clientcomputer moet een clientcertificaat om te verifiëren. Tijdens de installatie van het clientcertificaat, moet u het wachtwoord dat is gemaakt wanneer dit certificaat is geëxporteerd.

1. De .pfx-bestand kopiëren naar de clientcomputer.
2. Dubbelklik op de .pfx-bestand om het te installeren. Locatie voor de installatie niet wijzigen.

## <a name="connect"></a>Deel van 8 - verbinding maken met Azure

1. Als u wilt verbinden met uw VNet, klikt u op de clientcomputer, navigeer naar een VPN-verbindingen en zoek naar de VPN-verbinding die u hebt gemaakt. Dit is dezelfde naam als het virtuele netwerk genoemd. Klik op **verbinding maken**. Een pop-mogelijk het bericht weergegeven die verwijst naar met behulp van het certificaat. Als dit gebeurt, klikt u op **Doorgaan** om meer bevoegdheden gebruiken. 

2. Klik op **verbinding maken** om te starten van de verbinding op de statuspagina van **verbinding** . Als u een **Certificaat selecteren** -venster ziet, controleert u of het clientcertificaat met het besturingselement dat u wilt gebruiken om verbinding te. Als dit niet het geval is, gebruikt u de pijl omlaag om het juiste certificaat te selecteren en klik vervolgens op **OK**.

    ![VPN-client 2] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN-clientverbinding")

3. Moet nu de verbinding tot stand worden gebracht.

    ![VPN-client 3] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN-clientverbinding 2")

## <a name="verify"></a>Deel 9 – uw verbinding controleren

1. Open een opdrachtprompt met verhoogde bevoegdheid om te bevestigen dat uw VPN-verbinding actief is, en *ipconfig/alle*uitvoeren.

2. De resultaten bekijken. Zoals u ziet het IP-adres dat u hebt ontvangen een van de adressen in de punt-naar-Site VPN-Client groep adressen die u hebt opgegeven in de configuratie. Het resultaat moet iets ongeveer als volgt:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="add"></a>Toevoegen aan of verwijderen van vertrouwde basiscertificaten

U kunt vertrouwde basiscertificeringsinstantie certificaat verwijderen uit Azure. Wanneer u een vertrouwd certificaat verwijdert, is de clientcertificaten die zijn gegenereerd op basis van het certificaat hoofdsite niet langer verbinding maken met Azure via punt-naar-Site. Als u clients verbinding maakt wilt, moeten ze een nieuwe clientcertificaat dat is gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure installeren.

U kunt de lijst met ingetrokken certificaten van de configuratie **punt-naar-site beheren** blade. Dit is het blad dat u voor het [uploaden van een vertrouwde basiscertificeringsinstantie-certificaat gebruikt](#uploadfile).

## <a name="revokeclient"></a>Voor het beheren van de lijst met ingetrokken certificaten

U kunt clientcertificaten intrekken. De certificaatintrekkingslijsten kunt u selectief weigeren punt-naar-Site connectivity op basis van individuele clientcertificaten. Als u een hoofdmap certificaat .cer uit Azure verwijdert, trekt deze u de toegang voor alle clientcertificaten die zijn gegenereerd/ondertekend door het certificaat ingetrokken hoofdmap. Als u wilt intrekken van een bepaald clientcertificaat, niet de hoofdsite en kunt u doen. De certificaten die zijn gegenereerd op basis van het certificaat van de hoofdsite nog steeds worden op deze manier zijn geldig. 

De algemene manier is om het gebruik van de hoofdsite-certificaat voor het beheren van access op team of organisatie niveaus, met ingetrokken certificaten for fijnmazige toegangsbeheer voor individuele gebruikers.

U kunt de lijst met ingetrokken certificaten van de configuratie **punt-naar-site beheren** blade. Dit is het blad dat u voor het [uploaden van een vertrouwde basiscertificeringsinstantie-certificaat gebruikt](#uploadfile).


## <a name="next-steps"></a>Volgende stappen

U kunt een virtuele machine toevoegen aan uw virtuele netwerk. Zie [maken een virtuele Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.