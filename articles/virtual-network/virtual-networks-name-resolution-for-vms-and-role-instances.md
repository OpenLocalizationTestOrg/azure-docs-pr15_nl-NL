<properties 
   pageTitle="Resolutie voor VMs en rol exemplaren"
   description="Een naam geven resolutie scenario's voor Azure IaaS, hybride oplossingen, tussen verschillende cloudservices, Active Directory en het gebruik van uw eigen DNS-server "
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="telmos" />

# <a name="name-resolution-for-vms-and-role-instances"></a>Naamresolutie voor VMs en rol exemplaren

Afhankelijk van hoe u Azure gebruiken voor het hosten van IaaS, PaaS en hybride oplossingen, moet u mogelijk dat de VMs en rol-exemplaren die u maakt om te communiceren met elkaar zijn toegestaan. Deze communicatie kunt doen met behulp van IP-adressen, is maar het veel eenvoudiger gebruik van namen die u kunnen gemakkelijk kunnen worden onthouden en beter niet wijzigen. 

Als rol exemplaren en VMs ingesloten in een Azure nodig hebt om te zetten domeinnamen interne IP-adressen, kunnen ze gebruik een van twee methoden:

- [Azure-voorwaarde mailnamen omzetten](#azure-provided-name-resolution)

- [Naamresolutie met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) (die mogelijk query doorsturen naar de geleverde Azure DNS-servers) 

Het type naamresolutie die u gebruikt, is afhankelijk van hoe uw VMs en rol exemplaren moeten met elkaar communiceren.

**De volgende tabel ziet u scenario's en de bijbehorende naam resolutie oplossingen:**

| **Scenario** | **Oplossing** | **Achtervoegsel** |
|--------------|--------------|----------|
| Naamresolutie tussen exemplaren van de rol of VMs zich bevindt in de dezelfde cloudservice of virtueel netwerk | [Azure-voorwaarde mailnamen omzetten](#azure-provided-name-resolution)| hostname of FQDN-naam |
| Naamresolutie tussen exemplaren van de rol of VMs zich in verschillende virtuele netwerken | Klant-beheerde DNS-servers doorsturen van query's tussen vnets voor resolutie door Azure (DNS-proxy).  Zie [naamresolutie met uw eigen DNS-server](#name-resolution-using-your-own-dns-server)| Alleen FQDN |
| Resolutie van de lokale computer en service de namen van de rol exemplaren of VMs in Azure wordt aangegeven | Klant-beheerde DNS-servers (bijvoorbeeld on-premises domeincontroller, lokale alleen-lezen domeincontroller of een DNS-secundaire die is gesynchroniseerd met zone-overdrachten).  Zie [naamresolutie met uw eigen DNS-server](#name-resolution-using-your-own-dns-server)|Alleen FQDN |
| Resolutie van Azure hostnamen van on-premises computers | Doorsturen van query's naar een proxyserver klant managed DNS in de bijbehorende vnet, de proxyserver stuurt query's naar Azure voor resolutie. Zie [naamresolutie met uw eigen DNS-server](#name-resolution-using-your-own-dns-server)| Alleen FQDN |
| Omgekeerde DNS-records voor interne IP-adressen | [Naamresolutie met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) | n/b |
| Naamresolutie tussen VMs of rol exemplaren zich in verschillende cloudservices, niet in een virtueel netwerk| Niet van toepassing. Connectiviteit tussen VMs en exemplaren van de rol in verschillende cloudservices wordt niet ondersteund buiten een virtueel netwerk.| n/b |



## <a name="azure-provided-name-resolution"></a>Azure-voorwaarde mailnamen omzetten

Samen met de resolutie van openbare DNS-namen biedt Azure interne naamresolutie voor VMs en rol-exemplaren die zich in het hetzelfde virtuele netwerk of cloudservice bevinden.  VMs/exemplaren in een cloudservice delen het dezelfde achtervoegsel (zodat het alleen hostname voldoende is), maar in de klassieke virtuele netwerken verschillende cloud services hebben verschillende DNS-achtervoegsels zodat de FQDN-naam voor het oplossen van namen tussen verschillende cloudservices nodig is.  In de virtuele netwerken in het implementatiemodel resourcemanager, het achtervoegsel consistent is op het virtuele netwerk (zodat de FQDN-naam niet nodig is) en DNS-namen kunnen worden toegewezen aan zowel NIC's en VMs. Hoewel geleverde Azure naamresolutie niet voor de configuratie vereist is, is het niet de juiste keuze voor alle implementatie-scenario's, zoals gezien in de bovenstaande tabel.

> [AZURE.NOTE] In het geval van web en werknemer rollen, kunt u ook toegang tot het interne IP-adressen van de rol exemplaren die op basis van de rol en het exemplaar getal dat de Azure-Service Management REST API gebruiken. Zie [Service Management REST API Reference](https://msdn.microsoft.com/library/azure/ee460799.aspx)voor meer informatie.

### <a name="features-and-considerations"></a>Functies en overwegingen

**Functies:**

- Gebruiksgemak: geen configuratie is vereist om te kunnen gebruiken geleverde Azure naamresolutie.

- De naam van Azure resolutie-service is beschikbaar zijn, zodat u dat hoeft te maken en beheren van clusters van uw eigen DNS-servers.

- Kan worden gebruikt in combinatie met uw eigen DNS-servers voor het oplossen van zowel on-premises en Azure hostnamen.

- Naamresolutie wordt aangeboden tussen rol exemplaren/VMs binnen de dezelfde cloudservice zonder dat u een FQDN-naam.

- Naamresolutie wordt aangeboden tussen VMs in virtuele netwerken die gebruikmaken van het implementatiemodel resourcemanager, zonder dat u de FQDN-naam. Virtuele netwerken in het implementatiemodel klassieke nodig de FQDN-naam wanneer de namen in verschillende cloudservices. 

- U kunt hostnamen die het beste uw implementaties beschrijven, in plaats van werken met de automatisch gegenereerde namen.

**Overwegingen:**

- Het Azure gemaakte DNS-achtervoegsel kan niet worden gewijzigd.

- U kunt uw eigen records niet handmatig registreren.

- WINS- en NetBIOS worden niet ondersteund. (U kunt uw VMs in Windows Verkenner niet zien.)

- Hostnamen moet DNS-compatibele (ze alleen 0-9, a-z moeten gebruiken en '-', en kunnen niet beginnen of eindigen met een '-'. Zie RFC 3696 sectie 2.)

- DNS-queryverkeer is vertraagd voor elke VM. Dit niet mag voor invloed op de meeste toepassingen.  Als verzoek beperken is voldaan, moet u ervoor zorgen dat aan de clientzijde caching is ingeschakeld.  Zie [het beste geleverde Azure naamresolutie](#Getting-the-most-from-Azure-provided-name-resolution)voor meer informatie.

- Alleen VMs in de eerste 180 cloudservices zijn geregistreerd voor elk virtual netwerk in een implementatiemodel klassieke. Dit geldt niet voor virtuele netwerken in resourcemanager implementatiemodellen.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Ophalen van de meest uit geleverde Azure-mailnamen omzetten
**Aan de clientzijde Caching:**

Niet alle DNS-query moet worden verzonden via het netwerk.  Aan de clientzijde caching kunt verminderen latentie en flexibiliteit naar netwerk blips verbeteren door terugkerende DNS-query's uit een lokale cache oplossen.  DNS-records bevatten een Time To Live (TTL) waarmee de cache voor de opslag van de record voor zo lang mogelijk zonder die invloed hebben op record fris, zodat de meeste gevallen caching van aan de clientzijde is.

De standaard Windows DNS-Client heeft een ingebouwde DNS-cache.  Sommige Linux distros bevatten geen caching al dan niet standaard, het wordt aanbevolen dat een worden toegevoegd aan elke VM Linux (na het controleren dat nog niet is een lokale cache).

Er zijn een aantal verschillende DNS-cache pakketten beschikbaar wordt weergegeven, bijvoorbeeld dnsmasq, Hier vindt u de stappen voor het installeren van dnsmasq op de meest voorkomende distros:

- **Ubuntu (maakt gebruik van resolvconf)**:
    - Installeer alleen het dnsmasq-pakket ("sudo enz-get-installatie dnsmasq").
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

> [AZURE.NOTE] Het pakket 'dnsmasq' is slechts één van de vele DNS-cache die beschikbaar zijn voor Linux.  Controleer de geschiktheid voor uw specifieke wensen voordat u deze gebruikt en dat er geen andere cache is geïnstalleerd.

**Aan de clientzijde pogingen:**

DNS is hoofdzakelijk een UDP-protocol.  Als het UDP-protocol niet bericht is bezorgd garandeert, wordt opnieuw logica verwerkt in het DNS-protocol zelf.  Elke DNS-client (besturingssysteem) kan verschillende opnieuw logica afhankelijk van de voorkeur makers vertonen:

 - Besturingssystemen Windows opnieuw proberen na 1 tweede en kies nogmaals na een andere 2, 4 en andere 4 seconden. 
 - De standaard Linux setup pogingen na 5 seconden.  Het wordt aanbevolen om te wijzigen Hiermee kunt u proberen 5 keer met 1 tweede tussenpozen.  

De huidige instellingen in een Linux VM, 'kat /etc/resolv.conf' en kijkt u op de regel 'opties' bijvoorbeeld controleren:

    options timeout:1 attempts:5

Het bestand resolv.conf is meestal automatisch gegenereerd en niet mag worden bewerkt.  De procedure voor het toevoegen van de regel 'opties' is afhankelijk van het distro:

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
Er zijn een aantal situaties waarin de behoeften van uw naam resolutie mogelijk Ga voorbij de functies van Azure, bijvoorbeeld wanneer Active Directory domeinen gebruikt of wanneer u DNS-resolutie tussen virtuele netwerken (vnets) vereist.  Deze scenario's bedekt, kunt Azure u zich met uw eigen DNS-servers.  

DNS-servers binnen een virtueel netwerk kunnen DNS-query's doorsturen naar van Azure recursieve resolvers om op te lossen hostnamen binnen dat virtuele netwerk.  Een domein Controller (domeincontroller) uitgevoerd in Azure kan bijvoorbeeld reageren op DNS-query's voor de domeinen en alle andere query's doorsturen naar Azure.  Hierdoor VMs om uw on-premises bronnen (via de domeincontroller) en de geleverde Azure hostnamen (via de doorstuurservers) weer te geven.  Toegang tot van Azure recursieve resolvers is verstrekt via het virtuele IP 168.63.129.16.

DNS-doorsturen ook schakelt op inter vnet DNS-resolutie en kan uw on-premises machines voor het oplossen van Azure-voorwaarde hostnamen.  Om op te lossen van een VM hostname, de DNS-server VM moet zich bevinden in hetzelfde virtuele netwerk en worden geconfigureerd voor doorsturen hostname query's naar Azure.  Als het achtervoegsel is er anders in elke vnet, kunt u voorwaardelijke doorstuurregels DNS-query's verzenden naar de juiste vnet voor resolutie.  De volgende afbeelding ziet u twee vnets en een on-premises netwerk inter vnet DNS-resolutie met deze methode te doen.  Een voorbeeld van de DNS-doorstuurservers is beschikbaar in de [galerie met sjablonen van Azure snelstartgids](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) en [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Inter vnet DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Bij gebruik van Azure-voorwaarde naamresolutie, een interne DNS-achtervoegsel (*. internal.cloudapp.net) is opgegeven voor elke VM met DHCP.  Hiermee worden hostname resolutie als de hostname-records in de zone internal.cloudapp.net worden.  Wanneer u met uw eigen naam resolutie oplossing, is het achtervoegsel IDN verstrekt aan VMs omdat deze andere DNS-architecturen (zoals scenario's voor het domein behoren verstoort).  Wij bieden in plaats daarvan een niet-functioneel tijdelijke aanduiding (reddog.microsoft.com).  

Indien nodig kan het interne DNS-achtervoegsel worden bepaald via PowerShell of de API:

-  Het achtervoegsel is virtual netwerken in resourcemanager implementatiemodellen beschikbaar via de resource [netwerk interfacekaart](https://msdn.microsoft.com/library/azure/mt163668.aspx) of de cmdlet [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) .    
-  In de klassieke implementatiemodellen, het achtervoegsel is beschikbaar via de [Implementatie-API ophalen](https://msdn.microsoft.com/library/azure/ee460804.aspx) -oproep of via de [Get-AzureVM-fouten opsporen in](https://msdn.microsoft.com/library/azure/dn495236.aspx) cmdlet.


Als het doorsturen van query's naar Azure niet aansluiten op uw behoeften voldoet, moet u uw eigen DNS-oplossing bieden.  Uw DNS-oplossing moet:

-  Geef de juiste hostname resolutie, bijvoorbeeld via [DDNS](virtual-networks-name-resolution-ddns.md).  Opmerking Als DDNS mogelijk moet u DNS-record opruimen van Azure DHCP lease zijn heel lang en opruimen uitschakelen DNS-records voortijdig verwijderen. 
-  Geef de juiste recursieve resolutie toe te staan dat de resolutie van externe domeinnamen.
-  Toegankelijke (TCP- en UDP op poort 53) van de clients die deze fungeert en kunnen worden gebruikt voor toegang tot internet.
-  Worden beveiligd tegen toegang van internet, te verminderen risico's externe agenten.

> [AZURE.NOTE] Wanneer u Azure VMs als de DNS-servers, IPv6 moet worden uitgeschakeld voor de beste prestaties en een [Openbare IP exemplaar niveau](virtual-networks-instance-level-public-ip.md) moeten worden toegewezen aan elke DNS-server VM.  Als u besluit om het gebruik van Windows Server als uw DNS-server, wordt [in dit artikel](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) biedt aanvullende prestatie-analyse en optimalisaties toe.


### <a name="specifying-dns-servers"></a>DNS-servers opgeven

Wanneer u met uw eigen DNS-servers, Azure Hiermee kunt u meerdere DNS-servers per virtueel netwerk of per netwerkinterface (resourcemanager) opgeven / cloud service (klassieke).  Prioriteit van regels voor krijgen DNS-servers die zijn opgegeven voor de interface van een cloud-service/netwerk via die zijn opgegeven voor het virtuele netwerk.

> [AZURE.NOTE] Verbindingseigenschappen netwerk, zoals DNS-server IP-adressen, mag niet worden bewerkt rechtstreeks vanuit Windows VMs terwijl ze kunnen ophalen gewist tijdens de services retoucheren wanneer de virtuele netwerkadapter wordt vervangen. 


Wanneer u met het implementatiemodel resourcemanager, kunnen de DNS-servers worden opgegeven in de Portal API-sjablonen ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) of PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [nic](https://msdn.microsoft.com/library/mt619370.aspx)).

Wanneer u met het implementatiemodel klassieke, kunnen de DNS-servers voor het virtuele netwerk worden opgegeven in de Portal of [het bestand *Netwerkconfiguratie* ](https://msdn.microsoft.com/library/azure/jj157100).  Voor cloudservices, worden de DNS-servers zijn opgegeven via [de *Service* -configuratiebestand](https://msdn.microsoft.com/library/azure/ee758710) of in PowerShell ([Nieuw-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [AZURE.NOTE] Als u de DNS-instellingen voor een virtuele netwerk/virtuele machine die al is geïmplementeerd wijzigt, moet u opnieuw starten van elke desbetreffende VM totdat de wijzigingen te activeren.


## <a name="next-steps"></a>Volgende stappen

Resourcemanager implementatiemodel:

- [Een virtueel netwerk maken of bijwerken](https://msdn.microsoft.com/library/azure/mt163661.aspx)
- [Maken of bijwerken van een netwerk interface-kaart](https://msdn.microsoft.com/library/azure/mt163668.aspx)
- [Nieuwe AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
- [Nieuwe AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

 
Klassieke implementatiemodel:

- [Azure-Service configuratieschema](https://msdn.microsoft.com/library/azure/ee758710)
- [Virtual Network configuratieschema](https://msdn.microsoft.com/library/azure/jj157100)
- [Een virtueel netwerk met behulp van een netwerk configuratiebestand configureren](virtual-networks-using-network-configuration-file.md) 

