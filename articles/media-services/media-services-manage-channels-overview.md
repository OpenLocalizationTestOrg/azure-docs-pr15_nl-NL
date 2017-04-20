<properties 
    pageTitle="Overzicht van Live indien gestoomd met behulp van Azure Media Services | Microsoft Azure" 
    description="Dit onderwerp biedt een overzicht van live indien gestoomd Azure Media Services gebruiken." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="juliako"/>

#<a name="overview-of-live-steaming-using-azure-media-services"></a>Overzicht van Live indien gestoomd Azure Media Services gebruiken

##<a name="overview"></a>Overzicht

De volgende onderdelen zijn meestal betrokken tijdens het geven van live streaming gebeurtenissen met Azure Media Services:

- Een camera die wordt gebruikt voor het uitzenden van een gebeurtenis.
- Een live video encoder die signalen tussen de camera converteert naar streams die zijn verzonden naar een live streaming service.

    U kunt ook meerdere live tijd gesynchroniseerd encoders. Voor bepaalde kritieke live gebeurtenissen die zeer hoog beschikbaarheid van de aanvraag en kwaliteit van ervaring, het wordt aanbevolen om redundante encoders actieve met tijdsynchronisatie om te bereiken naadloze failover met geen gegevens verloren gaan.
- Een live streaming service waarmee u het volgende doen:
    
    - live met behoud van verschillende live streaming protocollen (bijvoorbeeld RTMP of vloeiende Streaming), nemen
    - (optioneel) coderen van de stream naar geavanceerde bitsnelheid
    - Voorbeeld van uw live stream
    - opnemen en de aangezogen inhoud opslaan om te worden streamen later (Video-on-Demand)
    - rechtstreeks naar uw klanten, of naar een inhoud bezorging netwerk (CDN) voor verdere distributie, kunt u de inhoud via algemene streaming protocollen (bijvoorbeeld MPEG streepje, vloeiend, HLS, schijven) aanbieden.


**Microsoft Azure mediaservices** (AMS) biedt de mogelijkheid om te nemen, coderen, voorbeeld, opslaan en geven van uw live streaming inhoud.

Tijdens het geven van uw inhoud naar klanten wordt uw doel is om aan te geven van een video van hoge kwaliteit voor verschillende apparaten onder andere netwerkcondities. Gebruik hiervoor live encoders coderen van de stream op een multi-bitrate (geavanceerde) video bitsnelheid.  Als u wilt verrichten streaming op verschillende apparaten, gebruik u Media Services [dynamische verpakking](media-services-dynamic-packaging-overview.md) uw stream naar verschillende protocollen dynamisch opnieuw inpakken. Media Services ondersteunt bezorging van de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders).

In Azure Media Services verwerken **kanalen**, **programma's**en **StreamingEndpoints** alle live streaming functies ingest, inclusief opmaak, DVR, beveiliging, schaalbaarheid en redundantie.

Een **kanaal** vertegenwoordigt een pijplijn voor het verwerken van live streaming inhoud. Een kanaal kan ontvangen een live streams ingevoerd in de volgende manieren:

- Een on-premises live encoder verzendt multi-bitrate **RTMP** of **Vloeiende Streaming** (gefragmenteerde MP4) naar het kanaal dat is geconfigureerd voor de bezorging van **Pass Through-query** . De bezorging **Pass Through-query** is wanneer de geconsumeerde streams **kanaal**s doorgegeven zonder verdere verwerking. U kunt de volgende live encoders die uitvoer multi-bitrate vloeiende Streaming: vorm van het element, Envivio, zoals Cisco.  De volgende live encoders uitvoer RTMP: Adobe Flash Media Live Encoder (FMLE), Telestream Wirecast en Tricaster transcoders.  Een live encoder kunt ook een één bitsnelheid verzenden naar een kanaal die niet is ingeschakeld voor het coderen van live, maar die niet wordt aanbevolen. Wanneer een aangevraagd, biedt Media Services de stream naar klanten.

    >[AZURE.NOTE] Met behulp van een Pass Through-methode, is de voordeligste manier om te live streaming wanneer u mee bezig zijn meerdere gebeurtenissen via een lange periode en u hebt al worden geïnvesteerd in on-premises implementatie encoders. Zie [prijzen](/pricing/details/media-services/) details.
    
    
- Een on-premises implementatie live encoder stuurt een enkel-bitsnelheid naar het kanaal dat is ingesteld voor het uitvoeren van live coderen met Media-Services op een van de volgende indelingen: RTMP of vloeiende Streaming (gefragmenteerde MP4). RTP (MPEG-TS) wordt ook ondersteund, mits u speciale verbinding met het Azure Datacenter hebt. De volgende live encoders met RTMP uitvoer bekend zijn voor gebruik met kanalen van dit type: Telestream Wirecast, FMLE. Het kanaal voert live codering van de binnenkomende één bitsnelheid op een multi-(geavanceerde) video bitsnelheid. Wanneer een aangevraagd, biedt Media Services de stream naar klanten.


Vanaf de versie van de Media Services 2.10, wanneer u een kanaal maken, kunt u opgeven op welke manier u voor uw kanaal voor het ontvangen van de invoer stream en of u wilt voor het kanaal om uit te voeren live coderen van de stream. U hebt twee opties:

- **Geen** (Pass Through-query) – deze waarde opgeven als u van plan bent een on-premises implementatie live encoder waarin de gegevens uit meerdere-bitsnelheid (een Pass Through-stream) gebruiken. In dit geval de binnenkomende stream doorgegeven aan de uitvoer zonder alle codering. Dit is het gedrag van een kanaal vóór 2,10 release.  

- **Standaard** – deze waarde, als u van plan bent het gebruik van Media Services voor het coderen van uw één live bitsnelheid op multi-bitsnelheid kiezen. Deze methode is rendabeler voor schaalbaarheid van snel voor onregelmatig gebeurtenissen. Er rekening mee dat er een factureringsbeheerder gevolgen voor het coderen van live en u onthouden moet dat een live codering kanaal blijft in de stand "Uitgevoerd" wordt gelden de facturering tarieven.  Het wordt aanbevolen dat u uw actieve kanalen onmiddellijk gestopt nadat uw live streaming kan worden geen extra per uur te betalen. 

##<a name="comparison-of-channel-types"></a>Vergelijking van typen kanalen

Volgende tabel vindt u een handleiding voor het vergelijken van de twee typen kanalen, die worden ondersteund in Media Services

Functie|Pass Through-query kanaal|Standaard-kanaal
---|---|---
Eén bitsnelheid invoer gecodeerd naar meerdere bitsnelheid in de cloud|Nee|Ja
Maximale resolutie, aantal lagen|1080p, 8 lagen, 60 + fps|720p, 6 lagen, 30 fps
Invoer protocollen|RTMP, vloeiend Streaming|RTMP, vloeiende Streaming en RTP
Prijs|Zie de [prijzen van de pagina](/pricing/details/media-services/) en klik op 'Live Video' tabblad|Zie de [prijzen van pagina](/pricing/details/media-services/) 
Maximum runtime|24 x 7|8 uur
Ondersteuning voor het invoegen van leien|Nee|Ja
Ondersteuning voor ad-signalering|Nee|Ja
Pass Through-query CEA 608/708 bijschriften|Ja|Ja
Mogelijkheid om te herstellen uit kort blokkeringen in bijdrage feed|Ja|Niet (kanaal begint slating na 6 + seconden zonder invoergegevens)
Ondersteuning voor de invoer GOPs ongelijkmatige|Ja|Niet-invoer moet worden opgelost 2sec GOPs
Ondersteuning voor variabele frame tarief invoer|Ja|Niet-invoer moet worden vast frame rentetarief.<br/>Secundaire variaties zijn toegestaan, bijvoorbeeld tijdens hoog beweging scènes. Maar encoder naar 10 frames/sec niet verwijderen.
Automatisch-signalen met kanalen wanneer input feed is verbroken|Nee|Na 12 uur, als er geen programma wordt uitgevoerd 

##<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Werken met kanalen die live multi-bitsnelheid van on-premises implementatie encoders (Pass Through-query ontvangt)

In het volgende diagram ziet u de belangrijkste onderdelen van het platform AMS die betrekking hebben op in de werkstroom **Pass Through-query** .

![Live werkstroom](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Zie voor meer informatie [werken met kanalen die ontvangen Multi-Live bitsnelheid van On-premises Encoders](media-services-live-streaming-with-onprem-encoders.md).

##<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Werken met kanalen die zijn ingeschakeld om uit te voeren live coderen met Azure Media Services

In het volgende diagram ziet u de belangrijkste onderdelen van het platform AMS die betrekking hebben op in Live Streaming werkstroom waarin een kanaal om uit te voeren live coderen met Media-Services is ingeschakeld.

![Live werkstroom](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Zie [werken met kanalen die voor het uitvoeren van Live coderen met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie.

##<a name="description-of-a-channel-and-its-related-components"></a>Beschrijving van een kanaal en de bijbehorende onderdelen

###<a name="channel"></a>Kanaal

In Media Services zijn [kanaal](https://msdn.microsoft.com/library/azure/dn783458.aspx)s verantwoordelijk voor het verwerken van live streaming inhoud. Een kanaal biedt een invoer eindpunt (nemen URL) dat u vervolgens naar een live transcoder opgeven. Het kanaal live invoer gegevensstromen ontvangt van de live transcoder en beschikbaar zijn voor het streamen via een of meer StreamingEndpoints. Kanalen bieden ook een voorbeeld-eindpunt (voorbeeld-URL) die u kunt een voorbeeld bekijken en de stream valideren voordat nadere verwerking en de bezorging van.

U kunt ophalen de URL ingest en de preview-URL wanneer u het kanaal maakt. Als u deze URL's, moet het kanaal niet worden gestart. Wanneer u klaar om te beginnen om gegevens uit een live transcoder in het kanaal bent, kan het kanaal moet worden gestart. Wanneer het live transcoder wordt gestart ingesting gegevens, kunt u uw stream bekijken.

Elk Media Services-account kan bevatten meerdere kanalen, meerdere programma's en meerdere StreamingEndpoints. Afhankelijk van de behoeften bandbreedte en beveiliging kunnen StreamingEndpoint services worden toegewezen aan een of meer kanalen. Een StreamingEndpoint kunt ophalen uit een kanaal.


###<a name="program"></a>Programma 

Een [programma](https://msdn.microsoft.com/library/azure/dn783463.aspx) kunt u de publicatie en opslag van segmenten in een live gegevensstroom. Kanalen beheren programma's. De relatie kanaal en hetzelfde programma is vergelijkbaar met traditionele media waar een kanaal heeft een constante stroom van inhoud en een programma is beperkt tot enkele vervaging gebeurtenis op dat kanaal.
U kunt het aantal uren die u behouden de opgenomen inhoud voor het programma wilt door de eigenschap **ArchiveWindowLength** opgeven. Deze waarde kan worden ingesteld van een minimum van 5 minuten tot maximaal 25 uur. 

ArchiveWindowLength bepaalt ook dat de maximale hoeveelheid tijd clients terug in tijd kunt zoeken vanaf de huidige live positie. Programma's kunnen worden uitgevoerd op de opgegeven tijdsduur, maar de inhoud die achter de lengte van het venster valt continu worden verwijderd. Deze waarde van deze eigenschap wordt ook bepaalt hoe lang de client-manifesten te vergroten.

Elk programma is gekoppeld aan een actief. Als u wilt publiceren van het programma, moet u een locator voor de bijbehorende activa maken. Met deze locator, kunt u de URL van een streaming dat u naar uw klanten kan leveren maken.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve programma's, zodat u kunt meerdere archieven van de dezelfde binnenkomende stream maken. Hiermee kunt u publiceren en archiveren van verschillende gedeelten van een gebeurtenis naar wens. Het vereiste voor uw bedrijf is bijvoorbeeld 6 uur van een programma archiveren, maar alleen laatste 10 minuten uitzenden. Hiervoor moet u twee gelijktijdig actieve programma's te maken. Een programma is ingesteld op het archiveren van 6 uur van de gebeurtenis, maar het programma niet wordt gepubliceerd. De andere programma's is ingesteld op archiveren voor 10 minuten en dit programma is gepubliceerd.


##<a name="billing-implications"></a>Facturering consequenties

Een kanaal wordt gestart zodra er staat overgangen naar 'Voorlopig' via de API facturering.  

De volgende tabel ziet u hoe kanaal Staten toewijzen aan billing Staten in de portal API en Azure. Houd er rekening mee dat de Staten iets anders tussen de API en Portal UX. zijn Als u een kanaal in de stand "Lopende" via de API, of in de stand 'Gereed' of 'Streaming' in de portal van Azure facturering wordt actief.

Als u wilt stoppen met het kanaal vanuit u verder facturering, moet u het kanaal via de API of in de portal van Azure stoppen.
U bent verantwoordelijk voor het stoppen van uw kanalen wanneer u klaar bent met het kanaal. Fout bij stoppen met het kanaal treden voortdurende facturering.

>[AZURE.NOTE]Tijdens het werken met standaard kanalen, wordt AMS automatisch signalen een kanaal dat zich nog in staat "Uitgevoerd" 12 uur na de invoer-feed verbroken is, en er zijn geen programma's worden uitgevoerd. Echter, wordt u nog steeds worden gefactureerd voor de tijd die het kanaal status 'Uitgevoerd heeft'.

###<a id="states"></a>Kanaal Staten en hoe ze naar de facturering modus toewijzen 

De huidige status van een kanaal. Mogelijke waarden zijn:

- **Gestopt**. Dit is de eerste weergave van het kanaal nadat de bijbehorende gemaakt (tenzij starten is geselecteerd in de portal.) Geen facturering wordt weergegeven in deze status. In deze status wordt de eigenschappen van het kanaal kunnen worden bijgewerkt, maar streaming is niet toegestaan.
- **Starten**. Het kanaal wordt gestart. Geen facturering wordt weergegeven in deze status. Geen updates of streaming is toegestaan tijdens deze status. Als er een fout optreedt, het kanaal keert terug naar de stand gestopt.
- **Uitgevoerd**. Het kanaal is kunnen live gegevensstromen verwerken. Dit is nu gebruik facturering. U moet het kanaal om te voorkomen dat verder facturering stoppen. 
- **Stoppen**. Het kanaal wordt gestopt. Geen facturering wordt weergegeven in deze status. Geen updates of streaming is toegestaan tijdens deze status.
- **Verwijderen**. Het kanaal wordt verwijderd. Geen facturering wordt weergegeven in deze status. Geen updates of streaming is toegestaan tijdens deze status.

De volgende tabel ziet u hoe kanaal Staten toewijzen aan de facturering modus. 
 
Kanaal staat|Portal UI-indicatoren|Is het facturering?
---|---|---
Starten|Starten|Geen (status)
Uitgevoerd|Bent u er klaar (geen programma's)<br/>of<br/>Streaming (ten minste één actieve programma)|JA
Stoppen|Stoppen|Geen (status)
Gestopt|Gestopt|Nee


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="related-topics"></a>Verwante onderwerpen

[Azure mediaservices gefragmenteerde MP4 Live nemen specificatie](media-services-fmp4-live-ingest-overview.md)

[Werken met kanalen die voor het uitvoeren van Live coderen met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)

[Werken met kanalen die Live Multi-bitsnelheid van On-premises Encoders ontvangen](media-services-live-streaming-with-onprem-encoders.md)

[Quota en beperkingen](media-services-quotas-and-limitations.md).  

[Media Services-concepten](media-services-concepts.md)
