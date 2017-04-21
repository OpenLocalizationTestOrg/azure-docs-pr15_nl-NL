
1. Op de lokale computer, moet u zich aanmelden bij de [Beheerportal van Azure](http://manager.windowsazure.com) (dit is de oude portal).

2. In de linkerbenedenhoek van het navigatiedeelvenster, selecteert u **+ Nieuw** > **App Services** > **BizTalk Service** > **Aangepaste maken**.

3. Geef een **Naam van de Service BizTalk** en selecteer een **Edition**. 

    In deze zelfstudie wordt **mobile1**. U moet een unieke naam voor uw nieuwe BizTalk Service opgeven.

4. Zodra de BizTalk-Service is gemaakt, selecteert u het tabblad **Hybride verbindingen** en klik op **toevoegen**.

    ![Hybride verbinding toevoegen](./media/hybrid-connections-create-new/3.png)

    Hiermee wordt een nieuwe hybride verbinding gemaakt.

5. Geef een **naam** en de **Hostnaam** voor uw hybride verbinding en **poort** ingesteld op `1433`. 
  
    ![Hybride verbinding configureren](./media/hybrid-connections-create-new/4.png)

    De hostnaam is de naam van de on-premises implementatie-server. Hiermee wordt de verbinding hybride voor toegang tot SQL Server wordt uitgevoerd op poort 1433. Als u een benoemde SQL Server-instantie gebruikt, gebruikt u in plaats daarvan de statische poort die u eerder hebt gedefinieerd.

6. Nadat de nieuwe verbinding is gemaakt, de status van de ziet **On-premises onvolledige installatie**van de nieuwe verbinding.

7. Ga terug naar uw mobiele service op **configureren**, schuif omlaag naar **hybride verbindingen** en klik op **toevoegen hybride verbinding**, klikt u vervolgens de hybride verbinding die u zojuist hebt gemaakt en klik op **OK**.

    Hiermee worden uw telefoonprovider uw nieuwe hybride verbinding te gebruiken.

Vervolgens moet u de hybride Connection Manager op de lokale computer installeren.