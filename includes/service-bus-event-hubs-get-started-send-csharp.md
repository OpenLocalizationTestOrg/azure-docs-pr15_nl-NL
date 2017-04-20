## <a name="send-messages-to-event-hubs"></a>Berichten verzenden naar gebeurtenis Hubs

In deze sectie, hebt u een Windows-console-app die gebeurtenissen naar uw Hub gebeurtenis schrijven.

1. Visual Studio, door een nieuw visueel C# bureaublad-App-project met de project-sjabloon van **Console-toepassing** te maken. De naam van de **afzender**van het project.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. In Solution Explorer met de rechtermuisknop op de oplossing en klik vervolgens op **NuGet-pakketten beheren voor oplossing**. 

3. Klik op het tabblad **Bladeren** en zoek vervolgens voor `Microsoft Azure Service Bus`. Zorg ervoor dat de naam van het project (**afzender**) is opgegeven in het vak **versie (s)** . Klik op **installeren**en accepteer de gebruiksvoorwaarden. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio is gedownload, installaties en voegt een verwijzing naar de [Azure Service Bus bibliotheek NuGet pakket](https://www.nuget.org/packages/WindowsAzure.ServiceBus).

4. Voeg de volgende `using` instructies boven aan het bestand **Program.cs** :

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. De volgende velden toevoegen aan de klas **programma** , wordt vervangen door de tijdelijke aanduiding voor waarden met de naam van de gebeurtenis Hub die u in de vorige sectie hebt gemaakt en de verbindingsreeks van naamruimte niveau die u eerder hebt opgeslagen.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. De volgende methode toevoegen aan de klas **programma** :

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Deze methode stuurt ononderbroken gebeurtenissen op de gebeurtenis-Hub met een vertraging 200-ms.

7. Tot slot toevoegen de volgende regels aan de methode **Main** :

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
