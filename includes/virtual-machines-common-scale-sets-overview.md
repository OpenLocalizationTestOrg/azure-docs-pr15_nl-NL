

Voor toepassingen moet die schaal berekeningscluster resources uit- en schaal bewerkingen impliciet zijn verdeeld over domeinen foutenstructuuranalyse en bijwerken. Een inleiding tot VM schaal Raadpleeg sets voor de recente [Azure blog aankondiging](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/).

Bekijk deze video's voor meer informatie over VM schaal sets:

 - [Mark Russinovich moment spreekt Azure schaal Sets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [VM schaal Sets met man Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Hiermee stelt u maken en beheren van VM schaal

VM schaal sets kunnen worden gedefinieerd en geïmplementeerd met behulp van JSON sjablonen en [REST API's](https://msdn.microsoft.com/library/mt589023.aspx) alleen zoals afzonderlijke Azure resourcemanager VMs. Daarom kunnen standaard Azure resourcemanager implementatiemethoden worden gebruikt. Zie voor meer informatie over sjablonen, [Azure resourcemanager Authoring sjablonen](../articles/resource-group-authoring-templates.md).

Een reeks voorbeeld sjablonen voor VM schaal sets vindt u in de Azure Quickstart sjablonen GitHub opslagplaats hier:

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) - uiterlijk voor sjablonen met _vmss_ in de titel.

In de projectdetailpagina's voor deze sjablonen ziet u een knop die naar de portal-implementatie-functie verwijst. Het instellen van de schaal VM implementeren, klik op de knop en vul alle parameters die in de portal vereist zijn. Als u niet precies weet of een resource ondersteunt hoofdletters of gemengde hoofdletters/kleine letters, het is veiliger kleine letters parameterwaarden altijd wilt gebruiken. Er is ook een handige video sectie van een VM schaal instellen sjabloon hier:

[VM schaal sjabloon sectie instellen](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Uit- en schaalbaarheid van de schaal van een VM instellen

Als u wilt vergroten of verkleinen van het aantal virtuele machines in een VM schaal instellen, wijzigen van de eigenschap _capaciteit_ en implementeer deze opnieuw de sjabloon. Deze eenvoudig kunt u gemakkelijk te schrijven van uw eigen aangepaste schaal laag als u wilt definiëren van een aangepaste schaal gebeurtenissen die niet worden ondersteund door Azure automatisch schalen.

Als u opnieuw van een sjabloon als u wilt wijzigen van de capaciteit distribueren, kunt u veel kleinere sjabloon die u alleen de SKU en de bijgewerkte capaciteit bevat definiëren. Hier ziet u een voorbeeld hiervan: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json).

Als u wilt Doorloop de stappen die een schaal instellen die automatisch wordt aangepast maakt, raadpleegt u [Automatisch schaal computers in een virtuele Machine schaal instellen](../articles/virtual-machines/virtual-machines-windows-vmss-powershell-creating.md)

## <a name="monitoring-your-vm-scale-set"></a>Cmdlets voor controle VM schaal instellen

Het verdient momenteel dat u de [Azure Resource Explorer](https://resources.azure.com) gebruiken om weer te geven VM schaal sets. VM schaal sets zijn een resource onder Microsoft.Compute, zodat u vanaf deze site kunt u ze zien door uit te vouwen van de volgende koppelingen:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>VM schaal instellen scenario 's

In deze sectie bevat enkele typische VM set scenario's voor schaal. Sommige hogere niveau Azure services (zoals batches, Service stof en Azure Container-Service) wordt gebruikt in deze scenario's.

 - **RDP / SSH VM schaalfactor instellen exemplaren** - A VM schaal instellen wordt gemaakt in een VNET en het openbare IP-adressen niet aantal afzonderlijke VMs in zijn toegewezen. Dit is een goede ding omdat u doorgaans niet dat de realiseren uitgaven en beheer wilt van het toewijzen van afzonderlijke IP-adressen tot alle stateless bronnen in het raster berekenen en u kunt eenvoudig verbinding maken met deze VMs uit andere bronnen in uw VNET inclusief waarvoor openbare IP-adressen zoals netwerktaakverdelers of zelfstandige virtuele machines.

 - **Verbinding maken met behulp van regels voor NAT VMs** - u kunt maken van een openbare IP-adres, toewijzen aan een taakverdeling en binnenkomende NAT-regels die een poort op het IP-adres zijn toegewezen aan een poort op een VM in het instellen van de schaal VM definiëren. Bijvoorbeeld

    Openbare IP-poort 50000 > -> vmss\_0 -> poort 22 openbare IP - > poort 50001 -> vmss\_1 -> poort 22 openbare IP - > poort 50002 -> vmss\_2 -> poort 22

    Hier volgt een voorbeeld van het maken van een VM schaal instellen waarin regels NAT SSH-verbinding voor elke VM in een set met een enkel openbare IP-kleurenschaal inschakelen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Hier volgt een voorbeeld van hetzelfde met RDP- en Windows doet: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **Verbinding maken met VMs met een "jumpbox"** - als u een schaal VM maken instellen en een zelfstandig product VM in de dezelfde VNET, de zelfstandige VM en de VM schaal set die VMS kunnen worden verbonden met elkaar via hun interne IP-adressen voor gedefinieerd door het VNET/Subnet. Als u een openbare IP-adres maken en aan de zelfstandige VM toewijzen kunt u RDP of SSH naar de zelfstandige VM en sluit u vervolgens op dat de computer en uw exemplaren VM schaal instellen. U ziet misschien op dit moment dat een eenvoudige VM schaal set veiliger dan een eenvoudige zelfstandige VM met een openbare IP-adres in de standaardconfiguratie is.

    Voor een voorbeeld van deze methode, deze sjabloon maakt u een eenvoudige Mesos cluster bestaande uit een zelfstandig product outmodel VM die worden beheerd door een cluster VM schaal instellen op basis van VMs: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Round robin taakverdeling VM schaalfactor instellen exemplaren** - als u wilt werken met een berekeningscluster van VMs met een 'round robin'-methode leveren, kunt u een Azure taakverdeling configureren met taakverdeling regels dienovereenkomstig gewijzigd. U kunt ook sondes om te controleren of de toepassing wordt uitgevoerd door het pingen definiëren poorten met een opgegeven protocol, interval en verzoek pad.

    Hier volgt een voorbeeld Hiermee maakt u een set VM schaal VMs IIS-webserver, en gebruik een taakverdeling taakverdeling dat elke VM ontvangt. Ook wordt het HTTP-protocol gebruikt om te ping de URL van een specifieke op elke VM: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) - Kennismaking met het type van de resource Microsoft.Network/loadBalancers en de networkProfile en extensionProfile in de virtualMachineScaleSet.

 - **Een schaal VM als een berekeningscluster mogelijk in een PaaS cluster manager instellen implementeren** - VM schaal sets soms als de rol van een medewerker van de volgende generatie worden beschreven. Is het een geldige beschrijving, maar deze ook wordt uitgevoerd de kans op pesterijen duidelijker is schaal set functies met PaaS v1 werknemer rol functies. In een zin VM schaal sets bieden een true 'werknemer rol' of werknemer resource, een resource generalized berekeningscluster, dat wil platform/runtime onafhankelijke zeggen, leveren aanpasbare en geïntegreerd met Azure resourcemanager IaaS.

    Een PaaS v1 werknemer rol-, terwijl beperkt met ondersteuning voor platform-runtime (alleen voor Windows-platform afbeeldingen) bevat ook services zoals VIP omwisselen, configureerbare upgrade-instellingen, runtime/app implementatie specifieke instellingen die niet zijn _nog_ beschikbaar in VM sets schalen of wordt geleverd door andere hogere niveau PaaS services zoals Service stof. Met dit kunt Vergeet niet dat u bekijken VM schaal sets als een infrastructuur die ondersteuning biedt voor PaaS. Dat wil zeggen PaaS oplossingen zoals Service stof of cluster managers zoals Mesos boven op VM maken kunt sets schaal als een laag scalable berekeningscluster.

    Hier volgt een voorbeeld van een Mesos cluster waarin een VM schaal instellen in de dezelfde VNET als een zelfstandig product VM implementeert. De zelfstandige VM is een outmodel Mesos en het instellen van de schaal VM vertegenwoordigt een reeks slave knooppunten: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json). Toekomstige versies van de [Azure Container-Service](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) implementeert meer complexe/harde versies van dit scenario op basis van VM schaal sets.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>VM schaal instellen prestaties en schaal richtlijnen

- In de openbare voorbeeldweergave, maak niet meer dan 500 VMs in meerdere VM schaal Sets tegelijk.
- Plan voor niet meer dan 40 VMs per opslag-account.
- Meer witruimte tussen de eerste letters van opslag accountnamen zo veel mogelijk invoegen.  Het voorbeeld VMSS sjablonen in [Azure Quickstart sjablonen](https://github.com/Azure/azure-quickstart-templates/) vindt u voorbeelden van hoe u dit doet.
- Als aangepaste VMs gebruikt, kunt u plannen voor niet meer dan 40 VMs per VM schaal instellen in een enkel opslag-account.  Moet u de afbeelding vooraf gekopieerd naar het opslag-account voordat u VM schaal set-implementatie kunt. Zie de veelgestelde vragen voor meer informatie.
- Plan voor niet meer dan 2048 VMs per VNET.  Deze limiet is in de toekomst verhoogd.
- Het aantal VMs kunt u wordt beperkt door de quota voor uw berekeningscluster core in elke regio. Mogelijk moet u contact opnemen met de klantenondersteuning om te verhogen van uw quotumlimiet berekeningscluster is groter, zelfs als u een hoge limiet van cores voor gebruik met cloudservices of IaaS v1 vandaag hebt. U kunt de volgende opdracht uit Azure CLI uitvoeren om de quota voor uw query: _azure vm lijst-gebruik_en de volgende PowerShell-opdracht: _Get-AzureRmVMUsage_ (indien met een versie van PowerShell onder 1.0 _Get-AzureVMUsage_).

## <a name="vm-scale-set-frequently-asked-questions"></a>VM schaal instellen Veelgestelde vragen

**Q.** Hoeveel VMs kunt u in een set van de schaal VM hebben?

**A.** 100 als u platform-afbeeldingen die kunnen worden gedistribueerd meerdere opslag-accounts gebruikt. Als u aangepaste afbeeldingen, maximaal 40, aangezien aangepaste afbeeldingen beperkt tot een account één opslagruimte in voorbeeldweergave zijn.

**Q** welke andere resource grenzen bestaan voor VM schaal sets?

**A.** U zijn beperkt tot het maken van maximaal 500 VMs in meerdere schaal sets per regio gedurende de periode preview. De bestaande [Azure-abonnementsservice limieten /](../articles/azure-subscription-service-limits.md) toepassen.

**Q.** Zijn de gegevens schijven ondersteund VM schaal sets?

**A.** Niet in de eerste versie. Uw opties voor het opslaan van gegevens zijn:

- Azure-bestanden (SMB gedeeld stations)

- OS station

- TEMP station (lokaal, niet wordt ondersteund door Azure opslag)

- Externe gegevens service (bijvoorbeeld externe DB, Azure tabellen, Azure BLOB's)

**Q.** Welke Azure regio's ondersteuning voor VM schaal sets?

**A.** Een gebied dat ondersteuning biedt voor Azure resourcemanager ondersteunt VM schaal Sets.

**Q.** Hoe maakt u een schaal VM sets met een aangepaste afbeelding?

**A.** De eigenschap vhdContainers leeg laat, bijvoorbeeld:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**Q.** Als ik mijn schaal VM capaciteit instellen van 20 tot 15, die VMs worden verwijderd?

**A.** Virtuele machines worden verwijderd uit de schaal gelijkmatig over de upgrade domeinen en domeinen voor foutenstructuuranalyse ingesteld beschikbaarheid maximaliseren.

**Q.** Hoe zit het als ik de capaciteit van 15 tot en met 18 vervolgens verhogen?

**A.** Als u de 18, VMs met index 15, 16, wordt 17 gemaakt. In beide gevallen zijn de VMs gelijkmatig over FDs en UDs.

**Q.** Wanneer u meerdere extensies in een set van de schaal VM, kan ik een reeks execution afdwingen?

**A.** Niet rechtstreeks, maar voor de extensie customScript uw script kan wachten op een andere extensie om uit te voeren (bijvoorbeeld door het controleren van de extensie Meld - Zie [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install\_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)).

**Q.** Werk VM schaal sets met Azure beschikbaarheid sets?

**A.** Ja. Een set van de schaal VM is een impliciete beschikbaarheid instellen met FDs 3 en 5 UDs. U hoeft niet te configureren alles onder virtualMachineProfile. In toekomstige versies, VM schaal sets hebben waarschijnlijk meerdere tenants's beslaan maar voor nu een schaal instellen is een set één beschikbaarheid.
