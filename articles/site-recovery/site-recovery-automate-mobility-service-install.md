<properties
    pageTitle="VMware virtuele machines repliceren naar Azure via herstel van de Site met Azure automatisering DSC | Microsoft Azure"
    description="Wordt beschreven hoe u met Azure automatisering DSC automatisch de service van Azure Site herstel mobiliteit en Azure agent voor virtual/fysieke machines implementeren naar Azure."
    services="site-recovery"
    documentationCenter=""
    authors="krnese"
    manager="lorenr"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="krnese"/>

# <a name="replicate-vmware-virtual-machines-to-azure-by-using-site-recovery-with-azure-automation-dsc"></a>VMware virtuele machines repliceren naar Azure via herstel van de Site met Azure automatisering DSC

In bewerkingen Management-Suite bieden we u met een uitgebreide back-up en noodgevallen hersteloplossing die u als onderdeel van uw plan voor bedrijfscontinuïteit gebruiken kunt.

We begonnen dit reis samen met Hyper-V met behulp van Hyper-V Replica. Maar we hebben uitgebreid met ondersteuning van een heterogene installatie omdat klanten meerdere hypervisors en platforms in hun wolken.

Als u met VMware werkbelasting en/of fysieke servers vandaag, een server management alle onderdelen herstel van Azure-Site wordt uitgevoerd in uw omgeving worden afgehandeld van de communicatie en gegevens replicatie met Azure wanneer Azure de bestemming is.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>De Site herstel mobiliteit-service met behulp van automatisering DSC implementeren
Laten we beginnen door te doen een beknopt overzicht van de werking van deze server management.

De server wordt uitgevoerd verschillende serverrollen. Een van deze rollen is *configuratie*coördinaten communicatie en processen voor het replicatie en herstel van gegevens beheren.

Bovendien fungeert de rol *proces* als een gateway herhaling. Deze rol herhaling gegevens ontvangt van beveiligde bron machines, optimaliseert met caching, compressie en versleuteling en verzendt deze vervolgens naar een Azure opslag-account. Een van de functies voor de Procesrol is ook push-installatie van de service mobiliteit voor beveiligde computers en automatische detectie van VMware VMs uitvoeren.

Als er een failback van Azure, wordt de functie *basispagina* de herhaling gegevens als onderdeel van deze bewerking te verwerken.

Voor de beveiligde machines, zijn wij afhankelijk van de *mobiliteit service*. Dit onderdeel wordt geïmplementeerd op elke computer (VMware VM of fysieke server) die u wilt repliceren naar Azure. Er wordt vastgelegd schrijft gegevens op de computer en ze doorstuurt naar het management server (Procesrol).

Wanneer u bent omgaan met bedrijfscontinuïteit, is het is belangrijk is voor meer informatie over uw werkbelasting, de infrastructuur van uw en de betrokken onderdelen. U kunt vervolgens voldoen aan de vereisten voor uw herstel tijd doelstelling (RTO) en herstel punt doelstelling (vrijgegeven Productieorder). In deze context is de service mobiliteit toets om ervoor te zorgen dat uw werkbelasting zijn beveiligd, zoals u zou verwachten.

Hoe kunt u, op een geoptimaliseerde manier zorgt ervoor dat er een betrouwbare beveiligde instellen met behulp van bepaalde onderdelen bewerkingen Management Suite?

In dit artikel vindt een voorbeeld van hoe u Azure automatisering gewenst staat configuratie (DSC), samen met het herstellen van een Site gebruiken kunt om ervoor te zorgen dat:

- De mobiliteit service en Azure VM agent worden geïmplementeerd op de Windows-computers die u wilt beveiligen.
- De mobiliteit service en Azure VM agent altijd wanneer Azure het doel herhaling is gebruikt.

## <a name="prerequisites"></a>Vereisten voor

- Een bibliotheek voor de opslag van de vereiste instellingen
- Een bibliotheek voor de opslag van de vereiste wachtwoordzin registreren bij de server

 > [AZURE.NOTE] Een unieke wachtwoordzin wordt gegenereerd voor elke management server. Als u meerdere management servers implementeren terechtkomen, hebt u om ervoor te zorgen dat de juiste wachtwoordzin is opgeslagen in het bestand passphrase.txt.

- Windows Management Framework (WMF) 5.0 geïnstalleerd op de computers die u wilt inschakelen voor de beveiliging (noodzakelijk om Automation DSC)

 > [AZURE.NOTE] Als u gebruiken DSC voor Windows-systemen waarop WMF 4.0 is geïnstalleerd wilt, raadpleegt u de sectie [DSC gebruiken in verbroken omgevingen](#Use DSC in disconnected environments).

De mobiliteit-service kan worden geïnstalleerd via de opdrachtregel en verschillende argumenten. Daarom moet u de binaire bestanden hebt (na het uitpakken ze uit de configuratie) en ze hebt opgeslagen op een plaats waar u ze ophalen kunt met behulp van een DSC-configuratie.

## <a name="step-1-extract-binaries"></a>Stap 1: Uitpakken binaire bestanden

1. Als u wilt extraheren van de bestanden die u nodig voor deze instelling hebt, gaat u naar de volgende map op uw server management:

    **\Microsoft azure Site Recovery\home\svsystems\pushinstallsvc\repository**

    In deze map, moet u een MSI-bestand met de naam zien:

    **Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**

    Gebruik de volgende opdracht uit om te pakken het installatieprogramma van:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe/q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**

2. Selecteer alle bestanden en stuurt u naar een gecomprimeerde (ingepakte) map.

U hebt nu de binaire bestanden die u de configuratie van de service mobiliteit automatiseren moet met behulp van automatisering DSC.

### <a name="passphrase"></a>Wachtwoordzin

Vervolgens moet u bepalen waar u deze gecomprimeerde map plaatsen. U kunt een Azure opslag-account, zoals later, voor de opslag van de wachtwoordzin die u nodig voor het instellen hebt. De agent registreert vervolgens met de server management als onderdeel van het proces.

De wachtwoordzin dat u hebt ontvangen wanneer u de management server geïmplementeerd kan worden opgeslagen in een tekstbestand als passphrase.txt.

Plaats de gecomprimeerde map en de wachtwoordzin in een speciale container in de opslag van Azure-account.

![De locatie van map](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Als u liever om deze bestanden op een share op uw netwerk te houden, kunt u dat doen. U hoeft om ervoor te zorgen dat de DSC resource die u later wilt gebruiken, toegang heeft en de instelling en wachtwoordzin kunt downloaden.

## <a name="step-2-create-the-dsc-configuration"></a>Stap 2: De configuratie DSC maken

De configuratie, is afhankelijk van WMF 5.0. WMF 5.0 moet aanwezig zijn op de computer niet toepassen op de configuratie door automatisering DSC.

De omgeving wordt het volgende voorbeeld DSC configuratie gebruikt:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
De configuratie, wordt het volgende doen:

- De variabelen de configuratie wordt aangeeft waar u de binaire bestanden ophalen voor de service mobiliteit en de VM Azure-agent, waar u de wachtwoordzin en opslaglocatie van de uitvoer.
- De configuratie wordt de resource xPSDesiredStateConfiguration DSC importeren, zodat u kunt `xRemoteFile` downloaden van de bestanden uit de bibliotheek.
- De configuratie maakt een map waarin u wilt opslaan van de binaire bestanden.
- De resource archief worden de bestanden ophalen uit de gecomprimeerde map.
- Het pakket installeren resource wordt de mobiliteit-service installeren vanuit de UNIFIEDAGENT. EXE installer met de specifieke argumenten. (De variabelen die maken van de argumenten moeten worden aangepast aan uw omgeving.)
- Het pakket AzureAgent resource wordt de agent Azure VM, die wordt aanbevolen voor elke VM die wordt uitgevoerd in Azure geïnstalleerd. De agent Azure VM maakt het ook mogelijk extensies toevoegen aan de VM na failover.
- De service of bronnen zorgt ervoor dat de gerelateerde mobiliteit en welke Azure services altijd worden uitgevoerd.

De configuratie opslaan als **ASRMobilityService**.

>[AZURE.NOTE] Onthoud dat voor het vervangen van de CSIP in uw configuratie zodat de werkelijke management server, zodat de agent juist wordt verbonden en de juiste wachtwoordzin wordt gebruikt.

## <a name="step-3-upload-to-automation-dsc"></a>Stap 3: Uploaden naar automatisering DSC

Omdat de DSC-configuratie die u hebt aangebracht, een vereiste resource DSC-module (xPSDesiredStateConfiguration) importeren wordt, moet u die module in automatisering importeren voordat u de configuratie DSC uploaden.

Meld u aan bij uw account automatisering, blader naar de **activa** > **Modules**, en klik op **Bladeren galerie**.

Hier kunt u zoeken naar de module en in uw account importeren.

![Module importeren](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Wanneer u klaar bent, gaat u naar uw computer waar zich de modules Azure Resource Manager is geïnstalleerd en gaat u verder met de zojuist gemaakte DSC-configuratie importeren.

### <a name="import-cmdlets"></a>Cmdlets voor het importeren

In PowerShell, meld u aan bij uw Azure-abonnement. De cmdlets uit om uw omgeving doorgevoerd en vastleggen van uw accountgegevens automatisering in een variabele wijzigen:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Upload de configuratie naar automatisering DSC met behulp van de volgende cmdlet:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>De configuratie in automatisering DSC compileren

Vervolgens moet u met het compileren van de configuratie in automatisering DSC, zodat u beginnen kunt met het registreren van knooppunten toe. U bereiken die door de volgende cmdlet uit te voeren:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Dit kan een paar minuten duren, omdat u de configuratie in principe bij de gehoste DSC halen-service implementeert bent.

Nadat u de configuratie compileren, kunt u de taakgegevens kunt ophalen via PowerShell (Get-AzureRmAutomationDscCompilationJob) of met behulp van de [Azure-portal](https://portal.azure.com/).

![Taak ophalen](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

U hebt nu is gepubliceerd en uw configuratie DSC geüpload naar automatisering DSC.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Stap 4: Ingebouwde computers automatisering DSC
>[AZURE.NOTE] Een van de vereisten voor het voltooien van dit scenario is dat uw Windows-computers worden bijgewerkt met de nieuwste versie van WMF. U kunt downloaden en installeren van de juiste versie voor uw platform vanuit het [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=50395).

Nu maakt u een metaconfig voor DSC waarop u de knooppunten van toepassing. Met deze slagen als die u wilt ophalen de URL van het eindpunt en de primaire sleutel voor uw geselecteerde automatisering-account in Azure wordt aangegeven. U vindt deze waarden onder **toetsen** op het blad **alle instellingen** voor het account automatisering.

![Belangrijke waarden](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

In dit voorbeeld hebt u een Windows Server 2012 R2 fysieke server die u wilt beveiligen met sites worden hersteld.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Controleren op alle wachtende bestands-bewerkingen naam wijzigen in het register

Voordat u begint met de server koppelen aan het eindpunt automatisering DSC, wordt u aangeraden dat u op alle wachtende bestands-bewerkingen naam wijzigen in het register controleert. Ze kunnen voorkomen dat de configuratie afronden vanwege een in behandeling opnieuw opstarten.

Voer de volgende cmdlet om te bevestigen dat er geen in behandeling opnieuw opstarten op de server is:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Als dit wordt leeg weergegeven, bent u OK om terug te gaan. Als dat niet zo is, moet u dit adres door de server tijdens een onderhoudsvenster opnieuw opstarten.

De configuratie toepassen op de server, start het filter geïntegreerde uitvoeren van scripts-omgeving (wissen) en voer het volgende script. Dit is in principe een DSC lokale configuratie waarmee de lokale Configuration Manager-engine met de automatisering DSC-service registreren en de specifieke configuratie (ASRMobilityService.localhost) op te halen.

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Deze configuratie wordt de lokale Configuration Manager-engine zelf met automatisering DSC registreren. Dit wordt ook bepalen hoe de engine moet worden toegepast, wat dit moet doen als er een configuratie drift (ApplyAndAutoCorrect) en hoe deze verder met de configuratie als nodig is.

Nadat u deze script uitvoeren, moet het knooppunt registreren bij automatisering DSC beginnen.

![Knooppunt registratie in behandeling](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Als u terug wilt naar de Azure-portal, kunt u zien dat de nieuw ingeschreven knooppunt nu is gepubliceerd in de portal.

![Geregistreerde knooppunt in de portal](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Op de server, kunt u het volgende PowerShell-cmdlet om te bevestigen dat het knooppunt juist is geregistreerd uitvoeren:

```powershell
Get-DscLocalConfigurationManager
```

Nadat de configuratie is verzameld en toegepast op de server, kunt u dit controleren door de volgende cmdlet uit te voeren:

```powershell
Get-DscConfigurationStatus
```

De uitvoer ziet u dat de server is de configuratie heeft verzameld:

![Uitvoer](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

De service-instellingen voor mobiliteit heeft daarnaast een eigen log dat kan worden gevonden op *systeemstation*\ProgramData\ASRSetupLogs.

Dat is. U hebt nu is geïmplementeerd en de service mobiliteit op de computer die u beveiligen wilt met behulp van de Site herstel geregistreerd. DSC zorgt ervoor dat de vereiste services altijd worden uitgevoerd zijn.

![Geslaagde implementatie](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Nadat de server management wordt gedetecteerd voor de implementatie, kunt u beveiliging configureren en inschakelen van replicatie op de computer met behulp van sites worden hersteld.

## <a name="use-dsc-in-disconnected-environments"></a>DSC gebruiken in verbroken omgevingen

Als uw computers niet worden verbonden met Internet, kunt u nog steeds vertrouwen op DSC te implementeren en de mobiliteit-service configureren op de werklast die u wilt beveiligen.

U kunt een exemplaar van uw eigen DSC halen server maken in uw omgeving op te geven in principe dezelfde mogelijkheden die u uit automatisering DSC. Dat wil zeggen wordt de clients klikt, de configuratie (nadat deze geregistreerd) naar het eindpunt DSC. Een andere optie is echter handmatig de configuratie DSC push op uw computers, lokaal of extern.

Houd er rekening mee dat in dit voorbeeld een extra parameter voor de naam van de computer is. De externe bestanden zich nu op een externe share die moet worden toegankelijk met de computers die u wilt beveiligen. Het einde van het script enacts de configuratie en start vervolgens de configuratie DSC toepassen op de doel-pc.

### <a name="prerequisites"></a>Vereisten voor

Zorg ervoor dat de xPSDesiredStateConfiguration PowerShell-module is geïnstalleerd. Voor Windows-computers waarop WMF 5.0 is geïnstalleerd, kunt u de module xPSDesiredStateConfiguration door de volgende cmdlet uit te voeren op de doelcomputers installeren:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

U kunt ook downloaden en sla de module geval moet u het verspreidt op Windows-computers die WMF 4.0 hebben. Deze cmdlet uitvoeren op een computer waarop PowerShellGet (WMF 5.0) aanwezig is:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Ook voor WMF 4.0, zorgen dat de [Windows 8.1 KB2883200 bijwerken](https://www.microsoft.com/download/details.aspx?id=40749) op de computers is geïnstalleerd.

De volgende configuratie kunt verplaatst naar de Windows-computers WMF 5.0 en WMF 4.0 hebben:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Als u uw eigen DSC halen-server op uw bedrijfsnetwerk wilt nabootsen van de mogelijkheden die u via automatisering DSC openen kunt wordt gestart, raadpleegt u [bij het instellen van een DSC halen webserver](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Optioneel: Een DSC-configuratie met behulp van een sjabloon Azure resourcemanager implementeren

In dit artikel is gericht op hoe u uw eigen DSC configuratie maken kunt om automatisch de service mobiliteit en de Azure VM-Agent--implementeren en zorg ervoor dat ze worden uitgevoerd op de computers die u wilt beveiligen. We hebben ook een resourcemanager Azure-sjabloon waarin deze DSC-configuratie wordt geïmplementeerd op een nieuw of bestaand automatisering Azure-account. De sjabloon wordt invoerparameters gebruiken om Automation activa waarin u de variabelen voor uw omgeving te maken.

Nadat u de sjabloon implementeert, kunt u gewoon verwijzen naar stap 4 in deze handleiding om ingebouwde uw computers.

De sjabloon wordt het volgende doen:

1. Gebruik van een bestaand automatisering-account of een nieuw account te maken
2. Invoerparameters voor maakt:
    - ASRRemoteFile--de locatie waar u de service-instellingen voor mobiliteit hebt opgeslagen
    - ASRPassphrase--de locatie waar u het bestand passphrase.txt hebt opgeslagen
    - ASRCSEndpoint--het IP-adres van uw management server
3. De PowerShell-module xPSDesiredStateConfiguration importeren
4. Maken en de configuratie DSC compileren

De voorgaande stappen gebeurt er in de juiste volgorde maken, zodat u onboarding uw machines voor de beveiliging kan gaan.

De sjabloon, klikt u met de instructies voor de implementatie, bevindt zich op [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

De sjabloon implementeren via PowerShell:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Volgende stappen

Nadat u de medewerkers van mobiliteit implementeert, kunt u [Replicatie inschakelen](site-recovery-vmware-to-azure.md#step-6-replicate-applications) voor de virtuele machines.
