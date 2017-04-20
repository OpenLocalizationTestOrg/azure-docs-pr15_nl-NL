<properties
    pageTitle="Bewegingen met Azure Media Analytics detecteren | Microsoft Azure"
    description="De Azure Media beweging detectie Mediaprocessor (MP) kunt u secties belangrijke binnen een video anders lange en probleemloze efficiënt te identificeren."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Bewegingen met Azure Media Analytics detecteren

##<a name="overview"></a>Overzicht

De **Azure Media beweging detectie** Mediaprocessor (MP) kunt u secties belangrijke binnen een video anders lange en probleemloze efficiënt te identificeren. Beweging detectie kan worden gebruikt op statische camera-opnamen om aan te geven van gedeelten van de video waar beweging is opgetreden. Een JSON-bestand met een metagegevens met tijdstempels en het selectiegebied waarop de gebeurtenis heeft plaatsgevonden wordt gegenereerd.

Deze technologie is gericht op video-feeds voor een waardepapier, mogen beweging verdeelt in relevante gebeurtenissen en onrechte zoals schaduwen en belichtingswijzigingen. Hiermee kunt u beveiligingsmeldingen vanuit camera-feeds zonder spam met eindeloos niet relevant gebeurtenissen, kunnen worden geëxtraheerd momenten belangrijke in erg lang toezicht video's te genereren.

De **Azure Media beweging detectie** MP is momenteel in de proefversie.

In dit onderwerp krijgt meer informatie over **Azure Media beweging detectie** en ziet u hoe u dit product gebruiken met Media Services SDK voor .NET


##<a name="motion-detector-input-files"></a>Beweging detectie invoer bestanden

Videobestanden. Op dit moment de volgende indelingen worden ondersteund: MP4, MOV en WMV.

##<a name="task-configuration-preset"></a>Configuratie van de taak (standaardoptie)

Wanneer u een taak maakt met **Azure Media beweging detectie**, moet u een configuratie vooraf ingestelde. 

###<a name="parameters"></a>Parameters

U kunt de volgende parameters gebruiken:

Naam|Opties|Beschrijving|Standaard
---|---|---|---
sensitivityLevel|Tekenreeks: 'laag', 'Gemiddeld', 'hoge'|Hiermee stelt u de gevoeligheid niveau aan welke bewegingen wordt gerapporteerd. Dit aanpassen om te hoeveelheid onrechte aanpassen.|'Gemiddeld'
frameSamplingValue|Positief, geheel getal|Hiermee stelt u de frequentie aan welke algoritme wordt uitgevoerd. 1 gelijk is aan elk frame, 2 betekent elke 2e frame, enzovoort.|1
detectLightChange|Booleaanse: 'waar', 'onwaar'|Hiermee stelt u of light wijzigingen zijn opgenomen in de resultaten|'False'
mergeTimeThreshold|X'en-tijd:: Mm: ss<br/>Voorbeeld: 00:00:03|Hiermee geeft u het tijdvenster tussen beweging gebeurtenissen waar 2 gebeurtenissen worden gecombineerd en gerapporteerd als 1.|00:00:00
detectionZones|Een matrix detectie gedeelten:<br/>-Zone detectie is een matrix van 3 of meer punten<br/>-Punts is een x en y coördinaten van 0 tot 1.|Beschrijving van de lijst met veelhoek detectie zones moet worden gebruikt.<br/>Resultaten weergegeven met de zones als een ID, met de eerste afbeelding wordt 'id': 0|Eén zone waarmee het hele frame gedekt.

###<a name="json-example"></a>Voorbeeld van JSON

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Detectie-uitvoer animaties

Een taak van de detectie beweging retourneren een JSON-bestand in de uitvoer van activa waarin de beweging waarschuwingen en hun categorieën binnen de video. Het bestand bevat informatie over de tijd en duur van beweging gedetecteerd in de video.

De beweging detectie-API biedt indicatoren zodra er objecten beweging in een vaste achtergrond video (bijvoorbeeld een toezicht video). De animatie-detectie is ervaren ONWAAR alarmen, zoals belichting en schaduw wijzigingen verkleinen. Huidige beperkingen van de algoritmen zijn 's nachts vision video's, semi-transparante objecten en kleine objecten.

###<a id="output_elements"></a>Elementen van het uitvoerbestand JSON

>[AZURE.NOTE]In de nieuwste versie, de indeling van JSON uitvoer is gewijzigd en mogelijk een recente wijziging vertegenwoordigen voor sommige klanten.

De volgende tabel beschrijft de elementen van het uitvoerbestand JSON.

Element|Beschrijving
---|---
Versie|Dit heeft betrekking op de versie van de Video-API. De huidige versie is 2.
Tijdschaal|"Tikken" per seconde van de video.
Verschuiving|Het tijdverschil voor tijdstempels in "tikken". Versie 1.0 van Video-API's zijn dit altijd 0. In toekomstige scenario's die wordt ondersteund, deze waarde kan veranderen.
Framesnelheid|Frames per seconde van de video.
Breedte, hoogte|Verwijst naar de breedte en hoogte van de video in pixels.
Starten|De begin-tijdstempel in "maatstreepjes".
Duur|De lengte van de gebeurtenis in "tikken".
Interval|Het interval van elk item in de gebeurtenis, in "maatstreepjes".
Gebeurtenissen|Elke gebeurtenis-fragment bevat de beweging gedetecteerd binnen de tijdsduur van die.
Type|In de huidige versie is dit altijd '2' voor algemene beweging. In dit label geeft Video API's de flexibiliteit om te categoriseren animatiepad in toekomstige versies.
RegionID|Zoals hierboven vermeld, zijn dit altijd 0 in deze versie. De dit label kunt Video API beweging zoeken in verschillende gebieden in toekomstige versies.
Regio 's|Verwijst naar het gebied in een video waarin u geïnteresseerd beweging. <br/><br/>-"-id" staat voor het gebied regio – in deze versie is slechts één ID 0. <br/>-"type" de vorm van het gebied dat u belangrijk vindt voor beweging aangeeft. "Rechthoek" en "veelhoek" worden momenteel ondersteund.<br/> Als u "rechthoek" hebt opgegeven, heeft het gebied dimensies in X, Y, breedte en hoogte. De X en Y-coördinaten vertegenwoordigen het bovenste linkerpagina XY-coördinaten van het gebied in een genormaliseerde schaal van 0,0 en 1,0. De breedte en hoogte vertegenwoordigen de grootte van het gebied in een genormaliseerde schaal van 0,0 en 1,0. In de huidige versie, zijn X, Y, breedte en hoogte altijd vast bij 0, 0 en 1, 1. <br/>Als u 'veelhoek' hebt opgegeven, heeft het gebied dimensies in punten. <br/>
Fragmenten|De metagegevens is gesegmenteerde omhoog in verschillende segmenten fragmenten genoemd. Elke fragment bevat een begindatum, duur, interval getal en gebeurtenis(sen). Een fragment met geen gebeurtenissen betekent dat er geen beweging is gevonden tijdens deze begintijd en duur.
Vierkante haken]|Elke haakjes staat één interval in de gebeurtenis. Lege vierkante haken voor dat interval betekent dat er geen beweging is gedetecteerd.
locaties|Deze nieuwe vermelding onder gebeurtenissen bevat de locatie waar de beweging heeft plaatsgevonden. Dit is specifieker zijn dan de zones detectie.

Hier volgt een voorbeeld van de uitvoer JSON

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Beperkingen

- De ondersteunde invoer video-indelingen zijn MP4, MOV en WMV.
- Beweging detectie is geoptimaliseerd voor vaste achtergrond video's. De algoritme van de ligt de nadruk op onwaar alarmen, zoals wijzigingen in belichting en schaduwen verkleinen.
- Sommige beweging kan niet wordt herkend vanwege technische uitdagingen; bijvoorbeeld 's nachts vision video's, semi-transparante objecten en kleine objecten.


## <a name="sample-code"></a>Voorbeeld van een code

De volgende programma ziet u hoe u:

1. Activa maken en een mediabestand uploaden naar de activa.
1. Hiermee maakt u een taak een video beweging detectie taak op basis van een configuratiebestand met de volgende json vooraf ingestelde. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. De uitvoer JSON-bestanden worden gedownload. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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

##<a name="related-links"></a>Verwante koppelingen
[Azure Media Services beweging detectie-blog](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
