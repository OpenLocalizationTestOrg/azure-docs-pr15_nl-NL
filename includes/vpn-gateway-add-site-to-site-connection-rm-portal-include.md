1. Zoek uw gateway virtueel netwerk en klik op **alle instellingen** als u wilt openen van het blad **Instellingen** .

2. Klik op het blad **Instellingen** , klikt u op **verbindingen**en klik vervolgens op **toevoegen** aan de bovenkant van het blad om te openen van het blad **verbinding toevoegen** .

    ![Site-naar-Site verbinding maken](./media/vpn-gateway-add-site-to-site-connection-rm-portal-include/addconnection250.png)

3. Klik op het blad **verbinding toevoegen** , **de naam van** de verbinding. 

4. Selecteer **Site-to-site(IPSec)**voor **type verbinding**.

5. Voor **virtuele netwerkgateway**, is de waarde opgelost, omdat u verbinding van deze gateway maakt.

6. Voor **lokale netwerkgateway**, klikt u op **kiezen een gateway lokale netwerk** en selecteer het lokale netwerkgateway die u wilt gebruiken. 

7. Voor **Shared Key**, hier de waarde moet overeenkomen met de waarde die u voor uw lokale VPN-apparaat gebruikt. Als uw apparaat VPN op het lokale netwerk niet zelf een gedeelde sleutel, kunt u een vul en deze invoeren hier en op uw lokale apparaat. Het belangrijkste is dat deze beide overeenkomen.

8. De resterende waarden voor het **abonnement**, **Resourcegroep**en **locatie** worden opgelost.

9. Klik op **OK** om uw verbinding te maken. Hier ziet u *Verbinding maken* met flash op het scherm.

10. Wanneer de verbinding voltooid is, ziet u deze weergegeven in het blad **verbindingen** voor de Gateway.

    ![Site-naar-Site verbinding maken](./media/vpn-gateway-add-site-to-site-connection-rm-portal-include/connectionstatus450.png)

