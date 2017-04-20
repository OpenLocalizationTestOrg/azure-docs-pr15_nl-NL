<properties
    pageTitle="SMS-service Bus-overzicht | Microsoft Azure"
    description="Service Bus Messaging: flexibele gegevens bezorging in de cloud"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="sethm"/>


# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Service Bus messaging: bezorging flexibele gegevens in de cloud

Microsoft Azure-Service Bus is een betrouwbare informatie bezorgingsservice. Het doel van deze service is om de communicatie te vereenvoudigen. Wanneer twee of meer partijen uitwisseling van gegevens wilt, moeten ze een communicatie-om. Service Bus is een methode brokered of externe communicatie. Dit is vergelijkbaar met een postdienst in de fysieke wereld. Diensten u heel gemakkelijk om te verzenden van verschillende soorten brieven en pakketten met een verscheidenheid aan bezorging garanties, overal ter wereld.

Service Bus is vergelijkbaar met de postdienst geven van letters, en is een flexibele levering van de afzender en de geadresseerde. De SMS-service zorgt ervoor dat de informatie die wordt geleverd, zelfs als beide partijen nooit beide online op hetzelfde moment, of als ze zijn niet beschikbaar op hetzelfde moment exacte. Op deze manier is messaging vergelijkbaar met het verzenden van een letter, terwijl de niet-brokered communicatie is vergelijkbaar met een telefoongesprek (of hoe een telefoongesprek gebruikt om te worden - voordat de oproep wachten en beller-ID, die zijn veel meer zoals brokered messaging) plaatsen.

Afzender van het bericht kunt allerlei bezorging kenmerken zoals transacties, dubbele detectie, verlooptijd op basis van tijd en batchen ook instellen. Deze patronen hebben ook posterijen vergelijkingen: Herhaal bezorging, vereiste handtekening, adres wijzigen of intrekken.

Service Bus ondersteunt twee afzonderlijke SMS patronen: *Relay* en *brokered messaging*.

## <a name="service-bus-relay"></a>Service Bus Relay

Het onderdeel [Relay](../service-bus-relay/service-bus-relay-overview.md) van Service Bus is een gecentraliseerde (maar ten zeerste taakverdeling) service die ondersteuning biedt voor een groot aantal verschillende transportprotocollen en Web services standaarden. Dit geldt ook voor SOAP WS-, *, en zelfs REST. De [relayservice](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) biedt een aantal verschillende relay connectiviteitsopties en kunnen helpen Onderhandel over directe peer-to-peer-verbindingen wanneer het is mogelijk. Service Bus is geoptimaliseerd voor .NET-ontwikkelaars die gebruikmaken van de Windows Communication Foundation (WCF), beide met betrekking tot de prestaties en bruikbaarheid, en volledige toegang tot de relayservice via via interfaces zoals SOAP en houd bevat. Hierdoor kunnen voor alle SOAP en de REST programming omgeving integreren met Service Bus.

De relayservice traditionele één richting berichten ondersteunt, aanvraag en respons messaging en peer-to-peer messaging. Ondersteunt ook gebeurtenis verdeling voor Internet-bereik om in te schakelen publiceren abonnementen scenario's en bidirectionele socket communicatie voor betere point-to-point efficiency. In het SMS-berichten indirecte patroon, een on-premises implementatie-service is verbonden met de relayservice via een uitgaande poort en Hiermee maakt u een socket bidirectionele voor communicatie die zijn gekoppeld aan een bepaald rendezvous-adres. De client communiceren door het verzenden van berichten naar de relayservice het adres rendezvous doelgroepen vervolgens met de on-premises implementatie-service. De relayservice wordt vervolgens "" berichten doorsturen naar de on-premises-service via de socket bidirectionele al op de plaats. De client hoeft niet een directe verbinding met de service on-premises implementatie, noch dit nodig is om te weten waar de service is geïnstalleerd en de on-premises implementatie-service hoeft niet alle binnenkomende poorten open op de firewall.

U start de verbinding tussen uw on-premises implementatie-service en de relayservice, met een reeks WCF "relay" bindingen. Achter de schermen Wijs de bindingen relay om bindingselementen die zijn ontworpen voor het maken van WCF kanaal onderdelen die zijn geïntegreerd met Service Bus in de cloud.

Service Bus Relay biedt veel voordelen, maar moeten dat de server en client tot beide online zijn op hetzelfde moment om te kunnen verzenden en ontvangen van berichten. Dit is geen optimale voor communicatie van HTTP-stijl, waarin de aanvragen mogelijk niet meestal lange levensduur, noch voor clients die verbinding maken met alleen af en toe, zoals browsers, mobiele toepassingen, enzovoort. Brokered messaging ondersteunt losgekoppelde communicatie en heeft een eigen voordelen; clients en servers kunnen verbinding maken wanneer nodig en hun bewerkingen op asynchrone wijze uitvoeren.

## <a name="brokered-messaging"></a>Brokered messaging

In tegenstelling tot het schema relay kan [brokered messaging](service-bus-queues-topics-subscriptions.md) worden beschouwd als asynchroon, of "ondersteuningsbeheerder ontkoppelde." Producenten (afzenders) en consumenten (ontvangers) hoeft niet te op hetzelfde moment online zijn. Berichten in een 'makelaar' de SMS-infrastructuur betrouwbaar opgeslagen (zoals een wachtrij) totdat de in beslag nemen partij gereed is voor ontvangen. Hierdoor kunnen de onderdelen van de gedistribueerde toepassing beëindigd, hetzij uit eigen beweging; bijvoorbeeld voor onderhoud of vanwege onderdeel vastlopen, zonder dat dit gevolgen heeft het gehele systeem. Bovendien de ontvangst-toepassing mogelijk alleen moet bepaalde momenten van de dag, zoals een systeem voor het beheer van voorraad die alleen is vereist om uit te voeren aan het einde van de werkdag online komt.

De belangrijkste onderdelen van de infrastructuur voor Service Bus-brokered zijn wachtrijen, onderwerpen en abonnementen.  Het belangrijkste verschil is dat onderwerpen publiceren/abonneren mogelijkheden die kunnen worden gebruikt voor geavanceerde inhoud gebaseerde routering en de bezorging van logica, waaronder het verzenden naar meerdere geadresseerden ondersteuning. Deze onderdelen nieuwe asynchroon SMS scenario's, zoals tijdelijke ontkoppeling inschakelen, publiceren/abonneren en taakverdeling. Zie voor meer informatie over deze SMS entiteiten, [Service Bus wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md).

Net als met de infrastructuur Relay, wordt de brokered SMS mogelijkheid voor WCF en .NET Framework-programmeurs en ook via REST aangeboden.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over het Service Bus messaging.

- [Service Bus over grondbeginselen](service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md)
- [Het gebruik van de Service Bus wachtrijen](service-bus-dotnet-get-started-with-queues.md)
- [Het gebruik van de Service Bus onderwerpen en abonnementen](./service-bus-dotnet-how-to-use-topics-subscriptions.md)
 
