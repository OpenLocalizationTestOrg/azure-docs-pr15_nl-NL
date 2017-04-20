<properties
    pageTitle="Met PlayReady en/of Widevine dynamische algemene codering | Microsoft Azure"
    description="Microsoft Azure Media Services kunt u voor het leveren van de MPEG-streepje, vloeiende Streaming en Http-Live-Streaming (HLS) streams die zijn beveiligd met Microsoft PlayReady DRM. Bovendien kunt u naar bezorging streepje versleuteld met Widevine DRM. Dit onderwerp wordt uitgelegd hoe u dynamisch versleutelen met PlayReady en Widevine DRM."
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
    ms.topic="get-started-article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>


#<a name="using-playready-andor-widevine-dynamic-common-encryption"></a>PlayReady en/of Widevine dynamische algemene-versleuteling gebruikt

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

Microsoft Azure Media Services kunt u voor het leveren van de MPEG-streepje, vloeiende Streaming en HTTP-Live-Streaming (HLS) streams die zijn beveiligd met [DRM van Microsoft PlayReady](https://www.microsoft.com/playready/overview/). Bovendien kunt u voor het leveren van gecodeerde streepje stream met Widevine DRM licenties. PlayReady zowel Widevine zijn versleuteld per specificatie van de algemene versleuteling (CENC ISO/IEC 23001 tot en met 7). U kunt [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (beginnend met de versie 3.5.1) of REST API gebruiken voor het configureren van uw AssetDeliveryConfiguration als u wilt gebruiken, Widevine.

Media Services biedt een service voor het bezorgen van PlayReady en Widevine DRM licenties. Media Services biedt ook API's die u configureren rechten en beperkingen die u voor PlayReady of Widevine DRM runtime afdwingen kunt wilt wanneer een gebruiker wordt afgespeeld beveiligde inhoud. Wanneer een gebruiker de inhoud van een beveiligd DRM aanvraagt, wordt de toepassing player een licentie van de service van de licentie AMS aanvragen. De service van de licentie AMS verleent een licentie aan de speler of deze is gemachtigd. Een licentie PlayReady of Widevine bevat de decoderingssleutel die kan worden gebruikt door de client-speler u ontsleutelt en de inhoud te streamen.


U kunt ook de volgende AMS partners gebruiken om te leveren Widevine licenties: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Zie voor meer informatie: integratie met [Axinom](media-services-axinom-integration.md) en [castLabs](media-services-castlabs-integration.md).

Media Services ondersteunt meerdere manieren machtigen gebruikers die een belangrijke aanvragen maken. Het autorisatiebeleid belangrijke voor inhoud kan zijn een of meer autorisatie-beperkingen: openen of token beperking. Het beleid voor token beperkte vergezeld van een token dat is uitgegeven door een Secure Token Service (STS). Media Services ondersteunt tokens in de [Eenvoudige Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) en [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-indeling. Zie voor meer informatie van de inhoud sleutel autorisatiebeleid configureren.

Om te profiteren van dynamische versleuteling, moet u beschikken over een actief dat met een verzameling multi-bitrate MP4-bestanden of bronbestanden voor multi-bitrate vloeiende Streaming. Moet u ook de bezorging van beleidsregels opgeven voor de activa (Zie verderop in dit onderwerp). Klik op basis van de notatie die is opgegeven in de streaming URL, de server op aanvraag Streaming zorgt ervoor dat dat de stream wordt bezorgd in het protocol dat u hebt gekozen. Hierdoor hoeft u alleen te slaan en te betalen voor de bestanden in een enkel opslagindeling en Media Services wordt bouwen en het juiste HTTP-antwoord op basis van elk verzoek om van een client fungeren.

In dit onderwerp zou nuttige informatie voor ontwikkelaars die bezig bent met toepassingen die media die zijn beveiligd met meerdere DRMs, zoals PlayReady en Widevine leveren. Het onderwerp ziet u hoe u de bezorgingsservice PlayReady licentie met autorisatie beleidsregels zodanig configureren dat alleen geautoriseerde clients PlayReady of Widevine licenties kunnen ontvangen. Ook ziet u hoe u dynamische codering codering met PlayReady of Widevine DRM via streepje.

>[AZURE.NOTE]Als u wilt beginnen met dynamische codering, moet u eerst ten minste één eenheid voor tijdschaal (ook wel bekend als streaming eenheid) ophalen. Zie [hoe u de schaal van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.


##<a name="download-sample"></a>Voorbeeld downloaden

U kunt de steekproef beschreven in dit artikel uit [hier](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm)downloaden.

##<a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Dynamische algemene versleuteling en DRM licentie bezorging Services configureren

Hier volgen algemene stappen die u uitvoeren moet tijdens het beveiligen van uw activa, met PlayReady, gebruikt de bezorgingsservice van de Media Services-licentie en ook dynamische versleuteling gebruikt.

1. Maak activa en bestanden uploaden naar de activa.
1. Coderen van de activa met het bestand naar de geavanceerde bitsnelheid die MP4 instellen.
1. Een inhoud sleutel maakt en deze koppelen aan het gecodeerde activum. In een Media-Services bevat de inhoud toets van het actief versleutelingssleutel.
1. Beleid van de inhoud sleutel configureren. Het autorisatiebeleid inhoud belangrijke moet worden geconfigureerd door u en voldaan door de client in volgorde voor de inhoud sleutel is bezorgd op de client.

Wanneer u het autorisatiebeleid inhoud belangrijke maakt, moet u het volgende opgeven: methode (PlayReady of Widevine), bezorgingsbeperkingen (geopend of token) en informatie die specifiek zijn voor het type key levering waarmee wordt gedefinieerd hoe de toets wordt bezorgd in de client ([PlayReady](media-services-playready-license-template-overview.md) of [Widevine](media-services-widevine-license-template-overview.md) licentie sjabloon).
1. Configureer het beleid bezorging voor activa. De bezorging van beleidsconfiguratie omvat: bezorging protocol (bijvoorbeeld, MPEG-streepje HLS, schijven, vloeiende Streaming of alle), het type dynamische codering (bijvoorbeeld veelgebruikte codering), PlayReady of Widevine licentie acquisition URL.

U kunt verschillende beleid toepassen voor elk protocol dat op dezelfde activa. U kunt bijvoorbeeld PlayReady versleuteling toepassen op vloeiend/streepje en AES envelop naar HLS. Geen protocollen die niet zijn gedefinieerd in een bezorging beleid (bijvoorbeeld u één beleid toevoegen die alleen HLS als het protocol) uit streaming worden geblokkeerd. De uitzondering hierop is als u geen activa bezorging beleid helemaal gedefinieerd. Vervolgens zijn alle protocollen toegestaan in de wissen.
1. Maak een locator OnDemand om te krijgen van de URL van een streaming.

U vindt u een volledig .NET-voorbeeld aan het einde van het onderwerp.

De volgende afbeelding ziet u de werkstroom die hierboven is beschreven. Hier wordt de token gebruikt voor verificatie.

![Beveiligen met PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

De rest van dit onderwerp vindt u gedetailleerde uitleg, codevoorbeelden en koppelingen naar onderwerpen waarin u kunt hoe zien u de taken die hierboven beschreven bereiken.

##<a name="current-limitations"></a>Huidige beperkingen

Als u toevoegen of bijwerken van een beleid voor de bezorging van activa, moet u de bijbehorende locator (indien aanwezig) verwijderen en een nieuwe locator maken.

Beperking versleutelen met Widevine met Azure Media Services: meerdere inhoud toetsen worden momenteel niet ondersteund.

##<a name="create-an-asset-and-upload-files-into-the-asset"></a>Activa maken en uploaden van bestanden in de activa

Om te kunnen beheren, coderen en streamen van uw video's, moet u eerst uw inhoud uploaden naar Microsoft Azure Media Services. Zodra u hebt geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor nadere verwerking en streaming.

Zie [Bestanden uploaden naar een Media Services-account](media-services-dotnet-upload-files.md)voor gedetailleerde informatie.

##<a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Coderen van de activa met het bestand naar de geavanceerde bitsnelheid die MP4 instellen

Met dynamische versleuteling is u hoeft te maken van activa met een verzameling multi-bitrate MP4-bestanden of bronbestanden voor multi-bitrate vloeiende Streaming. Klik vervolgens op basis van de opgegeven notatie in de uitnodiging manifest en fragment, de server op aanvraag Streaming zorgt ervoor dat u de stream ontvangt in het protocol dat u hebt gekozen. Hierdoor, hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services-service wordt bouwen en het juiste antwoord op basis van aanvragen van een client fungeren. Zie het onderwerp [Dynamisch verpakking overzicht](media-services-dynamic-packaging-overview.md) voor meer informatie.

Voor instructies voor het coderen, Zie [hoe u het coderen van activa met Media Encoder standaard](media-services-dotnet-encode-with-media-encoder-standard.md).


##<a id="create_contentkey"></a>Een inhoud sleutel maakt en te koppelen aan de gecodeerd activa

De inhoud toets bevat in Media-Services, de sleutel die u wilt versleutelen activa met.

Zie [inhoud sleutel maken](media-services-dotnet-create-contentkey.md)voor gedetailleerde informatie.


##<a id="configure_key_auth_policy"></a>Beleid van de inhoud sleutel configureren

Media Services ondersteunt meerdere manieren gebruikers die belangrijke aanvragen te verifiëren. Het autorisatiebeleid inhoud belangrijke moet worden geconfigureerd door u en voldaan door de client (player) in de volgorde voor de sleutel is bezorgd op de client. Het autorisatiebeleid belangrijke voor inhoud kan zijn een of meer autorisatie-beperkingen: openen of token beperking.

Zie [Inhoudbeleid voor sleutel-autorisatie configureren](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption)voor gedetailleerde informatie.

##<a id="configure_asset_delivery_policy"></a>Activa bezorging beleid configureren 

Het beleid bezorging voor uw activa configureren. Enkele zaken die de configuratie van activa bezorging beleid omvat:

- De URL van DRM licentie acquisition. 
- De activa bezorging protocol (bijvoorbeeld MPEG streepje, HLS, schijven, vloeiende Streaming of alle). 
- Het type dynamische versleuteling (in dit geval algemene versleuteling). 

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

##<a id="example"></a>Voorbeeld


Het volgende voorbeeld wordt gedemonstreerd functionaliteit gebruiken die is geïntroduceerd in Azure Media Services SDK voor .net-versie 3.5.2 (name de mogelijkheid om te definiëren van een sjabloon van de licentie Widevine en een licentie Widevine aanvraagt bij Azure Media Services). De volgende opdracht uit Nuget pakket is gebruikt voor het installeren van het pakket:

    PM> Install-Package windowsazure.mediaservices -Version 3.5.2


1. Maak een nieuwe Console-project.
1. Gebruik NuGet installeert en Azure Media Services .NET SDK toevoegen.
2. Aanvullende verwijzingen toevoegen: System.Configuration.
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

1. Ten minste één streaming eenheid voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud downloaden. Zie voor meer informatie: [streaming eindpunten configureren](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. De code in het bestand Program.cs overschrijven met de code die in deze sectie worden weergegeven.
    
    Zorg ervoor dat bijwerken van variabelen zodat deze verwijzen naar mappen waarin uw invoer bestanden opgeslagen worden.
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
        using Newtonsoft.Json;
        
        namespace DynamicEncryptionWithDRM
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
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
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
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
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey, 
                                                                                DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
                    // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).
                     
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);
        
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
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
        
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
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Multiple Bitrate 720p";
            
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
            
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
            
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
            
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset),    AssetCreationOptions.StorageEncrypted);
            
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
            
                    return job.OutputMediaAssets[0];
                }
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
        
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy          
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Open",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("", 
                            ContentKeyDeliveryType.Widevine, 
                            restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
        
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
                    // Associate the content key authorization policy with the content key.
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Token Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString,
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.Widevine,
                                restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
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
        
                static private string ConfigurePlayReadyLicenseTemplate()
                {
                    // The following code configures PlayReady License Template using .NET classes
                    // and returns the XML string.
        
                    //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
                    //It contains a field for a custom data string between the license server and the application 
                    //(may be useful for custom app logic) as well as a list of one or more license templates.
                    PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
        
                    // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
                    // to be returned to the end users. 
                    //It contains the data on the content key in the license and any rights or restrictions to be 
                    //enforced by the PlayReady DRM runtime when using the content key.
                    PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
                    //Configure whether the license is persistent (saved in persistent storage on the client) 
                    //or non-persistent (only held in memory while the player is using the license).  
                    licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
        
                    // AllowTestDevices controls whether test devices can use the license or not.  
                    // If true, the MinimumSecurityLevel property of the license
                    // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
                    licenseTemplate.AllowTestDevices = true;
        
                    // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
                    // It grants the user the ability to playback the content subject to the zero or more restrictions 
                    // configured in the license and on the PlayRight itself (for playback specific policy). 
                    // Much of the policy on the PlayRight has to do with output restrictions 
                    // which control the types of outputs that the content can be played over and 
                    // any restrictions that must be put in place when using a given output.
                    // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
                    //then the DRM runtime will only allow the video to be displayed over digital outputs 
                    //(analog video outputs won’t be allowed to pass the content).
        
                    //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
                    // If the output protections are configured too restrictive, 
                    // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.
        
                    // For example:
                    //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);
        
                    responseTemplate.LicenseTemplates.Add(licenseTemplate);
        
                    return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
                }
        
        
                private static string ConfigureWidevineLicenseTemplate()
                {
                    var template = new WidevineMessage
                    {
                        allowed_track_types = AllowedTrackTypes.SD_HD,
                        content_key_specs = new[]
                        {
                            new ContentKeySpecs
                            {
                                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                        policy_overrides = new
                        {
                            can_play = true,
                            can_persist = true,
                            can_renew = false
                        }
                    };
        
                    string configuration = JsonConvert.SerializeObject(template);
                    return configuration;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    // Get the PlayReady license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
                
                    // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
                    // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
                    // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
                    // to append /? KID =< keyId > to the end of the url when creating the manifest.
                    // As a result Widevine license acquisition URL will have KID appended twice, 
                    // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.
            
                    Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
                    UriBuilder uriBuilder = new UriBuilder(widevineUrl);
                    uriBuilder.Query = String.Empty;
                    widevineUrl = uriBuilder.Uri;
        
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                            {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}
        
                        };
        
                    // In this case we only specify Dash streaming protocol in the delivery policy,
                    // All other protocols will be blocked from streaming.
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryption,
                        AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);
        
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
        
                }
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
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
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int length)
                {
                    var returnValue = new byte[length];
        
                    using (var rng =
                        new System.Security.Cryptography.RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(returnValue);
                    }
        
                    return returnValue;
                }
            }
        }


## <a name="next-step"></a>Volgende stap

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Zie ook

[CENC met Multi-DRM en beheren in Access](media-services-cenc-with-multidrm-access-control.md)

[Widevine verpakking met AMS configureren](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Services voor de bezorging van Google Widevine licentie in Azure Media Services aankondigen](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
