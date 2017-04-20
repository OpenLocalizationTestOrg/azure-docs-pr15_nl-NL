<properties 
   pageTitle="Opties voor het omzetten van DNS-naam voor de VMs Linux in Azure wordt aangegeven"
   description="Naam resolutie scenario's voor Linux VMs in Azure IaaS, inclusief opgegeven DNS-services, hybride externe DNS en doen om uw eigen DNS-server."
   services="virtual-machines"
   documentationCenter="na"
   authors="RicksterCDN"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="rclaus" />

# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>DNS-naam-opties voor Opnameresolutie voor Linux VMs in Azure wordt aangegeven

Azure biedt DNS-naamresolutie al dan niet standaard voor alle VMs opgenomen in één virtuele netwerk. U zijn kunt uw eigen DNS-naam resolutie oplossing implementeren door uw eigen DNS-services configureren op uw Azure gehoste VMs. De volgende scenario's kunt u kiezen welke beter werkt voor uw specifieke situatie.

- [Azure-voorwaarde mailnamen omzetten](#azure-provided-name-resolution)

- [Naamresolutie met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) 

Het type naamresolutie die u gebruikt, is afhankelijk van hoe uw VMs en rol exemplaren moeten met elkaar communiceren.

**De volgende tabel ziet u scenario's en de bijbehorende naam resolutie oplossingen:**

| **Scenario** | **Oplossing** | **Achtervoegsel** |
|--------------|--------------|----------|
| Naamresolutie tussen exemplaren van de rol of VMs bevinden in hetzelfde virtuele netwerk | [Azure-voorwaarde mailnamen omzetten](#azure-provided-name-resolution)| hostname of FQDN-naam |
| Naamresolutie tussen exemplaren van de rol of VMs zich in verschillende virtuele netwerken | Klant-beheerde DNS-servers doorsturen van query's tussen vnets voor resolutie door Azure (DNS-proxy).  Zie [naamresolutie met uw eigen DNS-server](#name-resolution-using-your-own-dns-server)| Alleen FQDN |
| Resolutie van de lokale computer en service de namen van de rol exemplaren of VMs in Azure wordt aangegeven | Klant-beheerde DNS-servers (bijvoorbeeld on-premises domeincontroller, lokale alleen-lezen domeincontroller of een DNS-secundaire die is gesynchroniseerd met zone-overdrachten).  Zie [naamresolutie met uw eigen DNS-server](#name-resolution-using-your-own-dns-server)|Alleen FQDN |
| Resolutie van Azure hostnamen van on-premises computers | Doorsturen van query's naar een proxyserver klant managed DNS in de bijbehorende vnet, de proxyserver stuurt query's naar Azure voor resolutie. Zie [naamresolutie met uw eigen DNS-server](#name-resolution-using-your-own-dns-server)| Alleen FQDN |
| Omgekeerde DNS-records voor interne IP-adressen | [Naamresolutie met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) | n/b |

## <a name="azure-provided-name-resolution"></a>Azure-voorwaarde mailnamen omzetten

Samen met de resolutie van openbare DNS-namen biedt Azure interne naamresolutie voor VMs en rol-exemplaren die zich binnen hetzelfde virtuele netwerk bevinden.  In de ARM gebaseerde virtuele netwerken, het achtervoegsel consistent is op het virtuele netwerk (zodat de FQDN-naam niet nodig is) en DNS-namen kunnen worden toegewezen aan zowel NIC's en VMs. Hoewel geleverde Azure naamresolutie niet voor de configuratie vereist is, is het niet de juiste keuze voor alle implementatie-scenario's, zoals gezien in de vorige tabel.

### <a name="features-and-considerations"></a>Functies en overwegingen

**Functies:**

- Gebruiksgemak: geen configuratie is vereist voor het gebruiken van Azure-voorwaarde naamresolutie.

- De naam van Azure resolutie-service is beschikbaar zijn, zodat u dat hoeft te maken en beheren van clusters van uw eigen DNS-servers.

- Kan worden gebruikt samen met uw eigen DNS-servers voor het oplossen van zowel on-premises en Azure hostnamen.

- Naamresolutie wordt aangeboden tussen VMs in virtuele netwerken zonder dat u de FQDN-naam. 

- U kunt hostnamen die het beste uw implementaties beschrijven, in plaats van werken met de automatisch gegenereerde namen.

**Overwegingen:**

- Het Azure gemaakte DNS-achtervoegsel kan niet worden gewijzigd.

- U kunt uw eigen records niet handmatig registreren.

- WINS- en NetBIOS worden niet ondersteund.

- Hostnamen moet DNS-compatibele (ze alleen 0-9, a-z moeten gebruiken en '-', en kunnen niet beginnen of eindigen met een '-'. Zie RFC 3696 sectie 2.)

- DNS-queryverkeer is vertraagd voor elke VM. Dit niet mag voor invloed op de meeste toepassingen.  Als verzoek beperken is voldaan, moet u ervoor zorgen dat aan de clientzijde caching is ingeschakeld.  Zie [het beste geleverde Azure naamresolutie](#Getting-the-most-from-Azure-provided-name-resolution)voor meer informatie.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Ophalen van de meest uit geleverde Azure-mailnamen omzetten
**Aan de clientzijde Caching:**

Niet alle DNS-query is verzonden via het netwerk.  Aan de clientzijde caching kunt verminderen latentie en flexibiliteit naar netwerk blips verbeteren door terugkerende DNS-query's uit een lokale cache oplossen.  DNS-records bevatten een Time To Live (TTL) waarmee de cache voor de opslag van de record voor zo lang mogelijk zonder die invloed hebben op record fris.  Reden is caching van aan de clientzijde geschikt voor de meeste gevallen.

Sommige distros Linux Neem niet caching al dan niet standaard.  Het wordt aanbevolen dat u één voor elke VM Linux (na het controleren dat nog niet een lokale cache is) toevoegen.

Er zijn verschillende verschillende DNS-cache pakketten beschikbaar wordt weergegeven, bijvoorbeeld dnsmasq, Hier vindt u de stappen voor het installeren van dnsmasq op de meest voorkomende distros:

- **Ubuntu (maakt gebruik van resolvconf)**:
    - Installeer het pakket dnsmasq ("sudo enz-get-installatie dnsmasq").
- **SUSE (maakt gebruik van netconf)**:
    - Installeer het pakket dnsmasq ("sudo zypper installeren dnsmasq") 
    - Schakel de dnsmasq-service ("systemctl inschakelen dnsmasq.service") 
    - Start de dnsmasq-service ("systemctl start dnsmasq.service") 
    - bewerken '/ enzovoort/sysconfig/netwerk/config' en wijzig NETCONFIG_DNS_FORWARDER = "' naar"dnsmasq"
    - resolv.conf ("netconfig update') om in te stellen van de cache als de lokale DNS-resolvercache bijwerken
- **OpenLogic (maakt gebruik van NetworkManager)**:
    - Installeer het pakket dnsmasq ("sudo yum installeren dnsmasq")
    - Schakel de dnsmasq-service ("systemctl inschakelen dnsmasq.service")
    - Start de dnsmasq-service ("systemctl start dnsmasq.service")
    - "Voeg domain name servers 127.0.0.1;" toevoegen aan de "/etc/dhclient-eth0.conf"
    - de Netwerkservice ("service netwerk opnieuw starten') om in te stellen van de cache als de lokale DNS-resolvercache opnieuw starten

> [AZURE.NOTE]: Het pakket 'dnsmasq' is slechts één van de vele DNS-cache die beschikbaar zijn voor Linux.  Voordat u deze gebruikt, controleert u geschikt is voor uw specifieke wensen en dat er geen andere cache is geïnstalleerd.

**Aan de clientzijde pogingen:**

DNS is hoofdzakelijk een UDP-protocol.  Als het UDP-protocol niet bericht is bezorgd garandeert, wordt opnieuw logica verwerkt in het DNS-protocol zelf.  Elke DNS-client (besturingssysteem) kan verschillende opnieuw logica afhankelijk van de voorkeur makers vertonen:

 - Besturingssystemen Windows opnieuw proberen na een tweede en kies nogmaals na een andere twee, vier en andere vier seconden. 
 - De standaard Linux setup pogingen na vijf seconden.  U moet dit om te proberen vijf keer tijdsintervallen één seconde te wijzigen.  

De huidige instellingen in een Linux VM, 'kat /etc/resolv.conf' en kijkt u op de regel 'opties' bijvoorbeeld controleren:

    options timeout:1 attempts:5

Het bestand resolv.conf automatisch wordt gegenereerd en niet mag worden bewerkt.  De procedure voor het toevoegen van de regel 'opties' is afhankelijk van het distro:

- **Ubuntu** (gebruikt resolvconf):
    - de opties regel toe te voegen ' / etc/resolveconf/resolv.conf.d/head' 
    - 'resolvconf -i' uitvoeren om bij te werken
- **SUSE** (gebruikt netconf):
    - 'timeout:1 pogingen: 5' toevoegen aan de NETCONFIG_DNS_RESOLVER_OPTIONS = "" parameter in '/ enzovoort/sysconfig/netwerk/config' 
    - 'netconfig update' uitvoeren om bij te werken
- **OpenLogic** (gebruikt NetworkManager):
    - 'echo "opties timeout:1 pogingen: 5" ' toevoegen aan ' / etc/NetworkManager/dispatcher.d/11-dhclient' 
    - uitvoeren 'service-netwerk opnieuw starten' bijwerken

## <a name="name-resolution-using-your-own-dns-server"></a>Naamresolutie met uw eigen DNS-server
Er zijn verschillende situaties waarin de behoeften van uw naam resolutie mogelijk een stapje verder dan de functies mits door Azure, bijvoorbeeld wanneer u DNS-resolutie tussen virtuele netwerken (vnets vereist).  Dit scenario bedekt, kunt Azure u zich met uw eigen DNS-servers.  

DNS-servers binnen een virtueel netwerk kunnen DNS-query's doorsturen naar van Azure recursieve resolvers om op te lossen hostnamen binnen dat virtuele netwerk.  Een DNS-server in Azure kan bijvoorbeeld reageren op DNS-query's voor een eigen DNS-zone-bestanden en alle andere query's doorsturen naar Azure.  Hierdoor VMs om beide uw vermeldingen in de zonebestanden en Azure-voorwaarde hostnamen (via de doorstuurservers) weer te geven.  Toegang tot van Azure recursieve resolvers is verstrekt via het virtuele IP 168.63.129.16.

DNS-doorsturen ook schakelt op inter vnet DNS-resolutie en kan uw on-premises machines voor het oplossen van Azure-voorwaarde hostnamen.  U lost van een VM hostname door de DNS-server VM moet zich bevinden in hetzelfde virtuele netwerk en worden geconfigureerd voor doorsturen hostname query's naar Azure.  Als het achtervoegsel is er anders in elke vnet, kunt u voorwaardelijke doorstuurregels DNS-query's verzenden naar de juiste vnet voor resolutie.  De volgende afbeelding ziet u twee vnets en een on-premises netwerk inter vnet DNS-resolutie met deze methode te doen:

![Inter vnet DNS](./media/virtual-machines-linux-azure-dns/inter-vnet-dns.png)

Wanneer u met Azure-voorwaarde naamresolutie, is het interne DNS-achtervoegsel opgegeven voor elke VM met DHCP.  Wanneer u met uw eigen naam resolutie oplossing, is dit achtervoegsel verstrekt aan VMs omdat deze andere DNS-architecturen verstoort.  Om te verwijzen naar machines door de FQDN-naam of het achtervoegsel configureren op uw VMs, kan het achtervoegsel worden bepaald via PowerShell of de API:

-  Voor resourcebeheer Azure vnets beheerde, het achtervoegsel beschikbaar via de resource [netwerk interfacekaart is](https://msdn.microsoft.com/library/azure/mt163668.aspx) of u kunt de opdracht uitvoeren `azure network public-ip show <resource group> <pip name>` om de details van uw openbare IP, inclusief de FQDN-naam van de NIC weer te geven    


Als het doorsturen van query's naar Azure niet aansluiten op uw behoeften voldoet, moet u uw eigen DNS-oplossing bieden.  Uw DNS-oplossing moet:

-  Geef de juiste hostname resolutie, bijvoorbeeld via [DDNS](../virtual-network/virtual-networks-name-resolution-ddns.md).  Opmerking Als DDNS mogelijk moet u DNS-record opruimen van Azure DHCP lease zijn heel lang en opruimen uitschakelen DNS-records voortijdig verwijderen. 
-  Geef de juiste recursieve resolutie toe te staan dat de resolutie van externe domeinnamen.
-  Toegankelijke (TCP- en UDP op poort 53) van de clients die deze fungeert en kunnen worden gebruikt voor toegang tot internet.
-  Worden beveiligd tegen toegang van internet, te verminderen risico's externe agenten.

> [AZURE.NOTE] Wanneer u Azure VMs als de DNS-servers, IPv6 moet worden uitgeschakeld voor de beste prestaties en een [Openbare IP exemplaar niveau](../virtual-network/virtual-networks-instance-level-public-ip.md) moeten worden toegewezen aan elke DNS-server VM.  

