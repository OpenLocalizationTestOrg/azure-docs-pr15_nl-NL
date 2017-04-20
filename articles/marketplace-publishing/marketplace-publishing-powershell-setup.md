<properties
   pageTitle="PowerShell instellen voor het maken van een VM Marketplace | Microsoft Azure"
   description="Instructies voor het instellen van Azure PowerShell en deze als een optioneel proces flow als u wilt maken van VM afbeeldingen om te implementeren naar en verkopen op de Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Azure PowerShell instellen voor het maken van een aanbod voor Azure Marketplace
Zie voor gedetailleerde informatie over het instellen van PowerShell in Azure wordt aangegeven, [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Een eenvoudige manier is via de certificaat-methode, die is gedownload en de invoer van een certificaat dat u nodig hebt voor verificatie. U krijgt het benodigde certificaat, gebruikt u de cmdlet **Get-AzurePublishSettingsFile** . Sla het bestand wanneer u wordt gevraagd. Als u wilt importeren het certificaat in een PowerShell-sessie, gebruikt u de cmdlet **Importeren-AzurePublishSettingsFile** .

Als u wilt configureren en bewaren van de algemene instellingen voor de Microsoft Azure-abonnement voor de PowerShell-sessie, gebruikt u de cmdlets **Set-AzureSubscription** en **Selecteer AzureSubscription** :

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Een standaardaccount voor de opslag in koppelt de eerste opdracht aan het abonnement (nodig voor bepaalde VM inrichten bewerkingen).  De tweede kunt u het abonnement dat de huidige rij (herkend door andere cmdlets).

## <a name="see-also"></a>Zie ook
- [Aan de slag: een aanbieding publiceren naar de Azure Marketplace](marketplace-publishing-getting-started.md)
- [Maken van de afbeelding van een virtuele machine Marketplace](marketplace-publishing-vm-image-creation.md)
