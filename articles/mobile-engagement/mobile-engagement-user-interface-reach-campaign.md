<properties 
   pageTitle="Gebruikersinterface van Azure mobiele betrokkenheid - bereik campagne" 
   description="Laern hoe campagnes maken en beheren van push-bericht met Azure Mobile betrokkenheid" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Hoe u de push notification campagnes maken en beheren
De sectie bereik van de gebruikersinterface kunt u een nieuwe Push-campagne met een complexe formule maken op basis van de informatie die u moet een push-bericht verzenden. De opties van een campagne Push verschillen afhankelijk van de vier campagnetypen: aankondigingen, Polls, worden gegevens en tegels (alleen voor Windows Phone).

### <a name="option-applies-to"></a>Optie van toepassing op:
- Talen: Alle (aankondigingen, Polls, gegevens gereedschap duwt, tegels)
- Campagne: Alle (aankondigingen, Polls, gegevens gereedschap duwt, tegels)
- Melding: Aankondigingen, Polls
- Content: Unieke voor elk campagnetype
- Doelgroep: Alle (aankondigingen, Polls, gegevens gereedschap duwt, tegels)
- Tijdsbestek: aankondigingen, Polls, tegels
- Test: Alle (aankondigingen, Polls, gegevens gereedschap duwt, tegels)
 
![Bereik-Campaign1][20]

## <a name="languages"></a>Talen
Het vervolgkeuzemenu talen kunt u een andere versie van uw Push verzenden naar apparaten die zijn ingesteld op verschillende talen gebruiken. Standaard ontvangt alle apparaten de dezelfde Push ongeacht welke talen zijn ingesteld om te gebruiken. De standaardtaal-versie van de Push ontvangen gebruikers met hun apparaat dat is ingesteld op een andere taal. Veel van de opties van de campagne push kunnen u alternatieve inhoud opgeven voor elk van de aanvullende talen die u hebt geselecteerd. 
 
![Bereik-Campaign2][21]

### <a name="language-differences-apply-to"></a>Verschillende talen van toepassing op:
- Talen: Unieke talen kunnen worden geselecteerd naast de standaardtaal
- Campagne: Zelfde voor alle talen
- Melding: Unieke voor elke taal naast de standaardtaal
- Content: Unieke voor elke taal naast de standaardtaal
- Doelgroep: Kan worden gefilterd op een aparte taal criterium
- Tijdsbestek: dezelfde voor alle talen
- Test: Kan worden verzonden naar elke taal per keer
 
### <a name="supported-languages"></a>Ondersteunde talen:
- Arabisch (ar) 
- Bulgaars (bg) 
- Catalonië (ca) 
- Chinees (CH) 
- Kroatisch (h) 
- Czech (CS) 
- Deens (da) 
- Nederlands (nl) 
- Engels (en) 
- Fins (fi) 
- Frans (Frans) 
- Duits (de) 
- Grieks (el) 
- Hebreeuws (hij) 
- Hindi (Hallo) 
- Hongaars (hu) 
- Indonesisch (id) 
- Italiaans (it) 
- Japans (ja) 
- Koreaans (ko) 
- Lets (lv) 
- Litouws (lt) 
- Maleis (macrolanguage) ([ms) 
- Noors (Bokmål) (nb) 
- Pools (pl) 
- Portugees (pt) 
- Roemeens (ro) 
- Russisch (ru) 
- Servisch (sr) 
- Slowaaks (sk) 
- Sloveens (sl) 
- Spaans (es) 
- Zweeds (AVP) 
- Tagalog (tl) 
- Thais (th) 
- Turks (tr) 
- Oekraïens (Groot-Brittannië) 
- Vietnamees (vi) 
 
## <a name="campaign"></a>Campagne
De naam en de categorie van de campagne ook instellen als wanneer u van plan bent de sectie doelgroep van een campagne Push negeren en verzend deze campagne via de API hebt bereikt (en bepaalde elementen met de geringe Push-API) in plaats daarvan kunt u de sectie campagne. Categorieën kunnen worden gebruikt met een aangepaste meldingssjabloon besturingselement app meldingen op basis van vooraf gedefinieerde instellingen. U kunt een lijst met uw bestaande 'categorieën' via de API hebt bereikt.

> Waarschuwing: Als u de optie 'Negeren publiek, push wordt verzonden naar gebruikers via de API' in de sectie "Campagne" van de campagne van een bereik gebruikt, de campagne wordt niet automatisch verzenden, moet u handmatig verzenden via de API hebt bereikt.
 
![Bereik-Campaign3][22]
 
### <a name="option-applies-to"></a>Optie van toepassing op:
- Naam: alle
- Categorie: Aankondigingen, Polls
- Publiek, negeren push worden verzonden naar gebruikers via de API: alle
 
## <a name="notification"></a>Melding
U kunt de sectie melding basisinstellingen instellen voor het opnemen van uw push: de titel van de Push, het bericht, een afbeelding in de app, of als deze worden weggeklikt. Instellingen voor veel meldingen zijn specifiek voor het platform van uw apparaat. U kunt aangeven of uw push worden verzonden 'in de app' of 'uitzoomen app' of beide. (Vergeet niet dat gebruikers kunnen wel "opt-in" of "opt-out' van"Afmelden bij app"bij het besturingssysteem niveau op hun apparaten worden en Azure Mobile betrokkenheid worden niet overschreven door deze instelling. Denk eraan dat de API hebt bereikt 'in de app selectiegrepen"en"verouderd app"verplaatst. De Push-API kan worden gebruikt voor "af bij de app" gereedschap duwt te verwerken.) Gereedschap duwt kunnen worden aangepast met afbeeldingen of HTML-inhoud, waaronder uitgebreide koppelingen voor het koppelen van buiten uw App of naar een andere locatie in uw App (Android SDK 2.1.0 of later domeinbeheerpagina van categorieën vereist). U kunt wijzigen van het pictogram- of iOS-badge en verzenden van tekst of web-inhoud (een pop-up met HTML-inhoud, URL-koppeling naar een andere locatie binnen of buiten de app). U kunt ook Android-apparaten bellen of met de Push trillen. (Houd er rekening mee dat u de juiste moet SDK machtigingen in uw android-apparaat bestandenlijst bestand als u wilt bellen of een apparaat trillen.) Er is momenteel geen bedrijfstak standaard voor Android "totaalbeeld' de grootte, aangezien formaten verschillende gegevens weergeven op elk apparaat zijn, maar 400 x 100 afbeeldingen bezig bent bijna elke schermgrootte.

### <a name="delivery-types"></a>Bezorging typen:
-    Afmelden bij de app alleen: de melding, worden bezorgd wanneer de gebruiker geen van de toepassing gebruikmaakt.
- Het verouderd app alleen melding vereist een certificaat van Apple of Google (APNS of GCM certificaat).
- App alleen: deze melding wordt weergegeven wanneer er op de toepassing wordt uitgevoerd.
- De melding gebruikgemaakt Capptain bezorgingssysteem bereiken van de gebruiker. U kunt de visuele lay-out/weergave van uw push volledig aanpassen.
- Op elk gewenst moment: Deze optie zorgt ervoor dat u een melding die op de toepassing verzendt of niet wordt uitgevoerd.

 
![Bereik-Campaign4][23]

### <a name="option-applies-to"></a>Optie van toepassing op:
- Melding: Aankondigingen, Polls
 
## <a name="content"></a>Inhoud
U kunt de sectie inhoud voor het wijzigen van de inhoud van uw aankondigingen, Polls, worden gegevens en tegels (alleen voor Windows Phone). De instelling van de inhoud van Push campagnes hoort bij het type campagne. 

### <a name="see-also"></a>Zie ook
- [Documentatie UI - hebt bereikt - Push-inhoud][Link 29]
 
![Bereik-Campaign5][24]

## <a name="audience"></a>Publiek
U kunt de sectie doelgroep een gewone lijst met items beperkingen instellen voor uw campagne of beperkingen voor de campagne op basis van aangepaste criteria definiëren. De standaardset met opties voor het beperken van uw publiek kunt u push aan nieuwe of oude of alleen systeemeigen push-gebruikers. U kunt ook een quotum naar Beperk het aantal gebruikers die de push ontvangen instellen. U kunt handmatig de expressie voor het filteren van de campagne als u wilt opnemen van een of meer gebruikers van target het criterium bewerken. Een expressie publiek, kunt u handmatig typen. Een dergelijke expressie moet expliciet de relatie tussen criteria definiëren. Een criterium wordt beschreven door een id die moet beginnen met een hoofdletter en mogen geen spaties bevatten. De relatie tussen de criteria kan worden beschreven met 'and', 'of', 'not' operatoren, evenals '(',')'. Voorbeeld: "Criterion1 of (Criterion1 en niet Criterion2)".

> Opmerking: Met een groot publiek opgenomen in campagnes, de serverzijde doelgroepen gescande afbeelding mag traag is, vooral als u probeert te starten meerdere campagnes op hetzelfde moment.

- Indien mogelijk slechts één campagne een moment starten.
- Alleen het meeste, vier campagnes starten tegelijk.
- Push alleen naar uw actieve gebruikers (selectievakje 'deelnemen alleen gebruikers die kunnen worden bereikt systeemeigen Push met' en ' alleen actieve gebruikers ") zodat alleen gebruikers die nog steeds de app hebt geïnstalleerd en gebruik deze moet worden doorzocht.
Als uw publiek is gedefinieerd, kunt u de knop simulate wilt weten hoeveel gebruikers deze Push ontvangt. Hiermee berekent u het aantal bekende gebruikers mogelijk gericht door deze doelgroep (dit is een raming op basis van een steekproef van gebruikers). Let erop dat gebruikers die de toepassing hebt verwijderd, ook deel uitmaken van deze doelgroep, maar kunnen niet worden bereikt.

### <a name="see-also"></a>Zie ook
- [UI - bereik - documentatie nieuwe Push criterium][Link 28]

![Bereik-Campaign6][25]

### <a name="edit-expression"></a>Expressie bewerken
![Bereik-Campaign7][26]
 
### <a name="limit-your-audience-option-applies-to"></a>Limiet voor die uw publiek de optie is van toepassing op:
- Alleen een subset van gebruikers deelnemen: alle (aankondigingen, Polls, gegevens worden, tegels)
- Alleen oude gebruikers deelnemen: alle (aankondigingen, Polls, gegevens worden, tegels)
- Alleen nieuwe gebruikers deelnemen: alle (aankondigingen, Polls, gegevens worden, tegels)
- Alleen niet-actieve gebruikers deelnemen: aankondigingen, Polls, tegels
- Alleen actieve gebruikers deelnemen: alle (aankondigingen, Polls, gegevens worden, tegels)
- Alleen gebruikers die kunnen worden bereikt met systeemeigen Push deelnemen: aankondigingen, Polls
 
## <a name="time-frame"></a>Tijdsbestek
U kunt de sectie tijdsbestek om in te stellen wanneer de push worden verzonden of als u niets tijdsbestek de campagne om onmiddellijk te beginnen. Houd er rekening mee dat volgens de eindgebruikers tijdzone mogelijk de campagne start een dag eerder dan u zou voor uw eindgebruikers in Azië verwachten en kleine batches van gereedschap duwt tegelijk verzendt totdat alle tijdzones in de wereld overeenkomen met de periode instellen voor uw campagne. Volgens de eindgebruikers tijdzone kan ook worden vertraagd campagnes omdat het aanvragen van de tijd vanaf de telefoon voordat u begint met de push heeft.

> Opmerking: Campagnes zonder een einddatum in te voeren kunt cache gereedschap duwt lokaal en nog steeds waar ze worden weergegeven nadat u handmatig voltooid campagnes. Dit probleem, specifiek geen eindtijd voor campagnes kunt voorkomen.

### <a name="see-also"></a>Zie ook
- [Bereiken - hoe instructies – plannen][Link 3] 
 
![Bereik-Campaign8][27]

### <a name="settings-apply-to"></a>Instellingen van toepassing op:
- Tijdsbestek: aankondigingen, Polls, tegels
 
## <a name="test"></a>Test
U kunt de sectie Test deze push verzenden naar uw eigen testapparaat voordat u de campagne. Als u een aangepaste talen voor deze campagne hebt geconfigureerd, kunt u de push testen in elke taal. U kunt een testapparaat van 'Mijn Account' instellen.
> Opmerking: Geen serverzijde gegevens wordt geregistreerd bij het gebruik van de knop ' Test ' verplaatst, gegevens alleen is aangemeld voor reële push campagnes.

### <a name="see-also"></a>Zie ook
- [UI-documentatie - Mijn Account][Link 14]
 
![Bereik-Campaign9][28]

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
 
