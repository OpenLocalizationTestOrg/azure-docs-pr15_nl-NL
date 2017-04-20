## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Het maken van een klassieke VNet in de portal van Azure

Als u wilt een klassieke VNet op basis van de bovenstaande scenario hebt gemaakt, de onderstaande stappen uit te voeren.

1. Via een browser, gaat u naar http://portal.azure.com en, indien nodig, meld u aan met uw Azure-account.
2. Klik op **Nieuw** > **netwerkproblemen** > **virtuele netwerk**, melding dat de lijst **Selecteer een implementatiemodel** al ziet **klassieke**en klik vervolgens op **maken**, zoals gezien in de onderstaande afbeelding.

    ![VNet in Azure beheerportal maken](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. Klik op het blad **virtuele netwerk** , typ de **naam** van de VNet en klik op **-adresruimte**. De instellingen van uw adres ruimte voor de VNet en de eerste subnet configureren en klik op **OK**. De onderstaande afbeelding ziet u de instellingen voor het blokkeren van CIDR voor ons scenario.

    ![Adres ruimte blade](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. Klik op **Resourcegroep** en selecteer een resourcegroep om toe te voegen van de VNet naar, of klik op **nieuwe resourcegroep maken** om de VNet toevoegen aan een nieuwe resourcegroep. De onderstaande afbeelding ziet u de resource groepsinstellingen voor een nieuwe resourcegroep **TestRG**genoemd. Voor meer informatie over resourcegroepen, gaat u naar [Azure resourcemanager overzicht](../articles/virtual-network/resource-group-overview.md#resource-groups).

    ![Resource groep blade maken](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. Indien nodig, moet u de instellingen van het **abonnement** en **locatie** voor uw VNet wijzigen. 

6. Als u niet de VNet als een tegel in het **Startboard**wordt weergegeven wilt, schakelt u **vastmaken aan Startboard**. 

7. Klik op **maken** en de tegel **virtueel maken van netwerk** met de naam zoals wordt weergegeven in de onderstaande afbeelding ziet.

    ![VNet in beheerportal maken](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. Wacht totdat de VNet moet worden gemaakt en wanneer u de onderstaande tegel ziet, klikt u op als u wilt meer subnetten toevoegen.

    ![VNet in beheerportal maken](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. Ziet u de **configuratie** voor uw VNet zoals hieronder wordt weergegeven. 

    ![VNet in beheerportal maken](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. Klik op **subnetten** > **toevoegen**, typ een **naam** en geef een **adresbereiken (CIDR blok)** voor uw subnet, en klik vervolgens op **OK**. De onderstaande afbeelding ziet u de instellingen voor ons huidige scenario.

    ![VNet in Azure beheerportal maken](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)