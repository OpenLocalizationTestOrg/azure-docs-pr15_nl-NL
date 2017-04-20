<properties
   pageTitle="Aan de slag met Service stof betrouwbare betrokkenen | Microsoft Azure"
   description="Deze zelfstudie leert u de stappen voor het maken en implementeren van een eenvoudige acteur gebaseerde service met Service stof betrouwbare betrokkenen voor foutopsporing in."
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
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Aan de slag met betrouwbare betrokkenen

> [AZURE.SELECTOR]
- [C# in Windows](service-fabric-reliable-actors-get-started.md)
- [Java op Linux](service-fabric-reliable-actors-get-started-java.md)

In dit artikel worden de basisprincipes van Azure-Service stof betrouwbare betrokkenen uitgelegd en leert u via maken en implementeren van een eenvoudige betrouwbare acteur-toepassing in Visual Studio voor foutopsporing in.

## <a name="installation-and-setup"></a>Installatie en configuratie
Voordat u begint, moet u zorgen dat u de Service stof ontwikkelomgeving instellen op uw computer hebt.
Als u instellen wilt, raadpleegt u uitgebreide instructies over [het instellen van de ontwikkelomgeving](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Basisbegrippen
Als u wilt beginnen met betrouwbare betrokkenen, moet u alleen een paar eenvoudige basisbegrippen:

 * **Acteur-service**. Betrouwbare betrokkenen worden in betrouwbare Services die kan worden geïmplementeerd in de infrastructuur Service stof geleverd. Acteur exemplaren worden in een exemplaar van benoemde service geactiveerd.
 
 * **Registratie van acteur**. Met betrouwbare Services moet een betrouwbare acteur-service te worden geregistreerd bij de Service stof runtime. Het type acteur moet bovendien worden geregistreerd bij de acteur-runtime.
 
 * **Acteur-interface**. De acteur-interface wordt gebruikt voor het definiëren van een sterk getypte openbare interface van een acteur. In de betrouwbare acteur model terminologie bepaalt de acteur-interface de soorten berichten die de acteur kan begrijpen en proces. De acteur-interface wordt gebruikt door andere betrokkenen en clienttoepassingen "" (asynchroon) om berichten te verzenden naar de acteur. Betrouwbare betrokkenen kunnen meerdere interfaces implementeren.
 
 * **ActorProxy class**. De klasse ActorProxy wordt gebruikt door clienttoepassingen naar methoden weergegeven via de acteur-interface. De klasse ActorProxy biedt twee belangrijke functies:
    * Geef een naam resolutie: kan de acteur Zoek in het cluster (zoeken het knooppunt van de cluster waar deze wordt gehost).
    * Fout bij afhandeling: kan deze methode aanroepen opnieuw en opnieuw de locatie van acteur oplossen na, bijvoorbeeld een storing waarvoor de acteur moet worden verplaatst naar een ander knooppunt in het cluster.

De volgende regels die betrekking op acteur-interfaces hebben zijn vermelden we:

- Acteur Interfacemethoden kunnen niet worden overladen.
- Acteur-interface methoden mag geen af, ref of optionele parameters.
- Algemene interfaces worden niet ondersteund.

## <a name="create-a-new-project-in-visual-studio"></a>Een nieuw project maakt in Visual Studio
Nadat u de Service stof hulpprogramma's voor Visual Studio hebt geïnstalleerd, kunt u nieuwe projecttypen kunt maken. De nieuwe projecttypen zijn in de **Cloud** -categorie in het dialoogvenster **Nieuw Project** .


![Service stof tools voor Visual Studio - nieuw project][1]

Klik in het volgende dialoogvenster kunt u het type project dat u wilt maken.

![Service stof project-sjablonen][5]

Voor het project Hallo wereld gebruiken we de service-Service stof betrouwbare betrokkenen.

Nadat u de oplossing hebt gemaakt, ziet u de volgende structuur:

![Service stof projectstructuur][2]

## <a name="reliable-actors-basic-building-blocks"></a>Betrouwbare betrokkenen bouwstenen

Een typische betrouwbare betrokkenen-oplossing bestaat uit drie projecten:

* **De toepassingsproject (MyActorApplication)**. Dit is het project dat alle services samen voor implementatie-pakketten. De presentatie bevat de *ApplicationManifest.xml* en PowerShell-scripts voor het beheer van de toepassing.

* **De interface-project (MyActor.Interfaces)**. Dit is het project dat de interfacedefinitie voor de acteur bevat. In het project MyActor.Interfaces, kunt u de interfaces die worden gebruikt door de betrokkenen in de oplossing definiëren. Uw acteur-interfaces kunnen worden gedefinieerd in een project met een naam, maar de interface definieert het contract acteur die wordt gedeeld door de acteur-implementatie en de clients bellen van de acteur, dus dat meestal relevant is het definiëren van een constructie die losstaat van de acteur-implementatie en kan worden gedeeld door meerdere andere projecten.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Het project in de acteur-service (MyActor)**. Dit is het project gebruikt om te definiëren van de Service stof-service die u wilt de acteur hosten. De presentatie bevat de uitvoering van de acteur. Een acteur-implementatie is een klasse die afkomstig van het grondtal type is `Actor` en de interface (s) die zijn gedefinieerd in het project MyActor.Interfaces implementeert. Een klasse acteur moet ook implementeren voor een constructor waarin een `ActorService` exemplaar en een `ActorId` en geeft u deze naar het grondtal `Actor` class. Hiermee constructor afhankelijkheid webweergave van platform afhankelijkheden.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

De acteur-service moet zijn geregistreerd bij een servicetype in de Service stof runtime. In de volgorde voor de Service acteur uw acteur-sessies uitvoeren, moet het type acteur ook zijn geregistreerd met de acteur-Service. De `ActorRuntime` registratiemethode dit werk voor betrokkenen uitvoert.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Als u vanuit een nieuw project in Visual Studio starten en er slechts één acteur definitie, is de registratie standaard opgenomen in de code die Visual Studio gegenereerd. Als u andere betrokkenen in de service definieert, moet u de registratie acteur toevoegen met behulp van:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [AZURE.TIP] De Service stof betrokkenen runtime genereert sommige [evenementen en van prestatiemeteritems die zijn gerelateerd aan acteur methoden](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Ze zijn handig in diagnostisch hulpprogramma en prestatiecontroles uit.


## <a name="debugging"></a>Voor foutopsporing in

De Service stof tools voor Visual Studio ondersteuning voor foutopsporing op uw lokale computer. U kunt een sessie voor foutopsporing starten kunt u door op de toets F5. Visual Studio genereert (indien nodig) pakketten. Dit wordt ook de toepassing op de lokale Service stof cluster geïmplementeerd en de foutopsporing koppelen.

Tijdens de implementatie, kunt u de voortgang in **het uitvoervenster** zien.

![Service stof foutopsporing uitvoervenster][3]


## <a name="next-steps"></a>Volgende stappen
 - [Hoe betrouwbare betrokkenen het platform Service stof gebruiken](service-fabric-reliable-actors-platform.md)
 - [Beheer van acteur staat](service-fabric-reliable-actors-state-management.md)
 - [Acteur levenscyclus en ongewenste siteverzameling](service-fabric-reliable-actors-lifecycle.md)
 - [Acteur API-documentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Voorbeeld van een code](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
