<properties
    pageTitle="Goedgekeurd onderzoeken Linux | Microsoft Azure"
    description="Meer informatie over Linux op Azure ondersteunde onderzoeken, met inbegrip van de richtlijnen voor Ubuntu, OpenLogic, Oracle en SUSE."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>Linux op Azure ondersteunde onderzoeken

> [AZURE.NOTE] Als u even hebt, kunt u ons helpen de documentatie Azure Linux VM verbeteren door deze [Snelle enquête](https://aka.ms/linuxdocsurvey) van uw ervaringen. Elk antwoord kan wij help u uw werk te doen krijgt.

De Linux-afbeeldingen in de galerie met Azure of Marketplace worden verstrekt door een aantal partners en we werken met verschillende Linux-community's nog meer typen toevoegen aan de goedgekeurd distributielijst. Ondertussen voor onderzoeken die niet beschikbaar is in de galerie kunt u altijd voren-uw-eigenaar bent van-Linux door de richtlijnen op [deze pagina](virtual-machines-linux-classic-create-upload-vhd.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Ondersteunde onderzoeken en versies ##

De volgende tabel bevat de Linux onderzoeken en -versies die worden ondersteund op Azure. Raadpleeg tevens [ondersteuning voor Linux afbeeldingen in Microsoft Azure](https://support.microsoft.com/en-us/kb/2941892) voor meer informatie.

De Linux Integration Services (LIS)-stuurprogramma's voor Hyper-V en Azure zijn kernelmodules Microsoft rechtstreeks naar de boven Linux kernel vergeleken.  De LIS-stuurprogramma's ofwel in van de verdeling kernel al dan niet standaard zijn ingebouwd of voor oudere RHEL/CentOS gebaseerde onderzoeken zijn beschikbaar als een afzonderlijke download [hier](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Zie [dit artikel](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) voor meer informatie over de LIS-stuurprogramma's.

De Azure Linux-Agent is al vooraf geïnstalleerd op de galerie met Azure-afbeeldingen en uit van de verdeling pakket opslagplaats normaal gesproken beschikbaar bent.  Broncode kan worden gevonden op [GitHub](https://github.com/azure/walinuxagent).

Verdeling|Versie|Stuurprogramma 's|Agent
---|---|---|---
CentOS door OpenLogic | CentOS 6.3 +, 7.0 + | CentOS 6.3: [LIS downloaden](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: In de Kernel | Pakket: In [OpenLogic cessies‑retrocessies](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) onder "WALinuxAgent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | In Kernel | Broncode: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7,9 +, 8.2 + | In Kernel | Pakket: In cessies‑retrocessies onder "waagent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle Linux | 6.4 +, 7.0 + | In Kernel | Pakket: In cessies‑retrocessies onder "WALinuxAgent" <br/>Broncode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Rode rol Enterprise Linux | RHEL 6,7 +, 7.1 + | In Kernel|Pakket: In cessies‑retrocessies onder "WALinuxAgent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprise | SLES 11 SP4, SLES 12 SP1 + en <p> SLES voor SAP 11 SP3 + | In Kernel | Pakket: In de [Cloud: hulpmiddelen voor](https://build.opensuse.org/project/show/Cloud:Tools) cessies‑retrocessies onder "python-azure-agent" <br/>Broncode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | openSUSE 13.2 + | In Kernel | Pakket: In de [Cloud: hulpmiddelen voor](https://build.opensuse.org/project/show/Cloud:Tools) cessies‑retrocessies onder "python-azure-agent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04, 14.04, 16.04, 16,10 | In Kernel | Pakket: In cessies‑retrocessies onder "walinuxagent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Partners

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic is een toonaangevende leverancier van bedrijfsoplossingen de bron openen voor de cloud en het datacenter. OpenLogic helpt honderden enterprise voorloop in een breed scala van de industrie veilig ophalen, ondersteuning en bron openen software te beheren. OpenLogic biedt commerciële technische ondersteuning en vrijwaringsverplichtingen voor 600 bron openen-pakketten ondersteund door de OpenLogic Expert Community, inclusief enterprise niveau ondersteuning voor CentOS, evenals de partner starten voor het leveren van CentOS-installatiekopieën op Azure wordt.

### <a name="coreos"></a>CoreOS
[https://coreos.com/Docs/Running-coreos/cloud-providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Via de Website CoreOS:

*CoreOS is bedoeld voor beveiliging, consistentie en betrouwbaarheid. In plaats van pakketten via yum of huis installeert, wordt CoreOS Linux containers gebruikt voor het beheren van uw services op een hoger niveau abstractieniveaus. Een enkel servicecode en alle afhankelijkheden worden binnen een container die kan worden uitgevoerd op een of meer CoreOS machines geleverd.*


### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-blog/debian-images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ is een onafhankelijke consultingservices servicebedrijf en, gespecialiseerd in de ontwikkeling en uitvoering van professionele oplossingen met behulp van de gratis software. Als toonaangevende bron openen specialist heeft Credative internationale herkenning met veel IT-afdelingen met de ondersteuning. In combinatie met Microsoft, is Credativ momenteel voorbereiden corresponderende Debian afbeeldingen Debian 8 (Jessie) en Debian vóór 7 (Wheezy), die speciaal zijn ontworpen om uit te voeren op Azure en eenvoudig kan worden beheerd via het platform. Credativ ondersteunen ook de lange termijn onderhoud en bijwerken van de Debian afbeeldingen voor Azure via de Centers openen bron ondersteuning.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/Topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Van Oracle strategie is voor het aanbieden van een breed scala aan oplossingen voor openbare en persoonlijke wolken, terwijl u klanten keuze en flexibiliteit in hoe ze Oracle-software in Oracle wolken, evenals andere wolken implementeren.  Oracle van samenwerking met Microsoft kan klanten Oracle-software in Microsoft openbare en persoonlijke wolken probleemloos van certificering implementeren en te ondersteunen van Oracle.  Oracle van resourcebeperkingen en investering in Oracle-oplossingen voor openbare en persoonlijke cloud blijft ongewijzigd.

### <a name="red-hat"></a>Rode rol
[http://www.RedHat.com/en/partners/Strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

werelds toonaangevende leverancier van oplossingen van de bron openen, rode rol helpt meer dan 90% van Fortune 500 bedrijven zakelijke problemen oplossen, hun IT uitlijnen en bedrijven strategieën, en voorbereiden voor de toekomst van technologie. Rood rol biedt veilige oplossingen via een model geopend en een abonnementsmodel betaalbare, overzichtelijk.

### <a name="suse"></a>SUSE
[http://www.SUSE.com/SUSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server op Azure is een beproefde platform vindt u bovenliggende betrouwbaarheid en beveiliging voor cloud computing. De SUSE veelzijdige Linux platform wordt naadloos integreren met Azure cloudservices voor het leveren van een eenvoudige cloud-omgeving. En met meer dan 9,200 gecertificeerde toepassingen uit meer dan 1800 onafhankelijke softwareleveranciers voor SUSE Linux Enterprise Server, SUSE zorgt ervoor dat werkbelasting ondersteunde uitgevoerd in het midden van de gegevens vertrouwen kunnen worden geïmplementeerd op Azure.

### <a name="canonical"></a>Canonieke
[http://www.Ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Canonieke engineering en open Gemeenschap beheermodel station Ubuntu van success in client, server en cloud computing, inclusief persoonlijke cloudservices voor consumenten. Canonieke van visuele van een geïntegreerd gratis platform in Ubuntu, via de telefoon naar cloud, met een reeks stroken interfaces voor de telefoon, tablet, TV en bureaublad, kunt u de eerste keuze voor verschillende instellingen van openbare cloud providers naar de makers van consumenten elektronische en een favoriet tussen afzonderlijke technologen Ubuntu.

Met ontwikkelaars en technische centers overal ter wereld, Canonical uniek wordt geplaatst als u wilt samenwerken met hardware makers, inhoudsproviders en softwareontwikkelaars om Ubuntu oplossingen weer te geven op de markt, vanaf pc's op servers en draagbare apparaten.

