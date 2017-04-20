Hier wordt uitgelegd hoe de **Service Bus - wanneer een bericht is ontvangen in een wachtrij** -trigger gebruiken om te starten een werkstroom voor het app logica wanneer een nieuw item wordt verzonden naar een Service Bus wachtrij.  

>[AZURE.NOTE]U wordt gevraagd aan te melden met uw Service Bus-verbindingsreeks als u een verbinding met de Service Bus nog niet hebt gemaakt.  

1. Voer in het zoekvak in de ontwerpfunctie van de apps logica **service bus**. Selecteer de **Service Bus - wanneer een bericht is ontvangen in een wachtrij** -trigger.  
![Afbeelding van de trigger Service Bus 1](./media/connectors-create-api-servicebus/trigger-1.png)   
- Het dialoogvenster **wanneer een bericht is ontvangen in een wachtrij** wordt weergegeven.  
![Afbeelding van de trigger Service Bus 2](./media/connectors-create-api-servicebus/trigger-2.png)   
- Voer de naam van de Service Bus wachtrij die u wilt dat de trigger om te controleren.   
![Afbeelding van de trigger Service Bus 3](./media/connectors-create-api-servicebus/trigger-3.png)   

Nu is uw app logica geconfigureerd met een trigger. Wanneer een nieuw item wordt ontvangen in de wachtrij die u hebt geselecteerd, kan de trigger een uitvoeren van de andere triggers en acties in de werkstroom wordt gestart.    
