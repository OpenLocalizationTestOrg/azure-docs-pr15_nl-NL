<properties
   pageTitle="Docker hosts maken in Azure wordt aangegeven met Docker Machine | Microsoft Azure"
   description="Beschrijving gebruik van Docker Machine docker hosts maken in Azure wordt aangegeven."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Docker Hosts maken in Azure wordt aangegeven met Docker-computer

Voor het uitvoeren van [Docker](https://www.docker.com/) containers, moet een host VM de daemon docker uitgevoerd.
In dit onderwerp wordt beschreven hoe u de opdracht [docker-computer](https://docs.docker.com/machine/) gebruikt om te maken van nieuwe Linux VMs, geconfigureerd met de daemon Docker, uitgevoerd in Azure wordt aangegeven. 

**Opmerking:** 
- *In dit artikel is afhankelijk van docker-machine versie 0.7.0 of hoger*
- *Windows-Containers worden ondersteund door docker-machine in de nabije toekomst*

## <a name="create-vms-with-docker-machine"></a>VMs met Docker Machine maken

Het maken van de docker host VMs in Azure wordt aangegeven met de `docker-machine create` opdracht met de `azure` stuurprogramma. 

Het stuurprogramma voor Azure moet uw abonnement-ID. U kunt de [Azure CLI](xplat-cli-install.md) of de [Azure-Portal](https://portal.azure.com) gebruiken om op te halen van uw abonnement Azure. 

**Met behulp van de Azure Portal**
- Selecteer abonnementen in het linkernavigatiedeelvenster en kopiëren naar abonnements-id.

**Gebruik van de Azure CLI**
- Type ```azure account list``` en kopieer de abonnements-id.

Type `docker-machine create --driver azure` om te zien van de opties en de standaardwaarden.
U ziet ook de [Docker Azure stuurprogramma documentatie](https://docs.docker.com/machine/drivers/azure/) voor meer informatie. 

Het volgende voorbeeld is afhankelijk van de standaardwaarden, maar dit desgewenst poort 80 op de VM voor internet-toegang geopend. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Kies een host docker met docker-computer
Nadat u een vermelding in docker-computer voor uw host hebt, kunt u de standaardhost instellen wanneer u zich docker-opdrachten.
##<a name="using-powershell"></a>Via PowerShell

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>We vaker doen gebruiken

```bash
eval $(docker-machine env MyDockerHost)
```

U kunt nu docker opdrachten uitvoeren op de opgegeven host

```
docker ps
docker info
```

## <a name="run-a-container"></a>Een container uitvoeren

Met een host is geconfigureerd, kunt u nu een eenvoudige webserver om te controleren of uw host correct is geconfigureerd uitvoeren.
Hier wordt de afbeelding van een standaard nginx gebruiken, opgeven dat deze poort 80 moet luisteren en als de host VM opnieuw is opgestart, wordt de container wordt opnieuw als u ook (`--restart=always`). 

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

En controleer de lopende container, typ `docker-machine ip <VM name>` zoeken naar het IP-adres invoeren in de browser:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Actieve ngnix container](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Overzicht
Met docker-computer kunt u eenvoudig docker hosts in Azure wordt aangegeven voor de afzonderlijke docker host validatie inrichten.
Zie voor productie hosting van containers, de [Container-Azure-Service](http://aka.ms/AzureContainerService)

Als u wilt het ontwikkelen van toepassingen voor .NET-Core met Visual Studio, raadpleegt u [Docker Tools voor Visual Studio](http://aka.ms/DockerToolsForVS)
