<properties
    pageTitle="Media-bestanden met Azure Media indexering 2 Preview indexeren | Microsoft Azure"
    description="Azure Media indexering kunt u inhoud van uw mediabestanden doorzoekbare maken en genereert een transcriptie van de volledige tekst voor gesloten ondertiteling en trefwoorden. Dit onderwerp wordt uitgelegd hoe u Media indexering 2 Preview gebruikt."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016" 
    ms.author="adsolank;juliako;"/>


# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Media-bestanden met Azure Media indexering 2 Preview indexeren

##<a name="overview"></a>Overzicht

De **Azure Media indexering 2 Preview** Mediaprocessor (MP) kunt u media-bestanden en inhoud doorzoekbaar maken, evenals gesloten closed captioning nummers genereren. Vergeleken met de vorige versie van [Azure Media indexering](media-services-index-content.md), **Azure Media indexering 2 Preview** uitvoert sneller indexeren en bredere taal support biedt. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees, Portugees en Arabisch.

De **Azure Media indexering 2 Preview** MP is momenteel in de proefversie.

In dit onderwerp ziet hoe u indexing taken maken met **Azure Media indexering 2 Preview**.

>[AZURE.NOTE]De volgende punten zijn van toepassing:
>
>Indexering 2 wordt niet ondersteund in Azure China en Azure overheid.
>
>Het voorbeeld is beperkt tot ~ 10 minuten van verwerking, maar is gratis voor alle klanten.
>
>Wanneer u inhoud, zorg er dan voor dat mediabestanden met zeer wissen spraak (zonder de achtergrondmuziek, ruis, effecten of microfoon hiss) gebruiken. Voorbeelden van de juiste inhoud zijn: opgenomen vergaderingen, colleges of -presentaties. De volgende inhoud mogelijk niet geschikt voor het indexeren: films, tv-programma's, alles met gemengde audio- en geluidseffecten, opgenomen slecht inhoud met achtergrondruis (hiss).


In dit onderwerp krijgt meer informatie over **Azure Media indexering 2 Preview** en ziet u hoe u dit product gebruiken met Media Services SDK voor .NET

##<a name="input-and-output-files"></a>Invoer- en uitvoerbereik bestanden

###<a name="input-files"></a>Invoer-bestanden

Audio-of videobestanden

###<a name="output-files"></a>Uitvoerbestanden

Een taak indexing kunt ondertiteling bestanden genereren in de volgende indelingen:  

- **Sami**
- **TTML**
- **WebVTT**

Gesloten bijschrift (CC) bestanden in de volgende indelingen kunnen worden gebruikt om audio-en videobestanden toegankelijk te maken voor personen met de disability horen.

##<a name="task-configuration-preset"></a>Configuratie van de taak (standaardoptie)

Wanneer u een indexing-taak maakt met **Azure Media indexering 2 Preview**, moet u een configuratie vooraf ingestelde.

De volgende JSON instellen beschikbaar parameters

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

##<a name="supported-languages"></a>Ondersteunde talen  

Azure Media indexering 2 Preview ondersteunt spraak-naar-tekst voor de volgende talen (wanneer zoals hieronder wordt weergegeven, kunt u de taalnaam geven in de configuratie van de taak moeten gebruik 4-tekencode vierkante haken):

- Engels [EnUs]
- Spaans [EsEs]
- Chinees [ZhCn]
- Frans [FrFr]
- Duits [DeDe]
- Italiaans [ItIt]
- Portugese [PtBr]
- Arabisch (Egyptisch) [ArEg]


## <a name="sample-code"></a>Voorbeeld van een code

De volgende programma ziet u hoe u:

1. Activa maken en een mediabestand uploaden naar de activa.
1. Hiermee maakt u een taak een indexing-taak op basis van een configuratiebestand met de volgende json vooraf ingestelde.
            
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }

1. Uitvoerbestanden worden gedownload. 
    
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace IndexContent
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
        
                static void Main(string[] args)
                {
        
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
        
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
        
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
        
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
        
                    // Launch the job.
                    job.Submit();
        
                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        
                    progressJobTask.Wait();
        
                    // If job state is Error, the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                        Console.WriteLine(string.Format("Error: {0}. {1}",
                                                        error.Code,
                                                        error.Message));
                        return null;
                    }
        
                    return job.OutputMediaAssets[0];
                }
        
                static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
                {
                    IAsset asset = _context.Assets.Create(assetName, options);
        
                    var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                    assetFile.Upload(filePath);
        
                    return asset;
                }
        
                static void DownloadAsset(IAsset asset, string outputDirectory)
                {
                    foreach (IAssetFile file in asset.AssetFiles)
                    {
                        file.Download(Path.Combine(outputDirectory, file.Name));
                    }
                }
        
                static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors
                        .Where(p => p.Name == mediaProcessorName)
                        .ToList()
                        .OrderBy(p => new Version(p.Version))
                        .LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor",
                                                                   mediaProcessorName));
        
                    return processor;
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
                            // LogJobStop(job.Id);
                            break;
                        default:
                            break;
                    }
                }
            }
        }


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Verwante koppelingen

[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)