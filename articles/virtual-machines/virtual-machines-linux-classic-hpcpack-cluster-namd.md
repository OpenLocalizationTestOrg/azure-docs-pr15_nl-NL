<properties
 pageTitle="NAMD met Microsoft HPC Pack op Linux VMs | Microsoft Azure"
 description="Een Microsoft HPC Pack cluster op Azure implementeren en uitvoeren van een simulatie NAMD met charmrun op meerdere Linux berekeningscluster knooppunten"
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
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>NAMD uitvoeren met Microsoft HPC Pack op Linux berekeningscluster knooppunten in Azure wordt aangegeven

In dit artikel leest u een manier om uit te voeren een werkbelasting Linux high performance computing (HPC) op Azure virtuele machines. U kunt u een [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster op Azure met Linux berekeningscluster knooppunten instellen en voert u een simulatie [NAMD](http://www.ks.uiuc.edu/Research/namd/) om te berekenen en de structuur van een systeem grote biomoleculaire visualiseren.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** is een parallelle moleculaire dynamics-pakket dat is ontworpen voor krachtige simulatie van grote biomoleculaire-systemen met maximaal miljoenen atomen (voor Nanoscale moleculaire Dynamics programma). Voorbeelden van deze systemen zijn virussen, cel structuren en grote eiwitten. NAMD schaal honderden cores voor typische simulaties en naar meer dan 500.000 cores voor de grootste simulaties.

* **Microsoft HPC Pack** biedt functies grootschalige HPC en parallelle toepassingen uitvoert in clusters van lokale computers of in een Azure virtuele machines. Oorspronkelijk ontwikkeld als een oplossing voor Windows HPC-werkbelastingen, HPC Pack nu ondersteunt het uitvoeren van de Linux HPC toepassingen op Linux knooppunt VMs geïmplementeerd in een cluster HPC Pack berekenen. Zie [aan de slag met Linux berekeningscluster knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md) voor een inleiding.

Zie [technische bronnen voor batch en high-performance computing](../batch/batch-hpc-solutions.md)voor andere opties voor het uitvoeren van de Linux HPC werkbelasting in Azure.




## <a name="prerequisites"></a>Vereisten voor

* **HPC Pack cluster met Linux berekenen knooppunten** - een HPC Pack cluster met Linux berekeningscluster knooppunten op Azure met behulp van een [Azure resourcemanager sjabloon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) of een [Azure PowerShell-script](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)implementeren. Zie [aan de slag met Linux berekeningscluster knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md) voor de vereisten en stappen voor beide opties. Als u de PowerShell-script implementatie optie kiest, raadpleegt u het voorbeeldbestand configuratie in de voorbeeldbestanden aan het einde van dit artikel. Dit bestand configureert een HPC Pack op basis van Azure-cluster dat bestaat uit een hoofd knooppunt van Windows Server 2012 R2 en vier grootte grote CentOS 6.6 berekeningscluster knooppunten. Pas dit bestand zo nodig voor uw omgeving.


* **De software en zelfstudie bestanden NAMD** - NAMD downloaden software voor Linux van de site [NAMD](http://www.ks.uiuc.edu/Research/namd/) (registratie vereist). In dit artikel is gebaseerd op NAMD versie 2.10 en het archief [Linux-x86_64 (64-bits Intel/AMD met Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) gebruikt. Ook de [NAMD Zelfstudievideo bestanden](http://www.ks.uiuc.edu/Training/Tutorials/#namd)downloaden. De downloads .tar bestanden zijn en u een Windows-venster voor het extraheren van de bestanden op de kop van het knooppunt nodig. Als u wilt extraheren van de bestanden, volgt u de instructies verderop in dit artikel. 

* **VMD** (optioneel) - om te zien van de resultaten van uw project NAMD download en installeer het programma moleculaire visualisatie [VMD](http://www.ks.uiuc.edu/Research/vmd/) op een computer van uw keuze. De huidige versie niet wordt 1.9.2. Zie de VMD site aan de slag downloaden.  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Vertrouwensrelatie tussen berekeningscluster knooppunten instellen
Het uitvoeren van een taak diverse knooppunten op meerdere Linux knooppunten, moet de knooppunten voor elkaar te vertrouwen (door **rsh** of **ssh**). Wanneer u het cluster HPC Pack met de Microsoft HPC Pack IaaS implementatiescript maakt, het script blijvend vertrouwensrelatie voor het beheerdersaccount die u opgeeft automatisch ingesteld. Voor gebruikers die geen beheerder u in van het cluster domein maakt, moet u tijdelijke vertrouwensrelatie tussen de knooppunten instellen wanneer een taak is toegewezen aan deze. De relatie vervolgens destroy wanneer de taak voltooid is. Als u wilt deze stap herhalen voor elke gebruiker, vindt u een combinatie van RSA sleutel aan de cluster HPC Pack waarbij gebruik wordt tot stand brengen van de vertrouwensrelatie. Hier volgen de instructies.

### <a name="generate-an-rsa-key-pair"></a>Een combinatie van RSA-sleutel genereren
Het is eenvoudig om te genereren van een paar voor belangrijke op RSA, waarin een openbare sleutel en een persoonlijke sleutel, door de Linux **ssh-keygen** -opdracht uit te voeren.

1.  Meld u aan bij een Linux-computer.

2.  Voer de volgende opdracht:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Druk op **Enter** om de standaardinstellingen gebruiken totdat de opdracht is voltooid. Voer geen een wachtwoordzin hier; Wanneer u wordt gevraagd om een wachtwoord, druk op **Enter**.

    ![Een combinatie van RSA-sleutel genereren][keygen]

3.  De map wijzigen naar de map ~/.ssh. De persoonlijke sleutel is opgeslagen in id_rsa en de openbare sleutel in id_rsa.pub.

    ![Persoonlijke en openbare sleutels][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>De combinatie van de sleutel toevoegen aan het cluster HPC Pack
1.  [Verbinding maken met extern bureaublad](virtual-machines-windows-connect-logon.md) met het hoofd knooppunt VM met het domein referenties u voorwaarde wanneer u het cluster (bijvoorbeeld hpc\clusteradmin) geïmplementeerd. U beheren het cluster vanuit het hoofd knooppunt.

2. Gebruik Windows Server standaardprocedures maken van een domein-gebruikersaccount in van het cluster Active Directory-domein. Bijvoorbeeld, gebruikt u het hulpprogramma Active Directory-gebruiker en Computers op het hoofd knooppunt. De voorbeelden in dit artikel wordt ervan uitgegaan dat u een domeingebruiker met de naam hpcuser in het domein hpclab (hpclab\hpcuser) maakt.

3. De domeingebruiker toevoegen aan het cluster HPC Pack als een gebruiker cluster. Zie voor instructies voor het [toevoegen of verwijderen cluster van gebruikers](https://technet.microsoft.com/library/ff919330.aspx).

2.  Een bestand met de naam C:\cred.xml maken en kopieer de RSA belangrijke gegevens erin. U kunt een voorbeeld vinden in de voorbeeldbestanden aan het einde van dit artikel.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Open een opdrachtprompt en voer de volgende opdracht uit om in te stellen van de gegevens van de referenties voor de hpclab\hpcuser-account. U de parameter **extendeddata** gebruiken om de naam van het bestand C:\cred.xml dat u hebt gemaakt voor de belangrijkste gegevens te geven.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Deze opdracht is voltooid zonder uitvoer. Sla het bestand cred.xml in een veilige locatie na het instellen van de referenties voor de gebruikersaccounts voor het uitvoeren van taken, of verwijder deze.

5.  Als u de combinatie van RSA-sleutel op een van de knooppunten Linux gegenereerd, moet u de toetsen verwijderen nadat u klaar bent met het ze te gebruiken. HPC Pack stelt geen vertrouwensrelatie Als er een bestaand id_rsa-bestand of id_rsa.pub-bestand.

>[AZURE.IMPORTANT] Wordt niet aanbevolen een Linux-taak als de beheerder van een cluster wordt uitgevoerd op een gedeelde cluster, omdat een taak die is verzonden door een beheerder uitgevoerd onder de hoofdmap-account op de knooppunten Linux. Een taak die is verzonden door een gebruiker die geen beheerder compatibel is met een lokale Linux-gebruikersaccount met dezelfde naam als de gebruiker van de taak. In dit geval HPC Pack ingesteld vertrouwensrelatie voor deze gebruiker Linux op alle knooppunten die zijn toegewezen aan de taak. U kunt de gebruiker Linux handmatig op de knooppunten Linux instellen voordat u de taak of HPC Pack de gebruiker automatisch gemaakt wanneer de taak wordt verzonden. Als HPC Pack wordt gemaakt van de gebruiker, HPC Pack wordt deze verwijderd nadat de taak is voltooid. Als u wilt verkleinen beveiligingsrisico, worden de gebruikte toetsen worden verwijderd nadat de taak is voltooid op de knooppunten.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Een bestandsshare voor Linux knooppunten instellen

Nu een bestandsshare SMB instellen en het koppelen van de gedeelde map op alle Linux knooppunten kunnen de knooppunten Linux voor toegang tot NAMD bestanden met een algemene pad. Hieronder vindt u stappen voor het koppelen van een gedeelde map op het hoofd knooppunt. Een delen geschikt is voor onderzoeken zoals CentOS 6.6 die momenteel niet ondersteund door de service Azure-bestand. Als uw knooppunten Linux wordt ondersteund in een bestandsshare van Azure, raadpleegt u [het gebruik van Azure bestandsopslag met Linux](../storage/storage-how-to-use-files-linux.md). Zie [aan de slag met Linux berekeningscluster knooppunten in een HPC Pack Cluster in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md)voor extra opties met HPC Pack delen van bestanden.

1.  Een map op het hoofd knooppunt maken en deze voor iedereen te delen door in te stellen bevoegdheden voor alleen-lezen/schrijven. In dit voorbeeld \\ \\CentOS66HN\Namd is de naam van de map, waarbij CentOS66HN de hostnaam van het hoofd knooppunt.

2. Een submap namd2 in de gedeelde map maken. Maak in namd2, een andere submap namdsample.

3. Pak de NAMD-bestanden in de map met behulp van een Windows-versie van **tar** of een ander hulpprogramma van Windows die wordt toegepast op .tar archieven. 
    * Het archief NAMD tar naar extraheren \\ \\CentOS66HN\Namd\namd2.
    
    * Pak de zelfstudie bestanden onder \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Open een Windows PowerShell-venster en voer de volgende opdrachten voor het koppelen van de gedeelde map op de knooppunten Linux.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

De eerste opdracht maakt een map genaamd /namd2 op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de gedeelde map //CentOS66HN/Namd/namd2 naar de map met dir_mode en file_mode bits ingesteld op 777. De *gebruikersnaam* en *wachtwoord* in de opdracht moet de referenties van een gebruiker op het hoofd knooppunt.

>[AZURE.NOTE]De "\`" symbool in het tweede is een escapeteken voor PowerShell. "\`," betekent dat de "," (kommateken) is een onderdeel van de opdracht.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Een script we vaker doen als u wilt uitvoeren van een taak NAMD maken

Uw werk NAMD moet *een knooppuntenbestand voor **charmrun** om te bepalen van het aantal knooppunten gebruiken bij het starten van NAMD processen* . U gebruikt een script we vaker doen die wordt uitgevoerd **charmrun** met deze knooppuntenbestand en genereert van de knooppuntenbestand. Vervolgens kunt u een taak NAMD in HPC Cluster Manager die dit script belt verzenden.

Met een teksteditor van uw keuze, een script we vaker doen in de /namd2-map met de NAMD programma-bestanden maken en noem deze hpccharmrun.sh. Kopieer het voorbeeld hpccharmrun.sh script gegeven aan het eind van dit artikel voor een snelle aankoopbewijs concept, en Ga naar [een taak NAMD verzenden](#submit-a-namd-job).

>[AZURE.TIP] Opslaan uw script als een tekstbestand met Linux regeleinden (alleen LF, niet CR LF). Dit zorgt ervoor dat deze wordt uitgevoerd correct op de knooppunten Linux.

Hieronder vindt u meer informatie over de werking van dit script we vaker doen. 

1.  Sommige variabelen definiëren.

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Knooppunt informatie opgehaald uit de omgevingsvariabelen. $NODESCORES slaat een lijst van gesplitste woorden uit $CCP_NODES_CORES. $COUNT is de grootte van $NODESCORES.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    De notatie voor de $CCP_NODES_CORES-variabele is als volgt:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Deze variabele bevat het totale aantal knooppunten, knooppuntnamen en aantal kernen op elk knooppunt dat aan de taak is toegewezen. Bijvoorbeeld als de taak moet worden 10 cores om uit te voeren, lijkt de waarde van $CCP_NODES_CORES op:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Als de $CCP_NODES_CORES variabele niet is ingesteld, gaat u **charmrun** rechtstreeks in. (Dit moet alleen worden uitgevoerd wanneer u dit script rechtstreeks op uw knooppunten Linux uitvoeren.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Of maak een knooppuntenbestand voor **charmrun**.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  **Charmrun** uitvoeren met de knooppuntenbestand, krijgen de status van het resultaat en verwijder de knooppuntenbestand aan het einde.

    ${CCP_NUMCPUS} is een andere omgevingsvariabele instellen door het hoofd knooppunt HPC Pack. Het aantal totale cores toegewezen aan deze taak wordt opgeslagen. We gebruiken deze om het aantal processen voor charmrun te geven.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Sluit af met de status van het resultaat **charmrun** .

    ```
    exit ${RTNSTS}
    ```



Hieronder volgt de informatie in de knooppuntenbestand, waardoor het script gegenereerd:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Bijvoorbeeld:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Een taak NAMD verzenden

U bent nu klaar om in te dienen van een taak NAMD in HPC Cluster Manager.

1.  Verbinding maken met uw cluster hoofd knooppunt en HPC Cluster Manager te starten.

2.  Zorg ervoor dat de Linux berekeningscluster knooppunten in de stand **Online zijn** in **Resourcebeheer**. Als dat niet het geval is, selecteert u deze en klik op **Online brengen**.

2.  Klik op **Nieuwe taak**in **Taakbeheer**.

3.  Voer een naam voor de taak zoals *hpccharmrun*.

    ![Nieuwe HPC taak][namd_job]

4.  Op de pagina **Details van de taak** , klikt u onder **Taak Resources**, selecteer het type resource als **knooppunt** en stel de **Minimum** op 3. , wordt de taak drie Linux knooppunten wordt uitgevoerd en elk knooppunt vier cores heeft.

    ![Taak resources][job_resources]

5. **Taken bewerken** in het linker navigatiegedeelte op en klik vervolgens op **toevoegen** aan een taak toevoegen aan de taak.    


6. Klik op de pagina **Details van de taak en i/o-omleiding** instellen de volgende waarden:

    * **Opdrachtregel** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] De voorgaande opdrachtregel is één opdracht zonder regeleinden. Deze terugloopt op meerdere regels onder **opdrachtregel**moet worden weergegeven.

    * **Werkmap** - /namd2

    * **Minimum** - 3

    ![Taakgegevens][task_details]

    >[AZURE.NOTE] U kunt instellen de werkmap hier omdat **charmrun** wordt geprobeerd om te navigeren naar de dezelfde werkmap op elk knooppunt. Als de werkmap niet is ingesteld, begint u met HPC Pack de opdracht in een willekeurige naam map op een van de knooppunten Linux gemaakt. Hierdoor wordt het volgende foutbericht weergegeven op de andere knooppunten: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` om te voorkomen dat dit probleem, Geef een mappad die kan worden geopend door alle knooppunten als de werkmap.

5.  Klik op **OK** en klik vervolgens op **verzenden** als deze taak wilt uitvoeren.

    Standaard indient HPC Pack de taak als uw huidige aangemelde gebruiker-account. Het dialoogvenster een mogelijk gevraagd dat u de gebruikersnaam en wachtwoord invoeren nadat u op **verzenden**.

    ![Taak-referenties][creds]

    Onder bepaalde omstandigheden onthoudt HPC Pack de gegevens van de gebruiker u input vóór en worden niet weergegeven in het dialoogvenster. Als u HPC Pack opnieuw weergeven, voer de volgende opdracht opdrachtprompt en verzend de taak.

    ```command
    hpccred delcreds
    ```

6.  De taak duurt enkele minuten om te voltooien.

7.  De functie log bij zoeken \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log en de uitvoerbestanden in \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  (Optioneel) begin VMD om de resultaten van uw project te bekijken. De stappen voor het visualiseren van de NAMD uitvoer van bestanden (in dit geval een ubiquitin eiwit molecuul in een bol water) zijn buiten het bereik van dit artikel. Zie [NAMD zelfstudie](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) voor meer informatie.

    ![Resultaten van de taak][vmd_view]

## <a name="sample-files"></a>Voorbeeldbestanden

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>XML-configuratie voorbeeldbestand voor cluster implementatie door de PowerShell-script

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
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

### <a name="sample-hpccharmrunsh-script"></a>Voorbeeldscript voor hpccharmrun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png
