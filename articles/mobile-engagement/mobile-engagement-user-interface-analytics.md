<properties
   pageTitle="Gebruikersinterface van Azure mobiele betrokkenheid - analyses"
   description="Meer informatie over het analyseren van historische gegevens over uw-toepassing via Azure Mobile betrokkenheid"
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

# <a name="how-to-analyze-historical-data-about-your-application"></a>Het analyseren van historische gegevens over het gebruik van de toepassing

In dit artikel worden de **ANALYTICS** -tabblad van de **Betrokkenheid van de Mobile** -portal. U de **Betrokkenheid van de Mobile** -portal gebruiken om te controleren en beheren van uw mobiele apps. Houd er rekening mee dat om te beginnen met behulp van de portal moet u eerst een account **Azure Mobile betrokkenheid** maken.


De sectie analyses van de gebruikersinterface vindt geaggregeerde informatie over de toepassing op basis van historische gegevens die automatisch wordt bijgewerkt van elke 24 uur. De informatie die wordt weergegeven op verschillende dashboards op basis van de staaf-lijn-cirkel-diagrammen, rasters en kaarten. De gegevens kunnen ook worden gedownload als CSV-bestanden. Meest van dezelfde gegevens is beschikbaar in realtime in het gedeelte Beeldscherm van de gebruikersinterface en deze ook zijn toegankelijk vanaf de Analytics-API.

>[AZURE.NOTE] Veel secties van de portal voor **Mobiel betrokkenheid** UI bevatten de knop **HELP weergeven** . Druk op deze knop als u wilt meer contextuele informatie over een sectie.

## <a name="standard-and-custom-analytics"></a>Standaardkleuren of aangepaste Analytics

Azure Mobile betrokkenheid biedt een aantal eenvoudige, standaard analytische gegevens over uw toepassingen die kunnen worden waarde zodra u uw app met de SDK integreren. Azure Mobile betrokkenheid bovendien de mogelijkheid om extra aangepaste analytics-gegevens die u wilt over de werking van uw eindgebruikers te verzamelen. U kunt dit doen door te maken van een tag-abonnement van aangepaste "codes (app info)', gemaakt op basis van **Instellingen** zodat Azure Mobile betrokkenheid deze aanvullende gegevens voor u verzamelen kunt.



## <a name="analytics"></a>Analytics
- Dashboard: Bevat algemene informatie over uw nieuwe en u gebruikers en hun trends.
- Gebruikers: Gebruikers worden geïdentificeerd door hun apparaat-id: deze id is uniek voor elk apparaat (een nieuwe gebruiker is een nieuwe apparaat). Een gebruiker wordt beschouwd als nieuwe op een bepaald tijdsinterval als hij zijn eerste sessie tijdens dit tijdsinterval heeft uitgevoerd. Een gebruiker wordt beschouwd als zoals behouden als hij ten minste één sessie tijdens de laatste zeven dagen is uitgevoerd. Actieve gebruikers zijn gebruikers die ten minste één sessie gedurende een bepaalde periode. U kunt sorteren, maandelijks, wekelijks, dagelijks of per uur perioden. Alle grafieken lijken, maar u kunt u filteren op andere functies, zoals de versie van uw toepassing, en vervolgens sorteren op een bepaalde periode. De standaard door het integreren van de SDK verzamelde gegevens bevat de volgende handelingen uit: actieve gebruikers, nieuwe gebruiker, aantal sessies, lengte van elke sessie, technische informatie over het land, lokale variabelen, locatie, taal carrier, apparaten, firmware, netwerk (WIFI), versies van de app en SDK, die worden gebruikt door klanten. Deze informatie kan worden bekeken in realtime in de sectie met monitor.

> Opmerking: De periode is gebaseerd op de datum van de gebruikers apparaatinstellingen, zodat een gebruiker wiens telefoon is de datum niet correct ingesteld in de verkeerde tijdsperiode weergegeven kan.

- Bewaarbeleid: Een gebruiker wordt beschouwd als zoals behouden op een bepaald tijdsinterval als hij zijn eerste sessie tijdens dit tijdsinterval heeft uitgevoerd. U kunt de tijdsintervallen waarin behouden gebruikers (en nieuwe gebruikers) worden geteld wijzigen in uren, dagen, weken of maanden. De gebruiker bewaarbeleid analytics is gebouwd van dieren. Een cohort is het instellen van de nieuwe gebruikers gedetecteerd voor een bepaalde termijn (dat wil zeggen de set van de eerste sessie tijdens deze periode voeren gebruikers). We gebruiken dieren van 1 dag, 2 dagen, 4-dag, 7 dagen of 1 maand. Elke 1-dag 2-4-dag, 7 dagen, of 1 maand, Azure mobiele betrokkenheid berekent het instellen van alle gebruikers die tot de cohort en er wordt behoren een cohort gegeven, zich nog steeds actief (dat wil zeggen de set met gebruikers die ten minste één sessie gedurende de periode uitgevoerd). Deze groep gebruikers is een versie cohort genoemd. (Azure Mobile betrokkenheid kunt ziet u hoeveel van uw gebruikers de app nog steeds gebruikt, maar alleen de platform specifieke store kan u vertellen weten hoeveel van uw gebruikers verwijderd uit uw iTunes-app - bijvoorbeeld GooglePlay, Windows Store, enz.).
- Sessies: Één gebruik van de toepassing door een gebruiker. Sessies zijn gegenereerd op basis van de volgorde van activiteiten door gebruikers uitgevoerd (een activiteit is meestal gekoppeld aan het gebruik van één scherm van de toepassing, maar dit kan variëren, afhankelijk van de manier waarop de SDK is geïntegreerd in de toepassing). Een gebruiker kan alleen ene activiteit uitvoeren op een moment: een sessie wordt gestart zodra de gebruiker zijn eerste activiteit begint en stopt wanneer hij zijn laatste activiteit is voltooid. Als een gebruiker ongewijzigd meer dan een paar seconden zonder de uitvoering van een activiteit, klikt u vervolgens zijn volgorde van activiteiten splitsen in twee afzonderlijke sessies.
- Activiteiten: De namen van elk scherm in uw toepassing en de lengte van de gebruikers van de tijd besteden aan elk scherm. Activiteiten zijn de analytische optie van een aangepast die overeenkomen met de 'app info' tags die u hebt ingesteld voor uw eigen app:
- Gebruikerspad: Ziet u hoe uw gebruikers van uw toepassing activiteiten (schermen) navigeren. U kunt de schuifregelaar naar het niveau van details verplaatsen. Blauwe knooppunten vertegenwoordigen van uw toepassing activiteiten. De grootte is evenredig aan de tijd gebruikers besteed aan deze. Witte knooppunten vertegenwoordigen sessie start en beëindiging. Rode knooppunten vertegenwoordigen loopt. Koppelingen vertegenwoordigen overgangen tussen de activiteiten van uw toepassing (of tussen activiteiten en loopt). Klik op een knooppunt of een koppeling naar het weergeven van knopinfo met meer informatie over uw gegevens: de tijd besteed aan een bepaald scherm, het aantal overgangen en het percentage van een overgang uit de bron-activiteit naar de bestemming activiteit. (Een---60%---> B houdt in dat gebruikers van de activiteit A wordt Hiermee gaat u naar activiteit B 60% van de tijd.) U kunt opnieuw indelen in de grafiek die u wilt verduidelijken de positie ervan wordt opgeslagen telkens wanneer u een wijziging aanbrengt. U kunt weergeven of verbergen van de crashes om de grafiek lichter te maken.
- Gebeurtenissen: Bepaalde acties die u hebt gemaakt door een gebruiker in de toepassing. De verdeling van gebeurtenissen wordt weergegeven als het aantal gebeurtenissen per gebruiker per sessie. Een gebeurtenis vertegenwoordigt een direct actie, bijvoorbeeld een klik op een knop of de ontvangst van een melding. (De zin van gebeurtenissen is afhankelijk van hoe de SDK is geïntegreerd in de toepassing.) Een gebeurtenis kan kan optreden tijdens een sessie, of een taak of zelfstandig.
- Taken: Vergelijkbaar met gebeurtenissen, behalve ze de lengte van de actie te belichten. Bijvoorbeeld kunnen taken u vertellen technische informatie over hoe lang het duurt inhoud laden of een oproep naar webservice. Dit kan ook weergegeven hoe lang duurt een gebruiker een formulier invullen, maak een account of een aankoop. De duur van een taak van een taak aangeeft, bijvoorbeeld de duur van een downloadtaak of de tijd een banner wordt weergegeven op het scherm. (De zin van taken is afhankelijk van hoe de SDK is geïntegreerd in de toepassing.) Taken worden meestal gekoppeld aan achtergrondtaken die buiten het bereik van een sessie (dat wil zeggen zonder gebruikersactiviteit) worden uitgevoerd.
- Technicals: Technische informatie over de apparaten van de gebruikers van de app die u kunt, zoals de landinstelling, Carrier, netwerk, apparaat, Firmware bijhouden, en de grootte van de gebruikers-apparaten en de versie van uw App en de SDK-versie die wordt gebruikt in uw app scherm.
- Fouten: Informatie over technische fouten binnen de toepassing die de toepassing te lopen geen veroorzaken. Een fout vertegenwoordigt direct problemen ondervindt, bijvoorbeeld een netwerk is mislukt of een ongeldige bewerken. (De zin van gebeurtenissen is afhankelijk van hoe de SDK is geïntegreerd in de toepassing.) Een fout kan kan optreden tijdens een sessie, of een taak of zelfstandig.
- Loopt: Informatie over fouten die ertoe leiden dat uw toepassing te lopen. Vastlopen is een onverwachte voorwaarde waar de toepassing stopt uitvoering van de verwachte functies en moet worden beëindigd. Vastlopen is meestal vanwege een fout in de toepassing.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Toegang tot het bewaarbeleid-overzicht
![Analytics3][12]

Overzicht van het bewaarbeleid wordt opgesplitst in het midden naar meerdere kaarten, elk met het overzicht voor een bepaalde bewaarperiode. De bewaarperiode van 2 dagen wordt weergegeven in het voorbeeld. De andere kaarten bevatten de 4 en 7 dagen bewaarperiode.

## <a name="understanding-the-retention-overview-cards"></a>Informatie over het bewaarbeleid overzicht-kaarten
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Elke kaart bestaat uit 3 hoofdgebieden:
1. 1: de cohort en de periode beschouwd als
2. 2-4: het bewaarbeleid voor de huidige periode
3. 5: een Sparkline van de geschiedenis

### <a name="here-is-detailed-information-about-each-element"></a>Hier vindt u gedetailleerde informatie over elk element:
1.    Cohort en periode: deze kop geeft het type cohort. Hier betekent "2 dagen" dat gaan we het gedrag van gebruikers meer dan twee dagen, gebruikers die zijn aangekomen gedurende een periode van 2 dagen en of ze verbinding te maken in de volgende blokken van 2 dagen. De activiteiten van gebruikers tussen de 21 en 22e van November houdt rekening met het bovenstaande voorbeeld.
2.    Dit krijgt het tarief weer dat bewaarbeleid in de 21 en 22 november voor de gebruikers in 19 en 20 november binnengekomen. Hier hebben we 1 actieve gebruiker tussen de 21 en 22e, via de 3 dat nieuwe gebruikers tussen de 19e en 20e zijn.
3.    Deze visuele indicator geeft dezelfde gegevens als hierboven grafisch weergegeven. (De derde van de cirkel is van het getal 33%). De kleur biedt aanvullende informatie: groen geeft aan dat dit nummer groeit in de vorige berekening opgenomen. Geel betekent stabiele en rode betekent verlagen.
4.    Hiermee worden de waarden voor de berekening opgenomen.
5.    Dit is een Sparkline van de geschiedenis van de waarden van het bewaarbeleid. Dit kunt u de waarden in het verleden om te laten uitgebreid bekijken hoe deze die is voortgekomen zien.


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
