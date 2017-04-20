<properties
    pageTitle="Met behulp van Docker VM extensie voor Linux | Microsoft Azure"
    description="Beschrijft Docker en de extensies Azure virtuele Machines en het maken van Azure virtuele Machines die zijn docker hosts met de CLI Azure in klassieke implementatiemodel."
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
    ms.date="05/27/2016"
    ms.author="rasquill"/>


# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>Gebruik van de Docker VM extensie met de portal van Azure klassieke

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


[Docker](https://www.docker.com/) is een van de populairste aanpak virtualization [Linux containers](http://en.wikipedia.org/wiki/LXC) in plaats van virtuele machines gebruikt als een manier om gegevens te isoleren en computing op gedeelde resources. U kunt de Docker VM extensie beheerd door [Azure Linux Agent] maken een Docker VM waarop een willekeurig aantal containers voor uw toepassingen op Azure.

> [AZURE.NOTE] Dit onderwerp wordt beschreven hoe u een VM Docker maakt van de Azure klassieke portal. Het maken van een VM Docker via de opdrachtregel, raadpleegt u [het gebruik van de Docker VM extensie vanaf de opdrachtregel van Azure (Azure CLI)]. Een discussie op hoog niveau van containers en hun voordelen, raadpleegt u het [Docker hoog niveau Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="create-a-new-vm-from-the-image-gallery"></a>Een nieuwe VM maken vanuit de galerie met afbeelding
De eerste stap is een VM Azure uit een Linux-afbeelding die ondersteuning biedt voor de Docker VM extensie, gebruik van een afbeelding Ubuntu 14.04 LTS vanuit de galerie met afbeelding als een voorbeeldafbeelding van een server en Ubuntu 14.04 bureaublad als een client vereist. Klik in de portal klikt u op **+ nieuwe** in de linkerbenedenhoek om te maken van een nieuw exemplaar van de VM en selecteert u een afbeelding Ubuntu 14.04 LTS vanuit de selecties beschikbaar of de volledige afbeelding-galerie, zoals hieronder wordt weergegeven.

> [AZURE.NOTE] Op dit moment ondersteuning alleen Ubuntu 14.04 LTS afbeeldingen recenter dan juli 2014 voor de Docker VM extensie.

![Een nieuwe Ubuntu-afbeelding maken](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Docker certificaten maken

Nadat de VM is gemaakt, moet u ervoor zorgen dat Docker op uw computer is geïnstalleerd. (Zie [Docker installatie-instructies](https://docs.docker.com/installation/#installation)voor meer informatie.)

Het certificaat en van toetsen bestanden voor communicatie van Docker op basis van de [Docker uitgevoerd met https] kunt maken en plaatst u deze in de **`~/.docker`** map op de clientcomputer.

> [AZURE.NOTE] De extensie Docker VM in de portal vereist momenteel referenties die base64 codering zijn.

Via de opdrachtregel gebruiken **`base64`** of een ander hulpmiddel voor favoriete codering base64-codering onderwerpen maken. Dit wordt uitgevoerd met een eenvoudige reeks certificaat en van toetsen bestanden mogelijk aangepaste gegevens er ongeveer als volgt:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>De Docker VM extensie toevoegen
Als u wilt toevoegen de Docker VM extensie, zoek het VM exemplaar dat u hebt gemaakt en schuif omlaag naar **extensies** en klikt u erop om VM extensies, weer te geven zoals hieronder wordt weergegeven.
> [AZURE.NOTE] Deze functie wordt ondersteund in de preview-portal alleen: https://portal.azure.com/

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)
### <a name="add-an-extension"></a>Extensie toevoegen
Klik op het **+ toevoegen** om de mogelijke VM extensies die u aan deze VM toevoegen kunt weer te geven.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)
### <a name="select-the-docker-vm-extension"></a>Selecteer de Docker VM-extensie
Kies de uitbreiding van de VM Docker, die Hiermee worden de beschrijving van de Docker en belangrijke koppelingen, en klik op **maken** onder aan het begin van de installatieprocedure.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)
### <a name="add-your-certificate-and-key-files"></a>Uw certificaat en belangrijke bestanden toevoegen:

Voer in de velden van het formulier, de versies base64-codering van uw certificeringsinstantie, het certificaat van uw Server en uw Server-toets, zoals wordt weergegeven in de volgende afbeelding.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [AZURE.NOTE] Houd er rekening mee dat (zoals in de voorgaande afbeelding) de 2376 is ingevuld al dan niet standaard. U kunt een eindpunt hier invoert, maar de volgende stap is om de overeenkomende eindpunt te openen. Als u de standaard verandert, zorg om de overeenkomende eindpunt in de volgende stap te openen.

## <a name="add-the-docker-communication-endpoint"></a>Het eindpunt van de communicatie Docker toevoegen
Bij het weergeven van de resourcegroep die u hebt gemaakt, selecteert u de beveiligingsgroep van netwerk dat is gekoppeld aan uw VM en klik op **Regels voor binnenkomende verbindingen beveiliging** om weer te geven van de regels, zoals hier wordt getoond.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Klik op **+ toevoegen** als een andere regel wilt toevoegen en voer in het geval is standaard, een naam voor het eindpunt (in dit voorbeeld **Docker**) en 2376 'bestemming poort Range'. Stel de protocolwaarde met **TCP**en klik op **OK** om de regel te maken.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)


## <a name="test-your-docker-client-and-azure-docker-host"></a>Test uw Docker-Client en Azure Docker Host
Zoek en kopieer de naam van het domein van uw VM en op de opdrachtregel van de clientcomputer, type `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (waarbij *dockerextension* wordt vervangen door het subdomein voor uw VM).

Het resultaat ziet er ongeveer als volgt:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Nadat u de bovenstaande stappen hebt voltooid, hebt u nu een volledig functioneel Docker host waarop een Azure VM, niet worden verbonden met op afstand van andere clients geconfigureerd.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

U bent klaar om te gaan naar de [Gebruikershandleiding Docker] en uw VM Docker gebruiken. Als u automatiseren maken Docker hosts op Azure VMs via de opdracht lijn interface wilt, raadpleegt u [het gebruik van de Docker VM extensie vanaf de opdrachtregel van Azure (Azure CLI)]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Het gebruik van de Docker VM extensie vanaf de opdrachtregel van Azure (Azure CLI)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux-Agent]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Actieve Docker met https]: http://docs.docker.com/articles/https/
[De gebruikershandleiding docker]: https://docs.docker.com/userguide/
