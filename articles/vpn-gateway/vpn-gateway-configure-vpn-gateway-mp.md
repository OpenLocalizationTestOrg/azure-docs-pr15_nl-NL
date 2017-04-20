<properties 
   pageTitle="Een VPN-Gateway configureren in de Portal van Azure klassieke | Microsoft Azure"
   description="In dit artikel leert u via het netwerk van uw virtuele VPN gateway configureren en het wijzigen van een gateway VPN routeren type."
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
   ms.date="08/11/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vpn-gateway-for-the-classic-deployment-model"></a>Een gateway VPN voor het implementatiemodel klassieke configureren


Als u maken van een beveiligde cross lokale verbinding tussen Azure en de locatie van uw on-premises implementatie wilt, moet u een VPN-verbinding gateway configureren. In het implementatiemodel klassieke, een gateway zijn twee soorten VPN routeren: statische of dynamische. Het type die u kiest, is afhankelijk van uw abonnement voor het ontwerp van netwerk en het on-premises implementatie VPN apparaat dat u wilt gebruiken. 

Sommige connectiviteitsopties, zoals een verbinding punt-naar-site is bijvoorbeeld een dynamische routeren gateway vereist. Als u wilt configureren van de gateway ter ondersteuning van zowel punt-naar-site (P2S)-verbindingen en de verbinding van een site-naar-site (S2S), hebt u een dynamische routeren gateway te configureren, zelfs als site-naar-site kan worden geconfigureerd met een van beide gateway VPN routeren type. 

Daarnaast moet ervoor zorgen dat het apparaat dat u wilt gebruiken voor de mobiele verbinding voor ondersteuning biedt voor het routeren VPN-type die u wilt maken. Zie [informatie over VPN-apparaten](vpn-gateway-about-vpn-devices.md).


**Over in dit artikel** 

In dit artikel is geschreven voor het klassieke implementatiemodel met behulp van de [klassieke-portal](https://manage.windowsazure.com) (niet de Azure portal). 

**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-overview"></a>Configuratieoverzicht

De volgende stappen begeleid u bij het configureren van de gateway VPN in de portal van Azure klassieke. Deze stappen gelden voor gateways voor virtuele netwerken die zijn gemaakt met het implementatiemodel klassieke. Op dit moment zijn niet voor alle configuratie-instellingen voor gateways beschikbaar in de portal van Azure. Wanneer ze zijn, maakt we een nieuwe set instructies die van toepassing op de Azure-portal.


1. [Een VPN-gateway maken voor uw VNet](#create-a-vpn-gateway)

1. [Verzamel informatie voor uw apparaat VPN-configuratie](#gather-information-for-your-vpn-device-configuration)

1. [Uw apparaat VPN configureren](#configure-your-vpn-device)

1. [Controleer uw lokale netwerkbereiken en het IP-adres van VPN gateway](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### <a name="before-you-begin"></a>Voordat u begint

Voordat u uw gateway configureert, moet u eerst uw virtuele netwerk maken. Zie [een virtuele netwerk met een VPN-verbinding voor site-naar-site configureren](vpn-gateway-site-to-site-create.md)of [een virtuele netwerk met een VPN-verbinding punt-naar-site configureren](vpn-gateway-point-to-site-create.md)voor de stappen voor het maken van een virtueel netwerk voor cross-premises connectivity. Gebruik vervolgens de volgende stappen voor het configureren van de gateway VPN en verzamel de gegevens die u nodig hebt voor het configureren van uw apparaat VPN. 

Als u al een VPN-gateway hebt en u wilt wijzigen van de VPN-mailroutering type, raadpleegt u [het wijzigen van de VPN routeren type voor de gateway](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Een VPN-gateway maken

1. Controleer of de statuskolom voor het virtuele netwerk is **gemaakt**in de [klassieke Azure-portal](https://manage.windowsazure.com), klik op de pagina **netwerken te gebruiken** .

1. Klik in de kolom **naam** op de naam van uw virtuele netwerk.

1. Klik op de pagina **Dashboard** , zoals u ziet dat dit VNet geen een gateway nog geconfigureerd. U kunt deze status wordt weergegeven, terwijl u de stappen voor het configureren van de gateway doorloopt.

![Gateway niet gemaakt](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)


Klik vervolgens onder aan de pagina, klikt u op **Gateway maken**. U kunt *Omleiding van statische* of *Dynamische routering*selecteren. Het VPN routeren type die u selecteert, is afhankelijk van enkele factoren. Bijvoorbeeld, wat uw apparaat VPN ondersteunt en of u moet ondersteuning voor punt-naar-site-verbindingen. Schakel [Over VPN apparaten voor Virtual netwerkconnectiviteit](vpn-gateway-about-vpn-devices.md) om te controleren of het VPN routeren type die u nodig hebt. Zodra de gateway is gemaakt, kunt u niet wijzigen tussen gateway VPN routeren typen zonder het verwijderen en opnieuw de gateway te maken. Wanneer het systeem u om te bevestigen vraagt dat u wilt dat de gateway hebt gemaakt, klikt u op **Ja**.

![Gateway VPN routeren type](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Als uw gateway maakt, zoals u ziet de gateway-afbeelding op de pagina wordt gewijzigd in geel en zegt *Gateway maken*. Het duurt maximaal 45 minuten voor de gateway te maken. Wacht totdat de gateway voltooid is voordat u kunt verdergaan met andere configuratie-instellingen.

![Gateway maken](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Wanneer de gateway is veranderd in *verbinding maken*, kunt u de gegevens die u nodig hebt voor uw apparaat VPN verzamelen.

![Gateway-verbinding](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="gather-information-for-your-vpn-device-configuration"></a>Verzamel informatie voor uw apparaat VPN-configuratie

Nadat de gateway is gemaakt, kunt u gegevens voor uw apparaat VPN-configuratie verzamelen. Deze informatie bevindt zich op de pagina **Dashboard** voor uw virtuele netwerk:

1. **IP-adres gateway-** Het IP-adres kan worden gevonden op **de dashboardpagina** . Niet mogelijk om het te geven tot nadat de gateway is voltooid maken.

1. **Gedeeld toets-** Klik op de **Toets beheren** onder aan het scherm. Klik op het pictogram naast de toets om naar het Klembord, kopiëren en plakken en de sleutel opslaan. Deze knop werkt alleen als er één S2S VPN tunnel. Als er meerdere S2S VPN tunnels, gebruik de *Krijgen virtuele netwerk Gateway Shared Key* API of PowerShell-cmdlet.

![Toets beheren](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)


## <a name="configure-your-vpn-device"></a>Uw apparaat VPN configureren

Na het voltooien van de voorgaande stappen, moet u of uw netwerkbeheerder voor het configureren van het apparaat van de VPN om te maken van de verbinding. Zie [Over VPN apparaten voor Virtual netwerkconnectiviteit](vpn-gateway-about-vpn-devices.md) voor meer informatie over VPN-apparaten.

Nadat het VPN-apparaat is geconfigureerd, kunt u de bijgewerkte verbindingsgegevens bekijken op de pagina Dashboard voor uw VNet.

U kunt ook een van de volgende opdrachten om uw verbinding te testen uitvoeren:

|                      | Cisco ASA             | Cisco ISR/ASR         | Juniper SSG/ISG | Juniper SRX/J                            |
|----------------------|-----------------------|-----------------------|-----------------|------------------------------------------|
| **Belangrijkste modus SA's controleren**  | weergeven van cryptografische isakmp sa | weergeven van cryptografische isakmp sa | ike cookie ophalen  | weergeven van beveiliging ike beveiliging-koppeling   |
| **Snelle modus SA's controleren** | weergeven van cryptografische ipsec sa  | weergeven van cryptografische ipsec sa  | sa ophalen          | beveiliging IPSec-beveiliging-koppeling weergeven |


## <a name="verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Controleer uw lokale netwerkbereiken en het IP-adres van VPN gateway

### <a name="verify-your-vpn-gateway-ip-address"></a>Controleer of uw IP-adres van de VPN gateway

Voor de gateway verbinding, het IP-adres voor uw apparaat VPN moet correct zijn geconfigureerd voor het lokale netwerk die u hebt opgegeven voor uw cross-premises-configuratie. Dit is meestal geconfigureerd tijdens de configuratie van de site-naar-site. Als u dit lokale netwerk met een ander apparaat eerder hebt gebruikt, of het IP-adres voor dit lokale netwerk is gewijzigd, maar de instellingen als u wilt opgeven van het juiste IP-adres Gateway bewerken.

1. Als u wilt controleren of uw gateway IP-adres, klikt u op **netwerken te gebruiken** in het linkerdeelvenster van de portal en selecteer vervolgens **Lokale netwerken** boven aan de pagina. Hier ziet u het adres van de Gateway VPN van elke lokale netwerk die u hebt gemaakt. Als u wilt bewerken op het IP-adres, de VNet en klik op **bewerken** onder aan de pagina.

1. Klik op de pagina **details van uw lokale netwerk opgeven** bewerken van het IP-adres en klik vervolgens op de pijl-rechts onder aan de pagina.

1. Klik op het selectievakje in de rechterbenedenhoek om op te slaan van uw instellingen op de pagina **Geef de adresruimte** .

### <a name="verify-the-address-ranges-for-your-local-networks"></a>Controleer of de adresbereiken voor uw lokale netwerken

Voor de juiste verkeer via de gateway naar de locatie van uw on-premises implementatie, moet u controleren of dat elk IP-adresbereiken is opgegeven. Elk bereik moet worden vermeld in de configuratie van uw Azure **Lokale netwerken** . Afhankelijk van de netwerkconfiguratie van uw on-premises locatie, als dit is een enigszins grote taak. Verkeer dat afhankelijk is van voor een IP-adres dat is opgenomen in de lijst bereiken worden verzonden via de gateway van de VPN virtueel netwerk. De bereiken die u opgeeft, hoeft te worden privé bereiken, hoewel u wilt verifiëren dat de configuratie van uw on-premises implementatie de binnenkomende verkeer kan ontvangen.

Als u wilt toevoegen of bewerken van de bereiken voor een lokale netwerk, gebruikt u de volgende stappen uit.

1. Als u wilt bewerken de IP-adresbereiken voor een lokale netwerk, klikt u op **netwerken te gebruiken** in het linkerdeelvenster van de portal en selecteer vervolgens **Lokale netwerken** boven aan de pagina. Klik in de portal is de eenvoudigste manier om weer te geven van de bereiken die u hebt staan op de pagina **bewerken** . Als u wilt bereiken wordt weergegeven, de VNet en klik op **bewerken** onder aan de pagina.

1. Klik op de pagina **details van uw lokale netwerk opgeven** , geen wijzigingen aanbrengt. Klik op de pijl-rechts onder aan de pagina.

1. Klik op de pagina **Geef de adresruimte** Controleer uw netwerk adres ruimte wordt gewijzigd. Klik vervolgens op het vinkje om op te slaan van uw configuratie.

## <a name="how-to-view-gateway-traffic"></a>Het weergeven van de gateway-verkeer is toegestaan

U kunt uw gateway en de gateway-verkeer is toegestaan uit uw Virtual Network **Dashboard** -pagina weergeven.

U kunt de volgende taken weergeven op de pagina **Dashboard** :

- De hoeveelheid gegevens die is die doorloopt tot en met de gateway, zowel de gegevens in en de gegevens af.

- De namen van de DNS-servers die zijn opgegeven voor uw virtuele netwerk.

- De verbinding tussen uw gateway en uw VPN-apparaat.

- De gedeelde sleutel die wordt gebruikt voor het configureren van de gateway-verbinding met uw apparaat VPN.


## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Het wijzigen van de VPN routeren type voor uw gateway

Omdat sommige configuraties connectivity alleen beschikbaar voor bepaalde gateway routeren typen zijn, kan het gebeuren dat u wilt wijzigen van de gateway VPN routeren type van een bestaande VPN gateway. U wilt bijvoorbeeld connectivity punt-naar-site toevoegen aan een bestaande site-naar-site-verbinding met een statische gateway. Punt-naar-site verbindingen vragen om een dynamische gateway. Dit betekent dat de verbinding van een P2S configureren, moet u uw gateway VPN routeren type wijzigen van statische in dynamische.

Als u wijzigen van een gateway VPN routeren type wilt, wordt u de bestaande gateway verwijderen en klikt u vervolgens een nieuwe gateway te maken met het nieuwe routeren type. U hoeft niet te verwijderen van het hele virtuele netwerk als u wilt wijzigen van het type van gateway-mailroutering.

Voordat u wijzigt de gateway VPN routeren type, zorg ervoor dat u voor de verificatie van uw apparaat VPN ondersteuning biedt voor het routeren type die u wilt gebruiken. Voorbeelden van nieuwe routeren configuratie downloaden en VPN apparaat systeemvereisten controleren: Zie [Over VPN apparaten voor Virtual netwerkconnectiviteit](vpn-gateway-about-vpn-devices.md).

>[AZURE.IMPORTANT] Wanneer u een virtueel netwerk VPN gateway verwijdert, wordt de VIP die zijn toegewezen aan de gateway is uitgebracht. Wanneer u opnieuw de gateway te maken, is een nieuwe VIP toegewezen.

1. **Verwijder de bestaande VPN gateway.**

    Klik op de pagina **Dashboard** voor uw netwerk op virtuele Ga naar het einde van de pagina en klik op **Gateway verwijderen**. Wacht totdat de melding dat de gateway is verwijderd. Wanneer u de melding op het scherm dat de gateway is verwijderd ontvangt, kunt u een nieuwe gateway maken.

1. **Maak een nieuwe VPN gateway.**

    Gebruik van de procedure boven aan de pagina voor het maken van een nieuwe gateway: [een VPN-gateway maken](#create-a-vpn-gateway).


## <a name="next-steps"></a>Volgende stappen

U kunt virtuele machines toevoegen aan uw virtuele netwerk. Lees [hoe u een aangepaste virtuele machine maken](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Als u configureren, een VPN-verbinding punt-naar-site wilt, raadpleegt u [een VPN-verbinding punt-naar-site configureren](vpn-gateway-point-to-site-create.md).

 
