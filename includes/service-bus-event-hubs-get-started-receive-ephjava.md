## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Berichten met EventProcessorHost in Java ontvangen

EventProcessorHost is een Java-klasse waardoor het eenvoudiger ontvangen gebeurtenissen van gebeurtenis Hubs door beheren permanente controlepunten wordt en parallel ontvangt van die gebeurtenis Hubs. Met EventProcessorHost kunt splitsen gebeurtenissen over meerdere ontvangers, zelfs wanneer die worden gehost in verschillende knooppunten. In dit voorbeeld ziet u hoe u EventProcessorHost voor een enkele ontvanger.

###<a name="create-a-storage-account"></a>Een opslag-account maken

Pas EventProcessorHost gebruiken, moet u een [opslag van Azure-account][]hebt:

1. Meld u aan bij de [portal van Azure klassieke][]en klikt u op **Nieuw** onder aan het scherm.

2. Klik op **Gegevensservices**, en vervolgens **opslag** **Snelle maken**en typ een naam voor uw account opslag. Selecteer de gewenste regio en klik vervolgens op **Opslag-Account maken**.

    ![][11]

3. Klik op het account nieuw gemaakte opslag en klik vervolgens op **Toegangstoetsen beheren**:

    ![][12]

    Kopieer de primaire toegangstoets gebruiken verderop in deze zelfstudie.

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Een Java-project met behulp van de EventProcessor Host maken

De bibliotheek Java-client voor gebeurtenis Hubs is beschikbaar voor gebruik in Maven projecten uit het [Centrale opslagplaats Maven][Maven Package], en het gebruik van de volgende afhankelijkheid declaratie in het projectbestand Maven kan worden verwezen:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Voor verschillende soorten opbouwen omgevingen, kunt u de meest recente oppervlak bestanden expliciet ophalen uit het [Centrale opslagplaats Maven] [ Maven Package] of vanaf [het moment release verdeling GitHub](https://github.com/Azure/azure-event-hubs/releases).  

1. Voor het onderstaande voorbeeld, moet u eerst een nieuw project Maven voor een console/shell-toepassing maken in uw favoriete Java-ontwikkelomgeving. De klas wordt aangeroepen ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. De volgende code gebruiken om te maken van een nieuwe klasse ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Maken van een definitief klasse aangeroepen ```EventProcessorSample```, met de volgende code.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. De volgende velden vervangen door de waarden die worden gebruikt wanneer u de gebeurtenis Hub en opslag-account hebt gemaakt.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] In deze zelfstudie wordt één exemplaar van EventProcessorHost. Waardoor de doorvoercapaciteit, is het aanbevolen dat u meerdere exemplaren van EventProcessorHost uitvoeren. In dat geval de verschillende exemplaren automatisch afgestemd op elkaar om te kunnen taakverdeling ontvangen gebeurtenissen. Als u wilt dat meerdere ontvangers op elk proces *alle* gebeurtenissen, moet u het concept **ConsumerGroup** gebruiken. Wanneer gebeurtenissen van verschillende computers ontvangt, kan het zijn handig om op te geven van namen voor EventProcessorHost exemplaren op basis van de machines (of rol) in waarin ze zijn geïmplementeerd.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Azure opslag-account]: ../articles/storage/storage-create-storage-account.md
[Azure klassieke portal]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

