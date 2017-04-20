<properties
    pageTitle="LICHT implementeren op een virtuele Linux machine | Microsoft Azure"
    description="Leer hoe u de stapel licht installeert op een VM Linux"
    services="virtual-machines-linux"
    documentationCenter="virtual-machines"
    authors="jluk"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="juluk"/>

# <a name="deploy-lamp-stack-on-azure"></a>LICHT stapel op Azure implementeren
In dit artikel wordt u begeleid bij het implementeren van een webserver Apache, MySQL en PHP (de stapel licht) op Azure. Moet u een Azure-Account (het[ophalen van een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)) en de [Azure CLI](../xplat-cli-install.md) die is [gekoppeld aan uw Azure-account](../xplat-cli-connect.md).

Er zijn twee methoden voor het installeren van licht beschreven in dit artikel:

## <a name="quick-command-summary"></a>Overzicht van de opdracht snelle

1) LICHT op nieuwe VM implementeren

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

2) LICHT aan bestaande VM implementeren

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>LICHT op nieuwe VM scenario implementeren

U kunt beginnen met het maken van een nieuwe [resourcegroep](../azure-resource-manager/resource-group-overview.md) waarin u de VM:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Als u wilt de VM zelf hebt gemaakt, kunt u een al geschreven resourcemanager Azure-sjabloon gevonden [hier op GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Hier ziet u een antwoord waarin u wordt gevraagd enkele meer invoerwaarden:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

U hebt nu een VM Linux gemaakt met licht al is geïnstalleerd. Als u wilt, kunt u de installatie controleren door springen naar beneden af op [verifiëren licht geïnstalleerd].

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>LICHT aan bestaande VM scenario implementeren

Als u hulp bij het maken van een Linux VM nodig kunt onderweg [hier voor meer informatie over het maken van een Linux VM] (. / virtual-machines-linux-quick-create-cli.md). Vervolgens moet u naar SSH in de VM Linux. Als u hulp nodig hebt bij het maken van een sleutel SSH kunt onderweg [hier voor meer informatie over het maken van een sleutel SSH op Linux/Mac] (. / virtual-machines-linux-mac-create-ssh-keys.md).
Als u een SSH-sleutel al hebt, gaat u verder en SSH in uw VM Linux met `ssh username@uniqueDNS`.

Nu dat u binnen uw Linux VM werkt, doorlopen we de stapel licht installatie op Debian gebaseerde onderzoeken. De exacte opdrachten kunnen verschillen voor andere distros Linux.

#### <a name="installing-on-debianubuntu"></a>Installatie op Debian/Ubuntu

Moet u de volgende pakketten geïnstalleerd: `apache2`, `mysql-server`, `php5`, en `php5-mysql`. U kunt deze rechtstreeks deze pakketten toepassen of door gebruik Tasksel installeren. Hieronder ziet u de instructies voor beide opties.
Voordat u installeert moet u downloaden en pakket lijsten bijwerken.

    user@ubuntu$ sudo apt-get update
    
##### <a name="individual-packages"></a>Afzonderlijke-pakketten
Gebruik enz ophalen:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Tasksel gebruiken
U kunt ook Tasksel, een hulpmiddel Debian/Ubuntu die meerdere verwante pakketten installaties als een gecoördineerde 'taak' op uw systeem downloaden.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Na het uitvoeren van een van de bovenstaande opties wordt u gevraagd om deze pakketten en een aantal andere afhankelijkheden te installeren. Drukt u op 'y' en 'Enter' om te gaan en volg eventuele andere aanwijzingen voor het instellen van een beheerderswachtwoord voor MySQL. Hiermee installeert u de minimaal vereiste PHP extensies die nodig zijn voor het gebruiken van PHP met MySQL. 

![][1]

Voer de volgende opdracht uit om andere PHP uitbreidingen die beschikbaar als pakketten zijn weer te geven:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Info.php-document maken

U moet nu mogelijk om te controleren welke versie van Apache, MySQL en PHP via de opdrachtregel door te typen `apache2 -v`, `mysql -v`, of `php -v`.

Als u wilt om verder te testen, kunt u een snelle PHP info-pagina weergeven in een browser. Een nieuw bestand maken met Nano teksteditor met deze opdracht:

    user@ubuntu$ sudo nano /var/www/html/info.php

Voeg de volgende regels in de teksteditor GNU Nano:

    <?php
    phpinfo();
    ?>

Vervolgens opslaan en sluit de teksteditor af.

Opnieuw opstarten Apache met deze opdracht bevindt, zodat alle nieuwe installaties wordt pas van kracht worden weergegeven.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Controleer of licht is geïnstalleerd

Nu u de PHP info-pagina die u zojuist hebt gemaakt in uw browser door te gaan naar http://youruniqueDNS/info.php controleren kunt, is deze er ongeveer als volgt.

![][2]

U kunt uw installatie Apache controleren door te bekijken van de pagina Apache2 Ubuntu standaard door naar u http://youruniqueDNS/ te gaan. U ziet ongeveer zo uitziet.

![][3]

Gefeliciteerd, u hebt alleen setup een stapel licht op uw VM Azure.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de documentatie Ubuntu op de stapel licht:

- [https://Help.Ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png
