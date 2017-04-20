<properties
   pageTitle="Met Docker Client oplossen in Windows met behulp van Visual Studio | Microsoft Azure"
   description="Problemen met het oplossen van problemen optreden bij het gebruik van Visual Studio maken en implementeren van WebApps naar Docker in Windows met behulp van Visual Studio."
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
   ms.date="06/08/2016"
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Probleemoplossing Visual Studio Docker ontwikkeling

Tijdens het werken met Visual Studio Tools voor Docker Preview, kunt u sommige problemen vanwege de aard preview ondervinden.
Hier volgen enkele veelvoorkomende problemen en oplossingen.


## <a name="unable-to-validate-volume-mapping"></a>Kan niet worden gevalideerd volume-toewijzing
Volume-toewijzing is vereist voor het delen van de broncode en binaire bestanden van uw toepassing met de app-map in de container.  Specifieke volume toewijzingen zijn opgenomen in de docker-compose.dev.debug.yml en docker-compose.dev.release.yml-bestanden. Als u bestanden op uw hostcomputer worden gewijzigd, wordt in de containers deze wijzigingen in een soortgelijke mapstructuur doorgevoerd.

Schakel volume toewijzing door **instellingen...** openen vanuit het Docker voor Windows-pictogram voor het systeemvak van "moby" en selecteer vervolgens het tabblad **Gedeeld stations** .  Zorg ervoor dat de letter die host van uw project, evenals de letter waarin % USERPROFILE % zich bevindt worden gedeeld door ze controleren, en vervolgens te klikken op **toepassen**.

Als u wilt testen of het volume van toewijzing werkt, nadat de stations zijn gedeeld, opnieuw opbouwen en F5 uit binnen Visual Studio of probeer een van de opdracht van de volgende vragen:

*In een opdrachtprompt van Windows*

*[Opmerking: dit wordt ervan uitgegaan dat uw gebruikers-map bevindt zich op het station "C" en dat deze is gedeeld.  Werk zo nodig als u een ander station hebt gedeeld]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*In de container Linux*

```
/ # ls
```

Hier ziet u een vermelding uit de map gebruikers/openbare map.
Als er geen bestanden worden weergegeven en de map /c/Users/Public niet leeg is, wordt het volume van toewijzing is niet juist geconfigureerd. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Converteren naar de map wormhole om te zien van de inhoud van de `/c/Users/Public` directory:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Notitie:** *Tijdens het werken met Linux VMs, het bestandssysteem container is hoofdlettergevoelig.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Opbouwen: "PrepareForBuild" taak is onverwacht mislukt.

Microsoft.DotNet.Docker.CommandLine.ClientException: Is een fout opgetreden verbinding probeert te maken:

Controleer of dat de standaard docker host wordt uitgevoerd. Open een opdrachtprompt en uitvoeren:

```
docker info
```

Als dit geeft als resultaat een fout probeert te starten de **Docker voor Windows** -bureaublad-app.  Als de bureaublad-app wordt uitgevoerd vervolgens het pictogram **moby** in het systeemvak moet zichtbaar zijn. Klik met de rechtermuisknop op het pictogram in het systeemvak en open **Instellingen**.  Klik op het tabblad **opnieuw instellen** en klik vervolgens **opnieuw starten Docker..**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Versie 0.31 naar 0,40 bijwerken handmatig


1. Back-up maken van het project
1. Verwijder de volgende bestanden in het project:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Sluit de oplossing en verwijder de volgende regels uit het bestand .xproj:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Open de oplossing opnieuw
1. Verwijder de volgende regels uit het bestand Properties\launchSettings.json:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. De volgende bestanden die zijn gerelateerd aan Docker uit project.json in de publishOptions verwijderen:

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. De vorige versie verwijderen en installeer Docker extra 0,40 en vervolgens op **toevoegen -> Docker ondersteuning** opnieuw in het contextmenu voor uw ASP.Net Core Web of Console-toepassing. Hierdoor wordt de nieuwe vereiste Docker onderdelen terug naar uw project toegevoegd. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Dialoogvenster met een foutbericht wordt weergegeven wanneer u probeert om toe te **voegen-Docker ondersteuning >** of foutopsporing (F5) een ASP.NET-Core-toepassing in een container

We hebben af en toe gezien na de installatie ongedaan maken en extensies installeert, Visual Studio MEF (beheerde uitbreiden Framework) cache kunt beschadigd. Dit gebeurt wanneer deze kan leiden tot verschillende fout bij het toevoegen van Docker ondersteuning dialogs en/of wilt uitvoeren of foutopsporing (F5) uw ASP.NET Core-toepassing. Als een tijdelijke oplossing de volgende stappen uit als u wilt verwijderen en opnieuw wilt genereren de cache MEF worden uitgevoerd.

1. Sluit alle exemplaren van Visual Studio
1. %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\ openen
1. Verwijder de volgende mappen
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Open Visual Studio
1. Het scenario opnieuw proberen 
