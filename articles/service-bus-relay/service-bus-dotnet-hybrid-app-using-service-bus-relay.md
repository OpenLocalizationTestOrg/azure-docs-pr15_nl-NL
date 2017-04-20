<properties
    pageTitle="Hybride op-premises/cloud-toepassing (.NET) | Microsoft Azure"
    description="Informatie over het maken van een .NET-op-premises/cloud hybride-toepassing met behulp van de Azure Service Bus-relay."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>.NET-op-premises/cloud hybride toepassing met Azure Service Bus Relay

## <a name="introduction"></a>Inleiding

In dit artikel wordt beschreven hoe u een toepassing voor de cloud hybride implementatie met Microsoft Azure en Visual Studio kunt maken. De zelfstudie wordt ervan uitgegaan dat u hebt geen ervaring met Azure. Meer dan 30 minuten hebt u een toepassing die gebruikmaakt van meerdere Azure resources omhoog en worden uitgevoerd in de cloud.

U leert:

-   Het maken of een bestaande webservice voor verbruik aanpassen door een web-oplossing.
-   Het gebruik van de relayservice Azure Service Bus delen van gegevens tussen een Azure-toepassing en een webservice gehost ergens anders.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Hoe de Service Bus relay helpt met hybride oplossingen

Bedrijfsoplossingen meestal bestaan uit een combinatie van aangepaste code geschreven om aan te pakken nieuwe en unieke zakelijke vereisten en bestaande functionaliteit van oplossingen en systemen die al aanwezig zijn.

Oplossing architecten zijn gaan de cloud gebruiken voor eenvoudiger verwerken van schaal vereisten en operationele kosten verlagen. Daarbij vinden ze dat bestaande service activa die ze gebruikmaken willen als bouwstenen voor hun oplossingen binnen de bedrijfsfirewall en afmelden bij eenvoudig worden bereikt voor toegang door de cloud-oplossing. Veel interne services zijn niet gemaakt of die worden gehost op een manier die ze gemakkelijk kunnen worden blootgesteld aan de rand van het bedrijfsnetwerk.

De Service Bus-relay is bedoeld voor de gebruiksvoorbeeld bestaande Windows Communication Foundation (WCF)-webservices te nemen en deze services veilig toegankelijk te maken oplossingen die zich buiten de omtrek en zakelijke zonder storende wijzigingen moeten de infrastructuur van het bedrijfsnetwerk. Deze Service Bus relay diensten worden nog steeds gehost binnen hun bestaande omgeving, maar ze delegeren luisteren naar binnenkomende sessies en aanvragen voor de Service cloud gehoste Bus. Deze services beschermen Service Bus ook tegen ongeoorloofde toegang via [Gedeeld Access handtekening](../service-bus-messaging/service-bus-sas-overview.md) (SA's)-verificatie.

## <a name="solution-scenario"></a>Oplossing scenario

In deze zelfstudie maakt u een ASP.NET-website waarmee u kunt een overzicht van producten op de pagina van de voorraad product.

![][0]

De zelfstudie wordt ervan uitgegaan dat u productinformatie in een bestaande on-premises-systeem hebt en gebruikmaakt van de Service Bus relay bereiken in dat systeem. Dit is gesimuleerd met een webservice die wordt uitgevoerd in een eenvoudige consoletoepassing en wordt ondersteund door een set in het geheugen van producten. U bent kunnen uitvoeren van deze consoletoepassing op uw eigen computer en implementeren van de Webrol in Azure. Als u dit doet, ziet u hoe de Webrol uitgevoerd in het datacenter van de Azure daadwerkelijk wordt inbellen bij de computer, zelfs als uw computer vrijwel zeker achter ten minste één firewall uit en een netwerk adres NAT (Translation) laag zich bevinden.

Hierna volgt een schermafbeelding van de startpagina van de voltooide webtoepassing.

![][1]

## <a name="set-up-the-development-environment"></a>De ontwikkelomgeving instellen

Voordat u met het ontwikkelen van Azure-toepassingen beginnen kunt, krijgen van de hulpmiddelen en stelt u uw ontwikkelomgeving.

1.  De SDK Azure voor .NET installeren vanaf de pagina [krijgen hulpprogramma's en SDK][] .

2.  Klik op **de SDK installeren** voor de versie van Visual Studio u gebruikt. De stappen in deze zelfstudie Gebruik Visual Studio-2015.

4.  Wanneer u hierom wordt gevraagd wilt uitvoeren of opslaan van het installatieprogramma, klikt u op **uitvoeren**.

5.  Klik in het **Installatieprogramma van de Web-Platform**, klikt u op **installeren** en doorgaan met de installatie.

6.  Als de installatie voltooid is, hebt u alles nodig is om te beginnen met het ontwikkelen van de app. De SDK bevat hulpmiddelen waarmee u gemakkelijk Azure toepassingen ontwikkelen in Visual Studio. Als u nog geen Visual Studio is geïnstalleerd, installeert de SDK ook de gratis Visual Studio Express.

## <a name="create-a-namespace"></a>Een naamruimte maken

Als u wilt beginnen met het gebruik van functies van de Service Bus in Azure wordt aangegeven, moet u eerst de naamruimte van een service maken. Een naamruimte bevat een scope container voor de adressering van Service Bus bronnen binnen uw toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Een on-premises server maken

U wordt eerst een systeem (model) on-premises implementatie product catalogus maken. Het is eenvoudig; Hiermee kunt u zien dat een werkelijke on-premises product catalogus systeem met een volledige service oppervlak dat we probeert te integreren.

Dit project is een Visual Studio-console-toepassing en het [pakket van Azure Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) gebruikt om de Service Bus bibliotheken en configuratie-instellingen te nemen.

### <a name="create-the-project"></a>Het project maken

1.  Start Microsoft Visual Studio met behulp van beheerdersbevoegdheden. Als u wilt beginnen Visual Studio met beheerdersbevoegdheden, met de rechtermuisknop op het programmapictogram **Visual Studio** en klik vervolgens op **Als administrator uitvoeren**.

2.  In Visual Studio, klik in het menu **bestand** op **Nieuw**en klik vervolgens op **Project**.

3.  Van **Geïnstalleerde sjablonen**, **Visual C#**, klik op **Console-toepassing**. Typ in het vak **naam** de naam **ProductsServer**:

    ![][11]

4.  Klik op **OK** om het project **ProductsServer** te maken.

7.  Als u de manager NuGet pakket voor Visual Studio al hebt geïnstalleerd, gaat u verder met de volgende stap. Anders, bezoekt u [NuGet][] en klik op [NuGet installeren](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Volg de aanwijzingen voor het installeren van de NuGet pakket manager en vervolgens opnieuw starten Visual Studio.

7.  In Solution Explorer met de rechtermuisknop op het project **ProductsServer** , klik op **NuGet-pakketten beheren**.

8.  Klik op het tabblad **Bladeren** en zoek vervolgens voor `Microsoft Azure Service Bus`. Klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![][13]

    Houd er rekening mee dat de vereiste client stroombaan nu wordt verwezen.

9.  Een nieuwe klasse voor uw product contract toevoegen. In Solution Explorer met de rechtermuisknop op het project **ProductsServer** en klikt u op **toevoegen**en klik vervolgens op **Class**.

10. Typ in het vak **naam** de naam **ProductsContract.cs**. Klik vervolgens op **toevoegen**.

11. In **ProductsContract.cs**, kunt u de naamruimtedefinitie vervangen door de volgende code, waarin het contract voor de service.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. In Program.cs, kunt u de naamruimtedefinitie vervangen door de volgende code die de service gebruikersprofiel en de host voor deze toegevoegd.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. Dubbelklik in Solution Explorer op het bestand **App.config** om deze te openen in de Visual Studio-editor. Onderaan in de ** &lt;systeem. ServiceModel&gt; ** element (maar nog steeds binnen &lt;systeem. ServiceModel&gt;), de volgende XML-code hebt toegevoegd. Zorg ervoor dat u *yourServiceNamespace* met de naam van uw naamruimte en *yourKey* vervangen door de SA's-sleutel die u eerder hebt opgehaald uit de portal:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. Nog steeds in App.config, in de ** &lt;appSettings&gt; ** element, vervangen tekenreekswaarde van de verbinding met de verbindingsreeks die u eerder hebt aangeschaft bij de portal. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Druk op **Ctrl + Shift + B** of klik op **Oplossing maken** om te maken van de toepassing en de nauwkeurigheid van uw werk dusverre Controleer in het menu **maken** .

## <a name="create-an-aspnet-application"></a>Een ASP.NET-toepassing maken

In deze sectie wordt u maken op een eenvoudige ASP.NET-toepassing waarin gegevens opgehaald van uw productservice.

### <a name="create-the-project"></a>Het project maken

1.  Controleer of Visual Studio is uitgevoerd met beheerdersbevoegdheden.

2.  In Visual Studio, klik in het menu **bestand** op **Nieuw**en klik vervolgens op **Project**.

3.  Klik in de **Geïnstalleerde sjablonen**, onder **Visual C#**, op **ASP.NET-webtoepassing**. De naam van het project **ProductsPortal**. Klik vervolgens op **OK**.

    ![][15]

4.  In de lijst **Selecteer een sjabloon** , klikt u op **MVC**. 

6.  Schakel het selectievakje voor **Host in de cloud**.

    ![][16]

5. Klik op de knop **Wijzigen verificatie** . In het dialoogvenster **Verificatie wijzigen** , klikt u op **Geen verificatie**en klik vervolgens op **OK**. Voor deze zelfstudie, bent u een app die niet nodig voor een gebruikersaanmelding hebt implementeren.

    ![][18]

6.  Klik in de sectie **Microsoft Azure** van het dialoogvenster **Nieuw ASP.NET-Project** Zorg dat **in de cloud hosten** is ingeschakeld en dat de **App-Service** is geselecteerd in de vervolgkeuzelijst.

    ![][19]

7. Klik op **OK**. 

8. U moet nu Azure resources voor een nieuwe web-app configureren. Volg de stappen in de sectie [bronnen van Azure configureren voor een nieuwe WebApp](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Klik, Ga terug naar deze zelfstudie en gaat u verder met de volgende stap.

5.  Met de rechtermuisknop op **modellen** in Solution Explorer en klik op **toevoegen**en klik op **Class**. Typ in het vak **naam** de naam **Product.cs**. Klik vervolgens op **toevoegen**.

    ![][17]

### <a name="modify-the-web-application"></a>De webtoepassing wijzigen

1.  In het bestand Product.cs in Visual Studio, kunt u de bestaande naamruimtedefinitie vervangen door de volgende code.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  Vouw de map **Controllers** in Solution Explorer en dubbelklikt u op het bestand **HomeController.cs** te openen in Visual Studio.

3. In **HomeController.cs**, kunt u de bestaande naamruimtedefinitie vervangen door de volgende code.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  Vouw de map Views\Shared in Solution Explorer en dubbelklikt u op **_Layout.cshtml** om deze te openen in de Visual Studio-editor.

5.  Alle exemplaren van **Mijn ASP.NET-toepassing** wijzigen in **van LITWARE-producten**.

6. De koppelingen **Home**, **over**en **contactpersoon** verwijderen. Verwijder in het volgende voorbeeld wordt de gemarkeerde code.

    ![][41]

7.  Vouw de map Views\Home in Solution Explorer en dubbelklikt u op **Index.cshtml** om deze te openen in de Visual Studio-editor.
    Vervang de volledige inhoud van het bestand door de volgende code.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Om te controleren of de nauwkeurigheid van uw werk mooi vindt, kunt u ook op **Ctrl + Shift + B** om te maken van het project.


### <a name="run-the-app-locally"></a>De app lokaal uitvoeren

Voer de toepassing om te bevestigen dat deze altijd werkt.

1.  Ervoor zorgen dat **ProductsPortal** het actieve project is. Met de rechtermuisknop op de naam van het project in Oplossingverkenner en selecteer **Instellen als opstartproject**.
2.  Druk op F5 in Visual Studio.
3.  Uw toepassing moet worden weergegeven, uitgevoerd in een browser.

    ![][21]

## <a name="put-the-pieces-together"></a>De onderdelen samenvoegen

De volgende stap is aansluiten op de server on-premises implementatie producten met de ASP.NET-toepassing.

1.  Als dit nog niet is geopend, in Visual Studio het **ProductsPortal** project dat u hebt gemaakt in de sectie [maken een ASP.NET-toepassing](#create-an-aspnet-application) opnieuw openen.

2.  Het pakket NuGet vergelijkbaar met de stap in de sectie 'Een Server On-Premises maken' toevoegen aan de projectverwijzingen. In Solution Explorer met de rechtermuisknop op het project **ProductsPortal** , klik op **NuGet-pakketten beheren**.

3.  Zoeken naar 'Service Bus' en selecteer het item **Microsoft Azure Service Bus** . Vervolgens de installatie te voltooien en sluit het dialoogvenster.

4.  In Solution Explorer met de rechtermuisknop op het project **ProductsPortal** , klik op **toevoegen**, klikt u vervolgens op **Bestaand Item**.

5.  Ga naar het bestand **ProductsContract.cs** uit het project **ProductsServer** -console. Klik op om ProductsContract.cs markeren. Klik op de pijl-omlaag naast **toevoegen**en klik op **toevoegen als koppeling**.

    ![][24]

6.  Nu de **HomeController.cs** -bestand openen in de Visual Studio-editor en de naamruimtedefinitie vervangen door de volgende code. Zorg ervoor dat *yourServiceNamespace* met de naam van uw Servicenaamruimte en *yourKey* vervangen door uw sleutel SA's. Hiermee schakelt u de client om te bellen van de on-premises implementatie-service, geeft het resultaat van het gesprek.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  Klik in Solution Explorer met de rechtermuisknop op de **ProductsPortal** -oplossing (Zorg ervoor dat u met de rechtermuisknop op de oplossing, niet het project). Klik op **toevoegen**en klik op **Bestaand Project**.

8.  Ga naar het project **ProductsServer** en dubbelklikt u op het bestand van de oplossing **ProductsServer.csproj** toe te voegen.

9.  **ProductsServer** moet worden uitgevoerd om de gegevens op **ProductsPortal**weer te geven. Oplossingverkenner met de rechtermuisknop op de oplossing **ProductsPortal** en klik op **Eigenschappen**. Het dialoogvenster **Eigenschappenvensters** wordt weergegeven.

10. Klik op **Opstartproject**aan de linkerkant. Aan de rechterkant, klikt u op **meerdere opstarten projecten**. Zorg ervoor dat **ProductsServer** en **ProductsPortal** worden weergegeven, in die volgorde, met het **starten** instellen als de actie voor beide.

      ![][25]

11. Nog steeds Klik in het dialoogvenster **Eigenschappen** op **Projectafhankelijkheden** aan de linkerkant.

12. Klik in de lijst **projecten** op **ProductsServer**. Ervoor zorgen dat **ProductsPortal** is **niet** geselecteerd.

14. Klik in de lijst **projecten** op **ProductsPortal**. Zorg ervoor dat **ProductsServer** is geselecteerd. 

    ![][26]

15. Klik op **OK** in het dialoogvenster **Eigenschappenvensters** .

## <a name="run-the-project-locally"></a>Het project lokaal uitvoeren

Als u wilt testen van de toepassing lokaal, druk op **F5**in Visual Studio. De on-premises implementatie-server (**ProductsServer**) eerste moet beginnen, en vervolgens de toepassing **ProductsPortal** moet beginnen in een browservenster. Schakel ditmaal ziet u dat de productinventaris gegevens opgehaald uit het systeem service on-premises implementatie lijsten.

![][10]

Druk op de pagina **ProductsPortal** op **vernieuwen** . Telkens wanneer u de pagina vernieuwt, ziet u de server-app een bericht weergeven wanneer `GetProducts()` uit **ProductsServer** wordt genoemd.

Sluit beide toepassingen voordat u verder gaat met de volgende stap.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Het project ProductsPortal implementeren naar een Azure web-app

De volgende stap is het frontend **ProductsPortal** converteren naar een Azure web-app. Implementeer eerst het **ProductsPortal** project, de stappen in de sectie [Deploy het webproject naar de Azure WebApp](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app). Nadat de implementatie is voltooid, terug naar deze zelfstudie en gaat u verder met de volgende stap.

> [AZURE.NOTE] U ziet mogelijk een foutbericht wordt weergegeven in het browservenster wanneer het **ProductsPortal** webproject automatisch wordt gestart na de implementatie. Hiermee wordt verwacht en doet zich voor omdat de **ProductsServer** -toepassing nog niet actief.

Kopieer de URL van de geïmplementeerd web-app, zoals u de URL in de volgende stap moet. Verder kunt u deze URL in het venster Azure App serviceactiviteit in Visual Studio:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Set ProductsPortal als WebApp

Voordat u de toepassing in de cloud, moet u ervoor zorgen dat **ProductsPortal** is gestart vanuit Visual Studio als een web-app.

1. Met de rechtermuisknop op het project **ProjectsPortal** in Visual Studio, en klik vervolgens op **Eigenschappen**.

3. Klik in de linkerkolom op **Web**.

5. Klik op de knop **Start URL** in de sectie **Actie starten** en voer in het tekstvak de URL voor uw eerder geïmplementeerd WebApp; bijvoorbeeld `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. Klik op **Alles opslaan**in het menu **bestand** in Visual Studio.

7. In het menu Build in Visual Studio, klikt u op **Opnieuw opbouwen oplossing**.

## <a name="run-the-application"></a>Voer de toepassing

2.  Druk op F5 om te maken en uitvoeren van de toepassing. De on-premises implementatie-server (de toepassing **ProductsServer** console) eerste moet beginnen, en vervolgens de toepassing **ProductsPortal** moet beginnen in een browservenster, zoals wordt weergegeven in de volgende schermopname. Zoals u ziet opnieuw de productinventaris lijsten gegevens opgehaald uit het systeem service on-premises implementatie, en worden deze gegevens weergegeven in de WebApp. Controleer de URL om ervoor te zorgen dat **ProductsPortal** wordt uitgevoerd in de cloud, als een Azure WebApp. 

    ![][1]

    > [AZURE.IMPORTANT] De **ProductsServer** console-toepassing moet actief zijn en mogen de gegevens met de toepassing **ProductsPortal** fungeren. Als de browser wordt een fout weergegeven, wacht u enkele meer seconden **ProductsServer** laden en het volgende bericht weergegeven. Druk vervolgens op **vernieuwen** in de browser.

    ![][37]

3. Terug in de browser, druk op de pagina **ProductsPortal** op **vernieuwen** . Telkens wanneer u de pagina vernieuwt, ziet u de server-app een bericht weergeven wanneer `GetProducts()` uit **ProductsServer** wordt genoemd.

    ![][38]

## <a name="next-steps"></a>Volgende stappen  

Meer informatie over de Service Bus, raadpleegt u de volgende bronnen:  

* [Azure-Service Bus][sbwacom]  
* [Het gebruik van de Service Bus wachtrijen][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Get-hulpprogramma's en SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

