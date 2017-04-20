<properties
    pageTitle="Verbinding maken met Azure Storage in uw app Xamarin.Forms"
    description="Afbeeldingen toevoegen aan de taak lijst Xamarin.Forms mobiele app door te verbinden met Azure-blobopslag"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Verbinding maken met Azure Storage in uw app Xamarin.Forms

## <a name="overview"></a>Overzicht

De Mobile-Apps Azure-client en server SDK ondersteuning voor offlinesynchronisatie van gestructureerde gegevens met CRUD-bewerkingen ten opzichte van het eindpunt /tables. In het algemeen deze gegevens worden opgeslagen in een database of een vergelijkbare store en kunnen deze gegevens winkels grote binaire gegevens in het algemeen efficiënt opslaan. Ook gerelateerde sommige toepassingen gegevens die is opgeslagen elders (bijvoorbeeld-blobopslag, bestandsshares), en is nuttig om te kunnen maken van koppelingen tussen records in het eindpunt /tables en andere gegevens.

In dit onderwerp ziet u hoe u de ondersteuning voor afbeeldingen toevoegen aan de Mobile-Apps doen lijst quickstart. Eerst moet u de zelfstudie [een Xamarin.Forms-app maken op]uitvoeren.

In deze zelfstudie wordt u een opslag-account maken en toevoegen van een verbindingsreeks aan uw backend Mobile-App. U wordt toegevoegd een nieuwe overnemen van het nieuwe Mobile-Apps type `StorageController<T>` aan uw serverproject.

>[AZURE.TIP] Deze zelfstudie heeft een [companion steekproef](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) beschikbaar, dat kan worden toegepast op uw eigen Azure-account. 

## <a name="prerequisites"></a>Vereisten voor

* Hiermee voert u de zelfstudie [een Xamarin.Forms-app maken] , waarin andere vereisten. In dit artikel worden de voltooide app uit deze zelfstudie gebruikt.

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich aanmeldt voor een Azure-account, gaat u naar [De App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile). Hiervoor kunt u direct maken een mobiele app van tijdelijk starter in App Service, geen creditcard vereist en geen afspraken.

## <a name="create-a-storage-account"></a>Een opslag-account maken

1. Maak een account opslag volgens de zelfstudie [een Azure opslag-Account maken]. 

2. Ga naar uw account nieuw gemaakte opslag in de portal Azure en klik op het pictogram **toetsen** . Kopieer de **primaire verbindingsreeks**.

3. Ga naar uw mobiele app backend. Klik onder **Alle instellingen** -> **Toepassingsinstellingen** -> **Tekenreeksen met de verbinding**, maakt u een nieuwe registersleutel met de naam `MS_AzureStorageAccountConnectionString` en gebruik de waarde uit uw opslag-account hebt gekopieerd. **Aangepast** als het type sleutel gebruiken.

## <a name="add-a-storage-controller-to-the-server"></a>Een controller-opslag toevoegen aan de server

U moet een nieuwe domeincontroller toevoegen aan uw serverproject die wordt reageren op verzoeken om een token SA's voor de opslag van Azure, evenals een lijst met bestanden die overeenkomen terug naar een record:

- [Een controller-opslag toevoegen aan uw serverproject](#add-controller-code)
- [Routes geregistreerd door de controller-opslag](#routes-registered)
- [Clients en servers communicatie](#client-communication)

###<a name="add-controller-code"></a>Een controller-opslag toevoegen aan uw serverproject

1. Open in Visual Studio uw project .NET-server. Het pakket Nuget [Microsoft.Azure.Mobile.Server.Files]toevoegen. Zorg ervoor dat u Selecteer **voorlopige versie opnemen**.

2. Open in Visual Studio uw project .NET-server. Met de rechtermuisknop op de map **Controllers** en selecteer **toevoegen** -> **Controller** -> **Web API 2 Controller - leegmaken**. Naam van de controller `TodoItemStorageController`.

3. Voeg de volgende instructies:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. Wijzigen van de klasse base naar `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. De volgende manieren toevoegen aan de klas:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. De configuratie Web API voor het instellen van het kenmerk routering bijwerken. In **Startup.MobileApp.cs**, de volgende regel toe te voegen de `ConfigureMobileApp()` methode, na de definitie van de `config` variabele:

        config.MapHttpAttributeRoutes();

7. Uw serverproject publiceren naar uw mobiele app backend.

###<a name="routes-registered"></a>Routes geregistreerd door de controller-opslag

De nieuwe `TodoItemStorageController` beschrijft twee onderliggend resources onder de record deze beheert:

- StorageToken

    + HTTP-POST: Hiermee maakt u een token opslag
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET: Haalt een lijst met bestanden die zijn gekoppeld aan de record
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + HTTP-verwijderen: Hiermee verwijdert u het bestand dat is opgegeven in de resource-id van bestand
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>Clients en servers communicatie

Houd er rekening mee dat `TodoItemStorageController` wordt *niet* een route voor het uploaden of downloaden van een blob hebt. Dat komt doordat een mobiele client samenwerkt met blob storage *rechtstreeks* om te kunnen uitvoeren van deze bewerkingen, na de eerste aan een token SA's (gedeeld Access handtekening) veilig toegang tot een bepaalde blob of de container. Dit is een belangrijk ontwerp van de architectuur, zoals anders toegang tot opslag door de schaalbaarheid en beschikbaarheid van de mobiele backend beperkt. In plaats daarvan verbinding rechtstreeks met Azure opslag, kunt het mobiele client profiteren van de functies zoals automatisch partitioneren en geografische-verdeling.

Een handtekening gedeelde toegang gedelegeerd toegang geeft tot bronnen in uw account opslag. Dit betekent dat u een client beperkte machtigingen aan objecten in uw account opslagruimte voor een bepaalde termijn van tijd en met een opgegeven reeks machtigingen, zonder dat u moet het delen van uw account toegangstoetsen kunt verlenen. Meer informatie raadpleegt u [Wat gedeeld Access handtekeningen].

De onderstaande afbeelding ziet de client en server interacties. Voordat u een bestand uploadt, vraagt de client een token SA's van de service. De service gebruikt de verbindingsreeks van de opslagruimte voor het genereren van een nieuwe SA's, vervolgens naar de klant retourneert. De SA's is gedurende een beperkte periode en machtigingen tot alleen een bepaald bestand of de container wordt beperkt. De mobiele client vervolgens wordt deze SA's en de opslag van Azure-client SDK het bestand uploaden naar blobopslag.

![Aanvragen van een token SA 's](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Bijwerken van uw client-app als u wilt toevoegen van ondersteuning voor afbeeldingen

Open het project dat Xamarin.Forms quickstart in Visual Studio of Xamarin Studio. U wordt Nuget-pakketten installeren en bijwerken van het project draagbare bibliotheek en de iOS, Android en Windows client projecten:

- [Nuget pakketten toevoegen](#add-nuget)
- [IPlatform interface toevoegen](#add-iplatform)
- [FileHelper klasse toevoegen](#add-filehelper)
- [Een bestand synchronisatie-handler toevoegen](#file-sync-handler)
- [TodoItemManager bijwerken](#update-todoitemmanager)
- [Een detailweergave toevoegen](#add-details-view)
- [De belangrijkste weergave bijwerken](#update-main-view)
- [Het Android project bijwerken](#update-android), [iOS-project](#update-ios), [Windows-project](#update-windows)

>[AZURE.NOTE] Deze zelfstudie werkt alleen met instructies voor de Android-, iOS- en Windows Store platforms, niet Windows Phone.

###<a name="add-nuget"></a>Nuget pakketten toevoegen

Met de rechtermuisknop op de oplossing en selecteer **Nuget beheren-pakketten voor oplossing**. De volgende Nuget-pakketten voor **alle** projecten in de oplossing toevoegen. Zorg ervoor dat u om te controleren **voorlopige versie opnemen**.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

Dit voorbeeld wordt de bibliotheek [PCLStorage] gebruikt voor het gemak, maar deze niet nodig is voor de Mobile-Apps Azure-client SDK.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>IPlatform interface toevoegen

Maak een nieuwe interface `IPlatform` in het project belangrijkste draagbare bibliotheek. Dit komt overeen met het patroon [Xamarin.Forms DependencyService] om te laden van de klas rechts platform / regiospecifieke gedurende runtime. U toevoegen later platform / regiospecifieke implementaties in elk van de client-projecten.

1. Voeg de volgende instructies:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. De toepassing vervangen door het volgende:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>FileHelper klasse toevoegen

1. Maak een nieuwe klasse `FileHelper` in het project belangrijkste draagbare bibliotheek. Voeg de volgende instructies:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. De definitie class toevoegen:

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>Een bestand synchronisatie-handler toevoegen

Maak een nieuwe klasse `TodoItemFileSyncHandler` in het project belangrijkste draagbare bibliotheek. Deze klasse bevat terugbellen uit de SDK Azure uw code waarschuwen wanneer een bestand wordt toegevoegd of verwijderd.

De Azure mobiele Client SDK bestandsgegevens niet worden opgeslagen: de client SDK Hiermee wordt de implementatie van `IFileSyncHandler` die op zijn beurt bepaalt of en hoe bestanden worden opgeslagen op het lokale apparaat.

1. Voeg de volgende instructies:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. De definitie class vervangen door het volgende: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>TodoItemManager bijwerken

1. In **TodoItemManager.cs**, verwijder de opmerkingen bij de regel `#define OFFLINE_SYNC_ENABLED`.

2. In **TodoItemManager.cs**, voeg de volgende instructies:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. In de constructor van `TodoItemManager`, Voeg het volgende na de oproep door naar `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. Vervang de oproep door naar in de constructor `InitializeAsync` met de volgende handelingen uit. Dit zorgt ervoor dat dat er terugbellen wanneer records zijn gewijzigd in het lokale archief. De functie voor het synchroniseren van bestand wordt deze terugbellen gebruikt voor het starten van uw bestand synchronisatie-handler.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. In `SyncAsync()`, Voeg het volgende na de oproep door naar `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. De volgende manieren toevoegen `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>Een detailweergave toevoegen

In deze sectie, wordt u een nieuwe detailweergave voor een taak-item toevoegen. De weergave wordt gemaakt wanneer de gebruiker een item doen selecteert en nieuwe afbeeldingen moet worden toegevoegd aan een item kunt.

1. Een nieuwe klasse **TodoItemImage** toevoegen aan het project draagbare bibliotheek met de volgende implementatie:

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. **App.cs**bewerken. Vervang de initialisatie van `MainPage` met de volgende handelingen uit:
    
        MainPage = new NavigationPage(new TodoList());

3. In **App.cs**, moet u de volgende eigenschap toevoegen:

        public static object UIContext { get; set; }

4. Met de rechtermuisknop op het project draagbare bibliotheek en selecteer **toevoegen** -> **Nieuw Item** -> **platforms** -> **Formulieren Xaml-pagina**. Naam van de weergave `TodoItemDetailsView`.

5. Open **TodoItemDetailsView.xaml** en de hoofdtekst van het ContentPage vervangen door het volgende:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. **TodoItemDetailsView.xaml.cs** bewerken en voeg de volgende instructies:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Vervang de uitvoering van `TodoItemDetailsView` met de volgende handelingen uit:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>De belangrijkste weergave bijwerken 

Werk de belangrijkste weergave de om detailweergave te openen wanneer een item van de taak is geselecteerd.

Vervang in **TodoList.xaml.cs**, de uitvoering van `OnSelected` met de volgende handelingen uit:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Het Android project bijwerken

Platform / regiospecifieke-code toevoegen aan het Android project, inclusief code voor een bestand downloaden en gebruiken van de camera om een nieuwe afbeelding te halen. 

Deze code wordt de Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) de klas rechts platform / regiospecifieke gedurende runtime laden.

1. Voeg het onderdeel **Xamarin.Mobile** aan het project Android.

2. Een nieuwe klasse toevoegen `DroidPlatform` met de volgende implementatie. 'YourNamespace' vervangen door de belangrijkste naamruimte van uw project.

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. **MainActivity.cs**bewerken. In `OnCreate`, Voeg het volgende voordat u de oproep door naar `LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>De iOS-project bijwerken

Platform / regiospecifieke-code toevoegen aan het project iOS.

1. Voeg het onderdeel **Xamarin.Mobile** aan het project iOS.

2. Een nieuwe klasse toevoegen `TouchPlatform` met de volgende implementatie. 'YourNamespace' vervangen door de belangrijkste naamruimte van uw project.

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. **AppDelegate.cs** bewerken en verwijder de opmerkingen bij de oproep door naar `SQLitePCL.CurrentPlatform.Init()`.

###<a name="update-windows"></a>Het Windows-project bijwerken

1. Installeer de Visual Studio-extensie [SQLite voor Windows 8.1](http://go.microsoft.com/fwlink/?LinkID=716919). Zie de zelfstudie [inschakelen van offlinesynchronisatie voor uw Windows-app](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)voor meer informatie. 

2. **Package.appxmanifest** bewerken en controleren van de mogelijkheid **Webcam** .

3. Een nieuwe klasse toevoegen `WindowsStorePlatform` met de volgende implementatie. 'YourNamespace' vervangen door de belangrijkste naamruimte van uw project.

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Overzicht

In dit artikel wordt beschreven hoe u de nieuwe bestandsondersteuning voor het in de Azure mobiele client en server SDK gebruiken om te werken met Azure opslagmedia. 

- Maak een opslag-account en de verbindingsreeks toevoegen aan uw mobiele app backend. Alleen de backend de toets met Azure Storage heeft: de mobiele client vraagt een token SA's (gedeeld Access handtekening) wanneer deze moeten toegang hebben tot Azure opslag. Meer informatie over SA's tokens in Azure opslag, raadpleegt u [Wat gedeeld Access handtekeningen].

- Maak een controller die subcategorieën `StorageController` verwerken van de SA's token aanvragen en om de bestanden die zijn gekoppeld aan een record te gaan. Standaard zijn bestanden gekoppeld aan een record met behulp van de record-ID als onderdeel van de containernaam van de. het gedrag kan worden aangepast dat door het opgeven van een implementatie van `IContainerNameResolver`. Het beleid voor het beveiligingstoken van SA's kan ook zo worden aangepast.

- De Azure mobiele Client SDK worden niet opgeslagen bestandsgegevens worden opgeslagen. In plaats daarvan de client SDK roept uw `IFileSyncHandler`, die vervolgens wordt bepaald hoe (en als) bestanden worden opgeslagen op het lokale apparaat. De synchronisatie-handler is als volgt geregistreerd:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`Wanneer de Azure mobiele Client SDK moet de bestandsgegevens (bijvoorbeeld als onderdeel van het uploadproces) genoemd. Hiermee geeft u de mogelijkheid beheren hoe (en als) bestanden worden opgeslagen op het lokale apparaat en terug te keren die gegevens als dat nodig is.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`wordt aangeroepen als onderdeel van de stroom van de synchronisatie van bestand. Een verwijzing naar een bestand en een waarde van de inventarisatie FileSynchronizationAction worden geleverd, zodat u wat uw toepassing moet gebeuren met die gebeurtenis beslissen kunt (bijvoorbeeld automatisch downloaden van een bestand wanneer deze is gemaakt of bijgewerkt, een bestand verwijderen uit het lokale apparaat wanneer dat bestand wordt verwijderd op de server).

- A `MobileServiceFile` kan worden gebruikt in de modus online of offline met behulp van een `IMobileServiceTable` of `IMobileServiceSyncTable`respectievelijk. In het offline scenario de upload plaatsvindt wanneer de app belt `PushFileChangesAsync`. Hierdoor wordt de wachtrij offline bewerking moeten worden verwerkt; voor elke bestandsbewerking, de client Azure Mobile SDK wordt aangeroepen de `GetDataSource` methode op de `IFileSyncHandler` exemplaar om op te halen van de inhoud van het bestand voor het uploaden.

- Bellen om op te halen van een item bestanden, de '' GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` exemplaar. Deze methode geeft als resultaat een lijst met bestanden die zijn gekoppeld aan het gegevensitem. (Notitie: dit is een *lokale* bewerking en retourneert de bestanden op basis van de status van het object wanneer deze het laatst is gesynchroniseerd. Als u een bijgewerkte lijst met bestanden van de server, u moet een synchronisatiebewerking geïnitieerd eerst.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- De functie voor het synchroniseren van bestand gebruikt record wijzigen; meldingen op het lokale archief om op te halen van de records die de client als onderdeel van een bewerking push of halen ontvangen. Dit is bereikt door te schakelen op de lokale versie en meldingen voor de synchronisatie context met de `StoreTrackingOptions` parameter. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Andere opties voor tracering van store zijn beschikbaar, zoals lokaal alleen-lezen of server alleen-lezen meldingen. U kunt toevoegen of de eigenaar bent van het gebruik van de aangepaste terugbellen de `EventManager` eigenschap van `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- Het is mogelijk toevoegen of verwijderen van bestanden uit een record doordat blobopslag rechtstreeks, aangezien de koppeling wordt bereikt via een naamgevingsconventie. In dit geval moet u echter altijd **bijwerken van de tijdstempel van een record wanneer de bijbehorende BLOB's zijn gewijzigd**. De Azure mobiele client SDK bijgewerkt altijd een record bij toevoegen of verwijderen van een bestand. 

    De reden voor deze vereiste is dat sommige mobiele clients al de record in een lokale opslag heeft. Wanneer deze clients een incrementele halen uitvoert, wordt deze record wordt niet worden geretourneerd en de client wordt niet naar de bijbehorende nieuwe bestanden zoeken. Om te voorkomen dat dit probleem, is het aanbevolen dat u de record tijdstempel bijwerken tijdens het uitvoeren van elke wijziging van blob storage die geen van de Azure mobiele client SDK gebruikmaakt.

- Het project client gebruikt het patroon [Xamarin.Forms DependencyService] de klas rechts platform / regiospecifieke laden tijdens het uitvoeren. In dit voorbeeld hebben we een interface gedefinieerd `IPlatform` met implementaties in elk van de platform-specifieke projecten.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Een Xamarin.Forms-app maken]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[Wat zijn gedeelde Access handtekeningen]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Maak een Account Azure opslag]:  ../storage/storage-create-storage-account.md#create-a-storage-account
