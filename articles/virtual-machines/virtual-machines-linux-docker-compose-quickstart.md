<properties
   pageTitle="Docker en opstellen op een virtuele machine | Microsoft Azure"
   description="Snelle introductie over het werken met opstellen en Docker op Linux virtuele machines in Azure wordt aangegeven"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Aan de slag met Docker en opstellen om te definiëren en een toepassing meerdere container uitvoert op een Azure virtuele machines

Aan de slag Docker en [opstellen](http://github.com/docker/compose) gebruiken om te definiëren en een complexe toepassing uitvoert op een Linux virtuele machine in Azure wordt aangegeven. Met opstellen gebruikt u een eenvoudige tekstbestand een toepassing die bestaat uit meerdere Docker containers definiëren. U vervolgens kringveld van de toepassing in één opdracht die alles als u wilt implementeren van uw gedefinieerde omgeving werkt. 

Als voorbeeld, in dit artikel leest u hoe u snel een blog WordPress met een backend MariaDB SQL-database op een VM Ubuntu instellen. U kunt ook opstellen voor het instellen van complexere toepassingen gebruiken.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Stap 1: Een VM Linux als host Docker instellen

U kunt verschillende Azure procedures en beschikbaar afbeeldingen of resourcemanager sjablonen gebruiken in de Azure Marketplace maken van een Linux VM en stelt u dit als een Docker-host. Zie bijvoorbeeld [gebruik van de extensie Docker VM als u wilt implementeren van uw omgeving](virtual-machines-linux-dockerextension.md) snel een VM Ubuntu maken met de extensie Azure Docker VM met behulp van een [sjabloon quickstart](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Wanneer u de extensie Docker VM gebruikt, wordt uw VM automatisch is ingesteld als host Docker en opstellen al is geïnstalleerd. Het voorbeeld in dit artikel ziet u het gebruik van de [Azure opdrachtregel-interface voor Mac, Linux, en Windows](../xplat-cli-install.md) (de Azure CLI) in de modus resourcemanager de VM maken.

De eenvoudige opdracht uit het vorige document maakt u een resourcegroep met de naam `myResourceGroup` in de `West US` locatie en een VM implementeert met de extensie Azure Docker VM is geïnstalleerd:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Stap 2: Controleren of opstellen is geïnstalleerd

Zodra de implementatie is voltooid, geef SSH aan uw nieuwe Docker host de DNS-records met de naam u hebt opgegeven tijdens de implementatie. U kunt `azure vm show -g myDockerResourceGroup -n myDockerVM` details van uw VM, inclusief de naam van de DNS-bekijken.

Als u wilt controleren of opstellen op de VM is geïnstalleerd, voert u de volgende opdracht uit:

```bash
docker-compose --version
```

U ziet dat de uitvoer is vergelijkbaar met `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Als u een andere methode maken van een host Docker en moet installeren opstellen uzelf hebt gebruikt, raadpleegt u de [documentatie opstellen](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Stap 3: Een configuratiebestand docker-compose.yml maken

Vervolgens maakt u een `docker-compose.yml` bestand, dat wil een configuratie tekstbestand, de containers Docker zeggen uitvoeren op de VM definiëren. Hiermee geeft u het bestand op de afbeelding om uit te voeren op elke container (of wordt een opbouwen uit een Dockerfile) nodig omgevingsvariabelen en afhankelijkheden, poorten en de koppelingen tussen containers. Zie voor meer informatie over de syntaxis van de yml-bestand, [opstellen bestand waarnaar wordt verwezen](http://docs.docker.com/compose/yml/).

Maak de `docker-compose.yml` bestand als volgt:

```bash
touch docker-compose.yml
```

Gebruik uw favoriete teksteditor sommige gegevens toevoegen aan het bestand. Het volgende voorbeeld wordt de `vi` editor:

```bash
vi docker-compose.yml
```

Plak in het volgende voorbeeld in het tekstbestand. Deze configuratie wordt afbeeldingen van het [Register DockerHub](https://registry.hub.docker.com/_/wordpress/) gebruikt voor de installatie WordPress (de bron openen bloggen en inhoud management systeem) en een gekoppelde backend MariaDB SQL-database. Voer uw eigen `MYSQL_ROOT_PASSWORD` als volgt:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Stap 4: De containers beginnen met opstellen

In dezelfde map als uw `docker-compose.yml` bestand, voer de volgende opdracht (afhankelijk van uw omgeving, moet u mogelijk uitvoeren `docker-compose` met `sudo`.):

```bash
docker-compose up -d

```

Deze opdracht start de Docker containers opgegeven in `docker-compose.yml`. Het duurt minuten of twee voor deze stap om te voltooien. Ziet u de uitvoer is vergelijkbaar met het volgende voorbeeld:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Zorg ervoor dat de optie **-d** op opstart gebruiken, zodat de containers continu op de achtergrond uitvoeren.

Als u wilt controleren of de containers zijn omhoog, typ `docker-compose ps`. Worden er ongeveer als volgt:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

U kunt nu verbinding maken met WordPress rechtstreeks op de VM op poort 80. Open een webbrowser en typ de naam van de DNS van uw VM (zoals `http://myresourcegroup.westus.cloudapp.azure.com`). U ziet nu de WordPress startscherm, waar u de installatie kan uitvoeren en aan de slag met de toepassing.

![WordPress-startscherm][wordpress_start]


## <a name="next-steps"></a>Volgende stappen

* Ga naar de [Gebruikershandleiding voor Docker VM-extensie](https://github.com/Azure/azure-docker-extension/blob/master/README.md) voor meer opties voor het configureren van Docker en opstellen in uw VM Docker. Eén optie is bijvoorbeeld rechtstreeks in de configuratie van de extensie Docker VM aan het opstellen yml-bestand (geconverteerd naar JSON) heb opgeslagen.
* Bekijk de [gebruikershandleiding](http://docs.docker.com/compose/) voor meer voorbeelden van maken en implementeren van meerdere container apps en [opdrachtregelverwijzing opstellen](http://docs.docker.com/compose/reference/) .
* Een sjabloon Azure resourcemanager gebruiken uw eigen of een bijgedragen van de [community](https://azure.microsoft.com/documentation/templates/)naar het implementeren van een VM Azure met Docker en een toepassing instellen met opstellen. De sjabloon [Deploy een blog WordPress met Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) gebruikt bijvoorbeeld Docker en opstellen snel implementeren WordPress met een MySQL-end op een Ubuntu VM.
* Probeer een cluster [Docker Swarm](virtual-machines-linux-docker-swarm.md) integreren Docker opstellen. Zie [Gebruik opstellen met Swarm](https://docs.docker.com/compose/swarm/) voor scenario's.

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png
