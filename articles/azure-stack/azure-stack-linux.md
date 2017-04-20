<properties
    pageTitle="Linux gasten op Azure Stack | Microsoft Azure"
    description="Leer hoe Linux gebaseerde virtuele machines op Azure stapel maken."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>Linux virtuele machines op Azure Stack implementeren

U kunt Linux virtuele machines op de Azure stapel Haalbaarheidstest implementeren door een afbeelding op basis van Linux toe te voegen in de stapel Azure Marketplace. Verschillende Linux leveranciers, hebben opgegeven afbeeldingen die kunnen worden toegevoegd in een Azure stapel Haalbaarheidstest of u kunt maken op uw eigen.

## <a name="download-an-image"></a>Downloaden van een afbeelding

 1. Download en een afbeelding Azure stapel-compatibele extraheren uit de volgende koppelingen of uw eigen voorbereiden:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Afbeelding van de VHD zo nodig en [toevoegen van de afbeelding naar de marktplaats](azure-stack-add-vm-image.md)ophalen. Zorg ervoor dat de `OSType` parameter is ingesteld op `Linux`.
 
 3. Nadat u de afbeelding hebt toegevoegd aan de Marketplace, een item Marketplace is gemaakt en u kunt een virtuele Linux machine implementeren.
  
## <a name="prepare-your-own-image"></a>Uw eigen afbeelding voorbereiden

1. Uw eigen Linux-afbeelding met een van de volgende instructies voorbereiden:
 - [CentOS gebaseerde onderzoeken](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Rode rol Enterprise Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Download en installeer de [Azure Linux-Agent](https://github.com/Azure/WALinuxAgent/)

    De Azure Linux-Agent versie 2.1.3 of hoger is vereist voor het inrichten van uw VM Linux op Azure-Stack. Veel van de bovenstaande al onderzoeken deze versie van de agent of hoger als een pakket opnemen in hun opslagplaatsen (meestal genoemd `WALinuxAgent` of `walinuxagent`). Echter als de versie van het pakket Azure-agent minder dan 2.1.3 is (dat wil zeggen 2.0.18 of lager), en vervolgens moet u de agent handmatig installeren. De geïnstalleerde versie kan worden bepaald van de pakketnaam of door te voeren `/usr/sbin/waagent -version` op de VM.

    Volg de onderstaande instructies voor het installeren van de Azure Linux-agent handmatig:

 - Download eerst de meest recente Azure Linux-agent van [Github](https://github.com/Azure/WALinuxAgent/releases), voorbeeld:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - Uitpakken de Azure-agent:

            # tar -vzxf v2.2.0.tar.gz

 - Python-setuptools installeren

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - Installeer de Azure-agent:

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Systemen met Python 2.x en 3.x geïnstalleerd naast elkaar Python mogelijk moet u de volgende opdracht uit te voeren:

        # sudo python3 setup.py install --register-service

    Zie de Azure Linux-Agent [Leesmij](https://github.com/Azure/WALinuxAgent/blob/master/README.md)voor meer informatie.

3. [De afbeelding naar de marktplaats toevoegen](azure-stack-add-vm-image.md). Zorg ervoor dat de `OSType` parameter is ingesteld op `Linux`.

4. Nadat u de afbeelding hebt toegevoegd aan de Marketplace, een item Marketplace is gemaakt en u kunt een virtuele Linux machine implementeren.

## <a name="next-steps"></a>Volgende stappen

[Veelgestelde vragen over Azure-Stack](azure-stack-faq.md)