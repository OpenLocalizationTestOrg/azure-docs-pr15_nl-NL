<properties 
    pageTitle="Service-busarchitectuur | Microsoft Azure"
    description="Beschrijving van het bericht en relay verwerken van de architectuur van Azure Service Bus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm" />

# <a name="service-bus-architecture"></a>Service-busarchitectuur

In dit artikel worden de bericht- en relay verwerken van de architectuur van Azure Service Bus.

## <a name="service-bus-scale-units"></a>Service Bus schaaleenheden

Service Bus is geordend op *schaaleenheden*. Een eenheid voor tijdschaal is een maateenheid implementatie en bevat alle vereiste onderdelen uitvoeren van de service. Elke regio implementeert een of meer Service Bus schaaleenheden.

Een Service Bus naamruimte is toegewezen aan een eenheid voor tijdschaal. De eenheid voor tijdschaal omgaat met alle typen Service Bus entiteiten: relais en brokered SMS entiteiten (wachtrijen, onderwerpen, abonnementen). Een eenheid voor tijdschaal Service Bus bestaat uit de volgende onderdelen:

- **Een reeks knooppunten van de gateway.** Gateway-knooppunten verifiëren verzoeken voor oproepen en relay aanvragen afhandelen. Elk knooppunt gateway heeft een openbare IP-adres.

- **Een reeks makelaar knooppunten messaging.** SMS-berichten makelaar knooppunten verwerken van aanvragen betreffende SMS entiteiten.

- **Een gateway store.** De gateway-store bevat de gegevens voor elke entiteit die is gedefinieerd in deze eenheid voor tijdschaal. De gateway-store wordt geïmplementeerd boven aan een SQL Azure-database.

- **Meerdere berichten winkels.** SMS-berichten winkels houdt u de berichten van alle wachtrijen, onderwerpen en -abonnementen die zijn gedefinieerd in deze eenheid voor tijdschaal. Het bevat ook alle abonnementsgegevens. Tenzij [gepartitioneerde entiteiten messaging](service-bus-partitioning.md) is ingeschakeld, is een wachtrij of onderwerp toegewezen aan één SMS store. Abonnementen zijn opgeslagen in de dezelfde SMS store als hun bovenliggende onderwerp. Behalve voor de Service Bus [Premium Messaging](service-bus-premium-messaging.md), worden de SMS-berichten winkels geïmplementeerd boven aan de SQL Azure-databases.

## <a name="containers"></a>Containers

Elke SMS entiteit is een specifieke container toegewezen. Een container is een logische constructie waarin één messaging winkel alle relevante gegevens voor deze container. Elke container wordt toegewezen aan een SMS-berichten makelaar knooppunt. Er zijn meestal meer containers dan makelaar knooppunten messaging. Elk SMS makelaar knooppunt wordt daarom meerdere containers geladen. De verdeling van containers naar een SMS-berichten makelaar knooppunt waren georganiseerd zodanig dat alle berichten makelaar knooppunten gelijkmatig worden geladen. Als het selectievakje laden patroon wijzigen (bijvoorbeeld een van de containers haalt bezet) of als een SMS-berichten makelaar knooppunt tijdelijk niet beschikbaar is wordt, zijn de opnieuw van de containers verdeeld over de SMS-berichten makelaar knooppunten.

## <a name="processing-of-incoming-messaging-requests"></a>Verwerking van binnenkomende berichten aanvragen

Wanneer een client wordt een aanvraag naar de Service Bus verzonden, stuurt de Azure taakverdeling deze naar een van de gateway-knooppunten op te geven. Het knooppunt van de gateway is geautoriseerd het verzoek. Als het verzoek betrekking heeft op een SMS-berichten entiteit (wachtrij, onderwerp, abonnement), wordt het knooppunt van de gateway is de entiteit in de winkel gateway opgezocht en bepaalt in welke SMS store de entity zich bevindt. Hiermee wordt vervolgens gezocht naar welke SMS makelaar knooppunt deze container momenteel wordt en wordt de aanvraag verzonden naar dat SMS makelaar knooppunt. Het SMS-berichten makelaar knooppunt verwerkt de aanvraag en de entiteit staat in de container store wordt bijgewerkt. Het SMS-berichten makelaar knooppunt stuurt de reactie terug naar het knooppunt gateway stuurt een juiste reactie terug naar de client die de oorspronkelijke aanvraag uitgegeven.

![Verwerking van binnenkomende berichten aanvragen](./media/service-bus-architecture/IC690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Verwerking van binnenkomende relay aanvragen

Wanneer een client wordt een aanvraag naar de Service Bus verzonden, stuurt de Azure taakverdeling deze naar een van de gateway-knooppunten op te geven. Als de aanvraag een verzoek om te luisteren is, maakt het knooppunt van de gateway is een nieuwe relay. Als de aanvraag een verbinding verzoek om een specifieke relay is, doorgestuurd het knooppunt van de gateway is naar de gateway-knooppunt dat eigenaar is van de relay. De gateway-knooppunt dat eigenaar van de relay wordt een rendezvous-aanvraag verzonden naar de luisteren client, waarin wordt gevraagd de luisteraar ervan af om een tijdelijke kanaal naar het knooppunt gateway die het verzoek om verbinding ontvangen te maken.

Wanneer de relay-verbinding tot stand is gebracht, kunnen de clients berichten via het knooppunt gateway die wordt gebruikt voor de rendezvous uitwisselen.

![Verwerking van binnenkomende Relay aanvragen](./media/service-bus-architecture/IC690645.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt gelezen is een overzicht van de Service busarchitectuur, aan de slag gaat u naar de volgende koppelingen:

- [SMS-service Bus-overzicht](service-bus-messaging-overview.md)
- [Service Bus over grondbeginselen](service-bus-fundamentals-hybrid-solutions.md)
- [Een wachtrijtaken beheeroplossing Service Bus wachtrijen gebruiken](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)
