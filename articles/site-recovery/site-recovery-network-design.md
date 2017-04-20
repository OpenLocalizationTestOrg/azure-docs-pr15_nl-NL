<properties
    pageTitle="Ontwerpen van de infrastructuur van uw netwerk voor herstel | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe netwerk ontwerpoverwegingen voor de Site herstel van Azure"
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="pratshar"/>

#  <a name="designing-your-network-infrastructure-for-disaster-recovery"></a>De infrastructuur van uw netwerk voor herstel ontwerpen

In dit artikel wordt doorgestuurd naar IT-professionals die verantwoordelijk zijn voor uitbreidt, implementeren, en ondersteunende bedrijfscontinuïteit en noodgevallen herstel (BCDR)-infrastructuur en willen gebruikmaken van Microsoft Azure Site herstel (ASR) naar ondersteuning en hun BCDR-services te verbeteren. Dit artikel wordt beschreven hoe praktische aandachtspunten voor de implementatie van System Center VM Manager-server, de voor- en nadelen van uitgerekt subnetten versus subnet failover en structuur van herstel aan virtuele websites in Microsoft Azure.

## <a name="overview"></a>Overzicht

[Azure Site herstel (ASR)](https://azure.microsoft.com/services/site-recovery/) is een Microsoft Azure-service die orchestrates de beveiliging en herstel van uw gevirtualiseerde toepassingen voor bedrijven bedrijfscontinuïteit belangrijke bestanden (BCDR) verloren. In dit document is bedoeld om u te begeleiden de lezer door het proces van het ontwerpen van de netwerken, concentreren op het IP-bereiken en subnetten op de site noodgevallen herstel uitbreidt wanneer repliceren virtuele machines (VMs) met sites worden hersteld.

Bovendien in dit artikel wordt beschreven hoe herstel van de Site kunnen aanpassen en implementeren van een meerdere locaties virtuele datacenter ter ondersteuning van BCDR bij test of noodgevallen.

In de wereld waar iedereen 24 x 7 connectivity verwacht, is het meer belangrijk dan ooit vindt uw infrastructuur en toepassingen omhoog en wordt uitgevoerd. Het doel van bedrijfscontinuïteit en noodgevallen herstel (BCDR) is mislukte onderdelen terug te zetten, zodat de organisatie kunt normale bewerkingen snel weer. Strategieën voor noodgevallen te handelen niet waarschijnlijk, vernietigende gebeurtenissen ontwikkelen is zeer moeilijk. Dit is vanwege de inherent problemen tijdens de toekomst, voorspellen vooral met betrekking tot onwaarschijnlijke gebeurtenissen en de hoge kosten op te geven, passende maatregelen bescherming tegen groot catastrophes. 

Essentieel voor BCDR planning, herstel tijd doelstelling (RTO) en vrijgegeven herstel punt doelstelling (Productieorder) moeten worden gedefinieerd als onderdeel van een gaan. Wanneer een noodgevallen van de klant Datacenter, dat met Azure sites worden hersteld, klanten kunnen snel (lage RTO biedt) weer online hun gerepliceerde virtuele machines zich bevindt in de secundaire Datacenter of een Microsoft Azure met minimale gegevensverlies (lage vrijgegeven Productieorder). 

Failover is mogelijk gemaakt door ASR in eerste instantie aangewezen virtuele machines vanuit het beheercentrum primaire gegevens naar de secundaire Datacenter of gekopieerd Azure (afhankelijk van het scenario), en vervolgens de replica's regelmatig vernieuwt. Tijdens de infrastructuur planning moet netwerkontwerp worden beschouwd mogelijke knelpunt die verhinderen u van vergadering bedrijf RTO en vrijgegeven Productieorder doelstellingen dat kan.  

Als beheerders een noodgevallen herstel-oplossing implementeert wilt, is een van de belangrijkste vragen in een hoe de virtuele machine zou bereikbaar is nadat de overname is voltooid. ASR kan de beheerder het netwerk waaraan een virtuele machine na failover zou niet worden verbonden met kiezen. Als de primaire-site wordt beheerd door een server VMM wordt vervolgens bereikt met netwerk toewijzen. Raadpleeg [voorbereiden voor het netwerk toewijzen](site-recovery-network-mapping.md) voor meer informatie.

Tijdens het ontwerpen van het netwerk voor de site herstel, heeft de beheerder twee keuzes:

- Gebruik een andere IP-adresbereiken op het netwerk bij herstel site. In dit scenario krijgt de virtuele machine na failover een nieuwe IP-adres en de beheerder moet een DNS-update doen. Lees meer informatie over hoe u de DNS-records bijwerken [hier](site-recovery-vmm-to-vmm.md#test-your-deployment) 
- Gebruik dezelfde IP-adresbereiken voor het netwerk op de site herstellen. Beheerders liever behouden de IP-adressen die zich na de overname op de primaire-site nodig hebben in bepaalde scenario's. Een beheerder moet de routes om aan te geven van de nieuwe locatie van de IP-adressen bijwerken in een normale scenario. Maar in het scenario waar een uitgerekt VLAN tussen de primaire en de sites herstel wordt geïmplementeerd, behoudt de IP-adressen voor de virtuele machines verandert in een aantrekkelijke optie. Ervoor dat het hetzelfde IP adressen eenvoudiger herstellen door te ondernemen afwezig een netwerk gerelateerd post-failover stappen.


Als beheerders een noodgevallen herstel-oplossing implementeert wilt, is een van de belangrijkste vragen hun rekening hoe de toepassingen bereikbaar, worden nadat de overname is voltooid. Moderne toepassingen zijn bijna altijd afhankelijk netwerken tot op zekere hoogte, dus daadwerkelijk verplaatsen van dat een service van de ene site naar de andere vertegenwoordigt een netwerken uitdaging. Er zijn twee manieren die dit probleem is behandeld in noodgevallen hersteloplossingen. De eerste methode is voor het behoud van vaste IP-adressen. Ondanks de services verplaatsen en de hosting-servers in verschillende fysieke locaties, duren toepassingen voordat de IP-adres-configuratie met hen de nieuwe locatie. De tweede methode heeft betrekking op volledig wijzigen van het IP-adres bij de overgang in de herstelde site. Elke methode heeft verschillende implementatie variaties die hieronder worden samengevat.

Tijdens het ontwerpen van het netwerk voor de site herstel, heeft de beheerder twee keuzes:

## <a name="option-1-retain-ip-addresses"></a>Optie 1: IP-adressen behouden 

Noodgevallen herstel proces vanuit het perspectief van, met vaste IP-adressen is niet de eenvoudigste methode willen implementeren, maar er een aantal mogelijke uitdagingen die in de praktijk kunt u de minste populairste aanpak. Azure Site herstel biedt de mogelijkheid om te bewaren van het IP-adressen in alle scenario's. Voordat een besluit moeten worden bewaard IP, moet de juiste denken worden besteed aan de beperkingen die deze op de mogelijkheden failover u in rekening brengt. Laat ons kijkt u naar de factoren die u moet worden bewaard IP-adressen, al dan niet zelf kunnen helpen. Dit kan op twee manieren worden bereikt, met behulp van een uitgerekt subnet of als u een volledige subnet failover uitvoert.

### <a name="stretched-subnet"></a>Uitgerekt subnet

Hier is het subnet dat beschikbaar is tegelijk in zowel primaire als DR locaties. Kort gezegd betekent dit dat u kunt een server en de IP-(Layer 3) configuratie verplaatsen naar de tweede site en het netwerk wordt het verkeer routeren naar de nieuwe locatie automatisch. Dit vrij eenvoudig te handelen server vanuit het perspectief van is maar heeft een aantal uitdagingen:

- Laag 2 (gegevens koppeling layer) vanuit het perspectief van, vereist netwerkapparatuur die een uitgerekt VLAN kunt beheren, maar dit meer of minder een probleem als ze nu algemeen beschikbaar is geworden. Het tweede en moeilijker probleem is dat uit elkaar te bewegen het VLAN het mogelijke foutenstructuuranalyse domein wordt uitgebreid tot beide sites, dat u in feite een potentieel risico. Dit is waarschijnlijk niet voorkomt, heeft dit is er gebeurd dat een broadcast-storm gestart, maar niet geïsoleerd worden kan. We gemengde meningen over dit probleem laatste hebt gevonden en veel geslaagde implementaties alsmede "we implementeert nooit hier deze technologie" hebt gezien.
- Uitgerekt subnet is niet mogelijk als u Microsoft Azure als de DR-site gebruikt.


### <a name="subnet-failover"></a>Subnet failover

Het is mogelijk willen implementeren subnet failover voor de voordelen van de uitgerekt subnet-oplossing zonder uit elkaar te bewegen het subnet op meerdere sites hierboven is beschreven. Hier zou een bepaald subnet aanwezig zijn op Site-1 of 2, maar niet op beide sites tegelijk. Als u wilt behouden de IP-adresruimte bij failover, is het mogelijk te via een programma voor de infrastructuur router naar de subnetten van een site verplaatsen naar een andere schikken. Beveiligde VMs in een scenario voor failover die de subnetten met het bijbehorende wilt verplaatsen. Het belangrijkste nadeel van deze methode is bij een storing die u moet het hele subnet, die mogelijk OK verplaatsen, maar dit invloed kan zijn op de failover granulatie overwegingen. 

Laten we bekijken hoe een fictieve enterprise map Contoso mogen de VMs naar een locatie herstel terwijl mislukte via het hele subnet repliceren is. Laten we eerst zien hoe Contoso kunnen hun subnetten beheren is terwijl VMs repliceren tussen twee on-premises implementatie locaties, en vervolgens wordt wordt besproken hoe subnet failover werkt wanneer [dat Azure wordt gebruikt als de noodgevallen herstel-site](#failover-to-azure).

#### <a name="failover-to-a-secondary-on-premises-site"></a>Failover naar een secundaire on-premises-site

Laat ons kijkt u naar een scenario waarbij we wilt behouden de IP van elk van de VMs en storing het volledige subnet samen. De primaire site heeft applicaties subnet 192.168.1.0/24. Als de overname gebeurt, worden alle de virtuele machines die deel uitmaken van dit subnet mislukt naar de site herstel en bewaren van hun IP-adressen. Routes, wachten op de juiste wijze aangepast aan het feit dat alle de virtuele machines die deel uitmaken van subnet 192.168.1.0/24 nu naar de site herstellen verplaatst zijn. 

De routes tussen primaire site en herstel site, derde site en primaire-site en derde site en herstel site moeten correct worden gewijzigd in de volgende afbeelding. 

De volgende afbeeldingen ziet u de subnetten voordat u de overname. Subnet 192.168.0.1/24 actief is op de primaire Site voordat u de overname en actief van de Site herstel na de overname 

![Voordat u Failover](./media/site-recovery-network-design/network-design2.png)

Voordat u failover


De onderstaande afbeelding ziet netwerken en subnetten na failover.
    
![Na Failover](./media/site-recovery-network-design/network-design3.png)

Na failover

Op de secundaire site wordt on-premises en u een VMM-server gebruikt voor het beheren van dit vervolgens als u beveiliging voor een specifieke virtuele machine inschakelt, ASR netwerken resources op basis van de volgende werkstroom worden toegewezen:

- ASR ontvangt, een IP-adres van de statische IP-adresgroep gedefinieerd op de relevante netwerk voor elk exemplaar System Center VMM voor elke netwerkinterface op de virtuele machine.
- Als de beheerder de dezelfde IP-adres van toepassingen voor het netwerk op de site herstel als die van de groep IP-adressen van het netwerk op de primaire-site, definieert terwijl het IP-adres op de replica virtuele machine ASR toewijzen, zou hetzelfde IP-adres als die van de primaire virtuele machine toewijzen.  De IP in VMM is gereserveerd maar niet is ingesteld als failover IP. Failover IP is vlak voor de overname ingesteld.

![IP-adres behouden](./media/site-recovery-network-design/network-design4.png)
    
Afbeelding 5

Afbeelding 5 ziet de Failover TCP/IP-instellingen voor de replica virtuele machine (op de console Hyper-V). Deze instellingen zou worden ingevuld voordat de virtuele machine na een failover wordt gestart

Als de dezelfde IP niet beschikbaar is, zou ASR enkele andere beschikbare IP-adres van de gedefinieerde groep met IP-adressen toewijzen. 

Nadat de VM is ingeschakeld voor beveiliging kunt u volgen voorbeeldscript gebruiken om te controleren of de IP dat is toegewezen aan de virtuele machine. De dezelfde IP wilt instellen als Failover IP en toegewezen aan de VM op het moment van failover:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

>[AZURE.NOTE] In het scenario waar virtuele machines DHCP gebruikt, is het beheer van IP-adressen volledig buiten de controle van ASR. Een beheerder heeft om ervoor te zorgen dat de DHCP-server voor de IP-adressen op de site herstel van hetzelfde bereik als die van de primaire-site dienen kan.

#### <a name="failover-to-azure"></a>Failover naar Azure

Azure Site herstel (ASR) kunt Microsoft Azure moet worden gebruikt als een noodgevallen herstel-website voor uw virtuele machines. U moet in dit geval omgaan met een meer beperking. 

Laten we een scenario waarbij een fictief bedrijf met de naam Woodgrove Bank heeft on-premises implementatie-infrastructuur hostingprovider hun aantal zakelijke toepassingen en ze hun mobiele toepassingen op Azure host onderzoeken. Connectiviteit tussen Woodgrove Bank VMs in Azure en on-premises servers wordt aangeboden door een site-naar-site (S2S) Virtual Private Network (VPN). S2S VPN kan van Woodgrove Bank virtual netwerk in Azure worden gezien als een uitbreiding van van Woodgrove Bank on-premises netwerk. Deze communicatie is ingeschakeld door S2S VPN tussen Woodgrove Bank rand en Azure virtuele netwerk. Nu wil Woodgrove ASR gebruiken om te repliceren van de werkbelasting uitgevoerd in de datacenter naar Azure. Deze optie voldoet aan de behoeften van Woodgrove, dat wil een min kosten DR-optie en kunnen worden gegevens opgeslagen in openbare cloud-omgevingen. Woodgrove is te handelen toepassingen en configuraties die afhankelijk van hard gecodeerde IP-adressen zijn, zodat ze een vereiste moeten worden bewaard IP-adressen voor hun toepassingen na worden niet via Azure hebben.

Woodgrove heeft besloten om toe te wijzen IP-adressen van IP-adresbereiken (172.16.1.0/24, 172.16.2.0/24) tot de bronnen die wordt uitgevoerd in Azure wordt aangegeven.

Voor Woodgrove moeten kunnen de virtuele machines repliceren naar Azure behoud de IP-adressen, moet een Azure Virtual Network worden gemaakt. Een uitbreiding van de on-premises netwerk moet zijn zodat toepassingen van de on-premises implementatie-site naar Azure naadloos kunnen. Azure kunt u VPN-verbinding voor site-naar-site, evenals punt-naar-site toevoegen aan de virtuele netwerken die zijn gemaakt in Azure wordt aangegeven. Wanneer u de verbinding van uw site-naar-site instelt, wordt er Azure netwerk kunt u verkeer doorsturen naar de on-premises locatie (Azure gebeld deze LAN) alleen als het IP-adresbereiken van de on-premises implementatie IP-adresbereiken, verschilt omdat Azure biedt geen ondersteuning voor subnetten uit elkaar te bewegen.  Dit betekent dat er een 192.168.1.0/24 subnet on-premises implementatie, u kunt niet een LAN 192.168.1.0/24 toevoegen in het Azure netwerk. Dit is normaal omdat Azure niet zeker weet dat er geen actieve VMs in het subnet en dat het subnet alleen ter informatie DR wordt gemaakt. Als u wilt kunnen worden gerouteerd correct netwerkverkeer afmelden bij een Azure netwerk de subnetten die in het netwerk en het LAN mag geen conflict opleveren. 

![Voordat u Subnet Failover](./media/site-recovery-network-design/network-design7.png)

Voordat u failover

Om te voldoen aan de vereisten van hun bedrijven Woodgrove, moeten we implementeren van de volgende werkstromen:

- Een extra netwerk maken, laat ons Roep deze herstel netwerk, waar de is mislukt op virtuele machines moet worden gemaakt.
- De dezelfde IP dat de VM on-premises implementatie heeft om ervoor te zorgen dat de IP voor een VM na een failover blijft behouden, gaat u naar het tabblad configureren onder VM eigenschappen opgeven en klik vervolgens op opslaan. Wanneer de VM storing is opgetreden, wordt Azure Site herstel de meegeleverde IP toewijzen aan de virtuele machine. 

![Netwerkeigenschappen van het](./media/site-recovery-network-design/network-design8.png)

Zodra de overname wordt geactiveerd en de virtuele machines zijn gemaakt in het netwerk herstel met het gewenste IP-, kan verbinding met dit netwerk worden gemaakt met een [Vnet naar Vnet verbinding](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Als deze actie kan worden vastgelegd in een script vereist.  Wat is behandeld in de vorige sectie over subnet failover, zelfs wanneer een failover naar Azure, zou routes moeten correct aangepast aan dat die 192.168.1.0/24 heeft nu verplaatst naar Azure. 

![Na Subnet Failover](./media/site-recovery-network-design/network-design9.png)

Na failover

Als u geen een Azure netwerk zoals wordt weergegeven in de bovenstaande afbeelding. U kunt een site naar site vpn-verbinding tussen uw 'Primaire locatie' en 'Herstel netwerk' na de overname maken.  


## <a name="option-2-changing-ip-addresses"></a>Optie 2: IP-adressen wijzigen

Deze methode lijkt de meest gangbare op basis van wat wij hebben. Het duurt het formulier van het wijzigen van het IP-adres van elke VM die deel uitmaakt van de overname. Een nadeel van deze methode is de binnenkomende netwerk ' meer ' de toepassing die bij IPx is nu aan IPy vereist. Zelfs als IPx en IPy logische namen, DNS entries hebben meestal moeten worden gewijzigd of leeggemaakt overal in het netwerk, en in de cache opgeslagen gegevens uit netwerk tabellen moeten worden bijgewerkt of leeggemaakt, dus een downtime mogelijk zichtbaar, afhankelijk van hoe de DNS-infrastructuur ingesteld is. Deze problemen kunnen worden omzeild met lage TTL-waarden in het geval van intranettoepassingen en [Azure verkeer Manager met ASR](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) voor internet op basis van toepassingen gebruiken

### <a name="changing-the-ip-addresses---illustration"></a>De IP-adressen - wijzigen afbeelding

Laat ons kijkt u naar het scenario waar u van plan bent het gebruik van verschillende IP-adressen in de primaire en de herstel-sites. In het volgende voorbeeld we ook hebben een externe site vanaf waar de toepassingen die worden gehost op primair of herstel de site kan worden geopend.

![Verschillende IP - voordat failover wordt uitgevoerd](./media/site-recovery-network-design/network-design10.png)

Afbeelding 11

Afbeelding 11 zijn sommige toepassingen die zijn ingesloten in een subnet 192.168.1.0/24 subnet op de primaire-site en ze te kunnen komen op de site herstel in subnet 172.16.1.0/24 na een failover zijn geconfigureerd. VPN verbindingen/netwerk routes er correct zijn geconfigureerd, zodat alle drie sites toegankelijk elkaar.
 
Als afbeelding 12 aangeeft, nadat ze zijn mislukt via een of meer toepassingen, worden ze in het subnet herstelproces is hersteld. In dit geval worden we niet beperkt foutherstel voor het hele subnet op hetzelfde moment. Geen wijzigingen zijn vereist om te configureren, routes VPN of in een netwerk. Een failover en sommige DNS-updates zorgt ervoor dat toepassingen toegankelijk blijven zijn. Als de DNS-records is geconfigureerd voor dynamische updates toestaan zou klikt u vervolgens de virtuele machines registreren zichzelf via de nieuwe IP zodra ze na een failover starten. 

![Verschillende IP - na Failover](./media/site-recovery-network-design/network-design11.png)

Figuur 12

De replica virtuele machine mogelijk na mislukte-over een IP-adres dat is niet hetzelfde als het IP-adres van de primaire virtuele machine. Virtuele machines worden bijgewerkt via de DNS-server die ze gebruiken nadat ze starten. DNS-vermeldingen meestal moeten worden gewijzigd of leeggemaakt overal in het netwerk en in de cache opgeslagen gegevens uit tabellen van netwerk hoeft te worden bijgewerkt of leeggemaakt zodat u niet vaak worden geconfronteerd met downtime terwijl deze wijzigingen staat plaatsvinden. Dit probleem kan worden beperkt door:

- Lage TTL-waarden voor intranettoepassingen gebruiken.
- Met behulp van Azure verkeer Manager met ASR voor internet op basis van toepassingen.
- Gebruik van de volgende script binnen uw abonnement herstel bij de DNS-Server om ervoor te zorgen een tijdige bijwerken (het script is niet vereist als de dynamische DNS-registratie is geconfigureerd)

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


### <a name="changing-the-ip-addresses--dr-to-azure"></a>De IP-adressen – wijzigen DR naar Azure

Het blogbericht [Toegang infrastructuur Setup voor Microsoft Azure als een noodgevallen herstel-Site](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wordt uitgelegd hoe de vereiste Azure netwerkinfrastructuur instellen als behoudt IP-adressen niet een vereiste is. Deze begint met het met een beschrijving van de toepassing en kijk hoe u setup-netwerken on-premises implementatie en klik op Azure en vervolgens sluiting van hoe een failover testen en een geplande overname bij storing.



## <a name="next-steps"></a>Volgende stappen

[Leer](site-recovery-network-mapping.md) hoe Site herstel kaarten bronsite en doelsites netwerken wanneer een server VMM wordt gebruikt voor het beheren van de primaire-site. 
