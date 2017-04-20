<properties 
    pageTitle="Implementatie van Azure Mobile betrokkenheid voor spellen-App"
    description="Spellen app scenario voor het implementeren van Azure Mobile betrokkenheid" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-gaming-app"></a>Mobiele betrokkenheid met spellen App implementeren

## <a name="overview"></a>Overzicht

Een opstart spellen heeft een nieuwe over vissen gebaseerd role play/strategie spel app gestart. Het spel is al actief 6 maanden. Dit spel is een enorme succes, en er miljoenen downloads en het bewaarbeleid is zeer hoog in vergelijking met andere opstart spel-apps. Tijdens de vergadering kwartaal controleren akkoord belanghebbenden dat ze nodig hebben om uit te breiden gemiddelde omzet per gebruiker (ARPU). Premium spel-pakketten zijn beschikbaar als speciale aanbiedingen. Deze wedstrijd packs kunnen gebruikers het uiterlijk en de prestaties van hun over vissen lijnen en stelen of tackles in het spel upgrade uit te voeren. Er zijn echter pakket verkoop erg laag. Zodat ze eerst besluit te analyseren van de gebruikerservaring met een hulpmiddel analytics en klikt u vervolgens voor het ontwikkelen van een afspraak programma om uit te breiden verkoop gebruiken segmentatie geavanceerde.

Gebaseerd op de [Azure Mobile betrokkenheid - handleiding aan de slag met aanbevolen procedures](mobile-engagement-getting-started-best-practices.md) ze maakt u een strategie betrokkenheid.

##<a name="objectives-and-kpis"></a>Doelstellingen en KPI 's

Belangrijke belanghebbenden voor het spel vergaderen. Alle afspreken één hoofddoel - premium-pakket verkoop verhogen met 15%. Ze maken deze doelstelling bedrijven Key Performance Indicators (KPI's) naar de maateenheid en station

* Klik op welk niveau van het spel zijn deze pakketten gekocht?
* Wat is de omzet per gebruiker, per sessie, per week en per maand?
* Wat zijn de typen favoriete aanschaffen?

Deel 1 van de [Handleiding aan de slag](mobile-engagement-getting-started-best-practices.md) wordt uitgelegd hoe u het definiëren van de doelstellingen en KPI's. 

Met de zakelijke KPI's nu gedefinieerd, maakt het Product Mobile Manager betrokkenheid KPI's om te bepalen van de nieuwe gebruiker trends en het bewaarbeleid.

* Bewaarbeleid bewaken en het gebruik in de volgende intervallen: dagelijks, elke twee dagen, wekelijks, maandelijks en om de drie maanden
* Actieve gebruiker telt
* De classificatie van de app in de winkel

Op basis van aanbevelingen uit het team IT, zijn de volgende technische KPI's toegevoegd aan de volgende vragen beantwoorden:

* Wat is mijn gebruikerspad (welke pagina is bezocht, hoeveel gebruikers tijd besteden aan deze)
* Aantal loopt of fouten opgetreden per sessie
* Welke versies OS worden uitgevoerd voor Mijn gebruikers?
* Wat is de gemiddelde grootte van het scherm voor Mijn gebruikers?
* Welk soort internetconnectiviteit heb mijn gebruikers?

Voor elke KPI de Manager van Mobile Product de gegevens die ze nodig heeft en waar deze zich bevindt in haar playbook geeft.

## <a name="engagement-program-and-integration"></a>Betrokkenheid programma en integratie

Voordat het bouwen van een geavanceerde betrokkenheid-programma, moet de Mobile-Project-directeur verantwoordelijk voor het project een uitgebreide begrip van hoe en wanneer producten zijn verbruikt door de gebruikers hebben.

Na drie maanden, heeft de Mobile-Project directeur voldoende gegevens beter zijn app push melding verkoop verzameld. Hij leert die:

* De eerste aankoop gebeurt in het algemeen op het niveau van 14. Voor 90% van die gevallen is de aankoop nieuwe legendarische wapens voor $3.
* In 80% van die gevallen koopt de gebruikers die een aankoop, gaat u verder met het product en breng meer hebt aangebracht.
* Gebruikers van wie u het niveau 20, hebt doorgegeven starten besteden aan meer dan $10/ week.
* Gebruikers vaak premium-pakketten op niveau 16, 24 en 32 kopen.

Met behulp van deze analyse wil de Mobile-Project directeur melding reeksen om uit te breiden in de app-verkoop voor specifieke push maken. Hij drie push-reeksen die hij aanroept maakt: Welkom programma, verkoop-programma en niet-actieve programma. Voor meer informatie raadpleegt u de [hulpmiddelen marketing en verkoop](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)
    ![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
