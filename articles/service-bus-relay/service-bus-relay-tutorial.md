<properties 
    pageTitle="Service Bus Relay zelfstudie | Microsoft Azure"
    description="Hiermee maakt u een Service Bus-client-toepassing en service met behulp van de Service Bus Relay."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Service Bus Relay zelfstudie

Deze zelfstudie wordt beschreven hoe maakt u een eenvoudige Service Bus-clienttoepassing en service met behulp van de mogelijkheden van Service Bus 'relay'. Voor een bijbehorende zelfstudie die Service Bus [brokered messaging](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)gebruikt, raadpleegt u de [Service Bus Brokered Messaging .NET zelfstudie](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Werken door deze zelfstudie kunt u de stappen die vereist zijn voor het maken van een Service Bus-client en service-toepassing te begrijpen. Een service is zoals hun WCF tegenhangers van Excel, een constructie die een of meer eindpunten, beschrijft die allemaal een of meer servicebewerkingen. Het eindpunt van een service geeft een adres waar de service kan worden gevonden, een binding met de gegevens die een client moet communiceren met de service en een nieuw contract die de functionaliteit van de service aan clients definieert. Het belangrijkste verschil tussen een WCF- en een Service Bus-service is dat het eindpunt worden weergegeven in de cloud in plaats van lokaal op uw computer.

Nadat u de reeks onderwerpen in deze zelfstudie werkt, hebt u een actieve service en een client die de bewerkingen van de service kunt aanroepen. De eerste onderwerp wordt beschreven hoe u een account instelt. De volgende stappen wordt beschreven hoe u het definiëren van een service die gebruikmaakt van een nieuw contract, het implementeren van de service en hoe u de service configureren in code. Ze ook wordt uitgelegd hoe u hosten en uitvoeren van de service. De service die u maakt zelf gehoste is en de client en service worden uitgevoerd op dezelfde computer. U kunt de service configureren met behulp van de code of een configuratiebestand.

De laatste drie stappen wordt uitgelegd hoe u een clienttoepassing maken en configureren van de clienttoepassing, en maken en gebruiken van een client die toegang heeft tot de functionaliteit van de host.

Alle onderwerpen in deze sectie wordt ervan uitgegaan dat u Visual Studio als de ontwikkelomgeving gebruikt.

## <a name="sign-up-for-an-account"></a>Registreren voor een account

De eerste stap is een naamruimte maken en verkrijgen van een sleutel gedeeld Access handtekening (SA's). Een naamruimte bevat de grens van een toepassing voor elke toepassing weergegeven via de Service Bus. Een sleutel SA's wordt automatisch gegenereerd door het systeem wanneer de naamruimte van een service wordt gemaakt. De combinatie van de naamruimte van service en SA's sleutel biedt de referenties voor de Service Bus om te verifiëren toegang tot een toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>Een WCF-servicecontract voor gebruik met Service Bus definiëren

Welke bewerkingen Hiermee geeft u het servicecontract (de Web-service terminologie voor methoden of functies) de service ondersteunt. Contracten worden gemaakt door het definiëren van een interface C++, C# of Visual Basic. Elke methode in de interface overeenkomt met een specifieke servicebewerking. Elke interface moet het kenmerk [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) is toegepast, en elke bewerking moet hebben het kenmerk [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) is toegepast. Als een methode in een interface met het kenmerk [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) geen het [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) -kenmerk heeft, wordt deze methode niet beschikbaar. De code voor deze taken is opgegeven in het voorbeeld procedure te volgen. Zie voor een grotere discussie van contracten en -services, [ontwerpen en implementeren van Services](https://msdn.microsoft.com/library/ms729746.aspx) in de documentatie WCF.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Een Service Bus om contract te maken met een interface

1. Open Visual Studio als een beheerder met de rechtermuisknop op het programma in het menu **Start** selecteren **Als administrator uitvoeren**.

2. Maak een nieuw project van console-toepassing. Klik op het menu **bestand** en selecteert u daarna **Nieuw**en klik op **Project**. Klik in het dialoogvenster **Nieuw Project** op **Visual C#** (als **Visual C#** niet wordt weergegeven, zoek onder **Andere talen**). Klik op de sjabloon **Console-toepassing** en noem deze **EchoService**. Klik op **OK** om het project te maken.

    ![][2]

3. Het pakket Service Bus NuGet installeren. Dit pakket automatisch verwijzingen naar de Service Bus-bibliotheken, evenals de WCF- **traceerbron**toegevoegd. [Traceerbron](https://msdn.microsoft.com/library/system.servicemodel.aspx) is de naamruimte waarmee u kunt via programmacode toegang tot de basisfuncties van WCF. Service Bus maakt gebruik van veel van de objecten en kenmerken van WCF servicecontracten definiëren.

    In Solution Explorer met de rechtermuisknop op de oplossing en klik vervolgens op **NuGet-pakketten beheren voor oplossing**. Klik op het tabblad **Bladeren** en zoek vervolgens voor `Microsoft Azure Service Bus`. Zorg ervoor dat de naam van het project is geselecteerd in het vak **versie (s)** . Klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![][3]

3. Dubbelklik in Solution Explorer op het bestand Program.cs om deze te openen in de editor, als dit nog niet is geopend.

4. Voeg de volgende beweringen met aan de bovenkant van het bestand:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Wijzig de naamruimtenaam van de standaardnaam van **EchoService** in **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] Deze zelfstudie gebruikt de C#-naamruimte **Microsoft.ServiceBus.Samples**, dat wil zeggen de naamruimte van het beheerde contracttype die wordt gebruikt in het configuratiebestand in de stap [configureren de WCF-client](#configure-the-wcf-client) . U kunt elke gewenste als u dit voorbeeld; samenstelt naamruimte opgeven de zelfstudie worden echter niet werken alleen als u vervolgens de naamruimten van het contract wijzigen en service dienovereenkomstig gewijzigd, in het configuratiebestand. De naamruimte die is opgegeven in het bestand App.config moet hetzelfde als de opgegeven naamruimte in uw C#-bestanden.

1. Direct achter de `Microsoft.ServiceBus.Samples` declaratie van naamruimte, maar definiëren binnen de naamruimte, een nieuwe interface met de naam `IEchoContract` en pas de `ServiceContractAttribute` kenmerk aan de interface met een naamruimtewaarde van **http://samples.microsoft.com/ServiceModel/Relay/**. De naamruimtewaarde verschilt van de naamruimte die u overal in het bereik van uw code gebruiken. In plaats daarvan wordt de naamruimtewaarde gebruikt als een unieke id voor deze overeenkomst. Die de naamruimte expliciet aangeeft voorkomt u dat de standaardwaarde van de naamruimte wordt toegevoegd aan de naam van het contract.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] De service contract naamruimte bevat meestal een schema voor de naamgeving dat versie-informatie bevat. Versiegegevens inclusief in de service contract naamruimte kan services isoleren belangrijkste wijzigingen door te definiëren van een nieuw servicecontract met een nieuwe naamruimte en weer op een nieuw eindpunt. Op deze manier kunnen clients doorgaan met het gebruiken van de oude servicecontract zonder te worden bijgewerkt. Versiegegevens kan bestaan uit een datum of een getal opbouwen. Zie [Service versiebeheer](http://go.microsoft.com/fwlink/?LinkID=180498)voor meer informatie. Het schema voor naamgeving van de service contract naamruimte bevat voor de toepassing van deze zelfstudie wordt geen versie-informatie.

1. Binnen de `IEchoContract` interface, een methode voor de één bewerking declareren de `IEchoContract` contract worden weergegeven in de interface en toepassen de `OperationContractAttribute` kenmerk met de methode die u wilt laten zien als onderdeel van de opdracht Service Bus.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Direct achter de `IEchoContract` interface definitie, een kanaal bevindt waarvoor van beide overneemt declareren `IEchoContract` en ook naar de `IClientChannel` interface, zoals hier wordt getoond:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Een kanaal is het WCF-object waarmee de host en de client informatie aan elkaar grenzen doorgeven. Later, wordt u code ten opzichte van het kanaal echo informatie tussen de twee toepassingen schrijven.

1. Klik in het menu **maken** Klik op **Oplossing maken** of druk op **Ctrl + Shift + B** om de nauwkeurigheid van uw werk dusverre bevestigen.

### <a name="example"></a>Voorbeeld

De volgende code ziet u een eenvoudige interface die een contract Service Bus definieert.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Nu dat de interface is gemaakt, kunt u de interface kunt implementeren.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>Implementeren van het contract WCF-Service Bus gebruiken

Een Service Bus relay maken, moet u eerst de overeenkomst, die is gedefinieerd met behulp van een interface. Zie voor meer informatie over het maken van de interface, de vorige stap. De volgende stap is de interface implementeren. Dit heeft betrekking op het maken van een klasse met de naam `EchoService` die wordt geïmplementeerd de door de gebruiker gedefinieerde `IEchoContract` interface. Nadat u de interface implementeert, configureert u vervolgens de interface met een configuratiebestand App.config. Het configuratiebestand bevat de benodigde informatie voor de toepassing, zoals de naam van de service, de naam van het contract, en het type protocol dat wordt gebruikt om te communiceren met Service Bus. De code die wordt gebruikt voor deze taken is opgegeven in het voorbeeld procedure te volgen. Zie voor een meer algemene informatie over hoe u een opdracht implementeert, [Servicecontracten implementeren](https://msdn.microsoft.com/library/ms733764.aspx) in de documentatie WCF.

1. Maak een nieuwe klasse met de naam `EchoService` direct achter de definitie van de `IEchoContract` interface. De `EchoService` klasse implementeert de `IEchoContract` interface. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Net als andere interface-implementaties, kunt u de definitie implementeren in een ander bestand. Echter de uitvoering voor deze zelfstudie bevindt zich in hetzelfde als de interfacedefinitie en de `Main` methode.

1. Het kenmerk [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) naar toepassen de `IEchoContract` interface. Het kenmerk geeft de servicenaam en de naamruimte. Nadat u dit doet, de `EchoService` class ziet er als volgt:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Implementeren de `Echo` methode die zijn gedefinieerd in de `IEchoContract` interface in de `EchoService` class. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Klik op **maken**en klik op **Oplossing maken** om te bevestigen van de nauwkeurigheid van uw werk.

### <a name="to-define-the-configuration-for-the-service-host"></a>De configuratie voor de ServiceHost definiëren

1. Het configuratiebestand is vergelijkbaar met een WCF-configuratiebestand. Het bevat de servicenaam van de, eindpunt (dat wil zeggen, de locatie Service Bus beschrijft voor clients en hosts om te communiceren met elkaar) en de binding (het type protocol dat wordt gebruikt om te communiceren). Het belangrijkste verschil is dat deze geconfigureerde service-eindpunt verwijst naar een binding [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) , die geen deel uitmaakt van .NET Framework. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) is een van de bindingen gedefinieerd door de Service Bus.

1. Dubbelklik op het bestand App.config om deze te openen in de Visual Studio-editor in **Solution Explorer**.

2. In de `<appSettings>` element, de tijdelijke aanduidingen met de naam van de naamruimte van uw service en de SA's belangrijke die u hebt gekopieerd in een eerdere stap vervangen. 

1. Binnen de `<system.serviceModel>` tags toevoegen een `<services>` element. U kunt meerdere Service Bus toepassingen definiëren in een enkel configuratiebestand. Deze zelfstudie definieert echter slechts één.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. Binnen de `<services>` element, toevoegen een `<service>` element dat moet worden de naam van de service definiëren.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. Binnen de `<service>` element, de locatie van het eindpuntcontract en ook het type binding voor het eindpunt definiëren.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Het eindpunt wordt gedefinieerd waarin de client voor de hosttoepassing eruitziet. Later, wordt de zelfstudie deze stap om te maken van een URI die de host via Service Bus ondersteunt. De binding gedeclareerd dat we TCP als het protocol worden gebruikt om te communiceren met Service Bus.

1. Klik op **Oplossing maken** om te bevestigen van de nauwkeurigheid van uw werk in het menu **maken** .

### <a name="example"></a>Voorbeeld

De volgende code ziet u de uitvoering van het servicecontract.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

De volgende code toont de basisindeling van het bestand App.config die is gekoppeld aan de ServiceHost.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Hosten en uitvoeren van een eenvoudige webservice registreren bij Service Bus

Deze stap uitgelegd hoe u het uitvoeren van een eenvoudige Service Bus-service.

### <a name="to-create-the-service-bus-credentials"></a>De Service Bus-referenties maken

1. In `Main()`, twee variabelen waarin voor de opslag van de naamruimte maken en de SA's belangrijke die worden opgelezen vanuit het consolevenster.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    De toets SA's worden later gebruikt voor toegang tot uw Service Bus-project. De naamruimte wordt doorgegeven als een parameter voor `CreateServiceUri` een service URI maken.

4. Met een object [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) declareren dat u een sleutel SA's als het referentietype gebruikt. Voeg de volgende code direct achter de code in de laatste stap hebt toegevoegd.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>Een basisadres voor de service maken

1. Na de code die u hebt toegevoegd in de laatste stap maken een `Uri` exemplaar voor het basisadres van de service. Deze URI Hiermee geeft u de Service Bus kleurenschema, de naamruimte en het pad van de service-interface.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    'sb' is een afkorting voor het schema Service Bus en wordt aangegeven dat we TCP worden gebruikt als het protocol. Dit is ook eerder aangegeven in het configuratiebestand wanneer [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) is opgegeven als de binding.
    
    Voor deze zelfstudie de URI is `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Maken en configureren van de ServiceHost

1. De modus connectivity instelt op `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    De modus connectivity beschrijving van het protocol dat de service wordt gebruikt om te communiceren met Service Bus; HTTP- of TCP. Gebruik van de standaardinstelling `AutoDetect`, probeert de service via verbinding maken met Service Bus TCP als deze beschikbaar is en HTTP als TCP niet beschikbaar is. Hiermee geeft u notitie dat dit van het protocol de service verschilt voor clientcommunicatie van de. Protocol wordt bepaald door de binding gebruikt. Een service kan bijvoorbeeld de binding [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) , waarin wordt aangegeven dat het eindpunt (beschikbaar op Service bevinden) met klanten via HTTP communiceert gebruiken. Die dezelfde service kan **ConnectivityMode.AutoDetect** opgeven, zodat de service met Service Bus via TCP communiceert.

1. De servicehost, waarbij de URI eerder hebt gemaakt in deze sectie maken.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    De ServiceHost is het WCF-object dat de service wordt. Hier als u gebruikmaakt van het type service die u wilt maken (een `EchoService` type), en ook het adres waarop u wilt laten zien van de service.

1. Toevoegen aan de bovenkant van het bestand Program.cs, verwijzingen naar [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) en [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx).

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Terug in `Main()`, het eindpunt voor openbare toegang configureren.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Deze stap wordt geïnformeerd Service Bus die uw toepassing u openbaar vindt aan de hand van de Service Bus ATOM-feed voor uw project. Als u **DiscoveryType** op **privé instelt**, zou een client steeds toegang tot de service. De service zou echter niet weergegeven bij het zoeken van de naamruimte Service Bus. De client moet in plaats daarvan het pad van het eindpunt van tevoren weet.

1. De Servicereferenties van toepassing op de service-eindpunten gedefinieerd in het bestand App.config:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    De wijze beschreven in de vorige stap, kan u hebt meerdere services en eindpunten in het configuratiebestand hebt gedeclareerd. Als u had, wordt deze code zou doen via het configuratiebestand en zoek naar elke eindpunt waaraan dit uw referenties wilt gebruiken. Voor deze zelfstudie heeft het configuratiebestand echter slechts één eindpunt.

### <a name="to-open-the-service-host"></a>De ServiceHost openen

1. Open de service.

    ```
    host.Open();
    ```

1. Gemeld dat de service wordt uitgevoerd en wordt uitgelegd hoe u de service afgesloten.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Wanneer u klaar bent, sluit u de ServiceHost.

    ```
    host.Close();
    ```

1. Druk op **Ctrl + Shift + B** om te maken van het project.

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld bevat de servicecontract en de uitvoering van de voorgaande stappen in deze zelfstudie en host de service in een consoletoepassing. De volgende handelingen uit in een uitvoerbaar bestand met de naam EchoService.exe compileren.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Een WCF-client voor het servicecontract maken

De volgende stap is een eenvoudige Service Bus-clienttoepassing maken en het definiëren van de service-opdracht die u in de volgende stappen implementeren wilt. Opmerking dat veel van deze stappen lijken op de stappen voor het maken van een service: een contract, het bewerken van een App.config definiëren bestand, met referenties verbinding maken met de Service Bus, enzovoort. De code die wordt gebruikt voor deze taken is opgegeven in het voorbeeld procedure te volgen.

1. Een nieuw project maakt in de huidige Visual Studio-oplossing voor de client als volgt:
    1. In Solution Explorer in dezelfde oplossing met de service, met de rechtermuisknop op de huidige oplossing (niet de project) en klik op **toevoegen**. Klik vervolgens op **Nieuw Project**.
    2. In het dialoogvenster **Nieuw Project toevoegen** , klikt u op **Visual C#** (als **Visual C#** niet wordt weergegeven, zoek onder **Andere talen**), selecteert u de sjabloon **Console-toepassing** en noem deze **EchoClient**.
    3. Klik op **OK**.
<br />

1. Dubbelklik in Solution Explorer op het bestand Program.cs in het project **EchoClient** om dit te openen in de editor, als dit nog niet is geopend.

1. Wijzig de naamruimtenaam van de van de standaardnaam van `EchoClient` naar `Microsoft.ServiceBus.Samples`.

1. Installeer het [Service Bus NuGet-pakket](https://www.nuget.org/packages/WindowsAzure.ServiceBus). In Solution Explorer met de rechtermuisknop op het project **EchoClient** en klik vervolgens op **NuGet-pakketten beheren**. Klik op het tabblad **Bladeren** en zoek vervolgens voor `Microsoft Azure Service Bus`. Klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![][3]

1. Toevoegen een `using` instructie voor de naamruimte [traceerbron](https://msdn.microsoft.com/library/system.servicemodel.aspx) in het bestand Program.cs. 

    ```
    using System.ServiceModel;
    ```

1. Toevoegen aan de naamruimte, de definitie van het contract service zoals wordt weergegeven in het volgende voorbeeld. Houd er rekening mee dat deze definitie gelijk aan de definitie in het project **Service** gebruikt is. Moet u deze code toevoegen aan de bovenkant van de `Microsoft.ServiceBus.Samples` naamruimte.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Druk op **Ctrl + Shift + B** om te maken van de client.

### <a name="example"></a>Voorbeeld

De volgende code ziet u de huidige status van het bestand Program.cs in het project EchoClient.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>De WCF-client configureren

In deze stap maakt u een bestand App.config voor een eenvoudige clienttoepassing die toegang heeft tot de service die eerder in deze zelfstudie hebt gemaakt. Dit bestand App.config definieert het contract, binding en naam van het eindpunt. De code die wordt gebruikt voor deze taken is opgegeven in het voorbeeld procedure te volgen.

1. Dubbelklik in Solution Explorer in het project **EchoClient** **App.config** u opent het bestand in de Visual Studio-editor.

2. In de `<appSettings>` element, de tijdelijke aanduidingen met de naam van de naamruimte van uw service en de SA's belangrijke die u hebt gekopieerd in een eerdere stap vervangen.

1. Binnen het element traceerbron toevoegen een `<client>` element.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Deze stap gedeclareerd dat u een stijl van een WCF-clienttoepassing definieert.

1. Binnen de `client` element, de naam, contract en bindingstype voor het eindpunt definiëren.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Deze stap definieert de naam van het eindpunt, het contract gedefinieerd in de service en het feit dat de clienttoepassing TCP gebruikt om te communiceren met Service Bus. De naam van het eindpunt wordt gebruikt in de volgende stap het koppelen van deze eindpuntconfiguratie met de service-URI.

1. Klik op **bestand**en klik op **Alles opslaan**.

## <a name="example"></a>Voorbeeld

De volgende code ziet u het bestand App.config voor de client Echo.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>De WCF-client om te bellen Service Bus implementeren

In deze stap geeft implementeren u een eenvoudige clienttoepassing die toegang heeft tot de service die u eerder hebt gemaakt in deze zelfstudie. De client is vergelijkbaar met de service, voert veel bewerkingen uitvoeren voor toegang tot de Service Bus:

1. Hiermee stelt u de modus connectivity.
1. Hiermee maakt u de URI die wordt gezocht naar de hostservice.
1. Hiermee definieert u de beveiligingsreferenties.
1. Geldt de referenties voor de verbinding.
1. Hiermee opent de verbinding.
1. Hiermee kunt u de toepassing-specifieke taken uitvoeren.
1. Hiermee sluit u de verbinding.

Een van de belangrijkste verschillen is echter dat de clienttoepassing een kanaal verbinding met Service Bus, maakt terwijl de service wordt gebruikt voor een gesprek aan **ServiceHost**. De code die wordt gebruikt voor deze taken is opgegeven in het voorbeeld procedure te volgen.

### <a name="to-implement-a-client-application"></a>Een clienttoepassing implementeren

1. Stel de modus connectivity **detecteren**. Voeg de volgende code binnen de `Main()` methode van de toepassing **EchoClient** .

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Variabelen waarin de waarden voor de naamruimte van service en de toets SA's die in de console worden gelezen definiëren.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Maak de URI die de locatie van de host in uw project Service Bus definieert.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Maak het object referentie voor uw service naamruimte-eindpunt.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Maak het kanaalfactory waarmee wordt de configuratie die worden beschreven in het bestand App.config geladen.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Een kanaal-factory is een WCF-object dat wordt gemaakt van een kanaal waarmee de service en client-toepassingen communiceren.

1. De referenties Service Bus toepassen.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Maak en open het kanaal met de service.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Schrijf de eenvoudige gebruikersinterface en de functionaliteit voor het echo.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Houd er rekening mee dat de code het exemplaar van het object kanaal als proxy voor de service gebruikt.

1. Sluit het kanaal en sluit de fabriek.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>De toepassingen kunnen uitvoeren

1. Druk op **Ctrl + Shift + B** de oplossing. Dit genereert zowel het project client als de service-project dat u in de vorige stappen hebt gemaakt.

2. Voordat u de clienttoepassing, moet u ervoor zorgen dat de servicetoepassing wordt uitgevoerd. In de Verkenner van de oplossing in Visual Studio, met de rechtermuisknop op de oplossing **EchoService** en klik op **Eigenschappen**.

3. Het eigenschappenvenster van de oplossing **Opstartproject**, klik op de knop **meerdere opstarten projecten** . Zorg ervoor dat **EchoService** boven aan de lijst weergegeven. 

4. Stel het vak **actie** voor de **EchoService** en de **EchoClient** projecten te **starten**.

    ![][5]

5. Klik op **Projectafhankelijkheden**. Selecteer in het vak **projecten** **EchoClient**. Controleer in het vak **Depends.exe op** of **dat echoservice** is ingeschakeld.

    ![][6]

6. Klik op **OK** om te sluiten van het dialoogvenster **Eigenschappen** .

7. Druk op **F5** om uit te voeren beide projecten.

8. Beide consolevensters open en vraagt u de naamruimtenaam. De service moet eerst uitgevoerd, dus voer de naamruimte in het venster **EchoService** console en druk op **Enter**.

9. Vervolgens wordt u gevraagd voor uw sleutel SA's. Voer de SA's-toets ingedrukt en druk op ENTER.

    Hier ziet u een voorbeeld-uitvoer van het consolevenster. Houd er rekening mee dat de waarden voor zover hier bijvoorbeeld uitsluitend.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    De service-toepassing wordt afgedrukt naar het consolevenster het adres waarop luistert, zoals gezien in het volgende voorbeeld.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. Voer in het venster **EchoClient** console dezelfde gegevens die u eerder hebt opgegeven voor de servicetoepassing. De vorige stappen als u de naamruimte van dezelfde service en sleutelwaarden SA's voor de clienttoepassing.

11. Na het invoeren van deze waarden, de client een kanaal geopend naar de service en gevraagd of u bepaalde tekst zoals gezien in het volgende voorbeeld van de console-uitvoer moet invoeren.

    `Enter text to echo (or [Enter] to exit):` 

    Voer de tekst als u wilt de service-toepassing en druk op Enter. Deze tekst wordt verzonden naar de service tot en met de bewerking van de Echo en wordt weergegeven in het venster van de service-console zoals in het volgende voorbeeld-uitvoer.

    `Echoing: My sample text`

    De clienttoepassing ontvangt de retourwaarde van de `Echo` betrekking heeft, de oorspronkelijke tekst, waarna deze wordt afgedrukt naar de consolevenster. Hierna volgt een voorbeeld-uitvoer van het venster van de console-client.

    `Server echoed: My sample text`

12. U kunt doorgaan met het verzenden van SMS-berichten van de client naar de service op deze manier. Wanneer u klaar bent, drukt u op Enter in de client en service console-vensters tot het einde van beide toepassingen.

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld ziet u hoe u een clienttoepassing maakt, hoe u het bellen van de bewerkingen van de service en hoe de client sluiten nadat het gesprek bewerking is voltooid.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie blijkt het maken van een Service Bus-client-toepassing en service met behulp van de mogelijkheden van Service Bus 'relay'. Voor een soortgelijke zelfstudie die Service Bus [Messaging](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)gebruikt, raadpleegt u de [Service Bus Brokered Messaging .NET zelfstudie](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Zie de volgende onderwerpen voor meer informatie over de Service Bus.

- [SMS-service Bus-overzicht](../service-bus-messaging/service-bus-messaging-overview.md)
- [Service Bus over grondbeginselen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Service-busarchitectuur](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
