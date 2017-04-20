<properties
   pageTitle="Gewenste provinciale configuratie voor Azure overzicht | Microsoft Azure"
   description="Overzicht voor het gebruik van de handler Microsoft Azure-extensie voor PowerShell gewenst staat configuratie. Inclusief vereisten, architectuur, cmdlets..."
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

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Inleiding tot de configuratie van Azure staat gewenst extensie-handler #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

De Azure VM Agent en de bijbehorende extensies uitmaken deel van de Microsoft Azure-infrastructuurservices. VM extensies zijn software-onderdelen die de functionaliteit VM uitbreidt en verschillende VM management-bewerkingen te vereenvoudigen. Bijvoorbeeld de extensie VMAccess kan worden gebruikt om het wachtwoord van een beheerder of de extensie aangepast Script kan worden gebruikt om een script uitvoeren op de VM.

In dit artikel maakt u kennis met de extensie PowerShell gewenst staat configuratie (DSC) voor Azure VMs als onderdeel van de Azure PowerShell SDK. U kunt nieuwe cmdlets gebruiken om te uploaden en een PowerShell DSC-configuratie toepassen op een Azure VM met de extensie PowerShell DSC is ingeschakeld. De PowerShell-DSC extensie oproepen naar PowerShell DSC de ontvangen DSC-configuratie op de VM vast. Deze functionaliteit is ook beschikbaar via de portal van Azure.

## <a name="prerequisites"></a>Vereisten voor ##
**Lokale computer** Als u wilt werken met de extensie Azure VM, moet u de Azure-portal of de SDK van Azure PowerShell gebruiken. 

**Gast Agent** De Azure VM die worden geconfigureerd door de configuratie DSC moet een OS die ondersteuning biedt voor op Windows Management Framework (WMF) 4.0 of 5.0. De volledige lijst met ondersteunde OS versies kan worden gevonden op de [Versiegeschiedenis van DSC extensie](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Termen en concepten ##
Deze handleiding wordt ervan uitgegaan bekend zijn met de volgende begrippen:

Configuratie: een DSC configuratie-document. 

Knooppunt - een doel voor een DSC-configuratie. In dit document verwijst "knooppunt" altijd naar een VM Azure.

Configuratiegegevens - een .psd1 bestand met milieu gegevens voor een configuratie

## <a name="architectural-overview"></a>Overzicht van de architectuur ##

Azure VM Agent framework door de extensie Azure DSC wordt gebruikt om te leveren, vast en DSC configuraties waarop Azure VMs rapporteren. De extensie DSC verwacht een ZIP-bestand met ten minste een document configuratie en een set met parameters die worden verstrekt via de PowerShell-SDK Azure of via de portal van Azure.

Als de extensie voor de eerste keer wordt aangeroepen, wordt een installatieproces uitgevoerd. Dit proces wordt er een versie van de Windows Management Framework (WMF) gebruik van de volgende logica:

1. Als het besturingssysteem van Azure VM Windows Server 2016 is, geen actie is die u hebt gemaakt. Windows Server 2016 al de nieuwste versie van PowerShell is geïnstalleerd.
2. Als de `wmfVersion` eigenschap is opgegeven, wordt deze versie van de WMF is geïnstalleerd, tenzij deze is niet compatibel met van de VM OS.
3. Als er geen `wmfVersion` eigenschap is opgegeven, wordt de laatste toepassing versie van de WMF is geïnstalleerd.

Installatie van de WMF moet opnieuw opstarten. Na opnieuw opstarten, de extensie downloads het ZIP-bestand dat is opgegeven in de `modulesUrl` eigenschap. Als deze locatie in Azure-blobopslag, een token SA's kan worden opgegeven in de `sasToken` eigenschap voor toegang tot het bestand. Nadat de .zip is gedownload en uitgepakt, wordt de functie configuratie gedefinieerd in `configurationFunction` wordt uitgevoerd om de MOF-bestand te genereren. De extensie wordt uitgevoerd `Start-DscConfiguration -Force` op het gegenereerde MOF-bestand. De extensie wordt vastgelegd uitvoer en schrijft dat alleen terug om naar het kanaal Azure-Status. Vanaf nu op de KGV DSC verwerkt cmdlets voor controle en correctie als normale. 

## <a name="powershell-cmdlets"></a>PowerShell-cmdlets ##

PowerShell-cmdlets kan worden gebruikt met ARM of ASM naar inpakken, publiceren en DSC extensie implementaties controleren. De volgende cmdlets vermeld zijn de ASM modules, maar "Azure" kan worden vervangen door 'AzureRm' gebruik van het model ARM. Bijvoorbeeld `Publish-AzureVMDscConfiguration` gebruikmaakt van ASM, waar `Publish-AzureRmVMDscConfiguration` ARM gebruikt. 

`Publish-AzureVMDscConfiguration`gaat in een configuratiebestand, zoekt deze naar afhankelijke DSC bronnen en Hiermee maakt u een ZIP-bestand met de configuratie en DSC resources die nodig zijn voor het nemen van de configuratie. Dit kunt ook het pakket lokaal met maken de `-ConfigurationArchivePath` parameter. Anders het ZIP-bestand worden gepubliceerd naar Azure-blobopslag en beveiligen met een token SA's.

Het ZIP-bestand dat is gemaakt door deze cmdlet heeft een script voor de configuratie van de .ps1 in de hoofdmap van de archiefmap. Resources zijn de module-map geplaatst in de archiefmap. 

`Set-AzureVMDscExtension`de instellingen die nodig zijn voor de extensie PowerShell DSC in een VM configuratie-object vervolgens kan worden toegepast op een VM Azure met invoegt `Update-AzureVM`.

`Get-AzureVMDscExtension`de status van de extensie DSC van een bepaalde VM zijn opgehaald. 

`Get-AzureVMDscExtensionStatus`de status van de DSC-configuratie door de DSC extensie handler van kracht zijn opgehaald. Deze actie kan worden uitgevoerd op een enkele VM of groep VMs.

`Remove-AzureVMDscExtension`Hiermee verwijdert de extensie-handler uit een bepaalde virtuele machine. Deze cmdlet wordt **niet** verwijderen de configuratie, de WMF verwijderen of wijzigen van de toegepaste instellingen op de virtuele machine. Alleen de extensie-handler wordt verwijderd. 

**Belangrijke verschillen in ASM en ARM cmdlets**

- Op ARM cmdlets worden synchroon. ASM cmdlets zijn asynchroon.
- Zijn alle parameters voor de nieuwe vereiste ResourceGroupName, VMName ArchiveStorageAccountName, versie en locatie.
- ArchiveResourceGroupName is een nieuwe optionele parameter voor ARM. Wanneer uw opslagruimte account deel uitmaakt van een resourcegroep dan de tabel waar de virtuele machine die is gemaakt, kunt u deze parameter opgeven.
- ConfigurationArchive heet ArchiveBlobName in ARM
- ContainerName heet ArchiveContainerName in ARM
- StorageEndpointSuffix heet ArchiveStorageEndpointSuffix in ARM
- De schakeloptie AutoUpdate is toegevoegd aan de ARM om in te schakelen automatische updates van de extensie handler naar de meest recente versie, aangezien en wanneer deze beschikbaar is. Deze parameter heeft veroorzaken hoofdschema opnieuw is opgestart op de VM wanneer een nieuwe versie van de WMF wordt uitgebracht. 


## <a name="azure-portal-functionality"></a>Azure portal functionaliteit ##
Blader naar een klassieke VM. Klik onder Instellingen -> Algemeen klikt u op "Extensies." Een nieuw deelvenster wordt gemaakt. Klik op 'Toevoegen' en selecteer PowerShell DSC.

De portal moet invoer.
**Configuratie Modules of Script**: dit veld is verplicht. Vereist een .ps1-bestand met een configuratiescript of een ZIP-bestand met een script van de configuratie .ps1 bij de hoofdmap en alle afhankelijke bronnen in de mappen van een module in het zip. Dit kan worden gemaakt met de `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet opgenomen in de SDK van Azure PowerShell. Het ZIP-bestand wordt geüpload naar uw gebruikers-blobopslag beveiligd met een token SA's. 

**Configuratiebestand gegevens PSD1**: dit veld is optioneel. Als uw configuratie een configuratiebestand van de gegevens in .psd1 vereist, gebruikt u dit veld deze te selecteren en uploaden naar uw gebruikers-blobopslag, waar deze is beveiligd met een token SA's. 
 
**Naam van configuratie Module-Qualified**: .ps1 bestanden kunnen meerdere configuratie functies hebben. Voer de naam van de configuratie .ps1 script gevolgd door een '\' en de naam van de functie configuratie. Bijvoorbeeld, als uw script .ps1 heeft de naam 'configuration.ps1', en de configuratie is 'IisInstall', dat u wilt opgeven:`configuration.ps1\IisInstall`

**Argumenten van de configuratie**: als de functie configuratie argumenten duurt, geeft u deze hier in de indeling `argumentName1=value1,argumentName2=value2`. Opmerking dat deze indeling is een andere indeling dan hoe configuratie argumenten worden geaccepteerd via PowerShell-cmdlets of resourcemanager sjablonen. 

## <a name="getting-started"></a>Aan de slag ##

De extensie Azure DSC duurt in DSC configuratiedocumenten en deze op Azure VMs enacts. Een eenvoudig voorbeeld van een configuratie volgt. Dit lokaal opslaan als 'IisInstall.ps1':

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

De volgende stappen uit het script IisInstall.ps1 plaats op de opgegeven VM, de configuratie uitvoeren en te rapporteren terug op status.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Logboekregistratie ##

Logboekbestanden worden opgeslagen:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[versienummer]

## <a name="next-steps"></a>Volgende stappen ##

Voor meer informatie over PowerShell DSC, [gaat u naar het midden van de documentatie PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Bekijk de [resourcemanager Azure-sjabloon voor de extensie DSC](virtual-machines-windows-extensions-dsc-template.md
). 

Als u wilt zoeken naar extra functionaliteit kunt u met PowerShell DSC, [de PowerShell-galerie bladeren](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) naar meer DSC-bronnen beheren.

Zie voor meer informatie over het doorgeven van gevoelige parameters in configuraties, [beheren referenties veilig met de extensie-handler DSC](virtual-machines-windows-extensions-dsc-credentials.md).
