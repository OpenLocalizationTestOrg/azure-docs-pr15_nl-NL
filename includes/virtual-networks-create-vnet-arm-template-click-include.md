## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>De sjabloon ARM implementeren met behulp van Klik om te implementeren

U kunt vooraf gedefinieerde ARM sjablonen uploaden naar de bibliotheek van een github beheerd door Microsoft opnieuw en open naar de community. Deze sjablonen kunnen worden geïmplementeerd rechtstreeks uit github, of gedownload en aangepast aan uw wensen voldoet. Als u wilt implementeren van een sjabloon die Hiermee maakt u een VNet met twee subnetten, de onderstaande stappen uit te voeren.

1. Navigeer naar [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates)via een browser.
2. Schuif omlaag in de lijst met sjablonen en klikt u op **101-vnet-twee-subnetten**. Kijk of het bestand **README.md** , zoals hieronder wordt weergegeven.

    ![READEME.md-bestand in github](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Klik op **implementeren naar Azure**. Voer uw Azure aanmeldingsreferenties indien nodig. 
4. Voer de waarden die u wilt gebruiken om te maken van uw nieuwe VNet in het blad **Parameters** , en klik vervolgens op **OK**. De onderstaande afbeelding ziet u de waarden voor ons scenario.

    ![Op ARM Sjabloonparameters](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. Klik op **resourceveld groep** en selecteer een resourcegroep om toe te voegen van de VNet naar, of klik op **Nieuw** om de VNet toevoegen aan een nieuwe resourcegroep. De onderstaande afbeelding ziet u de resource groepsinstellingen voor een nieuwe resourcegroep **TestRG**genoemd.

    ![Resourcegroep](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. Indien nodig, moet u de instellingen van het **abonnement** en **locatie** voor uw VNet wijzigen.
6. Als u niet de VNet als een tegel in het **Startboard**wordt weergegeven wilt, schakelt u **vastmaken aan Startboard**.
5. Klik op **Leagl termen**, lees de gebruiksrechtovereenkomst en klikt u op **kopen** te accepteren. 
6. Klik op **maken** om te maken van de VNet.

    ![Verzendende implementatie-tegel in de preview-portal](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. Als de implementatie is ingevuld, klikt u op **TestVNet** > **alle instellingen** > **subnetten** om te zien van de eigenschappen voor subnet, zoals hieronder wordt weergegeven.

    ![VNet maken in de preview-portal](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)