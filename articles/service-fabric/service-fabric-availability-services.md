<properties
   pageTitle="Beschikbaarheid van Service configuratieservices | Microsoft Azure"
   description="Beschrijving van fout detectie, failover en herstel voor services"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="availability-of-service-fabric-services"></a>Beschikbaarheid van Service configuratieservices
Azure-Service configuratieservices kunnen statuscontrole of stateless zijn. In dit artikel geeft een overzicht van hoe Service stof beschikbaarheid van een service in het geval van fouten onderhoudt.

## <a name="availability-of-service-fabric-stateless-services"></a>Beschikbaarheid van Service stateless configuratieservices
Een stateless service is een toepassing die geen een [lokale permanente status](service-fabric-concepts-state.md).

Voor het maken van een stateless service moet een aantal exemplaar, dat wil zeggen het aantal exemplaren van de stateless service die moet worden uitgevoerd in het cluster definiëren. Dit is het aantal exemplaren van de toepassingslogica die wordt in het cluster wordt gestart. Het aantal exemplaren met groter wordende is de beste manier schaalbaarheid van een stateless-service.

Wanneer een fout op elk exemplaar van een stateless service wordt gevonden, wordt een nieuw exemplaar gemaakt op enkele andere in aanmerking komend knooppunt in het cluster.

## <a name="availability-of-service-fabric-stateful-services"></a>Beschikbaarheid van Service statuscontrole configuratieservices
Een statuscontrole service heeft enkele is gekoppeld aan staat. Een statuscontrole service is gebaseerd in Service stof, als een set replica's. Elke replica is een exemplaar van de code van de service met een kopie van de staat. Lezen en schrijven bewerkingen worden uitgevoerd in de ene replica (de primaire genoemd). Wijzigingen in de provincie van schrijven bewerkingen worden *gerepliceerd* naar meerdere andere replica's (active secundaire servers genoemd). De combinatie van primaire en actieve secundaire replica's is de replicaset van de service.

Kunnen er slechts één primaire replica onderhoud lezen en schrijven aanvragen, maar kunnen er meerdere actieve secundaire replica's. Het aantal actieve secundaire replica's kan worden geconfigureerd en een hoger aantal replica's mogelijk een hoger aantal gelijktijdige software en problemen met de hardware zonder.

In het geval van een storing (wanneer de primaire replica uitvalt), Service stof zorgt ervoor dat een van de actieve secundaire replica's de nieuwe primaire replica. Deze actieve secundaire replica is al de bijgewerkte versie van de staat (via *replicatie*), en deze kunt doorgaan met het verwerken van verdere lezen en schrijven bewerkingen.

Dit concept--van een replicaset wordt een primaire of een actief secundair--de rol replica wordt genoemd.

### <a name="replica-roles"></a>Replica rollen
De rol van een replicaset wordt gebruikt voor het beheren van de levenscyclus van de wordt beheerd door deze staat. Een replica waarvan de rol primaire services is lezen aanvragen. Deze ook services schrijven aanvragen door de staat bijwerken en de wijzigingen worden gerepliceerd naar de actieve secundaire servers in de replicaset. De rol van een actieve secundaire is ontvangen staat wijzigingen die de primaire replica heeft gerepliceerd en bijwerken van de weergave van de staat.

>[AZURE.NOTE] Een hoger niveau programming modellen zoals het [betrouwbare betrokkenen framework](service-fabric-reliable-actors-introduction.md) abstracte afwezig het concept van replica rol van de ontwikkelaar.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service stof concepten:

- [Schaalbaarheid van Service configuratieservices](service-fabric-concepts-scalability.md)

- [Service configuratieservices partitioneren](service-fabric-concepts-partitioning.md)

- [Definiëren en staat beheren](service-fabric-concepts-state.md)
