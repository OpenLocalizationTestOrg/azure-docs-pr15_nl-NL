<properties
   pageTitle="Het verdelen van uw Cluster met Azure-stof Cluster Resource servicebeheer | Microsoft Azure"
   description="Een inleiding tot het verdelen van uw cluster met de Service Cluster Resource Manager stof."
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

# <a name="balancing-your-service-fabric-cluster"></a>Het verdelen van uw service stof cluster
De Service stof Cluster Resource Manager kunt rapportage dynamische laden, reageren op wijzigingen in het cluster corrigeren van de nieuwe beperking en het cluster opnieuw indien nodig. Hoe vaak doet het hier rekening maar wat activeert? Er zijn verschillende besturingselementen die zijn gerelateerd aan dit.

De eerste reeks besturingselementen rond het verdelen van een reeks timers zijn. Deze timers bepalen hoe vaak de resourcemanager Cluster worden onderzocht, de status van de cluster voor zaken aan bod die moeten worden toegewezen. Er zijn drie verschillende categorieën van werk, elk met hun eigen corresponderende timer. Ze zijn:

1.  Plaatsing – deze fase behandelt plaatsen een statuscontrole replica's of stateless exemplaren die ontbreken. Dit heeft betrekking op nieuwe services en de verwerking van statuscontrole replica's of stateless exemplaren die zijn mislukt en moeten opnieuw worden ingesteld. Verwijderen en daar neerzetten replica's of exemplaren wordt ook hier verwerkt.
2.  Beperking controleert – deze fase Hiermee wordt gecontroleerd op en wordt gecorrigeerd schending van de beperkingen van de verschillende plaatsing (regels) binnen het systeem. Voorbeelden van regels zijn items, zoals ervoor te zorgen dat knooppunten niet over capaciteit zijn en dat de limieten valt plaatsing van een service (meer informatie over deze later) wordt voldaan.
3.  Het verdelen van – deze fase Hiermee wordt gecontroleerd om te zien als proactief opnieuw nodig is gebaseerd op het gewenste geconfigureerde niveau saldo verschillende doelstellingen en als dat het geval gezocht naar een rangschikking in het cluster die meer wordt verdeeld.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Resourcemanager Cluster stappen en Timers configureren
Elk van deze verschillende soorten correcties de resourcemanager Cluster kunt aanbrengen wordt bepaald door een andere timer waaraan u de frequentie. Als u alleen omgaan wilt met nieuwe service werkbelasting plaatsen in het cluster elk uur (naar batch ze omhoog), maar de gewenste normale verdeling om de paar seconden wordt gecontroleerd, kunt u bijvoorbeeld zo dat gedrag configureren. Wanneer elke timer wordt gestart, wordt de taak is gepland. Al dan niet standaard de resourcemanager de staat scant en updates (batchen alle wijzigingen die zijn opgetreden sinds de laatste scan, zoals daarvan die een knooppunt niet beschikbaar is) geldt elke 1/10e van een tweede, sets de plaatsing en de beperking selectievakje vlaggen elke tweede en het verdelen van elke vijf seconden markeren.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Vandaag we alleen voert u een van deze acties per keer, opeenvolging (daarom we verwijzen naar deze configuraties als "minimale intervallen")). Dit is zodat, bijvoorbeeld we al hebt gereageerd op een in behandeling aanvragen voor het maken van nieuwe replica's voordat we doorgaan naar het verdelen van het cluster. Zoals u door de standaard-tijdsintervallen die is opgegeven ziet, we kunt scannen en controleren op we zeer vaak uitvoert moeten, wat betekent dat de set wijzigingen we aan het einde van elke stap geven is meestal kleiner: we scant niet via de uren van wijzigingen in het cluster en probeert deze allemaal tegelijk worden gecorrigeerd, we probeert te verwerken dingen meer of minder terwijl deze worden aangebracht, maar met enkele batchen wanneer veel zaken optreden bij het tegelijk. Dit zorgt ervoor dat de Service stof resource manager zeer heeft gereageerd naar onderdelen die plaatsvinden in het cluster.

Tijdens het grootste deel van de volgende taken zijn ingewikkelde (als er nieuwe beperking, fix, als er services zijn wilt maken, maakt deze), de resourcemanager Cluster moet ook enkele aanvullende informatie om te bepalen of het imbalanced cluster. Voor die zijn er twee andere stukjes configuratie: *Drempelwaarden verdelen* en de *Activiteit drempels*.

## <a name="balancing-thresholds"></a>Het verdelen van drempelwaarden
Een drempelwaarde voor het verdelen van het besturingselement main proactief opnieuw activeert is (Vergeet niet dat de timer is alleen de resourcemanager Cluster voor hoe vaak moet controleren - dit betekent niet dat er gebeurt). De drempelwaarde voor het verdelen van Hiermee definieert u hoe imbalanced het cluster moet kunnen worden voor een bepaalde waarde in voor de Cluster Resource Manager u rekening moet houden dit imbalanced en het verdelen van trigger volgorde.

Taakverdeling drempelwaarden zijn gedefinieerd op basis van per metrisch als onderdeel van de definitie cluster. Voor meer informatie over de doelstellingen uitchecken [in dit artikel](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

De drempelwaarde voor het verdelen van voor een meting geeft de verhouding. Als de hoeveelheid laden op het meest geladen knooppunt gedeeld door de hoeveelheid laden op de minste geladen knooppunt dit nummer overschrijdt, en vervolgens het cluster wordt beschouwd als imbalanced en het verdelen wordt geactiveerd de volgende keer dat de resourcemanager Cluster Hiermee wordt gecontroleerd (standaard ooit 5 seconden, als geregeld door de MinLoadBalancingInterval, hierboven).

![Voorbeeld van de drempelwaarde voor het verdelen][Image1]

In dit voorbeeld eenvoudige verbruikt elke service één eenheid van enkele metrisch. In het bovenste voorbeeld wordt de maximumbelasting op een knooppunt 5 is en de minimumwaarde is 2. Stel dat de taakverdeling drempel voor deze metrisch 3 is. Daarom in het bovenste voorbeeld wordt het cluster wordt beschouwd als verdeeld en geen verdelen wordt geactiveerd wanneer de resourcemanager Cluster Hiermee wordt gecontroleerd (omdat de verhouding tussen in het cluster 5/2 = 2,5 en dat wil zeggen kleiner dan de drempelwaarde voor opgegeven taakverdeling van 3).

In het voorbeeld onder de maximale belasting van een knooppunt is 10, terwijl de minimumwaarde 2 is, wat resulteert in een verhouding van 5. Hiermee wordt het cluster de ontworpen taakverdeling overschrijdt van 3 voor die metrisch geplaatst. Een globale rebalancing klaar zijn daardoor geplande zodra de taakverdeling timer wordt uitgevoerd. Opmerking die het simpele feit dat een taakverdeling zoekopdracht wordt gestart nergens verplaatsing - soms komt het cluster imbalanced maar de situatie niet kan worden verbeterd - maar in een situatie als dit item (ten minste al dan niet standaard) enkele het selectievakje laden wordt vrijwel zeker gedistribueerd Knooppunt3. Houd er rekening mee dat omdat we niet werkt met een hebzuchtig aanpak sommige laden kan ook worden verdeeld over Knooppunt2 aangezien die het resultaat in minimalisering van de algehele verschillen tussen knooppunten, maar we verwachten zou dat het grootste deel van het selectievakje laden naar Knooppunt3 zou flow.

![Het verdelen van drempel voorbeeld acties][Image2]

Let erop dat aan onder de drempel van taakverdeling is niet een expliciete doel – het verdelen van drempelwaarden zijn alleen een *trigger* waarin staat dat de Cluster resourcemanager die dit in het cluster om te bepalen welke verbeteringen dit kunt u ziet, indien van toepassing.

## <a name="activity-thresholds"></a>Activiteit drempelwaarden
Soms is *het totaalbedrag van laden in het cluster* lage Hoewel knooppunten relatief imbalanced. Dit kan alleen vanwege de tijd van de dag, of omdat het cluster nieuw en alleen ophalen is bootstrap uitvoert. U kunt in beide gevallen niet het cluster worden verdeeld, omdat er heel daadwerkelijk weinig is te, maar u kunt alleen worden besteden netwerk- en berekeningscluster resources als u wilt verplaatsen van zaken rond, zonder dat een absolute verschil tijd. Omdat we dit voorkomen wilt, er is een ander besturingselement binnen de resourcemanager, bekend als activiteit drempelwaarden, waarmee u sommige absolute ondergrens voor activiteit – opgeven als er geen knooppunt ten minste dit heeft veel laden en vervolgens het verdelen wordt niet geactiveerd zelfs als de drempelwaarde voor het verdelen van is voldaan.

Stel de dat we rapporten met de volgende totalen voor verbruik op deze knooppunten hebben als voorbeeld. Stel dat we onze drempelwaarde voor het verdelen van 3 voor deze metrisch behouden, maar we ook de drempelwaarde voor een activiteit van 1536 hebben ook in. In het eerste geval terwijl het cluster imbalanced per de drempelwaarde voor het verdelen van is geen knooppunt voldoet aan deze minimale activiteit drempel, zodat we dingen laten. In het voorbeeld onder is knooppunt1 manier de activiteit overschrijdt, zodat het verdelen wordt uitgevoerd (sinds de drempelwaarde voor het verdelen van zowel de activiteit drempel voor de meetwaarde worden overschreden)

![Voorbeeld van de activiteit drempelwaarde][Image3]

Net als het verdelen van drempelwaarden zijn activiteit drempelwaarden gedefinieerde per-meetwaarde via de definitie cluster:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

U ziet dat taakverdeling en activiteit drempelwaarden beide gekoppeld aan de meetwaarde - verdelen wordt alleen geactiveerd als taakverdeling en activiteit drempelwaarden voor de dezelfde meetwaarde worden overschreden. Dus als we de drempelwaarde voor het verdelen voor geheugen en de activiteit drempel voor CPU overschrijdt, worden het verdelen van niet geactiveerd, zolang de resterende drempelwaarden (het verdelen van drempel voor CPU) en activiteit drempel voor geheugen niet worden overschreden.

## <a name="balancing-services-together"></a>Samen met het verdelen van services
Er is iets die interessant is dat of de cluster al dan niet imbalanced is een besluit cluster hele, maar de manier waarop we gaat over het oplossen van deze afzonderlijke service replica's en exemplaren rond worden verplaatst. Dit is zinvol, rechts? Als op een van de knooppunten meerdere replica's geheugen omhoog is gestapeld of exemplaren een bijdrage aan deze leveren kunnen, zodat deze kan moeten worden verplaatst een van de statuscontrole replica's of stateless exemplaren die de desbetreffende, imbalanced-meetwaarde gebruiken.

Een tickets zeggen dat een service die is niet imbalanced hebt u af en toe verplaatst Hoewel een klant telefonisch ons omhoog of bestand. Hoe kan het gebeuren dat een service rond wordt verplaatst, zelfs als alle van die service aan de doelstellingen zijn verdeeld, zelfs perfect dat het geval is, op het moment van het andere onevenwicht? Laten we eens kijken!

Neem bijvoorbeeld vier services, Service1 Service2, Service3 en Service4. Service1 rapporten met de maatstaven Metric1 en Metric2, Service2 tegen Metric2 en Mmetric3, Service3 tegen Metric3 en Metric4 en Service4 tegen sommige metrische Metric99. Nietwaar kunt u zien waar gaan we hier. We hebben een ketting! Vanuit het perspectief van de Cluster Resource Manager, niet echt zijn er vier onafhankelijke services, zijn er een reeks services die gerelateerde (Service1, Service2, en Service3) en een die is uitgeschakeld op een eigen.

![Samen met het verdelen van Services][Image4]

Is het mogelijk dat een onevenwicht in Metric1 leiden tot kan replica's of exemplaren die deel uitmaken van Service3 bewegen. Meestal die bewegingen zijn heel beperkt, maar kan worden groter is afhankelijk van precies hoe imbalanced Metric1 hebt en welke wijzigingen in het cluster nodig zijn om te corrigeren. We kunnen ook dat met u kunt dat een onevenwicht in aan de doelstellingen 1, 2 of 3 nooit de bewegingen in Service4 veroorzaakt – zou er geen punt sinds de replica's verplaatsen of exemplaren die deel uitmaken van Service4 rond kunnen Sla absoluut van invloed zijn op het saldo van de doelstellingen 1, 2 of 3.

De Manager van de Resource Cluster automatisch weten welke services zijn gerelateerd, aangezien services wellicht zijn toegevoegd, verwijderd, of de configuratiewijziging metrische – bijvoorbeeld, zoals tussen twee opeenvolgende het verdelen van Service2 mogelijk hebben opnieuw geconfigureerd als u wilt verwijderen Metric2 te achterhalen. Hiermee wordt de keten tussen Service1 en Service2. In plaats van twee groepen van services hebt u nu drie:

![Samen met het verdelen van Services][Image5]

## <a name="next-steps"></a>Volgende stappen
- Aan de doelstellingen zijn hoe de-stof Cluster Resource servicebeheer verbruik en capaciteit in het cluster beheert. Meer informatie over deze en hoe u deze configureert [in dit artikel](service-fabric-cluster-resource-manager-metrics.md) hebt uitchecken
- Kosten van de verplaatsingsopties is een manier om naar de Cluster Resource Manager-signalering dat bepaalde services zijn meer duur dan andere verplaatsen. Meer informatie over de verplaatsing kosten, raadpleegt u [in dit artikel](service-fabric-cluster-resource-manager-movement-cost.md)
- De Manager van de Resource Cluster heeft verschillende throttles die u configureren kunt als u lospeuteren in het cluster wilt vertragen. Dit zijn niet normaal nodig, maar als u deze nodig kunt u meer informatie over deze [hier](service-fabric-cluster-resource-manager-advanced-throttling.md)


[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
