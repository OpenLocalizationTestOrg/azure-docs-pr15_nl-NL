<properties 
    pageTitle="Azure wachtrij opslag gebruiken om te controleren van Media Services taak meldingen met .NET | Microsoft Azure" 
    description="Leer hoe u Azure wachtrij opslagruimte gebruiken om de Media Services taak meldingen te houden. In het voorbeeld is geschreven in C# en gebruikt de Media Services SDK voor .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016"   
    ms.author="juliako"/>

# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>Azure wachtrij opslagruimte gebruiken om de Media Services taak meldingen met .NET te houden

Wanneer u taken uitvoert, moet u vaak een manier om de taakvoortgang van bijhouden. U kunt de voortgang controleren door Azure wachtrij opslag gebruikt om de Media Services taak meldingen (zoals beschreven in dit onderwerp) te houden of een StateChanged gebeurtenis-handler definiëren (zoals beschreven in [Dit](media-services-check-job-progress.md) onderwerp.  

## <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications"></a>Azure wachtrij opslagruimte gebruiken om de Media Services taak meldingen te houden

Microsoft Azure Media Services heeft de mogelijkheid om aan te geven meldingsberichten voor de [opslag van Azure wachtrij](../storage-dotnet-how-to-use-queues.md#what-is) tijdens het verwerken van media-taken. Dit onderwerp wordt uitgelegd hoe u deze meldingsberichten uit de wachtrij opslag.

Berichten in wachtrij opslag zijn toegankelijk vanaf een willekeurige plaats in de wereld. De architectuur messaging Azure-wachtrij is betrouwbaar en ten zeerste scalable. Wachtrij opslag polling wordt aangeraden via andere methoden gebruiken. 

Een gebruikelijk voor het beluisteren van meldingen van Media Services is als u een inhoudsbeheersysteem dat moet enkele extra taken uitvoeren na een codering taak ontwikkelt is voltooid (bijvoorbeeld trigger de volgende stap in een werkstroom of publiceren van inhoud). 

###<a name="considerations"></a>Overwegingen

Houd rekening met het volgende bij het ontwikkelen van Media Services-toepassingen die gebruikmaken van Azure opslag wachtrij.

- De wachtrijen-service biedt geen garanties van first in first out (FIFO) hebt besteld bezorging. Zie [Azure wachtrijen en Azure Service Bus wachtrijen vergeleken en Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx)voor meer informatie.
- Azure opslag wachtrijen is niet een service push; u moet een poll uitvoeren onder de wachtrij. 
- U kunt een willekeurig aantal wachtrijen hebben. Zie [Wachtrij Service REST API](https://msdn.microsoft.com/library/azure/dd179363.aspx)voor meer informatie.
- Azure opslag wachtrijen heeft enkele beperkingen en de details die worden beschreven in het volgende artikel: [Azure wachtrijen en Azure Service Bus wachtrijen vergeleken en Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).

###<a name="code-example"></a>Codevoorbeeld

Het voorbeeld in deze sectie, gebeurt het volgende:

1. Hiermee definieert u de **EncodingJobMessage** klasse die is toegewezen aan de berichtindeling melding. De code deserializes die zijn ontvangen uit de wachtrij naar objecten van het type **EncodingJobMessage** .
1. De accountgegevens Media Services en opslag laadt van het bestand app.config. Deze gegevens gebruikt om de objecten **CloudMediaContext** en **CloudQueue** te maken.
1. Hiermee maakt u de wachtrij die ontvangt meldingen over de codering taak.
1. Hiermee maakt u het eindpunt van de melding die is toegewezen aan de wachtrij.
1. Het eindpunt van de melding is gekoppeld aan de taak in en verstuurt de codering taak. U kunt meerdere melding eindpunten aan een taak gekoppeld hebben.
1. In dit voorbeeld bent wordt alleen geïnteresseerd in definitief lidstaten van de taak verwerkt, zodat we **NotificationJobState.FinalStatesOnly** aan de methode **AddNew doorgeven** . 
        
        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
1. Als u NotificationJobState.All doorgeeft, u kunt verwachten om alle meldingen van status wijzigen: in de wachtrij -> gepland-verwerking > -> gereed. Echter, zoals hierboven is, de service Azure opslag wachtrijen garandeert geordende bezorging. U kunt de eigenschap tijdstempel (gedefinieerd op het type EncodingJobMessage in het onderstaande voorbeeld) volgorde berichten. Het is mogelijk dat u dubbele meldingsberichten krijgt. Gebruik de ETag-eigenschap (gedefinieerd op het type EncodingJobMessage) om te controleren op dubbele waarden. Het is ook mogelijk dat sommige staat wijzigingsmeldingen worden overgeslagen. 
1. Wacht voor de taak om de status voltooid door te schakelen van de wachtrij elke 10 seconden. Hiermee verwijdert u berichten nadat ze zijn verwerkt.
1. Hiermee verwijdert u de wachtrij en het eindpunt van de melding.

>[AZURE.NOTE]De aanbevolen wijze om de status van een taak te houden, is door te luisteren naar meldingsberichten, zoals wordt weergegeven in het volgende voorbeeld.
>
>U kunt ook over de status van een taak controleren met behulp van de eigenschap **IJob.State** .  Een melding over de voltooiing van een taak mogelijk komen terecht voordat de status op **IJob** is ingesteld op **voltooid**. De eigenschap **IJob.State** betrekking op nauwkeurige met een kleine vertraging.

    
    using System;
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Web;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Runtime.Serialization.Json;
    
    namespace JobNotification
    {
        public class EncodingJobMessage
        {
            // MessageVersion is used for version control. 
            public String MessageVersion { get; set; }
        
            // Type of the event. Valid values are 
            // JobStateChange and NotificationEndpointRegistration.
            public String EventType { get; set; }
        
            // ETag is used to help the customer detect if 
            // the message is a duplicate of another message previously sent.
            public String ETag { get; set; }
        
            // Time of occurrence of the event.
            public String TimeStamp { get; set; }
    
            // Collection of values specific to the event.
    
            // For the JobStateChange event the values are:
            //     JobId - Id of the Job that triggered the notification.
            //     NewState- The new state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
            //     OldState- The old state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
    
            // For the NotificationEndpointRegistration event the values are:
            //     NotificationEndpointId- Id of the NotificationEndpoint 
            //          that triggered the notification.
            //     State- The state of the Endpoint. 
            //          Valid values are: Registered and Unregistered.
    
            public IDictionary<string, object> Properties { get; set; }
        }
    
        class Program
        {
            private static CloudMediaContext _context = null;
            private static CloudQueue _queue = null;
            private static INotificationEndPoint _notificationEndPoint = null;
    
            private static readonly string _singleInputMp4Path =
                Path.GetFullPath(@"C:\supportFiles\multifile\BigBuckBunny.mp4");
    
            static void Main(string[] args)
            {
                // Get the values from app.config file.
                string mediaServicesAccountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
                string mediaServicesAccountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
                string storageConnectionString = ConfigurationManager.AppSettings["StorageConnectionString"];
    
    
                string endPointAddress = Guid.NewGuid().ToString();
    
                // Create the context. 
                _context = new CloudMediaContext(mediaServicesAccountName, mediaServicesAccountKey);
    
                // Create the queue that will be receiving the notification messages.
                _queue = CreateQueue(storageConnectionString, endPointAddress);
    
                // Create the notification point that is mapped to the queue.
                _notificationEndPoint = 
                        _context.NotificationEndPoints.Create(
                        Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);
    
    
                if (_notificationEndPoint != null)
                {
                    IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                    WaitForJobToReachedFinishedState(job.Id);
                }
    
                // Clean up.
                _queue.Delete();      
                _notificationEndPoint.Delete();
           }
    
    
            static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
            {
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);
    
                // Create the queue client
                CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
                // Retrieve a reference to a queue
                CloudQueue queue = queueClient.GetQueueReference(endPointAddress);
    
                // Create the queue if it doesn't already exist
                queue.CreateIfNotExists();
    
                return queue;
            }
     
    
            public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");
    
                //Create an encrypted asset and upload the mp4. 
                IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted, 
                    inputMediaFilePath);
    
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
                // Create a task with the conversion details, using a configuration file. 
                ITask task = job.Tasks.AddNew("My encoding Task",
                    processor,
                    "H264 Multiple Bitrate 720p",
                    Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Add a notification point to the job. You can add multiple notification points.  
                job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, 
                    _notificationEndPoint);
    
                job.Submit();
    
                return job;
            }
    
            public static void WaitForJobToReachedFinishedState(string jobId)
            {
                int expectedState = (int)JobState.Finished;
                int timeOutInSeconds = 600;
    
                bool jobReachedExpectedState = false;
                DateTime startTime = DateTime.Now;
                int jobState = -1;
    
                while (!jobReachedExpectedState)
                {
                    // Specify how often you want to get messages from the queue.
                    Thread.Sleep(TimeSpan.FromSeconds(10));
    
                    foreach (var message in _queue.GetMessages(10))
                    {
                        using (Stream stream = new MemoryStream(message.AsBytes))
                        {
                            DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                            settings.UseSimpleDictionaryFormat = true;
                            DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                            EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);
    
                            Console.WriteLine();
    
                            // Display the message information.
                            Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                            Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                            Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                            Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                            foreach (var property in encodingJobMsg.Properties)
                            {
                                Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                            }
    
                            // We are only interested in messages 
                            // where EventType is "JobStateChange".
                            if (encodingJobMsg.EventType == "JobStateChange")
                            {
                                string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                                if (JobId == jobId)
                                {
                                    string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                    string newJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "NewState").FirstOrDefault().Value;
    
                                    JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                    JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);
    
                                    if (newJobState == (JobState)expectedState)
                                    {
                                        Console.WriteLine("job with Id: {0} reached expected state: {1}", 
                                            jobId, newJobState);
                                        jobReachedExpectedState = true;
                                        break;
                                    }
                                }
                            }
                        }
                        // Delete the message after we've read it.
                        _queue.DeleteMessage(message);
                    }
    
                    // Wait until timeout
                    TimeSpan timeDiff = DateTime.Now - startTime;
                    bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                    if (timedOut)
                    {
                        Console.WriteLine(@"Timeout for checking job notification messages, 
                                            latest found state ='{0}', wait time = {1} secs",
                            jobState,
                            timeDiff.TotalSeconds);
    
                        throw new TimeoutException();
                    }
                }
            }
       
            static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
            {
                var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(), 
                    assetCreationOptions);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading of {0}", assetFile.Name);
    
                return asset;
            }
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }

Met het bovenstaande voorbeeld de volgende uitvoer geproduceerd. U waarden vindt, varieert.
    
    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4
    
    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35
    
    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected 
    State: Finished
    

## <a name="next-step"></a>Volgende stap

Media-Services leerpaden controleren

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
