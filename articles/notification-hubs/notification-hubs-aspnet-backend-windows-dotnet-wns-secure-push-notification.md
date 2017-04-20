<properties
    pageTitle="Azure melding Hubs Secure Push"
    description="Informatie over het verzenden van secure push-meldingen in Azure wordt aangegeven. Voorbeelden van de code is geschreven in C# de .NET-API gebruiken."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure melding Hubs Secure Push

> [AZURE.SELECTOR]
- [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android-apparaat](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Overzicht

Ondersteuning voor push-meldingen in Microsoft Azure krijgt u toegang tot een eenvoudig te gebruiken, meerdere platforms, schaal-out push-infrastructuur, waardoor enorm eenvoudiger de uitvoering van push-meldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms.

Vanwege wettelijke of beveiligingsbeperkingen, soms een toepassing mogelijk moeten worden opgenomen iets in de melding die niet kan worden overgebracht via de standaard push notification-infrastructuur. Deze zelfstudie wordt beschreven hoe de dezelfde ervaring bereiken door te sturen vertrouwelijke gegevens door een beveiligde, geverifieerde verbinding tussen het clientapparaat worden gebruikt en de app-end.

Op hoog niveau is de stroom als volgt uit:

1. De app back-enddatabase:
    - Winkels secure nettolading in back-enddatabase.
    - Hiermee worden de ID van deze melding bij het apparaat (er worden geen gegevens veilig is verzonden).
2. De app op het apparaat, wanneer u de melding ontvangt:
    - Het apparaat contact op met de back-enddatabase aanvragen van de beveiligde nettolading.
    - De app kunt u de nettolading weergeven als een melding op het apparaat.

Het is belangrijk te weten dat in de voorgaande stroom (en in deze zelfstudie), wordt ervan uitgegaan dat het apparaat een verificatietoken in de lokale opslag, opslaat nadat de gebruiker zich aanmeldt. Dit zorgt ervoor een volledig naadloze oplossing, terwijl het apparaat van de melding secure payload met deze token kunt ophalen. Als uw toepassing slaat geen verificatietokens op het apparaat, of als deze tokens kunnen worden verlopen, moet een algemene melding dat de gebruiker wordt gevraagd naar de app Start de app apparaat bij de melding worden weergegeven. De app wordt geverifieerd door de gebruiker vervolgens en ziet u de melding nettolading.

Deze zelfstudie Secure Push ziet u hoe u een melding push veilig verzendt. Dit is de zelfstudie die is gebaseerd op de zelfstudie [Gebruikers waarschuwen](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) , zodat u moet eerst de stappen in deze zelfstudie uitvoeren.

> [AZURE.NOTE] Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en configureren van uw melding-hub, zoals beschreven in [Aan de slag met melding Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
Bedenk ook, dat in Windows Phone 8.1 (niet Windows Phone) voor Windows-referenties zijn vereist en achtergrondtaken werken niet op Windows Phone 8.0 of Silverlight 8.1. Voor Windows Store-toepassingen, u kunt meldingen ontvangen via een achtergrondtaak alleen als de app wordt uitgevoerd vergrendelingsscherm ingeschakeld (Klik op het selectievakje in de Appmanifest).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Het Windows Phone-Project wijzigen

1. In het project **NotifyUserWindowsPhone** , de volgende code hebt toegevoegd aan App.xaml.cs taak op de achtergrond push registreren. Voeg de volgende regel met code aan het einde van de `OnLaunched()` methode:

        RegisterBackgroundTask();

2. Nog steeds in App.xaml.cs, voeg de volgende code direct na de `OnLaunched()` methode:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Voeg de volgende `using` instructies boven aan het bestand App.xaml.cs:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. Klik op **Alles opslaan**in het menu **bestand** in Visual Studio.

## <a name="create-the-push-background-component"></a>Het onderdeel van de achtergrond Push maken

De volgende stap is het opzetten van het onderdeel van de achtergrond push.

1. Met de rechtermuisknop op het bovenste knooppunt van de oplossing (**oplossing SecurePush** in dit geval) in Solution Explorer, klikt u op **toevoegen**en klik op **Nieuw Project**.

2. Vouw **Store Apps**, en vervolgens klikt u op **Windows Phone-Apps**en klik op van **Windows Runtime-Component (Windows Phone)**. Naam van het project **PushBackgroundComponent**en klik vervolgens op **OK** om het project te maken.

    ![][12]

3. In Solution Explorer met de rechtermuisknop op het project **PushBackgroundComponent (Windows Phone 8.1)** , en vervolgens klikt u op **toevoegen**, klik op **Class**. De naam van de nieuwe klasse **PushBackgroundTask.cs**. Klik op **toevoegen** om te genereren van de klas.

4. De volledige inhoud van de definitie van de naamruimte **PushBackgroundComponent** vervangen door de volgende code, wordt vervangen door de tijdelijke aanduiding `{back-end endpoint}` met het back-enddatabase eindpunt verkregen tijdens het implementeren van uw back-enddatabase:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. Klik in Solution Explorer met de rechtermuisknop op het project **PushBackgroundComponent (Windows Phone 8.1)** en klik vervolgens op **NuGet-pakketten beheren**.

6. Aan de linkerkant, klikt u op **Online**.

7. Typ in het vak **Zoeken** **Http-Client**.

8. Klik op **Microsoft http-clientbibliotheken**in de lijst met resultaten en klik vervolgens op **installeren**. De installatie te voltooien.

9. Typ **Json.net**terug in het vak NuGet **Zoeken** . Installeer het pakket **Json.NET** in en sluit het venster NuGet Package Manager.

10. Voeg de volgende `using` instructies boven aan het bestand **PushBackgroundTask.cs** :

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. Klik in Solution Explorer in het project **NotifyUserWindowsPhone (Windows Phone 8.1)** , met de rechtermuisknop op **verwijzingen**en klik **Verwijzing toevoegen**. Klik in het dialoogvenster verwijzing Manager het selectievakje in naast **PushBackgroundComponent**en klik vervolgens op **OK**.

12. Dubbelklik op **Package.appxmanifest** in het project **NotifyUserWindowsPhone (Windows Phone 8.1)** in Solution Explorer. Onder **meldingen**, stel **Mailpop nodig** op **Ja**.

    ![][3]

13. Nog steeds in **Package.appxmanifest**, klikt u op het menu **declaraties** boven. In de vervolgkeuzelijst **Beschikbaar declaraties** **Achtergrondtaken**op en klik vervolgens op **toevoegen**.

14. Schakel de **Push-meldingen**in **Package.appxmanifest**, onder **Eigenschappen**.

15. Typ **PushBackgroundComponent.PushBackgroundTask** in het veld **Ingangspunt** in **Package.appxmanifest**, klik onder **Instellingen van de App**.

    ![][13]

16. Klik in het menu **bestand** op **Alles opslaan**.

## <a name="run-the-application"></a>Voer de toepassing

Ga als volgt te werk om de toepassing uitvoert:

1. Visual Studio, voert u de **AppBackend** Web API-toepassing. Een ASP.NET-webpagina wordt weergegeven.

2. Visual Studio, moet u de **NotifyUserWindowsPhone (Windows Phone 8.1)** voor Windows Phone-app uitvoeren. De Windows Phone-emulator wordt uitgevoerd en de app automatisch wordt geladen.

3. In de app **NotifyUserWindowsPhone** UI, voert u een gebruikersnaam en wachtwoord. Deze kunnen een willekeurige tekenreeks, maar ze moeten hetzelfde resultaat.

4. Klik in de app **NotifyUserWindowsPhone** UI op **aanmelden en registreert**. Klik vervolgens op **push verzenden**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
