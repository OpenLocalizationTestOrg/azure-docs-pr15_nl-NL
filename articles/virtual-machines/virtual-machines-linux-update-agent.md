<properties
    pageTitle="Bijwerken van de Azure Linux-Agent van GitHub | Microsoft Azure"
    description="Leer hoe u de update Azure Linux Agent voor uw VM Linux in Azure wordt aangegeven naar de lateset-versie van Github"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Het bijwerken van de Azure Linux-Agent op een VM naar de nieuwste versie van GitHub

Als u wilt bijwerken van uw [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) op een VM Linux in Azure wordt aangegeven, moet u al hebben:

1. Een actieve Linux VM in Azure wordt aangegeven.
2. Een verbinding met die Linux VM via SSH.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Als u deze taak van een Windows-computer uitvoeren zult, kunt u stopverf naar SSH gebruiken in uw Linux-computer. Lees [hoe u aanmelden bij een virtuele Machine uitgevoerd Linux](virtual-machines-linux-mac-create-ssh-keys.md)voor meer informatie.

Azure ondersteunde Linux distros het pakket Azure Linux Agent hebt ingevoegd in hun opslagplaatsen, dus controleer en installeer de meest recente versie van die opslagplaats Distro eerst indien mogelijk.  

Voor Ubuntu, typ:

    #sudo apt-get install walinuxagent

En op CentOS, typt u:

    #sudo yum install waagent


Voor Oracle Linux, controleert u of de `Addons` opslagplaats is ingeschakeld. Kies het bestand wilt bewerken `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) of `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), en wijzig de regel `enabled=0` naar `enabled=1` onder **[ol6_addons]** of **[ol7_addons]** in dit bestand.

Als u wilt de nieuwste versie van de Azure Linux-Agent installeren, typ:


    #sudo yum install WALinuxAgent

Als u de invoegtoepassing opslagplaats geen geschikte kunt u deze regels gewoon toevoegen aan het einde van het bestand .repo op basis van uw Oracle Linux-versie:

Voor Oracle Linux 6 virtuele machines:

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Voor Oracle Linux 7 virtuele machines:

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Typ:

    #sudo yum update WALinuxAgent

Dit is meestal alle u nodig hebt, maar als om een bepaalde reden dat u wilt installeren vanaf https://github.com rechtstreeks, gebruikt u de volgende stappen uit.


## <a name="install-wget"></a>Wget installeren

Meld u aan uw VM via SSH.

Wget (Er zijn enkele distros die niet al dan niet standaard zoals Redhat, CentOS en Oracle Linux versies 6.4 en 6.5 installeren) installeren door te typen `#sudo yum install wget` op de opdrachtregel.


## <a name="download-the-latest-version"></a>Download de nieuwste versie

[De versie van Azure Linux Agent in GitHub](https://github.com/Azure/WALinuxAgent/releases) openen in een webpagina, en bekijk het nummer van de meest recente versie. (U kunt uw huidige versie vinden door te typen `#waagent --version`.)

### <a name="for-version-20x-type"></a>Voor versie 2.0.x, type:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   De volgende regel wordt versie 2.0.14 als voorbeeld gebruikt:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>Voor versie 2.1.x of hoger gebruikt, typ:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   De volgende regel wordt versie 2.1.0 als voorbeeld gebruikt:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>Installeren van de Azure Linux-Agent

### <a name="for-version-20x-use"></a>Voor versie 2.0.x, gebruiken:

 Controleer waagent uitvoerbare:

    #chmod +x waagent

 Nieuwe kunt u het uitvoerbare bestand naar /usr/sbin/kopiëren.

  Voor de meeste Linux, gebruiken:

    #sudo cp waagent /usr/sbin

  Gebruik voor CoreOS:

    #sudo cp waagent /usr/share/oem/bin/

  Als dit een nieuwe installatie van de Azure Linux-Agent uitvoeren is:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>Voor versie 2.1.x, gebruiken:

Mogelijk moet u het pakket installeren `setuptools` Zie eerst-- [hier](https://pypi.python.org/pypi/setuptools). Vervolgens uitvoeren:

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>De waagent-service opnieuw starten

Voor de meeste linux Distros:

    #sudo service waagent restart

Gebruik voor Ubuntu:

    #sudo service walinuxagent restart

Gebruik voor CoreOS:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>De versie van Azure Linux Agent bevestigen

    #waagent -version

Voor CoreOS, de bovenstaande opdracht werkt mogelijk niet.

U ziet dat de versie van Azure Linux-Agent is bijgewerkt naar de nieuwe versie.

Zie voor meer informatie aangaande de Azure Linux-Agent [Azure Linux Agent Leesmij](https://github.com/Azure/WALinuxAgent).
