<properties
   pageTitle="Problemen met systeem statusrapporten | Microsoft Azure"
   description="Beschrijving van de statusrapporten verzonden door Azure-Service stof onderdelen en hun gebruik voor probleemoplossing cluster of toepassing problemen."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="use-system-health-reports-to-troubleshoot"></a>Gebruik systeem statusrapporten om op te lossen

Azure-Service stof onderdelen rapport afmelden bij het vak op alle entiteiten in het cluster. De [status opslaan](service-fabric-health-introduction.md#health-store) wordt gemaakt en Hiermee verwijdert u entiteiten op basis van de systeemrapporten. Dit ook kunt ordenen in een hiërarchie die wordt vastgelegd entiteit interacties.

> [AZURE.NOTE] Meer informatie over gezondheid-gerelateerde concepten, bij [Service stof systeemstatus model](service-fabric-health-introduction.md).

Statusrapporten systeem bieden inzicht in cluster en de functies en de vlag problemen via de servicestatus. Voor toepassingen en services, Controleer systeem statusrapporten of entiteiten zijn geïmplementeerd en vanuit het perspectief Service stof zijn goed. De rapporten bieden geen eventuele statuscontrole van de bedrijfslogica van de service of detectie van vastgelopen processen. Gebruiker services kunnen de systeemstatus-gegevens met de informatie die specifiek zijn voor hun logica verrijken.

> [AZURE.NOTE] Statusrapporten watchdogs zijn zichtbaar alleen *nadat* de systeemonderdelen maken voor een entiteit. Wanneer een entiteit wordt verwijderd, worden alle statusrapporten die is gekoppeld aan in de store status automatisch verwijderd. En dat geldt wanneer een nieuw exemplaar van de entiteit wordt gemaakt (bijvoorbeeld een nieuw exemplaar van de service-replica is gemaakt). Alle rapporten die zijn gekoppeld aan het oude exemplaar worden verwijderd en in de store afgewerkt.

Het systeemonderdeel-rapporten worden aangeduid door de bron, die met begint het '**systeem.**" voorvoegsel. Watchdogs niet hetzelfde voorvoegsel gebruiken voor hun bronnen, zoals rapporten met ongeldige parameters worden genegeerd.
Sommige systeemrapporten om te weten wat ze activeert en hoe u het corrigeren van de problemen vertegenwoordigen we bekijken.

> [AZURE.NOTE] Service stof blijft Voeg rapporten van voorwaarden belangrijke die zicht op wat gebeurt er in de cluster en de toepassing te verbeteren.

## <a name="cluster-system-health-reports"></a>Cluster systeem statusrapporten
De cluster systeemstatus entiteit wordt automatisch gemaakt in de winkel status. Als alles goed werkt, heeft dit geen een systeemrapport.

### <a name="neighborhood-loss"></a>Groep verlies
**System.Federation** rapporten een fout wanneer wordt vastgesteld dat een verlies van de groep. Het rapport is van afzonderlijke knooppunten, en het knooppunt-ID is opgenomen in de naam van de eigenschap. Als een groep gaan in de hele Service stof bellen verloren, kunt u meestal twee gebeurtenissen (beide zijden van het rapport tussenruimte) verwachten. Als u meer groepen gaan verloren, zijn er meer gebeurtenissen.

Het rapport bevat de globale-out als de optie time to live. Het rapport opnieuw elke helft van de TTL-duur van verzonden zo lang maken als de voorwaarde actief blijft. De gebeurtenis wordt automatisch verwijderd wanneer het verloopt. Verwijder als verlopen gedrag zorgt ervoor dat het rapport wordt leeggemaakt uit het archief systeemstatus correct, zelfs als de rapportage knooppunt niet beschikbaar is is.

- **Bron-id**: System.Federation
- **Eigenschap**: begint met de **groep** en knooppunt informatie bevat
- **Volgende stappen**: achterhalen waarom de omgeving gaat verloren (bijvoorbeeld de communicatie tussen knooppunten controleren).

## <a name="node-system-health-reports"></a>Statusrapporten knooppunt-systeem
**System.FM**, waarmee de Failover Management-service, is de instantie die worden beheerd door informatie over knooppunten. Elk knooppunt moet één rapport van System.FM met de status hebben. Het knooppunt entiteiten worden verwijderd wanneer de status van het knooppunt wordt verwijderd (Zie [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)).

### <a name="node-updown"></a>Knooppunt omhoog/omlaag
System.FM rapporten als OK wanneer het knooppunt lid van de bellen (deze actief is). Wordt een fout wanneer het knooppunt de bellen afwijkt (is geselecteerd, hetzij voor het upgraden of gewoon omdat deze is mislukt). De status hiërarchie ingebouwd door de winkel systeemstatus neemt actie geïmplementeerd entiteiten in correlatie met System.FM knooppunt rapporten. Dit houdt rekening met het knooppunt een virtuele bovenliggende van alle geïmplementeerd entiteiten. De geïmplementeerd entiteiten op dat knooppunt worden query's via als het knooppunt als omhoog wordt gemeld door System.FM, met de zelfde instantie als het exemplaar dat is gekoppeld aan de entiteiten. Wanneer System.FM meldt dat het knooppunt is niet beschikbaar of opnieuw gestart (een nieuw exemplaar), wordt de store status automatisch opruimen van de geïmplementeerd entiteiten die alleen op het knooppunt omlaag of in het vorige exemplaar van het knooppunt kunnen bestaan.

- **Bron-id**: System.FM
- **Eigenschap**: staat
- **Volgende stappen**: als het knooppunt omlaag voor een upgrade, deze moet terugkomen omhoog zodra deze is bijgewerkt. In dit geval moet de status worden veranderd terug naar OK. Als het knooppunt niet u terug keert of mislukt, moet het probleem meer onderzoek.

Het volgende voorbeeld ziet de gebeurtenis System.FM met een status van OK voor knooppunt van:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Certificaat verloopbeleid
**System.FabricNode** rapporten een waarschuwing wanneer certificaten die worden gebruikt door het knooppunt bijna verlopen bent. Er zijn drie certificaten per knooppunt: **Certificate_cluster**, **Certificate_server**en **Certificate_default_client**. Wanneer de verlooptijd ten minste twee weken is, is de status van het rapport OK. Wanneer de verlooptijd binnen twee weken is, is het rapporttype een waarschuwing. TTL van deze gebeurtenissen is oneindig en ze worden verwijderd wanneer een knooppunt het cluster verlaat.

- **Bron-id**: System.FabricNode
- **Eigenschap**: begint met het **certificaat** en bevat meer informatie over het certificaattype
- **Volgende stappen**: de certificaten bijwerken als ze bijna verlopen zijn.

### <a name="load-capacity-violation"></a>Capaciteit overtreding laden
De verdeling van de Service stof belasting rapporten een waarschuwing weergegeven als er een overtreding van de capaciteit knooppunt worden gedetecteerd.

 - **Bron-id**: System.PLB
 - **Eigenschap**: begint met **capaciteit**
 - **Volgende stappen**: Schakel verstrekt aan de doelstellingen en de huidige capaciteit van het knooppunt weergeven.

## <a name="application-system-health-reports"></a>Statusrapporten van toepassing-systeem
**System.CM**, waarmee de Cluster Manager-service, is de instantie die worden beheerd door informatie over het gebruik van een toepassing.

### <a name="state"></a>De staat
System.CM rapporten als OK wanneer de toepassing is gemaakt of bijgewerkt. Deze melding aan de systeemstatus store wanneer de toepassing is verwijderd, zodat deze kan worden verwijderd uit de store.

- **Bron-id**: System.CM
- **Eigenschap**: staat
- **Volgende stappen**: als de toepassing is gemaakt, moet deze het statusrapport Cluster Manager opnemen. Controleer de status van de toepassing door een query (bijvoorbeeld de PowerShell-cmdlet * *Get-ServiceFabricApplication-ApplicationName *applicationName***).

Het volgende voorbeeld ziet u de gebeurtenis staat op de **stof: / WordCount** toepassing:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>Statusrapporten van service-systeem
**System.FM**, waarmee de Failover Management-service, is de instantie die worden beheerd door informatie over services.

### <a name="state"></a>De staat
System.FM rapporten als OK wanneer de service is gemaakt. Celbewerkingsmodus verwijdert u hiermee de entity vanuit de systeemstatus store wanneer de service is verwijderd.

- **Bron-id**: System.FM
- **Eigenschap**: staat

Het volgende voorbeeld ziet u de gebeurtenis staat op de service **stof: / WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Niet-geplaatste replica overtreding
**System.PLB** rapporten een waarschuwing als deze een plaatsing voor een of meer service replica niet kunt vinden. Het rapport wordt verwijderd wanneer het verloopt.

- **Bron-id**: System.FM
- **Eigenschap**: staat
- **Volgende stappen**: Controleer de service-beperkingen en de huidige status van de positie.

Het volgende voorbeeld ziet een overtreding van een service die is geconfigureerd met 7 doel replica's in een cluster met 5 knooppunten:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Statusrapporten Partition-systeem
**System.FM**, waarmee de Failover Management-service, is de instantie die worden beheerd door informatie over het service-partities.

### <a name="state"></a>De staat
System.FM rapporten als OK wanneer de partition is gemaakt en in orde is. Celbewerkingsmodus verwijdert u hiermee de entity vanuit de systeemstatus store wanneer de partition wordt verwijderd.

Als het gaat onder het aantal minimale replica, wordt een fout. Als de partition niet lager dan het aantal minimale replica is, maar het onder de doeltoepassing replica telling is, wordt een waarschuwing. Als het gaat in quorum verlies, rapporten System.FM een fout.

Andere belangrijke gebeurtenissen bevatten een waarschuwing weergegeven wanneer u het opnieuw configureren langer duren dan verwacht en wanneer het bouwen langer duurt dan verwacht. De verwachte tijden voor de opbouwen en het wijzigen van de configuratie geconfigureerd worden op basis van de service-scenario's. Bijvoorbeeld als een service een terabyte van staat, zoals SQL-Database heeft, langer het bouwen duren dan voor een service met een kleine hoeveelheid staat.

- **Bron-id**: System.FM
- **Eigenschap**: staat
- **Volgende stappen**: als de status niet OK is, is het mogelijk dat sommige replica's niet zijn gemaakt, geopend, of gepromoveerd tot primaire of secundaire correct. In veel gevallen is de oorzaak een service-fout in de implementatie openen of rol wijzigen.

Het volgende voorbeeld ziet een correct partition:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

Het volgende voorbeeld ziet u de status van een partition die zich onder doel replica tellen. De volgende stap is om de Partitiebeschrijving, waarin wordt getoond hoe deze is geconfigureerd: **MinReplicaSetSize** is twee en **TargetReplicaSetSize** is zeven. Vervolgens krijgt u het aantal knooppunten in het cluster: vijf. Zo in dat geval kunnen niet twee replica's worden geplaatst.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Replica beperking overtreding
**System.PLB** rapporten een waarschuwing als dit een overtreding van de beperking replica vastgesteld en kopieën van de partition kan niet worden geplaatst.

- **Bron-id**: System.PLB
- **Eigenschap**: begint met **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Statusrapporten replica-systeem
**System.RA**, waarmee u het onderdeel van de agent wijzigen van de configuratie, is de instantie voor de replica staat.

### <a name="state"></a>De staat
**System.RA** rapporten als OK wanneer de replica is gemaakt.

- **Bron-id**: System.RA
- **Eigenschap**: staat

Het volgende voorbeeld ziet een correct replica:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Status van replica open
De beschrijving van dit statusrapport bevat de begintijd (Coordinated Universal Time) wanneer de oproep API is geactiveerd.

**System.RA** rapporten een waarschuwing als de geopende replica langer duren dan de geconfigureerde periode (standaard: 30 minuten). Als de API beschikbare service effecten, wordt het rapport is veel sneller uitgegeven (een configureerbare interval, met een standaardwaarde van 30 seconden). De tijd gemeten bevat de tijd voor de distributeur openen en de service is geopend. De eigenschap verandert in OK als de openen is voltooid.

- **Bron-id**: System.RA
- **Eigenschap**: **ReplicaOpenStatus**
- **Volgende stappen**: als de status niet OK is, achterhalen waarom de geopende replica duurt langer dan verwacht.

### <a name="slow-service-api-call"></a>Traag service API-oproep
**System.RAP** en **System.Replicator** rapport een waarschuwing als u een oproep door naar de code van de gebruiker langer duren dan de geconfigureerde tijd. De waarschuwing wordt gewist wanneer het gesprek is voltooid.

- **Bron-id**: System.RAP of System.Replicator
- **Eigenschap**: de naam van de traag API. De beschrijving van de vindt u meer informatie over de tijd die de API in behandeling is.
- **Volgende stappen**: achterhalen waarom de oproep duurt langer dan verwacht.

Het volgende voorbeeld ziet een partition in quorum verlies en de onderzoek stappen gereed om te bepalen waarom. Een van de replica's heeft een waarschuwing status, zodat u toegang hebt eigen status. Deze ziet u dat de servicebewerking langer duurt dan verwacht, een gebeurtenis die door System.RAP gerapporteerd. Nadat u deze informatie wordt ontvangen, is de volgende stap kijkt u naar de servicecode en er onderzoeken. Voor deze zaak genereert de **RunAsync** uitvoering van de statuscontrole service een onverwerkte uitzondering. De replica's zijn hergebruik, zodat alle replica's in de waarschuwingstoestand mogelijk niet weergegeven. U kunt aan de status opnieuw en zoekt u eventuele verschillen in de replica-ID. In bepaalde gevallen, kunnen de nieuwe pogingen geven u aanwijzingen.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Wanneer u de toepassing niet juist werkt onder foutopsporing begint, weergeven de vensters diagnostische gebeurtenissen de uitzondering van RunAsync:

![Visual Studio-2015 diagnostische gebeurtenissen: RunAsync is mislukt in stof: / HelloWorldStatefulApplication.][1]

Visual Studio-2015 diagnostische gebeurtenissen: RunAsync is mislukt in **stof: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Replicatiewachtrij volledige
**System.Replicator** rapporten een waarschuwing als de replicatiewachtrij volledig is. Klik op de primaire gebeurt dit meestal omdat een of meer secundaire replica traag erkennen bewerkingen. Klik op de secundaire gebeurt dit meestal wanneer de service is traag om toe te passen de bewerkingen. De waarschuwing wordt gewist wanneer de wachtrij niet langer vol is.

- **Bron-id**: System.Replicator
- **Eigenschap**: **PrimaryReplicationQueueStatus** of **SecondaryReplicationQueueStatus**, afhankelijk van de rol replica

### <a name="slow-naming-operations"></a>Traag Naming bewerkingen

Wanneer u een bewerking Naming langer duren dan aanvaardbaar- **System.NamingService** systeemstatus rapporten op de primaire replica. Voorbeelden van Naming bewerkingen zijn [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) of [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). Meer manieren kunnen u vinden onder FabricClient, bijvoorbeeld onder [service management methoden](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) of [eigenschap management methoden](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx).

> [AZURE.NOTE] De Naming service servicenamen worden omgezet in een locatie in het cluster en waarmee gebruikers kunnen de servicenamen en eigenschappen beheren. Het is een Service stof partitioneren behouden gebleven service. Een van de partities Hiermee geeft u de eigenaar van de instantie, met metagegevens over alle Service stof namen en -services. De namen van de Service stof zijn toegewezen aan verschillende partities, genaamd naameigenaar partities, zodat de service extensible is. Lees meer over [Naming service](service-fabric-architecture.md).

Wanneer u een bewerking Naming langer duurt dan verwacht, wordt de bewerking is gemarkeerd met een waarschuwing-rapport op de *primaire replica van de Naming service partition die de bewerking fungeert*. Als de bewerking voltooid is, wordt de waarschuwing is uitgeschakeld. Als de bewerking is voltooid met een fout, bevat het statusrapport details over de fout.

- **Bron-id**: System.NamingService
- **Eigenschap**: begint met voorvoegsel **Duration_** en geeft aan wat het traag betrekking heeft en de naam van de Service stof waarop de bewerking is toegepast. Bijvoorbeeld als service bij naam stof maken: / Mijntoep/MijnService is te lang duurt, is de eigenschap Duration_AOCreateService.fabric:/MyApp/MyService. AO verwijst naar de rol van de partition Naming voor deze naam en de bewerking.
- **Volgende stappen**: controleren waarom het Naming mislukt. Elke bewerking kan verschillende oorzaken hebben. Bijvoorbeeld verwijderen service mogelijk een knooppunt blijven hangen omdat de toepassinghost van op een knooppunt vanwege een fout gebruiker in de servicecode blijft vast.

Het volgende voorbeeld ziet u een bewerking van de service maken. De bewerking duurde langer dan de geconfigureerde duur. AO pogingen en werk verzendt naar Nee. NIET de laatste bewerking time-out voltooid. In dit geval is de dezelfde replica primaire voor zowel het AO en geen rollen.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>Statusrapporten DeployedApplication-systeem
**System.Hosting** is de instantie op geïmplementeerd entiteiten.

### <a name="activation"></a>Activering
System.Hosting rapporten als OK wanneer een toepassing is geactiveerd op het knooppunt. Anders wordt een fout.

- **Bron-id**: System.Hosting
- **Eigenschap**: activering, inclusief de versie uitrol
- **Volgende stappen**: als de toepassing beschadigd is, achterhalen waarom de activering is mislukt.

Het volgende voorbeeld ziet succesvolle activering:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Downloaden
**System.Hosting** rapporten een fout als het downloaden van toepassing-pakket is mislukt.

- **Bron-id**: System.Hosting
- **Eigenschap**: * *downloaden:*RolloutVersion***
- **Volgende stappen**: achterhalen waarom het downloaden op het knooppunt is mislukt.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage systeem statusrapporten
**System.Hosting** is de instantie op geïmplementeerd entiteiten.

### <a name="service-package-activation"></a>Service-pakket activering
System.Hosting rapporten als OK als de activering van de service-pakket op het knooppunt gelukt is. Anders wordt een fout.

- **Bron-id**: System.Hosting
- **Eigenschap**: activering
- **Volgende stappen**: achterhalen waarom de activering is mislukt.

### <a name="code-package-activation"></a>Code pakket activering
**System.Hosting** rapporten als OK voor elk code-pakket als de activering gelukt is. Als de activering mislukt, wordt een waarschuwing zoals deze is geconfigureerd. Als **CodePackage** mislukt te activeren of met een groter is dan de geconfigureerde **CodePackageHealthErrorThreshold eindigt**, waarin rapporten worden gehost fout een fout. Als een servicepakket meerdere code-pakketten bevat, wordt een rapport activering gegenereerd voor elke batch.

- **Bron-id**: System.Hosting
- **Eigenschap**: gebruikmaakt van het voorvoegsel **CodePackageActivation** en bevat de naam van de code-pakket en het ingangspunt als * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint/ingangspunt* ** (bijvoorbeeld **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>De service type is geregistreerd
**System.Hosting** rapporten als OK als het servicetype is geregistreerd. Deze rapporten een fout als de registratie is niet uitgevoerd in tijd (zoals geconfigureerd met behulp van **ServiceTypeRegistrationTimeout**). Als het servicetype niet geregistreerd vanaf het knooppunt is, namelijk de runtime is gesloten. Een waarschuwing hostingprovider-rapporten.

- **Bron-id**: System.Hosting
- **Eigenschap**: gebruik het voorvoegsel **ServiceTypeRegistration** en bevat de naam van de service type (bijvoorbeeld **ServiceTypeRegistration:FileStoreServiceType**)

Het volgende voorbeeld ziet een correct geïmplementeerd servicepakket:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Downloaden
**System.Hosting** rapporten een fout als het downloaden van de service-pakket is mislukt.

- **Bron-id**: System.Hosting
- **Eigenschap**: * *downloaden:*RolloutVersion***
- **Volgende stappen**: achterhalen waarom het downloaden op het knooppunt is mislukt.

### <a name="upgrade-validation"></a>Upgraden van gegevensvalidatie
**System.Hosting** rapporten een fout als validatie tijdens de upgrade mislukt of als de upgrade is mislukt in het knooppunt.

- **Bron-id**: System.Hosting
- **Eigenschap**: gebruik het voorvoegsel **FabricUpgradeValidation** en bevat de upgrade-versie
- **Beschrijving**: verwijst naar de fout

## <a name="next-steps"></a>Volgende stappen
[Statusrapporten weergeven-Service stof](service-fabric-view-entities-aggregated-health.md)

[Hoe kunt melden en servicestatus controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Bewaken en diagnose stellen bij lokaal services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service stof toepassing upgrade](service-fabric-application-upgrade.md)
