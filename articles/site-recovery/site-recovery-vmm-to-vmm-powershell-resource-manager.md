<properties
    pageTitle="Hyper-V virtuele machines in VMM wolken repliceren naar een secundaire VMM-site via PowerShell (resourcemanager) | Microsoft Azure"
    description="Wordt beschreven hoe u het implementeren van Azure Site herstellen om de goedkeuringen herhaling, failover en herstel van Hyper-V VMs in VMM wolken naar een secundaire VMM-site via PowerShell (resourcemanager)"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Hyper-V virtuele machines in VMM wolken repliceren naar een secundaire VMM-site via PowerShell (resourcemanager)

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-vmm.md)
- [Klassieke Portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - resourcemanager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Welkom bij Azure Site herstel! In dit artikel als u wilt repliceren on-premises implementatie Hyper-V virtuele machines beheerd in System Center Virtual Machine Manager (VMM) wolken aan een secundaire site wordt gebruikt. 

In dit artikel leest u hoe u algemene taken die u uitvoeren moet wanneer u Azure Site herstel Hyper-V virtuele machines in System Center VMM wolken gerepliceerd naar System Center VMM wolken in secundaire site instelt automatiseren met PowerShell.

Lees het artikel vereisten voor het scenario bevat, en ziet u 

- Het instellen van een herstel Services kluis
- De Provider van Azure Site herstel installeren op de bronserver VMM en de doelserver VMM
- De VMM (s) in de kluis registreren
- Replicatiebeleid configureren voor de Cloud VMM. De replicatie-instellingen in het beleid wordt toegepast op alle beveiligde virtuele machines 
- Beveiliging voor de virtuele machines inschakelen. 
- Test de overname van VMs afzonderlijk of als onderdeel van een abonnement herstellen om ervoor te zorgen dat alles werkt zoals verwacht.
- Voer een geplande of een ongeplande overname van VMs afzonderlijk of als onderdeel van een abonnement herstellen om ervoor te zorgen dat alles werkt zoals verwacht.

Als u dit scenario instellen problemen ondervindt, kunt u uw vragen posten op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: Azure resourcemanager en klassiek. Azure, heeft ook twee portals – de Azure klassieke portal die ondersteuning biedt voor het implementatiemodel klassieke en de Azure-portal met ondersteuning voor beide implementatiemodellen. In dit artikel worden de resourcemanager implementatiemodel.



## <a name="on-premises-prerequisites"></a>Vereisten voor on-premises implementatie

Hier volgt wat u nodig hebt in de primaire en secundaire on-premises implementatie-sites om te implementeren in dit scenario:

**Vereisten voor** | **Meer informatie** 
--- | ---
**VMM** | Het is raadzaam om de implementatie van een server VMM in de primaire-site en een VMM in de secundaire site.<br/><br/> U kunt ook [repliceren tussen wolken op één VMM server](site-recovery-single-vmm.md). Hiervoor moet u ten minste twee wolken geconfigureerd op de server VMM.<br/><br/> VMM servers ten minste moeten worden uitgevoerd systeem Center 2012 SP1 met de meest recente updates.<br/><br/> Elke VMM-server moet op een of meer wolken geconfigureerd en alle wolken moet de capaciteit van Hyper-V-profiel instellen. <br/><br/>Wolken moeten een of meer VMM hostgroepen bevatten.<br/><br/>Meer informatie over het instellen van VMM wolken bij het [configureren van de VMM cloud stof](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), en [Stapsgewijze instructies: privé wolken maken met System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM servers moeten beschikken over internettoegang. 
**Hyper-V** | Hyper-V servers ten minste moeten worden uitgevoerd Windows Server 2012 met de functie Hyper-V en de meest recente updates hebt geïnstalleerd.<br/><br/> Een server Hyper-V moet een of meer VMs bevatten.<br/><br/>  Hyper-V hostservers zich bevinden in hostgroepen in de primaire en secundaire VMM wolken.<br/><br/> Als u Hyper-V in een cluster in Windows Server 2012 R2 uitvoert moet u installeren [2961977 bijwerken](https://support.microsoft.com/kb/2961977)<br/><br/> Als u werkt met Hyper-V in een cluster op Windows Server 2012 notitie die makelaar cluster wordt niet automatisch gemaakt als u een statische IP-adres gebaseerde cluster hebt. U moet de makelaar cluster handmatig configureren. [Meer informatie](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Provider** | Tijdens de implementatie van de Site herstel kunt u de Provider van Azure Site herstel installeren op VMM-servers. De Provider communiceert met Site-herstel via HTTPS 443 goedkeuringen herhaling. Replicatie gegevens tussen de primaire en secundaire Hyper-V servers via het LAN of een VPN-verbinding.<br/><br/> De Provider die wordt uitgevoerd op de server VMM nodig heeft toegang tot deze URL's: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Daarnaast kunt firewall communicatie van de VMM-servers met de [Azure datacenter IP-bereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653) en toestaan de (443) HTTPS-protocol.

### <a name="network-mapping-prerequisites"></a>Vereisten voor netwerk-toewijzing
Netwerk toewijzing kaarten tussen VMM VM netwerken op de primaire en secundaire VMM servers:

- Plaats optimaal replica VMs op secundaire Hyper-V hosts na failover.
- Replica VMs verbinden met juiste VM-netwerken te gebruiken.
- Als u geen netwerk toewijzing replica die VMS niet wordt aangesloten op een netwerk na failover configureren.
- Als u wilt netwerk instellen is tijdens het herstellen van de Site toewijzing hier implementatie wat u nodig hebt:

    - Zorg ervoor dat VMs op de bronserver voor de host van Hyper-V zijn verbonden met een netwerk VMM VM. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
    - Controleer of de secundaire cloud die u voor herstel gebruikt heeft een bijbehorende VM netwerk is geconfigureerd. VM netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de secundaire cloud.


Meer informatie over het configureren van VMM-netwerken te gebruiken in de onderstaande artikelen

- [Logische netwerken configureren in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [VM netwerken en gateways configureren in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Meer informatie](site-recovery-network-mapping.md) over de werking van netwerk-toewijzing.

###<a name="powershell-prerequisites"></a>PowerShell vereisten
Zorg ervoor dat er Azure PowerShell wilt verzenden. Als u al PowerShell gebruikt, moet u een upgrade uitvoeren naar versie 0.8.10 of hoger. Zie de [handleiding voor het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor informatie over het instellen van PowerShell. Als u hebt ingesteld en PowerShell geconfigureerd, kunt u alle de beschikbaar cmdlets voor de service bekijken [hier](https://msdn.microsoft.com/library/dn850420.aspx). 

Zie de [handleiding aan de slag met Azure cmdlets voor](https://msdn.microsoft.com/library/azure/jj554332.aspx)meer informatie over tips waarmee u kunnen dat u de cmdlets, zoals hoe parameterwaarden, invoer en uitvoer meestal worden verwerkt in Azure PowerShell gebruiken.

## <a name="step-1-set-the-subscription"></a>Stap 1: Het abonnement instellen 

1. Van Azure powershell, meld u aan bij uw Azure-account: Gebruik de volgende cmdlets
 
        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred 
    

2. Een lijst met uw abonnementen ophalen. Dit wordt ook een lijst met de subscriptionIDs voor elk van de abonnementen. Noteer de subscriptionID van het abonnement waarin u wilt maken van het herstelproces is services kluis    

        Get-AzureRmSubscription 

3. Het abonnement waarin de herstel services kluis gemaakt wordt door de vermelding van de abonnements-ID instellen

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Stap 2: Maak een kluis herstel Services 

1. Een resourcegroep Azure resourcemanager maken als u nog niet hebt

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Een nieuwe herstel Services kluis maken en opslaan van het gemaakte ASR kluis object in een variabele (wordt later gebruikt). U kunt ook het ASR kluis object bericht maken met de cmdlet Get-AzureRMRecoveryServicesVault ophalen:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>Stap 3: Stel de context herstel Services kluis

1.  Als er een kluis al hebt gemaakt, voert u het onder de opdracht om de kluis.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  De context kluis instellen door te voeren het onder de opdracht.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault



## <a name="step-4-install-the-azure-site-recovery-provider"></a>Stap 4: De Provider van Azure Site herstel installeren

1.  Op de computer VMM, moet u een map maken door de volgende opdracht uit te voeren:
    
        New-Item c:\ASR -type directory
        
2.  Pak de bestanden met de gedownloade provider door de volgende opdracht uit te voeren
    
        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q

    
3.  Installeer de provider met de volgende opdrachten:
    
        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Wacht totdat de installatie om te voltooien.
    
4.  De server in het gebruik van de volgende opdracht uit kluis registreren:
    
        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-and-associate-a-replication-policy"></a>Stap 5: Maken en een beleid herhaling koppelen

1.  Hyper-V 2012 R2 replicatie-beleid maken door de volgende opdracht uit te voeren:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] De cloud VMM Hyper-V hosts met verschillende versies van Windows Server (zoals die worden genoemd in de vereisten van Hyper-V) bevatten, maar het replicatiebeleid is OS versie specifieke. Hebt u verschillende hosts zich in verschillende versies besturingssystemen, klikt u vervolgens afzonderlijk herhaling beleidsregels voor elk type OS versie te maken. Voor bijvoorbeeld: als u vijf hosts uitgevoerd op Windows-Servers 2012 en drie op Windows Server 2012 R2 hebt, maakt u twee herhaling beleid – een handtekening voor elk type versies van besturingssystemen.

2.  Lees het primaire beveiliging container (primaire VMM Cloud) en herstel beveiliging container (herstel VMM Cloud) door de volgende opdrachten uit te voeren:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  Het beleid dat u hebt gemaakt in stap 1 met de beschrijvende naam van het beleid ophalen

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Start de koppeling van de container beveiliging (VMM Cloud) met het beleid herhaling:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Wacht totdat het beleid koppeling taak is voltooid. U kunt controleren als de taak is voltooid, gebruikt het volgende PowerShell-fragment.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    Nadat de taak zijn verwerkt, voert u de volgende opdracht uit:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


Volg de stappen in de [Activiteiten controleren](#monitor)om te controleren of de voltooiing van de bewerking.

## <a name="step-5-configure-network-mapping"></a>Stap 5: Netwerk toewijzing configureren

1. De eerste opdracht krijgt servers voor de huidige Site herstel van Azure kluis. De opdracht opgeslagen de servers van Microsoft Azure Site herstellen in de matrix $Servers variabele.

        $Servers = Get-AzureRmSiteRecoveryServer

2. De onder opdrachten de site herstel netwerk voor de bronserver VMM en de doelserver VMM ophalen.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] De bronserver VMM is op de eerste fase of de tweede rij in de matrix servers. De namen van de servers VMM en de netwerken correct verkrijgen


4. De cmdlet definitief Hiermee maakt u een toewijzing tussen het primaire netwerk en het herstelproces-netwerk. De cmdlet Hiermee geeft u het primaire netwerk als het eerste element van $PrimaryNetworks en het netwerk herstel als het eerste element van $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Stap 6: Opslag toewijzing configureren

1. Het onder de opdracht krijgt u de lijst met opslag classificaties in $storageclassifications variabele.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. De onderstaande opdrachten krijgen de bron-indeling in de variabele en doelsites indeling $SourceClassificaion in $TargetClassification variabele. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] De bronsite en doelsites classificaties mag elk element in de matrix. Verwijzen naar de uitvoer van het onder de opdracht om te bepalen de index van de bronsite en doelsites classificaties in $storageclassifications matrix. 
    
    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object - eigenschap FriendlyName, -Id | Tabel opmaken


3. De onderstaande cmdlet Hiermee maakt u een koppeling tussen de bron-indeling en de indeling doellijst. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>Stap 7: Beveiliging voor virtuele machines inschakelen

Nadat de servers, wolken en netwerken correct zijn geconfigureerd, kunt u beveiliging voor virtuele machines in de cloud. 


  1. Als u beveiliging, voert u de volgende opdracht uit om de container beveiliging:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. Krijg de beveiliging entity (VM) door de volgende opdracht uit te voeren:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. Replicatie voor VM inschakelen door de volgende opdracht uit te voeren:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>Test uw implementatie

Test uw implementatie kunt u uitvoeren van een failover test voor een enkele virtuele machine, of een plan voor herstel die bestaat uit meerdere virtuele machines maken en uitvoeren van een failover test voor het abonnement. Test failover simuleert uw failover- en herstelbestanden om in een geïsoleerd netwerk. 

> [AZURE.NOTE] U kunt een plan voor herstel maken voor uw toepassing in Azure-portal.

Volg de stappen in de [Activiteiten controleren](#monitor)om te controleren of de voltooiing van de bewerking.


### <a name="run-a-test-failover"></a>Een failover test uitvoeren

1.  Voer de onderstaande cmdlets om de VM netwerk waarnaar u wilt testen failover uw VMs aan.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Een test overname van een VM uitvoeren door het volgende te doen:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Een test overname van een abonnement herstel uitvoeren door het volgende te doen:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Een geplande failover uitvoeren

1. Een geplande overname van een VM uitvoeren door het volgende te doen:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Een geplande overname van een abonnement herstel uitvoeren door het volgende te doen:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

1. Een ongeplande overname van een VM uitvoeren door het volgende te doen:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2. voert u een niet-geplande overname van een abonnement herstel als volgt:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
## <a name=monitor></a>Activiteiten controleren

De volgende opdrachten gebruiken om de van de activiteit te houden. Houd er rekening mee dat er moet wachten tussen taken voor de verwerking om te voltooien.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Volgende stappen

[Lees meer](https://msdn.microsoft.com/library/azure/mt637930.aspx) over het herstellen van Azure-Site met Azure resourcemanager PowerShell-cmdlets.

