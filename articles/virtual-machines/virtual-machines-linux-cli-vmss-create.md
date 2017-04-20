<properties
    pageTitle="Wat VM schaal zijn ingesteld? | Microsoft Azure"
    description="Meer informatie over VM schaal sets."
    keywords="VM Linux, VM schaal sets" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Wat VM schaal zijn ingesteld?

VM schaal Sets kunt u meerdere VMs beheren als een set. Op hoog niveau bestaan schaal sets uit de volgende voor- en nadelen:

Professionals:

1. Beschikbaarheid. Elke set schaal wordt de VMs in een beschikbaarheid instellen met 5 foutenstructuuranalyse domeinen (FDs) en 5 Update domeinen (UDs) zodat deze zeker beschikbaar activeert (Zie voor meer informatie over FDs en UDs, [VM beschikbaarheid](./virtual-machines-linux-manage-availability.md)). 
2. Eenvoudige integratie met Azure taakverdeling en App Gateway.
3. Eenvoudige integratie met Azure automatisch schalen.
4. Eenvoudige implementatie, beheer en opschonen van VMs.
5. Ondersteuning voor algemene Windows en Linux typen, evenals aangepaste afbeeldingen.

Nadelen:

1. Geen gegevensschijven niet toevoegen aan VM exemplaren in een set schaal. Moet gebruiken in plaats daarvan Blob Storage, Azure-bestanden, Azure tabellen of andere opslagoplossing.

## <a name="quick-create-using-azure-cli"></a>Snel aan maken met Azure CLI

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Volgende stappen

Voor algemene informatie, raadpleegt u de [belangrijkste openingspagina voor schaal sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Voor meer documentatie, raadpleegt u de [documentatiepagina van de belangrijkste voor schaal ingesteld](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Bijvoorbeeld zoeken resourcemanager sjablonen schaal sets met naar 'vmss' in de [Azure Quickstart sjablonen github cessies‑retrocessies](https://github.com/Azure/azure-quickstart-templates).

