<properties
   pageTitle="Een Host Docker configureren met VirtualBox | Microsoft Azure"
   description="Stapsgewijze instructies voor het configureren van een standaardexemplaar Docker Docker Machine en VirtualBox gebruiken"
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
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Een Host Docker met VirtualBox configureren

## <a name="overview"></a>Overzicht
In dit artikel begeleidt u bij het configureren van een standaardexemplaar Docker Docker Machine en VirtualBox gebruiken. Als u de [bèta Docker voor Windows](http://beta.docker.com/)gebruikt, is deze configuratie niet nodig.

## <a name="prerequisites"></a>Vereisten voor
De volgende hulpprogramma's moeten worden geïnstalleerd.

- [Docker werkset](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>De client Docker configureren met Windows PowerShell

Als u een client Docker configureren, gewoon open Windows PowerShell en de volgende stappen uitvoeren:

1. Maak een standaardexemplaar docker-host.

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Controleer of dat de standaardexemplaar is geconfigureerd en wordt uitgevoerd. (Ziet u een exemplaar met de naam 'default' uitgevoerd.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![docker-machine ls uitvoer][0]
 
1. Standaard als de huidige host instellen en configureren van uw shell.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Het actieve Docker containers weergeven. De lijst moet niet leeg zijn.

    ```PowerShell
    docker ps
    ```

    ![docker ps uitvoer][1]
 
> [AZURE.NOTE]Telkens wanneer die u uw computer ontwikkeling opstart, moet u om de host van uw lokale docker opnieuw te starten.
> Klik hiertoe Geef de volgende opdracht opdrachtprompt: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
