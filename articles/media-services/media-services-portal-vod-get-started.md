<properties
    pageTitle=" Aan de slag met het geven van inhoud op aanvraag met behulp van de Azure portal | Microsoft Azure"
    description="Deze zelfstudie leert u de stappen van de uitvoering van een eenvoudige Video-on-Demand (VoD) contentlevering service met Azure Media Services (AMS)-toepassing met behulp van de Azure portal."
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Aan de slag met het geven van inhoud op aanvraag met behulp van de Azure portal

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Deze zelfstudie leert u de stappen van de uitvoering van een eenvoudige Video-on-Demand (VoD) contentlevering service met Azure Media Services (AMS)-toepassing met behulp van de Azure portal.

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie. 

Deze zelfstudie bevat de volgende taken uitvoeren:

1.  Maak een Azure Media Services-account.
2.  Streaming eindpunt configureren.
1.  Upload een videobestand opnemen.
1.  Coderen het bronbestand in een reeks geavanceerde bitsnelheid MP4-bestanden.
1.  De activa en get-streaming en geleidelijke downloaden URL's publiceren.  
1.  Uw inhoud afspelen.


## <a name="create-an-azure-media-services-account"></a>Maak een Azure Media Services-account

De stappen in deze sectie laten zien hoe een AMS-account maken.

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **+ nieuwe** > **Web + Mobile** > **mediaservices**.

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Voer de vereiste waarden in **MEDIA SERVICES-ACCOUNT maken** .

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Voer de naam van de nieuwe AMS-account in het vak **Naam van het Account**. De naam van een Media Services-account is alle kleine letters cijfers of letters zonder spaties, en is 3 tot en met 24 tekens.
    2. Selecteer in abonnement, tussen de verschillende Azure abonnementen waartoe u toegang hebt.
    
    2. Selecteer de nieuwe of bestaande resource in **Resourcegroep**.  Een resourcegroep is een verzameling resources die levenscyclus, machtigingen en beleid delen. Meer informatie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Schakel de geografische regio worden op **locatie**gebruikt voor de opslag van de media en metagegevens records voor uw Media Services-account. Dit gebied wordt gebruikt om te verwerken en media streamen. Alleen de beschikbare Media Services regio's weergegeven in het vak van de vervolgkeuzelijst. 
    
    3. Selecteer in de **Opslag-Account**, een opslag-account om u te bieden-blobopslag van het media-inhoud van uw Media Services-account. U kunt een bestaand account voor de opslag in hetzelfde geografische gebied, als uw account Media Services selecteren of kunt u een opslag-account maken. Een nieuwe opslag-account is gemaakt in dezelfde regio. De regels voor opslag-accountnamen zijn hetzelfde als voor Media Services-accounts.

        Meer informatie over de opslag [hier](storage-introduction.md).

    4. Selecteer **vastmaken aan dashboard** om te zien van de voortgang van de account-implementatie.
    
7. Klik op **maken** onder aan het formulier.

    Zodra het account is gemaakt, verandert de status **uitgevoerd**. 

    ![Media Services-instellingen](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voor het beheren van uw account AMS (bijvoorbeeld, video's uploaden, coderen van activa, taakvoortgang bewaken) Gebruik het venster **Instellingen** .

## <a name="manage-keys"></a>Toetsen beheren

Moet u de naam van het account en de primaire sleutel gegevens via programmacode toegang tot de Media Services-account.

1. Selecteer uw account in de portal Azure. 

    Het venster **Instellingen** wordt weergegeven aan de rechterkant. 

2. Selecteer in het venster **instellingen van** **toetsen**. 

    De vensters **sleutels beheren** ziet u de naam van het account en de primaire en secundaire sleutels wordt weergegeven. 
3. Druk op de knop kopiëren om de waarden te kopiëren.
    
    ![Media Services-toetsen](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="configure-streaming-endpoints"></a>Streaming eindpunten configureren

Wanneer u werkt met Azure Media Services een van de meeste gevallen video via geavanceerde bitsnelheid streaming levert aan uw klanten. Media Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders).

Media Services biedt dynamische verpakking, waarmee u uw geavanceerde bitsnelheid codering MP4-inhoud in streaming door Encoder ondersteunde Media Services (MPEG streepje, HLS, vloeiende Streaming, schijven) just-in-time, zonder dat u voor de opslag van vooraf ingepakte versies van elk van deze indelingen streaming hoeft te geven.

Om te profiteren van dynamische verpakking, moet u als volgt te werk:

- Coderen uw mezzanine (bron)-bestand in een reeks geavanceerde bitsnelheid MP4-bestanden (de codering stappen worden verderop in deze zelfstudie gedemonstreerd).  
- Ten minste één streaming eenheid voor het *eindpunt streaming* waaruit u wilt gaan bezorging van uw inhoud maken. De onderstaande stappen uitgelegd hoe u het aantal streaming eenheden wijzigt.

Met dynamische verpakking, hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services genereert en het juiste antwoord op basis van aanvragen van een client fungeert.

Ga als volgt te werk als u wilt maken en het aantal streaming gereserveerde eenheden wijzigt:


1. Klik in het venster **Instellingen** op **Streaming eindpunten**. 

2. Klik op de standaard streaming eindpunt. 

    Het venster **Standaard STREAMING EINDPUNTDETAILS** wordt geopend.

3. Als u wilt opgeven van het aantal streaming eenheden, schuift u de schuifregelaar **Streaming eenheden** .

    ![Streaming eenheden](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klik op de knop **Opslaan** als uw wijzigingen wilt opslaan.

    >[AZURE.NOTE]De toewijzing van een nieuwe eenheden kan maximaal 20 minuten duren.

## <a name="upload-files"></a>Bestanden uploaden

Naar stroom video's met behulp van Azure Media Services, moet u de bron-video's uploaden, coderen in meerdere bitsnelheid en publiceren van het resultaat. In deze sectie zijn van toepassing op de eerste stap. 

1. Klik in het venster **instelling** op **activa**.

    ![Bestanden uploaden](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. Klik op de knop **uploaden** .

    Het **uploaden van een video activum** -venster wordt weergegeven.

    >[AZURE.NOTE] Er is geen bestandsgrootte.
    
4. Blader naar de gewenste video op uw computer, selecteert u deze en klik op OK.  

    Hiermee start u de upload en ziet u de voortgang onder de bestandsnaam.  

Zodra de upload is voltooid, ziet u het nieuwe activum weergegeven in het venster **activa** . 

## <a name="encode-assets"></a>Coderen van activa

Wanneer u werkt met Azure Media Services een van de meeste gevallen geavanceerde bitsnelheid streaming levert aan uw klanten. Media Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders). Uw video's om voor te bereiden geavanceerde bitsnelheid streaming, moet u uw bronvideo coderen in multi-bitrate bestanden. U kunt de **Media Encoder standaard** encoder coderen van uw video's.  

Media Services biedt ook dynamische verpakking, waarmee u voor het leveren van uw MP4s multi-bitrate in de volgende streaming indelingen: MPEG-streepje, HLS, vloeiende Streaming of schijven, zonder dat u hoeft om te verpakken in deze indelingen streaming. Met dynamische verpakking, hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services genereert en het juiste antwoord op basis van aanvragen van een client fungeert.

Om te profiteren van dynamische verpakking, moet u als volgt te werk:

- Coderen het bronbestand in een reeks multi-bitrate MP4-bestanden (de codering stappen worden gedemonstreerd verderop in dit gedeelte).
- Ten minste één streaming eenheid voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud downloaden. Zie [streaming eindpunten configureren](media-services-portal-vod-get-started.md#configure-streaming-endpoints)voor meer informatie. 

### <a name="to-use-the-portal-to-encode"></a>Gebruik van de portal voor het coderen van

In dit gedeelte worden de stappen beschreven voor het coderen van de inhoud met Media Encoder standaard.

1.  Selecteer in het venster **instellingen van** **activa**.  
2.  Selecteer in het venster **activa** het activum dat u gebruiken wilt voor coderen.
3.  Druk op de knop **coderen** .
4.  Klik in het venster **coderen activa** de "Media Encoder standaard" processor en een vooraf ingestelde te selecteren. Bijvoorbeeld als u uw invoer video heeft een resolutie van 1920 x 1080 pixels weet, klikt u vervolgens kunt u de ' H264 meerdere bitsnelheid 1080p "vooraf ingestelde. Voor meer informatie over de vooraf ingestelde, Zie [Dit](https://msdn.microsoft.com/library/azure/mt269960.aspx) artikel – is het belangrijk om te selecteren van de vooraf ingestelde die meest geschikt is voor uw video invoer. Als u een video van lage resolutie (640 x 360) hebt, wordt u mag worden gemaakt van de standaard "H264 meerdere bitsnelheid 1080p" vooraf ingestelde.
    
    Om eenvoudiger te beheren hebt u een optie van het bewerken van de naam van de activa uitvoer en de naam van de taak.
        
    ![Coderen van activa](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Druk op **maken**.

### <a name="monitor-encoding-job-progress"></a>Tekstcodering taakvoortgang bewaken

De voortgang van de codering taak, klik op **Instellingen** (boven aan de pagina) en selecteer vervolgens **taken**.

![Taken](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Inhoud publiceren

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

>[AZURE.NOTE] Als u de portal gebruikt bij het maken van Locator vóór maart 2015, zijn Locator met een vervaldatum van twee jaar gemaakt.  

Als u een vervaldatum voor een locator bijwerken, [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) of [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) -API's te gebruiken. Wanneer u de vervaldatum van een locator SA's bijwerkt, verandert de URL.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Gebruik van de portal activa publiceren

Ga als volgt te werk als u de portal wilt publiceren activa:

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

##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


