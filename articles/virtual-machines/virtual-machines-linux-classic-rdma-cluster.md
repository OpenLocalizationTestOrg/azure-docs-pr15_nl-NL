<properties
 pageTitle="Linux RDMA cluster MPI toepassingen uit te voeren | Microsoft Azure"
 description="Een Linux cluster van grootte H16r, H16mr, A8 of A9 VMs bij het netwerk Azure RDMA om uit te voeren MPI apps maken"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Instellen van een cluster Linux RDMA MPI toepassingen uit te voeren


Informatie over het instellen van een Linux RDMA cluster in Azure wordt aangegeven met [H-reeks of computerintensieve A-reeks VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md) parallelle bericht Passing Interface (MPI)-toepassingen uit te voeren. In dit artikel stapsgewijze instructies voor het voorbereiden van een afbeelding Linux HPC Intel MPI uitvoeren op een cluster. Vervolgens kunt u een cluster van VMs gebruiken in deze afbeelding en een van de grootte van het RDMA-ondersteuning Azure VM (momenteel H16r, H16mr, A8 of A9) implementeren. Via het cluster MPI-toepassingen die efficiënt via een lage latentie communiceren uitvoert, hoge doorvoer netwerk op basis van de geheugentoegang van externe directe technologie (RDMA).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Cluster distributieopties

Methoden voor het maken van een Linux RDMA-cluster met of zonder een projectplanner kunt u hier volgen.


* **Azure CLI scripts** - zoals verderop in dit artikel gebruikt de [Interface van Azure-opdrachtregel](../xplat-cli-install.md) (CLI) om een script de implementatie van een cluster van RDMA-ondersteuning VMs. De CLI in de modus voor servicebeheer Hiermee maakt u knooppunten serie in het implementatiemodel klassieke, zodat veel berekeningscluster knooppunten implementeert, kan enkele minuten duren. Als u wilt de netwerkverbinding RDMA inschakelen wanneer u het implementatiemodel klassieke gebruikt, implementeren de VMs in de dezelfde cloudservice.

* **Azure resourcemanager sjablonen** - u kunt ook het implementatiemodel resourcemanager gebruiken om te implementeren van een cluster van RDMA-ondersteuning VMs dat is verbonden met het netwerk RDMA. U kunt [uw eigen sjabloon maken](../resource-group-authoring-templates.md), of de [Azure quickstart sjablonen](https://azure.microsoft.com/documentation/templates/) voor sjablonen bijgedragen door Microsoft of de community naar het implementeren van de oplossing die u wilt controleren. Resourcemanager sjablonen kunnen zelf een snelle en betrouwbare formule een cluster Linux implementeren. Als u wilt de netwerkverbinding RDMA inschakelen wanneer u het implementatiemodel resourcemanager gebruikt, de VMs in bepaalde beschikbaarheid te implementeren.

* **HPC Pack** - maken van een Microsoft HPC Pack cluster in Azure en RDMA-ondersteuning berekeningscluster knooppunten waarop een ondersteunde Linux-verdeling voor toegang tot het netwerk RDMA toevoegen. Zie [aan de slag met Linux berekeningscluster knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-classic-model"></a>Voorbeeld implementatiestappen in de klassieke model

De volgende stappen weergeven hoe u een SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM van Azure Marketplace implementeren, aanpassen en een aangepaste VM-afbeelding maken met de CLI Azure. Gebruik vervolgens de afbeelding de implementatie van een cluster van RDMA-ondersteuning VMs scripts. 

>[AZURE.TIP]Gebruik dezelfde stappen om te implementeren van een cluster van RDMA-ondersteuning VMs op basis van andere ondersteunde HPC afbeeldingen in de Azure Marketplace. Enkele stappen mogelijk enigszins anders, zoals beschreven. Intel MPI is bijvoorbeeld opgenomen en geconfigureerd in slechts enkele van deze afbeeldingen. En als u een SLES 12 HPC VM in plaats van een SLES 12 SP1 HPC VM implementeert, moet u de RDMA-stuurprogramma's bijwerken. Zie voor meer informatie [over het A8, A9, A10, en A11 computerintensieve exemplaren](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Vereisten voor

*   **Clientcomputer** - moet u een Mac, Linux of Windows-clientapparaten computer om te communiceren met Azure. Deze stappen wordt ervan uitgegaan dat u gebruikt een Linux-client.

*   **Azure abonnement** - als u geen een abonnement hebt, kunt u een [account vrij te geven](https://azure.microsoft.com/free/) in een paar minuten. Voor grotere kolomgroepen kunt u bovendien een pay-as-you-go abonnement of andere opties voor het aanschaffen. 

*   **VM grootte beschikbaarheid** - momenteel de grootte van de volgende instantie RDMA kunnen zijn: H16r, H16mr A8 en A9. Controleren welke [producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) beschikbaar in Azure regio's. 

*   **Quotum voor cores** - moet u mogelijk het quotum van cores om te implementeren van een cluster van computerintensieve VMs vergroten. Bijvoorbeeld, moet u ten minste 128 cores als u implementeren van 8 A9 VMs wilt, zoals in dit artikel. Uw abonnement mogelijk ook het aantal cores die u in bepaalde VM grootte gezinnen implementeren kunt, inclusief de H-reeks beperken. Als u wilt ontvangen een quotum vergroten, [open een verzoek voor een online-klant ondersteuning](../azure-supportability/how-to-create-azure-support-request.md) gratis. 

*   **Azure CLI** - [installeren](../xplat-cli-install.md) de CLI Azure en [verbinding maken met uw Azure-abonnement](../xplat-cli-connect.md) op de clientcomputer.


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Stap 1. Een SLES 12 SP1 HPC VM inrichten

Aanmelden bij Azure met de CLI Azure, en voer `azure config list` om te bevestigen dat de uitvoer servicebeheer-modus weergeven. Als dit niet het geval is, stelt u de modus door deze opdracht uit te voeren:


    azure config mode asm


Typ het volgende als u wilt vermelden van alle abonnementen die u zijn gemachtigd om te gebruiken:


    azure account list

Het huidige actieve abonnement wordt aangeduid met `Current` ingesteld op `true`. Als dit abonnement niet degene die u wilt gebruiken om te maken van het cluster, stelt u de juiste abonnements-Id als het actieve abonnement:

    azure account set <subscription-Id>

Als u wilt zien in de openbaar SLES 12 SP1 HPC afbeeldingen in Azure, voert u een opdracht uit de volgende strekking, ervan uitgaande dat er uw shell-omgeving ondersteunt **grep**:


    azure vm image list | grep "suse.*hpc"

Nu inrichten van een RDMA-ondersteuning VM met een afbeelding SLES 12 SP1 HPC door een vergelijkbaar met de volgende opdracht uit te voeren:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

waar

* De grootte (A9 in dit voorbeeld) is een van de formaten RDMA-ondersteuning VM.

* De externe SSH poortnummer (22 in dit voorbeeld is de standaardwaarde SSH) is een geldig poortnummer. Het interne SSH poortnummer is ingesteld op 22.

* Een nieuwe cloudservice is gemaakt in de Azure regio is opgegeven door de locatie. Geef een locatie op waarin de VM-grootte die u kiest beschikbaar is.

* De naam van de afbeelding SLES 12 SP1 momenteel kan worden `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` of `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` voor SUSE prioriteitsondersteuning (extra gesprekskosten zijn van toepassing).

### <a name="step-2-customize-the-vm"></a>Stap 2. De VM aanpassen

Nadat de VM is voltooid is geïnstalleerd, SSH voor VM van de VM extern IP-adres (of DNS-naam) en de externe poort nummeren u geconfigureerd en aanpassen. Lees [hoe u aanmelden bij een virtuele Machine uitgevoerd Linux](virtual-machines-linux-mac-create-ssh-keys.md)voor meer informatie verbinding. Alleen toegang tot de hoofdmap is vereist voor een stap hebt voltooid, kunt u opdrachten uitvoeren als de gebruiker die u voor de VM, geconfigureerd.

>[AZURE.IMPORTANT]Microsoft Azure biedt geen toegang tot de hoofdmap naar Linux VMs. Administratieve om toegang te krijgen wanneer u verbinding hebt als een gebruiker voor VM, uitvoeren met opdrachten `sudo`.

* **Updates** - updates installeren met **zypper**. U kunt ook NFS hulpprogramma's installeren. 

    >[AZURE.IMPORTANT]In een SLES 12 SP1 HPC VM, is het raadzaam dat u niet van toepassing zijn kernel update, die leiden problemen met de RDMA Linux stuurprogramma's tot kunnen.

* **Intel MPI** - voltooit u de installatie van Intel MPI op de SLES 12 SP1 HPC VM door de volgende opdracht uit te voeren:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Geheugen vergrendelingen** - codes voor MPI het beschikbare geheugen vergrendelen voor RDMA, toevoegen of wijzigen van de volgende instellingen in het bestand /etc/security/limits.conf. (U moet toegang tot dit bestand bewerken de hoofdmap.) 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]U kunt ook memlock onbeperkt voor testdoeleinden instellen. Bijvoorbeeld: `<User or group name>    hard    memlock unlimited`. Zie [Aanbevolen bekende methoden voor het instelling vergrendeld geheugengrootte](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size)voor meer informatie.

* **SSH toetsen voor SLES VMs** - genereren SSH toetsen tot stand brengen van vertrouwen voor uw gebruikersaccount tussen de knooppunten berekenen in het cluster SLES als actief MPI taken. (Als u een HPC CentOS gebaseerde VM geïmplementeerd, niet volgt u deze stap. Zie de instructies later in dit artikel voor het instellen van passwordless SSH vertrouwensrelatie tussen knooppunten nadat u de afbeelding vastleggen en implementeren van het cluster.) 

    Voer de volgende opdracht om SSH sleutels te maken. Wanneer u wordt gevraagd om invoer, druk op Enter om het genereren van de gebruikte toetsen op de standaardlocatie zonder in te stellen van een wachtwoordzin.

        ssh-keygen

    De openbare sleutel toevoegen aan het bestand authorized_keys voor bekende openbare sleutels.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    In de map ~/.ssh, bewerken of het configuratiebestand '' maken. Geef het bereik van de IP-adres van het particuliere netwerk dat u wilt gebruiken in Azure wordt aangegeven (10.32.0.0/16 in dit voorbeeld):

        host 10.32.0.*
        StrictHostKeyChecking no

    U kunt ook een lijst met het netwerk privé IP-adres van elke VM in uw cluster als volgt:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Configureren `StrictHostKeyChecking no` een veiligheidsrisico kunt maken wanneer een specifiek IP-adres of een bereik is niet opgegeven.

* **Toepassingen** - installeert u alle toepassingen die u nodig hebt over deze VM of andere aanpassingen uitvoeren voordat u de afbeelding opnemen.

### <a name="step-3-capture-the-image"></a>Stap 3. Afbeelding van het maken

Als u wilt vastleggen van de afbeelding, moet u eerst de volgende opdracht uitvoeren in de VM Linux. Deze opdracht de VM deprovisions maar behoudt gebruikersaccounts en SSH toetsen die u hebt ingesteld.

```
sudo waagent -deprovision
```

Van uw computer, voert u de volgende opdrachten Azure CLI om de afbeelding te halen. Lees [hoe u het vastleggen van een klassieke Linux virtuele machine als een afbeelding](virtual-machines-linux-classic-capture-image.md) voor meer informatie.  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Nadat u deze opdrachten uitvoeren, de afbeelding VM wordt vastgelegd voor het gebruik en de VM wordt verwijderd. Nu hebt u uw aangepaste afbeelding een cluster implementeren.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Stap 4. Een cluster met de afbeelding implementeren

Wijzig het volgende script we vaker doen met de juiste waarden voor uw omgeving en uitvoeren vanaf uw computer. Omdat Azure de VMs serie in het implementatiemodel klassieke implementeert, duurt het een paar minuten de 8 A9 VMs voorgestelde in dit script implementeren.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Overwegingen voor een CentOS HPC-cluster

Als u een cluster op basis van een van de afbeeldingen CentOS gebaseerde HPC in de Azure Marketplace in plaats van SLES 12 voor HPC instelt wilt, volgt u de algemene stappen in de vorige sectie. Houd rekening met de volgende verschillen wanneer u inrichten en configureren van de VM:

1. Intel MPI is al geïnstalleerd op een VM deze is ingericht uit een afbeelding CentOS gebaseerde HPC. 

2. Vergrendelen geheugeninstellingen zijn al toegevoegd in van de VM /etc/security/limits.conf bestand.

2. Genereren geen SSH toetsen op de VM u inrichten voor het vastleggen. Wij raden bij het instellen van verificatie op basis van een gebruiker nadat u de cluser geïmplementeerd. Zie de volgende sectie.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Passwordless SSH vertrouwen in het cluster instellen

Klik op een CentOS gebaseerde HPC-cluster, er zijn twee manieren voor het instellen van een vertrouwensrelatie tussen de knooppunten berekeningscluster: host gebaseerde verificatie en verificatie op basis van een gebruiker. Host gebaseerde verificatie is buiten het bereik van dit artikel en over het algemeen moet worden uitgevoerd via een script extensie tijdens de implementatie. Verificatie op basis van een gebruiker is handig voor het instellen van een vertrouwensrelatie na implementatie en de generatie en delen van sleutels SSH tussen de knooppunten berekeningscluster in het cluster vereist. Deze methode is zogenaamde passwordless SSH login en is vereist als u MPI taken. 

Een voorbeeldscript bijgedragen van de community is beschikbaar op [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) eenvoudig gebruikersverificatie op een cluster CentOS gebaseerde HPC inschakelen. Downloaden en gebruiken van dit script met de volgende stappen. U kunt ook dit script wijzigen of een andere methode selecteert tot stand brengen van passwordless SSH verificatie tussen knooppunten berekeningscluster gebruiken.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
U kunt het script uitvoeren die u moet weten het voorvoegsel voor uw subnet IP-adressen. Het voorvoegsel krijgen door de volgende opdracht uit te voeren op een van de knooppunten. De uitvoer ziet er als 10.1.3.5 en het voorvoegsel is de/m 10.1.3 gedeelte.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Het script met drie parameters nu uitvoeren: de algemene gebruikersnaam in te voeren op de knooppunten berekeningscluster, het algemene wachtwoord voor die gebruiker op de knooppunten berekenen en het voorvoegsel subnetten die is geretourneerd uit de vorige opdracht.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Dit script gebeurt het volgende:

* Hiermee maakt een map aan de hostknooppunt met .ssh, de naam die is vereist voor passwordless login. 
* Hiermee maakt u een configuratiebestand in de adreslijst .ssh waarmee passwordless Meld u aan bij aanmelding vanaf elk knooppunt toestaan in het cluster. 
* Hiermee maakt u bestanden met de knooppuntnamen en IP-adressen voor alle knooppunten in het cluster. Deze bestanden worden links nadat het script is uitgevoerd voor later gebruik. 
* Hiermee maakt u een persoonlijke en openbare sleutel paar voor elk clusterknooppunt waaronder het hostknooppunt en vermeldingen in het bestand authorized_keys maakt.

>[AZURE.WARNING]In dit script uitvoeren, kan een veiligheidsrisico maken. Zorg ervoor dat de openbare belangrijke informatie in ~/.ssh niet wordt uitgekeerd.


## <a name="configure-intel-mpi"></a>Intel MPI configureren

MPI toepassingen op Azure Linux RDMA uitgevoerd, moet u bepaalde omgevingsvariabelen die specifiek zijn voor Intel MPI configureren. Hier volgt een voorbeeldscript we vaker doen voor het configureren van de variabelen die nodig zijn voor het uitvoeren van een toepassing. Wijzig het pad naar mpivars.sh voor uw installatie van Intel MPI.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

De indeling van het hostbestand is als volgt. Eén regel voor elk knooppunt in uw cluster toevoegen. Geef privé IP-adressen van de VNet eerder hebt gedefinieerd, niet de namen van de DNS. Bijvoorbeeld, bevat het bestand voor twee hosts met IP-adressen 10.32.0.1 en 10.32.0.2 het volgende:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>MPI worden uitgevoerd op een eenvoudige cluster met twee knooppunten

Als u dit nog niet hebt gedaan, eerst de omgeving configureren voor Intel MPI. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Een eenvoudige MPI-opdracht uitvoeren

Een eenvoudige MPI-opdracht uitvoeren op een van de knooppunten berekeningscluster om weer te geven dat MPI juist is geïnstalleerd en communiceren kan tussen ten minste dat twee knooppunten berekenen. De volgende opdracht uit **mpirun** de opdracht **hostname** op twee knooppunten wordt uitgevoerd.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
De uitvoer moet de namen van de knooppunten die u hebt doorgegeven als invoer voor een lijst met `-hosts`. Een opdracht **mpirun** met twee knooppunten wordt bijvoorbeeld uitvoer ongeveer als volgt uit:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Een referentie MPI uitvoeren

De volgende opdracht uit Intel MPI wordt een referentie pingpong om te controleren of de configuratie van het cluster en de verbinding met het netwerk RDMA uitgevoerd.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Op een cluster werken met twee knooppunten ziet u de volgende strekking uitvoer. Klik op het netwerk Azure RDMA, verwachten latentie op of onder 3 microseconden voor bericht die groter zijn maximaal 512 bytes.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Volgende stappen

* Probeer te implementeren en het uitvoeren van de uw MPI Linux toepassingen op uw cluster Linux.

* Zie de [documentatie van Intel MPI bibliotheek](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) voor instructies over Intel MPI.

* Probeer een [quickstart sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) te maken van een Intel Lustre cluster met behulp van de afbeelding van een CentOS gebaseerde HPC. Zie voor meer informatie in dit [blogbericht](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
