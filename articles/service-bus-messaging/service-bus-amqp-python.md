<properties 
    pageTitle="Service-Bus en Python met AMQP 1.0 | Microsoft Azure"
    description="Service Bus uit Python gebruiken met AMQP."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-python-with-amqp-10"></a>Gebruik van de Service Bus uit Python met AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton-Python is een binding Python taal naar Proton-C; dat wil zeggen wordt Proton-Python geïmplementeerd als een Wikkel rond een engine geïmplementeerd in C.

## <a name="download-the-proton-client-library"></a>De bibliotheek van de client Proton downloaden

U kunt Proton-C en de bijbehorende Bindingen (inclusief Python) downloaden van [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Het downloaden is in de vorm van broncode. Maak de code door Volg de instructies in de gedownloade pakket.

Houd er rekening mee dat op het moment van deze schrijven, de SSL-ondersteuning in Proton-C alleen beschikbaar voor Linux-besturingssystemen is. Aangezien Azure Service Bus het gebruik van SSL vereist, kunnen Proton-C (en de taal bindingen) alleen worden gebruikt voor toegang tot Service Bus uit Linux op dit moment. Werk om in te schakelen Proton-C met SSL op Windows is al bezig dat het geval is selectievakje pagina regelmatig op updates.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Werken met Service Bus wachtrijen, onderwerpen en abonnementen van Python

De volgende code ziet hoe u berichten verzenden en ontvangen van een Service Bus messaging entiteit.

### <a name="send-messages-using-proton-python"></a>Berichten verzenden met Proton-Python

De volgende code ziet u hoe u een bericht verzenden naar een Service Bus messaging entiteit.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Ontvangen berichten via Proton-Python

De volgende code ziet hoe u een bericht ontvangt van een Service Bus messaging entiteit.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>Berichten tussen .NET en Proton-Python

### <a name="application-properties"></a>Eigenschappen van een servicetoepassing

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python naar Service-Bus .NET-API 's

Proton-Python berichten ondersteuning voor toepassingseigenschappen van de volgende typen: **int**, **lang**, **toegestane achterstand**, **uuid**, **bool**, **tekenreeks**. De volgende Python code ziet hoe u het instellen van eigenschappen voor een bericht met behulp van elk van deze eigenschaptypen.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

Eigenschappen van de toepassing berichten worden in de Service Bus .NET-API opgenomen in de collectie **Properties** van [BrokeredMessage][]. De volgende code ziet hoe u de toepassingseigenschappen van een bericht hebt ontvangen van een client Python gelezen.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

De volgende tabel worden de soorten Python eigenschappen aan de typen van de eigenschap .NET toegewezen.

| Type van de eigenschap Python | .NET eigenschapstype |
|----------------------|--------------------|
| int                  | int                |
| toegestane achterstand                | dubbele             |
| lange                 | Int64              |
| UUID                 | GUID               |
| BOOL                 | BOOL               |
| tekenreeks               | tekenreeks             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Service Bus .NET-API's Proton-Python

Het type [BrokeredMessage][] ondersteunt de toepassingseigenschappen van de van de volgende typen: **byte**, **sbyte**, **teken**, **korte**, **ushort**, **int**, **uint**, **lang**, **ulong**, **toegestane achterstand**, **dubbele**, **decimalen**, **bool**, **Guid**, **tekenreeks**, **Uri**, **DateTime**, **DateTimeOffset**, en **tijdspanne**. De volgende .NET-code ziet u hoe u eigenschappen instelt in een [BrokeredMessage][] -object met elk van deze eigenschaptypen.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

De volgende Python code ziet u hoe de toepassingseigenschappen van een bericht hebt ontvangen van een Service Bus .NET-client.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

De volgende tabel worden de soorten .NET eigenschappen aan de typen van de eigenschap Python toegewezen.

| .NET eigenschapstype | Type van de eigenschap Python | Notities                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte               | int                  | -                                                                                                                                                                     |
| SByte              | int                  | -                                                                                                                                                                     |
| teken               | teken                 | Proton-Python class                                                                                                                                                 |
| korte              | int                  | -                                                                                                                                                                     |
| USHORT             | int                  | -                                                                                                                                                                     |
| int                | int                  | -                                                                                                                                                                     |
| uint               | int                  | -                                                                                                                                                                     |
| lange               | int                  | -                                                                                                                                                                     |
| ULONG              | lange                 | Proton-Python class                                                                                                                                                 |
| toegestane achterstand              | toegestane achterstand                | -                                                                                                                                                                     |
| dubbele             | toegestane achterstand                | -                                                                                                                                                                     |
| decimaal            | Tekenreeks               | Decimaal getal wordt momenteel niet ondersteund met Proton.                                                                                                                     |
| BOOL               | BOOL                 | -                                                                                                                                                                     |
| GUID               | UUID                 | Proton-Python class                                                                                                                                                 |
| tekenreeks             | tekenreeks               | -                                                                                                                                                                     |
| Datum /           | tijdstempel            | Proton-Python class                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | Toegewezen aan AMQP type DateTimeOffset.UtcTicks:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Tijdspanne           | DescribedType        | Toegewezen aan AMQP type Timespan.Ticks:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType        | Toegewezen aan AMQP type Uri.AbsoluteUri:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Standaardeigenschappen

De volgende tabellen bevatten de toewijzing tussen de Proton-Python standaard bericht en de eigenschappen van de standaard bericht [BrokeredMessage][] .

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python naar Service-Bus .NET-API 's

| Proton-Python        | Service Bus .NET         | Notities                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| duurzame              | n/b                      | Service Bus ondersteunt alleen duurzame berichten.               |
| prioriteit             | n/b                      | Service Bus biedt alleen ondersteuning voor de prioriteit van een enkel bericht.      |
| TTL                  | Message.TimeToLive       | Conversie, Proton-Python TTL is gedefinieerd (in milliseconden). |
| eerste\_verwerver      | n/b                      | -                                                           |
| bezorging\_tellen      | n/b                      | -                                                           |
| ID                   | Message.MessageID        | -                                                           |
| gebruiker\_id             | n/b                      | -                                                           |
| adres              | Message.To               | -                                                           |
| Onderwerp              | Message.Label            | -                                                           |
| antwoord\_naar            | Message.ReplyTo          | -                                                           |
| correlatie\_id      | Message.CorrelationID    | -                                                           |
| inhoud\_type        | Message.ContentType      | -                                                           |
| inhoud\_codering    | n/b                      | -                                                           |
| verstrijken\_tijd         | n/b                      | -                                                           |
| Aanmaakdatum\_tijd       | n/b                      | -                                                           |
| groep\_id            | Message.SessionId        | -                                                           |
| groep\_sequentie      | n/b                      | -                                                           |
| antwoord\_naar\_groep\_id | Message.ReplyToSessionId | -                                                           |
| indeling               | n/b                      | -                                                           |

| Service Bus .NET        | Proton                       | Notities                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| ContentType             | Message.content\_type        | -                                                           |
| CorrelationId           | Message.Correlation\_id      | -                                                           |
| EnqueuedTimeUtc         | n/b                          | -                                                           |
| Label                   | Message.Subject              | -                                                           |
| MessageId               | Message.id                   | -                                                           |
| ReplyTo                 | Message.Reply\_naar            | -                                                           |
| ReplyToSessionId        | Message.Reply\_naar\_groep\_id | -                                                           |
| ScheduledEnqueueTimeUtc | n/b                          | -                                                           |
| Sessie-id               | Message.group\_id            | -                                                           |
| TimeToLive              | Message.TTL                  | Conversie, Proton-Python TTL is gedefinieerd (in milliseconden). |
| Aan                      | Message.Address              | -                                                           |

## <a name="next-steps"></a>Volgende stappen

Klaar voor meer informatie? Ga naar de volgende koppelingen:

- [Overzicht van de service Bus AMQP]
- [AMQP in Service Bus voor Windows Server]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP in Service Bus voor Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx

[Overzicht van de service Bus AMQP]: service-bus-amqp-overview.md
