<properties
   pageTitle="Systeemvereisten voor StorSimple | Microsoft Azure"
   description="Een beschrijving van de software, netwerken, en beschikbaarheid vereisten en aanbevolen procedures voor een Microsoft Azure StorSimple-oplossing."
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
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>StorSimple software, beschikbaarheid en netwerken vereisten

## <a name="overview"></a>Overzicht

Welkom bij Microsoft Azure StorSimple. In dit artikel worden de systeemvereisten voor belangrijke en aanbevolen procedures voor uw apparaat StorSimple en voor de opslag-clients toegang krijgen tot het apparaat. We raden u de gegevens zorgvuldig door voordat u uw systeem StorSimple implementeren, en klik vervolgens verwijs terug naar deze zo nodig tijdens de implementatie en volgende bewerking.

Er zijn de systeemvereisten:

- **Softwarevereisten voor clients voor opslag** - beschrijving van de ondersteunde besturingssystemen en eventuele aanvullende vereisten voor deze besturingssystemen.
- **Vereisten voor het apparaat StorSimple netwerkproblemen** - vindt u informatie over de poorten die moet worden geopend in uw firewall toe te staan dat voor iSCSI, cloud of management-verkeer is toegestaan.
- **Beschikbaarheid vereisten voor StorSimple** - beschrijving van beschikbaarheid vereisten en aanbevolen procedures voor uw StorSimple apparaat en host de computer. 


## <a name="software-requirements-for-storage-clients"></a>Softwarevereisten voor opslag-clients

De volgende softwarevereisten zijn bedoeld voor de opslag-clients die toegang uw StorSimple-apparaat tot.

| Ondersteunde besturingssystemen | Vereiste versie | Aanvullende vereisten/notities |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012, 2012R2 |StorSimple iSCSI-volumes worden ondersteund voor gebruik op alleen de volgende Windows schijftypen:<ul><li>Eenvoudig volume op eenvoudige schijf</li><li>Eenvoudige en gespiegeld volume op dynamische schijf</li></ul>Windows Server 2012 dunne inrichting en ODX-functies worden ondersteund als u een StorSimple iSCSI-volume gebruikt.<br><br>StorSimple kunt maken dun ingerichte en volledig ingerichte volumes. Deze maken geen gedeeltelijk ingerichte volumes.<br><br>Een dun ingerichte volume geformatteerd duurt erg lang. Het is raadzaam om het volume verwijderen en vervolgens een nieuwe record in plaats van geformatteerd te maken. Echter liever als u nog een volume opmaak te wijzigen:<ul><li>Voer de volgende opdracht voordat de herformatteren ruimte terug vertragingen vermijden: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Nadat u de opmaak die is voltooid, gebruikt u de volgende opdracht ruimte terug opnieuw inschakelen:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>De Windows Server 2012-hotfix toepassen, zoals is beschreven in [KB 2878635](https://support.microsoft.com/kb/2870270) naar uw computer Windows Server.</li></ul></li></ul></ul> Als u StorSimple momentopname Manager of StorSimple Adapter voor SharePoint configureert, gaat u naar de [softwarevereisten voor optionele onderdelen](#software-requirements-for-optional-components).|
| VMWare ESX | 5.5 en 6.0 | Ondersteund met VMWare vSphere als iSCSI-client. VAAI-blok functie wordt ondersteund met VMware vSphere op StorSimple apparaten.
| Linux RHEL/CentOS | 5, 6 en 7 | Ondersteuning voor Linux iSCSI-clients met openen-iSCSI initiator versies 5, 6 en 7. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] IBM AIX wordt momenteel niet ondersteund met StorSimple.

## <a name="software-requirements-for-optional-components"></a>Softwarevereisten voor optionele onderdelen

De volgende softwarevereisten zijn bedoeld voor de optionele onderdelen StorSimple (StorSimple momentopname Manager en StorSimple Adapter voor SharePoint).

| Onderdeel | Hostplatform | Aanvullende vereisten/notities |
| --------------------------- | ---------------- | ------------- |
| StorSimple momentopname Manager | Windows Server 2008R2 SP1, 2012, 2012R2 | Het gebruik van StorSimple momentopname Manager op Windows Server is vereist voor back-up of het herstellen van gespiegeld dynamische schijven en voor elke toepassing consistente back-ups.<br> StorSimple momentopname Manager wordt alleen ondersteund op Windows Server 2008 R2 SP1 (64-bits), Windows 2012 R2 en Windows Server 2012.<ul><li>Als u venster Server 2012 gebruikt, moet u .NET 3.5 – 4.5 installeren voordat u StorSimple momentopname Manager installeren.</li><li>Als u Windows Server 2008 R2 SP1 gebruikt, moet u Windows Management Framework 3.0 installeren voordat u StorSimple momentopname Manager installeren.</li></ul> |
| StorSimple Adapter voor SharePoint | Windows Server 2008R2 SP1, 2012, 2012R2 |<ul><li>StorSimple Adapter voor SharePoint wordt alleen ondersteund in SharePoint 2010 Experience en SharePoint 2013.</li><li>Resourcestructuur is vereist voor SQL Server Enterprise Edition, versie 2008 R2- of 2012.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>Vereisten voor uw apparaat StorSimple toegang

Uw apparaat StorSimple is een vergrendelde. Echter moeten poorten worden geopend in uw firewall om te staan voor iSCSI, cloud en beheer van verkeer. De volgende tabel bevat de poorten die moeten worden geopend in uw firewall. In deze tabel verwijst *in* of *inkomende* naar de richting van waaruit verzoeken voor oproepen client toegang uw apparaat tot. *Af* of *Uitgaand* verwijst naar de richting waarin uw apparaat StorSimple gegevens extern, voorbij de implementatie stuurt: bijvoorbeeld uitgaande met Internet.

| Poort Nee.-<sup>1,2</sup> | In- of uitzoomen | Poort bereik | Vereist | Notities |
|------------------------|-----------|------------|----------|-------|
|TCP 80 (HTTP)<sup>3</sup>|  Out |  WAN | Nee |<ul><li>Uitgaande poort wordt gebruikt voor toegang tot Internet om op te halen updates.</li><li>De uitgaande webproxy is gebruiker worden geconfigureerd.</li><li>Als u wilt toestaan systeemupdates, moet deze poort ook zijn geopend voor de controller vaste IP-adressen.</li></ul> |
|TCP 443 (HTTPS)<sup>3</sup>| Out | WAN | Ja |<ul><li>Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud.</li><li>De uitgaande webproxy is gebruiker worden geconfigureerd.</li><li>Als u wilt toestaan systeemupdates, moet deze poort ook zijn geopend voor de controller vaste IP-adressen.</li><li>Deze poort wordt ook gebruikt op beide de domeincontrollers opschonen.</li></ul>|
|UDP 53 (DNS) | Out | WAN | In sommige gevallen; Zie Opmerkingen. |Deze poort is vereist alleen als u een Internet gebaseerde DNS-server gebruikt. |
| UDP 123 (NTP) | Out | WAN | In sommige gevallen; Zie Opmerkingen. |Deze poort is vereist alleen als u een Internet gebaseerde NTP-server gebruikt. |
| TCP 9354 | Out | WAN | Ja |De uitgaande poort wordt gebruikt door het apparaat StorSimple om te communiceren met de service StorSimple Manager. |
| 3260 (iSCSI) | In | LAN | Nee | Deze poort wordt gebruikt voor toegang tot gegevens over iSCSI.|
| 5985 | In | LAN | Nee | Binnenkomende poort wordt door StorSimple momentopname Manager gebruikt om te communiceren met het apparaat StorSimple.<br>Deze poort wordt ook gebruikt wanneer u extern verbinding met Windows PowerShell voor StorSimple via HTTP maken. |
| 5986 | In | LAN | Nee | Deze poort wordt gebruikt wanneer u extern verbinding met Windows PowerShell voor StorSimple via HTTPS maken. |

<sup>1</sup> geen binnenkomende poorten moeten worden geopend op de openbare Internet.

<sup>2</sup> als meerdere poorten een gatewayconfiguratie uitvoeren, de volgorde uitgaand routering verkeer wordt bepaald op basis van de poort routeren volgorde in [poort routering](#routing-metric), hieronder beschreven.

<sup>3</sup> de controller vaste IP-adressen op uw apparaat StorSimple moet zijn geschikt en verbinding maken met Internet. De vaste IP-adressen worden gebruikt voor het onderhoud van de updates bij het apparaat. Apparaten verbinding maken met Internet via de vaste IP-adressen, is het niet mogelijk bij uw apparaat StorSimple.

> [AZURE.IMPORTANT] Zorg ervoor dat de firewall niet wijzigen of een SSL-verkeer tussen de StorSimple apparaat en Azure ontsleutelen.

### <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor firewallregels

Netwerkbeheerders kunnen vaak geavanceerde firewallregels op basis van de URL-patronen voor het filteren van het inkomende en uitgaand verkeer configureren. Uw apparaat StorSimple en de StorSimple Manager-service, is afhankelijk van andere Microsoft-toepassingen, zoals Azure Service Bus, Azure Active Directory-toegangsbeheer opslag accounts en Microsoft Update-servers. De URL-patronen die is gekoppeld aan deze toepassingen kunnen worden gebruikt voor het configureren van firewallregels. Het is belangrijk om te begrijpen dat de URL-patronen die is gekoppeld aan deze toepassingen kunnen wijzigen. Dit vereist om de netwerkbeheerder bewaken en firewallregels bijwerken voor uw StorSimple als en als dat nodig is.

Het is raadzaam om uw firewallregels voor uitgaand verkeer, op basis van vaste IP-adressen, opneemt in de meeste gevallen StorSimple in te stellen. U kunt echter de onderstaande informatie voor het instellen van geavanceerde firewallregels die nodig zijn voor het maken van beveiligde omgevingen.

> [AZURE.NOTE] Het apparaat (bron) IP-adressen moet altijd worden ingesteld op alle ingeschakelde netwerkinterfaces. De bestemming IP-adressen moet worden ingesteld op [Azure datacenter IP-bereiken](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).

#### <a name="url-patterns-for-azure-portal"></a>URL-patronen voor Azure-portal
| URL-patroon                                                      | Onderdeel/functionaliteit                                           | Apparaat IP-adressen                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple Manager-service<br>Access Control-Service<br>Azure-Service Bus| Via interfaces zoals cloud-netwerk        |
|`https://*.backup.windowsazure.com`|Apparaatregistratie| Alleen gegevens 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Certificaatintrekkingslijsten |Via interfaces zoals cloud-netwerk |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure opslag accounts en bewaken | Via interfaces zoals cloud-netwerk        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-servers<br>                             | Controller vaste IP-adressen alleen               |
| `http://*.deploy.akamaitechnologies.com`                         |CDN Akamai |Controller vaste IP-adressen alleen   |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-pakket                                                  | Via interfaces zoals cloud-netwerk        |

#### <a name="url-patterns-for-azure-government-portal"></a>URL-patronen voor de overheid Azure-portal
| URL-patroon                                                      | Onderdeel/functionaliteit                                           | Apparaat IP-adressen                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | StorSimple Manager-service<br>Access Control-Service<br>Azure-Service Bus| Via interfaces zoals cloud-netwerk        |
|`https://*.backup.windowsazure.us`|Apparaatregistratie| Alleen gegevens 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Certificaatintrekkingslijsten |Via interfaces zoals cloud-netwerk |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure opslag accounts en bewaken | Via interfaces zoals cloud-netwerk        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-servers<br>                             | Controller vaste IP-adressen alleen               |
| `http://*.deploy.akamaitechnologies.com`                         |CDN Akamai |Controller vaste IP-adressen alleen   |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-pakket                                                  | Via interfaces zoals cloud-netwerk        |

### <a name="routing-metric"></a>Routering

Een routeren meting is gekoppeld aan de interfaces en de gateway die de gegevens doorsturen naar de opgegeven netwerken. Routering wordt gebruikt door het protocol voor het berekenen van het beste pad naar een opgegeven bestemming, als deze achterhaalt dat meerdere paden bestaan naar dezelfde bestemming. De laagste de routering meetwaarde, hoe hoger de voorkeur.

In de context van StorSimple, als meerdere netwerkinterfaces en gateways zijn geconfigureerd voor het kanaal-verkeer is toegestaan, de doelstellingen van de routering wordt een rol spelen om te bepalen de relatieve volgorde waarin de interfaces gewend. De doelstellingen van de routering kan niet worden gewijzigd door de gebruiker. U kunt echter de `Get-HcsRoutingTable` cmdlet om af te drukken de routering tabel (en de doelstellingen) op uw apparaat StorSimple. Meer informatie over de cmdlet Get-HcsRoutingTable in [StorSimple probleemoplossing-implementatie](storsimple-troubleshoot-deployment.md).

De routering metrische algoritmen verschillen afhankelijk van de versie van de software op uw apparaat StorSimple uitgevoerd.

**Eerdere versies dan Update 1**

Dit geldt ook voor softwareversies vóór Update 1 zoals de GA, 0,1, 0,2 of 0,3 release. De volgorde op basis van de doelstellingen van de routering is als volgt:

   *Laatst geconfigureerd 10 GbE netwerkinterface > andere 10 GbE netwerkinterface > laatst geconfigureerd 1 GbE netwerkinterface > andere 1 GbE netwerkinterface*


**Versies starten vanuit Update 1 en vóór Update 2**

Dit geldt ook voor de versies van de software zoals 1, 1.1 of 1.2. De volgorde op basis van routeren gegevens wordt als volgt bepaald:

   *GEGEVENS 0 > laatst geconfigureerd 10 GbE netwerkinterface > andere 10 GbE netwerkinterface > laatst geconfigureerd 1 GbE netwerkinterface > andere 1 GbE netwerkinterface*

   In Update 1, de routering meetwaarde gegevens 0 bestaat uit de de laagste; Daarom is alle cloud-verkeer gerouteerd via gegevens 0. Maak een notitie hiervan als er meer dan één cloud ingeschakelde netwerkinterface op uw apparaat StorSimple.


**Versies vanaf Update 2**

Update 2 verschillende verbeteringen van de netwerken betrekking heeft en de doelstellingen van de routering is gewijzigd. Het gedrag kunt als volgt worden toegelicht.

- Een verzameling vooraf ingestelde waarden zijn toegewezen aan via netwerkinterfaces zoals.   

- Kijk eens naar een van de voorbeeldtabel hieronder wordt weergegeven met waarden die zijn toegewezen aan de verschillende netwerkinterfaces wanneer deze zijn cloud-ingeschakeld of cloud-uitgeschakeld, maar met een geconfigureerde gateway. Houd rekening met de waarden die zijn toegewezen hier zijn alleen voorbeeldwaarden.


  	| Netwerkinterface | Cloud ingeschakelde | Cloud-uitgeschakeld met gateway |
  	|-----|---------------|---------------------------|
  	| Gegevens 0  | 1            | -                        |
  	| Gegevens 1  | 2            | 20                       |
  	| Gegevens 2  | 3            | 30                       |
  	| Gegevens 3  | 4            | 40                       |
  	| Gegevens 4  | 5            | 50                       |
  	| Gegevens 5  | 6            | 60                       |


- De volgorde waarin de cloud-verkeer worden gerouteerd via het netwerkinterfaces luidt als volgt:

    *Gegevens 0 > gegevens 1 > datum 2 > gegevens 3 > gegevens 4 > gegevens 5*

    Dit kan door het volgende voorbeeld worden uitgelegd.

    Houd rekening met een apparaat StorSimple met twee cloud ingeschakelde netwerkinterfaces, gegevens 0 en gegevens 5. Gegevens 1 t/m 4 van de gegevens zijn cloud-uitgeschakeld maar wel een geconfigureerde gateway. De volgorde waarin verkeer worden gerouteerd voor dit apparaat is:

    *Gegevens 0 (1) > gegevens 5 (6) > gegevens 1 (20) > gegevens 2 (30) > gegevens 3 (40) > gegevens 4 (50)*

    *waar is de getallen tussen haakjes geven aan de doelstellingen van de desbetreffende routeren.*

    Als gegevens 0 is mislukt, wordt het verkeer cloud tot en met 5 van de gegevens worden gerouteerd. Gezien het feit dat een gateway is geconfigureerd in alle andere netwerk, alsof zowel gegevens 0 en 5 van gegevens mislukt, wordt het verkeer cloud leest u over gegevens 1.


- Als een cloud ingeschakelde-netwerkinterface mislukt, klikt u vervolgens zijn 3 nieuwe pogingen met een 30 tweede vertraging verbinding maken met de interface. Als alle pogingen mislukt, wordt het verkeer wordt doorgestuurd naar de volgende beschikbare cloud ingeschakelde interface volgens de routering tabel. Als alle cloud ingeschakelde netwerk interfaces fail, klikt u vervolgens het apparaat, mislukt boven aan de andere controller (opnieuw opstarten niet in dit geval).

- Als er een fout VIP voor een netwerkinterface iSCSI ingeschakelde, wordt er 3 nieuwe pogingen met een vertraging 2 seconden ingedrukt. Dit probleem is gebleven hetzelfde uit de vorige versies. Als alle iSCSI netwerkinterfaces mislukt, wordt een failover controller (vergezeld van opnieuw opstarten) plaatsvinden.


- Een waarschuwing treedt ook op uw apparaat StorSimple wanneer er een fout VIP. Ga naar de [Waarschuwing Snelzoekgids](storsimple-manage-alerts.md)voor meer informatie.

- Met nieuwe pogingen, heeft iSCSI voorrang boven cloud.

    Houd rekening met het volgende voorbeeld: A StorSimple apparaat twee netwerkinterfaces ingeschakeld heeft, gegevens 0 en 1 van de gegevens. Gegevens 0 is cloud ingeschakelde dat gegevens 1 beide cloud en iSCSI is ingeschakeld. Geen andere netwerkinterfaces op dit apparaat zijn ingeschakeld voor de cloud of iSCSI.

    Als 1 van gegevens mislukt?, gegeven dit is de laatste iSCSI netwerk-interface, resulteert dit in een failover controller-1 van de gegevens op de andere controller uit.


### <a name="networking-best-practices"></a>Aanbevolen procedures voor netwerken

Naast de bovenstaande netwerken vereisten voor de optimale prestaties van uw oplossing StorSimple, neemt u voldoen aan de volgende aanbevolen procedures:

- Zorg ervoor dat uw apparaat StorSimple over een speciale 40 Mbps bandbreedte (of meer) beschikbaar te allen tijde. Deze bandbreedte mag niet worden gedeeld (of toewijzing moet worden gewaarborgd met behulp van QoS-beleid) met andere toepassingen.

- Controleer netwerkconnectiviteit met Internet allen tijde beschikbaar is. Enkel geval of onbetrouwbare Internet-verbindingen met de apparaten, waaronder geen internetconnectiviteit dan ook is, resulteert in een niet-ondersteunde configuratie.

- Isoleren het iSCSI en cloud-verkeer door via netwerkinterfaces zoals op uw apparaat voor iSCSI en cloud toegang hebt toegewezen. Zie voor meer informatie het [wijzigen van netwerkinterfaces](storsimple-modify-device-config.md#modify-network-interfaces) op uw apparaat StorSimple.

- Een koppeling aggregatie Control Protocol (LACP)-configuratie niet gebruiken voor uw netwerkinterfaces. Dit is een niet-ondersteunde configuratie.


## <a name="high-availability-requirements-for-storsimple"></a>Beschikbaarheid vereisten voor StorSimple

Het hardwareplatform die deel uitmaakt van de oplossing StorSimple heeft beschikbaarheid en betrouwbaarheid functies die kunnen worden gebruikt voor de infrastructuur van een zeer beschikbaar, fouttolerantie opslagruimte in uw datacenter. Er zijn echter vereisten en aanbevolen procedures die u moet voldoen aan Richtlijn om de beschikbaarheid van uw oplossing StorSimple. Voordat u StorSimple implementeert, zorgvuldig door de volgende vereisten en aanbevolen procedures voor het StorSimple apparaat en verbonden host-computers.

Ga voor meer informatie over het controleren en onderhouden van de hardwareonderdelen van uw apparaat StorSimple naar [Gebruik de StorSimple Manager-service om de hardwareonderdelen en de status te houden](storsimple-monitor-hardware-status.md) en [StorSimple hardware onderdeel vervangende](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Beschikbaarheid vereisten en procedures voor uw apparaat StorSimple

Controleer de volgende gegevens zorgvuldig door om ervoor te zorgen de beschikbaarheid van uw apparaat StorSimple.

#### <a name="pcms"></a>PCMs

StorSimple apparaten zijn redundante, direct-verwisselbare power en koelingsmodules (PCMs). Elke PCM heeft onvoldoende capaciteit om service voor het hele chassis te leveren. Om ervoor te zorgen beschikbaarheid, moeten beide PCMs zijn geïnstalleerd.

- Uw PCMs verbinden met verschillende power bronnen op te geven van beschikbaarheid als een bron power mislukt.
- Als een PCM mislukt, vraagt u een vervangende direct in.
- Verwijder een mislukte PCM alleen wanneer u bent klaar om het te installeren en de vervangende hebt.
- Verwijder beide PCMs niet gelijktijdig. De module PCM bevat de back-kleine-module. Een afsluiten zonder kleine beveiliging verwijdering beide van de PCMs leidt en de status van het apparaat niet worden opgeslagen. Ga naar [behouden de back-kleine-module](storsimple-battery-replacement.md#maintain-the-backup-battery-module)voor meer informatie over het kleine.

#### <a name="controller-modules"></a>Controller modules

StorSimple apparaten zijn redundante, direct-verwisselbare controller modules. De controller-modules werken in een actieve/passieve wijze. Op elk gewenst één controllermodule actief is en verleent services, terwijl de andere controllermodule passieve is. De controllermodule passieve is ingeschakeld en operationele wordt als de actieve controllermodule mislukt of is verwijderd. Elke controllermodule heeft onvoldoende capaciteit om service voor het hele chassis te leveren. Beide modules controller moeten zijn geïnstalleerd om ervoor te zorgen beschikbaarheid.

- Zorg ervoor dat beide modules controller allen tijde zijn geïnstalleerd.

- Als een controllermodule mislukt, vraagt u een vervangende direct in.

- Verwijder een controllermodule mislukte alleen wanneer u bent klaar om het te installeren en de vervangende hebt. Een module verwijderen voor langere is van invloed op de luchtstroom en dus het koelen van het systeem.

- Zorg ervoor dat de netwerkverbindingen aan beide modules controller identiek zijn, en de verbonden netwerk-interfaces een identieke netwerkconfiguratie hebben.

- Als een controllermodule mislukt of vervangende moet, zorg ervoor dat de andere controllermodule zich in een actieve status voordat de controllermodule mislukte vervangen. Om te bevestigen dat een controller actief is, gaat u naar [de actieve controller op uw apparaat identificeren](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Verwijder niet beide modules controller op hetzelfde moment. Als een failover controller uitgevoerd wordt, niet de stand-by controllermodule afgesloten of verwijderen uit het chassis.

- Na een failover controller, wacht u ten minste vijf minuten voordat u verwijdert een van beide controllermodule.

#### <a name="network-interfaces"></a>Netwerkinterfaces

StorSimple apparaat controller modules elke hebt vier 1 Gigabit en twee 10 Gigabit Ethernet-netwerkinterfaces.

- Controleer of de netwerkverbindingen aan beide modules controller identiek zijn, en het netwerk interfaces dat de controller module interfaces als u wilt dat een identieke netwerkconfiguratie bent verbonden.

- Indien mogelijk, moet u Netwerkverbindingen installeren in verschillende schakelopties zodat deze zeker service beschikbaar in het geval van een netwerk apparaat is mislukt.

- Wanneer de enige of de laatste resterende iSCSI ingeschakelde-interface ontkoppelen (met IP-adressen die zijn toegewezen), de interface eerst uitschakelen en haal de kabels. Als de interface eerst losgekoppeld is, leidt dit het actieve controller mislukt boven aan de passieve controller. Als de passieve controller ook de bijbehorende interfaces losgekoppeld heeft, vervolgens beide de controllers wordt opnieuw opstarten meerdere keren voordat op één controller.

- Sluit ten minste twee DATA-interfaces bij het netwerk op elke controllermodule.

- Als u de twee hebt ingeschakeld 10 GbE-interfaces, die over verschillende schakelopties implementeren.

- Gebruik indien mogelijk MPIO op servers om ervoor te zorgen dat de servers mogelijk zonder een koppeling, netwerk of interface is mislukt.

Ga naar de [installatie van uw apparaat StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) of [uw apparaat StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device)uw apparaat gebruiken voor beschikbaarheid en prestaties, voor meer informatie over netwerken.

#### <a name="ssds-and-hdds"></a>SSD en harde schijven

StorSimple apparaten zijn effen staat schijven (SSD) en vaste schijven (harde schijven) die zijn beveiligd met spaties gespiegeld weergegeven. Gebruik van gespiegeld spaties zorgt ervoor dat het apparaat is bestand tegen een fout in een of meer SSD of harde schijven.

- Zorg ervoor dat alle SSD en harde schijf modules zijn geïnstalleerd.

- Als een SSD of de harde schijf mislukt, vraagt u een vervangende direct in.

- Als een SSD of de harde schijf mislukt of moet worden vervangen, zorg er dan voor dat alleen de SSD of de harde schijf die moet worden vervangen te verwijderen.

- Verwijder niet meer dan één SSD of harde schijf uit het systeem op een willekeurige plaats in de tijd.
Een storing van 2 of meer schijven van een bepaald type (harde schijf, SSD) of de opeenvolgende mislukt binnen een korte tijd kan dit leiden tot systeem storing en potentieel gegevensverlies. Als dit gebeurt, [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor hulp.

- Tijdens de vervangende, de **Status van de Hardware** in **de onderhoudspagina voor de stations in de SSD en harde schijven** te controleren. De status van een groen vinkje geeft aan dat de schijven zijn in orde of OK, terwijl wijst u een rood uitroepteken geeft aan dat een mislukte SSD of harde schijf.

- Het is raadzaam dat u cloud momentopnamen voor alle volumes die u nodig hebt om te beveiligen in geval van een systeemfouten configureren.

#### <a name="ebod-enclosure"></a>EBOD insluiting

StorSimple Apparaatmodel 8600 bevat een uitgebreide bundel van schijven (EBOD) insluiting behalve de primaire insluiting. Een EBOD EBOD reservedomeincontrollers bevat en vaste schijven (harde schijven) die zijn beveiligd met spaties gespiegeld weergegeven. Gebruik van gespiegeld spaties zorgt ervoor dat het apparaat is bestand tegen de fout een of meer harde schijven. De ruimte EBOD is verbonden met de primaire insluiting tot en met overtollige SA's kabels.

- Zorg ervoor dat beide EBOD insluiting controller modules, zowel SA's kabels en alle de vaste schijven allen tijde zijn geïnstalleerd.

- Als een module EBOD insluiting controller mislukt, vraagt u een vervangende direct in.

- Als een module EBOD insluiting controller mislukt, zorg dat de andere controllermodule actief is voordat u de mislukte module vervangen. Om te bevestigen dat een controller actief is, gaat u naar [de actieve controller op uw apparaat identificeren](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Tijdens een EBOD controller module vervanging, continu bewaken de status van het onderdeel in de StorSimple Manager-service via **onderhoud** > **hardwarestatus**.

- Als een kabel SA's mislukt of moet worden vervangen (Microsoft Support moet worden gebruikt om een dergelijke bepalen), zorg dat u alleen de SA's-kabel die moet worden vervangen het verwijdert.

- Niet gelijktijdig Verwijder beide kabels SA's uit het systeem op elk gewenst moment in tijd.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Beschikbaarheid aanbevelingen voor uw hostcomputers

Zorgvuldig door deze aanbevolen procedures om ervoor te zorgen de beschikbaarheid van hosts die zijn verbonden met uw apparaat StorSimple.

- StorSimple configureren met [twee knooppunten bestand server clusterconfiguraties][1]. Potentiële is mislukt en het bouwen van in redundantie aan de host verwijdert, wordt de volledige oplossing ten zeerste beschikbaar.

- Gebruik continu beschikbaar (CA) deelt beschikbaar met Windows Server 2012 (SMB 3.0) voor beschikbaarheid tijdens een overname van de opslagcontrollers. Voor aanvullende informatie voor het configureren van bestand serverclusters en waarden voor aandelen continu beschikbaar met Windows Server 2012, raadpleegt u deze [video demo](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Volgende stappen

- [Algemene informatie over limieten voor StorSimple-systeem](storsimple-limits.md).
- [Leer hoe u uw StorSimple-oplossing implementeert](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
