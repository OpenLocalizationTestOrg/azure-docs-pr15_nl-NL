<properties 
   pageTitle="Azure mobiele betrokkenheid de gids voor probleemoplossing - Push/bereik" 
   description="Problemen met gebruiker interactie en melding oplossen in Azure Mobile betrokkenheid" 
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

# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>De gids voor voor probleemoplossing Push en problemen hebt bereikt

Hier volgen mogelijke problemen optreden bij hoe Azure Mobile betrokkenheid informatie aan uw gebruikers verzendt.
 
## <a name="push-failures"></a>Push-fouten

### <a name="issue"></a>Probleem
- Gereedschap duwt werken niet (in de app, uitzoomen app of beide).

### <a name="causes"></a>Oorzaken
- Vaak een push-fout wordt aangegeven dat Azure Mobile betrokkenheid, bereik of een andere geavanceerde functie van Azure Mobile betrokkenheid is niet juist geïntegreerd of dat een upgrade in de SDK is vereist voor het oplossen van een bekend probleem met een nieuw OS of apparaat-platform.
- Alleen een push In App en alleen een afwezigheidsbericht App push om te bepalen of een ander nummer een probleem In App of afwezigheidsberichten App testen.
- Test u uit de gebruikersinterface en de API als stap bij probleemoplossing om te zien welke aanvullende foutgegevens zijn beschikbaar beide locaties.
- Afmelden bij de App werken gereedschap duwt alleen als zowel Azure Mobile betrokkenheid en het bereik zijn geïntegreerd in de SDK.
- Gereedschap duwt werkt niet als certificaten ongeldige of PRODUCTCATALOGUS versus Ontwikkelaar correct gebruiken (alleen iOS). (**Notitie:** "af bij de app" push-meldingen kunnen niet worden bezorgd bij iOS, als u zowel de ontwikkeling (Ontwikkelaar) en Productieversies (PRODUCTCATALOGUS) van uw toepassing is op hetzelfde apparaat is geïnstalleerd, omdat het beveiligingstoken dat is gekoppeld aan uw certificaat kunnen worden ongeldig door Apple. U lost dit probleem op door de Ontwikkelaar en PRODUCTCATALOGUS versies van de toepassing verwijderen en opnieuw installeren alleen de ene versie op uw apparaat.)
- Afmelden bij de App push aantallen worden anders in de andere platforms (iOS bevat minder dan Android als systeemeigen gereedschap duwt zijn uitgeschakeld op een apparaat, de API bieden mogelijk meer informatie dan de gebruikersinterface op push stat).
- Afmelden bij de App kunnen gereedschap duwt worden geblokkeerd door klanten op OS niveau (iOS en Android).
- Afmelden bij de App worden gereedschap duwt weergegeven als in de gebruikersinterface van Azure Mobile betrokkenheid uitgeschakeld als ze worden niet correct zijn geïntegreerd, maar stilte uit de API kunnen mislukken.
- In de App werken gereedschap duwt alleen als zowel Azure Mobile betrokkenheid en het bereik zijn geïntegreerd in de SDK.
- GCM en ADM gereedschap duwt werken alleen als Azure Mobile betrokkenheid en de specifieke server zijn geïntegreerd in de SDK (alleen voor Android).
- In de App en afwezigheidsberichten App moeten gereedschap duwt worden getest afzonderlijk als een probleem met de Push- of bereik is.
- In de App gereedschap duwt vereisen dat de app openen te ontvangen.
- In de App zijn gereedschap duwt vaak instelling door een opt-in- of opt-out app info tag worden gefilterd.
- Als u een aangepaste categorie gebruiken in het bereik in het app-meldingen wilt weergeven, moet u de juiste levenscyclus van de melding volgen, anders de melding kan niet worden gewist wanneer de gebruiker genegeerd.
- Als u een campagne zonder einddatum starten en een apparaat de in app ontvangt melding maar doet niet weergeven deze nog, de gebruikers hebben nog steeds ontvangen de melding de volgende keer dat ze zich aanmelden bij de app, zelfs als u handmatig de campagne beëindigen.
- Bevestig dat u echt de Push-API gebruiken in plaats van de API hebt bereikt wilt (omdat de API hebt bereikt vaker gebruikt wordt) en dat u niet de parameters "nettolading" en "kennisgever" zijn verwarrend voor problemen met de API Push.
- Test uw campagne push met zowel een apparaat verbonden via Wi-Fi en 3G de netwerkverbinding als een mogelijke bron van problemen.

## <a name="push-testing"></a>Push testen

### <a name="issue"></a>Probleem
- Gereedschap duwt kunnen worden verzonden naar een specifieke apparaat op basis van een apparaat-ID.

### <a name="causes"></a>Oorzaken

- Test-apparaten zijn ingesteld voor elk platform anders, maar de oorzaak van een gebeurtenis in uw app op een testapparaat en zoekt u uw apparaat-ID in de portal moeten werken om het zoeken van uw apparaat-ID voor alle platforms.
- Test-apparaten werken anders met IDFA versus IDFV (alleen iOS).


## <a name="push-customization"></a>Push-aanpassing

### <a name="issue"></a>Probleem
- Push inhoud item niet geschikt zijn voor geavanceerde (badge, bellen, Trillen, afbeelding, enzovoort).
- Koppelingen van gereedschap duwt werken niet (niet-app in naar een website, naar een locatie in de app-app).
- Push-statistieken weergeven dat een push niet is verzonden naar net zoveel mensen zoals verwacht (te veel of te weinig).
- Push gedupliceerd en tweemaal ontvangen.
- Kan geen testapparaat niet registreren voor Azure Mobile betrokkenheid worden (met uw eigen productcatalogus of Ontwikkelaar-app).

### <a name="causes"></a>Oorzaken

- Vereist om een koppeling naar een specifieke locatie in de app 'categorieën' (alleen voor Android).
- Uitgebreide koppelen schema om gebruikers te leiden naar een andere locatie nadat u hebt geklikt, een push-bericht moeten zijn gemaakt en beheerd door uw toepassing en het besturingssysteem van het apparaat niet door Mobile betrokkenheid rechtstreeks. (**Notitie:** af bij app meldingen kunnen geen koppeling rechtstreeks naar op app-locaties met iOS zoals dat bij Android.)
- Van de externe afbeeldingsservers moeten kunnen gebruiken HTTP "GET" en "HEAD" voor totaalbeeld verplaatst om te werken (alleen voor Android).
- U kunt in code, de betrokkenheid van Azure Mobile-agent uitschakelen wanneer het toetsenbord wordt geopend en uw code die de betrokkenheid van Azure Mobile-agent opnieuw te activeren wanneer het toetsenbord hebt gesloten, zodat het toetsenbord geen invloed op het uiterlijk van uw melding (alleen iOS) hebben.
- Sommige items werken niet in de test simulaties, maar alleen reële campagnes (badge, bellen, Trillen, afbeelding, enzovoort).
- Geen servergegevens kant is vastgelegd wanneer u de knop gereedschap duwt "Test". Gegevens alleen geregistreerd voor reële push campagnes.
- Om u te helpen uw probleem isoleren, problemen met: testen, simuleren, en een reële campagne aangezien deze allemaal iets anders werken.
- De tijdsduur die uw "in app" en "elk gewenst moment" campagnes zijn gepland om uit te voeren kunt bezorging getallen effect, omdat een campagne alleen worden bezorgd bij de gebruikers die 'in de app zijn"terwijl u de campagne wordt uitgevoerd (en gebruikers die hun apparaatinstellingen instellen"af bij de app"-meldingen wilt ontvangen hebben).
- Het verschil is tussen hoe Android iOS pagina's uitgevouwen afmelden bij de app-meldingen lastig rechtstreeks push statistieken vergelijken tussen de Android en iOS-versie van uw toepassing. Android vindt u meer OS niveau melding informatie dan iOS. Android-rapporten wanneer een systeemeigen melding wordt ontvangen, hebt geklikt, of verwijderd in het midden van de melding, maar iOS rapporteert niet deze informatie, tenzij de melding wordt geklikt. 
- De belangrijkste reden dat "gedistribueerde" getallen anders zijn dan andere dan "bezorgd" nummers van campagnes bereiken is dat 'in de app"en"af bij de app"meldingen worden geteld anders. "In app" meldingen worden afgehandeld door Mobile betrokkenheid, maar "af bij de app" meldingen worden afgehandeld door het beheercentrum melding in het besturingssysteem van uw apparaat.

## <a name="push-targeting"></a>Push-doelgroepen

### <a name="issue"></a>Probleem
- Ingebouwd doelgroepen werkt niet zoals verwacht.
- App Info Tag doelgroepen werkt niet zoals verwacht.
- Geografische locatie hebt samengesteld werkt niet zoals verwacht.
- Opties voor taal werken niet zoals verwacht.

### <a name="causes"></a>Oorzaken

- Zorg ervoor dat u de app info tags via de gebruikersinterface van Azure Mobile betrokkenheid of API hebt geüpload.
- De snelheid van de push of push quotum op het toepassingsniveau van de beperken of het publiek op het niveau van de campagne beperken kunt voorkomen dat een persoon ontvangt een specifieke push zelfs als ze aan uw andere doelgroepen criteria voldoen. 
- Voor het instellen van een 'taal' is anders dan hebt samengesteld op basis van land of de landinstelling, dat wil ook verschilt hebt samengesteld op basis van geografische locatie zeggen die is gebaseerd op een telefoon locatie of GPS-locatie.
- Het bericht in de "standaardtaal" is verzonden naar een klant die geen diens apparaat instellen op een van de alternatieve talen die u opgeeft.


## <a name="push-scheduling"></a>Push-plannen

### <a name="issue"></a>Probleem
- Push planning werkt niet zoals verwacht (verzonden te vroeg of vertraagde).

### <a name="causes"></a>Oorzaken

- Tijdzones kunt problemen met de planning, met name bij gebruik van de eindgebruiker-tijdzone.
- Geavanceerde push-functies, kunnen dit gereedschap duwt.
- Aangezien Azure Mobile betrokkenheid u gegevens uit het telefoon-realtime aanvragen moet mogelijk voordat u een push verzendt hebt samengesteld op basis van de telefoon instellingen (in plaats van de App Info Tags) mag vertragen worden verplaatst.
- Campagnes gemaakt zonder een einddatum de push lokaal opslaan op het apparaat en dit de volgende keer dat de app wordt geopend, zelfs als de campagne handmatig wordt beëindigd weergeven.
- Meer dan één campagne starten op hetzelfde moment kunt langer duren om te scannen van uw gebruiker base (kiest u alleen start één campagne tegelijk met maximaal vier, ook target alleen aan uw actieve gebruikers zodat oude gebruikers niet hoeft te worden gescand).
- Als u de optie 'Negeren publiek, push wordt verzonden naar gebruikers via de API' in de sectie "Campagne" van de campagne van een bereik gebruikt, de campagne wordt niet automatisch verzenden, moet u handmatig verzenden via de API hebt bereikt.
- Als u een aangepaste categorie gebruiken in het bereik in het app-meldingen wilt weergeven, moet u de juiste levenscyclus van een melding volgen, anders de melding kan niet worden gewist wanneer de gebruiker genegeerd.

 
