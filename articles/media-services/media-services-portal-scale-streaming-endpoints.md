<properties
    pageTitle=" Schaal streaming eindpunten in de portal van Azure | Microsoft Azure"
    description="Deze zelfstudie leert u de stappen van de schaalbaarheid van streaming eindpunten in de portal van Azure."
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


# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Schaal streaming eindpunten in de portal van Azure

##<a name="overview"></a>Overzicht

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie. 

Wanneer u werkt met Azure Media Services een van de meeste gevallen video via geavanceerde bitsnelheid streaming levert aan uw klanten. Media Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders).

Media Services biedt dynamische verpakking waarmee u uw geavanceerde bitsnelheid codering MP4-inhoud in streaming door Encoder ondersteunde Media Services (MPEG streepje, HLS, vloeiende Streaming, schijven) just-in-time, zonder dat u voor de opslag van vooraf ingepakte versies van elk van deze indelingen streaming hoeft te geven.

Om te profiteren van dynamische verpakking, moet u als volgt te werk:

- Coderen uw mezzanine (bron)-bestand in een reeks geavanceerde bitsnelheid MP4-bestanden (de codering stappen worden verderop in deze zelfstudie gedemonstreerd).  
- Ten minste één streaming eenheid voor het *eindpunt streaming* waaruit u wilt gaan bezorging van uw inhoud maken. De onderstaande stappen uitgelegd hoe u het aantal streaming eenheden wijzigt.

Met dynamische verpakking hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services wordt bouwen en het juiste antwoord op basis van aanvragen van een client fungeren.

Bovendien kunt u bepalen van de capaciteit van de service Endpoint Streaming worden afgehandeld groeiende bandbreedte behoeften door streaming eenheden aan te passen. Het wordt aanbevolen een of meer schaaleenheden voor toepassingen in productieomgeving toewijzen. Streaming eenheden bieden u zowel speciale egress capaciteit die kan worden aangeschaft in stappen van 200 Mbps en extra functionaliteit welke functionaliteit waaronder: [dynamische verpakking](media-services-dynamic-packaging-overview.md), CDN-integratie en geavanceerde configuratie. Zie [streaming eindpunten beheren in de portal van Azure](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

## <a name="scale-streaming-endpoints"></a>De schaal van streaming eindpunten aanpassen

Ga als volgt te werk als u wilt maken en het aantal streaming gereserveerde eenheden wijzigt:

1. Selecteer in de [portal van Azure](https://portal.azure.com/)uw Azure Media Services-account.
2. Selecteer in het venster **Instellingen** **Streaming eindpunten**.
3. Klik op het streaming-eindpunt dat u wilt aanpassen. 
4. Verplaats de schuifregelaar om het aantal streaming eenheden te geven
 
![Streaming eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

De volgende punten zijn van toepassing:

- De toewijzing van een nieuwe streaming eenheden kan ongeveer 20 minuten duren om te voltooien. 
- Op dit moment kunt meer doen met een positieve waarde van de eenheden terug naar geen streaming, uitschakelen op aanvraag streaming voor maximaal per uur.
- De hoogste aantal eenheden die zijn opgegeven voor de periode van 24 uur wordt gebruikt in de berekening van de kosten. Zie voor informatie over prijzen details [Media Services prijzen Details](http://go.microsoft.com/fwlink/?LinkId=275107).

##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


