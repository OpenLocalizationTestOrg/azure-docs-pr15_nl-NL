<properties
    pageTitle="Gebruik van zelfstudie Service Bus REST doorgegeven messaging | Microsoft Azure"
    description="Een eenvoudige Service Bus relay hosttoepassing dat toegang een interface REST gebaseerde biedt maken."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>Service Bus Relay REST zelfstudie

Deze zelfstudie beschreven hoe u een eenvoudige Service Bus hosttoepassing dat toegang een interface REST gebaseerde biedt maken. REST kunt een webclient, zoals een webbrowser, toegang tot de Service Bus API's via HTTP-aanvragen.

In deze zelfstudie wordt de REST Windows Communication Foundation (WCF) programming model als u wilt samenstellen van een REST-service op Service bevinden. Zie [WCF REST Programming Model](https://msdn.microsoft.com/library/bb412169.aspx) en [ontwerpen en implementeren van Services](https://msdn.microsoft.com/library/ms729746.aspx) in de WCF-documentatie voor meer informatie.

## <a name="step-1-create-a-service-namespace"></a>Stap 1: Maak een Servicenaamruimte

De eerste stap is een naamruimte maken en verkrijgen van een sleutel gedeeld Access handtekening (SA's). Een naamruimte bevat de grens van een toepassing voor elke toepassing weergegeven via de Service Bus. Een sleutel SA's wordt automatisch gegenereerd door het systeem wanneer de naamruimte van een service wordt gemaakt. De combinatie van de naamruimte van service en SA's sleutel biedt de referenties voor de Service Bus om te verifiëren toegang tot een toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Stap 2: Een contract REST gebaseerde WCF-service voor gebruik met Service Bus definiëren

Als met andere services Service Bus, moet bij het maken van een service REST-stijl, u het contract. Het contract Hiermee welke bewerkingen die ondersteuning biedt voor de host. Een servicebewerking kan worden beschouwd als een methode voor de webservice. Contracten worden gemaakt door het definiëren van een interface C++, C# of Visual Basic. Elke methode in de interface overeenkomt met een specifieke servicebewerking. Het kenmerk [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) moet worden toegepast op elke interface en de [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) -kenmerk moet worden toegepast op elke bewerking. Als een methode in een interface met het [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) geen de [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx)heeft, wordt deze methode niet beschikbaar. De code die wordt gebruikt voor deze taken wordt weergegeven in het voorbeeld procedure te volgen.

Het belangrijkste verschil tussen een eenvoudige Service Bus contract en een contract REST-stijl is het toevoegen van een eigenschap waarmee de [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Deze eigenschap kunt u een methode in uw interface toewijzen aan een methode aan de andere kant van de interface. In dit geval we [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) gebruiken om de koppeling van een methode naar HTTP GET. Hierdoor Service Bus nauwkeurig ophalen en opdrachten die zijn verzonden naar de interface interpreteren.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Een Service Bus om contract te maken met een interface

1. Open Visual Studio als beheerder: met de rechtermuisknop op het programma in het menu **Start** en klik vervolgens op **Als administrator uitvoeren**.

2. Maak een nieuw project van console-toepassing. Klik op het menu **bestand** en selecteert u daarna **Nieuw**en selecteer **Project**. Klik in het dialoogvenster **Nieuw Project** op **Visual C#**, selecteert u de sjabloon **Console-toepassing** en noem deze **ImageListener**. Gebruik de standaardwaarde **locatie**. Klik op **OK** om het project te maken.

3. Voor een C#-project Visual Studio maakt een `Program.cs` bestand. Deze klasse bevat een lege `Main()` methode vereist voor een project van de toepassing console correct maken.

4. Verwijzingen naar Service-Bus en **System.ServiceModel.dll** toevoegen aan het project door het installeren van de Service Bus NuGet-pakket. Dit pakket automatisch verwijzingen naar de Service Bus-bibliotheken, evenals de WCF- **traceerbron**toegevoegd. In Solution Explorer met de rechtermuisknop op het project **ImageListener** en klik vervolgens op **NuGet-pakketten beheren**. Klik op het tabblad **Bladeren** en zoek vervolgens voor `Microsoft Azure Service Bus`. Klik op **installeren**en accepteer de gebruiksvoorwaarden.

5. U moet een verwijzing naar **System.ServiceModel.Web.dll** expliciet toevoegen aan het project:

    een. Met de rechtermuisknop op de map **verwijzingen** onder de projectmap en klik op **Add Reference**in Solution Explorer.

    b. Klik op het tabblad **Framework** aan de linkerkant en in het vak **Zoeken** in het dialoogvenster **Verwijzing toevoegen** , typt u **System.ServiceModel.Web**. Schakel het selectievakje **System.ServiceModel.Web** en klik op **OK**.

6. Voeg de volgende `using` instructies boven aan het bestand Program.cs.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [Traceerbron](https://msdn.microsoft.com/library/system.servicemodel.aspx) is de naamruimte waarmee programma toegang tot de basisfuncties van WCF. Service Bus maakt gebruik van veel van de objecten en kenmerken van WCF servicecontracten definiëren. U gebruikt deze naamruimte in de meeste van uw Service Bus relay-toepassingen. Op dezelfde manier helpt [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) het kanaal, dat wil zeggen het object waarmee u Service Bus en de webbrowser client communiceert definiëren. Ten slotte bevat [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) de typen waarmee u kunt het maken van webtoepassingen.

7. Wijzig de naam van de `ImageListener` naamruimte **Microsoft.ServiceBus.Samples**.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Direct nadat de accolade voor het openen van de declaratie van de naamruimte, definiëren in een nieuwe interface **IImageContract** met de naam en het kenmerk **ServiceContractAttribute** toepassen op de interface met een waarde van `http://samples.microsoft.com/ServiceModel/Relay/`. De naamruimtewaarde verschilt van de naamruimte die u overal in het bereik van uw code gebruiken. De naamruimtewaarde wordt gebruikt als een unieke id voor deze overeenkomst en versie-informatie nodig hebt. Zie [Service versiebeheer](http://go.microsoft.com/fwlink/?LinkID=180498)voor meer informatie. Die de naamruimte expliciet aangeeft voorkomt u dat de standaardwaarde van de naamruimte wordt toegevoegd aan de naam van het contract.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. Binnen de `IImageContract` interface, een methode voor de één bewerking declareren de `IImageContract` contract worden weergegeven in de interface en toepassen de `OperationContractAttribute` kenmerk met de methode die u wilt laten zien als onderdeel van de opdracht Service Bus.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. In het kenmerk **OperationContract** toevoegen de waarde **WebGet** .

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Service Bus route HTTP GET aanvragen voor doet kunt `GetImage`, en de retourwaarden van vertalen `GetImage` in antwoord op een HTTP GETRESPONSE. Verderop in deze zelfstudie gebruikt u een webbrowser voor toegang tot deze methode, en om weer te geven van de afbeelding in de browser.

11. Direct achter de `IImageContract` definitie, een kanaal bevindt waarvoor van beide overneemt declareren de `IImageContract` en `IClientChannel` interfaces.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Een kanaal is het WCF-object waarmee de service en client informatie aan elkaar grenzen doorgeven. Later, maakt u het kanaal in uw hosttoepassing. Dit kanaal wordt Service Bus de HTTP GET-aanvragen vanuit de browser doorgeven aan uw **GetImage** -implementatie. Service Bus wordt ook gebruikt voor het kanaal kunt u de retourwaarde **GetImage** en deze vertalen in een HTTP-GETRESPONSE voor de clientbrowser.

12. Klik op **Oplossing maken** om te bevestigen de nauwkeurigheid van uw werk dusverre in het menu **maken** .

### <a name="example"></a>Voorbeeld

De volgende code ziet u een eenvoudige interface die een contract Service Bus definieert.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Stap 3: Een opdracht REST gebaseerde WCF-Service Bus gebruik implementeren

Een stijl van een REST Service Bus-service maken, moet u eerst de overeenkomst, die is gedefinieerd met behulp van een interface. De volgende stap is de interface implementeren. Dit heeft betrekking op het maken van een klasse met de naam **ImageService** dat de gebruiker gedefinieerde **IImageContract** -interface implementeert. Na het implementeren van het contract, configureren u vervolgens de interface met behulp van een bestand App.config. Het configuratiebestand bevat de benodigde informatie voor de toepassing, zoals de naam van de service, de naam van het contract, en het type protocol dat wordt gebruikt om te communiceren met Service Bus. De code die wordt gebruikt voor deze taken is opgegeven in het voorbeeld procedure te volgen.

Als u met de voorgaande stappen, moet u er weinig verschil tussen de uitvoering van een contract REST-stijl en een eenvoudige Service Bus contract is.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>Een stijl van een REST Service Bus contract implementeren

1. Maak een nieuwe klasse met de naam **ImageService** direct achter de definitie van de **IImageContract** -interface. De klasse **ImageService** implementeert de interface **IImageContract** .

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Net als andere interface-implementaties, kunt u de definitie implementeren in een ander bestand. Echter voor deze zelfstudie de uitvoering wordt weergegeven in hetzelfde als de definitie van de gebruikersinterface en `Main()` methode.

2. Het kenmerk [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) toepassen in de klas **IImageService** om aan te geven dat de klasse een implementatie van een WCF-overeenkomst is.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Zoals eerder is vermeld, wordt deze naamruimte is niet een traditionele naamruimte. In plaats daarvan maakt deze deel uit van de WCF-architectuur die aangeeft van het contract. Zie het onderwerp [Gegevens contractnamen](https://msdn.microsoft.com/library/ms731045.aspx) in de WCF-documentatie voor meer informatie.

3. Een .jpg-afbeelding toevoegen aan uw project.  

    Dit is een afbeelding die de service wordt weergegeven in de ontvangst browser. Met de rechtermuisknop op het project en klik op **toevoegen**. Klik vervolgens op **Bestaand Item**. Het dialoogvenster **Add Existing Item** gebruiken om naar een juiste .jpg, en klik vervolgens op **toevoegen**.

    Bij het toevoegen van het bestand, zorg dat **Alle bestanden** is geselecteerd in de vervolgkeuzelijst naast de **bestandsnaam:** veld. De rest van deze zelfstudie wordt ervan uitgegaan dat de naam van de afbeelding 'afbeelding.jpg' is. Als u een ander bestand hebt, kunt u moet de naam van de afbeelding wijzigen, of uw code ter.

4. Om ervoor te zorgen dat de actieve service het afbeeldingsbestand kunt vinden, in **Solution Explorer** met de rechtermuisknop op het afbeeldingsbestand en klik op **Eigenschappen**. Stel in het deelvenster **Eigenschappen** **kopiëren naar uitvoer Directory** kopie **als nieuwere**.

5. Een verwijzing naar de vergadering **System.Drawing.dll** toevoegen aan het project en ook toevoegen de volgende handelingen uit die is gekoppeld `using` instructies.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. In de klas **ImageService** toevoegen de volgende constructor die wordt geladen de bitmap en bereidt om het te verzenden naar de browser.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Direct na de vorige code, voegt u de volgende **GetImage** -methode in de klas **ImageService** om terug te keren een HTTP-bericht met de afbeelding.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Deze implementatie wordt **MemoryStream** om de afbeelding ophalen en voorbereiden voor streaming naar de browser. Deze Hiermee start u de positie van de stream bij nul, wordt de inhoud van de stream als jpeg gedeclareerd en streamt van de gegevens.

8. Klik op **Oplossing maken**in het menu **maken** .

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>De configuratie voor het uitvoeren van de webservice op Service bevinden definiëren

1. Dubbelklik in **Solution Explorer**op **App.config** om deze te openen in de Visual Studio-editor.

    Het bestand **App.config** lijkt op een WCF-configuratiebestand en bevat de servicenaam, eindpunt (dat wil zeggen, de locatie Service Bus beschrijft voor clients en hosts om te communiceren met elkaar) en binding (het type protocol dat wordt gebruikt om te communiceren). Het belangrijkste verschil hier is dat de geconfigureerde service-eindpunt verwijst naar een binding [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) , die geen deel uitmaakt van .NET Framework.

2. De `<system.serviceModel>` XML-element is een WCF-element die een of meer services definieert. Hier wordt gebruikt om de servicenaam en eindpunt te definiëren. Onderaan in de `<system.serviceModel>` element (maar nog steeds binnen `<system.serviceModel>`), toevoegen een `<bindings>` element dat de volgende inhoud heeft. Hiermee definieert u de bindingen in de toepassing gebruikt. U kunt meerdere bindingen definiëren, maar deze zelfstudie definieert u slechts één.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Deze stap definieert een Service Bus [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) binding met **relayClientAuthenticationType** ingesteld op **geen**. Deze instelling wordt aangegeven dat er geen een referentie client is vereist voor een eindpunt met deze binding.

3. Na het `<bindings>` element, toevoegen een `<services>` element. Net als bij de bindingen, kunt u meerdere services definiëren in een enkel configuratiebestand. Echter voor deze zelfstudie definieert u slechts één.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Deze stap configureert een service die gebruikmaakt van de eerder gedefinieerde standaard **webHttpRelayBinding**. De standaard **sbTokenProvider**, die is gedefinieerd in de volgende stap wordt ook gebruikt.

4. Na het `<services>` element, maak een `<behaviors>` element met de volgende inhoud, 'SAS_KEY' vervangen door de *Access-handtekening gedeeld* (SA's)-toets eerder afkomstig is van de [Azure-portal][].

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. Nog steeds in App.config, in de `<appSettings>` element, vervangen tekenreekswaarde van de volledige verbinding met de verbindingsreeks die u eerder hebt aangeschaft bij de portal. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. Klik op **Oplossing maken** om te maken van de volledige oplossing in het menu **maken** .

### <a name="example"></a>Voorbeeld

De volgende code ziet u de uitvoering van het contract en -service voor een REST gebaseerde service die wordt uitgevoerd op Service bevinden met de binding **WebHttpRelayBinding** .

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Het volgende voorbeeld ziet u het bestand App.config die is gekoppeld aan de service.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Stap 4: De REST gebaseerde WCF-service voor het gebruik van de Service Bus hosten

Deze stap uitgelegd hoe u het uitvoeren van een webservice met een consoletoepassing op Service bevinden. Een volledige lijst met de code die is geschreven in deze stap is opgegeven in het voorbeeld procedure te volgen.

### <a name="to-create-a-base-address-for-the-service"></a>Een basisadres voor de service maken

1. In de `Main()` declaratie werken, maakt u een variabele om op te slaan de naamruimte van uw project Service Bus. Zorg ervoor dat voor het vervangen van `yourNamespace` met de naam van de naamruimte van de service die u eerder hebt gemaakt.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    De naam van de naamruimte Service Bus gebruikt om te maken van een unieke URI.

2. Maak een `Uri` exemplaar voor het basisadres van de service die is gebaseerd op de naamruimte.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Maken en configureren van de web-service-host

- Maken van de web-service-host, via het adres URI eerder hebt gemaakt in deze sectie.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    De ServiceHost is het WCF-object dat de host-toepassing start. In dit voorbeeld wordt het het type host die u wilt maken (een **ImageService**), en het adres waarop u wilt laten zien de hosttoepassing.

### <a name="to-run-the-web-service-host"></a>De host van de service web uitvoeren

1. Open de service.

    ```
    host.Open();
    ```
    De service wordt nu uitgevoerd.

2. Een bericht aangegeven dat de service wordt uitgevoerd en hoe u de service stoppen weergeven.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Wanneer u klaar bent, sluit u de ServiceHost.

    ```
    host.Close();
    ```

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld bevat de servicecontract en de uitvoering van de voorgaande stappen in deze zelfstudie en host de service in een consoletoepassing. De volgende code compileren in een uitvoerbaar bestand met de naam ImageListener.exe.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>De code compileren

Na de oplossing bouwen, het volgende als u wilt uitvoeren van de toepassing te doen:

1. Druk op **F5**of blader naar de locatie van het uitvoerbare bestand (ImageListener\bin\Debug\ImageListener.exe), de service uit te voeren. Houd de app actief is, als dit nodig is om uit te voeren van de volgende stap.

2. Kopieer en plak het adres van de opdrachtprompt in een browser om de afbeelding te bekijken.

3. Wanneer u klaar bent, drukt u op **Enter** in het opdrachtpromptvenster te sluiten van de app.

## <a name="next-steps"></a>Volgende stappen

Nu die u kunt een toepassing die gebruikmaakt van de Service Bus relayservice hebt gemaakt, ziet u de volgende artikelen voor meer informatie over indirecte messaging:

- [Overzicht van Azure Service Bus-architectuur](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [Het gebruik van de Service van de Bus-Relay](service-bus-dotnet-how-to-use-relay.md)

[Azure-portal]: https://portal.azure.com