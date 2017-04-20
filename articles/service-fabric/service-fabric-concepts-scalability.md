<properties
   pageTitle="Schaalbaarheid van Service configuratieservices | Microsoft Azure"
   description="Wordt beschreven hoe u de schaal van de Service configuratieservices"
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

# <a name="scaling-service-fabric-applications"></a>Schaalbaarheid van stof Service-toepassingen
Azure-Service stof kunt u gemakkelijk scalable toepassingen door taakverdeling services, partities en replica's op alle knooppunten in een cluster maken. Hiermee worden maximale Resourcegebruik.

Hoge schaal voor Service stof toepassingen kan worden bereikt op twee manieren:

1. Schaalbaarheid op het niveau van partition

2. Schaalbaarheid op het niveau van de naam van service

## <a name="scaling-at-the-partition-level"></a>Schaalbaarheid op het niveau van partition
Service stof ondersteunt een afzonderlijke service partitioneren in meerdere kleinere partities. Het [Overzicht partitioneren](service-fabric-concepts-partitioning.md) wordt aandacht besteed aan de soorten partities schema's die worden ondersteund. De kopieën van elke partition zijn verdeeld over de knooppunten in een cluster. Houd rekening met een service die een ranged partities kleurenschema met een lage sleutel van 0, een hoge sleutel van 99 en vier partities gebruikt. In een cluster met drie knooppunten, kan de service worden ingedeeld met vier replica's die de bronnen op elk knooppunt delen, zoals hier wordt getoond:

![Partition-indeling met drie knooppunten](./media/service-fabric-concepts-scalability/layout-three-nodes.png)

Het aantal knooppunten verhogen kunt Service stof de bronnen op de nieuwe knooppunten door enkele van de replica's verplaatsen naar de lege knooppunten gebruiken. Door het aantal knooppunten vier met groter wordende, bevat de service nu drie replica's uitgevoerd op elke knooppunt (van verschillende partities), waardoor een betere Resourcegebruik en prestaties.

![Partition-indeling met vier knooppunten](./media/service-fabric-concepts-scalability/layout-four-nodes.png)

## <a name="scaling-at-the-service-name-level"></a>Schaalbaarheid op het niveau van de naam van service
Een exemplaar van de service is een specifiek exemplaar van de naam van een toepassing en de naam van een service-type (Zie [Service stof de levenscyclus van toepassing](service-fabric-application-lifecycle.md)). Tijdens het maken van een service, die u opgeeft de partition (Zie [Service-configuratieservices partitioneren](service-fabric-concepts-partitioning.md)) moet worden gebruikt in kleurenschema.

Het eerste niveau van de schaalbaarheid is door de servicenaam van de. U kunt nieuwe exemplaren van een service, maken met verschillende detailniveaus partitioneren, als uw oudere service-sessies worden bezet. Hiermee kunt nieuwe service consumenten minder bezet-service exemplaren, in plaats van veelgebruikte bestanden gebruiken.

Eén optie voor het vergroten van capaciteit, evenals toeneemt of degressieve partition tellingen komen, is het opzetten van een nieuw exemplaar van de service met een nieuwe partition kleurenschema. Hiermee voegt u complexiteit, echter als in beslag nemen clients moeten weten wanneer en hoe de anders benoemde service wilt gebruiken.

### <a name="example-scenario-embedded-dates"></a>Voorbeeldscenario: ingesloten van datums
Een voorbeeldscenario zou u datuminformatie gebruiken als onderdeel van de servicenaam van de. Bijvoorbeeld: u kunt een exemplaar van de service met een specifieke naam voor alle klanten die die zijn gekoppeld in 2013 en een andere naam voor klanten die die zijn gekoppeld in 2014. In dit schema voor naamgeving kan voor via programmacode met groter wordende de namen afhankelijk van de datum (2014 nadert, het exemplaar van de service voor 2014 kan worden gemaakt op aanvraag).

Deze benadering is echter gebaseerd op de clients toepassingsspecifieke naming gegevens die zich buiten het bereik van Service stof knowledge te gebruiken.

- *Een naamgevingsconventie gebruiken*: In 2013, wanneer uw toepassing live gaat, maakt u een service stof genoemd: / app/service2013. In de buurt van de tweede kwartaal van 2013, die u maakt een andere service, stof genoemd: / app/service2014. Beide services zijn van hetzelfde servicetype. In deze benadering moet de klant logica om de naam van de desbetreffende service op basis van het jaar te gebruiken.

- *Gebruik een lookup-service*: een ander patroon is bedoeld als een secundaire lookup-service, die de naam van de service voor een gewenste toets kunt bieden. Nieuwe exemplaren van de service kunnen worden gemaakt door de service opzoeken. De lookup-service zelf behouden niet toepassingsgegevens, alleen gegevens over de servicenamen die wordt gemaakt. Dus voor het jaar gebaseerde bovenstaande voorbeeld zou de client eerst contact opnemen met de service lookup om vast te stellen de naam van de service verwerking van gegevens voor een bepaald jaar en gebruik vervolgens de naam van die service voor de uitvoering van de werkelijke bewerking. Het resultaat van de eerste zoeken kan worden opgeslagen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service stof concepten:

- [Beschikbaarheid van Service configuratieservices](service-fabric-availability-services.md)

- [Service configuratieservices partitioneren](service-fabric-concepts-partitioning.md)

- [Definiëren en staat beheren](service-fabric-concepts-state.md)
