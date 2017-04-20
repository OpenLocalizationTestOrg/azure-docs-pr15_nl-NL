<properties
 pageTitle="Burst knooppunten toevoegen aan een cluster HPC Pack | Microsoft Azure"
 description="Leer hoe u een HPC Pack cluster in Azure op aanvraag door toe te voegen werknemer rol exemplaren die actief zijn in een cloudservice uitvouwen"
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
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Knooppunten op aanvraag 'burst' toevoegen aan een cluster HPC Pack in Azure wordt aangegeven



Als u een [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure hebt ingesteld, wilt u misschien een manier om snel de capaciteit cluster schalen omhoog of omlaag, zonder een set vooraf geconfigureerde berekeningscluster knooppunt VMs onderhouden. Dit artikel leest u hoe u het op aanvraag "burst" knooppunten (werknemer rol exemplaren wordt uitgevoerd in een cloudservice) toevoegen als berekeningscluster resources aan een kop knooppunt in Azure wordt aangegeven. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Knooppunten burst][burst]

De stappen in dit artikel kunt u snel Azure knooppunten toevoegen aan een cloudgebaseerde HPC Pack hoofd knooppunt VM voor een test of bewijs-van-concept-implementatie. De algemene stappen zijn hetzelfde als de stappen voor het "burst naar Azure" om toe te voegen cloud capaciteit aan een on-premises implementatie HPC Pack cluster berekenen. Zie [een hybride instellen berekenen cluster met Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)voor een zelfstudie. Zie [Burst naar Azure met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)voor gedetailleerde instructies en overwegingen voor productie implementaties.


## <a name="prerequisites"></a>Vereisten voor

* **HPC Pack hoofd knooppunt geïmplementeerd in een VM Azure** - kunt u een zelfstandig hoofd knooppunt VM of een die deel uitmaakt van een grotere cluster. Zie [Deploy een HPC Pack hoofd knooppunt in een VM Azure](virtual-machines-windows-hpcpack-cluster-headnode.md)maken van een zelfstandig hoofd knooppunt. Zie [Opties voor het maken en beheren van een Windows-HPC-cluster in Azure wordt aangegeven met Microsoft HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md)voor geautomatiseerde HPC Pack cluster opties voor distributie.

    >[AZURE.TIP] Als u het [implementatiescript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) gebruikt voor het maken van de cluster in Azure wordt aangegeven, kunt u Azure burst knooppunten opnemen in uw geautomatiseerde implementatie. Zie de voorbeelden in dit artikel.

* **Azure abonnement** - om toe te voegen Azure knooppunten, kunt u hetzelfde abonnement gebruikt om te implementeren het hoofd knooppunt VM, of een ander abonnement (of abonnementen).

* **Quotum voor cores** - moet u mogelijk Verhoog het quotum van cores, met name als u wilt implementeren van verschillende Azure knooppunten met multicore grootte. Een quotum, [opent u een verzoek voor ondersteuning van online-klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) gratis verhogen.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Stap 1: Maak een cloudservice en een account opslag voor de Azure knooppunten

Gebruik van de Azure klassieke portal of equivalente hulpmiddelen voor het configureren van de volgende bronnen die nodig zijn voor uw Azure knooppunten implementeren:

* Een nieuwe Azure cloudservice
* Een nieuw account voor de opslag van Azure

>[AZURE.NOTE] Niet een bestaande cloudservice in uw abonnement niet opnieuw gebruiken. 

**Overwegingen**

* Configureer een aparte cloudservice voor elke Azure knooppunt sjabloon die u van plan bent om te maken. U kunt echter hetzelfde account opslag gebruiken voor meerdere knooppunt sjablonen.

* Het is raadzaam dat u de cloudservice en het account opslag voor de implementatie in de dezelfde Azure regio vinden.




## <a name="step-2-configure-an-azure-management-certificate"></a>Stap 2: Een certificaat Azure management configureren

Als u Azure knooppunten als berekeningscluster resources toevoegen, moet u een certificaat management op de kop knooppunt en uploaden naar het Azure abonnement gebruikt voor de implementatie van een bijbehorend certificaat.

In dit scenario kunt u de **Standaard HPC Azure Management certificaat** dat HPC Pack installeert en configureert automatisch op het hoofd knooppunt kiezen. Dit certificaat is handig voor het testen van doeleinden en bewijs-van-concept implementaties. Upload het bestand C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer vanaf het hoofd knooppunt VM aan het abonnement als u wilt gebruiken dit certificaat. Klik op **Instellingen**om het certificaat in de [portal van Azure klassieke](https://manage.windowsazure.com) > **Management certificaten**.

Zie [scenario's voor het configureren van het certificaat voor het beheer van Azure voor Azure Burst implementaties](http://technet.microsoft.com/library/gg481759.aspx)voor extra opties voor het configureren van het certificaat management.

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Stap 3: Azure knooppunten implementeren naar het cluster



De stappen voor het toevoegen en starten van Azure knooppunten in dit scenario zijn meestal hetzelfde als de stappen met een on-premises implementatie hoofd knooppunt. Zie de volgende secties in [stappen voor het implementeren van Azure knooppunten met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx)voor meer informatie:

* Een sjabloon Azure knooppunt maken

* Azure knooppunten toevoegen aan de Windows-HPC cluster

* (Gegevens leveren) de Azure knooppunten starten

Nadat u toevoegen en starten van de knooppunten, zijn ze klaar voor gebruik cluster taken uitvoeren.

Als u problemen ondervindt bij het implementeren van Azure knooppunten, raadpleegt u [Problemen met implementaties van Azure knooppunten met Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Volgende stappen

* Zie de overwegingen een vraag over [H-reeks en computerintensieve A-reeks VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md)voor informatie over het gebruik van een exemplaar van computerintensieve-grootte voor de knooppunten burst.

* Als u wilt automatisch groter of kleiner van de Azure bronnen op basis van de werklast cluster, raadpleegt u [automatisch groter en kleiner maken van Azure berekeningscluster resources in een cluster HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png
