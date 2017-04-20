<properties
    pageTitle="Repliceren Hyper-V virtuele machines in VMM wolken met Azure Site herstel en PowerShell (resourcemanager) | Microsoft Azure"
    description="Repliceren Hyper-V virtuele machines in VMM wolken met Azure Site herstel en PowerShell"
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="rajanaki"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>Hyper-V virtuele machines in VMM wolken repliceren naar Azure PowerShell en Azure Resource Manager gebruiken

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - resourcemanager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassieke Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - klassiek](site-recovery-deploy-with-powershell.md)



## <a name="overview"></a>Overzicht

Azure Site herstel draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines in een aantal scenario's voor implementatie. Zie [overzicht van de Azure Site herstel](site-recovery-overview.md)voor een volledige lijst met scenario's voor implementatie.

In dit artikel leest u hoe u algemene taken die u uitvoeren wilt bij het instellen van Azure Site herstel Hyper-V virtuele machines in System Center VMM wolken repliceren met Azure storage automatiseren met PowerShell.

Lees het artikel vereisten voor het scenario bevat, en ziet u

- Het instellen van een herstel Services kluis
- De Provider van Azure Site herstel installeren op de bronserver VMM
- De server in de kluis registreren, een Azure opslag-account toevoegen
- Installeren van de Azure herstel Services-agent op Hyper-V hostservers
- Instellingen voor documentbeveiliging voor VMM wolken, die wordt toegepast op alle beveiligde virtuele machines configureren
- Beveiliging voor deze virtuele machines inschakelen.
- Test de storing om ervoor te zorgen dat alles werkt zoals verwacht.

Als u dit scenario instellen problemen ondervindt, kunt u uw vragen posten op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [resourcemanager en klassiek](../resource-manager-deployment-model.md). In dit artikel beschreven hoe u met het implementatiemodel resourcemanager.

## <a name="before-you-start"></a>Voordat u begint

Controleer of dat u deze vereisten hebt geplaatst:

### <a name="azure-prerequisites"></a>Azure vereisten

- U moet een [Microsoft Azure](https://azure.microsoft.com/) -account. Als u deze niet hebt, kunt u beginnen met een [gratis-account](https://azure.microsoft.com/free). Bovendien kunt u lezen over het [herstellen van Azure sitebeheerder prijzen](https://azure.microsoft.com/pricing/details/site-recovery/).
- U moet een CSP-abonnement als u de replicatie een scenario voor het abonnement van CSP uitprobeert. Meer informatie over het programma CSP in [hoe u zich kunt inschrijven in het programma CSP](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
- U moet een account van de Azure v2 opslag (resourcemanager) voor de opslag van gegevens die worden gerepliceerd naar Azure. Het account moet geografische-replicatie is ingeschakeld. Er moet worden in hetzelfde gebied, als de Site herstel van Azure-service en worden gekoppeld aan hetzelfde abonnement of het abonnement CSP. Meer informatie over het instellen van Azure opslag, raadpleegt u de [Inleiding tot Microsoft Azure Storage](../storage/storage-introduction.md) voor verwijzing.
- U moet om ervoor te zorgen dat virtuele machines die u wilt beveiligen voldoen aan de [vereisten van Azure virtuele machines](site-recovery-best-practices.md#azure-virtual-machine-requirements).

> [AZURE.NOTE] Er zijn momenteel alleen VM niveau bewerkingen mogelijke via Powershell. Ondersteuning voor niveau bewerkingen voor herstel-abonnement wordt binnenkort worden aangebracht.  Nu zijn u beperkt tot het uitvoeren van fail-foutherstel alleen een granulatie 'beveiligde VM' en niet naar een niveau herstel plannen.

### <a name="vmm-prerequisites"></a>VMM vereisten
- U moet uitvoeren op systeem Center 2012 R2 VMM-server.
- Een virtuele machines die u wilt beveiligen met VMM-server moet worden uitgevoerd als de Provider herstel van Azure-Site. Dit is geïnstalleerd tijdens de implementatie van Azure sites worden hersteld.
- U moet ten minste één cloud op de VMM-server die u wilt beveiligen. De cloud moet bevatten:
    - Een of meer VMM hostgroepen.
    - Een of meer Hyper-V hostservers of clusters in elke hostgroep.
    - Een of meer virtuele machines op de bronserver Hyper-V.
- Meer informatie over het instellen van VMM wolken:
    - Lees meer over privé VMM wolken in de [nieuwe functies in de Cloud privé met System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) en in [VMM 2012 en de wolken](http://go.microsoft.com/fwlink/?LinkId=324956).
    - Meer informatie over het [configureren van de VMM cloud-structuur](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
    - Wanneer uw cloud stof elementen voldaan informatie over het maken van persoonlijke wolken bij het [maken van een privé cloud in VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) en in [Stapsgewijze instructies: privé wolken maken met System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).


### <a name="hyper-v-prerequisites"></a>Vereisten voor Hyper-V

- De Hyper-V-hostservers ten minste moeten worden uitgevoerd met de functie Hyper-V of **Microsoft Hyper-V Server 2012** , **Windows Server 2012** en de meest recente updates hebt geïnstalleerd.
- Als u werkt met Hyper-V in een notitie cluster die makelaar cluster wordt niet automatisch gemaakt als u een statische IP-adres gebaseerde cluster hebt. U moet de makelaar cluster handmatig configureren. Voor
- Zie voor instructies [hoe u configureren Hyper-V Replica makelaar](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
- Alle Hyper-V hostserver of cluster waarvoor u wilt beheren beveiliging moet worden opgenomen in een wolk VMM.

### <a name="network-mapping-prerequisites"></a>Vereisten voor netwerk-toewijzing
Wanneer u virtuele machines in Azure wordt aangegeven, het netwerk toewijzing kaarten de virtuele machine op de bronserver VMM netwerken beveiligen en afstemmen van Azure-netwerken te gebruiken om in te schakelen het volgende:

- Alle computers die niet op in hetzelfde netwerk kunnen koppelen aan elkaar grenzen, ongeacht welk abonnement herstel ze zijn in.
- Als een netwerkgateway ingesteld op het doel Azure netwerk is, worden virtuele machines kunt koppelen aan andere on-premises implementatie virtuele machines.
- Als u het netwerk toewijzing niet configureert, kunnen alleen de virtuele machines die in hetzelfde herstelplan mislukken met elkaar verbinden na storing naar Azure.

Als u wilt implementeren netwerk toewijzing hebt u het volgende nodig:

- De virtuele machines die u wilt beveiligen op de bronserver VMM moeten zijn verbonden met een netwerk VM. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
- Een Azure netwerk waarmee gerepliceerde virtuele machines verbinding na storing maken kunt. U kunt dit netwerk selecteren op het moment van storing. Het netwerk moet in hetzelfde gebied, als uw abonnement Azure sites worden hersteld.

Meer informatie over het toewijzen van netwerk in

- [Logische netwerken configureren in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [VM netwerken en gateways configureren in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
- [Het configureren en controleren van virtuele netwerken in Azure wordt aangegeven](https://azure.microsoft.com/documentation/services/virtual-network/)


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

1. Een resourcegroep maken in Azure resourcemanager als u nog niet hebt

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Een nieuwe herstel Services kluis maken en opslaan van het gemaakte ASR kluis object in een variabele (wordt later gebruikt). U kunt ook het ASR kluis object bericht maken met de cmdlet Get-AzureRMRecoveryServicesVault ophalen:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Stap 3: Stel de context herstel Services kluis

1.  De context kluis instellen door te voeren het onder de opdracht.

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


## <a name="step-5-create-an-azure-storage-account"></a>Stap 5: Een Azure opslag-account maken

1. Als u niet een Azure opslag-account hebt, maakt u een account geografische-replicatie is ingeschakeld in de geografische hetzelfde als de kluis door de volgende opdracht uit te voeren:

        $StorageAccountName = "teststorageacc1" #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"  
        $ResourceGroupName =  “myRG”            #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Houd er rekening mee dat het opslag-account moet in hetzelfde gebied, als de Site herstel van Azure-service worden en gekoppeld aan hetzelfde abonnement zijn.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Stap 6: De Agent van de Services Azure herstel installeren

1. De agent Azure herstel Services bij [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) Download en installeer het op elke Hyper-V host-server die zich bevindt in de VMM wolken die u wilt beveiligen.

2. Voer de volgende opdracht uit op alle VMM hosts:

    marsagentinstaller.exe/q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>Stap 7: Configureren cloud instellingen voor documentbeveiliging

1.  Maak een beleid herhaling naar Azure door de volgende opdracht uit te voeren:


        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

2.  Krijg een container beveiliging door de volgende opdrachten uit te voeren:

        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

3.  Informatie over de beleid ophalen naar een variabele via de taak die is gemaakt en de beschrijvende beleidsnaam vermelden:

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Start de koppeling van de container beveiliging met het beleid herhaling:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  

5.  Nadat de taak is voltooid, voert u de volgende opdracht uit:

        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
        $isJobLeftForProcessing = $true;
        }

6.  Nadat de taak zijn verwerkt, voert u de volgende opdracht uit:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)

Volg de stappen in de [Activiteiten controleren](#monitor)om te controleren of de voltooiing van de bewerking.

## <a name="step-8-configure-network-mapping"></a>Stap 8: Netwerk toewijzing configureren

Voordat u begint of er netwerk toewijzing virtuele machines op de bronserver VMM verbonden bent met een netwerk VM. Daarnaast een of meer Azure virtuele netwerken maken.

Meer informatie over het maken van een virtueel netwerk Azure resourcemanager en PowerShell gebruiken in [een virtueel netwerk met een VPN-verbinding van het site-naar-site met Azure resourcemanager en PowerShell maken](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Houd er rekening mee dat meerdere VM-netwerken kunnen worden toegewezen aan één Azure netwerk. Als het doelnetwerk meerdere subnetten heeft en een van deze subnetten heeft dezelfde naam als subnet waarop de bron virtuele machine zich bevindt, worden klikt u vervolgens de replica virtuele machine verbonden met dat doel subnet na storing. Als er geen subnet doellijst met een overeenkomende naam, wordt de virtuele machine niet worden verbonden met het eerste subnet in het netwerk.

1. De eerste opdracht krijgt servers voor de huidige Site herstel van Azure kluis. De opdracht opgeslagen de servers van Microsoft Azure Site herstellen in de matrix $Servers variabele.

        $Servers = Get-AzureRmSiteRecoveryServer

2. De tweede opdracht krijgt de site herstel netwerk voor de eerste server in de matrix $Servers. De opdracht opgeslagen de netwerken in de variabele $Networks.


        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

3. De derde opdracht krijgt Azure virtuele netwerken, waarna deze waarde in de variabele $AzureVmNetworks.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork

4. De cmdlet definitief Hiermee maakt u een toewijzing tussen het primaire netwerk en het netwerk Azure virtuele machines. De cmdlet Hiermee geeft u het primaire netwerk als het eerste element van $Networks. De cmdlet Hiermee geeft u een netwerk met een virtuele machine als het eerste element van $AzureVmNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]


## <a name="step-9-enable-protection-for-virtual-machines"></a>Stap 9: Beveiliging voor virtuele machines inschakelen

Nadat de servers, wolken en netwerken correct zijn geconfigureerd, kunt u beveiliging voor virtuele machines in de cloud.

 Houd rekening met het volgende:

 - Virtuele machines moet voldoen aan vereisten voor Azure. Controleer de volgende [vereisten en ondersteuning](site-recovery-best-practices.md) in de Planningshandleiding.

 - Als u wilt inschakelen, beveiliging, het besturingssysteem en besturingssysteem moeten schijfeigenschappen worden ingesteld voor de virtuele machine. Wanneer u een virtuele machine maakt in VMM met een sjabloon VM kunt u de eigenschap instellen. U kunt ook deze eigenschappen voor bestaande virtuele machines instellen op de tabbladen **Algemeen** en **Hardwareconfiguratie** VM eigenschappen. Als u niet deze eigenschappen in VMM instellen kunt u zult kunt u deze in de portal-Site herstel van Azure configureren.


  1. Als u beveiliging, voert u de volgende opdracht uit om de container beveiliging:

            $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName

  2. Krijg de beveiliging entity (VM) door de volgende opdracht uit te voeren:

            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer

  3. De DR voor VM inschakelen door de volgende opdracht uit te voeren:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1



## <a name="test-your-deployment"></a>Test uw implementatie

Test uw implementatie kunt u uitvoeren van een test storing voor een enkele virtuele machine, of een plan voor herstel die bestaat uit meerdere virtuele machines maken en uitvoeren van een test storing voor het abonnement. Test storing simuleert uw storing en herstelbestanden om in een geïsoleerd netwerk. Houd rekening met:

- Als u verbinden met de virtuele machine in Azure wordt aangegeven met extern bureaublad na de storing wilt, moet u verbinding met extern bureaublad op de virtuele machine inschakelen voordat u de overname test uitvoeren.
- Na storing gebruikt u een openbare IP-adres verbinding maken met de virtuele machine in Azure wordt aangegeven met extern bureaublad. Als u dit doen wilt, moet u zorgen er geen domeinbeleid dat voorkomen dat u verbinding maakt met een virtuele machine met een openbare adres.

Volg de stappen in de [Activiteiten controleren](#monitor)om te controleren of de voltooiing van de bewerking.


### <a name="run-a-test-failover"></a>Een failover test uitvoeren

1.  Start de test overname door de volgende opdracht uit te voeren:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Een geplande failover uitvoeren

1. Start de geplande overname door de volgende opdracht uit te voeren:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

1. Start de niet-geplande overname door de volgende opdracht uit te voeren:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  


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
