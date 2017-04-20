1. Ga naar **Nieuw**in de portal. Typ "Virtuele netwerkgateway" in zoeken. Zoek **virtuele netwerkgateway** in het afzender zoeken en klik op het fragment. Hiermee opent u het blad **maken virtueel netwerkgateway** .
2. Klik op **maken** onder aan het **virtuele netwerkgateway** -blad. Het blad **maken virtueel netwerkgateway** wordt geopend. Voer de waarden voor de gateway virtueel netwerk.

    ![Maken virtueel netwerk gateway blade velden] (./media/vpn-gateway-add-gw-rm-portal-include/createvnetgw300.png "Maken virtueel netwerk gateway blade velden")

3. **Naam**: naam van de gateway. Dit is niet hetzelfde als de naam van een gateway-subnet. Dit is de naam van de gateway-object dat u maakt.

4. **Gateway-type**: Selecteer **VPN**. VPN doelgateway gebruiken het virtuele netwerk gateway type **VPN**. 

5. **VPN type**: Selecteer het type VPN die is opgegeven voor uw configuratie. De meeste configuraties moeten een type Route gebaseerde VPN.

6. **SKU**: Selecteer de gateway SKU in de vervolgkeuzelijst. De SKU's in de vervolgkeuzelijst wordt weergegeven, hangt af van het type VPN dat u selecteert.

7. **Locatie**: aanpassen van het veld **locatie** te laten verwijzen naar de plaats waar uw virtuele netwerk bevindt.
 
8. Kies het virtuele netwerk waarnaar u wilt deze gateway toevoegen. Klik op **virtuele netwerk** om te openen van het blad **kiezen een virtueel netwerk** . Selecteer de VNet. Als u uw VNet niet ziet, zorg er dan voor dat het veld **locatie** wijst naar de regio waarin uw virtuele netwerk bevindt.

9. Kies een openbare IP-adres. Klik op **het openbare IP-adres** als u wilt openen van het blad **kiezen openbare IP-adres** . Klik op **+ nieuwe maken** als u wilt openen met het **openbare IP-adres blade maken**. Een naam voor uw openbare IP-adres worden ingevoerd. Deze blade Hiermee maakt u een openbare IP-adres object waaraan een openbare IP-adres dynamisch worden toegewezen.<br>Klik op **OK** om uw wijzigingen opslaan in deze blade.

10. **Abonnement**: Controleer of het juiste abonnement is geselecteerd.

11. **Resourcegroep**: met deze instelling wordt bepaald door het virtuele netwerk die u selecteert. 

12. Niet de **locatie** niet aanpassen nadat u de vorige instellingen hebt opgegeven.

13. Controleer de instellingen. Als u wilt dat de gateway wordt weergegeven op het dashboard, kunt u **vastmaken aan dashboard** onderaan in het blad selecteren.

14. Klik op **maken** om te beginnen met het maken van de gateway. De instellingen wordt worden gevalideerd en ziet u de 'implementeren virtuele netwerkgateway"tegel op het dashboard. Maken van een gateway kan maximaal 45 minuten duren. Mogelijk moet u de portal-pagina als u wilt zien van de voltooide status vernieuwen.

    ![Virtuele implementeert netwerkgateway] (./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png "Virtuele implementeert netwerkgateway")

11. Nadat de gateway is gemaakt, kunt u het IP-adres dat is toegewezen aan deze door te zoeken bij het virtuele netwerk in de portal weergeven. De gateway wordt weergegeven als een verbonden apparaat. U kunt klikken op het verbonden apparaat (uw virtueel netwerkgateway) voor meer informatie.



