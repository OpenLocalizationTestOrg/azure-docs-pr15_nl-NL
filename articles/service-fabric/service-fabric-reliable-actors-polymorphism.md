<properties
   pageTitle="Polymorfisme in het kader betrouwbare betrokkenen | Microsoft Azure"
   description="Hiërarchieën van .NET-interfaces en typen in het kader betrouwbare betrokkenen functionaliteit en API definities opnieuw te maken."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polymorfisme in het kader betrouwbare betrokkenen

Het kader betrouwbare betrokkenen kunt u betrokkenen met veel van dezelfde technieken die u in het ontwerp van object-georiënteerd gebruiken wilt maken. Een van deze technieken polymorfisme, waarmee typen en -interfaces voor het overnemen van meer is generalized ouders. Overname in het kader betrouwbare betrokkenen volgt doorgaans het model .NET met een paar extra beperkingen.

## <a name="interfaces"></a>Interfaces

Het kader betrouwbare betrokkenen, moet u ten minste één interface om te worden uitgevoerd door het type acteur definiëren. Deze interface wordt gebruikt om een proxyklasse die kan worden gebruikt door clients om te communiceren met uw betrokkenen. Interfaces kunnen overnemen van andere interfaces zo lang maken als elke interface die wordt geïmplementeerd op een type acteur en alle bijbehorende bovenliggende items uiteindelijk zijn afgeleid van IActor. IActor is de platform gedefinieerde basisinterface voor betrokkenen. Dus kan het klassieke polymorfisme voorbeeld met behulp van shapes er ongeveer zo uitzien:

![Hiërarchie van de gebruikersinterface voor vorm betrokkenen][shapes-interface-hierarchy]


## <a name="types"></a>Typen

U kunt ook een hiërarchie met acteur typen, die zijn afgeleid van de basis acteur-klasse die is verstrekt door het platform maken. In het geval van shapes, hebt u een basis `Shape` type:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Subtypes van `Shape` methoden van het grondtal kunt negeren.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Opmerking de `ActorService` kenmerk van het type acteur. Dit kenmerk Hiermee wordt aan het betrouwbare acteur-kader dat een service voor het hosten van betrokkenen van dit type automatisch moeten worden gemaakt. In sommige gevallen wilt u mogelijk een basistoewijzing type dat is alleen bedoeld voor het delen van functionaliteit met subtypen en nooit worden gebruikt voor het starten van Betonnen betrokkenen maakt. In dat geval moet u de `abstract` trefwoord om aan te geven dat u nooit een acteur op basis van dat type wilt maken.


## <a name="next-steps"></a>Volgende stappen

- Zie [hoe het platform Service stof worden gebruikt in het kader betrouwbare betrokkenen](service-fabric-reliable-actors-platform.md) betrouwbaarheid, schaalbaarheid en consistente status op te geven.
- Meer informatie over de [levenscyclus van acteur](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
