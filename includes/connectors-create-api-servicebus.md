### <a name="prerequisites"></a>Vereisten voor

U moet een [Service Bus](https://azure.microsoft.com/services/service-bus/) -account hebt.  

Voordat u uw account Azure Service Bus in een app logica gebruiken kunt, moet u de app logica verbinding maken met uw account van de bus service machtigen. Gelukkig kunt u deze gemakkelijk uit stap binnen uw app logica op de Azure-portal.  

Hier volgen de stappen voor het toestaan van uw app logica verbinding maken met uw Service Bus-account:  

1. Als u wilt een verbinding maken met Service Bus, in de ontwerpfunctie van de app logica, selecteert u **weergeven Microsoft beheerde API's** in de vervolgkeuzelijst. Voer **service bus** in het zoekvak. Selecteer de trigger of de actie die u wilt gebruiken.  
    ![Afbeelding van de verbinding Service Bus 1](./media/connectors-create-api-servicebus/servicebus-1.png)  

2. Als u geen verbindingen met Service Bus voordat u dit nog niet hebt gemaakt, wordt u gevraagd uw referenties Service Bus op te geven. Deze referenties worden gebruikt voor het toestaan van uw app logica verbinding maken met en toegang tot de gegevens van uw Service Bus-account. De Service busconnector vereisten voor de verbindingsreeks voor de Service Bus naamruimte. Er moeten ook machtigingen **beheren** . Een goede manier om te weten als de verbindingsreeks is bedoeld voor de naamruimte of een specifieke entiteit is of deze bevat de `EntityPath` parameter. Als dat zo is, is het niet de juiste verbindingsreeks voor een app logica.  
    ![De verbindingsreeks Service Bus](./media/connectors-create-api-servicebus/connectionstring.png)

1. Nadat u de verbindingsreeks voor de naamruimte hebt ontvangen, kunt u deze kunt gebruiken voor de verbinding API in de logica Apps.  
    ![Afbeelding van de verbinding Service Bus 2](./media/connectors-create-api-servicebus/servicebus-2.png)  

3. Zoals u ziet de verbinding is gemaakt en u bent nu gratis om verder te gaan met de overige stappen in de logica-app.  
    ![Afbeelding van de verbinding Service Bus 3](./media/connectors-create-api-servicebus/servicebus-3.png)   
