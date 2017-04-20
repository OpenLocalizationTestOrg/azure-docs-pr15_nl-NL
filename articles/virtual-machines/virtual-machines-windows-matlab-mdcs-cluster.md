<properties
   pageTitle="MATLAB clusters op virtuele machines | Microsoft Azure"
   description="Gebruik van Microsoft Azure virtuele machines MATLAB verdeeld Computing-serverclusters om uit te voeren uw computerintensieve parallelle MATLAB werkbelasting maken"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mscurrell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Windows"
   ms.workload="infrastructure-services"
   ms.date="05/09/2016"
   ms.author="markscu"/>

# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>MATLAB Distributed Computing serverclusters op Azure VMs maken 

Gebruik Microsoft Azure virtuele machines maken van een of meer MATLAB verdeeld Computing serverclusters om uit te voeren uw computerintensieve parallelle MATLAB werkbelasting. Uw MATLAB Distributed Computing Server-software installeren op een VM wilt gebruiken als basis afbeelding en een Azure quickstart sjabloon of Azure PowerShell-script (beschikbaar op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) te implementeren en beheren van het cluster. Na implementatie, verbinding maken met het cluster uw werkbelasting uitvoeren. 

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>Over MATLAB en MATLAB Distributed Computing Server 

Het platform [MATLAB](http://www.mathworks.com/products/matlab/) is geoptimaliseerd voor het oplossen van problemen met technische en wetenschappelijke. MATLAB gebruikers met grootschalige simulaties en gegevensverwerking taken kunnen MathWorks parallel computing producten hun werkbelasting computerintensieve versnellen door te profiteren van computerclusters en raster services gebruiken. [Parallelle Computing werkset](http://www.mathworks.com/products/parallel-computing/) kunt MATLAB gebruikers parallelize toepassingen en profiteren van meerdere core-processors, GPU's, en clusters berekenen. [Server voor MATLAB gedistribueerde Computing](http://www.mathworks.com/products/distriben/) kunnen gebruikers MATLAB gebruikmaken van meerdere computers op een berekeningscluster. 


U kunt MATLAB Distributed Computing serverclusters met dezelfde regelingen beschikbaar zijn voor het indienen van parallelle werk als on-premises implementatie kolomgroepen zoals interactieve projecten, taken, onafhankelijke taken en communiceren taken maken met behulp van Azure virtuele machines. Veel voordelen vergeleken met de inrichting van Azure gebruiken in combinatie met het platform MATLAB heeft en gebruik van traditionele on-premises hardware: een bereik van virtuele machine die groter zijn, het maken van clusters op aanvraag zodat u alleen voor de resources berekeningscluster u betaalt gebruik en de mogelijkheid om te testen modellen op schaal.  

## <a name="prerequisites"></a>Vereisten voor

* **Clientcomputer** - moet u een Windows-clientcomputer om te communiceren met Azure en het cluster MATLAB Distributed Computing Server na implementatie. 

* **Azure PowerShell** - Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) om het te installeren op de clientcomputer. 

* **Azure abonnement** - als u geen een abonnement hebt, kunt u een [account vrij te geven](https://azure.microsoft.com/free/) in een paar minuten. Voor grotere kolomgroepen kunt u bovendien een pay-as-you-go abonnement of andere opties voor het aanschaffen. 

* **Quotum voor cores** - moet u mogelijk het quotum core als u wilt implementeren van een grote cluster of meer dan één MATLAB Distributed Computing Server cluster vergroten. Een quotum, [opent u een verzoek voor ondersteuning van online-klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) gratis verhogen. 

* **MATLAB, parallelle Computing werkset, en MATLAB Distributed Computing Server licenties** - de scripts wordt ervan uitgegaan dat dat de [MathWorks die worden gehost Licentiebeheer](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) wordt gebruikt voor alle licenties.  

* **MATLAB Distributed Computing serversoftware** - wordt geïnstalleerd op een VM die wordt gebruikt als de afbeelding VM voor het cluster VMs. 


## <a name="high-level-steps"></a>Hoog niveau stappen

Als u wilt gebruiken Azure virtuele machines voor uw MATLAB Distributed Computing serverclusters, zijn de volgende algemene stappen vereist. Gedetailleerde instructies zijn in de documentatie bij de Snelstartgids sjabloon en scripts op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Een basis VM-afbeelding maken**  
    * Download en installeer MATLAB Distributed Computing Server-software naar deze VM. 

    >[AZURE.NOTE]Dit proces kan een paar uur duren, maar u hoeft te doen als voor elke versie van MATLAB die u gebruikt.   
    
2. **Een of meer clusters maken**  
    * Gebruik van de opgegeven PowerShell-script of de sjabloon quickstart een cluster van de basis VM-afbeelding maken.   
    * De clusters met het opgegeven PowerShell-script waarmee u een lijst met, onderbreken, hervatten kunt beheren en clusters verwijderen. 
 
## <a name="cluster-configurations"></a>Clusterconfiguraties 

Op dit moment kunnen het script voor het maken van cluster en de sjabloon u een enkel MATLAB Distributed Computing Server topologie maken. Desgewenst kunt u een of meer aanvullende kolomgroepen maken met elk cluster met een verschillend aantal werknemer VMs, met verschillende tekengrootten VM, enzovoort. 

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB-client en cluster in Azure wordt aangegeven 

Het deelvenster MATLAB client knooppunt, MATLAB taak Scheduler knooppunt en MATLAB Distributed Computing Server "werknemer" knooppunten zijn alle geconfigureerd als Azure VMs in een virtueel netwerk, zoals wordt weergegeven in de volgende afbeelding. 

![Cluster topologie](./media/virtual-machines-windows-matlab-mdcs-cluster/mdcs_cluster.png)

* Als u wilt gebruiken het cluster, verbinding met extern bureaublad naar het knooppunt client. Het knooppunt client wordt uitgevoerd de MATLAB-client. 

* Het knooppunt client heeft een bestandsshare die kan worden geopend door alle werknemers.

* MathWorks die worden gehost Licentiebeheer wordt gebruikt voor de controles licentie voor alle MATLAB software. 

* Standaard één MATLAB Distributed Computing Server werknemer per core op de werknemer VMs is gemaakt, maar kunt u niet het getal opgeven. 


## <a name="use-an-azure-based-cluster"></a>Gebruik een Cluster op basis van Azure 

Als met andere soorten MATLAB Distributed Computing Server kolomgroepen moet u de Profielbeheer Cluster in de MATLAB-client (op de client VM) gebruiken om een MATLAB taak Scheduler cluster profiel te maken.

![Cluster Profielbeheer](./media/virtual-machines-windows-matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Volgende stappen

* Zie de [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) -bibliotheek met de sjablonen en scripts voor gedetailleerde instructies te implementeren en beheren van MATLAB Distributed Computing serverclusters in Azure wordt aangegeven. 

* Ga naar de [site MathWorks](http://www.mathworks.com/) gedetailleerde informatie voor MATLAB en MATLAB Distributed Computing Server.
