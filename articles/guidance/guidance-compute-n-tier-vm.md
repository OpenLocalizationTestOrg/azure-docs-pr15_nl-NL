<properties
   pageTitle="Windows VMs uitgevoerd voor een architectuur N lagen | Overzicht van de architectuur | Microsoft Azure"
   description="Het implementeren van een architectuur met meerdere niveaus op Azure, betaling vooral op beschikbaarheid, beveiliging, schaalbaarheid en beheerbaarheid beveiliging."
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
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-windows-vms-for-an-n-tier-architecture-on-azure"></a>Windows VMs voor een architectuur N lagen waarop Azure

> [AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Linux VMs voor een architectuur N lagen waarop Azure](guidance-compute-n-tier-vm-linux.md)
- [Windows VMs voor een architectuur N lagen waarop Azure](guidance-compute-n-tier-vm.md)

In dit artikel bevat een reeks bewezen procedures voor het uitvoeren van Windows virtuele machines (VMs) voor een toepassing met een architectuur N lagen. Dit is gebaseerd op het [meerdere VMs op Azure]-artikel[multi-vm].

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. In dit artikel worden de resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties gebruikt.

## <a name="architecture-diagram"></a>Architectuurdiagram

Er zijn variaties van N lagen architecturen. Voor het grootste deel van de verschillen niet mag belang voor de toepassing van deze aanbevelingen. In dit artikel wordt ervan uitgegaan van een typisch 3 laag web-app:

- **Weblaag.** Binnenkomende HTTP-verzoeken verwerkt. Antwoorden worden geretourneerd door deze laag.

- **Zakelijke laag.** Bedrijfsprocessen implementeert en andere functionele logica voor het systeem.

- **Databaselaag.** Permanente gegevensopslag, [SQL Server altijd op beschikbaarheid van de groepen] met[ sql-alwayson] voor maximale beschikbaarheid.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. In dit diagram is op de "berekeningscluster - meerdere pagina's laag (Windows).

![[0]][0]

- **Beschikbaarheid Sets.** Maak een [Beschikbaarheid instellen] [ azure-availability-sets] voor elk trapsgewijs en ten minste twee VMs in elke laag inrichten. Deze methode is vereist voor de beschikbaarheid [SLA] hebt bereikt[ vm-sla] voor VMs.

- **Subnetten.** Maak een afzonderlijk subnet voor elke laag. Geef het bereik en subnet masker met [CIDR] -notatie. 

- **Netwerktaakverdelers.** Gebruik een [internetgerichte taakverdeling] [ load-balancer-external] distributie van binnenkomende internetverkeer naar de weblaag en een [interne taakverdeling] [ load-balancer-internal] distributie van netwerkverkeer van de weblaag naar de laag bedrijven.

- **Jumpbox**. Een _jumpbox_, ook wel een [bastionhost host], is een VM op het netwerk waarmee beheerders verbinding maken met de andere VMs. De jumpbox heeft een NSG waarmee externe verkeer alleen in het openbare IP-adressen whitelisted. De NSG moet mogelijk met het maken van extern bureaublad (RDP)-verkeer is toegestaan.

- **Cmdlets voor controle**. Prestaties van software zoals [Nagios], [Zabbix]of [Icinga] krijgt, kunt u inzicht in antwoord tijd, VM beschikbaarheid en de algemene status van uw systeem. De controle software installeren op een VM die in een afzonderlijk management subnet wordt geplaatst.

- **NSGs**. Gebruik [beveiligingsgroepen netwerk] [ nsg] (NSGs) netwerkverkeer binnen de VNet beperken. Bijvoorbeeld, in de architectuur van 3 lagen Hier wordt getoond, accepteert de databaselaag geen verkeer van de web-front-end alleen in de laag bedrijven en het subnet management.

- **SQL Server altijd op beschikbaarheid van de groep.** Beschikbaarheid in de gegevenslaag bevat doordat herhaling en overname bij storing.

- **Active Directory Domain Services (AD DS)-Servers**. Active Directory Domain Services (AD DS) directory-gegevens worden opgeslagen en beheer van de communicatie tussen gebruikers en domeinen, inclusief gebruiker aanmeldingsprocessen, verificatie en zoekacties in Active directory. Een Active Directory-domeincontroller is een server waarop AD DS wordt uitgevoerd. Vóór 2016 voor Windows Server, moet altijd op beschikbaarheidsgroepen worden toegevoegd aan een domein. Dit komt omdat de beschikbaarheid van de groepen hangt af van de technologie voor Windows Server Failover Cluster (WSFC). Windows Server 2016 maakt u kennis met de mogelijkheid om een failovercluster zonder Active Directory te maken. Zie [Wat is er nieuw in Failoverclustering in 2016 van Windows Server] voor meer informatie[wsfc-whats-new]

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die deze aanbevelingen volgt opgegeven. Als u maakt de architectuur van uw eigen verwijzing deze aanbevelingen volgt tenzij u specifieke vereisten die een aanbeveling niet past hebt.

### <a name="vnet--subnets"></a>VNet / subnetten

Wanneer u de VNet maakt, moet u onvoldoende adresruimte toewijzen voor de subnetten die u nodig hebt. Geef het VNet adres bereik en subnet masker met [CIDR] -notatie. Gebruik een adresruimte die binnen de standaard [privé IP-adresblokken]valt[private-ip-space], die zijn 10.0.0.0/8, 172.16.0.0/12 en 192.168.0.0/16.

Kies een adresbereiken die geen overlap heeft met uw netwerk on-premises geval u nodig hebt voor het instellen van een gateway tussen de VNet en uw on-premises netwerk later. Nadat u de VNet hebt gemaakt, kunt u het adresbereik niet wijzigen.

Ontwerp subnetten eisen functionaliteit en beveiliging in gedachten. Alle VMs binnen de dezelfde rol of laag moeten terechtkomen in hetzelfde subnet, die de rand van een waardepapier kan zijn. Zie voor meer informatie over het ontwerpen van VNets en subnetten, [plannen en ontwerpen Azure virtuele netwerken][plan-network].

Voor elk subnet, geeft u de adresruimte voor het subnet in CIDR-notatie. '10.0.0.0/24' maakt u bijvoorbeeld een bereik van 256 IP-adressen. (VMs 251 van deze kunnen gebruiken, vijf worden gereserveerd. Zie [Virtual Network Veelgestelde vragen over][vnet faq].) Zorg ervoor dat de adresbereiken elkaar niet overlappen via subnetten.

### <a name="network-security-groups"></a>Beveiligingsgroepen netwerk

NSG regels gebruiken voor het beperken van verkeer tussen lagen. Bijvoorbeeld, in de architectuur van 3 lagen hierboven, communiceert de weblaag niet rechtstreeks met de databaselaag. Als u dit, moet de databaselaag binnenkomende verkeer vanaf het web laag subnet blokkeren.  

  1. Maak een NSG en deze te koppelen aan de database laag subnet.

  2. Een regel die al dan niet toestaan alle binnenkomende verkeer uit de VNet toevoegen. (Gebruik de `VIRTUAL_NETWORK` label in de regel.) 

  3. Een regel met een hogere prioriteit waarmee binnenkomende verkeer van het bedrijf laag subnet toevoegen. Deze regel de vorige regel overschreven en kunt de laag business contact opnemen met de databaselaag.

  4. Een regel waarmee binnenkomende verkeer uit in de database laag subnet zelf toevoegen. Deze regel maakt communicatie tussen VMs in de databaselaag, die nodig is voor databasereplicatie en overname bij storing.

  5. Een regel waarmee RDP-verkeer uit het subnet jumpbox toevoegen. Deze regel kan beheerders verbinding maken met de databaselaag vanuit de jumpbox.

  > [AZURE.NOTE] Een NSG heeft [standaardregels] [ nsg-rules] waarmee u kunt alle binnenkomende verkeer van binnen de VNet. Deze regels kunnen niet worden verwijderd, maar u kunt deze vervangen door hogere prioriteit regels te maken.

### <a name="load-balancers"></a>Netwerktaakverdelers

De externe taakverdeling internetverkeer naar de weblaag. Maak een openbare IP-adres van de verdeling van deze belasting. Zie [een taakverdeling internetgerichte maken][lb-external-create].

De interne taakverdeling netwerkverkeer van de weblaag naar de laag bedrijven. Als u deze taakverdeling een persoonlijk IP-adres, een frontend IP-configuratie maken en koppelen aan het subnet voor de laag bedrijven. Zie [aan de slag voor het maken van een interne taakverdeling][lb-internal-create].

### <a name="sql-server-always-on-availability-groups"></a>SQL Server altijd op van beschikbaarheidsgroepen

Het is raadzaam [Altijd op beschikbaarheid-groepen] te[ sql-alwayson] beschikbaarheid van SQL Server. Altijd op beschikbaarheidsgroepen een domeincontroller nodig is. Alle knooppunten in de groep beschikbaarheid moeten zich in het hetzelfde AD-domein.

Andere lagen verbinding maken met de database via een [beschikbaarheid van de groep luisteraar ervan af][sql-alwayson-listeners]. De luisteraar ervan af kunt een SQL-client verbinding maken met de naam van de fysieke exemplaar van SQL Server niet kent. VMs dat access de database moet worden toegevoegd aan het domein. De client (in dit geval een andere laag) gebruikt DNS van de luisteraar ervan af virtuele netwerknaam omzetten in IP-adressen.

Configureer SQL Server altijd op als volgt:

- Maak een cluster met Windows Server-Failover cluster (WSFC) en een SQL Server altijd op beschikbaarheid van de groep. Zie voor meer informatie [Aan de slag met altijd op beschikbaarheidsgroepen][sql-alwayson-getting-started].

- Een interne taakverdeling maken met een statische privé IP-adres.

- Maak een luisteraar ervan af groep beschikbaarheid en de luisteraar ervan af DNS-naam toewijzen aan het IP-adres van een interne taakverdeling. 

- Maak een regel van de verdeling van belasting voor de poort voor SQL Server luisteren (TCP-poort 1433 al dan niet standaard). De regel van de verdeling van belasting moet _zwevende IP_, ook genoemd directe Server terug inschakelen. Hierdoor wordt de VM rechtstreeks naar de client, waarmee een directe verbinding met de primaire replica beantwoorden.

    > [AZURE.NOTE] Wanneer zwevend IP is ingeschakeld, moet het front poortnummer hetzelfde als het poortnummer van back-enddatabase in de regel van de verdeling van laden.

Wanneer een SQL-client probeert verbinding te maken, stuurt de taakverdeling het verzoek om verbinding naar de replica die de huidige primaire. Als er een overname naar een andere replica is, stuurt de taakverdeling automatisch volgende aanvragen naar de nieuwe primaire replica. Zie voor meer informatie [configureren de belasting voor documentconversies voor SQL altijd op][sql-alwayson-ilb].

Tijdens een een overname, zijn bestaande clientverbindingen gesloten. Nadat de overname is voltooid, worden nieuwe verbindingen doorgestuurd naar de nieuwe primaire replica.

Als uw app aanzienlijk meer leest dan schrijft, staat u enkele van de alleen-lezenquery's met een secundaire replica. Zie [een luisteraar ervan af met verbinding maken met een secundair alleen-lezen Replica (alleen-lezen omleiding)][sql-alwayson-read-only-routing].

Uw implementatie testen door af te [dwingen een handmatige failover][sql-alwayson-force-failover].

### <a name="jumpbox"></a>Jumpbox

Toegang niet is toegestaan RDP van openbare Internet naar de VMs waarop de werklast van de toepassing wordt uitgevoerd. In plaats daarvan moet alle RDP/SSH toegang tot deze VMs worden door de jumpbox. Een beheerder die zich aanmeldt bij de jumpbox en meldt zich dan naar de andere VM uit de jumpbox. De jumpbox kunt RDP-verkeer van Internet, maar alleen uit bekende, whitelisted IP-adressen.

Plaats de jumpbox in de dezelfde VNet als de andere VMs, maar in een afzonderlijk management subnet.

Een [openbare IP-adres] voor de jumpbox maken.

Gebruik een kleine VM-grootte voor het jumpbox, zoals standaard A1.

Configureer de NSGs voor het weblaag, zakelijke laag en database laag subnetten zodat administratieve (RDP) verkeer doorgegeven vanaf het subnet management.

Als u wilt beveiligen het jumpbox, een NSG maken en deze toepassen op het subnet jumpbox. Een regel voor NSG waarmee RDP-verbindingen alleen uit een set whitelisted openbare IP-adressen toevoegen.

De NSG kan worden gekoppeld aan het subnet of aan de jumpbox NIC In dit geval is het raadzaam deze koppelen aan de NIC, zodat RDP-verkeer is toegestaan alleen voor de jumpbox, zelfs als u andere VMs aan hetzelfde subnet toevoegen.

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

Elke laag of VM rol in een set afzonderlijke beschikbaarheid plaatsen. Niet plaats VMs van verschillende niveaus in dezelfde beschikbaarheid set. 

In de databaselaag wordt met meerdere VMs niet automatisch vertalen in een maximaal beschikbare database. Voor een relationele database moet u meestal herhaling en overname bij storing gebruiken om te bereiken van beschikbaarheid. Voor SQL Server, raden u [Altijd op beschikbaarheid doelgroepen][sql-alwayson]. 

Als u nodig voor de beschikbaarheid van hoger dan de [Azure SLA voor VMs hebt] [ vm-sla] biedt, de toepassing repliceren tussen de twee regio's en Azure verkeer Manager gebruiken voor failover. Zie voor meer informatie, [Windows VMs uitgevoerd in meerdere regio's voor maximale beschikbaarheid][multi-dc].   

## <a name="security-considerations"></a>Veiligheidsoverwegingen

Gegevens in rust versleutelen. Gebruik van [Azure toets kluis] [ azure-key-vault] voor het beheren van de database versleuteling toetsen. Toets kluis kunt versleuteling toetsen opslaan in hardware beveiligingsmodules (HSM's). Zie voor meer informatie, [Azure toets kluis integratie configureren voor SQL Server Azure VMs] [ sql-keyvault] het is ook raadzaam geheimen van toepassing, zoals tekenreeksen voor databaseverbinding, opgeslagen in sleutel kluis.

Houd rekening met het toevoegen van een netwerk virtuele toestel (NVA) een DMZ tussen openbare Internet en het Azure virtuele netwerk maken. NVA is een algemene term voor een virtuele toestel die netwerk-gerelateerde taken zoals firewall, pakket controle, controle, aangepaste routering of allerlei andere bewerkingen kunt uitvoeren. Zie voor meer informatie, [een DMZ tussen Azure en Internet implementeren][dmz].

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

De netwerktaakverdelers distribueren netwerkverkeer naar de web- en -lagen. Horizontaal met het toevoegen van nieuwe VM exemplaren verkleinen. Houd er rekening mee dat u de web- en -lagen belooft op basis van laden schalen kunt. Als u wilt verkleinen mogelijke problemen die worden veroorzaakt door de nodig voor het behoud van clientaffiniteit, moet de VMs in de weblaag stateless. De VMs hostingprovider de bedrijfslogica moet ook stateless.

## <a name="manageability-considerations"></a>Aandachtspunten voor de beheerbaarheid

Beheer van het gehele systeem vereenvoudigen met behulp van gecentraliseerde beheerprogramma's zoals [Azure automatisering][azure-administration], [Microsoft bewerkingen Management Suite][operations-management-suite], [Chef][chef], of [Puppet][puppet]. Diagnostic en systeemstatus informatie vastgelegd uit meerdere VMs op te geven van een algemeen overzicht van het systeem kunnen worden geconsolideerd in deze hulpmiddelen.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een implementatie van een verwijzing architectuur dat deze aanbevelingen implementeert is beschikbaar op [Github][github-folder]. De architectuur van deze verwijzing bevat een weblaag, bedrijven laag, een gegevenslaag, evenals een jumpbox VM en Active Directory-domeincontroller.

De architectuur verwijzing kan worden geïmplementeerd aan de hand van de volgende instructies uit: 

1. Klik op de knop hieronder.  
[! ["Implementeren naar Azure"] [1]][2]

2. Nadat de koppeling in de portal van Azure is geopend, voert u de waarden volgen: 
  - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-ntier-sql-network-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
  - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
  - Klik op de knop **aanschaffen** .

3. Controleer Azure portal melding voor een bericht de implementatie is voltooid.

4. Klik op de knop hieronder.  
[! ["Implementeren naar Azure"] [1]][3]

5. Nadat de koppeling in de portal van Azure is geopend, voert u de waarden volgen: 
  - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus selecteer **Bestaande gebruiken** en voer `ra-ntier-sql-workload-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
  - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
  - Klik op de knop **aanschaffen** .

6. Controleer Azure portal melding voor een bericht de implementatie is voltooid.

7. Klik op de knop hieronder.  
[! ["Implementeren naar Azure"] [1]][4]

8. Nadat de koppeling in de portal van Azure is geopend, voert u de waarden volgen: 
  - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-ntier-sql-network-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
  - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
  - Klik op de knop **aanschaffen** .

9. Controleer Azure portal melding voor een bericht de implementatie is voltooid.

10. Deze parameterbestanden bevatten een vastgelegde beheerder gebruikersnamen en wachtwoorden en het is raadzaam dat u direct wijzigen beide op alle VMs. Klik op elke VM in de Portal Azure Klik op **wachtwoord opnieuw** in het blad **ondersteuning + probleemoplossing** . Selecteer **wachtwoord opnieuw** in het vak van de vervolgkeuzelijst **modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om te voorkomen van de nieuwe gebruikersnaam en wachtwoord. 

Zie voor informatie over andere manieren om te implementeren van deze verwijzing architectuur, het Leesmij-bestand in de [richtlijnen-één-vm] [ github-folder] Github map. 

## <a name="next-steps"></a>Volgende stappen

Als u wilt bereiken beschikbaarheid voor deze verwijzing architectuur, het is raadzaam te [implementeren naar meerdere gebieden][multi-dc].

<!-- links -->

[arm-templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[azure-administration]: ../automation/automation-intro.md
[azure-audit-logs]: ../resource-group-audit.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-key-vault]: https://azure.microsoft.com/services/key-vault.md
[azure-load-balancer]: ../load-balancer/load-balancer-overview.md
[bastionhost host]: https://en.wikipedia.org/wiki/Bastion_host
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier-sql
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters.md
[multi-vm]: guidance-compute-multi-vm.md
[n-tier]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[openbare IP-adres]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[sql-alwayson]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[sql-alwayson-force-failover]: https://msdn.microsoft.com/en-us/library/ff877957.aspx
[sql-alwayson-getting-started]: https://msdn.microsoft.com/en-us/library/gg509118.aspx
[sql-alwayson-ilb]: https://blogs.msdn.microsoft.com/igorpag/2016/01/25/configure-an-ilb-listener-for-sql-server-alwayson-availability-groups-in-azure-arm/
[sql-alwayson-listeners]: https://msdn.microsoft.com/en-us/library/hh213417.aspx
[sql-alwayson-read-only-routing]: https://technet.microsoft.com/en-us/library/hh213417.aspx#ConnectToSecondary
[sql-keyvault]: ../virtual-machines/virtual-machines-windows-ps-sql-keyvault.md
[vm-planned-maintenance]: ../virtual-machines/virtual-machines-windows-planned-maintenance.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[wsfc-whats-new]: https://technet.microsoft.com/windows-server-docs/failover-clustering/whats-new-in-failover-clustering
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/deploy-reference-architecture.sh
[vnet-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/virtualNetwork.parameters.json
[vnet-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/networkSecurityGroups.parameters.json
[nsg-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/webTier.parameters.json
[webtier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/webTier.parameters.json
[businesstier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/businessTier.parameters.json
[businesstier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/businessTier.parameters.json
[datatier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/dataTier.parameters.json
[datatier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/dataTier.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/compute-n-tier.png "Met Microsoft Azure architectuur met N lagen"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2FvirtualNetwork.azuredeploy.json
[3]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fworkload.azuredeploy.json
[4]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fsecurity.azuredeploy.json