<properties
   pageTitle="Linux VMs voor een architectuur N lagen waarop Azure | Microsoft Azure"
   description="Het uitvoeren van Linux VMs voor een architectuur N lagen in Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
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

# <a name="running-linux-vms-for-an-n-tier-architecture-on-azure"></a>Linux VMs voor een architectuur N lagen waarop Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]


> [AZURE.SELECTOR]
- [Linux VMs voor een architectuur N lagen waarop Azure](guidance-compute-n-tier-vm-linux.md)
- [Windows VMs voor een architectuur N lagen waarop Azure](guidance-compute-n-tier-vm.md)

In dit artikel bevat een reeks bewezen procedures voor het uitvoeren van Linux virtuele machines (VMs) voor een toepassing met een architectuur N lagen. Dit is gebaseerd op het [meerdere VMs op Azure]-artikel[multi-vm].

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. In dit artikel worden de resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties gebruikt.

## <a name="architecture-diagram"></a>Architectuurdiagram

Er zijn variaties van N lagen architecturen. Voor het grootste deel van de verschillen niet mag belang voor de toepassing van deze aanbevelingen. In dit artikel wordt ervan uitgegaan van een typisch 3 laag web-app:

- **Weblaag.** Binnenkomende HTTP-verzoeken verwerkt. Antwoorden worden geretourneerd door deze laag.

- **Zakelijke laag.** Bedrijfsprocessen implementeert en andere functionele logica voor het systeem.

- **Databaselaag.** Permanente gegevensopslag, met behulp van Apache Cassandra voor beschikbaarheid bevat.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. In dit diagram is op de "berekeningscluster - meerdere pagina's laag (Linux).

![[0]][0]

- **Beschikbaarheid Sets.** Maak een [Beschikbaarheid instellen] [ azure-availability-sets] voor elk trapsgewijs en ten minste twee VMs in elke laag inrichten. Deze methode is vereist voor de beschikbaarheid [SLA] hebt bereikt[ vm-sla] voor VMs.

- **Subnetten.** Maak een afzonderlijk subnet voor elke laag. Geef het bereik en subnet masker met [CIDR] -notatie. 

- **Netwerktaakverdelers.** Gebruik een [internetgerichte taakverdeling] [ load-balancer-external] distributie van binnenkomende internetverkeer naar de weblaag en een [interne taakverdeling] [ load-balancer-internal] distributie van netwerkverkeer van de weblaag naar de laag bedrijven.

- **Jumpbox**. Een _jumpbox_, ook wel een [bastionhost host], is een VM op het netwerk waarmee beheerders verbinding maken met de andere VMs. De jumpbox heeft een NSG waarmee externe verkeer alleen in het openbare IP-adressen whitelisted. De NSG moet mogelijk met het maken van secure shell (SSH)-verkeer is toegestaan.

- **Cmdlets voor controle**. Prestaties van software zoals [Nagios], [Zabbix]of [Icinga] krijgt, kunt u inzicht in antwoord tijd, VM beschikbaarheid en de algemene status van uw systeem. De controle software installeren op een VM die in een afzonderlijk management subnet wordt geplaatst.

- **NSGs**. Gebruik [beveiligingsgroepen netwerk] [ nsg] (NSGs) netwerkverkeer binnen de VNet beperken. Bijvoorbeeld, in de architectuur van 3 lagen Hier wordt getoond, accepteert de databaselaag geen verkeer van de web-front-end alleen in de laag bedrijven en het subnet management.

- **Apache Cassandra database**. Beschikbaarheid in de gegevenslaag bevat doordat herhaling en overname bij storing.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die deze aanbevelingen volgt opgegeven. Als u maakt de architectuur van uw eigen verwijzing deze aanbevelingen volgt tenzij u specifieke vereisten die een aanbeveling niet past hebt.

### <a name="vnet--subnets"></a>VNet / subnetten

Wanneer u de VNet maakt, moet u onvoldoende adresruimte toewijzen voor de subnetten die u nodig hebt. Geef het VNet adres bereik en subnet masker met [CIDR] -notatie. Gebruik een adresruimte die binnen de standaard [privé IP-adresblokken]valt[private-ip-space], die zijn 10.0.0.0/8, 172.16.0.0/12 en 192.168.0.0/16.

Kies een adresbereiken die geen overlap heeft met uw netwerk on-premises geval u nodig hebt voor het instellen van een gateway tussen de VNet en uw on-premises netwerk later. Nadat u de VNet hebt gemaakt, kunt u het adresbereik niet wijzigen.

Ontwerp subnetten eisen functionaliteit en beveiliging in gedachten. Alle VMs binnen de dezelfde rol of laag moeten terechtkomen in hetzelfde subnet, die de rand van een waardepapier kan zijn. Zie voor meer informatie over het ontwerpen van VNets en subnetten, [plannen en ontwerpen Azure virtuele netwerken][plan-network].

Voor elk subnet, geeft u de adresruimte voor het subnet in CIDR-notatie. '10.0.0.0/24' maakt u bijvoorbeeld een bereik van 256 IP-adressen. (VMs 251 van deze kunnen gebruiken, vijf worden gereserveerd. Zie [Virtual Network Veelgestelde vragen over][vnet faq].) Zorg ervoor dat de adresbereiken elkaar niet overlappen via subnetten.

### <a name="load-balancers"></a>Netwerktaakverdelers

De externe taakverdeling internetverkeer naar de weblaag. Maak een openbare IP-adres van de verdeling van deze belasting. Zie [een taakverdeling internetgerichte maken][lb-external-create].

De interne taakverdeling netwerkverkeer van de weblaag naar de laag bedrijven. Als u deze taakverdeling een persoonlijk IP-adres, een frontend IP-configuratie maken en koppelen aan het subnet voor de laag bedrijven. Zie [aan de slag voor het maken van een interne taakverdeling][lb-internal-create].

### <a name="cassandra"></a>Cassandra

We raden [DataStax Enterprise] [ datastax] voor productie gebruiken, maar deze aanbevelingen toepassen op alle Cassandra edition. Zie voor meer informatie over het uitvoeren van DataStax in Azure wordt aangegeven, [DataStax Enterprise-Implementatiehandleiding voor Azure][cassandra-in-azure]. 

Zet de VMs voor een Cassandra cluster in een set beschikbaarheid, om ervoor te zorgen dat de Cassandra replica's zijn verdeeld over meerdere foutenstructuuranalyse domeinen en upgraden van domeinen. Zie voor meer informatie over foutenstructuuranalyse domeinen en de upgrade domeinen [beheren de beschikbaarheid van virtuele machines][availability-sets-manage]. 

3 foutenstructuuranalyse domeinen (het maximale aantal) per beschikbaarheid instellen configureren. 

Tabblad 18 upgrade domeinen per beschikbaarheid instellen in Office. Hiermee kunt u het maximum aantal upgrade domeinen dan u kunt nog steeds gelijkmatig verdeeld over de domeinen van de fout.   

Knooppunten configureren in de modus voor rek-functionaliteit. Foutenstructuuranalyse domeinen toewijzen aan rekken in de `cassandra-rackdc.properties` bestand.

U kunt een taakverdeling vóór het cluster niet nodig hebt. De client verbinding maakt rechtstreeks naar een knooppunt in het cluster.

### <a name="jumpbox"></a>Jumpbox

Plaats de jumpbox in de dezelfde VNet als de andere VMs, maar in een afzonderlijk management subnet.

Een [openbare IP-adres] voor de jumpbox maken.

Gebruik een kleine VM-grootte voor de jumpbox, zoals standaard A1.

Configureer de NSGs voor het weblaag, zakelijke laag en database laag subnetten zodat administratieve (SSH) verkeer doorgegeven vanaf het subnet management.

Als u wilt beveiligen het jumpbox, een NSG maken en deze toepassen op het subnet jumpbox. Een regel voor NSG waarmee SSH verbindingen alleen uit een set whitelisted openbare IP-adressen toevoegen.

De NSG kan worden gekoppeld aan het subnet of aan de jumpbox NIC In dit geval is het raadzaam deze koppelen aan de NIC, zodat SSH-verkeer is toegestaan alleen voor de jumpbox, zelfs als u andere VMs aan hetzelfde subnet toevoegen.

Toegang niet is toegestaan SSH van openbare Internet naar de VMs waarop de werklast van de toepassing wordt uitgevoerd. In plaats daarvan moet alle SSH toegang tot deze VMs worden door de jumpbox. Een beheerder die zich aanmeldt bij de jumpbox en meldt zich dan naar de andere VM uit de jumpbox. De jumpbox staat SSH verkeer van Internet, maar alleen uit bekende, whitelisted IP-adressen.

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

Elke laag of VM rol in een set afzonderlijke beschikbaarheid plaatsen. Niet plaats VMs van verschillende niveaus in dezelfde beschikbaarheid set. 

In de databaselaag wordt met meerdere VMs niet automatisch vertalen in een maximaal beschikbare database. Voor een relationele database moet u meestal herhaling en overname bij storing gebruiken om te bereiken van beschikbaarheid.  

Als u nodig voor de beschikbaarheid van hoger dan de [Azure SLA voor VMs hebt] [ vm-sla] biedt, de toepassing repliceren tussen de twee regio's en Azure verkeer Manager gebruiken voor failover. Zie voor meer informatie, [Linux VMs uitgevoerd in meerdere regio's voor maximale beschikbaarheid][multi-dc].  

## <a name="security-considerations"></a>Veiligheidsoverwegingen

NSG regels gebruiken voor het beperken van verkeer tussen lagen. Bijvoorbeeld, in de architectuur van 3 lagen hierboven, communiceert de weblaag niet rechtstreeks met de databaselaag. Als u dit, moet de databaselaag binnenkomende verkeer vanaf het web laag subnet blokkeren.  

  1. Maak een NSG en deze te koppelen aan de database laag subnet.

  2. Een regel die al dan niet toestaan alle binnenkomende verkeer uit de VNet toevoegen. (Gebruik het `VIRTUAL_NETWORK` label in de regel.) 

  3. Een regel met een hogere prioriteit waarmee binnenkomende verkeer van het bedrijf laag subnet toevoegen. Deze regel de vorige regel overschreven en kunt de laag business contact opnemen met de databaselaag.

  4. Een regel waarmee binnenkomende verkeer uit in de database laag subnet zelf toevoegen. Deze regel maakt communicatie tussen VMs in de databaselaag, die nodig is voor databasereplicatie en overname bij storing.

  5. Een regel waarmee SSH verkeer uit het subnet jumpbox toevoegen. Deze regel kan beheerders verbinding maken met de databaselaag vanuit de jumpbox.

  > [AZURE.NOTE] Een NSG heeft [standaardregels] [ nsg-rules] waarmee u kunt alle binnenkomende verkeer van binnen het VNet. Deze regels kunnen niet worden verwijderd, maar u kunt deze vervangen door hogere prioriteit regels te maken.

Overweeg een netwerk virtuele toestel (NVA) een DMZ tussen openbare Internet en het Azure virtuele netwerk maken. NVA is een algemene term voor een virtuele toestel die netwerk-gerelateerde taken zoals firewall, pakket controle, controle, aangepaste routering of allerlei andere bewerkingen kunt uitvoeren. Zie voor meer informatie, [een DMZ tussen Azure en Internet implementeren][dmz].

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

De netwerktaakverdelers distribueren netwerkverkeer naar de web- en -lagen. Horizontaal met het toevoegen van nieuwe VM exemplaren verkleinen. Houd er rekening mee dat u de web- en -lagen belooft op basis van laden schalen kunt. Als u wilt verkleinen mogelijke problemen die worden veroorzaakt door de nodig voor het behoud van clientaffiniteit, moet de VMs in de weblaag stateless. De VMs hostingprovider de bedrijfslogica moet ook stateless.

## <a name="manageability-considerations"></a>Aandachtspunten voor de beheerbaarheid

Beheer van het gehele systeem vereenvoudigen met behulp van gecentraliseerde beheerprogramma's zoals [Azure automatisering][azure-administration], [Microsoft bewerkingen Management Suite][operations-management-suite], [Chef][chef], of [Puppet][puppet]. Diagnostic en systeemstatus informatie vastgelegd uit meerdere VMs op te geven van een algemeen overzicht van het systeem kunnen worden geconsolideerd in deze hulpmiddelen.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een implementatie van een verwijzing architectuur dat deze aanbevelingen implementeert is beschikbaar op [Github][github-folder]. Deze verwijzing architectuur bevat een weblaag, zakelijke laag en een gegevenslaag.

1. Klik op de knop hieronder.  
[! ["Implementeren naar Azure"] [1]][2]

2. Nadat de koppeling in de portal van Azure is geopend, voert u de waarden volgen: 
  - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-ntier-sql-network-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
  - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
  - Klik op de knop **aanschaffen** .

3. Controleer Azure portal melding voor een bericht de implementatie is voltooid.

4. Deze parameterbestanden bevatten een vastgelegde beheerder gebruikersnamen en wachtwoorden en het is raadzaam dat u direct wijzigen beide op alle VMs. Klik op elke VM in de Portal Azure Klik op **wachtwoord opnieuw** in het blad **ondersteuning + probleemoplossing** . Selecteer **wachtwoord opnieuw** in het vak van de vervolgkeuzelijst **modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om te voorkomen van de nieuwe gebruikersnaam en wachtwoord.

## <a name="next-steps"></a>Volgende stappen

Als u wilt bereiken beschikbaarheid voor deze verwijzing architectuur, het is raadzaam te [implementeren naar meerdere gebieden][multi-dc].

<!-- links -->

[azure-administration]: ../automation/automation-intro.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[availability-sets-manage]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[bastionhost host]: https://en.wikipedia.org/wiki/Bastion_host
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-consistency-usage]: http://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-in-azure]: https://docs.datastax.com/en/datastax_enterprise/4.5/datastax_enterprise/install/installAzure.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[datastax]: http://www.datastax.com/products/datastax-enterprise
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters-linux.md
[multi-vm]: guidance-compute-multi-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[openbare IP-adres]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[0]: ./media/blueprints/compute-n-tier-linux.png "Met Microsoft Azure architectuur met N lagen"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier%2Fazuredeploy.json


