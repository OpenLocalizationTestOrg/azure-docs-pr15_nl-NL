<properties
    pageTitle="Offline synchroniseren inschakelen voor uw Azure Mobile-App (Xamarin.Forms) | Microsoft Azure"
    description="Informatie over het gebruik van App-Service Mobile-App naar offline gegevens in uw toepassing Xamarin.Forms cache en synchroniseren"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Offline synchronisatie voor de mobiele app van Xamarin.Forms inschakelen

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie maakt u kennis met de functie offlinesynchronisatie van Azure Mobile-Apps voor Xamarin.Forms. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app--weergeven, toevoegen of wijzigen van gegevens, zelfs als er helemaal geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Als u het apparaat weer online hebt, worden deze wijzigingen worden gesynchroniseerd met de externe service.

Deze zelfstudie is gebaseerd op de Xamarin.Forms quickstart-oplossing voor Mobile-Apps die u hebt gemaakt wanneer u de zelfstudie [maken een iOS-app Xamarin] voltooit. De oplossing Snelstartgids voor Xamarin.Forms bevat de code voor offline synchroniseren, die moet worden ingeschakeld. In deze zelfstudie kunt u de oplossing quickstart inschakelen van de offline functies van Azure Mobile-Apps bijwerken. We ook markeren de offline-specifieke code in de app. Als u de gedownloade quickstart-oplossing niet gebruikt, moet u de gegevens access extensie-pakketten toevoegen aan uw project. Zie [werken met de .NET-endserver SDK voor Azure Mobile-Apps]voor meer informatie over server extensie-pakketten[1].

Meer informatie over de functie offlinesynchronisatie, Zie het onderwerp [Offline gegevens synchroniseren in de Mobile-Apps Azure][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Functionaliteit van de offline synchroniseren in de oplossing quickstart inschakelen

De code offline synchroniseren is opgenomen in het project met behulp van C# Voorverwerkingsinstructies. Wanneer de **OFFLINE\_SYNCHRONISEREN\_ingeschakeld** symbool is gedefinieerd, deze codepaden deel uitmaken van de build. Voor Windows-apps, moet u ook het platform SQLite installeren.

1. Met de rechtermuisknop op de oplossing in Visual Studio > **NuGet-pakketten beheren voor oplossing...**, en vervolgens zoeken naar en installeer het pakket **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet voor alle projecten in de oplossing.

2. In de Verkenner oplossing open het bestand TodoItemManager.cs van het project met **draagbare** in de naam, dat wil draagbare klassenbibliotheek project zeggen, klikt u vervolgens Verwijder de opmerkingen bij de volgende voorwerkingsinstructie:

        #define OFFLINE_SYNC_ENABLED

4. (Optioneel) Ter ondersteuning van apparaten met Windows, installeert u een van de volgende SQLite runtime-pakketten:

    * **Windows 8.1 Runtime:** [SQLite voor Windows 8.1]installeren[3].
    * **Windows Phone 8.1:** [SQLite voor Windows Phone 8.1]installeren[4].
    * **Universele Windows-Platform** [SQLite voor de Universal universele Windows]installeren[5].

    Hoewel de quickstart geen een universele Windows-project bevat, wordt het universele Windows-platform ondersteund met Xamarin-formulieren.

5. (Optioneel) In elk project-app van Windows, met de rechtermuisknop op **verwijzingen** > **Verwijzing toevoegen**, vouwt u de **Windows** -map > **uitbreidingen**.
    De juiste **SQLite voor Windows** SDK samen met de **Visuele C++ 2013 Runtime voor Windows** SDK inschakelen.
    De namen SQLite SDK verschillen met elke Windows-platform.

## <a name="review-the-client-sync-code"></a>Controleert u de synchronisatie-code van client

Hier volgt een beknopt overzicht van wat is al opgenomen in de Zelfstudievideo code binnen de `#if OFFLINE_SYNC_ENABLED` richtlijnen. De functionaliteit voor offlinesynchronisatie is in het projectbestand TodoItemManager.cs in het project draagbare klassenbibliotheek. Zie [Offline gegevens synchroniseren in Azure Mobile-Apps]voor een overzicht van de functie,[2].

* Voordat de tabelbewerkingen van een kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd. De lokale archief-database is in de klassenconstructor **TodoItemManager** geïnitialiseerd met behulp van de volgende code:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Deze code maakt een nieuwe lokale SQLite database met behulp van de klasse **MobileServiceSQLiteStore** .

    De methode **DefineTable** maakt een tabel in het lokale archief die overeenkomt met de velden in de meegeleverde type.  Het type heeft geen om op te nemen alle kolommen die in de externe database. Het is mogelijk om op te slaan, een subset van kolommen.

* Het veld **todoTable** in **TodoItemManager** is een type **IMobileServiceSyncTable** in plaats van **IMobileServiceTable**. Dit class wordt gebruikt de lokale database voor alle maken, lezen, bijwerken en verwijderen (CRUD) tabelbewerkingen. U besluit wanneer deze wijzigingen zijn verplaatst naar de backend Mobile-App door te bellen **PushAsync** op de **IMobileServiceSyncContext**. De synchronisatie-context helpt relaties tussen tabellen met het bijhouden en het uploaden van wijzigingen in alle tabellen die een client-app heeft gewijzigd wanneer **PushAsync** wordt aangeroepen behouden.

    De volgende **SyncAsync** methode wordt aangeroepen vereist voor synchronisatie met de Mobile-App-end:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    Dit voorbeeld gebruikt eenvoudige foutverwerking met de synchronisatie-standaardhandler. Een echte toepassing zou de verschillende fouten zoals netwerkcondities en server conflicten verwerken met behulp van een aangepaste **IMobileServiceSyncHandler** -implementatie.

## <a name="offline-sync-considerations"></a>Aandachtspunten voor de offline synchroniseren

Alleen in de steekproef, is de methode **SyncAsync** op opstart, en wanneer een synchronisatie wordt aangevraagd genoemd.  Als u wilt een synchronisatie in een Android- of iOS-app starten, halen omlaag in de lijst met items; voor Windows, gebruikt u de knop **synchroniseren** . In een echte-toepassing, kunt u de synchronisatie-trigger wanneer de status van het netwerk wordt gewijzigd.

Wanneer een halen is uitgevoerd voor een tabel die in behandeling heeft lokale updates bijgehouden door de context, die halen bewerking automatisch triggers een voorgaande context push. Bij vernieuwen, toevoegen en voltooien van items in dit voorbeeld, kunt u de expliciete **PushAsync** oproep weglaten.

Alle records in de tabel met externe TodoItem gevraagd in de opgegeven code, maar het is ook mogelijk om records te filteren door het doorgeven van een query-id en de query aan **PushAsync**. Zie voor meer informatie de sectie *Incrementele synchronisatie* in [Offline gegevens synchroniseren in de Mobile-Apps Azure][2].

## <a name="run-the-client-app"></a>De client-app uitvoeren

Ten minste eenmaal uitgevoerd de clienttoepassing op elk platform om de database lokale archief te vullen met offline synchronisatie nu ingeschakeld. Later een offline mogelijkheid simuleren en wijzig de gegevens in het lokale archief wanneer de app offline is.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Het gedrag van de synchronisatie van de client-app bijwerken

In dit gedeelte het client-project deze de vorm van een offline mogelijkheid met behulp van een ongeldige toepassings-URL voor uw back-end te wijzigen. U kunt ook netwerkverbindingen uitschakelen door te verplaatsen van uw apparaat naar "Vliegtuig modus."  Als u gegevensitems toevoegen of wijzigen, zijn deze wijzigingen gehouden in het lokale archief, maar niet gesynchroniseerd met de backend-gegevensopslag totdat de verbinding hersteld is.

1. Klik in de Solution Explorer het projectbestand Constants.cs openen vanuit het **Portable** project en wijzig de waarde van `ApplicationURL` zodat deze verwijzen naar een ongeldige URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. Open het bestand TodoItemManager.cs vanuit het **draagbare** project en vervolgens **onderschept** voor de klasse base **uitzondering** toevoegen aan het tekstblok **try... variabel** in **SyncAsync**. Dit blok **onderschept** schrijft de uitzonderingsbericht op de console, als volgt:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Maken en uitvoeren van de client-app.  Sommige nieuwe items toevoegen. Zoals u ziet dat een uitzondering wordt geregistreerd in de beheerconsole voor elke poging om te synchroniseren met de backend. Deze nieuwe items bestaat niet alleen in het lokale archief totdat ze naar het mobiele backend verplaatst kunnen. De app client alsof deze is verbonden met de backend, ondersteunende die alle maken, lezen, bijwerken, verwijderd (CRUD) bewerkingen.

4. De app te sluiten en opnieuw om te bevestigen dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.

5. (Optioneel) Gebruik Visual Studio om weer te geven van de tabel Azure SQL-Database om te zien of de gegevens in de backend-database niet is gewijzigd.

    Open in Visual Studio **Server Explorer**. Ga naar uw database in **Azure wordt aangegeven**->**SQL-Databases**. Met de rechtermuisknop op de database en selecteert u **openen in SQL Server-Object Explorer**. U kunt nu naar uw tabel van de SQL-database en de inhoud ervan bladeren.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Update van de client-app op uw mobiele backend opnieuw verbinding te maken

In dit gedeelte opnieuw verbinding maakt de app op de mobiele backend, die overeenkomt met de app eraan een online staat. Wanneer u de beweging vernieuwen uitvoert, worden gegevens wordt gesynchroniseerd met uw mobiele backend.

1. Opnieuw openen Constants.cs. Corrigeer de `applicationURL` zodat deze verwijzen naar de juiste URL.

2. Opnieuw maken en uitvoeren van de client-app. De app probeert om te synchroniseren met de mobiele app-end na het starten van. Controleer of dat er geen uitzonderingen worden geregistreerd in de Foutopsporingsconsole.

3. (Optioneel) Weergeven van de bijgewerkte gegevens met SQL Server-Object Explorer of een hulpmiddel REST zoals Fiddler of [Postman][6]. Zoals u ziet, dat is de gegevens gesynchroniseerd tussen de backend-database en het lokale archief.

    Zoals u ziet de gegevens is gesynchroniseerd tussen de database en het lokale archief en bevat de items die u hebt toegevoegd terwijl uw app nadat de verbinding is verbroken.

## <a name="additional-resources"></a>Aanvullende informatie

* [Offline gegevens synchroniseren in Azure mobiele Apps][2]
* [Azure mobiele Apps .NET SDK procedure][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md