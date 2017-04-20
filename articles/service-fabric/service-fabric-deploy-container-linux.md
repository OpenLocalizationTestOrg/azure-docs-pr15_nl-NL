<properties
   pageTitle="Service-stof en implementeren Containers in Linux | Microsoft Azure"
   description="Service stof en het gebruik van Docker containers te implementeren van microservice-toepassingen. In dit artikel worden de mogelijkheden die Service stof voor containers biedt en hoe u een afbeelding van de container Docker implementeren in een cluster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Een container Docker implementeren naar Service stof

> [AZURE.SELECTOR]
- [Windows-Container implementeren](service-fabric-deploy-container.md)
- [Docker Container implementeren](service-fabric-deploy-container-linux.md)

In dit artikel begeleidt u bij het maken van beperkte services in Docker containers op Linux.

Service stof heeft verschillende container mogelijkheden die u helpen bij het bouwen van toepassingen die bestaan van microservices die beperkte zijn. Deze worden beperkte services genoemd.

De mogelijkheden opnemen;

- Container afbeelding implementatie en activering
- Resource-beheermodel
- Opslagplaats verificatie
- Container poort host poort toewijzen aan
- Discovery naar-andere containers en communicatie
- Mogelijkheid om te configureren en omgevingsvariabelen instellen


## <a name="packaging-a-docker-container-with-yeoman"></a>Een container docker verpakking met yeoman
Wanneer u een container op Linux het verpakken, kunt u een met een yeoman sjabloon of [het toepassingspakket handmatig maken](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container).

Een toepassing voor de Service stof kan een of meer containers, elk voorzien van een specifieke rol in het geven van de functionaliteit van de toepassing bevatten. De Service stof SDK voor Linux bevat een [Yeoman](http://yeoman.io/) genereren kunt u heel gemakkelijk uw toepassing maken en toevoegen van een afbeelding van de container. We gaan gebruiken Yeoman naar een nieuwe toepassing maken met een enkel Docker zogenaamd *SimpleContainerApp*. U kunt toevoegen om dat meer services later door het bewerken van de gegenereerde bestandenlijst bestanden.

## <a name="create-the-application"></a>De toepassing maken

1. Typ in een terminal **yo azuresfguest**.

2. Kies de **Container** voor het kader.

3. Naam van uw toepassing bijvoorbeeld SimpleContainerApp

4. Geef de URL voor de container afbeelding vanuit een cessies‑retrocessies DockerHub. Hiermee gaat het formulier [cessies‑retrocessies] / [naam afbeelding]

![Service stof Yeoman genereren voor containers][sf-yeoman]

## <a name="deploy-the-application"></a>De toepassing implementeren

Zodra de toepassing is gemaakt, kunt u deze implementeert bij het lokale cluster met behulp van de Azure CLI.

1. Verbinding maken met de lokale Service stof cluster.

    ```bash
    azure servicefabric cluster connect
    ```

2. Gebruik het script voor installatie die beschikbaar zijn in de sjabloon voor het kopiëren van de toepassingspakket naar van het cluster afbeelding store, het toepassingstype registreren en maken van een exemplaar van de toepassing.

    ```bash
    ./install.sh
    ```

3. Open een browser en Ga naar Service stof Explorer bij http://localhost:19080/Explorer (localhost vervangen met de persoonlijke IP van VM als Vagrant op Mac OS X).

4. Vouw het knooppunt toepassingen en houd er rekening mee dat er nu een vermelding voor uw toepassingstype en een andere voor het eerste exemplaar van dit type is.

5. Het verwijderingsscript die beschikbaar zijn in de sjabloon gebruiken om te verwijderen exemplaar van de toepassing en het toepassingstype unregister.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Service stof en containers](service-fabric-containers-overview.md)
- [Interactief werken met Service stof clusters met de CLI Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png

