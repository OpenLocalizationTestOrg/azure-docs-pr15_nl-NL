<properties 
    pageTitle="Service Bus brokered SMS .NET-zelfstudie | Microsoft Azure"
    description="Brokered SMS .NET-zelfstudie."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Service Bus brokered SMS .NET zelfstudie

Azure-Service Bus biedt twee uitgebreide SMS oplossingen – één, via een gecentraliseerde "relay" uitgevoerd in de cloud die ondersteuning biedt voor een groot aantal verschillende transportprotocollen en Web services standaarden, met inbegrip van SOAP, WS-*, en de REST. De client hoeft niet een directe verbinding met de on-premises implementatie-service en ook niet hoeft deze weten waar de service is geïnstalleerd en de on-premises implementatie-service hoeft niet alle binnenkomende poorten open op de firewall.

De tweede SMS oplossing stelt "brokered" mogelijkheden voor SMS-berichten. Deze kunnen worden beschouwd als asynchroon of ontkoppelde SMS-functies die ondersteuning publicatie-abonnementen, tijdelijke ontkoppeling en scenario's met de Service Bus SMS-infrastructuur van taakverdeling. Losgekoppelde communicatie heeft vele voordelen; clients en servers kunnen bijvoorbeeld verbinding maken met zo nodig en hun bewerkingen uitvoeren op asynchrone wijze.

Deze zelfstudie is bedoeld om aan te geven u een overzicht en praktijkervaring met wachtrijen, een van de belangrijkste onderdelen van Service Bus brokered messaging. Nadat u de reeks onderwerpen in deze zelfstudie werkt, hebt u een toepassing die wordt gevuld van een lijst met berichten en berichten verzendt naar die wachtrij Hiermee maakt u een wachtrij. Ten slotte de toepassing ontvangt en worden de berichten uit de wachtrij weergegeven en opruimen van de resources en afgesloten. Zie de [Service Bus doorgegeven SMS zelfstudie](../service-bus-relay/service-bus-relay-tutorial.md)voor een bijbehorende zelfstudie die wordt beschreven hoe u een toepassing die gebruikmaakt van de Service Bus Relay kunt maken.

## <a name="introduction-and-prerequisites"></a>Inleiding en vereisten

Wachtrijen bieden eerste keer hebt aangemeld, berichtbezorging eerste Out (FIFO) naar een of meer concurrerende consumenten. FIFO betekent dat berichten meestal naar verwachting worden ontvangen en verwerkt door de ontvangers in de tijdelijke volgorde waarin deze in wachtrij zijn, en elk bericht wordt ontvangen en verwerkt door consumenten slechts één bericht. Een belangrijk voordeel van het gebruik van wachtrijen is om te bereiken *tijdelijke ontkoppeling* van toepassingsonderdelen: met andere woorden, de producenten en consumenten hoeft te worden berichten verzenden en ontvangen op hetzelfde moment, omdat de berichten worden blijvend opgeslagen in de wachtrij. Een gerelateerde voordeel is *geladen effenen*, waarmee producenten en op consumenten verzenden en ontvangen van berichten op verschillende tarieven.

Hier volgen enkele administratieve en vereiste stappen die u volgen moet voordat u de zelfstudie starten. De eerste is een Servicenaamruimte maken en verkrijgen van een gedeelde toegangstoets handtekening (SA's). Een naamruimte bevat de grens van een toepassing voor elke toepassing weergegeven via de Service Bus. Een sleutel SA's wordt automatisch gegenereerd door het systeem wanneer de naamruimte van een service wordt gemaakt. De combinatie van de naamruimte van service en SA's sleutel biedt een referentie met die Service Bus wordt geverifieerd toegang tot een toepassing.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Een Servicenaamruimte maken en het verkrijgen van een sleutel SA 's

De eerste stap is een Servicenaamruimte maken en verkrijgen van een sleutel [Gedeeld Access handtekening](service-bus-sas-overview.md) (SA's). Een naamruimte bevat de grens van een toepassing voor elke toepassing weergegeven via de Service Bus. Een sleutel SA's wordt automatisch gegenereerd door het systeem wanneer de naamruimte van een service wordt gemaakt. De combinatie van de naamruimte van service en SA's sleutel biedt een referentie voor Service Bus om te verifiëren toegang tot een toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

De volgende stap is om te maken van een project Visual Studio en twee Help-functies die het laden van een door komma's gescheiden lijst met berichten in een object [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .NET [lijst](https://msdn.microsoft.com/library/6sh2ey19.aspx) ten zeerste getypt schrijven.

### <a name="create-a-visual-studio-project"></a>Een project Visual Studio maken

1. Open Visual Studio als een beheerder met de rechtermuisknop op het programma in het menu Start te klikken op **Als administrator uitvoeren**.

1. Maak een nieuw project van console-toepassing. Klik op het menu **bestand** en selecteert u daarna **Nieuw**en klik op **Project**. Klik in het dialoogvenster **Nieuw Project** op **Visual C#** (als **Visual C#** niet wordt weergegeven, zoek onder **Andere talen**), klik op de sjabloon **Console-toepassing** en noem deze **QueueSample**. Gebruik de standaardwaarde **locatie**. Klik op **OK** om het project te maken.

1. Gebruik de NuGet pakket manager de Service Bus bibliotheken toevoegen aan uw project:
    1. Met de rechtermuisknop op het project **QueueSample** in Solution Explorer en klik vervolgens op **NuGet-pakketten beheren**.
    2. Klik in het dialoogvenster **Nuget-pakketten beheren** klikt u op het tabblad **Bladeren** en zoekt **Azure Service Bus**en klikt u op **installeren**.
<br />
1. Dubbelklik in Solution Explorer op het bestand Program.cs om deze te openen in de Visual Studio-editor. Wijzig de naamruimtenaam van de van de standaardnaam van `QueueSample` naar `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Wijzig de `using` instructies zoals wordt weergegeven in de volgende code.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Maak een tekstbestand met de naam Data.csv en kopiëren in de volgende door komma's gescheiden tekst.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Opslaan en sluit het bestand Data.csv onthouden de locatie waar u deze hebt opgeslagen.

1. Klik in Solution Explorer met de rechtermuisknop op de naam van uw project (in dit voorbeeld **QueueSample**), klikt u op **toevoegen**en klik op **Bestaand Item**.

1. Blader naar het Data.csv-bestand dat u hebt gemaakt in stap 6. Klik op het bestand en klik op **toevoegen**. Zorg ervoor dat * *alle bestanden (*.*) ** is geselecteerd in de lijst met bestandstypen.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Een methode maken die parseert u een lijst met berichten

1. In de `Program` klasse voordat u de `Main()` methode twee variabelen declareren: een of meer **gegevenstabel**, aan de lijst met berichten in Data.csv bevatten. De andere moet zijn van het type lijstobject, ten zeerste aan [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)hebt getypt. De laatste is de lijst met brokered berichten die worden gebruikt door opeenvolgende stappen in de zelfstudie.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Buitenkant `Main()`, definiëren een `ParseCSV()` methode die de lijst met berichten in Data.csv parseert en laadt de berichten in een [gegevenstabel](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) tabel, zoals hier wordt getoond. De methode retourneert een **gegevenstabel** -object.

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. In de `Main()` methode, een verklaring dat u belt toevoegen de `ParseCSVFile()` methode:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Een methode maken die wordt geladen de lijst met berichten

1. Buitenkant `Main()`, definiëren een `GenerateMessages()` methode die geretourneerd wordt door de **gegevenstabel** -object `ParseCSVFile()` en laadt u de tabel in een lijst ten zeerste getypt met brokered berichten. De methode, retourneert de **lijst** object, zoals in het volgende voorbeeld. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. In `Main()`, direct achter de oproep door naar `ParseCSVFile()`, een verklaring dat u belt toevoegen de `GenerateMessages()` methode met de retourwaarde van `ParseCSVFile()` als een argument:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Gebruikersreferenties verkrijgen

1. U maakt eerst drie globale tekenreeksvariabelen waarin deze waarden. Deze variabelen declareren direct achter de vorige variabeledeclaraties vereist; bijvoorbeeld:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Maak vervolgens een functie die worden geaccepteerd en de naamruimte van service en SA's toets wordt opgeslagen. Deze methode buiten toevoegen `Main()`. Bijvoorbeeld: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. In `Main()`, direct achter de oproep door naar `GenerateMessages()`, een verklaring dat u belt toevoegen de `CollectUserInput()` methode:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Maak de oplossing

U kunt in het menu **maken** in Visual Studio, klikt u op **Oplossing maken** of druk op **Ctrl + Shift + B** om de nauwkeurigheid van uw werk dusverre bevestigen.

## <a name="create-management-credentials"></a>Management-referenties maken

In deze stap definieert u de management bewerkingen die u gebruiken wilt voor het maken van gedeelde toegang handtekening (SA's) referenties waarmee uw toepassing kan worden toegestaan.

1. Alle bewerkingen in de wachtrij worden in deze zelfstudie voor duidelijkheid plaatst in een afzonderlijke methode. Maken van een asynchrone `Queue()` methode in de `Program` klasse, als u na het `Main()` methode. Bijvoorbeeld:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. De volgende stap is het opzetten van een SA's referentie met behulp van een object [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) . De methode maken wordt op de naam van de SA's sleutel en waarde verkregen de `CollectUserInput()` methode. Voeg de volgende code aan de `Queue()` methode:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Maak een nieuw object in de management naamruimte, met een URI met de naamruimtenaam van de en de referenties in de vorige stap als argumenten verkregen. Hiermee voegt u deze code rechtstreeks na de code in de vorige stap hebt toegevoegd. Zorg ervoor dat voor het vervangen van `<yourNamespace>` met de naam van de naamruimte van uw service:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Voorbeeld

Nu ziet uw code er ongeveer als volgt uit:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Berichten verzenden naar de wachtrij

In deze stap kunt u een wachtrij maakt en vervolgens de berichten in de lijst met brokered berichten naar die wachtrij verzenden.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Wachtrij maken en verzenden van berichten naar de wachtrij

1. U maakt eerst de wachtrij. Bijvoorbeeld Roep deze `myQueue`, en deze declareren direct achter de management bewerkingen die u hebt toegevoegd in de `Queue()` methode in de laatste stap:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. In de `Queue()` methode, een SMS-berichten factory-object maken met een URI Service Bus gemaakte als een argument. Voeg de volgende code direct achter de management bewerkingen die u hebt toegevoegd in de laatste stap. Zorg ervoor dat voor het vervangen van `<yourNamespace>` met de naam van de naamruimte van uw service:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Maak vervolgens het object in de wachtrij met de klasse [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) . Voeg de volgende code direct achter de code die u in de laatste stap hebt toegevoegd:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Voeg nu code die de lijst met brokered berichten die u eerder hebt, gemaakt verzenden van elk naar de wachtrij doorlopen. Voeg de volgende code direct na de `CreateQueueClient()` -instructie in de vorige stap:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Berichten ontvangt van de wachtrij

In deze stap geeft u de lijst met berichten uit de wachtrij die u in de vorige stap hebt gemaakt.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Een ontvanger maken en u ontvangt berichten van de wachtrij

In de `Queue()` methode de wachtrij doorlopen en ontvangen met behulp van de methode [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) , afdrukken van elk bericht aan de console. Voeg de volgende code direct achter de code die u in de vorige stap hebt toegevoegd:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Houd er rekening mee dat het `Thread.Sleep` wordt alleen gebruikt voor het bericht simuleren verwerking en u waarschijnlijk wouldn't deze nodig hebt in een echte SMS-toepassing.

### <a name="end-the-queue-method-and-clean-up-resources"></a>De methode wachtrij beëindigen en de resources opschonen

Direct achter de vorige code, moet u de volgende code om het bericht fabriek en de wachtrij resources op te schonen toevoegen:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>De wachtrij aanroept

De laatste stap is het toevoegen van een instructie dat u belt de `Queue()` methode uit `Main()`. Voeg de volgende gemarkeerde regel met code aan het einde van Main():
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Voorbeeld

De volgende code bevat de volledige **QueueSample** -toepassing.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Maken en uitvoeren van de toepassing QueueSample

U kunt nu dat u de voorgaande stappen hebt uitgevoerd, maken en uitvoeren van de toepassing **QueueSample** .

### <a name="build-the-queuesample-application"></a>De toepassing QueueSample maken

Klik in Visual Studio, in het menu **maken** op **Build Solution**of druk op **Ctrl + Shift + B**. Als er fouten optreden, controleert u of uw code juist is op basis van het volledige voorbeeld gepresenteerd aan het einde van de vorige stap.

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie blijkt het maken van een Service Bus-client-toepassing en het gebruik van de Service Bus brokered SMS-mogelijkheden-service. Voor een soortgelijke zelfstudie die Service Bus [Relay](service-bus-messaging-overview.md#Relayed-messaging)gebruikt, raadpleegt u de [Service Bus doorgegeven SMS zelfstudie](../service-bus-relay/service-bus-relay-tutorial.md).

Zie de volgende onderwerpen voor meer informatie over de [Service Bus](https://azure.microsoft.com/services/service-bus/).

- [SMS-service Bus-overzicht](service-bus-messaging-overview.md)
- [Service Bus over grondbeginselen](service-bus-fundamentals-hybrid-solutions.md)
- [Service-busarchitectuur](service-bus-architecture.md)

