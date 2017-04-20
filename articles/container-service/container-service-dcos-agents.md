<properties
   pageTitle="Openbare en persoonlijke domeincontroller/OS Agent voor groepen ACS | Microsoft Azure"
   description="Hoe werken de agent van openbare en persoonlijke groepen met een cluster Azure Container-Service."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>Domeincontroller/OS Agent van toepassingen voor de Container Azure-Service

Domeincontroller/OS Azure Container-Service verdeelt agenten in openbaar of privé-toepassingen. Een implementatie kan worden gemaakt met een groep, dat dit gevolgen heeft toegankelijkheid tussen computers in de container-service. De machines kunnen worden blootgesteld aan internet (openbaar) of interne (persoonlijk) bewaard. In dit artikel biedt een kort overzicht van waarom er op een openbare en persoonlijke groep.

### <a name="private-agents"></a>Privé agenten

Privé-agent knooppunten uitvoeren via een netwerk met een niet-omgeleid. Dit netwerk is alleen toegankelijk vanaf de zone beheerder of via de openbare zone rand router. Standaard start domeincontroller/OS apps op privé-agent knooppunten. Raadpleeg de [domeincontroller/OS documentatie](https://dcos.io/docs/1.7/administration/securing-your-cluster/) voor meer informatie over de beveiliging van het netwerk.

### <a name="public-agents"></a>Openbare agenten

Openbare agent knooppunten uitvoeren domeincontroller/OS apps en -services via een netwerk met een openbaar. Raadpleeg de [domeincontroller/OS documentatie](https://dcos.io/docs/1.7/administration/securing-your-cluster/) voor meer informatie over de beveiliging van het netwerk.

## <a name="using-agent-pools"></a>Agent van toepassingen gebruiken

Standaard implementeert **Marathon** een nieuwe toepassing op de *privé* -agent knooppunten. U moet expliciet implementeren de toepassing naar de *openbare* knooppunt tijdens het maken van de toepassing. Selecteer het tabblad **optioneel** en voer **slave_public** voor de **Resource rollen geaccepteerd** -waarde. Dit proces wordt beschreven [hier](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) en in de documentatie [DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) .

## <a name="next-steps"></a>Volgende stappen

Lees meer informatie over het [beheren van uw containers domeincontroller/OS](container-service-mesos-marathon-ui.md).

Leer hoe u [de firewall open](container-service-enable-public-access.md) verstrekt door Azure toe te staan dat openbare toegang tot uw domeincontroller/OS container.