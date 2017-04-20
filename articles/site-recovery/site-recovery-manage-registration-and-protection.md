<properties
    pageTitle="Verwijderen van servers en beveiliging uitschakelen | Microsoft Azure" 
    description="In dit artikel wordt beschreven hoe servers uit een Site herstel kluis registratie en beveiliging voor virtuele machines en fysieke servers uitschakelen." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="remove-servers-and-disable-protection"></a>Verwijderen van servers en beveiliging uitschakelen

De Site herstel van Azure-service draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines en fysieke servers. Computers kunnen worden gerepliceerd naar Azure of naar een secundaire on-premises implementatie-Datacenter. Voor een kort overzicht lezen [Wat Azure Site herstel is?](site-recovery-overview.md)

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe servers van de Site herstel kluis unregister en het uitschakelen van beveiliging voor virtuele machines die zijn beveiligd met sites worden hersteld. 

Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.

## <a name="unregister-a-vmm-server"></a>Een server VMM unregister

U kunt een server VMM uit een kluis unregister door te verwijderen van de server op het tabblad **Servers** in de portal-Site herstel van Azure. Houd rekening met:

-  **Verbonden VMM server**: We raden u de server VMM unregister wanneer deze verbonden met Azure. Dit zorgt ervoor dat instellingen op de on-premises VMM-server en de VMM-servers is gekoppeld aan (VMM-servers met wolken die zijn toegewezen aan wolken op de server die u wilt verwijderen) correct worden opgeschoond. Het is raadzaam om dat u alleen een niet-verbonden server verwijderen als er een probleem met de permanente met connectivity.
- **Niet-verbonden VMM server**: als de server VMM is niet verbonden wanneer u deze verwijderen moet u een script uitvoeren handmatig om het opruimen uitvoeren. Het script is beschikbaar in de [galerie met Microsoft](http://aka.ms/asr-cleanup-script-vmm). Opmerking de VMM-ID van de server het opruimen te voltooien.
- **VMM server in cluster**: als u moet een VMM-server die wordt geïmplementeerd in een cluster unregister het volgende doen:

    - Als de server is verbonden, verwijdert u de verbonden VMM-server op het tabblad **Servers** . Als u wilt de Provider op de server wilt verwijderen, meld u aan op elk clusterknooppunt en verwijdert u deze via het Configuratiescherm. Voer het opruimen script waarnaar wordt verwezen in de vorige sectie op alle passieve knooppunten in het cluster registervermeldingen verwijderen.
    - Als de server is niet verbonden moet u het opruimen script uitvoeren op alle knooppunten.

### <a name="unregister-an-unconnected-vmm-server"></a>Een niet-verbonden VMM server unregister

Klik op de server VMM die u wilt verwijderen:

1. De registratie van de server VMM van de Azure-portal.
2. Download het opruimen-script op de server VMM.
3. Open PowerShell met de uitvoeren als beheerder van optie voor het wijzigen van het beleid kan worden uitgevoerd voor het standaardbereik (LocalMachine).
4. Volg de instructies in het script. 

Op servers VMM met wolken die zijn gekoppeld aan wolken op de server die u wilt verwijderen:

1. Het opruimen script uitvoeren en volgt u stappen 2 tot en met 4.
2. Geef de VMM-ID voor de VMM-server die verwijderd is. 
3. Dit script worden de registratiegegevens voor de VMM-Server en informatie over het koppelen cloud verwijderd.


## <a name="unregister-a-hyper-v-server-in-a-hyper-v-site"></a>Een server Hyper-V in een site Hyper-V unregister

Als herstel van Azure-Site wordt geïmplementeerd op virtuele machines zich bevindt op een server Hyper-V in een site Hyper-V (met geen server VMM) beveiligen kunt u als volgt een Hyper-V server uit een kluis unregister:

1. Beveiliging voor virtuele machines zich bevindt op de server Hyper-V uitschakelen.
2. Selecteer op het tabblad **Servers** in de portal Azure sites worden hersteld, de server > verwijderen. De server hoeft te worden verbonden met Azure wanneer u dit doet.
3. De volgende script uitvoeren om instellingen op de server opschonen en unregister deze uit de kluis. 

        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }
    
            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host
        
            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }
        
            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }
        
            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
        
            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."  
                    Remove-Item -Recurse -Path $registrationPath
                }
    
                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }
    
                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }
    
            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {   
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0] 
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd


## <a name="stop-protecting-a-hyper-v-virtual-machine"></a>Stoppen met het beveiligen van Hyper-V virtuele machines

Als u wilt stoppen met het beveiligen van Hyper-V virtuele machines moet u beveiliging voor deze verwijderen. Afhankelijk van hoe u de beveiliging opheffen mogelijk moet u de instellingen voor documentbeveiliging handmatig op de computer opschonen. 

### <a name="remove-protection"></a>Beveiliging verwijderen

1. In het tabblad **virtuele Machines** van de cloud-eigenschappen en selecteer de virtuele machine > **verwijderen**.
2. Klik op de pagina **Bevestig verwijderen van Virtual Machine** hebt u een paar opties:

    - **Beveiliging uitschakelen**, als u inschakelt en sla deze optie de virtuele machine niet meer worden beveiligd door sites worden hersteld. Instellingen voor documentbeveiliging voor de virtuele machine wordt automatisch worden opgeschoond.
    - **Verwijderen uit de kluis**, als u deze optie alleen uit de kluis Site herstel de virtuele machine worden verwijderd. On-premises implementatie-instellingen voor documentbeveiliging voor de virtuele machine worden niet beïnvloed. U moet opschonen instellingen handmatig verwijderen van instellingen voor documentbeveiliging en de virtuele machine verwijderen uit het Azure abonnement en instellingen voor documentbeveiliging moet u opschonen ze handmatig met behulp van de onderstaande instructies verwijderen.

Als u selecteren om de virtuele machine en de vaste schijven die ze verwijderd uit de doellocatie te verwijderen.

### <a name="clean-up-protection-settings-manually-between-vmm-sites"></a>Instellingen voor documentbeveiliging handmatig (tussen VMM sites) opschonen

Als u **Stoppen met het beheer van de virtuele machine**hebt geselecteerd, handmatig instellingen verwijderen:

1. Klik op de primaire server dit script uitvoeren vanaf de console VMM om de instellingen voor de primaire virtuele machine op te schonen. Klik op de knop PowerShell om de VMM PowerShell-console te openen in de VMM-console. SQLVM1 vervangen door de naam van uw virtuele computer.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Klik op de secundaire VMM-server kunt u dit script om de instellingen voor de secundaire virtuele machine op te schonen uitvoeren:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force

3. Op de secundaire VMM-server, nadat u het script uitvoeren vernieuwt u de virtuele machines op de server van Hyper-V host zodat de secundaire virtuele machine opnieuw in de console VMM wordt gedetecteerd.
4. De bovenstaande stappen wordt de server voor replicatie instellingen VMM alleen gewist. Als u verwijderen VM herhaling voor de virtuele machine wilt. U moet doen van de onderstaande stappen op zowel de primaire en secundaire virtuele machines. Voer de onderstaande script herhaling verwijderen en vervangen van SQLVM1 met de naam van uw virtuele computer.

        Remove-VMReplication –VMName “SQLVM1”


### <a name="clean-up-protection-settings-manually-between-on-premises-vmm-sites-and-azure"></a>Instellingen voor documentbeveiliging handmatig (tussen on-premises implementatie VMM sites en Azure) opschonen

1. Klik op de bronserver VMM kunt u dit script om de instellingen voor de primaire virtuele machine op te schonen uitvoeren:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. De bovenstaande stappen wordt de server voor replicatie instellingen VMM alleen gewist. Zodra u hebt verwijderd zorgen de replicatie van VMM server als u wilt verwijderen, herhaling voor de virtuele machine op de server Hyper-V host met dit script. SQLVM1 vervangen door de naam van uw virtuele machine en host01.contoso.com met de naam van de Hyper-V host-server.

        $vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

### <a name="clean-up-protection-settings-manually-between-hyper-v-sites-and-azure"></a>Instellingen voor documentbeveiliging handmatig (tussen Hyper-V sites en Azure) opschonen

1. Klik op de Hyper-V host bronserver, als u wilt verwijderen herhaling voor de virtuele machine dit script gebruiken. SQLVM1 vervangen door de naam van uw virtuele computer.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

## <a name="stop-protecting-a-vmware-virtual-machine-or-a-physical-server"></a>Stoppen met het beveiligen van een virtuele VMware machine of een fysieke server

Als u wilt stoppen met het beveiligen van een virtuele VMware machine of een fysieke server moet u beveiliging voor deze verwijderen. Afhankelijk van hoe u de beveiliging opheffen mogelijk moet u de instellingen voor documentbeveiliging handmatig op de computer opschonen. 

### <a name="remove-protection"></a>Beveiliging verwijderen

1. In het tabblad **virtuele Machines** van de cloud-eigenschappen en selecteer de virtuele machine > **verwijderen**.
2. Selecteer een van de opties op de **virtuele Machine verwijderen** :

    - **Uitschakelen beveiliging (gebruik voor herstel meer details en volume formaat wijzigen)**, u alleen ziet en schakel deze optie als u hebt:
        - **Het volume VM resized**, wanneer u het formaat van een volume de virtuele machine in een kritieke toestand terechtkomt. Selecteer deze optie als dit gebeurt. Beveiliging en herstel punten in Azure behoudt wordt uitgeschakeld. Wanneer u beveiliging voor de computer opnieuw inschakelen worden de gegevens voor het volume van het formaat is gewijzigd worden overgebracht naar Azure.
        - **Een failover uitvoeren**, nadat u uw omgeving getest hebt door het uitvoeren van een failover van on-premises implementatie VMware virtuele machines of fysieke servers naar Azure, selecteer deze optie om het beveiligen van uw on-premises implementatie virtuele machines opnieuw starten. Deze optie uitschakelt elke virtuele machine en vervolgens moet u beveiliging voor hen inschakelen. Houd rekening met:
            - De virtuele machine met deze instelling uitschakelen, heeft dit geen invloed op de replica virtuele machine in Azure wordt aangegeven.
            - U kunt de mobiliteit-service kan niet verwijderen uit de virtuele machine.
    
    - **Beveiliging uitschakelen**, als u inschakelt en sla deze optie de computer niet meer worden beveiligd door sites worden hersteld. Instellingen voor documentbeveiliging voor de computer, wordt automatisch worden opgeschoond.
    - **Verwijderen uit de kluis**, als u deze optie alleen uit de kluis Site herstel de computer worden verwijderd. On-premises implementatie-instellingen voor documentbeveiliging voor de computer worden niet beïnvloed. Instellingen op de computer verwijderen en de virtuele machine verwijderen uit de Azure moet-abonnement en u de instellingen opschonen door de service mobiliteit ongedaan te maken.
    
        ![Opties voor verwijderen](./media/site-recovery-manage-registration-and-protection/remove-vm.png)

















