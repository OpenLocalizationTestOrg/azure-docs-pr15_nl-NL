<properties
   pageTitle="Betrouwbare WCF-Services communicatie stapel | Microsoft Azure"
   description="De ingebouwde WCF communicatie stack in Service stof biedt client-service WCF communicatie voor betrouwbare Services."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# <a name="wcf-based-communication-stack-for-reliable-services"></a>WCF-communicatie stapel voor betrouwbare Services
Het kader betrouwbare Services kan auteurs service kiest u de stapel communicatie die ze willen gebruiken voor de service. Ze kunnen de stapel communicatie van hun keuze via de **ICommunicationListener** geretourneerd uit de methoden [CreateServiceReplicaListeners of CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) aansluiten. Het kader biedt een implementatie van de stapel communicatie is gebaseerd op de Windows Communication Foundation (WCF) voor de service makers wilt WCF-communicatie gebruiken.

## <a name="wcf-communication-listener"></a>WCF communicatie luisteraar ervan af
Het WCF-specifieke implementatie van **ICommunicationListener** is opgegeven door de klasse **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** .

Lest Stel dat we een opdracht van het type hebben`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Een WCF-communicatie luisteraar ervan af kunnen we maken in de service de volgende manier.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>Clients voor de stapel WCF-communicatie schrijven
Voor het schrijven van klanten om te communiceren met services met behulp van WCF, vindt u in het kader **WcfClientCommunicationFactory**, dat wil de WCF-specifieke uitvoering van [ClientCommunicationFactoryBase zeggen](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

Het WCF-communicatiekanaal zijn toegankelijk vanaf de **WcfCommunicationClient** gemaakt door de **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Clientcode de beschikking over de **WcfCommunicationClientFactory** samen met de **WcfCommunicationClient** dat implementeert **ServicePartitionClient** om te bepalen de service-eindpunt en communiceren met de service.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] De standaard ServicePartitionResolver wordt ervan uitgegaan dat de client wordt uitgevoerd in hetzelfde cluster als de service. Als dat wil zeggen niet het geval is, maakt u een object ServicePartitionResolver en in de eindpunten van de verbinding cluster.

## <a name="next-steps"></a>Volgende stappen
* [Externe procedureaanroep met betrouwbare Services externe toegang](service-fabric-reliable-services-communication-remoting.md)

* [Web API met OWIN in betrouwbare Services](service-fabric-reliable-services-communication-webapi.md)

* [Communicatie voor betrouwbare Services beveiligen](service-fabric-reliable-services-secure-communication.md)
