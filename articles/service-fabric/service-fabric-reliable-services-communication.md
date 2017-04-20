<properties
   pageTitle="Overzicht van de communicatie betrouwbare Services | Microsoft Azure"
   description="Overzicht van het betrouwbare Services communicatiemodel, waaronder openen listeners op services, eindpunten oplossen en communicatie tussen services."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-to-use-the-reliable-services-communication-apis"></a>Het gebruik van de communicatie betrouwbare Services API 's

Azure-Service stof als een platform is volledig agnostische over de communicatie tussen services. Alle protocollen en stapels aanvaardbaar van UDP HTTP kunnen zijn. Is het aan de ontwikkelaar service kiezen hoe services moeten communiceren. Het kader van de toepassing betrouwbare Services biedt ingebouwde communicatie stapels, evenals API's die u gebruiken kunt om te maken van uw aangepaste communicatie-onderdelen. 

## <a name="set-up-service-communication"></a>Service communicatie instellen

De betrouwbare Services-API gebruikt een eenvoudige interface voor communicatie van de service. U opent een eindpunt voor uw service door gewoon implementeert deze interface:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Vervolgens kunt u uw implementatie van de luisteraar ervan af communicatie toevoegen door deze in een service gebaseerde klasse methode overschrijven te retourneren.

Voor stateless services:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Voor statuscontrole services:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

In beide gevallen kunt u een verzameling listeners retourneren. Hiermee kunt uw service voor meerdere eindpunten, mogelijk met verschillende protocollen, met behulp van meerdere listeners luisteren. Zoals wellicht u een HTTP luisteraar ervan af en een afzonderlijke WebSocket luisteraar ervan af. Elke luisteraar ervan af krijgt een naam en de resulterende verzameling *naam: adres* paren wordt weergegeven als een object JSON wanneer een client het luisteren adressen voor een exemplaar van service of een partition aanvraagt.

In een stateless service levert het overschrijven een verzameling ServiceInstanceListeners. Een ServiceInstanceListener bevat een functie als u wilt maken van een ICommunicationListener en hieraan een naam. Statuscontrole-services, de overschrijven geeft als resultaat een verzameling ServiceReplicaListeners. Dit is iets anders dan de stateless tegenhanger, omdat een ServiceReplicaListener een optie heeft voor het openen van een ICommunicationListener op secundaire replica's. Niet alleen kunt u meerdere communicatie listeners gebruiken in een service, maar u kunt ook opgeven welke listeners accepteren aanvragen op secundaire replica's en de functies die alleen op de primaire replica luistert.

Bijvoorbeeld, kunt u beschikken over een ServiceRemotingListener waarmee RPC oproepen alleen op primaire replica's en een tweede, aangepaste luisteraar ervan af die gelezen wordt verzoeken op secundaire replica's via HTTP:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Wanneer u meerdere listeners voor een service maakt, worden elke luisteraar ervan af **moet** een unieke naam opgegeven.

Tot slot beschrijven de eindpunten die vereist voor de service in de [service bestandenlijst](service-fabric-application-model.md) onder het gedeelte op de eindpunten zijn.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

De communicatie luisteraar ervan af toegang tot het eindpunt bronnen toegewezen aan deze uit de `CodePackageActivationContext` in de `ServiceContext`. De luisteraar ervan af kunt start luisteren naar aanvragen wanneer dit wordt geopend.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Eindpunt resources gelden voor de hele servicepakket en ze door de Service stof worden toegewezen wanneer het servicepakket is geactiveerd. Meerdere service replica's die worden gehost in de dezelfde ServiceHost mogelijk dezelfde poort delen. Dit betekent dat de communicatie luisteraar ervan af moet ondersteuning voor poort delen. De aanbevolen manier is voor de communicatie luisteraar ervan af de partition-ID en replica/exemplaar-ID gebruiken wanneer het beluisteren van adres wordt gegenereerd.

### <a name="service-address-registration"></a>De service-mailadres is geregistreerd

Een systeemservice genoemd de *Naming Service* wordt uitgevoerd op de Service stof clusters. De Naming Service is een registrar voor services en hun adres dat elk exemplaar of replica van de service luistert op. Wanneer de `OpenAsync` methode van een `ICommunicationListener` is voltooid, wordt de return-waarde in de Naming Service wordt geregistreerd. Deze waarde die wordt gepubliceerd in de Naming Service is een tekenreeks waarvan de waarde helemaal kan zijn. Deze tekenreekswaarde is wat clients te zien krijgen wanneer ze om een adres voor de service uit de Naming Service vragen.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Service stof biedt API's waarmee clients en andere services kunnen vervolgens vragen om dit adres door de servicenaam van de. Dit is belangrijk omdat het serviceadres niet statische is. Services worden verplaatst rond in het cluster voor resource taakverdeling en beschikbaarheid van de toepassing. Dit is de methode waarmee clients voor het oplossen van het luisteren adres van een service.

> [AZURE.NOTE] Voor een volledige overzicht van hoe u schrijft een `ICommunicationListener`, Zie [Service stof Web API-services met OWIN selfservice hostingprovider](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Communiceren met een service
De betrouwbare Services-API biedt de volgende bibliotheken voor het schrijven van clients die met services communiceren.

### <a name="service-endpoint-resolution"></a>Service-eindpunt resolutie
De eerste stap bij communicatie met een service is voor het oplossen van een eindpuntadres van de partition of exemplaar van de service die u wilt praten. De `ServicePartitionResolver` hulpprogrammaklasse is een eenvoudige primitief waarmee klanten het eindpunt van een service gedurende runtime bepalen. In de Service stof terminologie wordt het proces voor het bepalen van het eindpunt van een service de *resolutie van de service-eindpunt*genoemd.

Verbinding maken met services binnen een cluster, `ServicePartitionResolver` kan worden gemaakt met behulp van de standaardinstellingen. Dit is het aanbevolen gebruik voor de meeste gevallen:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Verbinding maken met services in een ander cluster, een `ServicePartitionResolver` kan worden gemaakt met een set cluster gateway eindpunten. U ziet dat gateway eindpunten alleen verschillende eindpunten om verbinding te maken naar hetzelfde cluster zijn. Bijvoorbeeld:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

U kunt ook `ServicePartitionResolver` kan een functie worden opgegeven voor het maken van een `FabricClient` intern gebruiken: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`is het object dat wordt gebruikt om te communiceren met verschillende management bewerkingen voor het cluster van de Service stof cluster. Dit is handig als u meer controle over hoe u wilt `ServicePartitionResolver` samenwerkt met uw cluster. `FabricClient`voert intern caching en is gewoonlijk dure maken, dus is het belangrijk om opnieuw `FabricClient` exemplaren zo veel mogelijk. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Een methode oplossen wordt vervolgens gebruikt om op te halen, het adres van een service of een service partition voor gepartitioneerde services.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Een serviceadres kan worden opgelost eenvoudig met een `ServicePartitionResolver`, maar meer werk nodig is om de opgelost adres correct kan worden gebruikt. De klant nodig hebt om te bepalen of de poging is mislukt vanwege een tijdelijke fout en opnieuw kan worden verzonden (bijvoorbeeld service verplaatst of is tijdelijk niet beschikbaar is), of een permanente fout (bijvoorbeeld service is verwijderd of de gevraagde resource niet langer bestaat). Service-exemplaren of replica's kunnen navigeren vanaf knooppunt naar knooppunt op elk gewenst moment om verschillende redenen. De serviceadres herleid tot en met `ServicePartitionResolver` is mogelijk verlopen op het moment dat uw clientcode probeert verbinding te maken. In dat geval opnieuw moet de client het adres te zetten. De vorige leveren `ResolvedServicePartition` wordt aangegeven dat de resolvercache moet probeer het opnieuw in plaats van gewoon ophalen een adres in cache.

Meestal de clientcode nodig werkt niet met de `ServicePartitionResolver` rechtstreeks. Het is gemaakt en aan communicatie client factory's in de betrouwbare Services-API doorgegeven. De factory's gebruiken de resolvercache intern te genereren van een client-object die kan worden gebruikt om te communiceren met services.

### <a name="communication-clients-and-factories"></a>Communicatie-mailclients en bedrijven

De communicatie factory-bibliotheek implementeert een typisch foutenstructuuranalyse foutafhandeling opnieuw patroon die bezig met opnieuw verbindingen met opgelost service eindpunten vergemakkelijkt. De factory-bibliotheek bevat het opnieuw om terwijl u de fout handlers opgeeft.

`ICommunicationClientFactory`Hiermee definieert u de basisinterface geïmplementeerd door een communicatie-client factory die clients die naar een service-Service stof kunnen praten oplevert. De implementatie van de CommunicationClientFactory, is afhankelijk van de communicatie stapel gebruikt door de service-Service stof waar de klant wil communiceren. De betrouwbare Services-API biedt een `CommunicationClientFactoryBase<TCommunicationClient>`. Dit biedt een grondtal implementatie van de `ICommunicationClientFactory` interface en taken die voor alle communicatie-stapels gelden uitgevoerd. (Deze taken omvatten het gebruik van een `ServicePartitionResolver` om te bepalen de service-eindpunt). Klanten wordt meestal de abstracte CommunicationClientFactoryBase-klasse om af te handelen logica die specifiek is voor de communicatie-stack implementeren.

De communicatie-client een adres ontvangt alleen en deze verbinding maakt met een service. De client kunt ongeacht protocol deze wil gebruiken.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

De client-fabriek is primair verantwoordelijk voor het maken van communicatie-clients. Voor clients die niet voor het behoud van een permanente verbinding, zoals een HTTP-client, hoeft de fabriek alleen te maken en de client terug te keren. Andere protocollen die voor het behoud van een permanente verbinding, zoals sommige binaire protocollen, moeten ook worden gevalideerd door de factory om te bepalen of de verbinding moet opnieuw worden gemaakt.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Tot slot is een uitzonderingshandler belast vaststellen welke actie moet worden uitgevoerd wanneer een uitzondering voordoet. Uitzonderingen zijn ingedeeld in **retriable** en **niet retriable**. 

 - **Niet retriable** uitzonderingen krijgen gewoon opnieuw gegenereerd terug naar de beller. 
 - **Retriable** uitzonderingen kunnen verder worden onderverdeeld in **tijdelijke** en **tijdelijke**.
  - **Tijdelijke** uitzonderingen zijn die opnieuw kunnen eenvoudig worden verzonden zonder opnieuw oplossen de eindpuntadres van service. Deze bevatten tijdelijke problemen met het netwerk of service fout antwoorden die aangeven dat de eindpuntadres van service niet bestaat. 
  - **Niet-tijdelijk** uitzonderingen zijn die de eindpuntadres van service worden opnieuw omgezet vereisen. Hierbij uitzonderingen die aangeven dat het service-eindpunt kan niet worden bereikt, is waarin wordt aangegeven dat de service verplaatst naar een ander knooppunt. 

De `TryHandleException` zorgt ervoor dat een beslissing over een opgegeven uitzondering. Als deze **niet weet** wat u over een uitzondering beslissingen, moeten worden geretourneerd **Onwaar**. Als deze **weet** wat besluit om te maken, moet deze het resultaat tijdzone instellen en terug te keren **waar**.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Alles samenbrengen
Met een `ICommunicationClient`, `ICommunicationClientFactory`, en `IExceptionHandler` gebaseerd op een communicatieprotocol, een `ServicePartitionClient` is deze helemaal terugloopt en bevat de foutenstructuuranalyse foutafhandeling en service partition adres resolutie lus om deze onderdelen.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Volgende stappen
 - Zie een voorbeeld van HTTP communicatie tussen services in een [voorbeeld van project op GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [RPC met betrouwbare Services externe toegang](service-fabric-reliable-services-communication-remoting.md)

 - [Web API die gebruikmaakt van OWIN in betrouwbare Services](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-communicatie met betrouwbare Services](service-fabric-reliable-services-communication-wcf.md)
