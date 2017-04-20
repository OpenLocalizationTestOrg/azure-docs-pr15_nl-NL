<properties
    pageTitle="Coderen van activa Media Encoder standaard gebruiken met de portal van Azure | Microsoft Azure"
    description="Deze zelfstudie leert u de stappen van de activa Media Encoder standaard gebruiken met de portal van Azure-codering."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Coderen van activa Media Encoder standaard gebruiken met de portal van Azure

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie. 

Wanneer u werkt met Azure Media Services een van de meeste gevallen geavanceerde bitsnelheid streaming levert aan uw klanten. Media Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders). Uw video's om voor te bereiden geavanceerde bitsnelheid streaming, moet u uw bronvideo coderen in multi-bitrate bestanden. U kunt de **Media Encoder standaard** encoder coderen van uw video's.  

Media Services biedt ook dynamische verpakking waarmee u voor het leveren van uw MP4s multi-bitrate in de volgende streaming indelingen: MPEG-streepje, HLS, vloeiende Streaming of schijven, zonder dat u hoeft opnieuw inpakken in deze indelingen streaming. Met dynamische verpakking hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services wordt bouwen en het juiste antwoord op basis van aanvragen van een client fungeren.

Om te profiteren van dynamische verpakking, moet u als volgt te werk:

- Coderen het bronbestand in een reeks multi-bitrate MP4-bestanden (de codering stappen worden gedemonstreerd verderop in dit gedeelte).
- Ten minste één streaming eenheid voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud downloaden. Zie [streaming eindpunten configureren](media-services-portal-vod-get-started.md#configure-streaming-endpoints)voor meer informatie. 

Als u de verwerking van de media verkleinen, raadpleegt u [Dit](media-services-portal-scale-media-processing.md) onderwerp.

## <a name="encode-with-the-azure-portal"></a>Coderen met de portal van Azure

In dit gedeelte worden de stappen beschreven voor het coderen van de inhoud met Media Encoder standaard.

1.  Selecteer in de [portal van Azure](https://portal.azure.com/)uw Azure Media Services-account.
2.  Selecteer in het venster **instellingen van** **activa**.  
2.  Selecteer in het venster **activa** het activum dat u gebruiken wilt voor coderen.
3.  Druk op de knop **coderen** .
4.  Klik in het venster **coderen activa** de "Media Encoder standaard" processor en een vooraf ingestelde te selecteren. Bijvoorbeeld als u uw invoer video heeft een resolutie van 1920 x 1080 pixels weet, klikt u vervolgens kunt u de ' H264 meerdere bitsnelheid 1080p "vooraf ingestelde. Voor meer informatie over de vooraf ingestelde, Zie [Dit](https://msdn.microsoft.com/library/azure/mt269960.aspx) artikel – is het belangrijk om te selecteren van de vooraf ingestelde die meest geschikt is voor uw video invoer. Als u een video van lage resolutie (640 x 360) hebt, wordt u mag worden gemaakt van de standaard "H264 meerdere bitsnelheid 1080p" vooraf ingestelde.
    
    Om eenvoudiger te beheren hebt u een optie van het bewerken van de naam van de activa uitvoer en de naam van de taak.
        
    ![Coderen van activa](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Druk op **maken**.


##<a name="next-step"></a>Volgende stap

U kunt de taakvoortgang van het codering met Azure portal controleren in [Dit](media-services-portal-check-job-progress.md) artikel beschreven.  

##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


