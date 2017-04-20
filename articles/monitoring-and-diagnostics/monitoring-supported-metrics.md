<properties
    pageTitle="Azure Monitor aan de doelstellingen - ondersteunde aan de doelstellingen per resourcetype | Microsoft Azure"
    description="Lijst met parameters die beschikbaar zijn voor elk resourcetype met Azure Monitor."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="supported-metrics-with-azure-monitor"></a>Ondersteunde aan de doelstellingen met Azure Monitor

Azure Monitor kunt op verschillende manieren om te communiceren met de doelstellingen, zoals ze in de portal voor grafieken, opent deze via de REST API of query's uitvoeren op deze via PowerShell of CLI. Hieronder is een volledige lijst van alle doelstellingen momenteel beschikbaar met metrische verkooppijplijn van Azure Monitor.

> [AZURE.NOTE] Het is mogelijk dat andere aan de doelstellingen beschikbaar in de portal of met oudere API's. Deze lijst bevat alleen de doelstellingen van de openbare preview is beschikbaar met openbare preview van de samengevoegde Azure Monitor metrische pijplijn.

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CoreCount|Core tellen|Tellen|Totaal|Totaal aantal kernen in de batch-account|
|TotalNodeCount|Knooppunt tellen|Tellen|Totaal|Totaal aantal knooppunten in de batch-account|
|CreatingNodeCount|Maken van knooppunt tellen|Tellen|Totaal|Aantal knooppunten wordt gemaakt|
|StartingNodeCount|Begin knooppunt tellen|Tellen|Totaal|Aantal knooppunten starten|
|WaitingForStartTaskNodeCount|In afwachting van voor Start taak knooppunt tellen|Tellen|Totaal|Aantal knooppunten wachten op de taak starten om te voltooien|
|StartTaskFailedNodeCount|Begin na einde knooppunt tellen is mislukt|Tellen|Totaal|Aantal knooppunten waar de taak starten is mislukt|
|IdleNodeCount|Niet-actieve knooppunt tellen|Tellen|Totaal|Het aantal niet-actieve knooppunten|
|OfflineNodeCount|Offline knooppunt tellen|Tellen|Totaal|Aantal offline knooppunten|
|RebootingNodeCount|Opgestart knooppunt tellen|Tellen|Totaal|Aantal knooppunten opgestart|
|ReimagingNodeCount|Reimaging knooppunt tellen|Tellen|Totaal|Aantal reimaging knooppunten|
|RunningNodeCount|Actieve knooppunt tellen|Tellen|Totaal|Nummer van de uitvoering van knooppunten|
|LeavingPoolNodeCount|Groep knooppunt tellen verlaten|Tellen|Totaal|Aantal knooppunten de groep verlaten|
|UnusableNodeCount|Onbruikbaar knooppunt tellen|Tellen|Totaal|Aantal onbruikbaar knooppunten|
|TaskStartEvent|Taak starten gebeurtenissen|Tellen|Totaal|Totaal aantal taken die zijn gestart|
|TaskCompleteEvent|Taak voltooid gebeurtenissen|Tellen|Totaal|Totaal aantal taken die voltooid|
|TaskFailEvent|Taak Fail gebeurtenissen|Tellen|Totaal|Totaal aantal taken die voltooid defect|
|PoolCreateEvent|Groep gebeurtenissen maken|Tellen|Totaal|Totaal aantal toepassingen die zijn gemaakt|
|PoolResizeStartEvent|Groep formaat wijzigen Start gebeurtenissen|Tellen|Totaal|Totaal aantal wijzigen van de groep grootte die zijn gestart|
|PoolResizeCompleteEvent|Groep formaat wijzigen Complete-gebeurtenissen|Tellen|Totaal|Totaal aantal wijzigen van de groep grootte die voltooid|
|PoolDeleteStartEvent|Start gebeurtenissen die groep verwijderen|Tellen|Totaal|Totaal aantal toepassingen verwijderen die zijn gestart|
|PoolDeleteCompleteEvent|Voltooid gebeurtenissen die groep verwijderen|Tellen|Totaal|Totaal aantal toepassingen verwijderen die u hebt voltooid|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|connectedclients|Verbonden Clients|Tellen|Maximum||
|totalcommandsprocessed|Totale bewerkingen|Tellen|Totaal||
|cachehits|Treffers in cache|Tellen|Totaal||
|cachemisses|Cachemissers|Tellen|Totaal||
|getcommands|Met deze eigenschap|Tellen|Totaal||
|setcommands|Sets|Tellen|Totaal||
|evictedkeys|Verwijderde sleutels|Tellen|Totaal||
|totalkeys|Totale aantal sleutels|Tellen|Maximum||
|expiredkeys|Verlopen toetsen|Tellen|Totaal||
|usedmemory|Gebruikte geheugen|Bytes|Maximum||
|usedmemoryRss|Gebruikte geheugen RSS|Bytes|Maximum||
|serverLoad|Belasting van de server|Percentage|Maximum||
|cacheWrite|Cache schrijven|BytesPerSecond|Maximum||
|cacheRead|Cache lezen|BytesPerSecond|Maximum||
|percentProcessorTime|CPU|Percentage|Maximum||
|connectedclients0|Verbonden Clients (Shard 0)|Tellen|Maximum||
|totalcommandsprocessed0|Totale activiteiten (Shard 0)|Tellen|Totaal||
|cachehits0|Cachetreffers (Shard 0)|Tellen|Totaal||
|cachemisses0|Cachemissers (Shard 0)|Tellen|Totaal||
|getcommands0|Als resultaat gegeven (Shard 0)|Tellen|Totaal||
|setcommands0|Sets (Shard 0)|Tellen|Totaal||
|evictedkeys0|Verwijderde toetsen (Shard 0)|Tellen|Totaal||
|totalkeys0|Totale aantal sleutels (knooppunt 0)|Tellen|Maximum||
|expiredkeys0|Verlopen toetsen (Shard 0)|Tellen|Totaal||
|usedmemory0|Gebruikte geheugen (Shard 0)|Bytes|Maximum||
|usedmemoryRss0|Gebruikte geheugen RSS (Shard 0)|Bytes|Maximum||
|serverLoad0|Belasting van de server (Shard 0)|Percentage|Maximum||
|cacheWrite0|Cache schrijven (Shard 0)|BytesPerSecond|Maximum||
|cacheRead0|Cache-gelezen (Shard 0)|BytesPerSecond|Maximum||
|percentProcessorTime0|CPU (Shard 0)|Percentage|Maximum||
|connectedclients1|Verbonden Clients (Shard 1)|Tellen|Maximum||
|totalcommandsprocessed1|Totale activiteiten (Shard 1)|Tellen|Totaal||
|cachehits1|Cachetreffers (Shard 1)|Tellen|Totaal||
|cachemisses1|Cachemissers (Shard 1)|Tellen|Totaal||
|getcommands1|Als resultaat gegeven (Shard 1)|Tellen|Totaal||
|setcommands1|Sets (Shard 1)|Tellen|Totaal||
|evictedkeys1|Verwijderde toetsen (Shard 1)|Tellen|Totaal||
|totalkeys1|Totale aantal sleutels (knooppunt 1)|Tellen|Maximum||
|expiredkeys1|Verlopen toetsen (Shard 1)|Tellen|Totaal||
|usedmemory1|Gebruikte geheugen (Shard 1)|Bytes|Maximum||
|usedmemoryRss1|Gebruikte geheugen RSS (Shard 1)|Bytes|Maximum||
|serverLoad1|Belasting van de server (Shard 1)|Percentage|Maximum||
|cacheWrite1|Cache schrijven (Shard 1)|BytesPerSecond|Maximum||
|cacheRead1|Cache-gelezen (Shard 1)|BytesPerSecond|Maximum||
|percentProcessorTime1|CPU (Shard 1)|Percentage|Maximum||
|connectedclients2|Verbonden Clients (Shard 2)|Tellen|Maximum||
|totalcommandsprocessed2|Totale activiteiten (Shard 2)|Tellen|Totaal||
|cachehits2|Cachetreffers (Shard 2)|Tellen|Totaal||
|cachemisses2|Cachemissers (Shard 2)|Tellen|Totaal||
|getcommands2|Als resultaat gegeven (Shard 2)|Tellen|Totaal||
|setcommands2|Sets (Shard 2)|Tellen|Totaal||
|evictedkeys2|Verwijderde toetsen (Shard 2)|Tellen|Totaal||
|totalkeys2|Totale aantal sleutels (knooppunt 2)|Tellen|Maximum||
|expiredkeys2|Verlopen toetsen (Shard 2)|Tellen|Totaal||
|usedmemory2|Gebruikte geheugen (Shard 2)|Bytes|Maximum||
|usedmemoryRss2|Gebruikte geheugen RSS (Shard 2)|Bytes|Maximum||
|serverLoad2|Belasting van de server (Shard 2)|Percentage|Maximum||
|cacheWrite2|Cache schrijven (Shard 2)|BytesPerSecond|Maximum||
|cacheRead2|Cache-gelezen (Shard 2)|BytesPerSecond|Maximum||
|percentProcessorTime2|CPU (Shard 2)|Percentage|Maximum||
|connectedclients3|Verbonden Clients (Shard 3)|Tellen|Maximum||
|totalcommandsprocessed3|Totale activiteiten (Shard 3)|Tellen|Totaal||
|cachehits3|Cachetreffers (Shard 3)|Tellen|Totaal||
|cachemisses3|Cachemissers (Shard 3)|Tellen|Totaal||
|getcommands3|Als resultaat gegeven (Shard 3)|Tellen|Totaal||
|setcommands3|Sets (Shard 3)|Tellen|Totaal||
|evictedkeys3|Verwijderde toetsen (Shard 3)|Tellen|Totaal||
|totalkeys3|Totale toetsen (knooppunt 3)|Tellen|Maximum||
|expiredkeys3|Verlopen toetsen (Shard 3)|Tellen|Totaal||
|usedmemory3|Gebruikte geheugen (Shard 3)|Bytes|Maximum||
|usedmemoryRss3|Gebruikte geheugen RSS (Shard 3)|Bytes|Maximum||
|serverLoad3|Belasting van de server (Shard 3)|Percentage|Maximum||
|cacheWrite3|Cache schrijven (Shard 3)|BytesPerSecond|Maximum||
|cacheRead3|Cache-gelezen (Shard 3)|BytesPerSecond|Maximum||
|percentProcessorTime3|CPU (Shard 3)|Percentage|Maximum||
|connectedclients4|Verbonden Clients (Shard 4)|Tellen|Maximum||
|totalcommandsprocessed4|Totale activiteiten (Shard 4)|Tellen|Totaal||
|cachehits4|Cachetreffers (Shard 4)|Tellen|Totaal||
|cachemisses4|Cachemissers (Shard 4)|Tellen|Totaal||
|getcommands4|Als resultaat gegeven (Shard 4)|Tellen|Totaal||
|setcommands4|Sets (Shard 4)|Tellen|Totaal||
|evictedkeys4|Verwijderde toetsen (Shard 4)|Tellen|Totaal||
|totalkeys4|Totale aantal sleutels (knooppunt 4)|Tellen|Maximum||
|expiredkeys4|Verlopen toetsen (Shard 4)|Tellen|Totaal||
|usedmemory4|Gebruikte geheugen (Shard 4)|Bytes|Maximum||
|usedmemoryRss4|Gebruikte geheugen RSS (Shard 4)|Bytes|Maximum||
|serverLoad4|Belasting van de server (Shard 4)|Percentage|Maximum||
|cacheWrite4|Cache schrijven (Shard 4)|BytesPerSecond|Maximum||
|cacheRead4|Cache-gelezen (Shard 4)|BytesPerSecond|Maximum||
|percentProcessorTime4|CPU (Shard 4)|Percentage|Maximum||
|connectedclients5|Verbonden Clients (Shard 5)|Tellen|Maximum||
|totalcommandsprocessed5|Totale activiteiten (Shard 5)|Tellen|Totaal||
|cachehits5|Cachetreffers (Shard 5)|Tellen|Totaal||
|cachemisses5|Cachemissers (Shard 5)|Tellen|Totaal||
|getcommands5|Als resultaat gegeven (Shard 5)|Tellen|Totaal||
|setcommands5|Sets (Shard 5)|Tellen|Totaal||
|evictedkeys5|Verwijderde toetsen (Shard 5)|Tellen|Totaal||
|totalkeys5|Totale aantal sleutels (knooppunt 5)|Tellen|Maximum||
|expiredkeys5|Verlopen toetsen (Shard 5)|Tellen|Totaal||
|usedmemory5|Gebruikte geheugen (Shard 5)|Bytes|Maximum||
|usedmemoryRss5|Gebruikte geheugen RSS (Shard 5)|Bytes|Maximum||
|serverLoad5|Belasting van de server (Shard 5)|Percentage|Maximum||
|cacheWrite5|Cache schrijven (Shard 5)|BytesPerSecond|Maximum||
|cacheRead5|Cache-gelezen (Shard 5)|BytesPerSecond|Maximum||
|percentProcessorTime5|CPU (Shard 5)|Percentage|Maximum||
|connectedclients6|Verbonden Clients (Shard 6)|Tellen|Maximum||
|totalcommandsprocessed6|Totale activiteiten (Shard 6)|Tellen|Totaal||
|cachehits6|Cachetreffers (Shard 6)|Tellen|Totaal||
|cachemisses6|Cachemissers (Shard 6)|Tellen|Totaal||
|getcommands6|Als resultaat gegeven (Shard 6)|Tellen|Totaal||
|setcommands6|Sets (Shard 6)|Tellen|Totaal||
|evictedkeys6|Verwijderde toetsen (Shard 6)|Tellen|Totaal||
|totalkeys6|Totale aantal sleutels (knooppunt 6)|Tellen|Maximum||
|expiredkeys6|Verlopen toetsen (Shard 6)|Tellen|Totaal||
|usedmemory6|Gebruikte geheugen (Shard 6)|Bytes|Maximum||
|usedmemoryRss6|Gebruikte geheugen RSS (Shard 6)|Bytes|Maximum||
|serverLoad6|Belasting van de server (Shard 6)|Percentage|Maximum||
|cacheWrite6|Cache schrijven (Shard 6)|BytesPerSecond|Maximum||
|cacheRead6|Cache-gelezen (Shard 6)|BytesPerSecond|Maximum||
|percentProcessorTime6|CPU (Shard 6)|Percentage|Maximum||
|connectedclients7|Verbonden Clients (Shard 7)|Tellen|Maximum||
|totalcommandsprocessed7|Totale activiteiten (Shard 7)|Tellen|Totaal||
|cachehits7|Cachetreffers (Shard 7)|Tellen|Totaal||
|cachemisses7|Cachemissers (Shard 7)|Tellen|Totaal||
|getcommands7|Als resultaat gegeven (Shard 7)|Tellen|Totaal||
|setcommands7|Sets (Shard 7)|Tellen|Totaal||
|evictedkeys7|Verwijderde toetsen (Shard 7)|Tellen|Totaal||
|totalkeys7|Totale aantal sleutels (knooppunt 7)|Tellen|Maximum||
|expiredkeys7|Verlopen toetsen (Shard 7)|Tellen|Totaal||
|usedmemory7|Gebruikte geheugen (Shard 7)|Bytes|Maximum||
|usedmemoryRss7|Gebruikte geheugen RSS (Shard 7)|Bytes|Maximum||
|serverLoad7|Belasting van de server (Shard 7)|Percentage|Maximum||
|cacheWrite7|Cache schrijven (Shard 7)|BytesPerSecond|Maximum||
|cacheRead7|Cache-gelezen (Shard 7)|BytesPerSecond|Maximum||
|percentProcessorTime7|CPU (Shard 7)|Percentage|Maximum||
|connectedclients8|Verbonden Clients (Shard 8)|Tellen|Maximum||
|totalcommandsprocessed8|Totale activiteiten (Shard 8)|Tellen|Totaal||
|cachehits8|Cachetreffers (Shard 8)|Tellen|Totaal||
|cachemisses8|Cachemissers (Shard 8)|Tellen|Totaal||
|getcommands8|Als resultaat gegeven (Shard 8)|Tellen|Totaal||
|setcommands8|Sets (Shard 8)|Tellen|Totaal||
|evictedkeys8|Verwijderde toetsen (Shard 8)|Tellen|Totaal||
|totalkeys8|Totale toetsen (knooppunt 8)|Tellen|Maximum||
|expiredkeys8|Verlopen toetsen (Shard 8)|Tellen|Totaal||
|usedmemory8|Gebruikte geheugen (Shard 8)|Bytes|Maximum||
|usedmemoryRss8|Gebruikte geheugen RSS (Shard 8)|Bytes|Maximum||
|serverLoad8|Belasting van de server (Shard 8)|Percentage|Maximum||
|cacheWrite8|Cache schrijven (Shard 8)|BytesPerSecond|Maximum||
|cacheRead8|Cache-gelezen (Shard 8)|BytesPerSecond|Maximum||
|percentProcessorTime8|CPU (Shard 8)|Percentage|Maximum||
|connectedclients9|Verbonden Clients (Shard 9)|Tellen|Maximum||
|totalcommandsprocessed9|Totale activiteiten (Shard 9)|Tellen|Totaal||
|cachehits9|Cachetreffers (Shard 9)|Tellen|Totaal||
|cachemisses9|Cachemissers (Shard 9)|Tellen|Totaal||
|getcommands9|Als resultaat gegeven (Shard 9)|Tellen|Totaal||
|setcommands9|Sets (Shard 9)|Tellen|Totaal||
|evictedkeys9|Verwijderde toetsen (Shard 9)|Tellen|Totaal||
|totalkeys9|Totale aantal sleutels (knooppunt 9)|Tellen|Maximum||
|expiredkeys9|Verlopen toetsen (Shard 9)|Tellen|Totaal||
|usedmemory9|Gebruikte geheugen (Shard 9)|Bytes|Maximum||
|usedmemoryRss9|Gebruikte geheugen RSS (Shard 9)|Bytes|Maximum||
|serverLoad9|Belasting van de server (Shard 9)|Percentage|Maximum||
|cacheWrite9|Cache schrijven (Shard 9)|BytesPerSecond|Maximum||
|cacheRead9|Cache-gelezen (Shard 9)|BytesPerSecond|Maximum||
|percentProcessorTime9|CPU (Shard 9)|Percentage|Maximum||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|NumberOfCalls|Totaal aantal aanroepen van API|Tellen|Totaal|Totaal aantal API-oproepen.|
|NumberOfFailedCalls|Totaal aantal mislukte API-oproepen|Tellen|Totaal|Totaal aantal mislukte API-oproepen.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|Percentage processor|Percentage Processorsnelheid|Percentage|Gemiddelde|Het percentage toegewezen berekeningscluster eenheden die momenteel in gebruik is door de Virtual Machine(s)|
|Klik In het netwerk|Klik In het netwerk|Bytes|Totaal|Het aantal bytes dat ontvangen op alle netwerkinterfaces door de Virtual Machine(s) (binnenkomende verkeer)|
|Netwerk af|Netwerk af|Bytes|Totaal|Het aantal bytes dat op alle netwerkinterfaces door de Virtual Machine(s) (uitgaand verkeer)|
|Schijfruimte gelezen Bytes|Schijfruimte gelezen Bytes|Bytes|Totaal|Totaal aantal bytes lezen van schijf tijdens de controle van de termijn|
|Schijfruimte schrijven Bytes|Schijfruimte schrijven Bytes|Bytes|Totaal|Totaal aantal bytes naar de schijf tijdens de periode monitoring geschreven|
|Schijf bewerkingen/Sec lezen|Schijf bewerkingen/Sec lezen|CountPerSecond|Gemiddelde|Lezen van de schijf IO's / s|
|Geschreven bewerkingen per seconde|Geschreven bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf schrijven IO's / s|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|Percentage processor|Percentage processor|Percentage|Gemiddelde|Het percentage toegewezen berekeningscluster eenheden die momenteel in gebruik is door de Virtual Machine(s)|
|Klik In het netwerk|Klik In het netwerk|Bytes|Totaal|Het aantal bytes dat ontvangen op alle netwerkinterfaces door de Virtual Machine(s) (binnenkomende verkeer)|
|Netwerk af|Netwerk af|Bytes|Totaal|Het aantal bytes dat op alle netwerkinterfaces door de Virtual Machine(s) (uitgaand verkeer)|
|Schijfruimte gelezen Bytes|Schijfruimte gelezen Bytes|Bytes|Totaal|Totaal aantal bytes lezen van schijf tijdens de controle van de termijn|
|Schijfruimte schrijven Bytes|Schijfruimte schrijven Bytes|Bytes|Totaal|Totaal aantal bytes naar de schijf tijdens de periode monitoring geschreven|
|Schijf bewerkingen/Sec lezen|Schijf bewerkingen/Sec lezen|CountPerSecond|Gemiddelde|Lezen van de schijf IO's / s|
|Geschreven bewerkingen per seconde|Geschreven bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf schrijven IO's / s|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|Percentage processor|Percentage processor|Percentage|Gemiddelde|Het percentage toegewezen berekeningscluster eenheden die momenteel in gebruik is door de Virtual Machine(s)|
|Klik In het netwerk|Klik In het netwerk|Bytes|Totaal|Het aantal bytes dat ontvangen op alle netwerkinterfaces door de Virtual Machine(s) (binnenkomende verkeer)|
|Netwerk af|Netwerk af|Bytes|Totaal|Het aantal bytes dat op alle netwerkinterfaces door de Virtual Machine(s) (uitgaand verkeer)|
|Schijfruimte gelezen Bytes|Schijfruimte gelezen Bytes|Bytes|Totaal|Totaal aantal bytes lezen van schijf tijdens de controle van de termijn|
|Schijfruimte schrijven Bytes|Schijfruimte schrijven Bytes|Bytes|Totaal|Totaal aantal bytes naar de schijf tijdens de periode monitoring geschreven|
|Schijf bewerkingen/Sec lezen|Schijf bewerkingen/Sec lezen|CountPerSecond|Gemiddelde|Lezen van de schijf IO's / s|
|Geschreven bewerkingen per seconde|Geschreven bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf schrijven IO's / s|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetrielogboek bericht verzenden pogingen|Tellen|Totaal|Aantal apparaat naar Cloud telemetrielogboek berichten probeert te worden verzonden naar uw hub IoT|
|d2c.telemetry.ingress.Success|Telemetrielogboek-berichten die zijn verzonden|Tellen|Totaal|Aantal apparaat Cloud telemetrielogboek berichten verzonden naar uw hub IoT|
|c2d.Commands.egress.complete.Success|Opdrachten die zijn voltooid|Tellen|Totaal|Aantal Cloud naar apparaatopdrachten voltooid door het apparaat|
|c2d.Commands.egress.Abandon.Success|Opdrachten afgebroken|Tellen|Totaal|Aantal Cloud naar apparaatopdrachten afgebroken door het apparaat|
|c2d.Commands.egress.Reject.Success|Opdrachten geweigerd|Tellen|Totaal|Aantal Cloud naar apparaatopdrachten geweigerd door het apparaat|
|devices.totalDevices|Totale apparaten|Tellen|Totaal|Aantal apparaten geregistreerd op uw hub IoT|
|devices.connectedDevices.allProtocol|Verbonden apparaten|Tellen|Totaal|Aantal apparaten die zijn verbonden met uw hub IoT|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|INREQS|Verzoeken voor oproepen|Tellen|Totaal|Gebeurtenis-Hub binnenkomende bericht doorvoer voor een naamruimte|
|SUCCREQ|Voltooide aanvragen|Tellen|Totaal|Totaal aantal succesvolle aanvragen voor een naamruimte|
|FAILREQ|Mislukte aanvragen|Tellen|Totaal|Totaal aantal mislukte aanvragen voor een naamruimte|
|SVRBSY|Server bezet fouten|Tellen|Totaal|Totale server bezet fouten voor een naamruimte|
|INTERR|Interne serverfouten|Tellen|Totaal|Totale interne serverfouten voor een naamruimte|
|MISCERR|Andere fouten|Tellen|Totaal|Totaal aantal mislukte aanvragen voor een naamruimte|
|INMSGS|Binnenkomende berichten|Tellen|Totaal|Totale binnenkomende berichten voor een naamruimte|
|OUTMSGS|Uitgaande berichten|Tellen|Totaal|Totaal uitgaande berichten voor een naamruimte|
|EHINMBS|Binnenkomende bytes per seconde|BytesPerSecond|Totaal|Gebeurtenis-Hub binnenkomende bericht doorvoer voor een naamruimte|
|EHOUTMBS|Uitgaande bytes per seconde|BytesPerSecond|Totaal|Totaal uitgaande berichten voor een naamruimte|
|EHABL|Achterstallig werk-berichten archiveren|Tellen|Totaal|Berichten van gebeurtenis Hub archiveren in achterstallig werk voor een naamruimte|
|EHAMSGS|Berichten archiveren|Tellen|Totaal|Gebeurtenis-Hub gearchiveerde berichten in een naamruimte|
|EHAMBS|Archief bericht doorvoer|BytesPerSecond|Totaal|Gebeurtenis-Hub gearchiveerde bericht doorvoersnelheid in een naamruimte|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|RunsStarted|Wordt uitgevoerd de slag|Tellen|Totaal|Nummer van de werkstroom wordt uitgevoerd gestart.|
|RunsCompleted|Wordt uitgevoerd voltooid|Tellen|Totaal|Nummer van de werkstroom wordt uitgevoerd voltooide.|
|RunsSucceeded|Wordt uitgevoerd is geslaagd|Tellen|Totaal|Nummer van de werkstroom wordt uitgevoerd verwijderd.|
|RunsFailed|Wordt uitgevoerd, is mislukt|Tellen|Totaal|Nummer van de werkstroom wordt uitgevoerd is mislukt.|
|RunsCancelled|Wordt uitgevoerd geannuleerd|Tellen|Totaal|Nummer van de werkstroom wordt uitgevoerd geannuleerde.|
|RunLatency|Latentie uitvoeren|Seconden|Gemiddelde|Latentie van voltooide werkstroom wordt uitgevoerd.|
|RunSuccessLatency|Success latentie uitvoeren|Seconden|Gemiddelde|Latentie van geslaagde werkstroom wordt uitgevoerd.|
|RunThrottledEvents|Vertraagd gebeurtenissen uitvoeren|Tellen|Totaal|Aantal werkstroomactie of trigger vertraagd gebeurtenissen.|
|RunFailurePercentage|Fout bij Percentage uitvoeren|Percentage|Totaal|Percentage van de werkstroom wordt uitgevoerd is mislukt.|
|ActionsStarted|Acties gestart |Tellen|Totaal|Aantal werkstroomacties gestart.|
|ActionsCompleted|Acties voltooid |Tellen|Totaal|Het aantal werkstroomacties voltooid.|
|ActionsSucceeded|Acties is geslaagd |Tellen|Totaal|Aantal werkstroomacties is voltooid.|
|ActionsFailed|Acties is mislukt|Tellen|Totaal|Aantal werkstroomacties is mislukt.|
|ActionsSkipped|Acties overgeslagen |Tellen|Totaal|Aantal werkstroomacties overgeslagen.|
|ActionLatency|Actie latentie |Seconden|Gemiddelde|Latentie van voltooide werkstroomacties.|
|ActionSuccessLatency|Actie Success latentie |Seconden|Gemiddelde|Latentie van geslaagde werkstroomacties.|
|ActionThrottledEvents|Actie vertraagd gebeurtenissen|Tellen|Totaal|Aantal werkstroomactie vertraagd gebeurtenissen...|
|TriggersStarted|Triggers gestart |Tellen|Totaal|Aantal werkstroom triggers gestart.|
|TriggersCompleted|Triggers voltooid |Tellen|Totaal|Het aantal werkstroom triggers is voltooid.|
|TriggersSucceeded|Triggers is geslaagd |Tellen|Totaal|Aantal triggers van de werkstroom is voltooid.|
|TriggersFailed|Triggers is mislukt |Tellen|Totaal|Aantal werkstroom triggers is mislukt.|
|TriggersSkipped|Triggers overgeslagen|Tellen|Totaal|Aantal werkstroom triggers overgeslagen.|
|TriggersFired|Triggers gestart |Tellen|Totaal|Aantal triggers van de werkstroom wordt gestart.|
|TriggerLatency|Trigger latentie |Seconden|Gemiddelde|Latentie van voltooide werkstroom triggers.|
|TriggerFireLatency|Trigger Fire latentie |Seconden|Gemiddelde|Latentie van triggers van de werkstroom wordt gestart.|
|TriggerSuccessLatency|Trigger Success latentie |Seconden|Gemiddelde|Latentie van is geslaagd werkstroom triggers.|
|TriggerThrottledEvents|Trigger vertraagd gebeurtenissen|Tellen|Totaal|Aantal werkstroom trigger vertraagd gebeurtenissen.|
|BillableActionExecutions|Factureerbare actie Executions|Tellen|Totaal|Het aantal werkstroom actie executions ophalen gefactureerd.|
|BillableTriggerExecutions|Factureerbare Trigger Executions|Tellen|Totaal|Het aantal werkstroom trigger executions ophalen gefactureerd.|
|TotalBillableExecutions|Totale factureerbare Executions|Tellen|Totaal|Het aantal werkstroom executions ophalen gefactureerd.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|Doorvoer|Doorvoer|BytesPerSecond|Gemiddelde||

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|SearchLatency|Latentie zoeken|Seconden|Gemiddelde|Latentie van gemiddeld zoeken voor de zoekservice|
|SearchQueriesPerSecond|Zoekopdrachten per seconde|CountPerSecond|Gemiddelde|Query's per seconde voor de zoekservice zoeken|
|ThrottledSearchQueriesPercentage|Vertraagd zoeken query's percentage|Percentage|Gemiddelde|Percentage van zoekquery's die zijn vertraagd voor de zoekservice|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CPUXNS|CPU-gebruik per naamruimte|Percentage|Maximum|Service bus premium naamruimte CPU-gebruik meetwaarde|
|WSXNS|Geheugengebruik de grootte per naamruimte|Percentage|Maximum|Service bus premium naamruimte geheugen gebruik metrisch|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|cpu_percent|CPU-percentage|Percentage|Gemiddelde|CPU-percentage|
|physical_data_read_percent|Gegevens IO percentage|Percentage|Gemiddelde|Gegevens IO percentage|
|log_write_percent|Log IO percentage|Percentage|Gemiddelde|Log IO percentage|
|dtu_consumption_percent|DTU percentage|Percentage|Gemiddelde|DTU percentage|
|opslag|Totale databasegrootte|Bytes|Maximum|Totale databasegrootte|
|connection_successful|Geslaagde verbindingen|Tellen|Totaal|Geslaagde verbindingen|
|connection_failed|Mislukte verbindingen|Tellen|Totaal|Mislukte verbindingen|
|blocked_by_firewall|Geblokkeerd door de Firewall|Tellen|Totaal|Geblokkeerd door de Firewall|
|deadlock|Deadlocks|Tellen|Totaal|Deadlocks|
|storage_percent|Database grootte percentage|Percentage|Maximum|Database grootte percentage|
|xtp_storage_percent|In het geheugen OLTP opslag percent(Preview)|Percentage|Gemiddelde|In het geheugen OLTP opslag percent(Preview)|
|workers_percent|Werknemers percentage|Percentage|Gemiddelde|Werknemers procent|
|sessions_percent|Sessies met percentage|Percentage|Gemiddelde|Sessies met percentage|
|dtu_limit|DTU limiet|Tellen|Gemiddelde|DTU limiet|
|dtu_used|DTU gebruikt|Tellen|Gemiddelde|DTU gebruikt|
|service_level_objective|Niveau doel van de service van de database|Tellen|Totaal|Niveau doel van de service van de database|
|dwu_limit|dwu limiet|Tellen|Maximum|dwu limiet|
|dwu_consumption_percent|DWU percentage|Percentage|Gemiddelde|DWU percentage|
|dwu_used|DWU gebruikt|Tellen|Gemiddelde|DWU gebruikt|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|cpu_percent|CPU-percentage|Percentage|Gemiddelde|CPU-percentage|
|physical_data_read_percent|Gegevens IO percentage|Percentage|Gemiddelde|Gegevens IO percentage|
|log_write_percent|Log IO percentage|Percentage|Gemiddelde|Log IO percentage|
|dtu_consumption_percent|DTU percentage|Percentage|Gemiddelde|DTU percentage|
|storage_percent|Opslag percentage|Percentage|Gemiddelde|Opslag percentage|
|workers_percent|Werknemers procent|Percentage|Gemiddelde|Werknemers procent|
|sessions_percent|Sessies met percentage|Percentage|Gemiddelde|Sessies met percentage|
|eDTU_limit|eDTU limiet|Tellen|Gemiddelde|eDTU limiet|
|storage_limit|Opslaglimiet|Bytes|Gemiddelde|Opslaglimiet|
|eDTU_used|eDTU gebruikt|Tellen|Gemiddelde|eDTU gebruikt|
|storage_used|Opslagruimte gebruikt|Bytes|Gemiddelde|Opslagruimte gebruikt|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|ResourceUtilization|SO % gebruik|Percentage|Maximum|SO % gebruik|
|InputEvents|Invoer gebeurtenissen|Tellen|Totaal|Invoer gebeurtenissen|
|InputEventBytes|Invoer gebeurtenis Bytes|Bytes|Totaal|Invoer gebeurtenis Bytes|
|LateInputEvents|Vertraagde invoer gebeurtenissen|Tellen|Totaal|Vertraagde invoer gebeurtenissen|
|OutputEvents|Uitvoer gebeurtenissen|Tellen|Totaal|Uitvoer gebeurtenissen|
|ConversionErrors|Gegevens conversiefouten|Tellen|Totaal|Gegevens conversiefouten|
|Fouten|Runtime-fouten|Tellen|Totaal|Runtime-fouten|
|DroppedOrAdjustedEvents|Afmelden bij volgorde gebeurtenissen|Tellen|Totaal|Afmelden bij volgorde gebeurtenissen|
|AMLCalloutRequests|Functie aanvragen|Tellen|Totaal|Functie aanvragen|
|AMLCalloutFailedRequests|Mislukte functie aanvragen|Tellen|Totaal|Mislukte functie aanvragen|
|AMLCalloutInputEvents|Functie gebeurtenissen|Tellen|Totaal|Functie gebeurtenissen|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CpuPercentage|CPU-Percentage|Percentage|Gemiddelde|CPU-Percentage|
|MemoryPercentage|Geheugenpercentage|Percentage|Gemiddelde|Geheugenpercentage|
|DiskQueueLength|Lengte|Tellen|Totaal|Lengte|
|HttpQueueLength|HTTP-wachtrij lengte|Tellen|Totaal|HTTP-wachtrij lengte|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|

## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CpuTime|CPU-tijd|Seconden|Totaal|CPU-tijd|
|Aanvragen|Aanvragen|Tellen|Totaal|Aanvragen|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|
|Http2xx|HTTP-2xx|Tellen|Totaal|HTTP-2xx|
|Http3xx|HTTP-3xx|Tellen|Totaal|HTTP-3xx|
|Http401|HTTP 401|Tellen|Totaal|HTTP 401|
|Http403|HTTP 403|Tellen|Totaal|HTTP 403|
|Http404|HTTP 404|Tellen|Totaal|HTTP 404|
|Http406|HTTP 406|Tellen|Totaal|HTTP 406|
|Http4xx|HTTP-4xx|Tellen|Totaal|HTTP-4xx|
|Http5xx|HTTP-Server fouten|Tellen|Totaal|HTTP-Server fouten|
|MemoryWorkingSet|Geheugenwerkset|Bytes|Totaal|Geheugenwerkset|
|AverageMemoryWorkingSet|Gemiddelde geheugen werkset|Bytes|Gemiddelde|Gemiddelde geheugen werkset|
|AverageResponseTime|Gemiddelde antwoord tijd|Seconden|Gemiddelde|Gemiddelde antwoord tijd|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrisch|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CpuTime|CPU-tijd|Seconden|Totaal|CPU-tijd|
|Aanvragen|Aanvragen|Tellen|Totaal|Aanvragen|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|
|Http2xx|HTTP-2xx|Tellen|Totaal|HTTP-2xx|
|Http3xx|HTTP-3xx|Tellen|Totaal|HTTP-3xx|
|Http401|HTTP 401|Tellen|Totaal|HTTP 401|
|Http403|HTTP 403|Tellen|Totaal|HTTP 403|
|Http404|HTTP 404|Tellen|Totaal|HTTP 404|
|Http406|HTTP 406|Tellen|Totaal|HTTP 406|
|Http4xx|HTTP-4xx|Tellen|Totaal|HTTP-4xx|
|Http5xx|HTTP-Server fouten|Tellen|Totaal|HTTP-Server fouten|
|MemoryWorkingSet|Geheugenwerkset|Bytes|Totaal|Geheugenwerkset|
|AverageMemoryWorkingSet|Gemiddelde geheugen werkset|Bytes|Gemiddelde|Gemiddelde geheugen werkset|
|AverageResponseTime|Gemiddelde antwoord tijd|Seconden|Gemiddelde|Gemiddelde antwoord tijd|

## <a name="next-steps"></a>Volgende stappen

- [Lees meer over de doelstellingen in Azure Monitor](./monitoring-overview.md#monitoring-sources)
- [Waarschuwingen maken van een maatstelsel](./insights-receive-alert-notifications.md)
- [Aan de doelstellingen exporteren naar opslag, gebeurtenis Hub of Log Analytics](./monitoring-overview-of-diagnostic-logs.md)
