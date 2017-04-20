<properties
   pageTitle="Aan de slag met betrouwbare Services | Microsoft Azure"
   description="Inleiding tot het maken van een toepassing voor de Microsoft Azure-Service stof met stateless en statuscontrole-services."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Aan de slag met betrouwbare Services

> [AZURE.SELECTOR]
- [C# in Windows](service-fabric-reliable-services-quick-start.md)
- [Java op Linux](service-fabric-reliable-services-quick-start-java.md)

In dit artikel worden de basisprincipes van Azure-Service betrouwbare configuratieservices uitgelegd en leert u via maken en implementeren van een eenvoudige betrouwbare servicetoepassing geschreven in Java.

## <a name="installation-and-setup"></a>Installatie en configuratie
Controleer voordat u begint, of dat u de Service stof ontwikkelomgeving instellen op uw computer hebt.
Als u instellen wilt, gaat u naar [aan de slag op Mac](service-fabric-get-started-mac.md) of [aan de slag op Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Basisbegrippen
Als u wilt beginnen met betrouwbare Services, moet u alleen een paar eenvoudige basisbegrippen:

 - **Servicetype**: dit is uw service-implementatie. Deze is gedefinieerd door de klasse u schrijft die uitbreidt `StatelessService` en eventuele andere code of afhankelijkheden die, samen met een naam en een versienummer.

 - **Benoemde service exemplaar**: als u wilt uitvoeren van uw service, u maakt benoemde exemplaren van het servicetype net zoals u objectexemplaren van een type class maakt. Service-exemplaren zijn in feite object exemplaren van uw klas service die u schrijft. 

 - **Service-host**: de benoemde service-exemplaren die u wilt uitvoeren in een host. De ServiceHost is alleen een proces waar exemplaren van uw service kunnen worden uitgevoerd.

 - **De service is geregistreerd**: registratie alles bij elkaar brengt. Het servicetype moet zijn geregistreerd bij de Service stof runtime in een ServiceHost toe te staan dat Service stof exemplaren van deze maken om uit te voeren.  

## <a name="create-a-stateless-service"></a>Een stateless service maken

Beginnen met het maken van een nieuwe Service stof-toepassing. De Service stof SDK voor Linux bevat een Yeoman genereren op te geven van de steiger voor een toepassing voor de Service stof met een stateless-service. Start met de volgende Yeoman opdracht:

```bash
$ yo azuresfjava
```

Volg de instructies voor het maken van een **Betrouwbare Stateless Service**. Voor deze zelfstudie een naam geven de toepassing "HelloWorldApplication" en "Hallo, wereld" van de service. Het resultaat bevat mappen voor de `HelloWorldApplication` en `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>De service implementeren

Open **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Deze klasse definieert het servicetype en een code, die kan worden uitgevoerd. De service-API biedt twee punten van de invoer voor uw code:

 - Een open punt invoermethode, genaamd `runAsync()`, waar u het uitvoeren van elke werkbelastingen, inclusief langdurige berekeningscluster werkbelasting kunt beginnen.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - Een communicatie-ingangspunt waar u de stapel communicatie keuze kunt aansluiten. Dit is waar u kunt beginnen ontvangt aanvragen van gebruikers en andere services.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

In deze zelfstudie gaan we in op de `runAsync()` invoermethode punt. Dit is waar u kunt direct begint u uw code uitvoert.

### <a name="runasync"></a>RunAsync

Het platform oproepen deze methode wanneer er een exemplaar van een service worden geplaatst en kan worden uitgevoerd. De cyclus openen/sluiten van een exemplaar van service kan zich voordoen vaak in de loop van de service als geheel. Dit kan gebeuren om verschillende redenen, waaronder:

- Uw service-sessies voor het verdelen van resource wordt verplaatst door het systeem.
- Fouten die voorkomen in uw code.
- De toepassing of systeem is bijgewerkt.
- De onderliggende hardware ervaringen een storing.

Deze situatie wordt beheerd door de Service stof wilt behouden van uw service ten zeerste beschikbaar en goed gebalanceerde.

#### <a name="cancellation"></a>Annulering

Het is belangrijk dat uw code in `runAsync()` worden uitgevoerd wanneer een melding ontvangen door de Service stof kunt stoppen. De `CompletableFuture` geretourneerd uit `runAsync()` wordt geannuleerd als Service stof moet uw service niet meer kan worden uitgevoerd. Het volgende voorbeeld ziet u hoe u omgaat met een gebeurtenis annulering: 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>De service is geregistreerd

Servicetypen moeten worden geregistreerd met de Service stof runtime. Het servicetype is gedefinieerd de `ServiceManifest.xml` en uw serviceklasse die implementeert `StatelessService`. Registratie van de service wordt uitgevoerd in het proces hoofdvermelding punt. In dit voorbeeld de proces hoofdvermelding komma is `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Voer de toepassing

De Yeoman steigers bevat een gradle-script om de toepassing bouwen en bash scripts om te implementeren en schakel-de-toepassing implementeren. Als u wilt de toepassing uitvoert, moet u eerst de toepassing met gradle maken:

```bash
$ gradle
```

Deze zullen Service stof toepassingspakket dat kan worden geïmplementeerd met Service stof Azure CLI produceren. Het script install.sh bevat Azure CLI opdrachten voor het implementeren van de toepassingspakket. Gewoon uitvoeren de install.sh-script om te implementeren:

```bask
$ ./install.sh
```
