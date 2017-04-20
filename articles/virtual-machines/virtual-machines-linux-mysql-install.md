<properties
    pageTitle="MySQL op een VM Linux instellen | Microsoft Azure "
    description="Leer hoe u de stapel MySQL installeert op een Linux virtuele machine (Ubuntu of RedHat familie OS) in Azure wordt aangegeven"
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
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="how-to-install-mysql-on-azure"></a>Het installeren van MySQL op Azure


In dit artikel leert u hoe installeren en configureren van MySQL op een Azure virtuele machine waarop Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


##<a name="install-mysql-on-your-virtual-machine"></a>MySQL installeren op uw VM

> [AZURE.NOTE] U moet al een Microsoft Azure virtuele machine Linux uitvoeren om u te voltooien van deze zelfstudie hebt. Zie de [Azure Linux VM zelfstudie](virtual-machines-linux-quick-create-cli.md) maken en instellen van een Linux VM met `mysqlnode` als de naam VM en `azureuser` als gebruiker voordat u verdergaat.

In dit geval 3306 poort als de MySQL-poort gebruiken.  

Verbinding maken met de Linux VM die u hebt gemaakt via stopverf. Als dit de eerste keer dat u Azure Linux VM gebruikt, raadpleegt u het gebruik van stopverf verbinding maken met een VM Linux [hier](virtual-machines-linux-mac-create-ssh-keys.md).

We gebruiken opslagplaats pakket MySQL5.6 installeren als een voorbeeld in dit artikel. Werkelijk, heeft MySQL5.6 meer verbetering prestaties dan MySQL5.5.  Meer informatie [hier](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).


###<a name="how-to-install-mysql56-on-ubuntu"></a>Hoe u MySQL5.6 installeert op Ubuntu
We gebruiken Linux VM hier met Ubuntu van Azure.

- Stap 1: Installatiefout MySQL Server 5.6 overschakelen naar `root` gebruiker:

            #[azureuser@mysqlnode:~]sudo su -

    Mysql-server 5.6 installeren:

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    Tijdens de installatie ziet u een dialoogvenster venster poping tot vragen voor het configureren van hieronder MySQL hoofdsite wachtwoord, en dat u nodig hebt Stel het wachtwoord hier.

    ![afbeelding](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)


    Het wachtwoord nogmaals ter bevestiging worden ingevoerd.

    ![afbeelding](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

- Stap 2: Login MySQL-Server

    Wanneer u klaar bent MySQL serverinstallatie wordt MySQL-service automatisch gestart. U kunt zich aanmeldt MySQL-Server met `root` gebruiker.
    Gebruik het onder de opdracht met een wachtwoord voor aanmelding en invoer.

             #[root@mysqlnode ~]# mysql -uroot -p

- Stap 3: De lopende MySQL-service beheren

    (a) krijgen de status van MySQL-service

             #[root@mysqlnode ~]# service mysql status

    (b) MySQL-Service starten

             #[root@mysqlnode ~]# service mysql start

    (c) MySQL-service stoppen

             #[root@mysqlnode ~]# service mysql stop

    (d) de MySQL-service opnieuw starten

             #[root@mysqlnode ~]# service mysql restart


###<a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Hoe u MySQL installeert op rood rol OS familie zoals CentOS, Oracle Linux
We gebruiken Linux VM hier met CentOS of Oracle Linux.

- Stap 1: De MySQL Yum opslagplaats schakeloptie toevoegen aan `root` gebruiker:

            #[azureuser@mysqlnode:~]sudo su -

    Download en installeer de release-pakket van MySQL:

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm

- Stap 2: Bewerk hieronder bestand zodat de MySQL-bibliotheek voor het downloaden van het pakket MySQL5.6.

            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    Elke waarde van dit bestand bijwerken in onder:

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- Stap 3: Installatiefout MySQL uit MySQL-bibliotheek MySQL installeren:

           #[root@mysqlnode ~]#yum install mysql-community-server

    MySQL-RPM-pakket en alle gerelateerde pakketten worden geïnstalleerd.

- Stap 4: De lopende MySQL-service beheren

    (a) Controleer de servicestatus van de MySQL-server:

           #[root@mysqlnode ~]#service mysqld status

    (b) Controleer of de standaard poort van MySQL-server wordt uitgevoerd:

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    (c) de MySQL-server starten:

           #[root@mysqlnode ~]#service mysqld start

    (d) stoppen de MySQL-server:

           #[root@mysqlnode ~]#service mysqld stop

    (e) MySQL instellen om te beginnen bij het opstarten-ups van systeem:

           #[root@mysqlnode ~]#chkconfig mysqld on


###<a name="how-to-install-mysql-on-suse-linux"></a>Hoe u MySQL installeert op SUSE Linux
We gebruiken Linux VM hier met OpenSUSE.

- Stap 1: Download en installeer MySQL-Server

    Overschakelen naar `root` gebruiker door onder de opdracht:  

           #sudo su -

    Download en installeer MySQL-pakket:

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- Stap 2: De lopende MySQL-service beheren

    (a) Controleer de status van de MySQL-server:

           #[root@mysqlnode ~]# rcmysql status

    (b) controleren of de standaardpoort naar de MySQL-server:

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    (c) de MySQL-server starten:

           #[root@mysqlnode ~]# rcmysql start

    (d) stoppen de MySQL-server:

           #[root@mysqlnode ~]# rcmysql stop

    (e) MySQL instellen om te beginnen bij het opstarten-ups van systeem:

           #[root@mysqlnode ~]# insserv mysql

###<a name="next-step"></a>Volgende stap
Zoeken naar meer gebruik en informatie over MySQL [hier](https://www.mysql.com/).
