<properties 
    pageTitle="Computer instellen voor de ontwikkeling van de Media-Services met .NET" 
    description="Meer informatie over de vereisten voor Media-Services met behulp van de Media Services SDK voor .NET. Ook informatie over het maken van een Visual Studio-app." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>Media Services ontwikkeling met .NET

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

In dit onderwerp wordt beschreven hoe om te beginnen met het ontwikkelen van Media Services-toepassingen met .NET.

De bibliotheek **Azure Media Services.NET SDK** kunt u voor het programmeren in .NET met Media-Services. Als u deze nog eenvoudiger ontwikkelen met .NET, krijgt u de bibliotheek **Azure Media Services .NET SDK extensies** . Deze bibliotheek bevat een set extensie methoden en Help-functies die uw .NET-code wordt vereenvoudigen. Beide bibliotheken zijn beschikbaar via **NuGet** en **GitHub**.


##<a name="prerequisites"></a>Vereisten voor

-   Een Media Services-account in een nieuw of bestaand Azure abonnement. Zie het onderwerp van [het maken van een Media Services-Account](media-services-portal-create-account.md).
-   Besturingssystemen: Windows 10, Windows 7, Windows 2008 R2 of Windows 8.
-   .NET framework 4.5.
-    Visual Studio-2015, Visual Studio 2013, Visual Studio 2012 of Visual Studio 2010 SP1 (Professional, Premium, Ultimate of Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Maken en configureren van een project Visual Studio

In dit gedeelte ziet u hoe u een project maken in Visual Studio en instellen voor de ontwikkeling van de Media Services.  In dit geval het project is een C# Windows console-toepassing, maar de dezelfde instellingsstappen Hier wordt getoond toepassen op andere typen projecten die u voor Media Services-toepassingen (bijvoorbeeld een toepassing voor de Windows-formulieren of een ASP.NET-webtoepassing maken kunt).

Hier vindt u het gebruik van **NuGet** Media Services .NET SDK en andere afhankelijke bibliotheken toevoegen.

U kunt ook de meest recente Media Services .NET SDK bits ophalen uit GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) en [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), maak de oplossing en de verwijzingen aan het project client toevoegen. Houd er rekening mee dat alle benodigde afhankelijkheden gedownload en automatisch uitgepakt.

1. Maak een nieuwe C#-Console-toepassing in Visual Studio 2010 SP1 of hoger van de VS. Voer de **naam**, **locatie**en **naam van de oplossing**en klik vervolgens op OK.

2. Maak de oplossing.

2. Gebruik **NuGet** installeert en **Azure Media Services .NET SDK extensies**toevoegen. In dit pakket installeert, ook **Media Services.NET SDK** is geïnstalleerd en alle andere vereiste afhankelijkheden toegevoegd.

    Zorgt ervoor dat u de nieuwste versie van NuGet is geïnstalleerd. Zie voor meer informatie en de installatie-instructies, [NuGet](http://nuget.codeplex.com/).

2. In Solution Explorer met de rechtermuisknop op de naam van het project en kies NuGet beheren-pakketten...

    Het dialoogvenster NuGet-pakketten beheren wordt weergegeven.

3. Klik in de galerie Online Azure MediaServices extensies zoekt, kies .NET SDK uitbreidingen voor Azure Media Services en klik vervolgens op de knop installeren.

    Het project is gewijzigd en verwijzingen naar de Media Services .NET SDK-extensies, .NET SDK van Media Services en andere afhankelijke verzamelingen worden toegevoegd.

4. Als u een duidelijkere ontwikkelomgeving verhogen, overweeg om in te NuGet pakket herstellen. Zie voor meer informatie [NuGet pakket herstellen '](http://docs.nuget.org/consume/package-restore).

3. Een verwijzing naar **System.Configuration** constructie toevoegen. Deze constructie bevat de System.Configuration. **ConfigurationManager** klasse die wordt gebruikt voor toegang tot configuratiebestanden (bijvoorbeeld App.config).

    Als u wilt toevoegen met het dialoogvenster beheren verwijzingen verwijzingen, met de rechtermuisknop op de naam van het project in de Solution Explorer. Selecteer vervolgens toevoegen en verwijzingen.

    Het dialoogvenster Verwijzingen beheren wordt weergegeven.

4. Onder .NET framework assemblies, zoeken, selecteer de vergadering System.Configuration en drukt u op OK.
5. Open het bestand App.config (het bestand toevoegen aan uw project als deze niet al dan niet standaard is toegevoegd) en een sectie *appSettings* toevoegen aan het bestand.     
Stel de waarden voor uw Azure Media Services naam en account accountsleutel, zoals wordt weergegeven in het volgende voorbeeld.

    U vindt de naam en de toets waarden, gaat u naar de Azure-portal en selecteer uw account. Het venster instellingen wordt weergegeven aan de rechterkant. Selecteer in het venster instellingen van toetsen. De waarde te klikken op het pictogram naast elk tekstvak worden gekopieerd naar het systeemklembord.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. De bestaande **gebruiken** beweringen aan het begin van het bestand Program.cs overschrijven met de volgende programmacode.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

U bent nu klaar om te beginnen met het ontwikkelen van een Media Services-toepassing.    


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
