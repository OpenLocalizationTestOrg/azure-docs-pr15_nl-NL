<properties
   pageTitle="Het weergeven van Azure-Service stof entiteiten samengevoegd systeemstatus | Microsoft Azure"
   description="Beschreven hoe u de query, weergeven en Azure-Service stof entiteiten geaggregeerde status, tot en met algemene query's systeemstatus evalueren."
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

# <a name="view-service-fabric-health-reports"></a>Statusrapporten weergeven-Service stof
Azure-Service stof maakt u kennis met een [systeemstatus model](service-fabric-health-introduction.md) dat bestaat uit servicestatus entiteiten op welke onderdelen en watchdogs lokale voorwaarden die ze controleert kunt melden. De [status opslaan](service-fabric-health-introduction.md#health-store) verzamelt alle systeemstatus gegevens om te bepalen of entiteiten in orde zijn.

Afmelden bij het vak wordt het cluster gevuld met statusrapporten verzonden door de onderdelen van het systeem. Meer informatie aan [het systeem-statusrapporten gebruiken om op te lossen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Service stof kunt u meerdere manieren om de statistische status van de entiteiten:

- [Service stof Explorer](service-fabric-visualizing-your-cluster.md) of andere visualisatie hulpprogramma 's

- Query's voor status (via PowerShell, de API of REST)

- Algemeen query's dat afzender van een lijst met entiteiten met status als een van de eigenschappen (via PowerShell, de API of REST)

U kunt deze opties zien, gaat u gebruiken een lokale cluster met vijf knooppunten. Naast de **stof: / systeem** -toepassing (die bestaat uit het vak), andere toepassingen zijn geïmplementeerd. Een van deze toepassingen is **stof: / WordCount**. Deze toepassing bevat een statuscontrole service die is geconfigureerd met zeven replica's. Omdat er slechts vijf knooppunten, geven de systeemonderdelen een waarschuwing weergegeven dat de partition lager dan het aantal doel is.

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Servicestatus in Service stof Explorer
Service stof Explorer biedt een visuele weergave van het cluster. In de onderstaande afbeelding ziet u dat:

- De toepassing **stof: / WordCount** is rode (in fout) omdat er een foutgebeurtenis gemeld door **MyWatchdog** voor de **beschikbaarheid**van de eigenschap.

- Een van de services, **stof: / WordCount/WordCountService** is gele (in de waarschuwing). Aangezien de service is geconfigureerd met zeven replica's, kunnen niet ze alle worden geplaatst, omdat er slechts vijf knooppunten. Hoewel het niet hier weergegeven wordt, is de service-partition gele vanwege het rapport. De gele partition gebeurtenis de gele-service.

- Het cluster is rode vanwege de rode toepassing.

Standaardbeleidsregels uit de cluster manifest en toepassingsmanifest maakt gebruik van de evaluatie. Ze strikte beleidsregels zijn en niet laat deze niet doen toe.

Weergave van het cluster met Service stof Explorer:

![Weergave van het cluster met Service stof Verkenner.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] Meer informatie over de [Service stof Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Servicestatus query 's
Service stof beschrijft systeemstatus query's voor elk van de ondersteunde [Entiteitstypen](service-fabric-health-introduction.md#health-entities-and-hierarchy). Ze kunnen worden geopend door de API (de methoden kunnen worden gevonden op **FabricClient.HealthManager**), de PowerShell-cmdlets en de REST. Deze query's retourneren voltooid systeemstatus informatie over de entiteit: de geaggregeerde status, entiteit systeemstatus gebeurtenissen, onderliggende statussen (indien van toepassing) en beschadigd evaluaties wanneer de entiteit beschadigd is.

> [AZURE.NOTE] Een entity status geretourneerd wanneer deze in de winkel systeemstatus volledig is gevuld. De entiteit moet actief zijn (niet verwijderd) en een rapport systeem. De bovenliggende entiteiten in de keten hiërarchie moeten ook systeemrapporten bevatten. Als aan al deze voorwaarden is niet is voldaan, retourneert de systeemstatus query's een uitzondering waarin waarom de entiteit niet wordt geretourneerd.

De status query's moeten in de entiteit-id, dat afhankelijk van het entiteitstype is doorgeven. De query's accepteren optioneel systeemstatus beleidsparameters. Als u geen beleidsregels systeemstatus opgeeft, wordt de [systeemstatus beleidsregels](service-fabric-health-introduction.md#health-policies) van het manifest cluster of een toepassing worden gebruikt voor evaluatie. De query's accepteren ook filters om alleen gedeeltelijke children of gebeurtenissen--de bouwstenen die respecteren van de opgegeven filters te retourneren.

> [AZURE.NOTE] De uitvoerfilters zijn toegepast op de server, zodat het bericht beantwoorden wordt verkleind. Het is raadzaam dat u de uitvoerfilters gebruiken om de geretourneerde gegevens beperken, in plaats van filters toepassen op de client.

De status van een entiteit bevat:

- De samengevoegde status van de entiteit. Berekend door de systeemstatus store op basis van entiteit statusrapporten, onderliggende statussen (indien van toepassing) en beleid van de servicestatus. Lees meer over [entiteit systeemstatus evaluatie](service-fabric-health-introduction.md#entity-health-evaluation).  

- De status gebeurtenissen op de entiteit.

- De verzameling statussen van alle onderliggende voor de entiteiten die kinderen kunnen hebben. De statussen bevatten entiteit-id's en de geaggregeerde status. Als u volledige servicestatus voor een onderliggende termen, belt u met de status van de query voor het type van de onderliggende entiteit en in de onderliggende-id.

- De beschadigde evaluaties die naar het rapport dat de status van de entiteit geactiveerd verwijzen als de entiteit beschadigd is.

## <a name="get-cluster-health"></a>Cluster medische
Geeft als resultaat de status van de entiteit cluster en bevat de statussen van toepassingen en knooppunten (kinderen van het cluster). Invoer:

- [Optionele] De status cluster beleid gebruikt voor het evalueren van de knooppunten en de Clustergebeurtenissen.

- [Optionele] De toepassing systeemstatus beleid toewijzing, met de systeemstatus beleid voor het beleid van toepassing-manifest genegeerd.

- [Optionele] Filters voor gebeurtenissen, knooppunten en toepassingen die welke posten opgeven van belang zijn en in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten) moeten worden geretourneerd. Alle gebeurtenissen, knooppunten en -toepassingen worden gebruikt voor het evalueren van de status entiteit samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u cluster status, maak een `FabricClient` en de methode [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) aanroepen op de **HealthManager**.

De volgende oproep krijgt de status cluster:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

De volgende code krijgt de status cluster met behulp van een aangepaste cluster beleid en filters voor knooppunten en toepassingen. Hiermee maakt u [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), waarin de invoergegevens.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de status cluster is [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx). Eerst verbinding maken met het cluster met behulp van de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De status van de cluster is vijf knooppunten, de system-toepassing en stof: / WordCount geconfigureerd, zoals wordt beschreven.

De volgende cmdlet krijgt cluster systeemstatus met behulp van de servicestatus standaardbeleidsregels. De samengevoegde status is in de waarschuwing, omdat de stof: / WordCount toepassing bevindt zich in de waarschuwing. Houd er rekening mee dat de beschadigde evaluaties details biedt van de voorwaarden die de geaggregeerde status geactiveerd.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

Het volgende PowerShell-cmdlet krijgt de status van het cluster met behulp van een maatwerktoepassing-beleid. Resultaten verkrijgen alleen fout of waarschuwing-toepassingen en knooppunten Hiermee worden gefilterd. Hierdoor kunnen worden geen knooppunten geretourneerd, terwijl ze alles in orde zijn. Alleen de stof: / WordCount toepassing houdt rekening met het filter toepassingen. Omdat het aangepaste beleid bepaalt overwegingen waarschuwingen als fout de stof: / WordCount-toepassing, de toepassing zoals in de fout wordt geëvalueerd en dus is het cluster.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>REST
U kunt de servicestatus cluster met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707669.aspx) of van een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707696.aspx) met statusbeleidsregels die worden beschreven in de hoofdtekst krijgen.

## <a name="get-node-health"></a>Knooppunt medische
Geeft als resultaat de status van een entiteit knooppunt en bevat de systeemstatus-gebeurtenissen voor het knooppunt gerapporteerd. Invoer:

- [Vereist] De knooppuntnaam die het knooppunt aangeeft.

- [Optionele] De cluster systeemstatus beleidsinstellingen gebruikt om te evalueren status.

- [Optionele] Filters voor gebeurtenissen die welke posten opgeven van belang zijn en in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten) moeten worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de status entiteit samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u knooppunt servicestatus via de API, maak een `FabricClient` en de methode [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) aanroepen op de HealthManager.

De volgende code wordt de status van het knooppunt voor de naam van de opgegeven knooppunt:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

De volgende code wordt de status van het knooppunt voor de naam van de opgegeven knooppunt en passeert in gebeurtenissen filter en aangepast beleid [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de status van het knooppunt is [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Eerst verbinding maken met het cluster met behulp van de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .
De volgende cmdlet wordt de status van het knooppunt met behulp van de servicestatus standaardbeleidsregels:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

De volgende cmdlet wordt de status van alle knooppunten in het cluster:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>REST
U kunt de servicestatus knooppunt met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707650.aspx) of van een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707665.aspx) met statusbeleidsregels die worden beschreven in de hoofdtekst krijgen.

## <a name="get-application-health"></a>Toepassingsstatus ophalen
Geeft als resultaat de status van een Toepassingsentiteit. De presentatie bevat de statussen van de gedistribueerde toepassing en service kinderen. Invoer:

- [Vereist] Naam van de toepassing (URI) die de toepassing identificeert.

- [Optionele] Het beleid voor de toepassing systeemstatus om uit te schakelen van de manifest beleidsregels van toepassing.

- [Optionele] Filters voor gebeurtenissen, services en gedistribueerde toepassingen die welke posten opgeven van belang zijn en in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten) moeten worden geretourneerd. Alle gebeurtenissen, services en gebruikte toepassingen worden gebruikt voor het evalueren van de status entiteit samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u de toepassingsstatus, maakt u een `FabricClient` en de methode [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) aanroepen op de HealthManager.

De volgende code wordt de toepassingsstatus voor de opgegeven toepassingsnaam (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

De volgende code krijgt de toepassingsstatus voor de opgegeven toepassingsnaam (URI), met de filters en aangepaste beleid opgegeven via [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de toepassingsstatus is [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Eerst verbinding maken met het cluster met behulp van de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De volgende cmdlet retourneert de status van de **stof: / WordCount** toepassing:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Het volgende PowerShell-cmdlet wordt doorgegeven in aangepaste beleid. Deze filters ook kinderen en gebeurtenissen.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>REST
U kunt de toepassingsstatus met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707681.aspx) of van een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707643.aspx) met statusbeleidsregels die worden beschreven in de hoofdtekst krijgen.

## <a name="get-service-health"></a>Servicestatus ophalen
Geeft als resultaat de status van een service-entiteit. De presentatie bevat de statussen partition. Invoer:

- [Vereist] De servicenaam (URI) waarmee de service.

- [Optionele] Het beleid voor de toepassing systeemstatus gebruikt om te overschrijven het manifest beleid van toepassing.

- [Optionele] Filters voor gebeurtenissen en partities die welke posten opgeven van belang zijn en in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten) moeten worden geretourneerd. Alle evenementen en partities worden gebruikt voor het evalueren van de status entiteit samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u de servicestatus via de API, maak een `FabricClient` en de methode [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) aanroepen op de HealthManager.

Het volgende voorbeeld wordt de status van een service met de opgegeven servicenaam (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

De volgende code wordt opgehaald van de servicestatus voor de opgegeven servicenaam URI (), waarbij u filters en aangepast beleid via [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx)opgeeft:

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de servicestatus is [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Eerst verbinding maken met het cluster met behulp van de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De volgende cmdlet wordt opgehaald van de servicestatus met behulp van de servicestatus standaardbeleidsregels:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


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
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
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
```

### <a name="rest"></a>REST
U kunt de servicestatus met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707609.aspx) of van een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707646.aspx) met statusbeleidsregels die worden beschreven in de hoofdtekst krijgen.

## <a name="get-partition-health"></a>Partition medische
Geeft als resultaat de status van een entiteit partition. De presentatie bevat de statussen replica. Invoer:

- [Vereist] De partition-ID (GUID) die de partition aangeeft.

- [Optionele] Het beleid voor de toepassing systeemstatus gebruikt om te overschrijven het manifest beleid van toepassing.

- [Optionele] Filters voor gebeurtenissen en replica's met welke posten opgeven van belang zijn en in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten) moeten worden geretourneerd. Alle evenementen en replica's worden gebruikt voor het evalueren van de status entiteit samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u partition servicestatus via de API, maak een `FabricClient` en de methode [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) aanroepen op de HealthManager. Als u wilt opgeven optionele parameters, [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx)te maken.

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de status partition is [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Eerst verbinding maken met het cluster met behulp van de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De volgende cmdlet wordt de status voor alle partities van de **stof: / WordCount/WordCountService** service:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
U kunt de servicestatus partition met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707683.aspx) of van een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707680.aspx) met statusbeleidsregels die worden beschreven in de hoofdtekst krijgen.

## <a name="get-replica-health"></a>Replica medische
Geeft als resultaat de status van een replica statuscontrole service of een exemplaar van stateless service. Invoer:

- [Vereist] De partition-ID (GUID) en replica-ID die de replica aangeeft.

- [Optionele] De toepassing systeemstatus beleidsparameters gebruikt om te overschrijven de manifest beleidsregels van toepassing.

- [Optionele] Filters voor gebeurtenissen die welke posten opgeven van belang zijn en in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten) moeten worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de status entiteit samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u de status replica via de API, maak een `FabricClient` en de methode [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) aanroepen op de HealthManager. Als u wilt opgeven geavanceerde parameters, gebruikt u [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de status replica is [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Eerst verbinding maken met het cluster met behulp van de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De volgende cmdlet wordt de status van de primaire replica voor alle partities van de service:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
U kunt de servicestatus replica met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707673.aspx) of van een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707641.aspx) met statusbeleidsregels die worden beschreven in de hoofdtekst krijgen.

## <a name="get-deployed-application-health"></a>Gedistribueerde toepassing medische
Geeft als resultaat de status van een toepassing geïmplementeerd op een knooppunt entiteit. De presentatie bevat de statussen van de geïmplementeerd service-pakket. Invoer:

- [Vereist] De naam van de toepassing (URI) en een knooppuntnaam (tekenreeks) waarmee de gedistribueerde toepassing.

- [Optionele] Het beleid voor de toepassing systeemstatus om uit te schakelen van de manifest beleidsregels van toepassing.

- [Optionele] Filters voor gebeurtenissen en geïmplementeerd servicepakketten die welke posten opgeven van belang zijn en in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten) moeten worden geretourneerd. Alle gebeurtenissen en geïmplementeerd servicepakketten worden gebruikt voor het evalueren van de status entiteit samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u de status van een toepassing geïmplementeerd op een knooppunt via de API, maak een `FabricClient` en de methode [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) aanroepen op de HealthManager. Als u wilt opgeven optionele parameters, gebruikt u [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
De cmdlet om de status gedistribueerde toepassing is [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx). Eerst verbinding maken met het cluster met behulp van de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) . Als u wilt weten waar een toepassing wordt geïmplementeerd, [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) uitvoeren en kijkt u naar de kinderen gedistribueerde toepassing.

De volgende cmdlet wordt de status van de **stof: / WordCount** toepassing geïmplementeerd op **_Node_2**.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
U kunt de servicestatus gedistribueerde toepassing met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707644.aspx) of van een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707688.aspx) met statusbeleidsregels die worden beschreven in de hoofdtekst krijgen.

## <a name="get-deployed-service-package-health"></a>Servicestatus geïmplementeerd pakket ophalen
Geeft als resultaat de status van een pakket geïmplementeerd service entiteit. Invoer:

- [Vereist] De naam van de toepassing (URI), (tekenreeks) de knooppuntnaam van het en service bestandenlijst naam (tekenreeks) die de geïmplementeerd servicepakket herkennen.

- [Optionele] Het beleid voor de toepassing systeemstatus gebruikt om te overschrijven het manifest beleid van toepassing.

- [Optionele] Filters voor gebeurtenissen die welke posten opgeven van belang zijn en in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten) moeten worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de status entiteit samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u de status van een uitgevouwen servicepakket via de API, maak een `FabricClient` en de methode [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) aanroepen op de HealthManager. Als u wilt opgeven optionele parameters, gebruikt u [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
Ophalen van de servicestatus geïmplementeerd-pakket-de cmdlet is [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Eerst verbinding maken met het cluster met behulp van de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) . Als u wilt zien waar een toepassing wordt geïmplementeerd, [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) uitvoeren en kijkt u naar de gebruikte toepassingen. Als u wilt zien welke service pakketten in een toepassing zijn, kijkt u naar de service gedistribueerde pakket onderliggende items in de uitvoer [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) .

De volgende cmdlet wordt de status van het pakket **WordCountServicePkg** service van de **stof: / WordCount** toepassing geïmplementeerd op **_Node_2**. De entiteit heeft **System.Hosting** rapporten voor succesvolle servicepakket en ingangspunt activering en registratie van voltooid servicetype.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
U gaat geïmplementeerd servicestatus pakket met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707677.aspx) of van een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707689.aspx) met statusbeleidsregels die worden beschreven in de hoofdtekst.

## <a name="health-chunk-queries"></a>Servicestatus segment query 's
De status segment query's kunnen met meerdere niveaus cluster kinderen (recursief), per invoer filters terugkeren. Geavanceerde filters waarmee veel flexibiliteit Express welke specifieke kinderen moeten worden geretourneerd, die wordt aangeduid met hun unieke id of andere groeps-id en/of status wordt ondersteund. Standaard zijn geen kinderen opgenomen, in plaats van de servicestatus-opdrachten die Neem altijd eerste niveau kinderen.

De [status query's](service-fabric-view-entities-aggregated-health.md#health-queries) retourneren alleen eerste niveau kinderen van de opgegeven entiteit per vereiste filters. Als u de onderliggende items van de onderliggende items, moet u aanvullende systeemstatus API's voor elke entiteit belangrijke bellen. Op dezelfde manier als u de status van specifieke entiteiten, moet u één systeemstatus API voor elke gewenste entiteit bellen. Segment kunt geavanceerde filteren u de query aanvragen van meerdere items belangrijke in een query, de grootte van berichten en het aantal berichten minimaliseren.

De waarde van de query segment is dat u status voor meer cluster entiteiten (mogelijk alle cluster entiteiten beginnend bij de vereiste hoofdsite) kunt openen in een gesprek. Status van complexe query kunt u zoals uitdrukken:

- Retourneren alleen toepassingen fout en voor deze toepassingen opnemen alle services op waarschuwing | fout. Voor geretourneerde services, voegt u alle partities.

- Alleen de status van 4-toepassingen, opgegeven door hun namen te retourneren.

- Alleen de status van toepassingen van een type gewenste toepassing retourneren.

- Alle geïmplementeerd entiteiten in een knooppunt retourneren Hiermee retourneert u alle toepassingen, alle toepassingen op het opgegeven knooppunt en alle geïmplementeerd servicepakketten op dat knooppunt geïmplementeerd.

- Alle replica's op een fout geretourneerd. Geeft als resultaat alle toepassingen, services, partities en alleen replica's op een fout.

- Ga terug alle toepassingen. Voor een opgegeven service, voegt u alle partities.

De status segment query worden momenteel weergegeven alleen voor de entiteit cluster. Wordt een deel van de servicestatus cluster, met als resultaat gegeven:

- De status van cluster samengevoegd.

- De status staat segment lijst van knooppunten die invoer filters respecteren.

- De status staat segment lijst met toepassingen die invoer filters respecteren. Elke toepassing systeemstatus staat segment bevat een lijst segment met alle services die respecteren invoer filters en een segment lijst met alle gebruikte toepassingen die respecteren van de filters. Dit geldt ook voor de onderliggende items van services en de gebruikte toepassingen. Op deze manier alle entiteiten in het cluster kunnen worden mogelijk geretourneerd als dit wordt gevraagd, hiërarchisch.

### <a name="cluster-health-chunk-query"></a>Cluster systeemstatus segment query
Geeft als resultaat de status van de entiteit cluster en bevat de vereiste kinderen hiërarchische systeemstatus staat stukken. Invoer:

- [Optionele] De status cluster beleid gebruikt voor het evalueren van de knooppunten en de Clustergebeurtenissen.

- [Optionele] De toepassing systeemstatus beleid toewijzing, met de systeemstatus beleid voor het beleid van toepassing-manifest genegeerd.

- [Optionele] Filters voor knooppunten en toepassingen die welke posten opgeven van belang zijn en in het resultaat moeten worden geretourneerd. De filters zijn specifiek voor een entiteit/groep entiteiten of zijn van toepassing op alle entiteiten op dat niveau. De lijst met filters kan bevatten een algemeen filter-en/of filters voor specifieke id's voor fijnmazige entiteiten die door de query. Als leeg en worden de onderliggende items worden niet al dan niet standaard geretourneerd.
Meer informatie over de filters op [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) en [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx). De toepassing filters kunnen recursief Geef geavanceerde filters voor kinderen.

Het resultaat segment bevat de onderliggende items waarvan de filters respecteren.

De query segment retourneert momenteel geen beschadigd evaluaties of entiteit gebeurtenissen. Deze extra informatie kan worden verkregen met behulp van de bestaande cluster systeemstatus query.

### <a name="api"></a>API
Als u cluster systeemstatus segment, maakt u een `FabricClient` en de methode [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) aanroepen op de **HealthManager**. U kunt doorgeven in [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) om te beschrijven van statusbeleidsregels voor en geavanceerde filters.

De volgende code krijgt cluster systeemstatus segment met geavanceerde filters.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de status cluster is [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Eerst verbinding maken met het cluster met behulp van de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De volgende code krijgt knooppunten alleen als deze fout zijn, behalve voor een bepaald knooppunt, die altijd moet worden geretourneerd.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

De volgende cmdlet krijgt cluster segment waarop de toepassingsfilters.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

De volgende cmdlet retourneert alle geïmplementeerd entiteiten op een knooppunt.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>REST
U gaat cluster systeemstatus segment met een [GET-verzoek](https://msdn.microsoft.com/library/azure/mt656722.aspx) of een [POST-aanvraag](https://msdn.microsoft.com/library/azure/mt656721.aspx) met statusbeleidsregels en geavanceerde filters die worden beschreven in de hoofdtekst.

## <a name="general-queries"></a>Algemene query 's
Algemene query's retourneren een lijst met Service stof entiteiten van een bepaald type. Ze worden weergegeven via de API (via de methoden op **FabricClient.QueryManager**), de PowerShell-cmdlets en de REST. Deze query's samenvoegen subquery's uit meerdere onderdelen. Een van deze is de [status opslaan](service-fabric-health-introduction.md#health-store), waarin de geaggregeerde status voor elke queryresultaat gevuld.  

> [AZURE.NOTE] Algemene query's de geaggregeerde status van de entiteit terug en geen uitgebreide systeemstatus gegevens bevatten. Als een entiteit beschadigd is, kunt u met de systeemstatus query's voor alle bijbehorende status informatie, inclusief gebeurtenissen, onderliggende statussen en beschadigd evaluaties opvolgen.

Als algemene query's een onbekende status voor een entiteit retourneert, is het mogelijk dat de servicestatus store geen volledige gegevens over de entiteit. Het is ook mogelijk dat een subquery naar de store status niet geslaagd is (bijvoorbeeld: Er is een communicatiefout of de systeemstatus store is vertraagd). Opvolgen met een query servicestatus voor de entiteit. Als de subquery heeft voorgedaan tijdelijke fouten, zoals netwerkproblemen, kan deze opvolging query mislukt. Dit kan u ook daarna nog meer informatie geven uit het archief systeemstatus over waarom de entiteit niet worden weergegeven.

De query's met **HealthState** voor entiteiten zijn:

- Knooppunten: geeft als resultaat de knooppunten lijst in het cluster (geheugen:).
  - API: [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx)
  - PowerShell: Get-ServiceFabricNode
- Lijst met toepassingen: geeft als resultaat de lijst met toepassingen in het cluster (geheugen:).
  - API: [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricApplication
- Lijst met Services: geeft als resultaat de lijst met services in een toepassing (geheugen:).
  - API: [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx)
  - PowerShell: Get-ServiceFabricService
- Partitielijst: geeft als resultaat de lijst met partities in een service (geheugen:).
  - API: [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx)
  - PowerShell: Get-ServiceFabricPartition
- Replicalijst: geeft als resultaat de lijst met replica in een partition (geheugen:).
  - API: [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx)
  - PowerShell: Get-ServiceFabricReplica
- -Toepassingslijst geïmplementeerd: geeft als resultaat de lijst met gebruikte toepassingen op een knooppunt.
  - API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication
- Service-pakketlijst geïmplementeerd: geeft als resultaat de lijst met servicepakketten in een gedistribueerde toepassing.
  - API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] Sommige van de query's verwisselbaar resultaten worden geretourneerd. Het rendement van deze query's is een lijst afgeleid van [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). De resultaten niet passen een bericht, alleen op een pagina wordt geretourneerd als een ContinuationToken die wordt bijgehouden waar inventarisatie is gestopt. U moet bellen dezelfde query en geven in het token voortgezet uit de vorige query naar de volgende resultaten blijven.

### <a name="examples"></a>Voorbeelden

De volgende code wordt de beschadigde toepassingen in het cluster:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

De volgende cmdlet wordt de Toepassingdetails van de voor de stof: / WordCount-toepassing. U ziet dat status op waarschuwing.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

De volgende cmdlet wordt opgehaald van de services met een status van de waarschuwing:

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Cluster en toepassing upgrades
Tijdens een gecontroleerde upgrade van de cluster en de toepassing controleert Service stof status om ervoor te zorgen dat alles in orde blijft. Als een entiteit beschadigd is zoals geëvalueerd met behulp van geconfigureerde systeemstatus beleidsregels, geldt de upgrade upgrade-specifiek beleid om te bepalen van de volgende actie. De upgrade kan worden onderbroken als u wilt dat de interactie van de gebruiker (zoals fouten op te lossen of wijzigen van beleidsregels) of deze mogelijk automatisch terugkeren naar de vorige goede versie.

Tijdens het upgraden van een *cluster* , kunt u de bijwerkstatus cluster verkrijgen. De bijwerkstatus bevat beschadigd evaluaties, die verwijzen naar wat beschadigd in het cluster is. Als de upgrade wordt hersteld vanwege servicestatusproblemen zijn, onthoudt de upgradestatus van de laatste beschadigd redenen. Deze informatie kunt beheerders onderzoeken wat er fout is gegaan nadat de upgrade ongedaan of gestopt.

Daarnaast tijdens de upgrade van een *toepassing* , een beschadigde evaluaties staan in de bijwerkstatus van toepassing.

Hieronder vindt u de bijwerkstatus van toepassing een gewijzigde stof: / WordCount-toepassing. Een fout een watchdog gemeld op een van de replica's. De upgrade is schuivend, omdat de servicestatus controles niet worden nageleefd.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Meer informatie over het [upgraden van Service stof-toepassing](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Servicestatus evaluaties gebruiken om op te lossen
Wanneer er een probleem met het cluster of een toepassing is, kijkt u naar de status cluster of een toepassing te achterhalen wat is er mis. De beschadigde evaluaties bieden meer informatie over wat de huidige beschadigde status geactiveerd. Als u wilt, kunt u op beschadigd onderliggende entiteiten om aan te geven van de onderliggende oorzaak inzoomen.

> [AZURE.NOTE] De beschadigde evaluaties weergeven in dat de eerste reden de entiteit is geëvalueerd als de huidige status. Er zijn mogelijk meerdere andere gebeurtenissen die door deze status wordt geactiveerd, maar ze worden niet weergegeven in de evaluaties. Als u meer informatie, op Inzoomen de systeemstatus entiteiten om te bepalen de beschadigde rapporten in het cluster.

## <a name="next-steps"></a>Volgende stappen
[Gebruik systeem statusrapporten om op te lossen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Aangepaste Service stof statusrapporten toevoegen](service-fabric-report-health.md)

[Hoe kunt melden en servicestatus controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Bewaken en diagnose stellen bij lokaal services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service stof toepassing upgrade](service-fabric-application-upgrade.md)
