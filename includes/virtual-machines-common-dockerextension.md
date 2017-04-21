

[Docker](https://www.docker.com/) is een van de populairste aanpak virtualization [Linux containers](http://en.wikipedia.org/wiki/LXC) in plaats van virtuele machines gebruikt als een manier van toepassingsgegevens te isoleren en computing op gedeelde resources. U kunt de [Azure Docker VM-extensie](https://github.com/Azure/azure-docker-extension/blob/master/README.md) voor de [Azure Linux Agent](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md) maken een Docker VM waarop een willekeurig aantal containers voor uw toepassingen op Azure.

Dit onderwerp wordt beschreven:

+ [Docker en Linux Containers]
+ [Het gebruik van de Docker VM extensie met Azure]
+ [VM uitbreidingen voor Linux en Windows]

Als u wilt Docker ingeschakelde VMs nu maken, raadpleegt u:

+ [Het gebruik van de Docker VM extensie vanaf de opdrachtregel van Azure (Azure CLI)]
+ [Het gebruik van de Docker VM extensie met Azure klassieke portal]
+ [Hoe u snel aan de slag met Docker in de Azure Marketplace]

Meer informatie over de extensie en hoe deze werkt, raadpleegt u de [Gebruikershandleiding voor Docker extensie](https://github.com/Azure/azure-docker-extension/blob/master/README.md).

## <a name="docker-and-linux-containers"></a>Docker en Linux Containers
[Docker](https://www.docker.com/) is een van de populairste virtualization aanpak [Linux containers](http://en.wikipedia.org/wiki/LXC) in plaats van virtuele machines gebruikt als een manier om gegevens te isoleren en computing op gedeelde bronnen en andere services waarmee u kunt maken of toepassingen snel samenstellen en distribueren ze tussen andere containers Docker biedt.

Docker en Linux containers zijn niet [Hypervisors](http://en.wikipedia.org/wiki/Hypervisor) zoals Windows Hyper-V en [KVM](http://www.linux-kvm.org/page/Main_Page) op Linux (Er zijn vele andere voorbeelden). Hypervisors virtueel het onderliggende besturingssysteem om in te schakelen voltooid besturingssystemen ( *virtuele machines*genoemd) om uit te voeren in de hypervisor alsof ze een toepassing.

Docker en andere methoden *container* radicale afgenomen verbruikte opstart tijd en verwerking zowel opslag belasting vereist met behulp van het proces en systeem moeten worden geïsoleerd, functies van de kernel Linux om weer te geven alleen kernel functies tot een anderszins geïsoleerd container.

De volgende tabel wordt beschreven op een zeer hoog niveau het soort Functieverschillen die bestaat tussen hypervisors en Linux containers. Houd er rekening mee dat sommige functies misschien meer of minder wenselijk afhankelijk van uw eigen behoeften toepassing.

|   Functie      | Hypervisors | Containers  |
| :------------- |-------------| ----------- |
| Processen isoleren | Meer of minder voltooid | Als de hoofdsite worden opgehaald, kan container host worden geknoeid |
| Geheugen op schijf vereist | Voltooid OS plus -apps | Alleen de vereisten voor de App |
| Tijd opstart | Veel langer: Opstarten van OS plus app laden | Aanzienlijk korter: alleen apps moeten worden gestart omdat kernel al wordt uitgevoerd  |
| Container automatisering | Varieert afhankelijk van OS en apps | [Afbeeldingengalerie docker](https://registry.hub.docker.com/); anderen

Een discussie op hoog niveau van containers en hun voordelen, raadpleegt u het [Docker hoog niveau Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

Zie voor meer informatie over wat Docker is en hoe deze echt werkt, [Wat is Docker?](https://www.docker.com/whatisdocker/)

#### <a name="docker-and-linux-container-security-best-practices"></a>Docker en Linux Container beveiliging aanbevolen procedures

Omdat containers toegang tot kernel van de hostcomputer, delen kunnen als schadelijke code krijgen van de hoofdmap is deze mogelijk ook toegang niet alleen naar de computer, maar ook de andere containers. Uw systeem container sterker dan de standaard-configuratie, [Docker wordt aanbevolen](https://docs.docker.com/articles/security/) met toevoeging-Groepsbeleid of [rollen gebaseerde beveiliging](http://en.wikipedia.org/wiki/Role-based_access_control) , alsmede, zoals [SELinux](http://selinuxproject.org/page/Main_Page) of [AppArmor](http://wiki.apparmor.net/index.php/Main_Page), bijvoorbeeld, evenals zo veel mogelijk wordt afgetrokken kernel mogelijkheden die de containers zijn verleend beveiligen. Daarnaast zijn er veel andere documenten op Internet die benaderingen beveiliging op basis van containers zoals Docker beschrijven.

## <a name="how-to-use-the-docker-vm-extension-with-azure"></a>Het gebruik van de Docker VM extensie met Azure

De Docker VM extensie is een onderdeel dat in het exemplaar VM die u maakt waarin de Docker-engine is geïnstalleerd en worden beheerd door externe communicatie met de VM is geïnstalleerd. Er zijn twee manieren voor het installeren van de extensie VM: U kunt uw VM via management portal maken of kunt u deze vanaf de opdrachtregel van Azure (Azure CLI).

U kunt de portal de Docker VM extensie toevoegen aan een compatibele Linux VM (momenteel alleen is de afbeelding waarop dit ondersteunt de recenter dan juli Ubuntu 14.04 LTS-afbeelding). Via de opdrachtregel Azure CLI, echter, kunt u de Docker VM extensie installeren en maken en uploaden van uw Docker communicatie certificaten allemaal tegelijk wanneer u het exemplaar VM maakt.

Als u wilt Docker ingeschakelde VMs nu maken, raadpleegt u:

+ [Het gebruik van de Docker VM extensie vanaf de opdrachtregel van Azure (Azure CLI)]
+ [Het gebruik van de Docker VM extensie met Azure klassieke portal]

## <a name="virtual-machine-extensions-for-linux-and-windows"></a>VM uitbreidingen voor Linux en Windows
De [Docker VM-extensie voor Azure](https://github.com/Azure/azure-docker-extension/blob/master/README.md) is slechts één van de verschillende VM extensies die speciaal gedrag opgeeft, en meer zijn in de ontwikkelingsfase bevindt. Bijvoorbeeld kunnen verscheidene [Linux VM Agent extensie](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md) functies u wijzigen en beheren van de virtuele Machine, inclusief beveiligingsfuncties, kernel en netwerken functies, enzovoort. De extensie VMAccess kunt bijvoorbeeld u opnieuw instellen van het beheerderswachtwoord of SSH-sleutel.

Zie [Azure VM uitbreidingen](../articles/virtual-machines/virtual-machines-windows-extensions-features.md)voor een volledige lijst.

<!--Anchors-->
[Het gebruik van de Docker VM extensie vanaf de opdrachtregel van Azure (Azure CLI)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Het gebruik van de Docker VM extensie met Azure klassieke portal]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/
[Hoe u snel aan de slag met Docker in de Azure Marketplace]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-ubuntu-quickstart/
[Docker en Linux Containers]: #Docker-and-Linux-Containers
[Het gebruik van de Docker VM extensie met Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[VM uitbreidingen voor Linux en Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
