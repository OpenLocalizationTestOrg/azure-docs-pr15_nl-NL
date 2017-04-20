<properties
    pageTitle="Een upgrade uitvoeren van mobiele Services naar Azure App-Service"
    description="Leer hoe u eenvoudige upgrade van uw mobiele Services-toepassing aan een App-Service Mobile-App"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Upgrade van uw bestaande .NET Azure Mobile Service-App-service

De mobiele App Service is een nieuwe manier mobiele toepassingen met Microsoft Azure maken. Zie meer informatie, [Wat Mobile-Apps zijn?].

Dit onderwerp wordt beschreven hoe u een bestaande .NET backend-toepassing een upgrade van Azure Mobile Services naar een nieuwe App Service Mobile-Apps. Terwijl u deze upgrade uitvoert, blijven uw bestaande Mobile Services-toepassing functioneren.   Als u moet een upgrade uitvoert voor een Node.js backend-toepassing, raadpleegt u [een upgrade van uw Node.js Mobile-Services](./app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Wanneer een mobiele backend wordt bijgewerkt naar Azure App-Service, heeft toegang tot alle functies van de App Service en op basis van de [App Service prijzen], niet Mobile Services prijzen zijn gefactureerd.

##<a name="migrate-vs-upgrade"></a>Migreren te upgraden

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Het wordt aanbevolen dat u [een migratie uitvoeren](app-service-mobile-migrating-from-mobile-services.md) voordat u verdergaat met een upgrade. Op deze manier kunt u ervoor zorgen dat beide versies van uw toepassing op de dezelfde App-Service plannen en geen extra kosten in rekening gebracht.

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>Verbeteringen in de Mobile-Apps .NET-server SDK

Een upgrade naar de nieuwe [Mobiele Apps SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) biedt de volgende voordelen:

- Meer flexibiliteit op NuGet afhankelijkheden. De hostomgeving biedt niet langer een eigen versies van NuGet-pakketten, zodat u kunt alternatieve compatibele versies gebruiken. Als er nieuwe kritieke bugfixes of beveiligingsupdates naar het mobiele Server SDK of afhankelijkheden, moet u handmatig een uw service bijwerken.

- Meer flexibiliteit in de mobiele SDK. U kunt expliciet bepalen welke functies en routes zijn ingesteld, klikt u zoals verificatie, tabel API's en het eindpunt van de registratie push. Meer informatie raadpleegt u [het gebruik van de server .NET SDK voor Azure Mobile-Apps](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup).

- Ondersteuning voor andere ASP.NET-projecttypen en routes. U kunt nu hostcontrollers MVC en Web API in hetzelfde project als uw mobiele backend-project.

- Ondersteuning voor de nieuwe App Service verificatiefuncties waarmee u kunt een algemene verificatie-configuratie gebruiken op uw web en mobiele apps.

##<a name="overview"></a>Eenvoudige upgrade, overzicht

In veel gevallen is een upgrade zo eenvoudig overschakelen naar de nieuwe SDK van de Mobile-Apps-server en uw code naar een nieuw exemplaar van de Mobile-App opnieuw te publiceren. Er zijn echter enkele scenario's die worden enkele extra configuratie, zoals geavanceerde verificatie-scenario's en het werken met moeten taken gepland. Elk van deze vindt u in de volgende onderdelen.

>[AZURE.TIP] Wordt aanbevolen dat u gelezen en de rest van dit onderwerp volledig begrijpen voordat u een upgrade starten. Noteer van functies u gebruiken die hieronder worden genoemd.

De Mobile-Services-client SDK's zijn **niet** compatibel met de nieuwe Mobile-Apps-server SDK. Kan alleen worden aangeboden bedrijfscontinuïteit van service voor de app, moet u geen wijzigingen publiceren naar een site die momenteel gepubliceerde clients van dienst. In plaats daarvan, moet u een nieuwe mobiele app dat als er een dubbel fungeert maken. U kunt deze toepassing plaatsen op hetzelfde App Service plan om te voorkomen dat extra financiële kosten.

U moet dan twee versies van de toepassing: een die hetzelfde blijft en fungeert apps zijn gepubliceerd in de natuur, en de andere die u vervolgens een upgrade uitvoert en doellijst met een nieuwe versie van de client. U kunt verplaatsen en testen van uw code in uw tempo, maar moet u ervoor zorgen dat alle correcties die u aanbrengt, worden toegepast op beide. Wanneer u denkt dat een gewenste aantal client-apps in de natuur hebt bijgewerkt naar de nieuwste versie, u de oorspronkelijke gemigreerde app verwijderen kunt desgewenst.

Het volledige overzicht voor het upgradeproces is als volgt:

1. Een nieuwe Mobile-App maken
2. Bijwerken van het project om de nieuwe Server SDK's gebruiken
3. Een nieuwe versie van de clienttoepassing
4. (Optioneel) Uw oorspronkelijke gemigreerde exemplaar verwijderen

##<a name="mobile-app-version"></a>Een tweede toepassingsexemplaar maken
De eerste stap bij een upgrade is het opzetten van de Mobile-App resource die wordt gehost in de nieuwe versie van uw toepassing. Als u hebt al een bestaande mobiele service gemigreerd, wilt u deze versie op hetzelfde hostingprovider plan maken. Open de [Azure-portal] en Ga naar uw gemigreerde toepassing. Noteer het App-Service Plan wordt uitgevoerd.

Maak vervolgens het tweede toepassingsexemplaar volgens de [instructies van .NET backend maken](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Kies het abonnement van uw gemigreerde toepassing wanneer u wordt gevraagd om te selecteren dat u App-abonnement of "plan hostingprovider".

Waarschijnlijk zult u de dezelfde database en de melding Hub gebruiken, zoals in de Mobile-Services. U kunt deze waarden door [Azure-portal] te openen en te navigeren naar de oorspronkelijke toepassing kopiëren en klik op **Instellingen** > **Toepassingsinstellingen**. Klik onder **Tekenreeksen met de verbinding**, kopiëren `MS_NotificationHubConnectionString` en `MS_TableConnectionString`. Navigeer naar de site van uw nieuwe upgrade en plak deze in een bestaande waarden overschrijven. Herhaal deze procedure voor elke andere toepassingsinstellingen de behoeften van uw app. Als een gemigreerde service niet gebruikt, kunt u de verbindingstekenreeksen en app-instellingen op het tabblad **configureren** van de sectie Mobile-Services van de [Azure klassieke portal]lezen.

Een kopie van het project ASP.NET voor uw toepassing maken en publiceren naar uw nieuwe site. Met een kopie van de clienttoepassing bijgewerkt met de nieuwe URL, valideren of alles werkt zoals verwacht.

## <a name="updating-the-server-project"></a>Bijwerken van het serverproject

Mobile-Apps vindt u een nieuwe [Mobiele App Server SDK] die veel van dezelfde functionaliteit als de runtime Mobile-Services biedt. Verwijder eerst alle verwijzingen naar de Mobile-Services-pakketten. Zoekt in de manager van het pakket NuGet `WindowsAzure.MobileServices.Backend`. De meeste apps ziet meerdere pakketten hier, waaronder `WindowsAzure.MobileServices.Backend.Tables` en `WindowsAzure.MobileServices.Backend.Entity`. In dat geval beginnen met het laagste pakket in de afhankelijkheidsstructuur, zoals `Entity`, en deze te verwijderen. Wanneer u wordt gevraagd, verwijder niet alle afhankelijk-pakketten. Herhaal deze procedure totdat u hebt verwijderd `WindowsAzure.MobileServices.Backend` zelf.

U hebt nu een project dat niet meer wordt verwezen naar mobiele Services SDK's.

Vervolgens moet u verwijzingen de SDK van de Mobile-Apps toevoegen. Voor deze upgrade, de meeste ontwikkelaars wilt downloaden en installeren de `Microsoft.Azure.Mobile.Server.Quickstart` inpakken, zoals dit, in het hele vereiste instellen klikt wordt.

Er is een aantal compileerprogramma fouten die voortvloeien uit verschillen tussen de SDK's, maar deze eenvoudige, adres en in de rest van deze sectie worden beschreven.

### <a name="base-configuration"></a>Basis configuratie

Vervolgens gaat u in WebApiConfig.cs, kunt u het volgende vervangen:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

met

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] Als u meer informatie over de nieuwe .NET-server SDK en hoe wilt u functies uit uw app toevoegen/verwijderen, raadpleegt u het onderwerp van [het gebruik van de server .NET SDK] .

Als uw app maakt gebruik van de verificatiefuncties voor, moet u ook een middleware OWIN registreren. In dit geval moet u de bovenstaande configuratiecode verplaatsen naar een nieuwe OWIN opstarten klasse.

1. Het pakket NuGet toevoegen `Microsoft.Owin.Host.SystemWeb` als deze niet al is opgenomen in uw project.
2. Klik met de rechtermuisknop op uw project in Visual Studio, en selecteer **toevoegen** -> **Nieuw Item**. Selecteer **Web** -> **algemene** -> **OWIN opstarten class**.
3. De bovenstaande code verplaatsen voor MobileAppConfiguration uit `WebApiConfig.Register()` naar de `Configuration()` methode van uw nieuwe Opstartklasse.

Zorg ervoor dat de `Configuration()` methode eindigt op:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Er zijn gerelateerd aan verificatie extra wijzigingen die worden beschreven in de sectie volledige verificatie hieronder.

### <a name="working-with-data"></a>Werken met gegevens

In de Mobile-Services, wordt de naam van de mobiele app dienden als de naam van het standaard schema bij het instellen van entiteit Framework.

Om ervoor te zorgen dat er hetzelfde schema waarnaar wordt verwezen als voorheen, gebruik de volgende het schema instellen in de DbContext voor uw toepassing:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Zorg ervoor dat er MS_MobileServiceName instellen als u de bovenstaande doen. Als uw toepassing dit eerder hebt aangepast, kunt u ook de naam van een ander schema opgeven.

### <a name="system-properties"></a>Systeemeigenschappen

#### <a name="naming"></a>Naam geven

In de Azure Mobile Services-server SDK Systeemeigenschappen altijd een dubbele onderstrepingsteken bevatten (`__`) voorvoegsel voor de eigenschappen:

- __createdAt
- __updatedAt
- __deleted
- __version

De Mobile-Services-client SDK's hebben speciale logica voor parseren van Systeemeigenschappen in deze indeling.

In de Mobile-Apps Azure, Systeemeigenschappen niet langer speciale opmaak en de volgende namen hebt:

- createdAt
- updatedAt
- verwijderd
- Versie

De Mobile-Apps-client SDK's gebruikt de nieuwe namen in de eigenschappen systeem, zodat er geen wijzigingen zijn vereist om te clientcode. Echter als u rechtstreeks REST oproepen naar uw service moet vervolgens u uw query's dienovereenkomstig gewijzigd.

#### <a name="local-store"></a>Lokale archief

De wijzigingen in de namen van Systeemeigenschappen betekenen dat een lokale database offline synchroniseren voor Mobile Services niet compatibel met Mobile-Apps is. Indien mogelijk, dient u geen apps uit Mobile Services naar Mobile-Apps totdat na wachtende wijzigingen zijn verzonden naar de server-client bijgewerkt. De bijgewerkte app moet vervolgens een nieuwe bestandsnaam van de database gebruiken.

Als een client-app wordt bijgewerkt van mobiele Services naar Mobile-Apps terwijl er in behandeling offline wijzigingen in de wachtrij betrekking heeft, moet klikt u vervolgens de database worden bijgewerkt om de nieuwe kolomnamen gebruiken. Klik op iOS, kan dit worden bereikt lightweight migraties gebruiken om te wijzigen van de kolomnamen. Klik op Android en de beheerde .NET-client, moet u aangepaste SQL om de naam van de kolommen voor de gegevenstabellen van het object te schrijven.

Klik op iOS, moet u uw schema Core gegevens voor uw gegevensentiteiten zodat deze overeenkomt met het volgende wijzigen. Houd er rekening mee dat de eigenschappen `createdAt`, `updatedAt` en `version` niet langer een `ms_` voorvoegsel:

| Kenmerk |  Type   | Opmerking                                                 |
|---------- |  ------ | -----------------------------------------------------|
| ID        | Tekenreeks, gemarkeerd als vereist  | primaire sleutel in de externe store         |
| createdAt | Datum    | (optioneel) kaarten aan createdAt systeemeigenschap         |
| updatedAt | Datum    | (optioneel) kaarten aan updatedAt systeemeigenschap         |
| Versie   | Tekenreeks  | (optioneel) gebruikt om te bepalen conflicten, kaarten naar versie |

#### <a name="querying-system-properties"></a>Query's uitvoeren Systeemeigenschappen

In de Mobile-Services van Azure, Systeemeigenschappen niet worden verzonden al dan niet standaard, maar alleen wanneer daarom wordt gevraagd het gebruik van de queryreeks `__systemProperties`. Daarentegen in de Mobile-Apps Azure-systeem zijn eigenschappen **altijd geselecteerd** omdat ze deel van het objectmodel van server SDK uitmaken.

Deze wijziging heeft hoofdzakelijk gevolgen voor aangepaste implementaties van managers voor domein, zoals uitbreidingen van `MappedEntityDomainManager`. In de Mobile-Services, als een client nooit een Systeemeigenschappen aanvraagt, is het mogelijk gebruik van een `MappedEntityDomainManager` die alle eigenschappen niet daadwerkelijk toewijzen. Echter in de Mobile-Apps Azure treedt deze niet-toegewezen eigenschappen een fout in GET-query's.

De eenvoudigste manier om het probleem te verhelpen is voor het wijzigen van uw DTOs zodat ze van overnemen `ITableData` in plaats van `EntityData`. Voeg vervolgens de `[NotMapped]` kenmerk in de velden die moeten worden weggelaten.

Bijvoorbeeld het volgende wordt gedefinieerd `TodoItem` zonder systeem eigenschappen:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Opmerking: als er fouten op `NotMapped`, een verwijzing naar de vergadering toevoegen `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS

Mobile-Services opgenomen sommige ondersteuning voor CORS door de oplossing ASP.NET CORS voor tekstterugloop. Deze laag tekstterugloop, zodat de ontwikkelaar meer controle, zodat u rechtstreeks van [ASP.NET-CORS ondersteuning gebruikmaken kunt](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)is verwijderd.

De belangrijkste gebieden van belang als CORS met zijn die de `eTag` en `Location` kopteksten moeten worden toegestaan om de client SDK's werkt alleen naar behoren.

### <a name="push-notifications"></a>Push-meldingen
Voor push is het belangrijkste item dat u mogelijk ontbreken zoeken via de Server SDK de klasse PushRegistrationHandler. Registraties iets anders worden verwerkt in Mobile-Apps en tagless registraties zijn standaard ingeschakeld. Beheer van tags kan worden uitgevoerd met behulp van aangepaste API's. Zie de instructies [registreren voor labels](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) voor meer informatie.

### <a name="scheduled-jobs"></a>Geplande taken
Geplande taken zijn niet ingebouwd in Mobile-Apps, zodat alle bestaande taken die u in uw backend .NET hebt moet afzonderlijk worden bijgewerkt. Eén optie is een geplande [Web taak] maken op de site van de code Mobile-App. U kunt ook een domeincontroller die uw taakcode instellen en configureren van de [Azure Scheduler] als u wilt dat eindpunt op de verwachte planning raken.

### <a name="miscellaneous-changes"></a>Diverse wijzigingen
Alle ApiControllers waarin wordt worden gebruikt door een mobiele client moet nu de `[MobileAppController]` kenmerk. Dit is niet meer standaard zodat andere ApiControllers om te gaan niet beïnvloed door de mobiele formatters opgenomen.

De `ApiServices` object is niet langer deel uit van de SDK. Toegang tot de instellingen van de mobiele App, kunt u het volgende:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Logboekregistratie is ook nu uitgevoerd met de standaard ASP.NET doelcellen schrijven:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="authentication"></a>Aandachtspunten voor de verificatie

De onderdelen van de verificatie van mobiele Services zijn nu verplaatst naar de App Service verificatie/autorisatie-functie. U kunt meer informatie over het inschakelen van dit voor uw site door de [verificatie van toevoegen aan uw mobiele app](app-service-mobile-ios-get-started-users.md) -onderwerp te lezen.

Bij sommige providers zoals AAD, Facebook en Google, moet u het volgende kunnen zijn om te profiteren van de bestaande registratie van uw toepassing kopiëren. U hoeft om te navigeren naar de identiteitsprovider-portal en een nieuwe Omleidings-URL toevoegen aan de registratie. App Service verificatie/autorisatie configureert met de client-ID en geheim.

### <a name="controller-action-authorization"></a>Autorisatie voor controller-actie
Alle exemplaren van de `[AuthorizeLevel(AuthorizationLevel.User)]` kenmerk moet nu worden gewijzigd als u wilt gebruiken de standaard ASP.NET `[Authorize]` kenmerk. Bovendien zijn controllers nu anoniem al dan niet standaard, zoals in andere ASP.NET-toepassingen.
Als u een van de andere AuthorizeLevel-opties, zoals beheerder of toepassing, houd er rekening mee dat deze verdwenen zijn. U kunt in plaats daarvan AuthorizationFilters instellen voor gedeelde geheimen of een Service AAD Principal veilig zodat oproepen van de service-naar-service configureren.

### <a name="getting-additional-user-information"></a>Aanvullende informatie ophalen

U gaat extra gebruikersgegevens, inclusief toegangstokens tot en met de `GetAppServiceIdentityAsync()` methode:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Ook als uw toepassing afhankelijkheden op gebruikers-id's, duurt zoals ze opslaan in een database, is het belangrijk te weten dat de gebruikers-id's tussen mobiele Services en App Service Mobile-Apps verschillend zijn. U kunt echter nog steeds de Mobile-Services gebruikers-ID, krijgen. Alle de subcategorieën ProviderCredentials hebben een gebruikers-id-eigenschap. Doorlopende zo uit in het voorbeeld voordat:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Als uw app afhankelijk op gebruikers-id's duurt, is het belangrijk dat u gebruikmaken van dezelfde registratie met een identiteitsprovider indien mogelijk. Gebruikers-id's zijn meestal beperkt tot de registratie van de toepassing die is gebruikt, zodat problemen met de overeenkomende gebruikers moeten hun gegevens Kennismaking met een nieuwe registratie kan maken.

### <a name="custom-authentication"></a>Aangepaste verificatie

Als uw app een oplossing van een aangepaste verificatie gebruikt is, wordt het gewenste van om ervoor te zorgen dat de bijgewerkte site toegang tot het systeem heeft. Volg de instructies van de nieuwe voor aangepaste verificatie in het [.NET server SDK overzicht] uw oplossing integreren. Houd er rekening mee dat het aangepaste verificatie-onderdelen nog steeds in de Preview-versie zijn.

##<a name="updating-clients"></a>Bijwerken van clients
Nadat u een operationele Mobile-App backend hebt, kunt u op een nieuwe versie van de clienttoepassing waarin deze verbruikt werken. Mobile-Apps bevat ook een nieuwe versie van de client SDK's en vergelijkbaar met de serverupgrade hierboven, moet u alle verwijzingen naar de mobiele Services SDK's verwijderen voordat u de versies Mobile-Apps installeert.

Een van de belangrijkste wijzigingen tussen de versies is dat de constructors niet langer een toepassingstoets vereist. U nu doorgeven gewoon in de URL van de Mobile-App. Bijvoorbeeld op de .NET-clients, de `MobileServiceClient` opbouwfunctie is nu:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

U kunt lezen over het installeren van de nieuwe SDK's en het gebruik van de structuur van de nieuwe via de onderstaande koppelingen:

- [iOS versie 3.0.0 of hoger](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) versie 2.0.0 of hoger](app-service-mobile-dotnet-how-to-use-client-library.md)

Als uw toepassing maakt gebruik van push-meldingen, noteer de specifieke registratie-instructies voor elk platform, als er zijn enkele wijzigingen er ook.

Wanneer u de nieuwe clientversie klaar hebt, uitproberen ten opzichte van uw serverproject bijgewerkte. Na de validatie dat deze altijd werkt, kunt u een nieuwe versie van uw toepassing naar klanten vrijgeven. Uiteindelijk nadat uw klanten hebt deze updates wilt ontvangen, kunt u de Mobile-Services-versie van uw app verwijderen. U hebt nu volledig bijgewerkt naar een App-Service Mobile-App wordt gebruikt, met de meest recente SDK van de Mobile-Apps-server.

<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[Azure klassieke portal]: https://manage.windowsazure.com/
[Wat zijn de Mobile-Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobiele App-Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Taak met webonderdelen]: ../app-service-web/websites-webjobs-resources.md
[Het gebruik van de server .NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[App-Service prijzen]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK-overzicht]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
