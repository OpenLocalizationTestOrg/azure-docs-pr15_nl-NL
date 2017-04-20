<properties 
    pageTitle="Hoe u met de API .NET Service Bus AMQP 1.0 | Microsoft Azure" 
    description="Informatie over het gebruik van geavanceerde bericht Queuing Protodol (AMQP) 1.0 met de API voor Bus van Azure .NET-Service." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>Hoe u AMQP 1.0 met de Service Bus .NET-API

De geavanceerde bericht Queuing-Protocol (AMQP) 1.0 is een efficiënte, betrouwbare, kabel niveau SMS protocol dat u gebruiken kunt om te maken van krachtige, platforms messaging toepassingen.

Ondersteuning voor AMQP 1,0 in Service Bus betekent dat u kunt het in de wachtrij gebruiken en publiceren/abonneren brokered functies berichten uit een reeks met een efficiënte binaire protocol platforms. Bovendien kunt u toepassingen bestaat uit onderdelen die zijn gemaakt met een combinatie van talen, kaders en besturingssystemen maken.

In dit artikel wordt uitgelegd hoe de Service Bus brokered SMS-functies gebruiken (wachtrijen en publiceren/abonneren onderwerpen) vanuit de Service Bus .NET-API gebruiken .NET-toepassingen. Er is een [companion artikel](service-bus-java-how-to-use-jms-api-amqp.md) waarin wordt uitgelegd hoe u de hetzelfde gebruikt de standaard Java bericht Service (JMS)-API. U kunt deze twee gidsen samen gebruiken voor meer informatie over meerdere platforms messaging AMQP 1.0 gebruiken.

## <a name="get-started-with-service-bus"></a>Aan de slag met Service Bus

In dit artikel wordt ervan uitgegaan dat u al een Service Bus naamruimte met een wachtrij met de naam "Wachtrij1." Als u niet het geval is, kunt klikt u vervolgens u de naamruimte en wachtrij met behulp van de [Azure-portal][]. Zie [aan de slag met Service Bus wachtrijen](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal)voor meer informatie over het maken van Service Bus naamruimten en wachtrijen.

## <a name="download-the-service-bus-sdk"></a>De Service-Bus SDK downloaden

AMQP 1.0-ondersteuning is beschikbaar in de Service Bus SDK versie 2.1 of hoger. U kunt de meest recente bits downloaden van NuGet bij [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/).

## <a name="code-net-applications"></a>Code .NET-toepassingen

Standaard wordt de bibliotheek van de client Service Bus .NET communiceert met de Service Bus-service met een speciale op basis van SOAP-protocol. Als u wilt AMQP 1.0 gebruiken in plaats van de standaard vereist protocol expliciete configuratie op de verbindingsreeks Service Bus zoals is beschreven in de volgende sectie. Dan deze wijziging ongewijzigd toepassingscode in principe bij gebruik van AMQP 1.0.

Er zijn een paar API-functies die niet worden ondersteund wanneer u een AMQP gebruikt in de huidige versie. Deze niet-ondersteunde functies worden later in de sectie [niet-ondersteunde functies en beperkingen](#unsupported-features-and-restrictions)weergegeven. Enkele van de geavanceerde configuratie-instellingen hebt ook een andere betekenis wanneer u een AMQP gebruikt. Geen van deze instellingen in dit artikel worden gebruikt, maar meer details zijn beschikbaar in de [Service Bus AMQP overzicht](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

### <a name="configure-via-appconfig"></a>Via App.config configureren

Het wordt aanbevolen dat toepassingen het configuratiebestand App.config gebruiken om op te slaan instellingen. Voor Bus Service-toepassingen, kunt u App.config voor de opslag van de Service Bus **ConnectionString**. In dit voorbeeld-toepassing wordt App.config ook gebruikt voor de opslag van de naam van de Service-Bus messaging entiteit die worden gebruikt.

Hier ziet u een voorbeeld van een App.config-bestand:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>De verbindingsreeks van de Service Bus configureren

De waarde van de instelling **Microsoft.ServiceBus.ConnectionString** is de verbindingsreeks van de Service Bus die wordt gebruikt voor het configureren van de verbinding met de Service Bus. De indeling is als volgt:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Waar `[namespace]` en `[SAS key]` van de [Azure portal][]worden opgehaald. Zie voor meer informatie, [het gebruik van de Service Bus wachtrijen] [].

Wanneer u met AMQP, de verbindingsreeks wordt toegevoegd met `;TransportType=Amqp`, waarin wordt uitgelegd de clientbibliotheek met de verbinding maken met de Service Bus AMQP 1.0 gebruiken.

### <a name="configure-the-entity-name"></a>De naam van de entiteit configureren

In dit voorbeeldtoepassing gebruikt de `EntityName` instellen in de sectie **appSettings** van het bestand App.config de naam van de wachtrij waarmee de toepassing berichten uitwisselt configureren.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Een eenvoudige .NET-toepassing met behulp van een Service Bus wachtrij

Het volgende voorbeeld verzendt en ontvangt berichten van en naar een Service Bus wachtrij.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>Voer de toepassing

Geeft een resultaat van het formulier uitvoeren van de toepassing:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Meerdere platforms messaging tussen JMS en .NET

In dit onderwerp het verzenden van berichten met .NET bus voor Service en ook hoe u deze ontvangen met behulp van .NET. Een van de belangrijkste voordelen van het AMQP 1.0 is echter dat deze kan worden opgebouwd uit onderdelen geschreven in verschillende talen, met berichten hebt uitgewisseld betrouwbaar en bij volledige beeldkwaliteit-toepassingen.

Gebruik van de steekproef .NET-toepassing die hierboven is beschreven en een soortgelijke Java-toepassing die u hebt gemaakt vanuit een handleiding, [het gebruik van de Java bericht Service (JMS) API met Service No & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md), is het mogelijk om berichten tussen .NET en Java te wisselen. 

Zie voor meer informatie over de details van meerdere platforms messaging Service Bus en AMQP 1.0 gebruiken, het [overzicht van de Service Bus AMQP 1.0](service-bus-amqp-overview.md).

### <a name="jms-to-net"></a>JMS naar .NET

Aantonen JMS .NET messaging:

* Start de toepassing van de steekproef .NET zonder opdrachtregelargumenten.
* Start de toepassing van de steekproef Java met de opdrachtregel argument "sendonly". In deze modus, de toepassing ontvangt geen berichten uit de wachtrij, wordt alleen verzonden.
* Druk op **Enter** malen de console Java-toepassing, waardoor de berichten worden verzonden.
* Deze berichten worden ontvangen door de .NET-toepassing.

### <a name="output-from-jms-application"></a>Uitvoer van JMS-toepassing

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>Uitvoer van .NET-toepassing

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.NET naar JMS

Aantonen .NET JMS messaging:

* Start de toepassing van de steekproef .NET met het argument van de opdrachtregel "sendonly". In deze modus, de toepassing ontvangt geen berichten uit de wachtrij, wordt alleen verzonden.
* Start de toepassing van de steekproef Java zonder opdrachtregelargumenten.
* Druk op **Enter** malen de console .NET-toepassing, waardoor de berichten worden verzonden.
* Deze berichten worden ontvangen door de Java-toepassing.

#### <a name="output-from-net-application"></a>Uitvoer van .NET-toepassing

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Uitvoer van JMS-toepassing

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Niet-ondersteunde functies en beperkingen

De volgende functies van de .NET Service Bus API worden momenteel niet ondersteund wanneer u een AMQP gebruikt:

 * Transacties
 * Via de bestemming van de bestandsoverdracht verzenden

Zie voor meer informatie, [niet-ondersteunde functies, beperkingen, en doorgevoerd verschillen](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Overzicht

In dit artikel werd hoe u de Service Bus brokered SMS-functies (wachtrijen en publiceren/abonneren onderwerpen) vanuit .NET AMQP 1.0 en de Service Bus .NET-API gebruiken.

U kunt ook Service Bus AMQP 1.0 van andere talen inclusief Java, C, Python en PHP. Onderdelen die zijn gemaakt met deze talen kunnen berichten uitwisselen betrouwbaar en bij volledige beeldkwaliteit AMQP 1.0 in Service Bus gebruiken. Zie [overzicht van de Service Bus AMQP](service-bus-amqp-dotnet.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een overzicht van Service Bus en AMQP met .NET hebt gelezen, raadpleegt u de volgende koppelingen voor meer informatie.

* [AMQP 1.0-ondersteuning in Azure Service Bus](service-bus-amqp-overview.md)
* [Het gebruik van de Java bericht Service (JMS) API met Service No & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [Het gebruik van de Service Bus wachtrijen](service-bus-dotnet-get-started-with-queues.md)
 
[Azure-portal]: https://portal.azure.com
