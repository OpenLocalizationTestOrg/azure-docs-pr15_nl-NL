<properties 
    pageTitle="Het gebruik van de Service Bus onderwerpen met Python | Microsoft Azure" 
    description="Informatie over het gebruik van Azure Service Bus onderwerpen en abonnementen van Python." 
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
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Het gebruik van de Service Bus onderwerpen en abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In dit artikel wordt beschreven hoe Service Bus onderwerpen en abonnementen gebruiken. In de voorbeelden in Python zijn geschreven en gebruik de [Python Azure-pakket][]. De scenario's waarvoor zijn **maken van onderwerpen en abonnementen** **abonnement filters maken**, **berichten verzenden naar een onderwerp**, **ontvangen van berichten van een abonnement**en **verwijderen van onderwerpen en abonnementen**. Zie voor meer informatie over onderwerpen en abonnementen, de sectie van de [Volgende stappen](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Notitie:** Als u nodig hebt Python of het [Python Azure-pakket][]te installeren, raadpleegt u de [Installatiehandleiding voor Python](../python-how-to-install.md).

## <a name="create-a-topic"></a>Een onderwerp maken

Het object **ServiceBusService** kunt u werken met onderwerpen. Voeg de volgende Klik boven aan elke Python bestand waarin u wilt openen via een programma Service Bus:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

De volgende code maakt een object **ServiceBusService** . Vervang `mynamespace`, `sharedaccesskeyname`, en `sharedaccesskey` met de werkelijke naamruimte, gedeeld Access handtekening (SA's) sleutelwaarde naam en key.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

U kunt de waarden voor de naam van de SA's sleutel en waarde ophalen van de [Azure-portal][].

```
bus_service.create_topic('mytopic')
```

**maken\_onderwerp** ondersteunt ook extra opties waarmee u kunt de standaardinstellingen overschrijven onderwerp zoals bericht tijd naar live of onderwerp maximale grootte. Het volgende voorbeeld wordt de grootte van de maximale onderwerp aan 5 GB aan en per keer wilt live (TTL)-waarde van 1 minuut:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Abonnementen maken

Abonnementen op onderwerpen zijn ook gemaakt met het object **ServiceBusService** . Abonnementen zijn met de naam en een optionele filter waarmee de verzameling berichten die in de virtuele wachtrij van het abonnement wordt beperkt kunnen hebben.

> [AZURE.NOTE] Abonnementen zijn permanente en blijft bestaan totdat ze, of het onderwerp waaraan ze zijn geabonneerd, worden verwijderd.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)

Het filter **MatchAll** is het standaardfilter die wordt gebruikt als er geen filter is opgegeven als een nieuw abonnement wordt gemaakt. Wanneer het **MatchAll** -filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp van het abonnement virtuele wachtrij geplaatst. In het volgende voorbeeld wordt gemaakt van een abonnement met de naam 'AllMessages' en het standaardfilter voor **MatchAll** gebruikt.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

Ook kunt u filters waarmee u kunt aangeven welke berichten die zijn verzonden naar een onderwerp dat moeten worden weergegeven binnen een bepaald onderwerp-abonnement.

Het meest flexibele type filter die worden ondersteund in abonnementen is een **SqlFilter**, waarmee een subset van SQL92-standaard worden geïmplementeerd. SQL-filters worden uitgevoerd op de eigenschappen van de berichten die zijn gepubliceerd naar het onderwerp. Zie voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter, de syntaxis van de [SqlFilter.SqlExpression][] .

U kunt filters toevoegen aan een abonnement met behulp van de **maken\_regel** methode van het object **ServiceBusService** . Deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

> [AZURE.NOTE] Omdat het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter verwijderen of de **MatchAll** overschrijft eventuele andere filters die u kunt opgeven. U kunt de standaardregel verwijderen met behulp van de **verwijderen\_regel** methode van het object **ServiceBusService** .

Het volgende voorbeeld wordt een abonnement met de naam `HighMessages` met een **SqlFilter** die selecteert alleen berichten met een aangepaste **messagenumber** eigenschap groter is dan 3:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Ook het volgende voorbeeld wordt een abonnement met de naam `LowMessages` met een **SqlFilter** die selecteert alleen berichten met een **messagenumber** eigenschap kleiner is dan of gelijk aan 3:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Nu, wanneer een bericht wordt verzonden naar `mytopic` wordt altijd bezorgd bij ontvangers geabonneerd op het abonnement dat **AllMessages** onderwerp en selectief bezorgd bij ontvangers geabonneerd op de **HighMessages** en **LowMessages** onderwerp abonnementen (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

Als u wilt een bericht verzenden naar een Service Bus onderwerp, de toepassing moet gebruiken de **verzenden\_onderwerp\_bericht** methode van het object **ServiceBusService** .

Het volgende voorbeeld ziet u hoe u verzendt vijf berichten om te testen `mytopic`. Opmerking die de waarde van de eigenschap **messagenumber** van elk bericht op de herhaling van de lus varieert (Hiermee bepaalt u welke abonnementen ontvangen):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus onderwerpen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een onderwerp gehouden, maar er is een initiaal van de totale grootte van de berichten die door een onderwerp. De grootte van dit onderwerp wordt opgegeven bij het maken, met een maximum van 5 GB. Zie [quota voor Service Bus][]voor meer informatie over quota.

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangt van een abonnement

Berichten worden ontvangen van een abonnement met de **ontvangen\_abonnement\_bericht** methode op het object **ServiceBusService** :

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Berichten worden verwijderd uit het abonnement als ze worden opgelezen wanneer de parameter **korte weergave\_vergrendelen** is ingesteld op **Onwaar**. U kunt lezen (korte weergave) en het bericht vergrendelen zonder deze te verwijderen uit de wachtrij door de parameter **korte weergave\_vergrendelen** op **True**.

Het gedrag van lezen en verwijderen van het bericht als onderdeel van de ontvangstbewerking is de eenvoudigste model en werkt het best voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus wordt het bericht als wordt verbruikt hebt gemarkeerd, wordt klikt u vervolgens wanneer de toepassing opnieuw is opgestart en begint door nogmaals, andere berichten deze heb gemist het bericht verbruikte vóór het vastlopen.

Als de **korte weergave\_vergrendelen** parameter is ingesteld op **waar**, wordt de ontvangen een bewerking van twee fase, die het mogelijk maakt naar ondersteuningstoepassingen die u kunnen geen ontbrekende berichten zonder. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door de ondersteuning voor methode **delete** op het object **bericht** voltooid. De methode **delete** markeert u het bericht worden gebruikt en wordt deze verwijderd van het abonnement.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hoe u omgaat met toepassing loopt en gelezen berichten

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvanger kan niet worden verwerkt van het bericht om een bepaalde reden is, kan deze de methode **ontgrendelen** aanroepen op het object **bericht** . Hierdoor wordt de Service Bus u kunt het bericht binnen het abonnement te ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht vergrendeld binnen het abonnement en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendelen time-out is verstreken (bijvoorbeeld als de toepassing loopt vast), en vervolgens op de Service wordt het bericht automatisch ontgrendeld en voor het opnieuw worden ontvangen beschikbaar stelt.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat de methode **delete** wordt aangeroepen, wordt klikt u vervolgens het bericht worden geleverd met de toepassing opnieuw wordt gestart. Heet dit vaak **Tenminste eenmaal Processing**, dat wil zeggen elk bericht wordt ten minste eenmaal worden verwerkt maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, kunnen softwareontwikkelaars moeten extra logica toevoegen aan de toepassing u omgaat met dubbele bericht is bezorgd. Dit is vaak bereikt met de eigenschap **MessageId** van het bericht, constant over bezorging pogingen blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

Onderwerpen en abonnementen zijn permanente en expliciet moeten worden verwijderd via de [portal van Azure][] of via een programma. Het volgende voorbeeld ziet u het verwijderen van het onderwerp met de naam `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Abonnementen die zijn geregistreerd met het onderwerp een onderwerp verwijderen ook worden verwijderd. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code ziet u hoe u het verwijderen van een abonnement met de naam `HighMessages` uit de `mytopic` onderwerp:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Service Bus onderwerpen hebt geleerd, volgt u deze koppelingen voor meer informatie.

-   Zie [wachtrijen, onderwerpen en abonnementen][].
-   Overzicht van [SqlFilter.SqlExpression][].

[Azure-portal]: https://portal.azure.com
[Python Azure-pakket]: https://pypi.python.org/pypi/azure  
[Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Quota voor Service Bus]: service-bus-quotas.md 
