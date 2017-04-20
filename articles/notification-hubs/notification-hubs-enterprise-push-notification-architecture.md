<properties
    pageTitle="Melding Hubs - Push ondernemingsarchitectuur"
    description="Informatie over het gebruik van Azure melding Hubs in een bedrijfsomgeving"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>Enterprise push architectuur richtlijnen

Ondernemingen zijn vandaag geleidelijk verplaatsen naar het maken van mobiele toepassingen voor eindgebruikers (extern) of voor de werknemers (intern). Ze beschikken over bestaande backend systemen op hun plaats staan mainframes of sommige LoB-toepassingen die moeten worden geïntegreerd in de mobiele toepassingsarchitectuur. Deze handleiding wordt communiceren over een mogelijke moet deze integratie mogelijke oplossing naar veelvoorkomende scenario's aanbevelen.

Een veelgebruikte eis is voor het verzenden van push-bericht naar de gebruikers via hun mobiele toepassing als een gebeurtenis belangrijke in de backend-systemen. Bijvoorbeeld een bank-klant die van de bank bank app op haar iPhone heeft wil een melding ontvangen wanneer een betaalkaart bestaat uit de boven een bepaald bedrag van haar account of een intranet-scenario waar een medewerker van de afdeling Financiën wie een budget goedkeuring-app op zijn voor Windows Phone heeft wil een melding ontvangen wanneer ze een aanvraag tot goedkeuring ontvangt.

De bankrekening of goedkeuring verwerking is waarschijnlijk worden uitgevoerd in een back-end-mailsysteem waarin een push aan de gebruiker moet starten. Er zijn mogelijk meerdere dergelijke backend systemen die moeten alle hetzelfde soort logica willen implementeren push als een gebeurtenis een melding wordt maken. De complexiteit hier veroorzaakt door verschillende backend-systemen samen met een enkel push-systeem waar de eindgebruikers mogelijk hebt geabonneerd op andere meldingen en kunnen zelfs er meerdere mobiele toepassingen voorbeeld in geval van intranet mobiele apps waarin één mobiele toepassing mogelijk meldingen wilt ontvangen van meerdere zoals backend-systemen integreren. De backend-systemen niet kent of moet weten van push semantiek/technologie zodat een algemene oplossing hier traditioneel een onderdeel dat de back-end-systemen voor belangrijke gebeurtenissen worden opgevraagd en is verantwoordelijk is voor de push-berichten verzenden naar de client introduceren.
Hier wordt we het gaan hebben over een nog beter oplossing met Azure Service Bus - onderwerp/abonnement model waarmee de complexiteit terwijl u de oplossing scalable wordt beperkt.

Als volgt de algemene architectuur van de oplossing (generalized met meerdere mobiele apps maar gelijkmatig van toepassing als er slechts één mobiele app)

## <a name="architecture"></a>Architectuur

![][1]

Het belangrijkste stuk in dit diagram architectuur is Azure Service Bus waarmee een onderwerpen/abonnementen programming model (meer op is geïnstalleerd op de [Service Bus Publisher/Sub programming]). De ontvanger, namelijk in dit geval de mobiele end (meestal [Azure Mobile Service], die een push aan de mobiele apps initiëren) niet berichten ontvangt rechtstreeks vanuit de back-end-systemen, maar in plaats daarvan zijn er een tussenliggende abstraction laag verstrekt door [Azure Service Bus] waarmee mobiele backend moet ontvangen berichten van een of meer backend-systemen. Een Service Bus onderwerp moet worden gemaakt voor elk van de back-end systemen bijvoorbeeld Account, HR, financiën die in principe "onderwerpen" van belang dat berichten worden verzonden als push-bericht wordt gestart. De backend-systemen stuurt berichten naar deze onderwerpen. Een Backend Mobile kunt u abonneren op een of meer van deze onderwerpen door te maken van een Service Bus-abonnement. Dit recht geven de mobiele backend moet ontvangen een melding van het bijbehorende backend-systeem. Mobiele backend blijft luisteren naar berichten op hun abonnementen en zodra u een bericht binnenkomt dit terug verandert en deze als melding op de melding-hub. Melding hubs uiteindelijk levert vervolgens het bericht naar de mobiele app. Als u wilt samenvatten belangrijke onderdelen, hebben we dus:

1. Back-end-systemen (LoB/verouderd systeem)
    - Hiermee maakt u Service Bus onderwerp
    - Bericht verzendt
2. Mobiele backend
    - Hiermee maakt u serviceabonnement
    - Bericht ontvangt (van back-end-systeem)
    - Er wordt melding gestuurd naar clients (via Azure melding Hub)
3. Mobiele toepassing
    - Ontvangt en melding weergeven

###<a name="benefits"></a>Voordelen:

1. De ontkoppeling tussen de ontvanger (mobiele app/service via melding Hub) en de afzender (backend-systemen), kunt aanvullende backend-systemen wordt geïntegreerd met minimale wijzigen.
2. Hierdoor wordt de scenario voor meerdere mobiele apps gebeurtenissen uit een of meer backend-systemen ontvangen.  

## <a name="sample"></a>Voorbeeld:

###<a name="prerequisites"></a>Vereisten voor
U kunt de volgende zelfstudies vertrouwd met de concepten, evenals de algemene maken en de volgende configuratiestappen uit moet uitvoeren:

1. [Service Bus Publisher/Sub programming] - dit de details van het werken met abonnementen/Service Bus onderwerpen en wordt uitgelegd hoe u een naamruimte aanduiding onderwerpen/abonnementen, maakt het verzenden en ontvangen van berichten van deze.
2. [Melding Hubs - Windows universele zelfstudie] - dit wordt uitgelegd hoe u een Windows Store-app instellen en gebruiken van melding Hubs registreert en klik vervolgens op meldingen.

###<a name="sample-code"></a>Voorbeeld van een code

De volledige steekproef-code is beschikbaar op de [Melding Hub voorbeelden]. Deze opgedeeld in drie componenten:

1. **EnterprisePushBackendSystem**

    een. Dit project gebruikt de *WindowsAzure.ServiceBus* Nuget-pakket en is gebaseerd op de [Service Bus Publisher/Sub programming].

    b. Dit is een eenvoudige C#-console-app kunt u een LoB-systeem waarvan het bericht is bezorgd op de mobiele app start simuleren.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c. `CreateTopic`wordt gebruikt om te maken van het onderwerp Service Bus waar we berichten sturen.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`wordt gebruikt voor het verzenden van berichten naar deze Service Bus onderwerp. Hier we zijn alleen het verzenden van een reeks willekeurige berichten naar het onderwerp regelmatig voor de steekproef. Normaal gesproken wordt er een back endsysteem dat berichten worden verzonden wanneer een gebeurtenis plaatsvindt.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    een. Dit project gebruikt de *WindowsAzure.ServiceBus* en *Microsoft.Web.WebJobs.Publish* Nuget-pakketten en is gebaseerd op de [Service Bus Publisher/Sub programming].

    b. Dit is een andere C#-console app dat we als een [Azure WebJob] wordt uitgevoerd, omdat er doorlopend te beluisteren voor berichten van de LoB/backend-systemen. Dit is deel van uw mobiele backend.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c. `CreateSubscription`wordt gebruikt om te maken van een Service Bus-abonnement voor het onderwerp waar het systeem backend berichten worden verzonden. Afhankelijk van het bedrijfsscenario maakt dit onderdeel een of meer abonnementen op de bijbehorende onderwerpen (bijvoorbeeld enkele mogelijk worden berichten van ontvangen HR systeem, wordt het aantal uit Financiën systeem, enzovoort)

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. ReceiveMessageAndSendNotification wordt gebruikt voor het bericht in het onderwerp met het abonnement lezen en als het lezen gelukt is vervolgens een melding (in het voorbeeldscenario een systeemeigen mailpop-upmelding voor Windows) om te worden verzonden naar het mobiele-toepassing via Azure melding Hubs Maak.

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    e. Voor het publiceren van dit als een **WebJob**, klik met de rechtermuisknop op de oplossing in Visual Studio en selecteer **publiceren als WebJob**

    ![][2]

    f. Selecteer uw publicatie profiel en maak een nieuwe Azure-WebSite als deze nog niet bestaat die wordt gehost in deze WebJob en nadat u het WebSite en vervolgens **Publiceren hebt**.

    ![][3]

    g. Het configureren van de taak om te worden "Continu uitvoeren" zodat wanneer u zich bij de [Portal van Azure klassieke aanmelden] moet er ongeveer als volgt te werk:

    ![][4]


3. **EnterprisePushMobileApp**

    een. Dit is een Windows Store-toepassing waarin wordt mailpop-meldingen ontvangt van de WebJob uitgevoerd als onderdeel van uw mobiele backend en weer te geven. Dit is gebaseerd op de [Melding Hubs - Windows universele zelfstudie].  

    b. Zorg ervoor dat uw toepassing is ingeschakeld mailpop-meldingen wilt ontvangen.

    c. Zorg ervoor dat de volgende melding Hubs registratiecode wordt aangeroepen bij de App opstart (na het *HubName* en *DefaultListenSharedAccessSignature*vervangen:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Voorbeeld gestart:

1. Zorg ervoor dat uw WebJob met succes wordt uitgevoerd en de planning moet 'Continu uitvoeren'.
2. Voer de **EnterprisePushMobileApp** waarin de Windows Store-app wordt gestart.
3. Voer de **EnterprisePushBackendSystem** -console-toepassing waarin de LoB-end simuleren en wordt beginnen met het verzenden van berichten en ziet u mailpop meldingen weergegeven als volgt uit:

    ![][5]

4. De berichten worden oorspronkelijk verzonden naar Service Bus onderwerpen die is wordt gevolgd door de Service Bus abonnementen in uw Web-taak. Wanneer een bericht is ontvangen, is een melding gemaakt en verzonden naar de mobiele app. U kunt Kijk in de logboeken WebJob om te bevestigen de verwerking wanneer u gaat u naar de koppeling Logboeken in [Klassieke Azure-Portal] voor uw Project Web:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Melding Hub voorbeelden]: https://github.com/Azure/azure-notificationhubs-samples
[Azure Mobile Service]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure-Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Publisher/Sub hoeft te programmeren]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Melding Hubs - Windows universele zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure klassieke Portal]: https://manage.windowsazure.com/
