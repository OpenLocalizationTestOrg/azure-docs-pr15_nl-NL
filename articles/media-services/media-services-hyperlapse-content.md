<properties
    pageTitle="Hyperlapse Media-bestanden met Azure Media Hyperlapse | Microsoft Azure"
    description="Azure Media Hyperlapse maakt vloeiende verstreken tijd video's van de eerste persoon of actie-camera-inhoud. Dit onderwerp wordt uitgelegd hoe u Media indexering gebruiken."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Hyperlapse Media-bestanden met Azure Media Hyperlapse

Azure Media Hyperlapse is een Media Processor (MP) die vloeiend verstreken tijd video's worden gemaakt van de eerste persoon of actie-camera-inhoud.  De cloudgebaseerde op hetzelfde niveau op [de Microsoft Research bureaublad Hyperlapse Pro en telefoongebaseerde Hyperlapse Mobile](http://aka.ms/hyperlapse), Microsoft Hyperlapse voor Azure Media Services maakt gebruik van de enorme schaal van het verwerken van Azure Media Services Media-platform horizontaal schaal en parallelize bulksgewijs Hyperlapse verwerken.

>[AZURE.IMPORTANT]Microsoft Hyperlapse is ontworpen om te werken het beste op inhoud van de eerste persoon met een zwevend camera.  Hoewel u nog steeds de camera-opnamen kunt nog steeds werkt, worden niet de prestaties en de kwaliteit van de Azure Media Hyperlapse Media Processor voor andere soorten inhoud gegarandeerd.  Voor meer informatie over Microsoft Hyperlapse voor Azure Media Services en bekijk enkele voorbeeld video's, raadpleegt u de [inleidende blog publiceren](http://aka.ms/azurehyperlapseblog) vanuit de openbare preview.

Een Azure Media Hyperlapse taak als neemt invoer een MP4, MOV of WMV activa bestand samen met een configuratiebestand die Hiermee wordt aangegeven welke frames van video moeten verstreken tijd en naar welke snelheid (bijvoorbeeld eerste 10.000 frames op 2 x).  De uitvoer is een constante en verstreken tijd weergave van de video invoer.

Zie voor de meest recente updates van de Azure Media Hyperlapse [Media Services blogs](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse activa

U moet eerst uw gewenste invoer bestand uploaden naar Azure Media Services.  Lees meer informatie over de concepten die nodig zijn voor het uploaden van inhoud en beheren, het [beheer van webinhoud artikel](media-services-portal-vod-get-started.md).

###  <a id="configuration"></a>Vooraf ingestelde configuratie voor Hyperlapse

Zodra de inhoud beschikbaar in uw Media Services-account is, moet u uw configuratie vooraf ingestelde maken.  De volgende tabel beschrijft de gebruiker gedefinieerde velden:

 Veld | Beschrijving
-------|-------------
StartFrame|Het frame waarop de verwerking van Microsoft Hyperlapse moet beginnen.
NumFrames|Het aantal frames verwerken
Snelheid|De factor waarmee u de invoer video wilt versnellen.

Hier volgt een voorbeeld van een conform configuratiebestand in XML- en JSON:

**XML-vooraf ingestelde:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**Vooraf ingestelde JSON:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a id="sample_code"></a>Microsoft Hyperlapse met de AMS .NET SDK

De volgende methode een media-bestand als een actief uploads en maakt een project met de Processor Azure Media Hyperlapse Media.

> [AZURE.NOTE] U moet een CloudMediaContext al in het bereik met de naam 'context' voor deze code werkt.  Lees meer informatie over dit, het [beheer van webinhoud artikel](media-services-dotnet-get-started.md).

> [AZURE.NOTE] Het tekenreeksargument 'hyperConfig' naar verwachting moeten een conform configuratie vooraf ingestelde in JSON of XML-zoals hierboven is beschreven.

statische bool RunHyperlapseJob (tekenreeksinvoer, tekenreeksuitvoer, tekenreeks hyperConfig) {/ / Maak activa met invoer bestand IAsset activa = context. Activa. CreateAssetAndUploadSingleFile (input, 'Mijn Hyperlapse invoer', AssetCreationOptions.None);

Pak exemplaren van Azure Media Hyperlapse MP IMediaProcessor mp = context. MediaProcessors. GetLatestMediaProcessorByName ("Azure Media Hyperlapse");

Taak maken met de taak IJob Hyperlapse = context. Taken. Maken (String.Format (input "Hyperlapse {0}")).

Als (String.IsNullOrEmpty(hyperConfig)) {/ / config mag geen lege afzender ONWAAR;}

hyperConfig = File.ReadAllText(hyperConfig);

ITask hyperlapseTask = taak. Tasks.AddNew ('Hyperlapse taak', mp, hyperConfig, TaskOptions.None); hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Hyperlapse uitvoer", AssetCreationOptions.None).


taak. Submit();

Voortgang af te drukken en query's uitvoeren taken taak progressPrintTask maken nieuwe Task(() = = > {

IJob jobQuery = null; Voer {var progressContext = context; jobQuery = progressContext.Jobs. Waar (j = > j.Id == taak. ID). First(); Console.WriteLine (tekenreeks. Opmaak ("\t {1} \t {0} {2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. Voortgang)); Thread.Sleep(10000); } terwijl (jobQuery.State! JobState.Finished = & & jobQuery.State! JobState.Error = & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a id="file_types"></a>Ondersteunde bestandstypen

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Verwante koppelingen

[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
