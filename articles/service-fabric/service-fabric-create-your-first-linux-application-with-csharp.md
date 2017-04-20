<properties
   pageTitle="Uw eerste stof Service-toepassing maken op Linux met C# | Microsoft Azure"
   description="Maak en implementeer een Service stof-toepassing met C#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Uw eerste stof van Azure-Service-toepassing maken

> [AZURE.SELECTOR]
- [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Service stof biedt SDK's voor het samenstellen van services op Linux in zowel .NET Core en Java. In deze zelfstudie kijken we naar het maken van een toepassing voor Linux en bouwen van een service met C# (.NET Core).

## <a name="prerequisites"></a>Vereisten voor

Controleer voordat u begint of dat u [uw ontwikkelomgeving Linux instellen hebt](service-fabric-get-started-linux.md). Als u Mac OS X gebruikt, kunt u [een Linux een en-klare mailomgeving in een virtuele machine met Vagrant instellen](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>De toepassing maken

Een toepassing voor de Service stof kan een of meer services, elk voorzien van een specifieke rol in het geven van de functionaliteit van de toepassing bevatten. De Service stof SDK voor Linux bevat een [Yeoman](http://yeoman.io/) genereren maakt het gemakkelijk om uw eerste service te maken en meer later toe te voegen. We gaan gebruiken Yeoman een toepassing maken met een enkele service.

1. Typ de volgende opdracht uit om te beginnen met het samenstellen van de steiger in een terminal:`yo azuresfcsharp`

2. Naam toewijzen aan uw toepassing.

3. Kies het type van uw eerste service en noem deze. Voor de toepassing van deze zelfstudie kiest u een betrouwbare acteur-Service.

  ![Service stof Yeoman genereren voor C#][sf-yeoman]

>[AZURE.NOTE] Zie voor meer informatie over de opties [Service stof programming model overzicht](service-fabric-choose-framework.md).

## <a name="build-the-application"></a>De toepassing bouwen

De Service stof Yeoman sjablonen bevatten een script maken waarmee u kunt maken van de app vanuit de terminal (na het navigeren naar de toepassingsmap).

  ```bash
 cd myapp 
 ./build.sh 
  ```

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

## <a name="start-the-test-client-and-perform-a-failover"></a>Start de testclient en een failover uitvoeren

Acteur projecten Doe op hun eigen niet. Zij nodig hebben een andere service of client ze om berichten te verzenden. De sjabloon acteur bevat een eenvoudige test-script die u gebruiken kunt om te communiceren met de acteur-service.

1. Het gebruik van het hulpprogramma controle om te zien van de uitvoer van de service acteur script uitvoeren.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Zoek de primaire replica voor de service acteur hostingprovider knooppunt in Service stof Verkenner. In de onderstaande schermafbeelding is het knooppunt 3.

    ![De primaire replica zoeken in Service stof Explorer][sfx-primary]

3. Het knooppunt dat u in de vorige stap hebt gevonden, klikt u op **deactiveren (opnieuw starten)** Selecteer in het menu Acties. Deze actie start opnieuw op een van de vijf knooppunten in uw lokale cluster afdwingen van een failover met een secundaire replica uitgevoerd op een ander knooppunt. Als u deze actie uitvoeren, Let op de uitvoer van de testclient en de notitie die het item nog steeds verhoogd ondanks de overname.


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over betrouwbare betrokkenen](service-fabric-reliable-actors-introduction.md)
- [Interactief werken met Service stof clusters met de CLI Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
