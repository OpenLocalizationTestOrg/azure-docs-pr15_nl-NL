<properties
    pageTitle="Azure-servers beveiligen met Azure PowerShell met Azure resourcemanager | Microsoft Azure"
    description="Beveiliging van servers Azure met Azure Site herstel automatiseren via PowerShell en Azure resourcemanager."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Repliceren tussen on-premises implementatie Hyper-V virtuele machines en Azure via PowerShell en Azure resourcemanager

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
- [PowerShell - resourcemanager](site-recovery-deploy-with-powershell-resource-manager.md)
- [Klassieke Portal](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>Overzicht

Azure Site herstel draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel door orchestrating herhaling, failover en herstel van virtuele machines in een aantal scenario's voor implementatie. Zie [overzicht van de Azure Site herstel](site-recovery-overview.md)voor een volledige lijst van scenario's voor implementatie.

Azure PowerShell is een module die cmdlets uit om het beheren van Azure via Windows PowerShell biedt. Dit kunt werken met twee typen modules: de module Azure-profiel of de resourcemanager Azure-module.

Site herstel PowerShell-cmdlets, beschikbaar voor communicatie met Azure PowerShell voor Azure resourcemanager, kunt u beveiligen en herstellen van uw servers in Azure wordt aangegeven.

In dit artikel wordt beschreven hoe u met Windows PowerShell, samen met Azure resourcemanager herstel van de Site om te configureren en goedkeuringen server beveiliging Azure implementeren. Het voorbeeld gebruikt in dit artikel ziet u hoe beveiligen en herstellen van virtuele machines op een host Hyper-V naar Azure, met behulp van Azure PowerShell met Azure resourcemanager mislukken.

> [AZURE.NOTE] De Site herstel PowerShell-cmdlets mogen momenteel u voor het configureren van de volgende handelingen uit: één VM Manager-site naar de andere, een site van Azure virtuele machines Manager en een site van Hyper-V Azure.

U hoeft te worden van een expert PowerShell voor het gebruik van dit artikel, maar u moet de basisbegrippen, zoals modules, cmdlets en sessies begrijpen. Zie [Aan de slag met Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx)voor meer informatie over Windows PowerShell.

U kunt ook meer informatie over het [Gebruik van Azure PowerShell met Azure resourcemanager](../powershell-azure-resource-manager.md).

> [AZURE.NOTE] Microsoft-partners die deel van het programma Cloud oplossing (CSP) van uitmaken kunnen configureren en beheren van beveiliging van een klant-servers op hun klanten desbetreffende CSP-abonnementen (tenant-abonnementen).

## <a name="before-you-start"></a>Voordat u begint

Controleer of dat u deze vereisten hebt geplaatst:

- Een [Microsoft Azure](https://azure.microsoft.com/) -account. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). Bovendien kunt u lezen over het [herstellen van Azure sitebeheerder prijzen](https://azure.microsoft.com/pricing/details/site-recovery/).
- Azure PowerShell 1.0. Zie voor meer informatie over deze release en hoe u het kunt installeren, [Azure PowerShell 1.0.](https://azure.microsoft.com/)
- De modules [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) en [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) . U kunt de meest recente versies van deze modules openen via de [PowerShell-galerie](https://www.powershellgallery.com/)

In dit artikel wordt beschreven hoe met Azure resourcemanager van Azure Powershell gebruiken om te configureren en beheren van beveiliging van uw servers. In het voorbeeld gebruikt in dit artikel ziet u hoe u een virtuele machine, uitgevoerd op een host Hyper-V, naar Azure beveiligen. De volgende vereisten zijn specifiek voor dit voorbeeld. Voor een meer uitgebreide reeks vereisten voor de verschillende scenario's voor sites worden hersteld, raadpleegt u de documentatie die betrekking hebben op dat scenario.

- Een Hyper-V host met Windows Server 2012 R2 of Microsoft Hyper-V Server 2012 R2 met een of meer virtuele machines.
- Hyper-V servers verbonden met Internet, rechtstreeks of via een proxy.
- [VM vereisten](site-recovery-best-practices.md#virtual-machines)moet voldoen aan de virtuele machines die u wilt beveiligen.


## <a name="step-1-sign-in-to-your-azure-account"></a>Stap 1: Meld u aan bij uw Azure-account


1. Open een PowerShell-console en voert u deze opdracht aan te melden bij uw Azure-account. De cmdlet Hiermee worden een webpagina die u om referenties van uw account vraagt.

        Login-AzureRmAccount

    U kunt ook u kunt ook opnemen referenties van uw account als een parameter voor de `Login-AzureRmAccount` cmdlet, met behulp van de `-Credential` parameter.

    Als u CSP partner werken namens een tenant bent, geeft u de klant als een tenant, met behulp van hun tenantID of tenant primaire domeinnaam.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Een account kan verschillende abonnementen, hebben, zodat u het abonnement dat u wilt gebruiken met het account moet koppelen.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Controleer of uw abonnement is geregistreerd als u wilt gebruiken van de Azure providers voor herstel Services en sites worden hersteld, met behulp van de volgende opdrachten:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    In de uitvoer van deze opdrachten, als de **RegistrationState** is ingesteld op **geregistreerd**, kunt u doorgaan naar stap 2. Als dat niet zo is, moet u de ontbrekende provider registreren in uw abonnement.

    Als u wilt de provider van Azure hebt geregistreerd voor een Site herstel en herstel Services, voert u de volgende opdrachten:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Controleer of de providers is geregistreerd met behulp van de volgende opdrachten: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` en `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Stap 2: De kluis herstel Services instellen

1. Een resourcegroep Azure resourcemanager waarin u wordt de kluis maken, of gebruik een bestaande resourcegroep maken. U kunt een nieuwe resourcegroep maken met behulp van de volgende opdracht uit:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    waar de variabele $ResourceGroupName bevat de naam van de resourcegroep die u wilt maken en de variabele $Geo bevat de Azure regio waarin de resourcegroep (bijvoorbeeld "Brazilië Zuid") te maken.

    U kunt een lijst met resourcegroepen in uw abonnement aanvragen met behulp van de `Get-AzureRmResourceGroup` cmdlet.

2. Hiermee maakt u een nieuwe Azure herstel Services kluis als volgt:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    U kunt een lijst met bestaande kluizen ophalen met behulp van de `Get-AzureRmRecoveryServicesVault` cmdlet.

> [AZURE.NOTE] Als u uitvoeren op de Site herstel kluizen gemaakt met de klassieke portal of de Azure-Service Management PowerShell-module wilt, kunt u een lijst van dergelijke kluizen ophalen met behulp van de `Get-AzureRmSiteRecoveryVault` cmdlet. Maak een nieuwe herstel Services kluis voor alle nieuwe bewerkingen. De Site herstel kluizen die u eerder hebt gemaakt, worden ondersteund, maar hoeft niet de nieuwste functies.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Stap 3: Stel de context van de kluis herstel Services

1.  De context kluis instellen door de volgende opdracht uit te voeren:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Stap 4: Een site Hyper-V maken en een nieuwe kluis registratie sleutel voor de site genereren.

1. Als volgt te werk om een nieuwe Hyper-V-site te maken:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Deze cmdlet Hiermee start u een taak Site herstellen om de site te maken en geeft als resultaat een object van de taak sites worden hersteld. Wacht totdat de taak te voltooien en controleren of de taak is voltooid.

    U kunt het taakobject ophaalt en daarmee ook controleren van de huidige status van de taak, met behulp van de cmdlet Get-AzureRmSiteRecoveryJob.
2. Genereren en downloaden van een sleutel registratie voor de site, als volgt:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Kopieer de gedownloade sleutel aan de host van Hyper-V. Moet u de toets om de host Hyper-V naar de site hebt geregistreerd.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Stap 5: De provider van Azure Site herstel en Azure herstel Services Agent installeren op uw host Hyper-V

1. Download het installatieprogramma voor de meest recente versie van de provider van [Microsoft](https://aka.ms/downloaddra).

2. Het installatieprogramma uitvoeren op uw host Hyper-V en aan het einde van de installatie gaat u verder met de registratiestap.

3. Wanneer u wordt gevraagd, bieden u de gedownloade site registratie toets vast en volledige registratie van de host Hyper-V naar de site.

4. Controleer of de host Hyper-V is geregistreerd naar de site met behulp van de volgende opdracht uit:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Stap 6: replicatiebeleid maken en koppelen aan de container beveiliging

1. Maak een replicatiebeleid als volgt:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Controleer de resulterende taak om ervoor te zorgen dat het maken van de beleid replicatie is uitgevoerd.

    >[AZURE.IMPORTANT] Het opgegeven opslag-account moet in hetzelfde Azure gebied, als uw kluis herstel Services en geografische-herhaling ingeschakeld nodig hebt.
    >
    > - Als de opgegeven herstel opslag-account is van het type Azure Storage (klassieke), herstellen overname van de beveiligde machines de machine Azure IaaS (klassieke).
    > - Als de opgegeven herstel opslag-account is van het type Azure Storage (Azure resourcemanager), herstellen overname van de beveiligde machines de machine Azure IaaS (Azure resourcemanager).

2. Krijg de beveiliging container overeenkomt met de site bent, als volgt:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  Start de koppeling van de container beveiliging met het beleid herhaling als volgt:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    Wacht totdat de koppeling taak is voltooid en zorg ervoor dat deze is voltooid.

##<a name="step-7-enable-protection-for-virtual-machines"></a>Stap 7: Beveiliging voor virtuele machines inschakelen

1. Krijg de beveiliging entity overeenkomt met de VM die u beveiligen wilt, als volgt:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Start de virtuele-computer als volgt beveiligen:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] Het opgegeven opslag-account moet in hetzelfde Azure gebied, als uw kluis herstel Services en geografische-herhaling ingeschakeld nodig hebt.
    >
    > - Als de opgegeven herstel opslag-account is van het type Azure Storage (klassieke), herstellen overname van de beveiligde machines de machine Azure IaaS (klassieke).
    > - Als de opgegeven herstel opslag-account is van het type Azure Storage (Azure resourcemanager), herstellen overname van de beveiligde machines de machine Azure IaaS (Azure resourcemanager).

    > Als de beveiliging van VM meer dan één schijf die zijn gekoppeld heeft, moet u de besturingssysteem-schijf opgeven met behulp van de parameter *OSDiskName* .

3. Wacht totdat de virtuele machines bereiken van een beveiligde status na de eerste replicatie. Dit kan duren, afhankelijk van factoren zoals de hoeveelheid gegevens worden gerepliceerd en de beschikbare boven bandbreedte voor Azure. De taakstatus en StateDescription worden als volgt bijgewerkt na de VM kunnen bereiken van een beveiligde status.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Herstel eigenschappen, zoals de grootte van de rol VM en het Azure netwerk om te koppelen van de virtuele machine netwerk interface kaarten bij failover bijwerken.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Stap 8: Een failover test uitvoeren

1. Een test failover taak uitvoert, als volgt:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Controleer of de test VM is gemaakt in Azure wordt aangegeven. (De failover testtaak is geschorst, na het maken van de test VM in Azure wordt aangegeven. De taak is voltooid door het opschonen van de gemaakte hoe na het hervatten van de taak, zoals in de volgende stap.)

3. Vul de overname test als volgt in:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Volgende stappen

[Lees meer](https://msdn.microsoft.com/library/azure/mt637930.aspx) over het herstellen van Azure-Site met Azure resourcemanager PowerShell-cmdlets.
