<properties
    pageTitle="Het gebruik van Apache Cordova-invoegtoepassing voor Azure mobiele Apps"
    description="Het gebruik van Apache Cordova-invoegtoepassing voor Azure mobiele Apps"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Het gebruik van Apache Cordova Client bibliotheek voor Azure mobiele Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding leert u uitvoeren van veelvoorkomende scenario's met de meest recente [Apache Cordova-invoegtoepassing voor Azure Mobile-Apps]. Als u nog niet eerder met Azure Mobile-Apps, eerste voltooid [Azure mobiele Apps snel aan de slag] met het maken van een back-end, een tabel maken en een vooraf gedefinieerde Apache Cordova project downloaden. In deze handleiding, richten we ons op de aan de clientzijde Apache Cordova-invoegtoepassing.

## <a name="supported-platforms"></a>Ondersteunde platformen

Deze SDK ondersteunt Apache Cordova v6.0.0 en later iOS, Android en Windows apparaten.  Het platformondersteuning is als volgt:

* Android (KitKat tot en met deze) 19-24-API
* iOS 8.0 en hoger.
* Windows Phone 8.0
* Windows Phone 8.1
* Universele Windows-Platform

##<a name="Setup"></a>Installatie en vereisten

Deze handleiding wordt ervan uitgegaan dat u een back-end met een tabel hebt gemaakt. Deze handleiding wordt ervan uitgegaan dat de tabel hetzelfde schema als de tabellen in deze zelfstudies. Deze handleiding wordt ook ervan uitgegaan dat u de invoegtoepassing voor Apache Cordova hebt toegevoegd aan uw code.  Als u niet hebt gedaan, kunt u de invoegtoepassing voor Apache Cordova kunt toevoegen aan uw project op de opdrachtregel:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Zie voor meer informatie over het maken van [uw eerste Apache Cordova app]hun documentatie.

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="auth"></a>Hoe u: gebruikers verifiëren

Azure App-Service ondersteuning biedt voor verificatie en machtiging van app-gebruikers met verschillende externe identiteitsproviders: Facebook, Google, Microsoft-Account en Twitter. U kunt machtigingen instellen voor tabellen beperken van toegang voor specifieke bewerkingen aan alleen geverifieerde gebruikers. U kunt ook de identiteit van geverifieerde gebruikers willen implementeren autorisatieregels in server-scripts gebruiken. Zie de zelfstudie [aan de slag met verificatie] voor meer informatie.

Wanneer u met verificatie in een app Apache Cordova, moeten de volgende Cordova Plug-ins beschikbaar zijn:

* [cordova-Plug-apparaat]
* [cordova-Plug-inappbrowser]

Twee verificatie loopt worden ondersteund: de stroom van een server en de stroom van een client.  De stroom server biedt de eenvoudigste verificatie-ervaring, zoals deze is afhankelijk van de provider web verificatie-interface. De client-stroom kan voor betere integratie met apparaat-specifieke functies, zoals één eenmalige aanmelding terwijl het bedrijf providerspecifieke apparaat-specifieke SDK's gebruikt.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Hoe u: uw mobiele App-Service voor externe Omleidings-URL's configureren.

Diverse soorten Apache Cordova toepassingen gebruikt u de mogelijkheid van een loopback worden afgehandeld OAuth UI loopt.  OAuth UI overdrachten op localhost veroorzaken problemen omdat de verificatieservice alleen weet hoe u uw service al dan niet standaard benut.  Voorbeelden van problematisch OAuth UI loopt zijn:

- De emulator rimpel.
- Opnieuw laden met ionische draaien of spiegelen
- De mobiele backend lokaal uitgevoerd
- De mobiele backend uitgevoerd in een andere Service van de App Azure dan de één die verificatie.

Volg deze instructies om toe te voegen van de map local settings in de configuratie:

1. Meld u aan bij de [portal van Azure]
2. Selecteer **alle resources** of **App Services** Klik op de naam van de Mobile-App.
3. Klik op **Extra**
4. Klik op **Resource explorer** in het menu OBSERVE en klik op **Ga**.  Een nieuw venster of tabblad wordt geopend.
5. Vouw de **zoekconfiguratie**, **authsettings** knooppunten voor uw site in het linkernavigatievenster.
6. Klik op **bewerken**
7. Zoek naar het element "allowedExternalRedirectUrls".  Dit kan worden ingesteld op null of een matrix van waarden.  Wijzig de waarde in de volgende waarde:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    De URL's vervangen door de URL's van uw service.  Voorbeelden hiervan zijn "http://localhost:3000" (voor de steekproef-service Node.js) of "http://localhost:4400" (voor de service Rimpel).  Deze URL's zijn echter voorbeelden - uw situatie, inclusief voor de services die worden genoemd in de voorbeelden afwijken.
8. Klik op de knop **Alleen-lezen/schrijven** in de rechterbovenhoek van het scherm.
9. Klik op de groene knop **plaatsen** .

De instellingen zijn op dit punt opgeslagen.  Niet het browservenster sluit totdat u klaar bent met de instellingen op te slaan.
Ook deze loopback-URL's aan de CORS-instellingen voor uw App-Service toevoegen:

1. Meld u aan bij de [portal van Azure]
2. Selecteer **alle resources** of **App Services** Klik op de naam van de Mobile-App.
3. Het blad instellingen automatisch wordt geopend.  Als dit niet het geval is, klikt u op **Alle instellingen**.
4. Klik op **CORS** in het menu API.
5. Geef de URL die u wilt toevoegen in het vak geleverd en druk op Enter.
6. Geef extra URL's naar wens.
7. Klik op **Opslaan** als de instellingen wilt opslaan.

Het duurt ongeveer 10-15 seconden voor de nieuwe instellingen pas van kracht.

##<a name="register-for-push"></a>Hoe u: registreren voor Push-meldingen

Installeer de [phonegap-Plug-push] om af te handelen push-meldingen.  Deze invoegtoepassing eenvoudig kan worden toegevoegd met de `cordova plugin add` opdracht op de opdrachtregel of via het installatieprogramma van de invoegtoepassing cijfer binnen Visual Studio.  De volgende code in uw app Apache Cordova registreert uw apparaat gebruiken voor push-meldingen:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

De melding Hubs SDK gebruiken om te verzenden van push-meldingen van de server.  Nooit push-meldingen rechtstreeks vanuit clients verzenden. Hiermee kan worden gebruikt voor het starten van een dergelijke aanval ten opzichte van de melding Hubs of de PNS.  De PNS kan uw verkeer grond van dergelijke aanvallen been.

<!-- URLs. -->
[Azure-portal]: https://portal.azure.com
[Azure mobiele Apps Quick Start]: app-service-mobile-cordova-get-started.md
[Aan de slag met verificatie]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova-invoegtoepassing voor Azure mobiele Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[uw eerste Apache Cordova-app]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-Plug-push]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-Plug-apparaat]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-Plug-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
