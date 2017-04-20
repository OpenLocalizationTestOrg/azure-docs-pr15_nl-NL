<properties
    pageTitle="Meerdere platforms meldingen verzenden naar gebruikers met melding Hubs (ASP.NET)"
    description="Leer hoe u de melding Hubs sjablonen gebruiken om te verzenden, in één aanvraag, een melding voor platform-agnostic die is bedoeld voor alle platforms."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Meerdere platforms meldingen verzenden naar gebruikers met melding Hubs


In de eerdere zelfstudie [Waarschuwen gebruikers met melding Hubs], hebt u geleerd hoe push-meldingen naar alle apparaten die zijn geregistreerd door een specifieke geverifieerde gebruiker. In deze zelfstudie zijn meerdere aanvragen vereist een bericht verzenden naar elke ondersteunde client-platform. Melding Hubs ondersteunt sjablonen, waarmee u kunnen opgeven hoe een specifiek apparaat wil meldingen wilt ontvangen. Hierdoor wordt vereenvoudigd met het verzenden van meerdere platforms meldingen. In dit onderwerp wordt beschreven hoe om te profiteren van de sjablonen wilt verzenden, in één aanvraag, een melding voor platform-agnostic die is bedoeld voor alle platforms. Voor meer informatie over sjablonen gedetailleerde, Zie [Azure melding Hubs overzicht][Templates].

> [AZURE.NOTE] Melding Hubs kan een apparaat registreren van meerdere sjablonen met dezelfde tag. In dit geval levert een binnenkomend bericht hebt samengesteld deze tag wordt gebruikt in meerdere meldingen bezorgd bij het apparaat, één voor elke sjabloon. Hiermee kunt u hetzelfde bericht weergeven in meerdere visuele meldingen, zoals beide als een badge en als een mailpop-upmelding in een Windows Store-app.

Voer de volgende stappen uit om meerdere platforms meldingen met sjablonen te verzenden:

1. Vouw de map **Controllers** in de Verkenner oplossing in Visual Studio en opent u het bestand RegisterController.cs.

2. Zoek de blokkering van de code in de methode **Post** die Hiermee maakt u een nieuwe registratie vervangen de `switch` inhoud met de volgende code:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Deze code roept de methode platform / regiospecifieke als u wilt maken van de registratie van een sjabloon in plaats van een systeemeigen registratie. Bestaande registraties nodig niet worden gewijzigd omdat sjabloon registraties afgeleid van systeemeigen registraties.

3. In de controller **meldingen** , kunt u de methode **sendNotification** vervangen door de volgende code:

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Deze code wordt een melding gestuurd naar alle platforms op hetzelfde moment en zonder te geven van een systeemeigen nettolading. Melding Hubs genereert en levert de juiste nettolading voor elk apparaat instellen met de waarde van de meegeleverde _tag_ , zoals opgegeven in de geregistreerde sjablonen.

4. Uw project van de back-enddatabase WebApi opnieuw publiceren.

5. De client-app opnieuw uitvoeren en controleer of de registratie is voltooid.

6. (Optioneel) De client-app implementeren naar een tweede apparaat en voer vervolgens de app.

    Houd er rekening mee dat er een melding op elk apparaat weergegeven.

## <a name="next-steps"></a>Volgende stappen

Nu dat u deze zelfstudie hebt voltooid, meer informatie over melding Hubs en sjablonen in de volgende onderwerpen:

+ **[Laatste nieuws verzenden via melding Hubs]** <br/>Ziet u een ander scenario voor het gebruik van sjablonen

+  **[Azure melding Hubs overzicht][Templates]**<br/>Overzichtsonderwerp gedetailleerde meer informatie over sjablonen.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Laatste nieuws verzenden via melding Hubs]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Gebruikers met melding Hubs hoogte]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
