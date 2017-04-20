<properties
   pageTitle="Betrouwbare betrokkenen gebeurtenissen | Microsoft Azure"
   description="Inleiding tot gebeurtenissen voor Service stof betrouwbare betrokkenen."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Acteur gebeurtenissen
Acteur gebeurtenissen zelf een formule efficiënt mogelijk meldingen van de acteur naar de clients verzenden. Acteur gebeurtenissen zijn ontworpen voor communicatie van acteur-naar-client en mag niet worden gebruikt voor communicatie van acteur-naar-acteur.

De volgende codefragmenten hoe acteur gebeurtenissen in uw toepassing gebruiken.

Een interface die goed de gebeurtenissen die zijn gepubliceerd door de acteur beschrijft definiëren. Deze interface moet zijn afgeleid van de `IActorEvents` interface. De argumenten van de methoden moeten [gegevens de contracten serialiseerbaar](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). De methoden moeten void retourneren, als de gebeurtenis meldingen zijn een manier en aanbevolen moeite.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

De gebeurtenissen die zijn gepubliceerd door de acteur in de interface van acteur declareren.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Aan de clientzijde, implementeer de gebeurtenis-handler.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Op de client maken van een proxy naar de acteur waarmee de gebeurtenis worden gepubliceerd en u hierop abonneren gebeurtenissen.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

In het geval van failovers mislukken de acteur via de knooppunten of een ander proces. De acteur-proxy beheert de actieve abonnementen en automatisch opnieuw afsluit ze. U kunt bepalen het abonnementinterval opnieuw via de `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Als u wilt opzeggen, gebruikt u de `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Klik op de acteur, gewoon publiceren de gebeurtenissen terwijl deze worden aangebracht. Als er abonnees van de gebeurtenis, stuurt de runtime betrokkenen ze de melding.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Volgende stappen
 - [Acteur herintreding](service-fabric-reliable-actors-reentrancy.md)
 - [Acteur diagnostisch hulpprogramma en prestaties controleren](service-fabric-reliable-actors-diagnostics.md)
 - [Acteur API-documentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Voorbeeld van een code](https://github.com/Azure/servicefabric-samples)
