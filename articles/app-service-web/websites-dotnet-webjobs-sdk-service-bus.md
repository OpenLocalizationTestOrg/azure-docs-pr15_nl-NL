<properties 
    pageTitle="Het gebruik van Azure Service Bus met de WebJobs SDK" 
    description="Informatie over het gebruiken van Azure Service Bus wachtrijen en onderwerpen met de WebJobs SDK." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Het gebruik van Azure Service Bus met de WebJobs SDK

## <a name="overview"></a>Overzicht

Deze handleiding bevat voorbeelden C#-code zien hoe u een proces te activeren wanneer een bericht Azure Service Bus wordt ontvangen. Voorbeelden van de code gebruik [WebJobs SDK](websites-dotnet-webjobs-sdk.md) versie 1.x.

De handleiding wordt ervan uitgegaan dat u weet [hoe u een project WebJob in Visual Studio met verbindingstekenreeksen die naar uw opslag-account verwijzen maken](websites-dotnet-webjobs-sdk-get-started.md).

De codefragmenten Toon alleen functies, niet de code die Hiermee maakt u de `JobHost` object zoals in dit voorbeeld:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

Een [voorbeeld van de code voltooid Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) is in de bibliotheek azure-webjobs-sdk-voorbeelden op GitHub.com.

## <a id="prerequisites"></a>Vereisten voor

Als u wilt werken met Service Bus die u moet het pakket [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet naast de andere WebJobs SDK-pakketten installeren. 

U moet ook de verbindingsreeks AzureWebJobsServiceBus naast de opslagruimte verbindingstekenreeksen instellen.  U kunt dit doen de `connectionStrings` sectie van het bestand App.config, zoals wordt weergegeven in het volgende voorbeeld:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Zie voor een voorbeeld van project met daarin de instelling voor de Service Bus tekenreeks in het bestand App.config, [Service Bus voorbeeld](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

De verbindingstekenreeksen kunnen ook worden ingesteld in de runtimeomgeving Azure, klikt u vervolgens de instellingen App.config heft wanneer de WebJob wordt uitgevoerd in Azure wordt aangegeven; Zie [Aan de slag met de SDK WebJobs](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account)voor meer informatie.

## <a id="trigger"></a>Hoe u een functie te activeren wanneer een Service Bus wachtrij bericht is ontvangen

Gebruikt u schrijft een functie die de WebJobs SDK belt wanneer een wachtrij bericht is ontvangen door de `ServiceBusTrigger` kenmerk. De attribuutconstructor kent een parameter waarmee de naam van de wachtrij worden gecontroleerd.

### <a name="how-servicebustrigger-works"></a>De werking van ServiceBusTrigger

De SDK ontvangt een bericht in `PeekLock` modus en oproepen `Complete` op het bericht als de functie is voltooid, of oproepen `Abandon` als de functie mislukt. Als de functie wordt uitgevoerd langer dan de `PeekLock` time-out, de vergrendeling automatisch wordt verlengd.

Service Bus omgaat met een eigen poison wachtrij die niet kan worden beheerd of kan worden geconfigureerd door de WebJobs SDK. 

### <a name="string-queue-message"></a>Tekenreeks wachtrij bericht

Het volgende voorbeeld wordt een wachtrij-bericht dat een tekenreeks bevat en de tekenreeks gegevens worden geschreven naar het dashboard WebJobs SDK gelezen.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Notitie:** Als u de berichten in wachtrij plaatsen in een toepassing die geen in de WebJobs SDK gebruikt maakt, zorg er dan voor dat [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) ingesteld op 'text/plain'.

### <a name="poco-queue-message"></a>POCO wachtrij bericht

De SDK wordt automatisch een wachtrij-bericht dat de JSON voor een POCO bevat terugconverteren [(normaal oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) type. Het volgende voorbeeld leest een wachtrij-bericht met een `BlobInformation` object waarvoor een `BlobName` eigenschap:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Zie de [opslagruimte wachtrijen versie van dit artikel](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs)voor voorbeelden van de code laat zien hoe u eigenschappen van de POCO gebruiken om te werken met BLOB's en tabellen in dezelfde functie.

Als uw code die wordt gemaakt van het bericht wachtrij niet de WebJobs SDK gebruikt, gebruikt u de code is vergelijkbaar met het volgende voorbeeld:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Typen ServiceBusTrigger werkt met

Naast `string` en POCO typen, kunt u de `ServiceBusTrigger` kenmerk met byte-matrix of een `BrokeredMessage` object.

## <a id="create"></a>Het maken van berichten van Service Bus wachtrij plaatsen

Een functie die Hiermee maakt u een nieuwe gebruik van de wachtrij-bericht schrijft de `ServiceBus` kenmerk en geven in de wachtrijnaam aan de attribuutconstructor. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Een bericht één wachtrij maken in een niet-asynchrone-functie

Een uitvoerparameter wordt in het volgende voorbeeld een nieuw bericht maken in de wachtrij met de naam "outputqueue" met dezelfde inhoud als het bericht hebt ontvangen in de wachtrij met de naam 'inputqueue'.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

De uitvoerparameter voor het maken van een bericht één wachtrij kan de volgende typen zijn:

* `string`
* `byte[]`
* `BrokeredMessage`
* Een serialiseerbaar POCO type die u definieert. Automatisch serienummer als JSON.

Voor POCO typeparameters, wordt een bericht wachtrij altijd gemaakt wanneer de functie wordt beëindigd. Als de parameter null is, geeft de SDK een wachtrij-bericht dat null retourneren zullen wanneer het bericht is ontvangen en gedeserialiseerd wordt gemaakt. Als de parameter null is is voor de andere typen, geen bericht wachtrij gemaakt.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Meerdere berichten maken of in asynchrone functies

Als u wilt meerdere berichten maken, gebruikt u de `ServiceBus` kenmerk met `ICollector<T>` of `IAsyncCollector<T>`, zoals wordt weergegeven in het volgende voorbeeld:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Elk bericht wachtrij onmiddellijk wordt gemaakt wanneer de `Add` methode wordt genoemd.

## <a id="topics"></a>Het werken met Service Bus onderwerpen

U schrijft een functie die de SDK belt wanneer een bericht is ontvangen over een onderwerp Service Bus, gebruikt u de `ServiceBusTrigger` kenmerk met de constructor waarmee de onderwerpnaam van het en de naam van abonnement, zoals wordt weergegeven in het volgende voorbeeld:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Als u wilt een bericht over een interessant onderwerp maakt, gebruikt u de `ServiceBus` kenmerk met de onderwerpnaam van een dezelfde manier als u dit gebruiken met de naam van een wachtrij.

## <a name="features-added-in-release-11"></a>Functies die zijn toegevoegd in versie 1.1

De volgende functies zijn toegevoegd in versie 1.1:

* Toestaan dat uitgebreide aanpassing van van berichten `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`aanpassing van de Service-Bus ondersteunt `MessagingFactory` en `NamespaceManager`.
* A `MessageProcessor` strategiepatroon kunt u een processor per wachtrij/onderwerp opgeven.
* Bericht verwerking gelijktijdigheid wordt al dan niet standaard ondersteund. 
* Eenvoudige aanpassing van `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.
* Toestaan dat [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) te worden opgegeven op `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (voor scenario's waar u het niet mogelijk is rechten beheren). 

## <a id="queues"></a>Verwante onderwerpen bestrijkt het opslag wachtrijen how-to artikel

Zie voor informatie over WebJobs SDK scenario's die niet specifiek zijn voor de Service Bus, [het gebruik van Azure wachtrij opslag met de SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Onderwerpen in dit artikel zijn:

* Asynchrone functies
* Meerdere exemplaren
* Correcte afsluiten
* WebJobs SDK kenmerken in de hoofdtekst van een functie gebruiken
* De SDK verbindingstekenreeksen in code instellen
* Waarden instellen voor WebJobs SDK constructor parameters in code
* Handmatig een functie activeren
* Logboeken schrijven

## <a id="nextsteps"></a>Volgende stappen

Deze handleiding biedt voorbeelden van de code die wordt aangegeven hoe u omgaat met veelvoorkomende scenario's voor het werken met Azure Service Bus. Zie voor meer informatie over het gebruik van Azure WebJobs en de SDK WebJobs [Azure WebJobs aanbevolen Resources](http://go.microsoft.com/fwlink/?linkid=390226).
 
