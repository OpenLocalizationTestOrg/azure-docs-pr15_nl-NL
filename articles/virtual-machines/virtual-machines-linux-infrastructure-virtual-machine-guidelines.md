<properties
    pageTitle="Linux virtuele Machines richtlijnen | Microsoft Azure"
    description="Meer informatie over de belangrijkste ontwerpen en implementeren richtlijnen voor het implementeren van Linux virtuele machines in Azure"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="virtual-machines-guidelines"></a>Virtuele machines richtlijnen

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

In dit artikel ligt de nadruk met informatie over de vereiste planning stappen voor het maken en beheren van virtuele machines (VMs) in uw Azure-omgeving.

## <a name="implementation-guidelines-for-vms"></a>Implementatie van richtlijnen voor VMs
Beslissingen:

- Hoeveel VMs hoeft u voor de verschillende lagen en onderdelen van de infrastructuur van uw?
- De bronnen van processor en geheugen heeft elke VM nodig en wat zijn de opslagvereisten?

Taken:

- Definieer de werkbelasting voor uw toepassing en de resources die de VMs vereisen.
- De systeembronnen voor elke VM met het juiste VM grootte en opslag type uitlijnen.
- Definieer uw resourcegroepen voor de andere niveaus en de onderdelen van de infrastructuur van uw.
- Definieer uw naamgevingsconventie VM.
- Maak uw VMs gebruik van de Azure CLI, web portal, of met resourcemanager sjablonen.

## <a name="virtual-machines"></a>Virtuele machines

Een van de belangrijkste onderdelen binnen uw Azure-omgeving is waarschijnlijk VMs. Dit is waar u uw toepassingen, databases, verificatieservices, enzovoort uitvoert.

Het is belangrijk is voor meer informatie over de [verschillende VM grootte](virtual-machines-linux-sizes.md) correct grootte van uw omgeving van een prestaties en kosten perspectief. Als uw VMs geen onvoldoende CPU cores of geheugen, daaronder de prestaties van uw toepassing ongeacht hoe u ook deze is ontworpen en ontwikkeld. Controleer de voorgestelde werkbelasting voor elke reeks VM als uitgangspunt bepaalt u welke grootte VM voor elk onderdeel in de infrastructuur van uw wilt gebruiken. U kunt [wijzigen van de grootte van een VM](virtual-machines-linux-change-vm-size.md) na implementatie.

Opslag speelt een belangrijke rol in VM prestaties. U kunt standaard opslag, die wordt gebruikt normale ronddraait schijven, of Premium opslag gebruiken voor hoge i/o-werkbelasting en optimale prestaties, met SSD schijven. Als met de grootte VM, er zijn kosten overwegingen aan het selecteren van het opslagmedium. U kunt de [opslagruimte infrastructuur richtlijnen artikel](virtual-machines-linux-infrastructure-storage-solutions-guidelines.md) als u wilt weten hoe u bij het ontwerpen van de juiste opslag voor optimale prestaties van uw VMs lezen.


## <a name="resource-groups"></a>Resourcegroepen
Onderdelen zoals VMs logisch gegroepeerd voor eenvoudige beheer en onderhoud met [Azure resourcegroepen](../azure-resource-manager/resource-group-overview.md). Met behulp van resourcegroepen, kunt u maken, beheren en controleren van alle resources die samen een bepaalde toepassing. U kunt ook [op basis van rollen toegang tot besturingselementen](../active-directory/role-based-access-control-what-is.md) om toegang tot anderen in uw team alleen de bronnen die zij nodig hebben implementeren. Tijd in uw resourcegroepen en roltoewijzingen plan nemen. Er zijn verschillende manieren daadwerkelijk ontwerpen en implementeren van resourcegroepen, daarom moet u de [resource groepen richtlijnen artikel](virtual-machines-linux-infrastructure-resource-groups-guidelines.md) als u wilt weten hoe het beste bouwen uw VMs lezen.


## <a name="templates"></a>Sjablonen 
U kunt sjablonen, gedefinieerd door declaratieve JSON-bestanden maken van uw VMs maken. Sjablonen meestal maken ook de vereiste opslag, netwerken, netwerkinterfaces, IP-adressen, enzovoort samen met de VMs zelf. U kunt sjablonen maken consistente, gereproduceerd omgevingen voor ontwikkeling en testen doeleinden eenvoudig repliceren productieomgevingen en vice versa. U kunt meer informatie over het [maken en gebruiken van sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment) voor meer informatie over hoe u deze kunt gebruiken voor het maken en implementeren van uw VMs.


## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 