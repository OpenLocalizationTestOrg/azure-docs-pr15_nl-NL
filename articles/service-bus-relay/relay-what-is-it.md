<properties
    pageTitle="Wat is Azure relay? | Microsoft Azure"
    description="Overzicht van Azure Relay"
    services="service-bus"
    documentationCenter=".net"
    authors="banisadr"
    manager="timlt"
    editor="" />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="babanisa" />

# <a name="what-is-azure-relay"></a>Wat is Azure Relay?

Azure relayservice vergemakkelijkt uw toepassingen hybride doordat u veilig worden services die zich bevinden binnen een zakelijke enterprise-netwerk met de openbare cloud zonder te hoeven openen van een firewallverbinding of storende wijzigingen moeten de infrastructuur van een bedrijfsnetwerk vereisen. Relay ondersteunt een groot aantal verschillende transportprotocollen en web services standaarden.

De relayservice ondersteunt traditionele één richting aanvraag en respons en peer-to-peer-verkeer is toegestaan. Ondersteunt ook gebeurtenis verdeling voor internet-bereik publiceren/abonneren scenario's en bidirectionele socket communicatie voor betere point-to-point efficiency kunt inschakelen. 

In het patroon dat in de overdracht indirecte gegevens, een on-premises implementatie-service is verbonden met de relayservice via een uitgaande poort en Hiermee maakt u een socket bidirectionele voor communicatie die zijn gekoppeld aan een bepaald rendezvous-adres. De client communiceren door te sturen van verkeer naar de relayservice het adres rendezvous doelgroepen vervolgens met de on-premises implementatie-service. De relayservice wordt vervolgens "doorsturen" gegevens naar de on-premises-service via een bidirectionele socket specifiek voor elke klant. De client hoeft niet een directe verbinding met de on-premises implementatie-service, is niet verplicht weten waar de service is geïnstalleerd en de on-premises implementatie-service hoeft niet alle binnenkomende poorten open op de firewall.

De elementen belangrijke mogelijkheid is verstrekt door Relay bidirectionele, niet-gebufferde communicatie zijn buiten het bedrijfsnetwerk bevinden met TCP-achtige beperken, eindpunt discovery, connectivity status en wat eindpunt beveiliging. De mogelijkheden van relay afwijken van netwerk niveau integratietechnologieën zoals VPN in dat deze kan worden beperkt tot een eindpunt één toepassing op één computer, terwijl de VPN-technologie is veel meer storende als dit is afhankelijk van het wijzigen van de netwerkomgeving.

Azure Relay heeft twee functies:

1. [Hybride verbindingen](#hybrid-connections) - gebruikmaakt van de open standaard Web Sockets inschakelen scenario's voor meerdere platforms

2. [WCF-relais](#wcf-relays) - gebruikmaakt van Windows Communication Foundation externe procedurele oproepen inschakelen

Hybride verbindingen en WCF-relais inschakelen beveiligde verbinding met assests die zijn opgeslagen in een zakelijke enterprise-netwerk. Gebruik van elkaar is afhankelijk van uw specifieke wensen hieronder beschreven:

|                                    | WCF-Relay | Hybride verbindingen |
| ---------------------------------- |:---------:|:------------------:|
| **WCF**                            |     x     |                    |
| **.NET core**                      |           |         x          |
| **.NET framework**                 |     x     |         x          |
| **JavaScript/NodeJS**              |           |         x          |
| **Java***                          |           |         x          |
| **Open Protocol gestandaardiseerde**  |           |         x          |
| **Meerdere RPC Programing modellen** |           |         x          |
*<sub>Door de algemene beschikbaarheid</sub>

## <a name="hybrid-connections"></a>Hybride verbindingen

Azure Relay hybride verbindingen de mogelijkheid is een beveiligde, openen-protocol ontwikkeling van de bestaande Relay-functies die kunnen worden geïmplementeerd op een willekeurig platform en in een taal die heeft een eenvoudige WebSocket mogelijkheid, waaronder expliciet de API WebSocket gemeenschappelijke webbrowsers. Hybride verbindingen is gebaseerd op http- en WebSockets.

## <a name="wcf-relays"></a>WCF-relais

Het WCF-Relay werkt voor het volledige .NET Framework (NETFX) en voor WCF. U start de verbinding tussen uw on-premises-service en de relayservice met een reeks WCF "relay" bindingen. Achter de schermen, wordt de bindingen relay toewijzen aan nieuwe transport bindingselementen ontworpen voor het maken van WCF kanaal onderdelen die zijn geïntegreerd met Service Bus in de cloud.

## <a name="service-history"></a>Servicegeschiedenis

Hybride verbindingen verspeende het eerste gelijkmatig met de naam "BizTalk Services"-functie die is gebaseerd op de Azure WCF-Bus Relay de Service. De nieuwe hybride verbindingen mogelijkheid is een aanvulling op de bestaande WCF-Relay en deze twee mogelijkheden aanwezig naast elkaar in de relayservice voor de nabije toekomst; ze geen algemene gateway delen, maar niet zo is verschillende implementaties.

WCF-Relay is de oudere Relay aanbod dat veel klanten al met hun WCF programing-modellen gebruikt mogelijk.

## <a name="next-steps"></a>Volgende stappen:

- [Relay Veelgestelde vragen](relay-faq.md)
- [Een naamruimte maken](relay-create-namespace-portal.md)
- [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Aan de slag met knooppunt](relay-hybrid-connections-node-get-started.md)