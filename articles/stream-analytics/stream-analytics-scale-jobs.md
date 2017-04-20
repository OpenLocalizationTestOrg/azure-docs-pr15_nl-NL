<properties
    pageTitle="Stream Analytics taken om uit te breiden doorvoer schaal | Microsoft Azure"
    description="Leer hoe u de taken van de Stream analyses met invoer partities configureren, de querydefinitie optimaliseren en het instellen van de taak streaming eenheden wilt verkleinen."
    keywords="gegevens-streaming, afstemmen streaming gegevensverwerking analytics"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>De schaal van Azure Stream Analytics taken waardoor de stroom gegevensverwerking doorvoercapaciteit aanpassen

Informatie over het afstemmen van analytics-taken en berekenen *streaming eenheden* voor Stream Analytics, hoe u de taken van de Stream analyses met invoer partities configureren, de querydefinitie analytics optimaliseren en configuratie van taak streaming eenheden wilt verkleinen. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Wat zijn de onderdelen van een taak Stream Analytics?
De definitie van een Stream Analytics bevat invoeritems, een query en uitvoer. Ingangen zijn vanaf waar de taak de gegevensstroom leest, de query wordt gebruikt om te transformeren van de invoer gegevensstroom en de uitvoer is waar de taak Hiermee worden de resultaten van de taak wilt.  

Een taak moet ten minste één invoerbron voor data streaming. De gegevens invoerbron voor de stream kan worden opgeslagen in een Hub Azure Service Bus gebeurtenis of in Azure-blobopslag. Zie [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md) en [aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)voor meer informatie.

## <a name="configuring-streaming-units"></a>Streaming eenheden configureren
Streaming eenheden (SUs) voor de resources en een rekenkracht nodig als een taak Azure Stream Analytics wilt uitvoeren. SUs om er zelf een toe om te beschrijven van de relatieve gebeurtenis processing capaciteit op basis van een gemengde maateenheid van de processor, geheugen, en lezen en schrijven tarieven. Elke streaming eenheid overeenkomt met ongeveer 1MB/tweede doorvoer. 

Kiezen hoeveel SUs zijn vereist voor een bepaalde taak is afhankelijk van de partitieconfiguratie voor de invoer en de query die is gedefinieerd voor de taak. U kunt snel aan de quota voor uw in eenheden voor een project met behulp van de Azure klassieke Portal streaming selecteren. Elke Azure abonnement al dan niet standaard heeft een limiet van maximaal 50 streaming eenheden voor alle analytics-taken in een bepaalde regio. Als u wilt vergroten streaming eenheden voor uw abonnementen, contact opnemen met [Microsoft ondersteuning](http://support.microsoft.com).

Het aantal streaming eenheden die van een taak gebruikmaken, is afhankelijk van de partitieconfiguratie voor de invoer en de query die is gedefinieerd voor de taak. Houd er ook rekening mee dat een ongeldige waarde voor de eenheden stream moet worden gebruikt. Geldige waarden beginnen bij 1, 3, 6 en kiest u vervolgens omhoog in stappen 6, zoals hieronder wordt weergegeven.

![Azure Stream Analytics streamen eenheden schaal][img.stream.analytics.streaming.units.scale]

In dit artikel wordt uitgelegd hoe u berekenen en het afstemmen van de query waardoor de doorvoercapaciteit voor analytics-taken.

## <a name="embarrassingly-parallel-job"></a>Taken embarrassingly parallel
De embarrassingly parallelle taak is het meest scalable scenario dat we in Azure Stream Analytics hebben. Deze maakt één partition van de invoer voor één exemplaar van de query verbinding met één partition van de uitvoer. Deze parallellisme bereiken, hebt u een paar dingen nodig:

1.  Als uw query logica afhankelijk van dezelfde sleutel wordt verwerkt door dezelfde query-sessie is, klikt u vervolgens moet u ervoor zorgen dat de gebeurtenissen gaat u naar de dezelfde partition van uw invoer. In het geval van een gebeurtenis Hubs, is dit betekent dat de gegevens van de gebeurtenis moet hebben van **PartitionKey** instellen of kunt u gepartitioneerde afzenders. Dit betekent dat de gebeurtenissen worden verstuurd naar dezelfde map partition voor Blob. Als uw query logica niet dezelfde sleutel worden verwerkt door het exemplaar van dezelfde query hoeft, kunt u deze vereiste negeren. Een voorbeeld van dit is een eenvoudige Selecteer/project/filter-query.  
2.  Nadat de gegevens is ingedeeld alsof deze moeten worden aan de invoer, nodig we om ervoor te zorgen dat uw query is partitioneren. Hiervoor is vereist om **Partition door** in alle stappen. Meerdere stappen zijn toegestaan, maar moeten worden alle partitioneren door de dezelfde sleutel. Een ander wat u moet opmerking is dat op dit moment de partities sleutels moet worden ingesteld op **PartitionId** om een volledig parallelle project hebt.  
3.  Momenteel ondersteund alleen gebeurtenis Hubs en Blob gepartitioneerde uitvoer. Voor de gebeurtenis Hubs uitvoer moet u het veld **PartitionKey** **PartitionId**configureren. Voor Blob hoeft u niet te doen.  
4.  Een ander object onthouden, het aantal invoer partities moet gelijk zijn aan het aantal uitvoer partities. BLOB uitvoer ondersteunt momenteel geen partities, maar dit is geen probleem omdat het partitieschema van de query boven worden overgenomen. Voorbeelden van partition waarden waarmee een volledig parallelle taak:  
    1.  8 gebeurtenis Hubs invoer partities en 8 gebeurtenis Hubs uitvoer partities
    2.  8 gebeurtenis Hubs exporteren partities Blob uitvoer  
    3.  8 blob invoer partities en Blob uitvoer  
    4.  8 blob invoer partities en 8 gebeurtenis Hubs uitvoer partities  

Hier volgen enkele voorbeelden van scenario's die embarrassingly parallelle zijn.

### <a name="simple-query"></a>Eenvoudige query
Invoer-gebeurtenis Hubs met 8 partities uitvoer – gebeurtenis Hub met 8 partities

**Query:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Deze query is een eenvoudig filter en zo we hoeft niet bang te partitioneren van de invoer die we naar gebeurtenis Hubs verzenden. U ziet dat de query **Partition door** van **PartitionId**, heeft zodat we #2 dan hierboven behoefte. Voor de uitvoer moeten we de gebeurtenis Hubs-uitvoer configureren in de taak naar de **PartitionKey** -veld ingesteld op **PartitionId**. Een laatste selectievakje, invoer partities == uitvoer partities. Deze topologie is embarrassingly parallelle.

### <a name="query-with-grouping-key"></a>Query met groepeer-toets
Invoer-gebeurtenis Hubs met 8 partities uitvoer – Blob

**Query:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Deze query heeft een groeperingssleutel en zo dezelfde sleutel moet worden verwerkt door het exemplaar van dezelfde query. Dit betekent moeten we onze gebeurtenissen met Hubs gebeurtenissen in een gepartitioneerde verzenden. Welke toets we belangrijk vindt? **PartitionId** is een taak logica concept, wordt de reële toets die we belangrijk vindt **TollBoothId**. Dit betekent we de **PartitionKey** moet ingesteld van de gebeurtenisgegevens we verzenden naar gebeurtenis Hubs moeten de **TollBoothId** van de gebeurtenis. De query heeft **Partition door** van **PartitionId**, zodat we er goed zijn. Voor de uitvoer, omdat het Blob, hoeft we niet bang te configureren **PartitionKey**. Klik nogmaals voor vereiste #4, is dit Blob, zodat we niet hoeft zorgen hoeft te maken. Deze topologie is embarrassingly parallelle.

### <a name="multi-step-query-with-grouping-key"></a>Meerdere stap Query met groepeer-toets ###
Invoer-gebeurtenis Hub met 8 partities uitvoer – gebeurtenis Hub met 8 partities

**Query:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Deze query heeft een groeperingssleutel en zo dezelfde sleutel moet worden verwerkt door het exemplaar van dezelfde query. We kunnen de dezelfde strategie gebruiken als de vorige query. De query heeft meerdere stappen. Heeft elke stap **Partition door** van **PartitionId**? Ja, zodat we handig zijn. Voor de uitvoer, moeten we de **PartitionKey** ingesteld op **PartitionId** zoals hierboven besproken en we kunt ook zien er hetzelfde aantal partities als invoer. Deze topologie is embarrassingly parallelle.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Voorbeelden van scenario's die niet zijn embarrassingly parallelle

### <a name="mismatched-partition-count"></a>Niet-overeenkomende Partition tellen ###
Invoer-gebeurtenis Hubs met 8 partities uitvoer – gebeurtenis Hub met 32 partities

Het maakt niet uit wat de query is in dit geval omdat de invoer partitioneren tellen! = aantal van de partities uitvoer.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Gebeurtenis Hubs of BLOB's niet gebruiken als uitvoer
Invoer-gebeurtenis Hubs met 8 partities uitvoer – PowerBI

PowerBI-uitvoer ondersteunt momenteel geen partitioneren.

### <a name="multi-step-query-with-different-partition-by-values"></a>Meerdere stap Query met verschillende Partition door waarden
Invoer-gebeurtenis Hub met 8 partities uitvoer – gebeurtenis Hub met 8 partities

**Query:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Zoals u ziet, wordt in de tweede stap **TollBoothId** gebruikt als de partities-toets. Dit is niet hetzelfde als de eerste stap en dus moet u ons een willekeurige volgorde doen. 

Hierna ziet u enkele voorbeelden en counterexamples van Stream Analytics-taken die kunnen bereiken van een embarrassingly parallelle topologie en ermee de mogelijkheden voor maximale schaal. Voor taken die niet een van deze profielen passen, toekomstige updates artikel wordt beschreven hoe bewaartemperatuur schaal enkele andere canonieke Stream Analytics scenario's beschreven komen.

Gebruik van de onderstaande algemene richtlijnen voor nu:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Het maximale aantal eenheden van een taak streaming berekenen
Het totale aantal streaming eenheden die kunnen worden gebruikt door een taak Stream analyses, is afhankelijk van het nummer van de stappen in de query die is gedefinieerd voor de taak en het aantal partities voor elke stap.

### <a name="steps-in-a-query"></a>Stappen in een query
Een query kan een of meer stappen hebben. Elke stap is een onderliggend query die is gedefinieerd door middel van het trefwoord **WITH** . De enige query die zich buiten het trefwoord **WITH** ook telt als een stap, bijvoorbeeld de **SELECT** -instructie in de volgende query:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

De vorige query bevat twee stappen.

> [AZURE.NOTE] Deze voorbeeldquery worden later in dit artikel beschreven.

### <a name="partition-a-step"></a>Een stap partitioneren

Een stap partitioneren, moet de volgende voorwaarden:

- De invoerbron moet partitioneren. Zie de [Gebeurtenis Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md)voor meer informatie.
- De **SELECT** -instructie van de query moet uit een gepartitioneerde invoerbron lezen.
- De query in de stap moet het trefwoord **Partition door** hebben

Wanneer een query is partities, de invoer gebeurtenissen worden verwerkt en samengevoegd in afzonderlijke partitiegroepen en uitvoer van gebeurtenissen worden gegenereerd voor elk van de groepen. Als een aggregatie gecombineerde wenselijk is, moet u een tweede stap voor niet-partities maken om samen te voegen.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>De eenheid die voor een taak streaming max berekenen

Alle stappen van de niet-partities kunnen samen maximaal zes streaming eenheden voor een taak Stream Analytics schalen. Als u wilt toevoegen extra streaming eenheden, een stap moet partitioneren. Elke partition kan zes streaming eenheden hebben.

<table border="1">
<tr><th>Query van een taak</th><th>Max streaming eenheden voor de taak</th></td>

<tr><td>
<ul>
<li>De query bevat één stap.</li>
<li>De stap is niet partitioneren.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>De stream invoergegevens is partitioneren met 3.</li>
<li>De query bevat één stap.</li>
<li>De stap is partitioneren.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>De query bevat twee stappen.</li>
<li>Er is geen van de stappen partitioneren.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>De invoer van de stream gegevens is partitioneren met 3.</li>
<li>De query bevat twee stappen. De invoer stap is partities en de tweede stap is het niet.</li>
<li>De SELECT-instructie leest van de gepartitioneerde invoer.</li>
</ul>
</td>
<td>24 (18 voor gepartitioneerde stappen) + 6 voor niet-partities stappen</td></tr>
</table>

### <a name="examples-of-scale"></a>Voorbeelden van schaal
De volgende query berekent het aantal auto's via een station betaalde met drie tollbooths binnen een venster drie minuten. Deze query kan maximaal zes streaming eenheden wordt aangepast.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Als u wilt meer streaming eenheden gebruiken voor de query, zowel de gegevensstroom invoer en de query moet partitioneren. Gezien het feit dat de gegevens stream partition is ingesteld op 3, kan de volgende gewijzigde query maximaal 18 streaming eenheden wordt aangepast:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Wanneer een query is partitioneren, zijn de invoer gebeurtenissen verwerkt en samengevoegd in afzonderlijke partitiegroepen. Uitvoer gebeurtenissen worden ook gegenereerd voor elk van de groepen. Partitioneren kan sommige onverwachte resultaten opleveren, als het veld **Group by** niet de partitiesleutel in de invoer van de stream gegevens is. Het veld **TollBoothId** in de vorige voorbeeldquery is bijvoorbeeld niet de Partition toets van Input1. De gegevens uit het stencil #1 kunnen worden verspreid in meerdere partities.

Elk van de partities Input1 wordt afzonderlijk worden verwerkt door Stream Analytics en meerdere records van de auto-keer-via-telling voor de dezelfde stencil in hetzelfde venster gewor wordt gemaakt. Als de partitiesleutel invoer kan niet worden gewijzigd, kunt u dit probleem verholpen door toe te voegen een extra stap van de niet-partition, bijvoorbeeld:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Deze query kan worden aangepast tot en met 24 streaming eenheden.

>[AZURE.NOTE] Als u twee streams samenvoegt, moet u ervoor zorgen dat de streams zijn partitioneren door de partitiesleutel van de kolom dat u de joins doen, en er hetzelfde aantal partities in beide stromen.


## <a name="configure-stream-analytics-job-partition"></a>Stream Analytics taak configureren

**Een streaming eenheid voor een taak aanpassen**

1. Meld u aan bij de [beheerportal](https://manage.windowsazure.com).
2. Klik op **Stream Analytics** in het linkerdeelvenster.
3. Klik op de Stream Analytics-taak die u wilt aanpassen.
4. Klik op **schaal** boven aan de pagina.

![Azure Stream Analytics streamen eenheden schaal][img.stream.analytics.streaming.units.scale]

Klik in de Portal Azure schaalinstellingen toegankelijk onder instellingen:

![Configuratie van de taak Azure Portal Stream Analytics][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Prestaties van de taak controleren

De beheerportal gebruikt, kunt u bijhouden de doorvoer van een taak in gebeurtenissen/tweede:

![Azure Stream Analytics monitor taken][img.stream.analytics.monitor.job]

Bereken de verwachte doorvoer van de werklast in gebeurtenissen/tweede. Als de doorvoer kleiner is dan verwacht, afstemmen van de invoer partition, afstemmen van de query en deze aanvullende streaming eenheden toevoegen aan uw project.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Stream Analytics doorvoer bij schaal - frambozen Pi scenario


Als u wilt weten hoe de stream analytics taken wordt schaal in een typisch voorbeeld met verwerking doorvoer over meerdere Streaming, als volgt een experiment sensorgegevens (clients) stuurt op gebeurtenis-Hub, verwerkt en wordt verzonden, waarschuwing of statistieken als een uitvoer op een andere gebeurtenis Hub.

De client kunstmatige sensorgegevens verstuurt met Hubs van de gebeurtenis in de indeling van JSON naar Stream Analytics en de gegevensuitvoer is ook in de indeling van JSON.  Hier ziet u hoe de voorbeeldgegevens eruitzien vind ik leuk:  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Query: "een melding krijgt wanneer het licht is uitgeschakeld"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Meten doorvoer: Doorvoersnelheid in deze context wordt de hoeveelheid invoergegevens verwerkt door Stream Analytics in een vaste hoeveelheid tijd (10 minuten). Als u wilt bereiken aanbevolen verwerking doorvoer voor de invoergegevens, zowel de gegevensstroom invoer en de query moet partitioneren. Ook is **COUNT()**opgenomen in de query meten hoeveel invoer gebeurtenissen zijn verwerkt. Om ervoor te zorgen voor dat de taak is niet gewoon wachten invoer gebeurtenissen om uit te komen, is elke partition van de invoer gebeurtenis Hub vooraf geïnstalleerd met voldoende invoergegevens (ongeveer 300MB).  

Hieronder vindt u de resultaten met groter wordende aantal Streaming eenheden en bijbehorende Partition worden geteld in gebeurtenis Hubs.  

<table border="1">
<tr><th>Invoer partities</th><th>Uitvoer partities</th><th>Streaming eenheden</th><th>Continue doorvoer
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>U kunt hulp krijgen
Probeer onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor meer hulp.


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
