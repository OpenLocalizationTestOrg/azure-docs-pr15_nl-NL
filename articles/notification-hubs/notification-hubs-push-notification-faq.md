<properties
    pageTitle="Azure melding Hubs - Veelgestelde vragen"
    description="Veelgestelde vragen over het ontwerpen/implementeren van oplossingen op melding Hubs"
    services="notification-hubs"
    documentationCenter="mobile"
    authors="ysxu"
    manager="erikre"
    keywords="push-bericht, push-meldingen, iOS push-meldingen, android push-meldingen, ios push, android push"
    editor="" />

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu" />

#<a name="push-notifications-with-azure-notification-hubs---frequently-asked-questions"></a>Push-meldingen met Azure melding Hubs - Veelgestelde vragen

##<a name="general"></a>Algemene
###<a name="1---what-is-the-price-model-for-notification-hubs"></a>1. Wat is het prijsmodel voor melding Hubs?
Melding Hubs wordt aangeboden in drie lagen:

* **Gratis** - krijgen tot 1 miljoen gereedschap duwt per abonnement per maand.
* **Eenvoudige** - get 10 miljoen gereedschap duwt per abonnement per maand als een basislijn, met quotum groei opties.
* **Standaard** - krijgen 10 miljoen gereedschap duwt per abonnement per maand als een basislijn, vergroten met TARGET opties, plus uitgebreide telemetrielogboek capabilties op te geven.

De meest recente informatie vindt u op de pagina [Melding Hubs prijzen] . De prijzen tot stand is gebracht op het niveau van het abonnement en is gebaseerd op het nummer van de push notification opening zodat het maakt niet uit hoeveel naamruimten of melding hubs die u hebt gemaakt in uw Azure-abonnement.

Laag **gratis** wordt aangeboden voor ontwikkeling doeleinden met geen garantie SLA. Terwijl deze laag mogelijk een goed beginpunt die u wilt verkennen van de mogelijkheden van push-meldingen via Azure melding Hubs, het niet mogelijk de beste keuze voor middelgrote tot grote schaal-toepassingen.

**Eenvoudige** & **standaard** lagen worden aangeboden voor productie gebruik met de volgende belangrijke functies ingeschakeld *alleen voor de standaard trapsgewijs*:

- *RTF-telemetrielogboek* - melding Hubs aanbieding een aantal mogelijkheden voor het exporteren van uw telemetriegegevens, evenals push-meldingen registratiegegevens voor offline weergave en analyse.
- *Meerdere pachtadres* - ideaal als u een mobiele app met melding Hubs ter ondersteuning van meerdere tenants maakt. Hiermee kunt u Push-meldingen Services (PNS) referenties instellen op het niveau van de naamruimte melding Hub voor de app en klikt u vervolgens kunt u de tenants leveren ze afzonderlijke hubs onder deze algemene naamruimte scheiden. Hiermee worden onderhoud te vereenvoudigen, terwijl de toetsen SA's als u wilt verzenden en ontvangen van push-meldingen van de melding hubs behouden gescheiden voor elke tenant ervoor zorgen dat niet cross-tenant elkaar overlappen.
- *Gepland Push* - kunt u plannen push-meldingen, die vervolgens worden in de wachtrij geplaatst en verzonden.
- *Bulksgewijs importeren* -, kunt u registraties bulksgewijs importeren.

###<a name="2---what-is-the-notification-hubs-sla"></a>2. Wat is de melding Hubs SLA?
Voor **eenvoudige** en **standaard** melding Hubs lagen garanderen we dat ten minste 99,9% van de tijd, correct geconfigureerde toepassingen kunnen verzenden push-meldingen of bewerkingen registratie management met betrekking tot een melding Hub geïmplementeerd binnen een ondersteunde laag. Meer informatie over onze SLA, gaat u naar de pagina [Melding Hubs SLA] .

> [AZURE.NOTE] Er zijn geen garanties SLA voor de arm tussen de Platform Notification-Service en het apparaat aangezien melding Hubs, hangt af van externe platform mailproviders, zodat u kunt de push-melding geven bij het apparaat.

###<a name="3---which-customers-are-using-notification-hubs"></a>3. welke klanten gebruikt melding Hubs?
We hebben een groot aantal klanten die gebruikmaken van de melding Hubs met een paar aantal aanzienlijke hieronder:

* Sochi 2014 – 100s rente groepen, 3 + miljoen apparaten, 150 miljoen melding verzonden in 2 weken. [CaseStudy - Sochi]
* Skanska - [CaseStudy - Skanska]
* Seattle tijden - [CaseStudy - Seattle tijden]
* Mural.LY - [CaseStudy - Mural.ly]
* 7Digital - [CaseStudy - 7Digital]
* Bing Apps – 10s miljoenen apparaten, 3 miljoen meldingen/dag verzenden.

###<a name="4-how-do-i-upgrade-or-downgrade-my-notification-hubs-to-change-my-service-tier"></a>4. hoe ik upgraden of downgraden mijn Hubs melding als u wilt wijzigen van mijn servicelaag?
Ga naar de [Klassieke Azure-Portal], klik op Service-Bus en klik op de naamruimte vervolgens uw hub-melding. Klik op het tabblad schaal is mogelijk om te wijzigen van uw melding Hubs servicelaag.

![](./media/notification-hubs-faq/notification-hubs-classic-portal-scale.png)

##<a name="design--development"></a>Ontwerp en ontwikkeling
###<a name="1---which-server-side-platforms-do-you-support"></a>1. welke platforms serverzijde ondersteund?
Wij bieden SDK's en [volledige voorbeelden] voor .NET, Java, PHP, Python, Node.js zodat een backend app kan worden ingesteld om te communiceren met melding Hubs met een van deze platforms. Melding Hubs-API's zijn gebaseerd op de REST-interfaces, zodat u rechtstreeks contact opnemen met die in plaats daarvan als u niet wilt toevoegen van een extra afhankelijkheid kunt kiezen. Klik op de pagina [NH - REST API's] vindt u meer informatie.

###<a name="2---which-client-platforms-do-you-support"></a>2. welke clientplatforms ondersteund?
Wordt ondersteund verzendende push-meldingen voor [Apple iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Universele Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android China (via Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) & [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Apps voor Chrome](notification-hubs-chrome-push-notifications-get-started.md) en [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) platforms. Voor een volledige lijst met aan de slag te gaan zelfstudies aanpak verzendende push-meldingen op deze platforms, gaat u naar onze pagina [NH - aan de slag zelfstudies] .

###<a name="3---do-you-support-smsemailweb-notifications"></a>3. ondersteund meldingen voor e-SMS-website?
Melding Hubs is bedoeld voor meldingen verzenden naar mobiele-apps met behulp van de bovenstaande platforms. We nog bieden niet de mogelijkheid om te verzenden van e-mail of SMS-berichten; platforms van derden die deze mogelijkheden te bieden kunnen echter worden geïntegreerd samen met de melding Hubs systeemeigen push-meldingen verzenden met behulp van [Azure Mobile-Apps].

Melding Hubs beschikken niet over een browser push melding bezorging service out-van-het-box. Klanten kunt dit doet met SignalR boven aan de ondersteunde platforms voor servers. Als u zoeken wilt om meldingen te verzenden naar browser-apps in de sandbox Chrome, raadpleegt u de [zelfstudie Chrome Apps].

###<a name="4---what-is-the-relation-between-azure-mobile-apps-and-azure-notification-hubs-and-when-do-i-use-what"></a>4. Wat is de relatie tussen Azure Mobile-Apps en Azure melding Hubs en wanneer gebruik ik wat?
Als u een bestaande mobiele app backend hebt en u alleen toevoegen van de functie wilt voor het verzenden van push-meldingen kunt u Azure melding Hubs gebruiken. Als u wilt instellen van uw mobiele app backend helemaal moet vervolgens u overwegen Azure Mobile-Apps. Een Azure Mobile-App bepalingen automatisch een melding Hub voor u kunnen eenvoudig verzenden van push-meldingen van de mobiele app-end. De kosten die worden basis voor een melding Hub prijzen voor Azure Mobile-Apps bevat en u betaalt alleen wanneer u een stapje verder dan de opgenomen ladingen. Meer informatie over de kosten zijn beschikbaar op de pagina [Prijzen van App-Service] .

###<a name="5---how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>5. hoe veel apparaten kan ik ondersteund als ik verzend push-meldingen via melding Hubs?
Raadpleeg de pagina [Melding Hubs prijzen] voor meer informatie over het aantal ondersteunde apparaten.

Voor bepaalde scenario's, als u ondersteuning nodig voor apparaten met meer dan 10.000.000 geregistreerde hebt, neem rechtstreeks [contact met ons opnemen](https://azure.microsoft.com/overview/contact-us/) en we kunt u de schaal van de oplossing aanpassen.

###<a name="6---how-many-push-notifications-can-i-send-out"></a>6. hoeveel push-meldingen kunnen ik uitsturen?
Afhankelijk van de geselecteerde laag Azure automatisch aangepast omhoog op basis van het aantal meldingen die doorloopt in het systeem.

>[AZURE.NOTE] De algehele gebruikskosten kunt omhoog gaan op basis van het aantal push-meldingen worden verzonden. Zorg ervoor dat u zich bewust bent van bestaande laag limieten overzicht op de pagina [Melding Hubs prijzen] .

Onze bestaande klanten gebruiken melding Hubs miljoenen push-meldingen dagelijks verzenden. U beschikt niet over niets speciale effecten aan uw push meldingen hebt bereikt zo lang maken als u van Azure melding Hubs gebruikmaakt schaal te doen.

###<a name="7---how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>7. hoe lang duurt voor verzonden push-meldingen bereiken van mijn apparaat?
Azure melding Hubs is ten minste verwerken scenario waarbij de binnenkomende laden heel consistent is en niet is spikey van aard **1 miljoen push-bericht stuurt minuten** in een normale gebruiken. Dit tarief mogelijk naar gelang het aantal labels, aard van binnenkomende verzendt en andere externe factoren.

Tijdens de geschatte levertijd, de service kan berekenen van de doelen per platform- en doorsturen van berichten met de desbetreffende push melding bezorging services op basis van de expressies geregistreerde tags/tag. U kunt op is de verantwoordelijkheid van de services Pushmeldingen (PNS) aan wie de melding bij het apparaat.

Een PNS garandeert eventuele SLA voor het leveren van meldingen; echter meestal een meeste push-meldingen worden afgeleverd in apparaten binnen enkele minuten (meestal binnen de grenzen van 10 minuten) vanaf het moment dat deze zijn verzonden naar ons platform. Het is mogelijk dat er een paar uitschieters die langer kunnen duren.

>[AZURE.NOTE] Azure melding Hubs heeft een beleid om een push-meldingen die niet kunnen worden bezorgd bij de PNS in 30 minuten neerzetten. Deze vertraging kan optreden voor een aantal redenen, meest meestal omdat de PNS is uw toepassing beperken.

###<a name="8---is-there-any-latency-guarantee"></a>8. is er een garantie latentie?
Vanwege de aard van push-meldingen (ze worden geleverd door een externe, platform / regiospecifieke Push Notification-Service), moet u er geen garantie latentie is. Meestal de meeste push-meldingen verzonden binnen enkele minuten.

###<a name="9---what-are-the-considerations-i-need-to-take-into-account-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>9. Wat zijn de overwegingen die ik rekening te houden bij het ontwerpen van een oplossing met naamruimten en melding Hubs nodig?

####<a name="mobile-appenvironment"></a>Mobiele App-omgeving

* Moet er een melding Hub per mobiele app, per omgeving zijn.
* In een scenario voor meerdere tenant, moet elke tenant een afzonderlijke hub hebben.
* Als dit mogelijk de regel omlaag bij het verzenden van meldingen, moet u nooit dezelfde melding Hub tussen test en productieomgevingen delen. Apple biedt bijvoorbeeld Sandbox en productie Push eindpunten met elk afzonderlijk referenties met.
* U kunt al dan niet standaard, test meldingen verzenden naar uw geregistreerde apparaten via de Portal Azure of het Azure geïntegreerde onderdeel in Visual Studio. De drempelwaarde is ingesteld op 10-apparaten die zijn willekeurig geselecteerd uit de registratie-groep.

>[AZURE.NOTE] Als de hub is oorspronkelijk heeft geconfigureerd met een Apple sandbox-certificaat en klik opnieuw geconfigureerd als u wilt gebruiken, een Apple-productiecertificaat, zou de oude apparaat tokens ongeldig met het nieuwe certificaat en ertoe leiden dat gereedschap duwt mislukt. Is het raadzaam te scheiden van uw productie en testen van omgevingen met verschillende hubs gebruiken voor verschillende omgevingen.

####<a name="pns-credentials"></a>PNS referenties

Als een mobiele app is geregistreerd met van een platform developer portal (bijvoorbeeld Apple of Google enzovoort) vervolgens krijgt u een app-id en beveiliging tokens dat een backend app vereist is voor het geven van het Platform Push Notification services kunnen push-meldingen verzenden naar de apparaten. Deze beveiligingstokens die zich op de vorm van certificaten (bijvoorbeeld voor Apple iOS of Windows Phone) of beveiligingssleutels (Google Android, Windows enzovoort) kunnen moeten worden geconfigureerd in de melding Hubs. Dit meestal op het niveau van de hub melding is voltooid, maar u kunt ook op het niveau van de naamruimte in een scenario voor meerdere tenant worden uitgevoerd.

####<a name="namespaces"></a>Naamruimten

Naamruimten kan worden gebruikt voor het groeperen van de implementatie.  Dit kan ook worden gebruikt om aan te geven van alle melding Hubs voor alle tenants van de dezelfde app in de scenario voor meerdere tenant.

####<a name="geo-distribution"></a>Geografische-verdeling

Geografische-verdeling is niet altijd kritieke in push notification-scenario's. Het is moet worden opgenomen of verschillende Push Notification Services (bijvoorbeeld APNS, GCM enzovoort), die uiteindelijk geven de push-meldingen voor de apparaten, niet gelijkmatig, verdeeld.

Als u een toepassing waarmee u overal ter wereld hebt, kunt u verschillende hubs in verschillende naamruimten profiteren van de beschikbaarheid van melding Hubs-service in verschillende Azure regio's over de hele wereld.

>[AZURE.NOTE] Hierdoor neemt de beheerkosten - vooral rond registraties, zodat dit echt wordt niet aanbevolen en alleen moet worden uitgevoerd als er een expliciete nodig.

###<a name="10--should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>10. moet ik doen registraties van de app-end of rechtstreeks via client apparaten?
Registraties van de app-end zijn nuttig wanneer u doen clientverificatie moet voordat u de registratie maakt of wanneer u labels die moeten worden gemaakt of gewijzigd door de app-end op basis van bepaalde logica app hebt. Voor meer informatie vindt u meer op de [Backend registratie instructies] en [richtlijnen voor registratie van de Backend - 2] -pagina's.

###<a name="11--what-is-the-push-notification-delivery-security-model"></a>11. Wat is het beveiligingsmodel push melding bezorging?
Azure melding Hubs gebruiken een [Gedeeld Access handtekening (SA's)](../storage/storage-dotnet-shared-access-signature-part-1.md)-beveiligingsmodel gebaseerd. U kunt de tokens SA's op het hoofdniveau van de naamruimte of op het niveau van de melding Hubs gedetailleerde gebruiken. Deze tokens SA's kan worden ingesteld met verschillende autorisatieregels bijvoorbeeld de machtigingen verzenden van berichten, luister melding machtigingen, enzovoort. Meer details zijn beschikbaar in het document [NH beveiligingsmodel] .

###<a name="12--how-should-i-handle-sensitive-payload-in-the-push-notifications"></a>12. Hoe moet ik gevoelige nettolading in de push-meldingen verwerken?
Alle berichten zijn afgeleverd in apparaten door van het platform Push-meldingen Services (PNS). Wanneer u een afzender verzendt een melding naar Azure melding Hubs vervolgens we verwerken en de melding geven aan het desbetreffende PNS.

Alle verbindingen van de afzender naar de Azure meldingen Hubs en vervolgens naar de PNS HTTPS gebruiken.

>[AZURE.NOTE] Azure meldingen Hubs wordt de nettolading van het bericht niet geregistreerd op geen enkele manier.

Voor het verzenden van gevoelige nettoladingen maar het is raadzaam een patroon Secure Push te waar de afzender van een melding 'ping' met een bericht-id biedt bij het apparaat zonder de gevoelige nettolading en wanneer de app op het apparaat deze nettolading ontvangt, is het kunnen bellen van een beveiligde API rechtstreeks als u wilt ophalen van de berichtdetails van het. Een handleiding over het implementeren van het patroon dat hierboven is beschreven is beschikbaar op de pagina [NH - Secure Push zelfstudie] .

##<a name="operations"></a>Bewerkingen
###<a name="1---what-is-the-disaster-recovery-dr-story"></a>1. Wat is het verhaal noodgevallen herstel (DR)?
Wij bieden metagegevens herstel dekking aan onze uiteinde (de naam van de melding-Hub, verbindingsreeks en andere cruciale informatie). Wanneer een DR-scenario wordt geactiveerd, is de gegevens van registraties het **alleen segment** van de melding Hubs-infrastructuur die verloren gaan. U moet een oplossing als u wilt deze gegevens opnieuw in te vullen in uw nieuwe hub na herstel implementeren.

- *Stap 1* - een secundaire Hub voor melding maken in een ander datacenter. U kunt dit al doende maken op het moment van de gebeurtenis DR of u kunt maken vanuit het ophalen gaan. Niet dat u veel van een verschil welke optie u kiest, omdat een relatief snel proces in de volgorde van een paar seconden melding Hub inrichting is. Een vanaf het begin hebt, wordt u vanuit de DR-gebeurtenis die invloed hebben op uw beheermogelijkheden, zodat u de optie ten zeerste aanbevolen beschermen.

- *Stap 2* - hydraat de secundaire Hub melding met de registraties van de primaire melding Hub. Het wordt niet aanbevolen om te proberen te onderhouden van registraties op beide hubs en probeert om ze te houden synchroon al doende zoals registraties binnenkort in - meestal die niets heeft opgeleverd ook vanwege inherent feit van registraties verloopt aan de kant PNS. Melding Hubs opschonen ze we PNS feedback over verlopen of ongeldige registraties ontvangen.  

Aanbeveling is te gebruiken van een app-backend waarin:

- Een bepaalde set van registraties aan het einde houdt, zodat dit kan een bulksgewijs invoegen in de hub secundaire melding voor het geval DR

**OF-BEWERKING**

- Haalt een gewone dump van registraties van de primaire hub als een back-up en bevat een groot aantal plaats in de secundaire NH.

>[AZURE.NOTE] Registraties exporteren/importeren functionaliteit beschikbaar in Standard laag wordt beschreven in het document [Registraties exporteren/importeren] .

Als u geen een back-end, klikt u vervolgens wanneer de app wordt gestart op een van de doelapparaten, deze wordt een nieuwe registratie uitvoeren in de secundaire melding-Hub, en uiteindelijk de secundaire melding Hub alle actieve hulpmiddelen die zijn geregistreerd.

Het nadeel is dat er wordt een periode wanneer apparaten waar de apps van dit nog niet hebt geopend geen meldingen ontvangt.

###<a name="2---is-there-any-audit-log-capability"></a>2. is er mogelijk log controle?
Alle bewerkingen van de melding Hubs Management gaat u naar de logboeken aan de bewerking die beschikbaar worden gemaakt in de [Klassieke Azure-Portal].

##<a name="monitoring--troubleshooting"></a>Cmdlets voor controle en probleemoplossing
###<a name="1---what-troubleshooting-capabilities-are-available"></a>1. welke voor probleemoplossing-mogelijkheden zijn beschikbaar?
Azure melding Hubs bieden verschillende functies om uit te voeren algemene problemen oplossen, met name in de meeste gevallen rond decoratieve meldingen. Zie voor meer informatie onze [NH - probleemoplossing] whitepaper.

###<a name="2---what-telemetry-features-are-available"></a>2. welke telemetrielogboek-functies zijn beschikbaar?
Azure melding Hubs kunt telemetriegegevens weergeven in de [Klassieke Azure-Portal]. Details van de doelstellingen van de beschikbare zijn beschikbaar op de pagina [NH - aan de doelstellingen] .

>[AZURE.NOTE] Succesvolle meldingen alleen betekent dat de push-meldingen hebt is bezorgd bij de externe Push Notification-Service (bijvoorbeeld APNS voor Apple, GCM voor Google enzovoort). Het is snel aan de PNS moet de melding geven voor apparaten. De PNS worden gewoonlijk niet bezorging Maatstelsel van derden.  

Wij bieden ook de functie voor het exporteren van de telemetriegegevens via programmacode (in **Standard** laag). Zie de [NH - voorbeeld van de doelstellingen] voor meer informatie.

[Azure klassieke Portal]: https://manage.windowsazure.com
[Melding Hubs prijzen]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Melding Hubs SLA]: http://azure.microsoft.com/support/legal/sla/
[CaseStudy - Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[CaseStudy - Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[CaseStudy - Seattle tijden]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[CaseStudy - Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[CaseStudy - 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH - REST API's]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH - zelfstudies aan de slag]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Chrome Apps zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Back-end registratie richtlijnen]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Richtlijnen voor registratie van de backend - 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[NH beveiligingsmodel]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[NH - Secure Push zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[NH - problemen oplossen]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[NH - aan de doelstellingen]: https://msdn.microsoft.com/library/dn458822.aspx
[NH - voorbeeld van de doelstellingen]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Registraties exporteren/importeren]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[volledige voorbeelden]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobiele Apps]: https://azure.microsoft.com/en-us/services/app-service/mobile/
[App-Service prijzen]: https://azure.microsoft.com/en-us/pricing/details/app-service/
