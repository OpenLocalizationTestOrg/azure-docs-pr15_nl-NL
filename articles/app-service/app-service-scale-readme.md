<properties
    pageTitle="Azure-App-Service: Schaalbaarheid App Service-toepassingen"
    description="Informatie over alles van toepassing schaling in App Service."
    keywords="App-service, azure app service, schaal scalable, app-abonnement, app servicekosten"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>Azure-App-Service: Schaalbaarheid App Service-toepassingen

Toepassingen die zijn ingesloten in een App-Azure-Service kunnen [bereiken enorme schaal](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Schaalbaarheid van een toepassing is echter een complexe probleem die geen een oplossing 'één maat voor iedereen'. Uw toepassing er zijn correct schaalfactor 3 belangrijke gebieden die worden bijdragen aan uw succes toepassingen:

1. Informatie over uw toepassingsarchitectuur en de weaknesses.
    * Is uw Stateful toepassing? Stateless?
    * Wat zijn de onderdelen van de toepassing?
        * Waar zijn de problemen in de toepassing?
    * Bij het laden wordt toegepast op de app, worden wat eerste afgebroken?
2. Informatie over de verwachte laden en prestaties vereisten.
    * Moet de toepassing worden dienen duizend gebruikers? of een miljoen?
    * Wordt met verkeer afkomstig zijn uit één geografische locatie of globaal?
    * Zijn er seizoensgebonden variaties? verkeer pieken?
    * Hoe snel moet de app reageren? 1 seconde? 1 milliseconden?
3. Informatie over en correct gebruikmaken van het hosten van uw app-platform.
    * Welke functies moet ik gebruikmaken van als u wilt bereiken van de doelen van mijn schaal?

In dit gedeelte kunt u meer informatie over de factoren en hulp bij het ontwerpen van een strategie die gebruikmaakt van de benodigde App Service-functies om uw doelstellingen schaalbaarheid te bereiken.

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]
