<properties
    pageTitle="Azure stapel Web Apps-overzicht | Microsoft Azure"
    description="Overzicht van Web-Apps Azure gestapelde"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Overzicht van Azure stapel Web Apps
    
> [AZURE.NOTE] De volgende informatie is alleen van toepassing op Azure stapel TP1 implementaties.

Azure stapel Web Apps is het eerste element van de Azure App Service aanbod Azure stapel heeft aangebracht. Het installatieprogramma van Azure stapel Web Apps wordt een exemplaar van elk van de vijf vereiste rol typen gemaakt en wordt ook een bestandsserver maken. Hoewel u meer exemplaren voor elk van de rol typen toevoegen kunt, weten dat er niet veel ruimte voor VMs in Technical Preview-1. De huidige mogelijkheden voor Azure stapel Web Apps zijn hoofdzakelijk foundation-mogelijkheden die nodig zijn voor het beheren van de systeem- en host web-apps.

![Azure stapel App Service WebApps in de Azure stapelen Portal][1]

## <a name="limitations-of-the-technical-preview"></a>Beperkingen van de technische bètaversie

Er is geen ondersteuning voor de preview-versies van Azure stapel App-Service. Plaats geen productie werkbelasting op deze preview-versie. Er is ook geen upgrade tussen Azure stapel App Service preview-versies. De primaire doelstellingen van deze preview-versies zijn om weer te geven wat we bieden en feedback worden opgehaald. 

## <a name="what-is-an-app-service-plan"></a>Wat is een App-Service plannen?

De provider van de resource Azure stapel Web Apps gebruikt dezelfde code die de functie Azure Web Apps in de App Azure-Service wordt gebruikt. Hierdoor worden enkele veelvoorkomende concepten zegt waarin wordt beschreven. In Web-Apps heet de prijzen container voor WebApps het App-abonnement. Staat dit voor het instellen van specifieke virtuele machines voor het opslaan van uw apps. Binnen een bepaald abonnement, kunt u meerdere App Service-abonnementen hebt. Dit geldt ook in Azure stapel Web Apps. 

In Azure wordt aangegeven, zijn er gedeelde en speciale werknemers. Een gedeelde werknemer ondersteunt HD multitenant app webhosting en er is slechts één set gedeelde werknemers. Speciale servers alleen worden gebruikt door een tenant en komt in drie grootten: kleine, middelgrote en grote. De behoeften van on-premises-klanten kunnen niet altijd worden beschreven met behulp van deze termen. In Azure stapel Web Apps, kunnen beheerders van de resource-provider voor de werknemer niveaus die ze beschikbaar maken wilt dat ze hebben meerdere sets met gedeelde werknemers of verschillende sets met speciale werknemers op basis van de behoeften van hun unieke webhosting definiëren. Met deze definities van de laag werknemer, kunnen ze opgeven hun eigen prijzen SKU's.

## <a name="portal-features"></a>Portal-functies

Zoals u geldt ook voor de back-end, gebruikt Azure stapel Web Apps dezelfde gebruikersinterface die gebruikmaakt van Azure Web Apps. Sommige functies zijn uitgeschakeld en worden niet nog functionele Azure gestapelde. Dit komt doordat Azure-specifieke verwachtingen of services waarvoor deze functies nog niet beschikbaar in Azure stapel zijn. 

## <a name="next-steps"></a>Volgende stappen

- [Voordat u aan de slag met Azure stapel Web Apps](azure-stack-webapps-before-you-get-started.md)
- [Installeer de Web-Apps Resource-Provider](azure-stack-webapps-deploy.md)

U kunt ook andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md), zoals de [resource-provider voor SQL Server](azure-stack-sql-rp-deploy-short.md) en [MySQL resource provider](azure-stack-mysql-rp-deploy-short.md)uitproberen.

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png
