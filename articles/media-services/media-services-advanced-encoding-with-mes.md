<properties 
    pageTitle="Geavanceerde coderen met Media Encoder Standard" 
    description="Dit onderwerp wordt uitgelegd hoe u geavanceerde codering door het aanpassen van Media Encoder standaard taak vooraf ingestelde uitvoert. Het onderwerp ziet hoe u Media Services .NET SDK gebruiken om een codering taak en de taak te maken. Ook ziet u hoe u aangepaste standaardinstellingen aan de codering taak opgeeft." 
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
    ms.date="09/25/2016"   
    ms.author="juliako"/>


#<a name="advanced-encoding-with-media-encoder-standard"></a>Geavanceerde coderen met Media Encoder Standard

##<a name="overview"></a>Overzicht

Dit onderwerp wordt uitgelegd hoe u geavanceerde codering taken met Media Encoder Standard uitvoert. Het onderwerp ziet u [het gebruik van .NET als een taak in een codering en een taak die wordt uitgevoerd van deze taak wilt maken](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Ook ziet u hoe u aangepaste standaardinstellingen aan de codering taak opgeeft. Beschrijving van elementen die worden gebruikt door de vooraf ingestelde, [het document](https://msdn.microsoft.com/library/mt269962.aspx)te zien. 

De aangepaste vooraf ingestelde die de volgende codering taken uitvoeren worden getoond:

- [Miniaturen genereren](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [Een video (schermopname) knippen](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Maken van een overlay](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Een stille audionummer invoegen wanneer invoer geen audio heeft](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Automatisch de-interlacing uitschakelen](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Alleen geluid vooraf ingestelde](media-services-custom-mes-presets-with-dotnet.md#audio_only)
- [Tekst.samenvoegen twee of meer videobestanden](media-services-custom-mes-presets-with-dotnet.md#concatenate)
- [Bijsnijden video's met Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md#crop)
- [Invoegen van een video bijhouden wanneer invoer geen video heeft](media-services-custom-mes-presets-with-dotnet.md#no_video)
- [Een video draaien](media-services-custom-mes-presets-with-dotnet.md#rotate_video)

##<a id="encoding_with_dotnet"></a>Coderen met mediaservices .NET SDK

Het volgende voorbeeld wordt Media Services .NET SDK gebruikt de volgende taken uitvoeren:

- Een codering taak maken.
- Een verwijzing naar de Media Encoder standaard encoder krijgen.
- De aangepaste XML laden of JSON vooraf ingestelde. U kunt de XML-gegevens opslaan of JSON [(bijvoorbeeld XML-](media-services-custom-mes-presets-with-dotnet.md#xml) of [JSON](media-services-custom-mes-presets-with-dotnet.md#json) in een bestand en gebruik de volgende code om het bestand te laden.

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
- Een codering taak toevoegen aan de taak. 
- Geef de invoer activa moeten worden gecodeerd.
- Activa uitvoer waarin het gecodeerde actief maken.
- Een gebeurtenis-handler als u wilt controleren van de taakvoortgang toevoegen.
- De taak verstuurt.
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
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
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
                
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
                
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
                
                    return job.OutputMediaAssets[0];
                }
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        

                private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                            break;
                        default:
                            break;
                    }
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
            }
        }


##<a name="support-for-relative-sizes"></a>Ondersteuning voor de relatieve grootte

Als miniaturen van deze worden gegenereerd, hoeft u niet moeten worden opgegeven altijd uitvoer breedte en hoogte in pixels. U kunt opgeven in percentages in het bereik [1%,..., 100%].

###<a name="json-preset"></a>Vooraf ingestelde JSON 
    
    "Width": "100%",
    "Height": "100%"

###<a name="xml-preset"></a>XML-vooraf ingestelde
    
    <Width>100%</Width>
    <Height>100%</Height>
    
##<a id="thumbnails"></a>Miniaturen genereren

In dit gedeelte ziet hoe u een vooraf ingestelde die wordt gegenereerd miniaturen aanpassen. De vooraf ingestelde hieronder bevat informatie over hoe u wilt uw bestand, evenals de gegevens die nodig zijn om te genereren miniaturen coderen. U kunt een van de MES vooraf ingestelde beschreven [hier](https://msdn.microsoft.com/library/mt269960.aspx) en code die wordt gegenereerd miniaturen toevoegen.  

>[AZURE.NOTE]De instelling **SceneChangeDetection** in alleen de volgende vooraf ingestelde kunnen worden ingesteld op waar als u naar een video één bitsnelheid worden gecodeerd. Als u naar een video multi-bitrate worden gecodeerd en **SceneChangeDetection** ingesteld op waar, retourneert de encoder een fout.  


Zie voor informatie over het schema, [in dit](https://msdn.microsoft.com/library/mt269962.aspx) onderwerp.

Zorg ervoor dat de sectie [overwegingen](media-services-custom-mes-presets-with-dotnet.md#considerations) .

###<a id="json"></a>Vooraf ingestelde JSON


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a id="xml"></a>XML-vooraf ingestelde


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Overwegingen

De volgende punten zijn van toepassing:

- Het gebruik van expliciete tijdstempels voor begin/stap/bereik wordt ervan uitgegaan dat de invoerbron ten minste 1 minuut lang is.
- Png-jpg/BmpImage elementen dat moet worden gestart, stap en tekenreekskenmerken bereik – deze kunnen worden geïnterpreteerd als:

    - Kader getal als deze niet-negatieve gehele getallen zijn, bijvoorbeeld "starten": "120,"
    - Ten opzichte van gegevensbron duur als uitgedrukt als % volgen, bijvoorbeeld "starten": "15%", of
    - Tijdstempel als u: mm: ss uitgedrukt... opmaken, bijvoorbeeld "starten": "00: 01:00"

    U kunt meng en notaties als u neemt overeenkomen.
    
    Start ondersteunt Daarnaast kunnen ook een speciale Macro: {aanbevolen}, die Hiermee kunt u proberen om te bepalen de eerste 'interessant' frame van de inhoud NOTITIE: (stap en bereik worden genegeerd bij het begin is ingesteld op {aanbevolen})
    
    - Standaardwaarden: Start: {aanbevolen}
- Uitvoerindeling moet expliciet worden verstrekt voor elke afbeelding van de indeling: Png-Jpg/BmpFormat. Wanneer aanwezig zijn, MES overeenkomen met de JpgVideo naar JpgFormat enzovoort. Uitvoerindeling maakt u kennis met een nieuwe afbeelding-codec specifieke Macro: {Index}, welke moet presenteren (eenmaal en slechts één keer) voor de afbeelding van de uitvoerindelingen.

##<a id="trim_video"></a>Een video (schermopname) knippen

In dit gedeelte moment spreekt over het wijzigen van de encoder vooraf ingestelde als u wilt knippen of de invoer video waar de invoer een zogenaamde mezzanine of op aanvraag-bestand is knippen. De encoder kan ook worden gebruikt om illustraties activa, die is geregistreerd of gearchiveerd uit een live gegevensstroom: de details hiervoor zijn beschikbaar in [Dit blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Als u wilt bijsnijden uw video's, kunt u een van de MES vooraf ingestelde beschreven [hier](https://msdn.microsoft.com/library/mt269960.aspx) en wijzigen van het element **bronnen** (zoals hieronder weergegeven). De waarde van de starttijd moet overeenkomen met de absolute tijdstempel van de invoer video. Bijvoorbeeld, als het eerste frame van de invoer video een tijdstempel van 12:00:10.000 heeft, klikt u vervolgens starttijd moet ten minste 12:00:10.000 en hoger. In het onderstaande voorbeeld nemen we u aan dat de invoer video een begindatum tijdstempel van nul heeft. **Bronnen** moet aan het begin van de vooraf ingestelde worden geplaatst. 
 
###<a id="json"></a>Vooraf ingestelde JSON
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    } 

###<a name="xml-preset"></a>XML-vooraf ingestelde
    
Als u wilt bijsnijden uw video's, kunt u een van de MES vooraf ingestelde beschreven [hier](https://msdn.microsoft.com/library/mt269960.aspx) en wijzigen van het element **bronnen** (zoals hieronder weergegeven).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a id="overlay"></a>Maken van een overlay

De Media Encoder standaard kunt u een afbeelding op een bestaande video overlay. Op dit moment de volgende indelingen worden ondersteund: png, jpg, GIF-bestand, en .bmp. De vooraf ingestelde hieronder is een voorbeeld van de eenvoudige van een video-overlay.

Naast het definiëren van een vooraf ingestelde bestand, moet u ook laten weten welke bestand in de activa is de overlay-afbeelding en welke is de bron video waarop u wilt de afbeelding overlay Media-Services. Het videobestand is het **primaire** bestand. 

Met het bovenstaande voorbeeld van .NET definieert twee functies: **UploadMediaFilesFromFolder** en **EncodeWithOverlay**. De functie UploadMediaFilesFromFolder uploadt bestanden van een map (bijvoorbeeld BigBuckBunny.mp4 en Image001.png) en het mp4-bestand naar het primaire bestand in de activa worden ingesteld. De functie **EncodeWithOverlay** het aangepaste vooraf ingestelde bestand dat is doorgegeven (bijvoorbeeld de standaardoptie die volgt) wordt gebruikt om de versleuteling taak te maken. 

>[AZURE.NOTE]Huidige beperkingen:
>
>De instelling van de dekking overlay wordt niet ondersteund.
>
>Uw bron-videobestand en de overlay-afbeeldingsbestand moeten zich op dezelfde activa en het videobestand moet worden ingesteld als de primaire-bestand in deze activa.

###<a name="json-preset"></a>Vooraf ingestelde JSON
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="xml-preset"></a>XML-vooraf ingestelde
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


##<a id="silent_audio"></a>Een stille audionummer invoegen wanneer invoer geen audio heeft

Als u een invoer voor de encoder met alleen video, en geen geluid en verzend vervolgens bevat de uitvoer van activa standaard bestanden die alleen video gegevens bevatten. Sommige spelers mogelijk niet worden verwerkt, zoals streams uitvoer. U kunt deze instelling gebruiken waarmee de encoder een stille-nummer toevoegen aan de uitvoer in dit scenario wordt afgedwongen.

Als u wilt dat de encoder zodat het eindresultaat activa met een stille audio bijhouden wanneer invoer geen audio heeft, geeft u de waarde "InsertSilenceIfNoAudio".

U kunt een van de MES vooraf ingestelde beschreven [hier](https://msdn.microsoft.com/library/mt269960.aspx), en volgende wijziging doorgevoerd:

###<a name="json-preset"></a>Vooraf ingestelde JSON

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>XML-vooraf ingestelde

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a id="deinterlacing"></a>Automatisch de-interlacing uitschakelen

Klanten hoeft niet te doen als ze de inhoud interlace worden automatisch opgeheven geïnterlinieerde. Als het automatisch de-interlacing ingeschakeld (standaard is) wordt de MES bevat de automatische detectie van geïnterlinieerde frames en frames gemarkeerd als geïnterlinieerd alleen opgeheven interlaces.

Kunt u de automatisch de-interlacing uitschakelen. Deze optie wordt niet aanbevolen.

###<a name="json-preset"></a>Vooraf ingestelde JSON
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>XML-vooraf ingestelde
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a id="audio_only"></a>Alleen geluid vooraf ingestelde

In dit gedeelte ziet u twee standaardinstellingen voor alleen geluid MES: AAC Audio- en AAC goede kwaliteit Audio.

###<a name="aac-audio"></a>AAC-Audio 

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>De beste audiokwaliteit AAC

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="concatenate"></a>Tekst.samenvoegen twee of meer videobestanden

Het volgende voorbeeld ziet u hoe u een vooraf ingestelde als u wilt samenvoegen van twee of meer videobestanden kunt genereren. De meest voorkomende scenario is wanneer u wilt een koptekst of een mag toevoegen aan de belangrijkste video. De beoogde gebruik is wanneer de videobestanden samen wordt bewerkt eigenschappen (video resolutie, frame rente, audionummer tellen, enzovoort deelt). U moet verrichten niet als u wilt samenstellen van video's met verschillende frame tarieven of met een verschillend aantal audio nummers.

###<a name="requirements-and-considerations"></a>Vereisten en overwegingen

- Invoer video's mag alleen één audionummer hebben.
- Invoer video's moeten hebben allemaal het tarief weer dat hetzelfde frame.
- U moet uw video's uploaden in afzonderlijke activa en de video's instellen als de primaire-bestand in elke activa.
- U moet weten van de duur van uw video's.
- De vooraf ingestelde onderstaande voorbeelden wordt ervan uitgegaan dat de invoer video's starten met een tijdstempel van nul. U moet wijzigen van de waarden starttijd als de video's verschillende begin tijdstempel hebben, zoals meestal het geval met live archieven.
- Expliciete verwijzingen naar het item-id-waarden van de invoer activa zorgt ervoor dat de JSON-definitie.
- De voorbeeldcode wordt ervan uitgegaan dat de JSON vooraf ingestelde is opgeslagen op een lokaal bestand, zoals "C:\supportFiles\preset.json". Er is ook wordt aangenomen dat twee activa zijn gemaakt door twee video bestanden uploaden en dat u weet dat de resulterende item-id-waarden.
- Het codefragment en JSON vooraf ingestelde toont een voorbeeld van het samenvoegen van twee videobestanden. U kunt deze uitbreiden naar meer dan twee video's door:

    1. De taak om te bellen kwijt. InputAssets.Add() herhaaldelijk om toe te voegen meer video's in volgorde.
    2. Waardoor overeenkomt wijzigingen in het element 'Bronnen' in de JSON, door meer vermeldingen toe te voegen in dezelfde volgorde. 


###<a name="net-code"></a>.NET-code

    
    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();
    
    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");
    
    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);
    
    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job. 
    // This output is specified as AssetCreationOptions.None, which 
    // means the output asset is not encrypted. 
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);
    
    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

###<a name="json-preset"></a>Vooraf ingestelde JSON

Werk uw aangepaste vooraf ingestelde met id's van de activa die u wilt samenvoegen en met de juiste tijd segment voor elke video.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="crop"></a>Bijsnijden video's met Media Encoder Standard

Zie het onderwerp [bijsnijden video's met Media Encoder Standard](media-services-crop-video.md) .

##<a id="no_video"></a>Invoegen van een video bijhouden wanneer invoer geen video heeft

Als u verzendt een invoer voor de encoder met alleen audio en geen video, klikt u vervolgens bevat de uitvoer van activa standaard bestanden die alleen audio gegevens bevatten. Sommige spelers, inclusief Azure Media Player (Zie [deze](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) mogelijk niet worden verwerkt, zoals streams. U kunt deze instelling gebruiken waarmee de encoder een zwart-wit video bijhouden toevoegen aan de uitvoer in dit scenario wordt afgedwongen. 

>[AZURE.NOTE]De encoder invoegen van een video bijhouden uitvoer afdwingen neemt de grootte van de uitvoer van activa, en daarmee ook de kosten weergegeven voor de codering taak. U kunt testen om te bevestigen dat deze resulterende toename alleen een bescheiden invloed op de kosten van uw maandelijkse heeft moet uitvoeren.

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Video invoegen op de laagste bitsnelheid gebruikt

Stel dat u gebruikt een meerdere Codering bitsnelheid vooraf ingestelde zoals ["H264 meerdere bitsnelheid 720p"](https://msdn.microsoft.com/library/mt269960.aspx) voor het coderen van de hele invoer catalogus voor streaming, waarin een combinatie van video's en bestanden met alleen geluid. In dit scenario wanneer de invoer geen video, klikt heeft u mogelijk wilt afdwingen van de encoder invoegen van een zwart-wit video bijhouden op alleen de laagste bitsnelheid gebruikt, in plaats van video invoegen op elke uitvoer bitsnelheid gebruikt. Hiervoor moet u de vlag 'InsertBlackIfNoVideoBottomLayerOnly' opgeven.

U kunt een van de MES vooraf ingestelde beschreven [hier](https://msdn.microsoft.com/library/mt269960.aspx), en volgende wijziging doorgevoerd:

#### <a name="json-preset"></a>Vooraf ingestelde JSON

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-vooraf ingestelde

    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideoBottomLayerOnly</Condition>

### <a name="inserting-video-at-all-output-bitrates"></a>Video helemaal invoegen uitvoer bitsnelheid

Stel dat u gebruikt een meerdere Codering bitsnelheid vooraf ingestelde zoals ["H264 meerdere bitsnelheid 720p](https://msdn.microsoft.com/library/mt269960.aspx) voor het coderen van de hele invoer catalogus voor streaming, waarin een combinatie van video's en bestanden met alleen geluid. In dit scenario wanneer de invoer geen video, klikt u heeft mogelijk wilt u de encoder om in te voegen een zwart-wit video bijhouden helemaal de uitvoer bitsnelheid afdwingen. Dit zorgt ervoor dat uw uitvoer activa zijn alle homogene met betrekking tot aantal nummers video en audio nummers. Hiervoor moet u de vlag 'InsertBlackIfNoVideo' opgeven.

U kunt een van de MES vooraf ingestelde beschreven [hier](https://msdn.microsoft.com/library/mt269960.aspx), en volgende wijziging doorgevoerd:

#### <a name="json-preset"></a>Vooraf ingestelde JSON

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-vooraf ingestelde
    
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideo</Condition>

##<a id="rotate_video"></a>Een video draaien

De [Media Encoder standaard](media-services-dotnet-encode-with-media-encoder-standard.md) ondersteunt draaiing door de hoeken van 0/90/180/270. Het standaardgedrag is 'Automatisch', waarbij wordt geprobeerd de draaiing metagegevens in de binnenkomende videobestand detecteren en dit voor deze. Zijn de volgende **bronnen** element naar een van de vooraf ingestelde gedefinieerde [hier](http://msdn.microsoft.com/library/azure/mt269960.aspx):

### <a name="json-preset"></a>Vooraf ingestelde JSON

    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...
### <a name="xml-preset"></a>XML-vooraf ingestelde

    <Sources>
        <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Zie ook [in dit](https://msdn.microsoft.com/library/azure/mt269962.aspx#PreserveResolutionAfterRotation) onderwerp voor meer informatie over hoe de breedte en hoogte-instellingen in de vooraf ingestelde, door de encoder worden geïnterpreteerd als draaiing vergoedingen wordt geactiveerd.

U kunt de waarde '0' gebruiken om de encoder genegeerd draaiing metagegevens, als deze aanwezig zijn, in het vak Invoerbereik video aan te geven. 


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook 

[Media Services-codering overzicht](media-services-encode-asset.md)
