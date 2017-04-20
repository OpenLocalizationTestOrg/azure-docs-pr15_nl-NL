<properties
   pageTitle="Logica app scenario: Maak een Azure functies Service Bus-trigger | Microsoft Azure"
   description="Azure-functie gebruiken om een Service Bus trigger maken voor een app logica"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Logica app scenario: een Azure Service Bus-trigger maken met behulp van Azure-functies

Azure-functies kunt u een trigger maken voor een app logica wanneer u moet een langdurige luisteraar ervan af of taak implementeren. U kunt bijvoorbeeld een functie die wordt luister mee bij een wachtrij en een app logica als een push-signaal wordt onmiddellijk gestart maken.

## <a name="build-the-logic-app"></a>De logica-app maken

In dit voorbeeld moet u een functie die wordt uitgevoerd voor elke app logica dat moet worden geactiveerd. Maak eerst een logica-app met een HTTP-verzoek-trigger. De functie oproepen dat eindpunt wanneer een bericht wachtrij wordt ontvangen.  

1. Maak een nieuwe logica app; Selecteer de trigger **handmatig - wanneer een HTTP-aanvraag is ontvangen** .  
   Desgewenst kunt u een schema JSON gebruiken met het bericht wachtrij met behulp van een hulpmiddel zoals [jsonschema.net](http://jsonschema.net)opgeven. Plak het schema in de trigger. Hierdoor wordt de ontwerpfunctie voor meer informatie over de vorm van de gegevens en meer eigenschappen via de werkstroom voor het gemakkelijk doorlopen.
1. Voeg eventuele extra stappen die moet worden uitgevoerd nadat een wachtrij bericht is ontvangen. Bijvoorbeeld Stuur een e-mail via Office 365.  
1. Sla de logica-app als u wilt de terugbellen-URL voor de trigger voor deze app logica genereren. De URL wordt weergegeven op de kaart trigger.

![De terugbellen URL wordt weergegeven op de kaart trigger][1]

## <a name="build-the-function"></a>De functie maken

Vervolgens moet u een functie die fungeert als de trigger en luister naar de wachtrij maken.

1. Selecteer **Nieuwe functie**in de [portal van Azure-functies](https://functions.azure.com/signin), en selecteert u de **ServiceBusQueueTrigger - C#** -sjabloon.

    ![Azure functies-portal][2]

2. Configureer de verbinding met de Service Bus wachtrij (die wordt gebruikt u de SDK van Azure Service Bus `OnMessageReceive()` luisteraar ervan af).
3. Een eenvoudige functie om te bellen van de logica app eindpunt (eerder hebt gemaakt) met behulp van het bericht wachtrij als een trigger schrijven. Hier volgt een volledige voorbeeld van een functie. Het voorbeeld wordt een `application/json` bericht inhoudstype, maar u kunt dit desgewenst wijzigen.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Als u wilt testen, moet u een wachtrij-bericht via een hulpprogramma zoals [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer)toevoegen. Zie de logica app fire direct nadat de functie het bericht ontvangt.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
