<properties
   pageTitle="FQDN maken voor een VM in Azure-portal | Microsoft Azure"
   description="Informatie over het maken van een volledig gekwalificeerde domeinnaam of FQDN-naam voor een Resource Manager op basis van virtuele machine in de portal van Azure."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Een volledig gekwalificeerde domeinnaam maken in de portal van Azure
Wanneer u een virtuele machine (VM) in de [portal van Azure](https://portal.azure.com) met het implementatiemodel resourcemanager maakt, wordt er automatisch een openbare IP-resource voor de virtuele machine gemaakt. U kunt dit IP-adres gebruiken extern toegang tot de VM. Hoewel de portal geen een [volledig gekwalificeerde domeinnaam](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), of FQDN, al dan niet standaard maakt, kunt u een maken nadat de VM is gemaakt. In dit artikel ziet u de stappen voor het maken van een DNS-naam of de FQDN-naam.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

U kunt nu extern verbinding maken met de VM met deze DNS-naam zoals voor Remote Desktop Protocol (RDP).

## <a name="next-steps"></a>Volgende stappen
Nu dat uw VM de naam van een openbare IP- en DNS bevat, kunt u veelvoorkomende toepassingskaders of services zoals IIS, SQL of SharePoint implementeren.

U kunt ook meer informatie over het [gebruik van resourcemanager](../azure-resource-manager/resource-group-overview.md) voor tips over het samenstellen van uw Azure implementaties.