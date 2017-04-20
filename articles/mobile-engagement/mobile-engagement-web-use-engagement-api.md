<properties
    pageTitle="Azure mobiele betrokkenheid Web API's SDK | Microsoft Azure"
    description="De meest recente updates en procedures voor het Web SDK voor Azure Mobile betrokkenheid"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>De Azure Mobile betrokkenheid-API gebruiken in een webtoepassing

In dit document is een aanvulling op het document dat wordt uitgelegd hoe u kunt [integreren Mobile betrokkenheid in een webtoepassing](mobile-engagement-web-integrate-engagement.md). Deze biedt uitgebreide informatie over het gebruik van de Azure Mobile betrokkenheid API om uw toepassing statistieken.

De mobiele betrokkenheid API is verstrekt door de `engagement.agent` object. De standaardinstelling Azure Mobile betrokkenheid Web SDK alias is `engagement`. U kunt deze alias uit de configuratie SDK opnieuw definiëren.

## <a name="mobile-engagement-concepts"></a>Mobile betrokkenheid concepten

U kunt de volgende onderdelen verfijnen gemeenschappelijke [Mobile betrokkenheid concepten](mobile-engagement-concepts.md) voor het webplatform.

### <a name="session-and-activity"></a>`Session`en`Activity`

Als de gebruiker niet actief geweest gedurende meer dan een paar seconden tussen twee activiteiten blijft, wordt de volgorde van de gebruiker van activiteiten splitsen in twee afzonderlijke sessies. Deze enkele seconden worden de sessietime genoemd.

Als uw webtoepassing het einde van de gebruiker activiteiten op zichzelf niet declareren (door de ondersteuning voor de `engagement.agent.endActivity` functie), de betrokkenheid van de Mobile-server automatisch verloopt de sessie van de gebruiker binnen drie minuten nadat de toepassing pagina is gesloten. Dit is de sessie servertime genoemd.

### `Crash`

Geautomatiseerde rapporten van onbekende JavaScript-uitzonderingen worden niet al dan niet standaard gemaakt. U kunt echter loopt melden handmatig met behulp van de `sendCrash` , functie (Zie het gedeelte over reporting loopt).

## <a name="reporting-activities"></a>Rapportage van activiteiten

Rapportage van de gebruikersactiviteit van de bevat wanneer een gebruiker een nieuwe activiteit wordt gestart en wanneer de gebruiker de huidige activiteit eindigt.

### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit

    engagement.agent.startActivity("MyUserActivity");

U moet bellen `startActivity()` elke activiteit van de gebruiker tijd verandert. De eerste aanroep voor deze functie wordt een nieuwe gebruikerssessie gestart.

### <a name="user-ends-the-current-activity"></a>Gebruiker beëindigt u de huidige activiteit

    engagement.agent.endActivity();

U moet bellen `endActivity()` ten minste eenmaal wanneer de gebruiker de laatste activiteit eindigt. Hiermee wordt de mobiele betrokkenheid Web SDK geïnformeerd dat de gebruiker momenteel niet actief is en dat de gebruikerssessie moet worden gesloten nadat de sessietime verloopt. Als u belt `startActivity()` voordat de sessietime verloopt, de sessie gewoon wordt hervat.

Omdat er geen betrouwbare aanroep voor wanneer de navigator-venster wordt gesloten, is het vaak of moeilijk te onderschept het einde van de activiteiten van de gebruikers in een webomgeving. Daarom heb ik de betrokkenheid van de Mobile-server automatisch verloopt de sessie van de gebruiker binnen drie minuten nadat de toepassing pagina is gesloten.

## <a name="reporting-events"></a>Rapportage van gebeurtenissen

Rapporten over alle gebeurtenissen behandelt sessiegebeurtenissen en zelfstandige.

### <a name="session-events"></a>Sessiegebeurtenissen

Sessiegebeurtenissen worden meestal gebruikt om de acties uitgevoerd door een gebruiker tijdens de sessie.

**Voorbeeld zonder extra gegevens:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Voorbeeld met extra gegevens:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen

In tegenstelling tot sessiegebeurtenissen, kunt zelfstandige plaats buiten de context van een sessie.

Daarvoor gebruik ``engagement.agent.sendEvent`` in plaats van ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Rapportage van fouten

Gerapporteerd op fouten, behandelt fouten in sessie en zelfstandige.

### <a name="session-errors"></a>Sessie fouten

Sessie fouten worden meestal gebruikt om de fouten die van invloed op de gebruiker tijdens de sessie zijn.

**Voorbeeld zonder extra gegevens:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Voorbeeld met extra gegevens:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Zelfstandige versie van fouten

In tegenstelling tot sessie fouten, kunnen zich voordoen zelfstandige fouten buiten de context van een sessie.

Daarvoor gebruik `engagement.agent.sendError` in plaats van `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Taken rapporteren

Gerapporteerd op taken voorbladen rapportage van fouten en gebeurtenissen die tijdens een taak optreden en rapportage loopt.

**Voorbeeld:**

Als u een AJAX-verzoek om te controleren wilt, gebruikt u het volgende:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Rapportage van fouten tijdens een taak

Fouten kunnen zijn gerelateerd aan een lopend project in plaats van naar de huidige gebruikerssessie van de.

**Voorbeeld:**

Als u rapporteren van een fout wilt als een AJAX-aanvraag mislukt:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Rapportage van gebeurtenissen tijdens een taak

Gebeurtenissen kunnen worden gerelateerd aan een actieve taak in plaats van naar de huidige gebruikerssessie, dank aan de `engagement.agent.sendJobEvent` functie.

Deze functie werkt precies zoals `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Rapportage van loopt

Gebruik de `sendCrash` functie aan rapport handmatig loopt vast.

De `crashid` een tekenreeks die aangeeft van het type van het vastlopen is.
De `crash` van de argumenten is meestal de stapel tracering van het vastlopen als een tekenreeks.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Extra parameters

U kunt willekeurige gegevens toevoegen aan een gebeurtenis, fout, activiteit of taak.

Een willekeurig object JSON (maar niet een matrix of primitief type), kunnen de gegevens zijn.

**Voorbeeld:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Limieten

Limieten die extra parameters van toepassing zijn op het gebied van reguliere expressies voor toetsen, waardetypen en grootte.

#### <a name="keys"></a>Toetsen

Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

    ^[a-zA-Z][a-zA-Z_0-9]*

Dit betekent dat sleutels moeten beginnen met ten minste één letter, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="values"></a>Waarden

Waarden zijn beperkt tot tekenreeks, getal en Booleaanse typen.

#### <a name="size"></a>Grootte

Extra's zijn beperkt tot 1024 tekens per gesprek (nadat u de mobiele betrokkenheid Web SDK gecodeerd deze JSON).

## <a name="reporting-application-information"></a>Informatie over toepassingen melden

U kunt handmatig melden voor het bijhouden van informatie (of andere informatie toepassingsspecifieke) met behulp van de `sendAppInfo()` functie.

Houd er rekening mee dat deze informatie stapsgewijs kan worden verzonden. Alleen de meest recente waarde voor een bepaalde toets wordt voor een specifieke apparaat bewaard.

Als de gebeurtenis extra's, kunt u een willekeurig object JSON abstracte toepassingsinformatie. Houd er rekening mee dat matrices of onderliggend objecten worden behandeld als platte tekenreeksen (via JSON serialisatie).

**Voorbeeld:**

Hier volgt een codevoorbeeld voor het verzenden van de gebruiker geslacht en Geboortedatum:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Limieten

Beperkingen voor die toepassingsgegevens van toepassing zijn op het gebied van reguliere expressies voor toetsen en grootte.

#### <a name="keys"></a>Toetsen

Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

    ^[a-zA-Z][a-zA-Z_0-9]*

Dit betekent dat sleutels moeten beginnen met ten minste één letter, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Toepassingsinformatie is beperkt tot 1024 tekens per gesprek (nadat u de mobiele betrokkenheid Web SDK gecodeerd deze JSON).

In het voorgaande voorbeeld worden de JSON verzonden naar de server 44 tekens bevatten:

    {"birthdate":"1983-12-07","gender":"female"}
