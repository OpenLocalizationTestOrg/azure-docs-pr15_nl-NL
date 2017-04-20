<properties
    pageTitle="Video bijsnijden | Microsoft Azure"
    description="In dit artikel leest hoe u video's met Media Encoder Standard bijsnijden."
    services="media-services"
    documentationCenter=""
    authors="anilmur"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"  
    ms.author="anilmur;juliako;"/>

# <a name="crop-videos-with-media-encoder-standard"></a>Bijsnijden video's met Media Encoder Standard

Media Encoder standaard (MES) kunt u uw invoer video bijsnijden. Bijsnijden, is het proces van het selecteren van een rechthoekig venster in het videoframe en codering van alleen de pixels in dat venster. In het volgende diagram helpt het proces te illustreren.

![Een video bijsnijden](./media/media-services-crop-video/media-services-crop-video01.png)

Stel dat u hebt als invoer een video die heeft een resolutie van 1920 x 1080 pixels (hoogte-breedteverhouding 16:9), maar zwarte balken (pijlers keuzelijsten) aan het links en rechts, zodat alleen een venster 4:3 of 1440 x 1080 pixels actieve videofeed bevat. U kunt gebruiken MES bijsnijden of bewerken van de zwarte balken en de regio 1440 x 1080 coderen.

Bijsnijden in MES is een vooraf verwerking fase, dus de bijsnijden parameters in de codering definitie op de oorspronkelijke invoer video toepassen. Een latere fase codering is en de breedte/hoogte-instellingen toepassen op de *vooraf verwerkt* video, en niet op de oorspronkelijke video. Bij het ontwerpen van de vooraf ingestelde moet u de volgende handelingen uit: (a) selecteert de bijsnijden parameters op basis van de oorspronkelijke invoer video en (b) uw instellingen op basis van de bijgesneden video coderen. Als u niet overeenkomen met uw instellingen naar de bijgesneden video coderen, de uitvoer worden niet zoals u zou verwachten.

Het [volgende](media-services-advanced-encoding-with-mes.md#encoding_with_dotnet) onderwerp wordt uitgelegd hoe een taak coderen met MES maakt en hoe u een aangepaste vooraf ingestelde voor de codering taak opgeven. 

## <a name="creating-a-custom-preset"></a>Een aangepaste vooraf ingestelde maken

In het voorbeeld wordt in het diagram:

1. Oorspronkelijke invoer is 1920 x 1080
1. Deze moeten worden bijgesneden naar een uitvoer van 1440 x 1080, waarin wordt gecentreerd in het kader van de invoer
1. Dit betekent dat er een X-verschuiving van (1920 – 1440) / 2 = 240 en een Y offset nul
1. De breedte en hoogte van de bijsnijdrechthoek zijn 1440 1080, respectievelijk en
1. Klik in het podium coderen de ask is tot drie lagen, zijn oplossingen 1440 x 1080, 960 x 720 en 480 x 360, respectievelijk

###<a name="json-preset"></a>Vooraf ingestelde JSON


    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


##<a name="restrictions-on-cropping"></a>Beperkingen voor het bijsnijden

De functie bijsnijden is handmatige bedoeld. U moet uw invoer video laden in een hulpmiddel voor het bewerken van geschikt waarmee u frames belangrijke selecteren, plaatst u de cursor om te bepalen verschuivingen van de rechthoek bijsnijden om te bepalen de codering vooraf ingestelde die is afgestemd voor dat bepaalde video's, enzovoort. Deze functie is niet bedoeld voor items, zoals: automatisch detecteren en verwijderen van de zwarte letterbox/pillarbox randen in uw video invoer.

Volgende beperkingen zijn van toepassing op de functie bijsnijden. Als deze is voldaan, kan de coderen taak mislukt of een onverwachte uitvoer produceren.

1. De coördinaten en de grootte van de bijsnijdrechthoek moeten voldoen aan binnen de invoer video
1. Bovengenoemde, moet de breedte en hoogte in de instellingen voor het coderen overeenkomen met de bijgesneden video
1. Bijsnijden is van toepassing op video's die zijn opgenomen in Liggend (dat wil zeggen niet van toepassing op video's die zijn opgenomen met een smartphone gehouden in Staand of verticaal)
1. Werkt het best met geleidelijke video vastgelegd met vierkante pixels

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Volgende stap
 
Zie Azure Media Services leerpaden uit om te helpen u meer informatie over handige functies die worden aangeboden door AMS.  

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
