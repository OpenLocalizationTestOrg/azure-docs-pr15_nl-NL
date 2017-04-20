<properties 
    pageTitle="Toepassingen die gebruikmaken van Service Bus wachtrijen schrijven | Microsoft Azure"
    description="Hoe u een eenvoudige wachtrij gebaseerde toepassing waarin Service Bus schrijven."
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-queues"></a>Toepassingen die gebruikmaken van Service Bus wachtrijen maken

In dit onderwerp wordt beschreven Service Bus wachtrijen en ziet u hoe u een eenvoudige wachtrij gebaseerde toepassing waarin Service Bus schrijven.

Bekijk een scenario van de wereld van detailhandel waarin de verkoopgegevens van afzonderlijke Point-of-Sale-gifte moet worden gerouteerd naar een voorraad management waarin de gegevens worden gebruikt om te bepalen wanneer de voorraad worden aangevuld heeft. Deze oplossing wordt gebruikt Service Bus messaging voor de communicatie tussen de overstapstations en het voorraad management-systeem, zoals in de volgende afbeelding:

![Afbeelding van de wachtrijen Service Bus 1](./media/service-bus-create-queues/IC657161.gif)

Elke terminal POS rapporten de verkoopgegevens door het verzenden van berichten naar de **DataCollectionQueue**. Deze berichten blijven in deze wachtrij totdat ze worden opgehaald met de voorraad management systeem. Dit patroon wordt *asynchroon messaging*, vaak genoemd, omdat de terminal POS geen moet wachten om een antwoord van het systeem voor het beheer van voorraad om verder te verwerken.

## <a name="why-queuing"></a>Waarom queuing?

Voordat we naar de code die is vereist kijken voor het instellen van deze toepassing, houd rekening met de voordelen van het gebruik van een wachtrij in dit scenario in plaats van na de overstapstations POS Spreek rechtstreeks (synchroon) aan het voorraad management system.

### <a name="temporal-decoupling"></a>Tijdelijke ontkoppeling

Met het asynchrone SMS patroon hoeft producenten en consumenten niet te worden online op hetzelfde moment. De SMS-infrastructuur betrouwbaar berichten worden opgeslagen totdat de in beslag nemen partij gereed is voor ontvangen. Dit betekent dat de onderdelen van de gedistribueerde toepassing wordt verbroken, hetzij uit eigen beweging; bijvoorbeeld voor onderhoud of vanwege onderdeel vastlopen, zonder dat dit gevolgen heeft het hele systeem. Bovendien de in beslag nemen toepassing mogelijk alleen moet online zijn bepaalde momenten van de dag. Bijvoorbeeld: in dit scenario detailhandel het voorraad management system mogelijk alleen moet online komt na het einde van de werkdag.

### <a name="load-leveling"></a>Laden effenen

In veel toepassingen varieert systeem laden na verloop van tijd dat de verwerkingstijd die is vereist voor elk exemplaar van werk meestal constante is. Intermediating bericht producenten en consumenten met een wachtrij betekent dat de in beslag nemen toepassing (de werknemer) alleen heeft af te handelen, een gemiddelde laden in plaats van een piek laden in te richten. De diepte van de wachtrij zal toenemen en de contracten terwijl de binnenkomende laden varieert. Hiermee slaat u rechtstreeks geld met betrekking tot de hoeveelheid infrastructuur die nodig zijn voor het laden van de toepassing.

![Afbeelding van de wachtrijen Service Bus 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Taakverdeling

Als de belasting toeneemt, kunnen meer werknemer-processen worden toegevoegd aan in de wachtrij werknemer lezen. Elk bericht wordt verwerkt door één van de werknemer-processen. Bovendien kan deze halen gebaseerde taakverdeling voor optimaal gebruik van de werknemer-computers zelfs als de werknemer-computers verschillen op het gebied power verwerking, terwijl ze worden berichten op hun eigen maximumsnelheid halen. Dit patroon wordt vaak de concurrerende consumenten patroon genoemd.

![Afbeelding van de wachtrijen Service Bus 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Losse koppeling

Berichtenwachtrij naar tussenliggende tussen bericht producenten en consumenten beschikt u over een ingebouwde losse koppeling tussen de onderdelen. Omdat producenten en consumenten niet op de hoogte van elkaar zijn, kan een consument zonder gevolgen voor de producent worden bijgewerkt. Bovendien kan de SMS-berichten topologie ontwikkelen handhaven de bestaande eindpunten. We bespreken dit meer wanneer we publiceren/abonneren nader bekeken.

## <a name="show-me-the-code"></a>De code weergeven

De volgende sectie ziet hoe u de Service Bus gebruiken om te maken van deze toepassing.

### <a name="sign-up-for-an-azure-account"></a>Registreren voor een Azure-account

U hebt een Azure-account nodig om na te gaan werken met Service Bus. Als u nog een, kunt u zich aanmeldt voor een gratis account [hier](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Een naamruimte maken

Nadat u een abonnement hebt, kunt u [een nieuwe naamruimte maken](service-bus-create-namespace-portal.md). Elke naamruimte fungeert als een scope container voor een reeks Service Bus entiteiten. Geef uw nieuwe naamruimte een unieke naam alle Service Bus-accounts. 

### <a name="install-the-nuget-package"></a>Het pakket NuGet installeren

Als u wilt de Service Bus naamruimte gebruiken, moet een toepassing verwijzen naar de vergadering Service Bus specifiek Microsoft.ServiceBus.dll. U vindt deze constructie als onderdeel van de Microsoft Azure SDK en het downloaden is beschikbaar op de [downloadpagina van Azure SDK](https://azure.microsoft.com/downloads/). Het [pakket Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is echter de eenvoudigste manier om de Service Bus API en voor het configureren van uw toepassing met alle de Service Bus afhankelijkheden.

### <a name="create-the-queue"></a>De wachtrij maken

Management bewerkingen voor Service Bus messaging entiteiten (onderwerpen wachtrijen en publiceren/abonneren) worden uitgevoerd via de klas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Service Bus gebruikmaakt van een [Gedeeld Access handtekening (SA's)](service-bus-sas-overview.md) op basis beveiligingsmodel. De klasse [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) geeft een token beveiligingstokenprovider met ingebouwde factory methoden sommige bekende token providers retourneren. Een methode [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) gebruiken we de referenties SA's hebben voor. Het exemplaar [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) wordt vervolgens opgesteld met het basisadres van de naamruimte Service Bus en de token-provider.

De klas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) biedt methoden voor het maken, opsommen en SMS entiteiten verwijderen. De code die hier wordt weergegeven, ziet u hoe het exemplaar [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) is gemaakt en gebruikt om de wachtrij **DataCollectionQueue** te maken.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
 
TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Houd er rekening mee dat er overbelastingen van de methode [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) waarmee de eigenschappen van de wachtrij worden ingesteld. U kunt bijvoorbeeld de time to live (TTL) standaardwaarde voor berichten die zijn verzonden naar de wachtrij instellen.

### <a name="send-messages-to-the-queue"></a>Berichten verzenden naar de wachtrij

Voor runtime-bewerkingen voor Service Bus entiteiten; bijvoorbeeld verzenden en ontvangen van berichten, een toepassing moet eerst een [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) object maken. Net als voor de klas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , het exemplaar [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) wordt gemaakt van het basisadres van de Servicenaamruimte en de token-provider.

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Berichten naar verzonden en ontvangen van Service Bus wachtrijen zijn exemplaren van de klasse [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Deze klasse bestaat uit een reeks standaardeigenschappen (zoals [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) en [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), een woordenlijst die wordt gebruikt om de toepassingseigenschappen van de en een hoofdtekst van willekeurige toepassingsgegevens. Een toepassing kunt instellen dat de hoofdtekst door doorgeven in een willekeurig serialiseerbaar object (in het volgende voorbeeld wordt doorgegeven in een **SalesData** -object met de verkoopgegevens vanaf de terminal POS), worden de [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) gebruikt bij het serialiseren van het object. U kunt ook kan een [Stream](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) -object worden verstrekt.

De eenvoudigste manier om berichten te verzenden naar een bepaalde wachtrij, in ons geval de **DataCollectionQueue**, is [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) gebruiken om te maken van een object [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) rechtstreeks vanuit het exemplaar [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) .

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Berichten van de wachtrij ontvangen

Als u wilt ontvangen berichten uit de wachtrij, kunt u een [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) -object dat u rechtstreeks vanuit de [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) met [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)maken. Ontvangers van berichten kunnen werken in twee verschillende modi: **ReceiveAndDelete** en **PeekLock**. De [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) wordt ingesteld wanneer de hoorn van het bericht is gemaakt, als een parameter voor de oproep [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) .


Als u de modus **ReceiveAndDelete** gebruikt, is de ontvangen een foto-bewerking; dat wil zeggen als Service Bus de aanvraag ontvangt, er markeert u het bericht worden gebruikt en naar de toepassing wordt geretourneerd. **ReceiveAndDelete** -modus is de eenvoudigste model en werkt het best voor scenario's waarin de toepassing mogelijk zonder niet verwerken van een bericht als er een fout zijn om te voorkomen. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Aangezien Service Bus dat het bericht als wordt verbruikt, wanneer de toepassing opnieuw opstarten en berichten weer die is gestart, deze wordt heb gemist het bericht verbruikte voordat het vastlopen is gemarkeerd.

In de modus voor **PeekLock** wordt de ontvangen een bewerking van twee fasen, waardoor u deze mogelijk te ondersteunen toepassingen die u kunnen geen ontbrekende berichten zonder. Als Service Bus de aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door de ondersteuning voor [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) op het ontvangen bericht voltooid. Wanneer Service Bus de [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) bellen, wordt het bericht als wordt verbruikt gemarkeerd.

Er zijn twee andere resultaten mogelijk. Eerst als de toepassing kan niet worden verwerkt van het bericht om welke reden, kunt deze bellen [verlaten](https://msdn.microsoft.com/library/azure/hh181837.aspx) op het ontvangen bericht (in plaats van [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Hierdoor Service Bus ontgrendelen van het bericht en deze beschikbaar moeten opnieuw worden ontvangen door de dezelfde consument of door een andere voltooien consumenten. Tweede, is er een time-out die is gekoppeld aan de vergrendeling en als het bericht kan niet worden verwerkt door de toepassing voordat u de vergrendeling time-out verloopt (bijvoorbeeld als de toepassing loopt vast) en klikt u Service Bus wordt het bericht te ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen (in feite bewerking [verlaten](https://msdn.microsoft.com/library/azure/hh181837.aspx) al dan niet standaard).

Houd er rekening mee dat als de toepassing loopt vast na het bericht worden verwerkt, maar vóór de [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) aanvraag is uitgegeven, het bericht wordt worden geleverd met de toepassing opnieuw wordt gestart. Dit wordt vaak *Tenminste eenmaal* verwerking genoemd. Dit betekent dat elk bericht ten minste eenmaal worden verwerkt, maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, is extra logica vereist in de toepassing dubbele waarden vinden. Dit kan worden bereikt op basis van de eigenschap [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) van het bericht. De waarde van deze eigenschap ongewijzigd over bezorging pogingen. Dit wordt *Exact eenmaal* verwerking genoemd.

De code die hier wordt weergegeven, ontvangen en verwerkt berichten in de modus **PeekLock** , dit de standaardwaarde is als geen waarde [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) expliciet is opgegeven.

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
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

### <a name="use-the-queue-client"></a>Gebruik de wachtrij-client

De voorbeelden in deze sectie eerder gemaakt [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) en [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objecten rechtstreeks vanuit de [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) verzenden en ontvangen van berichten uit de wachtrij respectievelijk. Een alternatief methode werkt het gebruik van de klasse [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) , welke ondersteunt verzenden en ontvangen bewerkingen naast meer geavanceerde functies, zoals sessies.

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);
            
BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van wachtrijen hebt geleerd, Zie [maken toepassingen die gebruikmaken van Service Bus onderwerpen en -abonnementen](service-bus-create-topics-subscriptions.md) kunt doorgaan met deze discussie met de mogelijkheden publiceren/abonneren van Service Bus onderwerpen en abonnementen.