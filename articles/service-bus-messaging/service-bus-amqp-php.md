<properties 
    pageTitle="Service-Bus en PHP met AMQP 1.0 | Microsoft Azure"
    description="Service Bus uit PHP gebruiken met AMQP."
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

# <a name="using-service-bus-from-php-with-amqp-10"></a>Gebruik van de Service Bus uit PHP met AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton-PHP is een PHP taal binding naar Proton-C; dat wil zeggen wordt Proton-PHP geïmplementeerd als een Wikkel rond een engine geïmplementeerd in C.

## <a name="downloading-the-proton-client-library"></a>De bibliotheek van de client Proton downloaden

U kunt Proton-C en de bijbehorende Bindingen (inclusief PHP) downloaden van [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Het downloaden is in de vorm van broncode. Als u de code, volgt u de instructies in de gedownloade pakket.

> [AZURE.IMPORTANT] Op het moment van deze schrijven is de SSL-ondersteuning in Proton-C alleen beschikbaar voor Linux-besturingssystemen. Aangezien Azure Service Bus het gebruik van SSL vereist, kunnen Proton-C (en de taal bindingen) alleen worden gebruikt voor toegang tot Service Bus uit Linux op dit moment. Werk om in te schakelen Proton-C met SSL op Windows is al bezig, dat het geval is selectievakje pagina regelmatig op updates.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Werken met Service Bus wachtrijen, onderwerpen en abonnementen van PHP

De volgende code ziet hoe u berichten verzenden en ontvangen van een Service Bus messaging entiteit.

### <a name="sending-messages-using-proton-php"></a>Verzenden van berichten met Proton-PHP

De volgende code ziet u hoe u een bericht verzenden naar een Service Bus messaging entiteit.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Ontvangen van berichten met Proton-PHP

De volgende code ziet hoe u een bericht ontvangt van een Service Bus messaging entiteit.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>Messaging tussen .NET en Proton-PHP

### <a name="application-properties"></a>Eigenschappen van een servicetoepassing

#### <a name="protonphp-to-service-bus-net-apis"></a>ProtonPHP naar Service-Bus .NET-API 's

Proton-PHP berichten ondersteuning voor toepassingseigenschappen van de volgende typen: **geheel getal**, **dubbele** **Booleaanse**, **tekenreeks**en **object**. De volgende PHP-code ziet hoe u het instellen van eigenschappen voor een bericht met behulp van elk van deze eigenschaptypen.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

In de Service Bus .NET API's, worden de eigenschappen van de servicetoepassing bericht opgenomen in de collectie **Properties** van [BrokeredMessage][]. De volgende code ziet hoe u de toepassingseigenschappen van een bericht hebt ontvangen van een client PHP gelezen.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

De volgende tabel worden de soorten PHP eigenschappen aan de typen van de eigenschap .NET toegewezen.

| Type van de eigenschap PHP | .NET eigenschapstype |
|-------------------|--------------------|
| geheel getal           | int                |
| dubbele            | dubbele             |
| Booleaanse waarde           | BOOL               |
| tekenreeks            | tekenreeks             |
| object            | Object             |

#### <a name="service-bus-net-apis-to-php"></a>Service Bus .NET-API's PHP

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
```

De volgende PHP-code ziet u hoe de toepassingseigenschappen van een bericht hebt ontvangen van een Service Bus .NET-client.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

De volgende tabel worden de soorten .NET eigenschappen aan de typen van de eigenschap PHP toegewezen.

| .NET eigenschapstype | Type van de eigenschap PHP | Notities                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte               | geheel getal           | -                                                                                                                                                                     |
| SByte              | geheel getal           | -                                                                                                                                                                     |
| teken               | Teken              | Proton-PHP class                                                                                                                                                    |
| korte              | geheel getal           | -                                                                                                                                                                     |
| USHORT             | geheel getal           | -                                                                                                                                                                     |
| int                | geheel getal           | -                                                                                                                                                                     |
| uint               | Geheel getal           | -                                                                                                                                                                     |
| lange               | geheel getal           | -                                                                                                                                                                     |
| ULONG              | geheel getal           | -                                                                                                                                                                     |
| toegestane achterstand              | dubbele            | -                                                                                                                                                                     |
| dubbele             | dubbele            | -                                                                                                                                                                     |
| decimaal            | tekenreeks            | Decimaal getal wordt momenteel niet ondersteund met Proton.                                                                                                                     |
| BOOL               | Booleaanse waarde           | -                                                                                                                                                                     |
| GUID               | UUID              | Proton-PHP class                                                                                                                                                    |
| tekenreeks             | tekenreeks            | -                                                                                                                                                                     |
| Datum /           | geheel getal           | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | Toegewezen aan AMQP type DateTimeOffset.UtcTicks:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| Tijdspanne           | DescribedType     | Toegewezen aan AMQP type Timespan.Ticks:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| URI                | DescribedType     | Toegewezen aan AMQP type Uri.AbsoluteUri:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Standaardeigenschappen

De volgende tabellen bevatten de toewijzing tussen de Proton-PHP standaard bericht en de eigenschappen van de standaard bericht [BrokeredMessage][] .

| Proton-PHP           | Service Bus .NET         | Notities                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Duurzame              | n/b                      | Service Bus ondersteunt alleen duurzame berichten.          |
| Prioriteit             | n/b                      | Service Bus biedt alleen ondersteuning voor de prioriteit van een enkel bericht. |
| TTL                  | Message.TimeToLive       | Conversie, Proton-PHP TTL is gedefinieerd (in milliseconden).   |
| eerste\_verwerver      | -                          | -                                                          |
| bezorging\_tellen      | -                          | -                                                          |
| ID                   | Message.Id               | -                                                          |
| gebruiker\_id             | -                          | -                                                          |
| Adres              | Message.To               | -                                                          |
| Onderwerp              | Message.Label            | -                                                          |
| antwoord\_naar            | Message.ReplyTo          | -                                                          |
| correlatie\_id      | Message.CorrelationId    | -                                                          |
| inhoud\_type        | Message.ContentType      | -                                                          |
| inhoud\_codering    | n/b                      | -                                                          |
| verstrijken\_tijd         | Message.ExpiresAtUTC     | -                                                          |
| Aanmaakdatum\_tijd       | n/b                      | -                                                          |
| groep\_id            | Message.SessionId        | -                                                          |
| groep\_sequentie      | -                          | -                                                          |
| antwoord\_naar\_groep\_id | Message.ReplyToSessionId | -                                                          |
| Indeling               | n/b                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Service Bus .NET-API's Proton-PHP

| Service Bus .NET        | Proton-PHP                                             | Notities                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| ContentType             | Bericht -\>inhoud\_type                                | -                                                        |
| CorrelationId           | Bericht -\>correlatie\_id                              | -                                                        |
| EnqueuedTimeUtc         | Bericht -\>aantekeningen [x-opt-in wachtrij-tijd]             | -                                                        |
| Label                   | Bericht -\>onderwerp                                      | -                                                        |
| MessageId               | Bericht -\>id                                           | -                                                        |
| ReplyTo                 | Bericht -\>antwoord\_naar                                    | -                                                        |
| ReplyToSessionId        | Bericht -\>antwoord\_naar\_groep\_id                         | -                                                        |
| ScheduledEnqueueTimeUtc | Bericht -\>aantekeningen ["x-opt-gepland-wachtrij plaatsen-time"] | -                                                        |
| Sessie-id               | Bericht -\>groep\_id                                    | -                                                        |
| TimeToLive              | Bericht -\>ttl                                          | Conversie, Proton-PHP TTL is gedefinieerd (in milliseconden). |
| Aan                      | Bericht -\>adres                                      | -                                                        |

## <a name="next-steps"></a>Volgende stappen

Klaar voor meer informatie? Ga naar de volgende koppelingen:

- [Overzicht van de service Bus AMQP]
- [AMQP in Service Bus voor Windows Server]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP in Service Bus voor Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[Overzicht van de service Bus AMQP]: service-bus-amqp-overview.md
