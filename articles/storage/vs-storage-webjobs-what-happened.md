<properties
    pageTitle="Wat is er gebeurd met mijn WebJob-project (Visual Studio Azure Storage verbonden service)? | Microsoft Azure"
    description="Beschrijving van wat is er gebeurd in een project Azure WebJob nadat de verbinding met een opslag-account gebruik van Visual Studio services verbonden"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Wat is er gebeurd met mijn WebJob-project (Visual Studio Azure Storage verbonden service)?

## <a name="references-added"></a>Verwijzingen toegevoegd

Het pakket Azure opslag NuGet is toegevoegd aan of bijgewerkt in uw project Visual Studio.  
Dit pakket wordt de volgende .NET-verwijzingen toegevoegd:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Verbindingsreeks voor de opslag van Azure toegevoegd
In het bestand App.config van uw project, zijn de vermeldingen **AzureWebJobsStorage** en **AzureWebJobsDashboard** bijgewerkt met de verbindingsreeks van de geselecteerde opslag-account en van toetsen.

Zie [Azure WebJobs documentatiebronnen](http://go.microsoft.com/fwlink/?linkid=390226)voor meer informatie.
