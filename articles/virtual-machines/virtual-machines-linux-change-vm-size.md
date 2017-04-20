<properties
   pageTitle="Hoe u het formaat van een Linux VM | Microsoft Azure"
   description="Hoe u de schaal omhoog of omlaag een virtuele Linux-computer met het wijzigen van de grootte VM wilt verkleinen."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>Hoe u het formaat van een Linux VM

## <a name="overview"></a>Overzicht 

Nadat u een virtuele machine (VM) inrichten, kunt u de schaal de VM omhoog of omlaag door de [grootte van de VM][vm-sizes]. In sommige gevallen, moet u eerst de VM toewijzing. Dit kan gebeuren als de nieuwe grootte is niet beschikbaar op het cluster hardware die als host de VM fungeert.

In dit artikel leest u hoe u het formaat van een Linux VM gebruik van de [Azure CLI][azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.


## <a name="resize-a-linux-vm"></a>De grootte van een VM Linux wijzigen 

Als u een VM, moet u de volgende stappen uitvoeren.

1. Voer de volgende opdracht uit CLI. Deze opdracht bevat de VM-grootte die beschikbaar zijn op het hardware cluster waar de VM wordt gehost.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Als de gewenste grootte wordt weergegeven, voert u de volgende opdracht uit om het formaat van de VM aan.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    De VM opnieuw tijdens dit proces. Na het opnieuw starten, de bestaande OS en de gegevensschijven opnieuw toegewezen. Iets op de tijdelijke schijf niet verloren.

    Gebruik de `--enable-boot-diagnostics` optie [opstarten diagnostische gegevens]kunt[boot-diagnostics], aan te melden eventuele fouten die betrekking hebben op opstarten.

3. Als de gewenste grootte niet wordt vermeld, voer de volgende opdrachten de VM, de toewijzing grootte wijzigt, en start de VM opnieuw.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] De VM vrijgeven loslaat dynamische IP-adressen die zijn toegewezen aan de VM ook. De schijven OS en gegevens worden niet beïnvloed.
   
## <a name="next-steps"></a>Volgende stappen

Voor extra schaalbaarheid, meerdere exemplaren van VM uitvoeren en schaal af. Zie voor meer informatie [automatisch schalen Linux-computers in een virtuele Machine schaal instellen][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md