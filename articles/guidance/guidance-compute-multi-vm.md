<properties
   pageTitle="Meerdere VMs uitgevoerd | Overzicht van de architectuur | Microsoft Azure"
   description="Het uitvoeren van meerdere VM exemplaren op Azure voor schaalbaarheid, tolerantie beheerbaarheid en beveiliging."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>Meerdere VMs op Azure uitgevoerd voor schaalbaarheid en beschikbaarheid 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel vindt u een overzicht van een reeks bewezen procedures voor het uitvoeren van meerdere exemplaren van virtuele machines (VM) om te verbeteren schaalbaarheid, beschikbaarheid, beheer en beveiliging.   

In deze architectuur, is de werklast verdeeld over de VM exemplaren. Er is een enkel openbare IP-adres en internetverkeer wordt verdeeld over het gebruik van een taakverdeling VMs. Deze architectuur kan worden gebruikt voor een niet-gelaagde-app, zoals een stateless web app of opslag cluster. Het is ook een bouwsteen voor N-aantal lagen-toepassingen. 

In dit artikel is gebaseerd op [een enkele VM op Azure uitgevoerd][single vm]. De aanbevelingen in dit artikel wordt ook toegepast op deze architectuur.

## <a name="architecture-diagram"></a>Architectuurdiagram

VMs in Azure vereisen ondersteunende netwerk- en opslag resources.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. In dit diagram is op de "berekeningscluster - tabblad voor multi-VM." 

![[0]][0]

De architectuur heeft de volgende onderdelen: 

- **Beschikbaarheid instellen.** De [beschikbaarheid instellen] [ availability set] bevat de VMs en is noodzakelijk is ter ondersteuning van de [beschikbaarheid van SLA voor Azure VMs][vm-sla].

- **VNet**. Elke VM in Azure is geïmplementeerd in een virtueel netwerk (VNet) die wordt onderverdeeld in **subnetten**.

- **Azure taakverdeling.** De [belasting voor documentconversies] distribueert verzoeken voor oproepen Internet in de VM-exemplaren in een set beschikbaarheid. De taakverdeling bevat enkele verwante resources:

  - **Openbare IP-adres.** Een openbare IP-adres is vereist voor de verdeling van de belasting voor het ontvangen van internetverkeer.

  - **Front configuratie.** Het openbare IP-adres worden gekoppeld aan de taakverdeling.

  - **Groep met back-enddatabase adressen.** Bevat de netwerkinterfaces (NIC's) voor de VMs die de binnenkomende verkeer ontvangt.

- **Laad de verdeling van regels.** Wordt gebruikt voor het netwerkverkeer wordt verdeeld over alle VMs in de groep adressen van de back-enddatabase distribueren. 

- **NAT regels.** Wordt gebruikt voor route-verkeer is toegestaan voor een specifieke VM. Bijvoorbeeld, om in te schakelen remote desktop protocol (RDP) naar de VMs, een afzonderlijk netwerk adres NAT (Translation) regel maken voor elke VM. 

- **Netwerk-interfaces (NIC's)**. Elke VM heeft een NIC verbinding maken met het netwerk.

- **Opslagruimte.** Opslag accounts houdt de VM afbeeldingen en andere resources bestand-gerelateerde, zoals VM diagnostische gegevens vastgelegd door Azure.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die volgt op de onderstaande aanbevelingen opgegeven. Als u wilt maken van de architectuur van uw eigen verwijzing moet u deze aanbevelingen volgt, tenzij u specifieke vereisten die een aanbeveling geen ondersteunt hebt. 

### <a name="availability-set-recommendations"></a>Beschikbaarheid instellen aanbevelingen

U moet ten minste twee VMs maken in de beschikbaarheid instellen voor de ondersteuning van de [beschikbaarheid van SLA voor Azure VMs][vm-sla]. Houd er rekening mee dat de verdeling van de Azure belasting ook moet VMs verdeeld tot dezelfde beschikbaarheid set behoren.

### <a name="network-recommendations"></a>Aanbevelingen voor netwerken

Het VMs achter de taakverdeling moeten alle worden teruggezet in hetzelfde subnet. Worden niet de VMs rechtstreeks met Internet, maar elke VM in plaats daarvan een persoonlijk IP-adres te geven. Clients verbinding maken via het openbare IP-adres van de taakverdeling.

### <a name="load-balancer-recommendations"></a>Aanbevelingen voor de verdeling van laden

Alle VMs in de beschikbaarheid ingesteld op de groep back-enddatabase adressen van de taakverdeling toevoegen.

Laden de verdeling van regels voor directe netwerkverkeer naar de VMs definiëren. Bijvoorbeeld: Schakel HTTP-verkeer door een regel maken die poort 80 tussen de front-configuratie en poort 80 op de groep back-enddatabase adressen kaarten. Wanneer de taakverdeling een aanvraag op poort 80 van het openbare IP-adres ontvangt, wordt deze het verzoek doorsturen naar poort 80 op een van de NIC's in de groep back-enddatabase adressen.

NAT afrondingsregels naar route-verkeer is toegestaan voor een specifieke VM. Bijvoorbeeld, om in te schakelen RDP naar de VMs een afzonderlijke NAT regel maken voor elke VM. Elke regel moet een unieke poortnummer toewijzen aan poort 3389, de standaardpoort voor RDP. (Bijvoorbeeld gebruiken poort 50001 voor "VM1," poort 50002 voor 'VM2', enzovoort.) De regels NAT toewijzen aan de netwerkadapters op de VMs. 

### <a name="storage-account-recommendations"></a>Aanbevelingen voor opslag-account

Afzonderlijke Azure opslag-accounts maken voor elke VM hebben de virtuele vaste schijven, om te voorkomen kunt u door de invoer/uitvoer-bewerkingen per tweede [(IO's / s) limieten] voor[ vm-disk-limits] voor opslag-accounts. 

Maak één opslag-account voor diagnostische logboeken. Dit account opslag kan worden gedeeld door alle VMs.

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

Er zijn twee opties voor het schalen VMs in Azure wordt aangegeven: 

- Gebruik een taakverdeling netwerkverkeer over een reeks VMs verdelen. Als u verkleinen, inrichten van extra VMs en plaatst u deze achter de taakverdeling. 

- [VM schaal Sets]gebruiken[vmss]. Een set schaal bevat een opgegeven aantal identieke VMs achter een taakverdeling. VM schaal Hiermee stelt u ondersteuning autoscaling op basis van de prestatiegegevens. Als de belasting op het VMs toeneemt, wordt extra VMs worden automatisch toegevoegd aan de taakverdeling. 

De volgende secties Vergelijk deze twee opties.

### <a name="load-balancer-without-vm-scale-sets"></a>De belasting voor documentconversies zonder VM schaal sets

Een taakverdeling duurt verzoeken netwerk voor oproepen en verdeeld over de NIC's in de groep back-enddatabase adressen. Als u horizontaal verkleinen, meer VM exemplaren toevoegen met de set beschikbaarheid (of toewijzing VMs verkleinen). 

Stel dat u gebruikt een webserver. U voegt een regel van de verdeling van belasting voor poort 80 en/of poort 443 (voor SSL) toe. Wanneer een client een HTTP-aanvraag stuurt, de taakverdeling een back-enddatabase IP-adres met een [hashing algoritme] uitgelicht[ load balancer hashing] die het bron-IP-adres bevat. Op die manier zijn aanvragen van clients verdeeld over alle VMs. 

> [AZURE.TIP] Wanneer u een nieuwe VM toevoegt aan een beschikbaarheid instellen, moet u een NIC maken voor de VM en de NIC toevoegen aan de groep back-enddatabase adressen op de taakverdeling. Internetverkeer won't anders worden doorgestuurd naar de nieuwe VM.

Standaardlimieten heeft elke Azure abonnement op hun plaats staan, waaronder een maximum aantal VMs per regio. U kunt de limiet vergroten door het opbergen van een verzoek voor ondersteuning. Zie [Azure-abonnement en limieten van de service, quota's en beperkingen]voor meer informatie,[subscription-limits].  

### <a name="vm-scale-sets"></a>VM schaal sets 

VM schaal sets helpen u te implementeren en beheren van een reeks identieke VMs. Met alle VMs geconfigureerd hetzelfde, VM schaal sets ondersteuning waar automatisch schalen, zonder vooraf inrichting VMs, wordt eenvoudiger grootschalige services doelgroepen groot berekeningscluster, big data en beperkte werkbelasting maken. 

Zie [VM schaal Sets overzicht]voor meer informatie over VM schaal sets,[vmss].

Overwegingen bij het gebruik van VM schaal sets:

- Houd rekening met schaal sets als u wilt snel af VMs schaal of moet automatisch schalen. 

- Op dit moment ondersteund schaal sets niet door gegevensschijven. De opties voor het opslaan van gegevens zijn Azure bestandsopslag, het station OS, het Temp-station of een externe store, zoals Azure opslag. 

- VM overal binnen een schaal automatisch ingesteld deel uitmaakt van dezelfde beschikbaarheid set, met 5 foutenstructuuranalyse domeinen en 5 update-domeinen.

- Standaard gebruik schaal sets "overprovisioning", hetgeen betekent dat de set schaal in eerste instantie meer VMs dan u vraagt bepalingen en vervolgens Hiermee verwijdert u de extra VMs. Dit verbetert de algehele kans dat bij de inrichting van de VMs. 

- En vervolgens wordt aangeraden niet meer dan 20 VMs per opslag account met ingeschakeld of niet meer dan 40 VMs overprovisioning met overprovisioning uitgeschakeld.  

- U kunt resourcemanager sjablonen kunt vinden voor de schaal Hiermee stelt u in de [Azure Quickstart sjablonen][vmss-quickstart].

- Er zijn twee eenvoudige manieren voor het configureren van VMs geïmplementeerd in een set schaal: een aangepaste afbeelding maken of uitbreidingen gebruiken voor het configureren van de VM nadat deze is ingericht.

    - Een set schaal is gebaseerd op een aangepaste afbeelding moet alle OS VHD's van schijf binnen één opslag-account maken. 

    - Met aangepaste afbeeldingen moet u de afbeelding up-to-date te houden.

    - Met extensies, kan het langer duren voor een nieuw ingerichte VM draaien omhoog.

Zie voor aanvullende overwegingen [Ontwerpen VM schaal Sets voor schaal][vmss-design].

> [AZURE.TIP]  Wanneer u een oplossing automatisch schalen, test u deze ruim met productieniveau werkbelastingen. 

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

De beschikbaarheid van de Set zorgt ervoor dat uw app beter bestand zowel geplande als niet-gepland onderhoud gebeurtenissen.

- _Gepland onderhoud_ treedt op wanneer Microsoft bijgewerkt het onderliggende platform soms veroorzaakt door VMs opnieuw worden gestart. Azure maakt ervoor dat de VMs in een set beschikbaarheid zijn niet alle opnieuw worden gestart op hetzelfde moment, ten minste één wordt bewaard uitgevoerd terwijl de anderen opnieuw bent opgestart.

- _Niet-gepland onderhoud_ gebeurt er als er een hardwarefout. Azure zorgt ervoor dat VMs in een set beschikbaarheid in meer dan één server rek is ingericht. Hierdoor vermindert de impact van problemen met de hardware, bijvoorbeeld netwerk, power onderbrekingen, enzovoort.

Zie voor meer informatie [beheren de beschikbaarheid van virtuele machines][availability set]. De volgende video bevat ook een handig overzicht van beschikbaarheid sets: [Hoe kan ik configureren dat een beschikbaarheid-ingesteld op schaal VMs][availability set ch9]. 

> [AZURE.WARNING]  Zorg ervoor dat de beschikbaarheid instellen wanneer u de VM inrichten configureren. Er is momenteel geen manier om een VM resourcemanager toevoegen aan een beschikbaarheid instellen nadat de VM is ingericht.

De taakverdeling gebruikt [controleert of systeemstatus] om te controleren van de beschikbaarheid van VM exemplaren. Als een test niet kan van een exemplaar binnen een time-out bereiken, stopt de taakverdeling verkeer naar die VM verzendt. Echter de taakverdeling blijft zoeken en als de VM weer beschikbaar is, wordt de taakverdeling hervat verkeer naar die VM verzendt.

Hier volgen enkele aanbevelingen op laden verdeling systeemstatus sondes:

- Sondes kunnen HTTP of TCP testen. Als uw VMs uitvoeren een HTTP-server (IIS, nginx Node.js app en dergelijke), maakt een HTTP-test. Anders maken een TCP-test.

- Geef het pad naar het eindpunt HTTP voor een HTTP-test. De test Hiermee wordt gecontroleerd op een reactie HTTP 200 van dit pad. Dit is het pad naar de hoofdmap ("/") of een statuscontrole eindpunt dat sommige aangepaste logica als u wilt controleren van de status van de toepassing implementeert. Het eindpunt moet anonieme HTTP-aanvragen toestaan.

- De test is verzonden vanuit een [bekende] [ health-probe-ip] IP-adres, 168.63.129.16. Zorg ervoor dat u geen verkeer naar of uit deze IP-in een firewall beleidsregels of netwerk groep (NSG) beveiligingsregels blokkeren.

- Gebruik van de [Servicestatus test logboeken] [ health probe log] de status van de servicestatus testpakketten weergeven. Logboekregistratie inschakelen in de Azure portal voor elke taakverdeling. Logboeken worden geschreven met Azure-blobopslag. De logboeken hoeveel VMs weergeven op de back-end ontvangen geen netwerkverkeer vanwege mislukte test antwoorden.

## <a name="manageability-considerations"></a>Aandachtspunten voor de beheerbaarheid

Met meerdere VMs wordt het belangrijk om te automatiseren van processen, zodat ze betrouwbaar en herhaald. U kunt [Azure automatisering] [ azure-automation] implementatie, OS patches en andere taken automatiseren. Azure automatisering is een Automatiseringsservice die wordt uitgevoerd op Azure en is gebaseerd op de Windows PowerShell. Voorbeeld automatische scripts zijn beschikbaar in de [Galerie met Runbook] op TechNet.

## <a name="security-considerations"></a>Veiligheidsoverwegingen

Virtuele netwerken zijn verkeer moeten worden geïsoleerd rand in Azure wordt aangegeven. VMs in één VNet kunnen niet rechtstreeks naar VMs in een andere VNet communiceren. VMs binnen dezelfde VNet kunnen communiceren, tenzij u [beveiligingsgroepen netwerk] maken[ nsg] (NSGs) verkeer beperken. Zie voor meer informatie, [Microsoft-cloudservices en netwerkbeveiliging][network-security].

Voor binnenkomende internetverkeer definiëren de regels van de verdeling van laden welke verkeer de back-end kunt bereiken. Laden de verdeling van regels ondersteunen echter niet IP-whitelisting, dus als u een NSG wilt "witte" lijst bepaalde openbare IP-adressen, toevoegen aan het subnet.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een implementatie van een verwijzing architectuur dat deze aanbevelingen implementeert is beschikbaar op GitHub. De architectuur van deze verwijzing bevat een virtueel netwerk (VNet), netwerk-beveiligingsgroep (NSG), taakverdeling en twee virtuele machines (VMs).

De architectuur verwijzing kan worden geïmplementeerd met Windows- of Linux VMs aan de hand van de volgende instructies uit: 

1. Met de rechtermuisknop op de knop hieronder en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster':  
[![Implementeren naar Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. Nadat de koppeling is geopend in de portal van Azure, moet u waarden opgeven voor een deel van de instellingen: 
    - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus selecteer **Bestaande gebruiken** en voer `ra-multi-vm-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
    - Selecteer het **Type besturingssysteem** uit de vervolgkeuzelijst vak, **windows** of **linux**. 
    - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
    - Klik op de knop **aanschaffen** .

3. Wacht totdat de implementatie om te voltooien.

4. De parameterbestanden bevatten een beheerder vastgelegde-gebruikersnaam en wachtwoord en het wordt ten zeerste aanbevolen dat u direct beide wijzigen. Klik op de VM met de naam `ra-multi-vm1` in de portal van Azure. Klik vervolgens op **wachtwoord opnieuw** in het blad **ondersteuning + probleemoplossing** . Selecteer **wachtwoord opnieuw** in het vak van de vervolgkeuzelijst **modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om op te slaan van de nieuwe gebruikersnaam en wachtwoord. Herhaal dit voor de benoemde VM `ra-multi-vm2`.

Zie voor informatie over andere manieren om te implementeren van deze verwijzing architectuur, het Leesmij-bestand in de [richtlijnen-één-vm] [ github-folder] GitHub map. 

## <a name="next-steps"></a>Volgende stappen

Verschillende VMs achter een taakverdeling plaatsen is een bouwsteen voor het maken van meerlagige architecturen. Zie voor meer informatie, [Windows VMs uitgevoerd voor een architectuur N lagen op Azure] [ n-tier-windows] en [Linux VMs uitgevoerd voor een architectuur N lagen op Azure][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[servicestatus sondes]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[de belasting voor documentconversies]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Galerie met Runbook]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "Architectuur van een oplossing multi-VM op Azure die bestaat uit een set met twee VMs en een taakverdeling beschikbaarheid"
