<properties
    pageTitle="Met dynamische AES-128-versleuteling en belangrijke bezorgingsservice | Microsoft Azure"
    description="Microsoft Azure Media Services kunt u de inhoud die zijn versleuteld met AES 128-bit versleuteling toetsen bezorgen. Media Services biedt ook de sleutel bezorgingsservice die versleuteling toetsen tot gemachtigde gebruikers biedt. Dit onderwerp wordt uitgelegd hoe u dynamisch versleutelen met AES-128 en gebruiken van de belangrijkste bezorgingsservice."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/24/2016"
    ms.author="juliako"/>

#<a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Dynamische AES-128-versleuteling en belangrijke bezorgingsservice gebruiken

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-aes128.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Overzicht

>[AZURE.NOTE] Zie [deze](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video voor een overzicht van het beveiligen van uw Media-inhoud met AES-versleuteling gebruikt.

Microsoft Azure Media Services kunt u voor het leveren van Http-Live-Streaming (HLS) en vloeiende Streams versleuteld met geavanceerde versleuteling Standard (AES) (via 128-bit versleuteling toetsen). Media Services biedt ook de sleutel bezorgingsservice die versleuteling toetsen tot gemachtigde gebruikers biedt. Als u voor Media Services wilt voor het coderen van activa, moet u een versleutelingssleutel koppelen aan het activum en ook configureren autorisatie beleidsregels voor de sleutel. Een stroom door een speler is aangevraagd, gebruikt Media Services de opgegeven sleutel dynamisch versleutelen uw inhoud AES-versleuteling gebruikt. Als u wilt versleutelen de stream, wordt de speler de toets vraag aan de belangrijkste bezorgingsservice. Om te bepalen of de gebruiker is gemachtigd om de toets, evalueert de service de autorisatie beleidsregels die u hebt opgegeven voor de sleutel.

Media Services ondersteunt meerdere manieren gebruikers die belangrijke aanvragen te verifiëren. Het autorisatiebeleid belangrijke voor inhoud kan zijn een of meer autorisatie-beperkingen: openen of token beperking. Het beleid voor token beperkte vergezeld van een token dat is uitgegeven door een Secure Token Service (STS). Media Services ondersteunt tokens in de [Eenvoudige Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) en [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-indeling. Zie [beleid van de inhoud sleutel configureren](media-services-protect-with-aes128.md#configure_key_auth_policy)voor meer informatie.

Om te profiteren van dynamische versleuteling, moet u beschikken over een actief dat met een verzameling multi-bitrate MP4-bestanden of bronbestanden voor multi-bitrate vloeiende Streaming. U moet ook de bezorging van beleid configureren voor de activa (Zie verderop in dit onderwerp). Klik op basis van de notatie die is opgegeven in de streaming URL, de server op aanvraag Streaming zorgt ervoor dat dat de stream wordt bezorgd in het protocol dat u hebt gekozen. Hierdoor, hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services-service wordt bouwen en het juiste antwoord op basis van aanvragen van een client fungeren.

In dit onderwerp zou nuttige informatie voor ontwikkelaars die bezig bent met toepassingen die beveiligde media leveren. Het onderwerp ziet u hoe u de belangrijkste bezorgingsservice met autorisatie beleidsregels zodanig configureren dat alleen geautoriseerde clients de toetsen versleuteling kunnen ontvangen. Ook ziet u hoe dynamische versleuteling.

>[AZURE.NOTE]Als u wilt beginnen met dynamische codering, moet u eerst ten minste één eenheid voor tijdschaal (ook wel bekend als streaming eenheid) ophalen. Zie [hoe u de schaal van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

##<a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Dynamische AES-128-versleuteling en de bezorging van belangrijke servicewerkstroom

Hier volgen algemene stappen die u uitvoeren moet wanneer uw activa, met AES, met de belangrijkste bezorgingsservice Media Services en ook met dynamische versleuteling coderen.

1. [Activa maken en uploaden van bestanden naar de activa](media-services-protect-with-aes128.md#create_asset).
1. [Coderen van de activa met het bestand naar de geavanceerde bitsnelheid die MP4 instellen](media-services-protect-with-aes128.md#encode_asset).
1. [Een inhoud sleutel maken en koppelen aan het gecodeerde activum](media-services-protect-with-aes128.md#create_contentkey). In een Media-Services bevat de inhoud toets van het actief versleutelingssleutel.
1. [Beleid van de inhoud sleutel configureren](media-services-protect-with-aes128.md#configure_key_auth_policy). Het autorisatiebeleid inhoud belangrijke moet worden geconfigureerd door u en voldaan door de client in volgorde voor de inhoud sleutel is bezorgd op de client.
1. [Het beleid bezorging voor activa configureren](media-services-protect-with-aes128.md#configure_asset_delivery_policy). De bezorging van beleidsconfiguratie omvat: belangrijke acquisition URL en initialisatie Vector (IV) (AES 128 hiervoor de dezelfde IV moeten worden verstrekt versleutelen en ontsleutelen), bezorging protocol (bijvoorbeeld, MPEG-streepje HLS, schijven, vloeiende Streaming of alle), het type dynamische codering (bijvoorbeeld envelop of geen dynamische codering).

U kunt verschillende beleid toepassen voor elk protocol dat op dezelfde activa. U kunt bijvoorbeeld PlayReady versleuteling toepassen op vloeiend/streepje en AES envelop naar HLS. Geen protocollen die niet zijn gedefinieerd in een bezorging beleid (bijvoorbeeld u één beleid toevoegen die alleen HLS als het protocol) uit streaming worden geblokkeerd. De uitzondering hierop is als u geen activa bezorging beleid helemaal gedefinieerd. Vervolgens zijn alle protocollen toegestaan in de wissen.

1. [Een locator OnDemand maken](media-services-protect-with-aes128.md#create_locator) om te krijgen van de URL van een streaming.

Het onderwerp wordt ook weergegeven [hoe een clienttoepassing een sleutel van de belangrijkste bezorgingsservice kunt aanvragen](media-services-protect-with-aes128.md#client_request).

U vindt u een volledig .NET- [voorbeeld](media-services-protect-with-aes128.md#example) aan het einde van het onderwerp.

De volgende afbeelding ziet u de werkstroom die hierboven is beschreven. Hier wordt de token gebruikt voor verificatie.

![Beveiligen met AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

De rest van dit onderwerp vindt u gedetailleerde uitleg, codevoorbeelden en koppelingen naar onderwerpen waarin u kunt hoe zien u de taken die hierboven beschreven bereiken.

##<a name="current-limitations"></a>Huidige beperkingen

Als u toevoegen of van uw actief bezorging beleid bijwerken, moet u een bestaande locator verwijderen (indien aanwezig) en een nieuwe locator maken.

##<a id="create_asset"></a>Activa maken en uploaden van bestanden in de activa

Om te kunnen beheren, coderen en streamen van uw video's, moet u eerst uw inhoud uploaden naar Microsoft Azure Media Services. Zodra u hebt geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor nadere verwerking en streaming. 

Zie [Bestanden uploaden naar een Media Services-account](media-services-dotnet-upload-files.md)voor gedetailleerde informatie.

##<a id="encode_asset"></a>Coderen van de activa met het bestand naar de geavanceerde bitsnelheid die MP4 instellen

Met dynamische versleuteling is u hoeft te maken van activa met een verzameling multi-bitrate MP4-bestanden of bronbestanden voor multi-bitrate vloeiende Streaming. Klik op basis van de opgegeven notatie in de uitnodiging manifest of fragment, het op aanvraag Streaming server zorgt ervoor dat u de stream ontvangt in het protocol dat u hebt gekozen. Hierdoor, hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services-service wordt bouwen en het juiste antwoord op basis van aanvragen van een client fungeren. Zie het onderwerp [Dynamisch verpakking overzicht](media-services-dynamic-packaging-overview.md) voor meer informatie.

Voor instructies voor het coderen, Zie [hoe u het coderen van activa met Media Encoder standaard](media-services-dotnet-encode-with-media-encoder-standard.md).

##<a id="create_contentkey"></a>Een inhoud sleutel maakt en te koppelen aan de gecodeerd activa

De inhoud toets bevat in Media-Services, de sleutel die u wilt versleutelen activa met.

Zie [inhoud sleutel maken](media-services-dotnet-create-contentkey.md)voor gedetailleerde informatie.

##<a id="configure_key_auth_policy"></a>Beleid van de inhoud sleutel configureren

Media Services ondersteunt meerdere manieren gebruikers die belangrijke aanvragen te verifiëren. Het autorisatiebeleid inhoud belangrijke moet worden geconfigureerd door u en voldaan door de client (player) in de volgorde voor de sleutel is bezorgd op de client. Het autorisatiebeleid belangrijke voor inhoud kan zijn een of meer autorisatie-beperkingen: open, token beperking of IP-beperking.

Zie [Inhoudbeleid voor sleutel-autorisatie configureren](media-services-dotnet-configure-content-key-auth-policy.md)voor gedetailleerde informatie.

##<a id="configure_asset_delivery_policy"></a>Activa bezorging beleid configureren 

Het beleid bezorging voor uw activa configureren. Enkele zaken die de configuratie van activa bezorging beleid omvat:

- De URL van de sleutel acquisition. 
- De initialisatie Vector (IV) voor de envelop-versleuteling wilt gebruiken. AES 128 is vereist voor de dezelfde IV moeten worden verstrekt versleutelen en ontsleutelen. 
- De activa bezorging protocol (bijvoorbeeld MPEG streepje, HLS, schijven, vloeiende Streaming of alle).
- Het type dynamische codering (bijvoorbeeld AES envelop) of geen dynamische codering. 

Zie [configureren activa bezorging beleid ](media-services-rest-configure-asset-delivery-policy.md)voor gedetailleerde informatie.

##<a id="create_locator"></a>Een OnDemand locator streaming om te krijgen van de URL van een streaming maken

U moet de gebruiker de streaming URL bieden voor vloeiende, streepje of HLS.

>[AZURE.NOTE]Als u toevoegen of van uw actief bezorging beleid bijwerken, moet u een bestaande locator verwijderen (indien aanwezig) en een nieuwe locator maken.

Zie [de URL van een streaming maken](media-services-deliver-streaming-content.md)voor instructies over het publiceren van activa en de URL van een streaming maken.

##<a name="get-a-test-token"></a>Een test token ophalen

Krijg een toets token op basis van een beperking voor de token dat is gebruikt voor het autorisatiebeleid belangrijke.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

U kunt de [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) gebruiken om te testen van de stream.

##<a id="client_request"></a>Hoe kan de klant een sleutel aanvragen van de belangrijkste bezorgingsservice?

In de vorige stap samengesteld hebt de URL die naar een manifest-bestand verwijst. De klant moet de benodigde gegevens ophalen uit de streaming manifest-bestanden om te maken van een verzoek voor de belangrijkste bezorgingsservice.

###<a name="manifest-files"></a>Manifest-bestanden

De client moet extraheren van de URL (die ook inhoud sleutel Id (kind) bevat) de waarde van het manifest bestand. De client vervolgens probeert te krijgen van de sleutel van de belangrijkste bezorgingsservice. De client moet ook om de waarde van IV en gebruik deze de stream ontsleutelen te halen. De volgende ziet u het codefragment van de <Protection> element van het manifest vloeiende Streaming.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

In het geval van HLS, is het manifest hoofdsite onderverdeeld in segment bestanden. 

De hoofdmap manifest is bijvoorbeeld: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) en bevat een lijst met bestandsnamen segment.
    
    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Als u een van de bestanden segment in teksteditor openen (bijvoorbeeld http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), moet deze bevatten #externe-X-toets, waarmee wordt aangegeven dat het bestand is versleuteld.
    
    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST
    
###<a name="request-the-key-from-the-key-delivery-service"></a>Vraag de sleutel van de belangrijkste bezorgingsservice

De volgende code ziet u hoe u een aanvraag verzendt de Media Services belangrijke bezorging-service met behulp van een belangrijke levering Uri (die is opgehaald uit het manifest) en een token (in dit onderwerp praat niet over het ophalen van eenvoudige Web Tokens van een Secure Token-Service).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);
                
        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;
    
        var response = request.GetResponse();
     
        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }
    
        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();
    
        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
    
##<a id="example"></a>Voorbeeld

1. Maak een nieuwe Console-project.
1. Gebruik NuGet installeert en Azure Media Services .NET SDK extensies toevoegen. In dit pakket installeert, ook Media Services .NET SDK is geïnstalleerd en alle andere vereiste afhankelijkheden toegevoegd.
2. Configuratiebestand met de naam van het account en belangrijke informatie toevoegen:

    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. De code in het bestand Program.cs overschrijven met de code die in deze sectie worden weergegeven.
    
    Zorg ervoor dat bijwerken van variabelen zodat deze verwijzen naar mappen waarin uw invoer bestanden opgeslagen worden.
            
        
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
        
        namespace AESDynamicEncryptionAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // A Uri describing the issuer of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                // The Audience or Scope of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
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
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine();
        
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the bit.ly/aesplayer Flash player to test the URL 
                    // (with open authorization policy). 
                    // Paste the URL and click the Update button to play the video. 
                    //
                    string URL = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
                    Console.WriteLine();
                    Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
                    Console.WriteLine();
        
                    Console.ReadLine();
                }
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
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
                        AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
                {
                    // Create envelope encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.EnvelopeEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy             
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("Open Authorization Policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                        new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "HLS Open Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null // no requirements needed for HLS
                                };
        
                    restrictions.Add(restriction);
        
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                        "policy",
                        ContentKeyDeliveryType.BaselineHttp,
                        restrictions,
                        "");
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("HLS token restricted authorization policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                            new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                            new ContentKeyAuthorizationPolicyRestriction
                            {
                                Name = "Token Authorization Policy",
                                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                                Requirements = tokenTemplateString
                            };
        
                    restrictions.Add(restriction);
        
                    //You could have multiple options 
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                            "Token option for HLS",
                            ContentKeyDeliveryType.BaselineHttp,
                            restrictions,
                            null  // no key delivery data is needed for HLS
                            );
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
        
                    return tokenTemplateString;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        
                    string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));
            
                    // When configuring delivery policy, you can choose to associate it
                    // with a key acquisition URL that has a KID appended or
                    // or a key acquisition URL that does not have a KID appended  
                    // in which case a content key can be reused. 

                    // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
                    // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

                    // The following policy configuration specifies: 
                    // key url that will have KID=<Guid> appended to the envelope and
                    // the Initialization Vector (IV) to use for the envelope encryption.
                    
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                    {
                                {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
                    };
        
                    IAssetDeliveryPolicy assetDeliveryPolicy =
                        _context.AssetDeliveryPolicies.Create(
                                    "AssetDeliveryPolicy",
                                    AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                                    AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                                    assetDeliveryPolicyConfiguration);
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
                    Console.WriteLine();
                    Console.WriteLine("Adding Asset Delivery Policy: " +
                        assetDeliveryPolicy.AssetDeliveryPolicyType);
                }
        
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                EndsWith(".ism")).
                                                FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private string GenerateTokenRequirements()
                {
                    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
        
                    template.PrimaryVerificationKey = new SymmetricVerificationKey();
                    template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
                    template.Audience = _sampleAudience.ToString();
                    template.Issuer = _sampleIssuer.ToString();
        
                    template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
        
                    return TokenRestrictionTemplateSerializer.Serialize(template);
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int size)
                {
                    byte[] randomBytes = new byte[size];
                    using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(randomBytes);
                    }
        
                    return randomBytes;
                }
            }
        }


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
