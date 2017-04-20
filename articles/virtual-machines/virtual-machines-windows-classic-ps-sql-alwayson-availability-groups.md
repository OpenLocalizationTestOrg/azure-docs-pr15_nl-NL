<properties
    pageTitle="Groep altijd op beschikbaarheid in Azure VM met PowerShell configureren"
    description="Deze zelfstudie resources die zijn gemaakt met het implementatiemodel klassieke gebruikt, en een altijd op beschikbaarheid-groep maken in Azure wordt aangegeven met PowerShell."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>Groep altijd op beschikbaarheid in Azure VM met PowerShell configureren

> [AZURE.SELECTOR]
- [Resourcemanager: sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Resourcemanager: handmatige](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassieke: UI](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassieke: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Azure virtuele machines (VMs) kunt databasebeheerders implementeren lager de kosten van een beschikbaarheid SQL Server wordt uitgevoerd. Deze zelfstudie ziet u hoe u het implementeren van een beschikbaarheidsgroep met SQL Server altijd op end-to-end binnen een Azure-omgeving. Aan het einde van de zelfstudie, wordt uw oplossing SQL Server altijd op in Azure bestaan uit de volgende elementen:

- Een virtueel netwerk met meerdere subnetten, inclusief een front-end en een back-enddatabase subnet

- Een domeincontroller met een domein AD (Active Directory)

- Twee SQL Server VMs geïmplementeerd in het back-enddatabase subnet en toegevoegd aan het AD-domein

- Een 3-knooppunt WSFC cluster met het knooppunt meeste quorum-model

- Een groep met twee synchroon doorvoeren replica's van de beschikbaarheid van een database beschikbaarheid

Dit scenario wordt gekozen voor de eenvoudig, niet voor de kosteneffectiviteit of andere factoren op Azure. U kunt bijvoorbeeld het aantal VMs voor een beschikbaarheidsgroep van de twee kopieën minimaliseren om op te slaan op een berekeningscluster uur in Azure wordt aangegeven met behulp van de domeincontroller als de quorum bestandssharewitness in een 2-knooppunt WSFC cluster. Deze methode beperkt het aantal VM door een van de bovenstaande configuratie.

Deze zelfstudie is bedoeld om aan te geven de benodigde stappen voor het instellen van de beschreven oplossing hierboven zonder bespreekt de details van elke stap. Daarom in plaats van waarin u de grafische gebruikersinterface volgende configuratiestappen uit, wordt deze PowerShell-scripts waarmee u snel stapsgewijs gebruikt. Wordt ervan uitgegaan dat het volgende:

- U hebt al een Azure-account aan het abonnement VM.

- U kunt de [Azure PowerShell-cmdlets](../powershell-install-configure.md)hebt geïnstalleerd.

- U hebt al een goed begrip van altijd op beschikbaarheidsgroepen voor on-premises-oplossingen. Zie [Altijd op beschikbaarheidsgroepen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx)voor meer informatie.

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Verbinding maken met uw Azure-abonnement en het virtuele netwerk maken

1. In een PowerShell-venster op uw lokale computer importeren van de Azure-module, een publicatie instellingenbestand downloaden naar uw computer en verbinden met uw PowerShell-sessie uw Azure-abonnement door deze te importeren de gedownloade publicatie-instellingen.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    De opdracht **Get-AzurePublishgSettingsFile** genereert automatisch een management certificaat met Azure downloaden naar uw computer. Een browser worden automatisch geopend en u wordt gevraagd om in te voeren van de referenties voor de Microsoft-account voor uw Azure-abonnement. Het gedownloade **.publishsettings** -bestand bevat alle informatie die u nodig hebt om uw Azure abonnement te beheren. Na dit bestand opslaat in een lokale map, moet u deze met de opdracht **Importeren-AzurePublishSettingsFile** importeren.

    >[AZURE.NOTE] Het bestand publishsettings bevat uw referenties (ongecodeerd) die worden gebruikt voor het beheren van uw Azure abonnementen en -services. Het beveiliging wordt aanbevolen voor dit bestand is tijdelijk opslaan buiten uw bron-mappen (bijvoorbeeld in de map Libraries\Documents), en verwijdert u deze wanneer het importeren is voltooid. Een kwaadwillende gebruiker toegang krijgen tot het bestand publishsettings kunt bewerken, maken en verwijderen van uw Azure services.

1. Een reeks variabelen die u gebruiken wilt voor het maken van uw cloud IT-infrastructuur definiëren.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Let op het volgende om ervoor te zorgen dat uw opdrachten later wordt uitgevoerd:

    - Variabelen **$storageAccountName** en **$dcServiceName** moet uniek zijn, omdat ze worden gebruikt om aan te geven van de cloud opslag-account en cloud-server, respectievelijk op internet.

    - Namen die zijn opgegeven voor de variabelen **$affinityGroupName** en **$virtualNetworkName** zijn geconfigureerd in het document voor het configureren van virtueel netwerk dat u later wilt gebruiken.

    - **$sqlImageName** Hiermee geeft u de bijgewerkte naam van de afbeelding VM met SQL Server 2012 Service Pack 1 Enterprise Edition.

    - Voor eenvoudig, **Contoso! 000** is het wachtwoord overal in de hele zelfstudie gebruikt.

1. Maak een groep affiniteit.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Maak een virtueel netwerk door een configuratiebestand te importeren.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Het configuratiebestand bevat de volgende XML-document. In het kort deze Hiermee geeft u een virtueel netwerk **ContosoNET** genoemd in de groep affiniteit is **ContosoAG**genoemd, en het adres ruimte **10.10.0.0/16** heeft en bestaat uit twee subnetten: **10.10.1.0/24** en **10.10.2.0/24**, die het front subnet en back-subnet, respectievelijk. Het front subnet is waar u clienttoepassingen zoals Microsoft SharePoint kunt plaatsen en het vorige subnet is waar u de SQL Server-VMs plaatst. Als u de variabelen **$affinityGroupName** en **$virtualNetworkName** eerder wijzigt, moet u ook de bijbehorende namen onderstaande wijzigen.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. Maak een opslag-account dat is gekoppeld aan de groep affiniteit u hebt gemaakt en dit als het huidige opslag-account instellen in uw abonnement.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. Maak de domeincontroller-server in nieuwe cloud-service en beschikbaarheid van de set.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Deze reeks opdrachten omgeleide het volgende doen:

    - **Nieuw-AzureVMConfig** Hiermee maakt u een VM-configuratie.

    - **Toevoegen-AzureProvisioningConfig** biedt de configuratieparameters van een zelfstandige versie van Windows server.

    - De gegevensschijf die u gebruiken wilt voor het opslaan van Active Directory-gegevens, met de optie ingesteld op geen caching toevoegen **Toevoegen-AzureDataDisk**

    - **Nieuw-AzureVM** Hiermee maakt u een nieuwe cloudservice en de nieuwe Azure VM maakt in de nieuwe cloudservice.

1. Wacht totdat de nieuwe VM om te worden volledig ingericht en het externe bureaublad bestand downloaden naar uw werkmap. Aangezien de nieuwe Azure VM duurt erg lang duren, de tijd herhalen blijft poll uitvoeren onder de nieuwe VM totdat de werkmap gereed voor gebruik is.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

De domeincontroller-server is nu is ingericht. Configureer het Active Directory-domein wordt u vervolgens op deze domeincontroller-server. Laat de PowerShell-venster openen op uw lokale computer. U kunt deze later opnieuw wilt gebruiken voor het maken van de twee VMs van SQL Server.

## <a name="configure-the-domain-controller"></a>De domeincontroller configureren

1. Maak verbinding met de server domeincontroller starten van het externe bureaublad bestand. De gebruikersnaam AzureAdmin en het wachtwoord van de beheerder van de computer **Contoso! 000**, die u hebt opgegeven bij het maken van de nieuwe VM.

1. Een PowerShell-venster openen in de beheerdersmodus voor.

1. Voer de volgende **DCPROMO. EXE** opdracht voor het instellen van het domein **corp.contoso.com** , klikt u met de gegevens-mappen op station M.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Zodra de opdracht is voltooid, wordt de VM automatisch opnieuw gestart.

1. Maak verbinding met de server domeincontroller opnieuw starten van het externe bureaublad bestand. Schakel ditmaal Meld u aan als **CORP\Administrator**.

1. Open een PowerShell-venster in de beheerdersmodus voor en de Active Directory-PowerShell-module met de volgende opdracht importeren:

        Import-Module ActiveDirectory

1. Voer de volgende opdrachten drie gebruikers toevoegen aan het domein.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** wordt gebruikt voor het configureren van alles betrekking hebben op de service-exemplaren van SQL Server, het cluster WSFC en de van de beschikbaarheidsgroep. **CORP\SQLSvc1** en **CORP\SQLSvc2** worden gebruikt als de accounts van de SQL Server-service voor de twee VMs van SQL Server.

1. Voer de volgende opdrachten **CORP\Install** de machtigingen geven om te maken van de computerobjecten in het domein.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    De bovenstaande GUID is de GUID voor het computertype. Het account **CORP\Install** moet de machtiging **Lezen alle eigenschappen** en **Computer-objecten maken** om te maken van de actieve directe objecten voor het cluster WSFC. De machtiging **Lezen alle eigenschappen** is al toegewezen aan CORP\Install al dan niet standaard, dus u niet hoeft toe te kennen expliciet. Zie voor meer informatie over de machtigingen die nodig zijn voor het maken van het cluster WSFC, [Failover Cluster stapsgewijze handleiding: Accounts configureren in Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Nu dat u klaar bent met het configureren van Active Directory en de gebruikersobjecten, u maakt u twee SQL Server VMs en voeg ze toe aan dit domein.

## <a name="create-the-sql-server-vms"></a>De SQL Server-VMs maken

1. Gaat u verder met het gebruik van de PowerShell-venster dat wordt geopend op uw lokale computer. De volgende aanvullende variabelen definiëren:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    Het IP-adres **10.10.0.4** wordt meestal toegewezen aan de eerste VM die u in het subnet **10.10.0.0/16** van uw Azure virtuele netwerk maken. U moet controleren of dat dit is het adres van de domeincontroller-server door **IPCONFIG**uit te voeren.

1. Voer de volgende piped opdrachten voor het maken van de eerste VM in het cluster WSFC, met de naam **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Houd rekening met het volgende met betrekking tot de bovenstaande opdracht:

    - **Nieuw-AzureVMConfig** maakt een VM-configuratie met de naam van de beschikbaarheid van de gewenste set. De volgende VMs wordt gemaakt met dezelfde naam van de beschikbaarheid van zodat ze op dezelfde beschikbaarheid zijn gekoppeld.

    - De VM samengevoegd **Toevoegen-AzureProvisioningConfig** het Active Directory-domein dat u hebt gemaakt.

    - **Set-AzureSubnet** geplaatst de VM in het subnet terug.

    - **Nieuw-AzureVM** Hiermee maakt u een nieuwe cloudservice en de nieuwe Azure VM maakt in de nieuwe cloudservice. De parameter **DnsSettings** geeft aan dat de DNS-server voor de servers in de nieuwe cloudservice heeft het IP-adres **10.10.0.4**, dat wil het IP-adres van de domeincontroller-server zeggen. Deze parameter is vereist voor de nieuwe VMs in de cloudservice toevoegen aan de Active Directory-domein is. Zonder deze parameter, moet u handmatig instellen van de IPv4-instellingen in uw VM gebruiken van de domeincontroller-server als de primaire DNS-server nadat de VM is ingericht en klikt u vervolgens de VM toevoegen aan het Active Directory-domein.

1. Voer de volgende piped opdrachten voor het maken van de SQL Server VMs, met de naam **ContosoSQL1** en **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Houd rekening met het volgende met betrekking tot de bovenstaande opdrachten:

    - **Nieuw-AzureVMConfig** dezelfde naam van de beschikbaarheid van gebruikt als de domeincontroller-server en wordt de afbeelding van de SQL Server 2012 Service Pack 1 Enterprise Edition gebruikt in de galerie VM. Ook wordt de schijf besturingssysteem ingesteld op alleen-caching alleen (niet schrijven in cache opslaan). Het wordt aanbevolen waarop u bestanden van de database naar een schijf gegevens apart migreren die u als bijlage aan de VM toevoegen en configureert u deze met geen lezen of in cache opslaan schrijven. Het volgende beste is echter wilt verwijderen schrijven naar cache op de schijf besturingssysteem, omdat u niet verwijderen lezen op de schijf besturingssysteem caching.

    - De VM samengevoegd **Toevoegen-AzureProvisioningConfig** het Active Directory-domein dat u hebt gemaakt.

    - **Set-AzureSubnet** geplaatst de VM in het subnet terug.

    - **Toevoegen-AzureEndpoint** toegevoegd access eindpunten zodat clienttoepassingen toegang deze services-exemplaren van SQL Server op internet tot hebben. Verschillende poorten zijn besteed aan ContosoSQL1 en ContosoSQL2.

    - **Nieuw-AzureVM** Hiermee maakt u de nieuwe SQL Server-VM in de dezelfde cloudservice als ContosoQuorum. Als u dat ze in bepaalde beschikbaarheid, moet u de VMs plaatsen in de dezelfde cloudservice.

1. Wacht totdat elk VM om te worden volledig ingericht en het externe bureaublad bestand downloaden naar uw werkmap. De gedurende lus bladert u door de drie nieuwe VMs en voert de opdrachten in het hoogste niveau accolades voor elk van deze.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    De SQL Server-VMs nu is ingericht en uitgevoerd, maar ze worden geïnstalleerd met SQL Server met standaardopties.

## <a name="initialize-the-wsfc-cluster-vms"></a>De WSFC Cluster VMs geïnitialiseerd

In deze sectie moet u de drie servers die u in het cluster WSFC en de installatie van SQL Server gebruiken zult wijzigen. Name:

- (Alle servers) U moet de functie **Failoverclustering** installeren.

- (Alle servers) U moet **CORP\Install** toevoegen als de computer- **beheerder**.

- (ContosoSQL1 en ContosoSQL2 alleen) U moet **CORP\Install** toevoegen als een rol **systeembeheerder** in de standaarddatabase.

- (ContosoSQL1 en ContosoSQL2 alleen) U moet **Systeemaccount** toevoegen als een aanmelding met de volgende machtigingen:

    - Een beschikbaarheidsgroep wijzigen

    - Verbinding maken met SQL

    - Status weergeven

- (ContosoSQL1 en ContosoSQL2 alleen) Het **TCP-** protocol is al ingeschakeld op de SQL Server-VM. U moet echter nog steeds de openen van de firewall voor externe toegang van SQL Server.

U bent nu klaar om te beginnen. Beginnen met **ContosoQuorum**, volg de onderstaande stappen:

1. Maak verbinding met **ContosoQuorum** starten van het externe bureaublad bestanden. De gebruikersnaam **AzureAdmin** en het wachtwoord van de beheerder van de computer **Contoso! 000**, die u hebt opgegeven bij het maken van de VMs.

1. Controleer of dat de computers hebt toegevoegd aan **corp.contoso.com**.

1. Wacht totdat de installatie van SQL Server om te voltooien actieve initialisatietaken van de geautomatiseerde voordat u verdergaat.

1. Een PowerShell-venster openen in de beheerdersmodus voor.

1. De functie Windows Failoverclustering installeren.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Voeg **CORP\Install** als beheerder.

        net localgroup administrators "CORP\Install" /Add

1. Meld u af bij ContosoQuorum. U bent nu klaar met deze server.

        logoff.exe

Vervolgens geïnitialiseerd **ContosoSQL1** en **ContosoSQL2**. Volg de onderstaande stappen, die zijn identiek voor beide van de SQL Server-VMs.

1. Maak verbinding met de twee SQL Server-VMs starten van het externe bureaublad bestanden. De gebruikersnaam **AzureAdmin** en het wachtwoord van de beheerder van de computer **Contoso! 000**, die u hebt opgegeven bij het maken van de VMs.

1. Controleer of dat de computers hebt toegevoegd aan **corp.contoso.com**.

1. Wacht totdat de installatie van SQL Server om te voltooien actieve initialisatietaken van de geautomatiseerde voordat u verdergaat.

1. Een PowerShell-venster openen in de beheerdersmodus voor.

1. De functie Windows Failoverclustering installeren.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. **CORP\Install** als lokale beheerder toevoegen

        net localgroup administrators "CORP\Install" /Add

1. Importeer de PowerShell-Provider van SQL Server.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. Voeg **CORP\Install** als de systeembeheerdersrol voor de standaard SQL Server-instantie.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. **Systeemaccount** toevoegen als uw aanmelding bij de drie machtigingen hierboven is beschreven.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Open de firewall voor externe toegang van SQL Server.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Meld u af bij beide VMs.

        logoff.exe

Ten slotte, bent u gereed is voor het configureren van de van de beschikbaarheidsgroep. U kunt de PowerShell-Provider van SQL Server al het werk op **ContosoSQL1**uitvoeren.

## <a name="configure-the-availability-group"></a>De beschikbaarheid van de groep configureren

1. Maak verbinding met **ContosoSQL1** opnieuw starten van het externe bureaublad bestanden. In plaats van logboekregistratie in het gebruik van de computeraccount, meld u aan met **CORP\Install**.

1. Een PowerShell-venster openen in de beheerdersmodus voor.

1. De volgende variabelen definiëren:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. Importeer PowerShell-Provider voor SQL Server.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. Wijzig de SQL Server-account voor ContosoSQL1 in CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Wijzig de SQL Server-account voor ContosoSQL2 in CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Download **CreateAzureFailoverCluster.ps1** uit [WSFC Cluster maken voor altijd op beschikbaarheid van groepen in Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) naar de lokale werkmap. U kunt dit script te maken van een functionele WSFC cluster. Zie voor belangrijke informatie over hoe WSFC moet met het Azure-netwerk samenwerken, [beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md).

1. Wijzigen in uw werkmap en het WSFC cluster met het gedownloade script maken.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Altijd op beschikbaarheidsgroepen inschakelen voor de standaard SQL Server-exemplaren op **ContosoSQL1** en **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Maak een back-upmap en machtigingen voor de accounts van de SQL Server-service. U kunt deze map wilt gebruiken voor het voorbereiden van de beschikbaarheid van de database op de secundaire replica.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Een database maakt op **ContosoSQL1** **MyDB1**genoemd, zowel een volledige back-up en een back-up en herstellen op **ContosoSQL2** met de optie **Gemarkeerd** .

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. De beschikbaarheid van groep eindpunten op de SQL Server-VMs maken en de juiste machtigingen instellen voor de eindpunten.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. De beschikbaarheid van replica's maken.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Ten slotte de van de beschikbaarheidsgroep hebt gemaakt en de secundaire replica deelnemen aan de van de beschikbaarheidsgroep.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Volgende stappen
U hebt nu geïmplementeerd SQL Server altijd op door te maken van een beschikbaarheidsgroep in Azure wordt aangegeven. Zie een luisteraar ervan af voor deze beschikbaarheidsgroep configureren [configureren een ILB luisteraar ervan af voor altijd op beschikbaarheid van groepen in Azure wordt aangegeven](virtual-machines-windows-classic-ps-sql-int-listener.md).

Zie voor meer informatie over het gebruik van SQL Server in Azure wordt aangegeven, [SQL Server op Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).
