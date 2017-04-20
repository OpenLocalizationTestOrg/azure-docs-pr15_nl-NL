In deze sectie, kunt u code bijwerkt in uw bestaande Mobile-Apps backend project voor het verzenden van een push-bericht telkens wanneer een nieuw item wordt toegevoegd. Dit is mogelijk gemaakt door [sjabloonfunctie voor de melding Hubs, zodat dit gereedschap duwt platforms](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) . De verschillende clients zijn geregistreerd voor push-meldingen met sjablonen en een enkel universele push alle clientplatforms kan bereiken.

Kies de onderstaande procedure die overeenkomt met uw projecttype backend&mdash; [.NET backend](#dotnet) of [Node.js backend](#nodejs).

### <a name="dotnet"></a>.NET backend-project
1. Visual Studio, met de rechtermuisknop op het serverproject en klik op **NuGet-pakketten beheren**, zoekt `Microsoft.Azure.NotificationHubs`, klik vervolgens op **installeren**. Hiermee installeert u de bibliotheek Hubs melding voor het verzenden van meldingen van uw end.

3. Open in de serverproject **Controllers** > **TodoItemController.cs**, en voeg de volgende instructies:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. In de methode **PostTodoItem** , voegt u de volgende code na de oproep door naar **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Hiermee stuurt een sjabloon-melding dat het item bevat. Tekst wanneer een nieuw item wordt ingevoegd.

4. Het serverproject publiceren. 

### <a name="nodejs"></a>Node.js backend-project

1. Als u nog niet hebt gedaan, [downloaden van het project van de backend quickstart](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) anders de [online editor in de portal van Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)gebruiken.

2. De bestaande code in todoitem.js vervangen door het volgende:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
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

    Hiermee stuurt een sjabloon-melding met de item.text wanneer een nieuw item wordt ingevoegd.

2. Bij het bewerken van het bestand op uw lokale computer, opnieuw publiceren het serverproject.
