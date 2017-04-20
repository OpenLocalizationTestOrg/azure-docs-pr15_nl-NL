<properties 
    pageTitle="Gids voor probleemoplossing voor live streaming | Microsoft Azure" 
    description="Dit onderwerp biedt suggesties op live streaming problemen oplossen." 
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
    ms.date="10/12/2016"  
    ms.author="juliako"/>

#<a name="troubleshooting-guide-for-live-streaming"></a>De gids voor probleemoplossing voor live streaming

Dit onderwerp biedt suggesties op sommige live streaming problemen oplossen.

## <a name="issues-related-to-on-premises-encoders"></a>Problemen met on-premises implementatie encoders 

Deze sectie vindt u suggesties over het oplossen van problemen met betrekking tot on-premises implementatie encoders die zijn geconfigureerd voor het verzenden van een één bitsnelheid naar AMS kanalen die zijn ingeschakeld voor het coderen van live.

###<a name="problem-would-like-to-see-logs"></a>Probleem: Zou willen zien Logboeken 

- **Mogelijke probleem**: kan niet zoeken encoder logboeken die mogelijk bij het oplossen van problemen foutopsporing.
    
    - **Telestream Wirecast**: vindt u doorgaans Logboeken onder C:\Users\{username} \AppData\Roaming\Wirecast\ 
    - **Elementaire Live**: vindt u koppelingen naar Logboeken op de beheerportal heeft. Klik op **stat**en klik vervolgens **Logboeken**. Klik op de pagina **Logboekbestanden** ziet u een lijst met Logboeken voor alle LiveEvent items; Selecteer de overeenkomen met de huidige sessie. 
    - **Flash Media Live Encoder**: U kunt de **Log Directory...** vinden door te schuiven naar het tabblad **Log codering** .
    
###<a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Probleem: Er is geen optie voor het uitvoeren van een geleidelijke stream

- **Mogelijke probleem**: de gebruikte encoder niet automatisch interliniëring ongedaan maken. 

    **Stappen voor probleemoplossing**: zoek naar een opgeheven ineengestrengeld optie binnen de encoder-interface. Eenmaal de-interlacing is ingeschakeld, opnieuw controleren voor geleidelijke uitvoerinstellingen. 
 
###<a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Probleem: Verschillende encoder uitvoerinstellingen hebt geprobeerd en nog steeds geen verbinding kunt maken. 

- **Mogelijke probleem**: Azure codering kanaal is niet correct opnieuw ingesteld. 

    **Stappen voor probleemoplossing**: Zorg ervoor dat de encoder is niet langer drukken naar AMS, stoppen en opnieuw instellen van het kanaal. Zodra opnieuw uit te voeren, kunt u proberen uw encoder met de nieuwe instellingen. Als dit nog steeds worden niet gecorrigeerd door het probleem, maakt u een nieuw kanaal geheel, soms kanalen kunnen beschadigd nadat verscheidene mislukte pogingen.  

- **Mogelijke probleem**: het GOP grootte of belangrijke frame instellingen niet optimaal zijn. 

    **Stappen voor probleemoplossing**: de grootte of hoofdframe interval aanbevolen GOP is 2 seconden ingedrukt. Sommige encoders berekenen met deze instelling in het aantal frames, terwijl de anderen seconden gebruiken. Bijvoorbeeld: bij het uitvoeren van 30fps, de grootte GOP zou 60 frames, die is gelijk aan 2 seconden ingedrukt.  
     
- **Mogelijke probleem**: gesloten poorten worden geblokkeerd door de stream. 

    **Stappen voor probleemoplossing**: wanneer streaming via RTMP, controleert u of firewall en proxy-instellingen om te bevestigen dat uitgaande poort 1935 en 1936 geopend zijn. Wanneer u met RTP-streaming, bevestig dat uitgaande poort 2010 geopend is. 


###<a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>Probleem: Tijdens het configureren van de encoder naar stroom met het RTP-protocol, is er geen centrale plaats om in te voeren een hostnaam. 

- **Mogelijke probleem**: veel RTP encoders niet toestaan voor hostnamen en een IP-adres moet worden opgehaald.  

    **Stappen voor probleemoplossing**: om het IP-adres hebt gevonden, open een opdrachtprompt op elke computer. Ga hiervoor in Windows opent het startpictogram uitvoeren (WIN + R) en typt u 'cmd' om te openen.  

    Zodra de opdrachtprompt geopend is, typt u "Ping [AMS Host Name]". 

    De hostnaam kan worden afgeleid door het weglaten van het poortnummer van de Azure nemen URL, zoals gemarkeerd in het volgende voorbeeld: 

    RTP://Test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 / 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

###<a name="problem-unable-to-playback-the-published-stream"></a>Probleem: Kan niet worden afspelen de gepubliceerde stream.
 
- **Mogelijke probleem**: Er is geen Streaming eindpunt die wordt uitgevoerd, of er is geen toegewezen streaming eenheden (schaaleenheden). 

    **Stappen voor probleemoplossing**: Ga naar het tabblad 'Eindpunt Streaming' in het hulpmiddel AMSE en er is een Streaming-eindpunt met één streaming eenheid bevestigen. 
    


>[AZURE.NOTE] Verzenden als na het uitvoeren van de stappen voor probleemoplossing die u nog steeds niet lukt om via streamen, een ondersteuningsticket met behulp van de Azure portal.

##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
