<properties
   pageTitle="Een container ASP.NET Core Linux Docker implementeren naar een externe Docker host | Microsoft Azure"
   description="Informatie over het gebruik van Visual Studio Tools for Docker naar een ASP.NET-Core-web-app implementeren naar een Docker container uitgevoerd op een Azure Docker Host Linux VM"   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Een container ASP.NET implementeren naar een externe Docker-host

## <a name="overview"></a>Overzicht
Docker is een lightweight container engine, vergelijkbare in enkele manieren om een virtuele machine, waarmee u hosttoepassingen en services kunt te.
Deze zelfstudie begeleidt u bij het gebruik van de extensie [Visual Studio 2015 Tools voor Docker](http://aka.ms/DockerToolsForVS) naar een ASP.NET-Core-app implementeren naar een host Docker op Azure via PowerShell.

## <a name="prerequisites"></a>Vereisten voor
Het volgende nodig om te voltooien van deze zelfstudie:

- Maak een Azure Docker Host VM zoals beschreven in [het gebruik van docker-machine met Azure](./virtual-machines/virtual-machines-linux-docker-machine.md)
- Update voor [Visual Studio 2015 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
- [Visual Studio 2015 Tools voor Docker - Preview](http://aka.ms/DockerToolsForVS) installeren

## <a name="1-create-an-aspnet-core-web-app"></a>1. een ASP.NET-Core-web-app maken
De volgende stappen begeleidt u bij het maken van een eenvoudige ASP.NET Core-app die wordt gebruikt in deze zelfstudie.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Docker ondersteuning toevoegen

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Gebruik de DockerTask.ps1 PowerShell-Script 

1.  Open een PowerShell-prompt naar de hoofdmap van uw project. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Valideer de externe host wordt uitgevoerd. U ziet nu de staat = uitvoeren 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Als u de bètaversie Docker gebruikt, hier uw host niet weergegeven.

1.  Maakt de app met de - parameter opbouwen

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Als u de bètaversie Docker gebruikt, weglaten het - argument Machine
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  De app, worden uitgevoerd met de - parameter uitvoeren

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Als u de bètaversie Docker gebruikt, weglaten het - argument Machine
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Zodra docker is voltooid, worden er resultaten ongeveer als volgt uit:

    ![Uw app bekijken][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
