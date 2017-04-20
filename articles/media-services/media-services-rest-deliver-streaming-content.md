<properties 
    pageTitle="Azure Media Services met behoud van REST publiceren" 
    description="Informatie over het maken van een locator die wordt gebruikt voor het maken van een streaming URL. REST API wordt gebruikt door de code." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="publish-azure-media-services-content-using-rest"></a>Azure Media Services met behoud van REST publiceren

> [AZURE.SELECTOR]
- [.NET](media-services-deliver-streaming-content.md)
- [REST](media-services-rest-deliver-streaming-content.md)
- [Portal](media-services-portal-publish.md)

##<a name="overview"></a>Overzicht


U kunt een geavanceerde bitsnelheid MP4 instellen door te maken van een OnDemand streaming locator en bouwen van een streaming URL stream. Het onderwerp [codering activa](media-services-rest-encode-asset.md) ziet hoe u coderen in een geavanceerde bitsnelheid die MP4 instellen. Als uw inhoud is versleuteld, configureert u activa bezorging beleid (zoals beschreven in [Dit](media-services-rest-configure-asset-delivery-policy.md) onderwerp) voordat u een locator maakt. 

U kunt ook een OnDemand streaming locator gebruiken voor het maken van URL's die verwijzen naar MP4-bestanden die geleidelijk kunnen worden gedownload.  

Dit onderwerp wordt uitgelegd hoe u een OnDemand streaming locator te publiceren van uw activa en maakt u een vloeiend, MPEG-streepje en HLS streaming van URL's maakt. U ziet ook populair om u te maken van URL's geleidelijke downloaden.

De [volgende](#types) sectie ziet u welke opsommen waarvan de waarden worden gebruikt in de REST-oproepen.   
  
##<a name="create-an-ondemand-streaming-locator"></a>Een OnDemand streaming locator maken

Als u wilt maken van de OnDemand streaming locator ophalen URL's moet u als volgt te werk:


   1. Als de inhoud is versleuteld, moet u een access-beleid definiëren.
   2. Maak een OnDemand streaming locator.
   3. Als u van plan bent om te streamen, kunt u het streaming manifest-bestand (.ism) in de activa. 
        
    Als u van plan bent om te geleidelijk downloaden, kunt u de namen van MP4-bestanden in de activa. 
   4. URL's naar de manifest-bestand of MP4-bestanden maken. 
   5. Opmerking die u kunt niet eerst een streaming locator met een AccessPolicy waarin maken schrijven of machtigingen verwijderen.


###<a name="create-an-access-policy"></a>Een access-beleid maken

Vraag:
        
    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstest1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1424263184&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=NWE%2f986Hr5lZTzVGKtC%2ftzHm9n6U%2fxpTFULItxKUGC4%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    Host: media.windows.net
    Content-Length: 68
    
    {"Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}
    
Antwoord:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 311
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https:/media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3A69c80d98-7830-407f-a9af-e25f4b0d3e5f')
    Server: Microsoft-IIS/8.5
    request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:52:09 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AccessPolicies/@Element","Id":"nb:pid:UUID:69c80d98-7830-407f-a9af-e25f4b0d3e5f","Created":"2015-02-18T06:52:09.8862191Z","LastModified":"2015-02-18T06:52:09.8862191Z","Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}

###<a name="create-an-ondemand-streaming-locator"></a>Een OnDemand streaming locator maken

Maak de locator voor de opgegeven activa en activa beleid.

Vraag:
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstest1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1424263184&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=NWE%2f986Hr5lZTzVGKtC%2ftzHm9n6U%2fxpTFULItxKUGC4%3d
    x-ms-version: 2.11
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    Host: media.windows.net
    Content-Length: 181
    
    {"AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872Z","Type":2}

Antwoord:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 637
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Abe245661-2bbd-4fc6-b14f-9cf9a1492e5e')
    Server: Microsoft-IIS/8.5
    request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:58:37 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#Locators/@Element","Id":"nb:lid:UUID:be245661-2bbd-4fc6-b14f-9cf9a1492e5e","ExpirationDateTime":"2015-03-20T06:34:47.267872+00:00","Type":2,"Path":"http://amstest1.streaming.mediaservices.windows.net/be245661-2bbd-4fc6-b14f-9cf9a1492e5e/","BaseUri":"http://amstest1.streaming.mediaservices.windows.net","ContentAccessComponent":"be245661-2bbd-4fc6-b14f-9cf9a1492e5e","AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872+00:00","Name":null}

###<a name="build-streaming-urls"></a>Streaming van URL's maken

Gebruik de waarde van het **pad** als resultaat gegeven na het maken van de locator maken van de vloeiende, HLS en URL's MPEG-streepje. 

Vloeiende Streaming: **Pad** + manifest bestandsnaam + "/ bestandenlijst"

Voorbeeld:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest

HLS: **Pad** + manifest bestandsnaam + "/ manifest(format=m3u8-aapl)"

Voorbeeld:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)


STREEPJE: **Pad** + manifest bestandsnaam + "/ manifest(format=mpd-time-csf)"


Voorbeeld:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


###<a name="build-progressive-download-urls"></a>Geleidelijke downloaden URL's maken

Gebruik de waarde van het **pad** als resultaat gegeven na het maken van de locator maken van de geleidelijke download-URL.   

URL: **Pad** + activa bestandsnaam mp4

Voorbeeld:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

##<a id="types"></a>Vaste tekst typen

    [Flags]
    public enum AccessPermissions
    {
        None = 0,
        Read = 1,
        Write = 2,
        Delete = 4,
        List = 8,
    }

    public enum LocatorType
    {
        None = 0,
        Sas = 1,
        OnDemandOrigin = 2,
    }

##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook

[Activa bezorging beleid configureren](media-services-rest-configure-asset-delivery-policy.md)
