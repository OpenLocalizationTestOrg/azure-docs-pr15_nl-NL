<properties 
   pageTitle="Een VM met een statische openbare IP-adres met behulp van de Azure portal in resourcemanager implementeren | Microsoft Azure"
   description="Informatie over het implementeren van VMs met een statische openbare IP-adres met behulp van de portal van de zure in resourcemanager"
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
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-portal"></a>Een VM met een statische openbare IP-adres met behulp van de Azure portal implementeren

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Een VM maken met een statische openbare IP-adres 

Als u wilt een VM maken met een statische openbare IP-adres in de portal van Azure, de onderstaande stappen uit te voeren.

1. Via een browser, Ga naar de [portal van Azure](https://portal.azure.com) en, indien nodig, meld u aan met uw Azure-account.
2. Klik op de bovenste linkerpagina hoek van de portal, op **Nieuw**>>**berekenen**>**Windows Server 2012 R2 Datacenter**.
3. Klik in de lijst **Selecteer een implementatiemodel** **Resourcemanager** selecteren en op **maken**.
4. In het blad **Basisbeginselen** , voert u de gegevens VM zoals hieronder wordt weergegeven en klik vervolgens op **OK**.

    ![Azure-portal - basisbeginselen](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)

5. Klik op **Standaard A1** zoals hieronder wordt weergegeven in het blad **Kies een grootte** en klik vervolgens op **selecteren**.

    ![Azure-portal - een tekengrootte kiezen](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)

6. In het blad **Instellingen** op **het openbare IP-adres**en klik in het blad **het openbare IP-adres maken** onder **toewijzing**, op **statische** zoals hieronder wordt weergegeven. En klik vervolgens op **OK**.

    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)

7. Klik op **OK**in het blad **Instellingen** .
8. Controleer het blad **Overzicht** , zoals hieronder wordt weergegeven en klik vervolgens op **OK**.

    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)

9. Zoals u ziet de nieuwe tegel in uw dashboard.

    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)

10. Nadat de VM is gemaakt, verschijnt het blad **Instellingen** zoals hieronder wordt weergegeven

    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)