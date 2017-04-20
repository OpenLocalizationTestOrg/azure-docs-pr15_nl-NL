1. Klik in de portal van **alle resources**, klikt u op **+ toevoegen**. Typ **lokale netwerkgateway**in het **Alles** blade zoekvak en klik om te zoeken. Hiermee herstelt u een lijst. Klik op het **lokale netwerkgateway** als u wilt openen van het blad en klik op **maken** om het blad **maken lokale netwerkgateway** .

    ![lokale netwerkgateway maken](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. Klik op de **maken lokale netwerk gateway blade**, Geef een **naam** voor uw LAN gateway-object.
 
3. Geef een geldige openbare **IP-adres** van de VPN-apparaat of de gateway virtueel netwerk waarnaar u een verbinding wilt maken.<br>Als het lokale netwerk een on-premises locatie vertegenwoordigt, is dit het openbare IP-adres van de VPN-apparaat dat u verbinding wilt maken. Er kan niet achter NAT en moet bereikbaar voor Azure.<br>Als het lokale netwerk een andere VNet vertegenwoordigt, moet u het openbare IP-adres dat is toegewezen aan de gateway virtueel netwerk voor die VNet opgeven.<br>

4. **Adresruimte** verwijst naar de adresbereiken voor het netwerk dat staat voor het lokale netwerk. U kunt meerdere adresbereiken ruimte toevoegen. Zorg ervoor dat de bereiken die u hier opgeeft elkaar niet overlappen met bereiken van andere netwerken waarmee u verbinding wilt maken.
 
5. Controleer voor **abonnement**, of dat het juiste abonnement wordt weergegeven.

6. Selecteer de resourcegroep die u wilt gebruiken voor **Resourcegroep**. U kunt een nieuwe resourcegroep maken, of Selecteer een die u al hebt gemaakt.

7. Voor **locatie**, selecteert u de locatie waar dit object wordt gemaakt in. Wilt u mogelijk Selecteer dezelfde locatie die uw VNet zich bevindt in, maar u niet moet doen.

8. Klik op **maken** om de gateway lokale netwerk te maken.
