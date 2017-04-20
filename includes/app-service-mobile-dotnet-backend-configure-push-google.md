Gebruik de procedure die overeenkomt met uw projecttype backend&mdash; [.NET backend](#dotnet) of [Node.js backend](#nodejs).

### <a name="dotnet"></a>.NET backend-project

1. Visual Studio, met de rechtermuisknop op het serverproject en klik op **NuGet-pakketten beheren**, zoekt `Microsoft.Azure.NotificationHubs`, klik vervolgens op **installeren**. Hiermee installeert u de bibliotheek Hubs melding-client.

2. In de map Controllers open TodoItemController.cs en voeg de volgende `using` instructies:

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;

3. Vervang de `PostTodoItem` methode met de volgende code:  

      
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send the push notification and log the results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write the success result to the logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write the failure result to the logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. Het serverproject publiceren.

### <a name="nodejs"></a>Node.js backend-project

1. Als u nog niet hebt gedaan, [downloaden van het project quickstart](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) anders de [online editor in de portal van Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)gebruiken.
 
1. De bestaande code in het bestand todoitem.js vervangen door het volgende:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
        
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    Hiermee stuurt een GCM-melding met de item.text wanneer een nieuw item van de taak wordt ingevoegd. 

2. Bij het bewerken van het bestand in uw lokale computer, opnieuw publiceren het serverproject. 
