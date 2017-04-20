<properties
    pageTitle="Overzicht van de Apps met webonderdelen | Microsoft Azure"
    description="Leer hoe Azure-Service voor App kunt u ontwikkelen en host-webtoepassingen"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Overzicht van de Web-Apps

*App Service Web Apps* is een volledig beheerde berekeningscluster-platform die is geoptimaliseerd voor het hosten van websites en webtoepassingen. Deze aanbieding [platform-als-een-service](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) van Microsoft Azure kunt u zich richten op uw bedrijfslogica terwijl Azure zorgt voor de infrastructuur uit te voeren en schaal van uw apps.

De volgende video 5 minuten maakt u kennis met Azure App Service Web Apps.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Wat is een web-app in de App Service?

In de App-Service is een *WebApp* de berekeningscluster resources die Azure biedt voor het hosten van een website of web-toepassing.  

De bronnen berekeningscluster mogelijk op gedeelde of speciale virtuele machines (VMs), afhankelijk van de prijzen laag die u kiest. De toepassing-programmacode wordt uitgevoerd in een beheerde VM die wordt geïsoleerd van andere klanten.

Uw code kan in een taal of framework die wordt ondersteund door [Azure App-Service](../app-service/app-service-value-prop-what-is.md), zoals ASP.NET, Node.js, Java, PHP of Python zijn. U kunt ook uitvoeren van scripts die [PowerShell en andere talen uitvoeren van scripts](web-sites-create-web-jobs.md#acceptablefiles) in een web-app gebruiken.

Zie [Web app-scenario's](https://azure.microsoft.com/documentation/scenarios/web-app/) en de sectie **scenario's en aanbevelingen** van [Azure App-Service, virtuele Machines, Service stof, en Cloud Services-vergelijking](choose-web-site-cloud-service-vm.md#scenarios)voor voorbeelden van veelgebruikte toepassing-scenario's die u kunt Web-Apps voor.

## <a name="why-use-web-apps"></a>Het nut van Web Apps?

Hier volgen enkele belangrijke functies van App-Service die van toepassing op Web Apps:

- **Meerdere talen en kaders** - App Service heeft eersteklas ondersteuning voor ASP.NET, Node.js Java, PHP en Python. U kunt ook [PowerShell en andere scripts of uitvoerbare bestanden](../app-service-web/web-sites-create-web-jobs.md) op App-Service VMs uitvoeren.

- **DevOps optimalisatie** - instellen [continue integratie en implementatie](../app-service-web/app-service-continuous-deployment.md) met Visual Studio Team Services, GitHub of BitBucket. Niveau verhogen updates door middel van [testen en tijdelijke omgevingen](../app-service-web/web-sites-staged-publishing.md). Uitvoeren [A / B testen](../app-service-web/app-service-web-test-in-production-get-start.md). Uw apps in de App-Service beheren met behulp van [Azure PowerShell](../powershell-install-configure.md) of de [interface van de opdrachtregel platforms (CLI)](../xplat-cli-install.md).

- **Globale schalen met hoge beschikbaarheid** - schaal [up](../app-service-web/web-sites-scale.md) - of [Uitzoomen](../monitoring-and-diagnostics/insights-how-to-scale.md) handmatig of automatisch. Hosten van uw apps op een willekeurige plek in de Microsoft datacenter van de globale infrastructuur en de App-Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) belooft beschikbaarheid.

- **Verbindingen met SaaS platforms en on-premises gegevens** - kiezen uit meer dan 50 [verbindingslijnen](../connectors/apis-list.md) voor enterprise-systemen (zoals SAP, Siebel en Oracle), SaaS-services (zoals Salesforce en Office 365) en internetservices (zoals Facebook en Twitter). Toegang tot on-premises gegevens met [Hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) en [Azure virtuele netwerken](../app-service-web/web-sites-integrate-with-vnet.md).

- **Beveiliging en naleving** - App-Service is [ISO, SOC, en PCI voldoen](https://www.microsoft.com/TrustCenter/).

- **Toepassingssjablonen** - kiezen uit een uitgebreide lijst met Toepassingssjablonen [Azure Marketplace](https://azure.microsoft.com/marketplace/) dat kunt u gebruikmaken van een wizard populaire open source software zoals WordPress, Joomla en Drupal te installeren.

- **Integratie van visual Studio** - specifieke tools in Visual Studio stroomlijnen de werklast van maken, implementeert en foutopsporing.

Daarnaast kunt een web-app profiteren van functies die worden aangeboden door [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (zoals CORS-ondersteuning) en [Mobile-Apps](../app-service-mobile/app-service-mobile-value-prop.md) (zoals push-meldingen). Zie [overzicht van de Azure App-Service](../app-service/app-service-value-prop-what-is.md)voor meer informatie over het typen van de app in de App-Service.

Naast de Web-Apps in de App Service biedt Azure andere diensten die kunnen worden gebruikt voor het hosten van websites en webtoepassingen. Web Apps is voor de meeste scenario's het beste keuze.  Houd rekening met [Service stof](https://azure.microsoft.com/documentation/services/service-fabric)voor microservice architectuur, en als u meer controle over de VMs die uw code wordt uitgevoerd op nodig hebt, kunt u [Azure virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). Zie voor meer informatie over het kiezen tussen deze Azure services [Azure App-Service, virtuele Machines, Service stof, en Cloud Services-vergelijking](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Aan de slag

Als u wilt beginnen met het voorbeeldcode wordt geïmplementeerd in een nieuwe WebApp in de App Service, volgt u de zelfstudie [Deploy uw eerste web-app naar Azure 5 minuten](app-service-web-get-started.md) . U hebt een gratis Azure-account nodig.

Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.
