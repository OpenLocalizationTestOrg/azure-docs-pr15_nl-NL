<properties
    pageTitle="Aan de slag met het geven van inhoud op aanvraag met .NET | Azure"
    description="Deze zelfstudie leert u de stappen van de uitvoering van een op aanvraag-toepassing die inhoud leveren met Azure Media Services met behulp van .NET."
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
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Aan de slag met het geven van inhoud op aanvraag met .NET SDK

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F)voor meer informatie. 
 
##<a name="overview"></a>Overzicht 

Deze zelfstudie leert u de stappen van de uitvoering van een Video-on-Demand (VoD) contentlevering-toepassing die via Azure Media Services (AMS) SDK voor .NET.


De zelfstudie maakt u kennis met de eenvoudige werkstroom voor Media Services en de meest voorkomende programming objecten en taken die zijn vereist voor de ontwikkeling van de Media Services. Aan het einde van de zelfstudie, is mogelijk streamen of geleidelijk downloaden van een steekproef media-bestand dat u hebt geüpload, codering en gedownload.

## <a name="what-youll-learn"></a>Wat u leert

De zelfstudie ziet u hoe u de volgende taken uitvoeren:

1.  Maak een Media Services-account (via de portal van Azure).
2.  Streaming eindpunt (via de portal van Azure) configureren.
3.  Maken en configureren van een project Visual Studio.
5.  Verbinding maken met de Media Services-account.
6.  Maak een nieuwe activa en upload een videobestand opnemen.
7.  Coderen het bronbestand in een reeks geavanceerde bitsnelheid MP4-bestanden.
8.  Publiceer de activa en URL's voor streaming en geleidelijke downloaden ophalen.
9.  Test door uw inhoud wordt afgespeeld.

## <a name="prerequisites"></a>Vereisten voor

Het volgende moeten zijn de zelfstudie voltooien.

- Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. 
    
    Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F)voor meer informatie. U krijgt tegoeden die kunnen worden gebruikt om te proberen betaalde Azure Services. Zelfs nadat de tegoeden zijn gebruikt, kunt u deze kunt houden van het account en gebruiken van gratis Azure services en functies, zoals de Web Apps-functie in Azure App-Service.
- Besturingssystemen: Windows 8 of hoger, Windows 2008 R2, Windows 7.
- .NET framework 4.0 of later
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate of Express) of hogere versies.


##<a name="download-sample"></a>Voorbeeld downloaden

Ophalen en uitvoeren van een steekproef vanaf [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Maak een Azure Media Services-account met behulp van de Azure portal

De stappen in deze sectie laten zien hoe een AMS-account maken.

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **+ nieuwe** > **Media + CDN** > **mediaservices**.

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Voer de vereiste waarden in **MEDIA SERVICES-ACCOUNT maken** .

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Voer de naam van de nieuwe AMS-account in het vak **Naam van het Account**. De naam van een Media Services-account is alle kleine letters cijfers of letters zonder spaties, en is 3 tot en met 24 tekens.
    2. Selecteer in abonnement, tussen de verschillende Azure abonnementen waartoe u toegang hebt.
    
    2. Selecteer de nieuwe of bestaande resource in **Resourcegroep**.  Een resourcegroep is een verzameling resources die levenscyclus, machtigingen en beleid delen. Meer informatie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Schakel de geografische regio worden op **locatie**gebruikt voor de opslag van de media en metagegevens records voor uw Media Services-account. Dit gebied wordt gebruikt om te verwerken en media streamen. Alleen de beschikbare Media Services regio's weergegeven in het vak van de vervolgkeuzelijst. 
    
    3. Selecteer in de **Opslag-Account**, een opslag-account om u te bieden-blobopslag van het media-inhoud van uw Media Services-account. U kunt een bestaand account voor de opslag in hetzelfde geografische gebied, als uw account Media Services selecteren of kunt u een opslag-account maken. Een nieuwe opslag-account is gemaakt in dezelfde regio. De regels voor opslag-accountnamen zijn hetzelfde als voor Media Services-accounts.

        Meer informatie over de opslag [hier](storage-introduction.md).

    4. Selecteer **vastmaken aan dashboard** om te zien van de voortgang van de account-implementatie.
    
7. Klik op **maken** onder aan het formulier.

    Zodra het account is gemaakt, verandert de status **uitgevoerd**. 

    ![Media Services-instellingen](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voor het beheren van uw account AMS (bijvoorbeeld, video's uploaden, coderen van activa, taakvoortgang bewaken) Gebruik het venster **Instellingen** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Streaming eindpunten met behulp van de Azure portal configureren

Wanneer u werkt met Azure Media Services een van de meeste gevallen video via geavanceerde bitsnelheid streaming levert aan uw klanten. Media Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders).

Media Services biedt dynamische verpakking, waarmee u uw geavanceerde bitsnelheid codering MP4-inhoud in streaming door Encoder ondersteunde Media Services (MPEG streepje, HLS, vloeiende Streaming, schijven) just-in-time, zonder dat u voor de opslag van vooraf ingepakte versies van elk van deze indelingen streaming hoeft te geven.

Om te profiteren van dynamische verpakking, moet u als volgt te werk:

- Coderen uw mezzanine (bron)-bestand in een reeks geavanceerde bitsnelheid MP4-bestanden (de codering stappen worden verderop in deze zelfstudie gedemonstreerd).  
- Ten minste één streaming eenheid voor het *eindpunt streaming* waaruit u wilt gaan bezorging van uw inhoud maken. De onderstaande stappen uitgelegd hoe u het aantal streaming eenheden wijzigt.

Met dynamische verpakking, hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services genereert en het juiste antwoord op basis van aanvragen van een client fungeert.

Ga als volgt te werk als u wilt maken en het aantal streaming gereserveerde eenheden wijzigt:


1. Klik in het venster **Instellingen** op **Streaming eindpunten**. 

2. Klik op de standaard streaming eindpunt. 

    Het venster **Standaard STREAMING EINDPUNTDETAILS** wordt geopend.

3. Als u wilt opgeven van het aantal streaming eenheden, schuift u de schuifregelaar **Streaming eenheden** .

    ![Streaming eenheden](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klik op de knop **Opslaan** als uw wijzigingen wilt opslaan.

    >[AZURE.NOTE]De toewijzing van een nieuwe eenheden kan maximaal 20 minuten duren.

##<a name="create-and-configure-a-visual-studio-project"></a>Maken en configureren van een project Visual Studio

1. Maak een nieuwe C#-Console-toepassing in Visual Studio 2013, Visual Studio 2012 of Visual Studio 2010 SP1. Voer de **naam**, **locatie**en **naam van de oplossing**en klik vervolgens op **OK**.

2. Gebruik het [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) NuGet pakket **Azure Media Services .NET SDK extensies**installeren.  De Media Services .NET SDK-extensies is een verzameling extensie methoden en Help-functies die uw code te vereenvoudigen en kunt u eenvoudiger ontwikkelen met Media-Services. In dit pakket installeert, ook **Media Services.NET SDK** is geïnstalleerd en alle andere vereiste afhankelijkheden toegevoegd.

3. Een verwijzing naar System.Configuration constructie toevoegen. Deze constructie bevat de **System.Configuration.ConfigurationManager** klasse die wordt gebruikt voor toegang tot configuratiebestanden, bijvoorbeeld App.config.

4. Open het bestand App.config (het bestand toevoegen aan uw project als deze niet al dan niet standaard is toegevoegd) en een sectie *appSettings* toevoegen aan het bestand. Stel de waarden voor uw Azure Media Services naam en account accountsleutel, zoals wordt weergegeven in het volgende voorbeeld. De naam van het account en belangrijke informatie downloaden, gaat u naar de [Azure-portal](https://portal.azure.com/) en selecteer uw account AMS. Selecteer **Instellingen** > **toetsen**. De toetsen beheren wordt de naam van het account en de primaire en secundaire sleutels wordt weergegeven.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. De bestaande **gebruiken** beweringen aan het begin van het bestand Program.cs overschrijven met de volgende programmacode.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Een nieuwe map onder de map projecten maken en kopieer een .mp4- of WMV-bestand dat u wilt versleutelen en streamen of geleidelijk downloaden. In dit voorbeeld is het pad 'C:\VideoFiles' wordt gebruikt.

##<a name="connect-to-the-media-services-account"></a>Verbinding maken met de Media Services-account

Wanneer u Media Services met .NET, moet u de klas **CloudMediaContext** gebruiken voor de meeste Media Services programming taken: verbinding maken met Media Services-account; maken, bijwerken, openen en verwijderen van de volgende objecten: activa, activa bestanden, taken, clienttoegangsbeleid, Locator, enzovoort.

De standaard-programma klasse overschrijven met de volgende code. De code ziet u hoe u de waarden van de verbinding uit het bestand App.config lezen en hoe de **CloudMediaContext** -object maken om te verbinden met Media-Services. Zie [verbinding maken met Media-Services met de Media Services SDK voor .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx)voor meer informatie over het verbinding maken met Media-Services.

De functie **Hoofdgegeven** roept methoden die worden gedefinieerd verdere in deze sectie.

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
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Maak een nieuwe activa en uploadt u een videobestand opnemen

In Media Services u uploaden (of nemen) de digitale bestanden naar een actief. De **activa** entity kunt video, audio, afbeeldingen, miniaturen verzamelingen, tekst sporen, en bevatten ondertiteling bestanden (en de metagegevens over deze bestanden.)  Nadat de bestanden die zijn geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. De bestanden in de activa worden **Activa bestanden**genoemd.

De methode **UploadFile** is gedefinieerd onder oproepen **CreateFromFile** (gedefinieerd in .NET SDK extensies). **CreateFromFile** Hiermee maakt u een nieuw activum waarin het opgegeven bestand wordt geüpload.

De methode **CreateFromFile** kost **AssetCreationOptions** waarmee u een van de volgende opties voor het maken van de activa opgeven:

- **Geen** : geen versleuteling wordt gebruikt. Dit is de standaardwaarde. Houd er rekening mee dat wanneer deze optie gebruikt, de inhoud niet is beveiligd tijdens overdracht of op rest in opslag.
Als u van plan bent om uit te voeren een MP4 geleidelijke downloaden, gebruikt u deze optie gebruikt.
- **StorageEncrypted** - Gebruik deze optie als u wilt versleutelen wissen inhoud lokaal met geavanceerde versleuteling Standard (AES)-256 bits codering, waardoor vervolgens geüpload naar Azure Storage waar deze zijn opgeslagen in rust versleuteld. Activa die zijn beveiligd met opslag versleuteling zijn automatisch niet versleuteld en geplaatst in een versleutelde bestandssysteem vóór codering en desgewenst opnieuw worden versleuteld voordat u het uploadt weer als een nieuwe uitvoer actief. De primaire use-case voor opslag-versleuteling is wanneer u wilt beveiligen van uw kwalitatief hoogwaardig invoer mediabestanden met codering in rust op schijf.
- **CommonEncryptionProtected** - Gebruik deze optie als u bezig bent met het uploaden van inhoud die al is versleuteld en beveiligd met algemene versleuteling of PlayReady DRM (bijvoorbeeld vloeiende Streaming beveiligd met DRM PlayReady).
- **EnvelopeEncryptionProtected** – Gebruik deze optie als u bezig bent met het uploaden van HLS versleuteld met AES. Houd er rekening mee dat de bestanden moeten codering en versleuteld door Manager transformeren.

De methode **CreateFromFile** kunt u ook opgeven een terugbellen om de voortgang van het uploaden van het bestand rapporteren.

Geef in het volgende voorbeeld wordt we op **geen** voor de activa-opties.

De volgende methode toevoegen aan de klas programma.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Coderen van het bronbestand in een reeks geavanceerde bitsnelheid MP4-bestanden

Na het ingesting activa in Media Services, media kunt worden gecodeerd, transmuxed, een watermerk, enzovoort, voordat deze wordt geleverd met clients. Deze activiteiten zijn gepland en uitvoeren op meerdere exemplaren van achtergrond rol om ervoor te zorgen optimale prestaties en beschikbaarheid. Deze activiteiten taken worden genoemd, en elke taak bestaat uit atomaire taken die de werkelijke hoeveelheid werk aan het bestand activa uitvoeren.

Zoals is gezegd, wanneer u werkt met Azure Media Services, levert een van de meeste gevallen geavanceerde bitsnelheid streaming naar uw klanten. Media-Services kunt dynamisch Inpakken voor een reeks geavanceerde bitsnelheid MP4-bestanden in een van de volgende indelingen: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders).

Om te profiteren van dynamische verpakking, moet u als volgt te werk:

- Coderen of transcoderen uw mezzanine (bron) opbergen in een reeks geavanceerde bitsnelheid MP4-bestanden of geavanceerde bitsnelheid vloeiende Streaming bestanden.  
- Ten minste één streaming eenheid voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud downloaden.

De volgende code ziet hoe u een codering-taak. De taak bevat één taak waarmee wordt opgegeven te transcoderen het mezzanine-bestand in een reeks geavanceerde bitsnelheid MP4s met **Media Encoder standaard**. De code dient van de taak en wacht totdat deze is voltooid.

Wanneer de taak is voltooid, kunt u zou kunnen uw activa streamen of MP4-bestanden die zijn gemaakt als gevolg van transcodering geleidelijk te downloaden.
De notitie die u niet moet hebt meer dan 0 streaming eenheden geleidelijk MP4-bestanden te downloaden.

De volgende methode toevoegen aan de klas programma.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Publiceren van de activa en kennis van URL's voor streaming en geleidelijke downloaden

Om te streamen of downloaden van activa, moet u eerst "publiceert' door te maken van een locator. Locator bieden toegang tot bestanden in de activa. Media Services ondersteunt twee soorten Locator: OnDemandOrigin Locator, gebruikt om te stream media (bijvoorbeeld MPEG streepje, HLS of vloeiende Streaming) en Locator Access handtekening (SA's), gebruikt om te downloaden van media-bestanden.

Nadat u de Locator hebt gemaakt, kunt u de URL's die worden gebruikt om te streamen of downloaden van uw bestanden kunt maken.


De URL van een streaming voor vloeiende Streaming heeft de volgende indeling:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

De URL van een streaming voor HLS heeft de volgende indeling:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

De URL van een streaming voor MPEG streepje heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

De URL van een SA's gebruikt om te downloaden van bestanden heeft de volgende indeling:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Media Services .NET SDK extensies bieden handige methoden waarmee opgemaakte URL's voor de gepubliceerde activa wordt geretourneerd.

De volgende code wordt gebruikt voor .NET SDK Extensions Locator maken en krijgen streaming en URL's geleidelijke downloaden. De code wordt ook het downloaden van bestanden naar een lokale map weergegeven.

De volgende methode toevoegen aan de klas programma.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>Test door uw inhoud wordt afgespeeld  

Als u het programma gedefinieerd in de vorige sectie hebt uitgevoerd, wordt de URL's die de volgende strekking weergegeven in het consolevenster.

Geavanceerde streaming URL's:

Vloeiende Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG-STREEPJE

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Geleidelijke downloaden URL's (audio en video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Gebruiken om te stromen u video, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Als u wilt testen geleidelijke downloaden, door een URL in een browser (bijvoorbeeld Internet Explorer, Chrome of Safari) te plakken.


##<a name="next-steps-media-services-learning-paths"></a>Volgende stappen: Mediaservices leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Zoekt u iets anders?

Als dit onderwerp niet bevatten wat u verwacht, ontbreekt er iets of in een andere manier niet aan uw wensen voldoet, geef ons uw feedback met de onderstaande Disqus-thread.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/
