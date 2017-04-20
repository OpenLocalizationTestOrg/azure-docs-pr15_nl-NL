<properties
    pageTitle="Linux en Open-Source Computing op Azure | Microsoft Azure"
    description="Een lijst Linux en Open-Source Computing artikelen op Azure, Linux basisgebruik, inclusief enkele basisconcepten bij het uitvoeren of Linux-afbeeldingen op Azure en andere inhoud over specifieke technologieën en optimalisaties uploaden."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux en open source computing op Azure

Zoek alle documentatie die u wilt maken en beheren van Linux gebaseerde virtuele machines in het implementatiemodel klassieke.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Aan de slag
- [Inleiding voor Linux op Azure](virtual-machines-linux-intro-on-azure.md)
- [Veelgestelde vragen over Azure virtuele Machines die is gemaakt met het implementatiemodel klassieke](virtual-machines-linux-classic-faq.md)
- [Over afbeeldingen voor virtuele machines](virtual-machines-linux-classic-about-images.md)
- [Uw eigen Distro afbeelding uploaden](virtual-machines-linux-classic-create-upload-vhd.md) (en ook instructies een [Azure-Endorsed verdeling](virtual-machines-linux-endorsed-distros.md)gebruikt)
- [Meld u aan bij een Linux VM gebruiken de Azure klassieke portal](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Instellen

- [Installeren van Azure opdrachtregel (Azure CLI)](../xplat-cli-install.md)


## <a name="tutorials"></a>Zelfstudies

- [De stapel licht installeren op een Linux virtuele machine in Azure wordt aangegeven](virtual-machines-linux-create-lamp-stack.md)
- [Ruby op Rails webtoepassing op een Azure-VM](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [Hoe u: installatiefout Apache Qpid Proton-C voor AMQP en Service Bus](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Databases
- [Prestaties van MySQL op Azure optimaliseren](virtual-machines-linux-classic-optimize-mysql.md)
- [MySQL-Clusters](virtual-machines-linux-classic-mysql-cluster.md)
- [Cassandra met Linux waarop Azure en toegang krijgen tot deze vanuit Node.js](virtual-machines-linux-classic-cassandra-nodejs.md)
- [Als u meerdere basispagina cluster MariaDbs maken](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Aan de slag met Linux berekeningscluster knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md)
- [NAMD uitvoeren met Microsoft HPC Pack op Linux berekeningscluster knooppunten in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [Instellen van een cluster Linux RDMA MPI toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Docker
- [Met de extensie Docker VM vanaf de opdrachtregel Azure Interface (Azure CLI)](virtual-machines-linux-classic-cli-use-docker.md)
- [Gebruik van de extensie Docker VM van de Azure-portal](virtual-machines-linux-classic-portal-use-docker.md)
- [Het gebruik van docker-machine op Azure](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Ubuntu
- [Hoe u: MySQL-Clusters](virtual-machines-linux-classic-mysql-cluster.md)
- [Hoe u: Node.js en Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [Hoe u: installeren en uitvoeren van MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>CoreOS
- [Hoe u: CoreOS op Azure gebruiken](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Planning
- [Richtlijnen voor de implementatie van Azure-infrastructuur services](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Linux gebruikersnamen selecteren](virtual-machines-linux-usernames.md)
- [Het configureren van een beschikbaarheid instellen voor virtuele machines in het implementatiemodel klassieke](virtual-machines-linux-classic-configure-availability.md)
- [Gepland onderhoud op Azure VMs plannen](virtual-machines-linux-planned-maintenance-schedule.md)
- [De beschikbaarheid van virtuele machines beheren](virtual-machines-linux-manage-availability.md)
- [Gepland onderhoud voor Linux virtuele machines in Azure wordt aangegeven](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Implementatie
- [Een aangepaste virtuele machine waarop Linux maken](virtual-machines-linux-classic-createportal.md)
- [De basisbeginselen: vastleggen van een Linux VM als een sjabloon wilt maken](virtual-machines-linux-classic-capture-image.md)
- [Informatie voor niet ondersteunde onderzoeken](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Projectmanagement

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Hoe u een wachtwoord of SSH eigenschappen voor Linux opnieuw instellen](virtual-machines-linux-classic-reset-access.md)
- [Gebruik van de hoofdsite](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Azure Resources

- [De Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md)
- [Azure VM Extensions en functies](virtual-machines-windows-extensions-features.md)
- [Aangepaste gegevens in een VM voor gebruik met de Cloud initialisatie invoegen](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Opslag

- [Een gegevensschijf koppelen aan een VM Linux](virtual-machines-linux-classic-attach-disk.md)
- [Een gegevensschijf uit een VM Linux loskoppelen](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Netwerken
- [Het instellen van de eindpunten van een klassieke virtuele machine in Azure wordt aangegeven](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Problemen oplossen
- [Problemen met Secure Shell (SSH)-verbindingen met Linux gebaseerde Azure virtuele machines](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Klassieke implementatie problemen met het maken van een nieuwe Linux virtuele machine in Azure wordt aangegeven](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Problemen oplossen klassieke implementatie met moet opnieuw worden gestart of de grootte van een bestaande Linux VM in Azure wordt aangegeven](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Verwijzing

- [Azure CLI-opdrachten in de modus voor beheer van Azure-Service (asm)](../virtual-machines-command-line-tools.md)
- [Azure servicebeheer REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Algemeen koppelingen
De volgende koppelingen zijn bedoeld voor Microsoft-blogs, Technet-pagina's, en externe sites plaats Azure.com documentatie als hierboven. Als zowel Azure en open source computing wereld snel bewegende worden doelen, is dat de volgende koppelingen vertonen een datum, *ondanks* het feit dat we onze aanbevolen continu nieuwere onderwerpen toevoegen en verwijderen van verouderde bestanden moeten doen. Als we een hebt gemist, geef laat het ons weten van de opmerkingen of halen te vragen om onze [GitHub cessies‑retrocessies](https://github.com/Azure/azure-content/).

- [ASP.NET-5 uitgevoerd op Linux Docker Containers gebruiken](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Het implementeren van een afbeelding van de VM CentOS uit OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [SUSE Update-infrastructuur](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [SUSE Linux Enterprise Server voor SAP Cloud toestel bibliotheek](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Samenstellen van maximaal beschikbare Linux op Azure in 12 stappen](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Inrichting Linux op Azure met Azure CLI, node.js, jhawk automatiseren](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [GlusterFS op Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [Actieve FreeBSD in Azure wordt aangegeven](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [Eenvoudig implementeren FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [Afbeelding van een aangepaste FreeBSD implementeren](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Kaspersky AV voor Linux bestandsserver](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [8 open source NoSql-Databases voor Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [Slideshare (MSOpenTech): Ervaringen met CouchDb op Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [CouchDB-als-een-Service met node.js, CORS en knorvis uitgevoerd](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [Bestand in Windows in de Cache Azure bestand Vgx. Service Vgx.](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [Aankondigen ASP.NET Session State-Provider voor de Preview-versie bestand Vgx.](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Blog: RavenHQ nu beschikbaar in de Azure Marketplace](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>BIG Data
- [Hadoop installatie op Azure Linux VMs](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relationele database
- [MySQL-hoog beschikbaarheid architectuur in Microsoft Azure wordt aangegeven](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Installeren van Postgres met corosync, pg_bouncer ILB gebruiken](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux high performance computing (HPC)

- [Quickstart sjabloon: kringvelden een cluster SLURM](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (en [blog publiceren](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [QuickStart sjabloon: een HPC-cluster met Linux berekeningscluster knooppunten maken](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops, beheer en optimalisatie

Als de wereld van devops, beheer en optimalisatie is helemaal uitgebreide en zeer snel wijzigen, moet u de onderstaande uitgangspunt overwegen.

- [Video: Azure virtuele Machines: Chef gebruiken, Puppet en Docker voor het beheren van Linux VMs](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Volledige handleiding voor het geautomatiseerde Kubernetes cluster-implementatie waarbij CoreOS en weefpatroon](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Jenkins Slave invoegtoepassing voor Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub cessies‑retrocessies: Jenkins opslag invoegtoepassing voor Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Derden: Invoegtoepassing voor Azure Hudson Slave](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Derden: Invoegtoepassing voor Azure Hudson opslag](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Video: Wat is Chef en hoe werkt dit?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Video: Hoe u Azure automatisering met Linux VMs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Blog: Hoe u Powershell DSC voor Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Docker Client DSC](https://github.com/anweiss/DockerClientDSC)

- [Verpakker-invoegtoepassing voor Azure](https://github.com/msopentech/packer-azure)
