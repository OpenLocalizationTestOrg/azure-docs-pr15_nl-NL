<properties
    pageTitle="Het gebruik van de Service Bus wachtrijen met Ruby | Microsoft Azure"
    description="Informatie over het gebruik van de Service Bus wachtrijen in Azure wordt aangegeven. Voorbeelden van de code in Ruby geschreven."
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

# <a name="how-to-use-service-bus-queues"></a>Het gebruik van de Service Bus wachtrijen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Deze handleiding wordt beschreven hoe Service Bus wachtrijen gebruiken. In de voorbeelden in Ruby zijn geschreven en gebruik van de Azure gem. De scenario's waarvoor zijn **wachtrijen maken, verzenden en ontvangen van berichten**en **wachtrijen verwijderen**. Zie voor meer informatie over de Service Bus wachtrijen, de sectie van de [Volgende stappen](#next-steps) .

## <a name="what-are-service-bus-queues"></a>Wat zijn Service Bus wachtrijen?

Service Bus wachtrijen ondersteuning voor een *brokered messaging* communicatie-model. Met wachtrijen communiceren onderdelen van een gedistribueerde toepassing niet met elkaar. in plaats daarvan uitwisselen deze berichten via een wachtrij bemiddelt. Een bericht producent (afzender) uit een bericht naar de wachtrij handen en vervolgens de verwerking.
Asynchroon, een bericht consumenten (ontvanger) ophaalt van het bericht uit de wachtrij en verwerkt. De producent beschikt niet over moet wachten om een antwoord van de consument om verder te kunnen verwerken en verder berichten verzenden. Wachtrijen bieden de **eerste keer hebt aangemeld, eerste Out (FIFO)** berichtbezorging naar een of meer concurrerende consumenten. Berichten zijn dat wil zeggen, meestal ontvangen en verwerkt door de ontvangers in de volgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht is ontvangen en verwerkt door consumenten slechts één bericht.

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

Service Bus wachtrijen zijn een algemene technologie die kan worden gebruikt voor een groot aantal scenario's:

-   De communicatie tussen web en werknemer rollen in een [meerlagige Azure-toepassing](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md).
-   Communicatie tussen on-premises implementatie-apps en Azure gehost apps in een [hybride oplossing](service-bus-dotnet-hybrid-app-using-service-bus-relay.md).
-   De communicatie tussen onderdelen van een gedistribueerde toepassing on-premises implementatie uitgevoerd in andere organisaties of afdelingen van een organisatie.

Gebruik van wachtrijen kunt u uw betere toepassingen, en inschakelen meer tolerantie voor uw architectuur.

## <a name="create-a-namespace"></a>Een naamruimte maken

Als u wilt gebruiken Service Bus wachtrijen in Azure wordt aangegeven, moet u eerst een naamruimte maken. Een naamruimte bevat een scope container voor de adressering van Service Bus bronnen binnen uw toepassing. Omdat de portal van Azure geen de naamruimte met een ACS-verbinding maakt, moet u de naamruimte via de opdrachtregel maken.

Een naamruimte maken:

1. Open een Azure Powershell-console.

2. Typ de volgende opdracht uit een Service Bus naamruimte maken. Geef uw eigen naamruimtewaarde en de dezelfde regio opgeven als de toepassing.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)
    ```

## <a name="obtain-management-credentials-for-the-namespace"></a>Referenties voor de naamruimte verkrijgen

Om te kunnen uitvoeren management bewerkingen uitvoeren, zoals het maken van een wachtrij voor de nieuwe naamruimte, moet u de management referenties voor de naamruimte.

De PowerShell-cmdlet die u hebt uitgevoerd als u wilt maken van de naamruimte van Azure service bus geeft de sleutel die u gebruiken kunt voor het beheren van de naamruimte. Kopieer de waarde **DefaultKey** . U gebruikt deze waarde in de code verderop in deze zelfstudie.

![Toets kopiëren](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE] U vindt deze toets ook als u zich aanmeldt bij de [portal van Azure](https://portal.azure.com/) en navigeer naar de verbindingsgegevens voor de naamruimte van uw Service Bus.

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing bouwen

Maak een Ruby-toepassing. Zie [een Ruby-toepassing op Azure maken](/develop/ruby/tutorials/web-app-with-linux-vm/)voor instructies.

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing Service Bus configureren

Als u wilt gebruiken Azure Service Bus, downloaden en gebruiken van het pakket Ruby Azure, waaronder een reeks gemak bibliotheken die met de opslagruimte REST-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>Gebruik RubyGems het pakket verkrijgen

1. Gebruik een opdrachtregel-interface zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix).

2. Typ 'azure gem installeren' in het opdrachtvenster de gem en afhankelijkheden wilt installeren.

### <a name="import-the-package"></a>Het pakket importeren

Met uw favoriete teksteditor, voeg de volgende naar het begin van het Ruby-bestand waarop u wilt opslagruimte gebruiken:

```
require "azure"
```

## <a name="set-up-an-azure-service-bus-connection"></a>Een verbinding Azure Service Bus instellen

De Azure module leest de omgevingsvariabelen **AZURE\_SERVICEBUS\_NAAMRUIMTE** en **AZURE\_SERVICEBUS\_ACCESS_KEY** voor informatie die nodig is om te verbinden met uw Service Bus naamruimte. Als deze omgevingsvariabelen niet zijn ingesteld, moet u de naamruimtegegevens voordat u **Azure::ServiceBusService** met de volgende code:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Stel de naamruimtewaarde aan de waarde die u hebt gemaakt, in plaats van de volledige URL. Gebruik bijvoorbeeld **"yourexamplenamespace"**, niet "yourexamplenamespace.servicebus.windows.net".

## <a name="how-to-create-a-queue"></a>Het maken van een wachtrij

Het object **Azure::ServiceBusService** kunt u werken met wachtrijen. Als u wilt een wachtrij maakt, gebruikt u de methode **create_queue()** . Het volgende voorbeeld wordt gemaakt van een wachtrij of fouten wordt afgedrukt.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

U kunt ook een object **Azure::ServiceBus::Queue** met extra opties waarmee u de standaardinstellingen wachtrij wordt weergegeven, zoals bericht tijd naar live of maximale grootte overschrijven doorgeven. Het volgende voorbeeld ziet u hoe de maximale grootte aan 5GB en tijd wilt live op 1 minuut instellen:

```
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Het verzenden van berichten naar een wachtrij

Een bericht verzenden naar een Service Bus wachtrij, u toepassing oproepen de **verzenden\_wachtrij\_message()** methode op het object **Azure::ServiceBusService** . Berichten verzonden naar (en ontvangen van) Service Bus wachtrijen zijn **Azure::ServiceBus::BrokeredMessage** objecten en een reeks standaardeigenschappen hebt (zoals **het label** en **tijd\_naar\_live**), een woordenlijst die wordt gebruikt om specifieke eigenschappen maatwerktoepassing en een willekeurige toepassing gegevens. Een toepassing de hoofdtekst van het bericht met het doorgeven van een tekenreekswaarde als het bericht kan worden ingesteld en alle vereiste eigenschappen van de standaard wordt gevuld met standaardwaarden.

Het volgende voorbeeld wordt getoond hoe een testbericht verzenden naar de wachtrij met de naam 'test-wachtrij' Gebruik **verzenden\_wachtrij\_message()**:

```
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Service Bus wachtrijen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een wachtrij gezet, maar er is een initiaal van de totale grootte van de berichten die door een wachtrij gehouden. De wachtrijgrootte van deze is opgegeven bij het maken, met een maximum van 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Hoe u berichten ontvangt van een wachtrij

Berichten worden ontvangen van een wachtrij met de **ontvangen\_wachtrij\_message()** methode op het object **Azure::ServiceBusService** . Standaard worden berichten lezen en vergrendeld zonder wordt verwijderd uit de wachtrij. U kunt echter verwijderen berichten uit de wachtrij als ze worden opgelezen door in te stellen de **: peek_lock** optie op **false**.

Het standaardgedrag zorgt ervoor dat de Leesmodus en verwijderen van een bewerking van twee fasen, waardoor ook ter ondersteuning van toepassingen die niet kunnen zonder ontbrekende berichten. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door te bellen voltooid **verwijderen\_wachtrij\_message()** methode en het bericht moet worden verwijderd als een parameter leveren. De **verwijderen\_wachtrij\_message()** methode wordt het bericht als wordt verbruikt markeren en deze te verwijderen uit de wachtrij.

Als de **: korte weergave\_vergrendelen** parameter zo is ingesteld op **Onwaar**lezen en verwijderen van het bericht wordt het eenvoudigste model en geschikt is voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus wordt het bericht als wordt verbruikt, wanneer de toepassing opnieuw is opgestart en begint verbruikt berichten opnieuw hebt gemarkeerd, wordt hebben dit het bericht verbruikte vóór het vastlopen gemist.

Het volgende voorbeeld wordt getoond hoe ontvangen en verwerken van berichten met **ontvangen\_wachtrij\_message()**. In het voorbeeld eerst ontvangt en Hiermee verwijdert u een bericht met behulp van **: korte weergave\_vergrendelen** ingesteld op **Onwaar**, wordt deze nog een bericht ontvangt en het gebruik van het bericht wordt verwijderd **verwijderen\_wachtrij\_message()**:

```
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hoe u omgaat met toepassing loopt en gelezen berichten

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvanger kan niet worden verwerkt van het bericht om welke reden, wordt deze kunt bellen de **ontgrendelen\_wachtrij\_message()** methode op het object **Azure::ServiceBusService** . Hierdoor Service Bus u kunt het bericht in de wachtrij ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht in de wachtrij vergrendeld en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendelen time-out is verstreken (bijvoorbeeld als de toepassing loopt vast), en vervolgens op de Service wordt het bericht automatisch ontgrendeld en voor het opnieuw worden ontvangen beschikbaar stelt.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat de **verwijderen\_wachtrij\_message()** methode wordt aangeroepen en vervolgens het bericht wordt worden geleverd met de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste één keer Processing**; dat wil zeggen elk bericht ten minste eenmaal worden verwerkt, maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, kunnen softwareontwikkelaars moeten extra logica toevoegen aan de toepassing u omgaat met dubbele bericht is bezorgd. Dit is vaak bereikt met de **bericht\_id** eigenschap van het bericht ongewijzigd over bezorging pogingen.

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Service Bus wachtrijen hebt geleerd, volgt u deze koppelingen voor meer informatie.

-   Overzicht van [wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md).
-   Ga naar de bibliotheek [Azure SDK voor Ruby](https://github.com/Azure/azure-sdk-for-ruby) op GitHub.

Zie voor een vergelijking tussen de Azure Service Bus wachtrijen besproken in dit artikel en Azure wachtrijen beschreven in het artikel [het gebruik van de wachtrij opslag van Ruby](../storage/storage-ruby-how-to-use-queue-storage.md) , [Azure wachtrijen en Azure Service Bus wachtrijen - vergeleken en Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 
