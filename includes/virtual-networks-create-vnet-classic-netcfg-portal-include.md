## <a name="how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal"></a>Het maken van een VNet met een netwerk config-bestand in de portal van Azure

Azure gebruikt een XML-bestand om te bepalen van alle VNets beschikbaar op een abonnement. U kunt downloaden van dit bestand bewerken als u wilt wijzigen of verwijderen van bestaande VNets en nieuwe bestanden maken. In dit document, wordt u meer informatie over het downloaden van dit bestand, netwerk-configuratie (of netcgf) bestand, genoemd en bewerken als u wilt een nieuwe VNet maken. Hiermee schakelt u het [schema van Azure virtuele netwerk configuratie](https://msdn.microsoft.com/library/azure/jj157100.aspx) voor meer informatie over het configuratiebestand netwerk.

Als u wilt een VNet met behulp van een bestand netcfg via de portal van Azure hebt gemaakt, de onderstaande stappen uit te voeren.

1. Via een browser, gaat u naar http://manage.windowsazure.com en, indien nodig, meld u aan met uw Azure-account.
2. Blader omlaag in de lijst met services en klik op **netwerken te gebruiken** , zoals hieronder wordt weergegeven.

    ![Azure virtuele netwerken](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure1.gif)

3. Vanaf de onderkant van de pagina, klikt u op de knop **EXPORTEREN** , zoals hieronder wordt weergegeven.

    ![Knop exporteren](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure2.png)

4. Selecteer het abonnement dat u wilt exporteren de virtuele netwerkconfiguratie uit en klik vervolgens op de knop vinkje in de benedenhoek van de linkerpagina van de pagina op de pagina **netwerkconfiguratie exporteren** .
5. Volg de instructies van uw browser het **NetworkConfig.xml** -bestand wilt opslaan. Zorg ervoor dat u Houd er rekening mee waar u het bestand opslaat.
6. Open het bestand dat u hebt opgeslagen in stap 5 boven het gebruik van een XML- of tekst editor-toepassing en zoek de **<VirtualNetworkSites>** element. Als er geen netwerken al hebt gemaakt, elk netwerk wordt weergegeven als een eigen **<VirtualNetworkSite>** element.
7. Als wilt maken van het virtuele netwerk die worden beschreven in dit scenario, voegt u de volgende XML net onder de **<VirtualNetworkSites>** element:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

8.  Sla het bestand van de configuratie netwerk.
9.  In de portal Azure in de benedenhoek van de linkerpagina van de pagina, klikt u op **Nieuw**, en vervolgens klikt u op **NETWERKSERVICES**, en vervolgens op **VIRTUAL NETWORK**en **Configuratie importeren** klik vervolgens op zoals weergegeven in de onderstaande afbeelding.

    ![Configuratie importeren](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure3.gif)

10.  Klik op de pagina **importeren het configuratiebestand netwerk** Klik op **BROWSE voor FILE...**, en vervolgens Ga naar de map die u het bestand hebt opgeslagen in stap 8 hierboven, schakelt u het bestand en klik vervolgens op **openen**. De pagina met webonderdelen moet er ongeveer de onderstaande afbeelding. Klik op de rechterbenedenhoek van de pagina, op de pijl om te verplaatsen naar de volgende stap.

    ![Pagina importeren netwerk configuratie-bestand](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure4.png)

11.   Klik op de pagina **voor het samenstellen van uw netwerk** ziet u de vermelding voor uw nieuwe VNet, zoals wordt weergegeven in de onderstaande afbeelding.

    ![Samenstellen van de pagina van uw netwerk](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure5.png)

12.   Klik op de knop vinkje op de rechterbenedenhoek van de pagina om de VNet maken. Na een paar seconden wordt uw VNet wordt weergegeven in de lijst met beschikbare VNets, zoals wordt weergegeven in de onderstaande afbeelding.

    ![Nieuwe virtueel netwerk](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure6.png)