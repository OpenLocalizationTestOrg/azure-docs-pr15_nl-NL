<properties
   pageTitle="Betrouwbare betrokkenen notities acteur Typ serialisatie | Microsoft Azure"
   description="Wordt beschreven hoe de basisvereisten voor serialiseerbaar klassen die kunnen worden gebruikt om te definiëren Service stof betrouwbare betrokkenen Staten en interfaces definiëren"
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
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Notities op uw Service stof betrouwbare betrokkenen Typ serialisatie


De argumenten van alle methoden, resultaattypen van de taken die het resultaat van elke methode in een acteur-interface en objecten die zijn opgeslagen in van een acteur staat Manager moeten [Gegevenscontract serialiseerbaar](https://msdn.microsoft.com/library/ms731923.aspx)... Dit geldt ook voor de argumenten van de methoden gedefinieerd in [acteur gebeurtenisinterfaces](service-fabric-reliable-actors-events.md#actor-events). (Acteur gebeurtenis interfacemethoden altijd void retourneren.)

## <a name="custom-data-types"></a>Aangepaste gegevenstypen

In dit voorbeeld de volgende acteur-interface dit is een methode die resulteert in een aangepast gegevenstype genoemd `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

De interface is impelemented door een acteur, waarbij de Manager staat voor het opslaan van een `VoicemailBox` object:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

In dit voorbeeld de `VoicemailBox` object serienummer wanneer:
 - Het object wordt verzonden tussen een acteur-instantie en een beller.
 - Het object wordt opgeslagen in de Manager staat waar het op schijf vastgelegd en gerepliceerd naar andere knooppunten.
 
Het framework betrouwbare acteur gebruikt DataContract serialisatie. Daarom moeten de aangepaste gegevens-objecten en hun leden worden gemarkeerd met de kenmerken **DataContract** en **DataMember** respectievelijk

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Volgende stappen
 - [Acteur levenscyclus en ongewenste siteverzameling](service-fabric-reliable-actors-lifecycle.md)
 - [Acteur timers en herinneringen](service-fabric-reliable-actors-timers-reminders.md)
 - [Acteur gebeurtenissen](service-fabric-reliable-actors-events.md)
 - [Acteur herintreding](service-fabric-reliable-actors-reentrancy.md)
 - [Acteur polymorfisme en patronen object-georiënteerd ontwerpen](service-fabric-reliable-actors-polymorphism.md)
 - [Acteur diagnostisch hulpprogramma en prestaties controleren](service-fabric-reliable-actors-diagnostics.md)
