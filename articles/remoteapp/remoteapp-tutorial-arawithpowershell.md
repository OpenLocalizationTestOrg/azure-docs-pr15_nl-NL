<properties
   pageTitle="PowerShell-cmdlets gebruiken met Azure RemoteApp | Microsoft Azure"
   description="Informatie over het gebruik van Windows PowerShell-cmdlets in Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Windows PowerShell-cmdlets gebruiken met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

 U kunt de Azure RemoteApp PowerShell-cmdlets gebruiken om te beheren en onderhouden van uw siteverzamelingen. Gebruik de volgende informatie aan de slag.

## <a name="get-the-cmdlets"></a>De cmdlets ophalen 
-------------
Download eerst het Azure Powershell-cmdlets [hier](http://go.microsoft.com/?linkid=9811175)de cmdlets zijn opgenomen in deze RemoteApp. 

Bekijk de [help van Azure RemoteApp-cmdlet](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Azure cmdlets uit om het gebruiken van uw abonnement configureren
------------------
[Deze handleiding](../powershell-install-configure.md) volgen zodat u de cmdlets ten opzichte van uw Azure-abonnement kunt gebruiken.

Deze stappen kunt u snel aan de slag:

1.  Download en installeer de [Azure PowerShell-cmdlets](http://go.microsoft.com/?linkid=9811175).
2.  Start Microsoft Azure PowerShell.
3.  Voer **Toevoegen-AzureAccount** om te verifiëren bij uw Azure-abonnement. Wanneer u wordt gevraagd, voer de gebruikersnaam en wachtwoord waarmee u zich aanmelden bij Azure portal.  
4.  Voer **Get-AzureSubscription** u kunt de abonnementen die is gekoppeld aan uw gebruikersaccount. 
5.  **Selecteer AzureSubscription** uitvoeren en geef de naam van abonnement of -ID wilt gebruiken in de PowerShell-console.

Gefeliciteerd, uw Azure PowerShell-console is geconfigureerd en klaar voor gebruik. Houd er rekening mee dat moet u herhaalt stappen 2 tot en met 5 telkens wanneer u begint de de Azure PowerShell-console.  

## <a name="create-a-cloud-collection"></a>Een cloud-verzameling maken
--------------------
Het is eenvoudig, voer de volgende opdracht:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

De bovenstaande opdracht publiceert automatisch Microsoft Office 365-toepassingen (Excel, OneNote, Outlook, PowerPoint, Visio en Word).

Maken van een siteverzameling kan 30 minuten duren voordat of langer duren. Deze opdracht het resultaat daarom een bijhouden-ID waarmee u als volgt kunt:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Wanneer de verzameling klaar is, kunt u gebruikers kunt toevoegen aan de collectie met de volgende opdracht uit:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

En u bent klaar! Die gebruiker moet kunnen verbinding maken met de toepassing van de Azure RemoteApp-client gevonden [hier](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Beschikbare cmdlets
Er zijn een groot aantal andere opdrachten die we hebben, de documentatie van deze wordt kort worden komen:

Cmdlets voor eenvoudige RemoteApp-siteverzameling: 

- Nieuwe AzureRemoteAppCollection
- Get-AzureRemoteAppCollection
- Set-AzureRemoteAppCollection
- Update-AzureRemoteAppCollection
- Verwijderen AzureRemoteAppCollection
- Toevoegen AzureRemoteAppUser
- Get-AzureRemoteAppUser
- Verwijderen AzureRemoteAppUser
- Get-AzureRemoteAppSession
- De verbinding verbreken AzureRemoteAppSession
- Roepen AzureRemoteAppSessionLogoff
- Verzenden-AzureRemoteAppSessionMessage
- Get-AzureRemoteAppProgram
- Get-AzureRemoteAppStartMenuProgram
- Publiceren AzureRemoteAppProgram
- Publicatie-AzureRemoteAppProgram
- Get-AzureRemoteAppCollectionUsageDetails
- Get-AzureRemoteAppCollectionUsageSummary
- Get-AzureRemoteAppPlan

Cmdlets voor RemoteApp virtueel netwerk:

- Nieuwe AzureRemoteAppVNet
- Get-AzureRemoteAppVNet
- Set-AzureRemoteAppVNet
- Verwijderen AzureRemoteAppVNet
- Get-AzureRemoteAppVpnDevice
- Get--AzureRemoteAppVpnDeviceConfigScript
- Beginwaarden AzureRemoteAppVpnSharedKey

Cmdlets voor RemoteApp-sjabloon afbeelding:

- Nieuwe AzureRemoteAppTemplateImage
- Get-AzureRemoteAppTemplateImage
- Naam wijzigen-AzureRemoteAppTemplateImage
- Verwijderen AzureRemoteAppTemplateImage

Andere RemoteApp-cmdlets:

- Get-AzureRemoteAppLocation
- Get-AzureRemoteAppWorkspace
- Set-AzureRemoteAppWorkspace
- Get-AzureRemoteAppOperationResult
 
