<properties 
    pageTitle="Het uitvoeren van live streaming met on-premises encoders met behulp van de Azure portal | Microsoft Azure" 
    description="Deze zelfstudie leert u de stappen voor het maken van een kanaal dat is geconfigureerd voor de bezorging van een Pass Through." 
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
    ms.topic="get-started-article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-with-on-premise-encoders-using-the-azure-portal"></a>Het uitvoeren van live streaming met on-premises encoders met behulp van de Azure portal

> [AZURE.SELECTOR]
- [Portal]( media-services-portal-live-passthrough-get-started.md)
- [.NET]( media-services-dotnet-live-encode-with-onpremises-encoders.md)
- [REST]( https://msdn.microsoft.com/library/azure/dn783458.aspx)

Deze zelfstudie leert u de stappen van het gebruik van de Azure-portal een **kanaal** dat is geconfigureerd voor de bezorging van een Pass Through maken. 

##<a name="prerequisites"></a>Vereisten voor

De volgende zijn vereist voor het voltooien van de zelfstudie:

- Een Azure-account. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie. 
- Een Media Services-account. Zie [het maken van een Media Services-Account](media-services-portal-create-account.md)maken van een Media Services-account.
- Een webcam. Bijvoorbeeld [Telestream Wirecast encoder](http://www.telestream.net/wirecast/overview.htm).

Het is raadzaam te controleren van de volgende artikelen:

- [De voor- en Live Encoders ondersteund Azure mediaservices RTMP](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Overzicht van Live indien gestoomd Azure Media Services gebruiken](media-services-manage-channels-overview.md)
- [Live streaming met on-premises encoders die multi-bitrate streams maken](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Live streaming gebruikelijk

De volgende stappen beschrijven taken die nodig zijn bij het maken van algemene live streaming toepassingen die gebruikmaken van kanalen die zijn geconfigureerd voor de bezorging van Pass Through. Deze zelfstudie wordt getoond hoe u een Pass Through-kanaal en live gebeurtenissen maken en beheren.

1. Een videocamera verbinden met een computer. Starten en configureren van een on-premises implementatie live encoder dat een multi-RTMP of gefragmenteerd MP4 bitsnelheid oplevert. Zie [Azure Media Services RTMP ondersteuning en Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824)voor meer informatie.
    
    Deze stap kan ook worden uitgevoerd nadat u uw kanaal hebt gemaakt.

1. Maken en een Pass Through-kanaal starten.
1. Ophalen het kanaal nemen URL. 

    De URL ingest wordt gebruikt door de live encoder de stream versturen naar het kanaal.
1. Hiermee kunt u de URL van de Preview-versie kanaal ophalen. 

    Gebruik deze URL om te controleren of uw kanaal is juist de live gegevensstroom ontvangt.

3. Maak een live gebeurtenis en programma's. 

    Wanneer u met de portal van Azure, maken van een live gebeurtenis ook Hiermee maakt u een actief. 
      
    >[AZURE.NOTE]Zorg ervoor dat ten minste één streaming gereserveerde eenheid op de streaming eindpunt waaruit u inhoud wilt hebben.
1. De gebeurtenis/programma te starten wanneer u klaar bent voor het starten van streaming en archiveren.
2. (Optioneel) het live encoder kunt signaal ontvangen een advertentie starten. De advertentie wordt ingevoegd in de uitvoer-stream.
1. De gebeurtenis en programma's stoppen wanneer u wilt stoppen streaming en archiveren van de gebeurtenis.
1. Verwijderen van de gebeurtenis en programma's (en desgewenst verwijderen de activa).     

>[AZURE.IMPORTANT] Raadpleeg [Live streaming met on-premises encoders die multi-bitrate streams maken](media-services-live-streaming-with-onprem-encoders.md) voor meer informatie over concepten en overwegingen die betrekking hebben op live streaming met on-premises encoders en Pass Through kanalen.

##<a name="to-view-notifications-and-errors"></a>Meldingen en fouten weergeven

Als u weergeven van meldingen en fouten geproduceerd door de Azure-portal wilt, klik u op het pictogram in het systeemvak.

![Meldingen](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##<a name="configure-streaming-endpoints"></a>Streaming eindpunten configureren 

Media Services biedt dynamische verpakking, waarmee u voor het leveren van uw MP4s multi-bitrate in de volgende streaming indelingen: MPEG-streepje, HLS, vloeiende Streaming of schijven, zonder dat u hoeft om te verpakken in deze indelingen streaming. Met dynamische verpakking hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services genereert en het juiste antwoord op basis van aanvragen van een client fungeert.

Om te profiteren van dynamische verpakking, moet u ten minste één streaming eenheid voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud te downloaden.  

Ga als volgt te werk als u wilt maken en het aantal streaming gereserveerde eenheden wijzigt:

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
1. Klik in het venster **Instellingen** op **Streaming eindpunten**. 

2. Klik op de standaard streaming eindpunt. 

    Het venster **Standaard STREAMING EINDPUNTDETAILS** wordt geopend.

3. Als u wilt opgeven van het aantal streaming eenheden, schuift u de schuifregelaar **Streaming eenheden** .

    ![Streaming eenheden](./media/media-services-portal-passthrough-get-started/media-services-streaming-units.png)

4. Klik op de knop **Opslaan** als uw wijzigingen wilt opslaan.

    >[AZURE.NOTE]De toewijzing van een nieuwe eenheden kan maximaal 20 minuten duren.
    
##<a name="create-and-start-pass-through-channels-and-events"></a>Maken en starten Pass Through-kanalen en gebeurtenissen

Een kanaal is gekoppeld aan gebeurtenissen/programma's waarmee u de publicatie en opslag van segmenten in een live gegevensstroom. Kanalen beheren gebeurtenissen. 
    
Het aantal uren die u wilt behouden de opgenomen inhoud voor het programma voor de lengte **Archief venster** instelt, kunt u opgeven. Deze waarde kan worden ingesteld van een minimum van 5 minuten tot maximaal 25 uur. Archief venster lengte bepaalt ook dat de maximale hoeveelheid tijd clients terug in tijd kunt zoeken vanaf de huidige live positie. Gebeurtenissen kunnen worden uitgevoerd op de opgegeven tijdsduur, maar de inhoud die achter de lengte van het venster valt continu worden verwijderd. Deze waarde van deze eigenschap wordt ook bepaalt hoe lang de client manifesten te vergroten.

Elke gebeurtenis is gekoppeld aan een actief. Als u wilt publiceren de gebeurtenis, moet u een locator OnDemand voor de bijbehorende activa maken. Deze locator ondervindt, kunt u de URL van een streaming dat u naar uw klanten kan leveren maken.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve gebeurtenissen, zodat u kunt meerdere archieven van de dezelfde binnenkomende stream maken. Hiermee kunt u publiceren en archiveren van verschillende gedeelten van een gebeurtenis naar wens. Het vereiste voor uw bedrijf is bijvoorbeeld 6 uur van een programma archiveren, maar alleen laatste 10 minuten uitzenden. Hiervoor moet u twee gelijktijdig actieve programma's te maken. Een programma is ingesteld op het archiveren van 6 uur van de gebeurtenis, maar het programma niet wordt gepubliceerd. De andere programma's is ingesteld op archiveren voor 10 minuten en dit programma is gepubliceerd.

U moet niet opnieuw gebruiken bestaande live gebeurtenissen. In plaats daarvan maken en start een nieuwe gebeurtenis voor elke gebeurtenis.

Start de gebeurtenis wanneer u klaar bent voor het starten van streaming en archiveren. Het programma stoppen wanneer u wilt stoppen streaming en archiveren van de gebeurtenis. 

Als u wilt verwijderen gearchiveerde inhoud, stoppen en verwijderen van de gebeurtenis en verwijder vervolgens het bijbehorende actief. Activa kan niet worden verwijderd als deze wordt gebruikt door een gebeurtenis; de gebeurtenis moet eerst worden verwijderd. 

Zelfs nadat u stoppen en de gebeurtenis verwijdert, is de gebruikers zou kunnen uw gearchiveerde inhoud streamen als een video op aanvraag, voor, zo lang maken als u de activa niet verwijderen.

Als u wilt de gearchiveerde inhoud behouden, maar niet beschikbaar voor streaming, verwijdert u de streaming locator.

###<a name="to-use-the-portal-to-create-a-channel"></a>Gebruik van de portal een kanaal maken 

In dit gedeelte ziet u hoe u de optie **Snelle maken** met een Pass Through-kanaal maken.

Zie voor meer informatie over Pass Through-kanalen, [Live streaming met on-premises encoders die multi-bitrate gegevensstromen maken](media-services-live-streaming-with-onprem-encoders.md).

1. Selecteer in de [portal van Azure](https://portal.azure.com/)uw Azure Media Services-account.
2. Klik in het venster **Instellingen** op **Live streaming**. 

    ![Aan de slag](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    Het venster **Live streaming** wordt weergegeven.

3. Klik op **Snelle maken** als u wilt een Pass Through-kanaal maken met de RTMP protocol nemen.

    Het venster **Nieuw kanaal maken** wordt weergegeven.
4. Geef een naam op voor het nieuwe kanaal en klik op **maken**. 

    Hierdoor wordt een Pass Through-kanaal gemaakt met de RTMP protocol nemen.

##<a name="create-events"></a>Gebeurtenissen maken

1. Selecteer een kanaal waarnaar u wilt een gebeurtenis toevoegen.
2. Druk op de knop **Live gebeurtenis** .

![Gebeurtenis](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)


##<a name="get-ingest-urls"></a>Get nemen URL 's

Wanneer het kanaal is gemaakt, kunt u een URL's die u naar de live encoder bieden wilt nemen. De encoder wordt deze URL's om een live gegevensstroom invoer.

![Gemaakt](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##<a name="watch-the-event"></a>Bekijk de gebeurtenis

De gebeurtenis te bekijken, klik op **controle** in de portal van Azure of de streaming URL kopiëren en gebruiken van een speler van uw keuze. 
 
![Gemaakt](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Live-gebeurtenis automatisch worden geconverteerd naar inhoud op aanvraag wanneer gestopt.

##<a name="clean-up"></a>Opschonen

Zie voor meer informatie over Pass Through-kanalen, [Live streaming met on-premises encoders die multi-bitrate gegevensstromen maken](media-services-live-streaming-with-onprem-encoders.md).

- Een kanaal kan het worden gestopt alleen wanneer alle gebeurtenissen/programma's op het kanaal zijn gestopt.  Nadat het kanaal is gestopt, wordt deze niet alle kosten oplopen. Wanneer u opnieuw start moet, heeft dit hetzelfde URL nemen zodat u niet nodig om te configureren, uw encoder.
- Een kanaal kan alleen worden verwijderd als alle live gebeurtenissen op het kanaal zijn verwijderd.

##<a name="view-archived-content"></a>Gearchiveerde inhoud weergeven

Zelfs nadat u stoppen en de gebeurtenis verwijdert, is de gebruikers zou kunnen uw gearchiveerde inhoud streamen als een video op aanvraag, voor, zo lang maken als u de activa niet verwijderen. Activa kan niet worden verwijderd als deze wordt gebruikt door een gebeurtenis; de gebeurtenis moet eerst worden verwijderd. 

Uw activa beheren, selecteer **instelling** en klik op **activa**.

![Activa](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##<a name="next-step"></a>Volgende stap

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
