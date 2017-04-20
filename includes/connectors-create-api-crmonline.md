#### <a name="prerequisites"></a>Vereisten voor
- Een Azure-account; u kunt een [gratis account](https://azure.microsoft.com/free) maken
- Een [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) -account 

Autoriseer voordat u uw account Dynamics in een app logica, de app logica verbinding maken met uw CRM Online account. U kunt dit eenvoudig doen binnen uw app logica op de Azure-portal. 

Autoriseer uw app logica verbinding maken met uw CRM Online account met de volgende stappen:

1. Een app logica maken. Klik in de ontwerpfunctie logica Apps **weergeven Microsoft beheerde API's** in de vervolgkeuzelijst te selecteren en voer 'dynamics' in het zoekvak. Selecteer een van de triggers of acties:  
  ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Als u geen verbindingen met Dynamics nog niet eerder hebt gemaakt, wordt u gevraagd aan te melden met uw referenties Dynamics:  
  ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Selecteer **aanmelden**en voer uw gebruikersnaam en wachtwoord. Selecteer **aanmelden**. 

    Deze referenties worden gebruikt voor het toestaan van uw app logica verbinden en de gegevens in uw account Dynamics openen. 
4. Zoals u ziet dat de verbinding is gemaakt. Nu kunt u doorgaan met de overige stappen in uw app logica:  
  ![](./media/connectors-create-api-crmonline/dynamics-properties.png)
