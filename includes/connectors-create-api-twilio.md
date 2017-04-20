### <a name="prerequisites"></a>Vereisten voor
- Een Twilio-account
- Een gecontroleerde Twilio telefoonnummer waarop u kunt SMS ontvangen
- Een gecontroleerde Twilio-telefoonnummer waarop SMS-bericht kunt verzenden

>[AZURE.NOTE] Als u een proefaccount Twilio gebruikt, kunt u alleen SMS verzenden naar telefoonnummers **geverifieerd** .  

Voordat u uw account Twilio in een app logica gebruiken kunt, moet u de app logica verbinding maken met uw account Twilio machtigen. Gelukkig kunt u deze gemakkelijk uit stap binnen uw app logica op de Azure-Portal. 

Hier volgen de stappen voor het toestaan van uw app logica verbinding maken met uw account Twilio:

1. Als u wilt een verbinding maken met Twilio, in de ontwerpfunctie van de app logica, schakelt u **Microsoft weergeven beheerde API's** in de vervolgkeuzelijst en typt u *Twilio* in het zoekvak. Selecteer de trigger of de actie die u graag kunt gebruiken:  
  ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Als u geen verbindingen met Twilio voordat u dit nog niet hebt gemaakt, moet u wordt gevraagd uw referenties Twilio op te geven. Deze referenties worden gebruikt voor uw app logica verbinding maken met Autoriseer en toegang tot de gegevens van uw Twilio-account:  
  ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. U moet de **Twilio account-id** en **Twilio toegangstoken** vanuit het dashboard in Twilio, dus Meld u aan bij uw account Twilio nu om te trekken van deze twee soorten informatie:  
  ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Verschillende namen Twilio en logica-apps gebruiken om aan te geven van deze twee soorten informatie. Hier ziet u hoe u ze moet toewijzen aan het dialoogvenster van de apps logica:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Selecteer de knop **verbinding maken** :  
  ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Kijk naar de verbinding is gemaakt en u bent nu gratis om verder te gaan met de overige stappen in uw app logica:  
  ![](./media/connectors-create-api-twilio/twilio-5.png)