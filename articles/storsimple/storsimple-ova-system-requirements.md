<properties
   pageTitle="Systeemvereisten voor StorSimple virtuele matrix"
   description="Meer informatie over de software en netwerken vereisten voor uw virtuele StorSimple-matrix"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/17/2016"
   ms.author="alkohli"/>

# <a name="storsimple-virtual-array-system-requirements"></a>Systeemvereisten voor StorSimple virtuele matrix

## <a name="overview"></a>Overzicht

In dit artikel worden belangrijke systeemvereisten voor uw Microsoft Azure StorSimple virtuele matrix (ook wel bekend als StorSimple on-premises implementatie virtueel apparaat of StorSimple virtueel apparaat) en voor de opslag-clients toegang krijgen tot de matrix. We raden u de gegevens zorgvuldig door voordat u uw systeem StorSimple implementeren, en klik vervolgens verwijs terug naar deze zo nodig tijdens de implementatie en volgende bewerking.

Er zijn de systeemvereisten:

-   **Softwarevereisten voor clients voor opslag** - beschrijving van de ondersteunde virtualization platforms, webbrowsers, iSCSI-initiators, SMB-clients, minimale virtueel apparaat vereisten en eventuele aanvullende vereisten voor deze besturingssystemen.

-   **Vereisten voor het apparaat StorSimple netwerkproblemen** - vindt u informatie over de poorten die moet worden geopend in uw firewall toe te staan dat voor iSCSI, cloud of management-verkeer is toegestaan.

StorSimple systeem informatie over de vereisten die zijn gepubliceerd in dit artikel is alleen van toepassing op StorSimple virtuele matrices.

- Ga naar de [systeemvereisten voor uw apparaat van de reeks StorSimple 8000](storsimple-system-requirements.md)voor 8000 reeks apparaten.
 
- Ga naar de [systeemvereisten voor uw apparaat StorSimple 5000-7000 reeks](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements)voor 7000 reeks apparaten.


## <a name="software-requirements"></a>Softwarevereisten

De softwarevereisten bevatten de informatie over de ondersteunde webbrowsers en SMB-versies, virtualization platforms en de minimale virtueel apparaat vereisten.

### <a name="supported-virtualization-platforms"></a>Ondersteunde virtualization platforms


| **Hypervisor** | **Versie**                          |
|----------------|--------------------------------------|
| Hyper-V        | Windows Server 2008 R2 SP1 en hoger |
| VMware ESXi    | 5,5 en hoger                        |

### <a name="virtual-device-requirements"></a>Vereisten voor virtueel apparaat

| **Onderdeel**                                | **Vereiste**            |
|----------------------------------------------|----------------------------|
| Minimum aantal virtuele processors (cores) | 4                          |
| Minimale geheugen (RAM)                         | 8 GB                       |
| Schijfruimte<sup>1</sup>                       | OS schijf - 80 GB <br></br>Gegevensschijf - 500 GB tot 8 TB|
| Minimum aantal netwerk-interfaces       | 1                          |
| Minimale internetbandbreedte<sup>2</sup>       | 5 Mbps                     |

<sup>1</sup> - dun deze is ingericht

<sup>2</sup> - netwerkvereisten, is afhankelijk van de snelheid van het dagelijkse gegevens wijzigen. Bijvoorbeeld als een apparaat back-up van 10 GB of meer wijzigingen tijdens een dag moet, kan klikt u vervolgens de dagelijkse back-up maken via een 5 Mbps verbinding krijgen tot 4,25 uur (als de gegevens kan niet worden gecomprimeerd of deduplicated).

### <a name="supported-web-browsers"></a>Ondersteunde webbrowsers

| **Onderdeel**     | **Versie** | **Aanvullende vereisten/notities** |
|-------------------|-----------------|-----------------------------------|
| Microsoft Edge    | Meest recente versie  |                                   |
| Internet Explorer | Meest recente versie  | Getest met Internet Explorer 11  |
| Google Chrome     | Meest recente versie  | Getest met Chrome 46             |

### <a name="supported-storage-clients"></a>Ondersteunde opslag-clients 

De volgende softwarevereisten zijn bedoeld voor de iSCSI-initiators die toegang uw StorSimple virtuele matrix tot (geconfigureerd als een iSCSI-server).

| **Ondersteunde besturingssystemen** | **Vereiste versie** | **Aanvullende vereisten/notities** |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012, 2012R2 |StorSimple kunt maken dun ingerichte en volledig ingerichte volumes. Deze maken geen gedeeltelijk ingerichte volumes. StorSimple iSCSI-volumes worden alleen ondersteund voor: <ul><li>Eenvoudige volumes op Windows standaard schijven.</li><li>Windows NTFS voor het opmaken van een volume.</li>|


De volgende softwarevereisten zijn voor het SMB-clients die toegang uw StorSimple virtuele matrix tot (geconfigureerd als een bestandsserver).

| **SMB-versie** |
|-------------|
| SMB 2.x     |
| SMB 3.0     |
| SMB 3.02    |

> [AZURE.IMPORTANT] Niet kopiëren of opslaan van bestanden beveiligd door Windows coderen File System (EFS) naar de server van het bestand StorSimple virtuele matrix. Hierdoor wordt in een niet-ondersteunde configuratie. 

## <a name="networking-requirements"></a>Vereisten voor netwerken 

De volgende tabel bevat de poorten die moeten worden geopend in uw firewall laat iSCSI, SMB, cloud of management-verkeer is toegestaan. In deze tabel verwijst *in* of *inkomende* naar de richting van waaruit verzoeken voor oproepen client toegang uw apparaat tot. *Af* of *Uitgaand* verwijst naar de richting waarin uw apparaat StorSimple gegevens extern, voorbij de implementatie stuurt: bijvoorbeeld uitgaande met Internet.

| **Poort Nee.<sup>1</sup>** | **In- of uitzoomen** | **Poort bereik** | **Vereist**              | **Notities**                                                                                                            |
|--------------------------|---------------|----------------|---------------------------|----------------------------------------------------------------------------------------------------------------------|
| TCP 80 (HTTP)            | Out           | WAN            | Nee                        | Uitgaande poort wordt gebruikt voor toegang tot Internet om op te halen updates. <br></br>De uitgaande webproxy is gebruiker worden geconfigureerd. |
| TCP 443 (HTTPS)          | Out           | WAN            | Ja                       | Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud. <br></br>De uitgaande webproxy is gebruiker worden geconfigureerd. |
| UDP 53 (DNS)             | Out           | WAN            | In sommige gevallen; Zie Opmerkingen. | Deze poort is vereist alleen als u een Internet gebaseerde DNS-server gebruikt. <br></br> **Opmerking**: als een bestandsserver implementeert, wordt aangeraden het gebruik van de lokale DNS-server.|
| UDP 123 (NTP)            | Out           | WAN            | In sommige gevallen; Zie Opmerkingen. | Deze poort is vereist alleen als u een Internet gebaseerde NTP-server gebruikt.<br></br> **Notitie:** Als een bestandsserver wordt geïmplementeerd, wordt u aangeraden tijd synchroniseren met uw Active Directory-domeincontroller.  |
| TCP 80 (HTTP)           | In            | LAN            | Ja                       | Dit is de binnenkomende poort voor lokale UI op het apparaat StorSimple lokaal beheer. <br></br> **Opmerking**: toegang krijgen tot de lokale UI via HTTP in HTTPS automatisch leiden.|
| TCP 443 (HTTPS)          | In            | LAN            | Ja                       | Dit is de binnenkomende poort voor lokale UI op het apparaat StorSimple lokaal beheer.|
| TCP 3260 (iSCSI)         | In            | LAN            | Nee                        | Deze poort wordt gebruikt voor toegang tot gegevens over iSCSI.|

<sup>1</sup> geen binnenkomende poorten moeten worden geopend op de openbare Internet.

> [AZURE.IMPORTANT] Zorg ervoor dat de firewall niet wijzigen of een SSL-verkeer tussen de StorSimple apparaat en Azure ontsleutelen.


### <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor firewallregels 

Netwerkbeheerders kunnen vaak geavanceerde firewallregels op basis van de URL-patronen voor het filteren van het inkomende en uitgaand verkeer configureren. Uw virtuele matrix en de StorSimple Manager-service, is afhankelijk van andere Microsoft-toepassingen, zoals Azure Service Bus, Azure Active Directory-toegangsbeheer opslag accounts en Microsoft Update-servers. De URL-patronen die is gekoppeld aan deze toepassingen kunnen worden gebruikt voor het configureren van firewallregels. Het is belangrijk om te begrijpen dat de URL-patronen die is gekoppeld aan deze toepassingen kunnen wijzigen. Dit vereist om de netwerkbeheerder bewaken en firewallregels bijwerken voor uw StorSimple als en als dat nodig is. 

Het is raadzaam om uw firewallregels voor uitgaand verkeer, op basis van vaste IP-adressen, opneemt in de meeste gevallen StorSimple in te stellen. U kunt echter de onderstaande informatie voor het instellen van geavanceerde firewallregels die nodig zijn voor het maken van beveiligde omgevingen.

> [AZURE.NOTE] 
> 
> - Het apparaat (bron) IP-adressen moet altijd worden ingesteld op alle interfaces in de cloud-netwerk. 
> - De bestemming IP-adressen moet worden ingesteld op [Azure datacenter IP-bereiken](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


| URL-patroon                                                      | Onderdeel/functionaliteit                                           |
|------------------------------------------------------------------|---------------------------------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple Manager-service<br>Access Control-Service<br>Azure-Service Bus|
|`http://*.backup.windowsazure.com`|Apparaatregistratie|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Certificaatintrekkingslijsten |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure opslag accounts en bewaken |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-servers<br>                        |
| `http://*.deploy.akamaitechnologies.com`                         |CDN Akamai |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-pakket                                                  |
| `http://*.data.microsoft.com `                   | Telemetrielogboek-service in Windows, raadpleegt u de [update voor klanttevredenheid en diagnostische telemetrielogboek](https://support.microsoft.com/en-us/kb/3068708)                                                  |

## <a name="next-step"></a>Volgende stap

-   [De portal om te implementeren van uw StorSimple virtuele matrix voorbereiden](storsimple-ova-deploy1-portal-prep.md)
