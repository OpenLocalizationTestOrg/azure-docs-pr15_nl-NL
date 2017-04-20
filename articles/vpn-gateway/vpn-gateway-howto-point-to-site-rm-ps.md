<properties 
   pageTitle="Een VPN-punt-naar-Site gateway-verbinding met virtuele netwerk door gebruik van het implementatiemodel resourcemanager configureren | Microsoft Azure"
   description="Veilige wijze te verbinden met uw Azure virtuele netwerk door een VPN-punt-naar-Site gateway-verbinding te maken."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Een punt-naar-Site-verbinding met een VNet via PowerShell configureren

> [AZURE.SELECTOR]
- [Resourcemanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resourcemanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassieke - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Een punt-naar-Site (P2S)-configuratie kunt u een beveiligde verbinding maken vanaf een afzonderlijke clientcomputer met een virtueel netwerk. Een verbinding P2S is handig als u wilt verbinden met uw VNet vanaf een externe locatie, zoals thuis of een telefonische vergadering, of als er slechts een paar clients die u moeten verbinding maken met een virtueel netwerk. 

Punt-naar-Site verbindingen hoeven niet een VPN-apparaat of een openbare IP-adres om te werken. Een VPN-verbinding tot stand is gebracht door te starten van de verbinding van de clientcomputer. Zie voor meer informatie over verbindingen punt-naar-Site, de [VPN Gateway-Veelgestelde vragen](vpn-gateway-vpn-faq.md#point-to-site-connections) en de [Planning en ontwerp](vpn-gateway-plan-design.md). 

In dit artikel begeleidt u tijdens het maken van een VNet met de verbinding van een punt-naar-Site in het resourcemanager implementatiemodel via PowerShell.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Implementatiemodellen en methoden voor P2S verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

De volgende tabel ziet u de twee implementatiemodellen en beschikbaar implementatiemethoden voor het P2S configuraties. Wanneer een artikel met configuratiestappen beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Eenvoudige werkstroom 

![Punt-naar--diagram trainingssite] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punt-naar-site")

In dit scenario maakt u een virtueel netwerk met een verbinding punt-naar-Site. De instructies kunt u ook certificaten die vereist voor deze configuratie zijn genereren. Een verbinding P2S bestaat uit de volgende items: A VNet met een VPN-gateway, een hoofdsite certificaat .cer-bestand (openbare sleutel), een clientcertificaat en de configuratie van de VPN op de client. 

We gebruiken de volgende waarden voor deze configuratie. We instellen de variabelen in de sectie [1](#declare) van het artikel. U kunt via de stappen als een overzicht en het gebruik van de waarden niet wijzigen of wijzigt u deze zodat uw omgeving. 

### <a name="example"></a>Voorbeeldwaarden

- **Naam: VNet1**
- **Adresruimte: 192.168.0.0/16** en **10.254.0.0/16**<br>In dit voorbeeld we meer dan één adresruimte gebruiken om te laten zien dat deze configuratie met meerdere adres spaties werkt. Meerdere adres spaties zijn echter niet vereist voor deze configuratie.
- **De subnetnaam van de: FrontEnd**
    - **Subnet adresbereiken: 192.168.1.0/24**
- **De subnetnaam van de: BackEnd**
    - **Subnet adresbereiken: 10.254.1.0/24**
- **Subnetnaam: GatewaySubnet**<br>De naam van de Subnet *GatewaySubnet* is verplicht voor de gateway VPN om te werken.
    - **Subnet adresbereiken: 192.168.200.0/24** 
- **Groep VPN-client-adressen: 172.16.201.0/24**<br>VPN-clients die verbinding maken met de VNet via deze verbinding punt-naar-Site ontvangen een IP-adres van de groep VPN-client adressen.
- **Abonnement:** Als u meer dan één abonnement hebt, controleert u of u het juiste account gebruikt.
- **Resourcegroep: TestRG**
- **Locatie: Oost-Amerikaanse**
- **DNS-Server: IP-adres** van de DNS-server die u wilt gebruiken om te zetten.
- **De naam van de GW: Vnet1GW**
- **Openbare IP-naam: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Voordat u begint

- Controleer of u een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).
    
- Installeer de meest recente versie van de Azure resourcemanager PowerShell-cmdlets. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets. Als u werkt met PowerShell voor deze configuratie, zorg dat u als administrator worden uitgevoerd. 

## <a name="declare"></a>Deel 1 – Log in en variabelen instellen

In deze sectie, meld u aan en de waarden voor deze configuratie declareren. De opgegeven waarden worden in de voorbeeldscripts gebruikt. Wijzig de waarden zodat uw eigen omgeving. Of u kunt de opgegeven waarden gebruiken en Ga door de stappen als oefening.

1. In de PowerShell-console, meld u aan bij uw Azure-account. Deze cmdlet wordt u gevraagd de aanmeldingsreferenties voor uw Azure-Account. Na het aanmelden, downloaden het uw accountinstellingen, zodat ze beschikbaar voor Azure PowerShell zijn.

        Login-AzureRmAccount 

2. Een lijst met uw Azure abonnementen ophalen.

        Get-AzureRmSubscription

3. Geef het abonnement waaraan u wilt gebruiken. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Declareren de variabelen die u wilt gebruiken. Gebruik in het onderstaande voorbeeld, wordt vervangen door de waarden voor uw eigen indien nodig.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="ConfigureVNet"></a>Deel 2: een VNet configureren 

1. Een resourcegroep maken.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Maak de subnet configuraties voor het virtuele netwerk, ze een naam *front-end*en *back-end* *GatewaySubnet*. Deze voorvoegsels moeten deel uitmaken van de VNet-adresruimte die u gedeclareerd.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Het virtuele netwerk maken. De DNS-server opgegeven moet een DNS-server die de namen voor de resources die u verbinding met maakt kunt oplossen. In dit voorbeeld hebben we een openbare IP-adres gebruikt. Zorg ervoor dat u uw eigen waarden gebruiken.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Geef de variabelen voor het virtuele netwerk dat u hebt gemaakt.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Een dynamisch toegewezen openbare IP-adres aanvragen. Dit IP-adres is vereist voor de gateway werkt alleen naar behoren. Later verbinding met de gateway bij naar de gateway IP-configuratie.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="Certificates"></a>Deel 3 – certificaten

Certificaten worden gebruikt door Azure om VPN clients voor punt-naar-Site VPN's te verifiëren. U exporteren openbare certificaatgegevens (niet in de persoonlijke sleutel) als een Base-64 encoded X.509 .cer-bestand van een basis-certificaat dat is gegenereerd door een bedrijfsoplossing certificaat of hoofd zelfondertekend certificaat. U het certificaat voor openbare gegevens importeren uit het certificaat hoofdsite naar Azure. Daarnaast moet u een clientcertificaat vanuit de hoofdsite-certificaat voor clients genereren. Elke client die verbinding maken met het virtuele netwerk met behulp van een verbinding P2S wil moet een clientcertificaat geïnstalleerd dat is gegenereerd vanuit de hoofdsite certificaat hebben.

### <a name="cer"></a>1. de .cer-bestand naar de hoofdsite certificaat verkrijgen

Moet u toegang tot de openbare certificaatgegevens naar de hoofdsite-certificaat dat u wilt gebruiken.

- Als u een enterprise certificatensysteem gebruikt, wijken de .cer-bestand naar de hoofdsite-certificaat dat u wilt gebruiken. 

- Als u een bedrijfsoplossing certificaat niet gebruikt, moet u een zelfondertekend basis-certificaat te genereren. Voor Windows 10 stappen, kunt u verwijzen naar het [werken met zelfondertekende basiscertificaten voor configuraties punt-naar-Site](vpn-gateway-certificates-point-to-site.md).


1. Als u een .cer-bestand van een certificaat, open **certmgr.msc** en zoek naar de hoofdmap. Met de rechtermuisknop op de hoofdmap zelfondertekend certificaat, klikt u op **alle taken**en klik vervolgens op **exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.

2. Klik in de Wizard op **volgende**, selecteert u **Nee, de persoonlijke sleutel niet exporteren**en klik vervolgens op **volgende**.

3. Klik op de pagina **Bestandsindeling voor Export** Selecteer **Base-64 encoded X.509 (. CER).** Klik vervolgens op **volgende**. 

4. Klik op het **bestand te exporteren**, **Blader** naar de locatie waarnaar u wilt exporteren van het certificaat. Geef het certificaatbestand voor de **bestandsnaam**. Klik vervolgens op **volgende**.

5. Klik op **Voltooien** als het certificaat wilt exporteren.

### <a name="generate"></a>2. het clientcertificaat genereren

Vervolgens genereert de clientcertificaten. Kunt u een unieke certificaat genereren voor elke klant die verbinding maken of kunt u hetzelfde certificaat op meerdere klanten gebruiken. Het voordeel van unieke clientcertificaten genereren is de mogelijkheid één certificaat intrekken, indien nodig. Anders als iedereen het dezelfde clientcertificaat gebruikt en u vindt dat u moet het certificaat voor één client intrekken, moet u genereren en installeren van nieuwe certificaten voor alle clients die het certificaat gebruiken om te verifiëren. De certificaten zijn op elke clientcomputer verderop in deze oefening geïnstalleerd.

- Als u een bedrijfsoplossing certificaat gebruikt, een clientcertificaat met de indeling van de algemene waarde genereren 'name@yourdomain.com', in plaats van de NetBIOS 'Domein\gebruikersnaam' opmaken. 

- Als u een zelfondertekend certificaat gebruikt, raadpleegt u [werken met zelfondertekende basiscertificaten voor punt-naar-Site configuraties](vpn-gateway-certificates-point-to-site.md) om een clientcertificaat te genereren.

### <a name="exportclientcert"></a>3. het clientcertificaat exporteren

Een clientcertificaat is vereist voor verificatie. Na het clientcertificaat worden gegenereerd, door deze te exporteren. Het clientcertificaat dat u exporteren wordt later geïnstalleerd op elke clientcomputer.

1. Als u wilt exporteren een clientcertificaat, kunt u *certmgr.msc*. Met de rechtermuisknop op de clientcertificaat dat u wilt exporteren, klikt u op **alle taken**en klik vervolgens op **exporteren**.
2. Exporteer het clientcertificaat met de persoonlijke sleutel. Dit is een *.pfx* -bestand. Zorg ervoor dat opnemen of het wachtwoord dat u hebt ingesteld voor dit certificaat (key) onthouden.

### <a name="upload"></a>4. de hoofdmap certificaat .cer-bestand uploaden

De variabele voor de certificaatnaam van uw, de waarde vervangen door uw eigen declareren:

        $P2SRootCertName = "Mycertificatename.cer"

De openbare certificaatgegevens naar de hoofdsite-certificaat toevoegen aan Azure. U kunt bestanden voor maximaal 20 basiscertificaten uploaden. U uploaden niet de persoonlijke sleutel voor de hoofdmap-certificaat naar Azure. Zodra de .cer-bestand wordt geüpload, wordt deze door Azure gebruikt om clients die verbinding met het virtuele netwerk maken te verifiëren. 

Het bestandspad vervangen door uw eigen en klik vervolgens op de cmdlets.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Deel 4 – de gateway VPN maken

Configureer en maken van de gateway virtueel netwerk voor uw VNet. De *-GatewayType* moet **Vpn** en de *-VpnType* **RouteBased**moet zijn. Dit kan maximaal 45 minuten duren.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Deel 5 – de VPN-client configuratie downloaden

Clients verbinding maken met Azure met P2S hebt zowel een clientcertificaat en een VPN-client configuratiepakket is geïnstalleerd. VPN-client configuratie-pakketten zijn beschikbaar voor Windows-clients. Het pakket VPN-client bevat informatie configureren van de VPN-client-software die is ingebouwd in Windows en specifiek is voor de VPN waarmee u verbinding wilt maken. Het pakket extra software niet geïnstalleerd. Raadpleeg de [VPN Gateway-Veelgestelde vragen](vpn-gateway-vpn-faq.md#point-to-site-connections) voor meer informatie.

1. Nadat de gateway is gemaakt, kunt u het pakket van de configuratie client downloaden. In dit voorbeeld is het pakket gedownload voor 64-bits-clients. Als u downloaden van de 32-bits-client wilt, vervangt u 'Amd64' door 'x86'. U kunt ook de VPN-client downloaden met behulp van de Azure-portal.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. De PowerShell-cmdlet retourneert een URL-koppeling. Dit is een voorbeeld van wat de resulterende URL ziet ernaar uit dat:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Kopieer en plak de koppeling die naar een webbrowser naar de downloaden wordt geretourneerd. Installeer het pakket op de clientcomputer. Als er een pop-up SmartScreen-, klik op **meer info**, vervolgens **toch uitvoeren** om te kunnen installeren van het pakket.

4. Ga naar **Netwerkinstellingen** op de clientcomputer en klik op **VPN**. Hier ziet u de verbinding wordt vermeld. Dit wordt de naam van het virtuele netwerk dat deze verbinding met en Hiermee wordt gezocht zoals in dit voorbeeld weergegeven: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN-client")


## <a name="clientcertificate"></a>Deel 6 - installatie het clientcertificaat

Elke clientcomputer moet een clientcertificaat om te verifiëren. Tijdens de installatie van het clientcertificaat, moet u het wachtwoord dat is gemaakt wanneer dit certificaat is geëxporteerd.

1. De .pfx-bestand kopiëren naar de clientcomputer.
2. Dubbelklik op de .pfx-bestand om het te installeren. Locatie voor de installatie niet wijzigen.

## <a name="connect"></a>Deel van 7 - verbinding maken met Azure

1. Als u wilt verbinden met uw VNet, klikt u op de clientcomputer, navigeer naar een VPN-verbindingen en zoek naar de VPN-verbinding die u hebt gemaakt. Dit is dezelfde naam als het virtuele netwerk genoemd. Klik op **verbinding maken**. Een pop-mogelijk het bericht weergegeven die verwijst naar met behulp van het certificaat. Als dit gebeurt, klikt u op **Doorgaan** om meer bevoegdheden gebruiken. 

2. Klik op **verbinding maken** om te starten van de verbinding op de statuspagina van **verbinding** . Als u een **Certificaat selecteren** -venster ziet, controleert u of het clientcertificaat met het besturingselement dat u wilt gebruiken om verbinding te. Als dit niet het geval is, gebruikt u de pijl omlaag om het juiste certificaat te selecteren en klik vervolgens op **OK**.

    ![VPN-clientverbinding] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN-clientverbinding")

3. Moet nu de verbinding tot stand worden gebracht.

    ![Verbinding maken] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Verbinding maken")

## <a name="verify"></a>Deel van 8 – uw verbinding controleren

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

## <a name="addremovecert"></a>Toevoegen aan of verwijderen van een vertrouwde basiscertificeringsinstantie-certificaat

Certificaten worden gebruikt om te verifiëren VPN-clients voor VPN's punt-naar-Site. De volgende stappen begeleiden u bij het toevoegen en verwijderen van basiscertificaten. Wanneer u een bestand Base64 gecodeerde X.509 (.cer) aan Azure toevoegen, geeft u aan dat Azure te vertrouwen van de hoofdsite-certificaat dat staat voor het bestand. 

U kunt toevoegen of verwijderen van vertrouwde basiscertificaten via PowerShell of in de portal van Azure. Als u dit doen met behulp van de Azure portal wilt, gaat u naar uw **virtueel netwerkgateway > Instellingen > punt-naar-site configuratie > Root certificaten**. De volgende stappen doorlopen u deze taken via PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Een vertrouwde basiscertificeringsinstantie-certificaat toevoegen

U kunt maximaal 20 vertrouwde basiscertificeringsinstantie certificaat .cer-bestanden toevoegen aan Azure. Volg de onderstaande stappen voor het toevoegen van een basis-certificaat.

1. Maken en het voorbereiden van het nieuwe hoofdsite-certificaat dat u aan Azure wordt toegevoegd. De openbare sleutel exporteren als een basis-64 encoded X.509 (. CER) en vervolgens opent in een teksteditor. Kopieer alleen in de sectie hieronder wordt weergegeven. 
 
    Kopieer de waarden, zoals wordt weergegeven in het volgende voorbeeld:

    ![certificaat] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "certificaat")
    
2. Geef de naam van het certificaat en belangrijke informatie als een variabele. De gegevens vervangen door uw eigen, zoals wordt getoond in het volgende voorbeeld:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Het nieuwe hoofdsite-certificaat toevoegen. U kunt alleen een certificaat tegelijk toevoegen.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. U kunt controleren dat het nieuwe certificaat juist is toegevoegd met behulp van de volgende cmdlet.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Een vertrouwde basiscertificeringsinstantie certificaat verwijderen

U kunt vertrouwde basiscertificeringsinstantie certificaat verwijderen uit Azure. Wanneer u een vertrouwd certificaat verwijdert, is de clientcertificaten die zijn gegenereerd op basis van het certificaat hoofdsite niet langer verbinding maken met Azure via punt-naar-Site. Als u clients verbinding maakt wilt, moeten ze een nieuwe clientcertificaat dat is gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure installeren.

1. Als u wilt verwijderen van een certificaat vertrouwde basiscertificeringsinstantie, wijzigen in het onderstaande voorbeeld:

    De variabelen declareren.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Verwijder het certificaat.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Gebruik de volgende cmdlet om te bevestigen dat het certificaat is verwijderd. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>Voor het beheren van de lijst met ingetrokken certificaten

U kunt clientcertificaten intrekken. De certificaatintrekkingslijsten kunt u selectief weigeren punt-naar-Site connectivity op basis van individuele clientcertificaten. Als u een hoofdmap certificaat .cer uit Azure verwijdert, trekt deze u de toegang voor alle clientcertificaten die zijn gegenereerd/ondertekend door het certificaat ingetrokken hoofdmap. Als u wilt intrekken van een bepaald clientcertificaat, niet de hoofdsite en kunt u doen. De certificaten die zijn gegenereerd op basis van het certificaat van de hoofdsite nog steeds worden op deze manier zijn geldig.

De algemene manier is om het gebruik van de hoofdsite-certificaat voor het beheren van access op team of organisatie niveaus, met ingetrokken certificaten for fijnmazige toegangsbeheer voor individuele gebruikers.

### <a name="revoke-a-client-certificate"></a>Een clientcertificaat intrekken

1. Krijgen de vingerafdruk van het clientcertificaat intrekken.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. De vingerafdruk toevoegen aan de lijst met ingetrokken vingerafdruk.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Controleer of de vingerafdruk is toegevoegd aan de certificaatintrekkingslijsten. Per keer, moet u één vingerafdruk toevoegen.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Een clientcertificaat herstellen

U kunt een clientcertificaat opnieuw wilt instellen door de vingerafdruk verwijderen uit de lijst met ingetrokken certificaten.

1.  Verwijder de vingerafdruk uit de lijst met ingetrokken client certificaatvingerafdruk.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Controleer als de vingerafdruk is verwijderd uit de lijst ingetrokken.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Volgende stappen

U kunt een virtuele machine toevoegen aan uw virtuele netwerk. Zie [maken een virtuele Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.