<properties
    pageTitle="Het gebruik van de bibliotheek Android mobiele Apps-Client"
    description="Het gebruik van Android-client SDK voor Azure Mobile-Apps."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>Het gebruik van de clientbibliotheek Android-voor Mobile-Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding ziet u hoe u met de Android-client SDK voor Mobile-Apps implementeren veelvoorkomende scenario's, zoals:

- Query's uitvoeren voor gegevens (invoegen, bijwerken en verwijderen).
- Verificatie.
- Fouten afhandelen.
- Het aanpassen van de client. 

Dit doet ook een uitvoerige in algemene clientcode in de meeste mobiele apps gebruikt.

Deze handleiding bevat informatie over de aan de clientzijde Android SDK.  Meer informatie over de serverzijde SDK's voor Mobile-Apps, raadpleegt u [werken met .NET-end SDK] [ 10] of [het gebruik van de backend Node.js SDK][11].

## <a name="reference-documentation"></a>Naslagmateriaal

U vindt de [Javadocs API verwijzing] [ 12] voor de Android clientbibliotheek op GitHub.

## <a name="supported-platforms"></a>Ondersteunde platformen

De Azure Mobile-Apps Android SDK ondersteunt API niveaus 19 tot en met 24 (KitKat tot en met deze).  

De verificatie 'server-mailstroom' wordt een webweergave gebruikt voor de gepresenteerde gebruikersinterface. Als het apparaat niet mogen een UI WebView presenteren is, zijn klikt u vervolgens andere methoden voor verificatie nodig die zich buiten het bereik van het product.  Deze SDK is niet geschikt voor controletype of op dezelfde manier beperkte apparaten.

## <a name="setup-and-prerequisites"></a>Installatie en vereisten

Voltooi de zelfstudie [quickstart Mobile-Apps](app-service-mobile-android-get-started.md) .  Deze taak zorgt ervoor dat alle minimumvereisten voor het ontwikkelen van Azure Mobile-Apps is voldaan.  De Snelstartgids ook kunt u uw account configureren en het maken van uw eerste Mobile-App backend.

Als u niet de zelfstudie Quickstart af te handelen, kunt u de volgende taken uitvoeren:

- [maken van een mobiele App backend] [ 13] voor gebruik met uw Android-app.
- Android Studio, [de Gradle opbouwen bestanden bijwerken](#gradle-build).
- [Internet machtiging inschakelen](#enable-internet).

###<a name="gradle-build"></a>De Gradle opbouwen bestand bijwerken

Beide bestanden **build.gradle** wijzigen:

1. Deze code toevoegen aan *het projectbestand niveau **build.gradle** binnen de tag *buildscript* * :

        buildscript {
            repositories {
                jcenter()
            }
        }

2. Deze code toevoegen aan de *Module app* niveau **build.gradle** -bestand in de tag *afhankelijkheden* :

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    De meest recente versie is momenteel 3.1.0. De ondersteunde versies, vindt u [hier][14].

###<a name="enable-internet"></a>Internet machtiging inschakelen

Voor toegang tot Azure, moet uw app de machtiging INTERNET is ingeschakeld. Als dit nog niet ingeschakeld, moet u de volgende regel met code aan uw bestand **AndroidManifest.xml** besturingselementen toevoegen:

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>De basisbeginselen uitgebreide Kennismaking

In deze sectie worden enkele van de code in de app Quickstart die betrekking heeft op gebruik Azure Mobile-Apps besproken.  

###<a name="data-object"></a>Client gegevensklassen definiëren

Voor toegang tot gegevens uit SQL Azure-tabellen, client gegevensklassen die overeenkomen met de tabellen in de Mobile-App-end te definiëren. Voorbeelden in dit onderwerp wordt ervan uitgegaan dat een tabel met de naam **ToDoItem**, waarin de volgende kolommen heeft:

- ID
- tekst
- voltooien

De bijbehorende getypt aan de clientzijde object:

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

De code bevindt zich in een bestand met de naam van **ToDoItem.java**.

Als uw SQL Azure-tabel meer kolommen bevat, voegt u de corresponderende velden toe aan deze klasse.  Bijvoorbeeld als de DTO (gegevens doorverbinden object) heeft een geheel getal prioriteit kolom en klik vervolgens kunt u dit veld, samen met de methoden getter en setter toevoegen:

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

Als u wilt weten hoe u aanvullende tabellen maken in de Mobile-Apps backend, raadpleegt u [hoe: definiëren van een tabel controller] [ 15] (.NET backend) of [tabellen definiëren met behulp van een dynamische Schema] [ 16] (Node.js backend). U kunt ook de instelling **eenvoudig tabellen** in de [portal van Azure]voor een backend Node.js gebruiken.

###<a name="create-client"></a>Hoe u: maken van de clientcontext

Deze code maakt het **MobileServiceClient** -object dat is gebruikt voor toegang tot uw backend Mobile-App. De code vindt u in de `onCreate` methode van de **activiteit** klasse opgegeven in *AndroidManifest.xml* als een **HOOFDGEGEVEN** actie en het **Startprogramma voor apps** categorie. In de code Quickstart gaat dit in het bestand **ToDoActivity.java** .

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

Vervang in deze code, `MobileAppUrl` met de URL van uw backend Mobile-App, die kan worden gevonden in de [portal van Azure] in het blad voor de backend Mobile-App. Voor deze regel met code compileren, moet u ook de volgende **importeren** -instructie toevoegen:

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>Hoe u: een tabelverwijzing maken

De eenvoudigste manier om de query of wijzigen van gegevens in de backend is met behulp van de die wordt *getypt programming model*, aangezien Java een sterk getypte taal is. Dit model biedt naadloze JSON serialisatie en deserialisatie met de [gson] [ 3] bibliotheek bij het verzenden van gegevens tussen de clientobjecten en -tabellen in de SQL Azure-end.

Voor toegang tot een tabel, maakt u eerst een [MobileServiceTable] [ 8] object door de ondersteuning voor de **getTable** methode op de [MobileServiceClient][9].  Deze methode heeft twee overbelastingen:

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

In de volgende code is **mClient** een verwijzing naar het object MobileServiceClient.  De eerste overbelasting wordt gebruikt, waarbij de klassenaam van de en de naam van de tabel hetzelfde zijn, en wordt het gebruikt in de Snelstartgids:

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

De tweede overbelasting worden gebruikt wanneer u de tabelnaam verschilt van de klassenaam: de eerste parameter is de naam van de tabel.

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>Hoe u: gegevens koppelt aan de gebruikersinterface

Gegevensbinding omvat drie componenten:

- De gegevensbron
- Opmaak van het scherm
- De adapter die de twee elkaar verbindt.

In ons voorbeeldcode rentabiliteit we de gegevens in de Mobile-Apps SQL Azure-tabel **ToDoItem** in een matrix. Deze activiteit is een algemene patroon voor gegevenstoepassingen.  Databasequery's retourneren vaak een verzameling rijen die de client in een lijst of een matrix ontvangt. In dit voorbeeld is de matrix de gegevensbron.

De code wordt een schermindeling die de weergave van de gegevens die wordt weergegeven op het apparaat wordt gedefinieerd.  De twee samen met een adapter, die in deze code een uitbreiding is zijn afhankelijk van de **ArrayAdapter&lt;ToDoItem&gt; ** class.

#### <a name="layout"></a>Hoe u: de indeling definiëren

De indeling is gedefinieerd door verschillende fragmenten van XML-code. Een bestaande indeling gegeven, staat de volgende code voor de **Lijstweergave** we met onze server-gegevens wilt vullen.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

In de voorgaande code bevat het kenmerk *listitem* de id van de indeling voor een afzonderlijke rij in de lijst. Deze code Hiermee geeft u het selectievakje en de bijbehorende tekst en één keer wordt gemaakt voor elk item in de lijst. Deze indeling wordt niet weergegeven voor het veld **id** en een meer complexe lay-out extra velden in de weergave wilt opgeven. Deze code is in het bestand **row_list_to_do.xml** .

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="adapter"></a>Hoe u: de adapter definiëren

Aangezien de gegevensbron van onze weergave een matrix van **ToDoItem is**, we subcategorie onze adapter van een **ArrayAdapter&lt;ToDoItem&gt; ** class. Deze subcategorie genereert een weergave voor elke **ToDoItem** met de **row_list_to_do** -indeling.

In onze code definiëren we de volgende klasse die is een uitbreiding van de **ArrayAdapter&lt;E&gt; ** class:

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

De adapters **getView** methode overschrijven. Bijvoorbeeld:

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

Maak een exemplaar van deze klasse in onze activiteit als volgt:

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

De tweede parameter voor de constructor ToDoItemAdapter is een verwijzing naar de lay-out. We kunnen nu exemplaar van de **Lijstweergave** en de adapter toewijzen aan de **Lijstweergave**.

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>De API-structuur

Mobile-Apps tabelbewerkingen en aangepaste API oproepen zijn asynchroon. Gebruik de objecten [toekomstige] en [AsyncTask] voor de asynchrone methoden met betrekking tot query's, wordt ingevoegd, bijwerken en verwijderen. Gebruik van toekomstige vergemakkelijkt meerdere bewerkingen uitvoeren op een achtergrondthread zonder dat u moet meerdere geneste terugbellen verwerken.

Het bestand **ToDoActivity.java** in het project Android quickstart van de [Azure-portal] voor een voorbeeld bekijken.

#### <a name="use-adapter"></a>Hoe u: Gebruik de adapter

U bent nu klaar voor gebruik van gegevensbinding. De volgende code ziet u hoe u items in de tabel en de lokale adapter ingevuld in de geretourneerde artikelen.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

Bel de adapter op elk gewenst moment dat u de tabel **ToDoItem** wijzigen. Aangezien wijzigingen zijn aangebracht op basis van de record na record, kunt u één rij in plaats van een siteverzameling verwerken. Wanneer u een item hebt ingevoegd, belt u de methode **toevoegen** op de adapter; Wanneer u de verwijdert, belt u de methode **verwijderen** .

##<a name="querying"></a>Hoe u: Query gegevens uit uw backend Mobile-App

In deze sectie wordt beschreven hoe actie-query's op de backend Mobile-App, waaronder de volgende taken uitvoeren:

- [Alle Items retourneren]
- [Geretourneerde gegevens filteren]
- [Gegevens sorteren]
- [Gegevens weergeven op pagina 's]
- [Specifieke kolommen selecteren]
- [Tekst.samenvoegen querymethoden](#chaining)

### <a name="showAll"></a>Hoe u: retourneren alle Items uit een tabel

De volgende query retourneert alle items in de tabel **ToDoItem** .

    List<ToDoItem> results = mToDoTable.execute().get();

De *resultaten* variabele resultaat het instellen door de query als een lijst.

### <a name="filtering"></a>Hoe u: resultaatgegevens Filter

De uitvoering van de volgende query retourneert alle items uit de tabel **ToDoItem** waar **voltooid** is gelijk aan **Onwaar**.

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable** is de verwijzing naar de mobiele servicetabel die we eerder hebt gemaakt.

Een filter met de methode **waar** oproep op de tabelverwijzing definiëren. De methode **waar** wordt gevolgd door een **veld** methode gevolgd door een methode die het logische predikaat aangeeft. Mogelijke predikaatfunctie methoden opnemen **eq** (is gelijk aan), **nieuwe** (niet gelijk aan), **BT** (groter dan), **ge** (groter dan of gelijk aan), **lt** (kleiner dan), **bestand** (kleiner dan of gelijk aan). Deze methoden kunnen u velden getal en tekenreeks toe aan specifieke waarden te vergelijken.

U kunt filteren op datums. De volgende manieren kunnen u de hele date-veld of de onderdelen van de datum vergelijken: **jaar**, **maand**, **dag**, **uur**, **minuut**en **tweede**. Het volgende voorbeeld wordt een filter voor items waarvan de *einddatum* gelijk is aan 2013.

    mToDoTable.where().year("due").eq(2013).execute().get();

De volgende methoden complexe filters ondersteunen op tekenreeksvelden: **startsWith**, **endsWith** **samenvoegen**, **subtekenreeks**, **indexOf**, **vervangen**, **toLower**, **toUpper**, **Knippen**en **lengte**. Het volgende voorbeeld wordt gefilterd voor tabelrijen waar *de tekstkolom* begint met "PRI0."

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

De volgende methoden voor de operator op numerieke velden worden ondersteund: **toevoegen**, **sub** **mul**, **deel**, **mod**, **afronden.beneden**, **ceiling**en **afronden**. Het volgende voorbeeld wordt gefilterd voor tabelrijen waar de **duur van** een even getal is.

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

U kunt predicaten combineren met deze logische methoden: **en**, **of** en **niet**. Het volgende voorbeeld worden twee van de voorgaande voorbeelden gecombineerd.

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

Groeperen en te nesten logische operators gebruiken:

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

Zie [verkennen van de uitgebreide functionaliteit van het model Android client-query](http://hashtagfail.com/post/46493261719/mobile-services-android-querying)voor meer informatie over en voorbeelden van filteren.

### <a name="sorting"></a>Hoe u: resultaatgegevens sorteren

De volgende code retourneert dat alle items uit een tabel van **ToDoItems** gesorteerd oplopend op *het tekstvak* . *mToDoTable* is de verwijzing naar de backend-tabel die u eerder hebt gemaakt:

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

De eerste parameter van de methode **orderBy** is een tekenreeks die gelijk is aan de naam van het veld waarop u wilt sorteren. De tweede parameter gebruikt de opsomming **QueryOrder** aangeven of u oplopend of Aflopend sorteren.  Als u met de methode ***waar*** filtert, moet de methode ***waar*** worden aangeroepen voordat u de methode ***sorteren op*** .

### <a name="paging"></a>Hoe u: gegevens in pagina's te retourneren

Het eerste voorbeeld ziet hoe u de bovenste vijf items selecteren uit een tabel. De query geeft als resultaat de items van een tabel met **ToDoItems**. **mToDoTable** is de verwijzing naar de backend-tabel die u eerder hebt gemaakt:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Hier volgt een query die Hiermee worden de eerste vijf items overgeslagen en wordt de volgende vijf als resultaat:

    mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>Hoe u: Selecteer specifieke kolommen

De volgende code ziet u hoe u alle items uit een tabel van **ToDoItems**terug, maar alleen worden de velden **voltooid** en **tekst** . **mToDoTable** is de verwijzing naar de backend-tabel die we eerder hebt gemaakt.

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

De parameters voor de select-functie zijn de tekenreeksnamen van de kolommen de tabel die u wilt retourneren.

De methode **select** moet volgen methoden zoals **waar** en **sorteren op**. Dit kan worden gevolgd door paginering methoden zoals de **bovenkant**.

### <a name="chaining"></a>Hoe u: Concatenate querymethoden

De methoden gebruikt in query's uitvoeren backend-tabellen kunnen worden samengevoegd. Querymethoden koppelen, kunt u specifieke kolommen met gefilterde rijen die zijn gesorteerd en het selecteren. U kunt complexe logische filters maken.
Elke querymethode retourneert een queryobject. Als u wilt de reeks methoden beëindigen en daadwerkelijk de query uitvoert, belt u de methode **uitvoeren** . Bijvoorbeeld:

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

De gekoppelde querymethoden moeten als volgt worden besteld:

1. Filteren (**waar**) methoden.
2. Sorteermethoden (**orderBy**).
3. (**Selecteer**) selectiemethoden.
4. methoden voor paginering (**overslaan** en **top**).

##<a name="inserting"></a>Hoe u: gegevens in de backend invoegen

Een instantie van de klasse *ToDoItem* en stel de eigenschappen.

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

Vervolgens gebruiken **insert()** om een object invoegen:

    ToDoItem entity = mToDoTable.insert(item).get();

De resulterende entiteit komt overeen met de gegevens die zijn ingevoegd in de tabel backend opgenomen de-ID en andere waarden in de backend instellen.

Mobile-Apps tabellen vereist een primaire-sleutelkolom met de naam **id**. Deze kolom is standaard een tekenreeks. De standaardwaarde van de kolom ID is een GUID.  U kunt andere unieke waarden, zoals e-mailadressen of gebruikersnamen opgeeft. Als een tekenreeks-ID-waarde is niet beschikbaar voor een ingevoegde record, wordt in de backend een nieuwe GUID gegenereerd.

String-ID-waarden bevatten de volgende voordelen:

+ Id's kunnen worden gegenereerd zonder een retour aan de database.
+ Records zijn gemakkelijker om samen te voegen uit verschillende tabellen of databases.
+ Id-waarden integratie betere met logica van een toepassing.

String-ID-waarden zijn **REQUIRED** voor ondersteuning voor offlinesynchronisatie.

##<a name="updating"></a>Hoe u: tabel bijwerken met gegevens in een mobiele app

Als u wilt bijwerken met gegevens in een tabel, geeft u het nieuwe object aan de methode **update()** .

    mToDoTable.update(item).get();

In dit voorbeeld is *item* een verwijzing naar een rij in de tabel *ToDoItem* , die enkele wijzigingen die zijn aangebracht heeft.
De rij met dezelfde **id** wordt bijgewerkt.

##<a name="deleting"></a>Hoe u: gegevens in een mobiele app verwijderen

De volgende code ziet hoe u gegevens uit een tabel verwijderen door het opgeven van het object.

    mToDoTable.delete(item);

U kunt ook een item verwijderen door het opgeven van het veld **id** van de rij te verwijderen.

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="lookup"></a>Hoe u: een specifiek item opzoeken

Een item met een specifieke **id** -veld opzoeken met de methode **lookUp()** :

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="untyped"></a>Hoe u: werken met zonder type gegevens

Het zonder type programming model biedt u exacte controle over JSON serialisatie.  Zijn er enkele veelvoorkomende scenario's waarin u wilt mogelijk een zonder type programming model gebruiken. Bijvoorbeeld als de backend-tabel bevat een groot aantal kolommen en hoeft u alleen te verwijzen naar een subset van de kolommen.  Het getypte model, moet u alle mobiele apps van kolommen in de tabel in uw klas gegevens definiëren.  

De meeste van de API-oproepen voor toegang tot gegevens zijn vergelijkbaar met de getypte programming oproepen. Het belangrijkste verschil is dat in het model zonder type methoden op het object **MobileServiceJsonTable** , in plaats van het object **MobileServiceTable aanroepen** .

### <a name="json_instance"></a>Hoe u: een exemplaar van een tabel zonder typen maken

Vergelijkbaar met het getypte model, u begint met het ophalen van een tabelverwijzing, maar in dit geval is een object **MobileServicesJsonTable** . De verwijzing verkrijgen door de ondersteuning voor de **getTable** methode op een exemplaar van de client:

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Nadat u een exemplaar van de **MobileServiceJsonTable**hebt gemaakt, heeft het vrijwel de dezelfde API die beschikbaar zijn als met het getypte programming model. In sommige gevallen duren de methoden een zonder type parameter in plaats van een getypte parameter.

### <a name="json_insert"></a>Hoe u: invoegen in een tabel zonder typen

De volgende code ziet u hoe u een invoegen. De eerste stap is het opzetten van een [JsonObject][1], onderdeel van de [gson] [ 3] bibliotheek.

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

Gebruik vervolgens **insert()** het zonder type object invoegen in de tabel.

    mJsonToDoTable.insert(jsonItem).get();

Als u nodig hebt om de ID van de ingevoegd object, gebruikt u de methode **getAsJsonPrimitive()** .

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>Hoe u: verwijderen uit een tabel zonder typen

De volgende code ziet hoe u het verwijderen van een exemplaar, in dit geval hetzelfde exemplaar van een **JsonObject** die is gemaakt in het voorbeeld voorafgaande *Invoegen* . De code is hetzelfde als met de getypte hoofdletters/kleine letters, maar de methode heeft een andere handtekening omdat deze verwijst naar een **JsonObject**.

         mToDoTable.delete(jsonItem);

U kunt ook een exemplaar verwijderen via de ID:

         mToDoTable.delete(ID);

### <a name="json_get"></a>Hoe u: alle rijen uit een tabel zonder typen retourneren

De volgende code ziet hoe u een hele tabel ophalen. Aangezien u een tabel JSON gebruikt, kunt u slechts enkele tabelkolommen selectief ophalen.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Dezelfde reeks filteren, filteren en paginering methoden die beschikbaar voor het getypte model zijn zijn beschikbaar voor het zonder type model ook.

##<a name="custom-api"></a>Hoe u: een aangepaste API bellen

Een aangepaste API kunt u aangepaste eindpunten, waarbij u kennismaakt serverfunctionaliteit die niet toewijzen aan een invoegen, bijwerken, verwijderen of lezen van bewerking definiëren. Met behulp van een aangepaste API, kunt u hebben meer controle over berichten, inclusief lezen en HTTP berichtkoppen instellen en een berichttekst de indeling dan JSON definiëren.

Op een Android-client belt u de methode **invokeApi** om te bellen van het aangepaste API-eindpunt. Het volgende voorbeeld ziet u hoe u een API-eindpunt met de naam **completeAll**, die als de klasse van een siteverzameling met de naam **MarkAllResult resultaat**bellen.

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

De methode **invokeApi** heet op de client, die wordt een POST-aanvraag naar de nieuwe aangepaste API verzonden. Het resultaat die het resultaat van de aangepaste API wordt weergegeven in het dialoogvenster van een bericht, zoals eventuele fouten zijn. Andere versies van **invokeApi** kunnen u desgewenst verzenden van een object in het hoofdgedeelte van de aanvraag, de HTTP-methode opgeven en queryparameters met het verzoek verzenden. Zonder type versies van **invokeApi** worden ook gegeven.

##<a name="authentication"></a>Hoe u: verificatie toevoegen aan uw app

Het toevoegen van deze functies beschreven zelfstudies al in detail.

App Service ondersteuning biedt voor [verificatie van app-gebruikers](app-service-mobile-android-get-started-users.md) met een verscheidenheid aan externe identiteitsproviders: Facebook, Google, Microsoft-Account, Twitter en Azure Active Directory. U kunt machtigingen instellen voor tabellen beperken van toegang voor specifieke bewerkingen aan alleen geverifieerde gebruikers. U kunt ook de identiteit van geverifieerde gebruikers willen autorisatieregels implementeren in uw backend.

Twee verificatie loopt worden ondersteund: de stroom van een **server** en de stroom van een **client** . De stroom server biedt de eenvoudigste verificatie-ervaring, zoals deze is afhankelijk van de identiteit providers web-interface.  Geen extra SDK's zijn vereist voor het implementeren van stroom serververificatie. Stroom serververificatie biedt geen een naadloze integratie naar het mobiele apparaat en alleen wordt aanbevolen voor het bewijs van concept scenario's.

Als dit is afhankelijk van SDK's verstrekt door de identiteitsprovider, kunt de stroom van de client voor betere integratie met apparaat-specifieke functies zoals eenmalige aanmelding.  U kunt bijvoorbeeld de SDK Facebook integreren in uw mobiele toepassing.  De mobiele client verwisselt in de Facebook-app en bevestigt uw aanmelding voordat u terug naar uw mobiele app verwisselen.

Vier stappen zijn vereist verificatie in uw app inschakelen:

- Uw app voor verificatie met een identiteitsprovider registreren.
- Uw backend App Service configureren.
- Tabelmachtigingen aan geverifieerde gebruikers alleen op de App Service-end beperken.
- Verificatiecode toevoegen aan uw app.

U kunt machtigingen instellen voor tabellen beperken van toegang voor specifieke bewerkingen aan alleen geverifieerde gebruikers. U kunt ook de beveiligings-id van een geverifieerde gebruiker gebruiken voor het wijzigen van aanvragen.  Bekijk [aan de slag met verificatie] en de Server SDK procedure documentatie voor meer informatie.

### <a name="caching"></a>Hoe u: verificatiecode toevoegen aan uw app

De volgende code begint een server-mailstroom login proces met de Google-provider:

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

De ID van de aangemelde gebruiker verkrijgen door een **MobileServiceUser** met behulp van de methode **getUserId** . Zie [aan de slag met verificatie]voor een voorbeeld van hoe u toekomstige gebruiken om te bellen van de asynchrone login API's.

### <a name="caching"></a>Hoe u: verificatietokens in Cache

In cache opslaan verificatietokens, moet u de gebruikers-ID en verificatietoken lokaal opslaan op het apparaat. De volgende keer dat de app wordt gestart, schakelt u de cache en als deze waarden aanwezig zijn, kunt u het logboek in procedure overslaan en rehydrate van de client met deze gegevens. Deze gegevens is echter gevoelige en moeten worden opgeslagen versleuteld voor de veiligheid geval de telefoon wordt gestolen.

U kunt een volledig voorbeeld van hoe naar cache verificatietokens in [Cache verificatie tokens sectie]zien[7].

Wanneer u probeert te gebruiken van een verlopen token, ontvangt u een reactie *401 geen toestemming* . Verificatiefouten filters gebruiken, kunt u verwerken.  Filters snijpunt aanvragen voor de App Service-end. De filtercode het antwoord voor een 401 getest, activeert het aanmeldingsproces en wordt de aanvraag die de 401 gegenereerd hervat. 

## <a name="adal"></a>Hoe u: gebruikers met Active Directory-verificatie Library verifiëren

U kunt de Active Directory verificatie bibliotheek (ADAL) gebruikers aan te melden bij uw-toepassing via Azure Active Directory. Met een client stroom aanmelding is het handiger voor het gebruik van de `loginAsync()` methoden zoals deze biedt een meer systeemeigen UX feel en kan voor aanvullende aanpassing.

1. Configureer uw backend mobiele app voor aanmelding AAD aan de hand van de zelfstudie voor [het configureren van App-Service voor Active Directory-aanmelding](app-service-mobile-how-to-configure-active-directory-authentication.md) . Zorg ervoor dat de optionele stap voor het registreren van een clienttoepassing native uitvoert.

2. Installeer ADAL door uw bestand build.gradle als u wilt opnemen van de volgende definities te wijzigen:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. De volgende code hebt toegevoegd aan uw toepassing, waardoor de vervanging van de volgende:

* Vervang **Hier-instantie-invoegen** door de naam van de tenant waarin u uw toepassing ingericht. De opmaak moet https://login.windows.net/contoso.onmicrosoft.com. Deze waarde kan worden gekopieerd vanuit het tabblad van het domein in uw Azure Active Directory in de [Azure klassieke portal].

* **Invoegen RESOURCE-ID-hier -** vervangen door de klant-ID voor de mobiele app backend. U kunt de client-ID aanvragen van het tabblad **Geavanceerd** onder **Azure Active Directory-instellingen** in de portal.

* **Invoegen-CLIENT-ID-hier** vervangen door de client-ID die u hebt gekopieerd uit de clienttoepassing native.

* **Invoegen-REDIRECT-URI-hier** vervangen door een van uw site _/.auth/login/done_ eindpunt, met het schema HTTPS. Deze waarde moet er ongeveer als _https://contoso.azurewebsites.net/.auth/login/done_.

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>Hoe u: push-bericht toevoegen aan uw app

U kunt [een overzicht lezen] [ 6] die wordt beschreven hoe Microsoft Azure melding Hubs ondersteunt een groot aantal push-meldingen.  In [deze zelfstudie][5], een push-bericht wordt verzonden naar alle apparaten telkens wanneer een record wordt ingevoegd.

## <a name="how-to-add-offline-sync-to-your-app"></a>Hoe u: offline synchroniseren naar uw app toevoegen

De zelfstudie Quickstart bevat code die wordt geïmplementeerd offline synchroniseren. Let op code voorafgegaan door opmerkingen:

    // Offline Sync

U kunt offline synchroniseren implementeren door de volgende programmacoderegels uncommenting en kunt u vergelijkbare code toevoegen aan andere code Mobile-Apps.

##<a name="customizing"></a>Hoe u: aanpassen van de client

Er zijn verschillende manieren u kunt het standaardgedrag van de client aanpassen.

### <a name="headers"></a>Hoe u: aanpassen kopteksten aanvragen

Een **ServiceFilter** als u wilt toevoegen van een aangepaste HTTP-kop aan elk verzoek om een configureren:

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>Hoe u: serialisatie aanpassen

De client wordt ervan uitgegaan dat de namen van tabellen, kolomnamen en gegevens op de backend alle overeenkomen met exact de gegevensobjecten die zijn gedefinieerd in de client is getypt. Er zijn een aantal redenen waarom de namen van de server en client voor het niet mogelijk overeenkomen. Misschien wilt u doen van de volgende soorten aanpassingen in uw scenario:

- De kolomnamen in de tabel App Service gebruikt, niet overeenkomen met de namen die u in de client gebruikt.
- Gebruik een App Service-tabel met een andere naam dan de klas die deze wordt toegewezen aan in de client.
- Schakel automatische eigenschap hoofdlettergebruik.
- Complexe eigenschappen toevoegen aan een object.

### <a name="columns"></a>Hoe u: verschillende clients en servers namen toewijzen

Stel dat uw code van de client Java standaardnamen Java-stijl voor de eigenschappen van het object **ToDoItem** , zoals de volgende eigenschappen gebruikt:

- mId
- mText
- mComplete
- AANG.duur

De clientnamen serialiseren in JSON-namen die overeenkomen met de kolomnamen van de tabel **ToDoItem** op de server. De volgende code wordt de [gson] [ 3] -bibliotheek met de eigenschappen van aantekeningen voorzien:

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="table"></a>Hoe u: verschillende tabelnamen tussen de client en de backend toewijzen

De naam van de tabel client toewijzen aan de naam van een andere mobiele services-tabel met behulp van een vervanging van de [getTable()] [ 4] methode:

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>Hoe u: naam kolomtoewijzingen automatiseren

U kunt opgeven dat een conversie strategie die wordt toegepast op elke kolom met de [gson] [ 3] API. De clientbibliotheek Android gebruikt [gson] [ 3] achter de schermen Java-objecten naar JSON gegevens serialiseren voordat de gegevens naar Azure App-Service wordt verzonden.  De volgende code wordt de **setFieldNamingStrategy()** methode gebruikt voor het instellen van de strategie. In dit voorbeeld wordt het eerste teken (een "m"), verwijderen en klik vervolgens kleine het volgende teken, voor elke veldnaam. Bijvoorbeeld: deze zou veranderen "deel" in "id".

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

Deze code moet worden uitgevoerd voordat u de **MobileServiceClient**gebruikt.

### <a name="complex"></a>Hoe u: de eigenschap van een object of een matrix opslaan in een tabel

Voorbeelden van onze serialisatie hebt dusverre primitieve typen zoals gehele getallen en tekenreeksen betrokken.  Primitieve typen serialiseren eenvoudig naar JSON.  Als u wilt toevoegen van een complexe object dat niet automatisch serialiseren aan JSON, moeten we bieden de JSON serialisatie-methode.  U kunt een voorbeeld van hoe u aangepaste JSON serialisatie bekijken het blogbericht [aanpassen serialisatie met de bibliotheek gson in de Mobile Services Android gewoon][2].

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Alle Items retourneren]: #showAll
[Geretourneerde gegevens filteren]: #filtering
[Gegevens sorteren]: #sorting
[Gegevens weergeven op pagina 's]: #paging
[Specifieke kolommen selecteren]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure-portal]: https://portal.azure.com
[Aan de slag met verificatie]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Toekomstige]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html