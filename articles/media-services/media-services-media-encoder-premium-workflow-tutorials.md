<properties 
    pageTitle="Avanced Media Encoder Premium werkstroom zelfstudies" 
    description="Dit document bevat walkthroughs die zien hoe u geavanceerde taken met Media Encoder Premium werkstroom uitvoeren en hoe u complexe om werkstromen te maken met werkstroomontwerper." 
    services="media-services" 
    documentationCenter="" 
    authors="xstof" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"  
    ms.author="xstof;xpouyat;juliako"/>

#<a name="advanced-media-encoder-premium-workflow-tutorials"></a>Geavanceerde Media Encoder Premium werkstroom zelfstudies

##<a name="overview"></a>Overzicht 

Dit document bevat walkthroughs zien hoe u werkstromen met **Werkstroomontwerper**aanpassen. U vindt de werkelijke Werkstroombestanden [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

##<a name="toc"></a>INHOUDSOPGAVE

De volgende onderwerpen vallen:

- [MXF in een enkel bitsnelheid MP4-codering](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
    - [Een nieuwe werkstroom starten](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new) 
    - [Met behulp van de invoer van Media-bestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
    - [Mediastreams controleren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
    - [Het toevoegen van een video encoder voor. MP4-Bestandsgeneratie](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
    - [Coderen van de audiostroom](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
    - [Audio en Video-streams multiplexing in een container MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
    - [Schrijven van het MP4-bestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
    - [Een Media Services activum maken op basis van het uitvoerbestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
    - [De voltooide werkstroom lokaal testen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
- [MXF in multibitrate MP4s - codering dynamische verpakking ingeschakeld](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
    - [Het toevoegen van extra MP4-uitvoer van een of meer](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
    - [De bestandsnamen uitvoer configureren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
    - [Een afzonderlijk-nummer toevoegen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
    - [Toevoegen van de. CONSOLE SMIL-bestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
- [MXF-codering naar multibitrate MP4 - verbeterde blauwdruk](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
    - [Overzicht van de werkstroom om te verbeteren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_overview)
    - [Naamgevingsconventies archiveren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
    - [Eigenschappen van het onderdeel naar de hoofdsite van de werkstroom publiceren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
    - [Uitvoerbestand namen, is afhankelijk van gepubliceerde eigenschapswaarden hebt gegenereerd](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
- [Miniaturen toevoegen aan multibitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
    - [Overzicht van de werkstroom toe te voegen miniaturen](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to_multibitrate_MP4_overview)
    - [Toe te voegen JPG-codering](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
    - [Omgaan met kleurruimte conversie](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
    - [De miniaturen schrijven](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
    - [Fouten opsporen in een werkstroom](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
    - [Klaar werkstroom](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
- [Op tijd gebaseerde inkorten van multibitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
    - [Overzicht van de werkstroom toe te voegen inkorten naar](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
    - [Gebruik van de Stream Trimmer](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
    - [Klaar werkstroom](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
- [Inleiding tot de script-Component](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
    - [Binnen een werkstroom voor het uitvoeren van scripts: Hallo, wereld](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
- [Op basis van een frame inkorten van multibitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
    - [Blauwdruk overzicht inkorten naar toevoegen](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
    - [Gebruik van de Knipsellijst XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
    - [De knipsellijst vanuit een script-Component aanpassen](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
    - [De eigenschap van een ClippingEnabled gemak toe te voegen](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

##<a id="MXF_to_MP4"></a>MXF in een enkel bitsnelheid MP4-codering
 
In dit scenario gaan we een één bitsnelheid maken. MP4-bestand met AAC-HE gecodeerd geluid ontvangen via een. Invoer MXF-bestand. 

###<a id="MXF_to_MP4_start_new"></a>Een nieuwe werkstroom starten 

Workflow Designer te openen en selecteer "Bestand"-"nieuwe werkruimte"-"transcoderen blauwdruk" 

De nieuwe werkstroom, 3 elementen wordt weergegeven: 

- Primaire bronbestand
- Knipsellijst XML
- Uitvoer bestand/activa  

![Nieuwe werkstroom voor het coderen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Nieuwe werkstroom voor het coderen*

###<a id="MXF_to_MP4_with_file_input"></a>Met behulp van de invoer van Media-bestand

Om te kunnen accepteren onze invoer mediabestand, begint een met het toevoegen van een Media-bestand invoer onderdeel. Als u wilt een onderdeel toevoegen aan de werkstroom, zoekt u hiernaar in het zoekvak van de bibliotheek en sleep de gewenste vermelding in het deelvenster ontwerpfunctie. Deze stap herhalen voor de invoer Media-bestand en het onderdeel primaire bronbestand verbindt met de bestandsnaam invoer pincode van de invoer Media-bestand.

![Verbonden mediabestanden invoer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Verbonden mediabestanden invoer*

Voordat we veel nog meer doen kunt, moet we eerst de werkstroomontwerper laten weten welke voorbeeldbestand we gebruiken willen voor het ontwerpen van onze workflow met. Als u wilt doen, klikt u op de achtergrond van de ontwerpfunctie deelvenster en zoek de eigenschap primaire bronbestand in het deelvenster rechts eigenschap. Klik op het mappictogram en selecteer het gewenste bestand om te testen van de werkstroom met. Als dit klaar is, wordt het onderdeel van de invoer van Media-bestand controleren van het bestand en vullen de uitvoer pincodes zodat het bestand dat deze gecontroleerd.

![Gevulde mediabestanden invoer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Gevulde mediabestanden invoer*

Terwijl u Hiermee geeft u met welke invoer willen we werken, niet duidelijk, maar waar de gecodeerde uitvoer moet terechtkomen in. Vergelijkbaar met hoe de primaire bronbestand is geconfigureerd, de uitvoer map variabele eigenschap, net onder het nu configureren.

![Geconfigureerde en uitvoer eigenschappen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Geconfigureerde en uitvoer eigenschappen*

###<a id="MXF_to_MP4_streams"></a>Mediastreams controleren

Vaak heeft deze de gewenste om te weten hoe de stream eruit ziet als loopt via de werkstroom. Als u wilt een stream op een willekeurige plaats in de werkstroom controleren, klikt u op een uitvoer of invoer pincode op een van de onderdelen. In dit geval probeert te klikken op de pincode niet-gecomprimeerde Video uitvoer van de invoer van onze Media-bestand. Een dialoogvenster wordt geopend waarmee moet worden gecontroleerd de uitgaande video.

![De pincode van niet-gecomprimeerde Video uitvoer controleren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*De pincode van niet-gecomprimeerde Video uitvoer controleren*

In ons geval wordt aangegeven ons bijvoorbeeld dat we bent omgaan met een invoer 1920 x 1080 snelheid 24 frames per seconde in 4:2:2 steekproeven voor een video van bijna 2 minuten.

###<a id="MXF_to_MP4_file_generation"></a>Het toevoegen van een video encoder voor. MP4-Bestandsgeneratie

Houd er rekening mee dat nu, een niet-gecomprimeerde Video en meerdere niet-gecomprimeerde Audio-uitvoer pincodes zijn beschikbaar voor gebruik van de invoer van onze Media-bestand. Om te kunnen de binnenkomende video coderen, moeten we een codering onderdeel - in dit geval voor het genereren. MP4-bestanden.

Als u wilt coderen van de video-stream naar H.264, voeg u het onderdeel AVC Video Encoder aan de ontwerpfunctie oppervlakte. Dit onderdeel wordt op een video-stream decomprimeren als invoer en biedt een AVC gecomprimeerde video stream op de pincode voor uitvoer.

![Niet-verbonden AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Niet-verbonden AVC Encoder*

De eigenschappen kunt u bepalen hoe de codering precies gebeurt. Laten we eens kijken enkele belangrijke instellingen:

- Uitvoer breedte en hoogte van de uitvoer: deze de resolutie van de gecodeerde video bepalen. In ons geval in dit artikel leest met 640 x 360
- Tarief van frame: als de waarde indirecte deze dan alleen neemt het tarief weer dat frame bron is het mogelijk deze waarde te vervangen door. Houd er rekening mee dat deze framesnelheid-conversie is niet beweging-betaald.
- Profiel- en niveau: deze bepalen de AVC profiel en niveau. Klik op het vraagteken-pictogram in het onderdeel AVC Video Encoder gemakkelijk als u wilt meer informatie over de verschillende niveaus en de profielen, en de help-pagina wordt meer details over de verschillende niveaus weergeven. In dit artikel Ga voor ons voorbeeld, met Hoofdgegeven profiel op niveau 3,2 (de standaardinstelling).
- Classificeren overschrijfmodus en bitsnelheid (k): in ons scenario we kiezen voor een constante bitsnelheid (CBR) uitvoer voor 1200 k
- Video-indeling: dit is over de VUI die in de stream H.264 (kant informatie die mogelijk gebruikt door een mediadecoder naar het verbeteren van de weergave, maar niet nodig zijn voor correct decoderen) wordt geschreven (Video bruikbaarheid informatie):
- NTSC (standaard voor Amerikaans of Japan, met 30 fps)
- PAL (standaard voor Europa, met 25 fps)
- Modus voor het formaat van GOP: we wordt vaste GOP grootte configureren voor ons doel met een Interval toets van 2 seconden ingedrukt met GOPs gesloten. Dit zorgt ervoor dat compatibel zijn met de dynamische verpakking Azure Media Services biedt.

Als u wilt feed onze encoder AVC, verbinden met de pincode niet-gecomprimeerde Video-uitvoer van het Media-bestand invoer onderdeel de niet-gecomprimeerde Video invoer pincode van de encoder AVC.

![Verbonden AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Verbonden AVC Hoofdgegeven encoder*

###<a id="MXF_to_MP4_audio"></a>Coderen van de audiostroom

Nu we hebben video codering, maar de oorspronkelijke niet-gecomprimeerde audiostroom nog moet worden gecomprimeerd. Hiervoor gaan we met AAC-codering door het onderdeel AAC Encoder (Dolby). Dit toevoegen aan de werkstroom.

![Niet-verbonden AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Niet-verbonden AAC encoder*

Nu er een incompatibiliteit is: alleen een één niet-gecomprimeerde audio invoer pincode van de Encoder AAC er is waarschijnlijk meer dan de invoer Media-bestand moet twee verschillende niet gecomprimeerd audiostroom beschikbaar: een voor het links audio kanaal en een voor de rechterkant. (Als u bent omgaan met rondom geluid, is dat de 6 kanalen.) Dus is het niet mogelijk om rechtstreeks verbinding maken met het geluid van de uit de bron van de invoer van Media-bestand in de audio AAC-encoder. Het onderdeel AAC verwacht een zogenaamde "afwisselende" audiostroom: één stream waarop de linker- en de juiste kanalen afwisselende met elkaar. Wanneer we weten uit onze mediabestand bron wat zijn welke audio nummers op welke positie in de bron, wordt deze afwisselende audiostroom met de posities correct toegewezen luidspreker voor links en rechts kunt genereren.

Een wilt eerst een afwisselende stroom van de vereiste bron audio kanalen gegenereerd. Het onderdeel geluid Stream Interleaver verwerkt dit voor ons. Voeg deze toe aan de werkstroom en verbindt u de uitvoer van de audio van de invoer van de Media-bestand erin.

![Verbonden audiostroom Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Verbonden audiostroom Interleaver*

Nu dat we een afwisselende audiostroom hebben, opgeven we nog steeds niet waar de linker- of luidspreker posities die u wilt toewijzen. Om op te geven dit, kunnen we gebruikmaken van de luidspreker positie wel statusrapporten.

![Een luidspreker positie wel statusrapporten toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Een luidspreker positie wel statusrapporten toevoegen*

De luidspreker positie wel statusrapporten voor gebruik met een stereo invoer door een Encoder vooraf ingestelde Filter van "Aangepaste" configureren en het kanaal vooraf ingestelde '2.0 (L, R)' genoemd. (Dit wordt de positie links luidspreker aan 1 kanaal en de positie van de juiste luidspreker aan toewijzen kanaal 2)

De uitvoer van de luidspreker positie wel statusrapporten verbinden met de invoer van de AAC-Encoder. Vervolgens geeft u de Encoder AAC voor gebruik met een "2.0 (L, R)" kanaal vooraf ingestelde, zodat deze weet te handelen stereogeluid als invoer.

###<a id="MXF_to_MP4_audio_and_fideo"></a>Audio en Video-streams multiplexing in een container MP4

De opgegeven onze AVC gecodeerde video-stream en onze AAC codering audiostroom, we kunt vastleggen beide in een. MP4-container. Het proces van het mengen van verschillende streams in een enkel heet "multiplexing" (of "muxing"). In dit geval bent we de audio en video-streams in één stroken interleaving. MP4-pakket. Het onderdeel dat coördinaten dit voor een. MP4 container heet het ISO-MPEG-4-Multiplexer. Toevoegen aan de ontwerpfunctie oppervlakte en de Video AVC Encoder zowel de AAC-Encoder verbinden met de invoer.

![Verbonden MPEG4 Multiplexer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Verbonden MPEG4 Multiplexer*

###<a id="MXF_to_MP4_writing_mp4"></a>Schrijven van het MP4-bestand

Bij het schrijven van een uitvoerbestand, is het onderdeel uitvoer in een bestand wordt gebruikt. We kunt verbinden met dit de uitvoer van de ISO MPEG-4-Multiplexer zodat de uitvoer wordt geschreven naar schijf. Klik hiertoe verbinden met de Container (MPEG-4) uitvoer pincode de schrijven invoer pincode van de uitvoer van het bestand.

![Bestandsuitvoer verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Bestandsuitvoer verbonden*

U kunt de bestandsnaam die wordt gebruikt, wordt bepaald door de eigenschap van het bestand. Hoewel deze eigenschap vastgelegde aan een gegeven waarde zijn kan, wordt meestal één wilt dit via een expressie in plaats daarvan.

De werkstroom automatisch bepalen de uitvoer bestand de van naameigenschap van een expressie, klikt u op de buton naast de bestandsnaam (naast het mappictogram van de). Selecteer 'Expressie' in de vervolgkeuzelijst. U krijgt de expressie-editor. Schakel eerst de inhoud van de editor.

![Lege expressie-Editor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Lege expressie-Editor*

De editor voor expressies kunt invoeren van een letterlijke waarde, gecombineerd met een of meer variabelen. Variabelen beginnen met een dollarteken. Als u de toets $ raakt, wordt een vervolgkeuzelijst in met een keuze van beschikbare variabelen weergegeven in de editor. In ons geval gebruiken we een combinatie van de uitvoervariabele directory en de variabele voor grondtal invoer bestandsnaam:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Filled out expressie-Editor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Filled out expressie-Editor*

>[AZURE.NOTE]Om te zien een uitvoerbestand zien van uw codering taak in Azure wordt aangegeven, moet u een waarde in de editor expressie opgeven. 

Als u de expressie bevestigt door te raken ok, wordt het eigenschappenvenster bekijken wat de bestand eigenschap wordt opgelost op dit moment waarde.

![Bestand expressie lost uitvoer dir](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Bestand expressie lost uitvoer dir*

###<a id="MXF_to_MP4_asset_from_output"></a>Een Media Services activum maken op basis van het uitvoerbestand

Terwijl we een MP4-uitvoerbestand hebt geschreven, moeten we nog steeds geven aan dat dit bestand deel uitmaakt van de activa uitvoer mediaservices wordt gegenereerd als gevolg van het uitvoeren van deze werkstroom. Daartoe wordt het knooppunt uitvoer bestand/activa op het canvas werkstroom gebruikt. Alle binnenkomende bestanden naar dit knooppunt maakt deel uit van de resulterende Azure Media Services actief.

Verbind de component uitvoer in een bestand naar het onderdeel uitvoer bestand/activa om te voltooien van de werkstroom.

![Klaar werkstroom](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Klaar werkstroom*

###<a id="MXF_to_MP4_test"></a>De voltooide werkstroom lokaal testen

Als u wilt testen van de werkstroom lokaal, druk op de knop afspelen in de werkbalk aan de bovenkant. Als de werkstroom uitgevoerd, controleren de uitvoer die in de uitvoermap geconfigureerde is gegenereerd. Hier ziet u het klaar MP4-uitvoerbestand dat uit het MXF invoerbron-bestand is gecodeerd.

##<a id="MXF_to_MP4_with_dyn_packaging"></a>MXF-codering naar MP4 - multibitrate dynamische verpakking ingeschakeld

In dit scenario maakt we een reeks meerdere bitsnelheid MP4-bestanden met AAC-codering audio uit één. Invoer MXF-bestand.

Wanneer een uitvoer van de activa multi-bitrate gewenst is voor gebruik in combinatie met de functies dynamische verpakking van Azure Media Services, meerdere GOP uitgelijnd MP4-bestanden van elk een verschillende bitsnelheid en resolutie moet worden gegenereerd. Klik hiervoor biedt de [MXF codering in een enkel bitsnelheid MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) -scenario ons met een goed beginpunt.

![Werkstroom starten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Werkstroom starten*

###<a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Het toevoegen van extra MP4-uitvoer van een of meer

Alle MP4-bestanden in onze resulterende Azure Media Services actief biedt ondersteuning voor een andere bitsnelheid en resolutie. Laten we een of meer MP4 uitvoerbestanden toevoegen aan de werkstroom.

Om ervoor te zorgen dat we onze video encoders die zijn gemaakt met dezelfde instellingen hebt, is het handigst het al bestaande AVC Video Encoder dupliceren en configureren van een andere combinatie van resolutie en bitsnelheid (we toevoegen een van de 960 x 540 bij 25 frames per seconde met 2,5 Mbps). Als u wilt dupliceren de bestaande encoder, kopie het plakt op de ontwerpfunctie oppervlak.

Verbinding maken met de pincode niet-gecomprimeerde Video uitvoer van de invoer van de Media-bestand in onze nieuwe AVC-component.

![Tweede AVC encoder verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Tweede AVC encoder verbonden*

Nu u de configuratie voor onze nieuwe AVC encoder om uit te voeren 960 x 540 met 2,5 Mbps aanpassen. (Gebruik de eigenschappen "uitvoer breedte", "Uitvoer hoogte" en "Bitsnelheid (k)" hiervoor.)

Gegeven we wilt gebruiken het resulterende actief samen met de dynamische verpakking Azure Media Services, de streaming eindpunt moet kunnen genereren van deze MP4-bestanden HLS/Fragmented MP4/streepje fragmenten die precies zijn uitgelijnd met elkaar op een manier die clients die zijn schakelen tussen verschillende bitsnelheid een enkel vloeiende continue video en audio-ervaring downloaden. Als u dit kunt doen, moeten we om ervoor te zorgen dat de tekengrootte voor beide MP4-bestanden in de eigenschappen van beide AVC encoders de GOP ("groep afbeeldingen') is ingesteld op 2 seconden ingedrukt, die kunnen worden aangebracht:

- Als u de modus voor het formaat van GOP op vaste GOP grootte en
- de toets Frame Interval op twee seconden.
- ook instellen zijn het besturingselement GOP IDR gesloten GOP om ervoor te zorgen alle GOP permanent op hun eigen zonder afhankelijkheden

Als u onze werkstroom handige kunnen begrijpen, wijzig de naam van de eerste AVC encoder naar "AVC Video Encoder 640 x 360 1200 k" en de tweede AVC encoder "AVC Video Encoder 960 x 540 2500 k".

Een tweede ISO MPEG-4 Multiplexer en een tweede bestand-uitvoer toevoegen. Sluit de multiplexer aan de nieuwe AVC encoder en controleert u of dat de uitvoer is doorgestuurd naar de uitvoer van het bestand. Sluit de AAC audio encoder uitvoer naar het nieuwe multiplexer van invoer ook. De uitvoer van het bestand worden op zijn beurt vervolgens verbonden met het knooppunt uitvoer bestand/activa toe te voegen aan de activa van het Media-Services die worden gemaakt.

![Tweede mixer en bestandsuitvoer verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Tweede mixer en bestandsuitvoer verbonden*

Voor compatibiliteit met dynamische verpakking Azure Media Services, de multiplexer segment modus in GOP tellen of duur configureren en de GOPs per segment ingesteld op 1. (Dit is de standaardwaarde.)

![Mixer segment modi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Mixer segment modi*

Opmerking: u kunt herhaalt u deze procedure voor alle andere bitsnelheid en -resolutie combinaties die u hebt toegevoegd aan de uitvoer van activa wilt maken.

###<a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>De bestandsnamen uitvoer configureren

Zijn er meer dan één enkel bestand toegevoegd aan de uitvoer van activa. Hierdoor wordt een nodig om te controleren of dat de bestandsnamen voor elk van de uitvoerbestanden die van elkaar verschillen zijn en een naamgevingsconventie misschien zelfs toepassen zodat deze verandert in wissen uit de bestandsnaam wat u bent omgaan met.

Naamgeving van bestanden-uitvoer kan worden gecontroleerd door expressies in de ontwerpfunctie. Open het eigenschappenvenster voor een van de onderdelen van het bestandsuitvoer en de expressie-editor voor de eigenschap van het bestand openen. Onze eerste uitvoerbestand is geconfigureerd met behulp van de volgende expressie (Zie de zelfstudie voor het navigeren van [MXF naar een enkel bitsnelheid MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Dit betekent dat onze bestandsnaam wordt bepaald door twee variabelen: de uitvoermap om in te schrijven en grondtal naam van het bronbestand. De voormalige wordt weergegeven als een eigenschap op de hoofdsite van de werkstroom en de laatste wordt bepaald door de binnenkomende bestand. Houd er rekening mee dat de uitvoermap wat u gebruiken is voor lokale testen; Deze eigenschap wordt overschreven door de engine werkstroom wanneer de werkstroom wordt uitgevoerd door de cloudgebaseerde Mediaprocessor in Azure Media Services.
Als u wilt geven beide onze uitvoerbestanden in een consistente uitvoer naming, het eerste bestand naamgeving van de expressie die moet te wijzigen:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

en de tweede naar:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Een tussenliggende test uitvoeren om ervoor te zorgen dat beide bestanden MP4-uitvoer juist zijn gegenereerd uitvoeren.

###<a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Een afzonderlijk-nummer toevoegen

Later zullen we zien wanneer we een .ism-bestand om te gaan met onze MP4-bestanden die uitvoer genereren, we ook moeten worden een bestand met alleen audio MP4 als het audionummer voor onze geavanceerde streaming. Dit bestand maken, een extra mixer toevoegen aan de werkstroom (Multiplexer ISO-MPEG-4) en van de AAC-encoder uitvoer pincode verbinden met de invoer pincode voor 1 bijhouden.

![Audio mixer toegevoegd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Audio mixer toegevoegd*

Maak een derde bestandsuitvoer onderdeel voor het uitvoeren van de uitgaande stream vanaf de mixer en configureren van het bestand naamgeving van de expressie als:
    
    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Audio mixer bestandsuitvoer maken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Audio mixer bestandsuitvoer maken*

###<a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Toevoegen van de. CONSOLE SMIL-bestand

Voor de dynamische verpakking voor gebruik in combinatie met zowel MP4-bestanden (en de MP4 alleen geluid) in onze Services Media-activa, moeten we ook een manifest-bestand (ook wel een bestand 'SMIL' genoemd: gesynchroniseerd Multimedia Integration Language). Dit bestand wordt naar Azure Media Services wordt aangegeven welke MP4-bestanden zijn beschikbaar voor dynamische verpakking en welke aandachtspunten voor de audio streaming. Een typische manifest bestand voor een reeks van MP4 met een enkel audiostroom ziet er zo uit:
    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

Het bestand .ism bevat binnen een schakeloptie-instructie is een verwijzing naar elk van de afzonderlijke MP4-videobestanden en naast ook een (of meer) audiobestand verwijzingen naar een MP4 met alleen het geluid.

Genereren van het manifest bestand voor onze reeks van MP4 kan worden uitgevoerd via een onderdeel dat de "AMS bestandenlijst schrijver' genoemd. Als u wilt gebruiken, sleep deze naar het oppervlak en verbindt u de "Schrijven voltooid" uitvoer pennen van de drie onderdelen van de uitvoer in een bestand naar de AMS bestandenlijst schrijver invoer. Controleer of om de uitvoer van de schrijver AMS bestandenlijst naar de uitvoer bestand/activa verbinding te maken.

Net als met onze andere bestand uitvoer onderdelen, de .ism uitvoer bestandsnaam met een expressie te configureren:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Onze klaar werkstroom eruit de hieronder:

![Klaar MXF met multibitrate MP4-werkstroom](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Klaar MXF met multibitrate MP4-werkstroom*

##<a id="MXF_to__multibitrate_MP4"></a>MXF-codering naar multibitrate MP4 - verbeterde blauwdruk

In de [vorige werkstroom rondleiding](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) we bovendien hebt geleerd hoe een enkel MXF invoer actief kan worden geconverteerd naar activa uitvoer met multi-bitrate MP4-bestanden, een bestand met alleen audio MP4 en een manifest bestand voor gebruik in combinatie met dynamische verpakking Azure Media Services.

In dit scenario wordt weergegeven hoe enkele van de aspecten kan worden verbeterd en handiger aangebracht.

###<a id="MXF_to_multibitrate_MP4_overview"></a>Overzicht van de werkstroom om te verbeteren

![Multibitrate MP4-werkstroom om te verbeteren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Multibitrate MP4-werkstroom om te verbeteren*

###<a id="MXF_to__multibitrate_MP4_file_naming"></a>Naamgevingsconventies archiveren

In de vorige werkstroom opgegeven we een eenvoudige expressie als basis voor het genereren van de bestandsnamen uitvoer. Zijn er enkele dupliceren door: al het de onderdelen van afzonderlijke uitvoer bestand opgegeven dergelijke expressie.

Bijvoorbeeld is onze onderdeel van de uitvoer bestand voor het eerste videobestand geconfigureerd met deze expressie:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

We hebben een expressie zoals terwijl u voor de tweede uitvoer van de video:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Zou het niet duidelijkere, minder fout vatbaar en handiger als we kan sommige van deze duplicaten verwijderen en dingen in plaats daarvan meer configureerbare maken? Gelukkig kunt we: van de ontwerpfunctie expressie mogelijkheden in combinatie met de mogelijkheid om te maken van aangepaste eigenschappen op de hoofdsite van onze werkstroom Stuur ons een toegevoegde laag van gemak.

Stel dat we wordt bestandsnaam configuratie-station van de bitsnelheid van de afzonderlijke MP4-bestanden. Deze bitsnelheid die wordt spijt configureren op één centrale locatie (in de hoofdmap van onze graph), uit waar ze moeten worden geopend om te configureren en station bestandsnaam wordt gegenereerd. Klik hiertoe begin met het publiceren van de eigenschap bitsnelheid uit beide encoders AVC in de hoofdmap van onze workflow, zodat deze verandert in toegankelijk zijn vanuit zowel de hoofdsite als u ook de encoders AVC in welk bestandstype. (Zelfs als weergegeven in twee verschillende nét, er wordt slechts één onderliggende waarde.)

###<a id="MXF_to__multibitrate_MP4_publishing"></a>Eigenschappen van het onderdeel naar de hoofdsite van de werkstroom publiceren

Open de eerste AVC encoder, gaat u naar de eigenschap bitsnelheid (k) en kies publiceren in de vervolgkeuzelijst.

![De eigenschap bitsnelheid publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*De eigenschap bitsnelheid publiceren*

Het dialoogvenster publiceren publiceren naar de hoofdmap van onze graph werkstroom configureren met een gepubliceerde naam 'video1bitrate' en een leesbare weergavenaam van 'Bitsnelheid Video 1'. Een aangepaste configureren groepsnaam 'Bitsnelheid Streaming' genoemd en klik op publiceren.

![De eigenschap bitsnelheid publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Publicatie van dialoogvenster voor de eigenschap bitsnelheid*

Herhaal dezelfde voor de eigenschap bitsnelheid van de tweede AVC encoder en noem deze "video2bitrate" met een weergavenaam van 'Video 2 bitsnelheid', in dezelfde aangepaste groep "Bitsnelheid Streaming".

Als we de eigenschappen van de hoofdsite werkstroom nu controleren, zien we onze aangepaste groep met de twee gepubliceerde eigenschappen worden weergegeven. Beide zijn de waarde van de desbetreffende AVC encoder bitsnelheid na te denken.

![Gepubliceerde bitsnelheid steunbalken op de hoofdsite van de werkstroom](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Op elk gewenst we voor toegang tot deze eigenschappen van code of van een expressie, kunnen we dit doen als volgt:

- van Inlinecode uit een onderdeel direct onder de hoofdsite: node.getPropertyAsString('.. / video1bitrate', null)
- in een expressie: ${ROOT_video1bitrate}
 
Laten we voert u de groep 'Bitsnelheid Streaming' door te publiceren van onze audionummer bitsnelheid ook op is geïnstalleerd. Binnen de eigenschappen van de Encoder AAC, zoek naar de instelling bitsnelheid en selecteer publiceren in de vervolgkeuzelijst ernaast. Publiceren naar de hoofdsite van de grafiek met de naam 'audio1bitrate' en de weergavenaam "Bitsnelheid Audio 1" binnen onze aangepaste groep "Bitsnelheid Streaming".

![Publicatie van het dialoogvenster audio bitsnelheid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Publicatie van het dialoogvenster audio bitsnelheid*

![Resulterende video en audio steunbalken in hoofdmap](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Resulterende video en audio steunbalken in hoofdmap*

Houd er rekening mee dat het wijzigen van deze drie een ook opnieuw configureert waarden en wijzigingen van de waarden in de desbetreffende onderdelen ze zijn gekoppeld aan (en waar uit die zijn gepubliceerd).

###<a id="MXF_to__multibitrate_MP4_output_files"></a>Uitvoerbestand namen, is afhankelijk van gepubliceerde eigenschapswaarden hebt gegenereerd

In plaats van hardcoding onze gegenereerde bestandsnamen, we kunnen nu wijzigen onze expressie bestandsnaam op elk van de onderdelen bestandsuitvoer afhankelijk van de eigenschappen van het bitsnelheid die we zojuist gepubliceerd op de hoofdsite van de grafiek. Beginnen met de bestandsuitvoer van onze eerste, zoekt u de eigenschap van het bestand en bewerken van de expressie als volgt:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

De andere parameters in deze expressie kunnen worden geopend en ingevoerd door de dollarteken op het toetsenbord ophaalt in het expressievenster raken. Een van de beschikbare parameters is onze video1bitrate-eigenschap die we die eerder zijn gepubliceerd.

![Toegang krijgen tot parameters in een expressie](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Toegang krijgen tot parameters in een expressie*

Doe hetzelfde voor de bestandsuitvoer voor onze tweede video:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

en voor het bestand met alleen audio-uitvoer:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Als we de bitsnelheid naar een of meer van de video of audio-bestanden nu wijzigt, wordt de desbetreffende encoder opnieuw wordt geconfigureerd en de overeenkomst van de naam bitsnelheid-bestand wordt kracht alle automatische.

##<a id="thumbnails_to__multibitrate_MP4"></a>Miniaturen toevoegen aan multibitrate MP4-uitvoer

Vanaf een werkstroom die [een multibitrate MP4-uitvoer van een MXF input genereert](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), bekijken we nu in de miniaturen toevoegen aan de uitvoer.

###<a id="thumbnails_to__multibitrate_MP4_overview"></a>Overzicht van de werkstroom toe te voegen miniaturen

![Multibitrate MP4-werkstroom starten vanuit](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Multibitrate MP4-werkstroom starten vanuit*

###<a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Toe te voegen JPG-codering

Het hart van onze miniaturen generatie is het onderdeel JPG Encoder, mogen JPG-bestanden.

![JPG Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG Encoder*

We kunnen niet onze stream gecomprimeerd Video echter rechtstreeks sluit op de invoer van de Media-bestand in de JPG-encoder. Deze verwacht in plaats daarvan afzonderlijke frames worden doorgegeven. Dit doet we kunt doen via het onderdeel Video Frame heeft bereikt.

![Een frame heeft bereikt verbinden met de JPG-encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Een frame heeft bereikt verbinden met de JPG-encoder*

Het frame heeft bereikt om de zoveel seconden of frames kunt een videoframe om door te geven. Het interval en de tijd die is verschoven met die dit kan voorkomen is geconfigureerd in de eigenschappen.

![Eigenschappen van video Frame heeft bereikt](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Eigenschappen van video Frame heeft bereikt*

Laten we maken een miniatuur elke minuut door in te stellen van de modus tijd (seconden) en het Interval 60.

###<a id="thumbnails_to__multibitrate_MP4_color_space"></a>Omgaan met kleurruimte conversie

Terwijl logische dat beide pincodes niet-gecomprimeerde Video van het frame heeft bereikt en de invoer van de Media-bestand nu kunnen worden verbonden lijkt, zou doen we er een waarschuwing weergegeven als we zou doen.

![Kleur ruimte invoerfout](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Kleur ruimte invoerfout*

Dit komt omdat de manier waarop in welke kleur informatie wordt weergegeven in onze oorspronkelijke onbewerkte niet-gecomprimeerde video-stream, die afkomstig zijn uit onze MXF, verschilt van wat de JPG-Encoder verwacht. Specifieke taken een zogenaamde "kleur ruimte' van"RGB"of"Grijswaarden"naar verwachting flow in. Dit betekent dat de Video Frame heeft bereikt van binnenkomende video-stream moeten hebben een conversie eerst met betrekking tot de kleurenruimte te toegepast.

Sleep naar de werkstroom kleur ruimte conversieprogramma - Intel en verbindt u deze met onze frame heeft bereikt.

![Verbinding maken van een kleur ruimte conversieprogramma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Verbinding maken van een kleur ruimte conversieprogramma*

Kies in het eigenschappenvenster voor de BGR 24-vermelding in de lijst met vooraf ingestelde.

###<a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>De miniaturen schrijven

Verschilt van de video van onze MP4, het onderdeel JPG Encoder wordt uitvoerbestand meer dan één. Om te kunnen handelen dit, een scène zoeken JPG-bestand schrijver-component kan worden gebruikt: wordt de binnenkomende JPG-miniaturen nemen en ze, elke bestandsnaam worden voorafgegaan door een ander nummer schrijven. (Het getal meestal waarin wordt aangegeven dat het aantal seconden/eenheden in de stream waarin de miniatuur van is getekend.)


![Inleiding tot de schrijver scène zoeken JPG-bestand](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Inleiding tot de schrijver scène zoeken JPG-bestand*

De eigenschap mappad uitvoer configureren met de expressie: ${ROOT_outputWriteDirectory} 

en de bestandsnaam voorvoegsel eigenschap met: 

    ${ROOT_sourceFileBaseName}_thumb_

Het voorvoegsel bepaalt hoe de miniaturen bestanden worden wordt genoemd. Ze wordt voorafgegaan door een getal dat aangeeft van het blokje positie in de stream.


![Eigenschappen van de scène zoeken JPG-bestand schrijver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Eigenschappen van de scène zoeken JPG-bestand schrijver*

Verbinding maken met de schrijver scène zoeken JPG-bestand naar het knooppunt uitvoer bestand/activa.

###<a id="thumbnails_to__multibitrate_MP4_errors"></a>Fouten opsporen in een werkstroom

De invoer van kleur ruimte conversieprogramma verbinden met de onbewerkte niet gecomprimeerd videosessie. Een lokale test uitvoeren voor de werkstroom nu uitvoeren. Er is een goede kans dat de werkstroom wordt ineens niet langer uitgevoerd en geven met een rode omtrek op het onderdeel dat is een fout opgetreden:

![Kleur ruimte conversieprogramma fout](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Kleur ruimte conversieprogramma fout*

Klik op het kleine rode "E"-pictogram in de hoek van het onderdeel kleur ruimte conversieprogramma om te zien wat is de reden poging tot het codering is mislukt.

![Kleur ruimte conversieprogramma fout dialoogvenster](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Kleur ruimte conversieprogramma fout dialoogvenster*

Het blijkt, zoals u ziet, dat de binnenkomende kleurruimte standaard voor kleur ruimte conversieprogramma moeten rec601 voor onze gevraagde conversie van YUV naar RGB heeft. Onze stream niet gehouden de rec601 aangeven. (Records 601 is een standaard voor het coderen van geïnterlinieerde analoge video signalen in digitale video vorm. Hiermee geeft u een actieve gebied die betrekking hebben op 720 helderheid voorbeelden en 360 chrominantie voorbeelden per regel. De kleur codering systeem heet YCbCr 4:2:2.)

U lost dit probleem wordt wordt aangegeven welke op de metagegevens van onze stream die we bent omgaan met rec601 inhoud. We gebruiken een onderdeel Video gegevens Type bijwerken, dat we tussen onze onbewerkte bron- en de component ruimte conversie plaatst kunt doen. Deze gegevens type bijwerken kunt voor het handmatig bijwerken van bepaalde videogegevens eigenschappen. Configureert u deze om aan te geven een kleur ruimte-standaard van "Records 601". Hierdoor wordt de Video gegevens Type bijwerken naar de stream met de "Records 601" kleurruimte markeren als er geen kleurruimte nog gedefinieerd is. (Niet overschrijft alle bestaande metagegevens, tenzij u het selectievakje overschrijven is ingeschakeld.)

![Ruimte-kleurenstandaard op het bijwerken van de typen gegevens bijwerken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Ruimte-kleurenstandaard op het bijwerken van de typen gegevens bijwerken*

###<a id="thumbnails_to__multibitrate_MP4_finish"></a>Klaar werkstroom

Nu ons onze werkstroom is voltooid, voert u een andere test uitvoeren om te zien geven.

![Klaar werkstroom voor multi-mp4-uitvoer met miniaturen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Klaar werkstroom voor multi-mp4-uitvoer met miniaturen*

##<a id="time_based_trim"></a>Op tijd gebaseerde inkorten van multibitrate MP4-uitvoer

Vanaf een werkstroom die [een multibitrate MP4-uitvoer van een MXF input genereert](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), bekijken we nu in het inkorten van het bronvideo op basis van een tijdstempel.

###<a id="time_based_trim_start"></a>Overzicht van de werkstroom toe te voegen inkorten naar

![Werkstroom wilt toevoegen van het inkorten van het om te beginnen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Werkstroom wilt toevoegen van het inkorten van het om te beginnen*

###<a id="time_based_trim_use_stream_trimmer"></a>Gebruik van de Stream Trimmer

De Stream Trimmer-component kunt naar het begin en einde van een invoer stream op informatie (seconden, minuten,...) tijdsinstellingen knippen. De trimmer biedt geen ondersteuning voor inkorten van het frame gebaseerde.

![Stream Trimmer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Stream Trimmer*

In plaats van rechtstreeks de AVC encoders en de luidspreker positie wel statusrapporten koppelen aan de invoer van de Media-bestand, wordt we tussen die de stream trimmer plaatsen. (Één voor de video signaal en één voor het afwisselende audio signaal).

![Stream Trimmer daartussen plaatsen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Stream Trimmer daartussen plaatsen*

Laten we de trimmer zodanig configureren dat we alleen worden verwerkt door video en audio tussen 15 seconden en 60 seconden in de video.

Ga naar de eigenschappen van de Video Stream Trimmer en zowel (gates 15) begintijd en eindtijd (60s) eigenschappen configureren. Zorg ervoor dat zowel onze trimmer audio en video altijd de dezelfde begin en einde-waarden zijn geconfigureerd, publiceert we die in de hoofdmap van de werkstroom.

![Start tijd, een eigenschap uit de Stream Trimmer publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Start tijdeigenschap uit de Stream Trimmer publiceren*

![Eigenschappenvenster voor begintijd publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Eigenschappenvenster voor begintijd publiceren*

![Eigenschappenvenster voor eindtijd publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Eigenschappenvenster voor eindtijd publiceren*


Als we de hoofdsite van onze werkstroom nu controleren, zijn beide eigenschappen netjes weergegeven en configureerbare daarvandaan.

![Gepubliceerde eigenschappen beschikbaar op de hoofdsite](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Gepubliceerde eigenschappen beschikbaar op de hoofdsite*

Nu de eigenschappen van het inkorten van het openen vanuit de audio trimmer en zowel begin- en eindtijden configureren met een expressie die naar de gepubliceerde eigenschappen in de hoofdmap van onze werkstroom verwijst.

Klik op begintijd voor het geluid van de inkorten:

    ${ROOT_TrimmingStartTime}

en voor de eindtijd:

    ${ROOT_TrimmingEndTime}

###<a id="time_based_trim_finish"></a>Klaar werkstroom

![Klaar werkstroom](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Klaar werkstroom*


##<a id="scripting"></a>Inleiding tot de script-Component

Script onderdelen kunnen willekeurige scripts uitvoeren tijdens de uitvoering fasen van onze werkstroom. Er zijn vier verschillende scripts die kunnen worden uitgevoerd, elk voorzien van een specifieke kenmerken en hun eigen plaats in de levenscyclus van werkstroom:

- **commandScript**
- **realizeScript**
- **processInputScript**
- **lifeCycleScript**

De documentatie van het onderdeel script gaat uitgebreider voor elk van de bovenstaande antwoorden. In [de volgende sectie](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), is het uitvoeren van scripts **realizeScript** -onderdeel gebruikt voor het maken van een xml cliplist al doende wanneer de werkstroom wordt gestart. Dit script heet tijdens de installatie onderdeel, wat slechts één keer in de levenscyclus gebeurt.


###<a id="scripting_hello_world"></a>Binnen een werkstroom voor het uitvoeren van scripts: Hallo, wereld

Sleep een onderdeel script naar de ontwerpfunctie oppervlak en wijzig de naam (bijvoorbeeld "SetClipListXML").

![Script onderdelen toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Script onderdelen toevoegen*

Wanneer u de eigenschappen van het onderdeel script controleren, zijn de vier verschillende scripttypen wordt weergegeven, elk configureerbare naar een ander script.

![Eigenschappen van script](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Eigenschappen van script*

Schakel de processInputScript en open de editor voor de realizeScript. Nu bent we instellen en klaar om te beginnen met het uitvoeren van scripts.

Scripts zijn in Groovy, een dynamisch gecompileerd scripttaal voor het Java-platform die compatibel zijn met Java behoudt geschreven. De meeste Java-code is werkelijk, geldige Groovy code.

We gaan schrijven een eenvoudige Hallo wereld groovy script in de context van onze realizeScript. Voer de volgende handelingen uit in de editor:

    node.log("hello world");

Nu uitvoeren een lokale test uitvoeren. Na het uitvoeren, controleren (via het tabblad systeem van de script-Component) de eigenschap Logboeken.

![Hallo wereld log uitvoer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Hallo wereld log uitvoer*

Het knooppuntobject verwijst naar de Logmethode op, verwijst naar onze huidige "knooppunt' of het onderdeel we bent uitvoeren van scripts in. Elk onderdeel heeft als zodanig de mogelijkheid om te logboekregistratie uitvoergegevens, beschikbaar via het tabblad systeem. In dit geval uitvoer we de letterlijke tekenreeks "Hallo allemaal". Belangrijk om te begrijpen Hier ziet u dat dit bewijzen kan moeten een bijzonder nuttig foutopsporing hulpmiddel inzicht van wat het script daadwerkelijk doen.

Van binnen onze omgeving van het uitvoeren van scripts hebben we ook toegang tot eigenschappen op andere onderdelen. Probeer dit:


    //inspect current node: 
    def nodepath = node.getNodePath(); 
    node.log("this node path: " + nodepath);
    
    //walking up to other nodes: 
    def parentnode = node.getParentNode(); 
    def parentnodepath = parentnode.getNodePath(); 
    node.log("parent node path: " + parentnodepath);
    
    //read properties from a node: 
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null ); 
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null); 
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

Het venster van onze logboek weergeven ons het volgende:

![De uitvoer van voor toegang tot Knooppuntpaden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*De uitvoer van voor toegang tot Knooppuntpaden*


##<a id="frame_based_trim"></a>Op basis van een frame inkorten van multibitrate MP4-uitvoer

Vanaf een werkstroom die [een multibitrate MP4-uitvoer van een MXF input genereert](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), bekijken we nu in het inkorten van het bronvideo op basis van het frame-tellingen komen.

###<a id="frame_based_trim_start"></a>Blauwdruk overzicht inkorten naar toevoegen

![Werkstroom voor het toevoegen van het inkorten van het aan](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Werkstroom voor het toevoegen van het inkorten van het aan*

###<a id="frame_based_trim_clip_list"></a>Gebruik van de Knipsellijst XML

In alle vorige werkstroom zelfstudies, we het Media-bestand invoer onderdeel gebruikt als onze video-invoerbron. In dit scenario's met een specifieke echter voortaan we gebruikt de component Clip lijst Source in plaats daarvan. Houd er rekening mee dat dit niet de beste manier van werken moet; de bron van de lijst Clip alleen gebruiken als er een reële reden dat te doen (zoals de onder hoofdletters/kleine letters, waarbij we maakt gebruik van de clip lijst inkorten mogelijkheden).

U kunt overschakelen van de invoer van onze Media-bestand voor de bron van de lijst met illustraties, Sleep de component Clip lijst Source naar het ontwerpvlak en verbindt u de Clip lijst XML-pincode naar het knooppunt XML-lijst van Clip van de werkstroomontwerper. Dit moet de bron van de lijst Clip met uitvoer pincodes, vullen op basis van onze video invoer. Nu verbinding maken met de niet-gecomprimeerde Video en niet-gecomprimeerde Audio pennen uit de de Clip lijst bron voor de desbetreffende AVC Encoders en Audio Stream Interleaver. De invoer van de Media-bestand nu verwijderen.

![De invoer van de Media-bestand vervangen door de bron van de lijst Clip](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*De invoer van de Media-bestand vervangen door de bron van de lijst Clip*

De component Clip lijst Source gaat als invoer een "Clip XML-lijst". Als u het bronbestand om te testen met lokaal selecteert, is deze clip lijst xml automatisch voor u ingevuld.

![Clip lijst XML-eigenschap automatisch ingevuld](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Clip lijst XML-eigenschap automatisch ingevuld*

Zoek een stapje dichter bij de xml, is dit hoe deze eruitziet als:

![Dialoogvenster met de clip lijst bewerken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Dialoogvenster met de clip lijst bewerken*

Dit echter doorgevoerd niet in de mogelijkheden van de clip lijst xml. Eén optie die we hebben is het toevoegen van een element "Knippen" onder beide de video en audio bron, zoals hier:

![Een spaties.wissen element toevoegen aan de knipsellijst](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Een spaties.wissen element toevoegen aan de knipsellijst*

Als u de clip lijst xml strekking hierboven wijzigt en een lokale test uitvoeren uitvoert, ziet u de video correct is bijgesneden tussen 10 en 20 seconden in de video.

In tegenstelling tot wat gebeurt er wanneer u een lokale uitvoeren via doet, deze dezelfde cliplist xml geen hetzelfde effect wanneer ze worden toegepast in een werkstroom die wordt uitgevoerd in Azure Media Services. Wanneer Azure Premium Encoder wordt gestart, wordt de xml cliplist gegenereerd telkens wanneer Klik nogmaals op basis van de invoer bestand die de codering taak is opgegeven. Dit betekent dat de wijzigingen die we op de xml doen helaas zou worden overschreven.

Om de cliplist xml wordt wordt gewist wanneer een codering taak is gestart, we kunnen opnieuw laten genereren al doende net na het begin van onze werkstroom. Aangepaste acties kunnen worden gemaakt door wat een 'script-Component"wordt genoemd. Zie [Inleiding tot de script-Component](media-services-media-encoder-premium-workflow-tutorials.md#scripting)voor meer informatie.


Sleep een onderdeel script naar de ontwerpfunctie oppervlak en wijzig de "SetClipListXML".

![Script onderdelen toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Script onderdelen toevoegen*

Wanneer u de eigenschappen van het onderdeel script controleren, zijn de vier verschillende scripttypen wordt weergegeven, elk configureerbare naar een ander script.

![Eigenschappen van script](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Eigenschappen van script*


###<a id="frame_based_trim_modify_clip_list"></a>De knipsellijst vanuit een script-Component aanpassen

Voordat we de cliplist xml die tijdens de werkstroom voor het opstarten wordt gegenereerd opnieuw schrijven kunt, moeten we toegang hebt tot de cliplist XML-eigenschap en de inhoud ervan. We kunt dit doen als volgt:

    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Binnenkomende knipsellijst aan te melden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Binnenkomende knipsellijst aan te melden*

Eerst moeten we een manier om te bepalen op welke punt tot welk punt we wilt knippen van de video. Als u dit handige aan de gebruiker minder technische van de werkstroom, twee eigenschappen naar de hoofdsite van de grafiek te publiceren. Klik hiertoe met de rechtermuisknop op de ontwerpfunctie oppervlak en selecteer 'Eigenschap toevoegen':

- Eerste eigenschap: "ClippingTimeStart" van het type: "TIJDCODE"
- Tweede eigenschap: "ClippingTimeEnd" van het type: "TIJDCODE"

![Eigenschappenvenster voor schermopname begintijd toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Eigenschappenvenster voor schermopname begintijd toevoegen*

![Tijd steunbalken knippen op de hoofdsite van de werkstroom die zijn gepubliceerd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Tijd steunbalken knippen op de hoofdsite van de werkstroom die zijn gepubliceerd*

Beide eigenschappen die u kunt een geschikte waarde configureren:

![Het begin van de schermopname configureren en te beëindigen eigenschappen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Het begin van de schermopname configureren en te beëindigen eigenschappen*

Nu vanuit in onze script, we toegang tot beide eigenschappen, zoals hier:

    
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Venster van het logboek met begin en einde van de schermopname](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Venster van het logboek met begin en einde van de schermopname*

Laten we de tijdcode tekenreeksen parseren tot een handiger om te gebruiken met behulp van een eenvoudige reguliere expressie:
    
    //parse the start timing: 
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1); 
    node.log("timecode start is: " + starttimecode); 
    def startframerate = startregresult.group(2); 
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend); 
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2); 
    node.log("framerate end is: " + endframerate);

![Logboekvenster met uitvoer van geparseerde tijdcode](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Logboekvenster met uitvoer van geparseerde tijdcode*

Met deze informatie bij de hand, kunnen we nu de xml cliplist zodat de begin- en eindtijden voor het gewenste frame nauwkeurig knipsel van de film wijzigen.

![Scriptcode wilt inkorten elementen toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Scriptcode wilt inkorten elementen toevoegen*

Dit is gedaan tot en met normale tekenreeksbewerkingen voor bewerken. De resulterende lijst xml voor aangepaste clip is geschreven terug naar de eigenschap clipListXML op de hoofdsite van de werkstroom tot en met de 'EigenschapInstellen'-methode. Het logboekvenster na het uitvoeren van een andere test, ziet u ons het volgende:

![De resulterende knipsellijst vastleggen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*De resulterende knipsellijst vastleggen*

Voer een test-klaar om te zien hoe de video en audio-streams hebt zijn afgekapt. Als u hebt meer dan één test-klaar met verschillende waarden voor de inkorten wordt verwezen, ziet u dat die wordt niet worden meegerekend echter! De reden hiervoor is dat de ontwerpfunctie, in tegenstelling tot de Azure runtime, niet de xml cliplist elke uitvoeren overschrijven. Dit betekent dat alleen de eerste keer u hebt ingesteld de in- en out wordt verwezen, de xml wordt in transformeren, alle andere gevallen geldt onze beveiliging-component (als (clipListXML.indexOf ("<trim>") == -1)), wordt voorkomen dat de werkstroom voor het toevoegen van een ander spaties.wissen element wanneer er al een aanwezige.

Als u onze werkstroom handige lokaal test, wordt aanbevolen house behoud code die Hiermee wordt gecontroleerd als een spaties.wissen element al aanwezig was toevoegen. Als dat het geval is, kunnen we deze verwijderen voordat u verdergaat met het wijzigen van de xml met de nieuwe waarden. In plaats van met simpel tekenreeks te springen, is het waarschijnlijk veiliger hiervoor via reële xml-objectmodel parseren.

Voordat we kunt die code toevoegen kunt, hoewel, moet u eerst een aantal importinstructies toevoegen aan het begin van onze script:
    
    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;

Na dit, kunnen we de vereiste opschonen code hebt toegevoegd:

    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already: 
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes: 
    elementsToDelete.each{ 
        e -> e.getParentNode().removeChild(e);
    }; 
    node.log("deleted any existing trim nodes");
    
    //serialize the modified clip list xml dom into a string: 
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result); 
    clipListXML = result.getWriter().toString();
    
Hiermee gaat u deze code net boven het punt waarop we de spaties.wissen elementen aan de xml cliplist toevoegen.

We kunnen nu uitvoeren en onze werkstroom zoals manier die sterk lijkt we horen terwijl de wijzigingen die zijn toegepast ooit tijd wijzigen.    

###<a id="frame_based_trim_clippingenabled_prop"></a>De eigenschap van een ClippingEnabled gemak toe te voegen

Als u niet altijd inkorten wilt mogelijk om optreden, laten we einddatum uit onze werkstroom door toe te voegen een handige Boole-vlag die aangeeft of we inkorten wilt / knippen inschakelen.

Publiceren voordat, net zoals een nieuwe eigenschap in de hoofdmap van onze werkstroom 'ClippingEnabled' genoemd van type 'Booleaanse waarde'.

![Een eigenschap voor het inschakelen van schermopname gepubliceerd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Een eigenschap voor het inschakelen van schermopname gepubliceerd*

Met de onder-Component eenvoudige beveiliging, we kunnen controleren als het inkorten van het is vereist en besluit als onze knipsellijst als zodanig worden gewijzigd moet of niet.

    //check if clipping is required: 
    def clippingrequired = node.getProperty("../ClippingEnabled"); 
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML); 
        node.log("no clipping required"); 
        return; 
    }


###<a id="code"></a>Volledige code

    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;
    
    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping: 
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);
    
    //for local testing: delete any pre-existing trim elements 
    //from the clip list xml by parsing the xml into a DOM:
    
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder(); 
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath(); 
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }
    
    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e -> 
        e.getParentNode().removeChild(e); 
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return; 
    }

    //add trim elements to cliplist xml 
    if ( clipListXML.indexOf("<trim>") == -1 ) 
    {
        //trim video 
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode + 
            " </outPoint>\n </trim> \n"); 
        //trim audio 
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" + 
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML ); 
        node.setProperty("../clipListXml",clipListXML); 
    }


##<a name="also-see"></a>Zie ook 

[Inleiding tot Premium codering in Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Het gebruik van de Premium-codering in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Coderen van de inhoud op aanvraag met Azure Media-Service](media-services-encode-asset.md#media_encoder_premium_workflow)

[Media Encoder Premium werkstroom-indelingen en Codecs](media-services-premium-workflow-encoder-formats.md)

[Voorbeeldbestanden werkstroom](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Hulpprogramma Azure Media Services Explorer](http://aka.ms/amse)

##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
