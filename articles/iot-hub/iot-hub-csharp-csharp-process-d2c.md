<properties
    pageTitle="Verwerking van IoT Hub apparaat naar cloud berichten (.Net) | Microsoft Azure"
    description="Volg deze zelfstudie handig patronen te verwerken IoT Hub apparaat naar cloud berichten meer."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Zelfstudie: Het verwerken van IoT Hub apparaat naar cloud berichten met .net

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Inleiding

Azure IoT Hub heeft een volledig beheerde service betrouwbare waarmee en beveiligde bidirectionele communicatie tussen miljoenen IoT apparaten en een toepassing een back-end. Andere zelfstudies ([aan de slag met IoT Hub] en het [verzenden van berichten van de cloud-naar-apparaat met IoT Hub][lnk-c2d]) hoe u met het apparaat naar cloud en cloud-to-device SMS basisfuncties van IoT Hub.

Deze zelfstudie is gebaseerd op de code die wordt weergegeven in deze zelfstudie [aan de slag met IoT Hub] en twee scalable patronen die u gebruiken kunt voor de verwerking apparaat naar cloud berichten worden weergegeven:

- De betrouwbare opslag van apparaat-naar-cloud-berichten in [Azure-blobopslag]. Een gebruikelijk is *koudwatersystemen pad* analytics, waarin u telemetriegegevens in BLOB's wilt gebruiken als invoer in analytics-processen opslaan. Deze processen kunnen worden aangestuurd door hulpprogramma's zoals [Factory van Azure-gegevens] of de stapel [HDInsight (Hadoop)] .

- De betrouwbare verwerking van *interactieve* apparaat naar cloud-berichten. Apparaat-naar-cloud berichten zijn interactief wanneer ze direct triggers voor een reeks acties in de back-end van toepassing zijn. Een apparaat kan bijvoorbeeld een waarschuwing weergegeven die een tickets invoegen in een CRM-systeem activeert verzenden. Berichten *gegevenspunt* feed daarentegen gewoon in een analyse-engine. Temperatuur telemetrielogboek vanaf een apparaat dat moet worden opgeslagen voor later analyse is bijvoorbeeld een bericht van de punt gegevens.

Omdat IoT Hub een [Gebeurtenis Hub beschrijft][lnk-event-hubs]-compatibele eindpunt apparaat naar cloud om berichten te ontvangen, deze zelfstudie gebruikt een exemplaar [EventProcessorHost] . Dit exemplaar:

* Er worden berichten *gegevenspunt* betrouwbaar opgeslagen in Azure-blobopslag.
* Stuurt *interactieve* apparaat naar cloud naar een Azure [Service Bus wachtrij] voor onmiddellijke verwerking.

Service Bus zorgt betrouwbaar verwerking van interactieve berichten, aangezien deze per bericht controlepunten en tijd venster verval dupliceren biedt.

> [AZURE.NOTE] Een exemplaar **EventProcessorHost** is slechts één manier om interactieve berichten te verwerken. Andere opties zijn onder andere [Azure Service stof] [ lnk-service-fabric] en [Azure Stream Analytics][lnk-stream-analytics].

Aan het einde van deze zelfstudie, moet u drie apps voor Windows-console uitvoeren:

* **SimulatedDevice**, een gewijzigde versie van de app gemaakt in de zelfstudie [aan de slag met IoT-Hub] , verzendt gegevens punt apparaat naar cloud berichten elke tweede en interactieve apparaat naar cloud-berichten elke 10 seconden. Deze app gebruikt het protocol AMQP om te communiceren met IoT Hub.
* **ProcessDeviceToCloudMessages** gebruikt de klasse [EventProcessorHost] berichten vanuit het eindpunt van de gebeurtenis Hub-compatibele ophaalt. Deze vervolgens betrouwbaar opslaan punt berichten van gegevens in Azure-blobopslag, en interactieve berichten naar een Service Bus wachtrij worden doorgestuurd.
* **ProcessD2CInteractiveMessages** wachtrijen uit de interactieve berichten uit de wachtrij Service Bus.

> [AZURE.NOTE] IoT Hub heeft SDK ondersteuning voor veel talen, inclusief C, Java en JavaScript en platforms voor apparaten. Meer informatie over hoe u het apparaat dat gesimuleerd in deze zelfstudie vervangen door een fysiek apparaat en hoe u de apparaten verbinden met een IoT Hub, raadpleegt u het [Azure IoT Developer Center].

Deze zelfstudie is rechtstreeks van toepassing op andere manieren gebeurtenis Hub-compatibele berichten, zoals [HDInsight (Hadoop)] projecten in beslag neemt. Zie [Azure IoT Hub-handleiding voor ontwikkelaars - apparaat naar cloud]voor meer informatie.

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

+ Microsoft Visual Studio 2015.

+ Een actieve Azure-account. <br/>Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) maken in een paar minuten.

U kunt bepaalde basiskennis van [Azure Storage] en [Azure Service Bus]nodig hebt.


## <a name="send-interactive-messages-from-a-simulated-device"></a>Interactieve berichten verzenden vanaf een gesimuleerd apparaat

In deze sectie, kunt u de gesimuleerd apparaattoepassing die u hebt gemaakt in de zelfstudie [aan de slag met IoT Hub] interactieve apparaat naar cloud-berichten verzenden naar de hub IoT wijzigen.

1. Visual Studio, in het project **SimulatedDevice** toevoegen de volgende methode aan de klas **programma** .

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Deze methode is vergelijkbaar met de methode **SendDeviceToCloudMessagesAsync** in het project **SimulatedDevice** . De enige verschillen zijn dat u de eigenschap **MessageId** system nu instellen en de gebruikerseigenschap van een **messageType**genoemd.
    De code wordt een globaal unieke id (GUID) toegewezen aan de eigenschap **MessageId** . De Service-Bus kunt u deze id gebruiken opgeheven dupliceren de berichten die zijn ontvangen. Dit voorbeeld worden de eigenschap **messageType** om te onderscheiden interactieve van berichten van gegevens punt. De toepassing geeft deze informatie in de berichteigenschappen van het, in plaats van in de berichttekst, zodat de gebeurtenis processor niet hoeft te converteren van het bericht om uit te voeren routering van berichten.

    > [AZURE.NOTE] Het is belangrijk om te maken van de **MessageId** opgeheven dupliceren interactieve berichten in de apparaatcode gebruikt. Communicatie van netwerk af en toe of andere fouten, kunnen leiden tot meerdere malen verzenden van hetzelfde bericht van dat apparaat. U kunt ook een semantische bericht-ID, zoals een hash van de gegevensvelden relevante bericht, in plaats van een GUID gebruiken.

2. De volgende methode in de methode **Main** toevoegen rechts voordat de `Console.ReadLine()` lijn:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Omdat dit eenvoudiger, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u een beleid opnieuw zoals exponentiële backoff, zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren.

## <a name="process-device-to-cloud-messages"></a>CMYK-apparaat in cloud-berichten

In dit gedeelte maakt u een console-app voor Windows die apparaat-naar-cloud berichten van IoT Hub verwerkt. IOT Hub beschrijft een [gebeurtenis Hub]-compatibele eindpunt een toepassing apparaat naar cloud-mailberichten te lezen inschakelen. Deze zelfstudie gebruikt de klasse [EventProcessorHost] verwerkingstijd van deze berichten in een console-app. Zie voor meer informatie over het proces berichten van gebeurtenis Hubs de zelfstudie [Aan de slag met Hubs gebeurtenis] .

De uitdaging wanneer u betrouwbare opslag van gegevens-punts berichten implementeren of doorsturen van berichten interactieve de verwerking van die gebeurtenis is, is afhankelijk van de bericht-consument om de voortgang van controlepunten voorzien. Daarnaast wordt om een hoge doorvoersnelheden tijdens het lezen van gebeurtenis Hubs dient u controlepunten in grote hoeveelheden. Deze methode Hiermee maakt u de mogelijkheid van dubbele processing voor een groot aantal berichten als er een fout is en u naar het vorige controlepunt terugkeren. In deze zelfstudie ziet u Azure Storage schrijft en Service Bus verval dupliceren windows synchroniseren met **EventProcessorHost** controlepunten.

U schrijft betrouwbaar berichten met Azure Storage door de steekproef gebruikt de functie voor het doorvoeren van afzonderlijke blokkeren van [blok BLOB's][Azure Block Blobs]. De gebeurtenis processor tijdje berichten in het geheugen staan totdat deze keer op te geven van een samenvatting. Bijvoorbeeld nadat de samengevoegde buffer van berichten bereikt de maximale blokgrootte van 4 MB of na de Service-Bus verval dupliceren tijdvenster is verstreken. Vervolgens gaat u een nieuw blok worden in de code voordat het controlepunt, doorgevoerd in de blob.

De gebeurtenis-processor gebruikt gebeurtenis Hubs verplaatst u bericht als blok id's. Deze methode kan de gebeurtenis processor verval controleert uitvoeren voordat deze de nieuw blok worden doorgevoerd in opslag, verzorgen van mogelijke vastlopen tussen een blok en het controlepunt doorvoeren.

> [AZURE.NOTE] Deze zelfstudie één Azure Storage account gebruikt om te schrijven van alle berichten die zijn opgehaald uit IoT Hub. Zie [Azure Storage schaalbaarheid richtlijnen]om te bepalen of u nodig hebt voor het gebruik van meerdere Azure Storage-accounts in uw oplossing.

De toepassing gebruikt de functie van de dubbele Service Bus om te voorkomen van dubbele waarden bij interactieve berichten worden verwerkt. Het apparaat dat gesimuleerd stempel van elk interactieve bericht met een unieke **MessageId**. Deze id's inschakelen Service Bus om ervoor te zorgen dat, geen twee berichten met de dezelfde **MessageId** in het venster van de tijd opgegeven verval dupliceren aan de ontvangers worden geleverd. Deze verval duplicaten, samen met de per bericht voltooiing semantiek verstrekt door de Service Bus wachtrijen, kunt u heel gemakkelijk de betrouwbare verwerking van interactieve berichten implementeren.

Om ervoor te zorgen dat er wordt geen bericht opnieuw wordt ingediend buiten het venster verval dupliceren, wordt in de code de **EventProcessorHost** controlepunt om synchroniseert met het venster met de Service Bus verval dupliceren. Deze synchronisatie wordt uitgevoerd door een controlepunt ten minste eenmaal afdwingen telkens wanneer het venster verval dupliceren verstrijkt (in deze zelfstudie, is het venster het één uur).

> [AZURE.NOTE] In deze zelfstudie wordt één gepartitioneerde Service Bus wachtrij aan alle berichten in de interactieve opgehaald uit IoT Hub proces. Zie de documentatie [Azure Service Bus] voor meer informatie over het gebruik van Service Bus wachtrijen om te voldoen aan de schaalbaarheidsvereisten van de oplossing.

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Een opslag van Azure-account en een wachtrij Service Bus inrichten
Als u wilt gebruiken in de klas [EventProcessorHost] , moet u een opslag van Azure-account om in te schakelen van de **EventProcessorHost** controlepunt informatie opnemen. U kunt een bestaand opslag van Azure-account of volg de instructies in [Over Azure-opslag] naar een nieuwe record maken. Noteer de verbindingsreeks van de opslag van Azure-account.

> [AZURE.NOTE] Wanneer u kopieert en de verbindingsreeks van de opslag van Azure-account plakt, zorg er zijn geen spaties opgenomen.

Moet u ook een Service Bus wachtrij naar betrouwbaar verwerking van interactieve berichten inschakelen. Zoals wordt uitgelegd [hoe u met de Service Bus wachtrijen][Service Bus wachtrij], kunt u via programmacode, met een één uur verval dupliceren-venster een wachtrij maken. U kunt ook de [Azure klassieke portal]gebruiken[lnk-classic-portal], door deze stappen uit:

1. Klik op **Nieuw** in de linkerbenedenhoek. Klik vervolgens op **App-Services** > **Service Bus** > **wachtrij** > **Aangepaste maken**. Voer de naam **d2ctutorial**, selecteert u een gebied, en een bestaande naamruimte gebruiken of een nieuwe record maken. Op de volgende pagina, selecteert u **dubbele detectie inschakelen**en stel de **detectie geschiedenis tijdvenster dupliceren** op één uur. Klik vervolgens op het vinkje in de rechterbenedenhoek om op te slaan van uw configuratie wachtrij.

    ![Maken van een wachtrij in Azure-portal][30]

2. Klik in de lijst van Service Bus wachtrijen **d2ctutorial**op en klik vervolgens op **configureren**. Maak twee gedeelde-beleidsregels, één genoemd **verzenden** met machtigingen voor **verzenden** en één genoemd **beluisteren** met **beluisteren** machtigingen. Als u klaar bent, klikt u onderaan op **Opslaan** .

    ![Een wachtrij configureren in Azure-portal][31]

3. Klik op **Dashboard** boven, en klik vervolgens **verbindingsgegevens** onderaan. Maak een notitie van de twee tekenreeksen.

    ![Wachtrij dashboard in Azure-portal][32]

### <a name="create-the-event-processor"></a>De processor gebeurtenis maken

1. Klik in de huidige Visual Studio-oplossing, een visuele C# Windows als project wilt maken met behulp van het project-sjabloon van **Console-toepassing** , op **bestand** > **toevoegen** > **Nieuw Project**. Zorg ervoor dat de .NET Framework-versie is 4.5.1 of hoger. Naam van het project **ProcessDeviceToCloudMessages**en klik op **OK**.

    ![Nieuw project in Visual Studio][10]

2. In Solution Explorer met de rechtermuisknop op het project **ProcessDeviceToCloudMessages** en klik vervolgens op **NuGet-pakketten beheren**. Het dialoogvenster **NuGet Package Manager** wordt weergegeven.

3. Zoeken naar **WindowsAzure.ServiceBus**, klikt u op **installeren**en accepteer de gebruiksvoorwaarden. Deze bewerking is gedownload, installaties en voegt een verwijzing naar het [pakket van Azure Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus), met alle bijbehorende afhankelijkheden.

4. Zoeken naar **Microsoft.Azure.ServiceBus.EventProcessorHost**, klikt u op **installeren**en accepteer de gebruiksvoorwaarden. Deze bewerking is gedownload, installaties en voegt een verwijzing naar de [Azure Service Bus gebeurtenis Hub - EventProcessorHost NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)met alle bijbehorende afhankelijkheden.

5. Met de rechtermuisknop op het project **ProcessDeviceToCloudMessages** , klikt u op **toevoegen**en klik vervolgens op **Class**. De nieuwe klasse **StoreEventProcessor**een naam en klik vervolgens op **OK** om de klasse te maken.

6. De volgende instructies toevoegen aan de bovenkant van het bestand StoreEventProcessor.cs:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. De volgende code voor het hoofdgedeelte van de klas vervangen:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    De klasse **EventProcessorHost** oproepen deze klasse te verwerken van IoT Hub ontvangen van berichten met het apparaat naar cloud. De code in deze klasse implementeert de logica betrouwbaar opslaan van berichten in een container blob en interactieve berichten doorsturen naar de wachtrij Service Bus.

    De methode **OpenAsync** geïnitialiseerd de variabele **currentBlockInitOffset** , die de huidige verschoven ten opzichte van het eerste bericht lezen door deze gebeurtenis processor bijhoudt. Houd er rekening mee dat elke processor verantwoordelijk voor een enkele partition is.

    De methode **ProcessEventsAsync** een reeks berichten ontvangt van IoT Hub en deze als volgt verwerkt: deze interactieve berichten verzendt naar de wachtrij Service Bus en punt-berichten van gegevens toegevoegd aan de geheugenbuffer **toAppend**genoemd. Als de geheugenbuffer de limiet van 4 MB bereikt of de vensters van de tijd verval dupliceren verstrijkt (één uur na een controlepunt in deze zelfstudie), klikt u vervolgens gebeurtenis de toepassing wordt een samenvatting.

    De methode **AppendAndCheckpoint** genereert eerst een blockId voor het blokkeren als u wilt toevoegen. Azure opslag alleen alle id's als u wilt dat dezelfde lengte, zodat de methode pads de verschoven met voorloopnullen - blokkeren `currentBlockInitOffset.ToString("0000000000000000000000000")`. Vervolgens gaat u als een blok met deze ID al in de blob is, worden deze in de methode door de huidige inhoud van de buffer overschreven.

    > [AZURE.NOTE] Om te vereenvoudigen de code, wordt een enkel blob per partition in deze zelfstudie gebruikt voor de opslag van de berichten. Een echte oplossing wilt implementeren bestand schuivend door te maken van extra bestanden na een bepaalde tijd, of wanneer ze een bepaalde grootte hebt bereikt. Houd er rekening mee dat een blok Azure blob maximaal 195 GB aan gegevens bevatten.

8. Toevoegen de volgende instructie met **behulp van** boven in de klas **programma** :

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. De methode **Main** in de klas **programma** als volgt wijzigen. **{Hub-verbindingsreeks iot}** vervangen door de verbindingsreeks **iothubowner** uit de handleiding [aan de slag met IoT Hub] . Vervang de verbindingsreeks opslag door de verbindingsreeks die u aan het begin van deze sectie hebt genoteerd. De verbindingsreeks van de Service Bus vervangen door machtigingen voor de wachtrij met de naam **d2ctutorial** die u aan het begin van deze sectie hebt genoteerd **verzenden** :

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Omdat dit eenvoudiger, wordt deze zelfstudie gebruikt één exemplaar van de klasse [EventProcessorHost] . Zie de [Gebeurtenis Hubs Programming Guide]voor meer informatie.

## <a name="receive-interactive-messages"></a>Interactieve berichten ontvangt
In deze sectie, kunt u een Windows-console-app die de interactieve berichten van de Service Bus wachtrij ontvangt schrijven. Zie [toepassingen met Service Bus met meerdere niveaus maken][]voor meer informatie over het ontwerpen van een oplossing die Service Bus gebruikt.

1. In de huidige Visual Studio-oplossing, door een Visual C# Windows-project te maken met behulp van het project-sjabloon van **Console-toepassing** . De naam van het project **ProcessD2CInteractiveMessages**.

2. In Solution Explorer met de rechtermuisknop op het project **ProcessD2CInteractiveMessages** en klik vervolgens op **NuGet-pakketten beheren**. Deze bewerking weergegeven de **NuGet Package Manager** .

3. Zoeken naar **WindowsAzure.ServiceBus**, klikt u op **installeren**en accepteer de gebruiksvoorwaarden. Deze bewerking is gedownload, installaties en voegt een verwijzing naar de [Bus van Azure-Service](https://www.nuget.org/packages/WindowsAzure.ServiceBus), met alle bijbehorende afhankelijkheden.

4. De volgende instructies voor het **gebruik van** toevoegen aan de bovenkant van het bestand **Program.cs** :

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Tot slot toevoegen de volgende regels aan de methode **Main** . Vervang de verbindingsreeks met machtigingen voor de wachtrij met de naam **d2ctutorial** **beluisteren** :

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1.  In Visual Studio, in Solution Explorer met de rechtermuisknop op uw oplossing en selecteer **Opstarten projecten instellen**. Selecteer **meerdere opstarten projecten**en selecteer **starten** als de actie voor de **ProcessDeviceToCloudMessages**, **SimulatedDevice**en **ProcessD2CInteractiveMessages** projecten.

2.  Druk op **F5** om de drie consoletoepassingen te starten. De toepassing **ProcessD2CInteractiveMessages** moet elk interactieve bericht verzonden vanuit de toepassing **SimulatedDevice** worden verwerkt.

  ![Drie consoletoepassingen][50]

> [AZURE.NOTE] Als u wilt zien-updates in uw blob, moet u mogelijk de constante **MAX_BLOCK_SIZE** in de klas **StoreEventProcessor** beperken tot een kleinere waarde, zoals **1024**. Deze wijziging is handig omdat het duurt enige tijd om te de bestandslimiet blok met de gegevens die zijn verzonden door het gesimuleerd apparaat hebt bereikt. Een kleinere blokkeren hoeft u niet te wachten zo lang om te zien van de blob wordt gemaakt en bijgewerkt. Gebruik van blok groter is echter de toepassing meer scalable.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe betrouwbaar verwerken gegevenspunt en interactieve apparaat naar cloud berichten met behulp van de klasse [EventProcessorHost] .

[Het verzenden van berichten van de cloud-naar-apparaat met IoT Hub] [ lnk-c2d] ziet u hoe u berichten verzenden naar uw apparaten van uw back-end.

Voorbeelden van volledige end-to-end-oplossingen die IoT Hub gebruikt, raadpleegt u [Azure IoT Suite][lnk-suite].

Meer informatie over het ontwikkelen van oplossingen met IoT-Hub, raadpleegt u de [Handleiding voor ontwikkelaars van IoT Hub].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Azure-blobopslag]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure gegevens Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Service Bus wachtrij]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT Hub handleiding voor ontwikkelaars - apparaat naar cloud]: iot-hub-devguide-messaging.md

[Azure-opslag]: https://azure.microsoft.com/documentation/services/storage/
[Azure-Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Handleiding voor ontwikkelaars van IoT Hub]: iot-hub-devguide.md
[Aan de slag met IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Tijdelijke foutenstructuuranalyse afhandeling]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Over Azure opslag]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Aan de slag met Hubs gebeurtenis]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure opslag schaalbaarheid richtlijnen]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Gebeurtenis-Hubs handleiding voor het programmeren]: ../event-hubs/event-hubs-programming-guide.md
[Tijdelijke foutenstructuuranalyse afhandeling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Toepassingen met Service Bus met meerdere niveaus maken]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
