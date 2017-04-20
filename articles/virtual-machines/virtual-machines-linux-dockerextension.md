<properties
   pageTitle="Met de extensie Azure Docker VM | Microsoft Azure"
   description="Informatie over het gebruik van de extensie Docker VM snel en veilig implementeren een Docker-omgeving in Azure wordt aangegeven met resourcemanager sjablonen."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/25/2016"
   ms.author="iainfou"/>

# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Een Docker-omgeving in Azure wordt aangegeven met de extensie Docker VM maken
Docker is een populaire container management en afbeeldingen platform waarmee u snel werken met containers op Linux (en Windows ook). In Azure wordt aangegeven, zijn er verschillende manieren kunt u Docker implementeren naar wens. In dit artikel bevat informatie over het gebruik van de extensie Docker VM en Azure resourcemanager sjablonen. 

Zie de volgende artikelen voor meer informatie over de van verschillende implementatiemethoden, met Docker Machine en Azure Container Services, waaronder:

- Met snel prototype een app, kunt u één Docker host [Docker Machine](./virtual-machines-linux-docker-machine.md)gebruiken.
- Voor grotere, stabiele omgevingen, kunt u de uitbreiding Azure Docker VM, die ook ondersteunt [Docker opstellen](https://docs.docker.com/compose/overview/) om te genereren consistente container implementaties. In dit artikel details met de extensie Azure Docker VM.
- Gereed voor productie, scalable omgevingen waarmee u aanvullende hulpmiddelen voor de plannings- en bouwen, kunt u een [Docker Swarm cluster op Azure Container Services](../container-service/container-service-deployment.md)implementeren.


## <a name="azure-docker-vm-extension-overview"></a>Overzicht van de extensie Azure Docker VM
De extensie Azure Docker VM installeert en configureert de Docker daemon, Docker-client en Docker opstellen in uw Linux virtuele machine (VM). Met behulp van de extensie Azure Docker VM, hebt u meer controle en mogelijkheden dan gewoon met Docker Machine of maken van de Docker-host. Met deze aanvullende functies, zoals [Docker opstellen](https://docs.docker.com/compose/overview/), Controleer de Azure Docker VM extensie geschikt zijn voor krachtiger ontwikkelaar of productieomgevingen.

Azure resourcemanager sjablonen definiëren de hele structuur van uw omgeving. Sjablonen kunt u maken en configureren van resources zoals de host Docker VMs, opslag, toegang tot besturingselementen voor rollen gebaseerde RBAC () en diagnostische gegevens. U kunt deze sjablonen voor als u wilt maken van extra implementaties op consistente wijze opnieuw gebruiken. Zie [overzicht van de Resource Manager](../azure-resource-manager/resource-group-overview.md)voor meer informatie over Azure resourcemanager en sjablonen. 


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Een sjabloon met de extensie Azure Docker VM implementeren
Laten we een bestaande quickstart-sjabloon gebruiken om te maken van een Ubuntu VM met de extensie Azure Docker VM installeren en configureren van de Docker-host. U kunt de sjabloon hier weergeven: [eenvoudige implementatie van een VM Ubuntu met Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Moet u de [Meest recente Azure CLI](../xplat-cli-install.md) geïnstalleerd en aangemeld met de modus resourcemanager als volgt:

```
azure config mode arm
```

De sjabloon voor het gebruik van de Azure CLI, geven de sjabloon URI implementeren. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `WestUS` locatie. Uw eigen groep resourcenaam en locatie als volgt gebruiken:

```
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Beantwoord de aanwijzingen voor de naam van uw account opslag, geeft u een gebruikersnaam en wachtwoord en geef een naam op DNS. De uitvoer is vergelijkbaar met het volgende voorbeeld:

```
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

Azure CLI keert terug u op de vraag nadat is maar een paar seconden, maar uw host Docker zich nog steeds wordt maken en configureren met de extensie Azure Docker VM. Het duurt een paar minuten voor de implementatie om te voltooien. U kunt meer informatie over het gebruik van Docker host status weergeven de `azure vm show` opdracht.

Het volgende voorbeeld wordt de status van de VM met de naam `myDockerVM` (de standaardnaam van de sjabloon - naam niet wijzigt deze) in de resourcegroep naam `myResourceGroup`. Voer de naam van de resourcegroep die u in de vorige stap hebt gemaakt:

```bash
azure vm show -g myResourceGroup -n myDockerVM
```

De uitvoer van de `azure vm show` opdracht is vergelijkbaar met het volgende voorbeeld:

```
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Klik boven aan de uitvoer, ziet u de `ProvisioningState` van VM. Wanneer dit wordt weergegeven `Succeeded`, de implementatie is voltooid en kunt u SSH voor VM.

Naar het einde van de uitvoer, `FQDN` geeft de FQDN-naam van uw Docker-host. Deze FQDN is wat u kunt SSH aan de host van uw Docker in de overige stappen.


## <a name="deploy-your-first-nginx-container"></a>Uw eerste nginx container implementeren
Zodra de implementatie is voltooid, SSH aan uw nieuwe Docker host van uw lokale computer. Typ uw eigen gebruikersnaam en het FQDN als volgt:

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Nadat u bent aangemeld aan de host Docker, laten we een container nginx te voeren:

```
sudo docker run -d -p 80:80 nginx
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, terwijl de afbeelding nginx wordt gedownload en een container gestart:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Controleer de status van de containers uitgevoerd op de host van uw Docker als volgt:

```
sudo docker ps
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, waarin u ziet dat de container nginx actief is en TCP-poorten 80 en 443 en wordt doorgestuurd:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Als u wilt uw container in actie zien, opent u een webbrowser en typ de FQDN-naam van uw Docker-host:

![Actieve ngnix container](./media/virtual-machines-linux-dockerextension/nginxrunning.png)


## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker VM extensie sjabloonverwijzing
Het vorige voorbeeld wordt een bestaande quickstart-sjabloon. U kunt ook de extensie Azure Docker VM met uw eigen sjablonen resourcemanager implementeren. Hiervoor kunt u het volgende toevoegen aan de Resource Manager-sjablonen, definiëren de `vmName` van uw VM correct:

```
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

U vindt meer gedetailleerd overzicht over het gebruik van resourcemanager sjablonen door te lezen [Azure resourcemanager overzicht](../azure-resource-manager/resource-group-overview.md).


## <a name="next-steps"></a>Volgende stappen
Wilt u mogelijk [de Docker daemon TCP-poorten configureren](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), [Docker beveiliging](https://docs.docker.com/engine/security/security/)begrijpen of containers met [Docker opstellen](https://docs.docker.com/compose/overview/)implementeren. Zie voor meer informatie over de Azure Docker VM extensie zelf, het [GitHub project](https://github.com/Azure/azure-docker-extension/).

Lees meer informatie over de extra opties voor de implementatie Docker in Azure wordt aangegeven:

- [Docker Machine gebruiken met de Azure-stuurprogramma](./virtual-machines-linux-docker-machine.md)  
- [Aan de slag met Docker en opstellen definieert en uitvoeren van een toepassing meerdere container op een Azure virtuele machine](virtual-machines-linux-docker-compose-quickstart.md).
- [Een cluster Azure Container-Service implementeren](../container-service/container-service-deployment.md)
