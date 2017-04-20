<properties 
    pageTitle="Implementatie van Azure Mobile betrokkenheid voor Media-App"
    description="Media-app scenario voor het implementeren van Azure Mobile betrokkenheid" 
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

#<a name="implement-mobile-engagement-with-media-app"></a>Mobiele betrokkenheid met Media-App implementeren

## <a name="overview"></a>Overzicht

John is een mobiele projectmanager voor een groot media-bedrijf. Hij onlangs een nieuwe app waarop een telling van zeer hoog downloaden dat is gestart. Hij zijn doelstellingen voor downloaden heeft limiet, maar nog steeds zijn terug op Investment(ROI) per gebruiker niet voldoet zijn vereisten. 

John geconstateerd al waarom zijn ROI te laag is. Gebruikers vaak stoppen met zijn app na alleen 2 weken en deze meestal nooit keert u terug. Hij wil het behoud van de zijn app vergroten.

Nadat enkele aanvankelijke testen, hij heeft geleerd wanneer hij zijn gebruikers met push-meldingen, voert u ze meestal de om door te gaan met zijn app. Gebruikers die inactief zijn wordt ook vaak terug naar de app afhankelijk van meldingen die hij deze verzendt. Jan beslist te investeren in een soort Engagement als standaardprogramma voor zijn app dat wordt gebruikgemaakt van geavanceerde hebt samengesteld en push-meldingen.

John heeft het [Azure Mobile betrokkenheid - handleiding aan de slag met aanbevolen procedures](mobile-engagement-getting-started-best-practices.md) onlangs lezen en heeft besloten willen implementeren van de aanbevelingen van de handleiding.

##<a name="objectives-and-kpis"></a>Doelstellingen en KPI 's

Belangrijke belanghebbenden voor voornaamwoorden app vergaderen. Zakelijke is gegenereerd op basis van advertenties als gebruikers zijn media gebruiken. Door te verhogen inhoud verbruikt per gebruiker, verhoogt John zijn inkomsten. Alle één hoofddoel afspreken: verkoop van advertenties verhogen met 25%. Ze maken deze doelstelling bedrijven Key Performance Indicators (KPI's) naar de maateenheid en station

* Aantal advertenties geklikt per gebruiker
* Hoeveel artikelpagina's bezocht (per gebruiker / per sessie / per week / per maand...)
* Wat zijn favoriete categorieën

Op basis van de vergadering voornaamwoorden met belangrijke belanghebbenden heeft hij zijn bedrijven KPI's gedefinieerd. Volgt hij deel 1 van de [Azure Mobile betrokkenheid - handleiding aan de slag met aanbevolen procedures](mobile-engagement-getting-started-best-practices.md). 

Hij maakt vervolgens de volgende betrokkenheid KPI's om ervoor te zorgen dat de doelstellingen worden bereikt:

* Bewaarbeleid controleren in de volgende intervallen: dagelijks, wekelijks, bi-wekelijks en maandelijkse.
* Actieve gebruikers telt
* De classificatie van de app in de app worden opgeslagen

Op basis van aanbevelingen uit het team IT, zijn de volgende technische KPI's toegevoegd aan de volgende vragen beantwoorden:

* Wat is mijn gebruikerspad (welke pagina is bezocht, hoeveel tijd gebruikers besteden aan deze)
* Het aantal loopt of fouten opgetreden per sessie?
* Welke versies OS worden uitgevoerd voor Mijn gebruikers?
* Wat is de gemiddelde grootte van het scherm voor Mijn gebruikers?
* Welk soort internetverbindingen heb mijn gebruikers?

Voor elke KPI hij de benodigde gegevens worden geclassificeerd en hij deze records in de juiste locatie van haar playbook.

## <a name="engagement-program-and-integration"></a>Betrokkenheid programma en integratie

Nu die John is voltooid zijn KPI's definiëren, wordt hij zijn betrokkenheid strategie fase gestart door 4 betrokkenheid-programma's en hun doelstellingen te definiëren:    ![][1]

Vervolgens gaat John grondigere door push-meldingen voor elk programma waarin. Push-bericht zijn gedefinieerd in vijf elementen:

1. Doelstelling: Wat is het doel van de melding
2. Hoe het doel wordt bereikt
3. Doel: wie de melding ontvangt?
4. Content: Wat is de tekst en de opmaak van de melding (In App/Out van App)
5. Wanneer: Wat is het beste moment deze push-bericht verzenden

    ![][2]

Voor meer informatie raadpleegt u de [hulpmiddelen marketing en verkoop](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Op basis van het deel 2 van de whitepaper John doelsectie gebruikt om te bepalen welke hij heeft voor het verzamelen van gegevens en schrijft zijn Tag Plan samen met de IT-team toe te passen van de oplossing. John na 1 week implementatie en testen van de gebruiker-acceptatie kunt ten slotte zijn programma's starten.

##<a name="program-results"></a>Programma resultaten

4 maanden later, reviseert John prestaties van programma's. Het programma dat Welkom en het wekelijkse programma zijn doelstellingen zijn. Het aantal gebruikers met slechts één sessie afneemt, meer functies van de app worden gebruikt en het aantal verbindingen per week is verdubbeld.

De **Niet-actieve programma** helpt John gebruiker instinct begrijpen. Ziet u dat 15% van de niet-actieve gebruikers keert u terug naar de app. Echter deze meestal blijven niet actief meer dan 1 maand. John voorziet in een mogelijke optimalisatie van deze reeks met extra meldingen en zijn inhoud keuzen uit te vouwen.

Het **Programma Discover** werkt niet goed. Neemt cross verkopen maar niet voldoende zijn doelstellingen bereiken. John aangeeft dat hij geen voldoende gegevens om relevante hebt samengesteld en de juiste inhoud voorstellen. Hij dit programma stopt en bevat informatie over het verzenden van "redactionele push-meldingen" met Azure Mobile betrokkenheid. Zijn journalisten al een CMS-oplossing voor het verzenden van push-meldingen en ze niet wilt wijzigen.

Jan beslist gebruik van de API hebt bereikt, dat wil een HTTP REST API waarmee bereik campagnes beheren zeggen zonder gebruik te maken van AZME Web-interface. Met deze methode kunt John de gegevens die hij moet en zijn schrijvers over het gebruik van de CMS-oplossing behouden toestaan verzamelen.

Om ervoor te zorgen dat functie goed werkt, vraagt John IT-team moeten waakzaam op de volgende punten:

1. **Besturingssystemen** : ze alle hun eigen regels voor het beheren van push-meldingen, zodat John wordt bepaald voor een overzicht van alle gevallen en Hiermee wordt gecontroleerd als de API's verwerkt hebben.
Bijvoorbeeld: Android push-systeem kan totaalbeeld wat niet het geval met iOS.

2. **Tijdsbestek**: John wil een API, waarmee de periode instellen en een einde aan campagnes instellen. Hij wil gebruikers uit een storend melding bombing behouden.

3. **Categorieën**: Marketing-team bereidt sjabloon voor elk type melding. John vraagt IT-afdeling om in te stellen van de categorieën in de API.

John is na een bepaalde tests tevreden. Met behulp van deze API, kunnen journalisten nog steeds verzenden push-meldingen met hun CMS en Azure Mobile betrokkenheid verzamelt alle gegevens worden doorgevoerd voor hen

Na deze 4 eerst maanden, resultaten een goede algehele prestaties en hebt betrouwbaarheidsinterval voor weerspiegelen John en zijn bord, ROI per gebruiker toeneemt per 15% en mobiele verkoop 17,5% van de totale verkoop, een stijging van 7.5% in slechts vier maanden weergeven.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
