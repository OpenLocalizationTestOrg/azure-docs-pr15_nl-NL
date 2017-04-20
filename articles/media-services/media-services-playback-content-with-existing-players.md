<properties 
    pageTitle="Afspelen van uw inhoud | Microsoft Azure" 
    description="In dit onderwerp vindt u een bestaande spelers waarmee u afspelen uw inhoud kunt." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>De inhoud met bestaande spelers wordt afgespeeld

Azure Media Services ondersteunt veel populaire streaming indelingen, zoals vloeiende Streaming, HTTP Live Streaming en MPEG-streepje. In dit onderwerp bevat verwijzingen naar bestaande spelers die u gebruiken kunt om te testen van uw streams.

>[AZURE.NOTE]Als u wilt afspelen dynamisch verpakt of dynamisch versleutelde inhoud, zorg dan dat u ten minste één streaming eenheid voor het streaming eindpunt waaruit u van plan bent om uit te voeren van uw inhoud te downloaden. Zie voor informatie over de schaalbaarheid van streaming eenheden: [hoe u de schaal van streaming eenheden](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>De Azure portal Media Services inhoud speler

De portal **Azure** biedt een inhoud speler die u gebruiken kunt om te testen van uw video.

Klik op de gewenste video (Zorg ervoor dat deze was [gepubliceerd](media-services-portal-publish.md)) en klik op de knop **afspelen** onderaan in de portal.

Enkele punten van toepassing:

- De **Inhoud MEDIASPELER SERVICES** wordt afgespeeld uit het standaardbeleid voor streaming eindpunt. Als u wilt laten afspelen van een niet-standaard streaming eindpunt, gebruikt u een andere speler. Bijvoorbeeld [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Azure MediaPlayer

[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) gebruiken om het afspelen te uw inhoud (wissen of beveiligde) in een van de volgende indelingen:

- Vloeiende Streaming
- MPEG-STREEPJE
- HLS
- Geleidelijke MP4


###<a name="flash-player"></a>Flash Player

####<a name="aes-encrypted-with-token"></a>AES-versleuteld met Token

[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Silverlight-spelers

####<a name="monitoring"></a>Cmdlets voor controle

[http://SMF.cloudapp.NET/HealthMonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>PlayReady met Token

[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>STREEPJE spelers

[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Andere

Test HLS-URL's die u kunt ook gebruiken:

- **Safari** op een iOS-apparaat of
- **3ivx HLS Player** op Windows.

##<a name="developing-video-players"></a>Video-spelers ontwikkelen

Zie voor informatie over het ontwikkelen van uw eigen spelers [video spelers ontwikkelen](media-services-develop-video-players.md)




##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
