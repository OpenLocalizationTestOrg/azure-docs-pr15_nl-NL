<properties 
    pageTitle="Media Encoder Premium werkstroom notaties en codecs | Microsoft Azure" 
    description="In dit onderwerp biedt een overzicht van Media Encoder Premium werkstroom-indelingen notaties en codecs" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium werkstroom notaties en codecs


>[AZURE.NOTE]Voor premium encoder vragen, e-mepd op Microsoft.com.
>
>Media Encoder Premium media werkstroomverwerking besproken in dit onderwerp is niet beschikbaar in China. 

Dit document bevat een lijst met invoer en uitvoer-bestandsindelingen en codecs die worden ondersteund door de openbare preview-versie van de **Media Encoder Premium werkstroom** encoder.

[Media Encoder Premium Worflow invoer-indelingen en Codecs](#input_formats)

[Media Encoder Premium Worflow uitvoerindelingen en Codecs](#output_formats)

**Media Encoder Premium werkstroom** ondersteunt ondertiteling die in [deze](#closed_captioning) sectie worden beschreven. 


##<a id="input_formats"></a>Media Encoder Premium werkstroom Input notaties en Codecs

De volgende sectie bevat de codecs en bestand indelingen die deze Mediaprocessor ondersteund als invoer.

###<a name="input-containerfile-formats"></a>Input Container/bestandsindelingen

- Adobe® Flash® F4V
- MXF/SMPTE 377M
- GXF
- MPEG-2-Transport Streams
- MPEG-2-programma-Streams
- MPEG-4/MP4
- Windows Media/AVP
- AVI (niet-gecomprimeerde 8-bits/10 bits)

###<a name="input-video-codecs"></a>Invoer videocodecs

- AVC 8-bits/10-bits, maximaal 4:2:2, inclusief AVCIntra
- Avid DNxHD (in MXF)
- DVCPro/DVCProHD (in MXF)
- JPEG2000
- MPEG-2 (maximaal 422 profiel- en hoog niveau, met inbegrip van varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)
- MPEG-1
- Windows Media Video/VC-1

###<a name="input-audio-codecs"></a>Invoer audiocodecs

- AES (SMPTE 331 M en 302 M, AES3-2003)
- Dolby® E
- Dolby® digitale (AC3)
- AAC (AAC-LC en AAC-HE-AAC-HEv2; maximaal 5.1)
- MPEG-laag 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio
- WAV/PCM
 
##<a id="output_format"></a>Werkstroom uitvoerindelingen voor Media Encoder Premium en Codecs

De volgende sectie bevat de codecs en bestandsindelingen die worden ondersteund als uitvoer van deze Mediaprocessor.

###<a name="output-containerfile-formats"></a>Uitvoer Container/bestandsindelingen

- Adobe® Flash® F4V
- MXF (OP1a, XDCAM en AS02)
- DPP (inclusief AS11)
- GXF
- MPEG-4/MP4
- Windows Media/AVP
- AVI (niet-gecomprimeerde 8-bits/10 bits)
- Vloeiende Streaming-bestandsindeling (PIFF 1.3)
- MPEG-TS 


###<a name="output-video-codecs"></a>Uitvoer videocodecs

- AVC (H.264; 8-bits; maximaal profiel op hoog, effenen 5,2; 4 K uiterst HD; AVC binnen)
- Avid DNxHD (in MXF)
- DVCPro/DVCProHD (in MXF)
- MPEG-2 (maximaal 422 profiel- en hoog niveau, met inbegrip van varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)
- MPEG-1
- Windows Media Video/VC-1
- Maken van miniaturen JPEG

###<a name="output-audio-codecs"></a>Uitvoer audiocodecs

- AES (SMPTE 331 M en 302 M, AES3-2003)
- Dolby® digitale (AC3)
- Dolby® digitale Plus (E-AC3) snel aan de 7.1
- AAC (AAC-LC en AAC-HE-AAC-HEv2; maximaal 5.1)
- MPEG-laag 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio

##<a id="closed_captioning"></a>Ondersteuning voor ondertiteling

Op te nemen, ondersteuning biedt voor **Media Encoder Premium werkstroom** :

1. SCC-bestanden
1. SMPTE-TT bestanden
1. De voor- of als de aanvullende gegevens in MXF/GXF bestanden ten uitgevoerd als de gebruikersgegevens (SEI berichten H.264 elementaire streams, ATSC/53, SCTE20) CEA-608/CEA-708 –
1. STL ondertitel bestanden

Klik op uitvoer zijn de volgende opties beschikbaar:

1. CEA-608 CEA-708 vertaling
1. CEA-608/CEA-708 doorgegeven (ingesloten in SEI berichten met H.264 elementaire streams of uitgevoerd als aanvullende gegevens in MXF-bestanden)
1. SCC
1. SMPTE Timed Text (uit de bron CEA-608 per SMPTE RP2052; inclusief DFXP bestand maken)
1. Sorteren ondertitel-bestand
1. DVB ondertitel streams

Opmerking: niet voor alle van de bovenstaande uitvoerindelingen worden ondersteund voor de bezorging van via streaming in Azure Media Services.

##<a name="known-issues"></a>Bekende problemen

Als uw invoer video bevat geen nog ondertiteling, de uitvoer van activa bevatten steeds een leeg TTML-bestand. 


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
