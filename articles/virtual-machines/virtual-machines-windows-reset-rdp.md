<properties
    pageTitle="Opnieuw het wachtwoord of extern bureaublad configuratie op een Windows-VM | Microsoft Azure"
    description="Informatie over het wachtwoord van een account of extern bureaublad-services op een Windows-VM via de portal van Azure of Azure PowerShell opnieuw in te stellen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Het opnieuw instellen van de service Extern bureaublad of het wachtwoord voor aanmelden bij in een Windows-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Als u geen verbinding maken met een Windows virtuele machine (VM), kunt u de lokale beheerderswachtwoord opnieuw instellen of opnieuw instellen van de configuratie van de service Extern bureaublad. U kunt de portal van Azure of de uitbreiding VM toegang tot in Azure PowerShell het wachtwoord opnieuw instellen. Als u PowerShell gebruikt, moet u de meest recente PowerShell-module geïnstalleerd op uw werkcomputer hebt en u bent aangemeld bij uw Azure-abonnement. Lees voor gedetailleerde stappen voor [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

> [AZURE.TIP] U kunt de versie van PowerShell die u hebt geïnstalleerd via controleren`Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version`

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Windows-VMs in resourcemanager implementatiemodel

### <a name="azure-portal"></a>Azure-Portal
Selecteer uw VM klikt u op **Bladeren** > **virtuele machines** > *virtuele uw Windows-computer* > **alle instellingen** > **wachtwoord opnieuw instellen**. Het wachtwoord opnieuw instellen blad wordt weergegeven:

![Pagina voor wachtwoordherstel](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Voer de gebruikersnaam en een nieuw wachtwoord en klik op **Opslaan**. Probeer nogmaals verbinding te maken voor uw VM.

### <a name="vmaccess-extension-and-powershell"></a>VMAccess extensie en PowerShell

Controleer of u Azure PowerShell 1.0 of hoger is geïnstalleerd en u hebt aangemeld bij uw account gebruiken de `Login-AzureRmAccount` cmdlet.

#### <a name="reset-the-local-administrator-account-password"></a>**De lokale account beheerderswachtwoord opnieuw instellen**

U kunt de naam van de beheerder wachtwoord of gebruikers herstellen met behulp van de [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShell-opdracht.

Maak de beheerder van uw lokale accountreferenties met behulp van de volgende opdracht uit:

    $cred=Get-Credential

Als u een andere naam dan de huidige account typt, de volgende opdracht uit VMAccess extensie wijzigt de naam van het lokale beheerdersaccount het wachtwoord aan dat account wordt toegewezen en problemen met een extern bureaublad afmelden gebeurtenis. Als het lokale beheerdersaccount is uitgeschakeld, de extensie VMAccess dit heeft ingeschakeld.

Gebruik de uitbreiding VM toegang tot de nieuwe referenties als volgt instellen:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Vervang `myRG`, `myVM`, `myVMAccess`, en de locatie met waarden die relevant zijn voor de configuratie.


#### <a name="reset-the-remote-desktop-service-configuration"></a>**Beginwaarden van de configuratie van de service Extern bureaublad**

U kunt externe toegang voor uw VM herstellen door de [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) of [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx), als volgt gebruiken. (Vervangen de `myRG`, `myVM`, `myVMAccess` en de locatie met uw eigen waarden.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Of:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Een nieuwe benoemde VM access agent toevoegen beide opdrachten aan de virtuele machine. Een VM kan slechts één VM access agent van een hebben op elk gewenst moment. De toegang VM Agenteigenschappen ingesteld succes verwijderen de eerder instellen met behulp van een access-agent `Remove-AzureRmVMAccessExtension` of `Remove-AzureRmVMExtension`. Azure PowerShell versie 1.2.2 vanaf, kunt u deze stap voorkomen bij gebruik van `Set-AzureRmVMExtension` met een `-ForceRerun` optie. Bij gebruik van `-ForceRerun`, zorg ervoor dat dezelfde naam voor de access-agent VM gebruiken als instellen door de vorige opdracht.

Als u nog steeds geen verbinding extern naar uw virtuele computer, raadpleegt u aanvullende stappen om te proberen bij [problemen met extern bureaublad-verbindingen met een Windows Azure virtuele machine](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="windows-vms-in-the-classic-deployment-model"></a>Windows-VMs in het implementatiemodel klassieke

### <a name="azure-portal"></a>Azure-portal

Voor virtuele machines gemaakt met behulp van het implementatiemodel klassieke, kunt u de [portal van Azure](https://portal.azure.com) opnieuw instellen van de service Extern bureaublad. Klik op: **Blader** > **virtuele machines (klassieke)** > *virtuele uw Windows-computer* > **Opnieuw externe...**. De volgende pagina wordt weergegeven.

![RDP configuratiepagina herstellen](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

U kunt ook proberen opnieuw instellen van de naam en het wachtwoord van het lokale beheerdersaccount. Klik op: **Blader** > **virtuele machines (klassieke)** > *virtuele uw Windows-computer* > **alle instellingen** > **wachtwoord opnieuw instellen**. De volgende pagina wordt weergegeven.

![Pagina voor wachtwoordherstel](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Nadat u de nieuwe gebruikersnaam en wachtwoord hebt ingevoerd, klikt u op **Opslaan**.

### <a name="vmaccess-extension-and-powershell"></a>VMAccess extensie en PowerShell

Zorg ervoor dat de VM-Agent is geïnstalleerd op de virtuele machine. De extensie VMAccess hoeft te worden geïnstalleerd voordat u gebruiken kunt, zolang de VM-Agent beschikbaar is. Controleer of de VM-Agent al is geïnstalleerd met behulp van de volgende opdracht uit. (Vervang 'myCloudService' en 'myVM' door de namen van uw cloudservice en uw VM, respectievelijk. U kunt deze namen meer door te voeren `Get-AzureVM` zonder parameters.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Als de opdracht **schrijven-host** wordt weergegeven **waar**, wordt de VM-Agent is geïnstalleerd. Als dit wordt **Onwaar**weergegeven, raadpleegt u de instructies en een koppeling naar het downloaden in de [VM-Agent en uitbreidingen - deel 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blogbericht.

Als u de virtuele machine gemaakt met behulp van de portal, controleert u of `$vm.GetInstance().ProvisionGuestAgent` retourneert **True**. Als dat niet zo is, kunt u deze instellen met behulp van deze opdracht:

    $vm.GetInstance().ProvisionGuestAgent = $true

Deze opdracht zorgt ervoor dat het volgende foutbericht weergegeven wanneer u de opdracht **Set-AzureVMExtension** in de volgende stappen uitvoert: "Inrichten Gast Agent moet zijn ingeschakeld op het object VM voordat u de uitbreiding van de toegang tot IaaS VM."

#### <a name="reset-the-local-administrator-account-password"></a>**De lokale account beheerderswachtwoord opnieuw instellen**

Een referentie aanmelden met de naam van de huidige lokale beheerder-account en een nieuw wachtwoord maken en voer vervolgens de `Set-AzureVMAccessExtension` als volgt.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Als u een andere naam dan de huidige account typt, de extensie VMAccess wijzigt de naam van het lokale beheerdersaccount het wachtwoord aan dat account wordt toegewezen en problemen met een extern bureaublad afmelden. Als het lokale beheerdersaccount is uitgeschakeld, de extensie VMAccess dit heeft ingeschakeld.

Deze opdrachten ook opnieuw instellen van de configuratie van de service Extern bureaublad.

#### <a name="reset-the-remote-desktop-service-configuration"></a>**Beginwaarden van de configuratie van de service Extern bureaublad**

Als u de configuratie van de service Extern bureaublad herstellen, voert u de volgende opdracht uit:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

De extensie VMAccess twee opdrachten op de virtuele machine wordt uitgevoerd:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

Deze opdracht kunt de ingebouwde Windows Firewall-groep waarmee binnenkomende extern bureaublad verkeer, waarin TCP-poort 3389.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

Deze opdracht wordt de fDenyTSConnections registerwaarde ingesteld op 0, extern bureaublad verbindingen inschakelen.


## <a name="next-steps"></a>Volgende stappen

Als de uitbreiding van de toegang tot Azure VM niet reageert en u niet kunt u het wachtwoord opnieuw instellen, kunt u [het lokale Windows-wachtwoord offline opnieuw instellen](virtual-machines-windows-reset-local-password-without-agent.md). Deze methode is een geavanceerdere proces en moet u de virtuele harde schijf van de problematisch VM verbinden met een andere VM. Volg de stappen in dit artikel beschreven eerst en probeert u alleen de methode voor het opnieuw instellen van offline wachtwoord noodgevallen.

[Azure VM extensions en functies](virtual-machines-windows-extensions-features.md)

[Verbinding maken met een Azure virtuele machines met RDP of SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problemen met extern bureaublad-verbindingen met een Windows Azure virtuele machine](virtual-machines-windows-troubleshoot-rdp-connection.md)
