<properties 
   pageTitle="Over VPN apparaten voor Site-naar-Site VPN Gateway verbindingen voor Azure virtuele netwerken | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe VPN apparaten en IPSec-parameters voor S2S VPN Gateway-verbindingen en bevat koppelingen naar configuratie-instructies en voorbeelden."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>Over VPN apparaten voor Site-naar-Site VPN Gateway-verbindingen

Een VPN-apparaat is vereist voor het configureren van een Site-naar-Site (S2S) VPN-verbinding. Site-naar-Site-verbindingen kunnen worden gebruikt om een hybride oplossing maakt of wanneer u een beveiligde verbinding tussen uw on-premises netwerk en uw virtuele netwerk dat wilt. In dit artikel wordt beschreven hoe compatibele VPN-apparaten en configuratieparameters. 

>[AZURE.NOTE] Tijdens het configureren van een Site op Site-verbinding, is een openbare IPv4-IP-adres vereist voor uw apparaat VPN.                                                                                                                                                                               

Als uw apparaat niet wordt weergegeven in de tabel [gevalideerd VPN-apparaten](#devicetable) , raadpleegt u de sectie [niet-geverifieerde VPN-apparaten](#additionaldevices) van dit artikel. Het is mogelijk dat uw apparaat nog steeds met Azure werken mogelijk. Neem contact op met de apparaatfabrikant van uw voor ondersteuning van VPN-apparaat.

**Items Houd er rekening mee bij het weergeven van de tabellen:**

- Er is een wijziging terminologie voor statische en dynamische-mailroutering. U waarschijnlijk treden beide termen. Er is geen functionaliteitswijziging van de, alleen de namen wilt wijzigen.
    - Statische routering = PolicyBased
    - Dynamische routering = RouteBased
- Specificaties voor hoge prestaties VPN gateway en RouteBased VPN gateway zijn hetzelfde, tenzij anderszins vermeld. Bijvoorbeeld zijn de gevalideerde VPN-apparaten die compatibel met RouteBased VPN gateways zijn ook compatibel met de gateway Azure hoge prestaties VPN. 


## <a name="devicetable"></a>Gevalideerde VPN-apparaten 

We hebben een reeks standaard VPN apparaten in samenwerking met leveranciers van apparaat gevalideerd. Alle apparaten in het apparaat gezinnen opgenomen in de volgende lijst moeten samenwerken met Azure VPN gateways. Zie [Over VPN Gateway](vpn-gateway-about-vpngateways.md) voor de verificatie van het type gateway die u moet maken voor de oplossing die u wilt configureren. 

Om u te helpen met het configureren van uw apparaat VPN, raadpleegt u de koppelingen die met de juiste apparaat familie overeenkomen. Neem contact op met de apparaatfabrikant van uw voor ondersteuning van VPN-apparaat.



| **Leverancier**                      | **Apparaat familie**                                        | **Minimale versie van het besturingssysteem**                             | **PolicyBased**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Allied Telesis                  | AR reeks VPN Routers                                    | 2.9.2                                              | Binnenkort beschikbaar                                                                                                                                                                                                                                          | Niet compatibel                                                                                                                                                                                               |
| Barracuda-netwerken te gebruiken, Inc.        | Barracuda NextGen Firewall F-reeks             | PolicyBased: 5.4.3, RouteBased: 6.2.0  | [Configuratie-instructies](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Configuratie-instructies](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda-netwerken te gebruiken, Inc.        |  Barracuda NextGen Firewall X-reeks                 | Barracuda Firewall 6.5 | [Barracuda Firewall](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Niet compatibel                                                                                                                                                                                               |
| Brokaatontwerp                         | Vyatta 5400 vRouter                                      | Virtuele Router 6.6R3 GA                            | [Configuratie-instructies](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Niet compatibel                                                                                                                                                                                               |
| Selectievakje punt                     | Beveiligingsgateway                                         | R75.40, R75.40VS                                     | [Configuratie-instructies](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Configuratie-instructies](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Voorbeelden van Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Niet compatibel                                                                                                                                                                                               |
| Cisco                           | ASR                                                      | IOS 15.1 (PolicyBased), IOS 15,2 (RouteBased)                | [Voorbeelden van Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Voorbeelden van Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15.0 (PolicyBased), IOS 15.1 (RouteBased *)               | [Voorbeelden van Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Voorbeelden van Cisco *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | NetScaler MPX, SDX, VPX      |10,1 en hoger                                           | [Integratie van instructies](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Niet compatibel                                                                                                                                                                                               |
| Dell SonicWALL                  | TZ reeks NSA reeks SuperMassive reeks E-klasse NSA reeks | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Instructies - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Instructies - SonicOS 5,9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Instructies - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Instructies - SonicOS 5,9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | Reeks BIG-IP                                 |           12.0                                            | [Configuratie-instructies](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Configuratie-instructies](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | FortiGate                                                | FortiOS 5.2.7                                      | [Configuratie-instructies](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Configuratie-instructies](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Internet initiatief Japan (IIJ) | SEIL reeks                                              | SEIL / X 4,60, SEIL/B1 4,60, SEIL/x86 3,20            | [Configuratie-instructies](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Niet compatibel                                                                                                                                                                                               |
| Juniper                         | SRX                                                      | JunOS 10.2 (PolicyBased), JunOS 11,4 (RouteBased)            | [Voorbeelden van Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Voorbeelden van Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Juniper                         | J-reeks                                                 | JunOS 10.4r9 (PolicyBased), JunOS 11,4 (RouteBased)          | [Voorbeelden van Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Voorbeelden van Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Juniper                         | ISG                                                      | ScreenOS 6.3 (PolicyBased en RouteBased)                  | [Voorbeelden van Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Voorbeelden van Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Juniper                         | SSG                                                      | ScreenOS 6.2 (PolicyBased en RouteBased)                  | [Voorbeelden van Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Voorbeelden van Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Routing and Remote Access Service                        | Windows Server 2012                                | Niet compatibel                                                                                                                                                                                                                                       | [Voorbeelden van Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Open systemen AG | Mission besturingselement beveiligingsgateway | N/B | [Installatiehandleiding](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Installatiehandleiding](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Binnenkort)                                                                                                                                                                                                                                        | Niet compatibel                                                                                                                                                                                               |
| Palo Alto-netwerken te gebruiken              | Alle apparaten met PANNEN-besturingssysteem     | PAN-OS 6.1.5 of hoger (PolicyBased), PANNEN-OS 7.0.5 of hoger (RouteBased)       | [Configuratie-instructies]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Configuratie-instructies](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| Watchguard                      | Alle                                                      | Fireware XTM v11.x                                 | [Configuratie-instructies](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Niet compatibel                                                                                                                                                                                               |

(*) ISR 7200 reeks routers ondersteund alleen PolicyBased VPN's.

## <a name="additionaldevices"></a>Niet-geverifieerde VPN-apparaten

Als u uw apparaat weergegeven in de tabel gevalideerd VPN-apparaten niet ziet, nog steeds werkt mogelijk met de verbinding van een Site-naar-Site. Controleer of uw apparaat VPN voldoet aan de minimale vereisten die worden beschreven in de sectie Gateway vereisten van het artikel [Over VPN Gateways](vpn-gateway-about-vpngateways.md#gateway-requirements) . Apparaten die voldoen aan de minimale vereisten moeten ook werken ook met VPN gateways. Contact opnemen met de apparaatfabrikant van uw voor extra ondersteuning en configuratie-instructies.


## <a name="editing-device-configuration-samples"></a>Voorbeelden van apparaat configuratie bewerken

Nadat u het opgegeven configuratie voorbeeld van de VPN apparaat downloaden, moet u enkele van de waarden die overeenkomen met de instellingen voor uw omgeving vervangen. 

**Een steekproef bewerken:**

1. Open de steekproef met Kladblok. 
1. Zoeken en vervangen van alle <*tekst*> tekenreeksen met de waarden die betrekking op uw omgeving hebben. Zorg ervoor dat u wilt opnemen < en >. Wanneer een naam is opgegeven, moet de naam die u selecteert uniek zijn. Als een opdracht niet werkt, raadpleegt u de documentatie van de fabrikant van uw apparaat.

| **Voorbeeldtekst**                | **Wijzigen**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | De naam van de door u gekozen voor dit object. Voorbeeld: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | De naam van de door u gekozen voor dit object. Voorbeeld: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | De naam van de door u gekozen voor dit object. Voorbeeld: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | De naam van de door u gekozen voor dit object. Voorbeeld: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | De naam van de door u gekozen voor dit object. Voorbeeld: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Geef bereik. Voorbeeld: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Subnet opgeven. Voorbeeld: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | On-premises implementatie bereik opgeven. Voorbeeld: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | On-premises implementatie subnetmasker opgeven. Voorbeeld: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Deze informatie die specifiek zijn voor uw virtuele netwerk en bevindt zich in de beheerportal als **Gateway IP-adres**. |
| &lt;SP_PresharedKey&gt;                | Deze informatie is specifiek voor uw virtuele netwerk en bevindt zich in de beheerportal als sleutel beheren.          |



## <a name="ipsec-parameters"></a>IPSec-Parameters

>[AZURE.NOTE] Hoewel de waarden in de volgende tabel worden ondersteund door de Gateway van de VPN Azure, is er momenteel geen manier kunt opgeven, of Selecteer een specifieke combinatie van de Azure VPN Gateway. Beperkingen van het apparaat van de VPN on-premises implementatie, moet u opgeven. Daarnaast moet u MSS klem bij 1350.

### <a name="ike-phase-1-setup"></a>IKE fase 1-instelling

| **Eigenschap**                                       | **PolicyBased** | **RouteBased en standaard of hoge prestaties VPN gateway** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE versie                                        | IKEv1                          | IKEv2                                                            |
| DH-groep                               | Groep 2 (1024 bits)             | Groep 2 (1024 bits)                                               |
| Verificatiemethode                              | Vooraf gedeelde sleutel                 | Vooraf gedeelde sleutel                                                   |
| Versleutelingsalgoritmen                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Hashing algoritme                                  | SHA1(SHA128)                   | SHA1(SHA128), SHA2(SHA256)                                                     |
| Fase 1 beveiligingsbeleid SA (Association) levensduur (tijd) | 28.800 seconden                 | 10,800 seconden                                                   |


### <a name="ike-phase-2-setup"></a>IKE fase 2-instelling

| **Eigenschap**                                                             | **PolicyBased**                 | **RouteBased en standaard of hoge prestaties VPN gateway**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE versie                                                              | IKEv1                                          | IKEv2                                                              |
| Hashing algoritme                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Fase 2 beveiligingsbeleid SA (Association) levensduur (tijd)                        | 3600 seconden                                  | 3600 seconden                                                                  |
| Fase 2 beveiligingsbeleid SA (Association) levensduur (doorvoer)                  | 102,400,000 KB                                 | -                                                                  |
| IPsec SA versleuteling en verificatie biedt (in de volgorde van voorkeur) | 1. ESP-AES256 2. ESP-AES128 3. ESP-3DES 4. N/B | Zie *RouteBased Gateway IPsec SA (Security Association) biedt* (hieronder) |
| Perfect doorsturen geheimhouding (optie)                                            | Nee                                             | Geen (*)|
| Dode Peer-detectie                                                      | Niet ondersteund                                  | Ondersteund                                                          |

(*) Azure Gateway als IKE eindpunt kunt akkoord gaan optie DH groep 1, 2, 5, 14, 24.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>Biedt RouteBased Gateway IPsec SA Security Association)

De volgende tabel bevat IPsec SA versleuteling en verificatie biedt. Aanbiedingen, vindt u de volgorde van voorkeur dat de aanbieding wordt gepresenteerd of geaccepteerd.

| **IPsec SA versleuteling en verificatie aanbiedingen** | **Azure Gateway als initiatiefnemer**                               | **Azure Gateway als eindpunt**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | AH SHA1 met ESP AES_128 met null HMAC                      |
| 5                                                 | AH SHA1 met ESP AES_256 met null HMAC                      | AH SHA1 met ESP 3_DES met null HMAC                        |
| 6                                                 | AH SHA1 met ESP AES_128 met null HMAC                      | AH MD5 met ESP 3_DES met null HMAC, geen levensduur kan worden voorgesteld |
| 7                                                 | AH SHA1 met ESP 3_DES met null HMAC                        | AH SHA1 met ESP 3_DES SHA1, geen levensduur                    |
| 8                                                 | AH MD5 met ESP 3_DES met null HMAC, geen levensduur kan worden voorgesteld | AH MD5 met ESP 3_DES MD5, geen levensduur                     |
| 9                                                 | AH SHA1 met ESP 3_DES SHA1, geen levensduur                    | ESP DES MD5                                                  |
| 10                                                | AH MD5 met ESP 3_DES MD5, geen levensduur                     | ESP DES SHA1, geen levensduur                                   |
| 11                                                | ESP DES MD5                                                  | Dit SHA1 met ESP DES null HMAC, geen levensduur kan worden voorgesteld        |
| 12                                                | ESP DES SHA1, geen levensduur                                   | Dit MD5 met ESP DES null HMAC, geen levensduur kan worden voorgesteld        |
| 13                                                | Dit SHA1 met ESP DES null HMAC, geen levensduur kan worden voorgesteld        | AH SHA1 met ESP DES SHA1, geen levensduur                      |
| 14                                                | Dit MD5 met ESP DES null HMAC, geen levensduur kan worden voorgesteld        | AH MD5 met ESP DES MD5, geen levensduur                       |
| 15                                                | AH SHA1 met ESP DES SHA1, geen levensduur                      | ESP SHA, geen levensduur                                        |
| 16                                                | AH MD5 met ESP DES MD5, geen levensduur                       | ESP MD5, geen levensduur                                        |
| 17                                                | -                                                            | Dit SHA, geen levensduur                                         |
| 18                                                | -                                                            | AH MD5, geen levensduur                                        |


- IPsec ESP NULL-codering kunt u opgeven met RouteBased en High Performance VPN gateways. Null gebaseerd codering biedt geen beveiliging met gegevens tijdens overdracht en mag alleen worden gebruikt als maximale doorvoer en minimum latentie is vereist.  Clients kunt gebruik deze opdracht in VNet-naar-VNet communicatie-scenario's of wanneer er versleuteling ergens anders in de oplossing wordt toegepast.

- Gebruik de standaardinstellingen voor de gateway van Azure VPN met versleuteling en weergegeven in de tabellen hierboven om ervoor te zorgen beveiliging van uw kritieke communicatie ontwikkeld voor cross-premises-connectiviteit via Internet.





