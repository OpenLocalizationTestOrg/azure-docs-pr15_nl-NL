<properties 
   pageTitle="Het maken van NSGs in de ARM-modus met behulp van de Azure portal | Microsoft Azure"
   description="Meer informatie over het maken en implementeren van NSGs in ARM met behulp van de Azure portal"
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

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>Het beheren van NSGs met behulp van de Azure portal

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel worden de resourcemanager implementatiemodel. U kunt ook [NSGs in het implementatiemodel klassieke maken](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

De steekproef PowerShell opdrachten onderstaande een eenvoudige omgeving al hebt gemaakt verwachten op basis van de bovenstaande scenario. Als u uitvoeren van de opdrachten wilt, zoals deze worden weergegeven in dit document, eerst de testomgeving maken door te implementeren van [deze sjabloon](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klik op **Deploy naar Azure**, vervangt u de waarden van de parameter standaard zo nodig en volg de instructies in de portal. De onderstaande stappen gebruiken **RG-NSG** als de naam van de sjabloon is geïmplementeerd op resourcegroep.

## <a name="create-the-nsg-frontend-nsg"></a>De NSG NSG-FrontEnd maken

Als u wilt de **NSG-FrontEnd** NSG maken zoals wordt weergegeven in het bovenstaande scenario, de onderstaande stappen uit te voeren.

1. Via een browser, gaat u naar http://portal.azure.com en, indien nodig, meld u aan met uw Azure-account.
2. Klik op **Bladeren >** > **beveiligingsgroepen netwerk**.

    ![Azure-portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. Klik in het blad **beveiligingsgroepen netwerk** op **toevoegen**.
  
    ![Azure-portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. In het blad **netwerk-beveiligingsgroep maken** , een NSG met de naam *NSG-FrontEnd* in de resourcegroep *RG-NSG* maken en klik vervolgens op **maken**.

    ![Azure-portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Maak regels in een bestaande NSG

U regels maakt in een bestaande NSG van de Azure-portal door de onderstaande stappen uit te voeren.

2. Klik op **Bladeren >** > **beveiligingsgroepen netwerk**.

3. Klik in de lijst met NSGs, op **NSG-FrontEnd** > **inkomende beveiligingsregels**

    ![Azure-portal - NSG-FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. In de lijst met **regels voor binnenkomende**, klikt u op **toevoegen**.

    ![Azure-portal - regel toevoegen](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. Een regel met de naam *web-regel* met de prioriteit van *200* poort *80* voor elke VM uit elke bron van toegang via *TCP* mogen maken in het blad **regel van de binnenkomende beveiliging toevoegen** en klik vervolgens op **OK**. Zoals u ziet dat de meeste van deze instellingen standaardwaarden al zijn.

    ![Azure-portal - regelinstellingen](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Na een paar seconden ziet u de nieuwe regel in de NSG.

    ![Azure-portal - nieuwe regel](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Herhaal stap tot en met 6 om te maken van een binnenkomende regel met de naam *rdp-regel* met een prioriteit van *250* toestaan van toegang via *TCP* op poort *3389* voor elke VM uit elke bron.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>De NSG naar het subnet FrontEnd koppelen

1. Klik op **Bladeren >** > **resourcegroepen** > **RG-NSG**.
2. Klik in het blad **RG-NSG** op **…**  >  **TestVNet**.

    ![Azure-portal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. Klik in het blad **Instellingen** op **subnetten** > **FrontEnd** > **netwerk beveiligingsgroep** > **NSG-FrontEnd**.

    ![Azure-portal - Subnet-instellingen](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. Klik in het blad **FrontEnd** op **Opslaan**.

    ![Azure-portal - Subnet-instellingen](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>De NSG-BackEnd-NSG maken

Als u wilt de **NSG-BackEnd** NSG maken en deze te koppelen aan de **BackEnd** -subnet, de onderstaande stappen uit te voeren.

1. Herhaal de stappen in [de NSG NSG-FrontEnd maken](#Create-the-NSG-FrontEnd-NSG) om te maken van een NSG met de naam *NSG-back-end*
2. Herhaal de stappen in [de regels maken in een bestaande NSG](#Create-rules-in-an-existing-NSG) voor het maken van de regels voor **binnenkomende** verbindingen in de onderstaande tabel.

  	|Binnenkomende regel|Uitgaande regel|
  	|---|---|
  	|![Azure-portal - binnenkomende regel](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Azure-portal - uitgaande regel](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Herhaal de stappen in [de NSG naar het subnet FrontEnd koppelen](#Associate-the-NSG-to-the-FrontEnd-subnet) aan de **NSG-Backend** NSG koppelen aan de **BackEnd** -subnet.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [beheren van bestaande NSGs](virtual-network-manage-nsg-arm-portal.md)
- [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor NSGs.