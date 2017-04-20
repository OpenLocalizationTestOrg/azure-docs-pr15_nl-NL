<properties 
   pageTitle="Beheren met behulp van de preview-portal in resourcemanager NSGs | Microsoft Azure"
   description="Informatie over het beheren van de huidige NSGs met behulp van de preview-portal in resourcemanager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-preview-portal"></a>NSGs met behulp van de preview-portal beheren

> [AZURE.SELECTOR]
- [Portal](virtual-network-manage-nsg-arm-portal.md)
- [PowerShell](virtual-network-manage-nsg-arm-ps.md)
- [Azure CLI](virtual-network-manage-nsg-arm-cli.md)

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Gegevens ophalen

U kunt bekijken van uw bestaande NSGs, regels ophalen voor een bestaande NSG en ontdek wat resources een NSG is gekoppeld aan.

### <a name="view-existing-nsgs"></a>Bestaande NSGs weergeven
Als u wilt bekijken alle bestaande NSGs in een abonnement, de onderstaande stappen uit te voeren.

1. Via een browser, gaat u naar http://portal.azure.com en, indien nodig, meld u aan met uw Azure-account.
2. Klik op **Bladeren >** > **beveiligingsgroepen netwerk**.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Controleer de lijst met NSGs in het blad **beveiligingsgroepen netwerk** .

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

Als u wilt weergeven de lijst met NSGs in de groep **RG-NSG** resource, de onderstaande stappen uit te voeren. 

1. Klik op **resourcegroepen >** > **RG-NSG** > **...**.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. In de lijst met resources, zoekt u items weergeven van het pictogram NSG, zoals wordt weergegeven in het onderstaande **Resources** -blad.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)
         
### <a name="list-all-rules-for-an-nsg"></a>Alle regels voor een NSG weergeven

Als u wilt bekijken van de regels van een NSG **NSG-FrontEnd**met de naam, de onderstaande stappen uit te voeren. 

1. Klik in het blad **beveiligingsgroepen netwerk** of het **Resources** blad hierboven, op **NSG-FrontEnd**.
2. Klik in het tabblad **Instellingen** op **inkomende beveiligingsregels**.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. Het blad **inkomende beveiligingsregels** wordt weergegeven, zoals hieronder wordt weergegeven.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. Klik in het tabblad **Instellingen** op **uitgaande beveiligingsregels** als u wilt zien van de uitgaande regels.

>[AZURE.NOTE] Als u wilt bekijken standaardregels, klikt u op het pictogram **standaardregels** aan de bovenkant van het blad waarin de regels.

### <a name="view-nsgs-associations"></a>NSGs koppelingen weergeven

Als u wilt weergeven wat de NSG **NSG-FrontEnd is** resources koppelen aan, de onderstaande stappen uit te voeren.

1. Klik in het blad **beveiligingsgroepen netwerk** of het **Resources** blad hierboven, op **NSG-FrontEnd**.
2. Klik in het tabblad **Instellingen** op **subnetten** om weer te geven welke subnetten zijn gekoppeld aan de NSG.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. Klik in het tabblad **Instellingen** op **Netwerkinterfaces** om weer te geven wat NIC's zijn gekoppeld aan de NSG.

## <a name="manage-rules"></a>Regels beheren

U kunt regels toevoegen aan een bestaande NSG, bestaande regels bewerken en verwijderen van regels.

### <a name="add-a-rule"></a>Een regel toevoegen

Als u wilt toevoegen van een regel voor **binnenkomende** verkeer vanaf een willekeurige computer naar de **NSG-FrontEnd** NSG mogen poort **443** , de onderstaande stappen uit te voeren.

1. Klik in het blad **beveiligingsgroepen netwerk** of het **Resources** blad hierboven, op **NSG-FrontEnd**.
2. Klik in het tabblad **Instellingen** op **inkomende beveiligingsregels**.
3. Klik in het blad **inkomende beveiligingsregels** op **toevoegen**. Vervolgens in het blad **regel voor binnenkomende toevoegen** , vul de waarden in, zoals hieronder wordt weergegeven en klik vervolgens op **OK**.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

4. U ziet de nieuwe regel in het blad **inkomende beveiligingsregels** na een paar seconden.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Een regel wijzigen

Als u de regel toe te staan dat binnenkomende verkeer van **Internet** alleen hiervoor hebt gemaakt, voert u de onderstaande stappen.

1. Klik in het blad **beveiligingsgroepen netwerk** of het **Resources** blad hierboven, op **NSG-FrontEnd**.
2. Klik op de regel hebt gemaakt boven in het tabblad **Instellingen** .
3. In het blad **toestaan https** wijzigen van de eigenschap **bron** , zoals hieronder wordt weergegeven en klik vervolgens op **Opslaan**.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Een regel verwijderen

Als u wilt verwijderen van de regel die hierboven hebt gemaakt, de onderstaande stappen uit te voeren.

1. Klik in het blad **beveiligingsgroepen netwerk** of het **Resources** blad hierboven, op **NSG-FrontEnd**.
2. Klik op de regel hebt gemaakt boven in het tabblad **Instellingen** .
3. Klik in het blad **toestaan https** , klikt u op **verwijderen**en klik vervolgens op **Ja**.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Koppelingen beheren

U kunt een NSG naar subnetten en NIC's koppelen. U kunt ook een NSG uit een willekeurige deze gekoppeld aan bron koppeling.

### <a name="associate-an-nsg-to-a-nic"></a>Een NSG naar een NIC koppelen

Als u wilt koppelen het **NSG-FrontEnd** NSG naar de **TestNICWeb1** NIC, de onderstaande stappen uit te voeren.

1. Klik in het blad **beveiligingsgroepen netwerk** of het **Resources** blad hierboven, op **NSG-FrontEnd**.
2. Klik in het tabblad **Instellingen** op **Netwerkinterfaces** > **koppelen** > **TestNICWeb1**.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Koppeling van een NSG uit een NIC

Als u wilt de **NSG-FrontEnd** NSG uit de **TestNICWeb1** NIC koppeling, de onderstaande stappen uit te voeren.

1. Klik in de portal Azure op **resourcegroepen >** > **RG-NSG** > **…**  >  **TestNICWeb1**.
2. Klik in het blad **TestNICWeb1** op **wijziging beveiliging...**  > **None**.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

>[AZURE.NOTE] U kunt ook deze blade gebruiken om te koppelen van de NIC naar een bestaande NSG.

### <a name="dissociate-an-nsg-from-a-subnet"></a>Koppeling van een NSG vanuit een subnet

Als u wilt de **NSG-FrontEnd** NSG koppeling uit het **FrontEnd** subnet, de onderstaande stappen uit te voeren.

1. Klik in de portal Azure op **resourcegroepen >** > **RG-NSG** > **…**  >  **TestVNet**.
2. Klik in het blad **Instellingen** op **subnetten** > **FrontEnd** > **netwerk beveiligingsgroep** > **geen**.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. Klik in het blad **FrontEnd** op **Opslaan**.

![Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Een NSG aan een subnet koppelen

Als u wilt koppelen het **NSG-FrontEnd** NSG opnieuw de subnet, **FronEnd** , de onderstaande stappen uit te voeren.

1. Klik in de portal Azure op **resourcegroepen >** > **RG-NSG** > **…**  >  **TestVNet**.
2. Klik in het blad **Instellingen** op **subnetten** > **FrontEnd** > **netwerk beveiligingsgroep** > **NSG-FrontEnd**.
3. Klik in het blad **FrontEnd** op **Opslaan**.

>[AZURE.NOTE] U kunt ook een NSG koppelen aan een subnet van thh NSG van **Instellingen** blade.

## <a name="delete-an-nsg"></a>Een NSG verwijderen

Als dit niet is gekoppeld aan een resource, kunt u alleen een NSG verwijderen. Als u wilt verwijderen van een NSG, de onderstaande stappen uit te voeren.

1. Klik in de portal Azure op **resourcegroepen >** > **RG-NSG** > **…**  >  **NSG-FrontEnd**.
2. Klik in het blad **Instellingen** op **netwerk-interfaces**.
3. Als er een NIC's weergegeven, klikt u op de knop presentatie beveiligen en volgt u stap 2 in [Dissociate een NSG uit een NIC](#Dissociate-an-NSG-from-a-NIC).
4. Herhaal stap 3 voor elke NIC
5. Klik in het blad **Instellingen** op **subnetten**.
6. Als er een subnetten die wordt weergegeven, klikt u op het subnet en volg de stappen 2 en 3 in [Dissociate een NSG vanuit een subnet](#Dissociate-an-NSG-from-a-subnet).
7. Schuiven links naar het blad **NSG-FrontEnd** , klik vervolgens op **verwijderen** > **Ja**.

[Azure-portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Volgende stappen

- [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor NSGs.
