<properties
    pageTitle="Azure Premium opslagruimte gebruiken met SQL Server | Microsoft Azure"
    description="In dit artikel resources die zijn gemaakt met het implementatiemodel klassieke gebruikt en leidraad voor het gebruik van Azure Premium opslagruimte in SQL Server wordt uitgevoerd op Azure virtuele Machines."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Azure Premium opslagruimte gebruiken met SQL Server op virtuele Machines


## <a name="overview"></a>Overzicht

[Azure Premium opslag](../storage/storage-premium-storage.md) is de volgende generatie opslag waarmee lage latentie en hoge doorvoer IO. Het werkt beste werkt voor belangrijke IO intensief werkbelastingen, zoals SQL Server op IaaS [virtuele Machines](https://azure.microsoft.com/services/virtual-machines/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


In dit artikel vindt u planning en richtlijnen voor het migreren van een virtuele Machine met SQL Server Premium opslag gebruiken. Dit geldt ook voor Azure-infrastructuur (netwerken, opslag) en Gast Windows VM stappen. Het voorbeeld in het [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) ziet een volledige volledig complete-migratie van het verplaatsen van grotere VMs om te profiteren van verbeterde lokale SSD opslag met PowerShell.

Het is belangrijk is voor meer informatie over het end-to-end-proces met Azure Premium-opslag met SQL Server op IAAS VMs. Deze groep omvat:

- Identificatie van de vereisten voor het gebruik van de Premium-opslag.
- Voorbeelden van de implementatie van SQL Server op IaaS naar de Premium-opslag voor nieuwe implementaties.
- Voorbeelden van migreren bestaande implementaties, zowel zelfstandige servers en implementaties met SQL altijd op beschikbaarheid van de groepen.
- Mogelijke migratie methoden.
- Volledige end-to-end-voorbeeld met Azure, Windows en SQL Server stappen voor de migratie van een bestaande altijd op implementatie.

Zie voor meer achtergrondinformatie over SQL Server in Azure virtuele Machines, [SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).

**Auteur:** Daniel Sol **technische revisoren:** Luis Jeroen Vargas haring, Sanjay Mishra, Pravin Mital, Schwertl Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Vereisten voor de Premium-opslag

Er zijn verschillende vereisten voor het gebruik van de Premium-opslag.

### <a name="machine-size"></a>Grootte van de computer

U moet DS reeks virtuele Machines (VM) gebruiken voor het gebruik van de Premium-opslag. Als u niet DS serie machines in uw cloudservice voordat u hebt gebruikt, moet u de bestaande VM verwijderen, het bijgevoegde schijven houden en vervolgens een nieuwe cloudservice maken voordat u opnieuw de VM als DS * rol grootte te maken. Zie voor meer informatie over VM formaten, [virtuele Machine en Cloud Service grootten voor Azure](virtual-machines-linux-sizes.md).

### <a name="cloud-services"></a>Cloudservices

U kunt alleen DS * VMs met Premium opslagmedia gebruiken wanneer ze worden gemaakt in een nieuwe cloudservice. Als u SQL Server altijd op in Azure gebruikt, wordt de altijd op luisteraar ervan af wordt verwezen naar de Azure interne en externe laden verdeling-IP-adres dat is gekoppeld aan een cloudservice. In dit artikel bevat informatie over het migreren behoud van beschikbaarheid in dit scenario.

> [AZURE.NOTE] Een reeks DS * moet de eerste VM die wordt geïmplementeerd in de nieuwe Cloudservice.

### <a name="regional-vnets"></a>Regionale VNETS

Voor DS * VMs moet u het virtuele netwerk (VNET) hosten van uw VMs om te worden regionale configureren. Dit "wordt"-de VNET is toe te staan dat het grotere VMs om te worden deze is ingericht in andere clusters en communicatie ertussen toestaan. In de volgende schermafbeelding ziet u de gemarkeerde locatie regionale VNETs, terwijl het eerste resultaat ziet u een "smalle" VNET.

![RegionalVNET][1]

U kunt een Microsoft-ondersteuningsticket om te migreren naar een regionale VNET verhogen, Microsoft wilt wijzigen en klik om te voltooien van de migratie naar regionale VNETs, de eigenschap AffinityGroup in de netwerkconfiguratie wijzigen. Eerst exporteren de netwerkconfiguratie in PowerShell en klikt u vervolgens de eigenschap **AffinityGroup** in het element **VirtualNetworkSite** vervangen door een eigenschap **locatie** . Geef `Location = XXXX` waar `XXXX` Azure regio is. Importeer de nieuwe configuratie.

Bijvoorbeeld overwegen de volgende VNET-configuratie:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Verplaats dit naar een regionale VNET in West Europa door de configuratie te wijzigen als volgt uit:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Opslag-accounts

U moet een nieuw opslag-account dat is geconfigureerd voor Premium opslag maken. Houd er rekening mee dat het gebruik van de Premium-opslag is ingesteld op de opslag-account, niet op afzonderlijke VHD's, maar bij gebruik van een reeks-VM DS * u de VHD van Premium en standaardopslag-accounts koppelen kunt. Kunt u overwegen dit als u niet wilt plaatsen van de VHD OS op de opslag van de Premium-account.

De volgende opdracht uit **Nieuw-AzureStorageAccountPowerShell** met "Premium_LRS" hetzelfde **Type** Hiermee maakt u een Premium-opslag-Account:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>VHD's Cache-instellingen

Het belangrijkste verschil tussen het maken van schijven die deel van een Premium-opslag-account uitmaken is de schijf cache-instelling. Het wordt aanbevolen dat u '**Caching van gelezen gebruikt**' voor SQL Server Data volume schijven. Bij transactie log volumes, worden de instelling van de cache schijf ingesteld op '**geen**'. Dit verschilt van de aanbevelingen voor standaardopslag-accounts.

Zodra de VHD's zijn gekoppeld, kan de instelling cache kan niet worden gewijzigd. U moet ontkoppelen en opnieuw de VHD koppelen met de instelling voor een bijgewerkte cache.

### <a name="windows-storage-spaces"></a>Windows-opslag spaties

U kunt [Windows opslag spaties](https://technet.microsoft.com/library/hh831739.aspx) net als met vorige standaardopslag, Hierdoor kunt u wilt migreren van een VM die al gebruik maakt van van opslag spaties. Het voorbeeld in [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (stap 9 en doorsturen) bevat de Powershell-code als u wilt extraheren en een VM met meerdere gekoppelde VHD's te importeren.

Opslag van toepassingen zijn met standaard Azure opslag-account gebruikt voor de algehele en latentie te verkleinen. Wellicht waarde in de opslag van toepassingen met de Premium-opslag voor nieuwe implementaties testen, maar kunnen ze extra complexiteit met setup opslag toevoegen.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Welke kaart Azure virtuele schijven opslag van toepassingen zoeken

Als er verschillende cache instelling aanbevelingen voor bijgevoegde VHD's zijn, wilt u mogelijk de VHD's kopiëren naar een Premium-opslag-account. Wanneer u opnieuw worden gekoppeld aan de nieuwe DS reeks VM, hoeft u wellicht de cache-instellingen wijzigen. Is het eenvoudiger om toe te passen de Premium-opslag aanbevolen cache-instellingen als er afzonderlijke VHD's voor de SQL-gegevensbestanden en logboekbestanden (en niet een enkel VHD met zowel).

> [AZURE.NOTE] Als u SQL Server-gegevens- en logbestanden bestanden op hetzelfde volume hebt, is het in de cache optie die u kiest afhankelijk van de IO access patronen voor uw database werkbelasting. Alleen testen kan laten zien welke optie in de cache meest geschikt is voor dit scenario.

Als u werkt met Windows opslag spaties die bestaan uit meerdere VHD's moet u kijkt u naar uw oorspronkelijke scripts om aan te geven die zijn toegevoegd VHD's zijn echter in specifieke toepassingen, dus vervolgens stelt u de cache-instellingen dienovereenkomstig gewijzigd voor elke schijf.

Als u geen oorspronkelijke script om aan te geven die VHD's die zijn toegewezen aan de groep opslagruimte beschikbaar hebt, kunt u de volgende stappen uit om te bepalen van de toewijzing van de groep/opslagruimte op een schijf.

Voor elke schijf, gebruikt u de volgende stappen:

1. Lijst met schijven die zijn gekoppeld aan VM met de opdracht **Get-AzureVM** ophalen:

    Get-AzureVM - servicenaam <servicename> -naam <vmname> | Get-AzureDataDisk

1. Noteer de Diskname en LUN.

    ![DisknameAndLUN][2]

1. Extern bureaublad naar de VM. Ga vervolgens naar de **Computer Management** | **Apparaatbeheer** | **schijfstations**. Bekijk de eigenschappen van elk van de 'Microsoft Virtual schijven'

    ![VirtualDiskProperties][3]

1. Het getal LUN hier is een verwijzing naar het LUN dat u opgeeft wanneer de VHD koppelen aan de VM.
1. Voor de 'Microsoft virtuele schijf' gaat u naar het tabblad **Details** , in de lijst **eigenschap** Ga vervolgens naar **Stuurprogramma-toets**. De **waarde**, kijk in de die wordt **verschoven**, welke 0002 in de volgende schermafbeelding is. De 0002 geeft de PhysicalDisk2 dat de opslag van toepassingen verwijst naar.

    ![VirtualDiskPropertyDetails][4]

2. Voor elke opslag van toepassingen, dump uit de bijbehorende schijven:

    Get-StoragePool - FriendlyName AMS1pooldata | Get-fysieke schijf

    ![GetStoragePool][5]

Nu kunt u deze gegevens om te koppelen aan zijn gekoppeld VHD's fysieke schijven in opslag van toepassingen.

Zodra u VHD's hebt toegewezen aan fysieke schijven in opslag van toepassingen kunt u vervolgens loskoppelen en deze naar een Premium-opslag-account via kopiëren en vervolgens koppel deze met de juiste cache-instelling. Zie het voorbeeld in de [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), stappen 8 tot en met 12. Deze stappen wordt aangegeven hoe extraheren van de configuratie van een VM bijgevoegd VHD schijf naar een CSV-bestand, kopieert de VHD's, de schijf configuratie cache-instellingen wijzigen, en ten slotte implementeren de VM als een reeks DS VM met alle gekoppelde schijven.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>VM opslag bandbreedte en VHD opslag doorvoer

De hoeveelheid opslagruimte prestaties, is afhankelijk van de opgegeven DS * VM grootte en de grootte VHD. De VMs hebben verschillende toegestane voor het aantal VHD's die kan worden gekoppeld en de maximale bandbreedte ze biedt ondersteuning voor (MB/s). Zie voor de getallen specifieke bandbreedte [VM en Cloud Service grootten voor Azure](virtual-machines-linux-sizes.md).

Verbeterde IO's / s worden bereikt met grotere schijf. Als u uw migratiepad bedenkt, moet u dit overwegen. Voor meer informatie [raadpleegt u de tabel voor IO's / s en het type](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Tot slot kunt u overwegen dat VMs hebben verschillende maximale schijf bandbreedte die ze biedt ondersteuning voor alle schijven die zijn gekoppeld. U kunt de maximale schijf bandbreedte beschikbaar voor dat formaat van de rol VM kan verzadigen bij hoge belasting. Bijvoorbeeld biedt een Standard_DS14 ondersteuning voor maximaal 512 MB/s; Daarom kunt u de bandbreedte schijf van de VM verzadigen met drie P30 schijven. Maar in dit voorbeeld de limiet van doorvoer afhankelijk van de combinatie van lezen en schrijven IOs kan worden overschreden.

## <a name="new-deployments"></a>Nieuwe implementaties

De volgende twee secties laten zien hoe u SQL Server VMs kunt implementeren naar Premium opslag. Zoals eerder hoeft u niet per se te plaatsen van de schijf OS naar Premium opslag. U kunt dit doen als u zijn willen eventuele intensief IO werkbelasting plaats op de VHD OS.

Het eerste voorbeeld bevat het gebruik van bestaande Azure-afbeeldingen. Het tweede voorbeeld ziet hoe u een aangepaste VM-afbeelding die u in een bestaand standaard opslag-account hebt gebruikt.

> [AZURE.NOTE] In deze voorbeelden wordt ervan uitgegaan dat u al een regionale VNET hebt gemaakt.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Een nieuwe VM maken met de Premium-opslag met afbeelding

Het volgende voorbeeld ziet hoe u de VHD OS naar premium opslag plaatsen en Premium opslag VHD's wilt toevoegen. U kunt echter ook de schijf OS plaatsen in een standaard-opslag-account en voeg vervolgens VHD's die zich in een Premium-opslag-account bevinden. Beide scenario's zijn gedemonstreerd.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Stap 1: Een Premium-opslag-Account maken


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Stap 2: Maak een nieuwe Cloudservice

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Stap 3: Reserveren VIP van de Cloud-Service (optioneel)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Stap 4: Een Container VM maken
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Stap 5: OS VHD in standaard of Premium opslag plaatsen
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Stap 6: VM maken
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Een nieuwe VM als wilt Premium opslag gebruiken met een aangepaste afbeelding maken

In dit scenario laat zien waar u een bestaande aangepaste afbeeldingen die zich in een standaard-opslag-account bevinden hebt. Zoals is vermeld als u wilt de VHD OS plaatsen op Premium opslag moet u de afbeelding die zich in het account standaardopslag kopiëren en deze overbrengen naar een Premium-opslag, voordat deze kan worden gebruikt. Als u een afbeelding van de on-premises hebt, kunt u ook deze methode gebruiken om te kopiëren die rechtstreeks aan de opslag van de Premium-account.

#### <a name="step-1-create-storage-account"></a>Stap 1: Opslag-Account maken
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Stap 2 Cloudservice maken
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Stap 3: De bestaande afbeelding gebruiken
U kunt een bestaande afbeelding kunt gebruiken. Of, kunt u [een afbeelding maken van een bestaande machine](virtual-machines-windows-classic-capture-image.md). Houd rekening met de afbeelding u niet hoeft te worden DS* machine machine. Nadat u de afbeelding hebt, zien de volgende stappen hoe om deze te kopiëren naar de Premium-opslag rekening met de * *Start-AzureStorageBlobCopy** PowerShell-commandlet.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Stap 4: Kopie Blob tussen opslag-Accounts
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Stap 5: Controleer regelmatig status kopiëren:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Stap 6: Afbeelding van de schijf toevoegen aan Azure schijf opslagplaats in abonnement
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] U vindt u mogelijk dat hoewel de statusrapporten als succes, er nog steeds een schijf lease foutbericht weergegeven wordt kan. In dit geval wacht ongeveer 10 minuten.

#### <a name="step-7--build-the-vm"></a>Stap 7: De VM maken
U maakt de VM hier uit uw afbeelding en twee Premium opslag VHD's koppelen:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Bestaande installaties die geen van altijd op beschikbaarheid-groepen gebruikmaakt

> [AZURE.NOTE] Voor bestaande implementaties, ziet u eerst de sectie [vereisten](#prerequisites-for-premium-storage) van dit onderwerp.

Er zijn verschillende overwegingen voor SQL Server-implementaties die gebruik niet altijd op beschikbaarheid van de groepen en degene die dit doen. Als u altijd op niet gebruikt en een bestaande zelfstandige versie van SQL Server hebt, kunt u met Premium Storage bijwerken met behulp van een nieuw cloud-service en opslag account. Houd rekening met de volgende opties:

- **Een nieuwe SQL Server-VM maken**. U kunt een nieuwe versie van SQL Server VM die gebruikmaakt van een account Premium opslag kunt maken, zoals beschreven in nieuwe implementaties. Vervolgens de back-up maken en terugzetten van SQL Server configuration en gebruiker-databases. De toepassing moet worden bijgewerkt om te verwijzen naar de nieuwe SQL Server als dit wordt geopend intern of extern. U moet alle 'afmelden bij db' objecten kopiëren als wanneer u een naast elkaar (SxS) SQL Server-migratie aan het doen bent. Dit geldt ook voor objecten zoals aanmeldingen, certificaten en gekoppelde servers.
- **Een bestaande SQL Server-VM migreren**. Dit houdt in dat de SQL Server-VM offline nemen en u deze overbrengt naar een nieuwe cloudservice, waaronder alle bijbehorende bijgevoegde VHD's kopiëren naar de opslag van de Premium-account. Wanneer u de VM online hebt, wordt de naam van de server host als voordat u verwijzen naar de toepassing. Vergeet niet dat de grootte van de bestaande schijf is van invloed op de prestatie-eigenschappen. Een schijf 400 GB wordt bijvoorbeeld afgerond op een P20. Als u weet dat u de schijfprestaties van die is niet vereist, kan u de VM als een DS reeks VM opnieuw, en bijvoegen Premium opslag VHD's van de grootte/prestaties specificatie die u nodig hebt. U kunt vervolgens loskoppelen en weer aan de SQL-DB-bestanden.

> [AZURE.NOTE] Wanneer de VHD schijven u rekening moet houden van de grootte, afhankelijk van de grootte kopiëren betekent dat welk type Premium opslagschijf worden ingedeeld in, Hiermee bepaalt u Schijfopruiming prestaties specificatie. Azure worden afgerond tot de dichtstbijzijnde schijf grootte, dus als u een 400 GB schijf hebt, dit afgerond naar een P20. Afhankelijk van uw bestaande IO vereisten van de VHD OS moet u mogelijk niet dit migreren naar een Premium-opslag-account.

Als uw SQL-Server extern wordt geopend, wordt klikt u vervolgens de cloud service VIP gewijzigd. U wordt ook moet bijwerken eindpunten, ACL's en DNS-instellingen.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Bestaande installaties die altijd op beschikbaarheidsgroepen gebruiken

> [AZURE.NOTE] Voor bestaande implementaties, ziet u eerst de sectie [vereisten](#prerequisites-for-premium-storage) van dit onderwerp.

In eerste instantie in deze sectie gaan we altijd op de interactie met Azure toegang. We vervolgens omlaag migraties in naar de twee scenario's worden afgebroken: waar sommige downtime kan worden toegestaan migraties en migraties waar u de minimale downtime moet bereiken.

Lokale SQL Server altijd op beschikbaarheidsgroepen gebruik een luisteraar ervan af on-premises implementatie die registreert een virtuele DNS-naam samen met een IP-adres dat wordt gedeeld door een of meer SQL-Servers. Wanneer clients verbinding maken zijn ze gerouteerd via de IP luisteraar ervan af met de primaire SQL Server. Dit is de server waarop de eigenaar is van de altijd op IP-resource op dat moment.

![DeploymentsUseAlways op][6]

In Microsoft Azure die u kunt slechts één IP-adres toegewezen aan een NIC in de VM hebben, dus het oog op dezelfde laag abstractieniveaus als on-premises Azure maakt gebruik van het IP-adres dat is toegewezen aan de netwerktaakverdelers intern/extern (ILB/ELB). De IP-resource die wordt gedeeld door de servers is ingesteld op de dezelfde IP als de ILB/ELB. Hiermee wordt gepubliceerd op de DNS-records en client-verkeer is toegestaan het ILB/ELB wordt doorgegeven aan de primaire SQL Server-replica. De ILB/ELB weet welke SQL Server is primaire, omdat deze sondes gebruikt om te zoeken van de resource altijd op IP. In het vorige voorbeeld, deze controleert of elk knooppunt waarop een eindpunt waarnaar wordt verwezen door de ELB/ILB, afhankelijk van wat reageert is de primaire SQL-Server.

> [AZURE.NOTE] De ILB en ELB zijn beide toegewezen aan een bepaald Azure-cloudservice, dus een cloud-migratie in Azure wordt aangegeven dat de belasting voor gelijkmatige verdeling IP verandert waarschijnlijk opleveren.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migreren altijd op implementaties waarmee sommige downtime

Er zijn twee strategieën altijd op implementaties die toestaan voor sommige downtime migreren:

1. **Meer secundaire replica's toevoegen aan een bestaand altijd op Cluster**
1. **Migreren naar een nieuwe altijd op Cluster**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. meer secundaire replica's toevoegen aan een bestaand altijd op Cluster

Een strategie is meer secundaire servers toevoegen aan de groep altijd-Klik op van beschikbaarheid. U moet deze naar een nieuwe cloudservice toevoegen en bijwerken van de luisteraar ervan af met de nieuwe laden de verdeling van IP.

##### <a name="points-of-downtime"></a>Punten van downtime:

- Cluster gegevensvalidatie.
- Altijd op failovers testen op nieuwe secundaire servers.

Als u Windows opslag van toepassingen binnen de VM voor snellere IO verwerking gebruikt, klikt u vervolgens deze komt offline tijdens een volledige Cluster validatie. De validatietest is vereist wanneer u knooppunten aan het cluster toevoegt. Hoe lang die het duurt de test wilt uitvoeren, kan variëren, dus moet u dit in uw vertegenwoordiger testomgeving om een niet-geheel exacte tijd van hoe lang deze duurt te testen.

U moet inrichten van tijd waar u handmatige failover en chaos testen op de toegevoegde knooppunten om ervoor te zorgen altijd op beschikbaarheid functies zoals verwacht kunt uitvoeren.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Moet u alle exemplaren van SQL Server waar de opslag van toepassingen zijn gebruikt stoppen voordat u de gegevensvalidatie wordt uitgevoerd.
##### <a name="high-level-steps"></a>Hoofdstappen

1. Maak twee nieuwe SQL-Servers in nieuwe cloudservice met bijgevoegde Premium opslagmedia.
1. VOLLEDIGE back-ups worden overschreven en herstellen met **NORECOVERY**.
1. Kopieer over 'afmelden bij user DB' afhankelijke objecten, zoals aanmeldingen enzovoort.
1. Nieuwe maken van een nieuwe interne laden verdeling (ILB) of gebruiken van een externe laden verdeling (ELB) en kies vervolgens laden verdeeld eindpunten instellen op beide nieuwe knooppunten.
> [AZURE.NOTE] Schakel dat alle knooppunten hebben de juiste eindpuntconfiguratie voordat u verdergaat

1. Stoppen gebruiker/Application toegang tot de SQL-Server (als u opslag van toepassingen).
1. Stoppen op alle knooppunten SQL Server-Engine Services (als u opslag van toepassingen).
1. Nieuwe knooppunten cluster en uitvoeren van de volledige gegevensvalidatie toevoegen.
1. Nadat de validatie is geslaagd, start u alle SQL Server-Services.
1. Back-up maken transactie-logboeken en herstellen van gebruikersdatabases.
1. Nieuwe knooppunten toevoegen in de groep altijd-Klik op van beschikbaarheid en plaats herhaling in **synchroon**.
1. De bron van het IP-adres van de nieuwe Cloud Service ILB/ELB via PowerShell voor altijd op op basis van het voorbeeld met meerdere locaties in de [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)toevoegen. In Windows cluster, kiest u de **mogelijke eigenaars** van de resource **IP-adres van** de oude nieuwe knooppunten. Zie het gedeelte 'Toe te voegen IP-adres Resource op hetzelfde Subnet' van de [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
1. Failover naar een van de nieuwe knooppunten.
1. Controleer de nieuwe knooppunten automatisch Failover Partners en test failovers.
1. Oorspronkelijke knooppunten uit beschikbaarheid van de groep verwijderen.

##### <a name="advantages"></a>Voordelen

- Nieuwe SQL-Servers kan worden getest (SQL Server en toepassingen) voordat ze worden toegevoegd aan altijd op.
- U kunt de VM-grootte wijzigen en aanpassen van de opslagruimte aan uw exacte vereisten. Echter is het nuttig om alle paden van de SQL-bestand dezelfde behouden.
- U kunt bepalen wanneer de overdracht van de DB back-ups op de secundaire replica's worden gestart. Dit verschilt van kopiëren met behulp van Azure **Start-AzureStorageBlobCopy** commandlet VHD's, omdat dit een asynchroon kopiëren.

##### <a name="disadvantages"></a>Nadelen
- Wanneer u een Windows-opslag van toepassingen gebruikt, moet u er Cluster downtime tijdens het volledige Cluster validatie voor de nieuwe extra knooppunten is.
- Afhankelijk van de SQL Server-versie en de bestaande aantal secundaire replica's, kunnen mogelijk niet meer secundaire replica's toevoegen zonder te verwijderen van bestaande secundaire servers.
- Er zijn SQL gegevens doorverbinden lang tijdens het instellen van de secundaire servers.
- Is er extra kosten tijdens de migratie terwijl er nieuwe computers waarop een parallel.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. migreren naar een nieuwe altijd op Cluster

Een andere strategie is een geheel nieuwe altijd op Cluster maken met de gloednieuwe knooppunten in nieuwe cloudservice en leid vervolgens om de clients als u wilt gebruiken.

##### <a name="points-of-downtime"></a>Punten van downtime

Er is downtime wanneer u toepassingen en gebruikers overbrengen naar de nieuwe altijd op luisteraar ervan af. Afhankelijk van de downtime:

- De tijd die u hebt gemaakt om te zetten definitief transactie-logboekbestanden naar databases op nieuwe servers.
- De tijd bijwerken clienttoepassingen gebruik van nieuwe altijd op luisteraar ervan af.

##### <a name="advantages"></a>Voordelen

- U kunt de werkelijke productieomgeving, SQL Server, maken en testen van OS wijzigingen.
- Hebt u de optie voor het aanpassen van de opslag en grootte van VM mogelijk te verminderen. Dit kan resulteren in kosten wilt verkleinen.
- U kunt uw versie van SQL Server of een versie bijwerken tijdens dit proces. U kunt ook het besturingssysteem upgraden.
- Het vorige altijd op Cluster kan fungeren als doel effen terugdraaien.

##### <a name="disadvantages"></a>Nadelen

- U moet de DNS-naam van de luisteraar ervan af wijzigen als u wilt dat beide altijd op-clusters tegelijk worden uitgevoerd. Hiermee voegt u beheer van de belasting tijdens de migratie als tekenreeksen van client-toepassingen moeten overeenkomen met de nieuwe naam voor de luisteraar ervan af.
- Een om synchronisatie tussen de twee omgevingen om ze zo dicht mogelijk is om te minimaliseren van de laatste synchronisatie vereisten voor de migratie te houden, moet u implementeren.
- Er wordt toegevoegd kosten tijdens de migratie terwijl u de nieuwe omgeving actief hebt.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migreren altijd op implementaties voor minimale downtime

Er zijn twee strategieën voor het migreren altijd op implementaties voor minimale uitvaltijd:

1. **Gebruikmaken van een bestaande secundair: één Site**
1. **Gebruikmaken van bestaande secundaire Replica(s): meerdere locaties**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. gebruikmaken van een bestaande secundair: één Site

Een strategie voor minimale uitvaltijd is een bestaande cloud secundaire maken en verwijderen van de huidige cloudservice. Vervolgens de VHD's kopiëren naar het nieuwe opslag van de Premium-account en de VM maken in de nieuwe cloudservice. Pas de luisteraar ervan af in cluster en overname bij storing.

##### <a name="points-of-downtime"></a>Punten van downtime

- Er is downtime wanneer u het laatste knooppunt met het eindpunt van taakverdeling bijwerkt.
- Uw client opnieuw verbinden mogelijk worden uitgesteld afhankelijk van de client/DNS-configuratie.
- Er is extra downtime als u de groep altijd op Cluster offline naar de IP-adressen verwisselt kunt tillen. U kunt dit voorkomen door een OR afhankelijkheid en mogelijke eigenaren voor de toegevoegde IP-adres resource. Zie het gedeelte 'Toe te voegen IP-adres Resource op hetzelfde Subnet' van de [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).

> [AZURE.NOTE] Als u het toegevoegde knooppunt wilt naar partake in als altijd op failover-Partner, moet u een eindpunt Azure met een verwijzing naar de laden verdeeld instellen toevoegen. Wanneer u de opdracht **Toevoegen-AzureEndpoint** hiervoor uitvoert, kunnen huidige verbindingen met het openen, maar nieuwe verbindingen met de luisteraar ervan af blijven niet worden gemaakt totdat de taakverdeling heeft bijgewerkt. In dit testen is zichtbaar voor naar laatste 90-120seconds, dit moet worden getest.

##### <a name="advantages"></a>Voordelen

- Geen extra kosten die zijn gemaakt tijdens de migratie.
- Een een-op-migratie.
- Minder complexiteit.
- Verbeterde IO's / s staat van Premium opslag SKU's. Wanneer de schijven van de VM losgemaakt zijn en een 3e gekopieerd naar de nieuwe cloudservice derden, kan hulpmiddel worden gebruikt om de grootte VHD, waarmee hoger doorvoercapaciteit. Zie voor groeiende VHD formaten, deze [forum discussie](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Nadelen

- Er is een tijdelijke kwaliteitsverlies HA en DR tijdens de migratie.
- Als dit een migratie 1:1 is, moet u een minimum VM grootte biedt ondersteuning voor uw aantal VHD's, zodat u mogelijk niet uw VMs afslanken gebruiken.
- Dit scenario gebruikt u de commandlet Azure **Start-AzureStorageBlobCopy** welke asynchroon is. Er is geen SLA op kopie voltooid. De tijd van de exemplaren varieert, terwijl dit hangt af van wachten in wachtrij die dit wordt ook zijn afhankelijk van de hoeveelheid gegevens om over te brengen. Als de overdracht is gaat u naar een andere Azure datacenter dat ondersteuning biedt voor Premium-opslag in een andere regio Hiermee verhoogt u de tijd kopiëren. Als u alleen 2 knooppunten hebt, kunt u een mogelijke risicobeperking geval de kopie langer duren dan in testen. Hierbij kan de volgende ideeën.
    - Voeg een tijdelijke 3e SQL Server-knooppunt voor HA vóór de migratie met overeengekomen downtime.
    - Voer de migratie buiten Azure gepland onderhoud.
    - Controleer of dat u uw clusterquorum correct hebt geconfigureerd.  

##### <a name="high-level-steps"></a>Hoofdstappen

Dit document bevat niet laten zien een volledig complete voorbeeld, maar de [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) bevat informatie die kunnen worden gebruikt om uit te voeren dit.

![MinimalDowntime][8]

- Verzamel schijfconfiguratie en verwijder het knooppunt (bijgevoegde VHD's niet verwijderd).
- Maak een Premium-opslag-account en kopieer VHD's uit het standaardopslag-account
- Maken van nieuwe cloudservice en implementeer deze opnieuw de VM SQL2 in die cloudservice. Maak de VM met de gekopieerde oorspronkelijke OS VHD en de gekopieerde VHD's koppelen.
- ILB configureren / ELB en eindpunten toevoegen.
- Update luisteraar ervan af door een:
    - De altijd op groep offline nemen en de altijd op luisteraar ervan af bijwerken met nieuwe ILB / ELB IP-adres.
    - Of de IP-adres resource van nieuwe Cloud Service ILB/ELB via PowerShell in Windows cluster toe te voegen. Vervolgens instellen de mogelijke eigenaars van de resource IP-adres naar de gemigreerde knooppunt, SQL2, en dit als of afhankelijkheid in de naam van het netwerk. Zie het gedeelte 'Toe te voegen IP-adres Resource op hetzelfde Subnet' van de [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
- Controleer de DNS-configuratie/doorgiftetaak aan de clients.
- SQL1 VM migreren en doorloop de stappen 2 tot en met 4.
- Als stappen 5ii gebruikt, voegt u SQL1 als mogelijke eigenaar voor de toegevoegde IP-adres Resource
- Test failovers.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. gebruikmaken van bestaande secundaire replica(s): meerdere locaties

Als er knooppunten in meer dan één Azure datacenter (domeincontroller) of als u een hybride omgeving hebt, kunt klikt u vervolgens u een configuratie altijd op in deze omgeving te minimaliseren ten downtime.

De methode werkt het wijzigen van de synchronisatie altijd op naar synchroon voor de on-premises of secundaire Azure domeincontroller, en klik vervolgens failover via naar SQL Server. Vervolgens de VHD's kopiëren naar een Premium-opslag-account en implementeer deze opnieuw de computer naar een nieuwe cloudservice. Werk de luisteraar ervan af en vervolgens mislukt terug.

##### <a name="points-of-downtime"></a>Punten van downtime

De downtime bestaat uit de tijd foutherstel naar het alternatieve domeincontroller en weer. Ook afhankelijk van de client/DNS-configuratie en uw client opnieuw verbinden mag worden uitgesteld.
Houd rekening met het volgende voorbeeld van een hybride altijd op configuratie:

![MultiSite1][9]

##### <a name="advantages"></a>Voordelen

- U kunt bestaande infrastructuur gebruiken.
- U hebt de optie voor de opslag Azure op de domeincontroller DR Azure eerst vóór de upgrade.
- De opslag DR Azure domeincontroller kan opnieuw worden geconfigureerd.
- Er is ten minste twee failovers tijdens de migratie, met uitzondering van test failovers.
- U hoeft niet te verplaatsen van SQL Server-gegevens met back-up en herstellen.

##### <a name="disadvantages"></a>Nadelen

- Afhankelijk van de clienttoegang tot SQL Server, kunnen er wachttijd wanneer SQL Server wordt uitgevoerd op een alternatief domeincontroller met de toepassing.
- Het is mogelijk dat de kopie-tijd van VHD's met Premium storage lang. Dit kan van invloed zijn op uw beslissing op of u wilt het knooppunt in de groep beschikbaarheid behouden. Overweeg dit voor momenten waarop log intensief werk laadtijd tijdens de migratie uitvoert vereist, is aangezien het primaire knooppunt moet de niet-gerepliceerde transacties behouden in het transactielogboek. Daarom kan dit aanzienlijk groeien.
- Dit scenario gebruikt u de commandlet Azure **Start-AzureStorageBlobCopy** welke asynchroon is. Er is geen SLA op voltooid. De tijd van de exemplaren varieert, terwijl dit hangt af van wachten in wachtrij, wordt deze afhankelijk is van de hoeveelheid gegevens om over te brengen. Daarom u slechts één knooppunt hebt in uw 2e Datacenter, u moet ingrijpen risicobeperking geval de kopie langer duren dan in testen. Hierbij kan de volgende ideeën.
    - Voeg een tijdelijke 2e SQL-knooppunt voor HA vóór de migratie met overeengekomen downtime.
    - Voer de migratie buiten Azure gepland onderhoud.
    - Controleer of dat u uw clusterquorum correct hebt geconfigureerd.

Dit scenario wordt ervan uitgegaan dat u uw installatie beschreven hebt en weten hoe de opslag is toegewezen om het te wijzigen voor optimale schijf cache-instellingen.

##### <a name="high-level-steps"></a>Hoofdstappen
![Multisite2][10]

- Controleer de on-premises implementatie / alternatieve Azure domeincontroller de SQL Server-primaire en kunt u de andere Auto failover-Partner (AFP).
- Verzamel informatie over de configuratie van de schijf over SQL2 en verwijder het knooppunt (bijgevoegde VHD's niet verwijderd).
- Maak een Premium-opslag-account en kopieer VHD's van de standaard-opslag-account.
- Een nieuwe cloudservice maken en de VM SQL2 maken met de premies schijven die zijn gekoppeld.
- ILB configureren / ELB en eindpunten toevoegen.
- Bijwerken van de altijd op luisteraar ervan af met nieuwe ILB / ELB IP-adres en test failover.
- Controleer de DNS-configuratie.
- Wijzig de AFP in SQL2, SQL1 migreren en doorloop de stappen 2 tot en met 5.
- Test failovers.
- De AFP Ga terug naar SQL1 en SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Bijlage: Een meerdere locaties altijd op Cluster migreren naar de Premium-opslag

De rest van dit onderwerp biedt een gedetailleerd voorbeeld van een site met meerdere altijd op cluster converteren naar Premium opslag. Ook converteert naar een interne taakverdeling (ILB) de luisteraar ervan af van het gebruik van een externe taakverdeling (ELB).

### <a name="environment"></a>Omgeving

- Windows 2k 12 / SQL 2k 12
- 1 DB-bestanden op SP
- 2 x opslag van toepassingen per knooppunt

![Appendix1][11]

### <a name="vm"></a>VM:

In dit voorbeeld gaan we demonstreren vanuit een ELB verplaatsen naar de ILB. ELB is beschikbaar voor ILB, zodat dit ziet u hoe u overschakelen naar dit tijdens de migratie.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Stappen van de pre: Verbinding maken met abonnement

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Stap 1: Maak nieuwe opslag-Account en Cloud Service
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Stap 2: De toegestane resources: fouten vergroten<Optional>
Bepaalde bronnen die deel uitmaakt van uw groep altijd-Klik op van beschikbaarheid zijn er limieten op hoeveel fouten die in een periode optreden kunnen, waarbij de cluster-service wordt geprobeerd om de resourcegroep opnieuw te starten. Het wordt aanbevolen dat u deze verhogen terwijl u zijn doorlopen deze procedure sinds als u niet handmatig failover en de inwerkingtreding failovers door af machines die u deze dicht bij deze limiet krijgen kunt te sluiten.

Moet worden beperkt dubbele de afschrijving mislukt, hiervoor in Failoverclusterbeheer, Ga naar de eigenschappen van de resourcegroep altijd op:

![Appendix3][13]

Wijzig de maximale fouten tot en met 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Stap 3: Toevoeging IP-adres resource voor Cluster groep<Optional>

Als u slechts één IP-adres voor de groep Cluster hebt en dit is uitgelijnd op de cloud-subnet, houd er rekening mee, als u per ongeluk offline alle knooppunten in de cloud in dat netwerk klikt u vervolgens de resource Cluster IP halen- en Cluster Network Name worden niet online komt. In dit het geval kan updates naar andere clusterbronnen.

#### <a name="step-4-dns-configuration"></a>Stap 4: DNS-configuratie

Willen implementeren van een soepele overgang is afhankelijk van hoe DNS wordt gebruikt en bijgewerkt.
Wanneer u altijd op is geïnstalleerd, wordt een Windows-Cluster resourcegroep, als u Failoverclusterbeheer opent, ziet u dat ten minste drie middelen beschikken, zijn de twee die het document naar verwijst:

- Virtuele Network Name (VNN) – Dit is de naam van de DNS-die client verbinding maken met wanneer u verbinding maakt met SQL-Servers via altijd op willen.
- IP Address Resource: dit is het IP-adres dat is gekoppeld aan de VNN, u kunt meer dan één hebben en in de configuratie van een meerdere locaties hebt u een IP-adres per site/subnet.

Wanneer u verbinding maakt met SQL Server, de SQL Server-Client stuurprogramma wordt opgehaald van de DNS-records die zijn gekoppeld aan de luisteraar ervan af en probeert te verbinden met elke altijd op deze IP-adres is gekoppeld, onder wordt besproken enkele factoren die dit kunnen beïnvloeden.

Het aantal gelijktijdige DNS-records die zijn gekoppeld aan de naam van de luisteraar ervan af afhankelijk is niet alleen het aantal IP-adressen die is gekoppeld, maar de ' RegisterAllIpProviders'setting in Failoverclustering voor de resource altijd op VNN.

Wanneer u altijd op in Azure implementeert er zijn verschillende stappen voor het maken van de luisteraar ervan af en IP-adressen, u moet voor het handmatig configureren van de 'RegisterAllIpProviders' 1, dit is anders naar een aan premises implementatie van altijd op waarop deze al is ingesteld op 1.

Als 'RegisterAllIpProviders' 0 is, klikt u vervolgens ziet alleen u een DNS-record in DNS die is gekoppeld aan de luisteraar ervan af:

![Appendix4][14]

Als 'RegisterAllIpProviders' 1 is:

![Appendix5][15]

De onderstaande code wordt dump uit de VNN-instellingen en stel deze in voor u, opmerking, zodat de wijziging van kracht u moet de VNN offline te zetten en schakel deze weer online, deze duurt de luisteraar ervan af offline veroorzaakt door client connectivity verstoringen.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

In een latere migratiestap moet u de altijd op luisteraar ervan af met een bijgewerkte IP-adres dat verwijst naar een taakverdeling, bijwerken, dient een IP-adres resource verwijderen en de toevoeging. Na het IP-bijwerken moet u zorgen dat het nieuwe IP-adres is bijgewerkt in DNS-Zone en dat de clients hun lokale DNS-cache wilt bijwerken.

Als uw klanten bevinden zich in een ander netwerksegment en verwijzen naar een andere DNS-server, moet u rekening houden wat gebeurt er over DNS-Zone overbrengen tijdens de migratie terwijl de toepassing opnieuw tijd worden beperkt door ten minste de Zone overbrengen tijd van een nieuwe IP-adressen voor de luisteraar ervan af. Als u onder tijdsbeperking hier, u moet bespreken en testen dwingt een zoneoverdracht incrementele met uw Windows-teams af, en ook de DNS-host-record hebt naar een lagere Time To Live (TTL) opgeslagen, zodat de clients bijwerken. Zie [Incrementele zoneoverdrachten](https://technet.microsoft.com/library/cc958973.aspx) en [Begin-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx)voor meer informatie.

Is al dan niet standaard de TTL voor DNS-Record die is gekoppeld aan de luisteraar ervan af in altijd op in Azure 1200 seconden. U kunt dit verkleinen als u onder tijd beperking tijdens de migratie om ervoor te zorgen de clients hun DNS bijwerken met de bijgewerkte IP-adres voor de luisteraar ervan af. U kunt zien en de configuratie door de configuratie van de VNN storten wijzigen:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Houd er rekening mee, hoe lager de 'HostRecordTTL', een hogere hoeveelheid DNS-verkeer zal plaatsvinden.

##### <a name="client-application-settings"></a>Client-toepassingsinstellingen

Als uw SQL-clienttoepassing de .net 4.5 ondersteunt SQLClient, moet u de beschikking over ' MULTISUBNETFAILOVER = TRUE' trefwoord, dit moeten worden toegepast als deze kan voor snellere verbinding met SQL altijd op beschikbaarheid van de groep tijdens een overname wordt aanbevolen. Deze alle IP-adressen die is gekoppeld aan de altijd op luisteraar ervan af parallel middels en voert een meer agressieve TCP opnieuw verbindingssnelheid tijdens een een overname.

Voor meer informatie over de bovenstaande instellingen, raadpleegt u de [MultiSubnetFailover trefwoord en functies die zijn gekoppeld](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Zie ook [SqlClient ondersteuning voor hoge beschikbaarheid, herstel](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Stap 5: Cluster quorum instellingen

Als u gaat te hebben van ten minste één SQL Server omlaag per keer, moet u de instelling van de quorum cluster wijzigen als bestand delen getuige (FSW) gebruikt met 2 knooppunten, moet u het quorum te knooppunt meeste toestaan en dynamische stemmen set en dit is om te staan voor één knooppunt moet blijven staand.


    Set-ClusterQuorum -NodeMajority  

Voor meer informatie over het beheren en configureren van het clusterquorum, raadpleegt u de [configureren en beheren het Quorum in een failovercluster van Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Stap 6: Extraheren bestaande eindpunten en ACL 's
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Sla deze naar een tekstbestand.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Stap 7: Failover Partners en herhaling modi wijzigen

Als u meer dan 2 SQL-Servers hebt, moet u de overname van een andere secundair in een andere domeincontroller of on-premises wijzigen in 'Synchroon' en kunt u een automatische failover-Partner (AFP), is dit zodat u voor het behoud van HA terwijl u wijzigingen wilt aanbrengen. U kunt dit doen via TSQL van wijzigen door SSMS:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Stap 8: Secundaire VM verwijderen uit de cloudservice

U moet eerst een secundaire knooppunt cloud migreren van worden plan als dit momenteel primaire is, moet u een handmatige failover initiëren.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Stap 9: Schijf caching van instellingen in het CSV-bestand wijzigen en opslaan

Voor gegevensvolumes moeten deze zijn ingesteld op alleen-lezen.

Voor TLOG volumes moeten deze zijn ingesteld op geen.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Stap 10: Kopie VHD 's
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



U kunt de status van de kopie van de VHD's aan de opslag van de Premium-account controleren:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Wacht totdat alle dit zijn vastgelegd als succes.

Voor informatie voor afzonderlijke BLOB's:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Stap 11: Register OS Schijfopruiming

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Stap 12: Secundaire importeren in nieuwe cloudservice

De onderstaande code ook gebruikt de toegevoegde optie als hier kunt u de computer importeren en de retainable VIP gebruiken.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Stap 13: Maak ILB op nieuwe Cloud Svc toevoegen laden verdeeld eindpunten en ACL's
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>Stap 14: Bijwerken altijd op
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Nu Verwijder de oude cloudservice IP-adres.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Stap 15: DNS-updates controleren

U moet nu DNS-Servers controleren of uw SQL Server-client-netwerken en zorg ervoor dat de hostrecord extra voor de toegevoegde IP-adres heeft toegevoegd als u een cluster. Als deze DNS-servers niet bijgewerkt, kunnen u dwingt de overdracht van een DNS-Zone af en zorg ervoor dat de clients in er subnet kan worden omgezet in beide altijd op IP-adressen zijn, wordt dit is dus u hoeft niet te wachten om door automatische DNS-replicatie.

#### <a name="step-16-reconfigure-always-on"></a>Stap 16: Altijd configureren, klik op

Nu wacht u totdat de secundaire dat knooppunt die volledig opnieuw te synchroniseren met het knooppunt on-premises en overschakelen naar knooppunt synchroon replicatie en kunt u de AFP zijn gemigreerd.  

#### <a name="step-17-migrate-second-node"></a>Stap 17: Tweede knooppunt migreren
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Stap 18: Schijf caching van instellingen in het CSV-bestand wijzigen en opslaan

Voor gegevensvolumes moeten deze zijn ingesteld op alleen-lezen.

Voor TLOG volumes moeten deze zijn ingesteld op geen.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Stap 19: Nieuw onafhankelijke opslag-Account voor secundaire knooppunt maken
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Stap 20: Kopie VHD 's
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


U kunt de status van de kopie VHD op alle VHD's controleren: ForEach ($disk in $diskobjects) {$lun = $disk. LUN $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Schijflabel $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Wacht totdat alle dit zijn vastgelegd als succes.

Voor informatie voor afzonderlijke BLOB's: #Check induvidual blob status Get-AzureStorageBlobCopyState-Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd"-Container $containerName-Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Stap 21: Register OS Schijfopruiming
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Stap 22: Laden toevoegen verdeeld eindpunten en ACL's
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Stap 23: Test failover

U moet nu de werkdruk in het gemigreerde knooppunt synchroniseren met de on-premises altijd op knooppunt, plaatst u deze in synchroon replicatie-modus en wacht totdat deze wordt gesynchroniseerd. Vervolgens failover van on-premises naar het eerste knooppunt gemigreerd, namelijk de AFP. Zodra die heeft gewerkt, moet u het laatste gemigreerde knooppunt wijzigen in de AFP.

U moet testen failovers tussen alle knooppunten en hoewel chaos tests om ervoor te zorgen failovers werk verwacht and in een tijdige manor uit te voeren.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Stap 24: Terugplaatsen cluster quorum instellingen / DNS TTL / Failover Pntrs / synchronisatie-instellingen
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Bron van het IP-adres op hetzelfde Subnet toevoegen

Als u alleen 2 SQL-Servers hebt en deze te migreren naar een nieuwe cloudservice wilt, maar wilt houden op hetzelfde subnet, kunt u voorkomen dat offline de luisteraar ervan af duurt verwijderen van het oorspronkelijke altijd op IP-adres en het nieuwe IP-adres toevoegen. Als u de VMs naar een ander subnet migreert moet u niet hiervoor omdat er een extra clusternetwerk dat verwijst naar dat subnet.

Nadat u de gemigreerde secundaire opgeroepen en in de nieuwe resource IP-adres voor de nieuwe cloudservice voordat u de bestaande primaire failover hebt toegevoegd, moet u binnen de Cluster Failover Manager van deze stappen uitvoeren:

Als u wilt toevoegen in het IP-adres, raadpleegt u de [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), stap 14.

1. Voor de huidige IP-adres resource, de eigenaar van de mogelijke naar 'Bestaande primaire SQL Server', in het onderstaande voorbeeld 'dansqlams4' te wijzigen:

    ![Appendix13][23]

1. Voor de nieuwe resource IP-adres, moet u de eigenaar van de mogelijke naar 'Migrated secundaire SQL Server', in het onderstaande voorbeeld 'dansqlams5' wijzigen:

    ![Appendix14][24]

1. Als dit is ingesteld kunt u en wanneer het laatste knooppunt wordt gemigreerd de mogelijke eigenaars moet worden bewerkt, zodat het knooppunt dat wordt toegevoegd als mogelijke eigenaar:

    ![Appendix15][25]

## <a name="additional-resources"></a>Aanvullende informatie
- [Azure Premium-opslag](../storage/storage-premium-storage.md)
- [Virtuele Machines](https://azure.microsoft.com/services/virtual-machines/)
- [SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
