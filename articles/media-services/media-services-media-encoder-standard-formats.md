<properties 
    pageTitle="Media Encoder standaardopmaak en codecs" 
    description="Dit onderwerp biedt een overzicht van Media Encoder standaardopmaak en codecs." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder standaardopmaak en Codecs


Dit document bevat een lijst met de meest voorkomende importeren en exporteren-bestandsindelingen die u met Media Encoder standaard gebruiken kunt.


##<a name="input-containerfile-formats"></a>Input Container/bestandsindelingen

Bestandsindelingen (bestandsextensies)|Ondersteund
---|---|---|---
FLV-bestand (met H.264 en AAC codecs) (FLV)          |Ja 
MXF (.mxf)                  |Ja 
GXF (.gxf)                  |Ja 
MPEG2-PS, MPEG2-TS, 3GP (.ts, PS, .3gp, .3gpp, .mpg)   |Ja 
Windows mediavideo (WMV) / AVP (.wmv, .asf) |Ja 
AVI (niet-gecomprimeerde 8-bits/10 bits) (.avi)|Ja 
MP4 (.mp4, .m4a, .m4v) / ISMV (.isma, .ismv)|Ja 
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Ja 
Matroska/WebM (.mkv)        |Ja 
GOLF/WAV (.wav) |Ja 
QuickTime (.mov) |Ja

>[AZURE.NOTE] Een lijst met de meer veelvoorkomende bestandsextensies luidt. Media Encoder standaard ondersteunt dergelijke (bijvoorbeeld: .m2ts, .mpeg2video, .qt). Als u probeert te coderen van een bestand en verschijnt er een foutbericht over de indeling niet wordt ondersteund, geef feedback [hier](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>Audio-indelingen in invoer containers 

Media Encoder standaard uitvoering van de volgende audio-indelingen in invoer containers worden ondersteund:

- MXF, GXF en QuickTime-bestanden die beschikt over audio nummers met afwisselende stereo of 5.1 voorbeelden

of

- MXF GXF QuickTime-bestanden, en waarbij het geluid wordt uitgevoerd als afzonderlijke PCM nummers, maar de kanaaltoewijzing (naar stereo of 5.1) kan worden afgeleid van de metagegevens van het bestand

Houd er rekening mee dat er geen ondersteuning voor het expliciete/een gebruiker opgegeven kanaaltoewijzing krijgt in de nabije toekomst.


##<a name="input-video-codecs"></a>Invoer videocodecs

Invoer videocodecs|Ondersteund
---|---|---|---
AVC 8-bits/10-bits, maximaal 4:2:2, inclusief AVCIntra   |8 bit 4:2:0 en 4:2:2 
Avid DNxHD (in MXF)                                 |Ja 
DVCPro/DVCProHD (in MXF)                            |Ja 
Digitale video (DV) (in AVI-bestanden)                   |Ja
JPEG 2000                                           |Ja 
MPEG-2 (maximaal 422 profiel- en hoog niveau, met inbegrip van varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)|Maximaal 422 profiel 
MPEG-1                                              |Ja 
VC-1/WMV9                                           |Ja 
Canopus hoofdkantoor/HQX                                      |Nee 
MPEG-4-deel 2                                       |Ja 
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ja 
Niet-gecomprimeerd YUV420 of mezzanine                   |Ja
Apple ProRes 422                                    |Ja
Apple ProRes 422 LT |Ja
Apple ProRes 422 hoofdkantoor |Ja
Apple ProRes-Proxy|Ja
Apple ProRes 4444 |Ja
Apple ProRes 4444 XQ |Ja



##<a name="input-audio-codecs"></a>Invoer audiocodecs

Invoer audiocodecs|Ondersteund
---|---|---|---
AAC (AAC-LC en AAC-HE-AAC-HEv2; maximaal 5.1)|Ja 
MPEG-laag 2|Ja 
MP3 (MPEG-1 Audio Layer 3)|Ja 
Windows Media Audio|Ja 
WAV/PCM|Ja 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ja 
[Opus](http://go.microsoft.com/fwlink/?LinkId=822667) |Ja 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ja 
AMR (geavanceerde meerdere snelheid)|Ja
AES (SMPTE 331 M en 302 M, AES3-2003)        |Nee 
Dolby® E                                    |Nee 
Dolby® digitale (AC3)                        |Nee 
Dolby® digitale Plus (E-AC3)                 |Nee 


##<a name="output-formats-and-codecs"></a>Uitvoerindelingen en codecs

De volgende tabel bevat de codecs en bestandsindelingen die worden ondersteund voor exporteren.


Bestandsindeling|Video-Codec|Audio-Codec
---|---|---
MP4 <br/><br/>(inclusief multi-bitrate MP4 containers) |H.264 (hoog, Main en basislijn profielen)|AAC-LC, HE-AAC v1, HE-AAC v2 
MPEG2-TS |H.264 (hoog, Main en basislijn profielen)|AAC-LC, HE-AAC v1, HE-AAC v2 



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook

[De inhoud op aanvraag met Azure mediaservices-codering](media-services-encode-asset.md)

[Hoe u coderen met Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)
