<properties 
    pageTitle="Maken van toepassingen die gebruikmaken van Service Bus onderwerpen en abonnementen | Microsoft Azure"
    description="Inleiding tot het publiceren-abonneren mogelijkheden aangeboden door Service Bus onderwerpen en abonnementen."
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Toepassingen die gebruikmaken van Service Bus onderwerpen en abonnementen maken

Azure-Service Bus ondersteunt een set cloudgebaseerde, bericht-georiënteerd middleware technologieën inclusief betrouwbare wachtrijen en duurzame publiceren/abonneren messaging. In dit artikel is gebaseerd op de informatie in [maken toepassingen die gebruikmaken van Service Bus wachtrijen](service-bus-create-queues.md) en biedt een introductie over de mogelijkheden publiceren/abonneren door de Service Bus onderwerpen worden aangeboden.

## <a name="evolving-retail-scenario"></a>Veranderende detailhandel-scenario

In dit artikel blijft de detailhandel-scenario gebruikt in [maken toepassingen die Service Bus wachtrijen gebruiken](service-bus-create-queues.md). Intrekken die verkoopgegevens van afzonderlijke punt van verkoop (POS) overstapstations moeten worden gerouteerd naar een systeem voor het beheer van voorraad waarin die gegevens gebruikt om te bepalen wanneer de voorraad worden aangevuld heeft. Elke terminal POS de veelvouden in Power View-rapporten door het verzenden van berichten naar de wachtrij **DataCollectionQueue** , waar ze blijven totdat ze worden ontvangen door het systeem van voorraad management, zoals hier wordt getoond:

![Service Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Als u wilt dit scenario ontwikkelen, een nieuwe vereiste is toegevoegd aan het systeem: de eigenaar van de store wil kunt controleren hoe de store in realtime wordt uitgevoerd.

Als u wilt deze vereiste adres, moet het systeem "toegang" uit de stream verkoopgegevens. We nog steeds elk bericht dat is verzonden door de overstapstations POS worden verzonden naar het management system voorraad voordat als wilt, maar we nog een exemplaar van elk bericht dat we gebruiken kunt om aan te geven van de dashboardweergave aan de eigenaar van de store.

U kunt in elke situatie zoals dit, waarin u elk bericht moet worden gebruikt door meerdere partijen vereist Service Bus *onderwerpen*. Onderwerpen bevatten een patroon publiceren/abonneren waarin elke gepubliceerde bericht beschikbaar wordt gemaakt voor een of meer abonnementen die zijn geregistreerd bij het onderwerp. Daarentegen is wachtrijen voor elk bericht ontvangen door een enkel consument.

Berichten worden verzonden naar een onderwerp op dezelfde manier als ze zijn verzonden naar een wachtrij. Echter worden berichten niet ontvangen van het onderwerp rechtstreeks; Deze worden ontvangen van een abonnement. U kunt zien van een abonnement op een onderwerp als een virtuele wachtrij die kopieën van de berichten die zijn verzonden naar dat onderwerp ontvangt. Berichten worden ontvangen van een abonnement op dezelfde manier als ze worden ontvangen uit een wachtrij.

Teruggaan naar de detailhandel-scenario, de wachtrij wordt vervangen door een onderwerp en een abonnement is toegevoegd, waarin de systeemonderdeel voorraad management kunt gebruiken. Het systeem wordt nu als volgt weergegeven:

![Service Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

De configuratie hier kunt u dezelfde uitvoeren naar het vorige wachtrij gebaseerde ontwerp. Dat wil zeggen berichten die zijn verzonden naar het onderwerp naar de **voorraad** -abonnement, van waaruit het **Voorraadbeheer** ze verbruikt doorgestuurd.

Ter ondersteuning van het dashboard management, maak een tweede abonnement op het onderwerp, zoals hier wordt getoond:

![Service Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Met deze configuratie wordt elk bericht van de overstapstations POS bestaat uit de beschikbaar op het **Dashboard** en de **voorraad** -abonnementen.

## <a name="show-me-the-code"></a>De code weergeven

Het artikel [maken toepassingen die gebruikmaken van Service Bus wachtrijen](service-bus-create-queues.md) uitgelegd hoe u zich registreren voor een Azure-account en een Servicenaamruimte maken. Als u wilt een naamruimte Service Bus gebruiken, moet een toepassing verwijzen naar de vergadering Service Bus specifiek Microsoft.ServiceBus.dll. De eenvoudigste manier om te verwijzen naar de Service Bus afhankelijkheden is voor het installeren van de Service Bus [Nuget-pakket](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). U kunt ook de vergadering als onderdeel van de Azure SDK vinden. Het downloaden is beschikbaar op de [downloadpagina van Azure SDK](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Het onderwerp en de abonnementen maken

Management bewerkingen voor Service Bus messaging entiteiten (onderwerpen wachtrijen en publiceren/abonneren) worden uitgevoerd via de klas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Juiste referenties zijn vereist om te kunnen maken van een exemplaar [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) voor een bepaalde naamruimte. Service Bus gebruikmaakt van een [Gedeeld Access handtekening (SA's)](service-bus-sas-overview.md) op basis beveiligingsmodel. De klasse [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) geeft een token beveiligingstokenprovider met ingebouwde factory methoden sommige bekende token providers retourneren. Een methode [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) gebruiken we de referenties SA's hebben voor. Het exemplaar [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) wordt vervolgens opgesteld met het basisadres van de naamruimte Service Bus en de token-provider.

De klas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) biedt methoden voor het maken, opsommen en SMS entiteiten verwijderen. De code die hier wordt weergegeven, ziet u hoe het exemplaar [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) is gemaakt en gebruikt om te maken van het onderwerp **DataCollectionTopic** .

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
     
TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);
 
namespaceManager.CreateTopic("DataCollectionTopic");
```

Houd er rekening mee dat er overbelastingen van de methode [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) waarmee u kunt het instellen van eigenschappen van het onderwerp. U kunt bijvoorbeeld de time to live (TTL) standaardwaarde voor berichten die zijn verzonden naar het onderwerp instellen. Voeg nu de **voorraad** en **Dashboard** abonnementen.

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Berichten verzenden naar het onderwerp

Voor runtime-bewerkingen voor Service Bus entiteiten; bijvoorbeeld verzenden en ontvangen van berichten, een toepassing moet eerst een [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) object maken. Net als voor de klas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , het exemplaar [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) wordt gemaakt van het basisadres van de Servicenaamruimte en de token-provider.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Berichten naar verzonden en ontvangen van Service Bus onderwerpen, worden exemplaren van de klasse [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Deze klasse bestaat uit een reeks standaardeigenschappen (zoals [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) en [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), een woordenlijst die wordt gebruikt om de toepassingseigenschappen van de en een hoofdtekst van willekeurige toepassingsgegevens. Een toepassing kunt instellen dat de hoofdtekst door doorgeven in een willekeurig serialiseerbaar object (in het volgende voorbeeld wordt doorgegeven in een **SalesData** -object met de verkoopgegevens vanaf de terminal POS), worden de [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) gebruikt bij het serialiseren van het object. U kunt ook kan een [Stream](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) -object worden verstrekt.

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

De eenvoudigste manier om berichten te verzenden naar het onderwerp is [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) gebruiken om te maken van een object [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) rechtstreeks vanuit het exemplaar [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) .

```
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Berichten ontvangt van een abonnement

Dit is vergelijkbaar met het gebruik van wachtrijen voor het ontvangen van berichten van een abonnement die kunt u een [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) -object dat u rechtstreeks vanuit de [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) met [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)maken. U kunt een van de twee verschillende ontvangen modi (**ReceiveAndDelete** en **PeekLock**), zoals is beschreven in [maken toepassingen die Service Bus wachtrijen gebruiken](service-bus-create-queues.md).

Houd er rekening mee dat wanneer u een [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) voor abonnementen maakt, de parameter *entityPath* aan het formulier is `topicPath/subscriptions/subscriptionName`. Daarom als wilt maken van een [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) voor het abonnement **voorraad** van het onderwerp **DataCollectionTopic** , *entityPath* moet zijn ingesteld op `DataCollectionTopic/subscriptions/Inventory`. De code ziet er als volgt uit:

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Abonnement filters

Tot dusverre in dit scenario zijn alle berichten die naar het onderwerp beschikbaar gesteld voor alle geregistreerde abonnementen. De belangrijkste woordgroep hier is 'beschikbaar worden gemaakt." Terwijl de Service Bus abonnementen ziet alle berichten die naar het onderwerp, kunt u slechts een subset van deze berichten kopiëren naar de wachtrij virtuele abonnement. Dit is uitgevoerd met abonnement *filters*. Wanneer u een abonnement hebt gemaakt, kunt u een filterexpressie in de vorm van een SQL92-standaard stijl predikaat dat via de eigenschappen van het bericht, zowel de Systeemeigenschappen (bijvoorbeeld [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) als de toepassingseigenschappen, zoals **winkelnaam** in het vorige voorbeeld werkt kunt opgeven.

Het scenario om te illustreren dit ontwikkeling, is een tweede winkel moet worden toegevoegd aan ons scenario detailhandel. Verkoopgegevens uit alle de overstapstations POS van beide winkels nog steeds moet worden doorgestuurd naar het centrale voorraad management system, maar de beheerder van een store het hulpprogramma voor het dashboard is alleen geïnteresseerd in de prestaties van die winkel. U kunt filteren dit abonnement. Houd er rekening mee dat wanneer de overstapstations POS berichten publiceert, ze stelt u de **winkelnaam** toepassing eigenschap op het bericht. Twee winkels, bijvoorbeeld **Redmond** en **Seattle**, gegeven opslaan de overstapstations POS in de Redmond tijdstempel hun verkoopgegevens berichten met een **winkelnaam** gelijk is aan **Redmond**, terwijl de Seattle store POS overstapstations een **winkelnaam gebruiken** gelijk aan **Seattle**. De store manager is van de store Redmond wil alleen gegevens uit de overstapstations POS bekijken. Het systeem ziet er als volgt uit:

![Service-Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Als u deze routering instelt, maakt u de **Dashboard** abonnement als volgt:

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Met dit filter abonnement worden alleen berichten met de eigenschap **winkelnaam** is ingesteld op **Redmond** gekopieerd naar de virtuele wachtrij voor het abonnement **Dashboard** . Er is veel meer met het filteren van abonnement, echter. Toepassingen kunnen meerdere filterregels per abonnement naast de mogelijkheid om te wijzigen van de eigenschappen van een bericht zoals dit wordt doorgegeven aan de virtuele wachtrij van een abonnement hebben.

## <a name="summary"></a>Overzicht

Alle redenen om gebruiken queuing omschreven in de [toepassingen die gebruikmaken van Service Bus wachtrijen maken](service-bus-create-queues.md) ook van toepassing op onderwerpen, specifiek:

- Tijdelijke ontkoppeling – bericht producenten en consumenten hoeft niet te op hetzelfde moment online zijn.

- Laden effenen: pieken in laden worden geëffend af door het onderwerp waardoor in beslag nemen toepassingen voorzien van gegevens voor gemiddelde laden in plaats van piek laden.

- Taakverdeling: vergelijkbaar met een wachtrij, kunt u beschikken over meerdere concurrerende consumenten luisteren op een één abonnement, met elk bericht ingeleverd bij slechts één van de consumenten, waardoor het verdelen van laden.

- Losse koppeling: u kunt het SMS-netwerk ontwikkelen handhaven bestaande eindpunten; bijvoorbeeld abonnementen toevoegen of wijzigen van filters in een onderwerp toe te staan dat voor nieuwe gebruikers.

## <a name="next-steps"></a>Volgende stappen

Zie de [toepassingen die gebruikmaken van Service Bus wachtrijen maken](service-bus-create-queues.md) voor informatie over het gebruik van wachtrijen in het POS detailhandel scenario.