<properties
   pageTitle="Installeren van de domeincontroller/OS CLI | Microsoft Azure"
   description="Installeer de domeincontroller/OS CLI."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Micro-services, domeincontroller/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] Dit is voor het werken met ACS domeincontroller/OS gebaseerde clusters. Er is geen nodig om deze stap herhalen voor ACS Swarm gebaseerde clusters.

Eerst [verbinding maken met uw cluster domeincontroller/OS gebaseerde ACS](../articles/container-service/container-service-connect.md). Als u dit hebt gedaan, kunt u de domeincontroller/OS CLI installeren op uw computer met de volgende opdrachten:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Als u een oude versie van Python gebruikt, ziet u mogelijk enkele "InsecurePlatformWarnings". U kunt deze gewoon negeren.

Om te beginnen geworden uw shell, voert u het:

```bash
source ~/.bashrc
```

Deze stap meer niet nodig wanneer u nieuwe houders start.

Nu kunt u controleren of de CLI is geïnstalleerd:

```bash
dcos --help
```