<properties 
   pageTitle="Het instellen van een statische privé IP-adres in de klassieke modus met behulp van de Portal Azure | Microsoft Azure"
   description="Lidmaatschap statische privé IP-adressen en hoe u ze in de klassieke modus met behulp van de Azure portal beheren"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-the-azure-portal"></a>Het instellen van een statische privé IP-adres (klassieke) in de portal van Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke. U kunt ook [een statische privé IP-adres in het implementatiemodel resourcemanager-beheren](virtual-networks-static-private-ip-arm-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

De volgende stappen uit de steekproef verwachten een eenvoudige omgeving al hebt gemaakt. Als u de stappen uitvoeren wilt, zoals deze worden weergegeven in dit document, moet u eerst de testomgeving wordt beschreven in het [maken van een vnet](virtual-networks-create-vnet-classic-pportal.md)maken.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statische privé IP-adres opgeven bij het maken van een VM
Als u wilt maken van een VM met de naam *DNS01* in het subnet *FrontEnd* van een VNet *TestVNet* met een statische privé IP-adres van *192.168.1.101*met de naam, de volgende stappen uit te voeren:

1. Via een browser, gaat u naar http://portal.azure.com en, indien nodig, meld u aan met uw Azure-account.
2. Klik op **Nieuw** > **berekenen** > **Windows Server 2012 R2 Datacenter**, melding dat de lijst **Selecteer een implementatiemodel** al ziet **klassieke**, en klik op **maken**.

    ![VM in Azure beheerportal maken](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3. Voer in het blad **VM maken** de naam van de VM wilt maken (*DNS01* in ons scenario), de lokale beheerdersaccount en het wachtwoord.

    ![VM in Azure beheerportal maken](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. Klik op **Optionele configuratie** > **netwerk** > **Virtual Network**, en klik vervolgens op **TestVNet**. Als **TestVNet** niet beschikbaar is, moet u de locatie *Central US* gebruikt en de testomgeving aan het begin van dit artikel beschreven hebt gemaakt.

    ![VM in Azure beheerportal maken](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5. In het blad **netwerk** , Controleer of het geselecteerde subnet is *FrontEnd*, en vervolgens op **IP-adressen**, onder **IP-adrestoewijzing** op **statische**en voer vervolgens een *192.168.1.101* voor **IP-adres** in zoals hieronder wordt weergegeven.

    ![VM in Azure beheerportal maken](./media/virtual-networks-static-ip-classic-pportal/figure04.png)   

6. Klik op **OK** in het blad **IP-adressen** en klik op **OK** in het **netwerk** blad en klik op **OK** in het blad **optioneel config** .
7. Klik in het blad **VM maken** op **maken**. Zoals u ziet de tegel weergegeven in uw dashboard.

    ![VM in Azure beheerportal maken](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische IP-adres privégegevens voor een VM ophalen

Als u wilt weergeven de statische privé IP-adresgegevens voor VM die zijn gemaakt met de bovenstaande stappen, in de onderstaande stappen worden uitgevoerd.

1. Klik op **Door alles bladeren**in de portal Azure Azure > **virtuele machines (klassieke)** > **DNS01** > **alle instellingen** > **IP-adressen** en kennisgeving de toewijzing van IP-adres en het IP-adres zoals hieronder wordt weergegeven.

    ![VM in Azure beheerportal maken](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statische privé IP-adres uit een VM verwijderen
Als u wilt het statische privé IP-adres verwijderen uit de VM hiervoor hebt gemaakt, de onderstaande stappen uit te voeren.
    
1. Uit het **IP-adressen** blad hierboven, klikt u op **dynamische** aan de rechterkant van de **toewijzing van IP-adressen**, en vervolgens op **Opslaan**en klik vervolgens op **Ja**.

    ![VM in Azure beheerportal maken](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statische privé IP-adres toevoegen aan een bestaande VM
Als u wilt toevoegen een statische privé IP-adres voor VM gemaakt met behulp van de bovenstaande stappen, de volgende stappen uit te voeren:

1. Klik op **statische** aan de rechterkant van de **toewijzing van IP-adres**van het **IP-adressen** blad hierboven.
2. Typ *192.168.1.101* voor **IP-adres**, en vervolgens op **Opslaan**en klik vervolgens op **Ja**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gereserveerde openbare IP-](virtual-networks-reserved-public-ip.md) adressen.
- Meer informatie over [openbare IP-(ILPIP) op gebruikersniveau exemplaar](virtual-networks-instance-level-public-ip.md) -adressen.
- Raadpleeg de [gereserveerde IP-REST API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).