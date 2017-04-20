<properties
    pageTitle="WebJobs in Azure App-Service"
    description="Informatie over het maken van WebJobs als achtergrond tests uitvoeren, werken met services zoals opslagcapaciteit en de Service Bus en geplande taken maken."
    services="app-service"
    documentationCenter=""
    authors="christopheranderson"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="chrande"/>

# <a name="using-webjobs-in-azure-app-service"></a>Gebruik van WebJobs in Azure App-Service

In dit artikel bevat koppelingen naar de documentatie over het gebruik van Azure WebJobs en de SDK van Azure WebJobs. Azure WebJobs bieden een eenvoudige manier om het uitvoeren van scripts of programma's als achtergrondprocessen op [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). U kunt uploaden en uitvoeren van een uitvoerbaar bestand zoals als cmd, bat, exe (.NET), ps1, v, php, kopiëren, js en oppervlak. Deze programma's worden uitgevoerd als WebJobs volgens een schema (cron) of doorlopend.

De WebJobs SDK gemakkelijker Azure opslag gebruiken. De WebJobs SDK vast binding en trigger systeem die met Microsoft Azure opslag BLOB's, wachtrijen en tabellen, evenals Service Bus wachtrijen werkt.

Maken, implementeert en beheren van WebJobs is naadloze met geïntegreerde tooling in Visual Studio. U kunt WebJobs maken op basis van sjablonen, publiceren en beheren (klaar/stoppen/monitor/foutopsporing) ze.

Het dashboard WebJobs in de portal van Azure biedt krachtige beheermogelijkheden waarmee u volledige controle over de uitvoering van WebJobs, waaronder de mogelijkheid om aan te roepen afzonderlijke functies binnen WebJobs. Het dashboard worden ook functie runtimes en logboekregistratie uitvoer weergegeven.

[AZURE.INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]
