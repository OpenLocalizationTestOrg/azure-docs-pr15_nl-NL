<properties
    pageTitle="Gebruik van de extensie CustomScript op een VM Linux | Microsoft Azure"
    description="Informatie over het gebruiken van de extensie CustomScript om te implementeren toepassingen op Linux virtuele Machines in Azure die zijn gemaakt met behulp van het implementatiemodel klassieke."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Een licht-app met behulp van de Azure CustomScript-extensie voor Linux implementeren#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


De Microsoft Azure CustomScript-extensie voor Linux biedt een manier om uw virtuele machines (VMs) aanpassen door willekeurige code geschreven in elke scripttaal die worden ondersteund door de VM (bijvoorbeeld Python en we vaker doen) uit te voeren. Hiermee kunt u zeer flexibele automatisering van toepassing op meerdere computers.

U kunt de CustomScript-extensie met de portal van Azure klassieke, Windows PowerShell of voor de opdrachtregel Azure (Azure CLI) implementeren.

In dit artikel we gebruiken de CLI Azure om te implementeren van een eenvoudige licht-toepassing voor een VM Ubuntu gemaakt met het implementatiemodel klassieke.

## <a name="prerequisites"></a>Vereisten voor

In dit voorbeeld maakt u eerst twee Azure VMs Ubuntu met 14.04 of hoger. De VMs worden *script-vm* en *licht-vm*genoemd. Gebruik een unieke naam wanneer u de VMs maakt. Een wordt gebruikt om de opdrachten CLI te voeren en een wordt gebruikt om de app licht implementeren.

Moet u ook een opslag van Azure-account en een sleutel toegang toe (u kunt dit openen via de portal van Azure klassieke).

Als u hulp bij het maken van Linux VMs op Azure moet verwijzen naar [een virtuele Machine uitgevoerd Linux maken](virtual-machines-linux-classic-createportal.md).

De opdrachten voor de installatie wordt ervan uitgegaan dat Ubuntu, maar u kunt de installatie voor alle ondersteunde Linux distro aanpassen.

De script-vm VM moet Azure CLI hebt geïnstalleerd, met een verbinding met Azure. Raadpleeg voor hulp bij dit [installeren](../xplat-cli-install.md)en configureren van de opdrachtregel Azure.

## <a name="upload-a-script"></a>Een script uploaden

We gebruiken de extensie CustomScript een script uitvoeren op een externe VM voor het installeren van de stapel licht en een PHP-pagina maken. Als u het script toegang vanaf elke locatie wilt uploadt we dit als een Azure blob.

### <a name="script-overview"></a>Overzicht van script

Het scriptvoorbeeld een stapel licht naar Ubuntu (inclusief het instellen van een stille installatie van MySQL) installeert, schrijft een eenvoudige PHP-bestand en Apache begint.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Script uploaden

Het script opslaan als een tekstbestand, bijvoorbeeld *install_lamp.sh*, en uploadt dit naar Azure Storage. U kunt dit eenvoudig doen met Azure CLI. In het volgende voorbeeld wordt het bestand naar een opslag container met de naam 'scripts' geüpload. Als de container niet bestaat moet u eerst maken.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Ook een JSON-bestand dat wordt beschreven hoe u het script downloaden van Azure Storage maken. Dit opslaan als *public_config.json* ('mystorage' vervangen door de naam van uw account opslag):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>De extensie implementeren

Nu kunt u de volgende opdracht om te implementeren van de Linux CustomScript-extensie voor gebruik van de Azure CLI externe VM.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

De vorige opdracht downloads en het *install_lamp.sh* -script op de VM genoemd *licht-vm*wordt uitgevoerd.

Aangezien de app een webserver bevat, moet u een HTTP luisteren poort op de externe VM met de volgende opdracht te openen.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Cmdlets voor controle en probleemoplossing

U kunt controleren op hoe u ook de aangepast script wordt uitgevoerd door te zoeken bij het logboekbestand op de externe VM. SSH *licht-vm* en uiteinde het logboekbestand met de volgende opdracht.

    cd /var/log/azure/customscript
    tail -f handler.log

Nadat u de extensie CustomScript hebt uitgevoerd, kunt u bladeren naar de PHP-pagina die u hebt gemaakt voor informatie. De pagina PHP voor het voorbeeld in dit artikel is *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Aanvullende informatie

U kunt de dezelfde basisstappen complexere apps implementeren. In dit voorbeeld is het script voor installatie opgeslagen als een openbare blob in Azure opslag. Een optie voor veiliger zou het script voor installatie opslaan als een beveiligde blob met een [Beveiligde toegang handtekening](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SA's).

Aanvullende informatiebronnen voor Azure CLI, Linux en de extensie CustomScript worden vervolgens weergegeven.

[Linux VM aanpassingstaken met de extensie CustomScript automatiseren](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux-extensies (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux en Open-Source Computing op Azure](virtual-machines-linux-opensource-links.md)
