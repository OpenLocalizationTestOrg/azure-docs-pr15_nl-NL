<properties
    pageTitle="Azure Media Services Analytics overzicht | Microsoft Azure"
    description="Azure Media Services biedt de openbare preview-versie van Azure Media Analytics, een verzameling spraak en computer vision-services op enterprise schaal, naleving, beveiliging en wereldwijde bereik. Azure Media Analytics-services zijn gemaakt met de basisonderdelen van de Azure Media Services-platform en dus bent gereed om het verwerken van media processing bij het op schaal op dag. "
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/24/2016"   
    ms.author="milanga;juliako;johndeu"/>

# <a name="azure-media-services-analytics-overview"></a>Azure Media Services Analytics-overzicht

##<a name="overview"></a>Overzicht

Meer organisaties en ondernemingen zijn omvat onderdelen die video als het gewenste medium trainen hun werknemers, klanten en document zakelijke functies deelnemen. Cloud computing maakt het effectieve opslaan, streamen en toegang tot deze grote mediabestanden, maar naarmate bedrijven hun video Inhoudsbibliotheek groeien, moet hebben een even efficiënt middel voor uitgepakt nieuwe inzichten te verkrijgen van een video om te maken logischere, interacties met hun doelgroepen hebt aangepast en hun bedrijf naar het volgende niveau kunt tillen.

Als u wilt deze groeiende behoefte in de marketplace adres, biedt Azure Media Services Media Analytics, een verzameling spraak en visuele onderdelen (aan de schaal van de onderneming, naleving, beveiliging en wereldwijde bereik) die het gemakkelijker voor bedrijven en ondernemingen om te leiden sneller inzicht krijgen in hun videobestanden. Azure Media Analytics-services zijn gemaakt met de basisonderdelen van de Azure Media Services-platform en dus bent gereed om het verwerken van media processing bij het op schaal op dag.

Azure Media Analytics kunnen ontwikkelaars snel aan de slag met visuele mogelijkheden voor video op beperkte schaal en deze geavanceerde functionaliteit in toepassingen weer. Azure Media Analytics is ontworpen om te worden gebruikt door de enterprise-omgevingen met de volledige schaal, naleving, beveiliging en wereldwijde bereik vereist door grote organisaties.

In het volgende diagram ziet u **Media analyses** en andere belangrijkste onderdelen van de Media Services-platform. 

![VoD werkstroom](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

Media-processors Media analyses vlees MP4-bestanden of JSON-bestanden. Als een Mediaprocessor geproduceerd een MP4-bestand, kunt u het bestand geleidelijk downloaden. Als een Mediaprocessor geproduceerd een JSON-bestand, kunt u het bestand downloaden van de Azure-blobopslag. 

## <a name="azure-media-analytics-services"></a>Azure Media Analytics-services

- **Indexering** – Azure Media indexering kunt u dat inhoud doorzoekbaar, alsmede gesloten closed captioning nummers genereren. Azure Media Services uitgebracht **Azure Media indexering 2 Preview** met sneller indexing en bredere taal support. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees, Portugees en Arabisch. Zie voor gedetailleerde informatie en voorbeelden [proces video's met Azure Media indexering 2](media-services-process-content-with-indexer2.md)
 
- **Hyperlapse** – Microsoft Hyperlapse is het resultaat van meer dan twintig jaar computer vision onderzoek op MSR (Microsoft Research), waarbij de video stabilisatie en de tijd vervallen als u wilt snel, verbruikbare, mooie video's uit uw lang formulier inhoud maken. Naast het tijd verstrijkt maken, kunt u ook Hyperlapse gebruiken om te maken van stabiele video's van beelden video's die zijn geregistreerd via mobiele telefoons en camcorders. Zie [Hyperlapse Media-bestanden met Azure Media Hyperlapse](media-services-hyperlapse-content.md) voor gedetailleerde informatie en voorbeelden
 
- **Beweging detectie** – kunt u deze service beweging in een video met e-mailpapier achtergronden detecteren. Dit is ideaal voor klanten die u wilt controleren op onrechte op beweging gebeurtenissen gedetecteerd door toezicht camera's op de video toezicht-feeds. Zie voor gedetailleerde informatie en voorbeelden [Beweging detectie van Azure Media analysegegevens](media-services-motion-detection.md).
 
- **Nominale detectie en nominale emoties** – met deze service, kunt u iemands vlakken en hun emoties, inclusief gelukkig, sadness, verrassing, volgt uitzien, contempt, bang, beantwoord en indifference/neutraal detecteren. Dit heeft verschillende handige industriële toepassingen, Zie hieronder, waaronder aggregeren en reacties van personen die een gebeurtenis bijwoont analyseren. Zie [nominale en emoties detectie van Azure Media analysegegevens](media-services-face-and-emotion-detection.md)voor gedetailleerde informatie en voorbeelden.
 
- **Samenvatting van de video** : Video samenvatting kunt u samenvattingen van lange video's door te selecteren automatisch interessante fragmenten uit de bronvideo maken. Dit is handig als u voorzien van een kort overzicht van wat u wilt kunt verwachten in een lange video. Zie voor gedetailleerde informatie en voorbeelden [Azure Media Video miniaturen gebruiken om te maken van een Video-samenvatting](media-services-video-summarization.md)

- **OCR** - Azure Media Analytics OCR (optical character recognition) kunt u tekstinhoud in videobestanden converteren naar bewerkbare, doorzoekbare digitale tekst. Hiermee kunt u de extractie van duidelijke metagegevens van de video signaal van uw media automatiseren.
 
- **Scalable nominale redactie** - **Azure Media Redactor** is een Azure Media Analytics MP die scalable nominale redactie in de cloud biedt. Nominale redactie kunt u uw video om te kunnen vervagen vlakken van geselecteerde personen wijzigen. U wilt de service van de redactie nominale op openbare veiligheid en nieuws media-scenario's gebruiken. Kunnen een paar minuten van opnamen met meerdere vlakken uur handmatig Redigeren duren, maar met deze service vereist het nominale redactie proces slechts een paar eenvoudige stappen. Zie [Dit](media-services-face-redaction.md) artikel voor meer informatie.

 
## <a name="common-scenarios"></a>Gebruikelijke scenario 's

Hierna volgt een aantal scenario's waarin Azure Media Analytics kunt organisaties en ondernemingen over bedrijven vindt nieuwe inzichten te verkrijgen van een video om meer gepersonaliseerde publiek en werknemer afspraken te maken, evenals grote hoeveelheid videomateriaal efficiënter te beheren:

- **Gesprek gecentreerd** – Even met de instelling van sociale media, klant-oproep centers vergemakkelijken nog steeds een grote percentage van de service-klanttransacties. In deze audio gegevens worden gecodeerd is een enorme hoeveelheid informatie over klanten die kunnen worden geanalyseerd om producten te verbeteren en ook trainen gesprek center werknemers over naar hogere klanttevredenheid bereiken. Via Azure Media indexering kunnen klanten om tekst en een zoekindex en dashboards worden geëxtraheerd intelligence rond de meest voorkomende klacht, bron van klacht opbouwen en andere dergelijke relevante gegevens te halen.

- **Goedkeuring van inhoud door de gebruiker worden gegenereerd** – uit nieuws media wandcontactdozen naar gerechtelijke afdelingen, veel organisaties hebben openbare omlaag portals waar ze UGC media, zoals video's en afbeeldingen accepteren. Het volume van inhoud kunt oploopt vanwege onverwachte gebeurtenissen. In deze situaties is het niet mogelijk om te leiden van een effectieve handmatig controleren van de inhoud op geschiktheid nabij. Klanten vertrouwen op de service goedkeuring van inhoud kunt richten op de inhoud die nodig is.

- **Toezicht** - met de groei van IP-camera's en wordt er een explosie van toezicht video's in. Handmatig controleren toezicht video is tijd intensief en vatbaar voor menselijke fouten. Azure Media Analytics vindt u verschillende onderdelen zoals beweging detectie, nominale detectie en Hyperlapse zodat het proces van het reviseren van, beheren en afgeleide eenvoudiger te maken.

## <a name="media-services-analytics-media-processors"></a>Media Services Analytics Media-Processors 

In deze sectie worden alle de Media Services Analytics Media Processors (MP) en ziet u hoe .NET of REST voor uw gebruiken een MP-object.

### <a name="mp-names"></a>MP namen


- Azure Media indexering 2 Preview
- Azure Media indexering
- Azure Media Hyperlapse
- Azure Media nominale detectie
- Azure Media beweging detectie
- Azure Media Video miniaturen
- Azure Media OCR

### <a name="net"></a>.NET

De volgende functie heeft een van de opgegeven MP namen en een object MP terug te keren.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


## <a name="rest"></a>REST

Vraag:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net
    
Antwoord:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

##<a name="demos"></a>Demo 's

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-articles"></a>Verwante artikelen

[Aankondiging van Media Services Analytics](https://azure.microsoft.com/blog/introducing-azure-media-analytics/)
  

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
