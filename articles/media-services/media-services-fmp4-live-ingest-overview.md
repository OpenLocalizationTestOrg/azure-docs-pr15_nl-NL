<properties 
    pageTitle="Azure Media Services gefragmenteerde MP4 live nemen specificatie | Microsoft Azure" 
    description="Deze specificatie beschrijft de protocol en de opmaak voor gefragmenteerd MP4 op basis van live streaming opname voor Microsoft Azure Media Services. Microsoft Azure Media Services biedt live streaming service waarmee klanten streamen live gebeurtenissen en inhoud in realtime uitzenden met Microsoft Azure als de cloud-platform. In dit document ook beschreven aanbevolen procedures voor het samenstellen van zeer overtollige en robuuste live regelingen nemen." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"     
    ms.author="cenkdin;juliako"/>

#<a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services gefragmenteerde MP4 live nemen specificatie

Deze specificatie beschrijft de protocol en de opmaak voor gefragmenteerd MP4 op basis van live streaming opname voor Microsoft Azure Media Services. Microsoft Azure Media Services biedt live streaming service waarmee klanten streamen live gebeurtenissen en inhoud in realtime uitzenden met Microsoft Azure als de cloud-platform. In dit document ook beschreven aanbevolen procedures voor het samenstellen van zeer overtollige en robuuste live regelingen nemen.


##<a name="1-conformance-notation"></a>1. de notatie conformiteit

De woorden "moet", moeten "Moet NOT", "REQUIRED", "Vermelde", "Wordt NOT", "SHOULD", "Moet NOT", "Aanbevolen", "Mogelijk" en "optioneel' in dit document worden beschouwd, zoals wordt beschreven in RFC 2119.

##<a name="2-service-diagram"></a>2. Diagram van de Service 

De onderstaande afbeelding ziet de hoogste niveau architectuur van de live streaming-service in Microsoft Azure Media Services:

1.  Live Encoder worden live-feeds in kanalen die zijn gemaakt en deze is ingericht via de Microsoft Azure Media Services SDK.
2.  Kanalen, programma's en Streaming eindpunt in Microsoft Azure Media Services greep alle live streaming functies ingest, opmaak, inclusief cloud DVR, beveiliging, schaalbaarheid en redundantie.
3.  Klanten kunt desgewenst een laag CDN tussen het eindpunt Streaming en de eindpunten van de client implementeren.
4.  Client eindpunten stream vanaf het Streaming-eindpunt dat met HTTP geavanceerde Streaming protocollen (bijvoorbeeld vloeiende Streaming, streepje, schijven of HLS).

![image1][image1]


##<a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3. bits-stream indeling – ISO 14496-12-gefragmenteerd MP4

De indeling kabel is voor live streaming nemen die wordt beschreven in dit document gebaseerd op [ISO-14496-12]. Raadpleeg [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx) voor gedetailleerde uitleg van de indeling MP4 gefragmenteerd en uitbreidingen voor beide bestanden video-on-demand en live streaming opname.

###<a name="live-ingest-format-definitions"></a>Live nemen definities van opmaak 

Hieronder volgt een lijst met speciale opmaak definities die van toepassing op live in Microsoft Azure Media Services nemen:

1. Het deelvenster, 'ftyp legt vast' en 'moov' vak LiveServerManifestBox moeten worden verzonden met elk verzoek om (HTTP POST).  Het aan het begin van de stream moet worden verzonden en altijd en overal de encoder moet opnieuw verbinding maken als u wilt hervatten stream nemen.  Raadpleeg sectie 6 in [1] voor meer informatie.
2. Sectie 3.3.2 in [1] definieert een optioneel vak StreamManifestBox voor live genoemd nemen. Vanwege de routering logica van Microsoft Azure van taakverdeling, gebruik van dit selectievakje is afgeschaft en moet niet aanwezig wanneer ingesting in Microsoft Azure Media-Service. Als dit vakje aanwezig is, Azure Media Services stilte deze genegeerd.
3. De TrackFragmentExtendedHeaderBox gedefinieerd in 3.2.3.2 in [1] zijn voor elke fragment aanwezig.
4. Versie 2 van de TrackFragmentExtendedHeaderBox moet worden gebruikt om te genereren mediasegmenten met identieke URL's in meerdere datacenters. Het veld fragment index vereist is voor het cross-datacenter van de overname van het index gebaseerde streaming-indelingen zoals Apple HTTP Live Streaming (HLS) en op basis van een index MPEG-streepje.  Om in te schakelen cross-datacenter failover, moet de index fragment worden gesynchroniseerd op meerdere encoders en vergroten door 1 voor elke opeenvolgende media-fragment, zelfs op encoder opnieuw opstarten of fouten.
5. Sectie 3.3.6 in [1] definieert vak MovieFragmentRandomAccessBox (mfra) die aan het einde van live opname mogelijk worden verzonden om aan te geven EOS (einde-van-Stream) naar het kanaal genoemd. Gebruik van EOS (einde-van-Stream) is afgeschaft vanwege de logica ingest van Azure Media Services, en het vak 'mfra' voor live opname niet moeten worden verzonden. Als verzonden, Azure Media Services stilte deze genegeerd. Het wordt aanbevolen [Kanaal opnieuw instellen](https://msdn.microsoft.com/library/azure/dn783458.aspx#reset_channels) met de status van de komma ingest opnieuw instellen en ook het wordt aanbevolen om te [Stoppen met programma](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) gebruiken tot het einde van een presentatie en de stream.
6. De duur van de fragment MP4 moet constante, om te verkleinen van de grootte van de client-manifesten en client downloaden heuristiek tot en met gebruik van herhaalde tags te verbeteren.  De duur kan om te dit voor geheel frame tarieven variëren.
7. De duur van de fragment MP4 moet liggen tussen ongeveer 2 en 6 seconden.
8. MP4 fragment tijdstempels en indexen (TrackFragmentExtendedHeaderBox fragment_ absolute_ tijd en fragment_index) moeten komen terecht in oplopende volgorde.  Hoewel Azure Media Services robuuste op dubbele fragmenten is, heeft dit zeer beperkte mogelijkheden voor het opnieuw ordenen fragmenten op basis van de tijdlijn media.

##<a name="4-protocol-format--http"></a>4. protocol indeling – HTTP

ISO gefragmenteerd MP4 op basis van live nemen for Microsoft Azure Media Services wordt een standaard langdurige HTTP POST verzoek om deel te zenden gecodeerd mediagegevens verpakt in de indeling MP4 gefragmenteerd bij de service. Elke HTTP-POST verzendt een volledige gefragmenteerd MP4 bits-stream ("Stream") vanaf beginnen met de koptekstvakken ('ftyp legt vast', 'Live Server bestandenlijst vak' en 'moov' vak) en kunt verdergaan met een reeks fragmenten ('moof' en 'mdat' vakken). Raadpleeg de sectie 9.2 in [1] voor de syntaxis van de URL voor HTTP POST-aanvraag. Een voorbeeld van de URL van het bericht luidt als volgt: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

###<a name="requirements"></a>Vereisten

Hier volgen de uitgebreide vereisten:

1. Encoder moet de uitzending beginnen door een HTTP POST-aanvraag met een lege 'hoofdtekst' (inhoud lengte nul) met dezelfde opname URL te verzenden. Dit kan helpen snel detecteren als het eindpunt live opname geldig is en er is een verificatie of andere voorwaarden die zijn vereist. Per HTTP-protocol, de server niet mogelijk om te achterste HTTP-antwoord verzenden totdat de gehele aanvraag, met inbegrip van de hoofdtekst van het bericht is ontvangen. Gezien het langdurige karakter van live-gebeurtenis, zonder deze stap de encoder niet mogelijk om te bepalen voor elke fout totdat het klaar is met het verzenden van alle gegevens.
2. Encoder moet omgaan met eventuele fouten of verificatie uitdagingen grond (1). Als (1) is uitgevoerd met een reactie 200, gaat u verder.
3. Encoder moet een nieuwe HTTP POST-aanvraag beginnen met de gefragmenteerde MP4-stream.  De nettolading moet beginnen met de koptekstvakken gevolgd door fragmenten.  Houd rekening met dat het vak 'ftyp legt vast', 'Live Server bestandenlijst vak' en 'moov' (in deze volgorde) moet worden verzonden met elk verzoek om een, zelfs als de encoder opnieuw verbinding maken moet omdat de vorige aanvraag is beëindigd vóór het einde van de stream. 
4. Encoder moet gedeelde overbrengen codering gebruiken voor het uploaden van niet te voorspellen de lengte van de volledige inhoud van de live gebeurtenis.
5. Wanneer de gebeurtenis is afgelopen, na het verzenden van het laatste fragment, moet het gedeelde overbrengen codering bericht reeks weergeven (de meeste HTTP-client stapels verwerkt door automatisch) zonder problemen in de encoder eindigen. Encoder moet wachten tot de service retourneren van het definitieve antwoord-code en vervolgens de verbinding verbreken. 
6. De Events() zelfstandig naamwoord Encoder moet niet gebruiken zoals is beschreven in 9.2 in [1] voor live opname in Microsoft Azure Media Services.
7. Als de HTTP POST-aanvraag wordt beëindigd of treedt er een vóór het einde van de stream met een fout TCP time-out, moet de encoder een nieuw bericht verzoek verzenden met behulp van een nieuwe verbinding en volgt u de vereisten boven aan de extra eis dat de encoder moet opnieuw verzenden van de vorige twee MP4-fragmenten voor elke track in de stream en ga verder met zonder Kennismaking met wijzigingen in de tijdlijn media.  Opnieuw verzenden van de laatste twee MP4-fragmenten voor elke bijhouden zorgt ervoor dat er geen gegevens verloren gaan.  Met andere woorden, als een stream een audio- en videogesprekken bijhouden bevat en de huidige POST-aanvraag mislukt, moet de encoder opnieuw verbinding maakt en opnieuw verzenden van de laatste twee fragmenten voor de audio schema, zijn eerder verzonden, en de laatste twee fragmenten voor de video schema, die zijn eerder verzonden, om ervoor te zorgen dat er geen gegevens verloren gaan.  De encoder moet een "doorsturen" buffer van media fragmenten, waarin deze opnieuw bij het verbinden behouden.

##<a name="5-timescale"></a>5. tijdschaal 

Het gebruik van "Tijdschaal" beschreven [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) voor SmoothStreamingMedia (sectie 2.2.2.1), StreamElement (sectie 2.2.2.3), StreamFragmentElement(2.2.2.6) en LiveSMIL (sectie 2.2.7.3.1). Als de waarde van de tijdschaal niet aanwezig is, is de standaardwaarde gebruikt 10,000,000 (10 MHz). Hoewel vloeiende Streaming notatiespecificatie gebruik van andere waarden van de tijdschaal, meestal de encoder implementaties doeleinden niet blokkeren deze standaardwaarde (10 MHz) te genereren nemen vloeiende Streaming gegevens. Aangezien het [Dynamische verpakking van Azure Media](media-services-dynamic-packaging-overview.md) -functie is het aanbevolen 90 kHz tijdschaal voor video-streams en 44,1 of 48.1 kHz wilt gebruiken voor audio-streams. Als verschillende tijdschaal waarden worden gebruikt voor verschillende gegevensstromen, kan de stream niveau tijdschaal moet worden verzonden. Raadpleeg [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

##<a name="6-definition-of-stream"></a>6. definitie van "Stream"  

"Stream" is de basiseenheid van bewerking in live opname voor het opstellen van live presentatie, streaming failover en redundantie scenario's verwerken. 'Stream' is gedefinieerd als één unieke gefragmenteerd MP4 bits-stream dat één track kan bevatten of meerdere nummers. Een volledige live presentatie kan een of meer streams afhankelijk van de configuratie van de live encoder(s) bevatten. De onderstaande voorbeelden illustreren verschillende opties van het gebruik van stream(s) voor het opstellen van een volledige live-presentatie.

**Voorbeeld:** 

Klant wil een live streaming presentatie, waaronder de volgende audio-of videogesprek bitsnelheid maken:

Video: 3000 k, 1500 k, 750 k

Audio-128 k

###<a name="option-1-all-tracks-in-one-stream"></a>Optie 1: Alle nummers in één stroom

In deze optie wordt een enkel encoder genereert alle audio-of videogesprek nummers en ze bundelen in één gefragmenteerd MP4 bits-stream die vervolgens wordt verzonden via een enkele HTTP POST-verbinding. In dit voorbeeld is er slechts één stream voor deze live presentatie:

![image2][image2]

###<a name="option-2-each-track-in-a-separate-stream"></a>Optie 2: Elke bijhouden in een afzonderlijk stroom

In deze optie wordt de encoder(s) alleen plaats een bijhouden in elke Fragment MP4-bits-stream en alle streams posten via meerdere afzonderlijke HTTP-verbindingen. Dit kan worden uitgevoerd met één encoders of meerdere encoders. Vanuit live opname oogpunt, deze live presentatie bestaat uit vier streams.

![image3][image3]

###<a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Optie 3: Bundelen audio bijhouden met de laagste bitsnelheid video bijhouden in één stream

In deze optie wordt de klant wil het audionummer met de laagste bitsnelheid video bijhouden in één Fragment MP4-bits-stream bundelen en laat de andere twee video sporen elke wordt een eigen stream. 


![image4][image4]


###<a name="summary"></a>Overzicht

Wat wordt weergegeven boven is niet een volledige lijst van alle mogelijke opname opties voor dit voorbeeld. Als een sterker nog, wordt groeperen van de nummers in streams ondersteund door het live innemen. Klanten en encoder leveranciers kunnen hun eigen implementaties, op basis van technische complexiteit, encoder capaciteit, en overbodige en failover overwegingen kiezen. Echter moet worden vermeld dat in de meeste gevallen er slechts één-nummer voor de hele presentatie live is dus het is belangrijk om ervoor te zorgen de gezondheid van de stream ingest met de audio bijhouden. Deze aandacht vaak levert audionummer in een eigen stream (bijvoorbeeld de optie 2) te plaatsen of bundeling deze met de laagste bitsnelheid video bijhouden (bijvoorbeeld de optie 3). Ook voor een betere redundantie en fouttolerantie nemen verzendt het geluid van de dezelfde bijhouden in twee verschillende stromen (optie 2 met overtollige audio nummers) of de audio bijhouden ten minste twee van de laagste bitsnelheid video nummers (optie 3 met audio in ten minste twee video-streams gebundelde) ten zeerste voor live aanbevolen wordt bundeling in Microsoft Azure Media Services.

##<a name="7-service-failover"></a>7. Failover Service 

Goede failover-ondersteuning is aard live streaming, kritieke voor zorgen dat de beschikbaarheid van de service. Microsoft Azure Media Services is bedoeld om het verwerken van verschillende soorten fouten zoals netwerkfouten, serverfouten, opslag, problemen, enzovoort. Wanneer gebruikt in combinatie met correcte foutvrije logica van de kant live encoder, kunt klant een uiterst betrouwbaar live streaming service vanuit de cloud bereiken.

In dit gedeelte bespreken we service failover-scenario's. In dit geval de fout gebeurt er ergens in de service en doet zich voor als een netwerkfout. Hier volgen enkele aanbevelingen voor de uitvoering encoder voor het afhandelen van service failover:


1. Gebruik een 10 tweede time-out voor het maken van de TCP-verbinding.  Als een poging tot stand brengen van de verbinding langer duren dan 10 seconden, de bewerking afbreken en probeer het opnieuw. 
2. Gebruik een korte time-out voor het verzenden van de HTTP-aanvraag bericht delen.  Als de duur van de doelsite MP4-fragment N seconden is, gebruikt u een time-out verzenden tussen N en 2N seconden; Gebruik bijvoorbeeld een-out van 6 t/m 12 seconden als de duur van de fragment MP4 6 seconden.  Als er een time-out optreedt, opnieuw instellen van de verbinding en opent u een nieuwe verbinding cv-stream nemen op de nieuwe verbinding. 
3. Voor het behoud van een lopende buffer met de laatste twee fragmenten, voor elke schema, die zijn correct en volledig zijn verzonden naar de service.  Als de HTTP POST-aanvraag voor een stream is beëindigd of treedt er een time-out voorafgaand aan het einde van de stream, opent u een nieuwe verbinding en beginnen met een ander HTTP POST-aanvraag, opnieuw verzenden van de stream koppen opnieuw verzenden van de laatste twee fragmenten voor elk nummer en de stream zonder Inleiding tot een discontinuity in de tijdlijn media hervatten.  Hierdoor wordt de kans op verlies van gegevens.
4. Het wordt aanbevolen dat de encoder doet geen afbreuk aan het aantal pogingen een verbinding tot stand brengen of hervatten streaming nadat een TCP-fout is opgetreden.
5. Na een TCP-fout:
    1. De huidige verbinding moeten eerst worden afgesloten en een nieuwe verbinding moet worden gemaakt voor een nieuwe HTTP POST-aanvraag.
    2. De nieuwe HTTP bericht URL moet hetzelfde als de eerste POST-URL.
    3. De nieuwe HTTP bericht moet opnemen stream kopteksten ('ftyp legt vast', 'Live Server bestandenlijst vak' en 'moov' vak) gelijk aan de kop van de stream in het oorspronkelijke bericht.
    4. De laatste twee fragmenten verzonden voor elk nummer moeten worden verzonden en streaming hervatten zonder Inleiding tot een discontinuity in de tijdlijn media.  De tijdstempels MP4-fragment oplopen continu, zelfs op HTTP POST-aanvragen.
6. De HTTP POST-aanvraag moet worden beëindigd in de encoder als gegevens niet met een proportionele met de duur van de fragment MP4 snelheid wordt verzonden.  Een HTTP POST-aanvraag die geen gegevens verzendt kunt voorkomen dat Azure Media Services snel de encoder verbreken in het geval van een service-update.  Daarom de HTTP-POST voor verspreid (ad signaal) nummers moeten worden korte gehouden, wordt beëindigd zodra het verspreid fragment wordt verzonden.

##<a name="8-encoder-failover"></a>8. Failover encoder

Encoder failover is de tweede type failover scenario dat moet worden toegewezen voor de bezorging van end-to-end-live-streaming. In dit scenario wordt de fout is er gebeurd aan de kant encoder. 

![image5][image5]


Hieronder vindt u de verwachtingen van het eindpunt live opname wanneer encoder failover gebeurt:

1. Een nieuw exemplaar van de encoder moet worden gemaakt om door te gaan streaming, zoals in het bovenstaande diagram (Stream voor 3000 k video met stippellijn).
2. De nieuwe encoder moet dezelfde URL gebruiken voor HTTP POST-aanvragen als het exemplaar is mislukt.
3. De nieuwe encoder POST-aanvraag moet bevatten de dezelfde gefragmenteerde MP4 koptekstvakken als het exemplaar is mislukt.
4. De nieuwe encoder moet correct worden gesynchroniseerd met alle andere actieve encoders voor dezelfde live presentatie gesynchroniseerde audio-of videogesprek voorbeelden met uitgelijnde fragment grenzen genereren.
5. De nieuwe stream moet semantisch gelijk met de vorige stream en verwisselbare op niveau van de kop- en fragment.
6. De nieuwe encoder moet proberen te minimaliseren verlies van gegevens.  De fragment_absolute_time en fragment_index van media fragmenten moeten verhogen van het punt waar de encoder laatst gestopt.  De fragment_absolute_time en fragment_index moeten verhogen in een doorlopend manier, maar het is toegestaan in te voeren van een discontinuity indien nodig.  Azure Media Services negeert fragmenten die dit al hebt ontvangen en verwerkt, zodat u beter foutbericht aan de zijkant van fragmenten dan introduceren wijzigingen in de tijdlijn media om te verzenden. 

##<a name="9-encoder-redundancy"></a>9. redundantie encoder 

Voor bepaalde kritieke live gebeurtenissen dat nog hogere beschikbaarheid van de aanvraag en kwaliteit van ervaring, het wordt aanbevolen om redundante encoders actieve om te bereiken naadloze failover met geen gegevens verloren gaan.

![image6][image6]

Zoals u in het bovenstaande diagram, zijn er twee groep encoders twee kopieën van elke gegevensstroom tegelijk te drukken in de live-service. Deze instelling wordt ondersteund omdat Microsoft Azure Media Services heeft de mogelijkheid om uit dubbele fragmenten op basis van de stream-ID en fragment tijdstempel te filteren. De resulterende live gegevensstroom en het archief worden één kopie van alle streams die het best mogelijke aggregatie uit de twee bronnen. Bijvoorbeeld in een hypothetische extreme hoofdletters/kleine letters, mits er één encoder (hoeft te zijn hetzelfde) uitgevoerd op elk gewenst moment in tijd voor elke gegevensstroom, de resulterende live gegevensstroom van de service is doorlopend zonder verlies van gegevens. 

Het vereiste voor dit scenario is nagenoeg hetzelfde als de vereisten Encoder Failover hoofdlettergebruik met de uitzondering die de tweede reeks encoders op hetzelfde moment als de primaire encoders worden uitgevoerd.

##<a name="10-service-redundancy"></a>10. redundantie Service  

Voor ten zeerste overtollige globale-verdeling gebruikt, is het soms vereist voor het back-up van cross-regio worden afgehandeld regionale systeemfouten hebt. Gegevensniveaus uitvouwen op de topologie 'Encoder redundantie', kunnen klanten kiezen voor een overtollige service-implementatie in een ander gebied dat is verbonden met het 2e set encoders hebt. Klanten kunnen ook werken met een CDN-provider voor de implementatie van een GTM (globale verkeer Manager) vóór de twee service-implementaties om te leiden naadloos client-verkeer is toegestaan. De vereisten voor de encoders zijn hetzelfde als de zaak 'Encoder redundantie' met de enige uitzondering die de tweede reeks encoders hoeft te worden doorverwezen naar een andere live eindpunt nemen. De onderstaande afbeelding ziet u deze instellingen:

![image7][image7]

##<a name="11-special-types-of-ingestion-formats"></a>11. speciale typen opname-indelingen 

In deze sectie wordt een speciaal type live opname-indelingen die zijn ontworpen om aan sommige specifieke scenario's beschreven.

###<a name="sparse-track"></a>Verspreid bijhouden

Wanneer u een live streaming presentatie met uitgebreide Clientervaring, is het meestal nodig zijn voor het verzenden van gebeurtenissen keer is gesynchroniseerd of signalen in-band met de gegevens van de belangrijkste media. Een voorbeeld hiervan is dynamische live advertenties invoegen. Dit soort gebeurtenis-signalering verschilt van de normale streaming vanwege de verspreid aard audio-of videogesprek. Met andere woorden, de signalering gegevens meestal niet plaatsvindt continu en het interval kan moeilijk zijn te voorspellen. Het concept van het verspreid bijhouden is speciaal ontworpen om te nemen en uitzenden in-band signalering van gegevens.

Hieronder vindt u een aanbevolen implementatie voor ingesting verspreid bijhouden:

1. Maak een afzonderlijke gefragmenteerd MP4 bits-stream die alleen verspreid nummer (s) zonder audio-of videogesprek nummers bevat.
2. Klik in het' Live Server bestandenlijst"zoals gedefinieerd in de sectie 6 in [1] door"parentTrackName"parameter te gebruiken om op te geven van de naam van het bijhouden van de bovenliggende. Raadpleeg de sectie 4.2.1.2.1.2 in [1] voor meer informatie.
3. Klik in het' Live Server bestandenlijst"manifestOutput moet zijn ingesteld op"true".
4. De verspreid aard van de signalering gebeurtenis gegeven, wordt het aanbevolen dat:
    1. Aan het begin van de live gebeurtenis verzonden encoder de koptekstvakken voor de eerste naar de service waarmee de service de verspreid bijhouden in het manifest client registreren.
    2. De encoder moet de HTTP POST-aanvraag beëindigen wanneer de gegevens niet wordt verzonden.  Een langdurige HTTP-bericht dat u geen gegevens verzendt kunt voorkomen dat Azure Media Services snel de encoder verbreken bij service bijwerken of server opnieuw opstarten, zoals de media-server tijdelijk een ontvangen betrekking heeft op de socket worden geblokkeerd. 
    3. Tijdens de periode wanneer signalering van gegevens niet beschikbaar is, de encoder SHOULD sluiten het HTTP-bericht ook zelf aanvragen.  Terwijl de POST-aanvraag actief is, de encoder moet worden gebruikt voor het verzenden van gegevens 
    4. Bij het verzenden van verspreid fragmenten kunt encoder expliciete inhoud lengte koptekst instellen als deze beschikbaar is.
    5. Bij het verzenden van verspreid fragment met een nieuwe verbinding moet encoder beginnen met het verzenden van de koptekstvakken gevolgd door de nieuwe fragmenten. Dit is worden afgehandeld wanneer failover is er gebeurd met tussen en de nieuwe verspreid verbinding wordt gemaakt met een nieuwe server die niet zichtbaar is voor de verspreid bijhouden voordat.
    6. Het fragment verspreid bijhouden komen beschikbaar voor de client wanneer de bijbehorende bovenliggende bijhouden fragment met gelijk of groter tijdstempel waarde aan de client beschikbaar wordt gesteld. Als het verspreid fragment een tijdstempel van t heeft = bijvoorbeeld 1000, naar verwachting nadat de client ziet video (aangenomen dat de naam van het bovenliggende bijhouden video) fragment tijdstempel 1000 of hoger, deze het verspreid fragment t kunt downloaden = 1000. Houd er rekening mee dat het werkelijke signaal kan uitstekend worden gebruikt voor een andere positie in de tijdlijn presentatie zijn aangewezen bestemming. In het bovenstaande voorbeeld is het mogelijk dat het verspreid fragment van t = 1000 heeft een XML-nettolading dat wil zeggen voor het invoegen van een advertentie in een positie die een paar seconden later.
    7. De nettolading van verspreid bijhouden fragment kan zijn in verschillende verschillende indelingen (bijvoorbeeld XML- of tekst of binaire, enzovoort) afhankelijk van de verschillende scenario's. 


###<a name="redundant-audio-track"></a>Overtollige Audio bijhouden

In een typisch HTTP geavanceerde Streaming scenario (bijvoorbeeld vloeiende Streaming of streepje) is vaak slechts één audionummer in de hele presentatie. In tegenstelling tot video sporen waarin meerdere kwaliteit niveaus voor de client kunt kiezen uit in de foutvoorwaarden, kan het audionummer een potentieel risico zijn als de opname van de stream waarin het audionummer verbroken is. 

U lost dit probleem, ondersteunt Microsoft Azure Media Services live opname van overtollige audio nummers. Het idee is dat het hetzelfde audionummer kan worden verzonden meerdere keren in verschillende stromen. Terwijl de service wordt alleen hebt geregistreerd de audionummer eenmaal in het manifest client, is het gebruikmaken van overtollige audio nummers als back-ups voor het ophalen van audio fragmenten als de primaire-nummer is problemen ondervindt. Om op te nemen overtollige audio sporen, de encoder moet:

1. Maak de dezelfde audionummer in meerdere Fragment MP4-bits-streams. De overtollige audio nummers moeten semantisch equivalente met precies het dezelfde fragment tijdstempels en verwisselbare op niveau van de kop- en fragment.
2. Zorg ervoor dat het 'geluid' vermelding in de Live Server bestandenlijst (sectie 6 in [1]) niet hetzelfde zijn voor alle overbodige audio nummers.

Hieronder ziet u een aanbevolen implementatie voor overtollige audio nummers:

1. Elke unieke audionummer verzenden in een stream door zelf.  Ook een overtollige stream verzenden voor elk van deze stromen audio bijhouden, waar de 2e stream van de 1e alleen op basis van de id in de HTTP POST-URL verschilt: {protocol} :// {serveradres} / {punt path}/Streams({identifier}) publiceren.
2. Afzonderlijke streams gebruiken om de twee laagste video bitsnelheid. Elk van deze streams moet ook een kopie van elk unieke audionummer bevatten.  Wanneer er meerdere talen worden ondersteund, moeten deze streams bijvoorbeeld audio nummers voor elke taal bevatten.
3. Afzonderlijke server (encoder) instanties gebruiken voor het coderen en stuur de overtollige streams die worden genoemd in (1) en (2). 



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png

 