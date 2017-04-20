<properties 
   pageTitle="Gebruikersinterface van Azure mobiele betrokkenheid - bereik" 
   description="Meer informatie over het contact maken voor de gebruikers van uw toepassing met push-meldingen met Azure Mobile betrokkenheid" 
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


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Hoe kan worden bereikt af aan de gebruikers van uw toepassing met push-meldingen

In dit artikel worden de tabblad **bereiken** van de **Betrokkenheid van de Mobile** -portal. U de **Betrokkenheid van de Mobile** -portal gebruiken om te controleren en beheren van uw mobiele apps. Houd er rekening mee dat om te beginnen met behulp van de portal moet u eerst een account **Azure Mobile betrokkenheid** maken. Zie [een Mobile betrokkenheid van Azure-account maken](mobile-engagement-create.md)voor meer informatie.

De sectie bereik van de gebruikersinterface de Push campaign hulpmiddel voor het beheer u maken/bewerken/activeren/voltooien/monitor kunt is en u statistieken over campagnes voor Push-meldingen en functies die ook kunnen worden geraadpleegd via de API hebt bereikt (en bepaalde elementen van laag niveau Push-API). Houd er rekening mee dat of u met de API's of de gebruikersinterface, moet u integreren zowel Azure Mobile betrokkenheid en het bereik in uw toepassing voor elk platform met de SDK voordat u de beschikking over campagnes hebt bereikt.

>[AZURE.NOTE] Veel secties van de portal voor **Mobiel betrokkenheid** UI bevatten de knop **HELP weergeven** . Druk op deze knop als u wilt meer contextuele informatie over een sectie.

## <a name="four-types-of-push-notifications"></a>Vier soorten Push-meldingen
1.    Aankondigingen - kunt u reclame berichten verzenden naar gebruikers die naar een andere locatie in uw app leiden of stuurt u naar een webpagina of winkel buiten uw app. 
2.    Polls - kunt u gegevens verzamelen van eindgebruikers door ze vragen.
3.    Gegevens gereedschap duwt - kunt u een binair getal of base64 gegevens-bestand verzenden. De gegevens in een push gegevens is verzonden naar uw toepassing voor het wijzigen van de huidige ervaring uw gebruikers in uw app. Uw toepassing moet kunnen de gegevens in een push gegevens verwerken.

## <a name="campaign-details"></a>Campagnedetails van de

U kunt bewerken, kopiëren, verwijderen of campagnes die u hebt nog niet geactiveerd door hun namen aanwijst activeren of u kunt klikken op om deze te openen. U kunt campagnes die al zijn geactiveerd door hun namen aanwijst klonen of u kunt klikken op om deze te openen. U kunt een campagne echter niet wijzigen zodra deze is geactiveerd.
 
![Reach1][18]

## <a name="reach-feedback"></a>Feedback hebt bereikt

Klik op **Statistieken** de details van een bereik campagne kunnen zien. De **eenvoudige** weergave biedt een visuele weergave in de vorm van een kolom staafdiagram over wat er is gebeurd nadat een campagne is geactiveerd. De weergave **Geavanceerd** biedt meer gedetailleerde informatie over de push-campagne. Deze gegevens is alleen beschikbaar als u een test campagne dat wil zeggen een push verzonden naar een testapparaat verzendt. Hier ziet u hoe u deze gegevens moet interpreteren:

1. **Pushed** - Hiermee wordt het aantal berichten verplaatst naar de apparaten. Dit nummer, is afhankelijk van de doelgroep die u hebt opgegeven bij het maken van de campagne push. Als u een doelgroep niet opgeeft, wordt klikt u vervolgens deze push worden verzonden naar de geregistreerde apparaten. Als alle andere push-services, doen we push-de meldingen niet rechtstreeks naar de apparaten, maar in plaats daarvan push ze naar het desbetreffende platform specifieke Push Notification Services (PNS - APNS/GCM/WNS) zodat ze de meldingen op de apparaten overbrengen kunnen. 

2.  **Geleverd** - Hiermee wordt het aantal berichten die goed worden geleverd door de PNS bij het apparaat en bevestigd als ontvangen door Mobile betrokkenheid SDK. 
        
    *Redenen om geleverd tellen wordt kleiner zijn dan gedistribueerde aantal:*
    
    1. Als de gebruiker de app van het apparaat is verwijderd, maar de PNS niets bekend over deze op het moment dat we de push naar de PNS verzenden wordt vervolgens het bericht verwijderd.
    2. Als het apparaat de app heeft, maar de apparaten zichzelf voor langere tijd offline was, klikt u vervolgens de PNS, mislukt het bericht te bezorgen bij het apparaat. 
    3. Als het bericht afgeleverd bij het apparaat, maar de mobiele betrokkenheid SDK in de app wordt niet herkend door de inhoud van het bericht, worden deze dat bericht. Dit kan gebeuren als de aanpassing van de melding in de app genereert een uitzondering die we onderschept in de SDK en decoratieve het bericht. Dit kan ook gebeuren als de app op het apparaat gebruikt een versie van de mobiele betrokkenheid SDK dat wil zeggen niet mogen de nieuwere versie van de push-bericht verzonden vanuit het platform begrijpen, maar dit is alleen wanneer u de app is bijgewerkt nadat de melding van de service-platform is verzonden. Het tabblad **Geavanceerd** laat weten hoeveel berichten zijn verwijderd. 
    4. Op apparaten met iOS, kunnen berichten soms niet verzonden als beide het apparaat op bijna leeg is of als de app aanzienlijke hoeveelheid power verbruikt tijdens het verwerken van externe meldingen. Dit is een beperking van de iOS-apparaten.   

3.  **Weergegeven** - Hiermee wordt het aantal berichten die wel aan de app-gebruiker op het apparaat in de vorm van een melding van de push-out-van-app systeem in het midden van de melding of een melding in-app in de mobiele app weergegeven worden.  Het tabblad **Geavanceerd** ziet u hoeveel zijn systeemmeldingen en hoeveel zijn in de app-meldingen. 
    
    *Redenen om weergegeven tellen wordt kleiner zijn dan geleverd aantal (wachten moet worden weergegeven)*
    
    1. Als de campagne melding hebt u er een einddatum in te voeren op deze vervolgens is het mogelijk dat het bericht is bezorgd, maar wanneer het tijd hebt gekregen om te openen en weer te geven aan de app-gebruiker, dit al verlopen is zodat deze nooit is weergegeven.   
    2. Als de melding een melding voor de app is wordt de melding alleen weergegeven wordt wanneer de app-gebruiker wordt geopend de app. In gevallen waar de app-gebruiker de app nog niet hebt geopend, worden de SDK wordt gemeld dat het bericht is bezorgd, maar nog niet weergegeven totdat de app wordt geopend. 
    2. Als de melding een melding voor de app wordt en geconfigureerd op een specifieke activiteit/scherm wordt weergegeven en vervolgens de melding wordt ook worden gerapporteerd als geleverd, maar nog niet is bezorgd totdat opent de gebruiker de app op een specifieke scherm. 
    
4.  **Gebruikersinteracties** - Hiermee wordt het aantal berichten waarin de app-gebruiker heeft gecommuniceerd met en er worden berichten die mailbericht of afgesloten zijn. 

    - *De app-gebruiker kan de actie een melding op de volgende manieren:*
            
        1. Als de melding is een melding systeem/out-van-app of een app-melding verzonden melding alleen-lezen en vervolgens de app-gebruiker op de melding klikt.
        2. Als de melding een melding voor de app met een tekst of webweergave of polls is vervolgens klikt de app-gebruiker op de actieknop in de melding.
        3. Als de melding een melding voor de app met een web-weergave is en de app-gebruiker op een URL in de webweergave [alleen Android klikt]
    
    - *De app-gebruiker kan een melding op de volgende manieren sluiten:*
    
        1. De knop sluiten op de melding rechtstreeks op te klikken. 
        2. Vegen afwezig of verwijderen van de melding. 
        3. App-meldingen met tekst/webinhoud en polls worden meestal weergegeven aan de gebruiker van de app in een proces. Ze eerst te zien krijgen een melding en ze klik erop, zien wanneer ze de volgende tekst/web/peiling-inhoud. De app-gebruiker kan een melding op een van de volgende stappen uit afsluiten en de details in de geavanceerde weergave wordt vastgelegd dit. 

5.  **Actioned** - Hiermee wordt het aantal berichten die expliciet mailbericht door de app-gebruiker waren. Dit is het meest interessant getal zoals dit wordt aangegeven hoeveel app-gebruikers zijn geïnteresseerd door het bericht dat u in de melding geactiveerd. 
 
> [AZURE.NOTE] Klik op iOS- en Windows platforms, als de gebruiker de app heeft openen en de campagne is een campagne "Op elk gewenst moment" dan is het mogelijk dat beide afmelden bij de app en meldingen in-app op hetzelfde moment worden weergegeven. Hierdoor kunnen een aantal weergegeven hoger zijn dan de geleverd. Als de gebruiker de interactie tussen en of acties de melding en zelfs de gebruiker interacties/Actioned telling is groter dan geleverd. 


![Reach2][19]

## <a name="see-also"></a>Zie ook

- [Concepten][Link 6]
- [Probleemoplossing Guide-Service][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
