<properties
   pageTitle="Introductiehandleiding voor handmatige installatie van SAP HANA op Azure VMs | Microsoft Azure"
   description="Introductiehandleiding voor handmatige installatie van SAP HANA op Azure VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="quickstart-guide-for-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Introductiehandleiding voor handmatige installatie van één exemplaar SAP HANA op Azure VMs

## <a name="introduction"></a>Inleiding

Deze handleiding helpt voor het instellen van een enkel exemplaar SAP HANA prototype/demo systeem op Azure VMs door een handmatige installatie van SAP NetWeaver 7.5 en SAP HANA SP12.
De handleiding wordt ervan uitgegaan dat de lezer vertrouwd met Azure IaaS basistaken, zoals het implementeren van virtuele machines of virtuele netwerken hetzij via de portal van Azure of Powershell/CLI is, inclusief de optie voor het gebruik van json-sjablonen. Bovendien wordt naar verwachting dat de lezer vertrouwd met SAP-HANA, SAP NetWeaver en hoe u het kunt installeren on-premises implementatie is.

Wordt naar verwachting dat de lezer op de hoogte van de algemene SAP-Azure-documentatie die worden genoemd in de sectie Algemene informatie aan het einde van het artikel is.

Vanwege de beperking naar niet-productieve systemen voor deze handleiding niet onderwerpen zoals HA, wordt besproken back-up maken, DR, krachtige of speciale beveiligingsoverwegingen.

Het instellen van de steekproef is klaar met twee virtuele machines voor het uitvoeren van een gedistribueerde SAP NetWeaver-installatie via het model Azure resourcemanager zoals SAP-Linux-Azure wordt alleen ondersteund op op Azure resourcemanager en niet het klassieke model. In de sectie Algemene informatie aan het einde van dit artikel vindt u koppelingen naar meer informatie over Azure resourcemanager.

Dit zijn de twee test VMs gebruikt voor de steekproef-installatie:

* Hana-appsrv (type DS3) voor het hosten van de NW 7.5 ASC's exemplaar + PAS
* Hana-dbsrv (type GS4) aan host HANA SP12
* beide VMs behoorden tot één Azure virtuele netwerk (azure-hana-test-vnet)
* OS in beide gevallen is SLES 12 SP1


Let op van het feit maar dat vanaf juli 2016 SAP HANA wordt alleen volledig ondersteund voor OLAP-(zwart/wit) systemen van Azure VM type GS5. Voor testdoeleinden waar een niet officiële SAP-ondersteuning verwacht is het geen probleem een kleinere zoals bijvoorbeeld GS4 gebruikt.
Wat moet altijd worden gebruikt voor SAP-HANA op Azure Azure premium-opslag voor HANA gegevens en logbestanden is - raadpleegt u de 'schijf Setup' sectie verder naar beneden. Controleer de sectie Algemene informatie aan het einde van dit artikel voor meer informatie over welke SAP producten op Azure worden ondersteund.


De gids worden twee verschillende manieren handmatig installeren SAP HANA op Azure VMs beschreven:

* SAP HANA via SAP Software inrichting Manager (SWPM) installeren als onderdeel van een gedistribueerde NetWeaver-installatie in de stap 'databaseinstantie'
* installeren SAP HANA met het hulpmiddel-hdblcm HANA levenscyclus Manager en vervolgens NetWeaver achteraf

Het is ook alle onderdelen (SAP HANA, SAP-toepassingsserver, ASC's instantie, SAP grafische gebruikersinterface) in één enkele VM installeren en gebruiken van SWPM. Deze optie niet wordt beschreven in dit artikel, maar de items die moeten worden beschouwd als dezelfde zijn.

Voordat u begint met een installatie van worden de sectie na de controlelijsten onder over het instellen van de Azure test VMs gelezen om verschillende eenvoudige fouten die gebeurt er wanneer u gebruikt alleen een standaard Azure VM configuratie te voorkomen.


## <a name="checklist-sap-hana-installation-via-sap-swpm"></a>Controlelijst SAP HANA installatie via een SAP-SWPM

Dit is een eenvoudige controlelijst met de code die items met betrekking tot een handmatige installatie van één exemplaar SAP HANA voor demo of prototypen pursposes via SAP SWPM wijze een verdeelde SAP NW 7.5 installeren. De afzonderlijke items zijn uitleg over en wordt weergegeven in de vorm van schermafbeeldingen uitgebreider overal in het artikel:

* een Azure virtuele netwerk met daarin de twee test VMs maken 
* twee Azure VMs met OS SLES 12 SP1 via Azure resourcemanager model implementeren 
* twee standaard schijven als bijlage toevoegen aan de app-server VM (bijvoorbeeld 75GB en 500GB)
* vier schijven als bijlage toevoegen aan de server HANA DB VM - 2 standaard schijven zoals voor de app-server VM + 2 premium schijven (bijvoorbeeld 2x512GB)
* afhankelijk van de grootte en/of doorvoer vereisten koppelen meerdere schijven en gestreepte volumes ofwel maken met lvm of mdadm op OS niveau binnen de VM
* XFS bestandssystemen maken in de gekoppelde schijven- / logische volumes
* Koppel de nieuwe XFS bestandssystemen op OS niveau. Gebruik een bestandssysteem om alle SAP-software en de andere waarde bijvoorbeeld de sapmnt directory en wellicht back-ups te houden. Klik op het SAP HANA DB server mountains de XFS systemen bestand op de premium-schijven als /hana en /usr/sap dit alles nodig is om te voorkomen dat de hoofdmap bestandssysteem die geen mailserver te groot op Linux Azure VMs is vol is
* Voer de lokaal IP-adressen van de test VMs in/enz/hosts
* Voer de parameter nofail in /etc/fstab
* kernel parameters instellen op basis van de notitie HANA-SLES-12-SAP (Zie details lager in de sectie van de maximumconcentratie kernel)
* omwisselen ruimte toevoegen
* Als het gewenste type - installeert u een grafische bureaublad op de toets VMs. Een externe sapinst installatie anderszins te gebruiken
* de SAP-software downloaden van SAP service marketplace
* het exemplaar SAP ASC's op de app-server VM installeren
* delen van de map sapmnt via NFS tussen de test VMs (app-server VM is de NFS-server)
* een exemplaar van de database HANA via SWPM inclusief op de server DB VM installeren
* de PAS installeren op de app-server VM
* SAP MC starten en verbindt u bijvoorbeeld via SAP grafische gebruikersinterface / HANA Studio 



## <a name="checklist-sap-hana-installation-via-hdblcm"></a>Controlelijst SAP HANA installatie via hdblcm

Dit is een eenvoudige controlelijst met de code die items met betrekking tot een handmatige installatie van één exemplaar SAP HANA voor demo of prototypen pursposes via SAP SWPM wijze een verdeelde SAP NW 7.5 installeren. De afzonderlijke items zijn uitleg over en wordt weergegeven in de vorm van schermafbeeldingen uitgebreider overal in het artikel:

* een Azure virtuele netwerk met daarin de twee test VMs maken 
* twee Azure VMs met OS SLES 12 SP1 via Azure resourcemanager model implementeren 
* twee standaard schijven als bijlage toevoegen aan de app-server VM (bijvoorbeeld 75GB en 500GB)
* vier schijven als bijlage toevoegen aan de server HANA DB VM - 2 standaard opslag zoals voor de app-server VM + 2 premium schijven (bijvoorbeeld 2x512GB)
* afhankelijk van de grootte en/of doorvoer vereisten koppelen meerdere schijven en gestreepte volumes ofwel maken met lvm of mdadm op OS niveau binnen de VM
* XFS bestandssystemen maken in de gekoppelde schijven- / logische volumes
* Koppel de nieuwe XFS bestandssystemen op OS niveau. Gebruik een bestandssysteem om alle SAP-software en de andere waarde bijvoorbeeld de sapmnt directory en wellicht back-ups te houden. Klik op het SAP HANA DB server mountains de XFS systemen bestand op de premium-schijven als /hana en /usr/sap dit alles nodig is om te voorkomen dat de hoofdmap bestandssysteem die geen mailserver te groot op Linux Azure VMs is vol is
* Voer de lokaal IP-adressen van de test VMs in/enz/hosts
* Voer de parameter nofail in /etc/fstab
* kernel parameters instellen op basis van de notitie HANA-SLES-12-SAP (Zie details lager in de sectie van de maximumconcentratie kernel)
* omwisselen ruimte toevoegen
* Als het gewenste type - installeert u een grafische bureaublad op de toets VMs. Een externe sapinst installatie anderszins te gebruiken
* de SAP-software downloaden van SAP service marketplace
* groep "sapsys" met groeps-id 1001 op de HANA DB Server VM maken
* SAP HANA installeren op de databaseserver VM hdblcm gebruiken
* het exemplaar SAP ASC's op de app-server VM installeren
* delen van de map sapmnt via NFS tussen de test VMs (app-server VM is de NFS-server)
* een exemplaar van de database HANA via SWPM inclusief op de server DB VM installeren
* de PAS installeren op de app-server VM
* SAP MC starten en verbindt u bijvoorbeeld via SAP grafische gebruikersinterface / HANA Studio 




## <a name="prepare-azure-vms-for-manual-installation-of-sap-hana"></a>Handmatige installatie van SAP HANA van Azure VMs voorbereiden

Dit hoofdstuk over het voorbereiden van de Azure VMs voor handmatige installatie van SAP HANA bestaat uit vijf secties die betrekking hebben op de volgende onderwerpen:

* Schijf-instelling
* Kernel Parameters
* FILESYSTEMS
* / enz/hosts
* / enzovoort/fstab


### <a name="disk-setup"></a>Schijf-instelling

Het bestandssysteem hoofdmap in een VM Linux op Azure is van beperkte grootte. Daarom moet u extra schijfruimte als bijlage toevoegen aan een VM voor het uitvoeren van SAP. Voor een SAP-app-server die VM in een puur prototype/demo-omgeving gebruikt is het geen probleem Azure standaard opslag schijven te gebruiken. Terwijl voor de SAP HANA DB gegevens en logbestanden - moeten Azure Premium schijven zelfs in een niet-productieve liggend worden gebruikt.

Sommige meer informatie over hoe u schijven koppelt aan een VM Linux [hier](virtual-machines-linux-add-disk.md)

Als het gaat om schijfcaching van Azure - moet een "Geen" voor schijven die worden gebruikt voor de opslag van de HANA transactie-Logboeken gebruiken. HANA verderop gegevensbestanden is het ok gebruiken in cache opslaan. Als HANA, een database in het geheugen is dit hangt af van de algehele gebruikspatroon hoeveel de gelezen cache op Azure schijf niveau worden de prestaties (bijvoorbeeld HANA starten en het lezen van gegevens vanaf schijf in het geheugen).

Meer informatie over de opslag van Azure Premium [hier](../storage/storage-premium-storage.md)

[Hier](https://github.com/Azure/azure-quickstart-templates) u json voorbeeldsjablonen maken VMs vinden.
De "101-vm-eenvoudige-linux' ziet u hoe een basissjabloon eruitziet met inbegrip van de sectie opslag waarmee een schijf met 100GB gegevens worden toegevoegd.

[In dit artikel](virtual-machines-linux-sap-on-suse-quickstart.md) bevat informatie over het zoeken naar de afbeelding van een SUSE via Powershell of CLI en de urgentie naar een schijf via UUID hebt toegevoegd.


Afhankelijk van de grootte van de systeem- en doorvoer vereisten kan het nodig zijn om meerdere schijven in plaats van één bijvoegen en een streep instellen op deze schijven op OS niveau hoger op te maken. Dit zijn de twee aspecten waarom een een streep instellen over meerdere Azure schijven wilt maken:

* doorvoer vergroten
* een enkel bestandssysteem > 1TB nodig hebt, zoals de huidige groottelimiet overschrijden voor Azure schijf 1TB (vanaf juli 2016 is)


Meer informatie over de twee belangrijkste hulpmiddelen voor het configureren van gesegmenteerd te verdelen vindt u hier:

[Artikel over het gebruik van mdadm Linux software raid op een VM Azure configureren](virtual-machines-linux-configure-raid.md)

[Artikel over het beheer van logische Volume configureren op een Linux Azure VM](virtual-machines-linux-configure-lvm.md)



![](./media/virtual-machines-linux-sap-hana-get-started/image003.jpg)

In de test zijn omgeving twee Azure standaard schijven gekoppeld aan de SAP-app-server VM. Een is gebruikt voor het opslaan van de SAP-software voor installatie (bijvoorbeeld NetWeaver 7.5 hebt, SAP-gebruikersinterface, SAP HANA... ) en de andere waarde naar voldoende ruimte voor datgene vereist zijn mogelijk (bijvoorbeeld back-up maken, testgegevens) en de adreslijst sapmnt (bijvoorbeeld SAP profiles) worden gedeeld met alle VMs die deel uitmaken van de dezelfde SAP-Liggend.

![](./media/virtual-machines-linux-sap-hana-get-started/image004.jpg)

VM vier schijven zijn in tegenstelling tot de app-server aangesloten op de server SAP HANA VM. Zoals voordat twee schijven zijn gebruikt voor het behouden van de SAP-software (een kan ook de schijf van de software SAP via NFS delen) en problemen onvoldoende ruimte bijvoorbeeld voor back-up. De extra twee schijven zijn Azure Premium schijven wilt behouden SAP HANA gegevens- en logbestanden bestanden, evenals de map /usr/sap.


### <a name="kernel-parameters"></a>Kernel parameters


SAP HANA vereist specifieke instellingen van de Linux kernel die geen deel uitmaken van de met standaard Azure-galerijafbeeldingen en moeten handmatig worden ingesteld. Er is een specifieke SAP-notitie waarin de instellingen. 


SAP Opmerking SAP HANA DB: Aanbevolen OS instellingen voor SLES 12 / SLES voor SAP-toepassingen 12: [SAP notitie 2205917](https://launchpad.support.sap.com/#/notes/2205917)

Een extra onderwerp reagrding pagina-cache gerelateerd aan het SAP HANA uitgevoerd op SLES vindt u [hier](https://www.suse.com/documentation/sles_for_sap/singlehtml/sles_for_sap_guide/sles_for_sap_guide.html#sec.s4s.configure.page-cache) in hoofdstuk 6.1 Kernel: pagina-Cache limiet

Er is ook een SAP-opmerking over de cache voor pagina-limiet [SAP notitie 1557506](https://service.sap.com/sap/support/notes/1557506)

SLES 12 heeft een nieuw hulpmiddel die het oude sapconf hulpprogramma vervangt. "In-of-adm" is en er is een speciale SAP HANA profiel beschikbaar. Een vindt meer informatie over dit hulpprogramma de onderstaande koppelingen volgen.

SLES documentatie over afgestemd adm profiel sap-hana vindt u [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html)

SLES documentatie over afgestemd adm sap-hana - profiel hoofdstuk 6.2 optimaliseren systemen voor SAP-werkbelasting met afgestemd-adm - vindt u [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf)


![](./media/virtual-machines-linux-sap-hana-get-started/image005.jpg)

Hier kunnen zien hoe de transparent_hugepage, evenals de numa_balancing waarden op basis van de vereiste HANA SAP-instellingen worden gewijzigd in 'in-of-adm'.


Als u de instellingen van de kernel SAP HANA permanent wilt hebben tot grub2 op SLES 12 gebruiken. Meer informatie over grub2 vindt u [hier](https://www.suse.com/documentation/sled-12/book_sle_admin/data/sec_grub2_file_structure.html)


![](./media/virtual-machines-linux-sap-hana-get-started/image006.jpg)

Deze schermafbeelding ziet u hoe de kernel-instellingen zijn gewijzigd in het configuratiebestand en vervolgens "gecompileerd" via grub2-mkconfig


![](./media/virtual-machines-linux-sap-hana-get-started/image007.jpg)

Een andere optie zou om de instellingen via Yast en de opstartlaadprogramma kernel parameterinstellingen te wijzigen.


### <a name="filesystems"></a>FILESYSTEMS 

![](./media/virtual-machines-linux-sap-hana-get-started/image008.jpg)

Hier ziet een de twee bestands-systemen die zijn gemaakt op de SAP-app-server VM boven aan de twee gekoppelde Azure standaard schijven. Beide filesystems zijn van het type XFS en /sapdata en /sapsoftware geladen.

Het is niet verplicht te deze methode. Er zijn verschillende opties hoe u de schijfruimte sturcture.
Het belangrijkste is om te voorkomen dat het bestandssysteem hoofdsite ruimte wordt uitgevoerd. 


![](./media/virtual-machines-linux-sap-hana-get-started/image009.jpg)

Met betrekking tot de SAP HANA DB VM het is belangrijk te weten dat tijdens de installatie van een database via sapinst (swpm) en alleen met de optie eenvoudige "" standaardinstallatie wordt geïnstalleerd items al dan niet standaard onder /hana en /usr/sap. De standaardinstelling voor back-up van SAP HANA is bijvoorbeeld onder /usr/sap.
Vind ik leuk voordat deze heel belangrijk om te voorkomen is dat het bestandssysteem hoofdsite ruimte wordt uitgevoerd. Daarom moet een Zorg ervoor dat er voldoende ruimte onder /hana en /usr/sap voordat u SAP HANA installeert via swpm.

[In dit artikel](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) uit SAP beschrijft de indeling standaard bestandssysteem van SAP HANA 


![](./media/virtual-machines-linux-sap-hana-get-started/image010.jpg)

Tijdens de installatie van SAP NetWeaver op een standaard afbeelding SLES 12 Azure wordt er een bericht dat er geen spatie uitwisselen. Als u wilt verwijderen van dit bericht kan een bijvoorbeeld handmatig toevoegen een bestand omwisselen zoals is beschreven in dit document via dd, mkswap en swapon. Alleen zoeken naar 'Toe te voegen een uitwisselen bestand handmatig' in [dit artikel](https://www.suse.com/documentation/sled-12/book_sle_deployment/data/sec_yast2_i_y2_part_expert.html)

Er is een andere optie voor het configureren van omwisselen ruimte via de Linux VM-agent. Meer informatie vindt u [hier](virtual-machines-linux-agent-user-guide.md)


### <a name="etchosts"></a>/ enz/hosts

![](./media/virtual-machines-linux-sap-hana-get-started/image011.jpg)

Een ander belangrijk aspect voordat u begint met het installeren van SAP is hostnamen en IP-adressen van de SAP-VMs opnemen in het bestand/etc/hosts. Een moet alle SAP-VMs binnen één Azure virtuele netwerk implementeren en gebruik vervolgens de interne IP-adressen.

### <a name="etcfstab"></a>/ enzovoort/fstab

![](./media/virtual-machines-linux-sap-hana-get-started/image000c.jpg)

Tijdens de testfase blijkt te worden van een goed idee om de parameter nofail toevoegen aan fstab. Als er iets mis met het schijven gaat zou de VM de nog steeds bovenkomen en niet vastloopt in het proces te starten. Maar hebben tot Let zoals in dit geval de extra schijfruimte mogelijk niet beschikbaar zijn en processen het bestandssysteem hoofdsite kunnen opvullen. Geval /hana zou ontbrekende wouldn't SAP HANA Hoewel helemaal starten.


## <a name="install-graphical-gnome-desktop-on-sles-12"></a>Grafische Gnome bureaublad installeren op SLES 12

Dit hoofdstuk bestaat uit twee secitions die betrekking hebben op de volgende onderwerpen:

* Installatie van Gnome bureaublad en xrdp op SLES 12
* Java gebaseerde SAP MC met Firefox op SLES 12 uitgevoerd

Een kan ook alternatieven zoals Xterminal, VNC gebruiken, maar vanaf Sep 2016 in dit artikel alleen xrdp worden.

### <a name="installation-of-gnome-desktop-and-xrdp-on-sles-12"></a>Installatie van Gnome bureaublad en xrdp op SLES 12

Er is een eenvoudige manier hiervoor met name voor mensen die u hebt Microsoft Windows achtergrond en wilt een grafische bureaublad rechtstreeks vanuit het SAP Linux VMs gebruiken voor het uitvoeren van Firefox, Sapinst, SAP grafische gebruikersinterface, SAP MC of HANA Studio en wellicht verbinding maken met de VM via RDP vanuit een Microsoft Windows-computer. Hoewel dit mogelijk niet geschikt te maken voor een database productieserver bijvoorbeeld is het ok voor een puur prototype/demo-omgeving. Hier volgen de stappen voor het bureaublad Gnome installeren op een Azure SLES 12 VM:

Installeer het bureaublad gnome door de volgende opdracht (bijvoorbeeld in een venster stopverf):

   zypper in -t patroon gnome basic

Installeer vervolgens xrdp om toe te staan verbinding met de VM via RDP:

   zypper in xrdp

/etc/sysconfig/windowmanager bewerken en stel het standaardbeheer van windows op Gnome:

   DEFAULT_WM = "gnome"

chkconfig om ervoor te zorgen dat xrdp automatisch wordt gestart na het opnieuw opstarten uitvoeren: 

  chkconfig-niveau 3 xrdp op

geval moet er een probleem met de RDP-verbinding probeert te starten (wellicht afmelden bij een stopverf venster):

  /etc/xrdp/xrdp.sh opnieuw starten

geval de xrdp opnieuw starten als hierboven genoemde niet selectievakje werken als er een bestand .pid en deze te verwijderen:

  /var/run controleren en kijk of xrdp.pid   
  Verwijder deze en probeer het opnieuw starten opnieuw



### <a name="sap-mc"></a>SAP MC


Als u wilt starten van de grafische Java gebaseerde SAP MC afmelden bij Firefox uitgevoerd in een Azure SLES 12 VM na de installatie van de Gnome krijgt bureaubaldachtergrond zoals is beschreven in de vorige sectie een een fout vanwege ontbrekende Java-browser-invoegtoepassing.

De URL voor het starten van de SAP-MC is <server>: 5 < instance_number > 13

Meer informatie vindt u [hier](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm)


![](./media/virtual-machines-linux-sap-hana-get-started/image013.jpg)

Klik op de bovenstaande schermafbeelding kunnen zien hoe het foutbericht eruit als wanneer de invoegtoepassing voor Java-Browser ontbreekt. 

![](./media/virtual-machines-linux-sap-hana-get-started/image014.jpg)

Eén optie voor het oplossen van het probleem is gewoon voor het installeren van de ontbrekende invoegtoepassing via Yast.

![](./media/virtual-machines-linux-sap-hana-get-started/image015.jpg)

De URL SAP MC herhalende brengt wat dialoogvenster de eerste keer vragen om te activeren van de invoegtoepassing.


Een aanvullende actie-item dat mogelijk pop-up wordt een foutbericht met betrekking tot een ontbrekende bestand: dit is waarschijnlijk zijn gerelateerd aan de installatie van Java 1,8 die vereist voor SAP grafische gebruikersinterface 7.4 is javafx.properties

De IBM Java-versie via Yast zichtbaar zijn niet opgenomen in dit bestand. De oplossing is het downloaden van een Java van Oracle.
Een artikel waarin moment over dit specifieke probleem spreekt vindt u [hier](https://scn.sap.com/thread/3908306)



## <a name="manual-sap-hana-installation-via-swpm-as-part-of-a-netweaver-75-installation"></a>Handmatige SAP HANA installatie via SWPM als onderdeel van een NetWeaver 7.5-installatie


De volgende lijst met schermafbeeldingen ziet u de belangrijkste stappen van de installatie van SAP NetWeaver 7.5 en SAP HANA SP12 via SWPM (sapinst). Als onderdeel van een 7.5 NW beschikt installatie SWPM over de mogelijkheden ook de database HANA installeren als één exemplaar.


![](./media/virtual-machines-linux-sap-hana-get-started/image012.jpg)

Voor de steekproef-test is omgeving slechts één ABAP app-server geïnstalleerd. De optie "Distributed System" is gebruikt voor het installeren van het exemplaar ASC's en de primaire-App-server-instantie in één Azure VM en SAP HANA als de databasesysteem in een andere Azure VM.


![](./media/virtual-machines-linux-sap-hana-get-started/image016.jpg)

Zodra het exemplaar ASC's op de app-server VM is geïnstalleerd en is ingesteld op 'groen' in de SAP MC de sapmnt map waaronder bijvoorbeeld de map in de SAP-profiel moet worden gedeeld met de server SAP HANA DB VM heeft.
De stap van de installatie DB nodig toegang tot deze informatie. De beste manier is NFS die kan worden geconfigureerd met Yast gebruiken.


![](./media/virtual-machines-linux-sap-hana-get-started/image017b.jpg)

Klik op de app moet server VM de adreslijst sapmnt via NFS worden gedeeld met behulp van de opties "rw" en "no_root_squash". Standaard is "ro" en "root_squash" kunnen leiden tot problemen bij het installeren van het exemplaar van de database.


![](./media/virtual-machines-linux-sap-hana-get-started/image018b.jpg)

Klik op de server SAP HANA DB VM de sapmnt delen van de app-server VM moet worden geconfigureerd via 'NFS-client' (bijvoorbeeld met behulp van Yast)


![](./media/virtual-machines-linux-sap-hana-get-started/image019.jpg)

Een heeft vervolgens aanmelden bij de server SAP HANA DB VM en de SWPM als u wilt uitvoeren van de volgende stap in een gedistribueerde NW 7.5 installatie - 'databaseinstantie' starten.


![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Zijn gerelateerd aan de installatie van het SAP HANA er niet is in feite te veel om in te voeren nadat u een "" standaardinstallatie hebt geselecteerd. Naast heeft het pad naar de installatiom media een een DB beveiligings-id, de hostnaam, het getal exemplaar en de DB Sys-beheerderswachtwoord invoeren.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Volgende stap is het wachtwoord voor het schema DBACOCKPIT invoeren.



![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Vervolgens wordt de vraag voor het wachtwoord van de schema SAPABAP1 geleverd.


![](./media/virtual-machines-linux-sap-hana-get-started/image023.jpg)

Aan het einde moet er alleen een groen vinkje vóór elke fase van het installatieproces DB en het weinig berichtvak dat verschijnt moet ik me "uitvoering van... Exemplaar van de database is voltooid".

 
![](./media/virtual-machines-linux-sap-hana-get-started/image024.jpg)

Nadat de installatie de SAP-MC ook het exemplaar DB als 'groene' en de volledige lijst met SAP-HANA processen (bijvoorbeeld hdbindexserver, hdbcompileserver) moet worden weergegeven


![](./media/virtual-machines-linux-sap-hana-get-started/image025.jpg)

Deze schermafbeelding ziet u onderdelen van de bestandsstructuur onder /hana/shared die SWPM tijdens de installatie HANA gemaakt. Er is geen optie om op te geven van een ander pad. Daarom zo is belangrijk om te koppelen extra schijfruimte onder /hana vóór de HANA SAP-installatie via SWPM om te voorkomen dat het bestandssysteem hoofdsite afmelden bij de beschikbare ruimte wordt uitgevoerd.


![](./media/virtual-machines-linux-sap-hana-get-started/image026.jpg)

Hier ziet een hetzelfde zoals is beschreven voordat u voor de map /usr/sap.


![](./media/virtual-machines-linux-sap-hana-get-started/image027b.jpg)

De laatste stap van de gedistribueerde ABAP installatie is de "primaire toepassing Server-instantie"


![](./media/virtual-machines-linux-sap-hana-get-started/image028b.jpg)

Zodra de PAS, evenals de grafische gebruikersinterface SAP hebt geïnstalleerd kunt een via de transactie "dbacockpit" dat alles er rechts met de installatie SAP HANA controleren.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Stap 1 kan als een laatste SAP HANA Studio hebt geïnstalleerd in de SAP-app-server VM en verbinding maken met het SAP HANA exemplaar uitgevoerd op de server DB VM.




## <a name="manual-sap-hana-installation-via-hana-life-cycle-manager-tool-hdblcm"></a>Handmatige SAP HANA installatie via HANA levenscyclus manager hulpmiddel hdblcm


Naast het installeren van SAP HANA als onderdeel van een gedistribueerde installatie via SWPM is het ook realistisch mogelijke eerst installeren HANA zelfstandige met hdblcm en installeer vervolgens bijvoorbeeld SAP NetWeaver 7.5 achteraf. De lijst met schermafbeeldingen hieronder ziet u hoe dit werkt.

Hier zijn drie bronnen van informatie over het HANA hdblcm hulpprogramma:

[De juiste SAP HANA HDBLCM voor uw taak kiezen](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)

[SAP HANA levenscyclus beheerprogramma 's](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)

[SAP HANA serverinstallatie- en Update-handleiding](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)



![](./media/virtual-machines-linux-sap-hana-get-started/image030.jpg)

Naar kunt u voorkomen dat in problemen met een standaard-id-instelling voor groep hoger op de \<HANA SID\>adm gebruiker (gemaakt door het hulpprogramma hdblcm) een moet een nieuwe groep met de naam "sapsys" met groeps-id 1001 voordat u installeert SAP HANA met hdblcm definiëren.




![](./media/virtual-machines-linux-sap-hana-get-started/image031.jpg)

Begintijd hdblcm de eerste er een eenvoudige startmenu waar hebben is tot het item selecteren 1 "installeren nieuwe systeem"



![](./media/virtual-machines-linux-sap-hana-get-started/image032.jpg)

Klik op deze schermafbeelding kunnen de belangrijkste opties die zijn ingevoerd voordat u zien. Belangrijk - mappen die zijn benoemde voor HANA log en gegevens volumes, evenals de installatiepad (/ hana/gedeeld in dit voorbeeld) en/usr/sap mag geen deel uit van de hoofdsite bestandssysteem maar Azure gegevensschijven die zijn gekoppeld aan de VM zoals is beschreven in de sectie Azure VM setup behoort. Hiermee wordt Vermijd het risico die het bestandssysteem hoofdsite onvoldoende ruimte beschikbaar is mogelijk.
Een kunt ook zien dat de gebruiker van de beheerder HANA gebruikers-id 1005 heeft en maakt deel uit van de sapsys-groep (id 1001) die is gedefinieerd voor de installatie.



![](./media/virtual-machines-linux-sap-hana-get-started/image033.jpg)

Een de HANA kunt controleren \<HANA SID\>Gebruikersdetails adm (azdadm in dit voorbeeld) in/enzovoort/passwd



![](./media/virtual-machines-linux-sap-hana-get-started/image034.jpg)

Na de installatie van SAP HANA met hdblcm kan deze worden bekeken in SAP HANA Studio. Het schema SAPABAP1 waaronder bijvoorbeeld alle SAP NetWeaver tabellen is nog niet beschikbaar.



![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Na de installatie van SAP HANA kunt een SAP NetWeaver installeren bevindt. In dit voorbeeld die deze werden geëffend gebruik van een 'gedistribueerde installatie"via SWPM zoals is beschreven in de bijbehorende sectie hierboven.
Wanneer het exemplaar van de database via SWPM een NET installatie heeft om in te voeren van dezelfde gegevens als voordat u met hdblcm (bijvoorbeeld hostnaam, HANA SID, exemplaar getal). SWPM vervolgens de bestaande HANA-installatie gebruiken en toevoegen van extra schema's.



![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Dit is de afbeelding van de stap van de installatie SWPM waar hebben tot het invoeren van gegevens met betrekking tot het schema DBACOCKPIT.


![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Vervolgens verwacht SWPM om gegevens over het schema SAPABAP1 te voeren.


![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Het schema SAPABAP1 in HANA Studio kunnen zien zodra de installatie van de SWPM database-exemplaar van de werkstroom is voltooid.



![](./media/virtual-machines-linux-sap-hana-get-started/image039b.jpg)

En ten slotte na de installatie van de SAP-app-server en SAP grafische gebruikersinterface een moet kunnen om te controleren of het exemplaar HANA DB met transactie "dbacockpit".




## <a name="general-information-related-to-sap-azure-certifications-running-sap-hana-on-azure-and-sap-software-download"></a>Algemene informatie over SAP Azure-certificering, SAP HANA op Azure en SAP softwaredownload uitgevoerd

* algemene SAP Azure docu over het uitvoeren van SAP op Azure met Windows-besturingssysteem in de klassieke modus: [Gebruik van SAP op Windows virtuele machines in Azure wordt aangegeven](virtual-machines-windows-classic-sap-get-started.md)

* informatie over bestaande SAP-sjablonen voor gebruik door klanten: [Azure Quickstart sjablonen voor SAP](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)

* algemene SAP Azure docu over het uitvoeren van SAP op Azure met Linux OS in Azure resourcemanager model: [Gebruik van SAP op Linux virtuele machines (VMs)](virtual-machines-linux-sap-get-started.md)

* gecertificeerde SAP HANA hardware directory waarin wordt aangegeven welke typen Azure VM worden ondersteund voor productie: [Gecertificeerd SAP HANA® Hardware Directory](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)

* informatie over de grootte VM met name voor Linux werkbelasting: [grootte voor virtuele machines in Azure wordt aangegeven](virtual-machines-linux-sizes.md)

* SAP-notitie waarin u alle ondersteunde SAP-producten op Azure en Azure VM typen ondersteund voor SAP: [SAP notitie 1928533](https://launchpad.support.sap.com/#/notes/1928533/E)

* SAP-opmerking over het SAP "enhanced monitoring" met Linux VMs op Azure: [SAP notitie 2191498](https://launchpad.support.sap.com/#/notes/2191498/E)

* SAP-HANA aanbod op Azure "Grote exemplaren". Het is belangrijk te begrijpen dat dit niet is over het uitvoeren van SAP HANA op Azure VMs, maar in een hybride omgeving waarin de SAP-app-servers worden uitgevoerd in Azure VMs maar SAP HANA wordt uitgevoerd op de voorziening servers: [SAP notitie 2316233](https://launchpad.support.sap.com/#/notes/2316233/E)

* SAP-notitie met informatie over SAPOSCOL op Linux: [SAP notitie 1102124](https://launchpad.support.sap.com/#/notes/1102124/E)

* Toets met het controleren van de doelstellingen voor SAP op Microsoft Azure: [SAP notitie 2178632](https://launchpad.support.sap.com/#/notes/2178632/E)

* Informatie over Azure resourcemanager: [resourcemanager Azure-overzicht](../azure-resource-manager/resource-group-overview.md)

* Informatie over het implementeren van Linux VMs via sjablonen: [Deploy en virtuele machines beheren met behulp van Azure resourcemanager sjablonen en de CLI Azure](virtual-machines-linux-cli-deploy-templates.md)

* Vergelijking van implementatie model tussen Azure resourcemanager en klassieke: [Azure resourcemanager versus klassieke implementatie: meer informatie over de implementatiemodellen en de status van uw resources](../resource-manager-deployment-model.md)

* Download NetWeaver 7.5 voor Linux/HANA van SAP Service Marketplace:![](./media/virtual-machines-linux-sap-hana-get-started/image001.jpg)

* Download HANA SP12 Platform-editie van SAP Service Marketplace:![](./media/virtual-machines-linux-sap-hana-get-started/image002.jpg)

