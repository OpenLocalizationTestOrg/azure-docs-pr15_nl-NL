<properties 
   pageTitle="Gebruikersinterface van Azure mobiele betrokkenheid - bereik hoe u"
   description="Gebruikersinterface, overzicht voor Azure mobiele betrokkenheid" 
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

# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Hoe u aan de slag gebruiken en beheren verplaatst naar om uw eindgebruikers bereiken

Zodra de SDK is volledig geïntegreerd in uw app, u kunt aan de slag met de de sectie bereik van de gebruikersinterface aan Push-meldingen aan de gebruikers van uw app.  

## <a name="do-your-first-push-notification-campaign"></a>Voer uw eerste campagne van de Push-meldingen
-    Bevestig dat uw bereik is geïntegreerd in uw app met de SDK. 
-    Selecteer de doeltoepassing
 
![First1][1]

-    Ga naar de sectie "Bereik" en klik op 'nieuwe aankondiging"
 
![First2][2]

-    Maak een nieuwe campagne en noem deze
 
 ![First3][3]

-    Selecteer hoe de melding moet worden weergegeven, als de app alleen
 
![First4][4]

-    Het bericht dat u wilt push maken
 
![First5][5]

-    U kunt een titel schrijven op de melding (optioneel).
-    Inhoud van de push-bericht schrijft.
-    U kunt een afbeelding uploaden. Let op van het bestand kan niet groter is dan 32.768 bytes.
-    U hebt ook de mogelijkheid om te nog meer opties kiezen, maar de nadruk in deze zelfstudie, we ziet die later.

-    Selecteer het inhoudstype alleen als melding
 
![First6][6]

-    Uw campagne push maken en deze wordt weergegeven in de lijst campagne.
 
![First7][7]

## <a name="test-your-push-notification-campaign"></a>Test uw campagne Push-meldingen
![Test1][8]

-    Uw apparaat registreren.
-    Klik op het selectievakje in van het apparaat dat u wilt push.
-    Klik op de knop 'Test' de push versturen naar het apparaat.
 
![Test2][9]

-    De campagne activeren
 
![Test3][10]

-    Nu u uw campagne hebt gemaakt alleen moet u deze voor de melding wordt verplaatst naar uw gebruikers te activeren.
 
## <a name="send-personalized-pushes"></a>Gepersonaliseerde gereedschap duwt verzenden
-    In dit voorbeeld wordt een push waar een aangepaste korting-code is ingevoerd in de melding push gemaakt.
 
![Personalize1][11]

Persoonlijke instellingen werkt door te vervangen door een markering voor uit een app info tag dus, hebt u om ervoor te zorgen dat de gebruiker heeft de juiste app-info eerst gedefinieerd. In dit voorbeeld hebben de beoogde gebruikers een app info tag benoemde rebate_code gedefinieerd.
Zoals u hierboven ziet wordt in de inhoud van de melding push de markering ${rebate_code} waarin wordt aangegeven dat dit is om te worden vervangen door de feitelijke inhoud van de app info tag bevat.

> Waarschuwing: Als de app info-code is gedefinieerd voor de gebruiker, ontvangt de gebruiker de push geen.

-    Resultaat
 
![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>U kunt de tekst die uw melding verder aanpassen
![Personalize3][13]

-    De titel van de melding, inclusief
-    En de inhoud van het bericht.
-    Kies het type aankondiging (weergave van de tekst en de weergave)
 
![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>De hoofdtekst van een aankondiging kan ook worden aangepast met:
-    De actie-URL, moet u wilt aanpassen van de aantekening toevoegen
-    De titel,
-    De hoofdtekst van het bericht.
 
 
## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Uw Push-meldingen onderscheiden (in- of uitzoomen app)
-    Kies het type melding u wordt push, selecteer uw toepassing gaat u naar de sectie 'Hebt bereikt', of een campagne push maken en Ga naar de sectie "Melding".
 
-    Klik op de modus"bezorging' die u wilt.
-    Klik op het selectievakje 'Activiteiten beperken' als u wilt dat de melding wordt uitgevoerd op specifieke activiteiten (schermen).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Niet alleen App" Bezorgingsmodus
![Differentiate2][16]

"Niet alleen App" Bezorgingsmodus biedt push-meldingen wanneer de toepassing is gesloten. Dit is de standaard push-bericht.
Wanneer u "niet alleen app" selecteert, moet u hebt al opgegeven certificaten uit het platform die uw toepassing is maken op (APNS of GCM).

### <a name="see-also"></a>Zie ook
-  [Apple Push Notification-Service – certificaten](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), Google Cloud Messaging – certificaat] (http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"App alleen" Bezorgingsmodus
![Differentiate3][17]

"App alleen" Bezorgingsmodus biedt push-meldingen wanneer de toepassing wordt uitgevoerd.
Voor deze melding hoeft u niet het systeem APNS en GCM doorlopen.
U kunt het systeem van de geadresseerde in de app uw eindgebruikers hebt bereikt.
U kunt volledig aanpassen van de melding en besluit waarin activiteit (scherm) de melding wordt weergegeven.

### <a name="anytime-delivery-mode"></a>"Op elk gewenst moment" Bezorgingsmodus
U kunt een modus "Op elk gewenst moment" bezorging, zorgt ervoor dat u te bereiken van uw eindgebruikers of de toepassing of niet wordt uitgevoerd.
Wanneer u "Op elk gewenst moment" selecteert, moet u hebt al opgegeven certificaten uit het platform die uw toepassing na (APNS of GCM). 
 
## <a name="schedule-a-push-campaign"></a>Een campagne Push plannen
### <a name="plan-to-start-a-campaign"></a>Plan een campagne starten
![Shedule1][18]

Dit is de 21 van maart en u hebt een aankondiging om te maken en voor de 22 van maart geschaafd middernacht. U hoeft niet te blijven vóór de interface moet een push! Vooraf de exacte minuut meldingen worden verzonden, kunt u plannen.
-    Schakel het selectievakje het, "geen" selectievakje en selecteert u een begintijd 
-    Kies de datum en de tijd die u wilt de campagne push starten.

### <a name="plan-to-end-a-campaign"></a>Tot het einde van een campagne plannen
![Shedule2][19]

U wilt dat uw campagne op de 25e van maart stopt bij 3-d: 00 uur, maar u weet dat er niet meer te doen.
U hoeft niet te blijven vóór de interface naar push! U kunt vooraf de exacte minuut die uw campagne worden niet meer kan plannen.
-    Klik op het, "geen" selectievakje of Selecteer een eindtijd
-    Kies de datum en de tijd die u wilt voltooien van de campagne push.

### <a name="end-a-campaign-manually"></a>Handmatig een campagne beëindigen
![Shedule3][20]

Standaard de "geen" selectievakjes zijn geselecteerd.
Dit betekent dat de campagne wordt gestart zodra u in de sectie bereik activeren en moet worden beëindigd wanneer u deze in de sectie bereik wilt stoppen.
 
> Opmerking: Campagnes gemaakt zonder een einddatum de push lokaal opslaan op het apparaat en dit de volgende keer dat de app wordt geopend, zelfs als de campagne handmatig wordt beëindigd weergeven.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Een Push-bericht met een tekstweergave verbeteren
### <a name="what-is-a-text-view"></a>Wat is de weergave van een tekst?
![TextView1][21]

Een tekstweergave is een pop-upvenster met de tekstinhoud van de. In dit pop-upvenster wordt weergegeven nadat de eindgebruiker heeft geklikt op de push-bericht.
Een tekstweergave kunt u meer inhoud aan uw eindgebruikers presenteren. Dit is ook de mogelijkheid om een oproep naar actie zoals springen aan een pagina van de app, omleiden naar een archief, voor het openen van een webpagina, een e-mail verzenden, het starten van een zoekopdracht geografische gelokaliseerd presenteren enzovoort...

### <a name="example-text-view"></a>Voorbeeld: Tekst weergeven
-    Uw campagne Push-meldingen in de sectie "Bereik" maken en geef een naam op voor de campagne
 
![TextView2][22]

-    Zet het bericht dat wordt weergegeven op de melding.
-    Selecteer het inhoudstype aankondiging van "tekst"
 
![TextView3][23]

> Opmerking: als u een tekstweergave push, altijd beschikt u over een melding eerst. 

- Het definiëren van de tekst (na hebt geselecteerd de inhoud van de aankondiging tekst de subsectie wordt weergegeven, zodat u kunt het definiëren van de tekst die u wilt weergeven.)
 
![TextView4][24]

-    Schrijf de titel die boven aan het bericht wordt weergegeven.
-    Schrijf de hoofdinhoud van de tekstweergave.
-    Schrijf de inhoud die wordt weergegeven op de actieknop (een actieknop kan de toepassing om een specifieke actie zoals het openen van een pagina van de toepassing, omleiden naar een App store of elk type bronnen die u kunt geven).
-    De inhoud die wordt weergegeven op de knop Afsluiten schrijven (door te klikken op de knop Afsluiten, de tekstweergave verdwijnt.)
 
-    De push notification-campagne maken en deze wordt weergegeven in de lijst campagne.
 
![TextView5][25]

-    Activeer de campagne push melding voor het verzenden van de tekstweergave aan uw gebruikers.
 
![TextView6][26]

-    Resultaat
 
![TextView7][27]

-    De gebruiker ontvangt de melding en klik op is geïnstalleerd.
-    De tekst wordt weergegeven als een pop-upvenster zodat de gebruiker interactief mee werken.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Een Push-bericht met een webweergave verbeteren
### <a name="what-is-a-web-view"></a>Wat is een webweergave?
![WebView1][28]

Een webweergave is een pop-upvenster met webinhoud. In dit pop-upvenster wordt weergegeven wanneer de eindgebruiker heeft op de push-meldingen hebt geklikt.
Een webweergave kunt u meer interactie met de eindgebruiker.
Dit is ook de mogelijkheid om een oproep naar actie zoals omleiding naar App Store, presenteren openen van een webpagina, een e-mail te verzenden, het starten van een zoekopdracht geografische gelokaliseerd, enzovoort...

### <a name="example-web-view"></a>Voorbeeld: Webweergave
-    Maak uw campagne Push in de sectie "Bereik" en geef een naam op voor de campagne.
 
![WebView2][29]

-    Zet het bericht dat wordt weergegeven op de melding.
-    Selecteer het inhoudstype aankondiging als "web"
 
![WebView3][30]

### <a name="about-announcement-types"></a>Over aankondiging typen:
- Melding alleen: dit is een eenvoudige standaard melding. Wat betekent dat als een gebruiker erop kan geen extra weergave wordt weergegeven, maar alleen de actie die is gekoppeld aan dit zal plaatsvinden.
- Aankondiging van tekst: dit is een melding die alleen mogelijk de gebruiker heeft een Kennismaking met een tekstweergave.
- Aankondiging van web: dit is een melding dat de gebruiker heeft een Kennismaking met een webweergave voert.
Selecteer de inhoud "Web aankondiging".

> Opmerking: Als u een webweergave push, altijd beschikt u over een melding eerst.

- De webinhoud definiëren (na hebt geselecteerd de aankondiging webinhoud, de subsectie wordt weergegeven, zodat u kunt het definiëren van de web weergave van inhoud moet worden weergegeven.)

 
![WebView4][31]

-    Schrijf de titel die boven aan het bericht (optioneel) wordt weergegeven.
-    Schrijf uw HTML-code hier.
-    Klik op de knop om te schakelen edition en Zie hoe deze eruitziet als bewerken bron.
-    Schrijf de inhoud die wordt weergegeven op de actieknop (een actieknop kan de toepassing om een specifieke actie zoals het openen van een pagina van de toepassing, omleiden naar een archief of een type bronnen die u kunt geven).
-    De inhoud die wordt weergegeven op de knop Afsluiten schrijven (door te klikken op de knop Afsluiten, de webweergave verdwijnt).
 
-    Resultaat
 
![WebView5][32]

-    De gebruiker de melding te ontvangen en klik erop.
-    De tekst wordt weergegeven als een pop-upvenster zodat de gebruiker interactief mee werken.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
