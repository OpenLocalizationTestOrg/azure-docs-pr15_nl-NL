<properties 
    pageTitle="Automatisch doorverbinden Service Bus messaging entiteiten | Microsoft Azure"
    description="Klik hier voor meer informatie over het koppelen van een wachtrij of een abonnement op een andere wachtrij of onderwerp."
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

# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Service Bus entiteiten met doorverbinden automatisch koppelen

De *auto -* doorstuurfunctie kunt u een abonnement naar een ander wachtrij of onderwerp die deel uitmaakt van dezelfde naamruimte of wachtrij koppelen. Wanneer automatisch doorsturen is ingeschakeld, wordt Service Bus automatisch verwijderd van berichten die in de eerste wachtrij of een abonnement (bron) worden geplaatst en plaatst u deze in de tweede wachtrij of onderwerp (doel). Houd er rekening mee dat het is nog steeds mogelijk een bericht verzenden naar de bestemming entity rechtstreeks. Bovendien is het niet mogelijk om te koppelen van een subwachtrij, zoals een wachtrij, naar een ander wachtrij of onderwerp.

## <a name="using-auto-forwarding"></a>Gebruik van automatisch doorsturen

U kunt automatisch doorsturen inschakelen door in te stellen van de [QueueDescription.ForwardTo][] of [SubscriptionDescription.ForwardTo][] eigenschappen op de objecten [QueueDescription][] of [SubscriptionDescription][] voor de bron, zoals in het volgende voorbeeld.

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

De bestemming entity moet bestaan op het moment dat de bron-entiteit wordt gemaakt. Als de waarde van doelentiteit niet bestaat, retourneert Service Bus een uitzondering wanneer u wordt gevraagd om te maken van de bron-entiteit.

U kunt automatisch doorverbinden schaal van een onderwerp. Service Bus beperkt het [aantal abonnementen op een bepaald onderwerp](service-bus-quotas.md) tot 2.000. U kunt aanvullende abonnementen uitgevoerd door het tweede niveau onderwerpen maken. Houd er rekening mee dat zelfs als u niet door de Service Bus beperking van het aantal abonnementen zijn verbonden, het toevoegen van een tweede niveau met onderwerpen de totale doorvoersnelheid van uw onderwerp verbeteren kunt.

![Stuur automatisch door scenario][0]

U kunt ook automatisch doorverbinden loskoppelen van afzenders van berichten van de ontvangers. Stel dat u een ERP-systeem dat uit drie modules bestaat: doorvoeren, voorraadbeheer en Customer Relationship Management. Elk van deze modules genereert berichten die in wachtrij in een bijbehorende onderwerp zijn. Lisa en Bob zijn verkopers die geïnteresseerd in alle berichten met betrekking tot hun klanten. Als u wilt deze berichten ontvangt, maak Lisa en Bob de een persoonlijke wachtrij en een abonnement op elk van de ERP-onderwerpen die automatisch alle berichten naar hun wachtrij doorsturen.

![Stuur automatisch door scenario][1]

Als Lisa op vakantie, haar persoonlijke wachtrij, maar niet het ERP-onderwerp gaat, vult omhoog. In dit scenario omdat een vertegenwoordiger niet alle berichten, ontvangen geen van de onderwerpen ERP ooit hebt bereikt quotum.

## <a name="auto-forwarding-considerations"></a>Aandachtspunten voor de automatisch doorsturen

Als de bestemming entity cumulatieve veel berichten en overschrijdt het quotum, of de bestemming entity is uitgeschakeld, wordt de bron-entiteit de berichten toegevoegd aan de [Global](service-bus-dead-letter-queues.md) totdat de afstand in de bestemming is (of de entity zich opnieuw worden ingeschakeld). Deze berichten blijft live in de wachtrij, zodat u moet expliciet ontvangen en ze uit de wachtrij verwerken.

Het wordt aanbevolen dat er een beperkt aantal abonnementen op het onderwerp van het eerste niveau en veel abonnementen op het tweede niveau onderwerpen bij het koppelen van desbetreffende onderwerpen voor een samengestelde onderwerp met veel abonnementen. Bijvoorbeeld kunnen een onderwerp van het eerste niveau met 20-abonnement, elk van deze geschakeld naar een onderwerp van het tweede niveau met 200 abonnementen sneller worden verwerkt dan een onderwerp van het eerste niveau met 200 abonnementen, elk geschakeld naar een onderwerp van het tweede niveau met 20 abonnementen.

Service Bus stuklijsten één bewerking voor elk bericht. Bijvoorbeeld een bericht verzenden naar een onderwerp met 20-abonnement, elk van deze berichten automatisch doorsturen naar een ander wachtrij of onderwerp geconfigureerd gefactureerd als 21 bewerkingen als alle abonnementen van het eerste niveau een kopie van het bericht ontvangt.

Als u wilt maken van een abonnement dat is geschakeld naar een andere wachtrij of onderwerp, moet de maker van het abonnement machtigingen **beheren** op zowel de bron en de bestemming entiteit. Verzenden van berichten naar het bron-onderwerp vereist voor het bron-onderwerp machtigingen **verzenden** alleen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor de verwijzing voor gedetailleerde informatie over auto doorverbinden:

- [SubscriptionDescription.ForwardTo][]
- [QueueDescription][]
- [SubscriptionDescription][]

Zie meer informatie over de Service Bus prestatieverbeteringen, [Partitioned messaging entiteiten][].

  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
  [0]: ./media/service-bus-auto-forwarding/IC628631.gif
  [1]: ./media/service-bus-auto-forwarding/IC628632.gif
  [Gepartitioneerde SMS entiteiten]: service-bus-partitioning.md