<properties 
    pageTitle="De gebruikershandleiding Linux-Agent | Microsoft Azure" 
    description="Informatie over het installeren en configureren van Linux-Agent (waagent) als u wilt uw VM interactie met Azure stof Controller beheren." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



# <a name="azure-linux-agent-user-guide"></a>De gebruikershandleiding Azure Linux-Agent

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Inleiding

De Microsoft Azure Linux-Agent (waagent) beheert Linux en FreeBSD inrichting en VM interactie met de Controller uit Azure stof. Biedt de volgende functionaliteit voor Linux en FreeBSD IaaS implementaties:

> [AZURE.NOTE] Zie de Azure Linux-agent [Leesmij](https://github.com/Azure/WALinuxAgent/blob/master/README.md) voor meer informatie.

* **Afbeelding van de inrichting**
  - Het maken van een gebruikersaccount
  - SSH verificatietypen configureren
  - Implementatie van openbare sleutels SSH en paren sleutels
  - De hostnaam instellen
  - De hostnaam publiceren naar het DNS-platform
  - SSH host belangrijke vingerafdruk rapporteren aan het platform
  - Resourcebeheer Schijfopruiming
  - Opmaak en het koppelen van de resource-schijf
  - Omwisselen ruimte configureren

* **Netwerken**
  - Beheert waarmee de compatibiliteit met platform DHCP-servers verbeteren
  - Zorgt ervoor dat de stabiliteit van de naam van het netwerk-interface

* **Kernel**
  - Hiermee configureert u virtuele NUMA (uitschakelen voor kernel < 2.6.37)
  - Verbruikt Hyper-V entropie voor /dev/random
  - Hiermee configureert u SCSI-bewerkingen voor de hoofdmap-apparaat (dat kan worden externe)

* **Diagnostische hulpprogramma 's**
  - Consoleomleiding naar de seriële poort

* **SCVMM-implementaties**
    - Worden gedetecteerd en de VMM-agent voor Linux bootstraps wanneer u zich in een omgeving systeem Center VM Manager 2012 R2

* **VM-extensie**
    - Onderdeel waaraan wordt gewerkt door Microsoft en Partners in Linux VM (IaaS) om in te schakelen van software en configuratie automatisering invoeren
    - VM extensie verwijzing implementatie op [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)


## <a name="communication"></a>Communicatie
De informatiestroom uit het platform naar de agent deze gebeurtenis vindt plaats via twee kanalen:

* Een opstarten gekoppeld DVD voor IaaS implementaties. Deze DVD bevat een OVF-compatibele configuratiebestand met alle inrichten informatie dan de werkelijke SSH keypairs.
* Een TCP-eindpunt dat een REST API gebruikt voor implementatie en topologieconfiguratie.


## <a name="requirements"></a>Vereisten
De volgende systemen zijn getest en bekend zijn voor gebruik met de Azure Linux-Agent:

> [AZURE.NOTE] Deze lijst kan afwijken van de officiële lijst van ondersteunde systemen op Microsoft Azure Platform, zoals hier wordt beschreven: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

* CoreOS
* CentOS 6.3 +
* Rode rol Enterprise Linux 6,7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6.4 +

Andere ondersteunde systemen:

* FreeBSD 10 + (Azure Linux-Agent v2.0.10 +)

De Linux-agent is afhankelijk van bepaalde systeempakketten goed:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Bestandssysteem hulpprogramma's: sfdisk, fdisk, mkfs, gescheiden
* Hulpmiddelen voor wachtwoord: chpasswd, sudo
* Verwerking van hulpmiddelen voor tekst: sed, grep
* Hulpmiddelen voor het netwerk: ip-doorsturen
* Kernel ondersteuning voor het koppelen van UDF filesystems.

## <a name="installation"></a>Installatie
Installatie met behulp van een RPM of een pakket DEB uit van de verdeling pakket opslagplaats is de voorkeursmethode voor het installeren en upgraden van de Azure Linux-Agent. Alle de [verdeling providers goedgekeurd](virtual-machines-linux-endorsed-distros.md) integreren met het pakket van Azure Linux agent hun afbeeldingen en opslagplaatsen.

Raadpleeg de documentatie in de [Azure Linux Agent cessies‑retrocessies op Github](https://github.com/Azure/WALinuxAgent) voor Geavanceerde installatieopties, zoals het installeren van de bron of naar aangepaste locaties of voorvoegsels voor eenheden.


## <a name="command-line-options"></a>Opdrachtregelopties

### <a name="flags"></a>Markeringen

- uitgebreide: gegevens opnemen van de opgegeven opdracht vergroten
- afdwingen: interactieve bevestiging voor sommige opdrachten overslaan

### <a name="commands"></a>Opdrachten

- Help: bevat de ondersteunde opdrachten en markeringen.

- identiteitsgegevens: proberen op te schonen het systeem en deze geschikt is voor het inrichten van opnieuw te maken. Deze bewerking verwijderd het volgende:
 * Alle SSH host sleutels (als Provisioning.RegenerateSshHostKeyPair 'y' in het configuratiebestand is)
 * Configuratie van de naamserver in /etc/resolv.conf
 * Wachtwoord van de hoofdsite van/etc/shadow (als Provisioning.DeleteRootPassword 'y' in het configuratiebestand is)
 * In de cache DHCP-client lease
 * Hiermee stelt u de hostnaam localhost.localdomain


> [AZURE.WARNING] Deprovisioning kan niet garanderen dat de afbeelding uitgeschakeld van alle gevoelige informatie en geschikt is voor distributie is.


- identiteitsgegevens + gebruiker: alles onder - identiteitsgegevens (Zie hierboven) wordt uitgevoerd en ook Hiermee verwijdert u het laatste ingerichte gebruikersaccount (verkregen uit /var/lib/waagent) en gekoppelde gegevens. Deze parameter is als het verwijderen van gegevens van een afbeelding die eerder is ingericht op Azure zodat deze kan worden vastgelegd en opnieuw te gebruiken.

- versie: de versie van waagent weergeven

- serialconsole: configureert WORMGATEN als u wilt markeren ttyS0 (de eerste seriële poort) als het opstarten-console. Dit zorgt ervoor dat kernel bootup logboeken worden verzonden naar de seriële poort en beschikbaar zijn voor foutopsporing worden gemaakt.

- daemon: waagent uitvoeren als een daemon interacties met het platform beheren. Dit argument is opgegeven om te waagent in het waagent initialisatie script.

- starten: waagent achtergrond uitvoeren


## <a name="configuration"></a>Configuratie

Een configuratiebestand (/ etc/waagent.conf) de acties van waagent besturingselementen. Hieronder vindt u een voorbeeld van configuratiebestand:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

De verschillende configuratieopties worden hieronder uitvoerig beschreven. Configuratieopties zijn drie typen; Booleaanse waarde, tekenreeks of geheel getal. De Booleaanse configuratieopties kunnen worden opgegeven als "y" of "n". Het speciale trefwoord 'Geen' kan worden gebruikt voor enkele tekenreeks type configuratie posten als gedetailleerde hieronder.

**Provisioning.Enabled:**  
Type: Booleaanse  
Standaard: y

Hiermee kan de gebruiker of de inrichten functionaliteit in de agent uit te schakelen. Geldige waarden zijn "y" of "n". Als inrichting is uitgeschakeld, SSH host en gebruiker sleutels in de afbeelding blijven behouden en eventuele configuratie is opgegeven in de inrichting van API Azure wordt aangegeven, wordt die genegeerd.

> [AZURE.NOTE] De `Provisioning.Enabled` weggelaten, wordt 'n"op Ubuntu Cloud afbeeldingen die cloud initialisatie voor het inrichten van gebruiken.

  
**Provisioning.DeleteRootPassword:**  
Type: Booleaanse  
Standaard: n

Als de set, het wachtwoord van de hoofdmap in het bestand/enz/schaduw gewist tijdens het inrichten.


**Provisioning.RegenerateSshHostKeyPair:**  
Type: Booleaanse  
Standaard: y

Als de set, alle SSH host belangrijke paren (ecdsa, dsa en rsa) tijdens het inrichten van/etc/ssh/worden verwijderd. En één vers combinatie van een sleutel wordt gegenereerd.

Het versleutelingstype voor de combinatie van vers sleutel kan worden geconfigureerd door het fragment Provisioning.SshHostKeyPairType. Houd er rekening mee dat sommige onderzoeken opnieuw SSH paren van sleutels voor eventuele ontbrekende versleutelingstypen gemaakt worden wanneer de daemon SSH opnieuw is opgestart (bijvoorbeeld na het opnieuw opstarten).


**Provisioning.SshHostKeyPairType:**  
Type: tekenreeks  
Standaard: rsa

Dit kan worden ingesteld op een type codering algoritme die wordt ondersteund door de daemon SSH op de virtuele machine. De meestal ondersteunde waarden zijn "rsa", "dsa" en "ecdsa". Houd er rekening mee dat "putty.exe" op Windows 'ecdsa' niet ondersteunt. Ja, als u putty.exe op Windows met verbinding maken met een Linux-implementatie wilt, gebruik "rsa" of "dsa".


**Provisioning.MonitorHostName:**  
Type: Booleaanse  
Standaard: y

Als instellen, waagent wordt de Linux virtuele machine voor hostname wijzigingen controleren (zoals die het resultaat van de opdracht "hostname") en de netwerkconfiguratie in de afbeelding aan de wijziging automatisch bijgewerkt. Om te kunnen push de naam wijzigen met de DNS-servers, netwerken gaan in de virtuele machine. Hierdoor wordt in het kort kwaliteitsverlies internetconnectiviteit.


**Provisioning.DecodeCustomData**  
Type: Booleaanse  
Standaard: n

Als instellen, waagent wordt decoderen CustomData uit Base64.


**Provisioning.ExecuteCustomData**  
Type: Booleaanse  
Standaard: n

Als de instelt, worden waagent CustomData na het inrichten uitgevoerd.


**Provisioning.PasswordCryptId**  
Type: tekenreeks  
Standaard: 6

Algoritme gebruikt door crypt bij het genereren van wachtwoordhash.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA-512  


**Provisioning.PasswordCryptSaltLength**  
Type: tekenreeks  
Standaard: 10

Lengte van willekeurig zout gebruikt bij het genereren van wachtwoordhash.


**ResourceDisk.Format:**  
Type: Booleaanse  
Standaard: y

Als instellen, de resource-schijf die door het platform worden opgemaakt en door waagent gekoppeld als het type bestandssysteem aangevraagd door de gebruiker in "ResourceDisk.Filesystem" iets anders dan 'ntfs'. Een enkel partition van het type Linux (83) komen beschikbaar op de schijf. Houd er rekening mee dat deze partition niet worden opgemaakt als het goed kan worden gekoppeld.


**ResourceDisk.Filesystem:**  
Type: tekenreeks  
Standaard: ext4

Hiermee geeft u het type bestandssysteem voor de resource-schijf. Ondersteunde waarden is afhankelijk van het Linux-verdeling. Als de tekenreeks X, klikt u vervolgens mkfs is. X moet op de afbeelding Linux aanwezig zijn. SLES 11 afbeeldingen moeten meestal 'ext3' gebruiken. FreeBSD afbeeldingen moeten hier 'ufs2' gebruiken.


**ResourceDisk.MountPoint:**  
Type: tekenreeks  
Standaard: / mnt/resource 

Hiermee geeft u het pad waarop de resource-schijf is gekoppeld. Houd er rekening mee dat de resource-schijf een *tijdelijke* schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven.


**ResourceDisk.MountOptions**  
Type: tekenreeks  
Standaard: geen

Hiermee geeft u de koppelingsopties schijf worden doorgegeven aan de opdracht koppelen -o. Dit is een door komma's gescheiden lijst met waarden, ex. 'nodev, nosuid'. Zie mount(8) voor meer informatie.


**ResourceDisk.EnableSwap:**  
Type: Booleaanse  
Standaard: n

Als instellen, een bestand uitwisselen (/ wisselbestand) is op de schijf resource gemaakt en toegevoegd aan het systeem omwisselen ruimte.


**ResourceDisk.SwapSizeMB:**  
Type: geheel getal  
Standaard: 0

De grootte van het bestand omwisselen in megabytes.


**Logs.Verbose:**  
Type: Booleaanse  
Standaard: n

Als de set, log gegevens opnemen is verhoogd. Waagent meldt zich bij /var/log/waagent.log en maakt gebruik van de functionaliteit van de logrotate systeem als u wilt draaien Logboeken.


**OS. EnableRDMA**  
Type: Booleaanse  
Standaard: n

Als instellen, de agent probeert te installeren en vervolgens laden een RDMA kernelstuurprogramma die overeenkomt met de versie van de firmware op de onderliggende hardware.


**OS. RootDeviceScsiTimeout:**  
Type: geheel getal  
Standaard: 300

Hiermee wordt de time-out SCSI in seconden op de schijf en gegevens stations voor OS. Als niet zo is ingesteld, het systeem standaardwaarden worden gebruikt.


**OS. OpensslPath:**  
Type: tekenreeks  
Standaard: geen

Dit kan worden gebruikt om op te geven van een alternatief pad voor het openssl binaire voor cryptografische bewerkingen wilt gebruiken.


**HttpProxy.Host, HttpProxy.Port**  
Type: tekenreeks  
Standaard: geen

Als de instelt, worden de agent deze proxyserver voor toegang tot internet gebruikt. 


## <a name="ubuntu-cloud-images"></a>Ubuntu Cloud-afbeeldingen

Houd er rekening mee dat Ubuntu Cloud afbeeldingen [cloud initialisatie](https://launchpad.net/ubuntu/+source/cloud-init) om uit te voeren veel configuratietaken uit te voeren die anders zou worden beheerd door de Azure Linux-Agent gebruiken.  Houd rekening met de volgende verschillen:


- **Provisioning.Enabled** standaard "n" op Ubuntu Cloud afbeeldingen die met cloud initialisatie inrichten taken uitvoeren.

- De volgende configuratieparameters hebben geen invloed op Ubuntu Cloud afbeeldingen die cloud initialisatie gebruiken voor het beheren van de resource-schijf en uitwisselen ruimte:

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- Zie de volgende bronnen voor het configureren van de resource schijf mountains komma en ruimte op Ubuntu Cloud afbeeldingen uitwisselen tijdens het inrichten:

 - [Ubuntu Wiki: Omwisselen partities configureren](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [Aangepaste gegevens invoegen in een Azure virtuele machines](virtual-machines-windows-classic-inject-custom-data.md)

