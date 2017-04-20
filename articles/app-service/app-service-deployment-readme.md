<properties
    pageTitle="Implementeren van toepassingen op Azure App-Service"
    description="Leer hoe u Deploy toepassingen App Service werk"
    keywords="App-service, azure app service, wordt geïmplementeerd, implementatie"
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="dariagrigoriu"/>

# <a name="azure-app-service-deployment-overview"></a>Overzicht van de implementatie van Azure App-Service

Azure App-Service biedt een uitgebreide en geïntegreerde functie instellen voor het maken van krachtige en flexibele implementatie werkstromen. Opties die bevatten continue integratie of besturingselement voor lokale gegevensbronnen publiceren, WebDeploy en FTP kunt gebruikmaken van App-implementatie. De aanbevolen methode voor productie app-implementatie is implementatie slot uitwisselen. Implementatie sleuven vertegenwoordigen tijdelijke en integratie omgevingen die is gekoppeld aan productie apps. Implementatie sleuven kunnen worden geconfigureerd en gericht met web-verkeer is toegestaan voor validatie en verkeer kan worden omgewisseld op aanvraag voor implementatie op productie geen omlaag tijd en wordt automatisch in opgewarmd. De stappen van een werkstroom implementatie kunnen eenvoudig worden geautomatiseerd via release management producten, zoals Visual Studio los Management. Dit is handig voor afhankelijk van andere bronnen oplossing (bijvoorbeeld gegevens opslaan), terugkeerpatroon en replicatie over meerdere van implementatie. 

[AZURE.INCLUDE [app-service-blueprint-deployment](../../includes/app-service-blueprint-deployment.md)]
