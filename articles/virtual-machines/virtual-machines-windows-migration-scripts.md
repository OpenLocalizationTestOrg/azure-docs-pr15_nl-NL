<properties
    pageTitle="Hulpmiddelen voor community's voor beheer van Azure-Service Azure resourcemanager migreren"
    description="In dit artikel verzamelt de hulpmiddelen die zijn verleend door de community om u te helpen bij de migratie van IaaS resources uit het beheer van Azure-Service aan de stapel resourcemanager Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Hulpmiddelen voor community's voor beheer van Azure-Service Azure resourcemanager migreren

In dit artikel verzamelt de hulpmiddelen die zijn verleend door de community om u te helpen bij de migratie van IaaS resources uit het beheer van Azure-Service aan de stapel resourcemanager Azure.

>[AZURE.NOTE]Deze hulpprogramma's worden niet officieel ondersteund door Microsoft Support. Daarom ze zijn geopend die afkomstig zijn op Github en we willen graag PRs accepteren voor reparaties of extra scenario's. Als u wilt rapporteren een probleem, moet u de gebruken Github problemen.
>
> Migreren met deze hulpmiddelen, treedt downtime voor uw klassieke virtuele Machine. Als u de migratie platform ondersteund zoekt, gaat u naar 
>
>- [Platform ondersteund migratie van IaaS resources van klassiek Azure resourcemanager stapel](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Technische uitgebreide kennismaking op Platform ondersteund migratie van klassiek naar Azure resourcemanager](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [IaaS resources migreren van klassiek naar Azure resourcemanager via Azure PowerShell](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

Dit is een PowerShell-script-module voor uw **één** virtuele Machine (VM) migreren van Azure servicebeheer stapel naar Azure resourcemanager stapel. 

[Koppeling naar de documentatie hulpmiddel](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

migAz is een extra optie om te migreren van een volledige reeks Azure-Service Management IaaS resources naar Azure resourcemanager IaaS resources. De migratie kan zich voordoen binnen hetzelfde abonnement of tussen de verschillende abonnementen en soorten abonnementen (ex: CSP abonnementen).

[Koppeling naar de documentatie hulpmiddel](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)