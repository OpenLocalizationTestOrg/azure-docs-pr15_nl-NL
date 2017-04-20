<properties
    pageTitle="Wat zijn de Mobile-Apps"
    description="Lees welke voordelen App Service voren en naar uw mobiele bedrijfs-apps."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="getting-started"> </a>Wat is Mobile-Apps?

Azure App-Service is een volledig beheerde [Platform als een Service](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) aanbod voor professionele ontwikkelaars die bij een groot aantal mogelijkheden voor het web, mobiele scenario's en integratie brengt. *Mobile-Apps* in *Azure App Service* bieden een zeer scalable, algemeen beschikbaar mobiele development platform voor softwareontwikkelaars en systeemintegrators die bij een groot aantal mogelijkheden voor mobiele ontwikkelaars brengt.

![Mobiele Apps](./media/app-service-mobile-value-prop/overview.png)

##<a name="why-mobile-apps"></a>Waarom mobiele Apps?
*Mobile-Apps* in *Azure App-Service* biedt een zeer scalable, algemeen beschikbaar mobiele development platform voor softwareontwikkelaars en systeemintegrators die bij een groot aantal mogelijkheden voor mobiele ontwikkelaars brengt. Met de Mobile-Apps kunt u het volgende doen:

- **Systeemeigen en cross platform apps maken** - of u samenstelt systeemeigen iOS, Android en Windows-apps of Xamarin of Cordova (Phonegap) van meerdere platforms apps, kunt u profiteren van App-Service systeemeigen SDK's gebruiken.
- **Verbinding maken met uw systemen enterprise** - met Mobile-Apps kunt u zakelijke Aanmeldingsadres toevoegen op in minuten, en verbinding maken met uw onderneming on-premises of cloud resources.
- **Gereed voor offline-apps gebruiken met de gegevenssynchronisatie van maken** - uw mobiele werknemers productief door apps bouwen die werken offline en maken Mobile-Apps gegevens te synchroniseren op de achtergrond wanneer connectiviteit met een van uw onderneming gegevensbronnen of SaaS APIs aanwezig is.
- **Push-meldingen naar miljoenen in seconden** : uw klanten met direct push-meldingen op elk apparaat, afgestemd op zij nodig hebben, die wordt verzonden wanneer het tijd geschikt is deelnemen.

## <a name="mobile-app-features"></a>Functies van de mobiele App
De volgende functies zijn belangrijk tot het ontwikkelen van mobiele cloud ingeschakelde:

- **Verificatie en machtiging** - selecteren uit een groeiende reeks identiteitsprovider, met inbegrip van Azure Active Directory voor enterprise-verificatie, plus sociale mailproviders zoals Facebook, Google, Twitter en Microsoft-Account.  Azure Mobile-Apps biedt een OAuth 2.0-service voor elke provider.  U kunt ook de SDK integreren voor het id-provider voor specifieke functionaliteit provider.

  Ontdek meer over onze [verificatiefuncties].

- **Data Access** - Azure Mobile-Apps biedt mobile-vriendelijke OData v3 gegevensbron gekoppeld aan SQL Azure wordt aangegeven of een lokale SQL Server.  Deze service kan worden gebaseerd op entiteit Framework, zodat u kunt eenvoudig integreren met andere NoSQL en SQL-gegevensproviders, met inbegrip van [Azure Table Storage], MongoDB, [DocumentDB] en SaaS API-providers zoals Office 365 en Salesforce.com.
- **Offlinesynchronisatie** - Our Client SDK's tabelmaakquery deze gemakkelijk kunt maken van krachtige en heeft gereageerd mobiele toepassingen die met een offline gegevens werken instellen die kunnen automatisch worden gesynchroniseerd met de backend-gegevens, inclusief conflict resolutie-ondersteuning.

  Ontdek meer over onze [gegevensfuncties].

- **Push-meldingen** : onze Client SDK's naadloos integreren met de mogelijkheden van de registratie van Azure melding Hubs, zodat u kunt de push-meldingen voor miljoenen gebruikers tegelijk verzenden.

  Ontdek meer over onze [push-meldingen functies].

- **Client SDK's** - wij bieden een volledige reeks Client SDK's waarin systeemeigen ontwikkeling ([iOS], [Android] en [Windows]), platforms ontwikkeling ([Xamarin voor iOS en Android], [Xamarin formulieren]) en ontwikkeling van de hybride-toepassingen ([Apache Cordova]).  Elke client SDK is beschikbaar met een MIT-licentie en open source is.

## <a name="azure-app-service-features"></a>Functies van Azure App-Service.
De volgende platform-functies zijn altijd nuttig zijn voor mobiele productielocaties.

- **Automatisch schalen** - App Service, kunt u snel schaal-up of uit te verwerken binnenkomende klant belasting. Handmatig selecteert u het aantal en het formaat van VMs of automatisch schalen naar uw mobiele app backend op basis van laden of planning schaal instellen.

  Ontdek meer over het [automatisch schalen].

- **Tijdelijke omgevingen** - App Service kan worden uitgevoerd meerdere versies van uw site, zodat u kunt uitvoeren A / B testen, testen in productie als onderdeel van een grotere DevOps-abonnement en in-place tijdelijke van een nieuwe backend.

  Ontdek meer over het [tijdelijk opslaan omgevingen].

- **Continue implementatie** - App Service kunt integreren met algemene SCM-systemen, zodat u kunt automatisch een nieuwe versie van uw backend implementeren door te drukken naar een tak van uw systeem SCM.

  Ontdek meer over de [implementatieopties voor].

- **Virtuele netwerkproblemen** - App Service kunnen worden verbonden met on-premises implementatie resources via virtuele netwerk, ExpressRoute of hybride verbindingen.

  Ontdek meer over [hybride verbindingen], [virtuele netwerken]en [ExpressRoute].

- **Geïsoleerd / speciale omgevingen** -App Service kan worden uitgevoerd in een volledig geïsoleerd en speciale milieu voor het uitvoeren van Azure-Service voor App-apps veilig op hoog niveau.  Dit is ideaal voor toepassing werkbelasting vereisen van zeer hoog schaal, moeten worden geïsoleerd of beveiligde netwerktoegang.

  Kennismaken met meer informatie over de [App serviceomgevingen].

## <a name="getting-started"></a>Aan de slag ##
Als u wilt beginnen met de Mobile-Apps, volgt u de zelfstudie [Aan de slag] .  Hier wordt uitgelegd hoe de basisbeginselen van een mobiele backend en de client van uw keuze en klik vervolgens integreren verificatie, offline synchroniseren en push-meldingen.  U kunt volgen de zelfstudie [Aan de slag] enkele malen - eenmaal voor elke clienttoepassing.

Raadpleeg de onze [kaart van leermateriaal]voor meer informatie over Azure Mobile-Apps.
Zie voor meer informatie over het platform Azure App-Service, [Azure App-Service].

>[AZURE.NOTE]Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

<!-- URLs. -->
[Migrate your Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Azure App-Service]: ../app-service/app-service-value-prop-what-is.md
[Aan de slag]: app-service-mobile-ios-get-started.md
[Azure-tabelopslag]: ../storage/storage-getting-started-guide.md
[DocumentDB]: ../documentdb/documentdb-get-started.md
[verificatiefuncties]: ./app-service-mobile-auth.md
[functies voor gegevens]: ./app-service-mobile-offline-data-sync.md
[functies voor push-meldingen]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android-apparaat]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin voor iOS en Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin formulieren]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[automatisch schalen]: ../app-service-web/web-sites-scale.md
[tijdelijk opslaan omgevingen]: ../app-service-web/web-sites-staged-publishing.md
[Opties voor distributie]: ../app-service-web/web-sites-deploy.md
[hybride verbindingen]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[virtuele netwerken]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service-web/app-service-app-service-environment-network-configuration-expressroute.md
[App-Service omgevingen]: ../app-service-web/app-service-app-service-environment-intro.md
[overzicht van leermateriaal]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/
