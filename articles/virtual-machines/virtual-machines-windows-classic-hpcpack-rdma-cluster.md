<properties
 pageTitle="Een Windows-RDMA cluster instellen om uit te voeren MPI toepassingen | Microsoft Azure"
 description="Informatie over het maken van een Windows HPC Pack cluster met grootte H16r, H16mr, A8 of A9 VMs bij het netwerk Azure RDMA om uit te voeren MPI apps."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Instellen van een Windows-RDMA cluster met HPC Pack MPI toepassingen uit te voeren

Een Windows-RDMA cluster in Azure wordt aangegeven met [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) en [H-reeks of computerintensieve A-reeks exemplaren](virtual-machines-windows-a8-a9-a10-a11-specs.md) instellen voor het uitvoeren van parallelle bericht Passing Interface (MPI)-toepassingen. Wanneer u RDMA-ondersteuning, op basis van Windows Server knooppunten in een cluster HPC Pack hebt ingesteld, wordt MPI toepassingen efficiënt communiceren via een lage latentie, hoge doorvoer netwerk in Azure wordt aangegeven die is gebaseerd op externe directe geheugen access (RDMA) technologie.

Als u uitvoeren MPI werkbelasting op Linux VMs die toegang het netwerk Azure RDMA wilt tot, raadpleegt u [een cluster Linux RDMA MPI toepassingen uit te voeren instelt](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack cluster distributieopties
Microsoft HPC Pack is een hulpprogramma voor zover gratis maken HPC on-premises implementatie clusters of in Azure wordt aangegeven of uit te voeren Windows Linux HPC toepassingen. HPC taalpakket bevat een runtimeomgeving voor de Microsoft-implementatie van het bericht wordt doorgegeven Interface voor Windows ([MS-MPI). Wanneer gebruikt met RDMA-compatibele processen met een ondersteunde Windows Server-besturingssysteem, biedt HPC Pack een efficiënte optie Windows MPI toepassingen toegang het netwerk Azure RDMA tot uitvoeren. 

In dit artikel maakt u kennis met twee scenario's en koppelingen naar gedetailleerde instructies voor het instellen van een Winodws RDMA cluster met Microsoft HPC Pack. 

* Scenario 1. Implementeren computerintensieve werknemer rol exemplaren (PaaS)

* Scenario 2. Implementeren berekeningscluster knooppunten in computerintensieve VMs (IaaS)

Zie voor algemene vereisten voor het gebruik van computerintensieve exemplaren met Windows, [over H-reeks en computerintensieve A-reeks VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md) .



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Scenario 1. Implementeren computerintensieve werknemer rol exemplaren (PaaS)

Van een bestaand HPC Pack cluster, moet u resources van de extra berekeningscluster in Azure werknemer rol processen (Azure knooppunten) uitgevoerd in een cloudservice (PaaS) toevoegen. Deze functie, ook wel 'burst naar Azure"uit HPC Pack, ondersteunt een aanbod van formaten voor de werknemer rol exemplaren. Geef een van de RDMA-ondersteuning formaten bij het toevoegen van de Azure knooppunten.

Hier volgen moet denken en stappen moet barsten naar RDMA-ondersteuning Azure exemplaren van een bestaande (meestal on-premises implementatie) cluster. Met soortgelijke procedures kunt medewerker rol exemplaren toevoegen aan een HPC Pack hoofd knooppunt dat is geïmplementeerd in een VM Azure.

>[AZURE.NOTE] Zie [een hybride cluster met HPC Pack instelt](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)voor een zelfstudie moet barsten naar Azure met HPC Pack. Houd rekening met de overwegingen in de onderstaande stappen die van toepassing zijn specifiek aan RDMA-ondersteuning Azure knooppunten.

![Burst naar Azure][burst]

### <a name="steps"></a>Stappen

4. **Implementeren en configureren van een hoofd knooppunt HPC Pack 2012 R2**

    Download de meest recente HPC Pack-installatiepakket vanuit het [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=49922). Zie voor instructies voor het voorbereiden van een Azure burst-implementatie en vereisten [HPC Pack handleiding aan de slag](https://technet.microsoft.com/library/jj884144.aspx) en [Burst in Azure werknemer exemplaren met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

5. **Een management-certificaat configureren in het Azure abonnement**

    Een certificaat om de verbinding tussen het hoofd knooppunt en Azure secure configureren. Zie [scenario's voor het configureren van het certificaat voor het beheer van Azure voor HPC Pack](http://technet.microsoft.com/library/gg481759.aspx)voor opties en procedures. Voor test implementaties HPC Pack een standaard Microsoft HPC Azure Management certificaat geïnstalleerd u snel naar uw Azure-abonnement uploaden kunt.

6. **Een nieuwe cloudservice en een opslag-account maken**

    Gebruik de Azure klassieke portal maken van een cloudservice en een account opslag voor de implementatie in een gebied waar de RDMA-ondersteuning exemplaren beschikbaar zijn.

7. **Een sjabloon Azure knooppunt maken**

    Gebruik de Wizard knooppunt in HPC Cluster Manager maken. Zie [een Azure knooppunt-sjabloon maken](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) in "Stappen om te implementeren Azure knooppunten met Microsoft HPC Pack" voor stappen.

    Voor de eerste tests wordt u aangeraden een beleid handmatige beschikbaarheid configureren in de sjabloon.

8. **Knooppunten toevoegen aan de cluster**

    Gebruik de Wizard knooppunten in HPC Cluster Manager toevoegen. Zie [Azure-knooppunten toevoegen aan het Windows-HPC-Cluster](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add)voor meer informatie.

    Wanneer u de grootte van de knooppunten opgeeft, selecteert u een van de RDMA-ondersteuning exemplaar grootte.
    
    >[AZURE.NOTE]In elke burst naar Azure-implementatie waarbij de exemplaren computerintensieve implementeert HPC Pack automatisch een minimum van 2 RDMA-ondersteuning exemplaren (zoals A8) als proxy knooppunten, naast de Azure werknemer rol-exemplaren die u opgeeft. De proxy-knooppunten gebruik cores die zijn toegewezen aan het abonnement en kosten samen met de exemplaren van de rol Azure werknemer oplopen.

9. **Start (gegevens leveren) de knooppunten en zet ze online in de taken uitvoeren**

    Selecteer de knooppunten en gebruik de actie **starten** in HPC Cluster Manager. Wanneer de inrichting van is voltooid, selecteert u de knooppunten en de **Online brengen** actie in HPC Cluster Manager gebruiken. De knooppunten zich taken uitvoeren.

10. **Taken aan het cluster indienen**

    Gebruik HPC Pack taak indiening hulpmiddelen cluster taken uitvoeren. Zie [Microsoft HPC Pack: vervullen Management](http://technet.microsoft.com/library/jj899585.aspx).

11. **Stoppen (identiteitsgegevens) de knooppunten**

    Wanneer u klaar bent met het actieve taken, de knooppunten offline te zetten en gebruik de actie **stoppen** in HPC Cluster Manager.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Scenario 2. Implementeren berekeningscluster knooppunten in computerintensieve VMs (IaaS)

In dit scenario u het hoofd knooppunt HPC Pack implementeert en cluster knooppunten op VMs die zijn gekoppeld aan een Active Directory-domein in een netwerk met Azure virtuele berekenen. HPC Pack biedt een aantal [implementatieopties voor in Azure VMs](virtual-machines-linux-hpcpack-cluster-options.md), waaronder geautomatiseerde implementatie scripts en Azure quickstart sjablonen. Als u bijvoorbeeld de overwegingen en onderstaande stappen helpen u de [HPC Pack IaaS implementatiescript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) gebruiken om te automatiseren meest van dit proces.

![Cluster in Azure VMs][iaas]



### <a name="steps"></a>Stappen

1. **Maken van een cluster hoofd knooppunt en knooppunt VMs berekenen door de implementatiescript HPC Pack IaaS uit te voeren op een clientcomputer**

    De HPC Pack IaaS implementatiescript downloaden vanuit het [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=49922).

    Als u wilt de clientcomputer voorbereiden, het configuratiebestand script maken en het script uitvoeren, raadpleegt u [een HPC Cluster met het implementatiescript HPC Pack IaaS maken](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). 
    
    Als u wilt implementeren RDMA-ondersteuning berekeningscluster knooppunten, moet u de volgende aanvullende overwegingen:
    
    * **Virtuele netwerk** - geeft u een nieuwe virtueel netwerk in een gebied waarin de RDMA-ondersteuning exemplaar grootte die u wilt gebruiken beschikbaar is.

    * **Besturingssysteem Windows Server** - ondersteuning RDMA verbindingen, opgeven van een Windows Server 2012 R2 of Windows Server 2012 besturingssysteem wordt uitgevoerd voor het knooppunt berekeningscluster VMs.

    * **Cloudservices** - het is raadzaam te implementeren uw hoofd knooppunt in een cloudservice en uw berekeningscluster knooppunten in een andere cloudservice.

    * **De grootte van de knooppunt hoofd** - in dit scenario rekening houden met een grootte van ten minste A4 (Extra groot) voor het hoofd knooppunt.

    * **HpcVmDrivers extensie** - de implementatiescript de Azure VM-Agent en de extensie HpcVmDrivers wordt automatisch geïnstalleerd wanneer u implementeert grootte A8 of A9 berekenen van knooppunten met een Windows Server-besturingssysteem. HpcVmDrivers installeert stuurprogramma's op het knooppunt berekeningscluster VMs zodat ze verbinding met het netwerk RDMA maken kunnen.

    * Het HPC Pack cluster in topologie 5 (alle knooppunten in de Enterprise-netwerk) **cluster netwerkconfiguratie** - de implementatiescript automatisch ingesteld. Deze topologie is vereist voor alle HPC Pack cluster implementaties in VMs. Wijzig de netwerktopologie cluster niet later.

2. **De knooppunten berekeningscluster online brengen taken uitvoeren**

    Selecteer de knooppunten en de **Online brengen** actie in HPC Cluster Manager gebruiken. De knooppunten zich taken uitvoeren.

3. **Taken aan het cluster indienen**

    Verbinding maken met het hoofd knooppunt taken of een lokale computer hiervoor instellen. Zie voor informatie [Indienen taken aan een HPC cluster in Azure wordt aangegeven](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **De knooppunten offline te zetten en stoppen (toewijzing) ze**

    Wanneer u klaar bent met het actieve taken, wordt de knooppunten offline in HPC Cluster Manager duren. Gebruik vervolgens Azure beheerprogramma's ze om af te sluiten.



## <a name="run-mpi-applications-on-the-cluster"></a>MPI-toepassingen worden uitgevoerd op het cluster

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Voorbeeld: Mpipingpong uitgevoerd op een cluster HPC Pack

Als u wilt controleren of een HPC Pack-implementatie van de RDMA-ondersteuning exemplaren, voert u de HPC Pack **mpipingpong** -opdracht op het cluster. **mpipingpong** stuurt-pakketten van gegevens tussen gepaarde knooppunten herhaaldelijk voor het berekenen van latentie en doorvoer metingen en statistieken worden aangepast voor het netwerk RDMA-toepassingen. In dit voorbeeld ziet u een typisch patroon voor het uitvoeren van een taak MPI (in dit geval **mpipingpong**) met behulp van de clusteropdracht **mpiexec** .

In dit voorbeeld wordt ervan uitgegaan dat u hebt toegevoegd Azure knooppunten in een configuratie "burst naar Azure" ([Scenario 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article). Als u HPC Pack geïmplementeerd op een cluster van Azure VMs, moet u de syntaxis van de opdracht extra omgevingsvariabelen om te leiden netwerkverkeer bij het netwerk RDMA instellen om op te geven van een ander knooppunt groep wijzigen.


Mpipingpong op het cluster uitvoeren:


1. Open een opdrachtprompt op het hoofd knooppunt of op een correct geconfigureerde clientcomputer.

2. Als u wilt schatten latentie tussen paren van knooppunten in een Azure burst-implementatie van 4 knooppunten, typt u de volgende opdracht uit om in te dienen van een taak te mpipingpong uitvoeren met een pakketgrootte kleine en een groot aantal iteraties:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    De opdracht geeft als resultaat de ID van de taak die is verzonden.

    Als u het HPC Pack cluster geïmplementeerd op Azure VMs, Geef een groep knooppunt met geïmplementeerd knooppunt VMs geïmplementeerd in een enkel cloudservice berekenen en de opdracht **mpiexec** als volgt wijzigen:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. Als de taak is voltooid, om weer te geven van de uitvoer (in dit geval de uitvoer van taak 1 van de taak), typt u het volgende

    ```
    task view <JobID>.1
    ```

    waar &lt; *taak-id* &gt; is de ID van de taak die is verzonden.

    De uitvoer bevat latentie resultaten ongeveer als volgt uit.

    ![Ping pong latentie][pingpong1]

4. Als u wilt schatten doorvoersnelheid tussen paren van Azure burst knooppunten, typt u de volgende opdracht uit om in te dienen van een taak te **mpipingpong** uitvoeren met een groot pakketgrootte en een klein aantal iteraties:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    De opdracht geeft als resultaat de ID van de taak die is verzonden.

    Wijzig de opdracht, zoals uit stap 2 op een HPC Pack cluster geïmplementeerd op Azure VMs.

5. Als de taak is voltooid, om weer te geven van de uitvoer (in dit geval de uitvoer van taak 1 van de taak), typ het volgende:

    ```
    task view <JobID>.1
    ```

  De uitvoer bevat de volgende strekking doorvoerresultaten.

  ![Ping pong doorvoer][pingpong2]


### <a name="mpi-application-considerations"></a>Aandachtspunten voor de toepassing MPI


Hieronder volgen enkele overwegingen voor het uitvoeren van MPI toepassingen met HPC Pack in Azure wordt aangegeven. Enkele gelden alleen voor de implementatie van Azure knooppunten (werknemer rol exemplaren toegevoegd in een configuratie "burst naar Azure").

* Werknemer rol exemplaren in een cloudservice worden regelmatig door de service zonder kennisgeving door Azure (bijvoorbeeld voor systeemonderhoud of in een exemplaar is mislukt hoofdletters/kleine letters). Als een exemplaar is door de service terwijl deze een taak MPI wordt uitgevoerd, wordt het exemplaar verliest de gegevens en keert terug naar de stand wanneer deze is eerst geïmplementeerd, waardoor de taak MPI mislukt. De meer knooppunten die u gebruikt voor één taak MPI en hoe langer de taak wordt uitgevoerd, hoe groter de kans dat een van de exemplaren wordt door de service terwijl een taak wordt uitgevoerd. U moet ook rekening houden dit als u één knooppunt in de implementatie als een bestandsserver aanwijzen.


* Als u wilt uitvoeren MPI taken in Azure wordt aangegeven, moet u niet de RDMA-ondersteuning exemplaren gebruiken. U kunt elke exemplaar grootte die wordt ondersteund door HPC Pack gebruiken. De RDMA-compatibele processen worden echter aanbevolen voor het uitvoeren van relatief grootschalige MPI taken die zijn afhankelijk van de latentie en de bandbreedte van het netwerk dat verbinding maakt met de knooppunten. Als u met een andere grootte latentie en bandbreedte gevoelige MPI taken uitvoeren, wordt u aangeraden de uitvoering van kleine taken, waarin één taak wordt uitgevoerd op alleen een paar knooppunten.

* Toepassingen die zijn geïmplementeerd in Azure-exemplaren zijn onderhevig aan de licentievoorwaarden die is gekoppeld aan de toepassing. Neem contact op met de leverancier van een commerciële toepassing voor volumelicenties of andere beperkingen voor het uitvoeren van in de cloud. Niet alle leveranciers bieden pay-as-you-go licenties.


* Azure exemplaren nodig verdere instellen voor toegang tot on-premises implementatie knooppunten, waarden voor aandelen en licentieservers. U kunt bijvoorbeeld een netwerk naar website Azure virtuele configureren om in te schakelen van de Azure knooppunten voor toegang tot de licentieserver van een on-premises implementatie.


* Als u wilt uitvoeren MPI toepassingen op Azure exemplaren, elke MPI toepassing registreren met Windows Firewall op de exemplaren door de **hpcfwutil** -opdracht uit te voeren. Hierdoor MPI communicatie vindt plaats op een poort die door de firewall dynamisch is toegewezen.

    >[AZURE.NOTE] U kunt ook een firewall uitzondering opdracht automatisch wordt uitgevoerd op alle nieuwe Azure knooppunten die zijn toegevoegd aan uw cluster configureren voor burst naar Azure implementaties. Nadat u de opdracht **hpcfwutil** uitvoeren en controleer of uw toepassing werkt, kunt u de opdracht toevoegen aan een opstartscript voor uw Azure knooppunten. Zie [een Script opstarten voor Azure knooppunten gebruiken](https://technet.microsoft.com/library/jj899632.aspx)voor meer informatie.



* De omgevingsvariabele CCP_MPI_NETMASK cluster HPC Pack gebruikt om op te geven van een reeks aanvaardbaar adressen voor MPI communicatie. Begin in HPC Pack 2012 R2 en de omgevingsvariabele CCP_MPI_NETMASK cluster geldt alleen voor MPI communicatie tussen een domein behoren berekeningscluster knooppunten (beide on-premises implementatie of in Azure VMs). De variabele wordt door knooppunten in een burst toegevoegd aan Azure configuratie genegeerd.


* MPI taken uitvoeren niet via Azure exemplaren die zijn geïmplementeerd in verschillende cloudservices (bijvoorbeeld in burst met Azure-installaties met verschillende knooppunt sjablonen of Azure VM berekeningscluster knooppunten geïmplementeerd in meerdere cloudservices). Als er meerdere Azure knooppunt implementaties die met verschillende knooppunt sjablonen worden gestart, wordt de taak MPI moet uitvoeren op slechts één set Azure knooppunten.


* Wanneer u Azure knooppunten aan uw cluster toevoegen en ze online weer, wordt de HPC taak Scheduler-Service direct probeert te starten van taken op de knooppunten. Als er slechts een deel van uw werkbelasting op Azure uitvoeren kunt, zorg ervoor dat u bijwerkt of taaksjablonen om te definiëren wat taaktypen kunnen worden uitgevoerd op Azure maken. Bijvoorbeeld, om ervoor te zorgen dat taken die zijn verzonden met taaksjablonen alleen worden uitgevoerd op Azure knooppunten, de eigenschap knooppuntgroepen toevoegen aan de taaksjabloon en selecteer AzureNodes als de gewenste waarde. Als u wilt maken van aangepaste groepen voor uw Azure knooppunten, gebruikt u de toevoegen-HpcGroup HPC PowerShell-cmdlet.


## <a name="next-steps"></a>Volgende stappen

* Ontwikkel als alternatief voor het gebruik van HPC Pack, met de Batch Azure-service MPI toepassingen uitvoert op beheerde groepen berekeningscluster knooppunten in Azure wordt aangegeven. Zie [Gebruik meerdere exemplaren taken aan het bericht Passing Interface (MPI) applicaties in Azure Batch](../batch/batch-mpi.md).

* Als u uitvoeren Linux MPI toepassingen die toegang het netwerk Azure RDMA wilt tot, raadpleegt u het [instellen van een cluster Linux RDMA MPI toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png