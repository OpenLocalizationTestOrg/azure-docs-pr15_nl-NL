<properties
    pageTitle="Docker hosts maken in Azure wordt aangegeven met Docker Machine | Microsoft Azure"
    description="Beschrijving gebruik van Docker Machine docker hosts maken in Azure wordt aangegeven."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="rasquill"/>

# <a name="use-docker-machine-with-the-azure-driver"></a>Docker Machine gebruiken met de Azure-stuurprogramma

[Docker](https://www.docker.com/) is een van de populairste aanpak virtualization Linux containers in plaats van virtuele machines gebruikt als een manier van toepassingsgegevens te isoleren en computing op gedeelde resources. Dit onderwerp wordt beschreven wanneer en hoe [Docker Machine](https://docs.docker.com/machine/) gebruiken (de `docker-machine` opdracht) om te maken van nieuwe Linux VMs in Azure ingeschakeld als host voor uw containers Linux docker.


## <a name="create-vms-with-docker-machine"></a>VMs met Docker Machine maken

Het maken van de docker host VMs in Azure wordt aangegeven met de `docker-machine create` opdracht met de `azure` stuurprogramma argument voor de optie stuurprogramma (`-d`) en alle andere argumenten. 

Het volgende voorbeeld is afhankelijk van de standaardwaarden, maar dit poort 80 op de VM met internet om te testen met een container nginx, wordt geopend `ops` de aangemelde gebruiker voor SSH en oproepen de nieuwe VM `machine`. 

Type `docker-machine create --driver azure` om te zien van de opties en de standaardwaarden. u kunt ook de [Docker Azure stuurprogramma documentatie](https://docs.docker.com/machine/drivers/azure/)lezen. (Houd er rekening mee dat als er tweeledige verificatie is ingeschakeld, wordt u gevraagd om te verifiëren met behulp van de tweede factor.)

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

De uitvoer moet er ongeveer zo uitzien, afhankelijk van of u tweeledige verificatie is geconfigureerd in uw account hebt.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## <a name="configure-your-docker-shell"></a>Uw shell docker configureren

Nu, typ `docker-machine env <VM name>` om te zien wat u moet doen om de shell configureren. 

```bash
docker-machine env machine
```

Die de omgevingsinformatie, die er ongeveer zo uitziet afdrukt. Houd rekening met dat het IP-adres is toegewezen, die u moet de VM testen.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

U kunt de configuratieopdracht voorgestelde uitvoeren of u kunt de omgevingsvariabelen zelf instellen. 

## <a name="run-a-container"></a>Een container uitvoeren

Nu kunt u een eenvoudige webserver test uitvoeren of alles correct werkt. Hier we gebruiken de afbeelding van een standaard nginx opgeven dat deze moet luisteren op poort 80 en als de VM opnieuw is opgestart wordt de container moet opnieuw als u ook (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

De uitvoer ziet er ongeveer als volgt te werk:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>De container testen

Actieve containers met onderzoeken `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

En controleer de lopende container, typ `docker-machine ip <VM name>` om het IP-adres (als u bent vergeten uit de `env` opdracht):

![Actieve ngnix container](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## <a name="next-steps"></a>Volgende stappen

Als u geïnteresseerd bent, kunt u de Azure [Docker VM extensie](virtual-machines-linux-dockerextension.md) moet u doen met de CLI Azure of Azure resource manager sjablonen dezelfde bewerking uitproberen. 

Zie [werken met Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) uit de [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinding maken met [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/)voor meer voorbeelden van het werken met Docker. Zie voor meer QuickStart uit de demo HealthClinic.biz [Azure ontwikkelaars hulpmiddelen voor QuickStart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

