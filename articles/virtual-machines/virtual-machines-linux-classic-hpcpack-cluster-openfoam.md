<properties
 pageTitle="OpenFOAM worden uitgevoerd met HPC Pack op Linux VMs | Microsoft Azure"
 description="Een Microsoft HPC Pack cluster op Azure implementeren en uitvoeren van een taak OpenFOAM op meerdere Linux berekeningscluster knooppunten via een netwerk RDMA."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>OpenFoam met Microsoft HPC Pack uitvoeren op een cluster Linux RDMA in Azure wordt aangegeven

In dit artikel leest u een manier om te OpenFoam worden uitgevoerd in Azure virtuele machines. Hier kunt u een Microsoft HPC Pack cluster met Linux berekeningscluster knooppunten op Azure en het uitvoeren van taken een [OpenFoam](http://openfoam.com/) met Intel MPI implementeren. Zodat de knooppunten berekeningscluster via het netwerk Azure RDMA communiceren, kunt u RDMA-ondersteuning Azure VMs voor de knooppunten berekeningscluster. Andere opties voor het uitvoeren van OpenFoam in Azure opnemen volledig geconfigureerde commerciële afbeeldingen beschikbaar zijn in de Marketplace, zoals de UberCloud [OpenFoam 2.3 op CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)en door te voeren op [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (voor veld bewerking openen en bewerken) is een pakket met open source rekenkundige flexibel dynamics (CFD)-software die sterk uiteen wordt gebruikt in de technische en wetenschappelijke, in commerciële en academische organisaties. Het bevat hulpprogramma's voor meshing, met name snappyHexMesh, een parallelized mesher voor complexe CAD-geometrieën en voor oude als na verwerking. Bijna alle processen worden uitgevoerd parallel, zodat gebruikers kunnen profiteren van de computerhardware beschikken.  

Microsoft HPC Pack biedt functies om uit te voeren grootschalige HPC en parallelle toepassingen, waaronder MPI-toepassingen, op clusters van Microsoft Azure virtuele machines. HPC Pack ondersteunt ook actieve Linux HPC toepassingen op Linux knooppunt VMs geïmplementeerd in een cluster HPC Pack berekenen. Zie [aan de slag met Linux berekeningscluster knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md) voor een introductie over het gebruik van Linux berekeningscluster knooppunten met HPC Pack.

>[AZURE.NOTE] In dit artikel ziet u hoe een werkbelasting Linux MPI uitvoeren met HPC Pack. Ervan uitgaande dat er enkele bekend zijn met Linux Systeembeheer en MPI werkbelasting op Linux clusters uitgevoerd. Als u versies van MPI en OpenFOAM afwijken van de instellingen in dit artikel vermelde gebruikt, moet u mogelijk enkele stappen installatie en configuratie wijzigen. 

## <a name="prerequisites"></a>Vereisten voor

*   **HPC Pack cluster met RDMA-ondersteuning Linux berekenen knooppunten** - een HPC Pack cluster met grootte A8, A9, H16r of H16rm Linux berekeningscluster knooppunten met behulp van een [Azure resourcemanager sjabloon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) of een [Azure PowerShell-script](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)implementeren. Zie [aan de slag met Linux berekeningscluster knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md) voor de vereisten en stappen voor beide opties. Als u de PowerShell-script implementatie optie kiest, raadpleegt u het voorbeeldbestand configuratie in de voorbeeldbestanden aan het einde van dit artikel. Deze configuratie gebruiken om te implementeren van een bestaande uit een grootte A8 Windows Server 2012 R2 hoofd knooppunt en 2 grootte A8 SUSE Linux Enterprise Server 12 berekeningscluster knooppunten op basis van Azure HPC Pack-cluster. Vervangt door de juiste waarden voor de namen van uw abonnement en -service. 

    **Extra wat u moet weten**

    *   Zie voor Linux RDMA netwerken vereisten in Azure wordt aangegeven, [over H-reeks en computerintensieve A-reeks VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md).

    *   Als u de optie Powershell-script implementatie gebruikt, implementeren alle Linux berekeningscluster knooppunten in een cloudservice gebruik van de netwerkverbinding RDMA.

    *   Na de knooppunten Linux wordt geïmplementeerd, verbinding te maken door SSH eventuele aanvullende beheertaken uitvoeren. De verbindingsgegevens SSH voor elke Linux VM vinden in de portal van Azure.  
        
*   **Intel MPI** - OpenFOAM uitvoeren op SLES 12 HPC berekenen knooppunten in Azure wordt aangegeven, moet u de runtime Intel MPI bibliotheek 5 van de [site Intel.com](https://software.intel.com/en-us/intel-mpi-library/)installeren. (Intel MPI 5 is vooraf geïnstalleerd op CentOS gebaseerde HPC afbeeldingen.)  Installeer Intel MPI op uw Linux berekeningscluster knooppunten in een latere stap, indien nodig. Als u wilt voorbereiden voor deze stap nadat u met Intel hebt geregistreerd, volgt u de koppeling in het bevestigingsbericht naar de bijbehorende webpagina. Kopieer vervolgens de koppeling naar het bestand .tgz voor de juiste versie van Intel MPI. In dit artikel is gebaseerd op Intel MPI versie 5.0.3.048.

*   **OpenFOAM bron Pack** - Download van de software OpenFOAM bron Pack voor Linux van de [OpenFOAM Foundation-site](http://openfoam.org/download/2-3-1-source/). In dit artikel is gebaseerd op bron Pack versie 2.3.1, downloaden als OpenFOAM-2.3.1.tgz. Volg de instructies verderop in dit artikel uitpakken en OpenFOAM op de Linux berekeningscluster knooppunten compileren.

*   **EnSight** (optioneel) - raadpleegt u de resultaten van uw simulatie OpenFOAM, downloaden en installeren van het programma dat [EnSight](https://www.ceisoftware.com/download/) visualisatie en analyse. Informatie over licenties en download zijn op de site met EnSight.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Vertrouwensrelatie tussen berekeningscluster knooppunten instellen

Het uitvoeren van een taak diverse knooppunten op meerdere Linux knooppunten, moet de knooppunten voor elkaar te vertrouwen (door **rsh** of **ssh**). Wanneer u het cluster HPC Pack met de Microsoft HPC Pack IaaS implementatiescript maakt, het script blijvend vertrouwensrelatie voor het beheerdersaccount die u opgeeft automatisch ingesteld. Voor gebruikers die geen beheerder u in van het cluster domein maakt, kunt u hebt tijdelijke vertrouwensrelatie tussen de knooppunten instellen wanneer een taak is toegewezen aan deze, en de relatie destroy wanneer de taak voltooid is. Als u wilt instellen voor elke gebruiker worden vertrouwd, vindt u een combinatie van RSA sleutel aan de cluster die HPC Pack voor de vertrouwensrelatie wordt gebruikt.

### <a name="generate-an-rsa-key-pair"></a>Een combinatie van RSA-sleutel genereren

Het is eenvoudig om te genereren van een paar voor belangrijke op RSA, waarin een openbare sleutel en een persoonlijke sleutel, door de Linux **ssh-keygen** -opdracht uit te voeren.

1.  Meld u aan bij een Linux-computer.

2.  Voer de volgende opdracht:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Druk op **Enter** om de standaardinstellingen gebruiken totdat de opdracht is voltooid. Voer geen een wachtwoordzin hier; Wanneer u wordt gevraagd om een wachtwoord, druk op **Enter**.

    ![Een combinatie van RSA-sleutel genereren][keygen]

3.  De map wijzigen naar de map ~/.ssh. De persoonlijke sleutel is opgeslagen in id_rsa en de openbare sleutel in id_rsa.pub.

    ![Persoonlijke en openbare sleutels][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>De combinatie van de sleutel toevoegen aan het cluster HPC Pack
1.  Een extern bureaublad verbinding maken met uw hoofd knooppunt met uw beheerdersaccount HPC Pack (het beheerdersaccount u instellen wanneer u de implementatiescript hebt uitgevoerd).

2. Gebruik Windows Server standaardprocedures maken van een domein-gebruikersaccount in van het cluster Active Directory-domein. Bijvoorbeeld, gebruikt u het hulpprogramma Active Directory-gebruiker en Computers op het hoofd knooppunt. De voorbeelden in dit artikel wordt ervan uitgegaan dat u een domeingebruiker met de naam hpclab\hpcuser maken.

3.  Een bestand met de naam C:\cred.xml maken en kopieer de RSA belangrijke gegevens erin. Een voorbeeld van een cred.xml-bestand is aan het einde van dit artikel.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Open een opdrachtprompt en voer de volgende opdracht uit om in te stellen van de gegevens van de referenties voor de hpclab\hpcuser-account. De parameter **extendeddata** kunt u de naam van C:\cred.xml bestand dat u hebt gemaakt voor de belangrijkste gegevens doorgeeft.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Deze opdracht is voltooid zonder uitvoer. Sla het bestand cred.xml in een veilige locatie na het instellen van de referenties voor de gebruikersaccounts voor het uitvoeren van taken, of verwijder deze.

5.  Als u de combinatie van RSA-sleutel op een van de knooppunten Linux gegenereerd, moet u de toetsen verwijderen nadat u klaar bent met het ze te gebruiken. Als HPC Pack vindt u een bestaand id_rsa-bestand of id_rsa.pub-bestand, wordt deze niet vertrouwensrelatie ingesteld.

>[AZURE.IMPORTANT] Wordt niet aanbevolen een Linux-taak als de beheerder van een cluster wordt uitgevoerd op een gedeelde cluster, omdat een taak die is verzonden door een beheerder uitgevoerd onder de hoofdmap-account op de knooppunten Linux. Echter compatibel een taak die is verzonden door een gebruiker die geen beheerder is met een lokale Linux-gebruikersaccount met dezelfde naam als de gebruiker van de taak. In dit geval HPC Pack ingesteld vertrouwensrelatie voor deze gebruiker Linux op de knooppunten toegewezen aan de taak. U kunt de gebruiker Linux handmatig op de knooppunten Linux instellen voordat u de taak of HPC Pack de gebruiker automatisch gemaakt wanneer de taak wordt verzonden. Als HPC Pack wordt gemaakt van de gebruiker, HPC Pack wordt deze verwijderd nadat de taak is voltooid. HPC Pack Hiermee als wilt verkleinen beveiligingsrisico's, verwijdert u de toetsen na Taakvoltooiing.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Een bestandsshare voor Linux knooppunten instellen

Nu een standaard SMB delen op een map op het hoofd knooppunt ingesteld. Als u wilt dat de knooppunten Linux voor toegang tot toepassingsbestanden met een algemene pad, de gedeelde map op de knooppunten Linux te koppelen. Als u wilt, kunt u een andere optie, zoals een Azure-bestanden-aandeel - aanbevolen voor veel scenario's- of een NFS-share met gedeelde bestanden. Zie het bestand delen van informatie en gedetailleerde stappen in [aan de slag met Linux berekeningscluster knooppunten in een HPC Pack Cluster in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Een map op het hoofd knooppunt maken en deze voor iedereen te delen door in te stellen bevoegdheden voor alleen-lezen/schrijven. Bijvoorbeeld C:\OpenFOAM delen op het hoofd knooppunt als \\ \\SUSE12RDMA-HN\OpenFOAM. Hier is *SUSE12RDMA-HN* de hostnaam van het hoofd knooppunt.

2.  Open een Windows PowerShell-venster en voer de volgende opdrachten:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

De eerste opdracht maakt een map genaamd /openfoam op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de gedeelde map //SUSE12RDMA-HN/OpenFOAM op de knooppunten Linux met dir_mode en file_mode bits ingesteld op 777. De *gebruikersnaam* en *wachtwoord* in de opdracht moet de referenties van een gebruiker op het hoofd knooppunt.

>[AZURE.NOTE]De "\`" symbool in het tweede is een escapeteken voor PowerShell. "\`," betekent dat de "," (kommateken) is een onderdeel van de opdracht.

## <a name="install-mpi-and-openfoam"></a>Installeer MPI en OpenFOAM

Als u wilt uitvoeren OpenFOAM als een taak MPI op het netwerk RDMA, moet u OpenFOAM compileren terwijl de Intel MPI-bibliotheken. 

Voer eerst verschillende **clusrun** opdrachten voor het installeren van Intel MPI-bibliotheken (als dit nog niet is geïnstalleerd) en OpenFOAM op uw knooppunten Linux. Gebruik het hoofd knooppunt-aandeel eerder geconfigureerd voor het delen van de installatiebestanden tussen de knooppunten Linux.

>[AZURE.IMPORTANT]Deze installatie en compileren stappen zijn voorbeelden. Moet u eerst enkele kennis van Linux Systeembeheer om ervoor te zorgen dat afhankelijke compileerprogramma's en -bibliotheken correct zijn geïnstalleerd. Mogelijk moet u bepaalde omgevingsvariabelen of andere instellingen voor uw versie van Intel MPI en OpenFOAM wijzigen. Zie [Intel MPI bibliotheek voor de installatiehandleiding Linux](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) en [OpenFOAM bron Pack installatie](http://openfoam.org/download/2-3-1-source/) voor uw omgeving voor meer informatie.


### <a name="install-intel-mpi"></a>Intel MPI installeren

Sla het gedownloade installatiepakket om Intel MPI (l_mpi_p_5.0.3.048.tgz in dit voorbeeld) in C:\OpenFoam op het hoofd knooppunt zodat de knooppunten Linux toegang hebt tot dit bestand uit /openfoam. Voer **clusrun** om te kunnen installeren Intel MPI bibliotheek op de Linux knooppunten.

1.  De volgende opdrachten kopiëren van het installatiepakket te gaan en pak het /opt/intel op elk knooppunt.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Intel MPI bibliotheek stilte installeren, via een silent.cfg-bestand. U kunt een voorbeeld vinden in de voorbeeldbestanden aan het einde van dit artikel. Dit bestand in de gedeelde map /openfoam plaatsen. Zie voor meer informatie over het bestand silent.cfg, [Intel MPI bibliotheek voor de installatiehandleiding voor Linux - installatie op de achtergrond](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Zorg ervoor dat u uw silent.cfg bestand opslaan als een tekstbestand met Linux regeleinden heb (alleen LF, niet CR LF). Dit zorgt ervoor dat deze wordt uitgevoerd correct op de knooppunten Linux.

3.  Installeer Intel MPI bibliotheek in de stille modus.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>MPI configureren

Voor het testen, moet u de volgende regels toevoegen aan de /etc/security/limits.conf op elk van de knooppunten Linux:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Start opnieuw op de knooppunten Linux nadat u het bestand limits.conf bijwerken. Bijvoorbeeld, gebruikt u de volgende opdracht uit **clusrun** :

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Start opnieuw op en zorg ervoor dat de gedeelde map is gekoppeld als /openfoam.

### <a name="compile-and-install-openfoam"></a>Compileren en OpenFOAM installeren

Sla het gedownloade installatiepakket voor de OpenFOAM bron Pack (OpenFOAM-2.3.1.tgz in dit voorbeeld) naar C:\OpenFoam op het hoofd knooppunt zodat de knooppunten Linux toegang hebt tot dit bestand uit /openfoam. Vervolgens **clusrun** opdrachten OpenFOAM compileren op alle Linux knooppunten uitvoeren.


1.  Een map /opt/OpenFOAM op elk knooppunt Linux maken, het bronpakket naar deze map kopiëren en deze er extraheren.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  OpenFOAM met de Intel MPI bibliotheek, stelt u eerst enkele omgevingsvariabelen voor zowel Intel MPI en OpenFOAM compileren. Gebruik een we vaker doen script settings.sh genoemd in te stellen van de variabelen. U kunt een voorbeeld vinden in de voorbeeldbestanden aan het einde van dit artikel. Dit bestand (opgeslagen met Linux regeleinden) in de gedeelde map /openfoam plaatsen. Dit bestand bevat ook instellingen voor de MPI en OpenFOAM runtimes dat u later gebruiken een OpenFOAM taak uit te voeren.

3. Afhankelijke nodig OpenFOAM compileren-pakketten installeren. Afhankelijk van uw Linux-verdeling gebruikt moet u mogelijk eerst een bibliotheek toevoegen. Voer **clusrun** opdrachten ongeveer als volgt uit:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    Klik zo nodig, SSH voor elk knooppunt Linux om uit te voeren van de opdrachten om te bevestigen dat ze correct worden uitgevoerd.

4.  Voer de volgende opdracht OpenFOAM compileren. Het proces gecompileerd duurt enige tijd om te voltooien en genereert een grote hoeveelheid logboekgegevens naar standaard uitvoer, zodat de optie **/ afwisselende** gebruiken om de uitvoer afwisselende weer te geven.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]De "\`" symbool in de opdracht is een escapeteken voor PowerShell. "\`&" betekent dat het "&" is een onderdeel van de opdracht.

## <a name="prepare-to-run-an-openfoam-job"></a>Een taak OpenFOAM uit te voeren voorbereiden

Nu voorbereiden op het uitvoeren van een MPI taak sloshingTank3D, dat wil een van de voorbeelden OpenFoam zeggen, op twee Linux knooppunten genoemd. 

### <a name="set-up-the-runtime-environment"></a>Stel de runtimeomgeving

Als u wilt de runtimeomgevingen voor MPI en OpenFOAM op de knooppunten Linux, voer de volgende opdracht in een Windows PowerShell-venster op het hoofd knooppunt. (Deze opdracht is alleen geldig voor het SUSE Linux.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Voorbeeldgegevens voorbereiden

Gebruik het hoofd knooppunt delen dat u eerder hebt geconfigureerd voor het delen van bestanden tussen de Linux-knooppunten (gekoppeld als /openfoam).

1.  SSH naar een van uw Linux berekenen knooppunten.

2.  Voer de volgende opdracht voor het instellen van de OpenFOAM runtime-omgeving als u dit nog niet hebt gedaan.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  De steekproef sloshingTank3D kopiëren naar de gedeelde map en Ga naar deze.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Wanneer u de standaardparameters in dit voorbeeld gebruikt, kan dit tientallen minuten om uit te voeren, duren, zodat u wijzigen van enkele parameters wilt mogelijk zodat deze sneller worden uitgevoerd. Een eenvoudige keuze is voor het wijzigen van de tijd stap variabelen deltaT en writeInterval in het systeem/controlDict-bestand. Dit bestand wordt alle ingevoerde gegevens betreffende de controle van tijd en bij het lezen en schrijven oplossingsgegevens. U kunt bijvoorbeeld de waarde van deltaT van 0,05 op 0,5 en de waarde van writeInterval van 0,05 op 0,5 wijzigen.

    ![Wijzig de variabelen in stap][step_variables]

5.  Geef de gewenste waarden voor de variabelen in het systeem/decomposeParDict-bestand. In dit voorbeeld worden twee Linux knooppunten elke gebruikt met 8 cores, dus ingesteld numberOfSubdomains op 16 en n van hierarchicalCoeffs naar (1 1 16), wat inhoudt dat parallel OpenFOAM met 16 processen. Zie voor meer informatie [OpenFOAM User Guide: 3,4 voorlopig toepassingen parallel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Ontleden processen][decompose]

6.  Voer de volgende opdrachten uit de adreslijst sloshingTank3D voor het voorbereiden van de voorbeeldgegevens.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  Klik op het knooppunt hoofd ziet u dat de gegevens voorbeeldbestanden worden gekopieerd naar C:\OpenFoam\sloshingTank3D. (C:\OpenFoam is de gedeelde map op het hoofd knooppunt.)

    ![Gegevensbestanden op het hoofd knooppunt][data_files]

### <a name="host-file-for-mpirun"></a>Hostbestand voor mpirun

In deze stap maakt u een hostbestand (een lijst met berekeningscluster knooppunten) waarin de opdracht **mpirun** wordt gebruikt.

1.  Klik op een van de knooppunten Linux, door een bestand met de naam hostfile onder /openfoam, zodat dit bestand bereikbaar /openfoam/hostfile op alle Linux knooppunten te maken.

2.  Schrijf uw Linux knooppuntnamen in dit bestand. In dit voorbeeld bevat het bestand de volgende namen:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]U kunt ook in dit bestand bij C:\OpenFoam\hostfile maken op het hoofd knooppunt. Als u deze optie kiest, opslaan als een tekstbestand met Linux regeleinden (alleen LF, niet CR LF). Dit zorgt ervoor dat deze wordt uitgevoerd correct op de knooppunten Linux.

    **We vaker doen script Wikkel**

    Als u veel Linux knooppunten hebt en u wilt dat uw taak uitvoeren op slechts enkele, is deze niet een goed idee om een hostbestand vaste gebruiken omdat u niet weet welke knooppunten wordt toegewezen aan uw project. In dit geval Schrijf een we vaker doen script Wikkel voor **mpirun** bestand met de host automatisch maken. U kunt een voorbeeld we vaker doen script Wikkel genoemd hpcimpirun.sh aan het einde van dit artikel vinden en opslaan als /openfoam/hpcimpirun.sh. In dit voorbeeldscript gebeurt het volgende:

    1.  Bepaalt de omgevingsvariabelen voor **mpirun**en sommige parameters van de opdracht toevoegen aan de taak MPI uitvoeren via het netwerk RDMA. In dit geval wordt de volgende variabelen ingesteld:

        *   I_MPI_FABRICS = shm:dapl
        *   I_MPI_DAPL_PROVIDER = weergeeft van een-v2-ib0
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  Hiermee maakt u een hostbestand op basis van de omgeving variabele $CCP_NODES_CORES, de knop is ingesteld door het hoofd knooppunt HPC wanneer de taak wordt geactiveerd.

        De indeling van $CCP_NODES_CORES volgt deze notatie:

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        waar

        * `<Number of nodes>`-het aantal knooppunten die zijn toegewezen aan deze taak.  
        
        * `<Name of node_n_...>`-de naam van elke knooppunt toegewezen aan deze taak.
        
        * `<Cores of node_n_...>`-het aantal cores op het knooppunt dat is toegewezen aan deze taak.

        Bijvoorbeeld als de taak moet worden twee knooppunten om uit te voeren, is $CCP_NODES_CORES vergelijkbaar met
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  De opdracht **mpirun** wordt aangeroepen en twee parameters toegevoegd aan de opdrachtregel.

        * `--hostfile <hostfilepath>: <hostfilepath>`-het pad van het Hiermee maakt u het script hostbestand

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-een omgevingsvariabele ingesteld door het HPC Pack hoofd knooppunt het nummer van de totale cores toegewezen aan deze taak bevat. In dit geval deze Hiermee geeft u het aantal processen voor **mpirun**.


## <a name="submit-an-openfoam-job"></a>Een taak OpenFOAM verzenden

Nu kunt u een taak in HPC Cluster Manager stuurt. U moet de hpcimpirun.sh script in de regels van de opdracht voor een deel van de projecttaken doorgeven.

1. Verbinding maken met uw cluster hoofd knooppunt en HPC Cluster Manager te starten.

2. **In resourcebeheer**ervoor te zorgen dat de Linux berekeningscluster knooppunten in de stand **Online** . Als dat niet het geval is, selecteert u deze en klik op **Online brengen**.

3.  Klik op **Nieuwe taak**in **Taakbeheer**.

4.  Voer een naam voor de taak zoals _sloshingTank3D_.

    ![Details van een taak][job_details]

5.  In **taak resources**, kies het type resource als "Knooppunt" en stel de Minimum op 2. Deze configuratie wordt de taak op twee Linux knooppunten, die elk acht cores in dit voorbeeld is uitgevoerd.

    ![Taak resources][job_resources]

6. **Taken bewerken** in het linker navigatiegedeelte op en klik vervolgens op **toevoegen** aan een taak toevoegen aan de taak. Vier taken toevoegen aan de taak met de volgende opdracht lijnen en instellingen.

    >[AZURE.NOTE]Actief `source /openfoam/settings.sh` stelt de OpenFOAM en MPI runtimeomgevingen, zodat u elk van de volgende taken uitvoeren voordat u de opdracht OpenFOAM aangeroepen.

    *   **Taak 1**. **DecomposePar** om te genereren gegevensbestanden voor het uitvoeren van **interDyMFoam** parallel uitvoeren.
    
        *   Één knooppunt aan de taak toewijzen

        *   **Opdrachtregel** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Werkmap** - / openfoam/sloshingTank3D
        
        Zie de volgende afbeelding. U configureren de resterende taken op dezelfde manier.

        ![Taak 1-details][task_details1]

    *   **Taak 2**. **InterDyMFoam** parallel te berekenen van de steekproef worden uitgevoerd.

        *   Twee knooppunten aan de taak toewijzen

        *   **Opdrachtregel** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Werkmap** - / openfoam/sloshingTank3D

    *   **Taak 3**. **ReconstructPar** de sets met tijd mappen uit de adreslijst van elke processor_N_ samenvoegen in één set uitvoeren.

        *   Één knooppunt aan de taak toewijzen

        *   **Opdrachtregel** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Werkmap** - / openfoam/sloshingTank3D

    *   **Taak 4**. **FoamToEnsight** parallel de OpenFOAM resultaat bestanden converteren naar EnSight en plaatst u de EnSight-bestanden in de map Ensight in de hoofdletters/kleine letters adreslijst uitvoeren.

        *   Twee knooppunten aan de taak toewijzen

        *   **Opdrachtregel** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Werkmap** - / openfoam/sloshingTank3D

6.  Afhankelijkheden toevoegen aan deze taken in oplopende taakvolgorde.

    ![Taakafhankelijkheden][task_dependencies]

7.  Klik op **verzenden** als deze taak wilt uitvoeren.

    Standaard indient HPC Pack de taak als uw huidige aangemelde gebruiker-account. Nadat u op **verzenden**klikt, ziet u mogelijk een dialoogvenster waarin u de gebruikersnaam en wachtwoord invoeren.

    ![Taak-referenties][creds]

    Onder bepaalde omstandigheden onthoudt HPC Pack de gegevens van de gebruiker u input vóór en worden niet weergegeven in het dialoogvenster. Als u HPC Pack opnieuw weergeven, voer de volgende opdracht opdrachtprompt en verzend de taak.

    ```
    hpccred delcreds
    ```

8.  De taak gaat uit tientallen minuten tot enkele uren op basis van de parameters die u hebt ingesteld voor de steekproef. In de heatmap ziet u de taak die is uitgevoerd op de knooppunten Linux. 

    ![Heatmap][heat_map]

    Klik op elk knooppunt worden acht processen gestart.

    ![Linux-processen][linux_processes]

9.  Als de taak is voltooid, moet u de resultaten van de taak zoeken in mappen onder C:\OpenFoam\sloshingTank3D en de logboekbestanden bij C:\OpenFoam.


## <a name="view-results-in-ensight"></a>De resultaten weergeven in EnSight

Desgewenst [EnSight](https://www.ceisoftware.com/) gebruiken om te visualiseren en de resultaten van de taak OpenFOAM analyseren. Zie voor meer informatie over de visualisatie en animaties in EnSight, deze [video handleiding](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Nadat u op het hoofd knooppunt EnSight hebt geïnstalleerd, kunt deze te starten.

2.  Open C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    U ziet een vervangen in de viewer.

    ![Vervangen in EnSight][tank]

3.  Een **Isosurface** maken op basis van **internalMesh**en kies vervolgens de variabele **alpha_water**.

    ![Een isosurface maken][isosurface]

4.  Hiermee stelt u de kleur voor **Isosurface_part** in de vorige stap hebt gemaakt. Bijvoorbeeld: Stel deze in op water blauwe.

    ![Isosurface kleur bewerken][isosurface_color]

5.  Maken van een **Iso-volume** van de **wanden** door **wanden** selecteren in het deelvenster **onderdelen** en klik op de knop **Isosurfaces** op de werkbalk.

6.  In het dialoogvenster Selecteer **Type** als **Isovolume** en stel de minimum van **Isovolume bereik** op 0,5. Als u wilt de isovolume maken, klikt u op **maken met geselecteerde onderdelen**.

7.  Hiermee stelt u de kleur voor **Iso_volume_part** in de vorige stap hebt gemaakt. Bijvoorbeeld: Stel deze in op uitgebreide water blauwe.

8.  De kleur voor **wanden**instellen. Stel deze bijvoorbeeld in op doorzichtige wit.

9. Klik nu op **afspelen** om de resultaten van de simulatie te zien.

    ![Resultaat vervangen][tank_result]

## <a name="sample-files"></a>Voorbeeldbestanden

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>XML-configuratie voorbeeldbestand voor cluster implementatie door de PowerShell-script

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Cred.xml voorbeeldbestand

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-to-install-mpi"></a>Silent.cfg voorbeeldbestand MPI installeren

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Voorbeeldscript voor settings.sh

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Voorbeeldscript voor hpcimpirun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png
