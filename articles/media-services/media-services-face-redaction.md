<properties
    pageTitle="Computer zien redactie met Azure media analytics | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe Redigeren vlakken met Azure media analytics."
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
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Nominale redactie met Azure media-analyses

##<a name="overview"></a>Overzicht

**Azure Media Redactor** is een [Azure Media Analytics](media-services-analytics-overview.md) Mediaprocessor (MP) die scalable nominale redactie in de cloud biedt. Nominale redactie kunt u uw video om te kunnen vervagen vlakken van geselecteerde personen wijzigen. U wilt de service van de redactie nominale op openbare veiligheid en nieuws media-scenario's gebruiken. Kunnen een paar minuten van opnamen met meerdere vlakken uur handmatig Redigeren duren, maar met deze service vereist het nominale redactie proces slechts een paar eenvoudige stappen. Zie voor meer informatie [in dit](https://azure.microsoft.com/blog/azure-media-redactor/) blog.

In dit onderwerp krijgt meer informatie over **Azure Media Redactor** en ziet u hoe u dit product gebruiken met Media Services SDK voor .NET.

De **Azure Media Redactor** MP is momenteel in de proefversie.

## <a name="face-redaction-modes"></a>Nominale redactie modi

Analyseert redactie werkt door vlakken opsporen in elk frame van video en bijhouden van het nominale object die beide voorwaarts en achterwaarts in tijd, zodat dezelfde persoon kunt worden vervaagd vanuit andere perspectieven ook. Het proces geautomatiseerde redactie is zeer complex en wordt niet altijd opzetten 100% van de gewenste uitvoer, daarom Media Analytics beschikt u over een aantal manieren voor het wijzigen van de uiteindelijke uitvoer.

Naast een volledig automatisch ingeschakelde modus is een werkstroom met twee keer waarmee de selectie/de / selection gevonden vlakken via een lijst van id's. Daarnaast zodat willekeurige per frame aanpassingen het Beheerderspaneel gebruikt een bestand met metagegevens in de indeling van JSON. Deze werkstroom opgesplitst **analyseren** en **Redact** modi. U kunt de twee modi in één stap die wordt uitgevoerd als beide taken in één taak; combineren Deze modus is de **gecombineerde**genoemd.

###<a name="combined-mode"></a>Gecombineerde modus

Hiermee wordt een geredigeerde mp4 automatisch produceren zonder een handmatige invoer.

Fase|Bestandsnaam|Notities
---|---|---
Invoer activa|foo.bar|Video in WMV, MOV of MP4-indeling
Invoer zoekconfiguratie|De configuratie van vooraf ingestelde|{'version':'1.0 ', 'opties': {"modus": "gecombineerd"}}
Uitvoer van activa|foo_redacted.mp4|Video met vervaging toegepast

####<a name="input-example"></a>Invoer voorbeeld:

[Bekijk deze video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Voorbeeld van de uitvoer:

[Bekijk deze video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Modus analyseren

De fase **analyseren** van de werkstroom met twee keer een video-invoer wordt ingevoegd en genereert een JSON-bestand van nominale locaties en jpg-afbeeldingen van elk gevonden vlak.

Fase|Bestandsnaam|Notities
---|---|----
Invoer activa|foo.bar|Video in WMV, MPV of MP4-indeling
Invoer zoekconfiguratie|De configuratie van vooraf ingestelde|{'version':'1.0 ', 'opties': {'modus': "analyseren"}}
Uitvoer van activa|foo_annotations.JSON|De gegevens van de aantekening nominale locaties in JSON-indeling. Dit kan worden bewerkt door de gebruiker voor het wijzigen van de wazigheid begrenzingsvak vakken. Zie onderstaande voorbeeld.
Uitvoer van activa|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Bijgesneden jpg van elk gedetecteerd gezicht, waar het nummer de labelId van het gezicht geeft

####<a name="output-example"></a>Voorbeeld van de uitvoer:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… de waarde afgekapt


###<a name="redact-mode"></a>Modus Redigeren

De tweede stap van de werkstroom wordt een groot aantal ingangen die moet worden gecombineerd in een enkel actief.

Dit geldt ook voor een lijst met id's toe aan vervagen, de oorspronkelijke video en de aantekeningen JSON. Deze modus wordt de aantekeningen gebruikt om toe te passen op de video invoer vervagen.

De uitvoer van de fase analyseren bevat geen de oorspronkelijke video. De video moet worden geüpload naar de invoer van activa voor de taak van de modus Redact en als het primaire bestand hebt geselecteerd.

Fase|Bestandsnaam|Notities
---|---|---
Invoer activa|foo.bar|Video in WMV, MPV of MP4-indeling. Hetzelfde video zoals in stap 1.
Invoer activa|foo_annotations.JSON|bestand met aantekeningen metagegevens van fase één, met optionele wijzigingen.
Invoer activa|foo_IDList.txt (optioneel)|Optionele nieuwe regel gescheiden lijst met nominale id's toe aan redigeren. Als u geen vervaging dit alle vlakken.
Invoer zoekconfiguratie|De configuratie van vooraf ingestelde|{'version':'1.0 ', 'opties': {'modus': 'Redigeren'}}
Uitvoer van activa|foo_redacted.mp4|Video met vervaging toegepast op basis van aantekeningen

####<a name="example-output"></a>Voorbeeld-uitvoer

Dit is de uitvoer van een IDList met één-ID die zijn geselecteerd.

[Bekijk deze video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Beschrijvingen van kenmerken

Het Beheerderspaneel redactie vindt u een hoge precisie nominale locatie detectie en bijhouden die kan maximaal 64 menselijke vlakken detecteren in een videoframe. Voorzijde vlakken de beste resultaten geven, terwijl de zijkanten en kleine vlakken (kleiner dan of gelijk is aan 24 x 24 pixels) uitdaging zijn.

De gevonden en bijgehouden vlakken worden geretourneerd met coördinaten waarin wordt aangegeven dat de locatie van vlakken, evenals een nominale-id-nummer dat aangeeft dat afzonderlijke volgen. Nominale-id-nummers zijn vatbaar voor opnieuw omstandigheden wanneer de voorzijde nominale is verbroken of overlapt in het frame, klikt u met enkele personen meerdere id's aan de toegewezen resultaat.

Zie [nominale detecteren en emoties met Azure Media Analytics](media-services-face-and-emotion-detection.md) -onderwerp voor een uitgebreide beschrijving voor de kenmerken.

## <a name="sample-code"></a>Voorbeeld van een code

De volgende programma ziet u hoe u:

1. Activa maken en een mediabestand uploaden naar de activa.
1. Een taak maken met een nominale redactie taak op basis van een configuratiebestand met de volgende json vooraf ingestelde. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. Download de uitvoer JSON-bestanden. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
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
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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


##<a name="next-step"></a>Volgende stap

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Verwante koppelingen

[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
