<properties 
    pageTitle="AMQP 1.0 gebruiken met de Java Service Bus API | Microsoft Azure" 
    description="Informatie over het gebruik van de Java bericht Service (JMS) met Azure Service Bus en geavanceerde bericht wachtrij plaatsen"
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"  
    manager="timlt" 
    editor="" />

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Het gebruik van de Java bericht Service (JMS) API met Service Bus en AMQP 1.0

De geavanceerde bericht Queuing-Protocol (AMQP) 1.0 is een efficiënte, betrouwbare, kabel niveau SMS protocol dat u gebruiken kunt om te maken van krachtige, platforms messaging toepassingen. AMQP 1.0-ondersteuning is toegevoegd aan Azure Service Bus in oktober 2012 en overgebracht naar algemene beschikbaarheid (GA) in mei 2013.

De toevoeging van AMQP 1.0 houdt in dat nu kunt u gebruikmaken van het in de wachtrij en publiceren/abonneren brokered SMS functies van Service Bus via een bereik van platforms met behulp van een efficiënte binaire protocol. Bovendien kunt u toepassingen bestaat uit onderdelen die zijn gemaakt met een combinatie van talen, kaders en besturingssystemen maken.

Deze Procedurebeschrijving wordt uitgelegd hoe de Service Bus brokered SMS-functies gebruiken (wachtrijen en publiceren/abonneren onderwerpen) vanuit Java-toepassingen met de populaire Java bericht Service (JMS) API-standaard.

## <a name="get-started-with-service-bus"></a>Aan de slag met Service Bus

Deze handleiding wordt ervan uitgegaan dat u al een Service Bus naamruimte die een wachtrij met de naam **Wachtrij1**bevat. Als u niet het geval is, klikt u vervolgens kunt u [de naamruimte en wachtrij maken](service-bus-create-namespace-portal.md) met behulp van de [Azure-portal](https://portal.azure.com). Zie voor meer informatie over het maken van Service Bus naamruimten en wachtrijen voor [het gebruik van de Service Bus wachtrijen](service-bus-dotnet-get-started-with-queues.md).

### <a name="downloading-the-amqp-10-jms-client-library"></a>De bibliotheek van de client AMQP 1.0 JMS downloaden

Voor informatie over waar u de nieuwste versie van de bibliotheek Apache Qpid JMS AMQP 1.0-client downloaden, gaat u naar [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

U moet de volgende vier oppervlak-bestanden vanuit het archief van de verdeling Apache Qpid JMS AMQP 1.0 toevoegen aan het Java-KLASSENPAD wanneer bouwen en uit te voeren JMS toepassingen met Service Bus:

*    geronimo-jms\_1.1\_specificatie-1.0.jar
*    qpid-amqp-1-0-client-[Version].jar
*    qpid-amqp-1-0-client-jms-[Version].jar
*    qpid-amqp-1-0-Common-[Version].jar

## <a name="coding-java-applications"></a>Kleurcodering Java-toepassingen

### <a name="java-naming-and-directory-interface-jndi"></a>Directory-Interface (JNDI) en een Java naam geven

JMS wordt de Java en een naam geven Directory-Interface (JNDI) om te maken van een scheiding tussen de namen van de logische en fysieke. Twee typen JMS objecten worden omgezet met JNDI: ConnectionFactory- en doeltabellen. JNDI gebruikt een providermodel waarin u de verschillende services voor het verwerken van naam resolutie rechten kunt aansluiten. De Apache Qpid JMS AMQP 1.0-bibliotheek wordt geleverd met een eenvoudige eigenschappen op basis van een bestand JNDI Provider die is geconfigureerd met behulp van een eigenschappenbestand van de volgende waarden opmaken:

```
# servicebus.properties – sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[username]:[password]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>De ConnectionFactory configureren

De invoer gebruikt voor het definiëren van een **ConnectionFactory** in de Qpid eigenschappen bestand JNDI Provider heeft de volgende indeling:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

**[Jndi_name]** en **[ConnectionURL]** waar de volgende betekenis hebben:

- **[jndi_name]**: de logische naam van de ConnectionFactory. Dit is de naam die in de Java-toepassing met behulp van de methode JNDI IntialContext.lookup() wordt opgelost.
- **[ConnectionURL]**: een URL die de bibliotheek JMS met de vereiste gegevens tot de makelaar AMQP biedt.

De opmaak van de **ConnectionURL** is als volgt:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Waar **[naamruimte]**, **[gebruikersnaam]** en **[wachtwoord]** de volgende betekenis heeft:

- **[naamruimte]**: de Service Bus naamruimte.
- **[gebruikersnaam]**: de naam van de Service Bus uitgever.
- **[wachtwoord]**: formulier URL-codering van de Service Bus uitgever-toets.

> [AZURE.NOTE] U moet URL-codering het wachtwoord handmatig. Er is een handige hulpprogramma voor het URL-codering beschikbaar op [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

#### <a name="configure-destinations"></a>Bestemmingen configureren

De invoer gebruikt om te definiëren van een bestemming in de Qpid eigenschappen bestand JNDI Provider heeft de volgende indeling:

```
queue.[jndi_name] = [physical_name]
```
of

```
topic.[jndi_name] = [physical_name]
```

Waar **[jndi\_naam]** en **[fysieke\_naam]** heeft de volgende betekenis:

- **[jndi_name]**: de logische naam van de bestemming. Dit is de naam die in de Java-toepassing met behulp van de methode JNDI IntialContext.lookup() wordt opgelost.
- **[physical_name]**: de naam van de Service Bus entiteit waaraan de toepassing berichten verzendt of ontvangt.

> [AZURE.NOTE] Wanneer het ontvangen van een Service Bus onderwerp-abonnement, moet de fysieke in JNDI opgegeven naam de naam van het onderwerp. Naam van het abonnement is opgegeven als het duurzame abonnement wordt gemaakt in de code van de toepassing JMS. De [handleiding voor ontwikkelaars van de Service Bus AMQP 1.0](service-bus-amqp-dotnet.md) bevat meer informatie over het werken met Service Bus onderwerp abonnementen van JMS.

### <a name="write-the-jms-application"></a>De toepassing JMS schrijven

Er zijn geen speciale API's of wanneer u JMS met Service Bus benodigde opties. Er zijn echter enkele beperkingen die later aan bod. Met een JMS-toepassing wordt de eerst te beginnen vereiste configuratie van de omgeving JNDI, kunnen een **ConnectionFactory** en bestemmingen op te lossen.

#### <a name="configure-the-jndi-initialcontext"></a>De InitialContext JNDI configureren

De JNDI-omgeving is geconfigureerd door een hashtable configuratie-informatie doorgeven aan de constructor van de klasse javax.naming.InitialContext. De twee vereiste elementen in de hashtable zijn de klassenaam van de eerste Context fabriek en de URL van de Provider. De volgende code laat zien hoe de omgeving JNDI het bestand Qpid-eigenschappen configureert JNDI Provider met een eigenschappenbestand met de naam **servicebus.properties**gebaseerd.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Een eenvoudige JMS-toepassing met behulp van een Service Bus wachtrij

Het volgende voorbeeldprogramma JMS TextMessages naar een Service Bus wachtrij met de logische naam JNDI van wachtrij verzendt en ontvangt de berichten weer.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Lookup ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>Voer de toepassing

De toepassing geeft een het volgende resultaat:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Meerdere platforms messaging tussen JMS en .NET

Deze handleiding blijkt hoe verzenden en ontvangen van berichten van en naar de Service Bus JMS gebruiken. Een van de belangrijkste voordelen van het AMQP 1.0 is echter dat deze kan worden opgebouwd uit onderdelen geschreven in verschillende talen, met berichten hebt uitgewisseld betrouwbaar en bij volledige beeldkwaliteit-toepassingen.

Gebruik van de steekproef JMS toepassing hierboven beschreven en een soortgelijke .NET-toepassing die u hebt gemaakt vanuit een handleiding, [hoe u AMQP 1.0 met de .NET Service Bus .NET-API gebruikt](service-bus-dotnet-advanced-message-queuing.md), kunt u berichten tussen .NET en Java uitwisselen. 

Zie voor meer informatie over de details van meerdere platforms messaging met Service Bus en AMQP 1.0, de [Service Bus AMQP 1.0 handleiding voor ontwikkelaars](service-bus-amqp-dotnet.md).

### <a name="jms-to-net"></a>JMS naar .NET

Aantonen JMS .NET messaging:

* Start de toepassing van de steekproef .NET zonder opdrachtregelargumenten.
* Start de toepassing van de steekproef Java met de opdrachtregel argument "sendonly". In deze modus, de toepassing ontvangt geen berichten uit de wachtrij, wordt alleen verzonden.
* Druk op **Enter** malen de console Java-toepassing, waardoor de berichten worden verzonden.
* Deze berichten worden ontvangen door de .NET-toepassing.

#### <a name="output-from-jms-application"></a>Uitvoer van JMS-toepassing

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Uitvoer van .NET-toepassing

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET naar JMS

Aantonen .NET JMS messaging:

* Start de toepassing van de steekproef .NET met de opdrachtregel argument "sendonly". In deze modus, de toepassing ontvangt geen berichten uit de wachtrij, wordt alleen verzonden.
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

De volgende beperkingen zijn bij gebruik van JMS via AMQP 1.0 met Service Bus, namelijk:

* Slechts één **MessageProducer** of **MessageConsumer** is per **sessie**toegestaan. Als u maken van meerdere **MessageProducers** of **MessageConsumers** in een toepassing wilt, maakt u een speciale **sessie** voor elk van deze.
* Vluchtige onderwerp abonnementen worden momenteel niet ondersteund.
* **MessageSelectors** worden momenteel niet ondersteund.
* Tijdelijke bestemmingen, dat wil zeggen **TemporaryQueue**, **TemporaryTopic** worden momenteel niet ondersteund, samen met de **QueueRequestor** en **TopicRequestor** API's die ze gebruiken.
* Uitgevoerde sessies en gedistribueerde transacties worden niet ondersteund.

## <a name="summary"></a>Overzicht

In dit artikel werd het gebruik van de Service Bus messaging functies (wachtrijen en publiceren/abonneren onderwerpen) uit Java gebruik van de populaire JMS API en AMQP 1.0.

U kunt ook Service Bus AMQP 1.0 gebruiken in andere talen, waaronder .NET, C, Python en PHP. Onderdelen die zijn gemaakt met behulp van deze verschillende talen kunnen wisselen berichten betrouwbaar en bij volledige beeldkwaliteit de 1.0 AMQP met de ondersteuning in Service Bus. Zie de [Service Bus AMQP 1.0 handleiding voor ontwikkelaars](service-bus-amqp-dotnet.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* [AMQP 1.0-ondersteuning in Azure Service Bus](service-bus-amqp-overview.md)
* [Hoe u AMQP 1.0 met de Service Bus .NET-API](service-bus-dotnet-advanced-message-queuing.md)
* [Handleiding voor ontwikkelaars van de service Bus AMQP 1.0](service-bus-amqp-dotnet.md)
* [Het gebruik van de Service Bus wachtrijen](service-bus-dotnet-get-started-with-queues.md)
 
