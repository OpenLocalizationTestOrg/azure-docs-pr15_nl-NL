<properties
   pageTitle="Een VPN-punt-naar-Site gateway-verbinding met een Azure virtuele netwerk met behulp van de klassieke portal configureren | Microsoft Azure"
   description="Veilige wijze te verbinden met uw Azure virtuele netwerk door een VPN-punt-naar-Site gateway-verbinding te maken."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Een punt-naar-Site-verbinding met een VNet met behulp van de klassieke portal configureren

> [AZURE.SELECTOR]
- [Resourcemanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resourcemanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassieke - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Klassieke - klassieke Portal](vpn-gateway-point-to-site-create.md)

Een punt-naar-Site (P2S)-configuratie kunt u een beveiligde verbinding maken vanaf een afzonderlijke clientcomputer met een virtueel netwerk. Een verbinding P2S is handig als u wilt verbinden met uw VNet vanaf een externe locatie, zoals thuis of een telefonische vergadering, of als er slechts een paar clients die u moeten verbinding maken met een virtueel netwerk.

In dit artikel begeleidt u tijdens het maken van een VNet met een punt-naar-Site verbinding in de **klassieke implementatiemodel** met behulp van de **klassieke portal**.

Punt-naar-Site verbindingen hoeven niet een VPN-apparaat of een openbare IP-adres om te werken. Een VPN-verbinding tot stand is gebracht door te starten van de verbinding van de clientcomputer. Zie voor meer informatie over verbindingen punt-naar-Site, de [VPN Gateway-Veelgestelde vragen](vpn-gateway-vpn-faq.md#point-to-site-connections) en de [Planning en ontwerp](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Implementatiemodellen en methoden voor P2S verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

De volgende tabel ziet u de twee implementatiemodellen en beschikbaar implementatiemethoden voor het P2S configuraties. Wanneer een artikel met configuratiestappen beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Eenvoudige werkstroom

![Punt-naar--diagram trainingssite] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "punt-naar-site")
 
De volgende stappen doorlopen u de stappen voor het maken van een beveiligde verbinding punt-naar-Site met een virtueel netwerk. 

De configuratie van een verbinding punt-naar-Site is onderverdeeld in vier secties. De volgorde waarin u elk van deze secties configureren is belangrijk. Niet, sla stappen of gehandhaafd.

- **Sectie 1** Maak een virtueel netwerk en een VPN-gateway.
- **Sectie 2** De certificaten gebruikt voor verificatie maken en uploaden.
- **Sectie 3** Exporteren en uw clientcertificaten installeren.
- **Deel 4** Configureer uw VPN-client.

## <a name="vnetvpn"></a>Sectie 1 - een virtueel netwerk en een VPN-gateway maken

### <a name="part-1-create-a-virtual-network"></a>Deel 1: Een virtueel netwerk maken

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/). Volgende stappen uit de klassieke-portal, niet de Azure-portal gebruiken. U kunt een P2S-verbinding met behulp van de Azure portal momenteel geen maken.

2. Klik in de linkerbenedenhoek van het scherm op **Nieuw**. Klik in het navigatiedeelvenster op **Netwerkservices**en klik vervolgens op **Virtual Network**. Klik op **Aangepaste maken** om te beginnen met de configuratiewizard.

3. Voer de volgende gegevens op de pagina **Details van virtuele netwerk** en klik vervolgens op de pijl-rechts op de rechterbenedenhoek.
    - **Naam**: het virtuele netwerk een naam. Bijvoorbeeld: 'VNet1'. Dit is de naam die u verwijzen moet naar wanneer u deze VNet VMs implementeert.
    - **Locatie**: de locatie is rechtstreeks zijn gerelateerd aan de fysieke locatie (regio) waar u uw resources (VMs) wilt opslaan. Als u wilt dat de VMs die u met deze virtuele netwerk te bevinden fysiek in Oost-VS implementeert, selecteert u bijvoorbeeld die locatie. U kunt het gebied dat is gekoppeld aan uw virtuele netwerk nadat u deze hebt gemaakt niet wijzigen.

4. Voer de volgende gegevens op de pagina **DNS-Servers en VPN-verbinding** en klik vervolgens op de pijl-rechts op de rechterbenedenhoek.
    - **DNS-Servers**: Voer de naam van de DNS-server en het IP-adres of Selecteer een eerder geregistreerde DNS-server in het snelmenu te openen. Deze instelling maakt geen een DNS-server. Dit kunt u de DNS-servers die u wilt gebruiken voor naamresolutie voor dit virtuele netwerk opgeven. Als u gebruiken van de Azure standaard naam resolutie-service wilt, laat u dit gedeelte leeg.
    - **VPN configureren punt-naar-Site**: Schakel het selectievakje in.

5. Geef het IP-adresbereiken waaruit uw klanten VPN een IP-adres ontvangt wanneer u verbinding hebt op de pagina **Connectivity punt-naar-Site** . Er zijn een paar regels met betrekking tot de adresbereiken die u kunt opgeven. Het is belangrijk om te bevestigen dat het bereik dat u opgeeft niet met een van de bereiken die zich op uw on-premises netwerk overlappen.

6. Voer de volgende gegevens en klik vervolgens op de pijl-rechts.
 - **Adresruimte**: eerste IP-en CIDR (aantal adres).
 - **Toevoegen-adresruimte**: Voeg adresruimte alleen als dit vereist voor het netwerkontwerp van uw is.

7. Geef de adresbereiken die u wilt gebruiken voor uw virtuele netwerk op de pagina **Virtuele netwerk adres spaties** . Hierna ziet u de dynamische IP-adressen (SPANNINGSDIPS) worden toegewezen aan de VMs en andere rol-exemplaren die u dashboard naar dit virtuele netwerk implementeren.<br><br>Het is met name belangrijk om te selecteren van een bereik dat geen overlap heeft met een van de bereiken die worden gebruikt voor uw on-premises netwerk. U moet afgestemd op uw netwerkbeheerder, die mogelijk moet u kunnen fungeren uit een bereik van IP-adressen van uw lokale netwerk-adresruimte kunt gebruiken voor uw virtuele netwerk.

8. Voer de volgende gegevens en klik vervolgens op het vinkje om te beginnen met het maken van uw virtuele netwerk.
 - **Adresruimte**: de interne IP-adresbereiken die u wilt gebruiken voor deze virtuele netwerk op, inclusief IP starten en aantal toevoegen. Het is belangrijk om te selecteren van een bereik dat geen overlap heeft met een van de bereiken die worden gebruikt voor uw on-premises netwerk. 
 - **Subnet toevoegen**: extra subnetten zijn niet verplicht, maar u kunt een afzonderlijk subnet maken voor VMs waarvoor statische SPANNINGSDIPS. Of u kunt uw VMs in een subnet die losstaat van uw andere exemplaren van de rol.
 - **Gateway-subnet toevoegen**: het subnet gateway is vereist voor een VPN-verbinding voor de punt-naar-Site. Klik om toe te voegen van de gateway-subnet. De gateway-subnet wordt alleen gebruikt voor de gateway virtueel netwerk.

9. Nadat uw virtuele netwerk is gemaakt, ziet u **dat gemaakt** vermeld onder **Status** op de pagina netwerken te gebruiken in de portal van Azure klassieke. Wanneer uw virtuele netwerk is gemaakt, kunt u uw dynamische routeren gateway maken.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Deel 2: Een dynamische routeren gateway maken

Het type gateway moet worden geconfigureerd als dynamische. Statische routeren gateways werken niet met deze functie.

1. Klik op het virtuele netwerk dat u hebt gemaakt en navigeer naar de pagina **Dashboard** in de klassieke Azure portal, klik op de pagina **netwerken te gebruiken** .

2. Klik op **Gateway maken**, zich onder aan **de dashboardpagina** . Een bericht gevraagd **wilt u een gateway voor virtuele netwerk "VNet1" maken**. Klik op **Ja** om te beginnen met het maken van de gateway. Het kan duren ongeveer 15 minuten voor de gateway te maken.

## <a name="generate"></a>Sectie 2 - genereren en uploaden van certificaten

Certificaten worden gebruikt om te verifiëren VPN-clients voor VPN's punt-naar-Site. U kunt een basis-certificaat dat is gegenereerd door een bedrijfsoplossing certificaat of kunt u een zelfondertekend certificaat. U kunt maximaal 20 basiscertificaten uploaden naar Azure. Zodra de .cer-bestand wordt geüpload, kunnen Azure de informatie in deze kunt gebruiken om te verifiëren clients die beschikken over een clientcertificaat geïnstalleerd. Dit certificaat moet worden gegenereerd op basis van hetzelfde certificaat dat staat voor het .cer-bestand.

In deze sectie wordt u het volgende doen:

- Download het .cer-bestand voor een basis-certificaat. Dit is een zelfondertekend certificaat of kunt u uw systeem enterprise-certificaat.
- Het .cer-bestand uploaden naar Azure.
- Genereer clientcertificaten.

### <a name="root"></a>Deel 1: De .cer-bestand naar de hoofdsite certificaat verkrijgen

Als u een enterprise certificatensysteem gebruikt, wijken de .cer-bestand naar de hoofdsite-certificaat dat u wilt gebruiken. In [deel 3](#createclientcert), kunt u de clientcertificaten van het certificaat hoofdsite genereren.

Als u een bedrijfsoplossing certificaat niet gebruikt, moet u een zelfondertekend basis-certificaat te genereren. Voor Windows 10 stappen, kunt u verwijzen naar het [werken met zelfondertekende basiscertificaten voor configuraties punt-naar-Site](vpn-gateway-certificates-point-to-site.md). Het artikel begeleidt u bij het gebruik van makecert voor een zelfondertekend certificaat genereren en vervolgens het .cer-bestand exporteren.

### <a name="upload"></a>Deel 2: Upload de hoofdmap certificaat .cer-bestand naar de klassieke Azure-portal

Een vertrouwd certificaat toevoegen aan Azure. Wanneer u een bestand Base64 gecodeerde X.509 (.cer) aan Azure toevoegen, geeft u aan dat Azure te vertrouwen van de hoofdsite-certificaat dat staat voor het bestand.

1. Klik in de Azure klassieke portal op de pagina **certificaten** voor uw netwerk op virtuele, klikt u op **een basis-certificaat uploaden**.

2. Blader naar het certificaat van de hoofdsite .cer op de pagina **Certificaat uploaden** en klik vervolgens op het vinkje.

### <a name="createclientcert"></a>Deel 3: Een clientcertificaat genereren

Vervolgens genereert de clientcertificaten. Kunt u een unieke certificaat genereren voor elke klant die verbinding maken of kunt u hetzelfde certificaat op meerdere klanten gebruiken. Het voordeel van unieke clientcertificaten genereren is de mogelijkheid één certificaat intrekken, indien nodig. Anders als iedereen het dezelfde clientcertificaat gebruikt en u vindt dat u moet het certificaat voor één client intrekken, moet u genereren en installeren van nieuwe certificaten voor alle clients die het certificaat gebruiken om te verifiëren.

- Als u een bedrijfsoplossing certificaat gebruikt, een clientcertificaat met de indeling van de algemene waarde genereren 'name@yourdomain.com', in plaats van de NetBIOS 'Domein\gebruikersnaam' opmaken. 

- Als u een zelfondertekend certificaat gebruikt, raadpleegt u [werken met zelfondertekende basiscertificaten voor punt-naar-Site configuraties](vpn-gateway-certificates-point-to-site.md) om een clientcertificaat te genereren.

## <a name="installclientcert"></a>Sectie 3 - exporteren en installeer het clientcertificaat

Een clientcertificaat installeren op elke computer waarop u wilt verbinden met het virtuele netwerk. Een clientcertificaat is vereist voor verificatie. U kunt automatiseren met het installeren van het clientcertificaat of u kunt handmatig installeren. De volgende stappen begeleiden u tot en met exporteren en het clientcertificaat handmatig installeren.

1. Als u wilt exporteren een clientcertificaat, kunt u *certmgr.msc*. Met de rechtermuisknop op de clientcertificaat dat u wilt exporteren, klikt u op **alle taken**en klik vervolgens op **exporteren**.
2. Exporteer het clientcertificaat met de persoonlijke sleutel. Dit is een *.pfx* -bestand. Zorg ervoor dat opnemen of het wachtwoord dat u hebt ingesteld voor dit certificaat (key) onthouden.
3. De *.pfx* -bestand kopiëren naar de clientcomputer. Dubbelklik op de *.pfx* -bestand om het te installeren op de clientcomputer. Voer het wachtwoord wanneer aangevraagd. Locatie voor de installatie niet wijzigen.

## <a name="vpnclientconfig"></a>Sectie 4 – uw VPN-client configureren

Als u wilt verbinden met het virtuele netwerk, moet u ook een VPN-client configureren. De client vereist zowel een clientcertificaat en de juiste configuratie van de VPN-client verbinding maken. Als u wilt een VPN-client configureren, moet u de volgende stappen, uitvoeren in volgorde.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Deel 1: De VPN-client configuratie-pakket maken

1. Klik in de Azure klassieke portal op de pagina **Dashboard** voor het virtuele netwerk, navigeer naar het menu snel aan te duiden in de rechterbovenhoek. Voor de lijst met client-besturingssystemen die worden ondersteund, raadpleegt u de verbindingen [punt-naar-Site](vpn-gateway-vpn-faq.md#point-to-site-connections) gedeelte van de VPN Gateway Veelgestelde vragen. Het pakket VPN-client bevat configuratiegegevens om te configureren van de VPN-clientsoftware die is ingebouwd in Windows. Het pakket extra software niet geïnstalleerd. De instellingen zijn specifiek voor het virtuele netwerk waarmee u verbinding wilt maken.<br><br>Selecteer het pakket die overeenkomt met het besturingssysteem van client waarop deze worden geïnstalleerd:
 - Voor 32-bits-clients, selecteert u **de 32-bits Client VPN downloaden**.
 - Voor 64-bits-clients, selecteert u **de 64-bits Client VPN downloaden**.

2. Het duurt een paar minuten om de clientpakket te maken. Wanneer het pakket is voltooid, kunt u het bestand downloaden. Het *.exe* -bestand dat u hebt gedownload kan veilig worden opgeslagen op uw lokale computer.

3. Nadat u genereren en het pakket VPN-client van de Azure klassieke-portal downloaden, kunt u het clientpakket installeren op de clientcomputer waaruit u wilt verbinden met uw virtuele netwerk. Als u van plan bent voor het installeren van de VPN-client-pakket voor meerdere clientcomputers, zorg er dan voor dat ze elke ook beschikken over een clientcertificaat is geïnstalleerd.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Deel 2: Installeer het pakket VPN configuratie op de client

1. Het configuratiebestand lokaal kopiëren naar de computer die u wilt verbinden met uw netwerk virtuele en dubbelklik op het .exe-bestand. 

2. Wanneer het pakket heeft geïnstalleerd, kunt u de VPN-verbinding starten. Het configuratiepakket is niet ondertekend door Microsoft. U wilt ondertekenen van het gebruik van uw organisatie ondertekend service-pakket of uzelf met [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)ondertekenen. Is het OK als het pakket gebruiken zonder aan te melden. Echter als het pakket is niet aangemeld, een waarschuwing weergegeven wanneer u het pakket installeert.

3. Ga naar **Netwerkinstellingen** op de clientcomputer en klik op **VPN**. Hier ziet u de verbinding wordt vermeld. De naam van het virtuele netwerk wordt dit weergegeven dat deze wordt verbonden en ziet er ongeveer als volgt: 

    ![VPN-client] (./media/vpn-gateway-point-to-site-create/vpn.png "VPN-client")


### <a name="part-3-connect-to-azure"></a>Deel 3: Verbinding maken met Azure

1. Als u wilt verbinden met uw VNet, klikt u op de clientcomputer, navigeer naar een VPN-verbindingen en zoek naar de VPN-verbinding die u hebt gemaakt. Dit is dezelfde naam als het virtuele netwerk genoemd. Klik op **verbinding maken**. Een pop-mogelijk het bericht weergegeven die verwijst naar met behulp van het certificaat. Als dit gebeurt, klikt u op **Doorgaan** om meer bevoegdheden gebruiken. 

2. Klik op **verbinding maken** om te starten van de verbinding op de statuspagina van **verbinding** . Als u een **Certificaat selecteren** -venster ziet, controleert u of het clientcertificaat met het besturingselement dat u wilt gebruiken om verbinding te. Als dit niet het geval is, gebruikt u de pijl omlaag om het juiste certificaat te selecteren en klik vervolgens op **OK**.

    ![VPN-client 2] (./media/vpn-gateway-point-to-site-create/clientconnect.png "VPN-clientverbinding")

3. Moet nu de verbinding tot stand worden gebracht.

    ![VPN-client 3] (./media/vpn-gateway-point-to-site-create/connected.png "VPN-clientverbinding 2")

### <a name="part-4-verify-the-vpn-connection"></a>Deel 4: Controleer de VPN-verbinding

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

Als u meer informatie over virtuele netwerken wilt, raadpleegt u de pagina [Virtuele documentatie van uw netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) .
