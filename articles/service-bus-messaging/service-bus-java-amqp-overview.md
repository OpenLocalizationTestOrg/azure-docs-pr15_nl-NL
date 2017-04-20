<properties 
    pageTitle="Overzicht van de service Bus AMQP met Java | Microsoft Azure" 
    description="Meer informatie over het gebruik van Java met de geavanceerde bericht Queuing-Protocol (AMQP) 1.0 in Azure wordt aangegeven." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="amqp-10-support-in-service-bus"></a>AMQP 1.0-ondersteuning in Service Bus

Azure-Service busdienst cloud zowel on-premises [Service Bus voor Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) ondersteunt de geavanceerde bericht wachtrij Protocol (AMQP) 1.0. AMQP kunt u meerdere platforms, hybride toepassingen met een open standaard protocol maken. U kunt de toepassingen die gebruikmaken van onderdelen die zijn gemaakt met verschillende talen en kaders en die worden uitgevoerd op verschillende besturingssystemen maken. Alle deze onderdelen verbinding met Service Bus en naadloos maken kunnen uitwisselen gestructureerde bedrijven berichten efficiënt en klik in de volledige kan optreden.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Inleiding: Wat is AMQP 1.0 en waarom is het belangrijk?

Traditioneel, bericht-georiënteerd middlewareproducten oorspronkelijke protocollen voor communicatie tussen clienttoepassingen en makelaars gebruikt. Dit betekent wanneer u een bepaalde leverancier SMS makelaar hebt geselecteerd, u van de leverancier bibliotheken gebruiken moet om verbinding maken met uw clienttoepassingen met die makelaar. Dit levert een mate van afhankelijkheid van die leverancier, aangezien het overbrengen van een toepassing naar een ander product codewijzigingen in de verbonden toepassingen vereist. 

Bovendien is het lastig SMS makelaars verbinding van andere leveranciers. Dit is meestal vereist niveau van de webtoepassing bruggen berichten van het ene systeem verplaatsen naar een ander en tussen hun eigen berichtindelingen vertalen. Dit is een algemene vereiste; bijvoorbeeld wanneer u moet een nieuwe geïntegreerd interface bieden tot oudere verschillende systemen of IT-systemen volgen van een fusie integreren.

De software-industrie is een bedrijf snel bewegende; nieuwe programming talen en -toepassingskaders worden geïntroduceerd met een soms heel breed snelheid. Op dezelfde manier de vereisten van IT-systemen ontwikkelen na verloop van tijd en ontwikkelaars wilt profiteren van de nieuwste functies van platform. Maar biedt soms de geselecteerde SMS leverancier geen ondersteuning deze platforms. Aangezien messaging-protocollen, bedrijfseigen, is het niet mogelijk voor bibliotheken bieden voor deze nieuwe platforms door anderen. Daarom moet u methoden zoals het bouwen van gateways of bruggen waarmee u kunt doorgaan met het SMS-berichten product gebruiken.

De ontwikkeling van de geavanceerde bericht Queuing-Protocol (AMQP) 1.0 onmogelijk was deze problemen. Deze afkomstig is van JP Morgan Chase, die, zoals de meest financiële services ondernemingen, zijn veel gebruikers van de bericht-georiënteerd middleware. Het doel is eenvoudig: maken van een open-standaard SMS protocol dat maken op basis van berichten toepassingen met de onderdelen die zijn gemaakt met verschillende talen, kaders en besturingssystemen, alles met behulp van aanbevolen beschikbare onderdelen van een bereik van leveranciers raadplegen.

## <a name="amqp-10-technical-features"></a>AMQP 1.0 technische functies

AMQP 1.0 is een efficiënte, betrouwbare, kabel niveau SMS protocol dat u gebruiken kunt om te maken van krachtige, platforms messaging toepassingen. Het protocol heeft een eenvoudige doel: het mechanisme van de veilig, betrouwbaar en efficiënt overdracht van berichten tussen twee partijen definiëren. De berichten zelf worden gecodeerd met een weergave draagbare gegevens waarmee heterogene afzenders en ontvangers gestructureerde bedrijven berichten op volledige beeldkwaliteit uitwisselen. Hier volgt een overzicht van de belangrijkste functies:

*    **Efficiënt**: AMQP 1.0 is een verbinding-georiënteerd protocol dat wordt gebruikt een binaire codering voor de protocol-instructies en de berichten bedrijven overgedragen. Het product te maximaliseren van het gebruik van het netwerk en de verbonden onderdelen stelsels van geavanceerde stroom-besturingselementen omvat. Echter wel verwijzen, het protocol is ontworpen voor een balans tussen efficiency, flexibiliteit en interoperabiliteit aantekening.
*    **Betrouwbaar**: het AMQP 1.0-protocol kunnen berichten toe te staan met een bereik van betrouwbaarheid garanties, uit fire-en-vergeet naar betrouwbaar, precies-eenmaal bevestigd bezorging.
*    **Flexibele**: AMQP 1.0 is een flexibele protocol dat kan worden gebruikt voor de ondersteuning van verschillende topologieën. Hetzelfde protocol kan worden gebruikt voor client-naar-client, client-naar-makelaar en makelaar-naar-makelaar communicatie.
*    **Makelaar-model onafhankelijke**: het AMQP 1.0-specificatie maakt geen eventuele vereisten model van de SMS-berichten die worden gebruikt door een makelaar. Dit betekent dat het is mogelijk eenvoudig AMQP 1.0-ondersteuning voor bestaande SMS elke makelaar toevoegen.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 is een standaard (met een hoofdletter van ')

AMQP 1.0 is in de ontwikkelingsfase bevindt sinds 2008 door een core-groep van meer dan 20 bedrijven, technologie leveranciers zowel eindgebruikers ondernemingen. Gebruiker ondernemingen hebben de vereisten van hun echte bedrijven bijgedragen gedurende die tijd en de technologieleveranciers hebt die is voortgekomen het protocol aan deze vereisten voldoet. Overal in het proces, hebben leveranciers deelgenomen aan workshops waarin ze voor het valideren van de interoperabiliteit tussen hun implementaties samengewerkt.

In oktober 2011, is het ontwikkelingswerk die worden overgebracht naar een technische Commissie binnen de organisatie voor de ontwikkeling van gestructureerde informatie standaarden (OASIS) en de standaard OASIS AMQP 1.0 uitgebracht in oktober 2012. De volgende ondernemingen deelgenomen in de technische Commissie tijdens de ontwikkeling van de standaard:

*    **Leveranciers van technologie**: Axway Software, Huawei technologieën, vorm Software, INETCO systemen, Kaazing, Microsoft, Mitre Corporation, Primeton technologieën, voortgang Software, rood rol, SITA, Software AG, Solace systemen, VMware, WSO2, Zenika.
*    **Gebruiker ondernemingen**: Bank-van-Amerika, krediet Suisse, Deutsche Boerse, Goldman Sachs, JPMorgan Chase.

Enkele van de meest geciteerde voordelen van open standaarden zijn:

*    Minder risico's vaste leverancier
*    Interoperabiliteit
*    Algemene beschikbaarheid van bibliotheken en uitrusting
*    Bescherming tegen veroudering
*    Beschikbaarheid van kennis personeel
*    Lagere en beheerbare risico

## <a name="amqp-10-and-service-bus"></a>AMQP 1,0 en Service Bus

AMQP 1.0-ondersteuning in Azure Service Bus betekent dat u kunt nu gebruikmaken van het in de Service Bus wachtrij en publiceren/abonneren brokered functies berichten uit een reeks met een efficiënte binaire protocol platforms. Bovendien kunt u toepassingen bestaat uit onderdelen die zijn gemaakt met een combinatie van talen, kaders en besturingssystemen maken.

De volgende afbeelding ziet u een voorbeeld-implementatie waarin Java-clients op Linux, geschreven met de standaard Java bericht Service (JMS) API en .NET-clients met Windows, berichten via Service-Bus met AMQP 1.0 uitwisselen.

![][0]

**Afbeelding 1: Implementatie voorbeeldscenario met meerdere platforms messaging Service Bus en AMQP 1.0 gebruiken**

De volgende clientbibliotheken zijn op dit moment bekende voor gebruik met Service Bus:

| Taal | Bibliotheek                                                                       |
|----------|-------------------------------------------------------------------------------|
| Java     | Apache Qpid Java bericht Service (JMS)-client<br/>VORM Software SwiftMQ Java-client |
| C        | Apache Qpid Proton-C                                                          |
| PHP      | Apache Qpid Proton-PHP                                                        |
| Python   | Apache Qpid Proton-Python                                                     |


**Afbeelding 2: Inhoudsopgave AMQP 1.0-client-bibliotheken**

Zie voor meer informatie over het downloaden en gebruiken van deze bibliotheken met Service Bus [Service Bus AMQP handleiding voor ontwikkelaars][]. Zie het gedeelte van de [volgende stappen](service-bus-java-amqp-overview.md#next-steps) voor koppelingen naar meer informatie.

## <a name="summary"></a>Overzicht

*    AMQP 1.0 is een openen, betrouwbare SMS protocol waarmee u kunt u meerdere platforms, hybride toepassingen kunt maken. AMQP 1.0 is een standaard OASIS.
*    AMQP 1.0-ondersteuning is nu beschikbaar in Azure Service Bus, evenals Service Bus voor Windows Server (Service Bus 1.1). Prijzen is hetzelfde als de bestaande protocollen voor.

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende koppelingen voor meer informatie over ondersteuning voor AMQP in Service Bus.

*    [Hoe u AMQP 1.0 met de Service Bus .NET-API](service-bus-dotnet-advanced-message-queuing.md)
*    [Het gebruik van de Java bericht Service (JMS) API met Service No & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Service-handleiding voor Bus AMQP ontwikkelaars][]
*    [OASIS geavanceerde bericht Queuing-Protocol (AMQP) versie 1.0-specificatie](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)

[0]: ./media/service-bus-java-amqp-overview/Example1.png
[Service-handleiding voor Bus AMQP ontwikkelaars]: service-bus-amqp-dotnet.md

 
