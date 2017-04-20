<properties
    pageTitle="Veelgestelde vragen over Azure stapel | Microsoft Azure"
    description="Azure stapel Veelgestelde vragen."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Veelgestelde vragen over Azure-Stack

## <a name="deployment"></a>Implementatie

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Moet ik mijn gegevensschijven opmaken voordat starten of een installatie opnieuw te starten?

Schijven moet onbewerkte indeling. Als u het besturingssysteem opnieuw moet installeren, moet u mogelijk controleren als de oude opslag van toepassingen nog steeds aanwezig is en te verwijderen met de volgende stappen:

1. Open Serverbeheer.
2. Selecteer de opslag van toepassingen.
3. Zie als een groep wordt weergegeven.
4. Met de rechtermuisknop op de **opslag van toepassingen** als vermeld en inschakelen van lezen / schrijven.
5. Met de rechtermuisknop op **virtuele harde schijf** (linksonder) en selecteer verwijderen.
6. Met de rechtermuisknop op de **Opslag van toepassingen** en klik op verwijderen.
7. Azure stapel script opnieuw starten en controleer of dat de schijf-verificatie wordt doorgegeven.

U kunt ook kan het volgende script worden gebruikt:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Kan ik alle SSD schijven voor de opslag van toepassingen in de installatie Haalbaarheidstest gebruiken?

Deze configuratie wordt niet ondersteund in deze release.  Zie voor meer informatie de [vereisten handleiding](azure-stack-deploy.md) voor meer informatie.

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>Kan ik NVMe gegevensschijven voor de Microsoft Azure stapel Haalbaarheidstest gebruiken?

Terwijl opslag spaties directe NVMe schijven ondersteunt, ondersteunt Azure stapel alleen een subset van de combinaties mogelijke en mogelijke stationstypen voor opslag spaties directe. 

### <a name="how-can-i-reinstall-azure-stack"></a>Hoe kan ik Azure stapel opnieuw installeren?
U kunt de stappen in de [handleiding voor opnieuw installeren](azure-stack-redeploy.md).  

## <a name="tenant"></a>Tenant

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Kan ik mijn eigen afbeeldingen implementeren als een tenant?

Ja, net als in Azure wordt aangegeven, een tenant kunt uploaden afbeeldingen in Azure stapel, naast de afbeeldingen van de service-beheerder. Zie voor een overzicht, het [toevoegen van een afbeelding VM](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Testen

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Kan ik geneste Virtualization gebruiken om te testen van de Microsoft Azure stapel Haalbaarheidstest?

Geneste virtualization niet wordt ondersteund of getest met Azure stapel Technical Preview 2.

## <a name="virtual-machines"></a>Virtuele machines

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Ondersteunt Azure stapel dynamische schijven voor virtuele machines?

Azure stapel biedt geen ondersteuning voor dynamische schijven.

## <a name="next-steps"></a>Volgende stappen

[Problemen oplossen](azure-stack-troubleshooting.md)
