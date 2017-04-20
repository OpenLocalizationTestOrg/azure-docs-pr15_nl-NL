<properties
    pageTitle=".NET meerlagige toepassing | Microsoft Azure"
    description="Een .NET-zelfstudie waarmee u een app met meerdere niveaus in Azure wordt aangegeven waarin Service Bus wachtrijen voor de communicatie tussen lagen ontwikkelen."
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
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>.NET met meerdere niveaus-toepassing via van Azure Service Bus wachtrijen

## <a name="introduction"></a>Inleiding

Ontwikkelen voor Microsoft Azure is gemakkelijk met behulp van Visual Studio en de gratis Azure-SDK voor .NET. Deze zelfstudie leert u de stappen voor het maken van een toepassing die meerdere Azure resources uitvoert in uw lokale omgeving gebruikt. De stappen wordt ervan uitgegaan dat u hebt geen ervaring met Azure.

Leert u het volgende:

-   Het inschakelen van uw computer voor Azure ontwikkeling met een enkel downloaden en installeren.
-   Het gebruik van Visual Studio ontwikkelen voor Azure.
-   Het maken van een toepassing met meerdere niveaus in Azure wordt aangegeven met web en werknemer rollen.
-   Hoe u de communicatie tussen lagen Service Bus wachtrijen gebruiken.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

In deze zelfstudie u maken en uitvoeren van de toepassing met meerdere niveaus in een Azure-cloudservice. De front-end is een ASP.NET-MVC Webrol en wordt de back-end een werknemer-rol die gebruikmaakt van een Service Bus wachtrij. Als een webproject dat is geïmplementeerd in een Azure-website in plaats van een cloudservice, kunt u dezelfde meerlagige toepassing maken met de front-end. Zie de sectie van de [volgende stappen](#nextsteps) voor instructies over wat u moet doen anders op een front-end van Azure-website. U kunt ook de [toepassing voor .NET-op-premises/cloud hybride](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) zelfstudie uitproberen.

De volgende schermafbeelding ziet u de voltooide toepassing.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Scenario-overzicht: de communicatie tussen rol

Als u wilt een bestelling voor verwerking verzendt, moet het front UI-component worden uitgevoerd in de Webrol communiceren met de middelste laag logica uitgevoerd in de rol werknemer. In dit voorbeeld wordt de Service Bus brokered messaging voor de communicatie tussen de lagen gebruikt.

Gebruik brokered messaging tussen het web en middelste lagen decouples de twee onderdelen. In tegenstelling tot directe messaging (dat wil zeggen, TCP of HTTP) verbinding de weblaag geen met de middelste laag rechtstreeks; in plaats daarvan deze werkeenheden, worden als berichten in Service Bus, die betrouwbaar behoudt totdat de middelste laag gereed is voor gebruik en deze te verwerken.

Service Bus biedt twee entiteiten om te ondersteunen brokered messaging: wachtrijen en onderwerpen. Met wachtrijen, wordt door een enkele ontvanger elk bericht verzonden naar de wachtrij gebruikt. Onderwerpen ondersteuning voor het publiceren/abonneren patroon waarin elke gepubliceerde bericht beschikbaar wordt gemaakt voor een abonnement dat is geregistreerd bij het onderwerp. Elk abonnement onderhoudt logisch zijn eigen wachtrij van berichten. Abonnementen kunnen ook worden geconfigureerd met filterregels die Beperk het aantal berichten doorgegeven aan de wachtrij abonnement naar die overeenkomen met het filter. Het volgende voorbeeld wordt de Service Bus wachtrijen.

![][1]

Deze methode communicatie heeft verschillende voordelen ten opzichte van directe messaging:

-   **Tijdelijke ontkoppeling.** Met het asynchrone SMS patroon, producenten en consumenten worden online op hetzelfde moment. Service Bus betrouwbaar berichten worden opgeslagen totdat de in beslag nemen partij gereed is voor ontvangen. Hiermee worden de onderdelen van de gedistribueerde toepassing beëindigd, hetzij uit eigen beweging, bijvoorbeeld voor onderhoud of vanwege onderdeel vastlopen, zonder die invloed hebben op het systeem als geheel. Bovendien de toepassing in beslag nemen alleen moet mogelijk bepaalde momenten van de dag online komt.

-   **Laad effenen.** In veel toepassingen varieert systeem laden na verloop van tijd terwijl de verwerkingstijd die is vereist voor elk exemplaar van werk meestal constante is. Intermediating bericht producenten en consumenten met een wachtrij betekent dat de in beslag nemen toepassing (de werknemer) alleen moet worden deze is ingericht zodat gemiddelde laden in plaats van piek laden. De diepte van de wachtrij in omvang groeit en opdrachten terwijl de binnenkomende laden varieert. Hiermee slaat u rechtstreeks geld wat betreft de hoeveelheid infrastructuur die nodig zijn voor het laden van de toepassing.

-   **Taakverdeling.** Als toename hebt geladen, kunnen meer werknemer-processen worden toegevoegd aan in de wachtrij lezen. Elk bericht wordt verwerkt door één van de werknemer-processen. Bovendien kunt deze halen gebaseerde taakverdeling optimaal gebruik van de werknemer machines zelfs als de werknemer machines tussen verwerking power verschillen, terwijl ze worden berichten op hun eigen maximumsnelheid halen. Dit patroon wordt vaak het patroon *dat consumenten* genoemd.

    ![][2]

De volgende secties worden de code die deze architectuur implementeert.

## <a name="set-up-the-development-environment"></a>De ontwikkelomgeving instellen

Voordat u met het ontwikkelen van Azure-toepassingen beginnen kunt, krijgen van de hulpmiddelen en stelt u uw ontwikkelomgeving.

1.  Installeer de Azure SDK voor .NET bij [hulpprogramma's en SDK ophalen][].

2.  Klik op **de SDK installeren** voor de versie van Visual Studio u gebruikt. De stappen in deze zelfstudie Gebruik Visual Studio-2015.

4.  Wanneer u hierom wordt gevraagd wilt uitvoeren of opslaan van het installatieprogramma, klikt u op **uitvoeren**.

5.  Klik in het **Installatieprogramma van de Web-Platform**, klikt u op **installeren** en doorgaan met de installatie.

6.  Als de installatie voltooid is, hebt u alles nodig is om te beginnen met het ontwikkelen van de app. De SDK bevat hulpmiddelen waarmee u gemakkelijk Azure toepassingen ontwikkelen in Visual Studio. Als u nog geen Visual Studio is geïnstalleerd, installeert de SDK ook de gratis Visual Studio Express.

## <a name="create-a-namespace"></a>Een naamruimte maken

De volgende stap is voor het maken van de naamruimte van een service en verkrijgen van een sleutel gedeeld Access handtekening (SA's). Een naamruimte bevat de grens van een toepassing voor elke toepassing weergegeven via de Service Bus. Een sleutel SA's wordt gegenereerd door het systeem wanneer een naamruimte wordt gemaakt. De combinatie van naamruimte en SA's sleutel biedt de referenties voor de Service Bus om te verifiëren toegang tot een toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Een Webrol maken

In deze sectie, kunt u de front-end van de toepassing maken. U maakt eerst de pagina's die uw toepassing wordt weergegeven.
Voeg daarna code indient items aan een Service Bus wachtrij en wordt statusinformatie weergegeven over de wachtrij.

### <a name="create-the-project"></a>Het project maken

1.  Start Microsoft Visual Studio met behulp van beheerdersbevoegdheden. Als u wilt beginnen Visual Studio met beheerdersbevoegdheden, met de rechtermuisknop op het programmapictogram **Visual Studio** en klik vervolgens op **Als administrator uitvoeren**. De emulator Azure berekeningscluster, verderop in dit artikel wordt beschreven is vereist dat Visual Studio met beheerdersbevoegdheden worden gestart.

    In Visual Studio, klik in het menu **bestand** op **Nieuw**en klik vervolgens op **Project**.

2.  Klik op de **Cloud** van **Geïnstalleerde sjablonen**, onder **Visual C#**, en klik op **Azure-Cloudservice**. De naam van het project **MultiTierApp**. Klik vervolgens op **OK**.

    ![][9]

3.  **.NET Framework 4.5** rol, dubbelklikt u op **ASP.NET Webrol**.

    ![][10]

4.  Plaats de muisaanwijzer op **WebRole1** onder **oplossing Azure-Cloudservice**, klik op het potloodpictogram en wijzig de naam van de Webrol **FrontendWebRole**. Klik vervolgens op **OK**. (Zorg ervoor dat u "Frontend" met een kleine letters 'e,' niet 'FrontEnd' invoert.)

    ![][11]

5.  Klik in het dialoogvenster **Nieuw ASP.NET-Project** in de lijst **Selecteer een sjabloon** op **MVC**.

    ![][12]

6. Klik op de knop **Wijzigen verificatie** nog steeds in het dialoogvenster **Nieuw ASP.NET-Project** . In het dialoogvenster **Verificatie wijzigen** , klikt u op **Geen verificatie**en klik vervolgens op **OK**. Voor deze zelfstudie, bent u een app die niet nodig voor een gebruikersaanmelding hebt implementeren.

    ![][16]

7. Klik op **OK** om het project te maken in het dialoogvenster **Nieuw ASP.NET-Project** .

6.  In **Solution Explorer**in het project **FrontendWebRole** , met de rechtermuisknop op **verwijzingen**en klik **NuGet-pakketten beheren**.

7.  Klik op het tabblad **Bladeren** en zoek vervolgens voor `Microsoft Azure Service Bus`. Klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![][13]

    Houd er rekening mee dat nu wordt verwezen naar de vereiste client stroombaan en enkele nieuwe codebestanden zijn toegevoegd.

9.  Met de rechtermuisknop op **modellen** in **Solution Explorer**en klikt u op **toevoegen**en klik op **Class**. Typ in het vak **naam** de naam **OnlineOrder.cs**. Klik vervolgens op **toevoegen**.

### <a name="write-the-code-for-your-web-role"></a>De programmacode voor uw Webrol schrijven

In dit gedeelte maakt u de verschillende pagina's die uw toepassing wordt weergegeven.

1.  In het bestand OnlineOrder.cs in Visual Studio, kunt u de bestaande naamruimtedefinitie vervangen door de volgende code:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  Dubbelklik op **Controllers\HomeController.cs**in **Solution Explorer**. De volgende instructies voor het **gebruik van** toevoegen boven aan het bestand naar de naamruimten voor het model die u zojuist hebt gemaakt, evenals Service Bus opnemen.

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  Ook in het bestand HomeController.cs in Visual Studio, de bestaande naamruimtedefinitie vervangen door de volgende code. Deze code bevat methoden voor het indienen van items naar de wachtrij voor de verwerking.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  Klik op **Oplossing maken** om de nauwkeurigheid van uw werk dusverre testen in het menu **maken** .

5.  Maak nu de weergave voor de `Submit()` methode die u eerder hebt gemaakt. Klik met de rechtermuisknop binnen de `Submit()` methode (de overbelasting van `Submit()` die geen parameters nodig heeft), en kies vervolgens **Weergave toevoegen**.

    ![][14]

6.  Er verschijnt een dialoogvenster voor het maken van de weergave. Kies in de lijst **sjabloon** **maken**. Klik op de class **OnlineOrder** in de lijst **Model class** .

    ![][15]

7.  Klik op **toevoegen**.

8.  De weergegeven naam van de toepassing nu wijzigen. Dubbelklik in **Solution Explorer**op de **Views\Shared\\_Layout.cshtml** bestand om dit te openen in de Visual Studio-editor.

9.  Vervang alle exemplaren van **Mijn ASP.NET-toepassing** door **van LITWARE-producten**.

10. De koppelingen **Home**, **over**en **contactpersoon** verwijderen. Verwijder de gemarkeerde code:

    ![][28]

11. Ten slotte de indiening-pagina als u wilt opnemen, vindt u informatie over de wachtrij wijzigen. Dubbelklik op het bestand **Views\Home\Submit.cshtml** om deze te openen in de Visual Studio-editor in **Solution Explorer**. Voeg de volgende regel na `<h2>Submit</h2>`. Nu de `ViewBag.MessageCount` leeg is. U nog deze later vullen.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. U hebt nu uw UI geïmplementeerd. U kunt druk op **F5** aan uw toepassing uitvoeren en Bevestig dat het lijkt erop zoals verwacht.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>De code voor het indienen van items aan een Service Bus wachtrij schrijven

Nu code voor het indienen van items aan een wachtrij toevoegen. U maakt eerst een klasse die de verbindingsgegevens van uw Service Bus-wachtrij bevat. Vervolgens uw verbinding uit Global.aspx.cs geïnitialiseerd. Tot slot bijwerken de indiening-code die u eerder hebt gemaakt in HomeController.cs om in te dienen daadwerkelijk items aan een Service Bus wachtrij.

1.  Klik in **Solution Explorer**met de rechtermuisknop op **FrontendWebRole** (met de rechtermuisknop op het project, niet de rol). Klik op **toevoegen**en klik vervolgens op **Class**.

2.  De naam van de klasse **QueueConnector.cs**. Klik op **toevoegen** om de klasse te maken.

3.  Voeg nu code bevat de verbindingsgegevens die en wordt de verbinding met een Service Bus wachtrij geïnitialiseerd. De volledige inhoud van QueueConnector.cs vervangen door de volgende code en voer waarden in voor `your Service Bus namespace` (uw naamruimtenaam) en `yourKey`, namelijk de **primaire sleutel** die u eerder hebt aangeschaft bij de portal van Azure.

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Nu kunt u zorgen dat uw methode **geïnitialiseerd** wordt aangeroepen. Dubbelklik op **Global.asax\Global.asax.cs**in **Solution Explorer**.

5.  De volgende regel met code toevoegen aan het einde van de methode **Application_Start** .

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Ten slotte de Webcode die u eerder hebt gemaakt, om in te dienen items naar de wachtrij bijwerken. Dubbelklik op **Controllers\HomeController.cs**in **Solution Explorer**.

7.  Update van de `Submit()` methode (de overbelasting die geen parameters) als volgt om het aantal berichten voor de wachtrij.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Update van de `Submit(OnlineOrder order)` methode (de overbelasting waarmee één parameter) als volgt om in te dienen ordergegevens naar de wachtrij.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  U kunt nu de toepassing opnieuw uitvoeren. Telkens wanneer die u een order, verzendt het aantal berichten wordt verhoogd.

    ![][18]

## <a name="create-the-worker-role"></a>De rol werknemer maken

Nu maakt u de rol van werknemer waarmee de volgorde ingediende items worden verwerkt. In dit voorbeeld wordt de **Rol van de werknemer met Service Bus wachtrij** Visual Studio project-sjabloon gebruikt. U de vereiste referenties al hebt aangeschaft bij de portal.

1. Controleer of dat Visual Studio zijn verbonden met uw Azure-account.

2.  In Visual Studio **Solution Explorer** met de rechtermuisknop op de map **rollen** onder het project **MultiTierApp** .

3.  Klik op **toevoegen**en klik vervolgens op **Nieuwe werknemer rol Project**. Het dialoogvenster **Nieuw rol-Project toevoegen** wordt weergegeven.

    ![][26]

4.  Klik op **De rol van de werknemer met Service Bus wachtrij**in het dialoogvenster **Nieuw rol-Project toevoegen** .

    ![][23]

5.  Geef in het vak **naam** een naam **OrderProcessingRole**van het project. Klik vervolgens op **toevoegen**.

6.  De verbindingsreeks die u hebt gekregen in stap 9 van de sectie 'Een naamruimte Service Bus maken' naar het Klembord kopiëren.

7.  Klik in **Solution Explorer**met de rechtermuisknop op de **OrderProcessingRole** die u hebt gemaakt in stap 5 (Zorg ervoor dat u met de rechtermuisknop op **OrderProcessingRole** onder **rollen**en niet de klas). Klik vervolgens op **Eigenschappen**.

8.  Klik op het tabblad **Instellingen** van het dialoogvenster **Eigenschappen** en klik in het vak **waarde** voor **Microsoft.ServiceBus.ConnectionString**en plak de eindpunt-waarde die u hebt gekopieerd in stap 6.

    ![][25]

9.  Een klasse **OnlineOrder** bevatten de orders tijdens het verwerken van deze uit de wachtrij te maken. U kunt een klasse die u al hebt gemaakt opnieuw gebruiken. **Oplossingverkenner**met de rechtermuisknop op de klas **OrderProcessingRole** (met de rechtermuisknop op het pictogram van class, niet de rol). Klik op **toevoegen**en klik op **Bestaand Item**.

10. Blader naar de submap voor **FrontendWebRole\Models**en dubbelklik vervolgens op **OnlineOrder.cs** toe te voegen aan dit project.

11. In **WorkerRole.cs**, wijzigt u de waarde van de variabele **wachtrijnaam** uit `"ProcessingQueue"` naar `"OrdersQueue"` zoals wordt weergegeven in de volgende code.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Voeg de volgende met de instructie boven aan het bestand WorkerRole.cs.

    ```
    using FrontendWebRole.Models;
    ```

13. In de `Run()` , functie, binnen de `OnMessage()` bellen, vervangen de inhoud van de `try` component met de volgende code.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. U kunt de toepassing hebt voltooid. U kunt de volledige toepassing testen door met de rechtermuisknop op het project MultiTierApp in Solution Explorer, **instellen als opstarten voor een Project**te selecteren en drukt u op F5. Houd er rekening mee dat het aantal berichten niet wordt verhoogd, omdat de voor- en deze als voltooid markeert items uit de wachtrij verwerkt de rol werknemer. Ziet u de trace-uitvoer van uw rol werknemer door te bekijken van de gebruikersinterface van Azure berekenen Emulator. U kunt dit doen door met de rechtermuisknop op het pictogram emulator in het systeemvak van de taakbalk en **Berekenen Emulator-gebruikersinterface weergeven**te selecteren.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Volgende stappen  

Meer informatie over de Service Bus, raadpleegt u de volgende bronnen:  

* [Azure-Service Bus][sbmsdn]  
* [Pagina van de service-Service Bus][sbwacom]  
* [Het gebruik van de Service Bus wachtrijen][sbwacomqhowto]  

Meer informatie over scenario's met meerdere niveaus, raadpleegt u:  

* [.NET met meerdere niveaus-toepassing via opslag tabellen, wachtrijen en BLOB 's][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Get-hulpprogramma's en SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  