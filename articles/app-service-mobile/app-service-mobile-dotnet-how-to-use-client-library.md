<properties
    pageTitle="Werken met de App Service Mobile-Apps beheerde client-bibliotheek (Windows | Xamarin) | Microsoft Azure"
    description="Informatie over het gebruik van een .NET-client voor Azure App Service Mobile-Apps met apps voor Windows en Xamarin."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Het gebruik van de beheerde-client voor Azure Mobile-Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>Overzicht

Deze handleiding ziet u hoe u veelvoorkomende scenario's met behulp van de bibliotheek beheerde client voor Windows Azure-App Service mobiele Apps voor Windows en Xamarin apps uitvoert. Als u eerder met Mobile-Apps, kunt u overwegen eerst voltooiing van de [Azure Mobile-Apps quickstart] [ 1] zelfstudie. In deze handleiding, richten we ons op de aan de clientzijde beheerde SDK. Meer informatie over de serverzijde SDK's voor Mobile-Apps, raadpleegt u de documentatie voor [.NET Server SDK] [ 2] of de [Node.js Server SDK][3].

## <a name="reference-documentation"></a>Naslagmateriaal

De documentatie bij de client SDK zich bevindt: [Azure mobiele Apps .NET-client verwijzing][4].
U kunt ook een aantal voorbeelden van de client vinden in de [bibliotheek van Azure-voorbeelden GitHub][5].

## <a name="supported-platforms"></a>Ondersteunde platformen

De volgende besturingssystemen worden ondersteund in het .NET-Platform:

* Xamarin Android releases voor API 19 tot en met 24 (KitKat tot en met deze)
* Xamarin iOS loslaat voor iOS 8.0 en hoger
* Universele Windows-Platform
* Windows Phone 8.1
* Windows Phone 8.0, behalve voor de Silverlight-toepassingen

De verificatie 'server-mailstroom' wordt een webweergave gebruikt voor de gepresenteerde gebruikersinterface.  Als het apparaat niet mogen een UI WebView presenteren is, zijn de andere methoden voor verificatie van nodig zijn.  Deze SDK is dus niet geschikt voor controletype of op dezelfde manier beperkte apparaten.

##<a name="setup"></a>Installatie en vereisten

We wordt ervan uitgegaan dat u hebt al gemaakt en de Mobile-App backend-project, waaronder ten minste één tabel gepubliceerd.  In de code die wordt gebruikt in dit onderwerp, de tabel heet `TodoItem` en heeft de volgende kolommen: `Id`, `Text`, en `Complete`. Deze tabel is dezelfde tabel gemaakt wanneer u de [Azure Mobile-Apps quickstart] hebt voltooid.

De bijbehorende getypte aan de clientzijde type in C# is de volgende klasse:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

De [JsonPropertyAttribute] [ 6] wordt gebruikt om te definiëren van de toewijzing *NaamEigenschap* tussen het clienttype en de tabel.

Meer informatie over het maken van tabellen in uw backend Mobile-Apps, raadpleegt u de informatie in het [.NET Server SDK onderwerp] [ 7] of het [onderwerp Node.js Server SDK][8]. Als u uw backend Mobile-App in de Azure-portal met behulp van de QuickStart hebt gemaakt, kunt u ook de instelling **eenvoudig tabellen** gebruiken in de [portal van Azure].

###<a name="how-to-install-the-managed-client-sdk-package"></a>Hoe u: de beheerde client-SDK installeren

Een van de volgende methoden gebruiken bij het installeren van de beheerde client-SDK voor Mobile-Apps van [NuGet][9]:

+ **Visual Studio** Met de rechtermuisknop op uw project, klikt u op **NuGet-pakketten beheren**, zoekt u de `Microsoft.Azure.Mobile.Client` Inpakken en klik vervolgens op **installeren**.

+ **Xamarin Studio** Met de rechtermuisknop op uw project, klikt u op **toevoegen** > **NuGet-pakketten toevoegen**, zoeken naar de `Microsoft.Azure.Mobile.Client `Inpakken en klik vervolgens op **Pakket toevoegen**.

Moet u de volgende instructie met **behulp van** toevoegen in het bestand belangrijkste activiteit:

    using Microsoft.WindowsAzure.MobileServices;

###<a name="symbolsource"></a>Hoe u: werken met symbolen in Visual Studio

De symbolen voor de naamruimte Microsoft.Azure.Mobile zijn beschikbaar op [SymbolSource][10].  Raadpleeg de [instructies SymbolSource] [ 11] SymbolSource integreren met Visual Studio.

##<a name="create-client"></a>De client Mobile-Apps maken

De volgende code maakt de [MobileServiceClient] [ 12] object dat is gebruikt voor toegang tot uw backend Mobile-App.

    var client = new MobileServiceClient("MOBILE_APP_URL");

Vervang in het voorgaande code, `MOBILE_APP_URL` met de URL van de Mobile-App-end, die in het blad voor de backend Mobile-App in de [portal van Azure]is gevonden. Het object MobileServiceClient moet een singleton.

## <a name="work-with-tables"></a>Werken met tabellen

De volgende sectie details kunt zoeken en ophalen van records en de gegevens in de tabel wijzigen.  De volgende onderwerpen vallen:

* [Een tabelverwijzing maken](#instantiating)
* [Querygegevens](#querying)
* [Geretourneerde gegevens filteren](#filtering)
* [Gegevens sorteren](#sorting)
* [Gegevens weergeven op pagina 's](#paging)
* [Specifieke kolommen selecteren](#selecting)
* [Een record op Id opzoeken](#lookingup)
* [Omgaan met zonder type query 's](#untypedqueries)
* [Gegevens invoegen](#inserting)
* [Gegevens bijwerken](#updating)
* [U gegevens verwijdert](#deleting)
* [Conflicten oplossen en optimistische gelijktijdigheid](#optimisticconcurrency)
* [Binden op een Windows-gebruikersinterface](#binding)
* [Het paginaformaat wijzigen](#pagesize)

###<a name="instantiating"></a>Hoe u: een tabelverwijzing maken

De code die toegang heeft tot of gegevens wijzigt in een tabel backend-functies oproepen op de `MobileServiceTable` object. Haal een verwijzing naar de tabel door de ondersteuning voor de methode [GetTable] , als volgt:

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

Het geretourneerde object het getypte serialisatie-objectmodel gebruikt. Een model zonder type serialisatie wordt ook ondersteund. Het volgende voorbeeld [wordt een verwijzing naar een tabel zonder typen]:

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

In query's zonder type, moet u de onderliggende tekenreeks van de OData-query opgeven.

###<a name="querying"></a>Hoe u: Query gegevens uit uw Mobile-App

In deze sectie wordt beschreven hoe actie-query's op de backend Mobile-App, waaronder de volgende functionaliteit:

- [Geretourneerde gegevens filteren](#filtering)
- [Gegevens sorteren](#sorting)
- [Gegevens weergeven op pagina 's](#paging)
- [Specifieke kolommen selecteren](#selecting)
- [Zoeken naar gegevens op ID](#lookingup)

>[AZURE.NOTE]Een server op basis van hoeveelheid werk paginaformaat wordt afgedwongen als u wilt voorkomen dat alle rijen worden geretourneerd.  Paginering blijft standaard verzoeken om grote gegevenssets uit die een negatieve invloed hebben op de service.  Als u wilt teruggaan naar meer dan 50 rijen, gebruikt u de `Skip` en `Take` methode, zoals wordt beschreven in [gegevens weergeven op pagina's].

###<a name="filtering"></a>Hoe u: resultaatgegevens Filter

De volgende code ziet u hoe u gegevens filteren met behulp van een `Where` component in een query. Deze retourneert alle items uit `todoTable` waarvan `Complete` eigenschap is gelijk aan `false`. De functie [waar] is van toepassing een rij predikaat aan de query ten opzichte van de tabel te filteren.

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

U kunt de URI van de aanvraag verzonden naar de backend met behulp van de software van het bericht inspectie, zoals speciale tools voor ontwikkelaars browser of [Fiddler]weergeven. Als u het verzoek URI bekijkt, ziet u dat de query-tekenreeks wordt gewijzigd:

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

Deze OData-aanvraag wordt omgezet in een SQL-query door de Server SDK:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

De functie die wordt doorgegeven aan de `Where` methode kan een willekeurig aantal voorwaarden hebben.

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

In dit voorbeeld zou worden omgezet in een SQL-query door de Server SDK:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

Deze query kan ook worden gesplitst in meerdere componenten:

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

De twee methoden gelijk zijn en door elkaar kunnen worden gebruikt.  De optie voormalige&mdash;van het samenvoegen van meerdere predicaten in een query&mdash;is compacte en aanbevolen.

De `Where` component ondersteunt bewerkingen die worden omgezet in de OData-subset. Bewerkingen bevatten:

* Relationele operatoren (==,! =, <, <, >, > =),
* Rekenkundige operatoren (+, -, /, *, %),
* Nummeren precisie (Math.Floor, Math.Ceiling)
* Tekenreeksfuncties (lengte, subtekenreeks, vervangen, IndexOf, StartsWith, EndsWith)
* Datumeigenschappen van de (jaar, maand, dag, uur, minuut, seconde)
* Toegang tot de eigenschappen van een object, en
* Expressies combineren van deze bewerkingen.

Wanneer u nadenkt wat de Server SDK ondersteunt, kunt u de [documentatie voor OData v3]rekening te houden.

###<a name="sorting"></a>Hoe u: resultaatgegevens sorteren

De volgende code wordt geïllustreerd hoe gegevens sorteren met behulp van een functie [OrderBy] of [OrderByDescending] in de query. Het resultaat bevat items uit `todoTable` gesorteerd oplopend op de `Text` veld.

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="paging"></a>Hoe u: gegevens in pagina's te retourneren

Standaard is retourneert de backend alleen de eerste 50 rijen. U kunt het aantal geretourneerde rijen vergroten door het aanroepen van de methode [uitvoeren] . Gebruik `Take` samen met de methode [overslaan] aanvragen van een specifiek 'pagina' van de totale gegevensset die door de query. De volgende query, wanneer uitgevoerd, retourneert de drie belangrijkste items in de tabel.

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

De volgende gereviseerde query wordt de eerste drie resultaten overgeslagen en geeft als resultaat de volgende drie resultaten. Deze query levert de tweede 'pagina' van gegevens, waarbij het paginaformaat drie items is.

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

De methode [IncludeTotalCount] om het totale aantal voor _alle_ vraagt de records die zou zijn geretourneerd, een paginering/limiet-component opgegeven negeren:

    query = query.IncludeTotalCount();

In een echte wereld-app, kunt u query's die vergelijkbaar is met het voorgaande voorbeeld met een pager besturingselement of een vergelijkbare UI om te navigeren tussen pagina's.

>[AZURE.NOTE]Als u wilt overschrijven de limiet van 50-rij in een backend Mobile-App, moet u ook de [EnableQueryAttribute] toepassen op de openbare GET-methode en het gedrag paginering opgeven. Wanneer op de methode hebt toegepast, wordt het volgende ingesteld het maximale aantal rijen op 1000 geretourneerd:
>
>    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>Hoe u: Selecteer specifieke kolommen

U kunt opgeven welke set eigenschappen wilt opnemen in de resultaten door een [Selecteer] -component toe te voegen aan uw query. De volgende code bevat bijvoorbeeld, het selecteren van slechts één veld en ook selecteren en opmaken van meerdere velden:

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

Zijn alle functies beschreven dusverre toevoegingsmiddel, zodat we houden ze koppelen kunt. Elke gekoppelde aanroep van invloed is op meer van de query. Een voorbeeld van meer:

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>Hoe u: gegevens op ID opzoeken

De functie [LookupAsync] kan worden gebruikt om objecten opzoeken in de database met een bepaalde-ID.

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="untypedqueries"></a>Hoe u: zonder type query's uitvoeren

Bij het uitvoeren van een query met behulp van een tabel zonder typen-object, moet u de OData-queryreeks expliciet opgeven door te bellen [ReadAsync], zoals in het volgende voorbeeld:

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

U terughalen JSON-waarden die u, zoals een eigenschap zak gebruiken kunt. Zie voor meer informatie over JToken en Newtonsoft Json.NET, de site [Json.NET] .

### <a name="inserting"></a>Hoe u: gegevens in een mobiele App backend invoegen

Clienttypen voor alle moeten lid zijn met de naam **Id**, dat wil al dan niet standaard een tekenreeks zeggen bevatten. Deze **Id** is vereist voor het CRUD-bewerkingen uitvoeren en voor offline synchroniseren. De volgende code ziet u hoe u met de methode [InsertAsync] nieuwe rijen in een tabel invoegen. De parameter bevat de gegevens die moeten worden ingevoegd als een .NET-object.

    await todoTable.InsertAsync(todoItem);

Als een unieke aangepaste ID-waarde niet is opgenomen in de `todoItem` tijdens een invoegen, een GUID wordt gegenereerd door de server.
U kunt de gegenereerde Id ophalen door te controleren van het object nadat het gesprek geeft als resultaat.

Als u wilt invoegen zonder type gegevens, kunt u profiteren van Json.NET:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

Hier volgt een voorbeeld met een e-mailadres als een unieke tekenreeks-id:

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>Werken met id-waarden

Mobile-Apps ondersteunt unieke aangepaste tekenreekswaarden voor de kolom van de **id** van de tabel. Een tekenreekswaarde geeft toepassingen gebruiken van aangepaste waarden zoals e-mailadressen of gebruikersnamen voor de-ID.  Tekenreeks-id's bieden u de volgende voordelen:

* Id's worden gegenereerd zonder een retour aan de database.
* Records zijn gemakkelijker om samen te voegen uit verschillende tabellen of databases.
* Id's waarden kunnen beter integreren met de logica van een toepassing.

Wanneer een tekenreekswaarde-ID niet is ingesteld op de record van een ingevoegde, de Mobile-App-end een unieke waarde gegenereerd voor de-ID. U kunt de [Guid.NewGuid] -methode gebruiken om uw eigen ID-waarden, op de client of in de backend te genereren.

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="modifying"></a>Hoe u: gegevens in een backend Mobile-App wijzigen

De volgende code wordt geïllustreerd hoe gebruikt u de methode [UpdateAsync] een bestaande record wordt bijgewerkt met dezelfde ID met nieuwe gegevens. De parameter bevat de gegevens die moeten worden bijgewerkt als een .NET-object.

    await todoTable.UpdateAsync(todoItem);

Als u wilt bijwerken zonder type gegevens, kunt u profiteren van [Json.NET] als volgt:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

Een `id` veld moet worden opgegeven bij het maken van een update. De backend gebruikt de `id` veld aan te geven welke rij bijwerken. De `id` veld kan worden opgehaald uit het resultaat van de `InsertAsync` bellen. Een `ArgumentException` treedt op als u probeert een item bijwerken zonder dat de `id` waarde.

###<a name="deleting"></a>Hoe u: gegevens in een backend Mobile-App verwijderen

De volgende code ziet u hoe de [DeleteAsync] -methode gebruiken om te verwijderen van een bestaand exemplaar. Het exemplaar wordt aangegeven door de `id` veld is ingesteld op de `todoItem`.

    await todoTable.DeleteAsync(todoItem);

Als u wilt verwijderen zonder type gegevens, kunt u profiteren van Json.NET als volgt:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

Wanneer u een verzoek om te verwijderen, moet een ID worden opgegeven. Andere eigenschappen niet worden doorgegeven aan de service of worden genegeerd bij de service. Het resultaat van een `DeleteAsync` oproep is meestal `null`. De ID om door te geven kunt ophalen uit het resultaat van de `InsertAsync` bellen. A `MobileServiceInvalidOperationException` wordt gegenereerd wanneer u probeert een item verwijderen zonder de `id` veld.

###<a name="optimisticconcurrency"></a>Hoe u: gebruik optimistische gelijktijdigheid voor conflictoplossing

Twee of meer klanten mogelijk wijzigingen schrijven naar hetzelfde item op hetzelfde moment. Zonder conflicten op te sporen overschrijft de laatste schrijven vorige updates. **Optimistische gelijktijdigheid besturingselement** wordt ervan uitgegaan dat elke transactie doorvoeren kunt en dus geen van een resource vergrendelen gebruikmaakt.  Voordat u een transactie vastlegt, optimistische gelijktijdigheid besturingselement wordt gecontroleerd of dat er geen andere transactie heeft gewijzigd de gegevens. Als de gegevens zijn gewijzigd, de committing transactie hersteld.

Mobile-Apps optimistische gelijktijdigheid besturingselement door het bijhouden van wijzigingen in het gebruik van elk item ondersteunt de `version` systeem eigenschapkolom die is gedefinieerd voor elke tabel in uw backend Mobile-App. Telkens wanneer een record wordt bijgewerkt, Mobile-Apps Hiermee stelt u de `version` eigenschap voor deze record op een nieuwe waarde. Tijdens de aanvraag voor elke update, de `version` eigenschap van de record die wordt geleverd bij het verzoek wordt vergeleken met dezelfde eigenschap voor de record op de server. Als de versie is uitgevoerd met het verzoek niet overeenkomen met de backend en vervolgens de clientbibliotheek verheft een `MobileServicePreconditionFailedException<T>` uitzondering. Het type wordt geleverd bij de uitzondering is de record van de end met de servers-versie van de record. De toepassing kunt vervolgens deze informatie gebruiken om te bepalen of ze willen uitvoeren van de updateaanvraag opnieuw met de juiste `version` waarde van de end wijzigingen doorvoeren.

Een kolom definiëren op de tabel klasse voor de `version` systeemeigenschap optimistische gelijktijdigheid inschakelen. Bijvoorbeeld:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


Toepassingen met behulp van zonder type tabellen optimistische gelijktijdigheid inschakelen door in te stellen de `Version` markeren op de `SystemProperties` van de tabel als volgt.

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

Naast het inschakelen van optimistische gelijktijdigheid u ook moet onderschept de `MobileServicePreconditionFailedException<T>` uitzondering in uw code bij het aanroepen van [UpdateAsync].  Het conflict oplossen door het toepassen van de juiste `version` naar de bijgewerkte record en de oproep [UpdateAsync] met de opgelost record. De volgende code toont het oplossen van een conflict schrijven eenmaal gedetecteerd:

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

Zie het onderwerp [Offline gegevens synchroniseren in Azure Mobile-Apps] voor meer informatie.

###<a name="binding"></a>Hoe u: binding Mobile-Apps gegevens op een Windows-gebruikersinterface

In dit gedeelte ziet u hoe geretourneerde gegevensobjecten gebruikersinterface-elementen gebruiken in een Windows-app kunt weergeven.  Het volgende voorbeeldcode is gebonden aan de bron van de lijst met een query voor onvoltooide items. De [MobileServiceCollection] Hiermee maakt u een mobiele Apps hoogte binding-siteverzameling.

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Bepaalde besturingselementen in de beheerde runtime ondersteuning voor een interface [ISupportIncrementalLoading]genoemd. Deze interface kunt besturingselementen extra gegevens aanvragen wanneer de gebruiker schuift. Er is een ingebouwde ondersteuning voor deze interface voor universele Windows-apps via [MobileServiceIncrementalLoadingCollection], waarin het aanroepen van de besturingselementen automatisch worden afgehandeld. Gebruik `MobileServiceIncrementalLoadingCollection` in Windows-apps als volgt:

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Om de nieuwe siteverzameling op Windows Phone 8 en "Silverlight" apps gebruiken de `ToCollection` extensie methoden op `IMobileServiceTableQuery<T>` en `IMobileServiceTable<T>`. Bellen om gegevens te laden, `LoadMoreItemsAsync()`.

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

Wanneer u de verzameling gemaakt door te bellen gebruiken `ToCollectionAsync` of `ToCollection`, krijgt u een verzameling die kan worden gekoppeld aan UI-besturingselementen.  Deze verzameling is paginering-bekend.  Aangezien de verzameling is laden van gegevens uit het netwerk, soms laadt, mislukt. Voor het verwerken van dergelijke fouten, overschrijven de `OnException` methode op `MobileServiceIncrementalLoadingCollection` u omgaat met uitzonderingen die resulteert uit oproepen naar `LoadMoreItemsAsync`.

Houd rekening met als uw tabel veel velden heeft, maar u alleen wilt ze in het besturingselement wordt weergegeven. U kunt de richtlijnen in de vorige sectie "[Selecteer specifieke kolommen](#selecting)" weer te geven in de gebruikersinterface van specifieke kolommen selecteren.

###<a name="pagesize"></a>Het paginaformaat wijzigen

Azure Mobile-Apps retourneert maximaal 50 items per aanvraag al dan niet standaard.  U kunt de grootte voor paginering wijzigen door te verhogen van de maximale grootte op de client en server.  Als u wilt de gevraagde pagina vergroten, geef `PullOptions` bij gebruik van `PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

Ervan uitgaande dat u hebt aangebracht de `PageSize` gelijk is aan of groter is dan 100 binnen de server is, retourneert een verzoek om maximaal 100 items.

##<a name="#offlinesync"></a>Werken met Offline tabellen

Een lokale SQLite winkel gegevens offline tabellen gebruiken voor gebruik wanneer u offline bent.  Alle tabel gebeurt ten opzichte van de lokale SQLite opslaan in plaats van de externe server store.  Als u wilt een offline-tabel hebt gemaakt, moet u eerst uw project voorbereiden:

1. Met de rechtermuisknop op de oplossing in Visual Studio > **NuGet-pakketten beheren voor oplossing...**, en vervolgens zoeken naar en installeer het pakket **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet voor alle projecten in de oplossing.

2. (Optioneel) Ter ondersteuning van apparaten met Windows, installeert u een van de volgende SQLite runtime-pakketten:

    * **Windows 8.1 Runtime:** [SQLite voor Windows 8.1]installeren[3].
    * **Windows Phone 8.1:** [SQLite voor Windows Phone 8.1]installeren[4].
    * **Universele Windows-Platform** [SQLite voor de Universal universele Windows]installeren[5].

3. (Optioneel). Voor apparaten met Windows, klikt u op **verwijzingen** > **Verwijzing toevoegen**, vouwt u de **Windows** -map > **extensies**en vervolgens de gewenste **SQLite voor Windows** SDK samen met de **Visuele C++ 2013 Runtime voor Windows** SDK inschakelen.
    De namen SQLite SDK verschillen met elke Windows-platform.

Voordat een tabelverwijzing kan worden gemaakt, moet het lokale archief worden voorbereid:

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

Initialisatie van de Store klaar is normaal gesproken onmiddellijk nadat de client is gemaakt.  De **OfflineDbPath** moet een bestandsnaam die geschikt is voor gebruik op alle platforms die u ondersteunt.  Als het pad een volledig pad is (dat wil zeggen, begint met een slash), wordt dat pad gebruikt.  Als het pad is niet volledig gekwalificeerd, wordt het bestand in een platform-specifieke locatie geplaatst.

* Voor iOS en Android-apparaten is het standaardpad de map "Persoonlijke bestanden".
* Voor apparaten met Windows is het standaardpad de toepassingsspecifieke 'AppData' map.

Een tabelverwijzing kan worden verkregen met de `GetSyncTable<>` methode:

    var table = client.GetSyncTable<TodoItem>();

U hoeft niet te verifiëren als een offline tabel wilt gebruiken.  U hoeft te verifiëren wanneer u met de backend-service communiceren wilt.

###<a name="syncoffline"></a>Synchronisatie van een Offline-tabel

Offline tabellen worden niet gesynchroniseerd met de backend al dan niet standaard.  Synchronisatie is splitsen in twee delen.  U kunt wijzigingen afzonderlijk push nieuwe items worden gedownload.  Hier volgt een methode typische synchroniseren:

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
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

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
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

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

Als het eerste argument bij `PullAsync` is null, incrementele synchroniseren niet wordt gebruikt.  Elke synchronisatiebewerking haalt alle records.

De SDK voert een impliciete `PushAsync()` voordat u records kunnen ophalen.

Conflict afhandeling gebeurt er in een `PullAsync()` methode.  U kunt conflicten handelen op dezelfde manier als online tabellen.  Het conflict is geproduceerd wanneer `PullAsync()` heet in plaats van tijdens het invoegen, bijwerken of verwijderen. Als meerdere conflicten is opgetreden, zijn ze samengevoegd in een enkel MobileServicePushFailedException.  Elke fout afzonderlijk verwerken.

##<a name="#customapi"></a>Werken met een aangepaste API

Een aangepaste API kunt u aangepaste eindpunten, waarbij u kennismaakt serverfunctionaliteit die niet toewijzen aan een invoegen, bijwerken, verwijderen of lezen van bewerking definiëren. Met behulp van een aangepaste API, kunt u hebben meer controle over berichten, inclusief lezen en HTTP berichtkoppen instellen en een berichttekst de indeling dan JSON definiëren.

U kunt een aangepaste API bellen door te bellen een van de methoden [InvokeApiAsync] op de client. De volgende regel met code wordt bijvoorbeeld een POST-aanvraag verzonden naar de **completeAll** API op de backend:

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

Dit formulier is van een gesprek getypte methode en vereist dat het retourtype **MarkAllResult** is gedefinieerd. Getypte en zonder type methoden worden ondersteund.

##<a name="authentication"></a>Gebruikers verifiëren

Mobile-Apps ondersteunt verificatie en machtiging van app-gebruikers met verschillende externe identiteitsproviders: Facebook, Google, Microsoft-Account, Twitter en Azure Active Directory. U kunt machtigingen instellen voor tabellen beperken van toegang voor specifieke bewerkingen aan alleen geverifieerde gebruikers. U kunt ook de identiteit van geverifieerde gebruikers willen implementeren autorisatieregels in server-scripts gebruiken. Zie de zelfstudie [verificatie toevoegen aan uw app]voor meer informatie.

Twee verificatie loopt worden ondersteund: _client-beheerd_ en _server beheerd_ stroom. De stroom server worden beheerd biedt de eenvoudigste verificatie-ervaring, zoals deze is afhankelijk van de provider web verificatie-interface. Als dit is afhankelijk van providerspecifieke apparaat-specifieke SDK's, kunt de stroom client beheerde voor betere integratie met apparaat-specifieke mogelijkheden.

>[AZURE.NOTE] Het is raadzaam een stroom client beheerde gebruiken in uw productie-apps.

Als u wilt verificatie instellen, moet u uw app registreren met een of meer identiteitsprovider.  De identiteitsprovider genereert een client-ID en een geheim client voor de app.  Deze waarden worden vervolgens ingesteld in uw backend Azure App Service verificatie/autorisatie inschakelen.  Volg de gedetailleerde instructies in de zelfstudie [verificatie toevoegen aan uw app]voor meer informatie.

De volgende onderwerpen worden in deze sectie beschreven:

+ [Verificatie-client worden beheerd](#clientflow)
+ [Verificatie server worden beheerd](#serverflow)
+ [Caching van het verificatietoken](#caching)

###<a name="clientflow"></a>Verificatie-client worden beheerd

Uw app kunt onafhankelijk contact opnemen met de identiteitsprovider en geef de resulterende token tijdens het aanmelden met uw backend. Deze stroom client kunt u een functionaliteit voor eenmalige aanmelding om gebruikers te bieden of extra gebruikersgegevens ophalen uit de identiteitsprovider. Client-mailstroom verificatie heeft de voorkeur over het gebruik van de stroom van een server, zoals de identiteitsprovider SDK een meer systeemeigen UX feel biedt en kan voor aanvullende aanpassing.

Voorbeelden zijn beschikbaar voor de volgende client-mailstroom verificatie patronen:

+ [Active Directory-verificatie-bibliotheek](#adal)
+ [Facebook- of Google](#client-facebook)
+ [Live SDK](#client-livesdk)

#### <a name="adal"></a>Gebruikers met Active Directory-verificatie Library verifiëren

U kunt de Active Directory verificatie bibliotheek (ADAL) starten gebruikersverificatie vanuit de client met Azure Active Directory-verificatie.

1. Uw mobiele app backend voor AAD eenmalige aanmelding configureren door de [App-Service voor Active Directory-aanmelding configureren] zelfstudie te volgen. Zorg ervoor dat de optionele stap voor het registreren van een clienttoepassing native uitvoert.
2. In Visual Studio of Xamarin Studio, open het project en voeg een verwijzing naar de `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-pakket. Wanneer u zoekt, voegt u voorlopige versies.
3. De volgende code hebt toegevoegd aan uw toepassing, op basis van het platform dat u werkt. Controleer het volgende vervanging in elke wordt:

    * Vervang **Hier-instantie-invoegen** door de naam van de tenant waarin u uw toepassing ingericht. De opmaak moet https://login.windows.net/contoso.onmicrosoft.com. Deze waarde kan worden gekopieerd vanuit het tabblad van het domein in uw Azure Active Directory in de [klassieke Azure-portal].
    * **Invoegen RESOURCE-ID-hier -** vervangen door de klant-ID voor de mobiele app backend. U kunt de client-ID aanvragen van het tabblad **Geavanceerd** onder **Azure Active Directory-instellingen** in de portal.
    * **Invoegen-CLIENT-ID-hier** vervangen door de client-ID die u hebt gekopieerd uit de systeemeigen clienttoepassing.
    * **Invoegen-REDIRECT-URI-hier** vervangen door een van uw site _/.auth/login/done_ eindpunt, met het schema HTTPS. Deze waarde moet er ongeveer als _https://contoso.azurewebsites.net/.auth/login/done_.

    De code die u nodig hebt voor elk platform volgt:

    **Windows:**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="client-facebook"></a>Eenmalige aanmelding een token van Google- of Facebook

U kunt de stroom client kunt gebruiken, zoals in deze fragment voor Facebook of Google.

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="client-livesdk"></a>Eén aanmeldingsproblemen met Microsoft-Account met de Live SDK

Verificatie van gebruikers, moet u uw app bij het Microsoft-account Developer Center registreren. Configureer de registratiedetails op uw backend Mobile-App. Voor het maken van de registratie van een Microsoft-account en maak verbinding met uw backend Mobile-App, voert u de stappen in [uw app als u wilt gebruiken, een Microsoft-accountaanmelding registreren]. Als u zowel de Windows Store en de Windows Phone 8/Silverlight-versie van uw app hebt, registreren u eerst de Windows Store-versie.

De volgende code wordt geverifieerd met Live SDK en wordt het resulterende token aanmelden bij uw backend Mobile-App.

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

Zie de [SDK van Windows Live] -documentatie voor meer informatie.

###<a name="serverflow"></a>Verificatie server worden beheerd

Nadat u uw identiteitsprovider hebt geregistreerd, belt u de methode [LoginAsync] op de [MobileServiceClient] met de waarde [MobileServiceAuthenticationProvider] van uw provider. De volgende code start bijvoorbeeld een server-mailstroom aanmelden met behulp van Facebook.

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Als u een identiteitsprovider dan Facebook gebruikt, wijzigt u de waarde van [MobileServiceAuthenticationProvider] aan de waarde voor uw provider.

In een stroom server beheert Azure App Service de stroom van de verificatie OAuth door de aanmeldingspagina van de geselecteerde provider weer te geven.  Zodra de identiteit provider retourneert, Azure-Service voor App genereert een token App Service-verificatie. De methode [LoginAsync] retourneert een [MobileServiceUser], waarmee de [gebruikers-id] van de geverifieerde gebruiker zowel de [MobileServiceAuthenticationToken], als JSON web token (JWT). Deze token kan worden opgeslagen in de cache en opnieuw gebruikt totdat het verloopt. Zie [Caching van het verificatietoken](#caching)voor meer informatie.

###<a name="caching"></a>Caching van het verificatietoken

In sommige gevallen kunt u de oproep door naar de methode login na de eerste succesvolle verificatie voorkomen door op te slaan het verificatietoken van de provider.  Windows Store- en UWP-apps kunnen [PasswordVault] gebruiken om de verificatietoken van de huidige in cache na een geslaagde aanmelden, als volgt:

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

De waarde van de gebruikers-id wordt opgeslagen als de gebruikersnaam van de referentie en het token is het opgeslagen als het wachtwoord. Klik op volgende startende, kunt u de **PasswordVault** om in de cache opgeslagen referenties controleren. Het volgende voorbeeld wordt in de cache opgeslagen referenties wanneer ze worden gevonden, en anders probeert te verifiëren nogmaals met de end:

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

Wanneer u zich afmeldt een gebruiker, moet u de opgeslagen referentie, ook als volgt verwijderen:

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

De [Xamarin.Auth] API's gebruik Xamarin apps referenties veilig opslaan in een **Account** -object. Zie voor een voorbeeld van het gebruik van deze API's, het bestand [AuthStore.cs] code in de [steekproef delen van foto's ContosoMoments].

Wanneer u client beheerde verificatie gebruikt, kunt u ook het toegangstoken verkregen van uw provider zoals Facebook of Twitter cache. Deze token kan worden opgegeven voor het aanvragen van een nieuwe verificatietoken van de end, als volgt:

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="pushnotifications"></a>Push-meldingen

De volgende onderwerpen behandelen Push-meldingen:

* [Register voor Push-meldingen](#register-for-push)
* [Een Windows Store-pakket beveiligings-id aanvragen](#package-sid)
* [Met meerdere platforms sjablonen registreren](#register-xplat)

###<a name="register-for-push"></a>Hoe u: registreren voor Push-meldingen

De client Mobile-Apps kunt u te registreren voor push-meldingen met Azure melding Hubs. Als het registreren, krijgt u een handgreep die u uit het platform / regiospecifieke Push-meldingen Service (PNS). U opgeven deze waarde samen met de gewenste tags vervolgens wanneer u de registratie maakt. De volgende code registreert uw Windows-app voor push-meldingen met de Windows melding Service (WNS):

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

Als u zet nieuwe naar WNS, moet u [een Windows Store-pakket beveiligings-id aanvragen](#package-sid).  Zie voor meer informatie over het Windows-apps, waaronder over het registreren voor sjabloon registraties, [push-meldingen toevoegen aan uw app].

Tags aanvragen van de client wordt niet ondersteund.  Tag aanvragen zijn stilte uit registratie verwijderd.
Als u registreren van uw apparaat met labels wilt, maakt u een aangepaste-API waarin de melding Hubs API voor het uitvoeren van de registratie namens wordt gebruikt.  [De aangepaste-API aanroepen](#customapi) in plaats van de `RegisterNativeAsync()` methode.

###<a name="package-sid"></a>Hoe u: verkrijgen van een Windows Store-pakket beveiligings-id

Een pakket beveiligings-id nodig is voor het inschakelen van push-meldingen in Windows Store-apps.  Als u wilt ontvangen een pakket beveiligings-id, uw toepassing te registreren met de Windows Store.

Als u deze waarde:

1. Klik in Visual Studio Solution Explorer met de rechtermuisknop op de Windows Store-app-project, klikt u op **Store** > **App koppelen in de Store...**.
2. In de wizard op **volgende**, meld u aan met uw Microsoft-account, typ een naam voor de app in **reserveren een nieuwe appnaam**en klik op **reserveren**.
3. Nadat de registratie van de app is gemaakt, selecteert u de naam van de app, klik op **volgende**en klik vervolgens op **koppelen**.
4. Meld u aan bij de [Windows ontwikkelaar beheercentrum] met uw Microsoft-Account. Klik onder **Mijn apps**, klikt u op de app-registratie die u hebt gemaakt.
5. Klik op **App-beheer** > **App-identiteit**en schuif omlaag om te zoeken van uw **Pakket beveiligings-id**.

Veel gebruikmaakt van het pakket beveiligings-id behandelen deze als een URI, zodat u moet gebruiken _ms-app: / /_ als het kleurenschema. Noteer de versie van uw pakket beveiligings-id gevormd door deze waarde als een voorvoegsel aaneen te schakelen.

Xamarin apps vereist enkele extra code kunnen registreren van een app waarop de iOS of Android platforms. Zie het onderwerp voor uw platform voor meer informatie:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="register-xplat"></a>Hoe u: Register push sjablonen om meerdere platforms meldingen te verzenden

Als u wilt sjablonen hebt geregistreerd, gebruikt u de `RegisterAsync()` methode met de sjablonen als volgt:

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

Uw sjablonen moeten worden `JObject` typt en meerdere sjablonen in de volgende indeling van JSON kan bevatten:

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

De methode **RegisterAsync()** accepteert ook secundaire tegels:

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

Alle tags zijn te vertalen tijdens de registratie voor waardepapier. Zie voor informatie over het toevoegen van tags op installaties of sjablonen binnen installaties [werken met de .NET-endserver SDK voor Azure Mobile-Apps].

Als u wilt verzenden met behulp van deze sjablonen voor geregistreerde meldingen, raadpleegt u de [Melding Hubs-API's].

##<a name="misc"></a>Diverse onderwerpen

###<a name="errors"></a>Hoe u: fouten

Wanneer een fout in de backend optreedt, de client SDK verheft een `MobileServiceInvalidOperationException`.  Het volgende voorbeeld ziet u hoe u omgaat met een uitzondering die wordt geretourneerd door de backend:

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

Een ander voorbeeld van omgaan met fouten vindt u in de [Mobiele Apps bestanden steekproef]. Het voorbeeld [LoggingHandler] biedt een logboekregistratie gemachtigde handler (volgende) om de verwijzingen naar de backend aanvragen aanmelden.

###<a name="headers"></a>Hoe u: aanpassen kopteksten aanvragen

Ter ondersteuning van uw scenario bepaalde app, moet u mogelijk communicatie met de Mobile-App-end aanpassen. U wilt bijvoorbeeld een aangepaste koptekst toevoegen aan alle uitgaande aanvragen of zelfs wijzigen antwoorden statuscodes. U kunt een aangepaste [DelegatingHandler], zoals in het volgende voorbeeld:

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Verificatie toevoegen aan uw app]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Offline gegevens synchroniseren in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md
[Push-meldingen toevoegen aan uw app]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Uw app als u wilt gebruiken, een Microsoft-accountaanmelding registreren]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Het App-Service configureren voor Active Directory-aanmelding]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[Hiermee maakt u een verwijzing naar een tabel zonder typen]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[Sorteren op]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[Uitvoeren]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Selecteer]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[Overslaan]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Gebruikers-id]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Waar]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure-portal]: https://portal.azure.com/
[Azure klassieke portal]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows-ontwikkelaar Center]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Melding Hubs-API 's]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobiele Apps bestanden steekproef]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[Documentatie voor OData v3]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[Delen van ContosoMoments werknemer]: https://github.com/azure-appservice-samples/ContosoMoments