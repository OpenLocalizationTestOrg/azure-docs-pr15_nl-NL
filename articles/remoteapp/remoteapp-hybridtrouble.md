
<properties
    pageTitle="Problemen met maken RemoteApp hybride verzamelingen | Microsoft Azure"
    description="Meer informatie over het oplossen van fouten bij RemoteApp hybride siteverzameling maken"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Problemen met maken Azure RemoteApp hybride verzamelingen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Een verzameling hybride wordt gehost en worden gegevens opgeslagen in de cloud Azure, maar ook kunt gebruikers access-gegevens en resources die zijn opgeslagen op uw lokale netwerk. Gebruikers hebben toegang tot apps door aanmelden met hun bedrijfsreferenties gesynchroniseerd of federatieve met Azure Active Directory. Kunt u een verzameling hybride die gebruikmaakt van een bestaand Azure virtuele netwerk implementeren, of u een nieuwe virtueel netwerk kunt maken. Het is raadzaam dat u maakt of een virtueel netwerk subnet met een bereik CIDR groot genoeg voor verwachte toekomstige groei voor Azure RemoteApp gebruiken.

De verzameling nog niet hebt worden gemaakt? Zie [maken een hybride siteverzameling](remoteapp-create-hybrid-deployment.md) voor de stappen.

Als u problemen ondervindt bij het maken van uw siteverzameling of als de verzameling niet zoals werkt u denkt dat nodig is, raadpleegt u de volgende informatie.

## <a name="your-image-is-invalid"></a>Uw afbeelding is ongeldig ##
Als u een bericht zoals 'GoldImageInvalid' ziet wanneer u Azure wacht voor het inrichten van uw siteverzameling, betekent dit dat de afbeelding van uw sjabloon niet voldoet aan de [vereisten van de afbeelding gedefinieerd](remoteapp-imagereqs.md). Ja, Ga lezen die [vereisten](remoteapp-imagereqs.md), uw afbeelding oplossen en probeert te maken van uw siteverzameling opnieuw.



## <a name="does-your-vnet-have-network-security-groups-defined"></a>Heeft uw VNET netwerk beveiligingsgroepen gedefinieerd? ##
Als er netwerk beveiligingsgroepen gedefinieerd op het subnet die u voor uw siteverzameling gebruikt, zorg er dan voor dat deze [URL's en -poorten](remoteapp-ports.md) zijn toegankelijk vanaf binnen uw subnet.

U kunt extra netwerk beveiligingsgroepen toevoegen aan de VMs geïmplementeerd door u in het subnet voor meer controle.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Gebruikt u uw eigen DNS-servers? En zijn ze toegankelijk zijn vanuit uw subnet, VNET? ##
>[AZURE.NOTE] U moet Controleer of dat de DNS-servers in uw VNET zijn altijd omhoog en altijd kunnen de virtuele machines gehost in het VNET oplossen. Gebruik geen Google DNS voor dit.


Voor hybride verzamelingen gebruikt u uw eigen DNS-servers. U ze in uw netwerk configuratieschema of via de beheerportal wanneer u uw virtuele netwerk maakt. DNS-servers worden gebruikt in de volgorde waarin ze zijn opgegeven in een failover wijze (in plaats van round robin).  
Raadpleeg [Naamresolutie voor VMs en rol exemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) om ervoor te zorgen dat uw DNS-servers zijn geconfigureerd correcly.

Controleer of de DNS-servers voor uw siteverzameling toegankelijk zijn en verkrijgbaar via het subnet VNET die u hebt opgegeven voor deze siteverzameling.

Bijvoorbeeld:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Uw DNS-definiëren](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Gebruikt u een Active Directory-domeincontroller in uw siteverzameling? ##
Momenteel alleen één Active Directory-domein gekoppeld aan Azure RemoteApp worden kan. De verzameling hybride ondersteunt alleen Azure Active Directory-accounts die zijn gesynchroniseerd met de functie DirSync vanuit een Windows Server Active Directory-implementatie; specifiek, hetzij gesynchroniseerd met de optie Wachtwoordsynchronisatie of gesynchroniseerd met Active Directory Federation Services (AD FS) Federatie geconfigureerd. U moet maken van een aangepast domein die overeenkomt met het domein UPN-achtervoegsel voor uw domein on-premises implementatie en directory-integratie instellen.

Zie [Active Directory voor Azure RemoteApp configureren](remoteapp-ad.md) voor meer informatie.

Controleer of de domeindetails opgegeven geldig zijn en de domeincontroller is bereikbaar vanuit de VM in het subnet gebruikt voor externe Azure-App hebt gemaakt. Zorg ook dat deze service accountreferenties machtigingen computers toevoegen aan de meegeleverde domein en dat de naam van de AD verstrekt opgelost zijn van de DNS-records die beschikbaar zijn in de VNET worden kan.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Welke domeinnaam hebt u opgegeven bij het maken van uw siteverzameling? ##

De domeinnaam die u hebt gemaakt of toegevoegd moet een interne domeinnaam (niet Azure AD naam van uw domein) en moet omgezet DNS-indeling (contoso.local). Bijvoorbeeld: u hebt een interne Active Directory-naam (contoso.local) en een Active Directory-UPN (contoso.com): u moet de naam van de interne gebruiken bij het maken van uw siteverzameling.
