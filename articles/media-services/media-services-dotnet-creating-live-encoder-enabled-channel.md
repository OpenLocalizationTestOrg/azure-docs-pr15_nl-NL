<properties 
    pageTitle="Het uitvoeren van live streaming Azure Media Services gebruiken om te maken van multi-bitrate streams met .NET | Microsoft Azure" 
    description="Deze zelfstudie leert u de stappen voor het maken van een kanaal bevindt waarvoor een live met één bitsnelheid ontvangt en deze op multi-bitsnelheid met .NET SDK gecodeerd." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Het uitvoeren van live streaming Azure Media Services gebruiken om te maken van multi-bitrate streams met .NET

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F)voor meer informatie.

##<a name="overview"></a>Overzicht

Deze zelfstudie leert u de stappen voor het maken van een **kanaal** bevindt waarvoor een live met één bitsnelheid ontvangt en deze op multi-bitsnelheid gecodeerd.

Zie [Live streaming met behulp van Azure Media Services als u wilt multi-bitrate streams maken](media-services-manage-live-encoder-enabled-channels.md)voor meer algemene informatie die betrekking hebben op kanalen die zijn ingeschakeld voor het coderen van live.


##<a name="common-live-streaming-scenario"></a>Live Streaming gebruikelijk

De volgende stappen beschrijven taken die nodig zijn bij het maken van algemene live streaming toepassingen.

>[AZURE.NOTE] De maximale aanbevolen duur van een live-gebeurtenis is momenteel 8 uur. Neem contact op met amslived op Microsoft.com als voor het uitvoeren van een kanaal voor langere periode.

1. Een videocamera verbinden met een computer. Starten en configureren van een on-premises implementatie live encoder die een één bitsnelheid in een van de volgende protocollen kunt uitvoeren: RTMP, vloeiende Streaming of RTP (MPEG-TS). Zie [Azure Media Services RTMP ondersteuning en Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824)voor meer informatie.

Deze stap kan ook worden uitgevoerd nadat u uw kanaal hebt gemaakt.

1. Maken en een kanaal starten.

1. Ophalen het kanaal nemen URL.

De URL ingest wordt gebruikt door de live encoder de stream versturen naar het kanaal.

1. Hiermee kunt u de URL van de Preview-versie kanaal ophalen.

Gebruik deze URL om te controleren of uw kanaal is juist de live gegevensstroom ontvangt.

2. Maak een actief.
3. Als u voor dynamisch worden versleuteld tijdens het afspelen van de activa wilt, als volgt:
1. Een inhoud sleutel maakt.
1. Beleid van de inhoud sleutel configureren.
1. Configureer activa bezorging beleid (gebruikt door dynamische verpakking en dynamische versleuteling).
3. Maak een programma en opgeven voor het gebruik van de activa die u hebt gemaakt.
1. De activa die is gekoppeld aan het programma door te maken van een locator OnDemand publiceren.

Zorg ervoor dat ten minste één streaming gereserveerde eenheid op de streaming eindpunt waaruit u inhoud wilt hebben.

1. Het programma te starten wanneer u klaar bent voor het starten van streaming en archiveren.
2. (Optioneel) het live encoder kunt signaal ontvangen een advertentie starten. De advertentie wordt ingevoegd in de uitvoer-stream.
1. Het programma stoppen wanneer u wilt stoppen streaming en archiveren van de gebeurtenis.
1. Verwijder het programma (en (optioneel) Verwijder de activa).

## <a name="what-youll-learn"></a>Wat u leert

In dit onderwerp ziet u hoe u andere bewerkingen op kanalen en programma's met behulp van Media Services .NET SDK uitvoeren. Omdat veel bewerkingen zijn langdurige .NET-API's die beheren langdurige worden bewerkingen gebruikt.

Het onderwerp wordt uitgelegd hoe u als volgt te werk:

1. Maken en een kanaal starten. Langdurige API's worden gebruikt.
1. Krijgen de kanalen nemen (input) eindpunt. Dit eindpunt moet worden gedaan naar het encoder die een live één bitsnelheid kunt verzenden.
1. Krijgen de preview-eindpunt. Dit eindpunt wordt gebruikt om een voorbeeld van de stream te.
1. Activa die wordt gebruikt voor de opslag van uw inhoud maken. Het beleid van de bezorging van activa moeten worden geconfigureerd, zoals in dit voorbeeld.
1. Een programma maken en geef de activa die eerder is gemaakt, worden gebruikt. Het programma te starten. Langdurige API's worden gebruikt.
1. Maak een locator voor de activa, zodat de inhoud wordt gepubliceerd en kan worden streamen naar uw klanten.
1. Weergeven en verbergen van leien. Starten en stoppen advertenties. Langdurige API's worden gebruikt.
1. Uw kanaal en alle bijbehorende hulpbronnen opschonen.


##<a name="prerequisites"></a>Vereisten voor

Het volgende moeten zijn de zelfstudie voltooien.

- Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account.

Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F)voor meer informatie. U krijgt tegoeden die kunnen worden gebruikt om te proberen betaalde Azure Services. Zelfs nadat de tegoeden zijn gebruikt, kunt u deze kunt houden van het account en gebruiken van gratis Azure services en functies, zoals de Web Apps-functie in Azure App-Service.
- Een Media Services-account. Als u wilt een Media Services-account hebt gemaakt, Zie [Account maken](media-services-portal-create-account.md).
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate of Express) of hogere versies.
- Moet u Media Services .NET SDK versie 3.2.0.0 of hoger.
- Een webcam en een encoder die een live één bitsnelheid kunt verzenden.

##<a name="considerations"></a>Overwegingen

- De maximale aanbevolen duur van een live-gebeurtenis is momenteel 8 uur. Neem contact op met amslived op Microsoft.com als voor het uitvoeren van een kanaal voor langere periode.
- Zorg ervoor dat ten minste één streaming gereserveerde eenheid op de streaming eindpunt waaruit u inhoud wilt hebben.

##<a name="download-sample"></a>Voorbeeld downloaden

Ophalen en uitvoeren van een steekproef vanaf [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>Instellen voor de ontwikkeling met Media Services SDK voor .NET

1. Een consoletoepassing gebruik van Visual Studio maken.
1. De Media Services SDK voor .NET toevoegen aan uw consoletoepassing met Media Services NuGet-pakket.

##<a name="connect-to-media-services"></a>Verbinding maken met mediaservices
Als een goede gewoonte, moet u een bestand app.config gebruiken voor de opslag van de Media Services naam en account-toets.

>[AZURE.NOTE]U vindt de naam en de toets waarden, gaat u naar de Azure-portal en selecteer uw account. Het venster instellingen wordt weergegeven aan de rechterkant. Selecteer in het venster instellingen van toetsen. De waarde te klikken op het pictogram naast elk tekstvak worden gekopieerd naar het systeemklembord.

De sectie appSettings toevoegen aan het bestand app.config en de waarden voor uw naam en account-toets voor de account voor Media Services instellen.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Codevoorbeeld

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
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
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Volgende stap

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Zoekt u iets anders?

Als dit onderwerp niet bevatten wat u verwacht, ontbreekt er iets of in een andere manier niet aan uw wensen voldoet, geef ons met u feedback met de onderstaande Disqus-thread.
