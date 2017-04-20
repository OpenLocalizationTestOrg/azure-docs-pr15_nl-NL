<properties
    pageTitle="Hyper-V virtuele machines in VMM wolken met Azure Site herstel en PowerShell repliceren | Microsoft Azure"
    description="Leer hoe u de replicatie van Hyper-V virtuele machines in VMM wolken met sites worden hersteld en PowerShell automatiseren."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Hyper-V virtuele machines in VMM wolken repliceren naar Azure via Powershell - klassiek

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - resourcemanager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassieke Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - klassiek](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>Overzicht

Azure Site herstel draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines in een aantal scenario's voor implementatie. Zie [overzicht van de Azure Site herstel](site-recovery-overview.md)voor een volledige lijst met scenario's voor implementatie.

In dit artikel leest u hoe u algemene taken die u uitvoeren wilt bij het instellen van Azure Site herstel Hyper-V virtuele machines in System Center VMM wolken repliceren met Azure storage automatiseren met PowerShell.

Lees het artikel vereisten voor het scenario bevat, en ziet u hoe u een Site herstel kluis instellen, de Provider van Azure Site herstel installeren op de bronserver VMM, de server in de kluis registreren, een Azure opslag-account toevoegen, installeren van de Azure herstel Services-agent op Hyper-V hostservers, configureren van instellingen voor documentbeveiliging voor VMM wolken die wordt toegepast op alle beveiligde virtuele machines , en vervolgens de beveiliging voor deze virtuele machines inschakelen. Voltooien door te testen van de overname om ervoor te zorgen dat alles werkt zoals verwacht.

Als u dit scenario instellen problemen ondervindt, kunt u uw vragen posten op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [resourcemanager en klassiek](../resource-manager-deployment-model.md). In dit artikel beschreven hoe u met het model Klassiek implementatie.



## <a name="before-you-start"></a>Voordat u begint

Controleer of dat u deze vereisten hebt geplaatst:

### <a name="azure-prerequisites"></a>Azure vereisten

- U moet een [Microsoft Azure](https://azure.microsoft.com/) -account. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- U moet een Azure opslag-account voor de opslag gerepliceerde gegevens. Het account moet geografische-replicatie is ingeschakeld. Er moet worden in hetzelfde gebied, als de kluis Azure sites worden hersteld en worden gekoppeld aan hetzelfde abonnement. [Meer informatie over Azure opslag](../storage/storage-introduction.md).
- U moet om ervoor te zorgen dat virtuele machines die u wilt beveiligen [Azure virtuele machines vereisten](site-recovery-best-practices.md#virtual-machines)voldoen.

### <a name="vmm-prerequisites"></a>VMM vereisten
- U moet uitvoeren op systeem Center 2012 R2 VMM-server.
- U moet ten minste één cloud op de VMM-server die u wilt beveiligen. De cloud moet bevatten:
    - Een of meer VMM hostgroepen.
    - Een of meer Hyper-V hostservers of clusters in elke hostgroep.
    - Een of meer virtuele machines op de bronserver Hyper-V.

### <a name="hyper-v-prerequisites"></a>Vereisten voor Hyper-V

- De Hyper-V-hostservers ten minste moeten worden uitgevoerd met de functie Hyper-V of **Microsoft Hyper-V Server 2012** , **Windows Server 2012** en de meest recente updates hebt geïnstalleerd.
- Als u werkt met Hyper-V in een notitie cluster die makelaar cluster wordt niet automatisch gemaakt als u een statische IP-adres gebaseerde cluster hebt. U moet de makelaar cluster handmatig configureren. Hiervoor in Serverbeheer > Failoverclusterbeheer, verbinding maken met het cluster op **Rol configureren** en selecteer **Hyper-V Replica makelaar** in het scherm **Selecteer de rol** van de wizard beschikbaarheid.
- Alle Hyper-V hostserver of cluster waarvoor u wilt beheren beveiliging moet worden opgenomen in een wolk VMM.

### <a name="network-mapping-prerequisites"></a>Vereisten voor netwerk-toewijzing
Wanneer u virtuele machines in Azure netwerk toewijzing kaarten tussen VM netwerken op de bronserver VMM beveiligen en afstemmen van Azure-netwerken te gebruiken om in te schakelen van de volgende handelingen uit:

- Alle computers die niet op in hetzelfde netwerk kunnen koppelen aan elkaar grenzen, ongeacht welk abonnement herstel ze zijn in.
- Als een netwerkgateway ingesteld op het doel Azure netwerk is, worden virtuele machines kunt koppelen aan andere on-premises implementatie virtuele machines.
- Als u niet zo configureert netwerk toewijzing alleen virtuele machines die in hetzelfde herstelplan mislukken kunnen verbinding maken met elkaar na failover naar Azure.

Als u wilt implementeren netwerk toewijzing hebt u het volgende nodig:

- De virtuele machines die u wilt beveiligen op de bronserver VMM moeten zijn verbonden met een netwerk VM. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
- Een Azure netwerk waarmee gerepliceerde virtuele machines verbinding na failover maken kunt. U kunt dit netwerk selecteren op het moment van failover. Het netwerk moet in hetzelfde gebied, als uw abonnement Azure sites worden hersteld.
- [Meer informatie](site-recovery-network-mapping.md) over het toewijzen van netwerk:

###<a name="powershell-prerequisites"></a>PowerShell vereisten
Zorg ervoor dat er Azure PowerShell wilt verzenden. Als u al PowerShell gebruikt, moet u een upgrade uitvoeren naar versie 0.8.10 of hoger. Zie voor informatie over het instellen van PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Als u hebt ingesteld en PowerShell geconfigureerd, kunt u alle de beschikbaar cmdlets voor de service bekijken [hier](https://msdn.microsoft.com/library/dn850420.aspx).

Zie [Aan de slag met Azure cmdlets voor](https://msdn.microsoft.com/library/azure/jj554332.aspx)meer informatie over tips waarmee u kunnen dat u de cmdlets, zoals hoe parameterwaarden, invoer en uitvoer meestal worden verwerkt in Azure PowerShell gebruiken.

## <a name="step-1-set-the-subscription"></a>Stap 1: Het abonnement instellen

Voer deze cmdlets in PowerShell:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Elementen in de "haken' vervangen door uw specifieke informatie.

## <a name="step-2-create-a-site-recovery-vault"></a>Stap 2: Een Site herstel kluis maken

Elementen in de "haken' vervangen door uw specifieke informatie in PowerShell, en deze opdrachten uitvoeren:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Stap 3: Een kluis registratiegegevens sleutel genereren

Genereer een registratiegegevens-sleutel in de kluis. Nadat u de Provider van Azure Site herstel downloaden en op de server VMM installeren, gebruikt u deze toets naar de server VMM in de kluis registreren.

1.  Het bestand van de instelling kluis ophalen en instellen van de context:

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  De context kluis instellen door de volgende opdrachten uit te voeren:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Stap 4: De Provider van Azure Site herstel installeren

1.  Op de computer VMM, moet u een map maken door de volgende opdracht uit te voeren:

    ```

    pushd C:\ASR\

    ```

2.  Pak de bestanden met de gedownloade provider door de volgende opdracht uit te voeren

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Installeer de provider met de volgende opdrachten:

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Wacht totdat de installatie om te voltooien.

4.  De server in het gebruik van de volgende opdracht uit kluis registreren:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Stap 5: Een Azure opslag-account maken

Als u niet een Azure opslag-account hebt, maakt u een account geografische-replicatie is ingeschakeld door de volgende opdracht uit te voeren:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Houd er rekening mee dat het opslag-account moet in hetzelfde gebied, als de Site herstel van Azure-service worden en gekoppeld aan hetzelfde abonnement zijn.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Stap 6: De Agent van de Services Azure herstel installeren

In de portal Azure installeren van de Azure herstel Services-agent op elke Hyper-V host-server die zich bevindt in de VMM wolken die u wilt beveiligen.

Voer de volgende opdracht uit op alle VMM hosts:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Stap 7: Configureren cloud instellingen voor documentbeveiliging

1.  Een profiel van de beveiliging cloud naar Azure maken door de volgende opdracht uit te voeren:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Krijg een container beveiliging door de volgende opdrachten uit te voeren:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  Start de koppeling van de container beveiliging met de cloud:

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  Nadat de taak is voltooid, voert u de volgende opdracht uit:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  Nadat de taak zijn verwerkt, voert u de volgende opdracht uit:


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



Volg de stappen in de [Activiteiten controleren](#monitor)om te controleren of de voltooiing van de bewerking.

## <a name="step-8-configure-network-mapping"></a>Stap 8: Netwerk toewijzing configureren
Voordat u begint of er netwerk toewijzing virtuele machines op de bronserver VMM verbonden bent met een netwerk VM. Daarnaast een of meer Azure virtuele netwerken maken. Houd er rekening mee dat meerdere VM-netwerken kunnen worden toegewezen aan één Azure netwerk.

Houd er rekening mee dat als het doelnetwerk meerdere subnetten heeft en een van deze subnetten dezelfde naam als subnet waarop de bron virtuele machine zich bevindt heeft, en u vervolgens de replica virtuele machine worden verbonden met dat doel subnet na failover. Als er een subnet doellijst met een overeenkomende naam is, wordt de virtuele machine niet worden verbonden met het eerste subnet in het netwerk.

De eerste opdracht krijgt servers voor de huidige Site herstel van Azure kluis. De opdracht opgeslagen de servers van Microsoft Azure Site herstellen in de matrix $Servers variabele.

    $Servers = Get-AzureSiteRecoveryServer


De tweede opdracht krijgt de site herstel netwerk voor de eerste server in de matrix $Servers. De opdracht opgeslagen de netwerken in de variabele $Networks.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

De derde opdracht krijgt van uw Azure abonnementen met behulp van de cmdlet Get-AzureSubscription en vervolgens wordt opgeslagen die waarde in de variabele $Subscriptions.

    $Subscriptions = Get-AzureSubscription



De vierde opdracht krijgt Azure virtuele netwerken met behulp van de cmdlet Get-AzureVNetSite, waarna deze waarde in de variabele $AzureVmNetworks.


    $AzureVmNetworks = Get-AzureVNetSite



De cmdlet definitief Hiermee maakt u een toewijzing tussen het primaire netwerk en het netwerk Azure virtuele machines. De cmdlet Hiermee geeft u het primaire netwerk als het eerste element van $Networks. De cmdlet geeft een netwerk met een virtuele machine als het eerste element van $AzureVmNetworks met behulp van de-ID. De opdracht bevat uw Azure abonnement-ID.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Stap 9: Beveiliging voor virtuele machines inschakelen

Nadat de servers, wolken en netwerken correct zijn geconfigureerd, kunt u beveiliging voor virtuele machines in de cloud. Houd rekening met het volgende:

Virtuele machines moet voldoen aan [vereisten Azure virtuele machines](site-recovery-best-practices.md#virtual-machines).

Beveiliging kunnen besturingssysteem en besturingssysteem moeten schijfeigenschappen worden ingesteld voor de virtuele machine. Wanneer u een virtuele machine maakt in VMM met een sjabloon VM kunt u de eigenschap instellen. U kunt ook deze eigenschappen voor bestaande virtuele machines instellen op de tabbladen **Algemeen** en **Hardwareconfiguratie** VM eigenschappen. Als u niet deze eigenschappen in VMM instellen kunt u zult kunt u deze in de portal-Site herstel van Azure configureren.



1.  Als u beveiliging, voert u de volgende opdracht uit om de container beveiliging:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. Krijg de beveiliging entity (VM) door de volgende opdracht uit te voeren:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. De DR voor VM inschakelen door de volgende opdracht uit te voeren:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>Test uw implementatie

Test uw implementatie kunt u uitvoeren van een failover test voor een enkele virtuele machine, of een plan voor herstel die bestaat uit meerdere virtuele machines maken en uitvoeren van een failover test voor het abonnement. Test failover simuleert uw failover- en herstelbestanden om in een geïsoleerd netwerk. Houd rekening met:

- Als u verbinden met de virtuele machine in Azure wordt aangegeven met extern bureaublad na de overname wilt, moet u verbinding met extern bureaublad op de virtuele machine inschakelen voordat u de overname test uitvoeren.
- Na een failover gebruikt u een openbare IP-adres verbinding maken met de virtuele machine in Azure wordt aangegeven met extern bureaublad. Als u dit doen wilt, moet u zorgen er geen domeinbeleid dat voorkomen dat u verbinding maakt met een virtuele machine met een openbare adres.

Volg de stappen in de [Activiteiten controleren](#monitor)om te controleren of de voltooiing van de bewerking.

### <a name="create-a-recovery-plan"></a>Maak een herstelplan

1. Een XML-bestand als een sjabloon voor uw herstel-abonnement met de onderstaande gegevens maken en deze opslaat als "C:\RPTemplatePath.xml".
2. Wijzig de RecoveryPlan knooppunt Id, naam, PrimaryServerId en SecondaryServerId.
3. Wijzig het knooppunt ProtectionEntity PrimaryProtectionEntityId (vmid van VMM).
4. U kunt meer VMs toevoegen door toe te voegen meer ProtectionEntity knooppunten.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Vul de gegevens in de sjabloon in:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. De RecoveryPlan maken:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>Een failover test uitvoeren

1.  Krijg het object RecoveryPlan door de volgende opdracht uit te voeren:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Start de test overname door de volgende opdracht uit te voeren:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


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

[Lees meer](https://msdn.microsoft.com/library/dn850420.aspx) over Azure Site herstel PowerShell-cmdlets. </a>.
