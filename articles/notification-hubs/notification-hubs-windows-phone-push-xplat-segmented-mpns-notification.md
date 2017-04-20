<properties
    pageTitle="Melding Hubs gebruiken om te verzenden van laatste nieuws (Windows Phone)"
    description="Via Azure melding Hubs tag in registraties naar het laatste nieuws verzenden naar een Windows Phone-app."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Laatste nieuws verzenden via melding Hubs

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Overzicht

In dit onderwerp ziet u hoe u het gebruiken van Azure melding Hubs uitzenden recente nieuws meldingen bij een Windows Phone 8.0/8.1 Silverlight-app. Als u hebt samengesteld Windows Store- of Windows Phone 8.1 app, Raadpleeg naar de [Windows universele](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) versie. Als alles compleet is, wordt u mogelijk te registreren voor verbreken nieuwscategorieën waarin u geïnteresseerd bent en u ontvangt alleen push-meldingen voor deze categorieën. Dit scenario is een algemene patroon voor veel apps waar meldingen moeten worden verzonden naar groepen gebruikers die eerder rente in deze, bijvoorbeeld RSS-lezer, apps voor muziek ventilatoren, enzovoort zijn gedeclareerd.

Uitzenden scenario's zijn ingeschakeld door een of meer _tags_ op te nemen bij het maken van een registratie in de hub melding. Wanneer meldingen worden verzonden naar een tag, worden alle apparaten die zijn geregistreerd voor de tag de melding ontvangt. Omdat tags gewoon tekenreeksen zijn, hebben ze geen vooraf worden ingericht. Voor meer informatie over de labels, raadpleegt u [de melding Hubs Routering en Tag expressies](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Vereisten voor

In dit onderwerp dat is gebaseerd op de app die u hebt gemaakt in [aan de slag met melding Hubs]. Voordat u deze zelfstudie begint, moet u al hebben uitgevoerd [aan de slag met melding Hubs].

##<a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app

De eerste stap is de gebruikersinterface-elementen toevoegen aan uw bestaande hoofdpagina waarmee de gebruiker om te selecteren van categorieën om u te registreren. De categorieën die door een gebruiker is geselecteerd zijn op het apparaat opgeslagen. Wanneer de app wordt gestart, wordt de apparaatregistratie van een in de hub melding met de geselecteerde categorieën gemaakt als labels.

1. Open het projectbestand MainPage.xaml en vervangt u vervolgens de elementen van het **raster** met de naam `TitlePanel` en `ContentPanel` met de volgende code:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. In het project, de optie **public** maakt u een nieuwe klasse **meldingen**, met de naam toevoegen aan de definitie van de klasse en de volgende instructies voor het **gebruik van** toevoegen aan de nieuwe codebestand:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. Kopieer de volgende code naar de nieuwe **meldingen** klasse:

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    Deze klasse gebruikt de geïsoleerd opslag voor de opslag van de categorieën van nieuws die dit apparaat moet ontvangen. De presentatie bevat ook methoden voor het registreren voor deze categorieën met de melding registratie van een [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) .


4. In het projectbestand App.xaml.cs, moet u de volgende eigenschap toevoegen aan de **App** -klasse. Vervang de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen met de naam van de hub van de melding en de verbindingsreeks voor *DefaultListenSharedAccessSignature* die u eerder hebt gekregen.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Omdat referenties die worden geleverd met een client-app niet in het algemeen veilige zijn, moet u alleen de sleutel voor beluisteren van access met uw client-app distribueren. Access wordt ingeschakeld uw app om u te registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd beluisteren en meldingen kunnen niet worden verzonden. De volledige toegang-toets wordt gebruikt in een beveiligde backend-service voor het verzenden van meldingen en bestaande registraties wijzigen.

5. Voeg de volgende regel in uw MainPage.xaml.cs:

        using Windows.UI.Popups;

6. In het projectbestand MainPage.xaml.cs, voegt u de volgende methode:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    Deze methode maakt van een lijst met categorieën en gebruikt de klas **meldingen** aan de lijst opslaan in de lokale opslag en de bijbehorende labels met de melding hub registreren. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

Uw app kan nu een set categorieën worden opgeslagen in een lokale opslag op het apparaat en registreert met de hub melding wanneer de gebruiker de selectie van categorieën wijzigt.

##<a name="register-for-notifications"></a>Register voor meldingen

Volgende stappen uit registreren met de hub melding bij het opstarten gebruik van de categorieën die zijn opgeslagen in de lokale opslag.

> [AZURE.NOTE] Omdat het kanaal URI toegewezen door het Microsoft Push-meldingen Service (MPNS) op elk gewenst moment wijzigen kunt, dient u te registreren voor meldingen regelmatig om te voorkomen dat melding fouten. In dit voorbeeld wordt voor meldingen elke keer dat de app wordt gestart. Apps die vaak worden uitgevoerd voor kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie wilt behouden bandbreedte wanneer minder dan een dag is verstreken sinds de vorige registratie.


1. Open het bestand App.xaml.cs en de wijzigingstoets **asynchrone** toevoegen aan **Application_Launching** methode en vervangen van de melding Hubs registratie-code die u hebt toegevoegd in [aan de slag met Hubs melding] met de volgende code:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    Dit zorgt ervoor dat telkens wanneer de app wordt gestart opgehaald van de categorieën uit lokale opslag en daarom een registratie voor deze categorieën.

2. Voeg de volgende code die de methode **OnNavigatedTo** implementeert in het projectbestand MainPage.xaml.cs:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
        }

    Hiermee vernieuwt u de hoofdpagina op basis van de status van eerder opgeslagen categorieën.

De app is voltooid en een set met categorieën kunt opslaan in de lokale opslag van apparaat wordt gebruikt om te registreren met de hub melding wanneer de gebruiker de selectie van categorieën wijzigt. Vervolgens definiëren we een backend die categorie meldingen voor deze app kunt verzenden.

##<a name="sending-tagged-notifications"></a>Gelabelde meldingen verzenden

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>De app uitvoeren en meldingen genereren

1. Druk op F5 om te compileren en start de app in Visual Studio.

    ![][1]

    Opmerking dat de UI-app biedt een aantal Hiermee schakelt u waarmee u Kies de categorieën die zich kunnen abonneren op.

2. Hiermee schakelt u een of meer categorieën inschakelen en klik op **abonneren**.

    De app zet om de geselecteerde categorieën in labels en vraagt een nieuwe apparaatregistratie voor de geselecteerde codes van de hub melding. De geregistreerde categorieën worden geretourneerd en in een dialoogvenster weergegeven.

    ![][2]

3. Nadat een bevestiging dat uw categorieën abonnement voltooid zijn ontvangen, kunt u de console-app als u wilt verzenden van meldingen voor elke categorieën uitvoeren. Controleer of er alleen een melding voor de categorieën die u zich hebt geabonneerd op.

    ![][3]

U kunt dit onderwerp hebt voltooid.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Aan de slag met melding Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

