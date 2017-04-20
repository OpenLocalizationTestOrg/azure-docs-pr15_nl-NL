<properties
 pageTitle="STER uitvoeren-CCM + met HPC Pack op Linux VMs | Microsoft Azure"
 description="Een Microsoft HPC Pack cluster op Azure implementeren en uitvoeren van een sterretje-CCM + taak op meerdere Linux knooppunten via een netwerk RDMA berekenen."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>STER uitvoeren-CCM + met Microsoft HPC Pack op een RDMA Linux cluster in Azure wordt aangegeven
In dit artikel leest u hoe u een Microsoft HPC Pack cluster op Azure en -klaar implementeren een [CD-adapco sterretje-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) taak op meerdere Linux berekeningscluster knooppunten onderling met InfiniBand.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack biedt functies als u wilt uitvoeren van tal van grootschalige HPC en parallelle toepassingen, waaronder MPI-toepassingen, op clusters van Microsoft Azure virtuele machines. HPC Pack ondersteunt ook actieve Linux HPC toepassingen op Linux berekeningscluster-knooppunt VMs die zijn geïmplementeerd in een cluster HPC Pack. Knooppunten met HPC Pack berekenen, raadpleegt u [aan de slag met Linux berekeningscluster knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md)voor een introductie over het gebruik van Linux.

## <a name="set-up-an-hpc-pack-cluster"></a>Een cluster HPC Pack instellen
Download de HPC Pack IaaS implementatie-scripts vanuit het [Downloadcentrum](https://www.microsoft.com/en-us/download/details.aspx?id=44949) en lokaal uitpakken.

Azure PowerShell is vereist. Als u PowerShell niet is geconfigureerd op uw lokale computer, leest u het artikel [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

Op het moment van deze schrijven zijn de Linux-afbeeldingen van Azure Marketplace (die de InfiniBand-stuurprogramma's bevat voor Azure) voor SLES 12, CentOS 6.5 en CentOS 7.1. In dit artikel is gebaseerd op het gebruik van SLES 12. Als u wilt ophalen van de naam van alle Linux-afbeeldingen die ondersteuning bieden voor HPC in de Marketplace, kunt u het volgende PowerShell-opdracht uitvoeren:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

De uitvoer bevat de locatie waarin deze afbeeldingen beschikbaar zijn en de naam van de afbeelding (**ImageName**) moet worden gebruikt in de implementatiesjabloon later.

Voordat u het cluster implementeert, die u moet maken van een sjabloonbestand HPC Pack-implementatie. Omdat we een kleine mogelijk doelgroepen bent, wordt het hoofd knooppunt de domeincontroller en wordt gehost in een lokale SQL-database.

De volgende sjabloon wordt een hoofd knooppunt implementeren, een XML-bestand met de naam **MyCluster.xml**maken en de waarden van **SubscriptionId**, **StorageAccount**, **locatie**, **VMName**en **servicenaam** vervangen door uw etiketten.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Start het hoofd-knooppunt maken door de PowerShell-opdracht in een opdrachtprompt met verhoogde bevoegdheid uit te voeren:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Het hoofd knooppunt moet klaar zijn na 20 tot 30 minuten. U kunt verbinding maken met deze van de Azure-portal door te klikken op het pictogram van het **verbinden** van de virtuele machine.

Mogelijk moet u uiteindelijk de DNS-doorstuurservers oplossen. Klik hiervoor DNS-beheer te starten.

1.  Met de rechtermuisknop op de naam van de server in DNS-beheer, selecteert u **Eigenschappen**en klik vervolgens op het tabblad **doorstuurservers** .

2.  Klik op de knop **bewerken** als u wilt een doorstuurservers verwijderen en klik vervolgens op **OK**.

3.  Zorg ervoor dat het selectievakje **hints voor de hoofdmap gebruiken als er geen doorstuurservers beschikbaar zijn** in is geselecteerd en klik vervolgens op **OK**.

## <a name="set-up-linux-compute-nodes"></a>Linux berekeningscluster knooppunten instellen
U implementeren de Linux berekeningscluster knooppunten met behulp van dezelfde implementatiesjabloon die u hebt gebruikt om het hoofd knooppunt te maken.

Het bestand **MyCluster.xml** van uw lokale computer kopiëren naar het hoofd knooppunt en de tag **NodeCount** bijwerken met het aantal knooppunten die u wilt implementeren (< = 20). Zorg ervoor dat voldoende cores beschikbaar zijn in de quota voor uw Azure, omdat elk exemplaar A9 16 cores in uw abonnement gebruikt. Als u wilt meer VMs gebruiken in hetzelfde budget, kunt u A8-exemplaren (8-cores) in plaats van A9.

Klik op het knooppunt hoofd kopieert u de HPC Pack IaaS implementatie-scripts.

Voer de volgende Azure PowerShell-opdrachten in een opdrachtprompt met verhoogde bevoegdheid:

1.  Voer **Toevoegen-AzureAccount** verbinding maken met uw Azure-abonnement.

2.  Als u meerdere abonnementen hebt, voert u **Get-AzureSubscription** u kunt ze.

3.  Een standaardabonnement instellen door te voeren de **Selecteer-AzureSubscription - SubscriptionName xxxx-standaard** opdracht.

4.  Uitvoeren **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** om te beginnen implementeert Linux knooppunten berekenen.

    ![Implementatie van hoofd-knooppunt in actie][hndeploy]

Open het hulpmiddel HPC Pack Cluster Manager. Na een paar minuten weergegeven Linux berekeningscluster knooppunten regelmatig in de lijst met knooppunten berekeningscluster. Met de implementatie van klassieke modus, worden IaaS VMs opeenvolging gemaakt. Dus als het aantal knooppunten belangrijk is dat ze al geïmplementeerd kan lang duren voordat een aanzienlijk.

![Linux knooppunten in HPC Pack Cluster Manager][clustermanager]

Nu dat alle knooppunten goed werken in het cluster, zijn er extra infrastructuur instellingen om te maken.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Een bestandsshare van Azure voor Windows en Linux knooppunten instellen
U kunt de service Azure-bestand gebruiken om op te slaan scripts, toepassingspakketten en gegevensbestanden. Azure-bestand bevat CIFS mogelijkheden boven op Azure-blobopslag als een permanente store. Vergeet niet dat dit niet de meest scalable oplossing is, maar dit is de eenvoudigste en geen speciale VMs nodig.

Maak een bestandsshare van Azure volgens de instructies in het artikel [aan de slag met Azure bestandsopslag in Windows](..\storage\storage-dotnet-how-to-use-files.md).

De naam van uw account opslag behouden als **saname**, de naam van het bestand delen als **sharenaam**en de accountsleutel opslag als **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Het koppelen van de Azure bestandsshare op het hoofd knooppunt
Open een opdrachtprompt met verhoogde bevoegdheid en voer de volgende opdracht voor de opslag van de referenties in de lokale computer kluis:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Klik als u wilt koppelen op het Azure bestandsshare, uitvoeren:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Het koppelen van de Azure bestandsshare op Linux berekeningscluster knooppunten
Een handig hulpmiddel dat wordt geleverd met HPC Pack is het hulpprogramma clusrun. U kunt dit hulpprogramma voor de opdrachtregel dezelfde opdracht tegelijk uitvoeren op een reeks berekeningscluster knooppunten. In ons geval wordt deze gebruikt om te koppelen van de Azure bestandsshare en deze opnieuw opstarten worden bewaard blijven behouden.
Voer de volgende opdrachten in een opdrachtprompt met verhoogde bevoegdheid op het hoofd knooppunt.

De gekoppelde map maken:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

De Azure bestandsshare koppelen:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Persistent de optie koppeling voor delen:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Installeren sterretje-CCM +
Azure VM exemplaren A8 en A9 bieden ondersteuning voor InfiniBand en RDMA mogelijkheden. De kernel-stuurprogramma's waarmee deze mogelijkheden zijn beschikbaar voor Windows Server 2012 R2, SUSE 12 CentOS 6.5 en CentOS 7.1 afbeeldingen in de Azure Marketplace. Microsoft MPI en Intel MPI (versie 5.x) zijn de twee MPI bibliotheken die ondersteuning bieden voor deze stuurprogramma's in Azure wordt aangegeven.

CD-adapco sterretje-CCM + 11.x los en later, wordt meegeleverd met Intel MPI versie 5.x, zodat InfiniBand ondersteuning voor Azure opgenomen is.

De Linux64 krijgen sterretje-CCM + pakket van de [CD-adapco portal](https://steve.cd-adapco.com). In ons geval we versie 11.02.010 in gemengde precisie gebruikt.

Klik op het hoofd knooppunt in het delen van de Azure bestand **/hpcdata** ook een shellscript maken met de naam **setupstarccm.sh** met de volgende inhoud. Dit script wordt uitgevoerd op elk knooppunt berekeningscluster voor het instellen van sterretje-CCM + lokaal.

#### <a name="sample-setupstarcmsh-script"></a>Voorbeeldscript voor setupstarcm.sh
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Nu, voor het instellen van sterretje-CCM + op alle uw Linux berekenen van knooppunten, open een opdrachtprompt met verhoogde bevoegdheid en voer de volgende opdracht:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Terwijl de opdracht wordt uitgevoerd, kunt u het CPU-gebruik controleren met behulp van de heatmap van Cluster Manager. Na een paar minuten alle knooppunten moeten correct zijn ingesteld.

## <a name="run-star-ccm-jobs"></a>STER uitvoeren-CCM + taken
HPC Pack wordt gebruikt voor de taak scheduler mogelijkheden om te kunnen uitvoeren sterretje-CCM + taken. Hiervoor moeten we de ondersteuning van een paar scripts die worden gebruikt voor de taak starten en uitvoeren van sterretje-CCM +. De invoergegevens wordt bewaard op de Azure bestandsshare eerste voor eenvoudig.

Het volgende PowerShell-script wordt gebruikt in de wachtrij plaatsen een STER-CCM + taak. Het kost drie argumenten:

*  Naam van het model

*  Het aantal knooppunten moet worden gebruikt

*  Het aantal cores op elk knooppunt moet worden gebruikt

Omdat sterretje-CCM + kunt doorvoeren de geheugenbandbreedte, is het meestal beter werken met minder kernen per berekeningscluster knooppunten en toevoegen van nieuwe knooppunten. De exacte aantal kernen per knooppunt, is afhankelijk van de processorfamilie en de snelheid aan elkaar.

De knooppunten exclusief voor de taak zijn toegewezen en kunnen niet worden gedeeld met andere taken. De taak is niet rechtstreeks als een taak MPI gestart. De **runstarccm.sh** -shellscript wordt MPI starten.

De invoer model en het script **runstarccm.sh** worden opgeslagen in de **/hpcdata** delen die eerder is gekoppeld.

Logboekbestanden heten met de taak-ID en zijn opgeslagen in de **/hpcdata delen**, samen met het sterretje-CCM + uitvoer bestanden.


#### <a name="sample-submitstarccmjobps1-script"></a>Voorbeeldscript voor SubmitStarccmJob.ps1
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
**Runner.java** vervangen door uw voorkeur sterretje-CCM + Java model startprogramma voor Apps en logboekregistratie code.

#### <a name="sample-runstarccmsh-script"></a>Voorbeeldscript voor runstarccm.sh
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

In ons test, we een token Power voor bellen-licentie gebruikt. Voor die u hebt om de omgevingsvariabele **$CDLMD_LICENSE_FILE** in te stellen **1999@flex.cd-adapco.com** en de sleutel in de optie **- podkey** van de opdrachtregel.

Na enkele initialisatie haalt het script--uit de **$CCP_NODES_CORES** omgevingsvariabelen die HPC Pack ingesteld--de lijst met knooppunten maken van een hostfile die het startpictogram voor het MPI wordt gebruikt. Deze hostfile bevat de lijst met berekeningscluster knooppuntnamen die worden gebruikt voor de taak, één naam per regel.

De indeling van **$CCP_NODES_CORES** volgt deze notatie:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Waarbij geldt:

* `<Number of nodes>`is het aantal knooppunten die zijn toegewezen aan deze taak.

* `<Name of node_n_...>`is de naam van elk knooppunt dat is toegewezen aan deze taak.

* `<Cores of node_n_...>`is het aantal cores op het knooppunt dat is toegewezen aan deze taak.

Het aantal cores (**$NBCORES**) wordt ook berekend op basis van het aantal knooppunten (**$NBNODES**) en het aantal cores per knooppunt (die is opgegeven als parameter **$NBCORESPERNODE**).

De opties MPI voor zijn de bouwstenen die worden gebruikt met Intel MPI op Azure:

*   `-mpi intel`Intel MPI opgeven.

*   `-fabric UDAPL`Azure InfiniBand werkwoorden gebruiken.

*   `-cpubind bandwidth,v`aan de bandbreedte optimaliseren voor MPI met sterretje-CCM +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Intel MPI werken met Azure InfiniBand en voor het instellen van het vereiste aantal kernen per knooppunt.

*   `-batch`om te beginnen sterretje-CCM + in de batchmodus met geen gebruikersinterface.


Ten slotte, als u wilt een taak, zorg dat uw knooppunten goed werken en online in Cluster Manager zijn. Vanuit een PowerShell-opdrachtprompt, voert u dit:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Knooppunten stoppen
Later nadat u klaar bent met uw tests, kunt u de volgende HPC Pack PowerShell-opdrachten stoppen en knooppunten starten:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Volgende stappen
Kunt u andere werkbelasting Linux. Zie bijvoorbeeld:

* [NAMD uitvoeren met Microsoft HPC Pack op Linux berekeningscluster knooppunten in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [OpenFOAM met Microsoft HPC Pack uitvoeren op een cluster Linux RDMA in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png
