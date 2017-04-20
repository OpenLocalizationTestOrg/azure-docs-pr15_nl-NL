<properties
    pageTitle="Service Bus Premium en Standard Messaging prijzen van lagen overzicht | Microsoft Azure"
    description="Service Bus Premium en Standard Messaging"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>

# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Service Bus Premium en Standard SMS lagen 

Service Bus messaging, waaronder messaging entiteiten zoals wachtrijen en onderwerpen, gecombineerd enterprise mogelijkheden met uitgebreide messaging publiceren abonnementen semantiek bij het op schaal van de cloud. Service Bus messaging wordt gebruikt als de backbone communicatie voor veel geavanceerde cloud-oplossingen.

De *Premium* -laag van Service Bus messaging adressen aanvragen van de algemene klanten rond schaal, prestaties en beschikbaarheid voor belangrijke toepassingen. Hoewel de functiesets bijna identiek zijn, worden deze twee lagen van Service Bus messaging bedoeld voor verschillende gebruik gevallen.

Enkele op hoog niveau verschillen zijn gemarkeerd in de onderstaande tabel.

| Premium                               | Standaard                       |
|---------------------------------------|--------------------------------|
| Hoge doorvoer                       | Variabele doorvoer            |
| Overzichtelijk prestaties               | Variabele latentie               |
| Overzichtelijk prijzen                   | Betaald as you go variabele prijzen |
| Mogelijkheid uit te breiden omhoog en omlaag werkbelasting | N/B                            |
| Bericht grootte > 256KB                  | Grootte van het bericht is 256KB          |

**Service Bus Premium Messaging** biedt resource moeten worden geïsoleerd bij de laag van processor en geheugen, zodat de werklast van elke klant op moeten worden geïsoleerd wordt uitgevoerd. Deze resource container heet een *messaging per eenheid*. Elke naamruimte premium is toegewezen aan ten minste één eenheid voor SMS-berichten. U kunt 1, 2 of 4 SMS eenheden voor elke Service Bus Premium naamruimte kopen. Een enkel werkbelasting of entiteit kan beslaan meerdere SMS eenheden en het aantal eenheden SMS-berichten kan worden gewijzigd op wordt Hoewel facturering in de 24-uurs- of dagelijkse bijdragen is. Het resultaat is overzichtelijk en herhaald prestaties voor uw oplossing op basis van een Service Bus.

Niet alleen deze prestaties is meer overzichtelijk en beschikbaar, maar het is ook sneller. Service Bus Premium messaging is gebaseerd op de opslag-engine geïntroduceerd in [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/). Piek prestaties is met Premium messaging, veel sneller dan met de standaard laag.

## <a name="premium-messaging-technical-differences"></a>Premium Messaging technische verschillen

Hier volgen enkele verschillen tussen Premium en Standard SMS lagen.

### <a name="partitioned-queues-and-topics"></a>Gepartitioneerde wachtrijen en onderwerpen

Gepartitioneerde wachtrijen en onderwerpen in Premium messaging worden ondersteund, maar ze werken niet hetzelfde als in de standaardweergave en de Basic niveaus van Service Bus messaging. Premium messaging gebruikt geen SQL als een gegevensopslag en niet meer heeft de concurrentie mogelijke resource die is gekoppeld aan een gedeelde platform. Partitioneren is daardoor niet nodig. Bovendien is het aantal partities gewijzigd van 16 partities in Standard messaging 2 partities in Premium. Hebt twee partities zorgt ervoor dat beschikbaarheid en een meer geschikt is voor de Premium-runtime-omgeving. Zie voor meer informatie over het partitioneren, [Partitioned wachtrijen en onderwerpen](service-bus-partitioning.md).

### <a name="express-entities"></a>Express entiteiten

Aangezien Premium Messaging wordt uitgevoerd in een volledig geïsoleerd runtimeomgeving, wordt express entiteiten worden niet ondersteund in Premium naamruimten. Zie voor meer informatie over de functie express, de eigenschap [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) .

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over het Service Bus messaging.

- [Inleiding tot Azure Service Bus Premium messaging (blogbericht)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Inleiding tot Azure Service Bus Premium messaging (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [SMS-service Bus-overzicht](service-bus-messaging-overview.md)
- [Het gebruik van de Service Bus wachtrijen](service-bus-dotnet-get-started-with-queues.md)
