<properties
    pageTitle="Azure opslag gebruiken in Windows Store-apps | Microsoft Azure"
    description="Informatie over het maken van een Windows Store-app waarin de opslag van Azure Blob, wachtrij, tabel of bestand."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Het gebruik van Azure-opslag in Windows Store-apps

## <a name="overview"></a>Overzicht

Deze handleiding ziet hoe u aan de slag met het ontwikkelen van een Windows Store-app die gebruikmaakt van Azure-opslag.

## <a name="download-required-tools"></a>Vereiste hulpmiddelen downloaden

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) kunt u gemakkelijk te bouwen, foutopsporing, localize,-pakket en implementeren van de Windows Store-apps. Visual Studio 2012 of hoger is vereist.
- De [Client-bibliotheek van Azure-opslagruimte](https://www.nuget.org/packages/WindowsAzure.Storage) biedt een klassenbibliotheek Windows Runtime voor het werken met Azure Storage.
- [WCF Data Services hulpprogramma's voor Windows Store Apps](http://www.microsoft.com/download/details.aspx?id=30714) breidt de Service-verwijzing toevoegen met aan de clientzijde OData-ondersteuning voor Windows Store-apps in Visual Studio.

## <a name="develop-apps"></a>Ontwikkel apps

### <a name="getting-ready"></a>Op het punt staat

Maak een nieuw project Windows Store-app in Visual Studio 2012 of later:

![Store-apps-opslag-vs-project][store-apps-storage-vs-project]

Voeg nu een verwijzing naar de bibliotheek van Azure opslag Client door met de rechtermuisknop op **verwijzingen**, op **Reference toevoegen**te klikken en vervolgens Bladeren naar de opslag Client bibliotheek voor Windows Runtime die u hebt gedownload:

![Store-apps-opslag-Kies-bibliotheek][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Gebruik van de bibliotheek met de services Blob en wachtrij

Uw app is nu klaar voor het bellen van de Azure Blob en wachtrij-services. De volgende instructies voor het **gebruik van** toevoegen zodat Azure Storage typen rechtstreeks kunnen worden verwezen:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Vervolgens moet u een knop toevoegen aan uw pagina. De volgende code aan de gebeurtenis **klikt u op** toevoegen en wijzigen van uw gebeurtenis-handler methode via het [asynchrone trefwoord](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Deze code wordt ervan uitgegaan dat u twee tekenreeksvariabelen, *accountnaam* en *accountKey hebt*. De naam van uw opslag-account en de accountsleutel die is gekoppeld aan dat account staan.

Maken en uitvoeren van de toepassing. Op de knop wordt gecontroleerd of een container met de naam *container1* in uw account bestaat en maak deze als dat niet.

### <a name="using-the-library-with-the-table-service"></a>Gebruik van de bibliotheek met de tabel-service

Typen die worden gebruikt om te communiceren met de tabel Azure-service is afhankelijk van WCF Data Services voor de Windows Store-app-bibliotheek. Vervolgens een verwijzing naar de vereiste WCF-bibliotheken toevoegen met behulp van de beheerconsole van pakket:

![Store-apps-opslag-pakket-manager][store-apps-storage-package-manager]

Gebruik de volgende opdracht punt Package Manager naar de locatie op uw computer:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Deze opdracht worden automatisch toegevoegd aan alle vereiste verwijzingen aan uw project. Als u niet gebruiken de Package Manager-Console wilt, kunt u de WCF Data Services NuGet-map op uw lokale computer toevoegen aan de lijst met bronnen pakket en voegt u de verwijzing via de gebruikersinterface, zoals wordt beschreven in het [Beheren van NuGet pakketten via het dialoogvenster](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Wanneer u verwijst naar het WCF Data Services NuGet-pakket, moet u de code in gebeurtenis **Click** van de knop wijzigen:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Deze code controleert of een tabel genaamd *tabel1* in uw account bestaat, en deze vervolgens gemaakt als niet.

U kunt ook een verwijzing naar Microsoft.WindowsAzure.Storage.Table.dll, toevoegen die beschikbaar is in hetzelfde pakket die u hebt gedownload. Deze bibliotheek bevat extra functionaliteit, zoals weerspiegeling gebaseerde serialisatie en algemene query's. Houd er rekening mee dat deze bibliotheek JavaScript niet ondersteunt.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
