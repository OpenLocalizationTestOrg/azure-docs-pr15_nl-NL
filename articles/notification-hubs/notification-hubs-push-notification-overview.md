<properties
    pageTitle="Azure melding Hubs"
    description="Informatie over het gebruik van push-meldingen in Azure wordt aangegeven. Voorbeelden van de code is geschreven in C# de .NET-API gebruiken."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Azure melding Hubs

##<a name="overview"></a>Overzicht

Azure melding Hubs beschikt over een eenvoudig te gebruiken, meerdere platforms, schaal-out push-infrastructuur die kunt u mobiele push-meldingen verzenden vanuit een back-end (in de cloud of on-premises implementatie) op een mobiel platform.

Met de melding Hubs, kunt u eenvoudig verzenden meerdere platforms, persoonlijke push-meldingen, samenvatten van de details van de verschillende platform melding systemen (PNS). U kunt afzonderlijke gebruikers of gehele publiek segmenten miljoenen gebruikers, op alle hun apparaten met afstemmen met een enkele API-oproep.

U kunt de melding Hubs gebruiken voor zowel enterprise en op consumenten-scenario's. Bijvoorbeeld:

- Recente nieuws meldingen verzenden naar miljoenen met lage latentie (melding Hubs bevoegdheden Bing-toepassingen vooraf geïnstalleerd op alle apparaten van Windows en Windows Phone).
- Locatie gebaseerde coupons verzenden naar Gebruikersegmenten.
- Gebeurtenismeldingen verzenden aan gebruikers of groepen voor sporten/Financiën/spellen-toepassingen.
- Gebruikers van enterprise gebeurtenissen zoals nieuwe berichten/e-mailberichten en verkoopleads melden.
- Verzend een / permanente-wachtwoorden voor meervoudige verificatie vereist.



##<a name="what-are-push-notifications"></a>Wat zijn Pushmeldingen?

Smartphones en tablets kunnen "" moet gebruikers laten weten wanneer een gebeurtenis heeft plaatsgevonden. Deze meldingen kunnen veel formulieren duren.

In Windows Store- en Windows Phone-toepassingen, de melding in de vorm van een _mailpop_kan zijn: een modaal venster wordt weergegeven, met een geluid, om aan te geven van een melding voor nieuwe. Andere meldingstypen die worden ondersteund opnemen _tegel_, _onbewerkte_en _badge_ meldingen. Zie voor meer informatie over de soorten berichten die worden ondersteund op apparaten met Windows, [tegels, Badges, en meldingen](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

Klik op apparaten met Apple iOS krijgt de push op dezelfde manier de gebruiker met een dialoogvenster aanvragen van de gebruiker om te bekijken of sluit u de melding. De toepassing die u het bericht ontvangt te klikken op **Beeld** worden geopend. Zie voor meer informatie over iOS meldingen, [iOS meldingen](http://go.microsoft.com/fwlink/?LinkId=615245).

Push-meldingen helpen mobiele apparaten vers informatie tijdens het resterende energie-efficiënte weergeven. Meldingen kunnen worden verzonden door de backend-systemen naar mobiele apparaten, zelfs wanneer de bijbehorende apps op een apparaat niet actief zijn. Push-meldingen zijn essentieel voor consumenten-apps, waar ze worden gebruikt om uit te breiden betrokkenheid van de app en het gebruik. Meldingen zijn ook handig voor ondernemingen, wanneer actuele informatie werknemer serverreactie op gebeurtenissen die bedrijven toeneemt.

Er zijn enkele specifieke voorbeelden van mobiele betrokkenheid scenario's:

1.  Een tegel in Windows 8 of Windows Phone wordt bijgewerkt met de huidige financiële gegevens.
2.  Een gebruiker met een mailpop dat sommige werkitem is toegewezen aan deze gebruiker, in een werkstroom gebaseerde enterprise-app waarschuwingen.
3.  Weergeven van een badge met het nummer van huidige verkoop helpt in een app CRM (zoals Microsoft Dynamics CRM).

##<a name="how-push-notifications-work"></a>Hoe Push-meldingen werk

Push-meldingen worden aangeboden via platform / regiospecifieke infrastructuur _Platform melding systemen_ (PNS) genoemd. Een PNS biedt barebones-functies (dat wil zeggen, geen ondersteuning voor uitzending, persoonlijke instellingen) en er geen algemene interface. Bijvoorbeeld een bericht verzenden naar een Windows Store-app, een ontwikkelaar moet contact opnemen met de WNS (Windows Notification-Service). Een bericht verzenden naar een iOS-apparaat, worden de dezelfde ontwikkelaar heeft tot contact opnemen met APNS (Apple Push Notification-Service), en verzend het bericht een tweede maal. Azure melding hubs u helpen met een algemene interface, samen met andere functies ter ondersteuning van push-meldingen over elke platform.

Op hoog niveau, echter alle platform melding systemen hetzelfde patroon volgen:

1.  De client-app contact op met de PNS om op te halen de _verwerken_. Het type greep, is afhankelijk van het systeem. Voor WNS, is dit een URI of "melding kanaal." Voor APNS is dit een token.
2.  De client-app worden opgeslagen in dit greep in het app- _back-enddatabase_ voor later gebruik. Voor WNS is het back-enddatabasebestand meestal een cloudservice. Apple heet het systeem een _provider_.
3.  Verzend een push-bericht door contact het app-back-enddatabasebestand op met de PNS met de greep afstemmen van de app-exemplaar van een specifieke client.
4.  De PNS Hiermee stuurt u de melding bij het apparaat dat is opgegeven door de greep.

![][0]

##<a name="the-challenges-of-push-notifications"></a>De uitdagingen van Push-meldingen

Hoewel deze systemen zeer krachtig zijn, laat ze nog steeds hoeveel werk op de app-ontwikkelaar om te kunnen zelfs veelvoorkomende push melding scenario's, zoals uitzenden of push-meldingen verzenden naar gesegmenteerde gebruikers implementeren.

Push-meldingen zijn een van de meest gevraagde functies in de cloudservices voor mobiele apps. De reden hiervoor is dat de infrastructuur vereist zodat u ze werken vrij gecompliceerde en voornamelijk gerelateerd aan de belangrijkste bedrijfslogica van de app wordt uitgevoerd. Enkele van de struikelblokken bij het samenstellen van een op aanvraag push-infrastructuur zijn:

- **Afhankelijkheid platform.** Om te kunnen verzenden meldingen naar apparaten op andere platforms, moeten meerdere interfaces worden gecodeerd in de back-enddatabase. Niet alleen de basisgegevens verschillen, maar de presentatie van de melding (tegel, mailpop of badge) is ook platform taalafhankelijk. Deze verschillen kunnen leiden tot complexe en moeilijk te onderhouden back-enddatabase code.

- **Schaal.** Schaalbaarheid van deze infrastructuur heeft twee aspecten:
    + Volgens de richtlijnen van PNS worden apparaat tokens vernieuwd telkens wanneer de app wordt gestart. Dit leidt tot een grote hoeveelheid verkeer (en na een database toegang) alleen op het apparaat tokens up-to-date te houden. Wanneer het aantal apparaten (mogelijk naar miljoenen), is de kosten van het maken en onderhouden van deze infrastructuur nonnegligible.

    + De meeste PNSs ondersteunen geen uitzending op meerdere apparaten. Betekent dit dat er een broadcast miljoenen apparaten in miljoenen oproepen naar de PNSs resulteert. Kunnen schalen dat deze aanvragen is zeer lastig, omdat dit meestal app ontwikkelaars wilt bewaren van de totale latentie omlaag. Bijvoorbeeld het laatste apparaat het bericht niet bericht moet krijgen de 30 minuten nadat de meldingen die is verzonden, als voor veel gevallen zou dit het doel hebben push-meldingen verslaan.
- **Routering.** PNSs om er zelf een toe aan een bericht verzenden naar een apparaat. Meldingen worden echter in de meeste apps gericht op gebruikers en/of rente groepen (bijvoorbeeld alle werknemers die zijn toegewezen aan een bepaalde klantaccount). Als zodanig om te kunnen de meldingen doorsturen naar de juiste hulpmiddelen, het app-back-enddatabasebestand moet voor het behoud van een register waarin rente groepen gekoppeld aan apparaat tokens. Deze boven toevoegen aan de totale tijd voor market en onderhoud kosten van een app

##<a name="why-use-notification-hubs"></a>Waarom melding Hubs gebruiken?

Melding Hubs complexiteit verwijderen: u geen voor het beheren van de uitdagingen van push-meldingen hebt. In plaats daarvan kunt u een melding-Hub. Melding Hubs gebruikmaken van de infrastructuur van een volledige meerdere platforms, schaal uit push-melding en de push-specifieke-code die wordt uitgevoerd in de app-end aanzienlijk te verkorten. Melding Hubs Implementeer de functionaliteit van een push-infrastructuur. Apparaten zijn alleen die verantwoordelijk is voor het registreren van PNS grepen en de backend is verantwoordelijk voor platform-onafhankelijke berichten verzenden naar gebruikers of groepen rente, zoals wordt weergegeven in de volgende afbeelding:

![][1]


Melding hubs bieden een kant-en-klare push notification-infrastructuur met de volgende voordelen:

- **Meerdere platforms.**
    +  Ondersteuning voor alle primaire mobiele platforms. Melding hubs kunnen push-meldingen verzenden naar de Windows Store, iOS-, Android en Windows Phone-apps.

    +  Melding hubs biedt een algemene interface om meldingen te verzenden naar alle ondersteunde platforms. Platform / regiospecifieke protocollen zijn niet vereist. Het app-back-enddatabasebestand te meldingen in platform / regiospecifieke of platform-onafhankelijke indelingen verzenden. De toepassing wordt alleen communiceert met melding Hubs.

    +  Greep van Apparaatbeheer. Melding Hubs onderhoudt de greep register en feedback van PNSs.

- **Identiteitsprogramma werkt met een back-enddatabase**: Cloud of on-premises .NET, PHP, Java, knooppunt, enzovoort.

- **Schaal.** Melding hubs aanpassing aan miljoenen apparaten zonder de nodig opnieuw bouwen of shard.


- **Uitgebreide set bezorging patronen**:

    - *Uitzending*: kan voor dicht bij de tegelijk uitzending miljoenen apparaten met een enkele API-oproep.

    - *Unicast/Multicast*: Push aan tags dat staat voor afzonderlijke gebruikers, waaronder al hun apparaten; of een grotere groep; bijvoorbeeld afzonderlijk formulier factoren (tablet versus telefoon).

    - *Segmentatie*: Push naar complexe segment gedefinieerd door de tag expressies (bijvoorbeeld apparaten in New York volgen de Yankees).

    Elk apparaat, bij het verzenden van de greep op een melding-hub, kan een of meer _tags_kunt opgeven. Voor meer informatie over [tags]. Labels hebben geen moeten worden vooraf deze is ingericht of verwijderd. Tags bieden een eenvoudige manier om meldingen te verzenden naar gebruikers of groepen rente. Omdat tags een app-specifieke-id (zoals gebruiker of groep-id's) bevatten kunnen, maakt het gebruik de app-back-enddatabase uit hoeven opslaan en beheren om het apparaat.

- **Persoonlijke instellingen**: elk apparaat kan een of meer sjablonen, om te bereiken van lokalisatie per apparaat en persoonlijke instellingen handhaven back-enddatabase code hebben.

- **Beveiliging**: gedeeld Access geheim (SA's) of federatieve verificatie.

- **RTF-telemetrielogboek**: beschikbaar in de portal en via programmacode.


##<a name="integration-with-app-service-mobile-apps"></a>Integratie met mobiele Apps van App-Service

[Mobile-Apps voor App-Service] heeft te vergemakkelijken een naadloos en uniforme ervaring op Azure services, ingebouwde ondersteuning voor push-meldingen met melding Hubs. [Mobile-Apps voor App-Service] biedt een zeer scalable, algemeen beschikbaar mobiele toepassing development-platform voor softwareontwikkelaars en systeemintegrators die bij een groot aantal mogelijkheden voor mobiele ontwikkelaars brengt.

Mobile-Apps ontwikkelaars kunnen melding Hubs gebruiken met de werkstroom voor het volgende:

1. Apparaat PNS greep ophalen
2. Apparaat en [sjablonen] met melding Hubs via handige Mobile-Apps Client SDK register API registreren
    + Houd er rekening mee dat Mobile-Apps afwezig alle tags op registraties om veiligheidsredenen oprollen. Werken met melding Hubs van uw end rechtstreeks naar het koppelen van labels aan apparaten.
3. Meldingen van uw app-end met melding Hubs verzenden

Hier volgen enkele voordelen ontwikkelaars integratie heeft aangebracht:

- **Mobiele Apps Client SDK's.** Deze meerdere platforms SDK's bieden eenvoudige API's voor registratie en neem contact op met de melding hub automatisch met de mobiele app gekoppeld. Ontwikkelaars hoeft niet te graven tot en met melding Hubs referenties en werken met extra service.
    + De SDK's labelen automatisch de opgegeven apparaat instellen met de Mobile-Apps geverifieerde gebruikers-ID om in te schakelen push gebruiker scenario.
    + De SDK's automatisch de Mobile-Apps installatie-ID als GUID gebruiken om u te registreren bij melding Hubs, opslaan van de problemen van meerdere service-GUID's onderhouden voor ontwikkelaars.
    
- **Installatie-model.** Mobile-Apps werkt met de melding Hubs nieuwste push-model om aan te geven van alle push-eigenschappen die is gekoppeld aan een apparaat in de installatie van een JSON die wordt uitgelijnd met de Push Notification-Services en het is eenvoudig te gebruiken.

- **Flexibiliteit.** Ontwikkelaars kunt altijd voor gebruik met melding Hubs rechtstreeks ook niet met de integratie op hun plaats staan.

- **Geïntegreerde ervaring in [Azure-portal].** Push terwijl een mogelijkheid visueel wordt weergegeven in de Mobile-Apps en ontwikkelaars eenvoudig met de bijbehorende melding hub tot en met de Mobile-Apps werken kunnen.



##<a name="next-steps"></a>Volgende stappen

U vindt meer informatie over melding Hubs in deze onderwerpen:

+ **[Hoe klanten melding Hubs gebruiken]**

+ **[Melding Hubs zelfstudies en hulplijnen]**

+ **Zelfstudies melding Hubs aan de slag** ([iOS], [Android], [Universal van Windows], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

De relevante .NET managed API-referenties voor push-meldingen zich hier bevinden:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Hoe klanten melding Hubs gebruiken]: http://azure.microsoft.com/services/notification-hubs
  [Melding Hubs zelfstudies en hulplijnen]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android-apparaat]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Windows Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [Mobiele Apps van App-Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [sjablonen]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure-portal]: https://portal.azure.com
  [labels]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
