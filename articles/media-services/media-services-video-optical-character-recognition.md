<properties
    pageTitle="Gebruik van Azure Media Analytics tekstuele inhoud in video-bestanden converteren naar digitale tekst | Microsoft Azure"
    description="Azure Media Analytics OCR (optical character recognition) kunt u tekstinhoud in videobestanden converteren naar bewerkbare, doorzoekbare digitale tekst.  Hiermee kunt u de extractie van duidelijke metagegevens van de video signaal van uw media automatiseren."
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
    ms.date="09/26/2016"   
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Azure Media Analytics tekstuele inhoud in video-bestanden converteren naar digitale tekst gebruiken 

##<a name="overview"></a>Overzicht

Als u wilt extraheren tekstuele inhoud uit de videobestanden en zorgt u voor een bewerkbare, doorzoekbare digitale tekst, moet u Azure Media Analytics OCR (optical character recognition). Deze Azure Media Processor tekstinhoud u videobestanden gedetecteerd en wordt gegenereerd tekstbestanden voor gebruik. OCR kunt u de extractie van duidelijke metagegevens van de video signaal van uw media automatiseren.

Wanneer gebruikt in combinatie met een zoekmachine, kunt u eenvoudig uw media indexering van tekst en de zichtbaarheid van uw inhoud te verbeteren. Dit is bijzonder nuttig in ten zeerste tekstuele video, zoals een video-opname of een schermopname van een diavoorstelling. De Azure OCR Media Processor is geoptimaliseerd voor digitale tekst.

De media-processor **Azure Media OCR** is momenteel in de proefversie.

In dit onderwerp krijgt meer informatie over **Azure Media OCR** en ziet u hoe u dit product gebruiken met Media Services SDK voor .NET. Zie voor meer informatie en voorbeelden [in dit blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

##<a name="ocr-input-files"></a>Invoer OCR-bestanden

Videobestanden. Op dit moment de volgende indelingen worden ondersteund: MP4, MOV en WMV.

##<a name="task-configuration"></a>Configuratie van de taak 

Configuratie van de taak (standaard). Wanneer u een taak maakt met **Azure Media OCR**, moet u een vooraf ingestelde met JSON of XML-configuratie. 

###<a name="attribute-descriptions"></a>Beschrijvingen van kenmerken

Kenmerknaam|Beschrijving
---|---
Taal|(optioneel) een beschrijving van de taal van tekst voor waarnaar moet worden gezocht. Een van de volgende handelingen uit: automatische detectie (standaard), Arabisch, ChineseSimplified, ChineseTraditional, Tsjechische Deens, Nederlands, Engels, Fins, Frans, Duits, Grieks, Hongaars, Italiaans, Japans, Koreaans, Noors, Pools, Portugees, Roemeens, Russisch, SerbianCyrillic, SerbianLatin, Slowaaks, Spaans, Zweeds, Turks.
TextOrientation|(optioneel) een beschrijving van de stand van tekst voor waarnaar moet worden gezocht.  'Links' betekent dat de bovenkant van alle letters wordt doorverwezen naar links.  Standaardtekst (zoals die die u kunt vinden in een boek) kan worden aangeroepen 'Van' in deze video.  Een van de volgende handelingen uit: automatische detectie (standaard), afgerond, rechts, omlaag, naar links.
TimeInterval|(optioneel) is, wordt het tarief weer dat steekproeven beschreven.  Standaard wordt elke tweede 1/2.<br/>Indeling van JSON –: mm: ss. SSS (standaard 00:00:00.500)<br/>XML-indeling – W3C XSD duur primitief (standaard PT0.5)
DetectRegions|(optioneel) Een matrix van DetectRegion objecten opgeven regio's in het videoframe waarin tekst wordt gedetecteerd.<br/>Een object DetectRegion bestaat de volgende vier waarden:<br/>Links – pixels vanaf de linkermarge<br/>Top-pixels uit de bovenmarge<br/>Breedte-breedte van het gebied in pixels<br/>Hoogte-hoogte van het gebied in pixels

####<a name="json-preset-example"></a>Vooraf ingestelde JSON-voorbeeld
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>XML-vooraf ingestelde voorbeeld

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>OCR uitvoerbestanden

De uitvoer van de processor van de media OCR is een JSON-bestand.

###<a name="elements-of-the-output-json-file"></a>Elementen van het uitvoerbestand JSON

De uitvoer Video OCR biedt tijd gesegmenteerd gegevens aan de tekens die zijn gevonden in uw video.  U kunt kenmerken zoals taal of de afdrukstand rovider in op exact de gewenste bij het analyseren van woorden. 

De uitvoer bevat de volgende kenmerken:

Element|Beschrijving
---|---
Tijdschaal|"maatstreepjes" per seconde van de video
Verschuiving|tijd voor tijdstempels offset. Versie 1.0 van Video-API's zijn dit altijd 0.
Framesnelheid|Frames per seconde van de video
Breedte|breedte van de video in pixels
hoogte|hoogte van de video in pixels
Fragmenten|matrix met op tijd gebaseerde delen van video waarin de metagegevens is gesegmenteerde
starten|Begintijd van een fragment in "tikken"
duur|lengte van een fragment in "tikken"
interval|interval van elke gebeurtenis in het opgegeven fragment
gebeurtenissen|matrix met regio 's
regio|object dat staat voor gedetecteerd woorden of woordgroepen
taal|taal van de tekst in een gebied gedetecteerd
afdrukstand|afdrukstand van de tekst in een gebied gedetecteerd
lijnen|matrix van regels met tekst in een gebied gedetecteerd
tekst|de werkelijke tekst

###<a name="json-output-example"></a>Voorbeeld van JSON-uitvoer

Het volgende voorbeeld van de uitvoer bevat algemene informatie video en verschillende video fragmenten. In elke video fragment, bevat elke regio is gevonden door OCR MP met de taal en de bijbehorende tekstrichting. Het gebied bevat ook elke regel van het woord in dit gebied met de tekst van de regel, de positie van de regel en elke informatie voor word (word-inhoud, positie en betrouwbaarheid) in deze regel. Hier volgt een voorbeeld en ik heb opgeslagen sommige inline opmerkingen.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Voorbeeld van een code

De volgende programma ziet u hoe u:

1. Activa maken en een mediabestand uploaden naar de activa.
1. Hiermee maakt u een taak een OCR configuratie/vooraf ingestelde-bestand.
1. De uitvoer JSON-bestanden worden gedownload. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
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

[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)
