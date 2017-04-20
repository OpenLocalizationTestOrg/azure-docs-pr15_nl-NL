<properties
    pageTitle="Push-meldingen toevoegen aan uw app universele Windows-Platform (UWP) | Azure mobiele Apps"
    description="Informatie over het gebruik van Azure App Service Mobile-Apps en Azure melding Hubs push-meldingen verzenden naar uw app universele Windows-Platform (UWP)."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Push-meldingen toevoegen aan uw Windows-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie toevoegen u push-meldingen aan het project [start van Windows snelle](app-service-mobile-windows-store-dotnet-get-started.md) , zodat een push-bericht wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u de push notification-extensie-pakket. Zie [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

##<a name="configure-hub"></a>Een melding Hub configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Uw app voor push-meldingen hebt geregistreerd

U moet uw app in de Windows Store indienen, moet u uw serverproject kunt integreren met Windows melding Services (WNS) om te verzenden push configureren.

1. Klik in Visual Studio Solution Explorer met de rechtermuisknop op het project UWP-app, klik op **Store** > **App koppelen in de Store...**. 

    ![Windows Store app koppelen](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. In de wizard op **volgende**, meld u aan met uw Microsoft-account, typ een naam voor de app in **reserveren een nieuwe appnaam**en klik op **reserveren**.

3. Nadat de registratie van de app is gemaakt, selecteert u de naam van de nieuwe app, klik op **volgende**en klik vervolgens op **koppelen**. Hiermee wordt de vereiste gegevens van de Windows Store-registratie toegevoegd aan manifest van de toepassing.  

7. Ga naar het [Windows ontwikkelaar Center](https://dev.windows.com/en-us/overview), aanmelden met uw Microsoft-account, op de nieuwe app registratie in **Mijn apps**en **Services**uitvouwen > **Push-meldingen**. 

8. Klik op **Live Services-site** onder **Services van Microsoft Azure mobiele** **Push-meldingen** op de pagina.

9. Maak een notitie van de waarde onder **toepassing geheimen** en **Pakket beveiligings-id**die u vervolgens gebruiken wilt voor het configureren van uw mobiele app backend registratie op de pagina. 

    ![Windows Store app koppelen](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] De client geheim en pakket beveiligings-id zijn belangrijk beveiligingsreferenties. Deze waarden met iedereen delen of verdelen met uw app niet. De **Toepassings-Id** wordt gebruikt met het geheim verificatie van Microsoft-Account configureren.

##<a name="configure-the-backend-to-send-push-notifications"></a>De backend als u wilt verzenden push-meldingen configureren

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>Bijwerken van de server als u wilt verzenden push-meldingen

Gebruik de volgende procedure die overeenkomt met uw projecttype backend&mdash; [.NET backend](#dotnet) of [Node.js backend](#nodejs).

### <a name="dotnet"></a>.NET backend-project

1. Visual Studio, met de rechtermuisknop op het serverproject en **NuGet-pakketten beheren**, zoekt u Microsoft.Azure.NotificationHubs, klik op **installeren**. Hiermee installeert u de bibliotheek Hubs melding-client.

2. Vouw **Controllers**, opent u TodoItemController.cs en voeg de volgende instructies:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. In de methode **PostTodoItem** , voegt u de volgende code na de oproep door naar **InsertAsync**:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Deze code wordt de melding hub voor het verzenden van een push-bericht wanneer een nieuw item invoeging is uitgelegd.

4. Het serverproject publiceren.

### <a name="nodejs"></a>Node.js backend-project

1. Als u nog niet hebt gedaan, [downloaden van het project quickstart](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) anders de [online editor in de portal van Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)gebruiken.

2. De bestaande code in het bestand todoitem.js vervangen door het volgende:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    Er wordt een WNS mailpop-upmelding waarin de item.text wanneer een nieuw item van de taak wordt ingevoegd gestuurd.

2. Bij het bewerken van het bestand op uw lokale computer, opnieuw publiceren het serverproject.

##<a id="update-app"></a>Push-meldingen toevoegen aan uw app

Uw app moet vervolgens registreren voor push-meldingen op opstart. Wanneer u verificatie al hebt ingeschakeld, zorg dat de gebruiker positief of negatief in voordat u probeert te registreren voor push-meldingen.

1. Open het projectbestand **App.xaml.cs** en voeg de volgende `using` instructies:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. In hetzelfde bestand, door de definitie van de volgende **InitNotificationsAsync** -methode te voegen aan de **App** -klasse:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Deze code de ChannelURI voor de app opgehaald uit WNS en registreert die ChannelURI vervolgens met de Mobile-App van uw App-Service.

3. Aan de bovenkant van de gebeurtenis-handler **OnLaunched** in **App.xaml.cs**, de wijzigingstoets **asynchrone** toevoegen aan de methodedefinitie en toevoegen van de volgende oproep door naar de nieuwe **InitNotificationsAsync** -methode, zoals in het volgende voorbeeld:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Dit zorgt ervoor dat de tijdelijk ChannelURI telkens wanneer die de toepassing wordt gestart is geregistreerd.

4. Uw project UWP-app opnieuw. Uw app is nu klaar mailpop-meldingen wilt ontvangen.

##<a id="test"></a>Test push-meldingen in uw app

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Volgende stappen

Meer informatie over push-meldingen:

* [Het gebruik van de beheerde-client voor Azure Mobile-Apps](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Sjablonen flexibiliteit om meerdere platforms gereedschap duwt en gelokaliseerde gereedschap duwt te verzenden. Leer hoe u sjablonen registreren.

* [Een diagnose stellen bij problemen van push-meldingen](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Er zijn verschillende redenen waarom meldingen mogelijk krijgen verbroken of naam niet eindigt op apparaten. In dit onderwerp ziet u hoe u analyseren en duidelijk de oorzaak van push-meldingen fouten. 

Houd rekening met doorlopende op een van de volgende zelfstudies:

+ [Verificatie toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Leer hoe u gebruikers van uw app met een identiteitsprovider verifiëren.

+ [Offline synchronisatie voor uw app inschakelen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Leer hoe u Offlineondersteuning uw app met een backend Mobile-App toevoegen. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er helemaal geen netwerkverbinding.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

