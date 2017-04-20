<properties
   pageTitle="Toepassing of bepaalde gebruiker Marathon service | Microsoft Azure"
   description="Maken van een toepassing of een bepaalde gebruiker Marathon service"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Marathon, Micro-services, domeincontroller/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>Maken van een toepassing of een bepaalde gebruiker Marathon service

Azure Container-Service biedt een reeks basispagina servers waarop we Apache Mesos en Marathon vooraf configureren. Deze kunnen worden gebruikt om de goedkeuringen van uw toepassingen op het cluster, maar het is raadzaam niet op de basispagina servers hiervoor gebruiken. Bijvoorbeeld de configuratie van Marathon SE moet aanmelden bij de basispagina servers zelf en aanbrengen van wijzigingen--dit aanmoedigt unieke basispagina servers die iets afwijkt van de standaard en moeten worden verzorgd en onafhankelijk beheerd. De configuratie is vereist door één team mogelijk ook niet de optimale configuratie voor een ander team.

In dit artikel wordt we wordt uitgelegd hoe u een toepassing of een bepaalde gebruiker Marathon service toevoegen.

Omdat deze service deel van een gebruiker of een team uitmaakt wordt, worden deze rustig configureert u deze op geen enkele manier die ze nodig hebben. Ook Azure Container-Service wordt ervoor te zorgen dat de service blijft om uit te voeren. Als de service is mislukt, wordt Azure Container-Service opnieuw. De meeste gevallen won't zelfs ziet u had downtime.

## <a name="prerequisites"></a>Vereisten voor

Met orchestrator type domeincontroller/OS [Deploy een exemplaar van Azure Container-Service](container-service-deployment.md) en [Zorg ervoor dat de klant verbinding met uw cluster maken kunt](container-service-connect.md). Bovendien kan de volgende stappen uit.

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Maken van een toepassing of een bepaalde gebruiker Marathon service

Beginnen door te maken van een JSON-configuratiebestand waarin de naam van de toepassingsservice die u wilt maken. Hier we gebruiken `marathon-alice` als de naam framework. Sla het bestand als ongeveer zo `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Gebruik vervolgens de domeincontroller/OS CLI het exemplaar Marathon met de opties die zijn ingesteld in het configuratiebestand installeren:

```bash
dcos package install --options=marathon-alice.json marathon
```

U ziet nu uw `marathon-alice` service uitgevoerd op het tabblad Services van de gebruikersinterface van de domeincontroller/OS. De gebruikersinterface worden `http://<hostname>/service/marathon-alice/` desgewenst kunt u rechtstreeks toegang toe.

## <a name="set-the-dcos-cli-to-access-the-service"></a>Instellen van de domeincontroller/OS CLI voor toegang tot de service

U kunt uw CLI domeincontroller/OS voor toegang tot deze nieuwe service door in te stellen (optioneel) configureren de `marathon.url` eigenschap zodat deze verwijzen naar de `marathon-alice` exemplaar als volgt:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

U kunt controleren of welke exemplaar van Marathon waarmee uw CLI werkt ten opzichte van de `dcos config show` opdracht. U kunt herstellen voor het gebruik van de basispagina Marathon-service met de opdracht `dcos config unset marathon.url`.
