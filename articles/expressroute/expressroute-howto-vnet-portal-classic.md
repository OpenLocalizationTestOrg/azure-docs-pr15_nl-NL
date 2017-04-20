<properties
   pageTitle="Een virtueel netwerk en de Gateway configureren voor ExpressRoute in de klassieke portal | Microsoft Azure"
   description="In dit artikel begeleidt u bij het instellen van een virtueel netwerk voor ExpressRoute met het implementatiemodel klassieke en de klassieke portal."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Een virtueel netwerk maken voor ExpressRoute in de klassieke portal

De stappen in dit artikel wordt u begeleid configureren van een virtueel netwerk en een gateway virtueel netwerk voor gebruik met ExpressRoute met het implementatiemodel klassieke en de klassieke portal.

Als u de instructies voor het implementatiemodel resourcemanager zoeken wilt, kunt u de volgende artikelen: [een virtueel netwerk via PowerShell maken](../virtual-network/virtual-networks-create-vnet-arm-ps.md) en [toevoegen een VPN-Gateway naar een VNet resourcemanager voor ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>Een klassieke VNet en gateway maken

De volgende stappen uit maken een klassieke VNet en een gateway virtueel netwerk. Als u al een klassieke VNet hebt, raadpleegt u de sectie [configureren een bestaande klassieke VNet](#config) in dit artikel.

1. Meld u aan bij de [portal van Azure klassieke](http://manage.windowsazure.com).

2. Klik in de linkerbenedenhoek van het scherm op **Nieuw**. Klik in het navigatiedeelvenster op **Netwerkservices**en klik vervolgens op **Virtual Network**. Klik op **Aangepaste maken** om te beginnen met de configuratiewizard.

3. Klik op de pagina **Details van virtuele netwerk** Typ het volgende:

    - **Naam** : de naam van uw virtuele netwerk. U gebruikt deze virtuele netwerknaam wanneer u uw exemplaren VMs en PaaS implementeert, zodat u mogelijk niet wilt maken van de naam te ingewikkeld.
    - **Locatie** : de locatie is rechtstreeks zijn gerelateerd aan de fysieke locatie (regio) waar u uw resources (VMs) wilt opslaan. Als u wilt dat de VMs die u met deze virtuele netwerk te bevinden fysiek in Oost-VS implementeert, selecteert u bijvoorbeeld die locatie. U kunt het gebied dat is gekoppeld aan uw virtuele netwerk nadat u deze hebt gemaakt niet wijzigen.

4. Voer de volgende gegevens op de pagina **DNS-Servers en VPN-verbinding** en klik vervolgens op de pijl-rechts op de rechterbenedenhoek. 

    - **DNS-Servers** - Voer de naam van de DNS-server en IP-adres of Selecteer een eerder geregistreerde DNS-server in het snelmenu te openen. Deze instelling maakt geen een DNS-server. Dit kunt u de DNS-servers die u wilt gebruiken voor mailnamen omzetten voor dit virtuele netwerk opgeven.
    - **Site-naar-Site Connectivity** - selecteren het selectievakje voor **een VPN-verbinding voor de site-naar-site configureren**.
    - **ExpressRoute** – Selecteer het selectievakje **Gebruik ExpressRoute**. Deze optie wordt alleen weergegeven als u **een VPN-verbinding voor de Site-naar-Site configureren**hebt geselecteerd.
    - **Lokale netwerk** - u moet een lokale netwerk-site voor ExpressRoute hebt. In het geval van een verbinding ExpressRoute worden, het adresvoorvoegsels voor de site lokale netwerk genegeerd. In plaats daarvan worden de adresvoorvoegsels aangekondigd naar Microsoft via de circuitlijnen ExpressRoute gebruikt voor de routering.<BR>Als u al een lokaal netwerk voor uw ExpressRoute-verbinding hebt gemaakt, kunt u deze kunt selecteren in de vervolgkeuzelijst. Als dat niet zo is, selecteer **een nieuwe lokale netwerk opgeven**.

5. De pagina **Site-naar-Site verbindingsmogelijkheden** wordt weergegeven als u een nieuwe lokale netwerk opgeven in de vorige stap hebt geselecteerd. Als u wilt configureren op uw lokale netwerk, voer de volgende gegevens en klik op de pijl-rechts. 

    - **De naam van** -de naam die u wilt bellen (on-premises implementatie) van uw lokale netwerk site.
    - **Adresruimte** - inclusief IP starten en CIDR (aantal adres). Zo lang maken als deze niet met het adresbereik voor uw virtuele netwerk overlappen, kunt u eventuele adresbereiken opgeven. Meestal wordt dit geeft de adresbereiken voor uw on-premises implementatie-netwerken te gebruiken, maar in het geval van ExpressRoute, deze instellingen niet worden gebruikt. Deze instelling is echter vereist om te maken van het lokale netwerk wanneer u de klassieke portal gebruikt.
    - **Toevoegen-adresruimte** - met deze instelling is niet relevant voor ExpressRoute.


6. Voer de volgende gegevens op de pagina **Virtuele netwerk adres spaties** en klik op het vinkje op de rechterbenedenhoek om uw netwerk te configureren. 

    - **Adresruimte** - zoals begin en het IP-adres tellen. Controleer of dat de adres-spaties die u opgeeft elkaar niet overlappen een van de adres-spaties die op uw lokale netwerk.
    - **Toevoegen subnet** - inclusief IP starten en aantal adressen. Extra subnetten zijn niet vereist.
    - **Toevoegen gateway subnet** - Klik om toe te voegen van de gateway-subnet. De gateway-subnet wordt alleen gebruikt voor de gateway virtueel netwerk en is vereist voor deze configuratie.<BR>De gateway-subnet CIDR (aantal adressen) voor ExpressRoute moet /28 of groter (/ 27/26 enz.). Hiermee voldoende IP-adressen in dat subnet toe te staan dat de configuratie om te werken. Klik in de klassieke portal als u het selectievakje Gebruik ExpressRoute, hebt geselecteerd de portal Hiermee geeft u een gateway-subnet met /28.  U kunt het aantal CIDR adressen in de klassieke portal niet kunt aanpassen. De gateway-subnet wordt weergegeven als **Gateway** in de klassieke-portal, hoewel de feitelijke naam van de gateway-subnetten die is gemaakt werkelijk **GatewaySubnet is**. U kunt deze naam via PowerShell of in de portal van Azure bekijken.

7. Klik op het selectievakje vanaf de onderkant van de pagina en uw virtuele netwerk maken wordt gestart. Wanneer deze is voltooid, ziet u **dat gemaakt** vermeld onder **Status** op de pagina **netwerken te gebruiken** in de klassieke portal.

## <a name="gw"></a>De gateway maken

1. Klik op de pagina **netwerken** het virtuele netwerk dat u zojuist hebt gemaakt, klik **Dashboard** boven aan de pagina.

2. Vanaf de onderkant van **de dashboardpagina** , klik op **Gateway maken** en selecteer **Dynamische routering**. Klik op **Ja** om te bevestigen dat u wilt maken van een gateway.

3. Wanneer de gateway wordt gestart maken, ziet u een bericht dat die u weet dat de gateway is gestart. Het duurt maximaal 45 minuten voor de gateway te maken.

4. Uw netwerk een koppeling naar een circuitlijnen. Volg de instructies in het artikel [het koppelen van VNets naar ExpressRoute circuits](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Een bestaande klassieke VNet configureren voor ExpressRoute

Als u al een klassieke VNet hebt, kunt u deze verbinding maken met ExpressRoute in de klassieke portal configureren. De instellingen zijn hetzelfde als de bovenstaande, secties lezen zodat deze secties om vertrouwd te raken met de gewenste instellingen. Als u een ExpressRoute/naar website naast elkaar bestaande verbinding maken wilt, raadpleegt u [in dit artikel](expressroute-howto-coexist-classic.md) voor de stappen. Anders dan de stappen in dit artikel zijn.
 
1. U moet het lokale netwerk maken voordat u de rest van de instellingen van uw VNet bijwerken. Als u wilt maken van een nieuwe lokale netwerk op, dat moet ondernomen worden tijdens het configureren van ExpressRoute via de portal klassieke, klikt u op **Nieuw** **>** **Netwerkservices** **>** **Virtual Network** **>** **toevoegen lokale netwerk**. Volg de stappen van de wizard voor het maken van het lokale netwerk.

2. Pagina **configureren** gebruiken voor het bijwerken van de rest van de instellingen voor uw VNet en de VNet met het lokale netwerk koppelen.

3. Nadat de instellingen zijn geconfigureerd, gaat u naar de sectie [de gateway maken](#gw) van dit artikel voor het maken van de gateway.


## <a name="next-steps"></a>Volgende stappen

- Als u virtuele machines toevoegen aan uw virtuele netwerk wilt, raadpleegt u [virtuele Machines leerpaden](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
- Als u weten over ExpressRoute wilt, raadpleegt u de [ExpressRoute overzicht](expressroute-introduction.md).


 
