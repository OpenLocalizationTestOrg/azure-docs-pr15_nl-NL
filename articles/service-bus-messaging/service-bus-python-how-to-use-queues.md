<properties 
    pageTitle="Het gebruik van de Service Bus wachtrijen met Python | Microsoft Azure" 
    description="Informatie over het gebruik van Azure Service Bus wachtrijen uit Python." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="sethm;lmazuel"/>


# <a name="how-to-use-service-bus-queues"></a>Het gebruik van de Service Bus wachtrijen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In dit artikel wordt beschreven hoe Service Bus wachtrijen gebruiken. In de voorbeelden in Python zijn geschreven en gebruik de [Python Azure Service Bus-pakket][]. De scenario's waarvoor zijn **wachtrijen maken, verzenden en ontvangen van berichten**en **wachtrijen verwijderen**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Zie de [Installatiehandleiding voor Python](../python-how-to-install.md)wilt installeren Python of het [Python Azure Service Bus-pakket][].

## <a name="create-a-queue"></a>Een wachtrij maken

Het object **ServiceBusService** kunt u werken met wachtrijen. De volgende code aan de bovenkant van een bestand Python waarin die u wilt openen via een programma Service Bus hebt toegevoegd:

```
from azure.servicebus import ServiceBusService, Message, Queue
```

De volgende code maakt een object **ServiceBusService** . Vervang `mynamespace`, `sharedaccesskeyname`, en `sharedaccesskey` met uw naamruimte, belangrijke naam van gedeelde toegang handtekening (SA's) en waarde.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

De waarden voor de naam van de SA's sleutel en de waarde vindt u in de verbindingsgegevens [Azure klassieke portal][] of in het deelvenster Visual Studio- **Eigenschappen** bij het selecteren van de naamruimte Service Bus in Server Explorer (zoals weergegeven in de vorige sectie).

```
bus_service.create_queue('taskqueue')
```

**create_queue** ondersteunt ook extra opties waarmee u kunt de standaardinstellingen overschrijven wachtrij zoals bericht tijd naar live (TTL) of de maximale grootte. Het volgende voorbeeld wordt de maximale grootte van 5GB en de TTL-waarde op 1 minuut:

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij

Een bericht verzenden naar een Service Bus wachtrij, uw oproepen toepassing de **verzenden\_wachtrij\_bericht** methode op het object **ServiceBusService** .

Het volgende voorbeeld wordt getoond hoe een testbericht verzenden naar de wachtrij met de naam *taskqueue gebruiken* **verzenden\_wachtrij\_bericht**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Service Bus wachtrijen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een wachtrij gezet, maar er is een initiaal van de totale grootte van de berichten die door een wachtrij gehouden. De wachtrijgrootte van deze is opgegeven bij het maken, met een maximum van 5 GB. Zie [quota voor Service Bus][]voor meer informatie over quota.

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangt van een wachtrij

Berichten worden ontvangen van een wachtrij met de **ontvangen\_wachtrij\_bericht** methode op het object **ServiceBusService** :

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Berichten worden verwijderd uit de wachtrij als ze worden opgelezen wanneer de parameter **korte weergave\_vergrendelen** is ingesteld op **Onwaar**. U kunt lezen (korte weergave) en het bericht vergrendelen zonder deze te verwijderen uit de wachtrij door de parameter **korte weergave\_vergrendelen** op **True**.

Het gedrag van lezen en verwijderen van het bericht als onderdeel van de ontvangstbewerking is de eenvoudigste model en werkt het best voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus wordt het bericht als wordt verbruikt hebt gemarkeerd, wordt klikt u vervolgens wanneer de toepassing opnieuw is opgestart en begint door nogmaals, andere berichten deze heb gemist het bericht verbruikte vóór het vastlopen.

Als de **korte weergave\_vergrendelen** parameter is ingesteld op **waar**, wordt de ontvangen een bewerking van twee fase, die het mogelijk maakt naar ondersteuningstoepassingen die u kunnen geen ontbrekende berichten zonder. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door de ondersteuning voor de methode **delete** op het object **bericht** voltooid. De methode **delete** wordt markeert u het bericht worden gebruikt en deze te verwijderen uit de wachtrij.

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hoe u omgaat met toepassing loopt en gelezen berichten

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvanger kan niet worden verwerkt van het bericht om een bepaalde reden is, kan deze de methode **ontgrendelen** aanroepen op het object **bericht** . Hierdoor wordt de Service Bus u kunt het bericht in de wachtrij ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht in de wachtrij vergrendeld en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendelen time-out is verstreken (bijvoorbeeld als de toepassing loopt vast), en vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat de methode **delete** wordt aangeroepen, wordt klikt u vervolgens het bericht worden geleverd met de toepassing opnieuw wordt gestart. Heet dit vaak **Tenminste eenmaal Processing**, dat wil zeggen elk bericht wordt ten minste eenmaal worden verwerkt maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, kunnen softwareontwikkelaars moeten extra logica toevoegen aan de toepassing u omgaat met dubbele bericht is bezorgd. Dit is vaak bereikt met de eigenschap **MessageId** van het bericht, constant over bezorging pogingen blijft.

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Service Bus wachtrijen hebt geleerd, volgt u deze koppelingen voor meer informatie.

-   Zie [wachtrijen, onderwerpen en abonnementen][].

[Azure klassieke portal]: https://manage.windowsazure.com
[Python Azure Service Bus pakket]: https://pypi.python.org/pypi/azure-servicebus  
[Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
[Quota voor Service Bus]: service-bus-quotas.md
 
