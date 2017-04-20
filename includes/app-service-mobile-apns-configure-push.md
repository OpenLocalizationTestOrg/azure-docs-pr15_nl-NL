

1. Op uw Mac, start u **Sleutelhangertoegang**. Open **Mijn certificaten** onder **categorie** op de balk links navigationn. Zoek het SSL-certificaat dat u in de vorige sectie hebt gedownload en vermelden van de inhoud ervan. Selecteer alleen het certificaat (niet inschakelt de persoonlijke sleutel), en [deze exporteren](https://support.apple.com/kb/PH20122?locale=en_US).

2. Klik in de [portal van Azure](https://portal.azure.com/)op **Door alles bladeren** > **App Services** > uw backend Mobile-App. Klik onder **Instellingen**op het **App-Service Push**, klik op de naam van de hub van de melding. Ga naar de **Apple Push Notification Services** > **certificaat uploaden**. Upload het bestand .p12, de juiste **modus** selecteren (afhankelijk van of de klant SSL-certificaat van eerder is productie of Sandbox). Wijzigingen opslaan.

Uw service is nu geconfigureerd voor het werken met push-meldingen op iOS!

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
