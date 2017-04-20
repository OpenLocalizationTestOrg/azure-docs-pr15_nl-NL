<properties
    pageTitle="Met behulp van de Docker VM-extensie voor Linux op Azure"
    description="Beschrijving Docker en de extensies Azure virtuele Machines, en ziet u hoe u via programmacode virtuele Machines op Azure die zijn docker hosts vanaf de opdrachtregel met de CLI Azure maken."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/29/2016"
    ms.author="rasquill"/>

# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Met de extensie Docker VM vanaf de opdrachtregel Azure Interface (Azure CLI)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]



In dit onderwerp wordt beschreven hoe een VM maken met de extensie Docker VM van de modus management (asm) in Azure CLI op elk platform. [Docker](https://www.docker.com/) is een van de populairste aanpak virtualization [Linux containers](http://en.wikipedia.org/wiki/LXC) in plaats van virtuele machines gebruikt als een manier om gegevens te isoleren en computing op gedeelde resources. U kunt de extensie Docker VM en de [Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) maken een Docker VM waarop een willekeurig aantal containers voor uw toepassingen op Azure. Een discussie op hoog niveau van containers en hun voordelen, raadpleegt u het [Docker hoog niveau Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).


##<a name="how-to-use-the-docker-vm-extension-with-azure"></a>Het gebruik van de Docker VM extensie met Azure
Als u wilt de extensie Docker VM met Azure gebruikt, moet u een versie van de [Azure opdrachtregel Interface](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) hoger zijn dan 0.8.6 installeren (zoals van schrijven van dit de huidige versie 0.10.0 is). U kunt de CLI Azure installeren op de Mac, Linux en Windows.


Het volledige proces voor het gebruik van Docker op Azure is eenvoudig:

+ De CLI Azure en de bijbehorende afhankelijkheden installeren op de computer van waaruit u bepalen Azure wilt (in Windows, dit is een Linux-verdeling wordt uitgevoerd met een virtuele machine)
+ De opdrachten Azure CLI Docker gebruiken om te maken van een host VM Docker in Azure wordt aangegeven
+ Gebruik de lokale Docker-opdrachten voor het beheren van uw containers Docker in uw VM Docker in Azure wordt aangegeven.


### <a name="install-the-azure-command-line-interface-azure-cli"></a>Installeren van de Azure opdrachtregel (Azure CLI)

Als u wilt installeren en configureren van de Azure CLI, raadpleegt u [het installeren van de opdrachtregel Azure](../xplat-cli-install.md). Om te bevestigen de installatie, typ `azure` bij de opdrachtprompt en na een korte moment u de illustratie Azure CLI ASCII ziet, waarin de eenvoudige opdrachten die beschikbaar zijn voor u. Als de installatie goed werkt, kunt u moeten kunt typen `azure help vm` en ziet dat een van de lijst met opdrachten "docker".

> [AZURE.NOTE] Docker heeft hulpprogramma's voor Windows, [Docker Machine](https://docs.docker.com/installation/windows/), die u ook gebruiken kunt om het maken van een klant docker die u gebruiken kunt om te werken met Azure VMs als docker hosts te automatiseren.

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>De Azure CLI aan verbinden met uw Azure-Account
Voordat u de CLI Azure kunt moet u uw referenties Azure-account koppelen aan de CLI Azure op uw platform. De sectie [verbinding maken met uw Azure-abonnement](../xplat-cli-connect.md) wordt uitgelegd hoe u downloaden en uw **.publishsettings** -bestand importeren, of om uw CLI Azure koppelen aan een organisatie-id.

> [AZURE.NOTE] Er zijn enkele verschillen in gedrag bij gebruik van een of de andere methoden voor verificatie, dus zorg ervoor dat u de bovenstaande voor meer informatie over de verschillende functionaliteit document lezen.

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Docker installeren en gebruiken van de Docker VM-extensie voor Azure
Volg de [installatie-instructies Docker](https://docs.docker.com/installation/#installation) om te kunnen installeren Docker lokaal op uw computer.

Als u wilt Docker met een Azure virtuele Machine gebruiken, moet de Linux-afbeelding die wordt gebruikt voor de VM de [Azure Linux VM-Agent](virtual-machines-linux-agent-user-guide.md) is geïnstalleerd. Er zijn op dit moment slechts twee soorten afbeeldingen waarmee dit:

+ Een afbeelding Ubuntu uit de afbeeldingengalerie Azure of

+ Een aangepaste Linux-afbeelding die u hebt gemaakt met de Agent Azure Linux VM geïnstalleerd en geconfigureerd. Zie [Azure Linux VM Agent](virtual-machines-linux-agent-user-guide.md) voor meer informatie over het maken van een aangepaste Linux VM met de Azure VM-Agent.

### <a name="using-the-azure-image-gallery"></a>Via de galerie met Azure-afbeelding

Vanuit een we vaker doen of een sessie, gebruikt u de volgende opdracht uit Azure CLI zoekt u de meest recente Ubuntu-afbeelding in de galerie VM gebruiken door te typen

`azure vm image list | grep Ubuntu-14_04`

en kies een van de namen van de afbeelding, zoals `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, gebruik van de volgende opdracht uit om een nieuwe VM die afbeelding gebruiken.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

waarbij geldt:

+ * &lt;vm-cloudservice naam&gt; * is de naam van de VM die de computer Docker container host in Azure wordt aangegeven

+  * &lt;gebruikersnaam&gt; * is de gebruikersnaam van de gebruiker van de hoofdsite standaard van VM

+ * &lt;wachtwoord&gt; * is het wachtwoord van het account *username* die voldoet aan de standaarden van complexiteit voor Azure

> [AZURE.NOTE] Op dit moment een wachtwoord moet ten minste 8 tekens, een kleine letters en één hoofdletter, een getal en een speciaal teken zoals een van de volgende tekens bevatten: `!@#$%^&+=`. Nee, de periode aan het einde van de voorgaande zin is niet een speciaal teken.

Als de opdracht geslaagd is, worden er ongeveer zo uit, afhankelijk van de exacte argumenten en de opties die u hebt gebruikt:

![](./media/virtual-machines-linux-classic-cli-use-docker/dockercreateresults.png)

> [AZURE.NOTE] Maken van een virtuele machine, kan een paar minuten duren, maar nadat deze is ingericht (de waarde status is `ReadyRole`) Hiermee start u de Docker daemon (de Docker-service) en u kunt verbinding maken met de host van de container Docker.

Als u wilt testen de Docker VM die u hebt gemaakt in Azure, typt u het volgende:

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

waar * &lt;vm-naam-u-gebruikt&gt; * is de naam van de virtuele machine die u hebt gebruikt in de oproep door naar `azure vm docker create`. U ziet er ongeveer uitzien als de volgende opties, die aangeeft dat uw Docker Host VM omhoog in Azure wordt aangegeven en uitgevoerd wachten op uw opdrachten. 

Nu u proberen kunt te verbinding maken met de klant docker om informatie te verkrijgen (in sommige Docker client-instellingen, zoals die op de Mac, moet u mogelijk gebruiken `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Net als u zeker weet dat u alle werken, kunt u de VM voor de extensie Docker controleren:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Docker Host VM verificatie

Naast het maken van de VM Docker de `azure vm docker create` opdracht ook automatisch de benodigde certificaten zodat de computer Docker-client verbinding maakt met de host van Azure container met HTTPS gemaakt en de certificaten zijn opgeslagen op de client en de host machines, zo nodig. Klik op volgende pogingen, worden de bestaande certificaten opnieuw kan worden gebruikt en gedeeld met de nieuwe host.

Standaard certificaten zijn geplaatst `~/.docker`, en Docker worden geconfigureerd om uit te voeren op poort **2376**. Als u wilt gebruiken een andere poort of de map en klikt u een van de volgende kunt `azure vm docker create` opdrachtregelopties voor het configureren van de container Docker hosten VM naar een andere poort of verschillende certificaten gebruiken om verbinding te maken clients:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

De daemon Docker op de host is geconfigureerd om te luisteren naar en verifiëren van clientverbindingen op de opgegeven poort met de certificaten die door de `azure vm docker create` opdracht. De clientcomputer moeten deze certificaten toegang te krijgen tot de host Docker hebben.

> [AZURE.NOTE] Een netwerk host uitgevoerd zonder deze certificaten is iedereen die u kunt verbinding maken met de computer. Voordat u de standaardconfiguratie wijzigen, moet u ervoor zorgen dat u de risico's op uw computers en toepassingen kennen.

## <a name="next-steps"></a>Volgende stappen

* U bent klaar om te gaan naar de [Gebruikershandleiding Docker] en uw VM Docker gebruiken. Zie [het gebruik van de extensie Docker VM met de Portal]voor informatie over het maken van een VM Docker ingeschakelde in de nieuwe portal.

* De extensie Azure Docker VM ondersteunt ook Docker opstelt, waarbij een declaratieve YAML-bestand aan een toepassing voor ontwikkelaars gebaseerd zijn op een willekeurige omgeving en zorgt u voor een consistente implementatie. Zie [Aan de slag met Docker en opstellen definieert en uitvoeren van een toepassing meerdere container op een Azure virtuele machine].  

<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

[How to use the Docker VM Extension with Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]: #Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
[Het gebruik van de Docker VM extensie met de Portal]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[De gebruikershandleiding docker]: https://docs.docker.com/userguide/
 
[Aan de slag met Docker en opstellen om te definiëren en een toepassing meerdere container uitvoert op een Azure virtuele machines]:virtual-machines-linux-docker-compose-quickstart.md