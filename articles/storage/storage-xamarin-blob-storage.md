<properties
    pageTitle="Het gebruik van Blob Storage uit Xamarin | Microsoft Azure"
    description="De bibliotheek Azure opslag-Client voor Xamarin kunnen ontwikkelaars iOS, Android en Windows Store-apps maken met hun eigen gebruikersinterface. Deze zelfstudie wordt getoond hoe u Xamarin gebruiken om te maken van een toepassing die gebruikmaakt van Azure-blobopslag."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>Het gebruik van Xamarin-blobopslag

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Overzicht

Xamarin kunnen ontwikkelaars gebruik van een gedeelde C# codebase als u wilt maken van iOS, Android en Windows Store-apps met hun eigen gebruikersinterface. Deze zelfstudie wordt getoond hoe u met een toepassing Xamarin Azure-blobopslag. Als u meer informatie over de opslag van Azure wilt, voordat u de code verdiepen, raadpleegt u [Inleiding tot Microsoft Azure Storage](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Maak een nieuwe Xamarin-toepassing

Voor deze aan de slag, maakt we een app die is bedoeld voor Android-, iOS- en Windows. Deze app gewoon maakt u een container en een blob uploaden naar deze container. We voortaan gebruikt Visual Studio op Windows voor deze aan de slag, maar de dezelfde geleerde lessen kunnen worden toegepast bij het maken van een app Xamarin Studio op Mac OS gebruiken.

Volg deze stappen om uw toepassing maken:

1. Als u dat nog niet gedaan hebt, downloadt en installeert u [Xamarin voor Visual Studio](https://www.xamarin.com/download).
2. Visual Studio openen en maken van een lege App (systeemeigen gedeeld): **Bestand > Nieuw > Project > platforms > lege App(Native Shared)**.
3. Met de rechtermuisknop op de oplossing in het deelvenster Oplossingverkenner en selecteer **NuGet-pakketten beheren voor oplossing**. Zoeken naar **WindowsAzure.Storage** en installeer de meest recente versie van de stabiele voor alle projecten in uw oplossing.
4. Maken en uitvoeren van uw project.

U hebt nu een toepassing waarmee u een knop waarmee wordt verhoogd van een item te klikken.

> [AZURE.NOTE] De volgende projecttypen worden momenteel ondersteund door de bibliotheek Azure opslag-Client voor Xamarin: systeemeigen gedeeld, Xamarin.Forms gedeeld Xamarin.Android en Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>Container maken en uploaden blob

Vervolgens u code wilt toevoegen aan de gedeelde klasse `MyClass.cs` die Hiermee maakt u een container en een blob uploads in deze container. `MyClass.cs`ziet er als volgt te werk:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Controleer of te vervangen door uw werkelijke accountnaam en accountsleutel "your_account_name_here" en "your_account_key_here". U kunt deze gedeelde klasse in uw iOS, Android en Windows Phone-toepassing. U kunt gewoon toevoegen `MyClass.createContainerAndUpload()` aan elk project. Bijvoorbeeld:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>Voer de toepassing

Nu kunt u deze toepassing uitvoeren in een Android- of Windows Phone-emulator. U kunt ook deze toepassing uitvoeren in een iOS-emulator, maar dit houdt in dat een Mac. Lees de documentatie om [Visual Studio om een Mac verbinding te maken](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/) voor specifieke instructies over hoe u dit doet,

Wanneer u uw app uitvoert, wordt deze de container maken `mycontainer` in uw account opslag. De label, moet bevatten `myblob`, die de tekst, bevat `Hello, world!`. U kunt dit controleren met behulp van de [Microsoft Azure opslag Explorer](http://storageexplorer.com/).

## <a name="next-steps"></a>Volgende stappen

In deze aan de slag, hebt u geleerd hoe maken van een toepassing voor meerdere platforms in Xamarin met Azure Storage. Deze aan de slag specifiek gericht op een scenario in Blob Storage. Echter, kunt u doen nog veel meer met, niet alleen Blob Storage, maar ook met de tabel, bestand en wachtrij opslag. Check de volgende artikelen voor meer informatie:
- [Aan de slag met Azure-blobopslag met .NET](storage-dotnet-how-to-use-blobs.md)
- [Aan de slag met Azure Table Storage met .NET](storage-dotnet-how-to-use-tables.md)
- [Aan de slag met Azure wachtrij opslagruimte met .NET](storage-dotnet-how-to-use-queues.md)
- [Aan de slag met Azure bestandsopslag in Windows](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]
