## <a name="receive-messages-with-eventprocessorhost"></a>Berichten met EventProcessorHost ontvangen

[EventProcessorHost][] is een .NET-klasse waardoor het eenvoudiger ontvangen gebeurtenissen van gebeurtenis Hubs door beheren permanente controlepunten wordt en parallel ontvangt van die gebeurtenis Hubs. [EventProcessorHost][]gebruikt, kunt u splitsen gebeurtenissen over meerdere ontvangers, zelfs wanneer die worden gehost in verschillende knooppunten. In dit voorbeeld ziet u hoe u [EventProcessorHost][] voor een enkele ontvanger. De [schaal van de gebeurtenis processing][] voorbeeld ziet hoe u [EventProcessorHost][] gebruiken met meerdere ontvangers.

Als u wilt gebruiken [EventProcessorHost][], moet u een [opslag van Azure-account][]hebt:

1. Meld u aan bij de [portal van Azure][]en klikt u op **Nieuw** aan de bovenkant van het scherm naar links.

2. Klik op **gegevens + opslagruimte**en klik op de **opslag-account**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. Typ een naam voor de opslag-account in het blad **opslag-account maken** . Kies een Azure-abonnement, resourcegroep en locatie voor het maken van de resource. Klik vervolgens op **maken**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. Klik op het account gemaakte opslag in de lijst met accounts van opslag.

5. Klik in het blad opslag-account op **toegangstoetsen**. Kopieer de waarde van **sleutel1** gebruiken verderop in deze zelfstudie.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. Visual Studio, door een nieuw visueel C# bureaublad-App-project met de project-sjabloon van **Console-toepassing** te maken. De naam van de **ontvanger**van het project.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. In Solution Explorer met de rechtermuisknop op de oplossing en klik vervolgens op **NuGet-pakketten beheren voor oplossing**.

6. Klik op het tabblad **Bladeren** en zoek vervolgens voor `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Zorg ervoor dat de naam van het project (**ontvanger**) is opgegeven in het vak **versie (s)** . Klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Visual Studio is gedownload, installaties en voegt een verwijzing naar de [Azure Service Bus gebeurtenis Hub - EventProcessorHost NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)met alle bijbehorende afhankelijkheden.

7. Met de rechtermuisknop op het project **ontvanger** , klikt u op **toevoegen**en klik vervolgens op **Class**. De nieuwe klasse **SimpleEventProcessor**een naam en klik vervolgens op **toevoegen** om de klasse te maken.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. De volgende instructies toevoegen aan de bovenkant van het bestand SimpleEventProcessor.cs:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Vervolgens vervangt door de volgende code voor het hoofdgedeelte van de klas:

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ```

    Deze klasse wordt aangeroepen door de **EventProcessorHost** om gebeurtenissen te verwerken van de Hub gebeurtenis ontvangen. Houd er rekening mee dat het `SimpleEventProcessor` klasse een stopwatch gebruikt om te bellen regelmatig de methode samenvatting van de context **EventProcessorHost** . Dit zorgt ervoor dat, als de ontvanger opnieuw is opgestart, niet meer dan vijf minuten verloren gaat van het verwerken van werk.

9. Voeg het volgende in de klas **programma** `using` instructie boven aan het bestand:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Sluit de `Main` methode in de `Program` klasse met de volgende code, wordt vervangen door de naam van de gebeurtenis Hub en de naamruimte niveau verbindingsreeks die u eerder hebt opgeslagen en de opslag-account en de sleutel die u hebt gekopieerd in de vorige gedeelten. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
      string eventHubName = "{Event Hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      var options = new EventProcessorOptions();
      options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

> [AZURE.NOTE] In deze zelfstudie wordt één exemplaar van [EventProcessorHost][]. Als u wilt vergroten doorvoer, wordt het aanbevolen dat u meerdere exemplaren van [EventProcessorHost][], uitvoeren zoals wordt weergegeven in de steekproef [schaal verwerking van de gebeurtenis][] . In dat geval de verschillende exemplaren automatisch afgestemd op elkaar voor het verdelen ontvangen gebeurtenissen. Als u wilt dat meerdere ontvangers op elk proces *alle* gebeurtenissen, moet u het concept **ConsumerGroup** gebruiken. Wanneer gebeurtenissen van verschillende computers ontvangt, kan het zijn handig om op te geven van namen voor [EventProcessorHost][] exemplaren op basis van de machines (of rol) in waarin ze zijn geïmplementeerd. Zie de [Gebeurtenis Hubs overzicht][] en [Gebeurtenis Hubs Programming Guide][] -onderwerpen voor meer informatie over deze onderwerpen.

<!-- Links -->
[Overzicht van de Hubs gebeurtenissen]: ../articles/event-hubs/event-hubs-overview.md
[Gebeurtenis-Hubs handleiding voor het programmeren]: ../articles/event-hubs/event-hubs-programming-guide.md
[Schaal gebeurtenis verwerking]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Azure opslag-account]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure-portal]: https://portal.azure.com