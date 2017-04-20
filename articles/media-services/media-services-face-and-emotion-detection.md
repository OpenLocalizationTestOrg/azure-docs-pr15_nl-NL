<properties
    pageTitle="Detecteren nominale en emoties met Azure Media Analytics | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe detecteren vlakken en emoties met Azure Media Analytics."
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
    ms.author="milanga;juliako;"/>
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Nominale en emoties met Azure Media Analytics detecteren

##<a name="overview"></a>Overzicht

De **Azure Media nominale detectie** Mediaprocessor (MP) kunt u tellen, verplaatsingen bijhouden en zelfs meter deelname aan de doelgroep en reactie via analyseert expressies. Deze service bevat twee functies: 

- **Nominale detectie**

    Nominale detectie worden gevonden en wordt bijgehouden menselijke vlakken binnen een video. Meerdere vlakken kunnen worden gedetecteerd en vervolgens worden bijgehouden terwijl ze, met de metagegevens van de tijd en locatie geretourneerd door een JSON-bestand bewegen. Tijdens bijhouden wordt geprobeerd een consistente ID toekennen aan de dezelfde nominale terwijl de persoon wordt navigeren op scherm, zelfs als ze zijn ondervindt hinder van obstakels of kort het frame laat.

    >[AZURE.NOTE]Deze services geen gelaatsherkenning uitgevoerd. Een persoon die het frame verlaat of ondervindt voor verandert hinder van obstakels te lang krijgt een nieuwe ID wanneer ze terugkeren.

- **Emoties detectie**
    
    Emoties detectie is een optioneel onderdeel van het nominale detectie Media Processor die resulteert in analyse op meerdere emotionele kenmerken van de vlakken aangetroffen, inclusief gelukkig, sadness, bang, volgt uitzien en meer. 

De **Azure Media nominale detectie** MP is momenteel in de proefversie.

In dit onderwerp krijgt meer informatie over **Azure Media nominale detectie** en ziet u hoe u dit product gebruiken met Media Services SDK voor .NET.

##<a name="face-detector-input-files"></a>Computer zien detectie invoer bestanden

Videobestanden. Op dit moment de volgende indelingen worden ondersteund: MP4, MOV en WMV.

##<a name="face-detector-output-files"></a>Computer zien detectie uitvoerbestanden

De nominale detectie en bijhouden API biedt hoge precisie nominale locatie detectie en bijhouden die u maximaal 64 menselijke vlakken in een video detecteren kunt. Voorzijde vlakken de beste resultaten bieden, bij zijkanten gemaakt en de kleine vlakken (kleiner dan of gelijk is aan 24 x 24 pixels) mogelijk niet nauwkeurig als.

De gevonden en bijgehouden vlakken worden geretourneerd met coördinaten (links, boven, breedte en hoogte) waarin wordt aangegeven dat de locatie van vlakken in de afbeelding in pixels, evenals een nominale-id-nummer dat aangeeft dat afzonderlijke volgen. Nominale-id-nummers zijn vatbaar voor opnieuw omstandigheden wanneer de voorzijde nominale is verbroken of overlapt in het frame, klikt u met enkele personen meerdere id's aan de toegewezen resultaat.

###<a id="output_elements"></a>Elementen van het uitvoerbestand JSON

Voor het nominale-detectie en bewerking bijhouden, bevat het resultaat uitvoer de metagegevens van de vlakken binnen het opgegeven bestand in de indeling van JSON.

Het nominale-detectie en bijhouden van JSON bevat de volgende kenmerken:

Element|Beschrijving
---|---
Versie|Dit heeft betrekking op de versie van de Video-API.
Tijdschaal|"Tikken" per seconde van de video.
Verschuiving|Dit is het tijdverschil voor tijdstempels. Versie 1.0 van Video-API's zijn dit altijd 0. In toekomstige scenario's die wordt ondersteund, deze waarde kan veranderen.
Framesnelheid|Frames per seconde van de video.
Fragmenten|De metagegevens is gesegmenteerde omhoog in verschillende segmenten fragmenten genoemd. Elke fragment bevat een begindatum, duur, interval getal en gebeurtenis(sen).
Starten|De begintijd van de eerste gebeurtenis in 'maatstreepjes'.
Duur|De lengte van het fragment, in "maatstreepjes".
Interval|Het interval van elke gebeurtenis-fragment in het fragment, in "tikken".
Gebeurtenissen|Elke gebeurtenis bevat de vlakken gedetecteerd en bijgehouden in die tijdsduur. Dit is een matrix van de matrix van gebeurtenissen. De outer matrix vertegenwoordigt een tijdsinterval. De binnenste matrix bestaat uit 0 of meer gebeurtenissen die op dat moment is gebeurd. Een lege haakjes [] betekent dat er dat geen vlakken zijn aangetroffen.
ID| De ID van het knopvlak dat wordt bijgehouden. Dit nummer kan onbedoeld veranderen als ook het gezicht niet gevonden wordt. Een bepaalde persoon dezelfde ID overal in de algehele video nodig hebt, maar dit kan niet worden gegarandeerd vanwege beperkingen in de detectiealgoritme (Occlusie, enz.)
X, Y|De linksboven X en Y-coördinaten van het selectiekader in een genormaliseerde schaal met 0,0 en 1,0 nominale. <br/>-X en Y coördinaten behoren ten opzichte van de liggende altijd, dus als u een staand video (of ondersteboven, in het geval van iOS) hebt, moet u de coördinaten dienovereenkomstig gewijzigd transponeren.
Breedte, hoogte|De breedte en hoogte van het selectiekader in een genormaliseerde schaal met 0,0 en 1,0 nominale.
facesDetected|Hiermee wordt gevonden aan het einde van de JSON-resultaten en bevat een overzicht van het aantal vlakken die de algoritme van de gedetecteerd tijdens de video. Omdat de id's per ongeluk kunnen worden hersteld als ook het gezicht niet gevonden wordt (bijvoorbeeld nominale scherm er afwezig), gaat dit nummer mogelijk niet altijd gelijk zijn aan het waar aantal vlakken in de video.

Nominale detectie technieken van fragmentatie (waar de metagegevens kan worden opgedeeld in stukken op basis van tijd en u kunt downloaden alleen wat u nodig hebt) en segmentatie (waar de gebeurtenissen die zijn opgedeeld in het geval verkregen te groot worden) wordt gebruikt. Enkele eenvoudige berekeningen kunt u de gegevens transformeren. Als een gebeurtenis de slag op 6300 (maatstreepjes), met een tijdschaal van 2997 (maatstreepjes/sec) bijvoorbeeld en framesnelheid van 29,97 (frames/sec), klikt u vervolgens:

- Begin/tijdschaal = 2.1 seconden
- Seconden x (framesnelheid/tijdschaal) = 63 frames

Hieronder volgt een eenvoudig voorbeeld van het extraheren van de JSON in een per frame-indeling voor nominale detectie en bijhouden:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Computer zien detectie invoer en uitvoer van voorbeeld

###<a name="input-video"></a>Invoer video

[Invoer Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Configuratie van de taak (standaardoptie)

Wanneer u een taak maakt met **Azure Media nominale detectie**, moet u een configuratie vooraf ingestelde. De volgende configuratie vooraf ingestelde is alleen betrekking heeft op nominale detectie.

    {"version":"1.0"}

###<a name="json-output"></a>JSON-uitvoer

Het volgende voorbeeld van JSON-uitvoer is afgebroken.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Emoties detectie invoer en uitvoer voorbeeld


###<a name="input-video"></a>Invoer video

[Invoer Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Configuratie van de taak (standaardoptie)

Wanneer u een taak maakt met **Azure Media nominale detectie**, moet u een configuratie vooraf ingestelde. Hiermee geeft u de volgende configuratie vooraf ingestelde als u wilt maken op basis van de detectie emoties JSON.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Beschrijvingen van kenmerken

Kenmerknaam|Beschrijving
---|---
Modus|Vlakken: Alleen computer zien detectie <br/>AggregateEmotion: Afzender gemiddelde emoties waarden voor alle vlakken in frame.
AggregateEmotionWindowMs|Gebruik als AggregateEmotion modus geselecteerd. Hiermee geeft u de lengte van de video die wordt gebruikt om elke statistische resultaat, (in milliseconden) te bereiken.
AggregateEmotionIntervalMs|Gebruik als AggregateEmotion modus geselecteerd. Hiermee geeft u met welke frequentie en statistische resultaten te retourneren.

####<a name="aggregate-defaults"></a>Statistische standaardwaarden

Hieronder worden aanbevolen waarden voor de statistische-venster en interval-instellingen. AggregateEmotionWindowMs mag langer dan AggregateEmotionIntervalMs.

   |Standaardwaarden (s)|Max(s)|Min(s)
---|---|---|---
AggregateEmotionWindowMs|0,5|2|0,25
AggregateEmotionIntervalMs|0,5|1|0,25

###<a name="json-output"></a>JSON-uitvoer

De JSON uitvoer voor statistische emoties (afgekapt):
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Beperkingen

- De ondersteunde invoer video-indelingen zijn MP4, MOV en WMV.
- Het bereik van de grootte detecteerbare nominale is 24 x 24 naar 2048 x 2048 pixels. De vlakken afmelden bij dit bereik wordt niet gedetecteerd.
- Voor elke video is het maximum aantal vlakken geretourneerd 64.
- Sommige vlakken kunnen niet wordt herkend vanwege technische uitdagingen; bijvoorbeeld zeer grote nominale hoeken (hoofd-pose) en grote Occlusie. Voorzijde en in de buurt binnen handbereik vlakken hebben de beste resultaten.


## <a name="sample-code"></a>Voorbeeld van een code

De volgende programma ziet u hoe u:

1. Activa maken en een mediabestand uploaden naar de activa.
1. Hiermee maakt u een taak een nominale detectie taak op basis van een configuratiebestand met de volgende json vooraf ingestelde. 
                    
        {
            "version": "1.0"
        }

1. De uitvoer JSON-bestanden worden gedownload. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
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
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
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

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)