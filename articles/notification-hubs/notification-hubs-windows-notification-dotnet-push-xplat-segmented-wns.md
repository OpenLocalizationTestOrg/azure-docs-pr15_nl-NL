<properties
    pageTitle="Melding Hubs gebruiken om te verzenden van laatste nieuws (Windows universele)"
    description="Laatste nieuws naar een Windows-app voor universele verzenden via Azure melding Hubs met labels in de registratie."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>


<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Laatste nieuws verzenden via melding Hubs


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Overzicht

In dit onderwerp ziet u hoe u het gebruiken van Azure melding Hubs uitzenden recente nieuws meldingen op een Windows Store of de app voor Windows Phone 8.1 (niet-Silverlight). Als u Windows Phone 8.1 Silverlight hebt samengesteld, raadpleegt u naar de [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) -versie. Als alles compleet is, wordt u mogelijk te registreren voor verbreken nieuwscategorieën waarin u geïnteresseerd bent en u ontvangt alleen push-meldingen voor deze categorieën. Dit scenario is een algemene patroon voor veel apps waar meldingen moeten worden verzonden naar groepen gebruikers die hebben eerder zijn gedeclareerd rente in deze, bijvoorbeeld RSS-lezer, apps voor muziek ventilatoren, enzovoort. 

Uitzenden scenario's zijn ingeschakeld door een of meer _tags_ op te nemen bij het maken van een registratie in de hub melding. Wanneer meldingen worden verzonden naar een tag, worden alle apparaten die zijn geregistreerd voor de tag de melding ontvangt. Omdat tags gewoon tekenreeksen zijn, hebben ze geen vooraf worden ingericht. Voor meer informatie over de labels, raadpleegt u [de melding Hubs Routering en Tag expressies](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Vereisten voor

In dit onderwerp is gebaseerd op de app die u hebt gemaakt in [aan de slag met melding Hubs][get-started]. Voordat u deze zelfstudie begint, u moet al hebben uitgevoerd [aan de slag met melding Hubs][get-started].

##<a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app

De eerste stap is de gebruikersinterface-elementen toevoegen aan uw bestaande hoofdpagina waarmee de gebruiker om te selecteren van categorieën om u te registreren. De categorieën die door een gebruiker is geselecteerd zijn op het apparaat opgeslagen. Wanneer de app wordt gestart, wordt de apparaatregistratie van een in de hub melding met de geselecteerde categorieën gemaakt als labels.

1. Open het projectbestand MainPage.xaml en kopieer de volgende code in het **raster** hoofdelement:

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Klik met de rechtermuisknop op het project **gedeeld** en een nieuwe klasse **meldingen**, met de naam toevoegen de optie **public** toevoegen aan de definitie class en de volgende instructies voor het **gebruik van** toevoegen aan de nieuwe codebestand:

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Kopieer de volgende code naar de nieuwe **meldingen** klasse:

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    Deze klasse gebruikt de lokale opslag voor de opslag van de categorieën van nieuws die dit apparaat heeft ontvangen. Houd er rekening mee dat in plaats van het aanroepen van de methode *RegisterNativeAsync* verwijst naar de *RegisterTemplateAsync* registreren voor de categorieën met de registratie van een sjabloon. 
    
    Wij bieden ook een naam voor de sjabloon ("simpleWNSTemplateExample"), omdat we mogelijk wilt registreren van meer dan één sjabloon (bijvoorbeeld een voor mailpop meldingen) en één voor tegels en moeten we ze een naam om te kunnen bijwerken toe of verwijder deze.

    Houd er rekening mee dat als een apparaat wordt meerdere sjablonen geregistreerd met dezelfde tag, een binnenkomend bericht hebt samengesteld die tag tot meerdere meldingen leidt bezorgd bij het apparaat (één voor elke sjabloon). Dit probleem is handig wanneer hetzelfde logische bericht heeft leiden tot meerdere visuele meldingen, bijvoorbeeld met zowel een badge een mailpop in een toepassing voor de Windows Store.

    Zie voor meer informatie over sjablonen, [sjablonen](notification-hubs-templates-cross-platform-push-messages.md).




4. In het projectbestand App.xaml.cs, moet u de volgende eigenschap toevoegen aan de **App** -klasse:

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Deze eigenschap wordt gebruikt voor het maken en openen van een exemplaar **meldingen** .

    Vervang in de bovenstaande code, de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen met de naam van de hub van de melding en de verbindingsreeks voor *DefaultListenSharedAccessSignature* die u eerder hebt gekregen.

    > [AZURE.NOTE] Omdat referenties die worden geleverd met een client-app niet in het algemeen veilige zijn, moet u alleen de sleutel voor beluisteren van access met uw client-app distribueren. Access wordt ingeschakeld uw app om u te registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd beluisteren en meldingen kunnen niet worden verzonden. De volledige toegang-toets wordt gebruikt in een beveiligde backend-service voor het verzenden van meldingen en bestaande registraties wijzigen.

5. Voeg de volgende regel in uw MainPage.xaml.cs:

        using Windows.UI.Popups;

6. In het projectbestand MainPage.xaml.cs, voegt u de volgende methode:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    Deze methode maakt van een lijst met categorieën en gebruikt de klas **meldingen** aan de lijst opslaan in de lokale opslag en de bijbehorende labels met de melding hub registreren. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

Uw app kan nu een set categorieën worden opgeslagen in een lokale opslag op het apparaat en registreert met de hub melding wanneer de gebruiker de selectie van categorieën wijzigt.

##<a name="register-for-notifications"></a>Register voor meldingen

Volgende stappen uit registreren met de hub melding bij het opstarten gebruik van de categorieën die zijn opgeslagen in de lokale opslag.

> [AZURE.NOTE] Omdat het kanaal URI toegewezen door de Windows melding Service (WNS) op elk gewenst moment wijzigen kunt, dient u te registreren voor meldingen regelmatig om te voorkomen dat melding fouten. In dit voorbeeld wordt geregistreerd voor melding elke keer dat de app wordt gestart. Apps die vaak worden uitgevoerd voor kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie wilt behouden bandbreedte wanneer minder dan een dag is verstreken sinds de vorige registratie.

1. Open het bestand App.xaml.cs en bijwerken van de methode **InitNotificationsAsync** gebruik van de `notifications` class zich kunnen abonneren op basis van categorieën.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    Dit zorgt ervoor dat telkens wanneer de app wordt gestart opgehaald van de categorieën uit lokale opslag en daarom een registeration voor deze categorieën. De methode **InitNotificationsAsync** is gemaakt als onderdeel van de [aan de slag met melding Hubs] [ get-started] zelfstudie.

3. In het projectbestand MainPage.xaml.cs de volgende code hebt toegevoegd aan de methode *OnNavigatedTo* :

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
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

    ![][19]

4. Een nieuw bericht verzenden van de end op een van de volgende manieren:

    + **Console-app:** start de console-app.

    + **Java/PHP:** uw app/script uitvoeren.

    Meldingen voor de geselecteerde categorieën worden weergegeven als mailpop meldingen.

    ![][14]

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u het laatste nieuws uitzenden per categorie. Houd rekening met voltooiing van een van de volgende zelfstudies die markeren van andere geavanceerde melding Hubs-scenario's:

+ [Gebruik melding Hubs gelokaliseerde laatste nieuws uitzenden]

    Leer hoe u de app recente nieuws om in te schakelen verzendende gelokaliseerde meldingen uitvouwen.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Gebruik melding Hubs gelokaliseerde laatste nieuws uitzenden]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
