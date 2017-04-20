<properties
    pageTitle="Meerdere virtuele machines maken | Microsoft Azure"
    description="Opties voor het maken van meerdere virtuele machines in Windows"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="guybo"/>

# <a name="create-multiple-azure-virtual-machines"></a>Meerdere Azure virtuele machines maken

Zijn er veel scenario's waar u moet maken van een groot aantal vergelijkbare virtuele machines (VMs). Enkele voorbeelden zijn high-performance computing (HPC), grootschalige gegevensanalyse scalable en vaak stateless middelste laag of backend servers (zoals webservers) en gedistribueerde databases.

In dit artikel wordt beschreven hoe de beschikbare opties voor het maken van meerdere VMs in Azure wordt aangegeven. Deze opties verder dan de eenvoudige zaken waar u een reeks VMs handmatig maken. Om u te veel VMs hebt gemaakt, worden de processen die u doorgaans niet schaal ook als u wilt meer dan een klein aantal VMs maken.

Er is een manier om u te veel dezelfde VMs maken gebruik van de Azure resourcemanager constructie van _resource lussen_.

## <a name="resource-loops"></a>Resource-lussen

Resource-lussen zijn syntactische afkortingen in resourcemanager Azure-sjablonen. Resource lussen kunnen maken van een reeks op vergelijkbare wijze geconfigureerde resources in een lus. Resource-lussen kunt u meerdere accounts opslag, netwerkinterfaces of virtuele machines maken. Raadpleeg [VMs maken in de beschikbaarheid wilt instellen met resource lussen](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/)voor meer informatie over resource lussen.

## <a name="challenges-of-scale"></a>Uitdagingen van schaal

Hoewel resource lussen gemakkelijker bouwen van een cloudinfrastructuur bij het op schaal en kunnen produceren beknoptere sjablonen, blijven bepaalde uitdagingen. Bijvoorbeeld als u een resource-lus gebruiken om te maken van 100 virtuele machines, moet u interface netwerkcontrollers (NIC) correlatie met bijbehorende VMs en opslag-accounts. Omdat het aantal VMs waarschijnlijk afwijken van het aantal opslag accounts, moet u ook andere resource lus grootte, behandelt. Dit zijn opgelost problemen, maar de complexiteit aanzienlijk met schaal.

Een andere uitdaging treedt op wanneer u een infrastructuur die wordt aangepast elastically nodig hebt. U kunt bijvoorbeeld een automatisch schalen-infrastructuur automatisch toeneemt of afneemt van het aantal VMs in antwoord op werkbelasting. VMs-nummer niet opgeeft een geïntegreerde om naar variëren in getal (schaal af en schaal in). Als u de schaal in doordat VMs, is het moeilijk te garanderen beschikbaarheid door ervoor te zorgen dat VMs zijn verdeeld over de update en foutenstructuuranalyse domeinen.

Ten slotte, als u een lus resource gebruikt, meerdere oproepen resources maken gaat u naar het onderliggende stof. Wanneer meerdere oproepen vergelijkbare resources maakt, heeft Azure impliciete gelegenheid te verbeteren na dit ontwerp en implementatie betrouwbaarheid en prestaties optimaliseren. Dit is waar _VM schaal sets_ komen.

## <a name="virtual-machine-scale-sets"></a>VM schaal sets

VM schaal sets zijn een resource Azure berekenen te implementeren en beheren van een reeks identieke VMs. Geconfigureerd met alle VMs dezelfde, VM schaal sets heel gemakkelijk de schaal zijn in en schaal af. Wijzigt u het aantal VMs in de set. U kunt ook VM schaal sets naar automatisch schalen op basis van de vereisten van de werklast configureren.

Voor toepassingen die moeten berekeningscluster resources uit- en schaal, zijn er wordt impliciet schaal bewerkingen gelijkmatig over domeinen foutenstructuuranalyse en bijwerken.

In plaats van meerdere resources zoals NIC's en VMs correleren, heeft een VM schaal set netwerk, opslag, VM en extensie eigenschappen die u centraal kunt configureren.

Raadpleeg de [VM schaal productpagina ingesteld](https://azure.microsoft.com/services/virtual-machine-scale-sets/)voor een inleiding tot VM schaal sets. Ga naar de [virtuele machines schaal Hiermee stelt u documentatie](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)voor meer informatie.
