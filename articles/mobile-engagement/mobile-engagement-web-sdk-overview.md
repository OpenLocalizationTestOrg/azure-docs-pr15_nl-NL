<properties
    pageTitle="Overzicht van Azure mobiele betrokkenheid Web SDK | Microsoft Azure"
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
    ms.date="10/18/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk"></a>Azure mobiele betrokkenheid Web SDK

Begin hier voor de informatie over het integreren van Azure Mobile betrokkenheid in een WebApp. Als u zelf proberen wilt voordat u aan de slag met uw eigen web-app, raadpleegt u onze [15 minuten zelfstudie](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Integratie van procedures
1. Informatie over [het integreren van Mobile betrokkenheid in uw web-app](mobile-engagement-web-integrate-engagement.md).

2. Informatie over [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in uw web-app](mobile-engagement-web-use-engagement-api.md)voor de tag abonnement implementatie.

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="202-10182016"></a>2.0.2 (10/18/2016)

-   Vaste vastlopen op InPrivate navigatie (Safari).
-   Vaste vastlopen in browsers met cookies uitgeschakeld.

Voor alle versies, raadpleegt u de [volledige releaseopmerkingen](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>De upgrade procedures

### <a name="upgrade-from-121-to-200"></a>Vanuit 1.2.1 upgraden naar 2.0.0

De volgende secties wordt beschreven hoe de integratie van een mobiele betrokkenheid Web SDK migreren van de service Capptain, door Capptain SAS, worden aangeboden aan een betrokkenheid van Azure Mobile-app. Als u vanuit een versie eerder dan 1.2.1 migreert, neem contact op met de website Capptain om te migreren naar 1.2.1 eerst en pas de volgende procedures uit.

Deze versie van de mobiele betrokkenheid Web SDK biedt geen ondersteuning voor Samsung slimme TV, Opera TV, webOS of de functie bereiken.

>[AZURE.IMPORTANT] Capptain en Azure Mobile betrokkenheid zijn niet dezelfde service en de volgende procedures markeren alleen het migreren van de client-app. Migreren van de mobiele betrokkenheid Web SDK in de app migreert geen gegevens van een server Capptain op een mobiele betrokkenheid-server.

#### <a name="javascript-files"></a>JavaScript-bestanden

Het bestand capptain-sdk.js vervangen door het bestand azure-engagement.js en past u uw invoer script dienovereenkomstig gewijzigd.

#### <a name="remove-capptain-reach"></a>Capptain bereik verwijderen

Deze versie van de mobiele betrokkenheid Web SDK biedt geen ondersteuning van de functie bereiken. Als u hebt Capptain bereik geïntegreerd in uw toepassing, moet u deze verwijderen.

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

#### <a name="remove-deprecated-apis"></a>Vervallen API's verwijderen

Sommige API's Capptain zijn afgeschaft in de Mobile betrokkenheid Web SDK.

Verwijder eventuele oproepen naar de volgende API's: `agent.connect`, `agent.disconnect`, `agent.pause`, en `agent.sendMessageToDevice`.

Een van de volgende terugbellen verwijderen uit uw configuratie Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, en `onPushMessageReceived`.

#### <a name="configuration"></a>Configuratie

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

De verbindingsreeks voor uw toepassing wordt weergegeven in de portal van Azure.

#### <a name="javascript-apis"></a>JavaScript-API 's

De globale JavaScript-object `window.capptain` heeft een andere naam gekregen `window.azureEngagement`, maar u kunt gebruiken de `window.engagement` alias voor API-oproepen. U kunt deze alias niet gebruiken om te definiëren van de configuratie SDK.

Bijvoorbeeld `capptain.deviceId` wordt `engagement.deviceId`, `capptain.agent.startActivity` wordt `engagement.agent.startActivity`, enzovoort.

Als u hebt al een eerdere versie van de Azure Mobile betrokkenheid Web SDK geïntegreerd in uw toepassing, leest u over het [upgraden van de procedures](mobile-engagement-web-upgrade-procedure.md).
