<properties 
    pageTitle="Een API voor logica-Apps maken" 
    description="Een aangepaste API voor gebruik met logica Apps maken" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>Een aangepaste API voor gebruik met logica Apps maken

Als u uitbreiden, de logica Apps-platform wilt, zijn er veel manieren waarop die u kunt inbellen bij de API's of systemen die zijn niet beschikbaar als een van onze veel out-van-het-box-verbindingslijnen.  Als u een van deze manieren om een API-App die u vanuit binnen een werkstroom logica-App bellen kunt maken.

## <a name="helpful-tools"></a>Handige hulpmiddelen

Voor API's werken het beste met logica Apps, wordt u aangeraden een [swagger](http://swagger.io) document waarin de ondersteunde bewerkingen en parameters voor uw API genereren.  Zijn er veel bibliotheken (zoals [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)) die automatisch de swagger voor u gegenereerd.  U kunt ook [TRex](https://github.com/nihaue/TRex) gebruiken om te helpen aantekeningen toevoegen aan de swagger werken ook met de logica-Apps (weergeven namen, eigenschap-gegevenstypen, enz.).  Voor enkele voorbeelden van API-Apps die is ingebouwd voor logica Apps, zorg ervoor dat u Bekijk onze [GitHub opslagplaats](http://github.com/logicappsio) of [blog](http://aka.ms/logicappsblog).

## <a name="actions"></a>Acties

De eenvoudige actie voor een App logica is een controller die wordt een HTTP-verzoek accepteren en terug te keren een reactie (meestal 200).  Er zijn echter verschillende patronen die u volgen kunt om acties op basis van uw behoeften uitbreiden.

Standaard wordt de logica App-engine time-out voor een aanvraag na 1 minuut.  U kunt wel uw API uitvoeren op de acties die langer duren en beschikken over de engine wachten is voltooid, volgens een asynchrone of webhook patroon hieronder beschreven.

Voor standaardacties, moet u gewoon een HTTP-verzoek methode schrijven in uw API die worden weergegeven via swagger.  Hier ziet u voorbeelden van API-apps die met logica Apps in onze [GitHub opslagplaats werken](https://github.com/logicappsio).  Hieronder vindt u manieren voor het uitvoeren van algemene patronen met een aangepaste verbindingslijn.

### <a name="long-running-actions---async-pattern"></a>Langdurige acties - asynchrone patroon

Wanneer een lange stap of taak uitvoert, is het eerste wat dat u moet doen Zorg ervoor dat de engine weet dat u dat nog niet hebt timed-out. U moet ook communiceren met de engine hoe deze worden weten wanneer u klaar met de taak bent en ten slotte, moet u relevante gegevens terug naar de engine, zodat u kunt doorgaan met de werkstroom. U kunt die uitvoeren via een API volgens de onderstaande stroom. Deze stappen zijn van het punt-van-weergave van de aangepaste API:

1. Wanneer een aanvraag is ontvangen, retourneren direct een reactie (voordat werk wordt uitgevoerd). Dit antwoord is een `202 ACCEPTED` antwoord, zodat de engine weet u hebt u de gegevens, de nettolading geaccepteerd en nu worden verwerkt. Het 202 antwoord moet de volgende koppen bevatten: 
 * `location`koptekst (vereist): dit is een absoluut pad naar de URL logica Apps kunt gebruiken om te controleren van de status van de taak.
 * `retry-after`(optioneel is, wordt standaard tot en met 20 voor acties). Dit is het aantal seconden die de engine er wachten moet voordat het polling-de URL van de koptekst locatie om status te controleren.

2. Wanneer de status van een taak is ingeschakeld, kunt u de volgende controles uitvoeren: 
 * Als de taak wordt uitgevoerd: retourneren een `200 OK` reactie, met de nettolading antwoord.
 * Als de taak is nog steeds verwerkt: keren andere `202 ACCEPTED` reactie, met dezelfde kopteksten als het oorspronkelijke antwoord

Dit patroon kunt u erg lang taken in een mailthread van uw aangepaste API, maar een actieve verbinding leven houden met de logica Apps-engine zodat deze geen time-out of gaat u verder voordat werk is voltooid. Wanneer u dit in uw App logica toevoegt, is het belangrijk te weten dat u hoeft niet iets in de definitie voor de App logica wilt blijven poll uitvoeren onder en controleer de status van uw doen. Zodra de engine 202 GEACCEPTEERDE reactie met de kop van een geldige locatie ziet, wordt deze voldoen aan het patroon asynchrone en gaat u verder met het poll uitvoeren onder de kop van de locatie totdat een niet-202 wordt geretourneerd.

Ziet u een voorbeeld van dit patroon in GitHub [hier](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Webhook acties

U kunt tijdens uw werkstroom, de App logica wacht totdat een 'terugbellen' om verder te hebben.  Deze terugbellen wordt geleverd in de vorm van een HTTP-POST.  Als u wilt deze patroon implementeren, moet u twee eindpunten op de controller: abonneren en afmelden.

Klik op 'abonneren', de logica-App maakt en een URL voor terugbellen die uw API kunt opslaan en terugbellen met gereed voor als een HTTP-POST registreren.  Alle inhoud/kopteksten worden doorgegeven naar de App logica en binnen de rest van de werkstroom kan worden gebruikt.  De logica App-engine belt de komma abonneren op worden uitgevoerd zodra deze die stap raakt.

Als de klaar is geannuleerd, wordt de logica App-engine bellen naar het eindpunt 'afmelden'.  Uw API kunt vervolgens de URL voor terugbellen unregister naar wens.

Momenteel wordt niet de logica App Designer ondersteund op een eindpunt webhook tot en met swagger, ontdekken, dus als u wilt gebruiken van dit type actie moet u de actie "Webhook" toevoegen en geef de URL, koppen en hoofdtekst van uw aanvraag kunt invullen.  U kunt de `@listCallbackUrl()` werkstroom-functie in een van deze velden naar wens om door te geven in de URL voor terugbellen.

Ziet u een voorbeeld van een patroon webhook in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>Triggers

U kunt uw aangepaste API act hebben als een trigger aan een App logica naast acties.  Er zijn twee patronen die u volgen kunt onder aan het activeren van een App logica:

### <a name="polling-triggers"></a>Peiling-Triggers

Peiling triggers fungeren vergelijkbaar met de bovenstaande lang uitgevoerd asynchrone-acties.  De logica App-engine belt het eindpunt trigger nadat een bepaalde periode is verstreken (afhankelijk van de SKU, 15 seconden voor Premium, 1 minuut voor standaard, en 1 uur voor gratis).

Als er geen gegevens beschikbaar is, de trigger geeft als resultaat een `202 ACCEPTED` antwoord, met een `location` en `retry-after` koptekst.  Echter voor triggers het wordt aanbevolen de `location` koptekst bevat een queryparameter van `triggerState`.  Dit is sommige id voor uw API weten wanneer de laatste keer dat de logica-App wordt gestart.  Als er gegevens beschikbaar is, de trigger geeft als resultaat een `200 OK` reactie met de inhoud nettolading.  Hiermee wordt de App logica gestart.

Bijvoorbeeld als ik is polling als een bestand beschikbaar was wilt zien, kan u maakt een polling-trigger die als volgt te werk zou:

* Als u een nieuw vergaderverzoek met geen triggerState is ontvangen de API retourneert een `202 ACCEPTED` met een `location` koptekst met een triggerState van de huidige tijd en een `retry-after` van 15.
* Als u een nieuw vergaderverzoek met een triggerState hebt ontvangen:
 * Controleer als alle bestanden zijn toegevoegd na de triggerState DateTime. 
  * Als er 1 bestand, retourneren een `200 OK` reactie met de inhoud nettolading, verhoogd de triggerState naar de DateTime van het bestand ik als resultaat gegeven en stel de `retry-after` op 15.
  * Als er meerdere bestanden, kan ik expressies wordt 1 geretourneerd tegelijk met een `200 OK`, verhoogd mijn triggerState in de `location` kop- en set `retry-after` 0.  De engine er zijn meer gegevens beschikbaar en wordt automatisch direct aangevraagd op weet u kunt de `location` header die is opgegeven.
  * Als er geen bestanden beschikbaar zijn, retourneert een `202 ACCEPTED` antwoord en laat de `location` triggerState hetzelfde.  Stel `retry-after` op 15.

Ziet u een voorbeeld van een peiling-trigger in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Webhook-Triggers

Webhook triggers fungeren net zoals Webhook acties hierboven.  De logica App-engine belt het eindpunt 'abonneren' wanneer een trigger webhook wordt toegevoegd en opgeslagen.  Uw API kunt de URL webhook registreren en noem deze via HTTP POST wanneer gegevens beschikbaar is.  De inhoud nettolading en kopteksten worden doorgegeven naar de logica-App uitvoeren.

Als een trigger webhook ooit wordt verwijderd (de logica-App geheel of alleen de trigger webhook), de engine wordt bellen naar de URL 'afmelden' waar uw API kunt unregister de URL voor terugbellen en alle processen stoppen indien nodig.

Momenteel wordt niet de logica App Designer ondersteund op een trigger webhook tot en met swagger, ontdekken, dus als u wilt gebruiken van dit type actie moet u de trigger 'Webhook' toevoegen en geef de URL, koppen en hoofdtekst van uw aanvraag kunt invullen.  U kunt de `@listCallbackUrl()` werkstroom-functie in een van deze velden naar wens om door te geven in de URL voor terugbellen.

Ziet u een voorbeeld van een trigger webhook in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)