<properties
   pageTitle="Een Container-Azure-Service cluster met Datadog controleren | Microsoft Azure"
   description="Een Container-Azure-Service cluster met Datadog bewaken. De domeincontroller/OS web UI gebruiken om te implementeren van de agenten Datadog aan uw cluster."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, domeincontroller/OS, Docker Swarm, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Monitor met een cluster Azure-Service voor Container met Datadog

In dit artikel wordt we Datadog gebruikersagenten dashboard implementeren naar alle agent knooppunten in uw cluster Azure Container-Service. Een account met Datadog moet u voor deze configuratie. 

## <a name="prerequisites"></a>Vereisten voor 

[Deploy](container-service-deployment.md) en [verbinding maken met](container-service-connect.md) een cluster geconfigureerd door Azure Container-Service. Kennismaken met de [Marathon UI](container-service-mesos-marathon-ui.md). Ga naar [http://datadoghq.com](http://datadoghq.com) voor het instellen van een Datadog-account. 

## <a name="datadog"></a>Datadog 

Datadog is een controle service die worden verzameld controlegegevens uit uw containers binnen uw cluster Azure Container-Service. Datadog heeft een Docker integratie Dashboard waar u specifieke aan de doelstellingen binnen uw containers kunt zien. Aan de doelstellingen uit uw containers verzameld worden door CPU-, geheugen-, netwerk- en i/o-ingedeeld. Datadog gesplitst aan de doelstellingen in containers en afbeeldingen. Een voorbeeld van hoe de gebruikersinterface voor CPU-gebruik eruitziet is hieronder.

![Datadog UI](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Een Datadog-implementatie met Marathon configureren

Deze stappen wordt uitgelegd hoe u configureren en gebruiken van uw cluster met Marathon Datadog-toepassingen. 

Toegang tot uw UI domeincontroller/OS via [http://localhost:80 /](http://localhost:80/). Eenmaal in de gebruikersinterface van de domeincontroller/OS Navigeer naar de "universum" die zich in de linkerbenedenhoek waarmee en zoek vervolgens naar "Datadog" en klikt u op 'Geïnstalleerd'.

![Datadog pakket binnen de domeincontroller/OS-universum](./media/container-service-monitoring/datadog1.png)

Nu om de configuratie te voltooien, moet u een Datadog-account of een gratis proefabonnement-account. Nadat u bent aangemeld bij het uiterlijk van de website Datadog naar links en Ga naar integraties -> vervolgens API's. 

![Datadog-API-sleutel](./media/container-service-monitoring/datadog2.png)

Voer vervolgens uw API-sleutel in de configuratie Datadog binnen de domeincontroller/OS-universum. 

![Configuratie van Datadog in de domeincontroller/OS-universum](./media/container-service-monitoring/datadog3.png) 

In de bovenstaande configuratie exemplaren die zijn ingesteld op 10000000 dat wanneer een nieuw knooppunt wordt toegevoegd aan het cluster Datadog wordt automatisch een agent implementeren naar dat knooppunt. Dit is een tijdelijke oplossing. Nadat u het pakket moet u Ga terug naar de website Datadog en vinden 'Dashboards.' hebt geïnstalleerd Hier ziet u aangepaste en integratie Dashboards. Het Dashboard van de integratie Docker heeft alle de container metrische gegevens die u nodig hebt voor uw cluster cmdlets voor controle. 
