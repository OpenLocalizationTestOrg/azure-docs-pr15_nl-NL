<properties 
   pageTitle="Azure mobiele betrokkenheid de gids voor probleemoplossing - Service" 
   description="Handleidingen voor Azure mobiele betrokkenheid probleemoplossing" 
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

# <a name="troubleshooting-guide-for-service-issues"></a>Gids voor probleemoplossing voor serviceproblemen

Hier volgen mogelijke problemen optreden bij hoe Azure Mobile betrokkenheid wordt uitgevoerd.

## <a name="service-outages"></a>Service bijvoorbeeld

### <a name="issue"></a>Probleem
- Problemen die worden veroorzaakt door Azure Mobile betrokkenheid Service bijvoorbeeld worden weergegeven.

### <a name="causes"></a>Oorzaken
- Problemen die worden weergegeven om te worden veroorzaakt door Azure Mobile betrokkenheid Service bijvoorbeeld kunnen worden veroorzaakt door verschillende verschillende problemen:
    - Geïsoleerd problemen die oorspronkelijk worden weergegeven op alle Azure Mobile betrokkenheid systematische
    - Bekende problemen die worden veroorzaakt door de server, bijvoorbeeld (niet altijd ziet in de status van de server):
    - Worden planning vertragingen, doelitems fouten, Badge update problemen, statistieken stoppen verzamelt, loopt Push, API's niet meer werkt, nieuwe apps of gebruikers kan niet gemaakt, fouten in DNS, en time-out in de gebruikersinterface, API of Apps op een apparaat.
    - Cloud afhankelijkheid bijvoorbeeld [Servicestatus van Azure](http://status.azure.com/)
    - Push-meldingen Services (PNS) afhankelijkheid bijvoorbeeld
    - App Store bijvoorbeeld

1) Als u wilt testen om te zien als het probleem systematische is kunt u dezelfde functie uit een andere testen
   
   - Azure Mobile betrokkenheid geïntegreerd-toepassing
   - Testapparaat
   - Test apparaat OS versie
   - Campagne
   - Beheerdersaccount
   - Browser (IE, Firefox, Chrome, enz.)
   - Computer

2) Om te testen of het probleem geldt alleen voor de gebruikersinterface of de API's:

   - Test de dezelfde functie van de gebruikersinterface van Azure Mobile betrokkenheid zowel de Azure Mobile betrokkenheid API's.

3) Als het probleem met het netwerk van uw mobiele telefoon is testen:

   - Terwijl u verbinding met Internet via Wi-Fi en terwijl verbonden via het netwerk van de mobiele telefoon 3G testen.
   - Controleer of uw firewall is niet geblokkeerd door een van de Azure Mobile betrokkenheid IP-adressen of poorten.

4) Als het probleem met uw apparaat is testen:

   - Als uw apparaat kan verbinding maken met Azure Mobile betrokkenheid met een andere Azure Mobile betrokkenheid geïntegreerde app testen.
   - Test of u kunt gebeurtenissen, taken en loopt vanaf uw telefoon die kan worden weergegeven in de gebruikersinterface van Azure Mobile betrokkenheid genereren. 
   - Testen of u push-meldingen van de gebruikersinterface van Azure Mobile betrokkenheid naar uw apparaat op basis van het apparaat-id verzenden kunt 

5) Als het probleem met uw App is testen:

   - Installeren en testen van de toepassing een emulator in plaats van vanaf een fysiek apparaat:
   
6) Om te testen of het probleem is met upgrades voor het besturingssysteem voor eindgebruiker apparaten, waarvoor een upgrade SDK om op te lossen:

   - Test uw toepassing op verschillende apparaten met verschillende versies van het besturingssysteem.
   - Bevestig dat u de meest recente versie van de SDK worden gebruikt.
 
## <a name="connectivity-and-incorrect-information-issues"></a>Connectiviteit en onjuiste informatie over problemen

### <a name="issue"></a>Probleem
- Problemen met logboekregistratie in de gebruikersinterface van Azure Mobile betrokkenheid.
- Fouten met de Azure Mobile betrokkenheid API's.
- Problemen met het uploaden van App Info Tags via de API-apparaat.
- Problemen met Logboeken of de geëxporteerde gegevens van Azure Mobile betrokkenheid downloaden.
- Onjuiste informatie weergegeven in de gebruikersinterface van Azure Mobile betrokkenheid.
- Onjuiste informatie wordt weergegeven in Azure Mobile betrokkenheid Logboeken.

### <a name="causes"></a>Oorzaken
* Bevestig dat uw gebruikersaccount heeft onvoldoende machtigingen om uit te voeren van de taak.
* Controleer of het probleem niet beperkt tot één computer of het lokale netwerk is.
* Bevestig dat u bijvoorbeeld dat de betrokkenheid van Azure Mobile-service geen heeft gerapporteerd.
* Bevestig dat uw bestanden App Info Tag al deze regels volgen:
    - Gebruik alleen de UTF8 tekenset (de ANSI-tekenset wordt niet ondersteund).
    - Een komma gebruiken "," als scheidingsteken (u kunt een service om aan te vragen om te wijzigen van het .csv-scheidingsteken uit een komma openen "," naar een ander teken zoals een puntkomma ";").
    - Gebruik voor Booleaanse waarden worden alle kleine letters "true" en "false".
    - Gebruik een bestand dat kleiner is dan de maximale bestandsgrootte van 35MB.
 
