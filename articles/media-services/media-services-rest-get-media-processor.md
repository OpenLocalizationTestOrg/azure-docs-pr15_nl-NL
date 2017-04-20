<properties 
    pageTitle="Het maken van een Mediaprocessor | Microsoft Azure" 
    description="Informatie over het maken van een onderdeel van de processor media voor het coderen, opmaak converteren, versleutelen of ontsleutelen media-inhoud voor Azure Media Services." 
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

##<a name="get-mediaprocessor"></a>MediaProcessor ophalen

>[AZURE.NOTE] Tijdens het werken met de Media Services REST API, zijn de volgende punten van toepassing:
>
>Bij het openen van entiteiten in Media Services, moet u specifieke koptekstvelden en waarden instellen in uw HTTP-aanvragen. Zie [configuratie van voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor meer informatie.

>Nadat de verbinding tot stand naar https://media.windows.net, ontvangt u een 301 omleiding precisie van een andere Media-URI voor Services. Mislukken volgende oproepen naar de nieuwe URI zoals is beschreven in [verbinding maken met Media-Services REST API gebruiken](media-services-rest-connect-programmatically.md), moet u ervoor. 


De volgende REST-oproep ziet u hoe u een exemplaar van de processor media met hun naam weergeven (in dit geval, **Media Encoder standaard**). 



    
Vraag:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Antwoord:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Volgende stappen

Als u weet hoe u een exemplaar van de processor media, gaat u naar het onderwerp van [het coderen van activa](media-services-rest-get-started.md) waarin wordt uitgelegd u hoe u met de Media Encoder Standard activa coderen.
