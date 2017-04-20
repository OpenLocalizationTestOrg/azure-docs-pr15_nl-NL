<properties
    pageTitle="Overzicht van de service Bus relay | Microsoft Azure"
    description="Overzicht van de Service Bus relay."
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
    ms.date="09/01/2016"
    ms.author="sethm"/>


# <a name="overview-of-service-bus-relay"></a>Overzicht van de Service Bus relay

Een belangrijk onderdeel van Service Bus is een gecentraliseerde (maar ten zeerste taakverdeling) *relay* -service waarmee u hybride toepassingen maken die worden uitgevoerd in een datacenter van de Azure zowel uw eigen enterprise on-premises omgeving.  De Service Bus relay ondersteunt een groot aantal verschillende transportprotocollen en web services standaarden. Dit geldt ook voor SOAP WS-, *, en zelfs REST. De relayservice kunt u uw hybride toepassingen doordat u veilig worden Windows Communication Foundation (WCF)-services die zich bevinden binnen een zakelijke enterprise-netwerk met de openbare cloud zonder te hoeven openen van een firewallverbinding of storende wijzigingen moeten de infrastructuur van een bedrijfsnetwerk vereisen. 

![Relay concepten](./media/service-bus-relay-overview/sb-relay-01.png)

De relayservice traditionele één richting berichten ondersteunt, aanvraag en respons messaging en peer-to-peer messaging. Ondersteunt ook gebeurtenis verdeling voor internet-bereik publiceren/abonneren scenario's en bidirectionele socket communicatie voor betere point-to-point efficiency kunt inschakelen. 

In het SMS-berichten indirecte patroon, een on-premises implementatie-service is verbonden met de relayservice via een uitgaande poort en Hiermee maakt u een socket bidirectionele voor communicatie die zijn gekoppeld aan een bepaald rendezvous-adres. De client communiceren door het verzenden van berichten naar de relayservice het adres rendezvous doelgroepen vervolgens met de on-premises implementatie-service. De relayservice wordt vervolgens "" berichten doorsturen naar de on-premises-service via de socket bidirectionele al op de plaats. De client hoeft niet een directe verbinding met de on-premises implementatie-service, is niet verplicht weten waar de service is geïnstalleerd en de on-premises implementatie-service hoeft niet alle binnenkomende poorten open op de firewall.

U start de verbinding tussen uw on-premises-service en de relayservice met een reeks WCF "relay" bindingen. Achter de schermen, wordt de bindingen relay toewijzen aan nieuwe transport bindingselementen ontworpen voor het maken van WCF kanaal onderdelen die zijn geïntegreerd met Service Bus in de cloud. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service Bus-relay.

- [Overzicht van Azure Service Bus architectuur](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Het gebruik van de service-Service Bus Relay](service-bus-dotnet-how-to-use-relay.md)

 