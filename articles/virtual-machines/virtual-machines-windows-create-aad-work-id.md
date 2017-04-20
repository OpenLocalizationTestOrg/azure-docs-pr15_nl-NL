<properties
   pageTitle="Een werk- of schoolaccount identiteit maken in AAD | Microsoft Azure"
   description="Informatie over het maken van een identiteit werk- of schoolaccount in Azure Active Directory voor gebruik met uw Windows virtuele machines."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-windows-vms"></a>Maken van een identiteit voor werk of School in Azure Active Directory voor gebruik met Windows VMs

Als u een persoonlijk Azure-account hebt gemaakt of een persoonlijke MSDN-abonnement hebt en gemaakt van de Azure-account om te profiteren van de tegoeden MSDN Azure--gebruikt u de identiteit van een *Microsoft-account* moet maken. Groot aantal handige functies [resource groep sjablonen](../azure-resource-manager/resource-group-overview.md) is een voorbeeld van Azure----een werk- of schoolaccount-account (een identiteit beheerd door Azure Active Directory) nodig om te werken. U kunt de onderstaande instructies voor het maken van een nieuw werk- of schoolaccount, omdat gelukkig een van de beste dingen over uw persoonlijke Azure-account is dat deze wordt geleverd met een standaard Azure Active Directory-domein dat u gebruiken kunt om te maken van een nieuw werk- of schoolaccount dat u kunt gebruiken met Azure functies waarvoor deze kunt volgen.

Echter recente wijzigingen kunnen voor het beheren van uw abonnement met een willekeurig type van het gebruik van Azure-account de `azure login` interactieve aanmelding methode beschreven [hier](../xplat-cli-connect.md). U kunt deze om gebruiken of u kunt de instructies die volgt. U kunt ook [maken van een werk of school identiteit in Azure Active Directory voor gebruik met Linux VMs](virtual-machines-linux-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
