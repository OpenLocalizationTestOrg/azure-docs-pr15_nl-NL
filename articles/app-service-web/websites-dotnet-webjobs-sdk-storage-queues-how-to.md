<properties 
    pageTitle="Het gebruik van Azure wachtrij opslagruimte met de WebJobs SDK" 
    description="Informatie over het gebruiken van Azure wachtrij opslagruimte met de WebJobs SDK. Maken en verwijderen van wachtrijen; invoegen, korte weergave, ophalen en verwijderen van berichten in wachtrij plaatsen en meer." 
    services="app-service\web, storage" 
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

# <a name="how-to-use-azure-queue-storage-with-the-webjobs-sdk"></a>Het gebruik van Azure wachtrij opslagruimte met de WebJobs SDK

## <a name="overview"></a>Overzicht

Deze handleiding bevat voorbeelden C#-code die wordt aangegeven hoe de versie van Azure WebJobs SDK 1.x met de Azure wachtrij storage-service.

De handleiding wordt ervan uitgegaan dat u weet [hoe u een project WebJob in Visual Studio met verbindingstekenreeksen die naar uw account opslag verwijzen maakt](websites-dotnet-webjobs-sdk-get-started.md#configure-storage) of [meerdere opslag-accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

De codefragmenten de meeste functies, niet de code die wordt gemaakt alleen weergeven de `JobHost` object zoals in dit voorbeeld:

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }
        
De handleiding bevat de volgende onderwerpen:

-   [Hoe u een functie te activeren wanneer een bericht wachtrij wordt ontvangen](#trigger)
    - Berichten van tekenreeks wachtrij plaatsen
    - Berichten van POCO wachtrij plaatsen
    - Asynchrone functies
    - Het kenmerk QueueTrigger met werkt typen
    - Peiling algoritme
    - Meerdere exemplaren
    - Parallelle uitvoering
    - Wachtrij of wachtrij bericht metagegevens
    - Correcte afsluiten
-   [Het maken van een bericht wachtrij tijdens het verwerken van een bericht wachtrij](#createqueue)
    - Berichten van tekenreeks wachtrij plaatsen
    - Berichten van POCO wachtrij plaatsen
    - Meerdere berichten maken of in asynchrone functies
    - Het kenmerk wachtrij met werkt typen
    - WebJobs SDK kenmerken in de hoofdtekst van een functie gebruiken
-   [Hoe u kunt lezen en schrijven BLOB's tijdens het verwerken van een bericht wachtrij](#blobs)
    - Berichten van tekenreeks wachtrij plaatsen
    - Berichten van POCO wachtrij plaatsen
    - Het kenmerk Blob met werkt typen
-   [Hoe u omgaat met poison berichten](#poison)
    - Verwerken van automatische verontreinigde berichten
    - Verwerken van handmatige verontreinigde berichten
-   [Hoe u de configuratie-opties instellen](#config)
    - Set SDK verbindingstekenreeksen in code
    - QueueTrigger-instellingen configureren
    - Waarden instellen voor WebJobs SDK constructor parameters in code
-   [Hoe u handmatig een functie activeren](#manual)
-   [Logboeken schrijven](#logs) 
-   [Hoe u fouten en time-out configureren](#errors)
-   [Volgende stappen](#nextsteps)

## <a id="trigger"></a>Hoe u een functie te activeren wanneer een bericht wachtrij wordt ontvangen

Gebruikt u schrijft een functie die de WebJobs SDK belt wanneer een wachtrij bericht is ontvangen door de `QueueTrigger` kenmerk. De attribuutconstructor kent een tekenreeksparameter waarmee de naam van de wachtrij worden gecontroleerd. U kunt ook de [naam van de wachtrij dynamisch instellen](#config).

### <a name="string-queue-messages"></a>Berichten van tekenreeks wachtrij plaatsen

In het volgende voorbeeld bevat de wachtrij een Tekenreeksbericht, de dus `QueueTrigger` wordt toegepast op de tekenreeksparameter van een met de naam `logMessage` waarin de inhoud van het bericht wachtrij bevat. De functie [wordt een logboek aan het Dashboard](#logs).
 

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Naast `string`, de parameter mogelijk een byte-matrix een `CloudQueueMessage` object of een POCO die u definieert.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(normaal oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij

In het volgende voorbeeld wordt het bericht wachtrij JSON voor bevat een `BlobInformation` object waaronder een `BlobName` eigenschap. De SDK deserializes automatisch het object.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

De SDK gebruikt de [Newtonsoft.Json NuGet pakket](http://www.nuget.org/packages/Newtonsoft.Json) serialiseren en terugconverteren van berichten. Als u berichten in wachtrij plaatsen in een programma dat niet wordt gebruikt de WebJobs SDK maakt, kunt u de code zoals in het volgende voorbeeld om te maken van een POCO wachtrij-bericht dat de SDK kan worden geparseerd schrijven. 

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Asynchrone functies

De volgende asynchrone functie [schrijft een logboek aan het Dashboard](#logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Asynchrone functies duurt een [annulering token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), zoals wordt weergegeven in het volgende voorbeeld dat een blob gekopieerd. (Voor een uitleg van de `queueTrigger` tijdelijke aanduiding, raadpleegt u de sectie [BLOB's](#blobs) .)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName, 
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <a id="qtattributetypes"></a>Het kenmerk QueueTrigger met werkt typen

U kunt `QueueTrigger` met de volgende typen:

* `string`
* Een POCO type serienummer als JSON
* `byte[]`
* `CloudQueueMessage`

### <a id="polling"></a>Peiling algoritme

De SDK implementeert een willekeurig exponentiële back-uitschakelen algoritme om te verkleinen van het effect van idle-wachtrij polling op opslag transactiekosten.  Wanneer een bericht wordt gevonden, de SDK Wacht twee seconden ingedrukt en klikt u vervolgens Hiermee wordt gecontroleerd op een ander bericht; Wanneer het bericht niet is gevonden wacht ongeveer vier seconden voordat u probeert het opnieuw. Na het volgende mislukte pogingen om een bericht wachtrij, blijft de wachttijd om uit te breiden tot de maximale wachttijd, namelijk een minuut worden bereikt. [De maximale wachttijd kan worden geconfigureerd](#config).

### <a id="instances"></a>Meerdere exemplaren

Als uw web-app wordt uitgevoerd op meerdere exemplaren, een doorlopend WebJob wordt uitgevoerd op elke computer en elke computer wordt wachten triggers en probeert uit te voeren functies. De WebJobs SDK wachtrij trigger automatisch wordt voorkomen dat een functie verwerken van een bericht wachtrij meerdere keren; functies hebben geen worden geschreven moeten idempotency is ingeschakeld. Als u wilt dat om ervoor te zorgen dat slechts één exemplaar van een functie wordt uitgevoerd, zelfs wanneer er meerdere exemplaren van de host WebApp, kunt u de `Singleton` kenmerk. 

### <a id="parallel"></a>Parallelle uitvoering

Als er meerdere functies luisteren op verschillende wachtrijen, wordt de SDK aangeroepen ze parallel wanneer berichten worden tegelijk ontvangen. 

Dit geldt ook als meerdere berichten worden ontvangen voor één wachtrij. Standaard de SDK krijgt een reeks 16 berichten tegelijk en voert de functie die ze parallel verwerkt. [De batchgrootte kan worden geconfigureerd](#config). Wanneer het nummer dat wordt verwerkt, wordt onmiddellijk omlaag helft van de batchgrootte, wordt de SDK ontvangt van een andere batch en start de verwerking van deze berichten. Het maximum aantal gelijktijdige berichten wordt verwerkt per functie dus één anderhalf maal de batchgrootte. Deze beperking geldt afzonderlijk voor elke functie met een `QueueTrigger` kenmerk. 

Als u niet parallelle uitvoering voor berichten ontvangen op één wachtrij wilt, kunt u de batchgrootte ingesteld op 1. Zie ook **meer controle over de wachtrij verwerking** in [Azure WebJobs SDK 1.1.0 RTM](/blog/azure-webjobs-sdk-1-1-0-rtm/).

### <a id="queuemetadata"></a>U wachtrij of wachtrij bericht metagegevens

U kunt de berichteigenschappen van het volgende openen door parameters toe te voegen aan de methodehandtekening:

* `DateTimeOffset`expirationTime
* `DateTimeOffset`insertionTime
* `DateTimeOffset`nextVisibleTime
* `string`queueTrigger (bevat berichttekst)
* `string`ID
* `string`popReceipt
* `int`dequeueCount

Als u werken rechtstreeks met de Azure opslag API wilt, kunt u ook toevoegen een `CloudStorageAccount` parameter.

Het volgende voorbeeld schrijft al deze metagegevens naar een logboek van de toepassing INFO. In het voorbeeld bevatten zowel logMessage als queueTrigger de inhoud van het bericht wachtrij.

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

Hier volgt een voorbeeldlogboek geschreven door de voorbeeldcode:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

### <a id="graceful"></a>Correcte afsluiten

Een functie die wordt uitgevoerd in een doorlopend WebJob kunt akkoord gaan met een `CancellationToken` parameter waarmee het besturingssysteem op de hoogte stellen van de functie als de WebJob wordt worden beëindigd. U kunt deze melding gebruiken om ervoor te zorgen dat de functie niet onverwachte beëindiging op een manier die gegevens in een inconsistente status blijven.

Het volgende voorbeeld wordt getoond hoe om aanstaande WebJob beëindiging in een functie te controleren.

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

**Notitie:** Het Dashboard mogelijk niet correct weergegeven de status en de uitvoer van functies die u hebt afgesloten.
 
Zie voor meer informatie [WebJobs correcte afsluiten](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a id="createqueue"></a>Het maken van een bericht wachtrij tijdens het verwerken van een bericht wachtrij

U schrijft een functie die Hiermee maakt u een nieuw bericht in de wachtrij, gebruikt u de `Queue` kenmerk. Zoals `QueueTrigger`, geeft u in de naam van de wachtrij als een tekenreeks of u kunt [de naam van de wachtrij dynamisch instellen](#config).

### <a name="string-queue-messages"></a>Berichten van tekenreeks wachtrij plaatsen

Het volgende voorbeeld niet asynchrone Hiermee maakt u een nieuw bericht in de wachtrij in de wachtrij met de naam "outputqueue" met dezelfde inhoud als het bericht dat in de wachtrij met de naam 'inputqueue' de wachtrij. (Van asynchrone functies gebruiken `IAsyncCollector<T>` zoals verderop in dit gedeelte.)


        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }
  
### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(normaal oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij

Als wilt maken een wachtrij-bericht dat een POCO in plaats van een tekenreeks bevat, geeft u het type POCO als een uitvoerparameter voor de `Queue` attribuutconstructor.
 
        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

De SDK serialiseert automatisch het object naar JSON. Een bericht wachtrij wordt altijd gemaakt, zelfs als het object null is.

### <a name="create-multiple-messages-or-in-async-functions"></a>Meerdere berichten maken of in asynchrone functies

Als u wilt maken van meerdere berichten, Controleer het parametertype voor de wachtrij uitvoer `ICollector<T>` of `IAsyncCollector<T>`, zoals weergegeven in het volgende voorbeeld.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Elk bericht wachtrij onmiddellijk wordt gemaakt wanneer de `Add` methode wordt genoemd.

### <a name="types-that-the-queue-attribute-works-with"></a>Typen die het kenmerk wachtrij met werkt

U kunt de `Queue` kenmerk op de volgende parametertypen:

* `out string`(Hiermee maakt u wachtrij bericht als parameterwaarde geen null-is wanneer de functie eindigt)
* `out byte[]`(werkt als `string`) 
* `out CloudQueueMessage`(werkt als `string`) 
* `out POCO`(een serialiseerbaar type, maakt u een bericht met een null-object als de parameter is null wanneer de functie eindigt)
* `ICollector`
* `IAsyncCollector`
* `CloudQueue`(voor het maken van berichten handmatig rechtstreeks met de API Azure opslag)

### <a id="ibinder"></a>WebJobs SDK kenmerken in de hoofdtekst van een functie gebruiken

Als u werken in uw functie wilt vóór het gebruik van een WebJobs SDK-kenmerk, zoals `Queue`, `Blob`, of `Table`, kunt u de `IBinder` interface.

Het volgende voorbeeld wordt een bericht invoer wachtrij en Hiermee maakt u een nieuw bericht met dezelfde inhoud in een wachtrij uitvoer. De naam van de uitvoer wachtrij is ingesteld door code in de hoofdtekst van de functie.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

De `IBinder` interface kan ook worden gebruikt met de `Table` en `Blob` kenmerken.

## <a id="blobs"></a>Hoe u kunt lezen en schrijven BLOB's en tabellen tijdens het verwerken van een bericht wachtrij

De `Blob` en `Table` kenmerken kunnen u lezen en schrijven BLOB's en tabellen. In de voorbeelden in deze sectie zijn van toepassing op BLOB's. Lees [hoe u met Azure-blobopslag met de SDK WebJobs](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)voor voorbeelden van code zien hoe u het activeren van processen wanneer BLOB's worden gemaakt of bijgewerkt, en voor voorbeelden van code die lezen en schrijven van tabellen, raadpleegt u [het gebruik van Azure-tabelopslag met de SDK WebJobs](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Berichten in wachtrij plaatsen tekenreeks activeert blob bewerkingen

Voor een wachtrij-bericht met een tekenreeks, `queueTrigger` is een tijdelijke aanduiding die u kunt gebruiken in de `Blob` van kenmerk `blobPath` parameter dat de inhoud van het bericht bevat. 

Het volgende voorbeeld wordt `Stream` objecten die u wilt lezen en schrijven BLOB's. Het bericht wachtrij is de naam van een blob zich in de container textblobs. Een kopie van de blob met '-nieuwe ' toegevoegd aan de naam is gemaakt in dezelfde container. 

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName, 
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

De `Blob` kenmerk constructor heeft een `blobPath` parameter waarmee de naam van de container en blob. Zie voor meer informatie over deze tijdelijke aanduiding voor [het gebruik van Azure-blobopslag met de SDK WebJobs](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), 

Wanneer het kenmerk wordt verfraaid een `Stream` object aan, een andere constructor parameter geeft de `FileAccess` modus als lezen, schrijven of alleen-lezen/schrijven. 

Het volgende voorbeeld wordt een `CloudBlockBlob` te verwijderen van een blob object. Het bericht wachtrij is de naam van de blob.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a id="pocoblobs"></a>POCO [(normaal oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij

Voor een POCO als JSON in het bericht wachtrij opgeslagen, kunt u tijdelijke aanduidingen voor naam van de eigenschappen van het object in de `Queue` van kenmerk `blobPath` parameter. U kunt ook [wachtrij metagegevens eigenschapnamen](#queuemetadata) gebruiken als tijdelijke aanduidingen. 

Het volgende voorbeeld wordt een blob naar een nieuwe blob met de extensie verschillende gekopieerd. Het bericht wachtrij is een `BlobInformation` object met `BlobName` en `BlobNameWithoutExtension` eigenschappen. De eigenschapnamen worden gebruikt als tijdelijke aanduidingen in het pad blob voor de `Blob` kenmerken. 
 
        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

De SDK gebruikt de [Newtonsoft.Json NuGet pakket](http://www.nuget.org/packages/Newtonsoft.Json) serialiseren en terugconverteren van berichten. Als u berichten in wachtrij plaatsen in een programma dat niet wordt gebruikt de WebJobs SDK maakt, kunt u de code zoals in het volgende voorbeeld om te maken van een POCO wachtrij-bericht dat de SDK kan worden geparseerd schrijven.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Als u werken in uw functie wilt voordat u een blob binden aan een object, kunt u het kenmerk in de hoofdtekst van de functie, [zoals eerder voor het kenmerk wachtrij](#ibinder)gebruiken.

### <a id="blobattributetypes"></a>U het kenmerk Blob met kunt typen
 
De `Blob` kenmerk kan worden gebruikt met de volgende typen:

* `Stream`(lezen of schrijven, opgegeven met de parameter van de constructor FileAccess)
* `TextReader`
* `TextWriter`
* `string`(alleen)
* `out string`(schrijven; Hiermee maakt u een blob alleen als de tekenreeksparameter niet-null wanneer de functie retourneert)
* POCO (lezen)
* out POCO (schrijven; altijd een blob maakt, wordt gemaakt als null-object als POCO parameter null is wanneer de functie retourneert)
* `CloudBlobStream`(schrijven)
* `ICloudBlob`(lezen of schrijven)
* `CloudBlockBlob`(lezen of schrijven) 
* `CloudPageBlob`(lezen of schrijven) 

## <a id="poison"></a>Hoe u omgaat met poison berichten

Berichten waarvan de inhoud wordt door een functie mislukt heten *poison berichten*. Wanneer de functie mislukt, wordt het bericht wachtrij wordt niet verwijderd en uiteindelijk wordt opgehaald nogmaals veroorzaakt door de cyclus worden herhaald. De SDK kan automatisch de cyclus worden onderbroken na een beperkt aantal iteraties of u deze handmatig kunt doen.

### <a name="automatic-poison-message-handling"></a>Verwerken van automatische verontreinigde berichten

De SDK wordt een functie maximaal 5 keer aanroep aan een bericht wachtrij verwerken. Als het vijfde uitproberen mislukt, wordt het bericht wordt verplaatst naar een poison wachtrij. [Het maximum aantal pogingen kan worden geconfigureerd](#config). 

De poison wachtrij heet *{originalqueuename}*-poison. Een functie op berichten uit de poison wachtrij door ze logboekregistratie of het verzenden van een melding dat handmatige aandacht nodig is, kunt u schrijven. 

In het volgende voorbeeld de `CopyBlob` functie loopt vast wanneer een bericht wachtrij bevat de naam van een blob die niet bestaat. In dat geval wordt het bericht uit de wachtrij copyblobqueue verplaatst naar de wachtrij copyblobqueue poison. De `ProcessPoisonMessage` meldt zich dan de verontreinigd bericht.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }
        
        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

De volgende afbeelding ziet console-uitvoer van deze functies wanneer een poison-bericht wordt verwerkt.

![Console-uitvoer voor het verwerken van verontreinigde berichten](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a>Verwerken van handmatige verontreinigde berichten

U kunt het aantal keren een bericht is opgenomen omhoog voor de verwerking openen door het toevoegen van een `int` parameter met de naam `dequeueCount` aan uw functie. U kunt vervolgens de wachtrij halen tellen in de functiecode en voert uw eigen poison afhandeling van berichten wanneer het getal groter is dan een drempelwaarde, zoals wordt weergegeven in het volgende voorbeeld.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a id="config"></a>Hoe u de configuratie-opties instellen

U kunt de `JobHostConfiguration` type voor het instellen van de volgende configuratieopties:

* Stel de SDK verbindingstekenreeksen in code.
* Configureren `QueueTrigger` instellingen zoals maximum aantal in wachtrij.
* Krijgen wachtrijnamen van de configuratie.

### <a id="setconnstr"></a>Set SDK verbindingstekenreeksen in code

Instellen van de SDK verbindingstekenreeksen in code kunt u uw eigen namen van de tekenreeks verbindingen in de van configuratiebestanden of omgevingsvariabelen, zoals wordt weergegeven in het volgende voorbeeld.

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;
        
            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;
        
            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;
        
            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a id="configqueue"></a>QueueTrigger-instellingen configureren

U kunt de volgende instellingen die van toepassing zijn op de wachtrij berichtverwerking configureren:

- Het maximum aantal berichten die tegelijk kunnen worden uitgevoerd parallel zijn opgenomen (de standaardinstelling is 16).
- Het maximum aantal pogingen voordat een bericht wachtrij wordt verzonden naar een poison wachtrij (de standaardinstelling is 5).
- Het maximale aantal wachttijd voordat het polling opnieuw wanneer een wachtrij leeg is (de standaardinstelling is 1 minuut).

Het volgende voorbeeld ziet u hoe deze instellingen configureren:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a id="setnamesincode"></a>Waarden instellen voor WebJobs SDK constructor parameters in code

Soms wilt u de naam van een wachtrij, een blob naam of de container opgeven of een tabel een naam in de code in plaats van vaste-code deze. Zoals u wilt mogelijk opgeven van de naam van de wachtrij voor `QueueTrigger` in een variabele van bestand of de omgeving configuratie. 

U kunt dit doen door doorgeven in een `NameResolver` object naar de `JobHostConfiguration` type. U speciale tijdelijke aanduidingen omgeven door percentage (%) positief of negatief in WebJobs SDK kenmerk constructor parameters, opnemen en uw `NameResolver` code Hiermee geeft u de werkelijke waarden moet worden gebruikt in plaats van de tijdelijke aanduidingen.

Stel dat u wilt een wachtrij met de naam logqueuetest in de testomgeving en één benoemde logqueueprod in productie gebruiken. In plaats van de naam van een hard gecodeerde wachtrij, die u wilt opgeven van de naam van een vermelding in de `appSettings` siteverzameling die de naam van de werkelijke wachtrij kan hebben. Als de `appSettings` sleutel is logqueue en uw functie kan uitzien zoals in het volgende voorbeeld.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

Uw `NameResolver` class kan vervolgens krijgt de naam van de wachtrij uit `appSettings` zoals wordt weergegeven in het volgende voorbeeld:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

U bent de `NameResolver` klasse naar de `JobHost` object, zoals wordt weergegeven in het volgende voorbeeld.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }
 
**Notitie:** Wachtrij, tabel en blob namen worden omgezet, elke keer een functie wordt genoemd, maar alleen wanneer de toepassing wordt gestart, blob container namen worden omgezet. U kunt de naam van de container blob niet wijzigen, terwijl de taak wordt uitgevoerd. 

## <a id="manual"></a>Hoe u handmatig een functie activeren

Als u wilt activeren handmatig een functie, gebruikt u de `Call` of `CallAsync` methode op de `JobHost` object en de `NoAutomaticTrigger` kenmerk op de functie, zoals wordt weergegeven in het volgende voorbeeld. 

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }
        
            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger, 
                string value, 
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a id="logs"></a>Logboeken schrijven

Het Dashboard ziet u Logboeken op twee plaatsen: de pagina voor de WebJob en de pagina voor een specifieke WebJob aanroep. 

![Logboeken op de pagina WebJob](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Logboeken aan de functie aanroep pagina](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Uitvoer van Console methoden die u in een functie belt of in de `Main()` wordt weergegeven in de Dashboard-pagina voor de WebJob, niet in de pagina voor het oproepen van een bepaalde methode. Uitvoer van het TextWriter-object dat u van een parameter in uw methodehandtekening krijgt wordt weergegeven in de Dashboard-pagina voor het oproepen van een methode.

Console-uitvoer niet kan worden gekoppeld aan een bepaalde methodeaanroep omdat de Console één thread, is terwijl er veel functies kunnen worden uitgevoerd op hetzelfde moment. Daarom heb ik de SDK biedt de aanroep van elke functie met een eigen unieke log schrijverobject.

Als u wilt schrijven [toepassing traceringslogboeken](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), gebruikt u `Console.Out` (Hiermee maakt u Logboeken dat is gemarkeerd als INFO) en `Console.Error` (Hiermee maakt u Logboeken als fout gemarkeerd). Een alternatief is via [doelcellen of TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), waarmee uitgebreid, waarschuwing en kritiek niveaus naast Info en -fout. Toepassing traceringslogboeken worden weergegeven in de logboekbestanden web app, Azure-tabellen, of Azure BLOB's afhankelijk van hoe u uw Azure web-app configureren. Geldt ook voor alle Console-uitvoer, de meest recente 100 toepassingslogboeken ook worden weergegeven in de dashboardpagina voor de WebJob, niet naar de pagina voor de aanroep van een functie. 

Console-uitvoer wordt weergegeven in het Dashboard alleen als het programma wordt uitgevoerd in een WebJob Azure niet als het programma lokaal wordt uitgevoerd of in sommige andere omgeving.

Dashboard logboekregistratie voor hoge doorvoer scenario's uitschakelen. Standaard de SDK schrijft logboeken naar opslag en deze activiteit prestaties kan afnemen wanneer u veel berichten verwerkt. Als logboekregistratie wilt uitschakelen, stelt u de verbindingsreeks dashboard op null zoals wordt weergegeven in het volgende voorbeeld.

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

Het volgende voorbeeld ziet verschillende manieren om te schrijven Logboeken:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

In het Dashboard van de SDK WebJobs, de uitvoer van de `TextWriter` object uitziet wanneer u naar de pagina voor een bepaalde gaat functie aanroep en klik op **De wisselknop uitvoer**:

![Klik op functie aanroep koppeling](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Logboeken aan de functie aanroep pagina](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

In het Dashboard WebJobs SDK uitvoeren de meest recente 100 regels van Console weergeven van wanneer u naar de pagina voor de WebJob (niet voor de functie-aanroep gaat) en klik op **De wisselknop uitvoer**.
 
![Klik op de wisselknop uitvoer](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

In een doorlopend WebJob weergegeven toepassingslogboeken aan de in/gegevens/taken/continue /*{webjobname}*/job_log.txt in het bestandssysteem van de web-app.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

In een Azure blob de toepassing Logboeken zien er zo uit: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hallo, wereld!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hallo, wereld!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hallo, wereld!,

En in een Azure-tabel de `Console.Out` en `Console.Error` logboeken er als volgt uit:

![Info log in tabel](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Het logboek met publicatiefouten in tabel](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

Als u uw eigen logboek invoegtoepassing wilt, raadpleegt u [in dit voorbeeld](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).

## <a id="errors"></a>Hoe u fouten en time-out configureren

De WebJobs SDK bevat ook een [time-out](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) kenmerk dat u gebruiken kunt om te leiden tot een functie te annuleren als niet voltooid binnen een vastgestelde periode. En als u verhogen van een melding wilt wanneer er te veel fouten optreden binnen een opgegeven periode, kunt u de `ErrorTrigger` kenmerk. Hier volgt een [voorbeeld van ErrorTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    To = "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors to the Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

U kunt ook dynamisch uitschakelen en inschakelen van functies om te bepalen of ze kunnen worden geactiveerd, met behulp van een schakeloptie configuratie die mogelijk een app-instelling of de naam van omgevingsvariabele. Voorbeeld van code, raadpleegt u de `Disable` kenmerk in [de bibliotheek van de voorbeelden WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).

## <a id="nextsteps"></a>Volgende stappen

Deze handleiding biedt voorbeelden van de code die wordt aangegeven hoe u omgaat met veelvoorkomende scenario's voor het werken met Azure wachtrijen. Zie voor meer informatie over het gebruik van Azure WebJobs en de SDK WebJobs [Azure WebJobs aanbevolen Resources](http://go.microsoft.com/fwlink/?linkid=390226).
 
