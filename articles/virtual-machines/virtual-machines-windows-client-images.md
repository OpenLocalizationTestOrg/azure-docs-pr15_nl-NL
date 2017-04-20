<properties
   pageTitle="Met behulp van Windows client-installatiekopieën voor ontwikkelaar/testen scenario's | Microsoft Azure"
   description="Het gebruik van de voordelen van de Visual Studio-abonnement om te implementeren van Windows 7/8/10 in Azure wordt aangegeven voor ontwikkelaar/testen scenario's"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>Met behulp van Windows client in Azure wordt aangegeven voor ontwikkelaar/testen scenario 's

Kunt u Windows 7, Windows 8 of Windows 10 in Azure wordt aangegeven voor ontwikkelaar/testen scenario's die dat u een juiste Visual Studio (voorheen MSDN)-abonnement hebt. In dit artikel worden de vereisten geschiktheid voor actieve Windows client in Azure en het gebruik van de galerie met Azure-afbeeldingen.


## <a name="subscription-eligibility"></a>Abonnement geschiktheid
Actieve Visual Studio abonnees (personen die een licentie Visual Studio-abonnement hebt aangeschaft) kunnen Windows-client gebruiken voor het ontwikkelen en testen. Windows-client kan worden gebruikt op uw eigen hardware en Azure virtuele machines uitgevoerd in een type Azure abonnement. Windows-client kan niet worden geïmplementeerd in of gebruikt op Azure voor gebruik in normale productieomgeving, of gebruikt door mensen die niet actieve Visual Studio abonnees.

Het uitkomt hebt we beschikbaar gemaakt voor bepaalde Windows 10-afbeeldingen uit de galerie met Azure binnen een [in aanmerking komend ontwikkelaar/testen biedt](#eligible-offers). Visual Studio abonnees binnen een willekeurig type aanbieding kunnen ook [voldoende voorbereiden en maak](virtual-machines-windows-prepare-for-upload-vhd-image.md) een 64-bits Windows 7, Windows 8 of Windows 10-afbeelding en klik vervolgens [uploaden naar Azure](virtual-machines-windows-upload-image.md). Het gebruik blijft beperkt tot ontwikkelaar/testen door actieve Visual Studio abonnees.


## <a name="eligible-offers"></a>In aanmerking komend aanbiedingen
De volgende tabel worden de aanbieding-id's die in aanmerking komen voor de implementatie van Windows 10 via de galerie met Azure. De Windows 10-afbeeldingen zijn alleen zichtbaar voor de volgende aanbiedingen. Visual Studio-abonnees die voor het uitvoeren van Windows client in een andere aanbieding type moeten u [voldoende voorbereiden](virtual-machines-windows-prepare-for-upload-vhd-image.md) en maken, 64-bits Windows 7, Windows 8 of Windows 10 en een afbeelding [en vervolgens uploaden naar Azure](virtual-machines-windows-upload-image.md).

| De naam van de aanbieding | Aanbieding getal | Beschikbare client-afbeeldingen |
|:-----------|:------------:|:-----------------------:|
| [Pay-As-You-Go ontwikkelaar/testen](https://azure.microsoft.com/offers/ms-azr-0023p/)                          | 0023P | Windows 10 |
| [Visual Studio Enterprise (MPN) abonnees](https://azure.microsoft.com/offers/ms-azr-0029p/)      | 0029P | Windows 10 |
| [Visual Studio Professional abonnees](https://azure.microsoft.com/offers/ms-azr-0059p/)          | 0059P | Windows 10 |
| [Visual Studio Test Professional abonnees](https://azure.microsoft.com/offers/ms-azr-0060p/)     | 0060P | Windows 10 |
| [Visual Studio Premium met MSDN (voordeel)](https://azure.microsoft.com/offers/ms-azr-0061p/)       | 0061P | Windows 10 |
| [Visual Studio Enterprise abonnees](https://azure.microsoft.com/offers/ms-azr-0063p/)            | 0063P | Windows 10 |
| [Abonnees van Visual Studio Enterprise (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10 |
| [Enterprise ontwikkelaar/testen](https://azure.microsoft.com/ofers/ms-azr-0148p/)                              | 0148P | Windows 10 |


## <a name="check-your-azure-subscription"></a>Controleer uw Azure-abonnement
Als u niet uw aanbieding-ID weet, kunt u dit alsnog via de portal van Azure of de Account-portal.

De aanbieding abonnements-ID wordt vermeld op het blad 'Abonnementen' binnen de Azure-portal:

![Details over voorstel-ID van de Azure-portal](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

U kunt ook de aanbieding-ID van het [tabblad 'Abonnementen'](http://account.windowsazure.com/Subscriptions) van de portal Azure-Account weergeven:

![Details over aanbieding-ID in de portal Azure-Account](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 


## <a name="next-steps"></a>Volgende stappen
Nu kunt u uw VMs met [PowerShell](virtual-machines-windows-ps-create.md) [resourcemanager sjablonen](virtual-machines-windows-ps-template.md)of [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)implementeren.
