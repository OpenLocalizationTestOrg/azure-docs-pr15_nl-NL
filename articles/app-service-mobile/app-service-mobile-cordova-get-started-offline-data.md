<properties
    pageTitle="Offline synchroniseren inschakelen voor uw Azure Mobile-App (Cordova) | Microsoft Azure"
    description="Informatie over het gebruik van App-Service Mobile-App naar offline gegevens in uw toepassing Cordova cache en synchroniseren"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Offline synchronisatie voor de mobiele app van Cordova inschakelen

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Deze zelfstudie maakt u kennis met de functie offlinesynchronisatie van Azure Mobile-Apps voor Cordova. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er helemaal geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Als u het apparaat weer online hebt, worden deze wijzigingen worden gesynchroniseerd met de externe service.

Deze zelfstudie is gebaseerd op de Cordova quickstart-oplossing voor Mobile-Apps die u hebt gemaakt wanneer u de zelfstudie [start Apache Cordova snelle]voltooit. In deze zelfstudie verandert u de oplossing quickstart offline functies van Azure Mobile-Apps wilt toevoegen. We worden ook de offline-specifieke code in de app gemarkeerd.

Zie het onderwerp [Offline gegevens synchroniseren in de Mobile-Apps Azure]meer informatie over de functie offlinesynchronisatie. Zie voor meer informatie van API-gebruik het [Leesmij] -bestand in de invoegtoepassing.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Offline synchroniseren met de oplossing quickstart toevoegen

De code offline synchroniseren moet worden toegevoegd aan de app. Offline synchronisatie is vereist voor de invoegtoepassing cordova-sqlite-opslag, die automatisch wordt toegevoegd aan uw app wanneer de Mobile-Apps Azure-invoegtoepassing is opgenomen in het project. Het project Quickstart bevat beide van deze plug-ins.

1. Klik in Visual Studio Solution Explorer index.js openen en de volgende code te vervangen

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    Met deze code:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Vervolgens vervangt u de volgende code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    Met deze code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    De voorgaande code toevoegingen de lokale archief starten en definiëren van een lokale tabel die overeenkomt met de kolomwaarden die worden gebruikt in uw Azure back-end. (U hoeft niet alle kolomwaarden in deze code opnemen.)

    Krijgt u een verwijzing naar de context synchroniseren door te bellen **getSyncContext**. De synchronisatie-context helpt relaties tussen tabellen met het bijhouden en het uploaden van wijzigingen in alle tabellen die een client-app heeft gewijzigd wanneer **push** heet behouden.

3. De URL van de toepassing bijwerken naar de URL van uw Mobile-App-toepassing.

4. Vervolgens vervangt u deze code:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    Met deze code:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    De bovenstaande code wordt de synchronisatie-context geïnitialiseerd en wordt vervolgens getSyncTable (in plaats van een getTable) als u een verwijzing naar de lokale tabel.

    Deze code wordt de lokale database voor alle maken, lezen, bijwerken en verwijderen (CRUD) tabelbewerkingen.

    In dit voorbeeld kunt u eenvoudige foutafhandeling op synchronisatieconflicten uitvoeren. Een echte toepassing zou de verschillende fouten zoals netwerkcondities, server conflicten en anderen verwerken. Zie de [steekproef offline synchroniseren]voor voorbeelden van programmacode.

5. Voeg nu deze functie om uit te voeren van de werkelijke synchroniseren.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    U bepalen wanneer u kunt wijzigingen push op de backend Mobile-App door te bellen **push** op het object **syncContext** is gebruikt door de client. Zoals u kunt een oproep naar **syncBackend** toevoegen aan een knop gebeurtenis-handler in de app zoals nieuwe knop synchroniseren of zou u de oproep door naar de functie **addItemHandler** om te synchroniseren wanneer een nieuw item wordt toegevoegd.

##<a name="offline-sync-considerations"></a>Aandachtspunten voor de offline synchroniseren

In de steekproef heet de methode **push** van **syncContext** alleen op app starten in de functie terugbellen voor aanmelding.  In een echte-toepassing, kunt u deze synchroniseren functionaliteit handmatig geactiveerd of wanneer de netwerk-status wordt gewijzigd.

Wanneer een halen is uitgevoerd voor een tabel die al in behandeling lokale updates bijgehouden door de context, wordt deze bewerking halen automatisch een voorgaande context push te starten. Bij het vernieuwen, toe te voegen en voltooien van items in dit voorbeeld kunt u weglaten het gesprek expliciete **push** , aangezien het mogelijk overtollige (eerste controle het [Leesmij-bestand] voor de huidige status van de functie).

Alle records in de tabel externe todoItem gevraagd in de opgegeven code, maar het is ook mogelijk om records te filteren door het doorgeven van een query-id en de query aan **push**. Zie het gedeelte *Incrementele synchronisatie* in [Offline gegevens synchroniseren in Azure Mobile-Apps]voor meer informatie.

## <a name="optional-disable-authentication"></a>(Optioneel) Verificatie uitschakelen

Als u niet al ingesteld verificatie en niet wilt verificatie voordat testen offline synchroniseren instellen, opmerkingen van de functie terugbellen voor aanmelding te plaatsen, maar laat u de code in de functie terugbellen zonder opmerkingen.

De code ziet er als volgt na opmerkingen om de regels login.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

De app wordt nu synchroniseren met de Azure-end wanneer u de app in plaats van wanneer u zich aanmeldt uitvoert.

## <a name="run-the-client-app"></a>De client-app uitvoeren

Met offline synchroniseren nu ingeschakeld, kunt u nu uitvoeren de clienttoepassing ten minste eenmaal op elke platform om de database lokale archief te vullen. Later, wordt u een offline mogelijkheid simuleren en wijzigen van de gegevens in het lokale archief wanneer de app offline is.

## <a name="optional-test-the-sync-behavior"></a>(Optioneel) Het gedrag van de synchronisatie testen

In deze sectie wijzigt u het project client deze de vorm van een offline mogelijkheid met behulp van een ongeldige toepassings-URL voor uw back-end. Wanneer u toevoegt of gegevensitems wijzigt, worden deze wijzigingen in het lokale archief gehouden, maar niet gesynchroniseerd met de backend-gegevensopslag totdat de verbinding hersteld is.

1. In de Verkenner oplossing het projectbestand index.js openen en de URL van de toepassing te laten verwijzen naar een ongeldige URL, zoals de volgende wijzigen:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. Werk de CSP in index.html, `<meta>` element met dezelfde ongeldige URL.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Maken en uitvoeren van de client-app en zoals u ziet dat er een uitzondering wordt geregistreerd in de console wanneer de app probeert te synchroniseren met de backend na login. Een nieuwe items die u hebt toegevoegd aanwezig alleen in het lokale archief totdat ze naar het mobiele backend verplaatst kunnen. De app client alsof is verbonden met de backend, ondersteunende die alle maken, lezen, bijwerken, verwijderd (CRUD) bewerkingen.

4. De app te sluiten en opnieuw om te bevestigen dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.

5. (Optioneel) Gebruik Visual Studio om weer te geven van de tabel Azure SQL-Database om te zien of de gegevens in de backend-database niet is gewijzigd.

    Open in Visual Studio **Server Explorer**. Ga naar uw database in **Azure wordt aangegeven**->**SQL-Databases**. Met de rechtermuisknop op de database en selecteert u **openen in SQL Server-Object Explorer**. U kunt nu naar uw tabel van de SQL-database en de inhoud ervan bladeren.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Optioneel) Het opnieuw verbinding maken met uw mobiele backend testen

In deze sectie wordt u opnieuw verbinding maakt de app op de mobiele backend, die overeenkomt met de app eraan een online staat. Wanneer u zich aanmeldt, wordt gegevens worden gesynchroniseerd met uw mobiele backend.

1. Open index.js en corrigeren van de URL van de toepassing te laten verwijzen naar de juiste URL.

2. Open opnieuw index.html en corrigeren van de URL van de toepassing in de CSP `<meta>` element.

3. Opnieuw maken en uitvoeren van de client-app. De app probeert om te synchroniseren met de mobiele app-end na login. Controleer of dat er geen uitzonderingen worden geregistreerd in de Foutopsporingsconsole.

4. (Optioneel) De bijgewerkte gegevens met SQL Server-Object Explorer of een hulpmiddel REST zoals Fiddler bekijken. Zoals u ziet, dat is de gegevens gesynchroniseerd tussen de backend-database en het lokale archief.

    Zoals u ziet de gegevens is gesynchroniseerd tussen de database en het lokale archief en bevat de items die u hebt toegevoegd terwijl uw app nadat de verbinding is verbroken.

## <a name="additional-resources"></a>Aanvullende informatie

* [Offline gegevens synchroniseren in Azure mobiele Apps]

* [Begeleidende cloud: Offline synchroniseren in Azure mobiele Services] \(Opmerking: de video is ingeschakeld Mobile-Services, maar offline synchroniseren op een soortgelijke manier in Azure Mobile-Apps werkt\)

* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Volgende stappen

* Bekijk meer geavanceerde offline synchroniseren functies zoals conflictoplossing in de [steekproef offline synchroniseren]
* Bekijk de offlinesynchronisatie API verwijzing in het [Leesmij-bestand]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova snel aan de slag]: app-service-mobile-cordova-get-started.md
[LEESMIJ VOOR]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[Voorbeeld van de offline synchroniseren]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Offline gegevens synchroniseren in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md
[Omslag van de cloud: Offline synchroniseren in Azure mobiele Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
