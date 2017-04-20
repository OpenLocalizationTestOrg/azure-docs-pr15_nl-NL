<properties 
   pageTitle="Azure mobiele betrokkenheid-handleidingen voor probleemoplossing" 
   description="Gids voor probleemoplossing voor Azure mobiele betrokkenheid" 
   services="mobile-engagement" 
   documentationCenter="" 
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

# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure mobiele betrokkenheid - gids voor probleemoplossing

## <a name="introduction"></a>Inleiding
De volgende gids voor probleemoplossing kunt u meer informatie over hoofdsite oorzaken van enkele regelmatig terugkomt en wordt u om op te lossen op uw eigen inschakelen. 

## <a name="general"></a>Algemene

In het algemeen, zorg er altijd het volgende:

1. Zorg ervoor dat u de stappen die zijn vereist voor integratie, zoals wordt beschreven in onze [aan de slag zelfstudies](mobile-engagement-windows-store-dotnet-get-started.md) hebt doorlopen
2. Gebruikt u de nieuwste versie van het platform SDK's. 
3. Testen op een werkelijke apparaat en een emulator omdat enkele problemen specifiek voor de emulator alleen zijn. 
4. U bent niet kunt u door een limieten/throttles van Mobile betrokkenheid die worden beschreven [hier](../azure-subscription-service-limits.md)
5. Als u geen verbinding maken met de betrokkenheid van Mobile service-end of zien gegevens niet worden geladen continu vervolgens Zorg ervoor dat er geen lopende service-incidenten door te schakelen [hier](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>'Monitor' problemen

### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Ik zie niet mijn apparaat niet weergegeven op het tabblad beeldscherm
Tabblad beeldscherm ziet u de apparaten die zijn verbonden met uw mobiele betrokkenheid platform in realtime. Als u foutopsporing op een emulator en het apparaat, ziet u ten minste één sessie hier. Als de app is verdeeld, ziet u het actieve sessies toelichtingen overeenkomen met de hulpmiddelen die zijn verbonden met het platform in realtime. 

Als u uw apparaat niet op het tabblad beeldscherm ziet is waarschijnlijk een probleem SDK-integratie. Dit zijn enkele algemene stappen uitvoeren om op te lossen:

1. Zorg ervoor dat u de juiste verbindingsreeks in de mobiele app gebruikt en afkomstig van de SDK toetsen en niet de API toetsen sectie is. De verbindingsreeks maakt verbinding met uw mobiele app het exemplaar van de betrokkenheid van de Mobile-app waarin u uw apparaat op het tabblad beeldscherm ziet. 
2. Voor Windows-platform - als de pagina overschrijft de `OnNavigatedTo` methode, zorg dan dat u belt `base.OnNavigatedTo(e)`.
3. Als u mobiele betrokkenheid in een mobiele app van bestaande integreert en u ook voor zorgen kunt dat u niet alle stappen gemist door te kijken in de geavanceerde integratie stappen [hier](mobile-engagement-windows-store-integrate-engagement.md)
4. Controleer of dat u ten minste één scherm/activiteit verzendt door te overschrijven van de pagina met EngagementActivity afhankelijk van het platform dat u werkt zoals is beschreven in de [zelfstudies aan de slag](mobile-engagement-windows-store-dotnet-get-started.md).

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Ik zie het tabblad beeldscherm een sessie met zelfs wanneer ik heb verbinding is verbroken of gesloten mijn app / emulator. 
Als u de enige die is verbonden met het platform op dit punt bent en u een emulator gebruikt om de app te openen is dit waarschijnlijk vanwege emulator quirks. In het algemeen, moet u ervoor zorgen dat u keert u terug naar het beginscherm op de emulator voor de app-sessie met succes verbreken. Daarnaast kunnen op Windows-platform, foutopsporing met Visual Studio, hebt u misschien nodig om ervoor te zorgen dat in Visual Studio u Ga naar de menubalk **Levenscyclus gebeurtenissen** en klik op **onderbreken** echt de sessie te sluiten. Zie [Windows zelfstudie](mobile-engagement-windows-store-dotnet-get-started.md) voor meer informatie. 

## <a name="analytics-issues"></a>'Analytics' problemen

### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Ik zie geen gegevens / vernieuwd gegevens op Analytics-tabblad 
Analytics-gegevens wordt opnieuw berekend regelmatig en het duurt maximaal 24 uur voor deze vernieuwen. Deze gegevens niet realtime en ziet u dat het vernieuwd binnen deze 24 uur tijdsperiode.
Controleer echter dat u ten minste één scherm of activiteit op de backend platform door een van beide overschrijven ten minste één pagina met verzendt `EngagementActivity` of inbelt `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Ik zie onjuist vastgelegde datum/tijd voor een apparaat op het tabblad Analytics
De periode van analysegegevens is gebaseerd op de datum van de gebruikers apparaatinstellingen. Dus controleer of het apparaat dat de datum correct ingesteld. 

## <a name="segment-issues"></a>'Segment' problemen

### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Ik heb een segment hebt gemaakt en wordt niet weergegeven zoals lichter gekleurd weergegeven of alle gegevens, niet weergegeven
Segment maken is momenteel niet realtime. Deze worden berekend op hetzelfde moment als het analytics-gegevens worden samengevoegd en zodat het tot 24 uur kan duren. U moet terug later controleren, maar ondertussen u moet ook voor zorgen dat uw mobiele apps voor de gegevens op basis waarvan u de segmenten zijn die daadwerkelijk verzendt. Bijvoorbeeld Als een gebeurtenis Zeg 'foo' niet is verzonden door elk mobiel apparaat, klik er niet zou segmentgegevens voor een segment die zijn gemaakt met EventName = foo als het criterium. U moet ook controleren integratie met de SDK om ervoor te zorgen van uw mobiele app van de gegevens correct verzendt. 

## <a name="reach-or-push-notifications-issues"></a>Problemen met 'Hebt bereikt' of Push-meldingen

### <a name="my-push-messages-are-not-being-delivered"></a>Mijn push-berichten worden niet wordt bezorgd. 

1. Probeer meldingen te sturen naar een testapparaat eerst om ervoor te zorgen dat alle onderdelen - mobiele app, SDK en de service zijn goed verbonden en kunnen leveren push-meldingen. 
2. De eenvoudigste out-van-app melding altijd eerst via een campagne die is niet gepland verzenden en of er een publiek-criterium dat is opgegeven. Dit is nogmaals om te bewijzen dat melding connectivity correct werkt. 
3. Als u problemen hebt bij het bezorgen van meldingen in-app vervolgens is ook een goede eerste stap om te proberen eerst verzenden van een melding out-van-app. 
4. Zorg ervoor dat de 'systeemeigen Push' correct is geconfigureerd voor uw mobiele app. Afhankelijk van het platform wordt deze al gebruikmaakt toetsen (Android, Windows) of certificaten (iOS). Zie [gebruikersinterface - instellingen](mobile-engagement-user-interface-settings.md)
5. Afmelden bij de app meldingen kunnen ook worden geblokkeerd door de gebruiker via het mobiele OS dus zorgen dat dit is niet het geval. 
6. Zorg ervoor dat u niet de optie *negeren publiek, push worden verzonden naar gebruikers via de API* in de sectie **campagne** van een bereik campagne instellen wilt omdat dit zorgt ervoor dat push-meldingen kunnen alleen worden verzonden via API's. 
7. Zorg ervoor dat u uw campagne push test met zowel een apparaat verbonden via Wi-Fi en telefoon operator-netwerk om te voorkomen van de netwerkverbinding als een mogelijke bron van problemen.
8. Zorg ervoor dat de systeem-datum/tijd op uw apparaatemulator juist is, omdat elk apparaat niet gesynchroniseerd ook verstoort de Push Notification-Service de mogelijkheid om uit te voeren meldingen. 

Meer platform specifieke probleemoplossing onderstaande instructies:

1. **iOS** 

    - Zorg ervoor dat de certificaten geldig en lopende voor iOS Push-meldingen zijn. 
    - Zorg ervoor dat u *een productiecertificaat* correct in uw betrokkenheid van de Mobile-app configureert. 
    - Zorg ervoor dat u wilt testen op een *apparaat echte.* Push-berichten kan niet worden verwerkt door de iOS-simulator.
    - Zorg ervoor dat de bundel-id correct is geconfigureerd in de mobiele app. Zie de instructies [hier](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
    - Wanneer u test, gebruik u "Ad-Hoc" verdeling voor uw mobiele inrichten profiel. Niet is mogelijk worden geïnformeerd als uw app is gecompileerd met "Foutopsporing"

2. **Android-apparaat**

    - Zorg ervoor dat u het juiste Project-nummer hebt opgegeven in de mobiele app AndroidManifest.xml bestand dat wordt gevolgd door \n teken. 
    
            <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
        
    - Zorg ervoor dat u niet verdwenen zijn of verkeerd geconfigureerd machtigingen in het bestand Android bestandenlijst 
    - Zorg ervoor dat het Project-nummer dat u aan uw client-app toevoegt afkomstig is van hetzelfde account waar u de sleutel GCM Server hebt ontvangen. Een verkeerde combinaties tussen de twee wordt voorkomen dat uw gereedschap duwt wordt verzonden. 
    - Als u ontvangt wel systeemmeldingen, maar niet in de app en controleren op [Geef een pictogram voor de sectie meldingen](mobile-engagement-android-get-started.md) kans geeft u niet de juiste pictogrammen in het bestand Android bestandenlijst. 
    - Als u een melding BigPicture verzendt en zorg ervoor dat als u externe afbeeldingsservers hebt en ze moeten kunnen naar ondersteuning HTTP 'Krijgt' en "Kop".

3. **Windows**
    
    - Zorg ervoor dat u de app hebt gekoppeld aan een geldige Windows Store-app. U moet in Visual Studio - Klik met de rechtermuisknop op het project en selecteer "App met Store koppelen" optie en selecteer de app die u hebt gemaakt in de Windows Store. Deze Windows Store-app moet hetzelfde vanaf waar u de referenties systeemeigen push configureren in de portal Mobile betrokkenheid hebt ontvangen.
    - Als u ontvangt wel out-van-app push-meldingen, maar niet in-app-meldingen met `EngagementOverlay` integratie vervolgens zorgen dat er is een raster hoofdelement in uw pagina. EngagementOverlay Hiermee gebruikt u het eerste "Raster"-element gevonden in uw bestand xaml voor het toevoegen van twee web op de pagina weergaven. Als u zoeken naar waar webweergaven wordt ingesteld wilt, kunt u een raster met de naam "EngagementGrid" zoals dit echter wordt er om ervoor te zorgen er voldoende hoogte en breedte voor de twee volgende web weergaven waarin de melding en de volgende beschrijving als app melding wordt weergegeven:
        
            <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Ik heb een push-meldingen/aankondiging gemaakt/marketingcampagne en zelfs nadat het mij de melding verzonden, wordt deze weergegeven als 'Actief'. Wat betekent dit? 
De **campagne** die u hebt gemaakt in Mobile betrokkenheid heet zodat omdat het een langdurige betekenis van de push-meldingen als nieuwe apparaten verbinding met uw mobiele betrokkenheid-platform maken, ze automatisch wordt verzonden de melding die u hier configureren zo lang maken als ze voldoen aan het criterium dat u in de campagne instellen. Dit is niet een schermafbeelding één melding-instelling. U moet handmatig klikken op de knop **klaar bent met** de campagne beëindigen zodat deze niet verder meldingen verzenden. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Ik heb een push-campagne gemaakt en ik ontvang meldingen is echter wanneer ik de app opent, de melding dezelfde verschijnt zelfs wanneer ik had mailbericht het eerder? 
Dit is waarschijnlijk optreden tijdens de test en als u een emulators of enkele gebruikt test framework zoals TestFlight. Wat gebeurt er als volgt aan elke app exemplaar uitgevoerd, is dit bij het verkrijgen van een nieuwe DeviceID en verzend deze naar onze backend waardoor het platform Mobile betrokkenheid het als een nieuwe apparaat en het verzenden van de melding te. 

## <a name="getting-support"></a>Ondersteuning

Als u niet het probleem op te lossen kunt u vervolgens een zelf doen:

1. Zoek het probleem in de bestaande threads op StackOverflow forum en [MSDN-forum](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) en als niet vervolgens een vraag er. 
2. Als u een functie vervolgens toevoegen/stem voor de aanvraag op onze [UserVoice-forum](https://feedback.azure.com/forums/285737-mobile-engagement/) ontbreken vinden
3. Als u Microsoft ondersteuning openen een incident ondersteuning hebt door op te geven van de volgende details letten: 
    - Azure abonnements-ID
    - Platform (bijvoorbeeld iOS, Android enzovoort)
    - App-ID
    - Campagne-ID (voor problemen van push-meldingen)
    - Apparaat-ID
    - Mobile betrokkenheid SDK versie (bijvoorbeeld Android SDK v2.1.0)
    - Meer informatie over fout met exacte foutbericht wordt weergegeven en scenario
