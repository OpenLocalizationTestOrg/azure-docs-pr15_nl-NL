<properties
    pageTitle="Azure App-Service voor het web, mobile en API apps | Microsoft Azure"
    description="Lees hoe Azure App Service helpt u ontwikkeling, implementatie en web en mobiele apps beheren."
    keywords="App-service, azure app-service, app service kosten, schaal scalable, app-implementatie, azure app-implementatie, paas, platform-als-een-service, website, website, web, azure mobiele"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Wat is de App-Azure-Service?

*App-Service* is een [platform-als-een-service](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) aanbod van Microsoft Azure. Web en mobiele apps voor platform of apparaten maken. Uw apps integreren met SaaS oplossingen, verbinding maken met on-premises implementatie-toepassingen en uw bedrijfsprocessen automatiseren. Azure wordt uitgevoerd van uw apps op volledig beheerde virtuele machines (VMs), met uw keuze van gedeelde VM bronnen of speciale VMs.

App-Service bevat de web en mobiele mogelijkheden die we eerder afzonderlijk bezorgd als Azure Websites en Azure Mobile Services. Het bevat ook nieuwe mogelijkheden voor bedrijfsprocessen automatiseren en hosten cloud API's. Als een geïntegreerde service kunt App Service u verschillende onderdelen--websites, achterste uiteinden van de mobiele app RESTful API's en bedrijfsprocessen--opstellen in een enkele oplossing.

De volgende 4 minuten video bevat een korte uitleg over hoe App Service zich verhoudt tot eerdere Azure aanbiedingen en wat is er nieuw in deze.

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>Waarom App Service gebruiken?

Hier volgen enkele belangrijke functies en mogelijkheden van de App Service:

- **Meerdere talen en kaders** - App Service heeft eersteklas ondersteuning voor ASP.NET, Node.js Java, PHP en Python. U kunt ook op App-Service VMs met [Windows PowerShell en andere scripts of uitvoerbare bestanden](../app-service-web/web-sites-create-web-jobs.md) uitvoeren.

- **DevOps optimalisatie** - instellen [continue integratie en implementatie](../app-service-web/app-service-continuous-deployment.md) met Visual Studio Team Services, GitHub of BitBucket. Niveau verhogen updates door middel van [testen en tijdelijke omgevingen](../app-service-web/web-sites-staged-publishing.md). Uitvoeren [A / B testen](../app-service-web/app-service-web-test-in-production-get-start.md). Uw apps in de App-Service beheren met behulp van [Azure PowerShell](../powershell-install-configure.md) of de [interface van de opdrachtregel platforms (CLI)](../xplat-cli-install.md).

- **Globale schalen met hoge beschikbaarheid** - schaal [up](../app-service-web/web-sites-scale.md) - of [Uitzoomen](../monitoring-and-diagnostics/insights-how-to-scale.md) handmatig of automatisch. Hosten van uw apps op een willekeurige plek in de Microsoft datacenter van de globale infrastructuur en de App-Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) belooft beschikbaarheid.

- **Verbindingen met SaaS platforms en on-premises gegevens** - kiezen uit meer dan 50 [verbindingslijnen](../connectors/apis-list.md) voor enterprise-systemen (zoals SAP, Siebel en Oracle), SaaS-services (zoals Salesforce en Office 365) en internetservices (zoals Facebook en Twitter). Toegang tot on-premises gegevens met [Hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) en [Azure virtuele netwerken](../app-service-web/web-sites-integrate-with-vnet.md).

- **Beveiliging en naleving** - App-Service is [ISO, SOC, en PCI voldoen](https://www.microsoft.com/TrustCenter/).

- **Toepassingssjablonen** - kiezen uit een uitgebreide lijst met Toepassingssjablonen [Azure Marketplace](https://azure.microsoft.com/marketplace/) dat kunt u gebruikmaken van een wizard populaire open source software zoals WordPress, Joomla en Drupal te installeren.

- **Integratie van visual Studio** - specifieke tools in Visual Studio stroomlijnen de werklast van maken, implementeert en foutopsporing.

## <a name="app-types-in-app-service"></a>App-typen in de App-Service

App Service bevat verschillende *typen van de app*, die elk is bedoeld voor het hosten van een bepaald soort werkbelasting:

- [**Web Apps**](../app-service-web/app-service-web-overview.md) - voor het hosten van websites en webtoepassingen.

- [**Mobiele Apps**](../app-service-mobile/app-service-mobile-value-prop.md) Een back-uiteinden voor het hosten van de mobiele app.

- [**API-Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) - voor het hosten van RESTful API's.

- [**Logica Apps**](../app-service-logic/app-service-logic-what-are-logic-apps.md) - voor bedrijfsprocessen automatiseren en het integreren van systemen en gegevens over wolken-code te schrijven.

De word- *app* hier verwijst naar de hostingprovider bronnen voor het uitvoeren van een werkbelasting. Maken van notities "WebApp' als voorbeeld, u bent waarschijnlijk gewend aan ik denk aan een web-app als de berekeningscluster resources en de toepassingscode die samen functionaliteit overbrengen op een browser. Maar in App Service een *WebApp* de berekeningscluster resources die Azure biedt voor het hosten van uw toepassingscode. Als uw toepassing bestaat van een web front-end en een RESTful API back-end, u beide naar een web-app kan implementeren of u uw front-code kan implementeren naar een web-app en uw back-enddatabase-code aan een API-app. Uw toepassing kan van meerdere App Service-apps van verschillende soorten bestaan.

## <a name="app-service-plans"></a>App-Service-abonnementen

[App-serviceplannen](azure-web-sites-web-hosting-plans-in-depth-overview.md) , geef het soort berekeningscluster resources die uw apps worden uitgevoerd op. Als u de light-verkeer is toegestaan laadtijd verwacht, kunt u gedeelde VMs (**gratis** en **gedeeld** prijzen van lagen). Voor hogere belasting, kunt u kiezen uit verschillende grootten van speciale VMs (**Basic**, **Standard**en **Premium** lagen). Meerdere App Service apps hetzelfde abonnement kunnen delen en ze de schaal van omhoog en omlaag de prijzen lagen samen in het abonnement.

Als u meer schaalbaarheid en netwerk moeten worden geïsoleerd, kunt u uw apps kunt uitvoeren in een [App-omgeving](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Prijzen

Zie voor informatie over hoeveel kosten App Service [Prijzen van App-Service](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>App-Service uittesten

[Een voorbeeld web-app maken, mobiele app, of logica app](http://go.microsoft.com/fwlink/?LinkId=523751) en afspelen met deze voor 1 uur staan, zonder creditcard vereist, niet verplichtingen, geen voorkomt.

Of een [gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/)opent, en probeer een van onze introductie zelfstudies:

* [Zelfstudie: Een WebApp maken](../app-service-web/app-service-web-get-started.md)
* [Zelfstudie: Een mobiele app maken](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Zelfstudie: Een API-app maken](../app-service-api/app-service-api-dotnet-get-started.md)
* [Zelfstudie: Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)
