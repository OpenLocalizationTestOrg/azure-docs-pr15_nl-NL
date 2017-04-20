## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Het maken van een VNet in de portal van Azure

Als u wilt een VNet op basis van de bovenstaande scenario hebt gemaakt, de onderstaande stappen uit te voeren.

1. Via een browser, gaat u naar http://manage.windowsazure.com en, indien nodig, meld u aan met uw Azure-account.
2. Klik op **Nieuw** > **NETWERKSERVICES** > **VIRTUAL NETWORK** > **Aangepaste maken** zoals weergegeven in de onderstaande afbeelding.

    ![VNet in beheerportal maken](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. Klik op de pagina **Details van virtuele netwerk** Typ de **naam** van de VNet, selecteert u de **locatie**en klik op de pijl in de rechterbenedenhoek van de pagina om naar stap 2 te gaan. De onderstaande afbeelding ziet u de instellingen voor ons scenario.

    ![De detailpagina van virtueel netwerk](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. Geef de naam en het IP-adres van maximaal 9 DNS-servers gebruiken op de pagina **DNS-Servers en VPN Connectivity** . Als u een DNS-server niet opgeeft, wordt uw VNet de interne naming resolutie resolutie verstrekt door Azure gebruikt. In ons scenario zal we geen DNS-servers configureren.
5. Als u wilt punt-naar-site VPN toegang geven tot uw VNet, schakelt u het selectievakje **configureren een VPN-verbinding voor de punt-naar-site** . Als u een punt-naar-site VPN niet is geconfigureerd, kunt u deze toevoegen aan uw VNet op elk gewenst moment na het maken. In ons scenario wordt we niet een VPN-verbinding voor de punt-naar-site configureren.
6. Als u bieden naar website VPN-verbinding tussen uw VNet en een andere VNet of uw on-premises netwerk wilt, activeert u het vakje **configureren een VPN-verbinding voor de site-naar-site** en opgeven als u **ExpressRoute** of notitie en de naam van het netwerk met verbinding maken wilt met. Als u een VPN-verbinding voor de site-naar-site niet is geconfigureerd, kunt u deze toevoegen aan uw VNet op elk gewenst moment na het maken. In ons scenario wordt we niet een VPN-verbinding voor de site-naar-site configureren.
7. Klik op de pijl in de rechterbenedenhoek van de pagina om naar stap 3. die de onderstaande afbeelding ziet u de instellingen voor ons scenario te gaan.

    ![DNS-Servers en VPN connectivity-pagina](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. Klik op de pagina **Virtuele netwerk adres spaties** onder **IP starten**, klik op *10.0.0.0* de adresruimte VNet wijzigen en typt u de eerste adresruimte die u wilt gebruiken. Typ in ons scenario *192.168.0.0*. 
9. Selecteer het aantal bits voor het subnetmasker onder **CIDR (aantal adres)** . Selecteer voor ons scenario *16 (65536)*.
10. Klik onder **SUBNETTEN**, klikt u op *Subnet-1* en naam van het subnet, indien nodig. In ons scenario Wijzig de *FrontEnd*.

    >[AZURE.NOTE] Als u klikt u buiten het tekstvak voor een subnet is niet mogelijk om te bewerken de naam als het subnet opnieuw. Om op te lossen die, moet u het subnet verwijderen door te klikken op de knop X rechts hiervan en vervolgens een nieuwe subnet toevoegen, zoals beschreven in stap 13 hieronder.

11. Geef onder **IP starten** voor het eerste subnet, de eerste IP-adres voor het subnet. Typ *192.168.1.0*ons scenario.
12. Selecteer het aantal bits voor het subnetmasker voor het eerste subnet onder **CIDR (aantal adres)** . Selecteer voor ons scenario *24 (256)*.
13. Klik op **subnet toevoegen** als u wilt toevoegen van een nieuwe subnet, indien nodig. In ons scenario een subnet toevoegen en Herhaal stap 10-12 voor het configureren van de VNet zoals wordt weergegeven in de onderstaande afbeelding.

    ![Virtuele netwerk adres spaties pagina](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. Klik op de knop vinkje op de rechterbenedenhoek van de pagina om de VNet maken. Na een paar seconden wordt uw VNet wordt weergegeven in de lijst met beschikbare VNets, zoals wordt weergegeven in de onderstaande afbeelding.

    ![Nieuwe virtueel netwerk](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)