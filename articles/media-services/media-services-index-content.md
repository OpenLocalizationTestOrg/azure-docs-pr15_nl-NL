<properties
    pageTitle="Media-bestanden met Azure Media indexering indexeren"
    description="Azure Media indexering kunt u inhoud van uw mediabestanden doorzoekbare maken en genereert een transcriptie van de volledige tekst voor gesloten ondertiteling en trefwoorden. Dit onderwerp wordt uitgelegd hoe u Media indexering gebruiken."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Media-bestanden met Azure Media indexering indexeren


Azure Media indexering kunt u inhoud van uw mediabestanden doorzoekbare maken en genereert een transcriptie van de volledige tekst voor gesloten ondertiteling en trefwoorden. U kunt één media-bestand of meerdere media-bestanden in een batch verwerken.  

>[AZURE.IMPORTANT] Wanneer u inhoud, zorg er dan voor dat mediabestanden met zeer wissen spraak (zonder de achtergrondmuziek, ruis, effecten of microfoon hiss) gebruiken. Voorbeelden van de juiste inhoud zijn: opgenomen vergaderingen, colleges of -presentaties. De volgende inhoud mogelijk niet geschikt voor het indexeren: films, tv-programma's, alles met gemengde audio- en geluidseffecten, opgenomen slecht inhoud met achtergrondruis (hiss).


Een taak indexing kunt genereren de uitvoer van de volgende:

- Gesloten bijschrift bestanden in de volgende indelingen: **SAMI**, **TTML**en **WebVTT**.

    Ondertiteling-bestanden bevatten een tag Recognizability, welke scores een indexing taak afhankelijk van hoe herkenbaar de spraak in de bronvideo genoemd.  U kunt de waarde van Recognizability scherm uitvoerbestanden voor bruikbaarheid. Een laag score betekent slechte indexing resultaten vanwege geluidskwaliteit.
- Trefwoordenbestand (XML).
- Audio indexeren blob-bestand (AIB) voor gebruik met SQL server.

    Zie [Bestanden met behulp van AIB met Azure Media indexering en SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)voor meer informatie.


Dit onderwerp wordt uitgelegd hoe u indexing taken naar **Index activa** en **indexeren van meerdere bestanden**maken.

Zie voor de meest recente updates van de Azure Media indexering [Media Services blogs](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Configuratie en manifest-bestanden gebruiken voor het indexeren van taken

Meer informatie kunt voor uw indexing taken u opgeven met behulp van de configuratie van een taak. Bijvoorbeeld, kunt u opgeven welke metagegevens wilt gebruiken voor het mediabestand. Deze metagegevens wordt gebruikt door de taal-engine om uit te vouwen van de woordenlijst en enorm verbetert de nauwkeurigheid van de spraakherkenning.  Bent u ook kunnen uw uitvoerbestanden gewenste opgeven.

U kunt ook meerdere media-bestanden in één keer verwerken met behulp van een manifest bestand.

Zie de [Taak vooraf ingestelde voor Azure Media indexering](https://msdn.microsoft.com/library/dn783454.aspx)voor meer informatie.

## <a name="index-an-asset"></a>Indexeren van activa

De volgende methode een media-bestand als een actief uploadt en maakt u een opdracht voor het indexeren van de activa.

Houd er rekening mee dat als er geen configuratiebestand is opgegeven, het mediabestand worden geïndexeerd met alle standaardinstellingen.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
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
<!-- __ -->
### <a id="output_files"></a>Uitvoerbestanden

Standaard genereert een indexing taak de volgende uitvoerbestanden. De bestanden worden opgeslagen in de eerste uitvoer activa.

Als er meer dan één invoer mediabestand, genereert indexering een manifest bestand voor de uitvoer van de taak, met de naam 'JobResult.txt'. Voor elk mediabestand invoert, zijn de resulterende AIB, SAMI TTML, WebVTT en trefwoord bestanden, opeenvolging genummerd en met de naam met de 'Alias'.

Bestandsnaam | Beschrijving
----------|------------
__InputFileName.aib__ | De Indexeerstatus blob audiobestand. <br /><br /> Audiobestand indexeren Blob (AIB) is een binair bestand dat kan worden doorzocht in Microsoft SQL server met volledige tekst zoeken.  Het bestand AIB is krachtiger dan de eenvoudige bijschrift-bestanden, omdat deze alternatieven voor elk woord, zodat een veel rijkere zoekfunctie bevat. <br/> <br/>Dit is de installatie van de invoegtoepassing voor indexering SQL op een computer waarop Microsoft SQL server 2008 of hoger vereist. Zoeken de AIB met Microsoft SQL biedt server volledige tekst zoeken nauwkeuriger zoekresultaten dan het zoeken naar de ondertiteling bestanden gegenereerd door WAMI. Dit komt omdat de AIB word alternatieven welke klinken bevat terwijl de ondertiteling-bestanden de hoogste betrouwbaarheid in word voor elk segment van de audio bevatten. Als zoeken naar gesproken woorden van upmost urgentie, wordt klikt u vervolgens het aanbevolen voor het gebruik van de AIB In combinatie met Microsoft SQL Server.<br/><br/> Klik op <a href="http://aka.ms/indexersql">Azure Media indexering SQL invoegtoepassing</a>wilt downloaden van de invoegtoepassing. <br/><br/>Het is ook mogelijk andere zoekprogramma's zoals Apache Lucene/Solr gewoon indexeren de video die is gebaseerd op de ondertiteling en trefwoord XML-bestanden gebruiken, maar resulteert dit in minder nauwkeurig lijst met zoekresultaten.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |Gesloten bijschrift (CC)-bestanden in SAMI, TTML en WebVTT indelingen.<br/><br/>Ze kunnen worden gebruikt om audio-en videobestanden toegankelijk te maken voor personen met de disability horen.<br/><br/>Gesloten bijschrift-bestanden bevatten een tag <b>Recognizability</b> welke scores een indexing taak op basis van hoe herkenbaar spraak in de video bron wordt genoemd.  U kunt de waarde van <b>Recognizability</b> scherm uitvoerbestanden voor bruikbaarheid. Een laag score betekent slechte indexing resultaten vanwege geluidskwaliteit.
__InputFileName.kw.xml<br />InputFileName.info__ |Trefwoord en op info-bestanden. <br/><br/>Trefwoordenbestand is een XML-bestand met trefwoorden opgehaald uit de spraak-inhoud, met frequentie en verschuiving gegevens. <br/><br/>Infobestand is een tekstbestand met gedetailleerde informatie over elke term die wordt herkend. De eerste regel is speciale en Recognizability score bevat. Elke volgende regel is een door tabs gescheiden lijst met de volgende gegevens: tijd, eindtijd, woord of woordgroep, betrouwbaarheid starten. De tijden worden gegeven in seconden en de betrouwbaarheid wordt weergegeven als een getal van 0-1. <br/><br/>Voorbeeld van de regel: "1,20 1,45 word 0.67" <br/><br/>Deze bestanden kunnen worden gebruikt voor een aantal doeleinden, zoals spraak-analyses uitvoeren of die worden aangeboden zoekmachines zoals Bing, Google of Microsoft SharePoint om de mediabestanden overzichtelijker, of zelfs die worden gebruikt voor het leveren van meer relevante advertenties.
__JobResult.txt__ |Uitvoer manifest, alleen beschikbaar wanneer er meerdere bestanden, met de volgende gegevens indexeren:<br/><br/><table border="1"><tr><th>Invoerbestand</th><th>Alias</th><th>MediaLength</th><th>Fout</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Als dat niet alle invoer media-bestanden worden geïndexeerd, de indexing taak, mislukt met foutcode 4000. Zie voor meer informatie [foutcodes](#error_codes).

## <a name="index-multiple-files"></a>Meerdere bestanden indexeren

De volgende methode meerdere media-bestanden uploadt als activa en maakt een taak als u wilt indexeren al deze bestanden in een batch.

Een manifest bestand met de extensie .lst worden gemaakt en uploaden naar de activa. Het manifest bestand bevat de lijst met alle activa bestanden. Zie de [Taak vooraf ingestelde voor Azure Media indexering](https://msdn.microsoft.com/library/dn783454.aspx)voor meer informatie.

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Gedeeltelijk geslaagd taak

Als dat niet alle invoer media-bestanden worden geïndexeerd, de indexing taak, mislukt met foutcode 4000. Zie voor meer informatie [foutcodes](#error_codes).


De uitvoer van dezelfde (als geslaagd taken) worden gegenereerd. U kunt ook verwijzen naar het manifest uitvoerbestand om vast te stellen welke invoer bestanden zijn mislukt, op basis van de foutwaarden van de kolom. Voor invoer bestanden die is mislukt, de resulterende AIB, SAMI, TTML, WebVTT en trefwoord worden bestanden niet gegenereerd.

### <a id="preset"></a>Taak definitie voor Azure Media indexering

De verwerking van Azure Media indexering kan worden aangepast doordat een optionele taak vooraf ingestelde samen met de taak.  De volgende tabel ziet de opmaak van deze configuratie-xml.

Naam | Vereisen | Beschrijving
----|----|---
__invoer__ | ONWAAR | Activa-bestanden die u wilt indexeren.</p><p>Azure Media indexering ondersteunt de volgende media-bestandsindelingen: MP4, WMV, MP3-bestand, M4A, WMA, AAC, WAV.</p><p>(Zoals hieronder weergegeven), kunt u de bestandsnaam (s) in de **naam** of **lijst** kenmerk van de **input** -element. Als u niet welke bestand activa moeten worden geïndexeerd opgeeft, wordt het primaire bestand gekozen. Als er geen primaire activa-bestand is ingesteld, wordt het eerste bestand in de invoer activa geïndexeerd.</p><p>Als u wilt de bestandsnaam van de activa expliciet opgeeft, volgt te werk:<br />`<input name="TestFile.wmv">`<br /><br />U kunt ook met de index meerdere activa bestanden in één keer (maximaal 10). Dit wilt doen:<br /><br /><ol class="ordered"><li><p>Een tekstbestand (manifest-bestand) maken en hieraan extensie .lst. </p></li><li><p>Een lijst met alle activa bestandsnamen in uw invoer activa toevoegen aan dit manifest bestand. </p></li><li><p>(Uploaden) thanifest bestand toevoegen aan de activa.  </p></li><li><p>Geef de naam van het manifest bestand in de lijst-kenmerk van de invoer.<br />`<input list="input.lst">`</li></ol><br /><br />Opmerking: Als u meer dan 10 bestanden aan de manifest-bestand toevoegen, de indexing taak mislukt met de foutcode 2006.
__metagegevens__ | ONWAAR | Metagegevens voor de opgegeven activa-bestanden die worden gebruikt voor de aanpassing van de woordenlijst.  Dit is handig voor het voorbereiden van indexering te herkennen niet-standaard woordenlijst woorden zoals eigennamen.<br />`<metadata key="..." value="..."/>` <br /><br />U kunt __waarden__ voor de vooraf gedefinieerde __toetsen__opgeeft. De volgende sleutels zijn momenteel ondersteund:<br /><br />"titel" en "Beschrijving" - gebruikt voor de woordenlijst aanpassing om de taal voor uw project modelleren en nauwkeurigheid van de spraakherkenning te verbeteren.  De waarden vullen zoekacties op Internet om contextueel relevante tekstdocumenten, met de inhoud naar het verbeteren van de interne woordenlijst voor de duur van uw taak Indexing te zoeken.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__functies__ <br /><br /> In versie 1.2 hebt toegevoegd. De enige ondersteunde functie is momenteel spraakherkenning ("ASR").| ONWAAR | De functie Spraakherkenning heeft de volgende instellingen te gebruiken:<table><tr><th><p>Toets</p></th>     <th><p>Beschrijving</p></th><th><p>Van Voorbeeldwaarde</p></th></tr><tr><td><p>Taal</p></td><td><p>De natuurlijke taal worden herkend in het multimediabestand.</p></td><td><p>Engels, Spaans</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>een puntkomma's gescheiden lijst met de gewenste bijschrift uitvoerindelingen (indien aanwezig)</p></td><td><p>ttml; sami; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Een Boole-vlag die aangeeft of een bestand AIB vereist (voor gebruik met SQL Server en de klant indexering IFilter is).  Zie <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Bestanden met behulp van AIB met Azure Media indexering en SQL Server</a>voor meer informatie.</p></td><td><p>ONWAAR zijn. ONWAAR</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Een Boole-vlag die aangeeft of een trefwoord XML-bestand vereist is.</p></td><td><p>ONWAAR zijn. De waarde False. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Een Boole-vlag precisie al dan niet afdwingen volledige bijschriften (ongeacht het betrouwbaarheidsniveau).  </p><p>Standaard is false in dat geval woorden en zinnen die kleiner dan 50% betrouwbaarheid hebt zijn weggelaten uit de uitvoer van het definitieve bijschrift en vervangen door de drie puntjes ('...').  De drie puntjes zijn handig om de kwaliteit van het bijschrift en controle.</p></td><td><p>ONWAAR zijn. De waarde False. </p></td></tr></table>

### <a id="error_codes"></a>Foutcodes

In het geval van een fout, Azure Media indexering moet rapporteren back-een van de volgende foutcodes:

Code | Naam | Mogelijke oorzaken
-----|------|------------------
2000 | Ongeldige configuratie | Ongeldige configuratie
2001 | Ongeldige invoer activa | Er ontbreken invoer activa of lege activa.
2002 | Ongeldig manifest | Manifest is leeg of manifest ongeldige items bevat.
2003 | Kan niet worden gedownload van media-bestand | Ongeldige URL in manifest-bestand.
2004 | Niet-ondersteunde protocol | Protocol van media-URL wordt niet ondersteund.
2005 | Niet-ondersteunde bestandstype | Type invoer mediabestand wordt niet ondersteund.
2006 | Te veel invoer bestanden | Er zijn meer dan 10 bestanden in het invoer manifest.
3000 | Media-bestand is mislukt | Niet-ondersteunde mediacodec <br/>of<br/> Beschadigde media-bestand <br/>of<br/> Geen audiostroom in invoer media.
4000 | Batch indexeren gedeeltelijk is geslaagd | Sommige van de invoer mediabestanden zijn niet moeten worden geïndexeerd. Zie <a href="#output_files">uitvoerbestanden</a>voor meer informatie.
andere | Interne fouten | Neem contact op met het ondersteuningsteam. indexer@microsoft.com


## <a id="supported_languages"></a>Ondersteunde talen

Op dit moment worden de Engels en Spaans talen ondersteund. Zie [het blogbericht van versie 1.2 release](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/)voor meer informatie.


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Verwante koppelingen

[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Gebruik van AIB bestanden met Azure Media indexering en SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Media-bestanden met Azure Media indexering 2 Preview indexeren](media-services-process-content-with-indexer2.md)
