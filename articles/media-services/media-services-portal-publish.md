<properties
    pageTitle="  Publiceren van inhoud in de portal van Azure | Microsoft Azure"
    description="Deze zelfstudie leert u de stappen van de inhoud met de portal van Azure publiceren."
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

# <a name="publish-content-with-the-azure-portal"></a>Inhoud in de portal van Azure publiceren

> [AZURE.SELECTOR]
- [Portal](media-services-portal-publish.md)
- [.NET](media-services-deliver-streaming-content.md)
- [REST](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>Overzicht

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie. 

Als u wilt uw gebruikers-voorzien van een URL die kan worden gebruikt om te streamen of downloaden van uw inhoud, moet u eerst "publiceren' uw activa door te maken van een locator. Locator bieden toegang tot bestanden in de activa. Media Services ondersteunt twee soorten Locator: 

- Streaming (OnDemandOrigin) Locator, die wordt gebruikt voor geavanceerde streaming (bijvoorbeeld naar stream MPEG streepje, HLS of vloeiende Streaming). Als u wilt maken van een streaming locator uw activa, moeten een bestand .ism bevatten. 
- Geleidelijke (SA's)-Locator, die wordt gebruikt voor de bezorging van video geleidelijke gedownload.


De URL van een streaming heeft de volgende indeling en u deze kunt gebruiken om af te spelen vloeiende Streaming activa.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Maak een URL-streaming HLS door toevoegen (opmaak = m3u8-aapl) naar de URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Maak een MPEG-streepje streaming URL door toevoegen (opmaak = mpd-tijd-csf) naar de URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

De URL van een SA's heeft de volgende indeling.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Zie [overzicht van de bezorgen](media-services-deliver-content-overview.md)voor meer informatie.

>[AZURE.NOTE] Als u de portal gebruikt bij het maken van Locator vóór maart 2015, zijn Locator met een vervaldatum twee jaar gemaakt.  

Als u een vervaldatum voor een locator bijwerken, [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) of [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) -API's te gebruiken. Houd er rekening mee dat wanneer u de vervaldatum van een locator SA's bijwerkt, de URL wordt gewijzigd.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Gebruik van de portal activa publiceren

Ga als volgt te werk als u de portal wilt publiceren activa:

1. Selecteer in de [portal van Azure](https://portal.azure.com/)uw Azure Media Services-account.
1. Selecteer **Instellingen** > **activa**.
1. Selecteer het activum die u wilt publiceren.
1. Klik op de knop **publiceren** .
1. Selecteer het type locator.
2. Druk op **toevoegen**.

    ![Publiceren](./media/media-services-portal-vod-get-started/media-services-publish1.png)

De URL wordt toegevoegd aan de lijst met **URL's die zijn gepubliceerd**.

## <a name="play-content-from-the-portal"></a>Inhoud van de portal afspelen

De Azure portal biedt een inhoud speler die u gebruiken kunt om te testen van uw video.

Klik op de gewenste video en klik op de knop **afspelen** .

![Publiceren](./media/media-services-portal-vod-get-started/media-services-play.png)

Enkele punten van toepassing:

- Zorg ervoor dat de video is gepubliceerd.
- Deze **MediaPlayer** wordt afgespeeld uit het standaardbeleid voor streaming eindpunt. Als u wilt laten afspelen van een niet-standaard streaming eindpunt, klikt u op de URL kopiëren en het gebruik van een andere speler. Bijvoorbeeld [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- Het streaming eindpunt waaruit u streaming moet worden uitgevoerd.  
- Om te stromen van een streaming-eindpunt, moet u ten minste één streaming eenheid toevoegen. Zie voor meer informatie [in dit](media-services-portal-scale-streaming-endpoints.md) onderwerp.   

##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


