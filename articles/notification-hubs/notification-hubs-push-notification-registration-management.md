<properties
    pageTitle="Registratie-beheer"
    description="In dit onderwerp wordt uitgelegd hoe apparaten registreren bij melding hubs push-meldingen ontvangen."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="registration-management"></a>Registratie-beheer

##<a name="overview"></a>Overzicht

In dit onderwerp wordt uitgelegd hoe apparaten registreren bij melding hubs push-meldingen ontvangen. Het onderwerp wordt beschreven registraties op hoog niveau en klik vervolgens maakt u kennis met de twee belangrijkste patronen voor het registreren van apparaten: registreren van het apparaat rechtstreeks aan de hub melding en via een toepassing backend registreren. 


##<a name="what-is-device-registration"></a>Wat is apparaatregistratie

Apparaatregistratie met een melding Hub is uitgevoerd met een **registratie** of **installatie**.

#### <a name="registrations"></a>Registraties
Een registratie koppelt de greep Platform melding Service (PNS) voor een apparaat met labels en mogelijk een sjabloon. De greep PNS mogelijk een ChannelURI, apparaat token of GCM registratie-id. Labels worden gebruikt voor het routeren van meldingen naar de juiste set om het apparaat. Zie [omleiden en Tag expressies](notification-hubs-tags-segment-push-message.md)voor meer informatie. Sjablonen worden gebruikt om te implementeren per registratie-transformatie. Zie [sjablonen](notification-hubs-templates-cross-platform-push-messages.md)voor meer informatie.

#### <a name="installations"></a>Installaties
Een installatie is een uitgebreide registratie met een groot aantal push gerelateerde eigenschappen. Dit is de meest recente en aanbevolen wijze op het registreren van uw apparaten. Dit is echter niet ondersteund door clientzijde .NET SDK ([Melding Hub SDK voor backend-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) vanaf nog.  Dit betekent dat als u vanaf het clientapparaat worden gebruikt zelf registreert, moet u de [Melding Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx) -methode gebruiken voor de ondersteuning van installaties. Als u een back-end-service gebruikt, kunt u moet gebruikmaken van de [Melding Hub SDK voor backend-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Hier volgen enkele belangrijke voordelen van de installaties:

* Maken of bijwerken van een installatie is volledig idempotency is ingeschakeld. Zo kunt u deze opnieuw zonder eventuele problemen met dubbele registraties.
* Het model installatie kunt u gemakkelijk moet afzonderlijke gereedschap duwt - doelgroepen specifieke apparaat. Een tag systeem **"$InstallationId: [de installatie-id]"** wordt automatisch toegevoegd met elke registratie installatie gebaseerd. U kunt zo een verzenden naar dit label op een specifieke apparaat zonder dat u moet doen eventuele aanvullende kleurcodering bellen.
* Met installaties kunt u doen gedeeltelijke registration wordt bijgewerkt. De gedeeltelijke update van een installatie wordt aangevraagd met een PATCH methode voor het gebruik van de [JSON-Patch standaard](https://tools.ietf.org/html/rfc6902). Dit is vooral handig als u wilt bijwerken markeringen in de registratie. U hoeft niet te halen omlaag de hele registratie en verzend de vorige tags opnieuw.

Een installatie kan bevatten de volgende eigenschappen. Voor een volledige lijst met de installatie eigenschappen ziet, [maken of een installatie oplossen met REST API overschrijven](https://msdn.microsoft.com/library/azure/mt621153.aspx) of [Installatie-eigenschappen](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) voor de.

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }

 

Het is belangrijk te weten dat registraties en installaties al dan niet standaard niet langer verloopt.

Registraties en installaties moeten een geldige PNS greep voor elk apparaat/kanaal bevatten. Omdat PNS grepen kunnen alleen worden verkregen in een client-app op het apparaat, is een patroon om rechtstreeks op een apparaat met de client-app te registreren. Aan de andere kant beveiligingsoverwegingen en bedrijfslogica die betrekking hebben op labels mogelijk moet u voor het beheren van apparaatregistratie in de app-back-enddatabase. 

#### <a name="templates"></a>Sjablonen

Als u wilt gebruiken, [sjablonen](notification-hubs-templates-cross-platform-push-messages.md), installatie van het apparaat ook houdt u alle sjablonen die zijn gekoppeld aan dat apparaat in een JSON opmaken (Zie voorbeeld hierboven). De sjabloonnamen helpen doel verschillende sjablonen voor hetzelfde apparaat.

Houd er rekening mee dat de sjabloonnaam van elke aan de hoofdtekst van een sjabloon en een optionele reeks labels toegewezen. Elk platform kunt bovendien aanvullende eigenschappen hebben. Voor de Windows Store (via WNS) en Windows Phone 8 (via MPNS), kan een extra set koppen deel uitmaken van de sjabloon. In het geval van APNs, kunt u een eigenschap verstrijken instellen op een van beide een constante of op een sjabloonexpressie. Voor een volledige lijst met de installatie ziet eigenschappen, [maken of een installatie oplossen met REST overschrijven](https://msdn.microsoft.com/library/azure/mt621153.aspx) onderwerp.

#### <a name="secondary-tiles-for-windows-store-apps"></a>Secundaire tegels voor Windows Store-Apps

Voor Windows Store-clienttoepassingen, meldingen verzenden naar secundaire tegels is hetzelfde als deze te verzenden naar de primaire bewerking. Dit wordt ook ondersteund in installaties. Houd er rekening mee dat secundaire tegels hebben een andere ChannelUri, waarin de SDK op de client-app transparant verwerkt.

De woordenlijst SecondaryTiles maakt gebruik van de dezelfde TileId die wordt gebruikt voor het maken van het object SecondaryTiles in de Windows Store-app.
Als u met de primaire ChannelUri kunt ChannelUris van secundaire tegels op elk moment wijzigen. Om de installaties in de melding hub bijgewerkt, moet het apparaat dat ze vernieuwen met de huidige ChannelUris van de tegels met secundaire.


##<a name="registration-management-from-the-device"></a>Beheer van de registratie van het apparaat

Bij het beheren van apparaatregistratie van client-apps, is de backend alleen die verantwoordelijk is voor het verzenden van meldingen. Clienttoepassingen PNS grepen up-to-date te houden en labels registreren. De volgende afbeelding ziet u dit patroon.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Het apparaat eerst de greep PNS opgehaald uit de PNS en registreert met de melding hub rechtstreeks. Nadat de registratie geslaagd is, kan de app-end een melding hebt samengesteld die registratiegegevens verzenden. Zie voor meer informatie over het verzenden van meldingen [omleiden en Tag expressies](notification-hubs-tags-segment-push-message.md).
Opmerking die in dit geval u wilt gebruiken luistert alleen rechten voor toegang tot uw hubs melding van het apparaat. Zie [beveiliging](notification-hubs-push-notification-security.md)voor meer informatie.

Registreren van het apparaat is de eenvoudigste methode, maar er enkele nadelen.
Het eerste nadeel is dat een client-app alleen de labels bijwerken kunt wanneer de app actief is. Bijvoorbeeld als een gebruiker twee apparaten die betrekking hebben op sport teams heeft, wanneer het eerste apparaat voor een aanvullende tag (bijvoorbeeld Seahawks registreert) tags registreren, ontvangt het tweede apparaat niet de meldingen over de Seahawks totdat de app op het tweede apparaat een tweede keer wordt uitgevoerd. Meer over het algemeen, als labels worden beïnvloed door meerdere apparaten, labels beheren vanaf de backend is een wenselijk optie.
Het tweede nadeel van registratie management vanuit de client-app is dat, aangezien apps kunnen worden hacked, beveiligen van de registratie aan specifieke tags moeten worden extra aandacht, zoals wordt beschreven in de sectie "beveiliging van het label op gebruikersniveau."



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Voorbeeldcode registreren bij een hub melding vanaf een apparaat met een installatie 

Op dit moment, is dit alleen ondersteund als u de [Melding Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx).

U kunt ook de PATCH methode dat de [JSON-Patch standaard](https://tools.ietf.org/html/rfc6902) gebruiken voor het bijwerken van de installatie.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }

   

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Voorbeeldcode registreren bij een hub voor melding van een apparaat met een registratie


Deze methoden maken of bijwerken van een registratie voor het apparaat waarop ze worden genoemd. Dit betekent dat de hele registratie om bij te werken de greep of de labels, moet overschrijven. Denk eraan dat registraties tijdelijke, zodat u altijd een betrouwbare store met de huidige tags waarvoor u een specifiek apparaat nodig hebt.


    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }
     
    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Beheer van de registratie van een end

Registraties beheren vanaf de backend is vereist voor het schrijven van extra code. De app van het apparaat moet ondersteuning bieden voor de bijgewerkte PNS worden verwerkt door op de backend telkens wanneer de app wordt gestart (samen met de knoppen labels en sjablonen) en de backend deze greep aan de hub melding moet bijwerken. De volgende afbeelding ziet u dit ontwerp.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

De voordelen van het beheren van registraties van de end omvatten de mogelijkheid voor het wijzigen van tags op registraties, zelfs als de bijbehorende-app op het apparaat niet actief is en de client-app verifiëren voordat u een tag toevoegt aan de registratie.


#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Voorbeeldcode registreren bij een hub voor melding van een end gebruiken op een installatie

Het clientapparaat worden gebruikt de greep PNS en de eigenschappen van de relevante installatie als voordat u krijgt nog steeds en een aangepaste API-oproepen op de backend die kan uitvoeren van de registratie en Autoriseer tags enzovoort. De backend kunt gebruikmaken van de [Melding Hub SDK voor backend-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

U kunt ook de PATCH methode dat de [JSON-Patch standaard](https://tools.ietf.org/html/rfc6902) gebruiken voor het bijwerken van de installatie.
 

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Voorbeeldcode registreren bij een hub voor melding van een apparaat met een registratie-id

Van uw app-end, kunt u basisbewerkingen CRUDS op registraties uitvoeren. Bijvoorbeeld:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");
            
    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


De backend moet omgaan met gelijktijdigheid tussen registration wordt bijgewerkt. Service Bus biedt optimistische gelijktijdigheid besturingselement voor registratie management. Dit wordt geïmplementeerd op het niveau van HTTP met het gebruik van ETag op registratie beheertaken uit te voeren. Deze functie wordt transparant gebruikt door Microsoft SDKs, die een uitzondering genereren als een update voor gelijktijdigheid redenen is geweigerd. De app-end is verantwoordelijk voor het afhandelen van deze uitzonderingen en opnieuw van de update indien nodig.