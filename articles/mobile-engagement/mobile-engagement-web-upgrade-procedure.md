<properties
    pageTitle="Azure Mobile betrokkenheid Web SDK upgrade procedures | Microsoft Azure"
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


# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure Mobile betrokkenheid Web SDK upgrade procedures

Als u hebt al een eerdere versie van de Azure Mobile betrokkenheid Web SDK geïntegreerd in uw webtoepassing, moet u overwegen de volgende punten wanneer u een upgrade uitvoert van de SDK.

Als u meerdere versies van de mobiele betrokkenheid Web SDK overgeslagen, moet u mogelijk een aantal procedures uitvoeren tijdens de upgrade. Bijvoorbeeld als u vanuit 1.4.0 naar 1.6.0 migreert, eerst als volgt de procedures voor het bijwerken van 1.4.0 naar 1.5.0. Volg de procedures voor het bijwerken van 1.5.0 naar 1.6.0.

Welke versie u een upgrade vanaf uitvoeren een eerdere versie van het bestand azure-engagement.js vervangen door de nieuwste versie van het bestand.

## <a name="upgrade-from-121-to-200"></a>Vanuit 1.2.1 upgraden naar 2.0.0

In deze sectie wordt beschreven hoe de integratie van een mobiele betrokkenheid Web SDK migreren van de service Capptain, door Capptain SAS, worden aangeboden aan een betrokkenheid van Azure Mobile-app. Als u vanuit een eerdere versie migreert, neem contact op met de website Capptain om eerst naar 1.2.1 migreren en pas de volgende procedures uit.

Deze versie van de mobiele betrokkenheid Web SDK biedt geen ondersteuning voor Samsung slimme TV, Opera TV, webOS of de functie bereiken.

>[AZURE.IMPORTANT] Capptain en Azure Mobile betrokkenheid zijn niet dezelfde service. De volgende procedure wordt alleen het migreren van de client-app gemarkeerd. Migreren van de mobiele betrokkenheid Web SDK in de app migreert geen gegevens van een server Capptain op een mobiele betrokkenheid-server.

### <a name="javascript-files"></a>JavaScript-bestanden

Het bestand capptain-sdk.js vervangen door het bestand azure-engagement.js en past u uw invoer script dienovereenkomstig gewijzigd.

### <a name="remove-capptain-reach"></a>Capptain bereik verwijderen

Deze versie van de mobiele betrokkenheid Web SDK biedt geen ondersteuning van de functie bereiken. Als u Capptain bereik geïntegreerd in uw toepassing, moet u deze verwijderen.

Het importeren van CSS hebt bereikt verwijderen uit de pagina en verwijder de gerelateerde CSS-bestand (capptain-reach.css, al dan niet standaard).

Verwijderen van de volgende bronnen voor het bereik: de sluiten afbeelding (capptain-close.png, al dan niet standaard), plus het pictogram huisstijl (al dan niet standaard capptain-melding-pictogram).

Verwijder de gebruikersinterface hebt bereikt voor app-meldingen. De standaardindeling ziet er zo uit:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

De gebruikersinterface hebt bereikt voor tekst en web aankondigingen en polls verwijderen. De standaardindeling ziet er zo uit:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Verwijder de `reach` object uit uw configuratie, indien aanwezig. Deze ziet er zo uit:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Verwijder eventuele andere bereik-aanpassen, zoals categorieën.

### <a name="remove-deprecated-apis"></a>Vervallen API's verwijderen

Sommige API's Capptain zijn afgeschaft in de Mobile betrokkenheid Web SDK.

Verwijder eventuele oproepen naar de volgende API's: `agent.connect`, `agent.disconnect`, `agent.pause`, en `agent.sendMessageToDevice`.

Alle exemplaren van de volgende terugbellen verwijderen uit de configuratie van uw Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, en `onPushMessageReceived`.

### <a name="configuration"></a>Configuratie

Een verbindingsreeks Mobile betrokkenheid gebruikt voor het configureren van SDK id's, bijvoorbeeld de toepassings-id.

Vervang de toepassings-ID met de verbindingsreeks. Opmerking het globale object voor de configuratie SDK veranderende `capptain` naar `azureEngagement`.

Voor de migratie:

    window.capptain = {
      appId: ...,
      [...]
    };

Na de migratie:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

De verbindingsreeks voor uw toepassing wordt weergegeven in de Portal Azure.

### <a name="javascript-apis"></a>JavaScript-API 's

De globale JavaScript-object `window.capptain` heeft een andere naam gekregen `window.azureEngagement` , maar u kunt de `window.engagement` alias voor API-oproepen. U kunt de alias niet gebruiken om te definiëren van de configuratie SDK.

Bijvoorbeeld `capptain.deviceId` wordt `engagement.deviceId`, `capptain.agent.startActivity` wordt `engagement.agent.startActivity`, enzovoort.
