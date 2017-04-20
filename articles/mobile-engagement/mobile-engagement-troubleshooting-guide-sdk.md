<properties 
   pageTitle="Azure mobiele betrokkenheid de gids voor probleemoplossing - SDK" 
   description="Problemen met SDK integratie oplossen in Azure Mobile betrokkenheid" 
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

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Gids voor probleemoplossing voor SDK integratieproblemen

Hier volgen mogelijke problemen optreden bij hoe Azure Mobile betrokkenheid in uw toepassing integreert.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>SDK problemen ontdekt door een fout in een ander gebied van de toepassing

### <a name="issue"></a>Probleem
- Gegevens van UI-siteverzameling fout (in analyses, controle, segmentatie of Dashboards).
- Push-fouten (gereedschap duwt werken niet in app uitzoomen app of beide).
- Geavanceerde functie fouten (bijhouden, Geolocatie of platform die specifieke ladingen werken niet).
- API fouten (API's fail vaak stilte zonder foutberichten).
- Service-fouten (geen van Azure Mobile betrokkenheid werkt voor uw toepassing).

### <a name="causes"></a>Oorzaken

- De meeste problemen die moeten worden opgelost met de SDK van Azure Mobile betrokkenheid moeten worden gevonden door een fout in uw toepassing (zoals een UI gegevens siteverzameling is mislukt, push mislukt, geavanceerde functie mislukt, API mislukt, toepassing loopt of zichtbare servicestoring).  
- Als een bepaalde functie van Azure Mobile betrokkenheid nooit in uw app voordat heeft gewerkt, moet u de integratie voltooien. 
- Als een bepaalde functie van Azure Mobile betrokkenheid werkte en gestopt, moet u mogelijk een upgrade uitvoeren naar de laatste versie met de SDK van Azure Mobile betrokkenheid. Denk eraan dat er een andere versie van de Azure Mobile betrokkenheid SDK voor elk platform worden ondersteund door Azure Mobile betrokkenheid (Android, iOS, Windows en Windows Phone).

#### <a name="sdk-integration"></a>SDK-integratie

- Azure Mobile betrokkenheid geïntegreerd niet correct in SDK (Analytics).
- Bereiken niet correct is geïntegreerd in SDK (In App en afwezigheidsberichten App verplaatst).
- Het certificaat is verlopen of zijn deze onjuist PRODUCTCATALOGUS versus Ontwikkelaar (alleen iOS).
- GCM of ADM niet correct zijn geïntegreerd in SDK (Android alleen - Service specifieke verplaatst).
- Bijhouden niet correct geïntegreerd in SDK (installeren store bijhouden).
- Fikse locatie of GPS-locatie niet correct geïntegreerd in SDK (doelitems per geografische locatie).


**Zie ook:**

- [SDK-documentatie - integratie hulplijnen][Link 5] 
- [Gids voor - Push probleemoplossing][Link 23]

#### <a name="sdk-upgrade"></a>SDK-Upgrade

- Upgrade moeten uitvoeren SDK om oplossen van problemen met oudere versies van de SDK (vaak betrekking hebben op nieuwere versies van het apparaat OS).
- Alle vorige versies van uw app van uw apparaat verwijderen en opnieuw installeren van de nieuwste versie van uw app, wordt het opnieuw registreren uw apparaat-ID van de gebruikersinterface van Azure Mobile betrokkenheid om te bevestigen dat de nieuwste versie van uw app wordt gebruikt door uw apparaat.

**Zie ook:**

- [SDK-documentatie - releaseopmerkingen](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [SDK-documentatie - Upgrade hulplijnen](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>SDK andere

- Fouten in Manifest "AndroidManifest.xml" van de toepassing kunnen leiden tot Azure Mobile betrokkenheid niet te werken (alleen voor Android).
- Er is een algemene problemen met SDK integratie en API-gebruik Verwar de SDK en de API-sleutel.

**Zie ook:**

- [Concepten - woordenlijst][Link 6]

## <a name="advanced-coding-issues"></a>Geavanceerde kleurcodering aan problemen

### <a name="issue"></a>Probleem
-  Specifieke platform-code niet rechtstreeks zijn gerelateerd aan Azure Mobile betrokkenheid kan problemen veroorzaken op iOS, Android en Windows Phone.

### <a name="causes"></a>Oorzaken

- Veel geavanceerde coding problemen met Azure Mobile betrokkenheid worden veroorzaakt door onjuist geschreven platform specifieke code niet rechtstreeks zijn gerelateerd aan Azure Mobile betrokkenheid. U moet de documentatie van specifiek zijn voor het platform dat u voor naast Azure Mobile betrokkenheid documentatie (Android, iOS Web, Windows en Windows Phone ontwikkelt).
- Niet correct configureren 'categorieën', vergrendelt, kunnen koppelen vanuit een bericht naar een andere locatie binnen of buiten de app (alleen voor Android). 
- De instelling van "UIKit.framework" naar "optioneel" in uw iOS-code, ziet u een "Symbool niet gevonden fout" en/of loopt vast bij oudere iOS-apparaten (alleen iOS).
- Certificaten verlopen of niet correct met de Ontwikkelaar of productcatalogus versie van het certificaat, oorzaken push problemen (alleen iOS).
- Er gelden enkele beperkingen voor een platform dat Azure Mobile betrokkenheid niet beheren (bijvoorbeeld hoe het beheercentrum systeem werkt voor afmelden bij de app in Android en iOS verplaatst).
- Azure Mobile betrokkenheid publiceert een volledige lijst met de interne pakketten gebruikt door Azure Mobile betrokkenheid voor iOS en Android voor verwijzing. Houd er rekening mee dat sommige functies van Azure Mobile betrokkenheid specifiek voor het platform (Android, iOS Web, Windows en Windows Phone zijn).

### <a name="see-also"></a>Zie ook

 - [Gids voor - Push probleemoplossing][Link 23] 
 - [SDK-documentatie - releaseopmerkingen][Link 5]
 - [SDK-documentatie - Upgrade hulplijnen][Link 5]

## <a name="application-crashes"></a>Toepassing loopt

### <a name="issue"></a>Probleem
- Uw toepassing loopt vast op de eindgebruikers apparaat.

### <a name="causes"></a>Oorzaken

- Vastlopen informatie kan worden weergegeven in de *Analyse-gebruikersinterface* of de *Analytics-API*
- U kunt de ID van het apparaat van uw testapparaat zoeken en de dezelfde actie uitvoeren die uw toepassing loopt vast voor een eindgebruiker ter identificatie van de oorzaak van het vastlopen veroorzaakt.
- Bekende problemen met de SDK van Azure Mobile betrokkenheid die ervoor zorgen dat vastlopen worden soms door een upgrade naar de nieuwste versie van de SDK verholpen. Zorg ervoor dat de release-informatie over uw platform controleren als wordt onderzocht loopt.

### <a name="see-also"></a>Zie ook

- [SDK-documentatie - releaseopmerkingen][Link 5]
- [SDK-documentatie - Upgrade hulplijnen][Link 5]

## <a name="app-store-upload-failures"></a>App store upload fouten

### <a name="issue"></a>Probleem
- Fouten met betrekking tot de meest recente versie van uw app uploaden naar Apple, Google of de Windows-App store.

### <a name="causes"></a>Oorzaken

- App worden opgeslagen soms blok apps met bepaalde functies die zijn ingeschakeld (bijvoorbeeld de Apple Store voorkomt dat het gebruik van IDFV in apps in de store en de store GooglePlay Hiermee voorkomt u dat het delen van informatie over toepassingen tussen apps). 
- Zorg ervoor dat u de release-informatie over uw platform en de huidige versie van de SDK controleren als u problemen ondervindt bij het uploaden van een app naar de store.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
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
 
