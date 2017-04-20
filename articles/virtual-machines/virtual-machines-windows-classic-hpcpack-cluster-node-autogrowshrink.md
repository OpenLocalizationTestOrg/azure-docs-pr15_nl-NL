<properties
 pageTitle="Automatisch schalen HPC Pack knooppunten | Microsoft Azure"
 description="Automatisch groter en kleiner maken van het aantal HPC Pack berekeningscluster knooppunten in Azure wordt aangegeven"
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
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automatisch groter en kleiner maken van de bronnen HPC Pack cluster in Azure op basis van de werklast cluster




Als u Azure "burst" knooppunten in uw cluster HPC Pack implementeert, of u een HPC Pack cluster in Azure VMs maken, kunt u een manier automatisch groter of kleiner het aantal Azure berekeningscluster resources zoals knooppunten of cores op basis van de huidige belasting op het cluster. Hiermee kunt u uw Azure resources efficiënter gebruiken en hun kosten te verifiëren.
Stel de eigenschap in HPC Pack cluster **AutoGrowShrink**hiervoor. U kunt ook het **AzureAutoGrowShrink.ps1** HPC PowerShell-script dat is geïnstalleerd met HPC Pack uitvoeren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Bovendien kunt momenteel u alleen automatisch groter en kleiner maken HPC Pack berekeningscluster knooppunten met een Windows Server-besturingssysteem.

## <a name="set-the-autogrowshrink-cluster-property"></a>Stel de eigenschap van de cluster AutoGrowShrink

### <a name="prerequisites"></a>Vereisten voor

* **HPC Pack 2012 R2 Update 2 of later cluster** - het hoofd knooppunt kan worden geïmplementeerd op lokale of in een VM Azure. Zie [een hybride cluster met HPC Pack instellen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) aan de slag met een on-premises implementatie hoofd knooppunt en Azure "burst" knooppunten. Zie de [HPC Pack IaaS implementatiescript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) om snel een HPC Pack cluster in Azure VMs implementeren.


* **Voor een cluster met een hoofd knooppunt in Azure** - als u de implementatiescript HPC Pack IaaS het cluster maakt, kunt u de eigenschap van de cluster **AutoGrowShrink** inschakelen door de optie AutoGrowShrink in het configuratiebestand cluster gebruikt. Zie de documentatie bij de [script downloaden](https://www.microsoft.com/download/details.aspx?id=44949)voor meer informatie. 

    U kunt ook de eigenschap van de cluster **AutoGrowShrink** inschakelen nadat u het cluster implementeert met behulp van HPC PowerShell-opdrachten die worden beschreven in de volgende sectie. Als u wilt voorbereiden voor deze, moet u eerst de volgende stappen uitvoeren:
    1. Een certificaat Azure management configureren op het hoofd knooppunt en van de Azure-abonnement. U kunt voor een test-implementatie gebruik van de standaard Microsoft HPC Azure zelfondertekend certificaat dat HPC taalpakket is geïnstalleerd op het hoofd knooppunt en gewoon dat certificaat uploaden naar uw Azure-abonnement. Zie de [richtlijnen van de TechNet-bibliotheek](https://technet.microsoft.com/library/gg481759.aspx)voor opties en stappen.
    2. **Regedit** worden uitgevoerd op het hoofd knooppunt, gaat u naar HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo en een nieuwe tekenreekswaarde toevoegen. Stelt u de naam van de waarde naar "Vingerafdruk" en Waardegegevens naar de vingerafdruk van het certificaat in stap 1.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>HPC PowerShell-opdrachten voor het instellen van de eigenschap AutoGrowShrink

Voorbeeld van de volgende manieren te werk zijn HPC PowerShell-opdrachten voor het instellen van **AutoGrowShrink** en om het gedrag met aanvullende parameters af te stellen. Zie [AutoGrowShrink parameters](#AutoGrowShrink-parameters) verderop in dit artikel voor de volledige lijst van instellingen. 

Als u wilt deze opdrachten uitvoeren, start u HPC PowerShell op de kop van het knooppunt als beheerder.

**De eigenschap AutoGrowShrink inschakelen**

    Set-HpcClusterProperty –EnableGrowShrink 1

**De eigenschap AutoGrowShrink uitschakelen**

    Set-HpcClusterProperty –EnableGrowShrink 0

**Het interval vergroten in minuten wijzigen**

    Set-HpcClusterProperty –GrowInterval <interval>

**Het interval verkleinen wijzigen in minuten**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**De huidige configuratie van AutoGrowShrink weergeven**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink parameters

Hier volgen AutoGrowShrink parameters die u met de opdracht **Set-HpcClusterProperty** kunt wijzigen.

* **EnableGrowShrink** - overschakelen naar de in- of uitschakelen van de eigenschap **AutoGrowShrink** .
* **ParamSweepTasksPerCore** - aantal parametrische opschonen taken om te laten groeien één core. De standaardinstelling is om te laten groeien één core per taak. 
 
    >[AZURE.NOTE] HPC Pack QFE KB3134307 verandert **ParamSweepTasksPerCore** in **TasksPerResourceUnit**. Er is gebaseerd op het taaktype van de resource en knooppunt, socket of core kan zijn.
    
* **GrowThreshold** - drempel van taken in de wachtrij voor automatische groei activeren. De standaardinstelling is 1, wat dat betekent als er 1 of meer taken in de wachtrij staat, automatisch vergroten knooppunten.
* **GrowInterval** - Interval in minuten om te activeren automatische groei. Het standaardinterval is 5 minuten.
* **ShrinkInterval** - Interval in minuten om te activeren automatische verkleinen. Het interval is standaard 5 minuten. |
* **ShrinkIdleTimes** - aantal continue controles verkleinen om aan te geven van de knooppunten zijn actief. De standaardinstelling is 3 tijden. Bijvoorbeeld als de **ShrinkInterval** 5 minuten is, HPC Pack Hiermee wordt gecontroleerd of het knooppunt niet actief geweest elke 5 minuten is. Als de knooppunten in de status niet-actief nadat 3 continue gecontroleerd (15 minuten), verkleint HPC Pack dat knooppunt.
* **ExtraNodesGrowRatio** - extra percentage van knooppunten om te laten groeien voor bericht Passing Interface (MPI) taken. De standaardwaarde is 1, wat betekent dat HPC Pack in omvang knooppunten 1% voor MPI taken groeit. 
* **GrowByMin** - schakeloptie om aan te geven of het beleid autogrow is gebaseerd op de minimale resources die zijn vereist voor de taak. De standaardinstelling is ONWAAR, wat betekent dat HPC Pack knooppunten voor taken op basis van de maximale resources die zijn vereist voor de taken in omvang groeit.
* **SoaJobGrowThreshold** - drempel van binnenkomende SOA aanvragen voor het activeren van de automatische groeien proces. De standaardwaarde is 50000.  
    
    >[AZURE.NOTE] Deze parameter wordt ondersteund in HPC Pack 2012 R2 Update 3 starten.
    
* **SoaRequestsPerCore** -aantal binnenkomende SOA aanvragen voor om te laten groeien één core. De standaardwaarde is 20000.  

    >[AZURE.NOTE] Deze parameter wordt ondersteund in HPC Pack 2012 R2 Update 3 starten.

### <a name="mpi-example"></a>Voorbeeld van MPI

Standaard HPC Pack 1% groeit extra knooppunten voor MPI taken (**ExtraNodesGrowRatio** is ingesteld op 1). De reden is dat MPI meerdere knooppunten vragen kan en de taak kan alleen worden uitgevoerd wanneer alle knooppunten gaat. Wanneer Azure wordt gestart knooppunten, af en toe één knooppunt mogelijk meer tijd nodig om te beginnen dan andere, veroorzaakt door andere knooppunten actief is terwijl u wacht op voor het knooppunt ter voorbereiden. Door groeiende extra knooppunten, HPC Pack Hiermee reduceert u deze resource wachttijd en slaat mogelijk kosten. Als u wilt vergroten het percentage van extra knooppunten voor MPI taken (bijvoorbeeld naar 10%), een vergelijkbaar met opdracht uitvoeren

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>SOA-voorbeeld

Standaard **SoaJobGrowThreshold** is ingesteld op 50000 en **SoaRequestsPerCore** is ingesteld op 200000. Als u één SOA taak met 70000 aanvragen indient, wordt één taak en verzoeken voor oproepen 70000 zijn. In dit geval HPC Pack in omvang groeit 1 core voor de taak in de wachtrij en voor verzoeken voor oproepen, kunt u het formaat (70000-50000) / core 20000 = 1, zodat in totaal 2 cores voor deze taak SOA zal toenemen.

## <a name="run-the-azureautogrowshrinkps1-script"></a>De AzureAutoGrowShrink.ps1-script uitvoeren

### <a name="prerequisites"></a>Vereisten voor

* **HPC Pack 2012 R2 Update 1 of later cluster** - het script **AzureAutoGrowShrink.ps1** is geïnstalleerd in de map % CCP_HOME % bin. Het hoofd knooppunt kan worden geïmplementeerd op lokale of in een VM Azure. Zie [een hybride cluster met HPC Pack instellen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) aan de slag met een on-premises implementatie hoofd knooppunt en Azure "burst" knooppunten. Zie het [HPC Pack IaaS implementatiescript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) snel implementeren een HPC Pack cluster in Azure VMs of gebruik een [Azure quickstart sjabloon](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).

* **Azure PowerShell 0.8.12** - het script momenteel is afhankelijk van deze specifieke versie van Azure PowerShell. Als u een latere versie op de kop knooppunt uitvoert, moet u mogelijk Azure PowerShell downgraden naar [versie 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) het script uitvoeren. 

* **Voor een cluster met Azure burst knooppunten** - het script uitvoeren op een clientcomputer waarop HPC taalpakket is geïnstalleerd, of op het hoofd knooppunt. Als uitgevoerd op een clientcomputer, controleert u of u de variabele $env ingesteld: CCP_SCHEDULER goed te laten verwijzen naar het hoofd knooppunt. De knooppunten Azure "burst" al moeten worden toegevoegd aan het cluster, maar ze mogelijk in de stand niet geïmplementeerd.


* **Voor een cluster geïmplementeerd in Azure VMs** - het script uitvoeren op het hoofd knooppunt VM, omdat dit hangt af van de **Begin-HpcIaaSNode.ps1** en **Stoppen HpcIaaSNode.ps1** scripts die zijn er geïnstalleerd. Deze scripts daarnaast een certificaat Azure management vereisen of instellingenbestand publiceren (Zie [beheren berekenen knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Zorg ervoor dat alle het knooppunt berekeningscluster VMs die u moet al zijn toegevoegd aan het cluster. Het is mogelijk dat ze in de stand gestopt.

### <a name="syntax"></a>Syntaxis

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Parameters

 * **NodeTemplates** - namen van de sjablonen voor knooppunt van Definieer het bereik voor de knooppunten vergroten en verkleinen. Als u niet opgeeft (de standaardwaarde is @()), alle knooppunten in de groep van **AzureNodes** knooppunt zijn in de scope wanneer **NodeType** een waarde van AzureNodes heeft en alle knooppunten in de groep van **ComputeNodes** knooppunt in de scope wanneer **NodeType** een waarde van ComputeNodes bevat.

 * **JobTemplates** - namen van de taaksjablonen naar Definieer het bereik voor de knooppunten om te laten groeien.

 * **NodeType** - het type knooppunt dat u wilt vergroten of verkleinen. Ondersteunde waarden zijn:

     * **AzureNodes** – voor Azure PaaS (burst) knooppunten in een on-premises of Azure IaaS cluster.

     * **ComputeNodes** - voor berekeningscluster knooppunt VMs alleen in een cluster Azure IaaS.

* **NumOfQueuedJobsPerNodeToGrow** - aantal wachtrijtaken vereist om te laten groeien één knooppunt.

* **NumOfQueuedJobsToGrowThreshold** - het minimumaantal gunstige wachtrijtaken om het formaat-proces te starten.

* **NumOfActiveQueuedTasksPerNodeToGrow** - het aantal actieve wachtrijtaken taken die zijn vereist om te laten groeien één knooppunt. Als **NumOfQueuedJobsPerNodeToGrow** is opgegeven met een waarde groter dan 0, wordt deze parameter genegeerd.

* **NumOfActiveQueuedTasksToGrowThreshold** - het minimumaantal gunstige actieve taken in de wachtrij om het formaat-proces te starten.

* **NumOfInitialNodesToGrow** - het eerste minimum aantal knooppunten om te laten groeien als alle knooppunten in een bereik **Niet geïmplementeerd** of **gestopt (Deallocated zijn)**.

* **GrowCheckIntervalMins** - het interval in minuten tussen controles om te laten groeien.

* **ShrinkCheckIntervalMins** - het interval in minuten tussen controles verkleinen.

* **ShrinkCheckIdleTimes** - het aantal continue verkleinen controles (gescheiden door **ShrinkCheckIntervalMins**) om aan te geven van de knooppunten actief zijn.

* **UseLastConfigurations** - de vorige configuraties in het argument-bestand opgeslagen.

* **ArgFile**- de naam van het argument-bestand dat is gebruikt om te slaan en de configuraties het script uitgevoerd bijwerken.

* **LogFilePrefix** - voorvoegsel van de naam van het logboekbestand. U kunt een pad opgeven. Het logboek is al dan niet standaard opgeslagen in de huidige werkmap.

### <a name="example-1"></a>Voorbeeld 1

Het volgende voorbeeld wordt de Azure burst knooppunten geïmplementeerd sjabloon AzureNode standaard naar groter en kleiner maken automatisch geconfigureerd. Als alle knooppunten zijn in eerste instantie in de stand **Niet geïmplementeerd** , worden ten minste 3 knooppunten gestart. Als het aantal wachtrijtaken groter is dan 8, begint het script knooppunten totdat hun getal groter is dan de verhouding tussen de opdrachten in de wachtrij **NumOfQueuedJobsPerNodeToGrow**. Als een knooppunt wordt gevonden in 3 inactief maal actief is, wordt deze is gestopt.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Voorbeeld 2

Het volgende voorbeeld wordt het knooppunt Azure berekeningscluster VMs geïmplementeerd sjabloon ComputeNode standaard naar groter en kleiner maken automatisch geconfigureerd.
De taken die is geconfigureerd door de standaardsjabloon voor de taak Definieer het bereik van de werklast op het cluster. Als alle knooppunten in eerste instantie zijn gestopt, worden ten minste 5 knooppunten gestart. Als het aantal actieve taken in de wachtrij groter is dan 15, begint het script knooppunten totdat hun getal groter is dan de verhouding tussen de actieve taken in de wachtrij naar **NumOfActiveQueuedTasksPerNodeToGrow**. Als een knooppunt wordt gevonden in 10 inactief maal actief is, wordt deze is gestopt.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
