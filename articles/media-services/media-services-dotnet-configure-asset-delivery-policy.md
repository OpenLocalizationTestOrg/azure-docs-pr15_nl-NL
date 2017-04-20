<properties 
    pageTitle="Activa bezorging beleidsregels configureren met .NET SDK | Microsoft Azure" 
    description="In dit onderwerp ziet hoe u verschillende activa bezorging beleidsregels configureren met Azure Media Services .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>Beleidsregels voor activa bezorging met .NET SDK configureren
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>Overzicht

Als u van plan aan bezorging versleuteld activa bent, is een van de stappen in de Media Services contentlevering werkstroom bezorging beleid voor activa configureren. Het beleid van de bezorging van activa wordt Media Services uitgelegd hoe u uw activum wilt is bezorgd: in welke streaming protocol moet uw activa worden dynamisch verpakt (voor bijvoorbeeld MPEG streepje, HLS, vloeiende Streaming of alle), al dan niet u wilt versleutelen dynamisch uw activa en hoe (envelop of algemene versleuteling).

In dit onderwerp wordt uitgelegd waarom en hoe u kunt maken en configureren van beleidsregels voor activa bezorging.

>[AZURE.NOTE]Als u wilt kunnen dynamische verpakking en dynamische versleuteling gebruiken, moet u ervoor zorgen dat ten minste één eenheid voor tijdschaal (ook wel bekend als streaming eenheid). Zie [hoe u de schaal van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.
>
>Uw activa moet ook een reeks geavanceerde bitsnelheid MP4s of geavanceerde bitsnelheid vloeiende Streaming-bestanden bevatten.

U kunt verschillende beleid toepassen op hetzelfde actief. U kunt bijvoorbeeld PlayReady versleuteling toepassen op vloeiende Streaming en envelop AES-versleuteling MPEG streepje en HLS. Geen protocollen die niet zijn gedefinieerd in een bezorging beleid (bijvoorbeeld u één beleid toevoegen die alleen HLS als het protocol) uit streaming worden geblokkeerd. De uitzondering hierop is als u geen activa bezorging beleid helemaal gedefinieerd. Vervolgens zijn alle protocollen toegestaan in de wissen.

Als u een versleutelde activum opslag leveren wilt, moet u van het actief bezorging beleid configureren. Voordat uw activa kan worden streamen, wordt de streaming server Hiermee verwijdert u de opslag-versleuteling en uw met behoud van het beleid opgegeven bezorging streamt. Bijvoorbeeld, om uw activa versleuteld met geavanceerde versleuteling Standard (AES) envelop versleutelingssleutel, het beleidstype ingesteld op **DynamicEnvelopeEncryption**. Als u wilt opslag versleuteling verwijderen en de activa in de wissen streamen, Stel in het type op **NoDynamicEncryption**. Voorbeelden van het configureren van dit soort beleid volgen.

Afhankelijk van hoe u het beleid van de bezorging van activa configureert u zou kunnen dynamisch inpakken, dynamisch versleutelen en de volgende streaming protocollen streamen: vloeiende Streaming, HLS, MPEG-streepje en schijven streams.

De volgende lijst bevat de opmaakkenmerken die u gebruikt om te streamen vloeiend, HLS, streepje en schijven.

Vloeiende Streaming:

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG-STREEPJE

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

SCHIJVEN

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Zie [de URL van een streaming maken](media-services-deliver-streaming-content.md)voor instructies over het publiceren van activa en de URL van een streaming maken.

##<a name="considerations"></a>Overwegingen

- U kunt een AssetDeliveryPolicy die is gekoppeld aan een actief terwijl een locator OnDemand (streaming) voor die actief bestaat niet verwijderen. De aanbeveling is het beleid verwijderen uit de activa voordat het beleid wordt verwijderd.
- Een streaming locator kan niet worden gemaakt op een opslag versleutelde actief wanneer er geen activa bezorging beleid is ingesteld.  Als de activa niet opslag versleuteld, het systeem, kunt u een locator maken en de activa in de wissen zonder een activum bezorging beleid streamen.
- U kunt meerdere activa bezorging beleidsregels die is gekoppeld aan een enkel actief hebben, maar u kunt slechts één manier voor het verwerken van een bepaald AssetDeliveryProtocol opgeven.  Wat betekent dat als u probeert te koppelen van twee bezorging beleidsregels die het protocol AssetDeliveryProtocol.SmoothStreaming die in een fout resulteert omdat het systeem niet welke thema dat u wilt toepassen weet wordt wanneer een client een verzoek voor een soepele Streaming opgeeft.
- Als u een actief met een bestaande streaming locator hebt, kunt u een nieuw beleid niet koppelen aan de activa (u kunt een bestaand beleid uit het actief ontkoppelen of een bezorging beleid dat is gekoppeld aan de activa bijwerken).  U moet eerst de streaming locator verwijderen, het beleid aanpassen en de streaming locator vervolgens opnieuw te maken.  U kunt de dezelfde locatorId gebruiken wanneer u de streaming locator opnieuw moet maken, maar u ervoor dat niet toe leiden dat problemen voor clients zorgen moet aangezien inhoud kan worden opgeslagen in de oorsprong of een volgende CDN.


##<a name="clear-asset-delivery-policy"></a>Wissen activa bezorging beleid

De volgende **ConfigureClearAssetDeliveryPolicy** methode Hiermee geeft u op dynamische versleuteling niet van toepassing en voor het leveren van de stream in een van de volgende protocollen: MPEG-streepje HLS en vloeiende Streaming protocollen. Mogelijk wilt u dit beleid van toepassing op uw activa opslag versleuteld.

Zie het gedeelte [typen gebruikt bij het definiëren van AssetDeliveryPolicy](#types) voor meer informatie over welke waarden die u bij het maken van een AssetDeliveryPolicy kunt opgeven.

statische openbare void ConfigureClearAssetDeliveryPolicy (IAsset activa) {IAssetDeliveryPolicy beleid = _context. AssetDeliveryPolicies.Create ("Wissen beleid", AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

activa. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption activa bezorging beleid


De volgende **CreateAssetDeliveryPolicy** -methode maakt de **AssetDeliveryPolicy** die is geconfigureerd voor het toepassen van dynamische algemene versleuteling (**DynamicCommonEncryption**) aan een vloeiende streaming protocol (andere protocollen worden geblokkeerd van streaming). De methode heeft twee parameters: **activa** (de activa die u wilt toepassen het beleid bezorging) en **IContentKey** (raadpleegt u de inhoud sleutel van het type **CommonEncryption** , voor meer informatie: [een inhoud sleutel maken](media-services-dotnet-create-contentkey.md#common_contentkey)).

Zie het gedeelte [typen gebruikt bij het definiëren van AssetDeliveryPolicy](#types) voor meer informatie over welke waarden die u bij het maken van een AssetDeliveryPolicy kunt opgeven.


statische openbare void CreateAssetDeliveryPolicy (IAsset activa, IContentKey toets) {Uri acquisitionUrl = toets. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

Woordenlijst < AssetDeliveryPolicyConfigurationKey, tekenreeks > assetDeliveryPolicyConfiguration nieuwe woordenlijst < AssetDeliveryPolicyConfigurationKey, tekenreeks > = {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Azure Media Services kunt u Widevine versleuteling toevoegen. Het volgende voorbeeld worden zowel PlayReady en Widevine wordt toegevoegd aan het beleid van de bezorging van activa.


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
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]Wanneer u versleutelt met Widevine, zou het alleen mogelijk om uit te voeren met streepje. Zorg ervoor dat streepje opgeven in het activa bezorging-protocol.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption activa bezorging beleid 

De volgende **CreateAssetDeliveryPolicy** -methode maakt de **AssetDeliveryPolicy** die is geconfigureerd voor dynamische envelop versleuteling (**DynamicEnvelopeEncryption**) toepassen op protocollen vloeiende Streaming, HLS en -streepje (als u besluit om op te geven niet sommige protocollen, ze worden geblokkeerd van streaming). De methode heeft twee parameters: **activa** (de activa die u wilt toepassen het beleid bezorging) en **IContentKey** (raadpleegt u de inhoud sleutel van het type **EnvelopeEncryption** , voor meer informatie: [een inhoud sleutel maken](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Zie het gedeelte [typen gebruikt bij het definiëren van AssetDeliveryPolicy](#types) voor meer informatie over welke waarden die u bij het maken van een AssetDeliveryPolicy kunt opgeven.   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
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
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>Typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


