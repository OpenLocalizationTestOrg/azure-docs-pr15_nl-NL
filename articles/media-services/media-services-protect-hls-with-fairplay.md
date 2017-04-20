<properties 
    pageTitle="Beveiligen van uw inhoud met Apple FairPlay en/of Microsoft PlayReady HLS | Microsoft Azure" 
    description="In dit onderwerp biedt een overzicht en wordt uitgelegd hoe u Azure Media Services dynamisch versleutelen de HTTP Live Streaming (HLS)-inhoud met Apple FairPlay. Ook ziet u het gebruik van de bezorgingsservice van de Media Services-licentie voor het leveren van FairPlay licenties aan clients." 
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
    ms.date="09/27/2016"
    ms.author="juliako"/>

# <a name="protect-your-hls-content-with-apple-fairplay-andor-microsoft-playready"></a>Uw inhoud met Apple FairPlay en/of Microsoft PlayReady HLS beveiligen

Azure Media Services kunt u dynamisch versleutelen uw HTTP Live Streaming (HLS)-inhoud met de volgende indelingen:  

- **Wissen AES-128 envelop-toets** 

    Het hele segment is versleuteld met de **AES-128 CBC** -modus. De beschrijving van de stream wel ondersteund door de iOS- en OSX player. Zie [dit artikel](media-services-protect-with-aes128.md)voor meer informatie.

- **Apple FairPlay** 

    De afzonderlijke video en audio voorbeelden zijn versleuteld met de **AES-128 CBC** -modus. **FairPlay Streaming** (FPS) is geïntegreerd in de besturingssystemen apparaat, met ondersteuning voor iOS en Apple TV. Safari in OS X kunt FPS met versleuteld Media extensies (EME) interfaceondersteuning.
- **Microsoft PlayReady**

De volgende afbeelding ziet u de werkstroom **HLS + FairPlay en/of PlayReady dynamische versleuteling** .

![Beveiligen met FairPlay](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

Dit onderwerp wordt beschreven hoe u uw inhoud HLS met Apple FairPlay dynamisch versleutelen met Azure Media Services. Ook ziet u het gebruik van de bezorgingsservice van de Media Services-licentie voor het leveren van FairPlay licenties aan clients.

>[AZURE.NOTE] Desgewenst kunt u ook de inhoud HLS met PlayReady versleutelen, moet u een gemeenschappelijke inhoud sleutel maken en koppelen aan uw activa. U moet ook configureren van de inhoud sleutel autorisatiebeleid, zoals beschreven in [Gebruik PlayReady dynamische algemene versleuteling](media-services-protect-with-drm.md) onderwerp.

    
## <a name="requirements-and-considerations"></a>Vereisten en overwegingen

- Hier volgen vereist wanneer u een AMS gebruikt voor het leveren van HLS versleuteld met FairPlay en voor het leveren van FairPlay licenties.

    - Een Azure-account. Zie [Azure gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F)voor meer informatie.
    - Een Media Services-account. Als u wilt een Media Services-account hebt gemaakt, Zie [Account maken](media-services-portal-create-account.md).
    - Aanmelden bij [Apple ontwikkeling programma](https://developer.apple.com/).
    - Apple is vereist voor de eigenaar van de inhoud het [implementatiepakket](https://developer.apple.com/contact/fps/)verkrijgen. Beschrijft de aanvraag die u al geïmplementeerd KSM (sleutel beveiligingsmodule) met Azure Media Services en dat u het uiteindelijke FPS-pakket aanvraagt. Er worden instructies in het definitieve FPS-pakket certificering genereren en verkrijgen van vragen, die u gebruiken wilt voor het configureren van FairPlay. 

    - Azure Media Services .NET SDK versie **3.6.0** of hoger.

- De volgende zaken aan bod moeten worden ingesteld op AMS belangrijke bezorging zijde:
    - **App-certificaat (WC)** - .pfx-bestand met persoonlijke sleutel. Dit bestand is gemaakt door de klant en versleuteld met een wachtwoord van dezelfde klant. 
        
        Als de klant belangrijke bezorging beleid configureert, moeten ze opgeven dat wachtwoord en de .pfx in base64-indeling.

        De volgende stappen wordt beschreven hoe een certificaat pfx genereren voor FairPlay.
        
        1. OpenSSL installeren vanaf https://slproweb.com/products/Win32OpenSSL.html
        
            Ga naar de map waar het certificaat FairPlay en andere bestanden geleverd door Apple zich bevinden.
        
        2. Opdrachtregel de cer converteren naar pem:
        
            "C:\OpenSSL-Win32\bin\openssl.exe" x509-informeren der-in fairplay.cer-out fairplay-out.pem
        
        3. Opdrachtregel pem converteren naar pfx met de persoonlijke sleutel (het wachtwoord voor het pfx-bestand wordt vervolgens gevraagd door OpenSSL).
        
            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-Exporteer - fairplay-out.pfx-inkey privatekey.pem-in fairplay-out.pem - passin file:privatekey-pem-pass.txt 
        
    - **App-certificaat wachtwoord** - klant-wachtwoord voor het maken van de .pfx-bestand.
    - **App-certificaat wachtwoord-ID** - de klant het wachtwoord die vergelijkbaar met hoe ze uploaden om een andere sleutels AMS en **ContentKeyType.FairPlayPfxPassword** opsommingswaarde moet uploaden. Als het resultaat worden verkregen AMS id die dit is wat ze moeten binnen de optie voor het beleid van belangrijke levering gebruiken.
    - **iv** - 16 bytes willekeurige waarde, moet overeenkomen met de iv in het beleid van de bezorging van activa. Klant genereert de IV en zet deze op beide locaties: activa beleid en van toetsen bezorging beleid optie levering. 
    - **ASK** - ASK (geheim toepassingstoets) wordt weergegeven wanneer u het certificaat dat met behulp van Apple Developer portal genereren. Elke ontwikkelteam ontvangt een unieke ASK. Sla een kopie van de vragen en sla deze op een veilige plaats. U moet ASK configureren als FairPlayAsk Azure Media Services later. 
    -  **Stel een vraag ID** - worden opgehaald wanneer de klant ASk uploadt naar AMS. De klant moet uploaden ASk **ContentKeyType.FairPlayASk** opsommingswaarde gebruiken. De AMS-id wordt geretourneerd als het resultaat, en dit is wat moet worden gebruikt bij het instellen van de optie voor het beleid van belangrijke levering.

- De volgende zaken aan bod moeten worden ingesteld door de client FPS:
    - **App-certificaat (WC)** -.cer/.der-bestand met openbare sleutel waardoor OS worden sommige nettolading versleutelen. AMS moet weten over deze omdat deze nodig is voor de speler. De belangrijkste bezorgingsservice ontsleutelt deze met de bijbehorende persoonlijke sleutel.

- Naar een versleutelde stream FairPlay afspelen moet u reële ASK eerste ophalen en vervolgens een reële certificaat te genereren. Proces Hiermee maakt u alle 3 delen:

    -  .der, 
    -  .pfx en 
    -  het wachtwoord voor de .pfx.
 
- Klanten die ondersteuning bieden voor HLS met **AES-128 CBC** versleuteling: Safari op OS X, Apple TV, iOS.

##<a name="steps-for-configuring-fairplay-dynamic-encryption-and-license-delivery-services"></a>Stappen voor het configureren van FairPlay dynamische versleuteling en licentie bezorging services

Hier volgen algemene stappen die u uitvoeren moet tijdens het beveiligen van uw activa, met FairPlay, gebruikt de bezorgingsservice van de Media Services-licentie en ook dynamische versleuteling gebruikt.

1. Maak activa en bestanden uploaden naar de activa. 
1. Coderen van de activa met het bestand naar de geavanceerde bitsnelheid die MP4 instellen.
1. Een inhoud sleutel maakt en deze koppelen aan het gecodeerde activum.  
1. Beleid van de inhoud sleutel configureren. Wanneer u het autorisatiebeleid inhoud belangrijke maakt, moet u het volgende opgeven: 
    
    - Afleveringsmethode (in dit geval FairPlay), 
    - Configuratie van FairPlay beleid opties. Zie voor meer informatie over het configureren van FairPlay ConfigureFairPlayPolicyOptions() methode in het onderstaande voorbeeld.
    
        >[AZURE.NOTE] Zou willen u meestal slechts één keer FairPlay beleidsopties configureren sinds u alleen één set-certificering en ASK hebt.
-beperkingen (geopend of token)- en informatie die specifiek zijn voor het type key levering waarmee wordt gedefinieerd hoe de toets wordt bezorgd in de client. 
    
2. Activa bezorging beleid configureren. De configuratie van het beleid bezorging bevat: 

    - bezorging-protocol (HLS), 
    - het type dynamische codering (algemene CBC codering), 
    - de URL van licentie. 
    
    >[AZURE.NOTE]Als u een stream die is versleuteld met FairPlay + een andere DRM leveren wilt, moet u beleidsregels voor afzonderlijke bezorging configureren 
    >
    >- Eén IAssetDeliveryPolicy streepje configureren met CENC (PlayReady + WideVine) en vloeiend met PlayReady. 
    >- Een andere IAssetDeliveryPolicy FairPlay configureren voor HLS

1. Maak een locator OnDemand als u de URL van een streaming.

##<a name="using-fairplay-key-delivery-by-playerclient-apps"></a>FairPlay belangrijke levering gebruikt door apps player-client

Klanten kunnen speler-apps met iOS SDK ontwikkelen. Klanten moeten licentie exchange protocol implementeren om te kunnen FairPlay inhoud af te spelen. Het exchange-protocol licentie is niet opgegeven door Apple. Deze als volgt snel aan de elke app belangrijke bezorging aanvragen verzenden. De services van de belangrijkste bezorging AMS FairPlay verwacht dat de SPC bij als www-form-url gecodeerde bericht bericht in de volgende notatie: 

    spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Player biedt geen ondersteuning FairPlay afspelen uit het vak. Klanten moeten de voorbeeld-speler verkrijgen met Apple ontwikkelaarsaccount om toegang te krijgen van FairPlay afspelen op de MAC OSX. 
 
##<a name="streaming-urls"></a>Streaming URL 's

Als uw activa is versleuteld met meer dan één DRM, moet u een tag versleuteling gebruiken in de streaming URL: (opmaak = 'm3u8-aapl', versleuteling = 'lengte').

De volgende punten zijn van toepassing:

- Alleen nul of één versleutelingstype kan worden opgegeven.
- Coderingstype hoeft te worden opgegeven in de url als er slechts één versleuteling op de activa is toegepast.
- Coderingstype is hoofdlettergevoelig.
- De volgende coderingstypen u kunt opgeven:  
    - **cenc**: algemene codering (Playready of Widevine)
    - **cbcs-aapl**: Fairplay
    - **CBC**: AES envelop-versleuteling gebruikt.


##<a name="net-example"></a>.NET-voorbeeld


Het volgende voorbeeld wordt gedemonstreerd functionaliteit gebruiken die is geïntroduceerd in Azure Media Services SDK voor .net-versie 3.6.0 (de mogelijkheid om Azure Media Services gebruiken om uw inhoud versleuteld met FairPlay). De volgende opdracht uit Nuget pakket is gebruikt voor het installeren van het pakket:

    PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Maak een Console-project.
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
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
        using Newtonsoft.Json;
        using System.Security.Cryptography.X509Certificates;
        
        namespace DynamicEncryptionWithFairPlay
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
        
                    IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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
        
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
        
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
        
                static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
                {
                    // Create HLS SAMPLE AES encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryptionCbcs);
        
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
        
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.FairPlay,
                        restrictions,
                        FairPlayConfiguration);
        
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
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
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                               ContentKeyDeliveryType.FairPlay,
                               restrictions,
                               FairPlayConfiguration);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                private static string ConfigureFairPlayPolicyOptions()
                {
                    // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
                    // However, for production you must use a real ASK from Apple bound to a real prod certificate.
                    byte[] askBytes = Guid.NewGuid().ToByteArray();
                    var askId = Guid.NewGuid();
                    // Key delivery retrieves askKey by askId and uses this key to generate the response.
                    IContentKey askKey = _context.ContentKeys.Create(
                                            askId,
                                            askBytes,
                                            "askKey",
                                            ContentKeyType.FairPlayASk);
        
                    //Customer password for creating the .pfx file.
                    string pfxPassword = "<customer password for creating the .pfx file>";
                    // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
                    var pfxPasswordId = Guid.NewGuid();
                    byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
                    IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                                            pfxPasswordId,
                                            pfxPasswordBytes,
                                            "pfxPasswordKey",
                                            ContentKeyType.FairPlayPfxPassword);
        
                    // iv - 16 bytes random value, must match the iv in the asset delivery policy.
                    byte[] iv = Guid.NewGuid().ToByteArray();
        
                    //Specify the .pfx file created by the customer.
                    var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
        
                    string FairPlayConfiguration =
                        Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                            appCert,
                            pfxPassword,
                            pfxPasswordId,
                            askId,
                            iv);
        
                    return FairPlayConfiguration;
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
        
                    var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
        
                    FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
        
                    // Get the FairPlay license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
        
                    // The reason the below code replaces "https://" with "skd://" is because
                    // in the IOS player sample code which you obtained in Apple developer account, 
                    // the player only recognizes a Key URL that starts with skd://. 
                    // However, if you are using a customized player, 
                    // you can choose whatever protocol you want. 
                    // For example, "https". 

                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                            {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
                        };
        
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
                        AssetDeliveryProtocol.HLS,
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


##<a name="next-steps-media-services-learning-paths"></a>Volgende stappen: Mediaservices leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
