<properties
    pageTitle="Een upgrade uitvoeren van mobiele Services naar Azure App Service - Node.js"
    description="Leer hoe u eenvoudige upgrade van uw mobiele Services-toepassing aan een App-Service Mobile-App"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Upgrade van uw bestaande Node.js Azure Mobile Service-App-service

De mobiele App Service is een nieuwe manier mobiele toepassingen met Microsoft Azure maken. Zie meer informatie, [Wat Mobile-Apps zijn?].

Dit onderwerp wordt beschreven hoe u een bestaande Node.js backend-toepassing een upgrade van Azure Mobile Services naar een nieuwe App Service Mobile-Apps. Terwijl u deze upgrade uitvoert, blijven uw bestaande Mobile Services-toepassing functioneren.  Als u moet een upgrade uitvoert voor een Node.js backend-toepassing, raadpleegt u [een upgrade van uw .NET Mobile-Services](./app-service-mobile-net-upgrading-from-mobile-services.md).

Wanneer een mobiele backend wordt bijgewerkt naar Azure App-Service, heeft toegang tot alle functies van de App Service en op basis van de [App Service prijzen], niet Mobile Services prijzen zijn gefactureerd.

## <a name="migrate-vs-upgrade"></a>Migreren te upgraden

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Het wordt aanbevolen dat u [een migratie uitvoeren](app-service-mobile-migrating-from-mobile-services.md) voordat u verdergaat met een upgrade. Op deze manier kunt u ervoor zorgen dat beide versies van uw toepassing op de dezelfde App-Service plannen en geen extra kosten in rekening gebracht.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Verbeteringen in de Mobile-Apps Node.js server SDK

Een upgrade naar de nieuwe [Mobiele Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) biedt een groot aantal verbeteringen, waaronder:

- Op basis van de [framework Express](http://expressjs.com/en/index.html), de nieuwe knooppunt SDK lichte is en ontworpen voor het bijhouden met nieuwe knooppunt versies wanneer ze zich voordoen af. U kunt het gedrag van de toepassing met Express middleware aanpassen.

- Aanzienlijke prestatieverbeteringen ten opzichte van de SDK van de Mobile-Services.

- U kunt nu een website hosten samen met uw mobiele backend; het is ook eenvoudig de SDK van Azure Mobile toevoegen aan een bestaande express.v4-toepassing.

- Die is ingebouwd voor meerdere platforms en lokale ontwikkeling, kan de SDK van de Mobile-Apps worden ontwikkeld en lokaal uitvoeren op Windows, Linux en OSX platforms. Het is nu eenvoudig te gebruiken algemene knooppunt-technieken zoals [Mocha](https://mochajs.org/) tests vóór implementatie uitgevoerd.

## <a name="overview"></a>Eenvoudige upgrade, overzicht

Azure-App-Service biedt hulp bij het bijwerken van een backend Node.js, een pakket compatibiliteit.  Na een upgrade hebt u een niew-site die kan worden toegepast op een nieuwe App Service-site.

De Mobile-Services-client SDK's zijn **niet** compatibel met de nieuwe Mobile-Apps-server SDK. Kan alleen worden aangeboden bedrijfscontinuïteit van service voor de app, moet u geen wijzigingen publiceren naar een site die momenteel gepubliceerde clients van dienst. In plaats daarvan, moet u een nieuwe mobiele app dat als er een dubbel fungeert maken. U kunt deze toepassing plaatsen op hetzelfde App Service plan om te voorkomen dat extra financiële kosten.

U moet dan twee versies van de toepassing: machtiging die hetzelfde blijft en fungeert apps zijn gepubliceerd in de natuur, en de andere die u vervolgens een upgrade uitvoert en doellijst met een nieuwe versie van de client. U kunt verplaatsen en testen van uw code in uw tempo, maar moet u ervoor zorgen dat alle correcties die u aanbrengt, worden toegepast op beide. Wanneer u denkt dat een gewenste aantal client-apps in de natuur hebt bijgewerkt naar de nieuwste versie, u de oorspronkelijke gemigreerde app verwijderen kunt desgewenst. Deze kosten niet eventuele aanvullende monetaire, als gehost in hetzelfde App Service plan als uw Mobile-App.

Het volledige overzicht voor het upgradeproces is als volgt:

1. Uw bestaande (gemigreerde) Azure-Service voor Mobile downloaden.
2. Het project converteren naar een Azure Mobile-App met het pakket compatibiliteit.
3. Corrigeer eventuele verschillen (zoals verificatie-instellingen).
4. Het geconverteerde Azure Mobile-App-project dashboard implementeren naar een nieuwe App-Service.
4. Een nieuwe versie van de clienttoepassing met de nieuwe Mobile-App.
5. (Optioneel) Uw oorspronkelijke gemigreerde mobiele service-app verwijderen.

Verwijdering kan optreden als u al het verkeer van uw oorspronkelijke gemigreerde mobiele service niet ziet.

## <a name="install-npm-package"></a>De minimumvereisten installeert

U moet [knooppunt] installeren op uw lokale computer.  Het pakket compatibiliteit moet u ook installeren.  Nadat het knooppunt is geïnstalleerd, kunt u de volgende opdracht uit een nieuwe cmd of de PowerShell-prompt uitvoeren:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Uw Scripts Azure mobiele Services verkrijgen

- Meld u aan bij de [Portal van Azure].
- **Alle Resources** of **App Services**gebruikt, zoek uw Mobile Services-site.
- Binnen de site, klik op **Extra** -> **Kudu** -> **gaat u** naar de site Kudu openen.
- Klik op **Fouten opsporen in Console** -> **PowerShell** de foutopsporing-console openen.
- Navigeer naar `site/wwwroot/App_Data/config` door te klikken op elke map beurtelings
- Klik op het downloadpictogram naast de `scripts` directory.

Hiermee wordt de scripts in ZIP-bestandsindeling gedownload.  Maak een nieuwe map op uw lokale computer en uitpakken de `scripts.ZIP` bestand binnen de adreslijst.  Hiermee maakt u een `scripts` directory.

## <a name="scaffold-app"></a>De nieuwe Azure Mobile-Apps-end scaffold

Voer de volgende opdracht uit de map met de map scripts:

```scaffold-mobile-app scripts out```

Hiermee maakt u een scaffolded backend Azure Mobile-Apps in de `out` directory.  Hoewel het niet verplicht, is het een goed idee om de `out` map naar de bibliotheek van een bron-code van uw keuze.

## <a name="deploy-ama-app"></a>Uw backend Azure Mobile-Apps implementeren

Tijdens de implementatie moet u als volgt te werk:

1. Maak een nieuwe Mobile-App in de [Portal van Azure].
2. Voer de `createViews.sql` script in uw verbonden database.
3. Koppeling van de database die is gekoppeld aan uw Mobile-Service naar de nieuwe App-Service.
4. Andere bronnen (zoals melding Hubs) een koppeling naar de nieuwe App-Service.
5. De gegenereerde code dashboard implementeren naar uw nieuwe site.

### <a name="create-a-new-mobile-app"></a>Een nieuwe Mobile-App maken

1. Meld u aan bij de [Portal van Azure].

2. Klik op **+ Nieuw** > **Web + Mobile** > **Mobile-App**, Geef een naam voor uw backend Mobile-App.

3. Selecteer een bestaande resourcegroep voor de **Resourcegroep**, of maak een nieuwe (met dezelfde naam als uw app). 
 
    U kunt een andere App serviceplan selecteren of een nieuw account te maken. Voor meer informatie over de App Services plannen en het maken van een nieuw abonnement in een verschillende prijzen trapsgewijs en op de gewenste locatie, Zie [uitgebreide overzicht van Azure-Service voor App-abonnementen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. Voor het **App-abonnement**, is de standaard-abonnement (in de [standaard laag](https://azure.microsoft.com/pricing/details/app-service/)) ingeschakeld. U kunt ook een ander abonnement of [Maak een nieuw](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan)selecteren. Instellingen van de planning van de App Service bepalen de [locatie, functies, kosten en berekenen van resources](https://azure.microsoft.com/pricing/details/app-service/) die zijn gekoppeld aan uw app. 

    Nadat u hebt besloten op het abonnement, klikt u op **maken**. Hiermee wordt de Mobile-App-end gemaakt. 


### <a name="run-createviewssql"></a>CreateViews.SQL uitvoeren

De scaffolded app bevat een bestand met de naam `createViews.sql`.  Dit script moet worden uitgevoerd voor de doeldatabase.  De verbindingsreeks voor de doeldatabase kan worden verkregen van uw gemigreerde mobiele service uit het blad **Instellingen** onder **Tekenreeksen met de verbinding**.  Deze heet `MS_TableConnectionString`.

U kunt dit script in SQL Server Management Studio of Visual Studio uitvoeren.

### <a name="link-the-database-to-your-app-service"></a>De Database koppelen aan uw App-Service

De bestaande database koppelen aan uw App-Service:

- Open uw App-Service in de [Portal van Azure].
- Selecteer **alle instellingen** -> **Data Connections**.
- Klik op **+ Add**.
- Selecteer in de vervolgkeuzelijst **SQL-Database**
- Klik onder **SQL-Database**, selecteert u de bestaande database en klik vervolgens op op **selecteren**.
- Klik onder **verbindingsreeks**, voert u de gebruikersnaam en wachtwoord voor de database en klik vervolgens op op **OK**.
- Klik op **OK**in het blad **gegevensverbindingen toevoegen** .

De gebruikersnaam en wachtwoord vindt u door de verbindingsreeks voor de doeldatabase in uw gemigreerde Mobile-Service te bekijken.


### <a name="set-up-authentication"></a>Verificatie instellen

Azure Mobile-Apps kunt u Azure Active Directory, Facebook, Google, Microsoft en Twitter verificatie binnen de service configureren.  Aangepaste verificatie moet afzonderlijk worden ontwikkeld.  Raadpleeg de [Verificatie concepten] documentatie en [Verificatie Quickstart] documentatie voor meer informatie.  

## <a name="updating-clients"></a>Bijwerken van mobiele clients

Nadat u een operationele Mobile-App backend hebt, kunt u op een nieuwe versie van de clienttoepassing waarin deze verbruikt werken. Mobile-Apps bevat ook een nieuwe versie van de client SDK's en vergelijkbaar met de serverupgrade hierboven, moet u alle verwijzingen naar de mobiele Services SDK's verwijderen voordat u de versies Mobile-Apps installeert.

Een van de belangrijkste wijzigingen tussen de versies is dat de constructors niet langer een toepassingstoets vereist. U nu doorgeven gewoon in de URL van de Mobile-App. Bijvoorbeeld op de .NET-clients, de `MobileServiceClient` opbouwfunctie is nu:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

U kunt lezen over het installeren van de nieuwe SDK's en het gebruik van de structuur van de nieuwe via de onderstaande koppelingen:

- [Android versie 2.2 of hoger](app-service-mobile-android-how-to-use-client-library.md)
- [iOS versie 3.0.0 of hoger](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) versie 2.0.0 of hoger](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova versie 2.0 of hoger](app-service-mobile-cordova-how-to-use-client-library.md)

Als uw toepassing maakt gebruik van push-meldingen, noteer de specifieke registratie-instructies voor elk platform, als er zijn enkele wijzigingen er ook.

Wanneer u de nieuwe clientversie klaar hebt, uitproberen ten opzichte van uw serverproject bijgewerkte. Na de validatie dat deze altijd werkt, kunt u een nieuwe versie van uw toepassing naar klanten vrijgeven. Uiteindelijk nadat uw klanten hebt deze updates wilt ontvangen, kunt u de Mobile-Services-versie van uw app verwijderen. U hebt nu volledig bijgewerkt naar een App-Service Mobile-App wordt gebruikt, met de meest recente SDK van de Mobile-Apps-server.

<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Wat zijn de Mobile-Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[App-Service prijzen]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Verificatie-concepten]: ../app-service/app-service-authentication-overview.md
[Verificatie Quickstart]: app-service-mobile-auth.md

[Azure-Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
