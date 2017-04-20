<properties
    pageTitle="VM schaal ontwerpen ingesteld voor schaal | Microsoft Azure"
    description="Meer informatie over het ontwerpen van uw VM schaal Sets voor schaal"
    keywords="VM Linux, VM schaal sets" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>VM schaal ontwerpen ingesteld voor schaal

In dit onderwerp wordt beschreven hoe ontwerpoverwegingen voor VM schaal Sets. Voor informatie over wat VM schaal Sets zijn, raadpleegt u [VM schaal Sets overzicht](virtual-machine-scale-sets-overview.md).


## <a name="storage"></a>Opslag

Een set schaal gebruikt opslagruimte accounts voor de opslag van de schijf OS van de VMs in de set. Het is raadzaam een breedteverhouding van 20 VMs per opslag-account of minder. Ook aangeraden dat u verdeeld over het alfabet het begin van de namen van de account opslag. Hierdoor geeft helpt laden verspreid over verschillende interne systemen. Bijvoorbeeld, in de volgende sjabloon we gebruiken de uniqueString resourcemanager sjabloonfunctie om te genereren voorvoegsel hashes die zijn toegevoegd aan de accountnamen opslag: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Overprovisioning

Beginnen met de versie van de API "2016-03-30", VM schaal Sets standaard ingesteld op "overprovisioning" VMs. Met overprovisioning ingeschakeld, wordt in de schaal daadwerkelijk draaien omhoog meer VMs dan u wordt gevraagd om ingesteld, wordt Hiermee verwijdert u de extra VMs die of van laatste. Overprovisioning verbetert inrichten success tarieven. U niet zijn gefactureerd voor deze extra VMs en ze niet naar uw quotumlimiet worden meegerekend.

Terwijl overprovisioning, wordt de inrichten success tarieven verbeterd, kan deze leiden tot verwarrende gedrag voor een toepassing die niet is ontworpen voor het verwerken van VMs te verdwijnen aangekondigd. Zorgen dat u hebt de volgende tekenreeks in uw sjabloon om te schakelen overprovisioning uitschakelen,: "overprovision": "false". Meer informatie vindt u in de [documentatie VM schaal instellen REST API](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Als u overprovisioning uitschakelt, krijgt u afwezig bent met een grotere ratio van VMs per opslag-account, maar slag boven 40 wordt niet aanbevolen.


## <a name="limits"></a>Limieten
Een set schaal is gebaseerd op een aangepaste afbeelding (een door u gemaakt) moet alle OS VHD's van schijf binnen één opslag-account maken. Hierdoor is het maximum aantal VMs in een set schaal is gebaseerd op een aangepaste afbeelding aanbevolen 20. Als u overprovisioning uitschakelt, kunt u maximaal 40 kunt gaan.

Een set schaal is gebaseerd op de afbeelding van een platform is momenteel beperkt tot 100 VMs (wordt aangeraden 5 opslag-accounts voor deze schaal).

Voor meer VMs dan deze limieten toestaat, moet u meerdere schaal sets implementeren, zoals in [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).