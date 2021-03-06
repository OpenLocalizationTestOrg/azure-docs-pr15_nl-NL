<properties
   pageTitle="Linux VMs uitgevoerd in meerdere gebieden voor beschikbaarheid | Overzicht van de architectuur | Microsoft Azure"
   description="Het implementeren van VMs in meerdere gebieden op Azure voor beschikbaarheid en tolerantie."
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
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-in-multiple-regions-for-high-availability"></a>Linux VMs uitgevoerd in meerdere gebieden voor beschikbaarheid

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Linux VMs uitgevoerd in meerdere gebieden voor beschikbaarheid](guidance-compute-multiple-datacenters-linux.md)
- [Met het Windows VMs in meerdere gebieden voor beschikbaarheid](guidance-compute-multiple-datacenters.md)

In dit artikel wordt aangeraden een reeks procedures Linux virtuele machines (VMs) uitvoeren in meerdere Azure regio's om beschikbaarheid en de infrastructuur van een robuuste noodgevallen herstel te bereiken.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource groups] en klassieke. In dit artikel worden de resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties gebruikt.

Betere beschikbaarheid dan implementeren naar een enkel gebied kan worden verstrekt door een architectuur meerdere landen/regio. Als een regionale storing van invloed is op de primaire regio, kunt u [Verkeer Manager] [ traffic-manager] mislukt via de secundaire regio. Deze architectuur kunt ook als een afzonderlijke subsysteem van de toepassing mislukt.

Er zijn verschillende algemene benaderingen bij het bereiken van beschikbaarheid in datacenters:   
  
- Actieve/passieve met warm stand-by. Verkeer gaat naar één regio, terwijl de andere wacht stand-by. VMs in de tweede regio zijn toegewezen en actieve allen tijde.

- Actieve/passieve koudwatersystemen vereist. De dezelfde, maar VMs in de tweede regio zijn niet toegewezen totdat u nodig hebt voor failover. Deze methode kosten kleiner om uit te voeren, maar worden in het algemeen meer omlaag tijd tijdens een fout.

- Actief/actief. Beide regio's actief zijn en gelijkmatig verdeeld ertussen. Als één datacenter niet beschikbaar is, wordt deze draaiing uitgeschakeld.

Deze architectuur is gericht op actieve/passieve met warm stand-by, wordt verkeer Manager gebruiken voor failover. Houd er rekening mee dat kunt u ook een klein aantal VMs voor warm stand-by implementeren en vervolgens af te schalen naar wens.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram dat is gebaseerd op de architectuur wordt weergegeven in [de betrouwbaarheid toevoegen aan een architectuur N lagen op Azure](guidance-compute-n-tier-vm-linux.md). 

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. In dit diagram is op de "berekeningscluster - pagina voor multi-regio (Linux).

![[0]][0]

- **Primaire en secundaire regio's**. Deze architectuur gebruikt twee regio's om te rekenen op grotere beschikbaarheid. Een is de primaire regio. Tijdens normale bewerkingen wordt netwerkverkeer doorgestuurd naar de primaire regio. Maar als die niet beschikbaar is, wordt verkeer wordt doorgestuurd naar de tweede regio.

- ** [Azure verkeer Manager] [ traffic-manager] ** stuurt verzoeken voor oproepen naar de primaire regio. Als dat gebied niet beschikbaar is, wordt wordt verkeer Manager overgenomen door de tweede regio. Zie het gedeelte [Verkeer Manager configureren](#configuring-traffic-manager)voor meer informatie.

- **Resource-groepen**. Afzonderlijke [resourcegroepen] maken[ resource groups] voor de primaire regio, de tweede regio, en voor verkeer Manager. Dit hebt u de flexibiliteit voor het beheren van elke regio als een eenmalige verzameling resources. U kunt één regio, bijvoorbeeld kan implementeren zonder dat u omlaag de andere waarde. [Koppeling van de resourcegroepen][resource-group-links], zodat u kunt een query om de bronnen voor de toepassing weer uitvoeren.

- **VNets**. Maak een afzonderlijke VNet voor elke regio. Zorg ervoor dat de adres-spaties elkaar niet overlappen.

- **Apache Cassandra** geïmplementeerd in gegevens centers Azure regio's. Cassandra datacenters zijn geïmplementeerd in verschillende Azure regio's voor maximale beschikbaarheid. In de regio's knooppunten zijn geconfigureerd in hoogte rek modus met foutenstructuuranalyse en domeinen voor tolerantie binnen het gebied van de upgrade.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die deze aanbevelingen volgt opgegeven. Als u maakt de architectuur van uw eigen verwijzing deze aanbevelingen volgt tenzij u specifieke vereisten die een aanbeveling niet past hebt.

### <a name="regional-pairing"></a>Regionale koppelen

Elke Azure regio is gekoppeld aan een ander gebied in de dezelfde Geografie. Kies in het algemeen, regio's in dezelfde regionale twee (bijvoorbeeld Oost Amerikaans 2 en ons centraal). Voordelen van het doet zijn:

- Als er een globaal storing, wordt herstel van ten minste één regio afmelden bij elk paar prioriteit.

- Geplande Azure systeemupdates worden geïmplementeerd op gepaarde regio's opeenvolging, als u wilt mogelijk uitvaltijd.

- Paren zich bevinden binnen de dezelfde Geografie, om te voldoen aan de hand van vestigingsplaats gegevens.

Echter ervoor zorgen dat beide regio's ondersteuning bieden voor alle van de Azure services die u nodig hebt voor uw toepassing (Zie [Services per regio][services-by-region]). Zie voor meer informatie over regionale paren, [bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR): Azure gepaarde t-toets regio's][regional-pairs].

### <a name="traffic-manager-configuration"></a>Configuratie van verkeer beheer

Houd rekening met de volgende punten tijdens het configureren van verkeer manager voor uw scenario:

- **Routering.** Verkeer Manager ondersteunt verschillende [routeren algoritmen][tm-routing]. Voor de scenario wordt beschreven in dit artikel, gebruikt u _prioriteit_ routering (voorheen _failover_ routering). Met deze instelling stuurt verkeer Manager alle aanvragen naar de primaire regio, tenzij er het primaire regio niet bereikbaar wordt. Op dat moment wordt deze automatisch overgenomen door de tweede regio. Zie [Failover configureren routeren methode][tm-configure-failover].

- **Status-test.** Verkeer Manager gebruikt een HTTP (of HTTPS) [test] [ tm-monitoring] om de beschikbaarheid van elke regio de te houden. De test Hiermee wordt gecontroleerd op een HTTP 200 antwoord voor een opgegeven URL-pad. Een aanbevolen om een eindpunt dat de algemene status van de toepassing-rapporten maken en dit eindpunt gebruiken voor de systeemstatus-test. De test mogelijk anders een 'goed' eindpunt rapporteren wanneer kritieke onderdelen van de toepassing daadwerkelijk worden verbroken. Zie voor meer informatie, [Systeemstatus eindpunt Monitoring patroon][health-endpoint-monitoring-pattern].

Wanneer het verkeer Manager wordt overgenomen, is er een periode wanneer de toepassing is en dat kan enkele minuten zijn door clients kunnen niet worden bereikt. Twee factoren van invloed op de totale duur:

- De status-test moet worden gedetecteerd dat de primaire Datacenter niet bereikbaar is geworden.

- DNS-servers moeten de in de cache DNS-records voor het IP-adres, dat afhankelijk van de DNS-time to live (TTL is) bijwerken. De standaard-TTL is 300 seconden (5 minuten), maar u kunt deze waarde configureren bij het maken van het profiel verkeer Manager.

Zie voor meer informatie [Over het verkeer Manager Monitoring][tm-monitoring].

Als het verkeer Manager wordt overgenomen, raden uitvoering van een handmatige failback, in plaats van de automatisch verbroken terug. Controleer of dat alle toepassing subsystemen eerst in orde zijn. Anders kunt u een situatie waar de toepassing gespiegeld heen en weer tussen datacenters maken.

Standaard mislukt verkeer Manager automatisch terug. U kunt dit voorkomen, verlaagt u handmatig de prioriteit van de primaire regio na een gebeurtenis failover. Stel de primaire regio is prioriteit 1 en de secundaire prioriteit 2 is. Na een failover, het primaire gebied in te stellen op prioriteit 3, om te voorkomen dat automatische failback. Wanneer u klaar om terug te schakelen bent, moet u de prioriteit bijwerken naar 1.

De volgende opdracht uit Azure CLI updates de prioriteit:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

Een andere manier om te voorkomen flip-flop is tijdelijk uitschakelen van het eindpunt:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```    

Afhankelijk van de oorzaak van een failover, u moet mogelijk redploy de resources in een gebied. Uitvoeren voordat u terug verbroken, een operationele gereedheid-toets. De test moet controleren of items, zoals:

- VMs zijn correct geconfigureerd. (Alle benodigde software is geïnstalleerd, IIS is uitgevoerd, enz.)

- Toepassing subsystemen in orde zijn.

- Functionele testen. (Bijvoorbeeld de databaselaag is bereikbaar vanuit de weblaag.)

### <a name="cassandra-deployment-across-multiple-regions"></a>Cassandra implementatie tussen meerdere regio 's

Cassandra datacenters zijn onderverdelingen van werkbelasting: een groep verwante knooppunten die binnen een cluster voor replicatie en werkbelasting scheiding samen zijn geconfigureerd.

Het is raadzaam te [DataStax Enterprise] [ datastax] voor productie gebruiken. Zie voor meer informatie over het uitvoeren van DataStax in Azure wordt aangegeven, [DataStax Enterprise-Implementatiehandleiding voor Azure][cassandra-in-azure]. De volgende algemene aanbevelingen zijn van toepassing op alle Cassandra-editie.

- Een openbare IP-adres toewijzen aan elk knooppunt. Hiermee worden de clusters communiceren via regio's met de infrastructuur van Azure backbone, hoge doorvoer tegen een lage prijs leveren.

- Knooppunten met de juiste firewall uit en NSG configuraties, het verkeer alleen naar en vanuit bekende hosts, inclusief clients en andere knooppunten beveiligen. Houd er rekening mee dat Cassandra andere poorten voor communicatie, OpsCenter elektrische en dergelijke gebruikt. Zie [configureren firewall poort toegang]voor het poortgebruik van de in Cassandra,[cassandra-ports].

- SSL-versleuteling voor alle [client tot knooppunt] [ ssl-client-node] en [naar knooppunten] [ ssl-node-node] communicatie.

- In een gebied, volgt u de richtlijnen in [Cassandra aanbevelingen](guidance-compute-n-tier-vm-linux.md#cassandra-recommendations).

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

Met een complexe N lagen-app moet u mogelijk niet de hele toepassing in de tweede regio repliceren. U mogelijk alleen in plaats daarvan een kritieke subsysteem die nodig is voor de ondersteuning van bedrijfscontinuïteit repliceren.

Verkeer Manager is een mogelijke fout in het systeem. Als de service is mislukt, clients, geen toegang tot uw toepassing tijdens de downtime. Controleer de [Verkeer Manager SLA][tm-sla], en bepaalt of met verkeer Manager alleen voldoet aan uw vereisten voor bedrijven voor maximale beschikbaarheid. Als dat niet zo is, kunt u een ander verkeer beheeroplossing toevoegen als een failback. Als de Azure verkeer Manager-service niet, wijzigt u de CNAME-records in DNS te laten verwijzen naar de andere verkeer management-service. (Deze stap handmatig moet worden uitgevoerd en uw toepassing nog niet beschikbaar totdat de DNS-wijzigingen worden doorgegeven.)

Voor het cluster Cassandra, is de failover volgende voorbeeldscenario's zijn afhankelijk van de consistentie niveaus die worden gebruikt door de toepassing, alsmede het aantal replica's gebruikt. Zie [consistentie van de gegevens configureren] voor consistentie niveaus en gebruik in Cassandra[ cassandra-consistency] en [Cassandra: het aantal knooppunten zijn gesproken naar met Quorum?][cassandra-consistency-usage] De beschikbaarheid van de gegevens in Cassandra wordt bepaald door de consistentie niveau die worden gebruikt door de toepassing en het herhaling. Zie [Gegevensreplicatie in NoSQL Databases uitleg]voor replicatie in Cassandra,[cassandra-replication].

## <a name="manageability-considerations"></a>Aandachtspunten voor de beheerbaarheid

Wanneer u uw implementatie bijwerkt, moet u één regio bijwerken per keer, om te voorkomen dat een algemene fout uit een onjuiste configuratie of een fout in de toepassing.

Test de tolerantie van het systeem op fouten. Hier volgen enkele veelvoorkomende scenario's van de fout uit om te testen:

- VM exemplaren afgesloten.

- Druk resources zoals processor en geheugen.

- De verbinding verbreken/vertraging netwerk.

- Loopt vast processen.

- Certificaten zijn verlopen.

- Simuleren hardwarefouten.

- De DNS-service op de domeincontrollers afgesloten.

Meet de tijden herstel en controleer of dat ze overeenkomen met uw vereisten voor bedrijven. Combinaties van mislukt modi, ook testen.

## <a name="next-steps"></a>Volgende stappen

Deze reeks is gericht op puur cloud-implementaties. Scenario's voor Enterprise vereisen vaak een hybride-netwerk, een on-premises netwerk verbinden met een Azure virtuele netwerk. Zie informatie over het maken van dergelijke een hybride-netwerk, [een hybride netwerkarchitectuur met Azure en On-premises VPN implementeren][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[cassandra-in-azure]: https://academy.datastax.com/resources/deployment-guide-azure
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[cassandra-consistency-usage]: https://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-ports]: http://docs.datastax.com/en/latest-dse/datastax_enterprise/sec/secConfFirePort.html
[datastax]: https://www.datastax.com/products/datastax-enterprise
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssl-client-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLClientToNode_t.html
[ssl-node-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLNodeToNode_t.html
[tablediff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc-linux.png "Maximaal beschikbare netwerkarchitectuur voor toepassingen Azure N-lagen"
