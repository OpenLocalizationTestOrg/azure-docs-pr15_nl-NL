<properties
    pageTitle="Integratie van Azure Mobile betrokkenheid Web SDK | Microsoft Azure"
    description="De meest recente updates en procedures voor het Azure Mobile betrokkenheid Web SDK"
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
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Azure Mobile betrokkenheid integreren in een webtoepassing

> [AZURE.SELECTOR]
- [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-apparaat](mobile-engagement-android-integrate-engagement.md)

De eenvoudigste manier om te activeren de analyses en controle functies in Azure Mobile betrokkenheid in uw webtoepassing door de procedures in dit artikel wordt beschreven.

Volg de stappen voor het activeren van de log-rapporten die nodig zijn voor het berekenen van alle statistieken over gebruikers, sessies, activiteiten, loopt en technicals. Voor de statistieken afhankelijk zijn van toepassing, zoals gebeurtenissen, fouten en taken, moet u handmatig log rapporten activeren met behulp van de Azure Mobile betrokkenheid API. Informatie over [het gebruik van de geavanceerde Mobile betrokkenheid API in een webtoepassing labelen](mobile-engagement-web-use-engagement-api.md)voor meer informatie.

## <a name="introduction"></a>Inleiding

[Downloaden van het Web Azure mobiele betrokkenheid SDK](http://aka.ms/P7b453).
De mobiele betrokkenheid Web SDK is verzonden als één JavaScript-bestand, azure-engagement.js, die u wilt opnemen in elke pagina van uw site of web-toepassing.

> [AZURE.IMPORTANT] Voordat u dit script uitvoeren, moet u een script of code-fragment dat u schrijft Mobile betrokkenheid configureren voor uw toepassing uitvoeren.

## <a name="browser-compatibility"></a>Compatibiliteit webbrowser

De mobiele betrokkenheid Web SDK gebruikt systeemeigen JSON coderen en decoderen, naast AJAX-aanvragen van een ander domein (te vertrouwen op de W3C CORS specificatie). Dit is compatibel met de volgende browsers:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chrome 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Mobiele betrokkenheid configureren

Een script schrijven dat Hiermee maakt u een globale `azureEngagement` JavaScript-object, zoals in het volgende voorbeeld. Omdat uw site veelvouden pagina's hebt mogelijk, in dit voorbeeld wordt ervan uitgegaan dat dit script is opgenomen in elke pagina. In dit voorbeeld de JavaScript-object heet `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

De `connectionString` waarde voor uw toepassing wordt weergegeven in de portal van Azure.

> [AZURE.NOTE] `appVersionName`en `appVersionCode` zijn optioneel. Echter, is het raadzaam dat u deze zo configureren dat analytics versiegegevens kunt verwerken.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Mobile betrokkenheid scripts opnemen in uw pagina 's
Mobile betrokkenheid scripts toevoegen aan uw pagina's in een van de volgende manieren:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Of dit:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias

Nadat het script Mobile betrokkenheid Web SDK is geladen, wordt de alias **betrokkenheid** voor toegang tot de API SDK gemaakt. U kunt deze alias niet gebruiken om te definiëren van de configuratie SDK. Deze alias wordt gebruikt als een verwijzing in deze documentatie.

Houd er rekening mee dat als de standaardalias met een andere globale variabele van uw pagina conflicteert, u deze in de configuratie als volgt desgewenst kunt voordat u de mobiele betrokkenheid Web SDK laadt:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Eenvoudige rapportage

Eenvoudige rapportage in Mobile betrokkenheid behandelt sessie niveau statistieken, zoals statistieken over gebruikers, sessies, activiteiten en loopt.

### <a name="session-tracking"></a>Sessie bijhouden

Een sessie Mobile betrokkenheid in een reeks activiteiten is verdeeld, elk die wordt aangeduid met een naam.

In een klassieke website, is het raadzaam dat u de activiteit in een andere op elke pagina van uw site declareren. Voor een website of web toepassing waarin de huidige pagina nooit gewijzigd, wilt u mogelijk de activiteiten bijhouden op kleinere schaal, zoals binnen de pagina.

In beide gevallen om te beginnen of wijzigen van de huidige gebruikersactiviteit, bellen de `engagement.agent.startActivity` functie. Bijvoorbeeld:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

De betrokkenheid van de Mobile-server beëindigd automatisch een geopende sessie binnen drie minuten nadat de pagina toepassing gesloten is.

U kunt ook u kunt een sessie beëindigen handmatig door te bellen `engagement.agent.endActivity`. Hiermee stelt u de huidige gebruikersactiviteit op 'Niet-actief'.  De sessie 10 seconden later moet worden beëindigd, tenzij een nieuw gesprek naar `engagement.agent.startActivity` cv's de sessie.

U kunt als volgt de 10 seconden vertraging in het object globale betrokkenheid configureren:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] U kunt niet gebruiken `engagement.agent.endActivity` in de `onunload` terugbellen omdat u niet AJAX oproepen in deze fase.

## <a name="advanced-reporting"></a>Geavanceerde rapportage

(Optioneel) als u rapporteren toepassingsspecifieke gebeurtenissen, fouten en taken wilt, moet u de mobiele betrokkenheid-API gebruiken. U toegang hebt tot de API Mobile betrokkenheid tot en met de `engagement.agent` object.

U kunt alle geavanceerde mogelijkheden in Mobile-betrokkenheid in de Mobile-betrokkenheid-API openen. De API wordt beschreven in het artikel [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in een webtoepassing](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-the-urls-used-for-ajax-calls"></a>De URL's die worden gebruikt voor gesprekken met AJAX aanpassen

U kunt de URL's die in de Mobile betrokkenheid Web SDK wordt gebruikt. Als u wilt definiëren de log-URL (het eindpunt SDK voor logboekregistratie), kunt u bijvoorbeeld de configuratie als volgt negeren:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Als uw URL-functies als een tekenreeks die begint resultaat met `/`, `//`, `http://`, of `https://`, niet wordt gebruikt als de standaardschema. Standaard de `https://` kleurenschema wordt gebruikt voor deze URL's. Als u aanpassen van de standaard-kleurenschema wilt, overschreven door de configuratie, als volgt:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };
