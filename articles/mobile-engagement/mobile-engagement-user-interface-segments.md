<properties 
   pageTitle="Gebruikersinterface van Azure mobiele betrokkenheid - segmenten" 
   description="Meer informatie over het maken en beheren van segmenten van gebruikers gebruikspatronen met Azure Mobile betrokkenheid identificeren" 
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

# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Het maken en beheren van segmenten van gebruikers gebruikspatronen te identificeren

In dit artikel worden de tabblad **segmenten** van de **Betrokkenheid van de Mobile** -portal. U de **Betrokkenheid van de Mobile** -portal gebruiken om te controleren en beheren van uw mobiele apps.

De sectie segmenten van de gebruikersinterface kunt u bezig bent met uw gebruikers op basis van de verschillende gedrag en analyses die u kunt krijgen van de toepassing en u kunt ook openen via de API segmenten segmenteren. Segmenten zijn 24 uur nadat ze zijn gemaakt, en ze elke 24 uur op basis van de meest recente analytics-gegevens worden zijn herberekend eerst berekend. Nadat een segment wordt berekend, wordt een grafiek "Day naar dag geschiedenis" elke dag.


>[AZURE.NOTE] Veel secties van de portal voor **Mobiel betrokkenheid** UI bevatten de knop **HELP weergeven** . Druk op deze knop als u wilt meer contextuele informatie over een sectie.

## <a name="create-segments"></a>Segmenten maken
U kunt een segment op basis van maximaal 10 criteria op een bepaalde termijn van 60 dagen in het verleden uit de sectie analyses maken. U kunt bijvoorbeeld een segment op basis van de personen die u hebt bekeken bepaalde pagina's of specifieke inhoud in uw app zoekt in de afgelopen 10 dagen maken. Deze informatie is beschikbaar in de sectie analytics. Zo is, kunt u het maken van een segment en kies vervolgens een melding push afstemmen van deze een subset van de gebruikers instellen ze keert u terug naar de toepassing te laten. 
 
> Opmerking: Zodra een segment is berekend, deze kan niet worden bewerkt. alleen kan worden gekopieerd (gekopieerde) of verwijderd (verwijderd). Een segment kan worden gekopieerd binnen dezelfde toepassing (met de dezelfde toepassings-id) en deze kan ook worden gekopieerd naar andere toepassingen (met een ander toepassings-id). 
 
 ![segments1][35] 

## <a name="examples-segments"></a>Voorbeelden van segmenten
 ![segments2][36]

Segmenten kunnen u de eindgebruikers van uw toepassing segmenten.
Uw gebruikers segmenteren is een belangrijk marketingactiviteit strategie. Azure Mobile betrokkenheid kunt u historische gegevens ophalen en aangepaste segmenten maken. Deze krachtige hulpprogramma waarmee u wilt leren werken met uw klanten-ervaring in uw toepassing. U kunt eenvoudig uw segmenten analyseren en de segmenten als push doelen gebruiken.
Een algemene gebruiksvoorbeeld is dat u een push verzendt een melding wilt naar uw eindgebruikers waarderen uw toepassing in de winkel aanmoedigen. In plaats van een melding naar alle uw eindgebruikers verzendt, kunt u een segment dat alleen gebruikers die uw toepassing elke dag gebruikt voor de laatste maand en een goede gebruikersnaam ervaring hebben gehad wilt opgeven. Als u minder en zeer gerichte push-meldingen verzendt, krijgt u een betere ROI.
 
 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Segmenten die u kunt maken op basis van de belangrijkste elementen van de Azure Mobile betrokkenheid:
- Gebeurtenis: Maak een segment die doelen een specifieke gebeurtenis van de toepassing die is er gebeurd meer dan twee keer een week. 
- Sessie: Maak een segment van gebruikers aan wie de toepassing van de vorige week meer dan 5 keer hebt gebruikt.
- Activiteit: Maak een segment van de gebruikers die op één pagina of inhoud meer of minder dan 10 tijd afgelopen maand gebruikte eerder.
- Taak: Maak een segment van gebruikers aan wie een taak meer dan twee keer een dag hebt voltooid.
- Vastlopen: een segment van de gebruikers waarvoor vastlopen meer dan 10 keer vorige week maken. (U kunt dit segment met een verontschuldiging of zelfs een coupon kan push!)
- Fout: Maak een segment van alle gebruikers die een fout meer dan laatste 100 keer 3 dagen hebben gehad.
- App-Info: Maak een segment dat is bedoeld voor een aangepaste App Info die is er gebeurd tijdens de afgelopen 25 dagen.
 
 ![segments4][38]

Als u wilt gebruiken Segment optimaal, moet een aangepaste integratie van de SDK hebben gedaan in uw toepassing met een sociaalnetwerklabels plan 'App Info' tags.
Ga vervolgens naar de startpagina van de interface, selecteert u de toepassing die u wilt en klik op de sectie "Segmenten".

1. Selecteer de sectie "Segmenten".
2. Klik op het 'nieuwe segment"knop om een nieuwe segment te maken.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Reële leven voorbeeld: Een eenvoudige segment op basis van "Sessie" gegevens maken
Maak een segment van alle eindgebruikers die uw app ten minste 50 keer in de laatste week hebt gebruikt. Zoek hierin alleen de eindgebruikers die ten minste 30 seconden in uw app per sessie hebt besteed. Hier ziet alle eindgebruikers die een positieve ervaring in uw app hebben. Vervolgens kan het segment gemaakt worden gebruikt voor het push van een melding naar deze eindgebruikers ze uw app in de winkel beoordelen vragen.
 
 ![segments5][39]

1. Geef het segment een naam om te kunnen snel vinden in de lijst "Segment".
2. Klik op de knop 'Maken'.
 
 ![segments6][40]

Selecteer de sessie.
 
 ![segments7][41]

1. Selecteer de periode van 'Vorige week'.
2. Klik op volgende.
 
 ![segments8][42]

1. Selecteer de Operator die relevant zijn uit de lijst is: =; ≥, ≤.
2. Voer het gewenste aantal.
3. Selecteer de gewenste exemplaar. 
4. Klik op volgende.
In dit voorbeeld is ingesteld zodat die na verloop van de vorige week, overeenkomen met gebruikers die ten minste 50 sessies hebt aangebracht.
 
 ![segments9][43]

Voor gesegmenteerd sessie kunt u het aantal tekens per sessie als criterium.

1. Selecteer de Operator in de lijst.
2. Geef het aantal tekens per sessie.
3. Klik op volgende.
In dit voorbeeld wordt er aangegeven dat dat via alle sessies die in de sectie die exemplaar hebt is gesegmenteerd, selecteert u alleen de gebruikers die meer dan 30 seconden per sessie hebt besteed.
 
 ![segments10][44]

De naam van uw criterium om op te halen in de volledige Verkoopoverzicht en klik op voltooien.
 
 ![segments11][45]

Wanneer u ingesteld criterium hebt, wordt deze wordt weergegeven in het segment Verkoopoverzicht.
Omdat een segment is gebaseerd op analytics-gegevens, worden één keer per dag segmenten berekend.
In dit voorbeeld 47,7% van de totale eindgebruikers overeenkomt met het criterium. Ze moeten de gebruikers die een goede ervaring hebben gehad en wordt kans op een hogere classificatie als u ze een melding waarin u hen vraagt de app in de winkel waarderen push.


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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
