<properties
   pageTitle="Voor foutopsporing in apps in een lokale Docker container | Microsoft Azure"
   description="Informatie over het aanpassen van een app die wordt uitgevoerd op een lokale Docker container, het vernieuwen van de container via bewerken en vernieuwen en foutopsporing onderbrekingspunten instellen"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Voor foutopsporing in apps in een lokale Docker container

## <a name="overview"></a>Overzicht
De Visual Studio Tools for Docker biedt een consistente manier ontwikkelen een en uw toepassing lokaal in een container Linux Docker valideren.
U hoeft niet te opnieuw starten van de container telkens wanneer u een code wijzigen.
In dit artikel wordt het gebruik van de functie "Bewerken en vernieuwen" een ASP.NET Core Web-app starten in een lokale Docker container, breng de benodigde wijzigingen en vernieuw de browser om deze wijzigingen zien illustreren.
Dit wordt ook uitgelegd hoe u onderbrekingspunten voor foutopsporing instellen.

> [AZURE.NOTE] Ondersteuning voor Windows Container wordt afkomstig in toekomstige versie

## <a name="prerequisites"></a>Vereisten voor
De volgende hulpprogramma's moeten worden geïnstalleerd.

- [Visual Studio 2015 Update 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Update voor [Visual Studio 2015 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Als u wilt uitvoeren Docker containers lokaal, moet u een lokale docker-client.
U kunt de uitgebrachte [Docker werkset](https://www.docker.com/products/overview#/docker_toolbox) waarvoor Hyper-V worden uitgeschakeld of kunt u [Docker voor Windows Beta](https://beta.docker.com) die gebruikmaakt van Hyper-V en moet worden Windows 10.

Als Docker werkset gebruikt, moet u [de Docker-client configureren](./vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. een WebApp maken

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Docker ondersteuning toevoegen

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3. het bewerken van uw code en vernieuwen

Als u wilt snel sequentieel wijzigingen, kunt u uw toepassing binnen een container start en gaat u verder met het aanbrengen van wijzigingen in deze weergeeft, zoals u zou met IIS Express doen.

1. De configuratie van de oplossing ingesteld op `Debug` en drukt u op ** &lt;CTRL + F5 >** voor het maken van uw afbeelding docker en deze lokaal uitvoeren.

    Zodra de container afbeelding is gemaakt en wordt uitgevoerd in een container Docker, wordt de Web-app in de browser in Visual Studio gestart.
    Als u de browser Microsoft Edge gebruikt of anderszins fouten, Zie de sectie [Probleemoplossing](vs-azure-tools-docker-troubleshooting-docker-errors.md) .

1. Ga naar de pagina over, dat wil zeggen waar gaan we onze wijzigingen aanbrengen.

1. Ga terug naar Visual Studio en open `Views\Home\About.cshtml`.

1. De volgende HTML-inhoud toevoegen aan het einde van het bestand en de wijzigingen op te slaan.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Het venster output weergeven wanneer de build .NET is voltooid en u deze regels zien, gaat u terug naar uw browser en vernieuw de pagina over.

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  Uw wijzigingen zijn toegepast!

## <a name="4-debug-with-breakpoints"></a>4. opsporen onderbrekingspunten

Vaak moet wijzigingen verder inspectie gebruikmaken van de foutopsporing functies van Visual Studio.

1.  Ga terug naar Visual Studio en openen`Controllers\HomeController.cs`

1.  De inhoud van de methode About() vervangen door het volgende:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Een onderbrekingspunt instellen aan de linkerkant van de `string message`... lijn.

1.  Druk op ** &lt;F5 >** foutopsporing te starten.

1.  Navigeer naar de pagina over naar uw onderbrekingspunt.

1.  Ga naar Visual Studio om het onderbrekingspunt bekijken en controleren van de waarde van het bericht.

    ![][2]

##<a name="summary"></a>Overzicht

Met [Visual Studio 2015 Tools voor Docker](https://aka.ms/DockerToolsForVS), kunt u de productiviteit van lokaal, werken met de realisme productie van het ontwikkelen van binnen een container Docker verkrijgen.

## <a name="troubleshooting"></a>Problemen oplossen

[Probleemoplossing Visual Studio Docker ontwikkeling](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Meer informatie over Docker met Visual Studio, Windows en Azure

- [Docker Tools voor Visual Studio](http://aka.ms/dockertoolsforvs) - ontwikkelen van uw .NET Core-code in een container
- [Docker Tools voor Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - bouwen en implementeren van docker containers
- [Docker Tools voor Visual Studio-Code](http://aka.ms/dockertoolsforvscode) - taalservices voor het bewerken van bestanden docker, met meer e2e-scenario's die afkomstig zijn
- [Informatie over de Container van Windows](http://aka.ms/containers)- Windows Server en Nano servergegevens
- [Azure Container Service](https://azure.microsoft.com/services/container-service/) - [Azure Container Service inhoud](http://aka.ms/AzureContainerService)
-    Zie [werken met Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) uit de [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinding maken met [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/)voor meer voorbeelden van het werken met Docker. Zie voor meer QuickStart uit de demo HealthClinic.biz [Azure ontwikkelaars hulpmiddelen voor QuickStart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Verschillende Docker hulpmiddelen

[Enkele technieken uitstekende docker (Steven Lasker van blog)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Goede artikelen

[Inleiding tot Microservices van NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Presentaties

- [Steven Lasker: Tegenover Live Las Vegas 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Inleiding tot ASP.NET Core @ 2016 - waar u bij Demo maken](https://channel9.msdn.com/Events/Build/2016/B810)
- [.NET-apps in containers, kanaal 9 ontwikkelen](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
