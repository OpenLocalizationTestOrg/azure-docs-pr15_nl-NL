<properties 
   pageTitle="Het instellen van een statische privé IP-adres in ARM-modus met behulp van de Azure portal | Microsoft Azure"
   description="Wat zijn persoonlijke IP-adressen (Spanningsdips) en hoe u ze in de ARM-modus met behulp van de Azure portal beheren"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Het instellen van een statische privé IP-adres in de portal van Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel worden de resourcemanager implementatiemodel. U kunt ook een [statische privé IP-adres in het implementatiemodel klassieke beheren](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

De volgende stappen uit de steekproef verwachten een eenvoudige omgeving al hebt gemaakt. Als u de stappen uitvoeren wilt, zoals deze worden weergegeven in dit document, moet u eerst de testomgeving wordt beschreven in het [maken van een vnet](virtual-networks-create-vnet-arm-pportal.md)maken.

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Het maken van een VM voor het testen van statische privé IP-adressen

U instellen niet een statische privé IP-adres tijdens het maken van een VM in de resourcemanager implementatie-modus met behulp van de Azure-portal. Moet u eerst de VM maken, tehn de privé IP statisch instellen.

Als u wilt maken van een VM met de naam *DNS01* in het subnet *FrontEnd* van een VNet *TestVNet*met de naam, de onderstaande stappen uit te voeren.

1. Via een browser, gaat u naar http://portal.azure.com en, indien nodig, meld u aan met uw Azure-account.
2. Klik op **Nieuw** > **berekenen** > **Windows Server 2012 R2 Datacenter**, melding dat de lijst **Selecteer een implementatiemodel** al ziet **Resourcemanager**en klik vervolgens op **maken**, zoals gezien in de onderstaande afbeelding.

    ![VM in Azure beheerportal maken](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. Voer de naam van de VM (*DNS01* in ons scenario), worden gemaakt in het blad **Basisbeginselen** de lokale beheerdersaccount en het wachtwoord, zoals gezien in de onderstaande afbeelding.

    ![Basisbeginselen blade](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Controleer of de **locatie** die is geselecteerd is *Central US*, en klik op **bestaand bestand selecteren** onder **resourcegroep**en vervolgens klikt u nogmaals op **resourcegroep** en vervolgens klikt u op *TestRG*en klik op **OK**.

    ![Basisbeginselen blade](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. In het blad **Kies een grootte** , selecteer **A1 standaard**en klik vervolgens op **selecteren**.

    ![Kies een grootte blade](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6. Controleer of de volgende eigenschappen zijn ingesteld in het blad **Instellingen** worden ingesteld met de onderstaande waarden en klik vervolgens op **OK**.

    -**Opslag-account**: *vnetstorage*
    - **Netwerk**: *TestVNet*
    - **Subnet**: *FrontEnd*

    ![Kies een grootte blade](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7. Klik op **OK**in het blad **Overzicht** . Zoals u ziet de tegel weergegeven in uw dashboard.

    ![VM in Azure beheerportal maken](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische IP-adres privégegevens voor een VM ophalen

Als u wilt weergeven de statische privé IP-adresgegevens voor VM die zijn gemaakt met de bovenstaande stappen, in de onderstaande stappen worden uitgevoerd.

1. Klik op **Door alles bladeren**in de portal Azure Azure > **virtuele machines** > **DNS01** > **alle instellingen** > **netwerk-interfaces** en klik vervolgens op de enige netwerkinterface wordt vermeld.

    ![VM tegel implementeren](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. Klik op **alle instellingen**in het blad **netwerkinterface**  > **IP-adressen** en kijk naar de waarden **toewijzing** en **IP-adres** .

    ![VM tegel implementeren](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statische privé IP-adres toevoegen aan een bestaande VM
Als u wilt toevoegen een statische privé IP-adres voor VM gemaakt met behulp van de bovenstaande stappen, de volgende stappen uit te voeren:

1. Klik in het **IP-adressen** blad hierboven, op **statische** onder **toewijzing**.
2. Typ *192.168.1.101* voor **IP-adres**en klik vervolgens op **Opslaan**.

    ![VM in Azure beheerportal maken](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Als na het klikken op **Opslaan** u ziet dat de toewijzing nog steeds is ingesteld op **dynamische**, betekent dit dat het IP-adres dat u hebt getypt al gebruikt wordt. Probeer een ander IP-adres.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statische privé IP-adres uit een VM verwijderen
Als u wilt het statische privé IP-adres verwijderen uit de bovenstaande VM, voer de onderstaande stappen.
    
1. Vanuit het **IP-adressen** blad hierboven, klikt u op **dynamische** onder **toewijzing**en klik vervolgens op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gereserveerde openbare IP-](virtual-networks-reserved-public-ip.md) adressen.
- Meer informatie over [openbare IP-(ILPIP) op gebruikersniveau exemplaar](virtual-networks-instance-level-public-ip.md) -adressen.
- Raadpleeg de [gereserveerde IP-REST API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).