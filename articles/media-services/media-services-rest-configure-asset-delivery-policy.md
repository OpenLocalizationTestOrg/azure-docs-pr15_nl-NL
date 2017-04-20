<properties 
    pageTitle="Activa bezorging beleid met Media Services REST API configureren | Microsoft Azure" 
    description="In dit onderwerp ziet hoe u verschillende activa bezorging beleidsregels voor Media Services REST API met configureren." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>Activa bezorging beleid configureren

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Als u van plan bent om uit te voeren dynamisch versleutelde activa, is een van de stappen in de Media Services contentlevering werkstroom bezorging beleid voor activa configureren. Het beleid van de bezorging van activa wordt Media Services uitgelegd hoe u uw activum wilt is bezorgd: in welke streaming protocol moet uw activa worden dynamisch verpakt (voor bijvoorbeeld MPEG streepje, HLS, vloeiende Streaming of alle), al dan niet u wilt versleutelen dynamisch uw activa en hoe (envelop of algemene versleuteling).

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
- Als u een actief met een bestaande streaming locator hebt, niet kunt u een nieuw beleid koppelen aan de activa, ontkoppelen van een bestaand beleid uit het actief of een bezorging beleid dat is gekoppeld aan de activa bijwerken.  U moet eerst de streaming locator verwijderen, het beleid aanpassen en de streaming locator vervolgens opnieuw te maken.  U kunt de dezelfde locatorId gebruiken wanneer u de streaming locator opnieuw moet maken, maar u ervoor dat niet toe leiden dat problemen voor clients zorgen moet aangezien inhoud kan worden opgeslagen in de oorsprong of een volgende CDN.

>[AZURE.NOTE] Tijdens het werken met de Media Services REST API, zijn de volgende punten van toepassing:
>
>Bij het openen van entiteiten in Media Services, moet u specifieke koptekstvelden en waarden instellen in uw HTTP-aanvragen. Zie [configuratie van voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor meer informatie.

>Nadat de verbinding tot stand naar https://media.windows.net, ontvangt u een 301 omleiding precisie van een andere Media-URI voor Services. Mislukken volgende oproepen naar de nieuwe URI zoals is beschreven in [verbinding maken met Media-Services REST API gebruiken](media-services-rest-connect-programmatically.md), moet u ervoor.


##<a name="clear-asset-delivery-policy"></a>Wissen activa bezorging beleid

###<a id="create_asset_delivery_policy"></a>Activa bezorging-beleid maken
De volgende HTTP-aanvraag Hiermee maakt u een beleid voor het bezorging van activa waarin wordt aangegeven niet van toepassing dynamische versleuteling en voor het leveren van de stream in een van de volgende protocollen: MPEG-streepje HLS en vloeiende Streaming protocollen. 

Zie het gedeelte [typen gebruikt bij het definiëren van AssetDeliveryPolicy](#types) voor meer informatie over welke waarden die u bij het maken van een AssetDeliveryPolicy kunt opgeven.   


Vraag:
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Antwoord:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a id="link_asset_with_asset_delivery_policy"></a>Koppeling activa met activa bezorging beleid

De opgegeven activa de volgende HTTP-aanvraag gekoppeld aan het beleid van de bezorging van activa aan.

Vraag:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Antwoord:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption activa bezorging beleid 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Inhoud sleutel van het type EnvelopeEncryption maken en dit koppelen aan de activa

Wanneer u DynamicEnvelopeEncryption bezorging beleid opgeeft, moet u Zorg ervoor dat uw activum koppelen aan een inhoudstype sleutel van het type EnvelopeEncryption. Zie voor meer informatie: [een inhoud sleutel maken](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Bezorging URL ophalen

Voor de afleveringsmethode voor het opgegeven van de inhoud sleutel in de vorige stap hebt gemaakt, zodat u de URL voor de bezorging. Een client gebruikt de resulterende URL aanvragen van een sleutel AES of een PlayReady licentie in volgorde voor het afspelen van de beveiligde inhoud.

Het type van de URL in de hoofdtekst van het HTTP-verzoek om te geven. Als u de inhoud met PlayReady, beveiligen aanvragen Media Services PlayReady licentie acquisition URL, met behulp van 1 voor de keyDeliveryType: {"keyDeliveryType": 1}. Als u de beveiliging van uw inhoud met de envelop-versleuteling, de URL van een belangrijke acquisition aanvragen door het opgeven van 2 voor keyDeliveryType: {"keyDeliveryType": 2}.

Vraag:
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Antwoord:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>Activa bezorging-beleid maken

De volgende HTTP-aanvraag Hiermee maakt u de **AssetDeliveryPolicy** die is geconfigureerd voor dynamische envelop versleuteling (**DynamicEnvelopeEncryption**) toepassen op de **HLS** -protocol (in dit voorbeeld andere protocollen worden geblokkeerd van streaming). 


Zie het gedeelte [typen gebruikt bij het definiëren van AssetDeliveryPolicy](#types) voor meer informatie over welke waarden die u bij het maken van een AssetDeliveryPolicy kunt opgeven.   

Vraag:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Antwoord:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Koppeling activa met activa bezorging beleid

Zie [koppeling activa met activa bezorging beleid](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption activa bezorging beleid 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Inhoud sleutel van het type CommonEncryption maken en dit koppelen aan de activa

Wanneer u DynamicCommonEncryption bezorging beleid opgeeft, moet u Zorg ervoor dat uw activum koppelen aan een inhoudstype sleutel van het type CommonEncryption. Zie voor meer informatie: [een inhoud sleutel maken](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Bezorging URL ophalen

Voor de afleveringsmethode PlayReady van de inhoud sleutel in de vorige stap hebt gemaakt, zodat u de URL voor de bezorging. Een client gebruikt de resulterende URL aanvragen van een licentie PlayReady in volgorde naar de beveiligde inhoud afspelen. Zie voor meer informatie [Krijgen bezorging URL](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>Activa bezorging-beleid maken

De volgende HTTP-aanvraag Hiermee maakt u de **AssetDeliveryPolicy** die is geconfigureerd voor dynamische algemene versleuteling (**DynamicCommonEncryption**) toepassen op de **Vloeiende Streaming** -protocol (in dit voorbeeld andere protocollen worden geblokkeerd van streaming). 

Zie het gedeelte [typen gebruikt bij het definiëren van AssetDeliveryPolicy](#types) voor meer informatie over welke waarden die u bij het maken van een AssetDeliveryPolicy kunt opgeven.   


Vraag:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Als u beveiligen van uw inhoud met Widevine DRM wilt, bijwerken van de waarden AssetDeliveryConfiguration als u wilt gebruiken, WidevineLicenseAcquisitionUrl (die de waarde van 7 heeft) en geef de URL van een licentie bezorgingsservice. U kunt de volgende AMS partners gebruiken om te leveren Widevine licenties: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Bijvoorbeeld: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Wanneer u versleutelt met Widevine, zou het alleen mogelijk om uit te voeren met streepje. Controleer of moeten worden opgegeven streepje (2) in de activa bezorging-protocol.
  
###<a name="link-asset-with-asset-delivery-policy"></a>Koppeling activa met activa bezorging beleid

Zie [koppeling activa met activa bezorging beleid](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

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

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

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

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

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
