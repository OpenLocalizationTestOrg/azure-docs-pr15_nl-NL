## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Het maken van een VNet in de portal van Azure

Als u wilt maken van een VNet op basis van de scenario hierboven met behulp van de portal Azure preview, de onderstaande stappen uit te voeren.

1. Via een browser, gaat u naar http://portal.azure.com en, indien nodig, meld u aan met uw Azure-account.
2. Klik op **Nieuw** > **netwerkproblemen** > **virtuele netwerk**, klik op **Resource Manager** in de lijst **Selecteer een implementatiemodel** en klik vervolgens op **maken**, zoals gezien in de onderstaande afbeelding.

    ![VNet in Azure beheerportal maken](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. Klik op het blad **virtueel netwerk maken** door de VNet-instellingen te configureren, zoals wordt weergegeven in de onderstaande afbeelding.

    ![Virtuele netwerk blade maken](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. Klik op **resourceveld groep** en selecteer een resourcegroep om toe te voegen van de VNet naar, of klik op **Nieuw** om de VNet toevoegen aan een nieuwe resourcegroep. De onderstaande afbeelding ziet u de resource groepsinstellingen voor een nieuwe resourcegroep **TestRG**genoemd. Voor meer informatie over resourcegroepen, gaat u naar [Azure resourcemanager overzicht](../articles/resource-group-overview.md#resource-groups).

    ![Resourcegroep](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. Indien nodig, moet u de instellingen van het **abonnement** en **locatie** voor uw VNet wijzigen. 

6. Als u niet de VNet als een tegel in het **Startboard**wordt weergegeven wilt, schakelt u **vastmaken aan Startboard**. 

7. Klik op **maken** en de tegel **virtueel maken van netwerk** met de naam zoals wordt weergegeven in de onderstaande afbeelding ziet.

    ![Tegel van virtueel netwerk maken](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. Wacht totdat de VNet moet worden gemaakt en klik in het blad **virtuele netwerk** op **alle instellingen** > **subnetten** > **toevoegen** zoals hieronder wordt weergegeven.

    ![Subnet toevoegen in de portal van Azure](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. Geef de subnetinstellingen voor het *back-end* -subnet, zoals hieronder wordt weergegeven en klik vervolgens op **OK**. 

    ![Subnetinstellingen](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. U ziet u de lijst met subnetten, zoals wordt weergegeven in de onderstaande afbeelding.

    ![Lijst met subnetten in VNet](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
