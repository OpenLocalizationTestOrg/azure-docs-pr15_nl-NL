<properties
   pageTitle="Hoe u een melding geven voor een virtuele Linux machine | Microsoft Azure"
   description="Meer informatie over een Linux virtuele machine die is gemaakt in het implementatiemodel resourcemanager met Azure labelen."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Hoe u een melding geven voor een Linux virtuele machine in Azure wordt aangegeven

In dit artikel worden de verschillende manieren om te markeren Linux virtuele machines in Azure wordt aangegeven door het implementatiemodel resourcemanager. Labels komen van de gebruiker gedefinieerde sleutel/waardeparen die rechtstreeks in een resource of een resourcegroep kunnen worden gebracht. Azure ondersteunt momenteel maximaal 15 tags per resource en resourcegroep. Labels kunnen worden ingesteld voor een bron op het moment van maken of toegevoegd aan een bestaande bron. Houd er rekening mee, labels worden ondersteund voor resources die zijn gemaakt via het alleen resourcemanager implementatiemodel.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Met Azure CLI labelen

Moet beginnen, [installeren en configureren van de Azure CLI](../xplat-cli-azure-resource-manager.md) en controleer of u bent in de modus resourcemanager (`azure config mode arm`).

U kunt alle eigenschappen weergeven voor een bepaalde virtuele Machine, inclusief de markeringen, met deze opdracht:

        azure vm show -g MyResourceGroup -n MyTestVM

Als u wilt een nieuwe code VM tot en met de CLI Azure toevoegt, kunt u de `azure vm set` command samen met de tag parameter **-t**:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Als u wilt verwijderen alle tags, kunt u de parameter **– T** in het `azure vm set` opdracht.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Nu dat we tags aan uw resources Azure CLI en de Portal toegepaste we even verder kijken op de details van het gebruik om de labels in de portal van de factuur weer te geven.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het taggen van uw Azure resources, raadpleegt u [Azure resourcemanager overzicht][] en [Tags gebruiken om u te organiseren van uw Azure-Resources][].
* Als u wilt zien hoe tags kunt u het gebruik van Azure resources beheren, raadpleegt u [informatie over uw factuur Azure][] en [inzicht in uw verbruik van Microsoft Azure krijgen][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure resourcemanager-overzicht]: ../azure-resource-manager/resource-group-overview.md
[Markeringen gebruiken om te organiseren van uw Azure-Resources]: ../resource-group-using-tags.md
[Informatie over uw Azure factuur]: ../billing/billing-understand-your-bill.md
[Inzicht in uw verbruik van Microsoft Azure krijgen]: ../billing-usage-rate-card-overview.md
