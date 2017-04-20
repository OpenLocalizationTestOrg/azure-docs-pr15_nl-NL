<properties 
    pageTitle="Geavanceerde coderen met Media Encoder Premium werkstroom | Microsoft Azure" 
    description="Leer hoe u coderen met Media Encoder Premium werkstroom. Voorbeelden van code worden geschreven in C# en gebruiken van de Media Services SDK voor .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>

#<a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Geavanceerde coderen met Media Encoder Premium werkstroom

>[AZURE.NOTE] Media Encoder Premium media werkstroomverwerking besproken in dit onderwerp is niet beschikbaar in China.

Voor premium encoder vragen, e-mepd op Microsoft.com.

##<a name="overview"></a>Overzicht

Microsoft Azure Media Services introduceert de **Media Encoder Premium** media werkstroomverwerking. Deze aanbiedingen processor vooraf coderen capaciteiten voor uw premium op aanvraag-werkstromen. 

Details **Media Encoder Premium werkstroom**voor een overzicht maken van de volgende onderwerpen: 

- [Indelingen ondersteund door de Media Encoder Premium werkstroom](media-services-premium-workflow-encoder-formats.md) – Discusses de bestandsindelingen en codecs die worden ondersteund door **Media Encoder Premium werkstroom**.

- De sectie [vergelijken encoders](media-services-encode-asset.md#compare_encoders) verschilt de codering mogelijkheden van **Media Encoder Premium werkstroom** en **Media Encoder standaard**.

In dit onderwerp wordt beschreven hoe coderen met **Media Encoder Premium werkstroom** met .NET.

Tekstcodering taken voor de **Media Encoder Premium werkstroom** moeten een configuratie apart bestand, met de naam van een werkstroom voor het bestand. Deze bestanden hebben de extensie .workflow en zijn gemaakt met de functie [Werkstroomontwerper](media-services-workflow-designer.md) .

##<a name="encode"></a>Coderen

Tekstcodering taken voor de **Media Encoder Premium werkstroom** moeten een configuratie apart bestand, met de naam van een werkstroom voor het bestand. Deze bestanden hebben de extensie .workflow en zijn gemaakt met de functie [Werkstroomontwerper](media-services-workflow-designer.md) .


U kunt de standaard ook openen Werkstroombestanden [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). De map bevat ook de beschrijving van deze bestanden.

De Werkstroombestanden moeten worden geüpload naar uw account Media Services als activa en deze activa moet worden doorgegeven aan de codering taak.

Het volgende voorbeeld laat zien hoe coderen met **Media Encoder Premium werkstroom**. 

De volgende stappen worden uitgevoerd: 
 
1. Activa maken en uploaden van een werkstroom voor het bestand. 
2. Activa maken en een bron media-bestand uploaden.
3. De "Media Encoder Premium" media werkstroomverwerking krijgen.
4. Een taak en een taak maken. 

    In de meeste gevallen, de configuratietekenreeks voor de taak leeg is (zoals in het volgende voorbeeld). Er zijn enkele geavanceerde scenario's (waarvoor u te runtime eigenschappen instellen voor dynamisch) in dat geval zou u een XML-tekenreeks aan de codering taak opgeeft. Voorbeelden van scenario's zijn: een overlay, parallelle of de opeenvolgende media wilt, subtitling maken.
5. Twee invoer elementen toevoegen aan de taak.
    
    een. 1e – de activa van de werkstroom.

    b. 2e – de video activa.
    
    **Opmerking**: de activa van de werkstroom moet worden toegevoegd aan de taak voordat u de media-activa. De configuratietekenreeks voor deze taak moet niet leeg zijn. 

6. De codering taak verstuurt.

Hierna volgt een volledig voorbeeld. Zie voor informatie over het instellen van Media Services .NET ontwikkelen, [Media Services ontwikkeling met .NET](media-services-dotnet-how-to-use.md).


    using System; 
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.MediaServices.Client;
    
    namespace MediaEncoderPremiumWorkflowSample
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;
    
            private static readonly string _supportFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _workflowFilePath =
                Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");
            
            private static readonly string _singleMP4InputFilePath =
                Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
    
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset); 
    
            }
    
            static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
            {
                var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
    
                var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                    AccessPermissions.Write | AccessPermissions.List);
    
                var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);
    
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading {0}", assetFile.Name);
    
                locator.Delete();
                accessPolicy.Delete();
    
                return asset;
            }
    
            static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                    processor,
                    "",
                    TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(workflow);
                task.InputAssets.Add(video); // we add one asset
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new
                        EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish. 
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                progressJobTask.Wait();
    
                // If job state is Error the event handling 
                // method for job progress should log errors.  Here we check 
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    throw new Exception("\nExiting method due to job error.");
                }
    
                return job.OutputMediaAssets[0];
            }
    
            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);
    
                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
                        break;
                    case JobState.Canceling:
                    case JobState.Queued:
                    case JobState.Scheduled:
                    case JobState.Processing:
                        Console.WriteLine("Please wait...\n");
                        break;
                    case JobState.Canceled:
                    case JobState.Error:
                        // Cast sender as a job.
                        IJob job = (IJob)sender;
                        // Display or log error details as needed.
                        LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
            static private void LogJobStop(string jobId)
            {
                StringBuilder builder = new StringBuilder();
                IJob job = _context.Jobs.Where(j => j.Id == jobId).FirstOrDefault();
    
                builder.AppendLine("\nThe job stopped due to cancellation or an error.");
                builder.AppendLine("***************************");
                builder.AppendLine("Job ID: " + job.Id);
                builder.AppendLine("Job Name: " + job.Name);
                builder.AppendLine("Job State: " + job.State.ToString());
                builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
                builder.AppendLine("Media Services account name: " + _mediaServicesAccountName);
                // Log job errors if they exist.  
                if (job.State == JobState.Error)
                {
                    builder.Append("Error Details: \n");
                    foreach (ITask task in job.Tasks)
                    {
                        foreach (ErrorDetail detail in task.ErrorDetails)
                        {
                            builder.AppendLine("  Task Id: " + task.Id);
                            builder.AppendLine("    Error Code: " + detail.Code);
                            builder.AppendLine("    Error Message: " + detail.Message + "\n");
                        }
                    }
                }
                builder.AppendLine("***************************\n");
    
                Console.Write(builder.ToString());
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


Voor premium encoder vragen, e-mepd op Microsoft.com.

##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
