<properties
    pageTitle="Schaalbaarheid van Media Processing overzicht | Microsoft Azure"
    description="In dit onderwerp is bedoeld voor een overzicht van de schaal Media verwerken met Azure Media Services."
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
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Schaalbaarheid van Media Processing-overzicht

Deze pagina kunt een overzicht van hoe en waarom u de schaal van media verwerking aanpassen. 

## <a name="overview"></a>Overzicht

Een Media Services-account is gekoppeld aan een gereserveerde eenheid het Type, waarmee wordt bepaald van de snelheid waarmee uw media verwerking van taken worden verwerkt. U kunt kiezen uit de volgende gereserveerde eenheidstypen: **S1**, **S2**of **S3**. Bijvoorbeeld dat dezelfde codering taak sneller wordt uitgevoerd wanneer u de **S2** gereserveerd eenheid type vergelijken met het type **S1** gebruikt. Zie voor meer informatie de [Gereserveerde eenheidstypen](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Naast het opgeven van het eenheidstype gereserveerde, kunt u opgeven inrichten van uw account met gereserveerde eenheden. Het aantal ingerichte gereserveerde eenheden bepaalt het aantal media-taken dat gelijktijdig kan worden verwerkt in een bepaalde account. Bijvoorbeeld: als uw account vijf gereserveerde eenheden, bevat en vervolgens vijf media taken gelijktijdig zo lang uitvoert als er taken moeten worden verwerkt. De resterende taken wacht in de wachtrij en wordt worden opgenomen voor het verwerken van opeenvolging wanneer een actieve taak is voltooid. Als u een account heeft geen alle gereserveerde eenheden deze is ingericht, wordt klikt u vervolgens taken verwerkt opeenvolging. In dit geval afhankelijk de wachttijd tussen één taak voltooien en de volgende één begin van de beschikbaarheid van resources in het systeem.

## <a name="choosing-between-different-reserved-unit-types"></a>Kiezen tussen verschillende gereserveerde eenheidstypen

De volgende tabel kunt u besluit bij het kiezen tussen verschillende codering snelheden. Ook vindt u enkele vergelijkende gevallen en biedt SA's-URL's die u gebruiken kunt om te downloaden van video's waarop u uw eigen tests kunt uitvoeren:

Scenario 's|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Bedoeld hoofdletters/kleine letters| Eén Codering bitsnelheid. <br/>Bestanden op SD of onder oplossingen, afstemmen niet gevoelige, lage kosten.|Eén bitsnelheid en codering van meerdere bitsnelheid.<br/>Normale gebruik voor zowel SD en HD-codering. |Eén bitsnelheid en codering van meerdere bitsnelheid.<br/>Volledige HD en 4K resolutie video's. Tijd gevoelige, sneller levertijd codering. 
Vergelijkende|[Invoer-bestand: 5 minuten lang 640x360p bij 29,97 frames/tweede](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Codering naar een enkel bitsnelheid MP4-bestand, met de resolutie, duurt ongeveer 11 minuten.|[Invoer-bestand: 5 minuten lang 1280x720p bij 29,97 frames per seconde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Coderen met "H264 één bitsnelheid 720p" vooraf duurt ongeveer 5 minuten.<br/><br/>Coderen met ' H264 meerdere bitsnelheid 720p "vooraf ingestelde duurt ongeveer 11.5 minuten.|[Invoer-bestand: 5 minuten lang 1920x1080p bij 29,97 frames/tweede](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Coderen met "H264 één bitsnelheid 1080p" vooraf duurt ongeveer 2,7 minuten.<br/><br/>Coderen met ' H264 meerdere bitsnelheid 1080p "vooraf ingestelde duurt ongeveer 5,7 minuten.

##<a name="considerations"></a>Overwegingen

>[AZURE.IMPORTANT] Bekijk overwegingen die in deze sectie.  

- Gereserveerde eenheden werken voor alle media-verwerking, met inbegrip van taken met behulp van Azure Media indexering indexeren parallelizing.  Echter, in tegenstelling tot codering indexing doen niet verwerkt sneller met sneller gereserveerde eenheden.

- Als de gedeelde toepassingen gebruikt, dat wil zeggen, zonder een gereserveerde eenheden, klikt u vervolgens uw coderen taken hebben dezelfde prestaties net als met S1 RUs. Er is echter geen bovengrens voor de tijd die uw taken besteden kunnen in in de wachtrij staat en op elk gewenst, één taak is worden uitgevoerd.

- De volgende datacenters bieden geen het type van de eenheid **S2** gereserveerd: Brazilië Zuid, India West Centraal India en India Zuid.

- De volgende datacenters bieden geen het type van de eenheid **S3** gereserveerd: Brazilië Zuid, India West, India centraal.

- De hoogste aantal eenheden die zijn opgegeven voor de periode van 24 uur wordt gebruikt in de berekening van de kosten.


##<a name="quotas-and-limitations"></a>Quota en beperkingen

Zie [quota en beperkingen](media-services-quotas-and-limitations.md)voor informatie over quota en beperkingen en hoe u een ondersteuningsticket openen.

##<a name="next-step"></a>Volgende stap

De schaal taak voor de verwerking van media met een van deze technologieën bereiken: 

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portal](media-services-portal-scale-media-processing.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
