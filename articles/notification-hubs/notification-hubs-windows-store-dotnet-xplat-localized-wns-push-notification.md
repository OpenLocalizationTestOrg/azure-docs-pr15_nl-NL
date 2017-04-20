<properties
    pageTitle="Melding Hubs gelokaliseerd recente nieuws zelfstudie"
    description="Informatie over het gebruik van Azure melding Hubs gelokaliseerde recente nieuws meldingen wilt verzenden."
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

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Melding Hubs via gelokaliseerde laatste nieuws verzenden

> [AZURE.SELECTOR]
- [Windows Store-C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>Overzicht

In dit onderwerp ziet u hoe de functie te gebruiken **sjabloon** van Azure melding Hubs uitzenden recente nieuws meldingen die door de taal- en apparaat zijn aangepast. In deze zelfstudie begint u met de Windows Store-app in [Gebruik melding Hubs naar het laatste nieuws verzenden]hebt gemaakt. Als alles compleet is, dat is mogelijk registreren voor categorieën waarin u geïnteresseerd bent, Geef een taal waarin u de meldingen wilt ontvangen en ontvangt alleen push-meldingen voor de geselecteerde categorieën in die taal.


Er zijn twee onderdelen dit scenario:

- de Windows Store-app kan client apparaten om op te geven van een taal en zich kunnen abonneren op verschillende recente nieuwscategorieën;

- de back-enddatabase verzendt de meldingen, met het **label** - en **sjabloon** funcites van Azure melding Hubs.



##<a name="prerequisites"></a>Vereisten voor

U moet al hebben uitgevoerd de zelfstudie [Gebruik melding Hubs naar het laatste nieuws verzenden] en hebt u de code die beschikbaar zijn, omdat deze zelfstudie gebaseerd rechtstreeks op die code.

U moet ook Visual Studio 2012 of hoger.


##<a name="template-concepts"></a>Sjabloon concepten

In [Gebruik melding Hubs naar het laatste nieuws verzenden] ingebouwd u een app die **labels** gebruikt om u te abonneren op meldingen voor verschillende nieuwscategorieën.
Veel apps, meerdere markten doelgerichte echter en lokalisatie vereisen. Dit betekent dat de inhoud van de meldingen zelf hoeft te worden gelokaliseerd en verzonden naar de juiste reeks apparaten.
In dit onderwerp wordt wordt uitgelegd hoe de functie te gebruiken **sjabloon** van Hubs melding voor het leveren van eenvoudig gelokaliseerde recente nieuws meldingen.

Opmerking: een manier om gelokaliseerde meldingen te verzenden is het opzetten van meerdere versies van elk naamkaartje. Bijvoorbeeld ter ondersteuning van Engels, Frans en Mandarijn, zou moeten we drie verschillende labels voor wereldnieuws: "world_en", "world_fr," en "world_ch". Zou moeten we vervolgens een gelokaliseerde versie van de wereldnieuws verzenden naar elk van deze tags. In dit onderwerp we sjablonen gebruiken om te voorkomen dat de verspreiding van tags en de vereiste meerdere berichten te verzenden.

Sjablonen zijn op hoog niveau, een manier om te bepalen hoe een bepaald apparaat een melding moet ontvangen. De sjabloon geeft de exacte nettolading-indeling door te verwijzen naar de eigenschappen die deel van het bericht is verzonden door uw app back-enddatabase uitmaken. In ons geval stuurt we een landinstelling-agnostic-bericht met alle ondersteunde talen:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Klik we ervoor dat zorgt dat apparaten met een sjabloon die naar de juiste eigenschap verwijst registreren. Een Windows Store-app die wil ontvangen van een eenvoudige mailpop-bericht wordt bijvoorbeeld registreren voor de volgende sjabloon met alle bijbehorende labels:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Sjablonen zijn krachtige functies die kunt u meer informatie over in onze artikel [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) . 


##<a name="the-app-user-interface"></a>De app-gebruikersinterface

We wordt nu de verbreken nieuws-app die u hebt gemaakt in het onderwerp [Gebruik melding Hubs naar het laatste nieuws verzenden] om te verzenden gelokaliseerde laatste nieuws met sjablonen wijzigen.

In de Windows Store-app:

Uw MainPage.xaml als u wilt opnemen van een keuzelijst landinstelling wijzigen:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>Samenstellen van de Windows Store-app-client

1. Toevoegen als u een parameter landinstelling aan uw methoden *StoreCategoriesAndSubscribe* en *SubscribeToCateories* in uw klas meldingen.

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Houd er rekening mee dat in plaats van het aanroepen van de methode *RegisterNativeAsync* verwijst naar de *RegisterTemplateAsync*: we een specifieke melding-indeling waarin de sjabloon is afhankelijk van de landinstelling registreert. Wij bieden ook een naam voor de sjabloon ("localizedWNSTemplateExample"), omdat we mogelijk wilt registreren van meer dan één sjabloon (bijvoorbeeld een voor mailpop meldingen) en één voor tegels en moeten we ze een naam om te kunnen bijwerken toe of verwijder deze.

    Houd er rekening mee dat als een apparaat wordt meerdere sjablonen geregistreerd met dezelfde tag, een binnenkomend bericht hebt samengesteld die tag tot meerdere meldingen leidt bezorgd bij het apparaat (één voor elke sjabloon). Dit probleem is handig wanneer hetzelfde logische bericht heeft leiden tot meerdere visuele meldingen, bijvoorbeeld met zowel een badge een mailpop in een toepassing voor de Windows Store.

2. De volgende methode voor het ophalen van de opgeslagen landinstelling toevoegen:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. De knop bijwerken in uw MainPage.xaml.cs, handler door te klikken en deze te leveren aan het gesprek door naar de klas meldingen ophalen van de huidige waarde van de landinstelling keuzelijst met invoervak op, zoals wordt weergegeven:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. In het bestand App.xaml.cs Zorg er ten slotte bij uw `InitNotificationsAsync` methode voor het ophalen van de landinstelling en deze gebruiken als u zich abonneert:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>Gelokaliseerde meldingen verzenden vanuit uw back-end

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Laatste nieuws verzenden via melding Hubs]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
