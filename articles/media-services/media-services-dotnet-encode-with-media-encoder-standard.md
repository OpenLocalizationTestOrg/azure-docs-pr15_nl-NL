<properties 
    pageTitle="Activa coderen met Media Encoder Standard met .NET | Microsoft Azure" 
    description="Dit onderwerp wordt uitgelegd hoe u met .NET activa met Media Encoder Strandard coderen." 
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
    ms.date="09/19/2016"
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Activa coderen met Media Encoder Standard met .NET

Tekstcodering taken zijn een van de meest voorkomende verwerking in Media Services. U maken codering taken als u wilt converteren media-bestanden van één codering naar een andere. Wanneer u coderen, kunt u de Media Services ingebouwde Media Encoder. U kunt ook een encoder verstrekt door een partner Media Services; derde partij encoders zijn beschikbaar via de Azure Marketplace. 

In dit onderwerp wordt uitgelegd hoe u uw activa met Media Encoder standaard (MES) coderen met .NET. Media Encoder standaard is geconfigureerd met een van de encoder vooraf ingestelde beschreven [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Het wordt aanbevolen voor het coderen van altijd uw mezzanine-bestanden in een geavanceerde bitsnelheid MP4 instellen en vervolgens de set converteren naar de gewenste indeling met de [Dynamische verpakking](media-services-dynamic-packaging-overview.md). Om te profiteren van dynamische verpakking, moet u eerst ten minste één op aanvraag streaming eenheid ophalen voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud. Lees [hoe u de schaal Media Services](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

Als uw activa uitvoer versleuteld opslag is, moet u activa bezorging beleid configureren. Zie [configureren activa bezorging beleid](media-services-dotnet-configure-asset-delivery-policy.md)voor meer informatie.

>[AZURE.NOTE]MES genereert een uitvoerbestand met een naam die de eerste 32 tekens van de invoer bestandsnaam bevat. De naam is gebaseerd op wat is opgegeven in de vooraf ingestelde bestand. Bijvoorbeeld: "bestandsnaam": "{Basename} _ {Index} {extensie}". {Basename} wordt vervangen door de eerste 32 tekens van de invoer bestandsnaam.

###<a name="mes-formats"></a>MES indelingen

[Notaties en codecs](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>Vooraf ingestelde MES

Media Encoder standaard is geconfigureerd met een van de encoder vooraf ingestelde beschreven [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Invoer- en uitvoerbereik metagegevens

Als u invoer activa (of activa) met MES coderen, krijgt u activa uitvoer bij de succesvolle afronding van die taak coderen. De uitvoer van activa bevat video, audio, miniaturen, manifest, enzovoort, op basis van de codering vooraf ingestelde die u gebruikt.

De uitvoer van activa bevat ook een bestand met metagegevens over de invoer activa. De naam van het XML-metagegevensbestand heeft de volgende indeling: < asset_id > _metadata.xml (bijvoorbeeld 41114ad3-eb5e - 4c 57-8d 92-5354e2b7d4a4_metadata.xml), waarbij < asset_id > de waarde van het item-id van de invoer actief is. Het schema van deze invoer XML-metagegevens wordt beschreven [hier](http://msdn.microsoft.com/library/azure/dn783120.aspx).

De uitvoer van activa bevat ook een bestand met metagegevens over de uitvoer van activa. De naam van het XML-metagegevensbestand heeft de volgende indeling: < source_file_name > _manifest.xml (bijvoorbeeld BigBuckBunny_manifest.xml). Het schema van deze uitvoer metagegevens XML wordt beschreven [hier](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Als u onderzoeken welke gevolgen een van de twee metagegevensbestanden wilt, kunt u een locator SA's maken en kunt u het bestand downloaden naar uw lokale computer. U kunt een voorbeeld vinden over het maken van een locator SA's en het downloaden van een bestand met de Media Services .NET SDK-uitbreidingen.

##<a name="download-sample"></a>Voorbeeld downloaden

Ophalen en uitvoeren van een steekproef vanaf [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##<a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt Media Services .NET SDK gebruikt de volgende taken uitvoeren:

- Een codering taak maken.
- Een verwijzing naar de Media Encoder standaard encoder krijgen.
- Geef als u wilt gebruiken de "H264 meerdere bitsnelheid 720p" vooraf ingestelde. U kunt zien dat de vooraf ingestelde [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). U kunt ook controleren het schema waaraan deze vooraf ingestelde moeten voldoen [hier](https://msdn.microsoft.com/library/mt269962.aspx) onderwerp.
- Eén codering taak toevoegen aan de taak. 
- Geef de invoer activa moeten worden gecodeerd.
- Activa uitvoer waarin u de gecodeerde activa maken.
- Een gebeurtenis-handler als u wilt controleren van de taakvoortgang toevoegen.
- De taak verstuurt.
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
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


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook 

[Het genereren van de miniatuur van het gebruik van Media Encoder standaard met .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Overzicht van Media Services-codering](media-services-encode-asset.md)
