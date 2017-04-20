<properties
    pageTitle="Geografische omheind push-meldingen met Azure melding Hubs en ruimte gegevens van Bing | Microsoft Azure"
    description="In deze zelfstudie leert u hoe om uit te voeren op basis van een locatie push-meldingen met Azure melding Hubs en ruimte gegevens van Bing."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="push-bericht, push-bericht"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Geografische omheind push-meldingen met Azure melding Hubs en ruimte Bing-gegevens
 
 > [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02)voor meer informatie.

In deze zelfstudie leert u hoe om uit te voeren op basis van een locatie push-meldingen met Azure melding Hubs en Bing ruimte gegevens, overgenomen van binnen een toepassing universele Windows-platforms.

##<a name="prerequisites"></a>Vereisten voor
Allereerst omdat u nodig hebt om ervoor te zorgen dat er alle de software en services minimumvereisten:

* [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) of hoger ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) doet ook). 
* Meest recente versie van de [Azure SDK](https://azure.microsoft.com/downloads/). 
* [Bing Maps ontwikkelaar Center account](https://www.bingmapsportal.com/) (u kunt een gratis maken en koppelen aan uw Microsoft-account). 

##<a name="getting-started"></a>Aan de slag

Laten we beginnen met het maken van het project. Visual Studio, start u een nieuw project van het type **Lege App (Universal Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Zodra het maken van het project voltooid is, kunt u de kabelboom voor de app zelf nodig hebt. Nu we instellen Alles voor de infrastructuur geografische-fencing. Omdat we Bing services hiervoor gebruiken gaat, moet u er een openbare REST API-eindpunt waarmee wij specifieke locatie frames query is:

    http://spatial.virtualearth.net/REST/v1/data/
    
U moet de volgende parameters om de host werken te opgeven:

* **Gegevensbron-ID** en de **Naam van de gegevensbron** – in Bing Maps API, gegevensbronnen verschillende bucketed metagegevens, zoals locaties en tijdens kantooruren van bewerking bevatten. U vindt meer informatie over deze hier. 
* **De naam van de entiteit** : de entiteit die u wilt gebruiken als een verwijzing voor de melding. 
* **Bing Maps API-sleutel** : dit is de sleutel die u eerder hebt aangeschaft bij het maken van het account Bing ontwikkelaar Center.
 
Laten we een uitvoerige van de instellingen voor elk van de bovenstaande elementen doen.

##<a name="setting-up-the-data-source"></a>Bij het instellen van de gegevensbron

U kunt dit doen in het midden van Bing Maps ontwikkelaar. Klikt u op **gegevensbronnen** in de bovenste navigatiebalk en selecteer **Gegevensbronnen beheren**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Als u niet met Bing Maps-API gewerkt heb, waarschijnlijk er niet alle gegevensbronnen die aanwezig zijn, zodat u kunt alleen een nieuwe record door te klikken op gegevens uploaden naar een gegevensbron maken. Controleer of dat u Vul alle verplichte velden:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

U mogelijk worden afvraagt – wat het gegevensbestand is en wat moet u worden geüpload? We kunnen alleen de steekproef recht streepje gebaseerde die een deel van de kust San Francisco frames gebruiken voor de toepassing van deze toets wordt:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
De bovenstaande vertegenwoordigt deze entiteit:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Gewoon kopiëren en plakken van de bovenstaande tekenreeks in een nieuw bestand opslaan als **NotificationHubsGeofence.pipe**en uploadt dit in het midden van de ontwikkelaar Bing.

>[AZURE.NOTE]U mogelijk gevraagd om op te geven van een nieuwe sleutel voor de **sleutel** die verschilt van de **Query-toets**. Gewoon een nieuwe sleutel via het dashboard maken en vernieuw de pagina gegevensbron uploaden.

Nadat u het gegevensbestand uploadt, moet u om ervoor te zorgen dat u de gegevensbron publiceren. 

Ga naar **Gegevensbronnen beheren**, zoals we hebben gedaan hierboven, vindt u uw gegevensbron in de lijst en klik op **publiceren** in de kolom **Acties** . In een stapje, ziet u de gegevensbron op het tabblad **Gegevensbronnen die zijn gepubliceerd** :

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Als u op **bewerken**, is mogelijk om te zien in een oogopslag welke we geïntroduceerd in deze locaties:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Nu wordt de portal niet weergegeven u de grenzen voor de geofence dat we hebben gemaakt – moeten we een bevestiging dat de opgegeven locatie is in de juiste nabijheid is.

Nu hebt u de vereisten voor de gegevensbron. Als u de informatie in de URL van de aanvraag voor het gesprek API, in het midden van Bing Maps ontwikkelaar, klikt u op **gegevensbronnen** en selecteer **Informatie van gegevensbronnen**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

De **URL van de Query** is hier wat we zoeken. Dit is het eindpunt die we query's om te controleren of het apparaat momenteel binnen de grenzen van een locatie of niet is kunt uitvoeren. Als u wilt deze controle wordt uitgevoerd, moeten we gewoon een GET-oproep ten opzichte van de URL van de query uitvoeren met de volgende parameters toegevoegd:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Op deze manier bepaalt u een doel wijs die we downloaden van het apparaat en Bing Maps wordt automatisch de berekeningen uitvoeren om te zien of het binnen de geofence is. Wanneer u het verzoek tot en met een browser (of krul) uitvoert, ontvangt u standaard een JSON-antwoord:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Dit antwoord gebeurt alleen als de komma werkelijk binnen de grenzen van de aangewezen is. Als dit niet het geval is, krijgt u een lege **resultaten** Emmertje:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##<a name="setting-up-the-uwp-application"></a>Bij het instellen van de toepassing UWP

Nu dat we de gegevensbron die gereed hebben, kunnen we gaan werken op de UWP-toepassing die we eerder bootstrap uitvoert.

Allereerst omdat moet we locatie services voor de toepassing inschakelen. Dubbelklik hiertoe op `Package.appxmanifest` -bestand in **Verkenner oplossing**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Op het tabblad van de pakket eigenschappen die u zojuist hebt geopend, klikt u op **mogelijkheden** en zorg ervoor dat u **locatie selecteert**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Als de mogelijkheid locatie wordt aangegeven, een nieuwe map maken in uw oplossing met de naam `Core`, en voeg een nieuw bestand erin, genaamd `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

De `LocationHelper` klasse zelf is op dit moment vrij eenvoudige – deze methode wordt kunnen we de gebruikerslocatie via het systeem API verkrijgen:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

U kunt meer informatie over het aanvragen van de locatie van de gebruiker in UWP-apps in het officiële [MSDN-document](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

Als u wilt controleren of de overname locatie daadwerkelijk werkt, opent u de code-kant van de hoofdpagina (`MainPage.xaml.cs`). Maak een nieuwe gebeurtenis-handler voor de `Loaded` gebeurtenis in de `MainPage` constructor:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

De uitvoering van de gebeurtenis-handler is als volgt:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Zoals u ziet dat we de handler gedeclareerd als asynchrone omdat `GetCurrentLocation` awaitable is en daarom moet worden uitgevoerd in de context van een asynchrone. Ook, omdat onder bepaalde omstandigheden kan we dit uiteindelijk met een null-locatie (bijvoorbeeld de locatie services zijn uitgeschakeld of de toepassing is geweigerd machtigingen naar access locatie), we nodig om ervoor te zorgen dat deze op correcte wijze is verwerkt met een null controleren.

Voer de toepassing. Controleer of dat u de locatie toegang:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Wanneer de toepassing gestart, moet u de coördinaten in **het uitvoervenster** zien:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Nu weet u dat locatie acquisition works – je mag rustig verwijderen van de gebeurtenis-handler test voor geladen, omdat we niet deze meer gebruikt.

De volgende stap is om vast te leggen locatie wijzigingen. Voor die, in dit artikel gaat u terug naar de `LocationHelper` klasse en toevoegen van de gebeurtenis-handler voor `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

De implementatie, wordt de locatiecoördinaten weergegeven in het venster **uitvoer** :

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##<a name="setting-up-the-backend"></a>Bij het instellen van de back-end

Download de [.NET Backend steekproef uit GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Zodra het downloaden is voltooid, opent u de `NotifyUsers` map, en vervolgens – de `NotifyUsers.sln` bestand.

Stel de `AppBackend` project als het **Opstartproject** en deze te starten.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Het project is al geconfigureerd voor het verzenden van push-meldingen naar apparaten, zodat we moet slechts twee dingen doen – verwisselt de juiste verbinding tekenreeks van de hub melding en identificatie van de rand als u de melding alleen verzenden indien de gebruiker zich binnen de geofence wilt toevoegen.

Voor het configureren van de verbindingsreeks het `Models` map openen `Notifications.cs`. De `NotificationHubClient.CreateClientFromConnectionString` functie moet de gegevens over uw melding hub die u in de [Portal van Azure](https://portal.azure.com) (uiterlijk binnen het blad **Clienttoegangsbeleid** in **instellingen vinden kunt**) bevatten. Sla het configuratiebestand bijgewerkte.

Nu moeten we een model voor het resultaat van Bing Maps API maken. De eenvoudigste manier om daarvoor is met de rechtermuisknop op de `Models` **toevoegen**de map > **Class**. Noem deze `GeofenceBoundary.cs`. Als ingevuld, kopieert u de JSON uit het API-antwoord dat we in de eerste sectie en in Visual Studio gebruik **bewerken**besproken > **Plakken speciaal** > **Plakken JSON als klassen**. 

Op deze manier die we Zorg ervoor dat het object worden gedeserialiseerd exact in zoals deze is bedoeld. De resulterende class set moet er dan ongeveer als volgt:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Open vervolgens `Controllers`  >  `NotificationsController.cs`. We nodig om het gesprek posten in account voor de doeltoepassing lengte en breedte. Daarvoor gewoon toevoegen twee tekenreeksen aan de functiehandtekening – `latitude` en `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Maak een nieuwe klasse binnen het project genoemd `ApiHelper.cs` – we gebruiken deze verbinding maken met Bing om te controleren grens snijpunten wijst. Implementeren een `IsPointWithinBounds` , functie, als volgt:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Zorg ervoor dat het eindpunt API met de URL van de query die u eerder hebt aangeschaft bij het beheercentrum van Bing ontwikkelaar (geldt ook voor de API-sleutel) vervangen. 

Als er resultaten op de query, dat betekent dat het opgegeven punt binnen de grenzen van de geofence, zodat we retourneren `true`. Als er geen resultaten, Bing is vertellen ons dat de komma buiten het frame opzoeken, is zodat we retourneren `false`.

Terug in `NotificationsController.cs`, een controle rechts voordat u de schakeloptie-instructie maken:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Op deze manier de melding wordt alleen verzonden als de komma binnen de grenzen is.

##<a name="testing-push-notifications-in-the-uwp-app"></a>Push-meldingen in de app UWP testen

Teruggaan naar de app UWP, moet we nu kunnen meldingen testen. Binnen de `LocationHelper` klasse, maakt u een nieuwe functie – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Uitwisselen de `POST_URL` naar de locatie van uw geïmplementeerd webtoepassing die we in de vorige sectie hebt gemaakt. Nu is het OK als lokaal worden uitgevoerd, maar terwijl u werkt op een openbare versie implementeren, moet u deze host bij een externe provider.

Laten we nu Zorg ervoor dat we de UWP-app voor push-meldingen hebt geregistreerd. Klik in Visual Studio, op **Project** > **Opslaan** > **-app waarin de store koppelen**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Nadat u zich bij uw ontwikkelaarsaccount aanmelden, Controleer of u een bestaande app selecteren of een nieuw account te maken en het pakket koppelen. 

Ga naar het beheercentrum ontwikkelaar en open de app die u zojuist hebt gemaakt. Klik op **Services** > **Pushmeldingen** > **Live Services-site**.

![](./media/notification-hubs-geofence/ms-live-services.png)

Klik op de site Let op de **Toepassing geheim** en het **Pakket beveiligings-id**. Moet u zowel in de Portal Azure – open de melding-hub, klik op **Instellingen** > **Notification Services** > **Windows (WNS)** en voer de gegevens in de desbetreffende velden.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Klik op **Opslaan**.

Klik met de rechtermuisknop op de **verwijzingen** in **Solution Explorer** en selecteer **NuGet-pakketten beheren**. Moeten we toevoegen van een verwijzing naar de **Microsoft Azure Service Bus beheerde bibliotheek** -gewoon zoeken voor `WindowsAzure.Messaging.Managed` en voeg deze toe aan uw project.

![](./media/notification-hubs-geofence/vs-nuget.png)

We kunnen maken voor testdoeleinden de `MainPage_Loaded` gebeurtenis-handler opnieuw en voeg deze codefragment toe:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

De bovenstaande registreert de app met de hub melding. U bent klaar! 

In `LocationHelper`, binnen de `Geolocator_PositionChanged` handler, kunt u een deel van een test-code die overname de locatie in de geofence toevoegen:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Omdat de reële coördinaten (die inhoud mogelijk niet binnen de grenzen op het moment dat) niet doorgeeft en testwaarden van de vooraf gedefinieerde gebruikt, zien we een melding weergegeven op update:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##<a name="whats-next"></a>Hoe nu verder?

Er zijn een paar stappen die u mogelijk moet volgen naast de bovenstaande om ervoor te zorgen dat de oplossing productie gereed is.

Allereerst omdat, moet u ervoor te zorgen dat geofences dynamische. Dit vereist wat extra werk met de API Bing om te kunnen uploaden nieuwe grenzen binnen de bestaande gegevensbron. Raadpleeg de [documentatie van Bing ruimte Data Services API](https://msdn.microsoft.com/library/ff701734.aspx) voor meer informatie over het onderwerp.

Tweede, als u werkt om ervoor te zorgen dat de bezorging aan de juiste deelnemers is voltooid, kan het handig deze via [labelen](notification-hubs-tags-segment-push-message.md)afstemmen.

De oplossing die hierboven wordt een scenario u een groot aantal doelplatforms, hebt mogelijk zodat we heeft geen afbreuk aan de geofencing naar systeem / regiospecifieke mogelijkheden beschreven. Echter wel verwijzen, het universele Windows-Platform biedt mogelijkheden om te [bepalen geofences rechts kant-en-klare](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).

Bekijk onze [documentatie portal](https://azure.microsoft.com/documentation/services/notification-hubs/)voor meer informatie over mogelijkheden voor melding Hubs.
