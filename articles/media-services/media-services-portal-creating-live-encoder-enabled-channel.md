<properties 
    pageTitle="Het uitvoeren van live streaming Azure Media Services gebruiken om te maken van multi-bitrate streams in de portal van Azure | Microsoft Azure" 
    description="Deze zelfstudie leert u de stappen voor het maken van een kanaal bevindt waarvoor een live met één bitsnelheid ontvangt en deze op multi-bitsnelheid met behulp van de Azure portal gecodeerd." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
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


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>Het uitvoeren van live streaming Azure Media Services gebruiken om te maken van multi-bitrate streams in de portal van Azure

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Deze zelfstudie leert u de stappen voor het maken van een **kanaal** bevindt waarvoor een live met één bitsnelheid ontvangt en deze op multi-bitsnelheid gecodeerd.

>[AZURE.NOTE]Zie [Live streaming met behulp van Azure Media Services als u wilt multi-bitrate streams maken](media-services-manage-live-encoder-enabled-channels.md)voor meer algemene informatie die betrekking hebben op kanalen die zijn ingeschakeld voor het coderen van live.

##<a name="common-live-streaming-scenario"></a>Live Streaming gebruikelijk

Hier volgen algemene stappen voor het maken van algemene live streaming toepassingen.

>[AZURE.NOTE] De maximale aanbevolen duur van een live-gebeurtenis is momenteel 8 uur. Neem contact op met amslived op Microsoft.com als voor het uitvoeren van een kanaal voor langere periode.

1. Een videocamera verbinden met een computer. Starten en configureren van een on-premises implementatie live encoder die een één bitsnelheid in een van de volgende protocollen kunt uitvoeren: RTMP, vloeiende Streaming of RTP (MPEG-TS). Zie [Azure Media Services RTMP ondersteuning en Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824)voor meer informatie.
    
    Deze stap kan ook worden uitgevoerd nadat u uw kanaal hebt gemaakt.

1. Maken en een kanaal starten. 

1. Ophalen het kanaal nemen URL. 

    De URL ingest wordt gebruikt door de live encoder de stream versturen naar het kanaal.
1. Hiermee kunt u de URL van de Preview-versie kanaal ophalen. 

    Gebruik deze URL om te controleren of uw kanaal is juist de live gegevensstroom ontvangt.

3. Maak een gebeurtenis en programma's (die activa, wordt ook worden gemaakt). 
1. Publiceer de gebeurtenis (die u maakt een locator OnDemand voor de bijbehorende activa).  

    Zorg ervoor dat ten minste één streaming gereserveerde eenheid op de streaming eindpunt waaruit u inhoud wilt hebben.
1. Start de gebeurtenis wanneer u klaar bent voor het starten van streaming en archiveren.
2. (Optioneel) het live encoder kunt signaal ontvangen een advertentie starten. De advertentie wordt ingevoegd in de uitvoer-stream.
1. De gebeurtenis wanneer u niet langer streaming en archiveren van de gebeurtenis wilt stoppen.
1. De gebeurtenis verwijderen (en desgewenst verwijderen de activa).   

##<a name="in-this-tutorial"></a>In deze zelfstudie

In deze zelfstudie wordt de portal van Azure gebruikt de volgende taken uitvoeren: 

2.  Configureer streaming eindpunten.
3.  Maak een kanaal bevindt waarvoor om uit te voeren live codering is ingeschakeld.
1.  De URL nemen om af te leveren Live encoder krijgen. De live encoder gebruikt deze URL voor het nemen van de stream in het kanaal. .
1.  Een gebeurtenis en programma's (en activa) maken
1.  Publiceren van de activa te krijgen streaming van URL 's  
1.  Uw inhoud afspelen 
2.  Opschonen

##<a name="prerequisites"></a>Vereisten voor

Het volgende moeten zijn de zelfstudie voltooien.

- Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie.
- Een Media Services-account. Als u wilt een Media Services-account hebt gemaakt, Zie [Account maken](media-services-portal-create-account.md).
- Een webcam en een encoder die een live één bitsnelheid kunt verzenden.

##<a name="configure-streaming-endpoints"></a>Streaming eindpunten configureren 

Media Services biedt dynamische verpakking waarmee u voor het leveren van uw MP4s multi-bitrate in de volgende streaming indelingen: MPEG-streepje, HLS, vloeiende Streaming of schijven, zonder dat u hoeft opnieuw inpakken in deze indelingen streaming. Met dynamische verpakking hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services wordt bouwen en het juiste antwoord op basis van aanvragen van een client fungeren.

Om te profiteren van dynamische verpakking, moet u ten minste één streaming eenheid voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud te downloaden.  

Ga als volgt te werk als u wilt maken en het aantal streaming gereserveerde eenheden wijzigt:

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/) en selecteer uw account AMS.
1. Klik in het venster **Instellingen** op **Streaming eindpunten**. 

2. Klik op de standaard streaming eindpunt. 

    Het venster **Standaard STREAMING EINDPUNTDETAILS** wordt geopend.

3. Als u wilt opgeven van het aantal streaming eenheden, schuift u de schuifregelaar **Streaming eenheden** .

    ![Streaming eenheden](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Klik op de knop **Opslaan** als uw wijzigingen wilt opslaan.

    >[AZURE.NOTE]De toewijzing van een nieuwe eenheden kan maximaal 20 minuten duren.

##<a name="create-a-channel"></a>Een kanaal maken

1. Klik in de [portal van Azure](https://portal.azure.com/)Media Services selecteren en klik op de naam van uw Media Services-account.
2. Selecteer **Live Streaming**.
3. Selecteer **aangepaste maken**. Deze optie kunt u een kanaal dat is ingeschakeld voor het coderen van live maken.

    ![Een kanaal maken](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Klik op **Instellingen**.
    
    1.  Kies het kanaaltype **Live-codering** . Dit type geeft aan dat u maken van een kanaal bevindt waarvoor is ingeschakeld wilt voor het coderen van live. Dit betekent dat de binnenkomende één bitsnelheid is verzonden naar het kanaal en codering in een multi-bitsnelheid met opgegeven live encoder-instellingen. Zie [Live streaming met behulp van Azure Media Services als u wilt maken van multi-bitrate streams](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie. Klik op OK.
    2. Geef de naam van een kanaal.
    3. Klik op OK onder aan het scherm.
    
5. Selecteer het tabblad **Ingest** .

    1. Klik op deze pagina kunt u een streaming protocol. Voor het kanaaltype **Live codering** zijn geldig protocolopties:
        
        - Eén bitsnelheid Fragmented MP4 (vloeiende Streaming)
        - Eén bitsnelheid RTMP
        - RTP (MPEG-TS): MPEG-2-transportstream via RTP.
        
        Zie [Live streaming met behulp van Azure Media Services als u wilt maken van multi-bitrate streams](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie over elke protocol.
    
        U kunt de protocoloptie terwijl het kanaal niet wijzigen of de bijbehorende gebeurtenissen/programma's worden uitgevoerd. Als u verschillende protocollen vereist, moet u afzonderlijke kanalen voor elk streaming protocol maken.  

    2. U kunt IP-beperking toepassen op de ingest. 
    
        U kunt de IP-adressen zijn toegestaan naar een video op dit kanaal nemen definiëren. Toegestane IP-adressen kunnen worden opgegeven als één IP-adres (bijvoorbeeld ' 10.0.0.1'), een IP-bereik met een IP-adres en een CIDR subnetmasker (bijvoorbeeld ' 10.0.0.1/22') of een IP-bereik met een IP-adres en een stippellijn decimaal subnetmasker (bijvoorbeeld ' 10.0.0.1(255.255.252.0)').

        Als geen IP-adressen zijn opgegeven en er geen regeldefinitie is vervolgens geen IP-adres toegestaan. Als u wilt toestaan dat elk IP-adres, een regel maken en stel 0.0.0.0/0.

6. Klik op het tabblad **Preview** toepassen IP-beperking voor de Preview-versie.
7. Klik op het tabblad **codering** Geef de codering vooraf ingestelde. 

    Op dit moment het enige systeem vooraf ingestelde kunt u is **standaard 720 p**. Als u een aangepaste vooraf ingestelde, open een Microsoft-ondersteuningsticket. Voer vervolgens de naam van de vooraf ingestelde voor u gemaakt. 

>[AZURE.NOTE] Het begin van het kanaal kan momenteel maximaal 30 minuten duren. Beginwaarden kanaal kan maximaal 5 minuten duren.

Nadat u het kanaal hebt gemaakt, kunt u Klik op het kanaal en selecteer **Instellingen** , waar u uw configuraties kanalen kunt bekijken. 

Zie [Live streaming met behulp van Azure Media Services als u wilt maken van multi-bitrate streams](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie.


##<a name="get-ingest-urls"></a>Get nemen URL 's

Wanneer het kanaal is gemaakt, kunt u een URL's die u naar de live encoder bieden wilt nemen. De encoder wordt deze URL's om een live gegevensstroom invoer.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##<a name="create-and-manage-events"></a>Maken en beheren van gebeurtenissen

###<a name="overview"></a>Overzicht

Een kanaal is gekoppeld aan gebeurtenissen/programma's waarmee u de publicatie en opslag van segmenten in een live gegevensstroom. Kanalen beheren gebeurtenissen/programma's. De relatie kanaal en hetzelfde programma is vergelijkbaar met traditionele media waar een kanaal heeft een constante stroom van inhoud en een programma is beperkt tot enkele vervaging gebeurtenis op dat kanaal.

Het aantal uren die u wilt behouden de opgenomen inhoud voor de gebeurtenis de lengte **Archief venster** instelt, kunt u opgeven. Deze waarde kan worden ingesteld van een minimum van 5 minuten tot maximaal 25 uur. Archief venster lengte bepaalt ook dat de maximale hoeveelheid tijd clients terug in tijd kunt zoeken vanaf de huidige live positie. Gebeurtenissen kunnen worden uitgevoerd op de opgegeven tijdsduur, maar de inhoud die achter de lengte van het venster valt continu worden verwijderd. Deze waarde van deze eigenschap wordt ook bepaalt hoe lang de client-manifesten te vergroten.

Elke gebeurtenis is gekoppeld aan een actief. Als u wilt publiceren van de gebeurtenis moet u een locator OnDemand voor de bijbehorende activa maken. Met deze locator, kunt u de URL van een streaming dat u naar uw klanten kan leveren maken.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve gebeurtenissen, zodat u kunt meerdere archieven van de dezelfde binnenkomende stream maken. Hiermee kunt u publiceren en archiveren van verschillende gedeelten van een gebeurtenis naar wens. Het vereiste voor uw bedrijf is bijvoorbeeld archiveren van 6 uur van een gebeurtenis, maar alleen de laatste 10 minuten uitzenden. Hiervoor moet u twee tegelijk draaiend maken gebeurtenis. Een gebeurtenis is ingesteld op het archiveren van 6 uur van de gebeurtenis, maar het programma niet wordt gepubliceerd. De andere gebeurtenis is ingesteld op archiveren voor 10 minuten en dit programma is gepubliceerd.

U moet niet opnieuw gebruiken bestaande programma's voor nieuwe gebeurtenissen. In plaats daarvan maken en start een nieuwe programma voor elke gebeurtenis.

Een gebeurtenis/programma te starten wanneer u klaar bent voor het starten van streaming en archiveren. De gebeurtenis wanneer u niet langer streaming en archiveren van de gebeurtenis wilt stoppen. 

Als u wilt verwijderen gearchiveerde inhoud, stoppen en verwijderen van de gebeurtenis en verwijder vervolgens het bijbehorende actief. Activa kan niet worden verwijderd als deze wordt gebruikt door de gebeurtenis plaatsvindt. de gebeurtenis moet eerst worden verwijderd. 

Zelfs nadat u stoppen en de gebeurtenis verwijdert, is de gebruikers zou kunnen uw gearchiveerde inhoud streamen als een video op aanvraag, voor, zo lang maken als u de activa niet verwijderen.

Als u wilt de gearchiveerde inhoud behouden, maar niet beschikbaar voor streaming, verwijdert u de streaming locator.

###<a name="createstartstop-events"></a>Gebeurtenissen maken/starten en stoppen

Nadat u de stream die doorloopt in het kanaal hebt, kunt u de streaming kan beginnen door te maken van een activum, programma en Streaming Locator. Hiermee wordt de stream archiveren en beschikbaar maken voor gebruikers die zijn door het eindpunt Streaming. 

Er zijn twee manieren om gebeurtenissen: 

1. Druk op de pagina **kanaal** op **Live gebeurtenis** aan een nieuwe gebeurtenis toevoegen.

    Geef: naam voor de gebeurtenis, de Activumnaam van de archief-venster en versleutelingsoptie.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Als u **deze live gebeurtenis nu publiceren** ingeschakeld, wordt de gebeurtenis de publicatie van URL's worden gemaakt.
    
    U kunt drukt u op **Start**, wanneer u klaar bent voor de gebeurtenis streamen.

    Als u de gebeurtenis, drukt u op **controle** om te beginnen met het afspelen van de inhoud weer te geven.

2. U kunt ook een snelkoppeling gebruiken en drukt u op **Ga Live** -knop op de pagina **kanaal** . Hiermee maakt u een standaard activa, programma en Streaming Locator.

    De gebeurtenis heet **standaard** en het venster archief is ingesteld op acht uur.

U kunt de gepubliceerde gebeurtenis van de **gebeurtenis Live** -pagina bekijken. 

Als u op **Uit lucht**, stopt alle live gebeurtenissen. 


##<a name="watch-the-event"></a>Bekijk de gebeurtenis

De gebeurtenis te bekijken, klik op **controle** in de portal van Azure of de streaming URL kopiëren en gebruiken van een speler van uw keuze. 
 
![Gemaakt](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Gebeurtenissen converteert Live-gebeurtenis automatisch naar inhoud op aanvraag wanneer gestopt.

##<a name="clean-up"></a>Opschonen

Als u streaming gebeurtenissen worden uitgevoerd en u wilt opruimen de resources die deze is ingericht eerder, volgt u de volgende procedure.

- Stop de stroom van de encoder drukken.
- Stoppen met het kanaal. Nadat het kanaal is gestopt, wordt deze niet alle kosten oplopen. Wanneer u opnieuw start moet, heeft dit hetzelfde URL nemen zodat u niet nodig om te configureren, uw encoder.
- U kunt uw eindpunt Streaming stoppen, tenzij u doorgaan wilt naar het archief van uw live-gebeurtenis als een stroom op aanvraag bieden. Als het kanaal gestopt is, wordt deze niet alle kosten oplopen.
  
##<a name="view-archived-content"></a>Gearchiveerde inhoud weergeven

Zelfs nadat u stoppen en de gebeurtenis verwijdert, is de gebruikers zou kunnen uw gearchiveerde inhoud streamen als een video op aanvraag, voor, zo lang maken als u de activa niet verwijderen. Activa kan niet worden verwijderd als deze wordt gebruikt door een gebeurtenis; de gebeurtenis moet eerst worden verwijderd. 

Uw activa beheren, selecteer **instelling** en klik op **activa**.

![Activa](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##<a name="considerations"></a>Overwegingen

- De maximale aanbevolen duur van een live-gebeurtenis is momenteel 8 uur. Neem contact op met amslived op Microsoft.com als voor het uitvoeren van een kanaal voor langere periode.
- Zorg ervoor dat ten minste één streaming gereserveerde eenheid op de streaming eindpunt waaruit u inhoud wilt hebben.


##<a name="next-step"></a>Volgende stap

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
