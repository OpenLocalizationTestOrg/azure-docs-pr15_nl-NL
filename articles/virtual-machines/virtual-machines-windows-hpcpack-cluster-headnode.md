<properties
 pageTitle="Maken van een HPC Pack hoofd knooppunt in een VM Azure | Microsoft Azure"
 description="Informatie over het gebruik van de Azure-portal en het implementatiemodel resourcemanager maken van een Microsoft HPC Pack hoofd knooppunt in een VM Azure."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/17/2016"
 ms.author="danlep"/>

# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Het hoofd knooppunt van een cluster HPC Pack maken in een VM Azure met een Marketplace-afbeelding


Een [Microsoft HPC Pack VM afbeelding](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) van de Azure Marketplace en de Azure-portal gebruiken om het hoofd knooppunt van een HPC cluster te maken. In deze afbeelding HPC Pack VM is gebaseerd op Windows Server 2012 R2 Datacenter HPC Pack 2012 R2 Update 3 vooraf geïnstalleerd. Gebruik dit hoofd knooppunt voor een aankoopbewijs concept-implementatie van HPC taalpakket in Azure wordt aangegeven. U kunt vervolgens berekeningscluster knooppunten toevoegen aan het cluster HPC werkbelasting uitvoeren.



>[AZURE.TIP]Als u wilt implementeren een volledige HPC Pack cluster in Azure wordt aangegeven dat het hoofd knooppunt en berekeningscluster knooppunten bevat, is het raadzaam een geautomatiseerde methode te gebruiken. Opties voor opnemen het [implementatiescript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) en de [cluster HPC Pack voor Windows-werkbelastingen](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) resourcemanager sjabloon. Zie [Opties voor HPC Pack cluster in Azure wordt aangegeven](virtual-machines-windows-hpcpack-cluster-options.md) voor aanvullende sjablonen. 


## <a name="planning-considerations"></a>Overwegingen bij de planning

In de volgende afbeelding ziet u het HPC Pack hoofd knooppunt in een Active Directory-domein in een netwerk met Azure virtuele implementeren.

![HPC Pack hoofd knooppunt][headnode]

* **Active Directory-domein** - het HPC Pack hoofd knooppunt moet worden toegevoegd aan een Active Directory-domein in Azure wordt aangegeven voordat u de HPC-services op de VM begint. Zoals in dit artikel voor een aankoopbewijs concept implementatie, kunt u de VM die u voor het hoofd knooppunt als domeincontroller maakt voordat u begint met de services HPC promoveren. Er is een andere optie om een afzonderlijk domeincontroller en bos in Azure wordt aangegeven waaraan u deelneemt aan het hoofd knooppunt VM te implementeren.

* **Azure virtuele netwerk** - wanneer u het implementatiemodel resourcemanager gebruikt voor het hoofd knooppunt implementeren, die u opgeven of een Azure virtuele netwerk maken. U kunt het virtuele netwerk gebruiken als u moet het hoofd knooppunt toevoegen aan een bestaande Active Directory-domein. Moet u ook deze later wilt berekenen knooppunt VMs toevoegen aan het cluster.

    
## <a name="steps-to-create-the-head-node"></a>Stappen voor het maken van het hoofd knooppunt

Volgen hoofdstappen gebruik van de Azure-portal een VM Azure voor het hoofd knooppunt HPC Pack maken met behulp van het implementatiemodel resourcemanager-. 


1. Als u maken van een nieuwe Active Directory-bos in Azure wordt aangegeven met afzonderlijk domeincontroller VMs wilt, is Eén optie met een [resourcemanager sjabloon](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/). Voor een eenvoudige aankoopbewijs concept implementatie is dit fijn voor deze stap overslaan en configureren van het hoofd knooppunt VM zelf als domeincontroller. Deze optie wordt verderop in dit artikel beschreven.
    
2. Klik op de [HPC Pack 2012 R2 op Windows Server 2012 R2 pagina](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) in de Azure Marketplace, op **VM maken**. 

3. Selecteer het model **Resourcemanager** -implementatie in de portal, klik op de pagina **HPC Pack 2012 R2 op Windows Server 2012 R2** en klik op **maken**.

    ![Afbeelding van de HPC Pack][marketplace]

4. Gebruik de portal om de instellingen configureren en de VM te maken. Als u geen ervaring met Azure hebt, volgt u de zelfstudie [maken van een virtuele Windows-computer in de portal van Azure](virtual-machines-windows-hero-tutorial.md). Voor een aankoopbewijs concept implementatie, u kunt meestal accepteer de standaardwaarde of aanbevolen instellingen.

    >[AZURE.NOTE]Als u het hoofd knooppunt toevoegen aan een bestaande Active Directory-domein in Azure wordt aangegeven wilt, zorg er dan voor dat u het virtuele netwerk voor dat domein opgeven bij het maken van de VM.
       
4. Nadat u de VM hebt gemaakt en de VM wordt uitgevoerd, [verbinding maken met de VM](virtual-machines-windows-connect-logon.md) door extern bureaublad. 

5. De VM te koppelen aan een bestaand domein of maakt u een domein op de VM zelf.

    * Als u de VM in een Azure virtuele netwerk met een bestaand domein gemaakt, deelnemen aan vergadering de VM naar de bos met hulpmiddelen voor de standaard Serverbeheer of Windows PowerShell. Start.

    * Als u de VM gemaakt in een nieuw virtuele netwerk (zonder een bestaand domein), moet u vervolgens de VM als domeincontroller promoveren. Gebruik standaard stappen voor het installeren en configureren van de rol van Active Directory Domain Services op het hoofd knooppunt. Zie [een nieuwe Windows Server 2012 Active Directory bos installeren](https://technet.microsoft.com/library/jj574166.aspx)voor meer informatie.

5. Nadat de VM wordt uitgevoerd en is gekoppeld aan een Active Directory-bos, start u de HPC Pack-services als volgt:

    een. Verbinding maken met het hoofd knooppunt VM met behulp van een domeinaccount die deel uitmaakt van de lokale beheerdersgroep. Bijvoorbeeld, gebruikt u het beheerdersaccount dat u instellen wanneer u het hoofd knooppunt VM hebt gemaakt.

    b. Windows PowerShell starten als een beheerder voor een standaard-hoofd knooppunt-configuratie, en typt u het volgende:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Het kan enkele minuten duren voor de services HPC Pack te starten.

    Voor extra hoofd knooppunt configuratieopties, typt u `get-help HPCHNPrepare.ps1`.


## <a name="next-steps"></a>Volgende stappen

* Nu kunt u werken met het hoofd knooppunt van uw cluster HPC Pack. Bijvoorbeeld start HPC Cluster Manager en voert u de [Implementatie takenlijst te klikken](https://technet.microsoft.com/library/jj884141.aspx).
* [Als u wilt laten oplopen het cluster capaciteit op aanvraag, berekenen Azure burst knooppunten](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) toevoegen in een cloudservice. 

* Voer de werklast van een toets op het cluster. Zie de HPC Pack- [handleiding aan de slag](https://technet.microsoft.com/library/jj884144)voor een voorbeeld.

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png
