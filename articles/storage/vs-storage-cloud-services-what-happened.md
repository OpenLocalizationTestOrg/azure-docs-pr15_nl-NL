<properties
    pageTitle="Wat is er gebeurd met mijn project cloud-service? | Microsoft Azure | Visual Studio verbonden services"
    description="Wordt beschreven wat gebeurt er in een cloud services-project nadat de verbinding met een Azure opslag-account gebruik van Visual Studio services verbonden"
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

# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Wat is er gebeurd met mijn cloud services-project (Visual Studio Azure Storage verbonden service)?

## <a name="references-added"></a>Verwijzingen toegevoegd

Het pakket Azure opslag NuGet is toegevoegd aan uw project Visual Studio.  
Dit pakket wordt de volgende .NET-verwijzingen toegevoegd:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Verbindingsreeks voor de opslag van Azure toegevoegd
Elementen zijn gemaakt met de verbindingsreeks van de geselecteerde opslag-account en de sleutel. Wijzigingen zijn aangebracht in de volgende bestanden:

- **ServiceDefinition.csdef**
- **ServiceConfiguration.Cloud.cscfg**
- **ServiceConfiguration.Local.cscfg**
