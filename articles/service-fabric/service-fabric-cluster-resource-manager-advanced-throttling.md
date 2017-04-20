<properties
   pageTitle="Beperken in de Service stof cluster resourcemanager | Microsoft Azure"
   description="Leer hoe u de opgegeven door de Service Cluster Resource Manager stof throttles configureren."
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


# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>Het gedrag van de Service Cluster Resource Manager stof beperken
Zelfs als u de resourcemanager Cluster correct hebt geconfigureerd, kan het cluster kunt ophalen onderbroken. Bijvoorbeeld kan er tegelijk knooppunt foutenstructuuranalyse domein fouten in- of - wat gebeurt er als die zijn opgetreden tijdens een upgrade? De resourcemanager optimaal probeert om op te lossen alles, maar in tijden strekking u misschien wilt u een backstop zodat het cluster zelf een kans gestabiliseerd (de knooppunten waarbij gaat om uit te komen terug doen, de netwerkcondities Retoucheren zelf, gecorrigeerde bits geïmplementeerd). Om u te helpen met dit soort situaties, bevat het servicebeheer stof Cluster Resource verschillende throttles. Opmerking dat deze throttles vrij storend en over het algemeen niet mag worden gebruikt, tenzij er sprake is van bepaalde pas op dat u gereed rond de hoeveelheid werk die parallelle die daadwerkelijk kan worden uitgevoerd in het cluster, evenals een veelgebruikte wiskundige moet reageren op deze worden gesorteerd van (ahem) ongeplande macroscopische nieuwe configuratie gebeurtenissen (BTW: "Zeer ongeldige dagen").

In het algemeen wordt aangeraden voorkomen van zeer ongeldige dagen door andere opties (zoals normale code updates en voorkomen het cluster allereerst overscheduling) in plaats van uw cluster om te voorkomen dat het gebruik van resources wanneer deze probeert om op te lossen zelf beperken). De throttles heb standaardwaarden dat we tot en met ervaring moeten ok standaardwaarden hebt gevonden, maar u moet waarschijnlijk bekijken en deze af te stellen aan uw verwachte werkelijke laden. Terwijl u niet te bepalen of het cluster een goede gewoonte die u bepalen is mogelijk dat er wordt geladen waarin gevallen (totdat u ze verhelpen kunt) is waar moet u beschikken over een aantal throttles op hun plaats staan, zelfs als betekent dit het cluster duurt langer stabiliseren.

##<a name="configuring-the-throttles"></a>De throttles configureren
De throttles die standaard zijn opgenomen zijn:

-   GlobalMovementThrottleThreshold – Hiermee bepaalt u het totale aantal verplaatsingen in het cluster via enige tijd (gedefinieerd als de GlobalMovementThrottleCountingInterval, waarde in seconden)
-   MovementPerPartitionThrottleThreshold – Hiermee bepaalt u het totale aantal verplaatsingen voor elke service partition via enige tijd (de MovementPerPartitionThrottleCountingInterval, waarde in seconden)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Let erop dat die we hebt gezien klanten gebruiken deze throttles minimaal is meestal omdat deze al in een omgeving resource beperkt zijn (zoals beperkte netwerkbandbreedte in afzonderlijke knooppunten of schijven die niet snel aan de vereisten van parallelle replica zijn genereert die in deze zijn wordt gebracht) waarin deze bewerkingen bedoelde slagen wouldn't of zou traag toch.  In deze situaties zijn klanten vertrouwd weten dat ze zijn mogelijk de hoeveelheid tijd die nodig zou hebben om het cluster bereiken van een stabiele staat, inclusief weten dat ze kunnen dit uiteindelijk uitgevoerd op de onderste algemene betrouwbaarheid terwijl ze zijn vertraagd uitbreiden.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over hoe de resourcemanager Cluster beheert en saldi laden in het cluster, raadpleegt u het artikel over [het verdelen van laden](service-fabric-cluster-resource-manager-balancing.md)
- De Manager van de Resource Cluster heeft een groot aantal opties voor de beschrijving van het cluster. Als u wilt weten meer informatie over deze in dit artikel bij het [beschrijven van een Service stof cluster](service-fabric-cluster-resource-manager-cluster-description.md) hebt uitchecken
