<properties 
    pageTitle="Het maken van een Mediaprocessor | Microsoft Azure" 
    description="Informatie over het maken van een onderdeel van de processor media voor het coderen, opmaak converteren, versleutelen of ontsleutelen media-inhoud voor Azure Media Services. Voorbeelden van code worden geschreven in C# en gebruiken van de Media Services SDK voor .NET." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="how-to-get-a-media-processor-instance"></a>Hoe u: een exemplaar van de Processor Media ophalen

> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [REST](media-services-rest-get-media-processor.md)


##<a name="overview"></a>Overzicht

Maak in Media Services die een Mediaprocessor een onderdeel waarmee een specifieke verwerkingstaak is, zoals codering, conversie, coderen, of decoderen media-inhoud. Gewoonlijk maakt u een Mediaprocessor bij het maken van een taak coderen, versleutelen of converteren van de indeling van media-inhoud.

De volgende tabel bevat de naam en beschrijving van elke beschikbare media-processor.

De naam van de Media-Processor|Beschrijving|Meer informatie
---|---|---
Media Encoder Standard|Biedt standaard mogelijkheden voor het op aanvraag-codering. |[Overzicht en vergelijking van Azure op aanvraag Media Encoders](media-services-encode-asset.md)
Media Encoder Premium werkstroom|Kunt u codering taken met Media Encoder Premium-werkstroom uitvoeren.|[Overzicht en vergelijking van Azure op aanvraag Media Encoders](media-services-encode-asset.md)
Azure Media indexering| Kunt u media-bestanden en inhoud doorzoekbaar maken, evenals genereren gesloten closed captioning nummers en trefwoorden.|[Azure Media indexering](media-services-index-content.md)
Azure Media Hyperlapse (preview)|Kunt u de 'dalen"in uw video met video stabilisatie vloeiend. Ook kunt u uw inhoud naar een verbruikbare clip versnellen.|[Azure Media Hyperlapse](media-services-hyperlapse-content.md)
Azure Media Encoder|Afgeschreven
Opslag ontsleutelen| Afgeschreven|
Azure Media Packager|Afgeschreven|
Azure Media coderen|Afgeschreven|

##<a name="get-media-processor"></a>Mediaprocessor ophalen

Hoe kom ik aan een exemplaar van de processor media ziet u de volgende methode. Het voorbeeld wordt ervan uitgegaan van het gebruik van de module op gebruikersniveau variabele **_context** om te verwijzen naar de servercontext zoals is beschreven in de sectie [hoe: verbinding maken met Media-Services via programmacode](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
        return processor;
    }


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-steps"></a>Volgende stappen

Als u weet hoe u een exemplaar van de processor media, gaat u naar het onderwerp van [het coderen van activa](media-services-dotnet-encode-with-media-encoder-standard.md) waarin wordt uitgelegd u hoe u met de Media Encoder Standard activa coderen.


