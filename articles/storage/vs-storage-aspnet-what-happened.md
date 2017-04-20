<properties
    pageTitle="Wat is er gebeurd met mijn ASP.NET-project? | Microsoft Azure | Visual Studio verbonden services"
    description="Wordt beschreven wat gebeurt er als Azure opslag toevoegt aan een ASP.NET-project Visual Studio met services verbonden"
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

# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>Wat is er gebeurd met mijn ASP.NET-project (Visual Studio Azure Storage verbonden service)?

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

##<a name="connection-string-for-azure-storage-added"></a>Verbindingsreeks voor de opslag van Azure toegevoegd
In het bestand web.config van uw project is een element gemaakt met de verbindingsreeks van de geselecteerde opslag-account en de sleutel.

Zie [ASP.NET](http://www.asp.net)voor meer informatie.
