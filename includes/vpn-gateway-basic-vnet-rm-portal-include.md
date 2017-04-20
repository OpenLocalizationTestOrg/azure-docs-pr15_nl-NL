Als u wilt een VNet in het implementatiemodel resourcemanager maken met behulp van de Azure-portal, de onderstaande stappen uit te voeren. De schermafbeeldingen die worden als voorbeelden gegeven. Zorg ervoor dat de waarden vervangen door uw eigen. Zie voor meer informatie over het werken met virtuele netwerken [Virtuele netwerk overzicht](../articles/virtual-network/virtual-networks-overview.md).

1. Via een browser, Ga naar de [portal van Azure](http://portal.azure.com) en, indien nodig, meld u aan met uw Azure-account.

2. Klik op **Nieuw**. Typ in het **vak Zoeken de marketplace** , "Virtual Network". Zoek **Virtual Network** in de resulterende lijst en klik op om het blad **Virtual Network** .

    ![Zoek Virtual Network resource blade] (./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Zoek virtueel netwerk resource blade")

3. Onderaan in het blad Virtual Network, klikt u in de lijst **Selecteer een implementatiemodel** **Resourcemanager**selecteren en klik vervolgens op **maken**.


    ![Selecteer resourcemanager] (./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Selecteer resourcemanager")

4. Klik op het blad **virtueel netwerk maken** door de VNet-instellingen te configureren. Als u in de velden ingevuld, worden de rood uitroepteken zodat een groen vinkje wanneer de tekens die zijn ingevoerd in het veld geldig zijn.

    ![Validatie van veld] (./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Validatie van veld")

5. Het blad **virtueel netwerk maken** lijkt op het volgende voorbeeld. Er zijn mogelijk waarden die automatisch ingevuld worden. Als dat het geval is, kunt u de waarden vervangen door uw eigen.

    ![Maken virtueel netwerk blade] (./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Maken virtueel netwerk blade")

6. **Naam**: Voer een naam voor uw virtuele netwerk.

7. **Adresruimte**: Voer de adresruimte. Als er meerdere adres spaties om toe te voegen, moet u uw eerste adresruimte toevoegen. U kunt extra adres spaties later toevoegen na het maken van de VNet.
 
8. **De naam van de subnet**: de subnetnaam en subnet adresbereiken toevoegen. U kunt extra subnetten later toevoegen na het maken van de VNet.

10. **Abonnement**: Controleer of u het abonnement dat is vermeld de juiste. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.

11. **Resourcegroep**: Selecteer een bestaande resourcegroep of maak een nieuwe record door een naam voor de nieuwe resourcegroep te typen. Als u een nieuwe groep maakt, Geef een naam de resourcegroep op basis van de geplande configuratiewaarden. Voor meer informatie over resourcegroepen, gaat u naar [Azure resourcemanager overzicht](resource-group-overview.md#resource-groups).

12. **Locatie**: Selecteer de locatie voor uw VNet. De locatie bepaalt waar de resources die u dashboard naar deze VNet implementeren zich bevinden.

13. Selecteer **vastmaken aan dashboard** als u wel uw VNet terugvinden op het dashboard wilt, en klik vervolgens op **maken**.
    
    ![Vastmaken aan het dashboard] (./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "vastmaken aan het dashboard")

14. Nadat u hebt geklikt **maken**, ziet u een tegel op het dashboard die de voortgang van uw VNet worden doorgevoerd. De tegel verandert wanneer de VNet wordt gemaakt.

    ![Tegel van virtueel netwerk maken] (./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Tegel van virtueel netwerk maken")