<properties
   pageTitle="Maak VNet Peering met behulp van de Azure portal | Microsoft Azure"
   description="Informatie over het maken van een virtueel netwerk met behulp van de Azure portal in resourcemanager."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Maak een virtueel netwerk peering met behulp van de Azure portal

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Als u wilt maken op een VNet peering op basis van de scenario hierboven met behulp van de Azure-portal, de onderstaande stappen uit te voeren.

1. Via een browser, gaat u naar http://portal.azure.com en, indien nodig, meld u aan met uw Azure-account.
2. Om te bepalen VNET peering, moet u twee koppelingen, één voor elke richting, tussen twee VNets maken. U kunt eerst VNET peering koppeling voor VNET1 naar VNET2 maken. In de portal, klikt u op **Bladeren** > **virtuele netwerken kiezen**

    ![VNet peering in Azure-portal maken](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. Virtuele netwerken blade, kiest u VNET1, Peerings, klik op toevoegen

    ![Kies peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. Geef een koppelingsnaam voor peering LinkToVnet2, kies het abonnement en de peer virtuele netwerk VNET2 in het blad Peering toevoegen, klikt u op OK.

    ![Koppeling naar VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Nadat deze VNET peering koppeling is gemaakt. U kunt de verbindingsstatus zien als volgt te werk:

    ![Verbindingsstatus](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Maak de VNET peering koppeling voor VNET2 naar VNET1. Virtuele netwerken blade, kiest u VNET2, Peerings, klik op toevoegen

    ![Peer uit andere VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. Geef een koppelingsnaam voor peering LinkToVnet1 in het blad Peering toevoegen, kiest u het abonnement en de peer virtueel netwerk, klik op OK.

    ![Tegel van virtueel netwerk maken](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Nadat deze VNET peering koppeling is gemaakt. U kunt de verbindingsstatus zien als volgt te werk:

    ![Definitief verbindingsstatus](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. De status voor LinkToVnet2 controleren en deze wordt nu gewijzigd in verbonden ook.  

    ![Definitief verbindingsstatus 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

    > [AZURE.NOTE] VNET peering is alleen tot stand gebracht als beide koppelingen zijn verbonden.

Er zijn een paar configureerbare eigenschappen voor elke koppeling:

|Optie|Beschrijving|Standaard|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Opgeven of adresruimte van Peer-VNet moeten deel uit van de Tag Virtual_network|Ja|
|AllowForwardedTraffic|Opgeven of verkeer niet die afkomstig zijn van een peered VNet wordt geaccepteerd of verwijderd|Nee|
|AllowGatewayTransit|Kunt u de peer VNet gebruik van de gateway VNet|Nee|
|UseRemoteGateways|Gebruik van de peer VNet gateway. De peer VNet moet een gateway geconfigureerd en AllowGatewayTransit is geselecteerd. U kunt deze optie niet gebruiken als er een gateway geconfigureerd|Nee|

Elke koppeling in het VNet peering heeft een set hierboven eigenschappen. Vanuit de portal kunt u klikt op de koppeling VNet Peering en wijzig de beschikbare opties, klik op Opslaan om het effect te wijzigen.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Via een browser, gaat u naar http://portal.azure.com en, indien nodig, meld u aan met uw Azure-account.
2. In dit voorbeeld we gebruiken twee abonnementen A en B en twee gebruikers gebruiker a en verleent met bevoegdheden in de abonnementen respectievelijk
3. In de portal, klik op Bladeren, kies virtuele netwerken. Klik op de VNET en klikt u op toevoegen.

    ![Scenario 2 bladeren](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Klik op het blad toevoegen access, selecteer een rol Inzender netwerk kiezen, klikt u op gebruikers toevoegen, typ het teken verleent in het vak naam en klik op OK.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Dit is niet een vereiste, peering kan worden vastgesteld evenzeer peering aanvragen gebruikers afzonderlijk voor de desbetreffende Vnets verhogen zo lang maken als de aanvragen overeenkomen. Bevoegdheden van de gebruiker van de andere VNet toe te voegen als gebruikers in de lokale VNet vergemakkelijkt Stel in de portal.

5. Vervolgens Meld u aan bij Azure-portal met verleent die de gebruiker bevoegdheden voor SubscriptionB. Volg bovenstaande stappen voor het toevoegen van gebruiker a als Inzender netwerk.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    > [AZURE.NOTE] U kunt zich afmelden en meld u aan beide sessies in browser om te zorgen dat de verificatie is is ingeschakeld.

6. Aanmelden bij de portal als gebruiker a, gaat u naar het blad VNET3, klikt u op Peering, controleren ' Ik weet dat mijn resource-ID "selectievakje en typ de resource-ID voor VNET5 in onder opmaak.

    / subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}

    ![Resource-ID](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Meld u bij de portal als gebruiker b en volg bovenstaande stap naar peering koppeling maken van VNET5 naar VNet3.

    ![Resource-ID 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Peering wordt vastgesteld en een virtuele machine in VNet3 moet kunnen communiceren met een virtuele machine in VNet5

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Als eerste stap VNET peering koppelingen van HubVnet naar VNET1. Houd er rekening mee dat de optie doorgestuurd verkeer toestaan niet is ingeschakeld voor de koppeling.

    ![Eenvoudige Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Een volgende stap, kunnen peering koppelingen van VNET1 naar HubVnet worden gemaakt. Houd er rekening mee dat toestaan doorgestuurd verkeer optie is geselecteerd.

    ![Eenvoudige Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Nadat peering is gemaakt, kunt u verwijzen naar dit [artikel](virtual-network-create-udr-arm-ps.md) en gebruiker gedefinieerd Route(UDR) om te leiden VNet1 verkeer door een virtueel toestel gebruik van de mogelijkheden definiëren. Wanneer u het volgende Hop-mailadres in route opgeeft, kunt u deze op het IP-adres van het virtuele apparaat in peer VNet HubVNet instellen


[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]



1. Via een browser, gaat u naar http://portal.azure.com en, indien nodig, meld u aan met uw Azure-account.

2. Om te bepalen VNET peering in dit scenario, moet u slechts één koppeling, maken van het virtuele netwerk in Azure resourcemanager de wijziging in de klassieke. Dat wil zeggen uit **VNET1** naar **VNET2**. In de portal, klikt u op **Bladeren** > **Virtuele netwerken** kiezen

3. Kies **VNET1**in het blad virtuele-netwerken te gebruiken. Klik op **Peerings**en klik op **toevoegen**.

4. In het blad toevoegen Peering naam op de koppeling. Hier wordt genoemd **LinkToVNet2**. Selecteer **klassieke**onder Peer details.

5. Kies vervolgens het abonnement en de peer Virtual Network **VNET2**. Klik vervolgens op OK.

    ![Vnet1 koppelen aan Vnet 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)

6. Nadat deze VNet peering koppeling is gemaakt, de twee virtuele netwerken zijn peered en is mogelijk om het volgende weer te geven:

    ![Peering verbinding controleren](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)


## <a name="remove-vnet-peering"></a>VNet Peering verwijderen

1.  Via een browser, gaat u naar http://portal.azure.com en, indien nodig, meld u aan met uw Azure-account.
2.  Ga naar virtueel netwerk blade, klik op Peerings, klikt u op de koppeling die u wilt verwijderen, klikt u op de knop verwijderen.

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Wanneer u een koppeling in het VNET peering verwijderen, wordt de verbindingsstatus peer gaat u naar verbroken.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. In deze status wordt niet kunt u de koppeling opnieuw maken totdat de verbindingsstatus peer in gestart verandert. Het is raadzaam om dat u de beide koppelingen verwijderen voordat u opnieuw de VNET peering maken.
