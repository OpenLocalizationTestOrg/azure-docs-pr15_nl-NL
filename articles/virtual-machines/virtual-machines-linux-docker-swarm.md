<properties
   pageTitle="Aan de slag met docker met swarm op Azure"
   description="Uitgelegd hoe u een groep van VMs maken met de extensie van de VM Docker en swarm gebruiken om te maken van een cluster Docker."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Hoe u docker met swarm

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


In dit onderwerp ziet u een zeer eenvoudige manier om te gebruiken van [docker](https://www.docker.com/) met [swarm](https://github.com/docker/swarm) voor het maken van een cluster swarm worden beheerd op Azure. Vier virtuele machines wordt gemaakt in Azure wordt aangegeven, een fungeert als de manager swarm en drie als onderdeel van het cluster van docker hosts. Wanneer u klaar bent, kunt u swarm om te zien van het cluster en breng de gewenste wijzigingen docker gebruiken op is geïnstalleerd. De oproepen Azure CLI in dit onderwerp gebruik bovendien de service management (asm)-modus. 

> [AZURE.NOTE] In dit onderwerp wordt docker met swarm en de Azure CLI *zonder* **docker-machine** gebruiken om te kunnen weergeven hoe de verschillende functies samenwerken, maar blijven onafhankelijke gebruikt. **docker-machine** heeft **--swarm** schakelopties waarmee u kunt het **docker-machine** knooppunten rechtstreeks toevoegen aan een swarm gebruiken. Zie de documentatie [docker-machine](https://github.com/docker/machine) voor een voorbeeld. Als u **docker-machine** uitgevoerd ten opzichte van Azure VMs hebt gemist, raadpleegt u [het gebruik van docker-machine met Azure](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Docker hosts met Azure virtuele Machines maken

In dit onderwerp wordt gemaakt van vier VMs, maar u kunt een getal dat u wilt gebruiken. De volgende handelingen uit met bellen * &lt;wachtwoord&gt; * vervangen door het wachtwoord die u hebt gekozen.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Wanneer u klaar bent zou u moeten kunnen **azure vm lijst** gebruikt om te zien van uw VMs Azure:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Swarm installatie op de basispagina swarm VM

In dit onderwerp wordt gebruikt de die wordt [container model van de installatie van de docker swarm documentatie](https://github.com/docker/swarm#1---docker-image) --, maar u kunt ook SSH in de **swarm-outmodel**. In dit model, wordt **swarm** gedownload als docker container swarm uitgevoerd. Onder, wordt deze stap uitvoeren *van onze laptop met behulp van docker op afstand* als u wilt verbinden met het **swarm-outmodel** VM, waarbij wordt aangegeven dat het gebruik van de cluster-id maken opdracht **swarm maken**. De cluster-id is hoe de leden van de groep swarm **swarm** ontdekt. (U kunt ook de opslagplaats klonen en bouwen uzelf, waarin wordt bieden u volledig beheer en foutopsporing inschakelen.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Deze laatste regel is de id cluster; Kopieer deze ergens omdat u deze opnieuw gebruiken wordt wanneer u deelneemt aan het knooppunt VMs aan het model swarm maken van de 'swarm'. In dit voorbeeld is de id van de cluster **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Net als u wilt wissen, gebruiken we onze installatie lokale docker verbinding maken met het **swarm-outmodel** VM in Azure en instructieset **swarm-outmodel** om terug te downloaden, installeren en uitvoeren van de opdracht **maken** , die als resultaat de id van onze cluster die we voor de toepassing discovery later gebruiken.
<!-- -->
> U kunt dit controleren uitvoeren `docker -H tcp://` * &lt;hostname&gt; * ` images` voor een overzicht van de container-processen op de computer **swarm-outmodel** en klik op een ander knooppunt voor vergelijking (omdat we de vorige swarm-opdracht uitvoeren met de schakeloptie **--rm** , de container is verwijderd nadat deze is voltooid, met behulp van **docker ps - a** niet resultaat).:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Als u bekend met **docker bent**, kunt u weet dat de andere knooppunten geen posten hebt omdat geen afbeeldingen zijn gedownload en nog uitgevoerd.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Deelnemen aan het knooppunt VMs aan onze cluster docker

Voor elk knooppunt, een lijst met de weergegeven gegevens over het gebruik van de Azure CLI. Hieronder doen we die voor de host van de docker **swarm-knooppunt-1** voor het verkrijgen van het knooppunt docker poort.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Met **docker** en de `-H` optie voor het knooppunt VM, wijst u de client docker dat knooppunt te koppelen aan de swarm die u wilt maken door de cluster-id en van het knooppunt docker poort (de laatste **--adres**gebruiken):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Die er goed uitziet. Om te bevestigen dat **swarm** wordt uitgevoerd op **swarm-knooppunt-1** die we Typ:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Herhaal dit voor alle andere knooppunten in het cluster. In ons geval doen we die voor **swarm-knooppunt-2** - en **swarm-knooppunt-3**.

## <a name="begin-managing-the-swarm-cluster"></a>Beginnen met het beheren van het cluster swarm

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

en vervolgens u kunt een lijst van de knooppunten in uw cluster:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Ga zaken uitvoeren op uw swarm. Als u wilt zoeken Inspiratie, raadpleegt u [https://github.com/docker/swarm/](https://github.com/docker/swarm/)of misschien een [video](https://www.youtube.com/watch?v=EC25ARhZ5bI).

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 
