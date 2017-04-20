<properties
   pageTitle="Defragmentatie van de doelstellingen in Azure-Service stof | Microsoft Azure"
   description="Een overzicht van defragmentatie gebruiken of als een strategie doelstellingen in Service stof verpakken"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentatie van de doelstellingen en laden in Service stof
De Service stof Cluster resourcemanager heeft voornamelijk betrekking op verdelen met het selectievakje laden distribueren – ervoor te zorgen dat alle knooppunten in het cluster gelijkmatig worden gebruikt. Dit is meestal de beste en slimste indeling met die fouten leven blijven, aangezien dit ervoor zorgt dat elk opgegeven mislukt niet uit het sommige grote percentage van een bepaalde werkbelasting kost. De Service stof Cluster Resource Manager ondersteunt, ook een andere strategie welke defragmentatie is. Defragmentatie betekent doorgaans dat in plaats van het gebruik van een meting verdelen over het cluster opzetten, we daadwerkelijk proberen moet om samen te voegen deze. Dit is een fortunate voor vertaling knopinfo van onze normale strategie – in plaats van het optimaliseren van het cluster op basis van de gemiddelde standaarddeviatie van metrische laden voor een bepaalde waarde minimaliseren, we beginnen met het optimaliseren voor toename van de standaarddeviatie. Maar waarom zou u wilt dat deze strategie?

Ook als u hebt het selectievakje laden gelijkmatig verdeeld tussen de knooppunten in het cluster hebt vervolgens u geschikt is wat de resources die de knooppunten hoeft te bieden. Normaal gesproken dit geen probleem, maar soms de services die zijn bijzonder grote en de meeste knooppunt gebruiken voor het maken van sommige werkbelasting: zeg 75% tot 95% van het knooppunt resources uiteindelijk een speciale aan een exemplaar van de service voor eenmalige of replica. Dit geen probleem, wordt de resourcemanager Cluster detecteren bij Aanmaaktijd van een service die nodig zijn voor het cluster om te kunnen ruimte maken voor dit grote werkbelasting en stel over zaken reorganiseren, maar in de tussentijd die werkbelasting er moet wachten in de cluster moeten worden gepland.

Gezien het feit dat de planning van nieuwe werkbelasting meestal ten minste een beetje latentie gevoelige, is als we iets anders niet doen kunnen we soms bLaag rechts door deze serviceovereenkomsten als er een groot aantal services en staat bewegen, vooral als werkbelasting in het cluster meestal grote zijn (en dus het duurt langer om te navigeren in het cluster). Daadwerkelijk, we maken tijden in simulaties op basis van reële clustergegevens gemeten, hebt gezien die als services groot genoeg zijn en het cluster is vrij gebruikt bij het maken van deze grote services zou worden vertraagd en dat we dit kan verbeteren door de introductie van het beleid van defragmentatie aan de doelstellingen.

Net als bestand kan maken of toegang krijgen vertraagd dat als een schijf is gefragmenteerd en kan worden sped omhoog door het station defragmenteren zodat grotere aaneengesloten blokken er beschikbaar zijn, kunt u de doelstellingen defragmentatie als u wilt dat de resourcemanager Cluster proactief te proberen om te verkleinen van de werklast van de services in minder knooppunten, zodat er (bijna) altijd ruimte voor even grote services is configureren , zodat ze snel worden gemaakt. De meeste mensen dit niet nodig omdat services meestal moet klein en dus niet moeilijk te vinden van de ruimte voor deze, maar als u grote services hebben en moet ze snel gemaakt (en willen accepteren van de andere compromissen nodig zijn zoals verbeterde impactfulness van fouten en enkele bronnen blijft unutilized wachten op de werkbelasting moet worden gepland) en vervolgens de defragmentatie strategie voor u is.

Het onderstaande diagram biedt een visuele weergave van twee verschillende clusters, een die is gedefragmenteerd en een die niet. In het geval is gebalanceerde, kunt u de bewegingen die ter plaatse een van de grootste-objecten, als een nieuw worden gemaakt, vergeleken met het gedefragmenteerde cluster, waar direct op knooppunten 4 of 5 kan worden geplaatst.

![Verdeeld en gedefragmenteerd Clusters met elkaar vergelijken][Image1]

## <a name="defragmentation-pros-and-cons"></a>Defragmentatie voor- en nadelen
Wat zijn zodat deze andere conceptuele compromissen nodig zijn? U wordt aangeraden uitgebreide afmetingen van uw werkbelasting vóór het inschakelen van defragmentatie aan de doelstellingen. Hier volgt een snelle tabel dingen om na te denken over:

| Defragmentatie-professionals  | Defragmentatie nadelen |
|----------------------|----------------------|
|Sneller maken van grote services | Concentraten op minder knooppunten, conflict met groter wordende laden
|Hiermee Verlaag verplaatsing van gegevens tijdens het maken van    | Fouten kunnen van invloed zijn op meer services en meer lospeuteren veroorzaken
|Kunt u uitgebreide beschrijving van de vereisten en ruimte terug | Complexere configuratie van algemene resourcebeheer

U kunt aan de doelstellingen gedefragmenteerde en normale in hetzelfde cluster combineren en de resourcemanager doet de aanbevolen om ervoor te zorgen dat u een indeling die worden samengevoegd zo veel mogelijk van de doelstellingen van de defragmentatie terwijl deze kunt terwijl de rest verdeeld wilt ophalen. De exacte resultaten u krijgt afhankelijk van het nummer van het verdelen van de doelstellingen vergeleken met het aantal defragmentatie aan de doelstellingen, het gewicht en hun huidige laadtijd, enzovoort.

## <a name="configuring-defragmentation-metrics"></a>Aan de doelstellingen defragmentatie configureren
Aan de doelstellingen defragmentatie configureren is een algemene beslissing in het cluster en afzonderlijke aan de doelstellingen voor defragmentatie kan worden geselecteerd:

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## <a name="next-steps"></a>Volgende stappen
- De Manager van de Resource Cluster heeft een groot aantal opties voor de beschrijving van het cluster. Als u wilt weten meer informatie over deze in dit artikel bij het [beschrijven van een Service stof cluster](service-fabric-cluster-resource-manager-cluster-description.md) hebt uitchecken
- Aan de doelstellingen zijn hoe de-stof Cluster Resource servicebeheer verbruik en capaciteit in het cluster beheert. Meer informatie over deze en hoe u deze configureert [in dit artikel](service-fabric-cluster-resource-manager-metrics.md) hebt uitchecken

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
