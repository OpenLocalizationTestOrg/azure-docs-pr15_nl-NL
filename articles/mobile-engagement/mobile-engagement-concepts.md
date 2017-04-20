<properties
    pageTitle="Mobile betrokkenheid concepten | Microsoft Azure"
    description="Azure Mobile betrokkenheid concepten"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-concepts"></a>Azure Mobile betrokkenheid concepten

Mobile betrokkenheid Hiermee definieert u een paar concepten algemene naar alle ondersteunde platforms. In dit artikel is een korte beschrijving van deze concepten.

In dit artikel is een goede begin als u nog niet eerder met mobiele betrokkenheid. Zorg ook Lees de documentatie die specifiek zijn voor het platform dat u gebruikt, zoals deze wordt de concepten beschreven in dit artikel met meer details en voorbeelden, alsmede mogelijke beperkingen verfijnen.

## <a name="devices-and-users"></a>Apparaten en gebruikers
Mobile betrokkenheid geeft aan wat gebruikers door te genereren van een unieke id voor elk apparaat. Deze id heet het apparaat-id (of `deviceid`). Zodanig dat alle toepassingen uitvoeren van hetzelfde apparaat delen dezelfde apparaat-id wordt gegenereerd.

Er wordt impliciet, betekent dit dat Mobile betrokkenheid één apparaat acht wilt toevoegen aan één gebruiker en dus gebruikers en apparaten equivalente concepten zijn.

## <a name="sessions-and-activities"></a>Sessies en activiteiten
Een sessie is één gebruik van de toepassing uitgevoerd door een gebruiker, wordt de gebruiker gestart vanaf het moment gebruiken, totdat de stopt van de gebruiker.

Een activiteit is één gebruik van een bepaald onderliggend deel van de toepassing uitgevoerd door één gebruiker (het is gewoonlijk een scherm, maar het iets geschikt is voor de toepassing kan zijn).

Een gebruiker kan slechts één activiteit tegelijk uitvoeren.

Een activiteit wordt aangegeven door een naam (beperkt tot 64 tekens) en kunt u kunt ook enkele extra gegevens insluiten (in de limiet van 1024 bytes).

Sessies worden automatisch berekend van de volgorde van activiteiten door gebruikers uitgevoerd. Een sessie begint wanneer de gebruiker zijn eerste activiteit begint en stopt wanneer hij zijn laatste activiteit is voltooid. Dit betekent dat een sessie niet hoeft expliciet worden gestart of gestopt. In plaats daarvan zijn activiteiten expliciet gestart of gestopt. Als er geen activiteit wordt gemeld, wordt geen sessie vermeld.

## <a name="events"></a>Gebeurtenissen
Gebeurtenissen worden gebruikt om direct acties (zoals knop ingedrukt of artikelen gelezen door gebruikers).

Een gebeurtenis kan worden gerelateerd aan de huidige sessie, aan een lopend project of dit kan een gebeurtenis zelfstandige.

Een gebeurtenis wordt aangegeven door een naam (beperkt tot 64 tekens) en kunt u kunt ook enkele extra gegevens insluiten (in de limiet van 1024 bytes).

## <a name="error"></a>Fout
Fouten worden gebruikt om problemen correct gedetecteerd door de toepassing (zoals onjuiste gebruikersacties, en API gesprek mislukte).

Een fout kan zijn gerelateerd aan de huidige sessie, aan een lopend project of dit kan een zelfstandig product is opgetreden.

Een fout wordt aangegeven door een naam (beperkt tot 64 tekens) en kunt u kunt ook enkele extra gegevens insluiten (in de limiet van 1024 bytes).

## <a name="job"></a>Taak
Taken worden gebruikt om acties met een duur (zoals de duur van API-oproepen, weergeven tijd van advertenties, duur van achtergrondtaken of de duur van gebruikersacties).

Een taak is niet zijn gerelateerd aan een sessie, omdat een taak kan worden uitgevoerd op de achtergrond, zonder tussenkomst van de gebruiker.

Een taak wordt aangegeven door een naam (beperkt tot 64 tekens) en kunt u kunt ook enkele extra gegevens insluiten (in de limiet van 1024 bytes).

## <a name="crash"></a>Loopt vast
Loopt worden automatisch uitgegeven door de mobiele betrokkenheid SDK toepassingsfouten waar problemen niet wordt gedetecteerd door de toepassing u vastlopen rapporteren.

## <a name="application-information"></a>Informatie over toepassingen
Toepassingsinformatie (of app info) wordt gebruikt om te taggen van gebruikers, dat wil zeggen om bepaalde gegevens aan de gebruikers van een toepassing (dit is vergelijkbaar met web cookies, behalve dat app info is opgeslagen op de server op het platform Azure Mobile betrokkenheid) te koppelen.

App info kan worden geregistreerd met behulp van de mobiele betrokkenheid SDK API of met behulp van het platform betrokkenheid van mobiele apparaat API.

App-info is de combinatie van een sleutel/waarde die is gekoppeld aan een apparaat. De sleutel is de naam van de app-informatie (maximaal 64 ASCII letters [a-zA-Z], [0-9] getallen en onderstrepingstekens (_)). De waarden (beperkt tot 1024 tekens) zijn een tekenreeks, geheel getal, datum (jjjj-MM-dd) of Booleaanse waarde (true of false).

Een willekeurig aantal app info kan worden gekoppeld aan een apparaat, de limieten niet overschrijden die is gedefinieerd door de mobiele betrokkenheid prijzen voorwaarden. Voor een bepaalde toets, Mobile betrokkenheid alleen houdt van de meest recente waarde set (geen geschiedenis). Instellen of wijzigen van de waarde van een app-info dwingt Mobile betrokkenheid publiek set van de criteria op deze app info (indien aanwezig) wat betekent dat info app kan worden gebruikt om te activeren realtime gereedschap duwt opnieuw wordt berekend.

## <a name="extra-data"></a>Extra gegevens
Extra gegevens (of extra's) is ook willekeurige gegevens die kan worden gekoppeld aan gebeurtenissen, fouten, activiteiten en taken.

Extra's op dezelfde manier zijn gestructureerd in JSON-objecten: deze worden aangebracht van een structuur van de sleutel/waardeparen. Toetsen zijn beperkt tot 64 ASCII-letters [a-zA-Z], [0-9] getallen en onderstrepingstekens (_)) en de totale grootte van extra's is beperkt tot 1024 tekens (één keer gecodeerd in JSON door de mobiele betrokkenheid SDK).

De volledige structuur van de sleutel/waardeparen wordt opgeslagen als een object JSON. Echter alleen het eerste niveau van sleutels/waarden opgesplitste rechtstreeks toegankelijk zijn voor bepaalde geavanceerde functies, zoals segmenten is (bijvoorbeeld eenvoudig kunt u een segment genoemd 'SciFi ventilatoren' die is gemaakt van alle gebruikers ten minste 10 keer de gebeurtenis met de naam 'content_viewed' hebt verzonden met de extra belangrijke "content_type" ingesteld op de waarde 'scifi' in de laatste maand). Het is dus ten zeerste aanbevolen om te verzenden alleen extra's die zijn aangebracht van eenvoudige lijsten met sleutel/waardeparen met scalaire waarden (bijvoorbeeld tekenreeksen, datums, gehele getallen of Booleaanse waarde).

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van de Windows universele SDK voor Azure Mobile betrokkenheid](mobile-engagement-windows-store-sdk-overview.md)
- [Overzicht van de Windows Phone Silverlight SDK voor Azure Mobile betrokkenheid](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS SDK voor Azure Mobile betrokkenheid](mobile-engagement-ios-sdk-overview.md)
- [Android SDK voor Azure mobiele betrokkenheid](mobile-engagement-android-sdk-overview.md)
