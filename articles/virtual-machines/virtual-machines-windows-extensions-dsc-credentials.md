<properties
   pageTitle="Referenties worden doorgegeven aan Azure DSC met | Microsoft Azure"
   description="Overzicht van de referenties veilig worden doorgegeven aan Azure virtuele machines met PowerShell gewenst staat configuratie"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Referenties worden doorgegeven aan de Azure DSC extensie-handler #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

In dit artikel worden de gewenste staat configuratie-extensie voor Azure. Een overzicht van de extensie-handler DSC kan worden gevonden op [Inleiding tot de configuratie van Azure staat gewenst extensie handler](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>Doorgeven in referenties
Als onderdeel van het configuratieproces, u mogelijk nodig hebt voor het instellen van gebruikersaccounts, toegang tot services of een programma in de context van een gebruiker installeren. Als u wilt deze dingen doet, moet u uw referenties. 

DSC kan met parameters configuraties waarin referenties zijn doorgegeven aan de configuratie en veilig worden opgeslagen in MOF-bestanden. Referentiebeheer eenvoudiger de Azure extensie Handler doordat automatisch beheer van certificaten. 

Houd rekening met het volgende DSC configuratiescript die Hiermee maakt u een lokale gebruikersaccount met het opgegeven wachtwoord:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Het is belangrijk om op te nemen *knooppunt localhost* als onderdeel van de configuratie. Als deze instructie ontbreekt, werken de volgende stappen niet zoals de extensie handler specifiek Hiermee wordt gezocht naar het knooppunt localhost-instructie. Het is ook belangrijk om op te nemen de typecast *[PsCredential]*, als deze specifiek type de extensie activeert voor het coderen van de referentie. 

Dit script publiceren met blob storage:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

De extensie Azure DSC instellen en geef de referentie:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Hoe referenties zijn beveiligd
Vraagt u deze code uitvoert voor een referentie. Zodra deze is opgegeven, wordt deze opgeslagen in het geheugen kort. Wanneer deze wordt gepubliceerd met `Set-AzureVmDscExtension` cmdlet, op deze is verzonden via HTTPS voor VM, waarbij Azure opgeslagen versleuteld op schijf, met behulp van de lokale VM-certificaat. Vervolgens wordt kort ontsleuteld in het geheugen en opnieuw als u wilt deze doorgeven aan DSC is versleuteld.

Dit probleem is anders dan [secure configuraties zonder de extensie-handler](https://msdn.microsoft.com/powershell/dsc/securemof). De Azure-omgeving kunt configuratiegegevens veilig via certificaten verzenden. Wanneer u met de extensie-handler DSC, er is niet nodig om aan te bieden $CertificatePath of een $CertificateID / $Thumbprint vermelding in ConfigurationData.


## <a name="next-steps"></a>Volgende stappen ##

Zie [Inleiding tot de configuratie van Azure staat gewenst extensie handler](virtual-machines-windows-extensions-dsc-overview.md)voor meer informatie over de Azure DSC extensie handler. 

Bekijk de [resourcemanager Azure-sjabloon voor de extensie DSC](virtual-machines-windows-extensions-dsc-template.md).

Voor meer informatie over PowerShell DSC, [gaat u naar het midden van de documentatie PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Als u wilt zoeken naar extra functionaliteit kunt u met PowerShell DSC, [de PowerShell-galerie bladeren](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) naar meer DSC-bronnen beheren.
