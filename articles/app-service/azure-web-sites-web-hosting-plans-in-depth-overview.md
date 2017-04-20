<properties
    pageTitle="Azure App Service abonnementen uitgebreide overzicht | Microsoft Azure"
    description="Leer hoe App Service-abonnementen voor werk dat Azure App-Service en hoe ze de management-functionaliteit profiteert."
    keywords="App-service, azure app service, schaal scalable, app-abonnement, app servicekosten"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-plans-in-depth-overview"></a>Uitgebreide overzicht van het abonnement Azure App-Service#

Een App-serviceplan vertegenwoordigt een reeks functies en capaciteit die u tussen meerdere apps delen kunt. WebApps, Mobile-Apps, Apps van de functie of API-Apps, in de [App-Azure-Service](http://go.microsoft.com/fwlink/?LinkId=529714) alle worden uitgevoerd in een App-serviceplan. Deze abonnementen ondersteunen vijf prijzen lagen: *gratis*, *gedeeld*, *Basic*, *Standard*en *Premium*. Elke laag heeft een eigen mogelijkheden en capaciteit. Apps in de hetzelfde abonnement en geografische locatie kunnen delen van een abonnement. De apps die delen van een abonnement kunnen gebruiken voor alle voorzieningen en functies die zijn gedefinieerd door de niveaus van de planning. Alle apps die zijn gekoppeld aan een abonnement worden uitgevoerd op de resources die het abonnement wordt gedefinieerd.

Bijvoorbeeld als uw abonnement is geconfigureerd voor gebruik van twee "kleine" exemplaren in de standaard service-laag, alle apps die zijn gekoppeld aan dat plan uitgevoerd op beide exemplaren en hebben toegang tot de functionaliteit van de laag standaard service. Plan exemplaren waarop apps uitvoert zijn volledig beheerde en ten zeerste beschikbaar.

In dit artikel bevat informatie over de belangrijkste kenmerken, zoals laag en schaal van een abonnement dat is App en hoe ze zich voordoen bij afspelen tijdens het beheren van uw apps.

## <a name="apps-and-app-service-plans"></a>Apps en App-Service-abonnementen

Een app in de App-Service kan worden gekoppeld aan slechts één App serviceplan op elk gewenst moment.

Apps en -abonnementen zijn opgenomen in een resourcegroep. Een resourcegroep fungeert als de levenscyclus van de grens voor elke resource die erin. U kunt resourcegroepen gebruiken voor het beheren van alle onderdelen van een toepassing samen.

Omdat een één resourcegroep meerdere App Service-abonnementen hebt kan, kunt u verschillende-apps voor verschillende fysieke resources toewijzen. U kunt bijvoorbeeld bronnen tussen ontwikkelaar, test- en -omgevingen scheiden. Aparte omgevingen voor productie en ontwikkelaar/testen ondervindt, kunt u isoleren resources. Op deze manier laden testen ten opzichte van een nieuwe versie van uw apps niet zijn geconfigureerd voor dezelfde bronnen als uw productie-apps, waarin nu reële klanten bedienen.

Wanneer u meerdere abonnementen in een één resourcegroep hebt, kunt u ook een toepassing die betrekking hebben op geografische regio's definiëren. Een maximaal beschikbare app uitgevoerd in twee regio's bevat bijvoorbeeld ten minste twee abonnementen, één voor elke regio en één app dat is gekoppeld aan elke abonnement. In een dergelijke situatie, bevinden alle kopieën van de app vervolgens zich in een één resourcegroep. Een resourcegroep met meerdere abonnementen en meerdere apps ondervindt, kunt u gemakkelijk kunt beheren, beheren en weergeven van de status van de toepassing.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Maken van een App Service-abonnement of bestaande gebruiken

Wanneer u een app maakt, kunt u overwegen een resourcegroep maken. Aan de andere kant, als de app die u wilt maken, een onderdeel voor een grotere toepassing is, kan deze app moet worden gemaakt in de resourcegroep die toegewezen aan die groter toepassing.

Of de nieuwe app een geheel nieuwe toepassing of een deel van een groter voorbeeld is, kunt u de host deze of een nieuw account te maken met een bestaand App-serviceplan. Deze beschikking is meer een vraag van capaciteit en verwachte laden.

Als deze nieuwe app dient te veel resources gebruiken en andere factoren van de andere apps schaling in een bestaand abonnement gehost hebt, kunt u deze isoleren in een eigen abonnement.

Wanneer u een abonnement hebt gemaakt, kunt u een nieuwe set resources toewijzen voor de app en betere controle over resourcetoewijzing krijgen, omdat elk abonnement een eigen set exemplaren krijgt.

Omdat u apps in abonnementen verplaatsen kunt, kunt u de manier waarop resources worden toegewezen over de toepassing groter te maken.

Ten slotte, als u wilt maken van een app in een andere regio en dat gebied geen een bestaand abonnement, maak een plan in dat gebied voor het hosten van uw app er kunnen.

## <a name="create-an-app-service-plan"></a>Een App Service maken

>[AZURE.TIP] Als u een App-Service-omgeving hebt kunt u de documentatie specifiek zijn voor de App serviceomgevingen hier controleren: [een App Service plannen in een omgeving van de Service App maken](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

U kunt een lege App-serviceplan maken uit de App Service abonnement bladeren ervaring of als onderdeel van de app maken.

Klik in de [portal van Azure](https://portal.azure.com)op **Nieuw** > **Web + mobile**, en selecteer vervolgens **Web App** of ander type App Service-app.
![Een app maken in de portal van Azure.][createWebApp]

U kunt vervolgens selecteren of maak de App Service-plan voor de nieuwe app.

 ![Maak een plan App Service.][createASP]

Als u wilt een nieuwe App-serviceplan hebt gemaakt, klik op **[+] nieuwe maken**, typt u de naam van de **App-abonnement** en selecteer vervolgens een veilige **locatie**. **Prijzen laag**op en selecteer vervolgens een juiste prijzen laag voor de service. Selecteer **Alles weergeven** om weer te geven meer prijzen opties, zoals **vrije** en **gedeeld**. Nadat u de prijzen laag hebt geselecteerd, klikt u op de knop **selecteren** .

## <a name="move-an-app-to-a-different-app-service-plan"></a>Een app verplaatsen naar een ander abonnement van de App-Service

Een app kunt u verplaatsen naar een andere app serviceplan in de [portal van Azure](https://portal.azure.com). U kunt apps verplaatsen tussen abonnementen zo lang maken als de abonnementen in de resourcegroep voor dezelfde en geografisch gebied zijn.

Als u wilt een app verplaatst naar een ander abonnement, gaat u naar de app die u wilt verplaatsen. Zoek op het menu **Instellingen** naar de **Wijziging App Service plannen**.

**Wijziging App-abonnement** wordt geopend de Gegevenskiezer **App serviceplan** . Nu kunt u kiezen van een bestaand abonnement of een nieuw account te maken. Alleen geldige abonnementen (in de dezelfde resourcegroep en de geografische locatie) worden weergegeven.

![App-Service plannen Gegevenskiezer.][change]

Elk abonnement bevat een eigen prijzen van laag. Bijvoorbeeld wanneer u een site van een gratis laag verplaatsen naar een gewone laag, uw app nu kunt gebruiken de functies en bronnen van de standaard laag.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Een app naar een ander abonnement van de App Service klonen
Als u de app verplaatsen naar een ander gebied wilt, is een alternatief-app klonen. Klonen, wordt een kopie van de app in een nieuw of bestaand App-abonnement of de App Service-omgeving in elke regio.

 ![Een app klonen.][appclone]

U kunt **Klonen App** vinden in het menu **Extra** .

Klonen heeft enkele beperkingen die u over op [Azure App Service App klonen met behulp van Azure portal lezen kunt](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>De schaal van een App-serviceplan aanpassen

Er zijn drie manieren om te schalen dat een abonnement:

- **Wijzigen van het plan prijzen laag**. Bijvoorbeeld een abonnement in de eenvoudige laag kan worden omgezet in een laag standaard of Premium en alle apps die nu zijn gekoppeld aan dat plan de beschikking over de functies die de nieuwe servicelaag biedt.
- **Van het plan exemplaar grootte wijzigen**. Als u bijvoorbeeld kunt een abonnement in de eenvoudige laag met kleine exemplaren wijzigen in grote exemplaren. Alle apps die nu zijn gekoppeld aan dit abonnement kunnen gebruiken voor de meer geheugen en CPU-bronnen die de groter exemplaar biedt.
- **Wijzigen van het plan exemplaar tellen**. Een standaard-abonnement dat wordt aangepast in drie exemplaren kan bijvoorbeeld worden vergroot in 10-exemplaren. Een Premium-abonnement kan worden aangepast af in 20-exemplaren (afhankelijk van beschikbaarheid). Alle apps die nu zijn gekoppeld aan dit abonnement kunnen gebruiken voor de meer geheugen en CPU-bronnen die de grotere exemplaar telling biedt.

U kunt de prijzen laag en exemplaar grootte wijzigen door te klikken op het item **Schaal omhoog** onder instellingen voor de app of het App-abonnement. Wijzigingen toepassen op het App-abonnement en invloed op alle apps waarop deze gehost.

 ![Waarden voor de schaal van een app instellen.][pricingtier]

## <a name="app-service-plan-cleanup"></a>App-Service plannen opruimen
**App-Service-abonnementen** die geen apps die is gekoppeld aan hen hebt gelden de tarieven aangezien ze gaat u verder met het reserveren van de capaciteit berekeningscluster is geconfigureerd in de App Service abonnement schaaleigenschappen.
Om te voorkomen onverwachte kosten, wanneer de laatste app gehost in een App Service-abonnement wordt verwijderd, wordt de resulterende lege App serviceplan ook verwijderd.


## <a name="summary"></a>Overzicht

App-Service-abonnementen bevatten een set functies en capaciteit die u tussen uw apps delen kunt. App-Service-abonnementen voor de flexibiliteit specifieke apps naar een reeks resources toewijzen en verder optimaliseren uw Azure Resourcegebruik. Op deze manier als u wilt geld opslaan op uw testomgeving, kunt u een abonnement delen tussen meerdere apps. U kunt ook doorvoer maximaliseren voor uw productieomgeving door deze schaalbaarheid op meerdere regio's en abonnementen.

## <a name="whats-changed"></a>Wat er gewijzigd

* Zie voor een handleiding voor het wijzigen van Websites naar App-Service,: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png
