<properties
    pageTitle="Offline synchroniseren inschakelen voor uw Azure Mobile-App (Xamarin Android)"
    description="Informatie over het gebruik van App-Service Mobile-App naar offline gegevens in uw toepassing Xamarin Android cache en synchroniseren"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Offline synchronisatie voor de mobiele app van Xamarin.Android inschakelen

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie maakt u kennis met de functie offlinesynchronisatie van Azure Mobile-Apps voor Xamarin.Android. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app--weergeven, toevoegen of wijzigen van gegevens, zelfs als er helemaal geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database.
Als u het apparaat weer online hebt, worden deze wijzigingen worden gesynchroniseerd met de externe service.

In deze zelfstudie kunt u het project client vanaf de zelfstudie [een Xamarin Android-app maken] voor de ondersteuning van de offline functies van Azure Mobile-Apps bijwerken. Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u de gegevens access extensie-pakketten toevoegen aan uw project. Voor meer informatie over de server extensie-pakketten, raadpleegt u [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp [Offline gegevens synchroniseren in de Mobile-Apps Azure]meer informatie over de functie offlinesynchronisatie.

## <a name="update-the-client-app-to-support-offline-features"></a>De client-app ondersteuning offline functies bijwerken

Azure offline Mobile-App-functies kunt u om te communiceren met een lokale database wanneer u in een scenario voor offline bent. Als u wilt deze functies gebruiken in uw app, moet u een [SyncContext] aan een lokale winkel geïnitialiseerd. Vervolgens verwijzingen maken naar de tabel door de interface [IMobileServiceSyncTable] [IMobileServiceSyncTable]. SQLite wordt gebruikt als het lokale archief op het apparaat.

1. Open in Visual Studio, de manager NuGet pakket in het project dat u in deze zelfstudie [maken een Xamarin Android-app] hebt voltooid.  Zoek en installeer het **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket.

2. Open het bestand ToDoActivity.cs en verwijder de opmerkingen bij de `#define OFFLINE_SYNC_ENABLED` definitie.

3. Druk op **F5 opnieuw maken en uitvoeren van de client-app** in Visual Studio. De app werkt hetzelfde als voordat u offline synchronisatie ingeschakeld. De lokale database is echter nu gevuld met gegevens die kunnen worden gebruikt in een scenario voor offline.

## <a name="update-sync"></a>De app bijwerken naar de backend verbreken

In deze sectie, kunt u de verbinding met uw backend Mobile-App kunt u een offline situatie simuleren verbreken. Wanneer u gegevens toevoegt, worden uw uitzonderingshandler vermeld dat de app wordt uitgevoerd in de offlinemodus. In deze status wordt nieuwe items toegevoegd in het lokale archief en zijn gesynchroniseerd met de mobiele app-end wanneer een push wordt uitgevoerd in een verbonden staat.

1. ToDoActivity.cs bewerken in het gedeelde project. De **applicationURL** zodat deze verwijzen naar een ongeldige URL wijzigen:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    U kunt ook offline gedrag aantonen uit te schakelen via Wi-Fi en mobiele netwerken op het apparaat of vliegtuig modus.

2. Druk op **F5** om te maken en uitvoeren van de app. Kennisgeving uw synchronisatie mislukt bij vernieuwen wanneer de app wordt gestart.

3. Voer nieuwe items en u ziet dat push is mislukt met de status van een [CancelledByNetworkError] telkens wanneer die u op **Opslaan**klikt. Echter bestaat de nieuwe taak items niet in het lokale archief totdat ze naar de mobiele app-end verplaatst kunnen.  In een productie-app als u deze uitzonderingen onderdrukken alsof de client-app deze nog steeds verbonden met de mobiele app-end.

4. De app te sluiten en opnieuw om te bevestigen dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.

5. (Optioneel) Open in Visual Studio **Server Explorer**. Ga naar uw database in **Azure wordt aangegeven**->**SQL-Databases**. Met de rechtermuisknop op de database en selecteert u **openen in SQL Server-Object Explorer**. U kunt nu naar uw tabel van de SQL-database en de inhoud ervan bladeren. Controleer of de gegevens in de backend-database niet is gewijzigd.

6. (Optioneel) Een hulpmiddel REST zoals Fiddler of Postman gebruiken om query's in uw mobiele backend, met een GET-query in het formulier `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update van de app uw backend Mobile-App opnieuw verbinding te maken

In dit gedeelte opnieuw verbinding maakt de app op de mobiele app backend. Wanneer u de toepassing voor het eerst uitvoert de `OnCreate` aanroepen gebeurtenis-handler `OnRefreshItemsSelected`. Deze methode roept `SyncAsync` om uw lokale archief met de backend-database te synchroniseren.

1. Open ToDoActivity.cs in het gedeelde project en uw wijziging van de eigenschap **applicationURL** ongedaan maken.

2. Druk op **F5 opnieuw maken en uitvoeren van de app** . De app worden uw lokale wijzigingen gesynchroniseerd met de Mobile-App van Azure-end bewerkingen push en halen wanneer de `OnRefreshItemsSelected` methode wordt uitgevoerd.

3. (Optioneel) De bijgewerkte gegevens met SQL Server-Object Explorer of een hulpmiddel REST zoals Fiddler bekijken. Kennisgeving de gegevens is gesynchroniseerd tussen de backend-database van Azure Mobile-App en het lokale archief.

4. Klik op het selectievakje naast een paar items om ze te voltooien in het lokale archief in de app.

  `CheckItem`oproepen `SyncAsync` naar synchroniseren elke voltooid item met de Mobile-App-end. `SyncAsync`oproepen push zowel halen. **Wanneer u een halen ten opzichte van een tabel die de client heeft gewijzigd, uitvoert een push altijd automatisch wordt uitgevoerd**. Dit zorgt ervoor dat alle tabellen in het lokale archief samen met relaties ongewijzigd. Dit probleem kan resulteren in een onverwachte push. Zie voor meer informatie over dit probleem, [Offline gegevens synchroniseren in Azure Mobile-Apps].

## <a name="review-the-client-sync-code"></a>Controleert u de synchronisatie-code van client

Het Xamarin client project die u hebt gedownload wanneer u de zelfstudie [maken van een Xamarin Android-app] al hebt voltooid bevat code ondersteunende offlinesynchronisatie met een lokale SQLite-database. Hier volgt een beknopt overzicht van wat al in de zelfstudie c ode opgenomen is. Zie voor een overzicht van de functie, [Offline gegevens synchroniseren in Azure Mobile-Apps].

* Voordat de tabelbewerkingen van een kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd. De database lokale archief is geïnitialiseerd wanneer `ToDoActivity.OnCreate()` wordt uitgevoerd `ToDoActivity.InitLocalStoreAsync()`. Deze methode Hiermee maakt u een lokale SQLite database met de `MobileServiceSQLiteStore` class van de Azure Mobile-Apps-client SDK.

    De `DefineTable` methode maakt u een tabel in het lokale archief die overeenkomt met de velden in het opgegeven type, `ToDoItem` in dit geval. Het type heeft geen om op te nemen alle kolommen die in de externe database. Het is mogelijk om op te slaan alleen een subset van kolommen.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }


* De `toDoTable` lid zijn van `ToDoActivity` is van de `IMobileServiceSyncTable` typt u in plaats van `IMobileServiceTable`. De IMobileServiceSyncTable zorgt ervoor dat alle maken, lezen, bijwerken en verwijderen (CRUD) tabelbewerkingen in de lokale archief-database.

    U hebt besloten wanneer wijzigingen zijn verplaatst naar de backend Azure Mobile-App door te bellen `IMobileServiceSyncContext.PushAsync()`. De synchronisatie-context helpt behouden van relaties tussen tabellen met het bijhouden en het uploaden van wijzigingen in alle tabellen een client-app heeft gewijzigd als `PushAsync` wordt genoemd.

    De opgegeven code oproepen `ToDoActivity.SyncAsync()` vereist voor synchronisatie wanneer de lijst todoitem wordt vernieuwd of een todoitem wordt toegevoegd of voltooid. De code gesynchroniseerd na elke lokale wijziging.

    In de opgegeven code, alle records in de externe `TodoItem` tabel gevraagd, maar het is ook mogelijk om te filteren dat records doorgeven van een query-id en query naar `PushAsync`. Zie het gedeelte *Incrementele synchronisatie* in [Offline gegevens synchroniseren in Azure Mobile-Apps]voor meer informatie.

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Aanvullende informatie

* [Offline gegevens synchroniseren in Azure mobiele Apps]
* [Azure mobiele Apps .NET SDK procedure][8]

<!-- URLs. -->
[Een Xamarin Android-app maken]: ../app-service-mobile-xamarin-android-get-started.md
[Offline gegevens synchroniseren in Azure mobiele Apps]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Een Xamarin Android-app maken]: app-service-mobile-xamarin-android-get-started.md
[Offline gegevens synchroniseren in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
