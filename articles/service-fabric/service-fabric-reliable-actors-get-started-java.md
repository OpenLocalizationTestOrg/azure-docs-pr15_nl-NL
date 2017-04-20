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
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Aan de slag met betrouwbare betrokkenen

> [AZURE.SELECTOR]
- [C# in Windows](service-fabric-reliable-actors-get-started.md)
- [Java op Linux](service-fabric-reliable-actors-get-started-java.md)

In dit artikel worden de basisprincipes van Azure-Service stof betrouwbare betrokkenen uitgelegd en leert u via maken en implementeren van een eenvoudige betrouwbare acteur-toepassing in Java.

## <a name="installation-and-setup"></a>Installatie en configuratie
Controleer voordat u begint, of dat u de Service stof ontwikkelomgeving instellen op uw computer hebt.
Als u instellen wilt, gaat u naar [aan de slag op Mac](service-fabric-get-started-mac.md) of [aan de slag op Linux](service-fabric-get-started-linux.md).

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

## <a name="create-an-actor-service"></a>Maak een acteur-service
Beginnen met het maken van een nieuwe Service stof-toepassing. De Service stof SDK voor Linux bevat een Yeoman genereren op te geven van de steiger voor een toepassing voor de Service stof met een stateless-service. Start met de volgende Yeoman opdracht:

```bash
$ yo azuresfjava
```

Volg de instructies voor het maken van een **Betrouwbare acteur-Service**. De toepassing 'HelloWorldActorApplication' een naam voor deze zelfstudie en de acteur "HelloWorldActor." De volgende steiger worden gemaakt:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Betrouwbare betrokkenen bouwstenen

De basisbegrippen hierboven vertalen in de bouwstenen van een betrouwbare acteur-service.

### <a name="actor-interface"></a>Acteur-interface

De interfacedefinitie voor de acteur bevat deze optie. Deze interface definieert het contract acteur die wordt gedeeld door de acteur-implementatie en de clients bellen van de acteur, zodat dit meestal relevant is deze op een plaats die losstaat van de uitvoering acteur definiëren en kan worden gedeeld door meerdere andere services of clienttoepassingen.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Acteur-service 
Dit document bevat uw acteur-implementatie en registratiecode acteur. De klas acteur implementeert de acteur-interface. Dit is waar uw acteur zijn werk doet.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Registratie van acteur

De acteur-service moet zijn geregistreerd bij een servicetype in de Service stof runtime. In de volgorde voor de Service acteur uw acteur-sessies uitvoeren, moet het type acteur ook zijn geregistreerd met de acteur-Service. De `ActorRuntime` registratiemethode dit werk voor betrokkenen uitvoert.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Testclient

Dit is een eenvoudige test-clienttoepassing afzonderlijk van de configuratietoepassing Service stof Test uw acteur-service kan worden uitgevoerd. Dit is een voorbeeld van waar de ActorProxy kan worden gebruikt om te activeren en te communiceren met acteur exemplaren. Het is niet geïmplementeerd met uw service.

### <a name="the-application"></a>De toepassing 

De toepassing verpakt ten slotte de acteur-service en eventuele andere services die u in de toekomst samen voor implementatie toevoegen kunt. De presentatie bevat de houders *ApplicationManifest.xml* en locatie voor het pakket van acteur-service.

## <a name="run-the-application"></a>Voer de toepassing

De Yeoman steigers bevat een gradle-script om de toepassing bouwen en bash scripts om te implementeren en schakel-de-toepassing implementeren. Als u wilt de toepassing uitvoert, moet u eerst de toepassing met gradle maken:

```bash
$ gradle
```

Deze zullen Service stof toepassingspakket dat kan worden geïmplementeerd met Service stof Azure CLI produceren. Het script install.sh bevat Azure CLI opdrachten voor het implementeren van de toepassingspakket. Gewoon uitvoeren de install.sh-script om te implementeren:

```bask
$ ./install.sh
```
