<properties
   pageTitle="Een Container-Azure-Service cluster met Sysdig controleren | Microsoft Azure"
   description="Een Container-Azure-Service cluster met Sysdig bewaken."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, domeincontroller/OS Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Monitor met een cluster Azure-Service voor Container met Sysdig

In dit artikel wordt we Sysdig agenten dashboard implementeren naar alle agent knooppunten in uw cluster Azure Container-Service. U hebt een account met Sysdig nodig voor deze configuratie. 

## <a name="prerequisites"></a>Vereisten voor 

[Deploy](container-service-deployment.md) en [verbinding maken met](container-service-connect.md) een cluster geconfigureerd door Azure Container-Service. Kennismaken met de [Marathon UI](container-service-mesos-marathon-ui.md). Ga naar [http://app.sysdigcloud.com](http://app.sysdigcloud.com) voor het instellen van een Sysdig cloud-account. 

## <a name="sysdig"></a>Sysdig

Sysdig is een controle service waarmee u uw containers binnen uw cluster controleren. Sysdig bekend is bij het oplossen van problemen met maar ook de doelstellingen van uw eenvoudige controle heeft voor CPU, netwerken, geheugen en i/o. Sysdig kunt u gemakkelijk om te zien welke containers werkt het hardest of in principe optimaal geheugen en CPU gebruiken. Deze weergave is in de sectie 'Overzicht', die geschreven beta is. 

![Sysdig UI](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Een Sysdig-implementatie met Marathon configureren

Deze stappen wordt uitgelegd hoe u configureren en gebruiken van uw cluster met Marathon Sysdig-toepassingen. 

Toegang tot uw UI domeincontroller/OS via [http://localhost:80 /](http://localhost:80/) eenmaal in de gebruikersinterface van de domeincontroller/OS Ga naar het 'universum', dat wil op de linkerbenedenhoek waarmee zeggen en zoek vervolgens naar "Sysdig."

![Sysdig in domeincontroller/OS universum](./media/container-service-monitoring-sysdig/sysdig1.png)

U voltooit de configuratie moet u nu een Sysdig cloud-account of een gratis proefabonnement-account. Nadat u bent aangemeld bij de website van de cloud Sysdig, klikt u op uw gebruikersnaam en klik op de pagina ziet u uw "toegangstoets". 

![Sysdig API-sleutel](./media/container-service-monitoring-sysdig/sysdig2.png) 

Voer vervolgens uw Access-sleutel in de configuratie Sysdig binnen de domeincontroller/OS-universum. 

![Configuratie van Sysdig in de domeincontroller/OS-universum](./media/container-service-monitoring-sysdig/sysdig3.png)

Nu instellen de exemplaren wilt 10000000 dus wanneer een nieuw knooppunt wordt toegevoegd aan het cluster Sysdig wordt automatisch een agent dashboard implementeren naar dat nieuwe knooppunt. Dit is een tijdelijke oplossing om ervoor te zorgen dat Sysdig wordt implementeren naar alle nieuwe agenten binnen het cluster. 

![Configuratie van Sysdig in de domeincontroller/OS universum-exemplaren](./media/container-service-monitoring-sysdig/sysdig4.png)

Nadat u hebt geïnstalleerd het pakket Ga terug naar de Sysdig UI en u kunt wel de doelstellingen van de verschillende gebruik voor de containers in uw cluster verkennen. 