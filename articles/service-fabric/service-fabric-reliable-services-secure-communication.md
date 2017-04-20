<properties
   pageTitle="Beveiligde communicatie voor services in Service stof Help | Microsoft Azure"
   description="Overzicht van hoe u beveiligde communicatie voor betrouwbare services die worden uitgevoerd in een cluster Azure-Service stof kunt."
   services="service-fabric"
   documentationCenter=".net"
   authors="suchiagicha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/06/2016"
   ms.author="suchiagicha"/>

# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Beveiligde communicatie van de Help voor services in Azure-Service stof

Beveiliging is een van de belangrijkste aspecten van communicatie. Het kader van de toepassing betrouwbare Services vindt u enkele vooraf gedefinieerde communicatie stapels en hulpmiddelen die u gebruiken kunt om beveiliging te verbeteren. In dit artikel wordt nader bekeken beveiliging verbeteren wanneer u externe service en de stapel van de communicatie Windows Communication Foundation (WCF) gebruikt.

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Beveiligen van een service wanneer u externe service gebruikt

We voortaan een bestaande [voorbeeld](service-fabric-reliable-services-communication-remoting.md) , waarin wordt uitgelegd hoe voor het instellen van externe toegang voor betrouwbare services gebruikt. Als u wilt beveiligen een service wanneer u externe service gebruikt, als volgt te werk:

1. Maken van een interface, `IHelloWorldStateful`, die definieert de methoden die beschikbaar zijn voor een externe procedureaanroep van uw service. Uw service wordt gebruikt `FabricTransportServiceRemotingListener`, waarin wordt aangegeven de `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` naamruimte. Dit is een `ICommunicationListener` functies voor externe biedt-implementatie.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```

2. Instellingen van de luisteraar ervan af en beveiligingsreferenties toevoegen.

    Zorg ervoor dat het certificaat dat u gebruiken wilt voor de beveiliging van uw service communicatie is geïnstalleerd op alle knooppunten in het cluster. Er zijn twee manieren dat kunt u instellingen voor de luisteraar ervan af en beveiligingsreferenties opgeven:

    1. Geef hen dan rechtstreeks in de servicecode:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listenerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
            };
        }

        private static SecurityCredentials GetSecurityCredentials()
        {
            // Provide certificate details.
            var x509Credentials = new X509Credentials
            {
                FindType = X509FindType.FindByThumbprint,
                FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
                StoreLocation = StoreLocation.LocalMachine,
                StoreName = "My",
                ProtectionLevel = ProtectionLevel.EncryptAndSign
            };
            x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
            return x509Credentials;
        }
        ```
    2. Geef hen dan met behulp van een [pakket met zoekconfiguraties](service-fabric-application-model.md):

        Toevoegen een `TransportSettings` sectie in het bestand settings.xml.

        ```xml
        <!--Section name should always end with "TransportSettings".-->
        <!--Here we are using a prefix "HelloWorldStateful".-->
        <Section Name="HelloWorldStatefulTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509" />
            <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
            <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
            <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
            <Parameter Name="CertificateStoreName" Value="My" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
            <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
        </Section>
        ```

        In dit geval de `CreateServiceReplicaListeners` methode ziet er als volgt:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(
                        context,this,FabricTransportListenerSettings.LoadFrom("HelloWorldStateful")))
            };
        }
        ```

         Als u een `TransportSettings` sectie in het bestand settings.xml zonder voorvoegsel, `FabricTransportListenerSettings` alle instellingen van deze sectie al dan niet standaard wordt geladen.

         ```xml
         <!--"TransportSettings" section without any prefix.-->
         <Section Name="TransportSettings">
             ...
         </Section>
         ```
         In dit geval de `CreateServiceReplicaListeners` methode ziet er als volgt:

         ```csharp
         protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
         {
             return new[]
             {
                 return new[]{
                         new ServiceReplicaListener(
                             (context) => new FabricTransportServiceRemotingListener(context,this))};
             };
         }
         ```

3. Wanneer u methoden bellen op een beveiligde service met behulp van de stapel externe in plaats van de `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` klasse naar een serviceproxy maken, gebruikt u `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Doorgeven `FabricTransportSettings`, die bevat `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");

    FabricTransportSettings transportSettings = new FabricTransportSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Als de clientcode wordt uitgevoerd als onderdeel van een service, kunt u laden `FabricTransportSettings` uit het bestand settings.xml. Maak een sectie TransportSettings die vergelijkbaar is met de servicecode, zoals eerder weergegeven. De volgende wijzigingen aanbrengen in de clientcode:

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Als de client niet als onderdeel van een service wordt uitgevoerd, kunt u een client_name.settings.xml-bestand maken op dezelfde locatie waar de client_name.exe is. Een sectie TransportSettings maken in dat bestand.

    Vergelijkbaar met de service, als u een `TransportSettings` sectie zonder voorvoegsel in client settings.xml/client_name.settings.xml, `FabricTransportSettings` alle instellingen van deze sectie al dan niet standaard wordt geladen.

    In dat geval is de eerdere code nog verder vereenvoudigd:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>Beveiligen van een service wanneer u een stapel WCF-communicatie gebruikt

We voortaan een bestaande [voorbeeld](service-fabric-reliable-services-communication-wcf.md) , waarin wordt uitgelegd hoe voor het instellen van een stapel WCF-communicatie voor betrouwbare services gebruikt. Als u wilt beveiligen een service wanneer u een stapel WCF-communicatie gebruikt, als volgt te werk:

1. Voor de service, moet u de luisteraar ervan af communicatie WCF beveiligen (`WcfCommunicationListener`) die u maakt. Klik hiertoe wijzigen de `CreateServiceReplicaListeners` methode.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```

2. In de client, de `WcfCommunicationClient` klasse die is gemaakt in het vorige [voorbeeld](service-fabric-reliable-services-communication-wcf.md) blijft ongewijzigd. Maar u moet overschrijven de `CreateClientAsync` methode van `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Gebruik `SecureWcfCommunicationClientFactory` maken van een WCF-communicatie-client (`WcfCommunicationClient`). Gebruik de client om aan te roepen servicemethoden.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a>Volgende stappen

* [Web API met OWIN in betrouwbare Services](service-fabric-reliable-services-communication-webapi.md)
