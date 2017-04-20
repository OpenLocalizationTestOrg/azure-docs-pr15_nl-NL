<properties
   pageTitle="Geavanceerd gebruik van betrouwbare Services | Microsoft Azure"
   description="Meer informatie over Geavanceerd gebruik van de Service van betrouwbare configuratieservices voor extra flexibiliteit in uw services."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Geavanceerd gebruik van de betrouwbare Services programming model
Azure-Service stof eenvoudiger schrijven en betrouwbare stateless en statuscontrole services beheren. Deze handleiding wordt nader bekeken Geavanceerd gebruik van betrouwbare Services meer controle en flexibiliteit krijgen via uw services. Voordat u deze handleiding leest, moet Maak uzelf vertrouwd met [De betrouwbare Services programming model](service-fabric-reliable-services-introduction.md).

Statuscontrole zowel stateless services bestaan uit twee primaire invoer punten voor gebruikerscode:

 - `RunAsync`is een algemene ingangspunt voor uw servicecode.
 - `CreateServiceReplicaListeners`en `CreateServiceInstanceListeners` is voor het openen van communicatie listeners voor clientaanvragen.
 
Voor de meeste services zijn deze twee invoer wordt verwezen voldoende. In sommige gevallen zijn meer controle over de levenscyclus van een service is vereist, de levenscyclus van de aanvullende gebeurtenissen beschikbaar.

## <a name="stateless-service-instance-lifecycle"></a>Stateless service exemplaar lifecycle

Een stateless service levenscyclus is heel eenvoudig. Een stateless service kan alleen worden geopend, gesloten of afgebroken. `RunAsync`in een stateless service wordt uitgevoerd wanneer een exemplaar van service wordt geopend, en wanneer een exemplaar van service wordt gesloten of afgebroken geannuleerd. 

Hoewel `RunAsync` moet voldoende in bijna alle gevallen, het is geopend, sluit en afbreken gebeurtenissen in een stateless service zijn ook beschikbaar:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
  OnOpenAsync wordt aangeroepen wanneer het exemplaar stateless service wordt gebruikt. Uitgebreide service initialisatietaken kunnen worden gestart op dit moment.

- `Task OnCloseAsync(CancellationToken)`
  OnCloseAsync wordt aangeroepen wanneer het exemplaar stateless service dient zonder problemen worden afgesloten. Dit kan gebeuren wanneer de code van de service wordt bijgewerkt, het exemplaar van de service wordt verplaatst gevolg van taakverdeling of een tijdelijk probleem wordt aangetroffen. OnCloseAsync kan worden gebruikt voor het veilig alle bronnen sluiten, de achtergrondverwerking van een stoppen, voltooien externe staat op te slaan of sluiten bestaande verbindingen.

- `void OnAbort()`
  OnAbort wordt aangeroepen wanneer het exemplaar stateless service overname wordt afgesloten. In het algemeen heet dit als een permanente foutenstructuuranalyse op het knooppunt wordt gevonden of als Service stof kan geen betrouwbaar beheren voor de levenscyclus van de service-instantie vanwege interne fouten.

## <a name="stateful-service-replica-lifecycle"></a>Statuscontrole-service replica lifecycle

Statuscontrole-service wordt op basis van levenscyclus is veel complexere dan een exemplaar van stateless service. Ook als u wilt openen, sluiten en gebeurtenissen breken, betrokken een replica statuscontrole service rol wijzigingen tijdens de levenscyclus. Wanneer een replica statuscontrole service rol, verandert de `OnChangeRoleAsync` gebeurtenis wordt geactiveerd:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
  OnChangeRoleAsync wordt aangeroepen wanneer de replica statuscontrole-service bijvoorbeeld rol in de primaire of secundaire wijzigen is. Primaire replica's krijgen schrijven status (zijn toegestaan maken en naar betrouwbaar verzamelingen schrijven). Secundaire replica's krijgen leesstatus (kan alleen worden gelezen uit bestaande betrouwbare collecties). De meeste kunnen worden geopend in een statuscontrole service wordt uitgevoerd met de primaire replica. Secundaire replica's kunnen uitvoeren validatie alleen-lezen, rapporten, datamining of andere taken alleen-lezen.

In een statuscontrole service, worden alleen de primaire replica heeft schrijftoegang tot stand en dus is gewoonlijk wanneer de service werkelijke hoeveelheid werk wordt uitgevoerd. De `RunAsync` methode in een statuscontrole service wordt uitgevoerd alleen wanneer de replica statuscontrole service primaire is. De `RunAsync` methode is geannuleerd als primaire op basis van de rol gewijzigd van primair af, alsmede tijdens de gebeurtenissen sluiten en afbreken. 

Met de `OnChangeRoleAsync` gebeurtenis kunt u inzetten afhankelijk van replica rol ook zoals in antwoord op rol wijzigen.

Een statuscontrole service biedt ook dezelfde vier levenscyclus gebeurtenissen als een stateless service, met dezelfde semantiek en gebruik zaken:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer geavanceerde onderwerpen die betrekking hebben op stof Service:

- [Statuscontrole betrouwbare Services configureren](service-fabric-reliable-services-configuration.md)

- [Service stof systeemstatus Inleiding](service-fabric-health-introduction.md)

- [Statusrapporten systeem gebruiken voor probleemoplossing](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [Met de Service Cluster Resource Manager stof voor Services configureren](service-fabric-cluster-resource-manager-configure-services.md)
