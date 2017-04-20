<properties
   pageTitle="MongoDB installeren op een VM Linux | Microsoft Azure"
   description="Informatie over het installeren en configureren van MongoDB op een Linux virtuele machine in Azure wordt aangegeven met het implementatiemodel resourcemanager."
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
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Installeren en configureren van MongoDB op een VM Linux in Azure wordt aangegeven
[MongoDB](http://www.mongodb.org) is een populaire open source, krachtige NoSQL database. In dit artikel leest u hoe installeren en configureren van MongoDB op een VM Linux in Azure wordt aangegeven met het implementatiemodel resourcemanager. Voorbeelden worden weergegeven die detail hoe naar:

- [Handmatig installeren en configureren van een eenvoudige MongoDB exemplaar](#manually-install-and-configure-mongodb-on-a-vm)
- [Een eenvoudige MongoDB-exemplaar van een Resource Manager-sjabloon maken](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Een complexe MongoDB een laptopgeheugen cluster met replica wilt instellen met een resourcemanager-sjabloon maken](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Vereisten voor
In dit artikel is het volgende nodig:

- een Azure-account (het[ophalen van een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)).
- de [Azure CLI](../xplat-cli-install.md) bent aangemeld`azure login`
- de Azure CLI *moet zijn* bij het gebruik van Azure resourcemanager-modus`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Handmatig installeren en configureren van MongoDB op een VM
MongoDB [installatie-instructies geven](https://docs.mongodb.com/manual/administration/install-on-linux/) voor Linux distros inclusief rood rol / CentOS, SUSE Ubuntu en Debian. Het volgende voorbeeld wordt een `CoreOS` VM met een SSH-sleutel die is opgeslagen op `.ssh/azure_id_rsa.pub`. Beantwoord de aanwijzingen voor opslagaccountnaam, de naam van de DNS- en beheerdersreferenties:

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Meld u aan bij de VM via het openbare IP-adres weergegeven aan het einde van de vorige stap voor VM maken:

```bash
ssh ops@40.78.23.145
```

Als u wilt de installatiebronnen toevoegen voor MongoDB, maak een `yum` opslagplaatsbestand als volgt:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

Open het bestand MongoDB cessies‑retrocessies om te bewerken. Voeg de volgende regels:

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Installeren met MongoDB `yum` als volgt:

```bash
sudo yum install -y mongodb-org
```

Standaard wordt SELinux afgedwongen op CentOS afbeeldingen, waardoor u toegang krijgen tot MongoDB. Hulpmiddelen voor projectbeheer van beleid installeren en configureren van SELinux zodat MongoDB werken op de standaardpoort naar TCP 27017 als volgt. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Als volgt te werk om de MongoDB-service te starten:

```bash
sudo service mongod start
```

Controleer of de installatie MongoDB door verbinding te maken met de lokale `mongo` client:

```bash
mongo
```

Test nu het exemplaar MongoDB door sommige gegevens toevoegen en vervolgens te zoeken:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Desgewenst configureren MongoDB worden automatisch gestart tijdens het systeem opnieuw:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Eenvoudige MongoDB exemplaar op CentOS met een sjabloon maken
U kunt een eenvoudige MongoDB exemplaar op een enkele CentOS VM met de volgende Azure quickstart sjabloon Github maken. Deze sjabloon heeft de extensie aangepast Script voor Linux om toe te voegen een `yum` opslagplaats naar uw nieuwe CentOS VM en installeer MongoDB.

- [Eenvoudige MongoDB exemplaar op CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Het volgende voorbeeld wordt een resourcegroep gemaakt met de naam `myResourceGroup` in de `WestUS` regio. Voer uw eigen waarden als volgt in:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] De CLI Azure geeft u op een vraag binnen een paar seconden voor het maken van de implementatie van, maar de installatie en configuratie duurt een paar minuten. Controleer de status van de implementatie met `azure group deployment show myResourceGroup`, de naam van uw resourcegroep dienovereenkomstig gewijzigd invoert. Wacht totdat de `ProvisioningState` 'Geslaagd' geeft voordat u probeert te SSH voor VM.

Wanneer de implementatie voltooid, SSH voor VM is. Het IP-adres van het gebruik van uw VM de `azure vm show` command zoals in het volgende voorbeeld:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

Bij het einde van de uitvoer, de `Public IP address` wordt weergegeven. SSH voor uw VM met het IP-adres van uw VM:

```bash
ssh ops@138.91.149.74
```

Controleer of de installatie MongoDB door verbinding te maken met de lokale `mongo` client als volgt:

```bash
mongo
```

Test nu het exemplaar door sommige gegevens toevoegen en als volgt te zoeken:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Een complexe MongoDB een laptopgeheugen Cluster op CentOS met een sjabloon maken
U kunt een complexe MongoDB een laptopgeheugen cluster met behulp van de volgende Azure quickstart-sjabloon van Github maken. Deze sjabloon volgt de [MongoDB een laptopgeheugen cluster aanbevolen procedures](https://docs.mongodb.com/manual/core/sharded-cluster-components/) voor redundantie en betere beschikbaarheid. De sjabloon maakt twee shards, met drie knooppunten in elke replicaset. Eén config server replicaset met drie knooppunten ook wordt gemaakt, plus twee `mongos` router-servers om te voorzien in consistentie naar toepassingen uit de shards.

- [MongoDB Sharding Cluster op CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] Dit complexe MongoDB een laptopgeheugen cluster implementeert moet meer dan 20 cores, welke is meestal de standaard core telling per regio voor een abonnement. Open een ondersteuningsverzoek voor een Azure om uit te breiden uw core tellen.

Het volgende voorbeeld wordt een resourcegroep gemaakt met de naam `myResourceGroup` in de `WestUS` regio. Voer uw eigen waarden als volgt in:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] De CLI Azure geeft u op een vraag binnen een paar seconden voor het maken van de implementatie van, maar de installatie en configuratie kunnen overnemen per uur om te voltooien. Controleer de status van de implementatie met `azure group deployment show myResourceGroup`, past de naam van uw resourcegroep aan. Wacht totdat de `ProvisioningState` 'Geslaagd' geeft voordat u verbinding maakt met de VMs.


## <a name="next-steps"></a>Volgende stappen
In deze voorbeelden wordt verbinding met het exemplaar MongoDB lokaal uit de VM. Als u verbinden met de instantie MongoDB uit een ander VM of netwerk wilt, controleert u het juiste [netwerk-beveiligingsgroep regels worden gemaakt](virtual-machines-linux-nsg-quickstart.md).

Zie [overzicht van de Azure resourcemanager](../azure-resource-manager/resource-group-overview.md)voor meer informatie over het maken van sjablonen gebruiken.

De resourcemanager Azure-sjablonen gebruiken de aangepast Script extensie downloaden en uitvoeren van scripts op uw VMs. Zie [gebruik van de Azure aangepast Script extensie met Linux virtuele Machines](virtual-machines-linux-extensions-customscript.md)voor meer informatie.