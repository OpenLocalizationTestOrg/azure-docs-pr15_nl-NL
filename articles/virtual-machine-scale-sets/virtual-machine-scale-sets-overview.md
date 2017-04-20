<properties
    pageTitle="VM schaal Sets overzicht | Microsoft Azure"
    description="Meer informatie over VM schaal Sets"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

# <a name="virtual-machine-scale-sets-overview"></a>VM schaal Sets overzicht

VM schaal sets zijn een resource Azure berekenen die u gebruiken kunt om te implementeren en beheren van een reeks identieke VMs. Met alle VMs geconfigureerd hetzelfde, VM schaal sets ter ondersteuning van waar automatisch schalen – geen vooraf inrichting van VMs is vereist – en zo eenvoudiger grootschalige services doelgroepen groot berekeningscluster, big data en beperkte werkbelasting maken.

Voor toepassingen moet die schaal berekeningscluster resources uit- en schaal bewerkingen impliciet zijn verdeeld over domeinen foutenstructuuranalyse en bijwerken. Een inleiding tot VM schaal Raadpleeg sets voor de [aankondiging van Azure blog](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Bekijk deze video's voor meer informatie over VM schaal sets:

 - [Mark Russinovich moment spreekt Azure schaal Sets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [VM schaal Sets met man Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Hiermee stelt u maken en beheren van VM schaal

U kunt een VM schaal instellen in de [portal van Azure](https://portal.azure.com) maken door _Nieuw_ te selecteren en typen in 'schaal' in de zoekbalk. 'VM schaal instellen' ziet u in de zoekresultaten. Hier kunt u de vereiste velden aan te passen en implementeren van een set schaal invullen. 

VM schaal sets kunnen ook worden gedefinieerd en geïmplementeerd met behulp van JSON sjablonen en [REST API's](https://msdn.microsoft.com/library/mt589023.aspx) alleen zoals afzonderlijke Azure resourcemanager VMs. Daarom kunnen standaard Azure resourcemanager implementatiemethoden worden gebruikt. Zie voor meer informatie over sjablonen, [Azure resourcemanager Authoring sjablonen](../resource-group-authoring-templates.md).

Een reeks voorbeeld sjablonen voor VM schaal sets vindt u in de bibliotheek van Azure Quickstart sjablonen GitHub [hier.](https://github.com/Azure/azure-quickstart-templates) (zoek naar sjablonen met _vmss_ in de titel)

In de projectdetailpagina's voor deze sjablonen ziet u een knop die naar de portal-implementatie-functie verwijst. Het instellen van de schaal VM implementeren, klik op de knop en vul alle parameters die in de portal vereist zijn. Als u niet precies weet of een resource ondersteunt hoofdletters of gemengde hoofdletters/kleine letters, het is veiliger kleine letters parameterwaarden altijd wilt gebruiken. Er is ook een handige video sectie van een VM schaal instellen sjabloon hier:

[VM schaal sjabloon sectie instellen](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Uit- en schaalbaarheid van de schaal van een VM instellen

Als u wilt vergroten of verkleinen van het aantal virtuele machines in een VM schaal instellen, wijzigen van de eigenschap _capaciteit_ en implementeer deze opnieuw de sjabloon. Deze eenvoudig kunt u gemakkelijk te schrijven van uw eigen aangepaste schaal laag als u wilt definiëren van een aangepaste schaal gebeurtenissen die niet worden ondersteund door Azure automatisch schalen.

Als u opnieuw van een sjabloon als u wilt wijzigen van de capaciteit distribueren, kunt u veel kleinere sjabloon die u alleen de SKU en de bijgewerkte capaciteit bevat definiëren. Een voorbeeld hiervan wordt weergegeven [hier.](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing)

Als u wilt Doorloop de stappen die een schaal instellen die automatisch wordt aangepast maakt, raadpleegt u [Automatisch schaal computers in een virtuele Machine schaal instellen](virtual-machine-scale-sets-windows-autoscale.md)

## <a name="monitoring-your-vm-scale-set"></a>Cmdlets voor controle VM schaal instellen

De [Azure portal](https://portal.azure.com) lijsten schaal sets en ziet u eenvoudige eigenschappen, evenals vermelding VMs in de set. Voor meer informatie kunt u de [Azure Resource Explorer](https://resources.azure.com) VM schaal sets weergeven. VM schaal sets zijn een resource onder Microsoft.Compute, zodat u vanaf deze site kunt u ze zien door uit te vouwen van de volgende koppelingen:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>VM schaal instellen scenario 's

In deze sectie bevat enkele typische VM set scenario's voor schaal. Sommige hogere niveau Azure services (zoals batches, Service stof en Azure Container-Service) wordt gebruikt in deze scenario's.

 - **RDP / SSH VM schaalfactor instellen exemplaren** - A VM schaal instellen wordt gemaakt in een VNET en het openbare IP-adressen niet aantal afzonderlijke VMs in de set schaal zijn toegewezen. Dit is een goede ding omdat u doorgaans niet dat de realiseren uitgaven en beheer wilt van het toewijzen van afzonderlijke openbare IP-adressen tot alle stateless bronnen in het raster berekenen en u kunt eenvoudig verbinding maken met deze VMs uit andere bronnen in uw VNET inclusief waarvoor openbare IP-adressen zoals netwerktaakverdelers of zelfstandige virtuele machines.

 - **Verbinding maken met behulp van regels voor NAT VMs** - u kunt maken van een openbare IP-adres, toewijzen aan een taakverdeling en binnenkomende NAT-regels die een poort op het IP-adres zijn toegewezen aan een poort op een VM in het instellen van de schaal VM definiëren. Bijvoorbeeld:
 
    Bron | Bronpoort | Bestemming | Bestemmingspoort
    --- | --- | --- | ---
    Openbare IP | Poort 50000 | vmss\_0 | Poort 22
    Openbare IP | Poort 50001 | vmss\_1 | Poort 22
    Openbare IP | Poort 50002 | vmss\_2 | Poort 22

    Hier volgt een voorbeeld van het maken van een VM schaal instellen waarin regels NAT SSH-verbinding voor elke VM in een set met een enkel openbare IP-kleurenschaal inschakelen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Hier volgt een voorbeeld van hetzelfde met RDP- en Windows doet: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **Verbinding maken met VMs met een "jumpbox"** - als u een schaal VM maken instellen en een zelfstandig product VM in de dezelfde VNET, de zelfstandige VM en de VM schaal set die VMS kunnen worden verbonden met elkaar via hun interne IP-adressen voor gedefinieerd door het VNET/Subnet. Als u een openbare IP-adres maken en aan de zelfstandige VM toewijzen kunt u RDP of SSH naar de zelfstandige VM en sluit u vervolgens op dat de computer en uw exemplaren VM schaal instellen. U ziet misschien op dit moment dat een eenvoudige VM schaal set veiliger dan een eenvoudige zelfstandige VM met een openbare IP-adres in de standaardconfiguratie is.

    [Deze sjabloon maakt voor een voorbeeld van deze methode, een eenvoudige Mesos cluster bestaande uit een zelfstandig product outmodel VM die een cluster VM schaal instellen op basis van VMs beheert.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Exemplaren taakverdeling VM schaalfactor instellen** - als u wilt werken met een berekeningscluster van VMs met een 'round robin'-methode leveren, kunt u een Azure taakverdeling met regels taakverdeling dienovereenkomstig gewijzigd. U kunt definiëren sondes om te controleren of de toepassing wordt uitgevoerd door het pingen poorten met een opgegeven protocol, interval en verzoek pad. De Azure [Toepassingsgateway](https://azure.microsoft.com/services/application-gateway/) ondersteunt ook schaal sets, samen met meer geavanceerde taakverdeling scenario's.

    [Hier volgt een voorbeeld Hiermee maakt u een set VM schaal VMs IIS-webserver, en gebruik een taakverdeling taakverdeling dat elke VM ontvangt. Ook wordt met het HTTP-protocol ping de URL van een specifieke op elke VM.](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) (kijkt u naar het type van de resource Microsoft.Network/loadBalancers en de networkProfile en extensionProfile in de virtualMachineScaleSet)

 - **Een schaal VM als een berekeningscluster mogelijk in een PaaS cluster manager instellen implementeren** - VM schaal sets soms als de rol van een medewerker van de volgende generatie worden beschreven. Is het een geldige beschrijving, maar deze ook wordt uitgevoerd de kans op pesterijen duidelijker is schaal set functies met PaaS v1 werknemer rol functies. In een zin VM schaal sets bieden een true 'werknemer rol' of werknemer resource, een resource generalized berekeningscluster, dat wil platform/runtime onafhankelijke zeggen, leveren aanpasbare en geïntegreerd met Azure resourcemanager IaaS.

    Een PaaS v1 werknemer rol-, terwijl beperkt met ondersteuning voor platform-runtime (alleen voor Windows-platform afbeeldingen) bevat ook services zoals VIP omwisselen, configureerbare upgrade-instellingen, runtime/app implementatie specifieke instellingen die niet zijn _nog_ beschikbaar in VM sets schalen of wordt geleverd door andere hogere niveau PaaS services zoals Service stof. Met dit kunt Vergeet niet dat u bekijken VM schaal sets als een infrastructuur die ondersteuning biedt voor PaaS. Dat wil zeggen PaaS oplossingen zoals Service stof of cluster managers zoals Mesos boven op VM maken kunt sets schaal als een laag scalable berekeningscluster.

    [Deze sjabloon maakt voor een voorbeeld van deze methode, een eenvoudige Mesos cluster bestaande uit een zelfstandig product outmodel VM die een cluster VM schaal instellen op basis van VMs beheert.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json) Toekomstige versies van de [Azure Container-Service](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) implementeert meer complexe/harde versies van dit scenario op basis van VM schaal sets.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>VM schaal instellen prestaties en schaal richtlijnen

- Maak niet meer dan 500 VMs in meerdere VM schaal Sets tegelijk.
- Plan voor niet meer dan 20 VMs per opslag-account (tenzij u de eigenschap _overprovision_ op 'onwaar' hebt ingesteld, die u kunt gaan maximaal 40).
- Meer witruimte tussen de eerste letters van opslag accountnamen zo veel mogelijk invoegen.  Het voorbeeld VMSS sjablonen in [Azure Quickstart sjablonen](https://github.com/Azure/azure-quickstart-templates/) vindt u voorbeelden van hoe u dit doet.
- Als aangepaste VMs gebruikt, kunt u plannen voor niet meer dan 40 VMs per VM schaal instellen in een enkel opslag-account.  Moet u de afbeelding vooraf gekopieerd naar het opslag-account voordat u VM schaal set-implementatie kunt. Zie de veelgestelde vragen voor meer informatie.
- Plan voor niet meer dan 4096 VMs per VNET.
- Het aantal VMs kunt u is door het quotum core in de regio waarin u implementeert beperkt. Mogelijk moet u contact opnemen met de klantenondersteuning om te verhogen van uw quotumlimiet berekeningscluster is groter, zelfs als u een hoge limiet van cores voor gebruik met cloudservices of IaaS v1 vandaag hebt. U kunt de volgende opdracht uit Azure CLI uitvoeren om de quota voor uw query: `azure vm list-usage`, en de volgende PowerShell-opdracht: `Get-AzureRmVMUsage` (als met een versie van PowerShell onder 1,0 Gebruik `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>VM schaal instellen Veelgestelde vragen

**Q.** Hoeveel VMs kunt u in een set van de schaal VM hebben?

**A.** 100 als u platform-afbeeldingen die kunnen worden gedistribueerd meerdere opslag-accounts gebruikt. Als u aangepaste afbeeldingen, maximaal 40 (indien de eigenschap _overprovision_ is standaard ingesteld op "false", 20), sinds zijn aangepaste afbeeldingen momenteel beperkt tot een enkel opslag-account.

**Q** welke andere resource grenzen bestaan voor VM schaal sets?

**A.** U zijn beperkt tot het maken van maximaal 500 VMs in meerdere schaal sets per regio tijdens een periode van 10 minuten. De bestaande [Azure-abonnementsservice limieten /](../azure-subscription-service-limits.md) toepassen.

**Q.** Zijn de gegevens schijven ondersteund VM schaal sets?

**A.** Niet in de eerste versie. Uw opties voor het opslaan van gegevens zijn:

- Azure-bestanden (SMB gedeeld stations)

- OS station

- TEMP station (lokaal, niet wordt ondersteund door Azure opslag)

- Azure gegevensservice (bijvoorbeeld Azure tabellen, Azure BLOB's)

- Externe gegevensservice (bijvoorbeeld externe DB)

**Q.** Welke Azure gebieden worden door VM schaal sets ondersteund?

**A.** Een gebied dat ondersteuning biedt voor Azure resourcemanager ondersteunt VM schaal Sets.

**Q.** Hoe maak ik een VM schaal instellen door een aangepaste afbeelding?

**A.** De eigenschap vhdContainers leeg laat, bijvoorbeeld:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": "https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd"
            },
            "osType": "Windows"
        }
    },


**Q.** Als ik mijn schaal VM capaciteit instellen van 20 tot 15, die VMs worden verwijderd?

**A.** Virtuele machines worden verwijderd uit de schaal gelijkmatig over de upgrade domeinen en domeinen voor foutenstructuuranalyse ingesteld beschikbaarheid maximaliseren. VMs met de hoogste id worden eerst verwijderd.

**Q.** Hoe zit het als ik de capaciteit van 15 tot en met 18 vervolgens verhogen?

**A.** Als u de capaciteit om 18, wordt klikt u vervolgens 3 nieuwe VMs gemaakt. Telkens wanneer de VM exemplaar-id wordt verhoogd uit de vorige hoogste waarde (bijvoorbeeld 20, 21, 22). VMs zijn verdeeld over FDs en UDs.

**Q.** Wanneer u meerdere extensies in een set van de schaal VM, kan ik een reeks execution afdwingen?

**A.** Niet rechtstreeks, maar voor de extensie customScript uw script kan wachten op een andere extensie om uit te voeren ([bijvoorbeeld door het controleren van de extensie log](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Aanvullende richtlijnen op extensie volgordebepaling vindt u in dit blogbericht: [Extensie volgordebepaling in Azure VM schaal Sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**Q.** Werk VM schaal sets met Azure beschikbaarheid sets?

**A.** Ja. Een set van de schaal VM is een impliciete beschikbaarheid instellen met 5 FDs en 5 UDs. U hoeft niet te configureren alles onder virtualMachineProfile. In toekomstige versies, VM schaal sets hebben waarschijnlijk meerdere tenants's beslaan maar voor nu een schaal instellen is een set één beschikbaarheid.
