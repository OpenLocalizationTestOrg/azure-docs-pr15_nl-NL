<properties
    pageTitle="Wat is er gebeurd met mijn ASP.NET-5-project (Visual Studio verbonden services) | Microsoft Azure-opslag"
    description="Wordt beschreven wat gebeurt er als de verbinding met een account Azure opslag in een Visual Studio ASP.NET-5-project Visual Studio met services verbonden"
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

# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Wat is er gebeurd met mijn ASP.NET-5-project (Visual Studio Azure Storage verbonden services)?

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

Ook het pakket NuGet **Microsoft.Framework.Configuration.Json** is toegevoegd.

## <a name="connection-string-for-azure-storage-added"></a>Verbindingsreeks voor de opslag van Azure toegevoegd
Een element is in het bestand config.json van uw project gemaakt met de verbindingsreeks van de geselecteerde opslag-account en de sleutel.

Zie [ASP.NET-5](http://www.asp.net/vnext)voor meer informatie.
