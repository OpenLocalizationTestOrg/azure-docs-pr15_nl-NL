<properties 
    pageTitle="Veelgestelde vragen | Microsoft Azure" 
    description="Veelgestelde vragen" 
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
    ms.date="09/19/2016" 
    ms.author="juliako"/>


#<a name="frequently-asked-questions"></a>Veelgestelde vragen

##<a name="general-ams-faqs"></a>Algemeen AMS Veelgestelde vragen

V: hoe u de schaal van indexering?

A: de eenheden die zijn gereserveerde zijn hetzelfde voor versleutelen en indexering taken. Volg de instructies over [het schaal codering gereserveerde eenheden](media-services-scale-media-processing-overview.md). **Opmerking** deze prestaties indexering wordt niet beïnvloed door gereserveerde Type van de eenheid.

V: ik hebt geüpload, codering en een video die zijn gepubliceerd. Wat is de reden de video niet wordt afgespeeld wanneer ik wil streamen deze?

A: een van de meest voorkomende redenen is er geen ten minste één gereserveerde streaming eenheid toegewezen op het streaming eindpunt waaruit u te afspelen probeert.  Volg de instructies over [het schaal Streaming gereserveerde eenheden](media-services-portal-scale-streaming-endpoints.md).

V: kan ik samenstellen op een live gegevensstroom doen?

A: samenstellen op live gegevensstromen is momenteel niet beschikbaar in Azure Media Services, zodat u nodig hebt voor het opstellen van vooraf op uw computer.

V: kan ik Azure CDN gebruiken met Live Streaming?

A: Media Services ondersteunt integratie met Azure CDN (Zie [hoe u Streaming eindpunten in een Media Services-Account beheren](media-services-portal-manage-streaming-endpoints.md)voor meer informatie).  U kunt Live streaming met CDN gebruiken. Azure Media Services biedt vloeiende uitvoer van Streaming, HLS en MPEG-streepje. Alle volgende indelingen HTTP gebruiken voor het overbrengen van gegevens en voordelen van HTTP in cache opslaan. In live streaming werkelijke gegevens van de video/audio wordt gedeeld op fragmenten en deze afzonderlijke fragmenten ophalen met cache in CDN. Alleen gegevens nodig heeft om te worden vernieuwd is de manifest gegevens. CDN worden manifest gegevens regelmatig vernieuwd.

V: worden Azure Media services ondersteunen opslaan afbeeldingen?

A: als u alleen op zoek bent om op te slaan JPEG of PNG-afbeeldingen, moet u de gebruikersprofielen in Azure-blobopslag behouden. Er is geen voordeel voor uw account Media Services plaatsen, tenzij u wilt dat is gekoppeld aan uw Video of Audio activa kunt houden. Of als u een reden voor het gebruik van de afbeeldingen als overlays in de video encoder hebt. Media Encoder standaard ondersteunt hierbij afbeeldingen boven aan de video's en die is deze lijsten JPEG en PNG zoals ondersteund invoer van indelingen. Zie [Overlays maken](media-services-custom-mes-presets-with-dotnet.md#overlay)voor meer informatie.

V: hoe kan ik activa van de ene Media Services-account naar een andere kopiëren.

A: naar activa van de ene Media Services-account naar een andere kopiëren met behulp van .NET, gebruikt u [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) extensie methode beschikbaar in de bibliotheek [Azure Media Services .NET SDK uitbreidingen](https://github.com/Azure/azure-sdk-for-media-services-extensions/) . Zie [deze](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) thread-forum voor meer informatie.

V: wat zijn de ondersteunde tekens voor de naamgeving van bestanden tijdens het werken met AMS?

A: Media Services gebruikt de waarde van de eigenschap IAssetFile.Name bij het maken van URL's voor de streaming inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Daarom is percentagecodering niet toegestaan. De waarde van de eigenschap **Name** geen van de volgende [tekens percentage-codering-gereserveerd](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie.


V: hoe verbinding maken met behulp van de REST?

A: na verbinding tot stand naar https://media.windows.net, ontvangt u een 301 omleiding precisie van een andere Media-URI voor Services. Mislukken volgende oproepen naar de nieuwe URI zoals is beschreven in [verbinding maken met Media-Services REST API gebruiken](media-services-rest-connect-programmatically.md), moet u ervoor. 


V: hoe kan ik een video tijdens de codering de stand.

A: het [Media Encoder standaard](media-services-dotnet-encode-with-media-encoder-standard.md) ondersteunt draaiing door de hoeken van 90/180/270. Het standaardgedrag is 'Automatisch', waarbij wordt geprobeerd de draaiing metagegevens detecteren in het binnenkomende MP4/MOV-bestand en dit voor deze. Zijn de volgende **bronnen** element naar een van de json vooraf ingestelde gedefinieerde [hier](http://msdn.microsoft.com/library/azure/mt269960.aspx):
    
    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...




##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
