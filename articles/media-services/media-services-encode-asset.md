<properties 
    pageTitle="Overzicht en vergelijking van Azure op aanvraag media encoders | Microsoft Azure" 
    description="Dit onderwerp biedt een overzicht en vergelijking van Azure op aanvraag media encoders." 
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
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Overzicht en vergelijking van Azure op aanvraag media encoders

##<a name="encoding-overview"></a>Codering overzicht

Azure Media Services biedt meerdere opties voor het coderen van media in de cloud.

Wanneer u begint met Media-Services, is het belangrijk dat u het verschil tussen codecs en bestandsindelingen.
Codecs zijn de software waarmee de algoritmen compressie/decompressie dat bestandsindelingen containers waarin de gecomprimeerde video.

Media Services biedt dynamische verpakking waarmee u voor het leveren van de inhoud van het geavanceerde bitsnelheid MP4 of vloeiende Streaming codering in door Services Media (MPEG streepje, HLS, vloeiende Streaming, schijven) Encoder ondersteunde streaming zonder dat u hoeft opnieuw inpakken in deze indelingen streaming.

Om te profiteren van [dynamische verpakking](media-services-dynamic-packaging-overview.md), moet u als volgt te werk:

- Coderen uw mezzanine (bron)-bestand in een reeks geavanceerde bitsnelheid MP4-bestanden of geavanceerde bitsnelheid vloeiende Streaming-bestanden (de codering stappen worden verderop in deze zelfstudie gedemonstreerd).
- Ten minste één op aanvraag streaming eenheid voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud downloaden. Lees [hoe u de schaal op aanvraag Streaming gereserveerde eenheden](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

Media Services ondersteunt de volgende handelingen uit op aanvraag encoders die worden beschreven in dit artikel:

- [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
- [Media Encoder Premium werkstroom](media-services-encode-asset.md#media-encoder-premium-workflow)

In dit artikel biedt een kort overzicht van op aanvraag encoders media en koppelingen naar artikelen met meer gedetailleerde informatie geven. Het onderwerp bevat ook vergelijking van de encoders.

Houd er rekening mee dat al dan niet standaard elk Media Services-account één actieve codering taak tegelijk hebben kan. Codering eenheden waarmee u kunt meerdere codering taken tegelijkertijd worden uitgevoerd, één voor elke codering gereserveerde eenheid die u kopen hebt, kunt u reserveren. Zie de [schaal codering eenheden](media-services-scale-media-processing-overview.md)voor informatie.

##<a name="media-encoder-standard"></a>Media Encoder Standard

###<a name="how-to-use"></a>Het gebruik van

[Hoe u coderen met Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Indelingen

[Notaties en codecs](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Vooraf ingestelde

Media Encoder standaard is geconfigureerd met een van de encoder vooraf ingestelde beschreven [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Invoer- en uitvoerbereik metagegevens

De invoer metagegevens encoders wordt beschreven [hier](http://msdn.microsoft.com/library/azure/dn783120.aspx).

De metagegevens van de uitvoer encoders wordt beschreven [hier](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>Miniaturen genereren

Lees [hoe u het genereren van miniaturen met Media Encoder standaard](media-services-custom-mes-presets-with-dotnet.md#thumbnails)voor informatie.

###<a name="trim-videos-clipping"></a>Knippen van video's (schermopname)

Zie [het knippen van video's met Media Encoder standaard](media-services-custom-mes-presets-with-dotnet.md#trim_video)voor informatie.

###<a name="create-overlays"></a>Overlays maken

Zie [het maken van overlays met Media Encoder standaard](media-services-custom-mes-presets-with-dotnet.md#overlay)voor informatie.

###<a name="see-also"></a>Zie ook

[De Media Services-blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Media Encoder Premium werkstroom

###<a name="overview"></a>Overzicht

[Inleiding tot Premium codering in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>Het gebruik van

Media Encoder Premium werkstroom is geconfigureerd met het gebruik van complexe werkstromen. Werkstroombestanden kunnen worden gemaakt en bijgewerkt met de functie [Werkstroomontwerper](media-services-workflow-designer.md) .

[Het gebruik van de Premium-codering in Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Bekende problemen

Als uw invoer video bevat geen nog ondertiteling, de uitvoer van activa bevatten steeds een leeg TTML-bestand. 


##<a id="compare_encoders"></a>Encoders vergelijken

###<a id="billing"></a>Facturering meter die worden gebruikt door elke encoder

De naam van de Media-Processor|Toepasselijk prijzen|Notities
---|---|---
**Media Encoder Standard** |ENCODER|Coderen van taken moet betalen op basis van de grootte van de uitvoer activa in GBytes, op het tarief weer dat opgegeven [hier][1], onder de kolom ENCODER.
**Media Encoder Premium werkstroom** |PREMIUM ENCODER|Coderen van taken moet betalen op basis van de grootte van de uitvoer activa in GBytes, op het tarief weer dat opgegeven [hier][1], onder de kolom PREMIUM ENCODER.


In dit gedeelte worden de codering mogelijkheden van **Media Encoder standaard** en **Media Encoder Premium werkstroom**vergeleken.


###<a name="input-containerfile-formats"></a>Input Container/bestandsindelingen

Input Container/bestandsindelingen|Media Encoder Standard|Media Encoder Premium werkstroom
---|---|---
Adobe® Flash® F4V           |Ja|Ja
MXF/SMPTE 377M              |Ja|Ja
GXF                         |Ja|Ja
MPEG-2-Transport Streams    |Ja|Ja
MPEG-2-programma-Streams      |Ja|Ja
MPEG-4/MP4                  |Ja|Ja
Windows Media/AVP           |Ja|Ja
AVI (niet-gecomprimeerde 8-bits/10 bits)|Ja|Ja
3GPP/3GPP2                  |Ja|Nee
Vloeiende Streaming-bestandsindeling (PIFF 1.3)|Ja|Nee
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Ja|Nee
Matroska/WebM               |Ja|Nee
QuickTime (.mov) |Ja|Nee

###<a name="input-video-codecs"></a>Invoer videocodecs

Invoer videocodecs|Media Encoder Standard|Media Encoder Premium werkstroom
---|---|---
AVC 8-bits/10-bits, maximaal 4:2:2, inclusief AVCIntra   |8 bit 4:2:0 en 4:2:2|Ja
Avid DNxHD (in MXF)                                 |Ja|Ja
DVCPro/DVCProHD (in MXF)                            |Ja|Ja
JPEG2000                                            |Ja|Ja
MPEG-2 (maximaal 422 profiel- en hoog niveau, met inbegrip van varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)|Maximaal 422 profiel|Ja
MPEG-1                                              |Ja|Ja
Windows Media Video/VC-1                            |Ja|Ja
Canopus hoofdkantoor/HQX                                      |Nee|Nee
MPEG-4-deel 2                                       |Ja|Nee
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ja|Nee
Apple ProRes 422    |Ja|Nee
Apple ProRes 422 LT |Ja|Nee
Apple ProRes 422 hoofdkantoor |Ja|Nee
Apple ProRes-Proxy|Ja|Nee
Apple ProRes 4444 |Ja|Nee
Apple ProRes 4444 XQ |Ja|Nee

###<a name="input-audio-codecs"></a>Invoer audiocodecs

Invoer audiocodecs|Media Encoder Standard|Media Encoder Premium werkstroom
---|---|---
AES (SMPTE 331 M en 302 M, AES3-2003)        |Nee|Ja
Dolby® E                                    |Nee|Ja
Dolby® digitale (AC3)                        |Nee|Ja
Dolby® digitale Plus (E-AC3)                 |Nee|Ja
AAC (AAC-LC en AAC-HE-AAC-HEv2; maximaal 5.1)|Ja|Ja
MPEG-laag 2|Ja|Ja
MP3 (MPEG-1 Audio Layer 3)|Ja|Ja
Windows Media Audio|Ja|Ja
WAV/PCM|Ja|Ja
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ja|Nee
[Opus](https://en.wikipedia.org/wiki/Opus_(audio_format)) |Ja|Nee
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ja|Nee


###<a name="output-containerfile-formats"></a>Uitvoer Container/bestandsindelingen

Uitvoer Container/bestandsindelingen|Media Encoder Standard|Media Encoder Premium werkstroom
---|---|---
Adobe® Flash® F4V|Nee|Ja
MXF (OP1a, XDCAM en AS02)|Nee|Ja
DPP (inclusief AS11)|Nee|Ja
GXF|Nee|Ja
MPEG-4/MP4|Ja|Ja
MPEG-TS|Ja|Ja
Windows Media/AVP|Nee|Ja
AVI (niet-gecomprimeerde 8-bits/10 bits)|Nee|Ja
Vloeiende Streaming-bestandsindeling (PIFF 1.3)|Nee|Ja

###<a name="output-video-codecs"></a>Uitvoer videocodecs

Uitvoer videocodecs|Media Encoder Standard|Media Encoder Premium werkstroom
---|---|---
AVC (H.264; 8-bits; maximaal profiel op hoog, effenen 5,2; 4 K uiterst HD; AVC binnen)|Alleen 8 bit 4:2:0|Ja
Avid DNxHD (in MXF)|Nee|Ja
MPEG-2 (maximaal 422 profiel- en hoog niveau, met inbegrip van varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)|Nee|Ja
MPEG-1|Nee|Ja
Windows Media Video/VC-1|Nee|Ja
Maken van miniaturen JPEG|Nee|Ja

###<a name="output-audio-codecs"></a>Uitvoer audiocodecs

Uitvoer audiocodecs|Media Encoder Standard|Media Encoder Premium werkstroom
---|---|---
AES (SMPTE 331 M en 302 M, AES3-2003)|Nee|Ja
Dolby® digitale (AC3)|Nee|Ja
Dolby® digitale Plus (E-AC3) snel aan de 7.1|Nee|Ja
AAC (AAC-LC en AAC-HE-AAC-HEv2; maximaal 5.1)|Ja|Ja
MPEG-laag 2|Nee|Ja
MP3 (MPEG-1 Audio Layer 3)|Nee|Ja
Windows Media Audio|Nee|Ja


##<a name="error-codes"></a>Foutcodes  

De volgende tabel bevat foutcodes die kunnen worden geretourneerd voor het geval een fout tijdens de uitvoering van de codering taken opgetreden is.  Als u meer informatie over fout in uw .NET-code, gebruikt u de klasse [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) . Als u meer informatie over fout in de REST-code, gebruikt u de [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.

ErrorDetail.Code|Mogelijke oorzaken gegeven voor fout
-----|-----------------------
Onbekend| Onbekende fout tijdens het uitvoeren van de taak
ErrorDownloadingInputAssetMalformedContent|Categorie van fouten die betrekking heeft op fouten bij het downloaden van de invoer van de activa zoals ongeldige bestandsnamen nul lengte-bestanden, onjuiste indelingen enzovoort.
ErrorDownloadingInputAssetServiceFailure|Categorie van fouten die betrekking heeft op problemen aan de kant van de service - voorbeeld netwerk of opslag fouten tijdens het downloaden.
ErrorParsingConfiguration|Categorie van fouten waar de taak <see cref="MediaTask.PrivateData"/> (configuratie) ongeldig is, bijvoorbeeld de configuratie is niet een geldige systeem vooraf ingestelde of ongeldige XML bevat.
ErrorExecutingTaskMalformedContent|De categorie van fouten tijdens de uitvoering van de taak waar problemen binnen de invoer mediabestanden fout veroorzaken.
ErrorExecutingTaskUnsupportedFormat|Categorie van fouten waar de bestanden die worden geleverd kan niet worden verwerkt door de Mediaprocessor - media opmaken niet worden ondersteund, of niet overeenkomen met de configuratie. Bijvoorbeeld, probeert te maken van een alleen audio-uitvoer van een actief met alleen video
ErrorProcessingTask|De categorie van andere fouten die het Mediaprocessor wordt aangetroffen tijdens het verwerken van de taak die niet gerelateerd aan de inhoud zijn.
ErrorUploadingOutputAsset|Categorie van fouten bij het uploaden van de uitvoer van activa
ErrorCancelingTask|Categorie van fouten bedekt fouten tijdens een poging om de taak annuleren
TransientError|Categorie van fouten bedekt tijdelijke problemen (bv. tijdelijke netwerken problemen met Azure opslag)


Als u de help van het team **Media Services** , open een [tickets ondersteunen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Verwante artikelen

- [Geavanceerde codering taken uitvoeren met het aanpassen van Media Encoder standaard vooraf ingestelde](media-services-custom-mes-presets-with-dotnet.md)
- [Quota en beperkingen](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
