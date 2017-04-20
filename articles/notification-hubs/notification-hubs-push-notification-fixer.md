<properties 
    pageTitle="Azure melding Hubs - diagnose richtlijnen" 
    description="Richtlijnen voor het vaststellen van veelvoorkomende problemen met Azure melding Hubs." 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure melding Hubs - diagnose richtlijnen

##<a name="overview"></a>Overzicht

Een van de meest voorkomende vragen we van Azure melding Hubs klanten horen is hoe om te bepalen waarom zien een melding verzonden van hun toepassing-end weergegeven op het clientapparaat - waar en waarom meldingen zijn verwijderd en hoe u lost dit probleem. In dit artikel wordt we leest u over de verschillende redenen waarom meldingen mogelijk krijgen verbroken of naam niet eindigt omhoog op de apparaten. We ziet er ook uit door op welke manieren u kunt analyseren en de onderliggende oorzaak duidelijk. 

Ten eerste is het belangrijk om te begrijpen hoe Azure melding Hubs meldingen verplaatst naar de apparaten.
![][0]

In een normale verzenden melding stroom, wordt het bericht van de **toepassing backend** naar **Azure melding Hub (NH)** die op zijn beurt sommige verwerking op alle registraties rekening houdend met de geconfigureerde labels en tag expressies om te bepalen "doelen" dat wil zeggen alle registraties die moeten de push-meldingen ontvangen. Deze registraties kunnen bestrijkt een of meer van de ondersteunde platforms - iOS, Google, Windows, Windows Phone, Kindle en Baidu voor Android China. Als de doelen tot stand worden gebracht, NH vervolgens gereedschap duwt meldingen, splitsen in meerdere batch van registraties, naar het apparaatplatform specifieke **Push-meldingen Service (PNS)** - bijvoorbeeld APNS voor Apple, GCM voor Google enzovoort. NH verifieert met de desbetreffende PNS op basis van de referenties die u in de klassieke Azure-Portal op de pagina configureren melding Hub instellen. De PNS stuurt de meldingen aan de desbetreffende **clientapparaten**. Dit is het aanbevolen manier op te leveren push-meldingen en houd er rekening mee dat de laatste fase van een bericht van levering tussen het platform PNS en het apparaat plaatsvindt platform. Daarom zijn er vier belangrijke onderdelen - *client*, *toepassing backend*, *Azure melding Hubs (NH)* en *Push-meldingen Services (PNS)* en een van deze meldingen steeds verbroken veroorzaken. Meer informatie over deze architectuur is beschikbaar op de [Melding Hubs overzicht].

Fout bij het geven van meldingen kan optreden tijdens de eerste test/tijdelijke fase waarin mogelijk een configuratieprobleem aangeven of dit kan optreden in productie waar alle of enkele van de meldingen die mogelijk worden aan decoratieve waarin wordt aangegeven dat sommige grondigere toepassing of patroon probleem messaging. Klik in de sectie gaan onder we verschillende scenario's voor decoratieve meldingen die variëren van algemene voor het sommige type, dat enkele van de plaats waar u voor de hand liggende vinden kan en sommige anderen niet zo veel. 

##<a name="azure-notifications-hub-mis-configuration"></a>Azure meldingen Hub foutieve configuratie 

Azure melding Hubs nodig heeft om zichzelf te verifiëren in de context van de ontwikkelaar van toepassing moeten kunnen meldingen kunnen verzenden naar de desbetreffende PNS. Dit is het mogelijk gemaakt door de ontwikkelaar een ontwikkelaarsaccount maken met het desbetreffende platform (Google, Apple, Windows enzovoort) en klik vervolgens registreren hun waar worden verkregen referenties die moeten worden geconfigureerd in de portal onder melding Hubs bij configuratie van toepassing. Als er geen meldingen via maakt, moet eerste stap om ervoor te zorgen dat de juiste referenties zijn geconfigureerd in de melding Hub ze overeenkomen met de toepassing gemaakt onder hun platform bepaalde ontwikkelaar-account. U vindt u onze [Aan de slag zelfstudies] nuttige informatie voor dit proces eens op een wijze stap voor stap. Hier volgen enkele veelvoorkomende foutieve configuraties:

1. **Algemene**
 
    een) Zorg ervoor dat de naam van de melding hub (zonder typefouten) hetzelfde is:

    - Waar u de client registreert 
    - Waar u meldingen van de end, verstuurt  
    - Waar u de referenties PNS hebt geconfigureerd en 
    - Wie SA's referenties die u op de client en de backend hebt ingesteld. 
        
    b) Zorg ervoor dat u de juiste SA's configuratie tekenreeksen zijn gebruikt op de client als de backend toepassing. Als een vuistregel, moet u de **DefaultListenSharedAccessSignature** op de client en **DefaultFullSharedAccessSignature** op de toepassing backend (waardoor gemachtigd te kunnen verzenden melding naar de NH)

2. **Configuratie van de Apple Push-meldingen Service (APNS)**
 
    U moet u twee verschillende hubs - één voor productie wilt behouden en andere voor het testen van doel. Dit betekent dat het certificaat dat u wilt gebruiken in sandbox-omgeving op een aparte hub en het certificaat dat u wilt gebruiken in productie op een aparte hub uploaden. Probeer geen verschillende soorten certificaten uploaden naar dezelfde hub zoals dit leiden de melding-fouten de regel omlaag tot kan. Als u zelf in een positie waar u per ongeluk verschillende soorten certificaat hebt geüpload naar dezelfde hub vinden, is het raadzaam de hub verwijderen en opnieuw beginnen. Als om een bepaalde reden u niet kunt verwijderen van de hub vervolgens tenminste, moet u alle bestaande registraties verwijderen van de hub. 

3. **Configuratie van Google Cloud Messaging (GCM)** 

    een) Zorg ervoor dat u 'Google Cloud Messaging voor Android' onder uw project cloud inschakelen wilt. 
    
    ![][2]
    
    b) Zorg ervoor dat u een sleutel"Server" maakt terwijl het verkrijgen van de referenties voor welke NH wordt gebruikt om te verifiëren met GCM. 
    
    ![][3]
    
    c) Zorg ervoor dat u "Project-ID" hebt geconfigureerd op de client die een geheel numerieke entiteit die u vanuit het dashboard krijgen kunt is:
    
    ![][1]

##<a name="application-issues"></a>Problemen met toepassingen

1) **Labels / markeren van expressies**

Als u labels of tag expressies segmenten van het publiek gebruikt, is het altijd mogelijk dat wanneer u de melding verzendt, er is geen doel op basis van de labels/tag expressies die u in uw gesprek verzenden opgeeft kan worden gevonden. Het is raadzaam te controleren van uw registraties om ervoor te zorgen dat er labels die bij het verzenden van melding en controleer vervolgens of het ontvangstbewijs alleen uit de clients met deze registraties overeenkomen. Bijvoorbeeld Als al uw registraties met NH zijn gemaakt met tag "Politiek" zegt en u een melding met de markering verzendt 'Sporten', wordt deze niet worden verzonden naar elk apparaat. Een complexe zaak kan gebruikmaakt van tag expressies waar u alleen geregistreerd met "Tag A" of "Tag B", maar bij het verzenden van meldingen, u hebt samengesteld "Tag A & & Tag B". Klik in de sectie zelf een diagnose stellen bij tips onderstaande zijn er manieren waarin u uw registraties samen met de tags die ze hebben kunt bekijken. 

2) **Sjabloon problemen**

Als u sjablonen gebruikt en zorg ervoor dat volgt u de richtlijnen beschreven op [sjabloon richtlijnen]. 

3) **Ongeldige registraties**

Als de melding Hub correct is geconfigureerd en eventuele labels/tag-expressies zijn gebruikt juist resultaat in het zoeken van geldige doelen waaraan de meldingen die moeten worden verzonden, NH uit verschillende verwerking batches parallel - elke batch verzenden van berichten naar een reeks registraties wordt uitgevoerd. 

> [AZURE.NOTE] Aangezien we de verwerking parallel doet, garanderen niet we de volgorde waarin de meldingen worden afgeleverd. 

Nu is Azure meldingen Hub geoptimaliseerd voor een 'op meest eenmaal' bericht bezorging-model. Dit betekent dat we verval dubbel proberen, zodat er geen meldingen meer dan één keer worden afgeleverd naar een apparaat. Om ervoor te zorgen dat dit we Kijk in de registraties en zorg ervoor dat slechts één bericht voordat het bericht dat is wel verzonden naar de PNS per apparaat-id wordt verzonden. Zoals elke batch wordt verzonden naar de PNS, die op zijn beurt is geaccepteerd en gevalideerd de registraties, is het mogelijk dat de PNS vastgesteld een fout opgetreden bij een of meer van de registraties in een partij, geeft als resultaat een fout tot Azure NH en daarmee ook volledig weghalen die batch verwerking beëindigd. Dit geldt vooral met APNS waarin een stream TCP-protocol. Hoewel we zijn geoptimaliseerd voor op meest zodra bezorging, in dit geval moeten we de trapframe registratie van onze database verwijderen en probeer de bezorging van de melding voor de rest van de apparaten in die batch.

U kunt foutinformatie opvragen voor de bezorging van mislukte poging ten opzichte van een registratie voor het gebruik van de Azure melding Hubs REST API's: [Per bericht Telemetrielogboek: krijgen melding bericht Telemetrielogboek](https://msdn.microsoft.com/library/azure/mt608135.aspx) en [PNS Feedback](https://msdn.microsoft.com/library/azure/mt705560.aspx). Zie de [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) bijvoorbeeld code.

##<a name="pns-issues"></a>PNS problemen

Zodra het meldingsbericht is ontvangen door de desbetreffende PNS wordt het bijbehorende verantwoordelijk voor het leveren van de melding bij het apparaat. Azure melding Hubs buiten de afbeelding die u hier is en geen controle op heeft als of als u de melding wilt worden bezorgd bij het apparaat. Aangezien de platform meldingsservices heel robuuste zijn, meestal meldingen de apparaten in een paar seconden uit de PNS hebt bereikt. Als u echter de PNS is beperken vervolgens Azure melding Hubs geldt een exponentiële terug uitschakelen strategie en als de PNS niet bereikbaar gedurende 30 minuten blijft klik er is een beleid op hun plaats staan voor het verlopen en deze berichten permanent neerzetten. 

Als een PNS probeert te geven van een melding, maar het apparaat offline is, is de melding die zijn opgeslagen door de PNS gedurende een beperkte periode en bezorgd bij het apparaat wanneer deze beschikbaar. Slechts één recente melding voor een bepaalde app is opgeslagen. Als meerdere meldingen worden verzonden terwijl het apparaat offline is, wordt de melding voor elke nieuwe de voorafgaande kennisgeving kunnen worden verwijderd. Dit gedrag van het behouden van alleen de nieuwste melding is aanwezig meldingen in APNS en samenvouwen in GCM (die gebruikmaakt van een sleutel samenvouwen) genoemd. Als het apparaat offline lange tijd blijft, worden geen meldingen die waren wordt opgeslagen voor deze verwijderd. Bron - [APNS richtlijnen] & [GCM richtlijnen]

Met Azure melding Hubs - kunt u een samenvoegen sleutel doorgeven via een HTTP-header met de algemene `SendNotification` API (bijvoorbeeld voor .NET SDK – `SendNotificationAsync`) naar de desbetreffende PNS die ook HTTP-headers die worden doorgegeven als duurt is. 

##<a name="self-diagnose-tips"></a>Zelf een diagnose stellen bij tips

Hier bespreken we de verschillende mogelijkheden voor het vaststellen en root ertoe leiden dat eventuele problemen melding Hub:

###<a name="verify-credentials"></a>Referenties verifiëren

1. **PNS developer portal**

    Controleer of u ze aan de desbetreffende PNS developer portal (APNS, GCM, WNS enzovoort) met behulp van onze [Aan de slag zelfstudies].

2. **Azure klassieke-portal**

    Ga naar het tabblad configureren om te controleren en overeenkomen met de referenties met die uit de PNS developer portal opgehaald. 

    ![][4]

###<a name="verify-registrations"></a>Registraties controleren

1. **Visual Studio**

    Als u Visual Studio voor de ontwikkeling gebruikt kunt u verbinding maken met Microsoft Azure en weergeven en beheren van een reeks Azure services met inbegrip van de Hub van meldingen van "Server Explorer". Dit is vooral handig voor uw omgeving ontwikkelaar/testen. 

    ![][9]

    U kunt weergeven en beheren van alle registraties in de hub uitvoeren die zijn aangepast gecategoriseerd voor platform, systeemeigen of sjabloon registratie, labels, PNS id, registratie-id en de vervaldatum. U kunt ook een registratie in de browser - nuttig zeggen als u wilt bewerken de gewenste tags bewerken. 

    ![][8]
 
    > [AZURE.NOTE] Visual Studio-functionaliteit voor het bewerken van registraties mag alleen worden gebruikt tijdens ontwikkelaar/testen met een beperkt aantal registraties. Als er een nodig om op te lossen uw registraties bulksgewijs voordoet, kunt u de functionaliteit van de registratie exporteren/importeren die hier is beschreven - [Exporteren/importeren registraties] (https://msdn.microsoft.com/library/dn790624.aspx)

2. **Service Bus explorer**

    Veel klanten gebruiken ServiceBus explorer hier - [ServiceBus Explorer] voor weergeven en beheren van hun hub melding beschreven. Het is een bron openen project verkrijgbaar via code.microsoft.com - [ServiceBus Explorer-code]

###<a name="verify-message-notifications"></a>Controleer of de meldingen voor berichten

1. **Azure klassieke Portal**

    U kunt gaan naar het tabblad "Foutopsporing" test u meldingen wilt verzenden naar uw klanten zonder een service backend hoeft omhoog en uit te voeren. 

    ![][7]

2. **Visual Studio**

    U kunt ook test meldingen vanuit de comforts van Visual Studio verzenden:

    ![][10]

    U kunt meer informatie over de hier - Visual Studio melding Hub Azure explorer-functionaliteit 
    
    - [Overzicht van VS Server Explorer]
    - [TEGENOVER Server Explorer blogbericht - 1]
    - [TEGENOVER Server Explorer blogbericht - 2]

###<a name="debug-failed-notifications-review-notification-outcome"></a>Fouten opsporen in mislukte meldingen / melding resultaat controleren

**De eigenschap EnableTestSend**

Wanneer u een melding via melding Hubs verzendt, aanvankelijk deze alleen wordt in de wachtrij staat voor NH moet processing om erachter te komen alle doelen en vervolgens uiteindelijk NH verzendt naar de PNS. Dit betekent dat wanneer u REST API of vanaf een van de client SDK gebruikt, de weer van uw gesprekken verzenden alleen betekent dat het bericht zijn in de wachtrij omhoog met melding Hub geplaatst heeft. Geeft een beter inzicht in wat is er gebeurd wanneer NH uiteindelijk hebt u het bericht verzenden naar PNS geen. Als uw aanmelding is niet bezorgd bij het clientapparaat worden gebruikt, bestaat de kans dat wanneer NH geprobeerd het bericht te bezorgen naar PNS, er is een fout opgetreden die bijvoorbeeld de grootte van de nettolading langer dan toegestaan door de PNS of de referenties die is geconfigureerd in NH ongeldig enzovoort zijn. Als u een beter inzicht in de PNS fouten, hebben we een eigenschap met de naam van [de functie EnableTestSend]ingevoerd. Deze eigenschap wordt automatisch ingeschakeld wanneer u testberichten vanuit de portal of Visual Studio-client verzenden en daarom kunt u om gedetailleerde informatie over foutopsporing weer te geven. U kunt deze via API's het voorbeeld van de .NET SDK waar dit nu beschikbaar is en wordt toegevoegd aan alle client-SDK's uiteindelijk. Om dit te gebruiken met de REST-oproep, toe te voegen een queryreeks-parameter bijvoorbeeld 'testen' genoemd aan het einde van uw gesprekken verzenden 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Voorbeeld (.NET SDK)*
 
Stel dat u .NET SDK via een systeemeigen mailpop-upmelding verzenden:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State`wordt gewoon provinciale `Enqueued` aan het einde van de uitvoering zonder eventuele inzicht in wat is er gebeurd met de push. Nu kunt u de `EnableTestSend` Booleaanse eigenschap tijdens de initialisatie van de `NotificationHubClient` en gedetailleerde status over de PNS fouten opgetreden bij het verzenden van de melding kan verkrijgen. De oproep verzenden hier duurt extra tijd om terug te keren omdat dit alleen retourneren is nadat NH de melding bij PNS om te bepalen van het resultaat is bezorgd. 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Voorbeeld van uitvoer*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
Dit bericht geeft aan beide ongeldige referenties zijn geconfigureerd in de hub melding of een probleem met de registraties op de hub en de aanbevolen cursus zou dit registratie verwijderen en zorg dat de client die steeds opnieuw voordat u het bericht verzendt. 
 
> [AZURE.NOTE] Houd er rekening mee dat het gebruik van deze eigenschap intensief wordt vertraagd en dus u alleen dit in ontwikkelaar/testomgeving met beperkte set van registraties gebruiken moet. We alleen verzenden foutopsporing meldingen naar 10-apparaten. We hebben ook een limiet van het verwerken van foutopsporing verzendt 10 per minuut. 

###<a name="review-telemetry"></a>Telemetrielogboek controleren 

1. **Azure klassieke-Portal gebruiken**

    De portal kunt u krijg een kort overzicht van alle activiteiten op uw Hub melding. 
    
    a) u kunt een samengevoegde weergave van de registraties, meldingen, evenals fouten per platform weergeven op het tabblad "dashboard". 
    
    ![][5]
    
    b) u kunt ook vele andere platform specifieke aan de doelstellingen van het tabblad 'Monitor' naar het een grondigere kijken vooral specifieke fouten geretourneerd wanneer NH probeert te verzenden van de melding naar de PNS PNS toevoegen. 
    
    ![][6]
    
    c) u moet beginnen met beoordeling van de **Binnenkomende berichten**, **Registreren**, **Succesvolle meldingen** en ga vervolgens naar per platform tabblad naar PNS specifieke fouten te bekijken. 
    
    d) als u de melding hub onjuist geconfigureerd met de verificatie-instellingen, ziet u PNS verificatiefout hebt. Dit is een goede aanwijzing de referenties PNS controleren. 

2) **Toegang via programmacode**

Meer details hier- 

- [Programma Telemetrielogboek Access]
- [Telemetrielogboek toegang via API's steekproef] 

> [AZURE.NOTE] Verschillende telemetrielogboek gerelateerde functies zoals **Exporteren/importeren registraties**, **Telemetrielogboek toegang via API's** enzovoort zijn alleen beschikbaar in Standard laag. Als u probeert te gebruiken van deze functies als u bezig gratis of eenvoudige laag bent vervolgens krijgt u uitzonderingsbericht daartoe bij het gebruik van de SDK en een HTTP 403 (niet toegestaan) ze rechtstreeks vanuit de REST API's te gebruiken. Zorg ervoor dat u bent aangekomen snel aan de standaard trapsgewijs via Azure klassieke Portal.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[Melding Hubs overzicht]: notification-hubs-push-notification-overview.md
[Zelfstudies aan de slag]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Sjabloon richtlijnen]: https://msdn.microsoft.com/library/dn530748.aspx 
[APNS richtlijnen]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM richtlijnen]: http://developer.android.com/google/gcm/adv.html
[Registraties exporteren/importeren]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[ServiceBus Explorer-code]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Overzicht van VS Server Explorer]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[TEGENOVER Server Explorer blogbericht - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[TEGENOVER Server Explorer blogbericht - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend-functie]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Programma Telemetrielogboek Access]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetrielogboek toegang via API's steekproef]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 