<properties
    pageTitle="Het gebruik van de Service Bus onderwerpen (Ruby) | Microsoft Azure"
    description="Informatie over het gebruik van de Service Bus onderwerpen en abonnementen in Azure wordt aangegeven. Voorbeelden van code worden geschreven voor Ruby-toepassingen."
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topicssubscriptions"></a>Het gebruik van de Service Bus onderwerpen/abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In dit artikel wordt beschreven hoe Service Bus onderwerpen en abonnementen van Ruby-toepassingen gebruiken. De scenario's waarvoor opnemen **maken van onderwerpen en -abonnement, abonnement filters, verzenden van berichten maken** naar een onderwerp, **ontvangen van berichten van een abonnement**en **verwijderen van onderwerpen en abonnementen**. Zie voor meer informatie over onderwerpen en abonnementen, de sectie van de [Volgende stappen](#next-steps) .

## <a name="service-bus-topics-and-subscriptions"></a>Service Bus onderwerpen en abonnementen

Ondersteund in een *publicatie/abonnementen* communicatiemodel messaging service Bus onderwerpen en abonnementen. Wanneer u onderwerpen en abonnementen, communiceren onderdelen van een gedistribueerde toepassing niet rechtstreeks met elkaar. ze uitwisselen berichten via een onderwerp bemiddelt in plaats daarvan.

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

Geef een **een-op-veel** -formulier van communicatie, met een patroon publiceren/abonneren in tegenstelling tot de Service Bus wachtrijen, waarbij elk bericht wordt verwerkt door een enkel consument, onderwerpen en abonnementen. Het is mogelijk meerdere abonnementen op een onderwerp registreren. Wanneer een bericht wordt verzonden naar een onderwerp, dit is vervolgens ter beschikking gesteld aan elk abonnement onafhankelijk van proces.

Een abonnement onderwerp lijkt op een virtuele wachtrij kopieën van de berichten die zijn verzonden naar het onderwerp. U kunt desgewenst filterregels voor een onderwerp op basis van een per abonnement, waarmee u kunt het filter/beperken welke berichten u wilt een onderwerp worden ontvangen door welke abonnementen onderwerp registreren.

Service Bus onderwerpen en -abonnementen kunnen u aan de nieuwe schaal verwerkingstijd van een groot aantal berichten over een groot aantal gebruikers en toepassingen.

## <a name="create-a-namespace"></a>Een naamruimte maken

Als u wilt gebruiken Service Bus wachtrijen in Azure wordt aangegeven, moet u eerst een naamruimte maken. Een naamruimte bevat een scope container voor de adressering van Service Bus bronnen binnen uw toepassing. Omdat de [Azure portal][] geen de naamruimte met een ACS-verbinding maakt, moet u de naamruimte via de opdrachtregel maken.

Een naamruimte maken:

1. Open een Azure Powershell-console-venster.

2. Typ de volgende opdracht uit een naamruimte maken. Geef uw eigen naamruimtewaarde en de dezelfde regio opgeven als de toepassing.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true
    ```

    ![Namespace maken](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## <a name="obtain-default-management-credentials-for-the-namespace"></a>Standaard management referenties voor de naamruimte verkrijgen

Om te kunnen uitvoeren management bewerkingen uitvoeren, zoals het maken van een wachtrij voor de nieuwe naamruimte, moet u de management referenties voor de naamruimte.

De PowerShell-cmdlet die u hebt uitgevoerd als u wilt maken van de naamruimte Service Bus geeft de sleutel die u gebruiken kunt voor het beheren van de naamruimte. Kopieer de waarde **DefaultKey** . U gebruikt deze waarde in de code verderop in deze zelfstudie.

![Toets kopiëren](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> U vindt deze toets ook als u zich aanmeldt bij de [portal van Azure][] en navigeer naar de verbindingsgegevens voor de naamruimte.

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing bouwen

Zie [een Ruby-toepassing op Azure maken](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)voor instructies.

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren om te gebruiken Service Bus

Als u wilt gebruiken Service Bus, downloaden en gebruiken van het pakket Ruby Azure, waaronder een reeks gemak bibliotheken die met de opslagruimte REST-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>Gebruik RubyGems het pakket verkrijgen

1. Gebruik een opdrachtregel-interface zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix).

2. Typ 'azure gem installeren' in het opdrachtvenster de gem en afhankelijkheden wilt installeren.

### <a name="import-the-package"></a>Het pakket importeren

Met uw favoriete teksteditor, voeg de volgende naar het begin van het Ruby bestand waarin u van plan bent opslag gebruiken:

```
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Een Service Bus-verbinding instellen

De Azure module leest de omgevingsvariabelen **AZURE\_SERVICEBUS\_NAAMRUIMTE** en **AZURE\_SERVICEBUS\_ACCESS\_sleutel** voor informatie die nodig is om te verbinden met uw naamruimte. Als deze omgevingsvariabelen niet zijn ingesteld, moet u de naamruimtegegevens voordat u **Azure::ServiceBusService** met de volgende code:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Stel de naamruimtewaarde aan de waarde die u hebt gemaakt in plaats van de volledige URL. Gebruik bijvoorbeeld **"yourexamplenamespace"**, niet "yourexamplenamespace.servicebus.windows.net".

## <a name="create-a-topic"></a>Een onderwerp maken

Het object **Azure::ServiceBusService** kunt u werken met onderwerpen. De volgende code maakt een object **Azure::ServiceBusService** . Als u wilt maken van een onderwerp, gebruikt u de **maken\_topic()** methode. Het volgende voorbeeld wordt gemaakt van een onderwerp of de fouten wordt afgedrukt als er een.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

U kunt ook een **Azure::ServiceBus::Topic** object met extra opties waarmee u kunt de standaardinstellingen negeren onderwerp zoals bericht tijd naar live of de maximale grootte doorgeven. Het volgende voorbeeld ziet de maximale grootte aan 5GB en tijd wilt live op 1 minuut instellen:

```
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Abonnementen maken

Onderwerp abonnementen worden ook gemaakt met het object **Azure::ServiceBusService** . Abonnementen zijn met de naam en een optionele filter waarmee de verzameling berichten die in de virtuele wachtrij van het abonnement wordt beperkt kunnen hebben.

Abonnementen zijn permanente en blijft tot beide ze bestaan, of het onderwerp ze zijn gekoppeld, worden verwijderd. Als uw toepassing logica bevat voor het maken van een abonnement, moet deze eerst controleren of het abonnement dat al aanwezig is met de methode getSubscription.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)

Het filter **MatchAll** is het standaardfilter die wordt gebruikt als er geen filter is opgegeven als een nieuw abonnement wordt gemaakt. Wanneer het **MatchAll** -filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp van het abonnement virtuele wachtrij geplaatst. In het volgende voorbeeld wordt gemaakt van een abonnement met de naam 'Alles '-berichten en het standaardfilter voor **MatchAll** gebruikt.

```
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

Ook kunt u filters waarmee u kunt aangeven welke berichten die zijn verzonden naar een onderwerp dat moeten worden weergegeven binnen een bepaald abonnement.

Het meest flexibele type filter die worden ondersteund in abonnementen is de **Azure::ServiceBus::SqlFilter**, waarmee een subset van SQL92-standaard worden geïmplementeerd. SQL-filters worden uitgevoerd op de eigenschappen van de berichten die zijn gepubliceerd naar het onderwerp. Bekijk de syntaxis van de [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter.

U kunt filters toevoegen aan een abonnement met behulp van de **maken\_rule()** methode van het object **Azure::ServiceBusService** . Deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

Aangezien het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter verwijderen of de **MatchAll** overschrijft eventuele andere filters die u kunt opgeven. U kunt de standaardregel verwijderen met behulp van de **verwijderen\_rule()** methode op het object **Azure::ServiceBusService** .

Het volgende voorbeeld wordt een abonnement met de naam "hoog-berichten" met een **Azure::ServiceBus::SqlFilter** die selecteert alleen berichten met een aangepaste **bericht\_getal** eigenschap groter is dan 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Het volgende voorbeeld wordt ook een abonnement met de naam 'laag-berichten"met een **Azure::ServiceBus::SqlFilter** die selecteert alleen berichten met een **message_number** eigenschap kleiner is dan of gelijk aan 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Wanneer een bericht wordt nu verzonden naar "-test-onderwerp", is deze altijd worden geleverd met ontvangers geabonneerd op het abonnement van "alle-berichten" onderwerp en selectief bezorgd bij ontvangers geabonneerd op de "hoog-berichten" en "laag-berichten" onderwerp-abonnementen (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

Als u wilt een bericht verzenden naar een Service Bus onderwerp, de toepassing moet gebruiken de **verzenden\_onderwerp\_message()** methode op het object **Azure::ServiceBusService** . Berichten die zijn verzonden naar Service Bus onderwerpen zijn exemplaren van de objecten **Azure::ServiceBus::BrokeredMessage** . **Azure::ServiceBus::BrokeredMessage** objecten bevatten een set standaardeigenschappen (zoals **het label** en **tijd\_naar\_live**), een woordenlijst die wordt gebruikt om specifieke eigenschappen maatwerktoepassing en een tekenreeks gegevens. Een toepassing de hoofdtekst van het bericht kunt instellen door een tekenreekswaarde naar de **verzenden\_onderwerp\_message()** methode en een vereist standaardeigenschappen wordt gevuld met standaardwaarden.

Het volgende voorbeeld wordt het verzenden van vijf berichten naar "-test-onderwerp" testen. Opmerking die de waarde van de aangepaste eigenschap **message_number** van elk bericht op de herhaling van de lus varieert (Hiermee bepaalt u welk abonnement ontvangt):

```
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Service Bus onderwerpen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een onderwerp gehouden, maar er is een initiaal van de totale grootte van de berichten die door een onderwerp. De grootte van dit onderwerp wordt opgegeven bij het maken, met een maximum van 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangt van een abonnement

Berichten worden ontvangen van een abonnement met de **ontvangen\_abonnement\_message()** methode op het object **Azure::ServiceBusService** . Standaard worden read(peak) berichten en vergrendeld zonder deze te verwijderen van het abonnement. U kunt lezen en het bericht van het abonnement verwijderen door in te stellen de **korte weergave\_vergrendelen** optie op **false**.

Het standaardgedrag zorgt ervoor dat de Leesmodus en verwijderen van een bewerking van twee fasen, waardoor ook ter ondersteuning van toepassingen die niet kunnen zonder ontbrekende berichten. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door te bellen voltooid **verwijderen\_abonnement\_message()** methode en het bericht moet worden verwijderd als een parameter leveren. De **verwijderen\_abonnement\_message()** methode wordt het bericht markeren als wordt verbruikt en van het abonnement verwijderen.

Als de **: korte weergave\_vergrendelen** parameter zo is ingesteld op **Onwaar**lezen en verwijderen van het bericht wordt het eenvoudigste model en geschikt is voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus wordt het bericht als wordt verbruikt hebt gemarkeerd, wordt klikt u vervolgens wanneer de toepassing opnieuw is opgestart en begint door nogmaals, andere berichten deze heb gemist het bericht verbruikte vóór het vastlopen.

Het volgende voorbeeld wordt gedemonstreerd hoe berichten kunnen worden ontvangen en het gebruik van verwerkte **ontvangen\_abonnement\_message()**. In het voorbeeld eerst ontvangt en Hiermee verwijdert u een bericht met behulp van het abonnement "laag-berichten" **: korte weergave\_vergrendelen** ingesteld op **Onwaar**, wordt deze ontvangt van een ander bericht van de 'hoog berichten"en vervolgens het bericht met verwijdert **verwijderen\_abonnement\_message()**:

```
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Greep toepassing loopt en gelezen berichten

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvanger kan niet worden verwerkt van het bericht om welke reden, wordt deze kunt bellen de **ontgrendelen\_abonnement\_message()** methode op het object **Azure::ServiceBusService** . Hierdoor Service Bus u kunt het bericht binnen het abonnement te ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht vergrendeld binnen het abonnement en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendelen time-out is verstreken (bijvoorbeeld als de toepassing loopt vast), en vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat de **verwijderen\_abonnement\_message()** methode wordt aangeroepen en vervolgens het bericht wordt geleverd met de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste één keer Processing**; dat wil zeggen elk bericht ten minste eenmaal worden verwerkt, maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, kunnen softwareontwikkelaars moeten extra logica toevoegen aan de toepassing u omgaat met dubbele bericht is bezorgd. Deze logica wordt vaak bereikt voor het gebruik van de **bericht\_id** eigenschap van het bericht, constant over bezorging pogingen blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

Onderwerpen en abonnementen zijn permanente en expliciet moeten worden verwijderd via de [portal van Azure][] of via een programma. In het onderstaande voorbeeld laat zien hoe het onderwerp met de naam "test-onderwerp" verwijderen.

```
azure_service_bus_service.delete_topic("test-topic")
```

Abonnementen die zijn geregistreerd met het onderwerp een onderwerp verwijderen ook worden verwijderd. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code wordt getoond hoe het abonnement met de naam "hoog-berichten" uit het onderwerp "test-onderwerp" verwijderen:

```
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Service Bus onderwerpen hebt geleerd, volgt u deze koppelingen voor meer informatie.

- Zie [wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md).
- API-Naslaggids voor [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx).
- Ga naar de bibliotheek [Azure SDK voor Ruby](https://github.com/Azure/azure-sdk-for-ruby) op GitHub.
 
[Azure-portal]: https://portal.azure.com
