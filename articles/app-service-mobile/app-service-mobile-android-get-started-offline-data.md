<properties
    pageTitle="Offline synchroniseren inschakelen voor uw Azure Mobile-App (Android)"
    description="Informatie over het gebruik van App-Service Mobile-Apps naar offline gegevens in uw Android-toepassing cache en synchroniseren"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Inschakelen van offlinesynchronisatie voor uw Android mobiele app

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie behandelt de offline synchronisatiefunctie van Azure Mobile-Apps voor Android. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er helemaal geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Als u het apparaat weer online hebt, worden deze wijzigingen worden gesynchroniseerd met de externe-end.

Als dit uw eerste ervaring met Azure Mobile-Apps, moet u eerst de zelfstudie [maken van een Android-App]uitvoeren. Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u de gegevens access extensie-pakketten toevoegen aan uw project. Voor meer informatie over de server extensie-pakketten, raadpleegt u [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp [Offline gegevens synchroniseren in de Mobile-Apps Azure]meer informatie over de functie offlinesynchronisatie.

## <a name="update-the-app-to-support-offline-sync"></a>De app om te ondersteunen offline synchroniseren bijwerken

Met offline synchroniseren, om te lezen en schrijven uit een *synchronisatietabel* (via de *IMobileServiceSyncTable* -interface), die deel uitmaakt van een **SQLite** database op uw apparaat.

Als u push en wijzigingen tussen het apparaat en Azure Mobile Services halen, moet u een *synchronisatiecontext* (*MobileServiceClient.SyncContext*), die u met de lokale database voor de opslag lokaal gegevens initialisatie gebruiken.

1. In `TodoActivity.java`, de bestaande definitie van commentaar `mToDoTable` en verwijder de opmerkingen bij de versie van de tabel synchroniseren:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. In de `onCreate` methode, de bestaande initialisatie van commentaar `mToDoTable` en verwijder de opmerkingen bij deze definitie:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. In `refreshItemsFromTable` commentaar de definitie van `results` en verwijder de opmerkingen bij deze definitie:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. De definitie van commentaar `refreshItemsFromMobileServiceTable`.

5. Verwijder de opmerkingen bij de definitie van `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Verwijder de opmerkingen bij de definitie van `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>De app testen

In deze sectie, kunt u het gedrag met WiFi testen op, en vervolgens uitschakelen via Wi-Fi een offline scenario te maken.

Wanneer u gegevens toevoegt, worden ze gehouden in de lokale SQLite store, maar u kunt niet gesynchroniseerd met de mobiele service totdat u drukt u op de knop **vernieuwen** . Andere apps mogelijk hebben verschillende vereisten aangaande wanneer gegevens moeten worden gesynchroniseerd, maar voor demodoeleinden deze zelfstudie heeft de gebruiker die expliciet dit aanvraagt.

Als u op deze knop drukt, wordt een nieuwe achtergrondtaak wordt gestart. Deze worden eerst alle wijzigingen in het lokale archief met synchronisatiecontext, wordt de gegevens worden gewijzigd van Azure aan de lokale tabel.

### <a name="offline-testing"></a>Offline testen

1. Plaats het apparaat of simulator in *Vliegtuig modus*. Hiermee maakt u een offline scenario.

2. *Sommige items die moeten worden* toevoegen of sommige items als voltooid markeren. Sluit het apparaat of simulator (of de app automatisch sluiten) en opnieuw starten. Controleer of dat de wijzigingen hebt is vastgelegd, op het apparaat omdat ze worden beheerd in de lokale SQLite store.

3. De inhoud van de Azure *TodoItem* -tabel met een SQL hulpprogramma zoals *SQL Server Management Studio*of een REST-client zoals *Fiddler* of *Postman*weergeven. Controleer de nieuwe items _niet_ zijn gesynchroniseerd met de server

    + Klik voor een backend Node.js, Ga naar de [Azure-portal](https://portal.azure.com/)en klik in de Mobile-App backend op **Eenvoudig tabellen** > **TodoItem** om weer te geven van de inhoud van de `TodoItem` tabel.
    + Voor een backend .NET de tabelinhoud met een SQL-hulpmiddel zoals *SQL Server Management Studio*, of een REST-client zoals *Fiddler* of *Postman*te bekijken.

4. Wi-Fi inschakelen in het apparaat of simulator. Druk vervolgens op de knop **vernieuwen** .

5. De gegevens TodoItem weer in de portal van Azure. De nieuwe en gewijzigde TodoItems wordt nu weergegeven.

## <a name="additional-resources"></a>Aanvullende informatie

* [Offline gegevens synchroniseren in Azure mobiele Apps]

* [Begeleidende cloud: Offline synchroniseren in Azure mobiele Services] \(Opmerking: de video in Mobile-Services, maar offline synchroniseren op een soortgelijke manier in Azure Mobile-Apps werkt\)


<!-- URLs. -->

[Offline gegevens synchroniseren in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md

[Een Android-App maken]: app-service-mobile-android-get-started.md

[Omslag van de cloud: Offline synchroniseren in Azure mobiele Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

