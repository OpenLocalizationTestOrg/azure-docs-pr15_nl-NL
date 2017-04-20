<properties
    pageTitle="Overzicht van de dynamische verpakking | Microsoft Azure"
    description="Het onderwerp hebt en een overzicht van dynamische verpakking."
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
    ms.date="10/24/2016" 
    ms.author="juliako"/>


# <a name="dynamic-packaging"></a>Dynamische verpakking

##<a name="overview"></a>Overzicht

Microsoft Azure Media Services kunnen worden gebruikt voor het leveren van veel bron mediabestandsindelingen, media-indelingen, streaming en inhoudsbeveiliging bestandsindelingen naar tal van client-technologieën (bijvoorbeeld iOS, XBOX, Silverlight, Windows 8). Deze clients begrijpen verschillende protocollen, bijvoorbeeld een HTTP Live Streaming (HLS) V4-indeling in iOS is vereist en Silverlight en Xbox vereisen vloeiende Streaming. Als u een reeks geavanceerde bitsnelheid (multi-bitrate) hebt MP4-bestanden (ISO Base Media 14496-12) of een reeks geavanceerde bitsnelheid vloeiende Streaming bestanden die u wilt gebruiken voor clients die MPEG streepje, HLS of vloeiende Streaming begrijpen, u moet profiteren van Media Services dynamische verpakking.

Met dynamische verpakking is u hoeft te maken van activa met een verzameling geavanceerde bitsnelheid MP4-bestanden of geavanceerde bitsnelheid vloeiende Streaming bestanden. Klik op basis van de opgegeven notatie in de uitnodiging manifest of fragment, het op aanvraag Streaming server zorgt ervoor dat u de stream ontvangt in het protocol dat u hebt gekozen. Hierdoor, hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services-service wordt bouwen en het juiste antwoord op basis van aanvragen van een client fungeren.

In het volgende diagram ziet u de traditionele codering en statische verpakking werkstroom.

![Statische codering](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

In het volgende diagram ziet u de werkstroom dynamische verpakking.

![Dynamische codering](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]Om te profiteren van dynamische verpakking, moet u eerst ten minste één op aanvraag streaming eenheid ophalen voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud. Lees [hoe u de schaal Media Services](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

##<a name="common-scenario"></a>Veelvoorkomende scenario

1. Upload een invoer-bestand (ook wel een mezzanine-bestand genoemd). Bijvoorbeeld: H.264, MP4 of WMV (voor de lijst met ondersteunde bestandsindelingen Zie [Indelingen ondersteund door de Media Encoder Standard](media-services-media-encoder-standard-formats.md).

1. Uw mezzanine bestand converteren naar H.264 MP4 geavanceerde bitsnelheid sets.

1. De activa met de geavanceerde bitsnelheid MP4 instellen door te maken van de Locator op aanvraag publiceren.

1. De streaming URL's om toegang te streamen van uw inhoud maken.


##<a name="preparing-assets-for-dynamic-streaming"></a>Activa voorbereiden voor dynamische streaming

Als u wilt uw activa voorbereiden voor dynamische streaming hebt u twee opties:

1. [Een basispagina bestand uploaden](media-services-dotnet-upload-files.md).
2. [Gebruik van de Media Encoder standaard encoder H.264 MP4 geavanceerde bitsnelheid sets produceren](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Stream uw inhoud](media-services-deliver-content-overview.md).


##<a id="unsupported_formats"></a>Indelingen die niet worden ondersteund door dynamische verpakking

De volgende bron-bestandsindelingen worden niet ondersteund door dynamische verpakking.

- Dolby digitale mp4-bestanden.
- Dolby digitale vloeiende bestanden.

##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
