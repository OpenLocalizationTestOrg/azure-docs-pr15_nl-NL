<properties
    pageTitle="Push-meldingen met Azure melding Hubs verzenden op Windows Phone | Microsoft Azure"
    description="In deze zelfstudie leert u hoe u met Azure melding Hubs push-meldingen in een Windows Phone 8 of Windows Phone 8.1 Silverlight-toepassing."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="push-meldingen, push-meldingen, push op windows phone"
    authors="ysxu"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Push-meldingen met Azure melding Hubs verzenden op Windows Phone

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F)voor meer informatie.

Deze zelfstudie wordt getoond hoe u met Azure melding Hubs push-meldingen verzenden naar een Windows Phone 8 of Windows Phone 8.1 Silverlight-toepassing. Als u Windows Phone 8.1 (niet-Silverlight) hebt samengesteld, klikt u vervolgens raadpleegt u de [Windows universele](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) versie.
In deze zelfstudie maakt u een lege app voor Windows Phone 8 die push-meldingen ontvangt met behulp van de Microsoft Push Notification-Service (MPNS). Wanneer u klaar bent, kunt u zult gebruikmaken van uw hub melding uitzenden van push-meldingen naar alle apparaten die uw app uitvoeren.

> [AZURE.NOTE] De melding Hubs Windows Phone SDK biedt geen ondersteuning voor het gebruik van de Windows Push-meldingen Service (WNS) met Windows Phone 8.1 Silverlight-apps. Voer de [Melding Hubs - Windows Phone Silverlight zelfstudie], waarbij gebruik REST API's wordt met apps voor Windows Phone 8.1 Silverlight met WNS (in plaats van MPNS).

De zelfstudie ziet u de eenvoudige broadcast scenario in de melding Hubs gebruiken.

##<a name="prerequisites"></a>Vereisten voor

Deze zelfstudie is het volgende nodig:

+ [Visual Studio 2012 Express voor Windows Phone]of een latere versie.

Voltooien van deze zelfstudie is vereist voor alle andere meldingen Hubs zelfstudies voor Windows Phone 8-apps.

##<a name="create-your-notification-hub"></a>Maken van uw hub-melding

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Klik op het gedeelte <b>Meldingsservices</b> (binnen <i>Instellingen</i>), klikt u op <b>Windows Phone (MPNS)</b> en klik vervolgens op het selectievakje <b>niet-geverifieerde push inschakelen</b> in.</p>
</li>
</ol>

&emsp;&emsp;![Azure-Portal - unathenticated push-meldingen inschakelen](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Uw hub is nu gemaakt en geconfigureerd voor het verzenden van niet-geverifieerde meldingen voor Windows Phone.

> [AZURE.NOTE] Deze zelfstudie gebruikt MPNS in niet-geverifieerde modus. Niet-geverifieerde MPNS-modus wordt geleverd met beperkingen worden ingeschakeld op de meldingen die u naar elke kanaal verzenden kunt. Melding Hubs ondersteunt [MPNS geverifieerd modus](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) doordat u kunt uw certificaat uploaden.

##<a name="connecting-your-app-to-the-notification-hub"></a>Uw app verbinden met de melding hub

1. Maak een nieuwe toepassing voor de Windows Phone 8 in Visual Studio.

    ![Visual Studio - nieuw Project - Windows Phone-App][13]

    In Visual Studio 2013 Update 2 of later kunt maken u in plaats daarvan een Windows Phone Silverlight-toepassing.

    ![Visual Studio - nieuw Project - leeg App - Windows Phone Silverlight][11]

2. Met de rechtermuisknop op de oplossing in Visual Studio, en klik op **NuGet-pakketten beheren**.

    Hiermee worden weergegeven in het dialoogvenster **NuGet-pakketten beheren** .

3. Zoeken naar `WindowsAzure.Messaging.Managed` en klikt u op **installeren**en accepteer de gebruiksvoorwaarden.

    ![Visual Studio - NuGet Package Manager][20]

    Dit is gedownload, installaties en voegt een verwijzing naar de bibliotheek Azure Messaging voor Windows met behulp van het <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet-pakket</a>.

4. Open het bestand App.xaml.cs en voeg de volgende `using` instructies:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. De volgende code hebt toegevoegd aan de bovenkant van **Application_Launching** methode in App.xaml.cs:

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] De waarde **MyPushChannel** is een index die wordt gebruikt voor het opzoeken van een bestaand kanaal in de verzameling [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) . Als er niet er is, maakt u een nieuw item met die naam.
    
    Zorg ervoor dat de naam van de hub en de verbindingsreeks **DefaultListenSharedAccessSignature** dat u hebt opgehaald die in de vorige sectie invoegen.
    Deze code het kanaal URI voor de app opgehaald uit MPNS en vervolgens dat kanaal URI registreert bij uw hub melding. Deze ook zorgt u ervoor dat het kanaal dat URI is geregistreerd in de hub melding telkens wanneer die de toepassing wordt gestart.

    >[AZURE.NOTE]Deze zelfstudie verzendt een mailpop-upmelding naar het apparaat. Wanneer u een melding tegel verzendt, moet u in plaats daarvan de methode **BindToShellTile** aanroepen op het kanaal. Als u beide mailpop ondersteuning en meldingen tegel, belt u zowel **BindToShellTile** als **BindToShellToast**.

6. Vouw in Solution Explorer **Eigenschappen**, opent u de `WMAppManifest.xml` bestand, klik op het tabblad **mogelijkheden** en controleert u of de mogelijkheid **ID_CAP_PUSH_NOTIFICATION** is ingeschakeld.

    ![Visual Studio - Windows Phone App mogelijkheden][14]

    Dit zorgt ervoor dat uw app push-meldingen kan ontvangen. Elke poging een push-bericht verzenden naar de app, zonder, mislukt.

7. Druk op de `F5` toets om de app uitvoeren.

    Een registratiebericht wordt weergegeven in de app.

8. Sluit de app.  

   >[AZURE.NOTE] Als u wilt een mailpop push-bericht ontvangt, moet de toepassing niet worden uitgevoerd op de voorgrond.

##<a name="send-push-notifications-from-your-backend"></a>Push-meldingen verzenden vanuit uw back-end

U kunt de push-meldingen verzenden via melding Hubs van een back-end via de openbare <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST API interface</a>. In deze zelfstudie kunt u met een .NET-console-toepassing push-meldingen verzenden. 

Zie voor een voorbeeld van het push-meldingen verzenden vanuit een ASP.NET-WebAPI backend die geïntegreerd met melding Hubs, [Azure melding Hubs hoogte gebruikers met .NET-end](./notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Voor een voorbeeld van hoe u push-meldingen verzendt met behulp van de [REST API's](https://msdn.microsoft.com/library/azure/dn223264.aspx), raadpleegt u [het gebruik van de melding Hubs uit Java](./notification-hubs-java-push-notification-tutorial.md) en [het gebruik van de melding Hubs van PHP](./notification-hubs-php-push-notification-tutorial.md).

1. Met de rechtermuisknop op de oplossing, selecteer **toevoegen** en een **Nieuw Project...**, en klik vervolgens onder **Visual C#**, klikt u op **Windows** en **Console-toepassing**en klik op **OK**.

    ![Visual Studio - nieuw Project - Console-toepassing][6]

    Hiermee voegt u een nieuwe Visual C# consoletoepassing voor de oplossing. U kunt dit ook doen in een afzonderlijke oplossing.

4. Klik op **Extra**, klik op **Bibliotheek Package Manager**en klik vervolgens op **Package Manager-Console**.

    Hiermee worden de Package Manager-Console weergegeven.

5.  Klik in het venster **Package Manager Console** instellen met de **project standaard** naar uw nieuwe console-toepassingsproject en klik vervolgens in het consolevenster Voer de volgende opdracht uit:

        Install-Package Microsoft.Azure.NotificationHubs

    Hiermee voegt u een verwijzing naar de Azure melding Hubs SDK met het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-pakket</a>.

6. Open de `Program.cs` bestands- en voeg de volgende `using` instructie:

        using Microsoft.Azure.NotificationHubs;

6. In de `Program` klasse, voegt u de volgende methode:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Vervang de `<hub name>` tijdelijke aanduiding met de naam van de hub van de melding die wordt weergegeven in de portal. Vervang de tijdelijke aanduiding voor tekenreeks verbinding ook met de verbindingsreeks **DefaultFullSharedAccessSignature** dat u hebt opgehaald die in de sectie "Configureren uw melding hub."

    >[AZURE.NOTE]Zorg ervoor dat u de verbindingsreeks met **volledige** toegang, access niet **beluisteren gebruikt** . De tekenreeks luisteren toegang heeft geen machtigingen voor het verzenden van push-meldingen.

4. Voeg de volgende regel in uw `Main` methode:

         SendNotificationAsync();
         Console.ReadLine();

5. Met uw Windows Phone-emulator uitgevoerd en uw app gesloten, het console-toepassingsproject als het standaard opstarten-project instellen en druk vervolgens op de `F5` toets om de app uitvoeren.

    U ontvangt een melding van de push mailpop. De app te tikken op de banner mailpop worden geladen.

U kunt de mogelijke nettoladingen vinden in de onderwerpen [mailpop-catalogus] en [tegel catalogus] op MSDN.

##<a name="next-steps"></a>Volgende stappen

In dit voorbeeld eenvoudige uitgezonden u push-meldingen voor al uw Windows Phone 8-apparaten. 

Om u te richten op specifieke gebruikers, raadpleegt u de zelfstudie [Gebruik melding Hubs push-meldingen aan gebruikers] . 

Als u wilt uw gebruikers door rente groepen segmenten, vindt u [Gebruik melding Hubs naar het laatste nieuws verzenden]. 

Meer informatie over het gebruik van de melding Hubs in de [Melding Hubs richtlijnen].



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express voor Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Melding Hubs richtlijnen]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Gebruik van de melding Hubs om push-meldingen aan gebruikers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Laatste nieuws verzenden via melding Hubs]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[mailpop-catalogus]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[tegel-catalogus]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Melding Hubs - Windows Phone Silverlight zelfstudie]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

