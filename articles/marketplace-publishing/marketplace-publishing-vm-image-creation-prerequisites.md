<properties
   pageTitle="Technische vereisten voor het maken van de afbeelding van een virtuele machine Azure Marketplace | Microsoft Azure"
   description="Meer informatie over de vereisten voor het maken en implementeren van de afbeelding van een virtuele machine met de Azure Marketplace voor anderen kunt kopen."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="04/29/2016"
  ms.author="hascipio; v-divte"/>

# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Technische vereisten voor het maken van de afbeelding van een virtuele machine Azure Marketplace
Het proces zorgvuldig voordat u begint lezen en begrijpen waar en waarom elke stap wordt uitgevoerd. Zoveel mogelijk, u moet uw bedrijfsgegevens en andere gegevens voorbereiden, benodigde hulpprogramma's downloaden en/of technische onderdelen maken voordat u begint met het maakproces aanbieding. Deze items moeten worden uit het controleren van dit artikel.  

## <a name="download-needed-tools--applications"></a>Download nodig hulpprogramma's en toepassingen
U kunt de volgende items klaar zijn voordat u begint met het proces nodig hebt:

- Afhankelijk van welk besturingssysteem u hebt samengesteld, moet u de [Azure PowerShell-cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) of [Linux opdrachtregel hulpmiddel](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) installeren vanaf de downloadpagina van [Azure](https://azure.microsoft.com/downloads/) .
- Azure opslag Explorer installeert via CodePlex.
- Download en installeer het hulpprogramma-certificering Test voor Azure gecertificeerd:
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). U moet een Windows-computer het hulpprogramma certificering uitvoeren. Als u een Windows-computer beschikbaar niet hebt, kunt u het hulpmiddel voor het gebruik van een VM op basis van Windows in Azure kunt uitvoeren.

## <a name="platforms-supported"></a>Ondersteunde platforms
U kunt op basis van Azure VMs in Windows of Linux ontwikkelen. Bepaalde elementen van het publicatieproces--zoals het maken van een compatibel zijn met Azure virtuele harde schijf (VHD)--gebruik andere hulpprogramma's en stappen afhankelijk van welk besturingssysteem u gebruikt:  

- Als u Linux gebruikt, raadpleegt u de sectie "Maken een Azure-compatibele VHD (Linux-basis)" van de [VM afbeelding publicerende handleiding](marketplace-publishing-vm-image-creation.md).
- Als u Windows gebruikt, raadpleegt u de sectie "Maken een Azure-compatibele VHD (Windows-basis)" van de [VM afbeelding publicerende handleiding](marketplace-publishing-vm-image-creation.md).

> [AZURE.NOTE] U hebt toegang tot een Windows-machine naar nodig:
- Voer de validatie certificeringsinstantie.
- Maak de VHD gedeeld access handtekening-URL voor het indienen van VHD-certificering.

## <a name="develop-your-vhd"></a>Ontwikkel uw VHD
Azure VHD's in de cloud of on-premises implementatie, kunt u ontwikkelen:

- Cloudgebaseerde ontwikkeling betekent dat alle stappen van de ontwikkeling extern worden uitgevoerd op een VHD op Azure.
- Ontwikkeling van de on-premises implementatie is vereist voor downloaden van een VHD en ontwikkelen van deze via on-premises implementatie-infrastructuur. Hoewel dit mogelijk is, wordt deze instelling niet aanbevolen. Houd er rekening mee dat ontwikkelen voor Windows of SQL on-premises implementatie, moet u de toetsen relevante on-premises implementatie-licentie hebben. U kunt geen opnemen of SQL Server na het maken van een VM installeren. U moet uw aanbod ook baseren op een goedgekeurde SQL-afbeelding van de Azure-portal. Als u besluit het ontwikkelen van on-premises implementatie, moet u anders dan als u in de cloud ontwikkelt enkele stappen uitvoeren. U vindt de relevante informatie in [een on-premises implementatie VM-afbeelding maken](marketplace-publishing-vm-image-creation-on-premise.md).

## <a name="next-steps"></a>Volgende stappen
Nu dat u de vereisten beoordeeld en de benodigde taken uitgevoerd, kunt u verdergaan met het maken van uw voorstel VM afbeelding zoals wordt beschreven in de [virtuele machine afbeelding publicerende handleiding](marketplace-publishing-vm-image-creation.md).

## <a name="see-also"></a>Zie ook
- [Aan de slag: een aanbieding publiceren naar de Azure Marketplace](marketplace-publishing-getting-started.md)
- [Een virtuele machine waarop Windows wordt uitgevoerd in de portal Azure preview maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
