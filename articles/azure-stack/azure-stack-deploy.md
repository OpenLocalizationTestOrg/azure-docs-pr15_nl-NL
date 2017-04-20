<properties
    pageTitle="Voordat u Azure stapel Haalbaarheidstest implementeren | Microsoft Azure"
    description="Bekijk de omgeving en hardware vereisten voor Azure stapel Haalbaarheidstest (service-beheerder)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Vereisten voor implementatie van Azure stapel

Controleer voordat u Azure stapel Haalbaarheidstest ([Bewijs van Concept](azure-stack-poc.md)) implementeert, of uw computer voldoet aan de volgende vereisten.
De Technical Preview-2-implementatie-vereisten voor de Haalbaarheidstest zijn hetzelfde als die vereist zijn voor Technical Preview-1. Daarom kunt u dezelfde hardware die u hebt gebruikt voor de vorige één en-klare preview.

## <a name="hardware"></a>Hardware

| Onderdeel | Minimum  | Aanbevolen |
|---|---|---|
| Stations: besturingssysteem | 1 OS schijf met minimaal 200 GB beschikbaar voor systeem partition (SSD of harde schijf) | 1 OS schijf met minimaal 200 GB beschikbaar voor systeem partition (SSD of harde schijf) |
| Stations: algemene Azure stapel Haalbaarheidstest-gegevens | 4 schijven. Elke schijf bevat ten minste 140 GB van capaciteit (SSD of harde schijf). Alle beschikbare schijven worden gebruikt. | 4 schijven. Elke schijf bevat ten minste 250 GB van capaciteit (SSD of harde schijf). Alle beschikbare schijven worden gebruikt.|
| Berekenen: processor | Twee-Socket: 12 fysieke Cores (totaal)  | Twee-Socket: 16 fysieke Cores (totaal) |
| Berekenen: geheugen | 96 GB RAM  | 128 GB RAM |
| Berekenen: BIOS | Hyper-V ingeschakeld (met SLAT-ondersteuning)  | Hyper-V ingeschakeld (met SLAT-ondersteuning) |
| Netwerk: NIC | Windows Server 2012 R2-certificering vereist voor NIC; geen speciale functies vereist | Windows Server 2012 R2-certificering vereist voor NIC; geen speciale functies vereist |
| HW logo-certificering | [Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Gegevens schijf configuratie:** Alle gegevensstations moeten van hetzelfde type (alle SA's of alle SATA) en capaciteit. Als de schijfstations SA's worden gebruikt, moet de schijfstations worden gekoppeld via één pad (geen MPIO, ondersteuning voor meerdere paden wordt geleverd).

**Configuratieopties HBA**
 
- (Aanbevolen) Eenvoudige HBA
- RAID HBA-Adapter moet worden geconfigureerd in de modus 'voldoet tot en met'
- RAID HBA – schijven moeten worden geconfigureerd als verschillende schijven, RAID-0

**Ondersteunde bus en media combinaties te typen**

-   SATA HARDE SCHIJF

-   SA 'S HARDE SCHIJF

-   RAID HARDE SCHIJF

-   RAID SSD (als het mediatype niet opgegeven/onbekend is\*)

-   SATA SSD + SATA HARDE SCHIJF

-   SA 'S SSD + SA 'S HARDE SCHIJF

\*RAID-controllers zonder Pass Through mogelijkheid herkent niet het mediatype. Deze controllers wordt zowel harde schijf en SSD markeren als niet opgegeven. In dat geval wordt de SSD worden gebruikt als permanente opslagruimte in plaats van caching van apparaten. Daarom kunt u de Microsoft Azure stapel Haalbaarheidstest op deze SSD implementeren.

**Voorbeeld HBA's**: LSI 9207 8i, LSI-9300-8i of LSI-9265-8i in Pass Through-modus

Voorbeeld OEM-configuraties zijn beschikbaar.

## <a name="operating-system"></a>Besturingssysteem

| | **Vereisten**  |
|---|---|
| **Versie van het besturingssysteem** | Windows Server 2012 R2 of hoger. De versie van het besturingssysteem is niet kritieke voordat de implementatie wordt gestart, terwijl u de computer wordt opstart met de VHD die deel van Azure stapel installatie zip uitmaakt. Het besturingssysteem en alle vereiste patches zijn al geïntegreerd in de afbeelding. Gebruik geen toetsen om alle Windows Server-exemplaren die worden gebruikt in de Haalbaarheidstest activeren.|

## <a name="deployment-requirements-check-tool"></a>Implementatievereisten controleren hulpmiddel

Na de installatie van het besturingssysteem, kunt u de [Implementatie toegankelijkheidscontrole voor Azure stapel Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) om te bevestigen dat de hardware aan alle vereisten voldoet.



## <a name="microsoft-azure-active-directory-accounts"></a>Microsoft Azure Active Directory-accounts

De implementatie van Microsoft Azure stapel Haalbaarheidstest moet worden verbonden met Azure. Daarom moet u een Microsoft Azure Active Directory-account voordat u de implementatie PowerShell-script voorbereiden. Dit account wordt de globale beheerder voor de Azure Active Directory-tenant. Deze worden gebruikt inrichten en het delegeren van toepassingen en service principes voor alle Azure stapel-services die met Azure Active Directory en afbeelding API samenwerken. Dit wordt ook gebruikt als de eigenaar van het abonnement dat standaard provider (die u kunt later wijzigen). U kunt aanmelden bij uw Azure stapel-mailsysteem-beheerportal via dit account.

1. Maak een Azure AD-account dat is de directory-beheerder voor ten minste één Azure Active Directory. Als u al een hebt, kunt u die kunt gebruiken. Anders kunt u een gratis voor [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (in China, gaat u naar <http://go.microsoft.com/fwlink/?LinkID=717821> in plaats daarvan.)

    Sla deze referenties voor gebruik in stap 6 van [de PowerShell-implementatie-script uitvoeren](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). Dit account *service-beheerder* kunt configureren en beheren van resource wolken, gebruikersaccounts tenant-abonnementen, quota en prijzen. Klik in de portal kunnen ze website wolken, VM privé wolken maken, abonnementen maken en gebruikersabonnementen beheren.

2. [Maken](azure-stack-add-new-user-aad.md) ten minste één account zodat u zich kunt aanmelden bij de Haalbaarheidstest Azure stapel als een tenant.

  	| **Azure Active Directory-account**  | **Ondersteund?** |
  	|---|---| 
  	| Werk- of schoolaccount account met geldige openbare Azure-abonnement  | Ja |
  	| Microsoft-Account met geldige openbare Azure-abonnement  | Nee |
  	| Werk- of schoolaccount account met geldige China Azure-abonnement  | Ja |
  	| Werk- of schoolaccount account met geldige ons overheid Azure-abonnement  | Ja |


## <a name="network"></a>Netwerk

### <a name="switch"></a>Schakeloptie

Een beschikbare poort op een schakeloptie voor de Haalbaarheidstest machine.  

De computer Azure stapel Haalbaarheidstest ondersteunt verbinding maakt met een access veranderen of hoofdtransmissielijn poort. Geen specialistische functies nodig op de schakeloptie. Als u een hoofdtransmissielijn poort gebruikt of als u moet een ID VLAN configureren, moet u de VLAN-ID als een parameter implementatie bieden. Hier ziet u voorbeelden in de [lijst met parameters die implementatie](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Subnet

De computer Haalbaarheidstest geen verbinding met de volgende subnetten:
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

Deze subnetten gereserveerd voor de interne netwerken binnen de stapel Haalbaarheidstest van Microsoft Azure-omgeving.

### <a name="ipv4ipv6"></a>IPv4/IPv6

Alleen IPv4 wordt ondersteund. U kunt geen IPv6-netwerken maken.

### <a name="dhcp"></a>DHCP

Controleer of er is een DHCP-server beschikbaar op het netwerk waarmee de NIC verbinding maakt. Als DHCP niet beschikbaar is, moet u een extra statische IPv4-netwerk naast het account waarmee door host voorbereiden. U moet opgeven dat IP-adres en de gateway als een parameter implementatie. Hier ziet u voorbeelden in de [lijst met parameters die implementatie](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>Internettoegang

Azure stapel vereist toegang tot Internet, rechtstreeks of via een transparante proxy. Azure stapel biedt geen ondersteuning voor de configuratie van een webproxy om in te schakelen internettoegang. Zowel de host IP- en de nieuwe IP die is toegewezen aan de MAS-BGPNAT01 (door DHCP of statische IP-) moet kunnen toegang tot Internet. Poort 80 en 443 worden onder domeinen de graph.windows.net en login.windows.net gebruikt.

### <a name="telemetry"></a>Telemetrielogboek

Als u wilt telemetrielogboek gegevensstroom wordt ondersteund, is poort 443 (HTTPS) geopend in uw netwerk. Het eindpunt van de client is https://vortex-win.data.microsoft.com.


## <a name="next-steps"></a>Volgende stappen

[Het implementatiepakket Azure stapel Haalbaarheidstest downloaden](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Azure stapel Haalbaarheidstest implementeren](azure-stack-run-powershell-script.md)
