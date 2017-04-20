<properties
    pageTitle="Inschakelen van offlinesynchronisatie voor uw app universele Windows-Platform (UWP) met de Mobile-Apps | Azure App-Service"
    description="Informatie over het gebruik van een Azure Mobile-App met offline cache en synchroniseren in uw app universele Windows-Platform (UWP)."
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>Inschakelen van offlinesynchronisatie voor uw Windows-app

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie ziet u hoe u Offlineondersteuning toevoegt aan een universele Windows-Platform (UWP)-app met een backend Azure Mobile-App. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app--weergeven, toevoegen of wijzigen van gegevens -, zelfs als er helemaal geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Als u het apparaat weer online hebt, worden deze wijzigingen worden gesynchroniseerd met de externe-end.

In deze zelfstudie kunt u het project UWP app uit de zelfstudie [een Windows-app maken] voor de ondersteuning van de offline functies van Azure Mobile-Apps bijwerken. Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u de gegevens access extensie-pakketten toevoegen aan uw project. Voor meer informatie over de server extensie-pakketten, raadpleegt u [werken met .NET endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp [Offline gegevens synchroniseren in de Mobile-Apps Azure]meer informatie over de functie offlinesynchronisatie.

## <a name="requirements"></a>Vereisten

Deze zelfstudie hebt u de volgende minimumvereisten nodig:

* Visual Studio 2013 in Windows 8.1 of hoger.
* De voltooiing van [een Windows-app maken op][een windows-app maakt].
* [Azure Mobile Services SQLite Store][sqlite store nuget]
* [SQLite voor de ontwikkeling van universele Windows-Platform](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>De client-app ondersteuning offline functies bijwerken

Azure offline Mobile-App-functies kunt u om te communiceren met een lokale database wanneer u in een scenario voor offline bent. Als u wilt deze functies gebruiken in uw app, u een [SyncContext] geïnitialiseerd[ synccontext] naar een lokale archief. Vervolgens verwijzen naar de tabel via de [IMobileServiceSyncTable][IMobileServiceSyncTable] -interface. SQLite wordt gebruikt als het lokale archief op het apparaat.

1. Installeer de [SQLite runtime voor het universele Windows-Platform](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).

2. Open in Visual Studio, de manager NuGet pakket voor het UWP app project die u in de zelfstudie [een Windows-app maken] voltooid.
    Zoek en installeer het **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket.

4. Klik in Solution Explorer met de rechtermuisknop op **verwijzingen** > **Verwijzing toevoegen**  >  **Universele Windows** > **extensies**, schakelt u vervolgens zowel **SQLite voor universele Windows-Platform** en **Visuele C++ 2015 Runtime voor universele Windows-Platform-apps**.

    ![Een verwijzing naar SQLite UWP][1]

5. Open het bestand MainPage.xaml.cs en verwijder de opmerkingen bij de `#define OFFLINE_SYNC_ENABLED` definitie.

6. Druk op **F5 opnieuw maken en uitvoeren van de client-app** in Visual Studio. De app werkt hetzelfde als voordat u offline synchronisatie ingeschakeld. De lokale database is echter nu gevuld met gegevens die kunnen worden gebruikt in een scenario voor offline.

## <a name="update-sync"></a>De app bijwerken naar de backend verbreken

In deze sectie, kunt u de verbinding met uw backend Mobile-App kunt u een offline situatie simuleren verbreken. Wanneer u gegevens toevoegt, worden uw uitzonderingshandler vermeld dat de app wordt uitgevoerd in de offlinemodus. In deze status wordt nieuwe items in het lokale archief toegevoegd en wordt gesynchroniseerd met de mobiele app-end wanneer push stand wordt uitgevoerd.

1. App.xaml.cs bewerken in het gedeelde project. Opmerkingen toevoegen uit de initialisatie van de **MobileServiceClient** en voeg de volgende regel, waarin de URL van een ongeldige mobiele app:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    U kunt ook offline gedrag demonstreren door te schakelen via Wi-Fi en mobiele netwerken op het apparaat of vliegtuig-modus gebruiken.

2. Druk op **F5** om te maken en uitvoeren van de app. U ziet u de synchronisatie is mislukt bij vernieuwen wanneer de app wordt gestart.

3. Voer nieuwe items en u ziet dat push is mislukt met de status [CancelledByNetworkError] telkens wanneer die u op **Opslaan**klikt. Echter bestaat de nieuwe taak items niet in het lokale archief totdat ze naar de mobiele app-end verplaatst kunnen.  In een productie-app als u deze uitzonderingen onderdrukken alsof de client-app deze nog steeds verbonden met de mobiele app-end.

4. De app te sluiten en opnieuw om te bevestigen dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.

5. (Optioneel) Open in Visual Studio **Server Explorer**. Ga naar uw database in **Azure wordt aangegeven**->**SQL-Databases**. Met de rechtermuisknop op de database en selecteert u **openen in SQL Server-Object Explorer**. U kunt nu naar uw tabel van de SQL-database en de inhoud ervan bladeren. Controleer of de gegevens in de backend-database niet is gewijzigd.

6. (Optioneel) Een hulpmiddel REST zoals Fiddler of Postman gebruiken om query's in uw mobiele backend, met een GET-query in het formulier `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update van de app uw backend Mobile-App opnieuw verbinding te maken

In deze sectie, kunt u de app op de mobiele app backend herstellen. Deze wijzigingen simuleren vanaf netwerk op de app.

Wanneer u de toepassing voor het eerst uitvoert de `OnNavigatedTo` aanroepen gebeurtenis-handler `InitLocalStoreAsync`. Deze methode beurtelings roept `SyncAsync` om uw lokale archief met de backend-database te synchroniseren. De app probeert om te synchroniseren bij het opstarten.

1. Open App.xaml.cs in het gedeelde project en verwijder de opmerkingen bij uw vorige initialisatie van `MobileServiceClient` gebruiken de juiste de mobiele app-URL.

2. Druk op **F5 opnieuw maken en uitvoeren van de app** . De app worden uw lokale wijzigingen gesynchroniseerd met de Mobile-App van Azure-end bewerkingen push en halen wanneer de `OnNavigatedTo` gebeurtenis-handler wordt uitgevoerd.

3. (Optioneel) De bijgewerkte gegevens met SQL Server-Object Explorer of een hulpmiddel REST zoals Fiddler bekijken. Kennisgeving de gegevens is gesynchroniseerd tussen de backend-database van Azure Mobile-App en het lokale archief.

4. Klik op het selectievakje naast een paar items om ze te voltooien in het lokale archief in de app.

  `UpdateCheckedTodoItem`oproepen `SyncAsync` naar synchroniseren elke voltooid item met de Mobile-App-end. `SyncAsync`oproepen push zowel halen. Echter **Wanneer u een halen ten opzichte van een tabel die de client heeft gewijzigd, uitvoert een push altijd automatisch wordt uitgevoerd**. Dit probleem zorgt ervoor dat alle tabellen in het lokale archief samen met relaties ongewijzigd. Dit probleem kan resulteren in een onverwachte push.  Zie voor meer informatie over dit probleem, [Offline gegevens synchroniseren in Azure Mobile-Apps].


##<a name="api-summary"></a>API-overzicht

Ter ondersteuning van de offline functies van mobiele services, we de interface [IMobileServiceSyncTable] gebruikt en geïnitialiseerd [MobileServiceClient.SyncContext] [ synccontext] met een lokale SQLite-database. Wanneer u offline bent, wordt de normale CRUD-bewerkingen voor Mobile-Apps werken als wanneer de app nog steeds verbonden is terwijl de bewerkingen ten opzichte van het lokale archief plaatsvinden. De volgende manieren worden gebruikt voor het lokale archief synchroniseren met de server:

*  **[PushAsync]** Omdat deze methode een lid van [IMobileServicesSyncContext is], worden wijzigingen in alle tabellen worden verplaatst naar de backend. Alleen records met lokale wijzigingen zijn verzonden naar de server.

* **[PullAsync] ** 
   een halen uit een [IMobileServiceSyncTable]wordt gestart. Wanneer er bijgehouden wijzigingen in de tabel, wordt een impliciete push uitgevoerd om ervoor te zorgen dat alle tabellen in het lokale archief samen met relaties consistent blijven. De parameter *pushOtherTables* besturingselementen of andere tabellen in de context in een impliciete push worden geplaatst. *De queryparameter* duurt een [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
   of OData-queryreeks om de geretourneerde gegevens te filteren. De parameter *queryId* wordt gebruikt om te definiëren incrementele synchroniseren. Zie  [Offline gegevens synchroniseren in Azure Mobile-Apps](app-service-mobile-offline-data-sync.md#how-sync-works)voor meer informatie.

* **[PurgeAsync]** Uw app moet regelmatig belt u deze methode als u wilt verwijderen verouderde gegevens uit het lokale archief. Gebruik de parameter *afdwingen* wanneer u moet verwijderen van de wijzigingen aan die nog niet zijn gesynchroniseerd.

Zie voor meer informatie over deze concepten [Offline gegevens synchroniseren in Azure Mobile-Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Meer informatie

De volgende onderwerpen bieden meer achtergrondinformatie over de functie offlinesynchronisatie van Mobile-Apps:

* [Offline gegevens synchroniseren in Azure mobiele Apps]
* [Azure mobiele Apps .NET SDK procedure][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Offline gegevens synchroniseren in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md
[een windows-app maken]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
