<properties
    pageTitle="Cassandra worden uitgevoerd met Linux op Azure | Microsoft Azure"
    description="Het uitvoeren van een cluster Cassandra op Linux in Azure virtuele Machines via een Node.js-app"
    services="virtual-machines-linux"
    documentationCenter="nodejs"
    authors="hanuk"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="hanuk;robmcm"/>

# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Cassandra met Linux waarop Azure en toegang krijgen tot deze vanuit Node.js

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](https://azure.microsoft.com/documentation/templates/datastax-on-ubuntu/).

## <a name="overview"></a>Overzicht
Microsoft Azure is een geopende cloud-platform die zowel Microsoft als u ook als niet-Microsoft-software waaronder besturingssystemen, toepassingsservers, SMS middleware, evenals SQL versus NoSQL databases uit beide bronmodellen commerciële en open wordt uitgevoerd. Samenstellen van robuuste services op openbare wolken inclusief Azure vereist zorgvuldige planning en opzettelijk architectuur voor beide toepassingsservers als CRL opslag lagen. De Cassandra verdeelde opslagarchitectuur helpt natuurlijk in het bouwen van maximaal beschikbare systemen die fouttolerantie voor cluster fouten zijn. Cassandra is een cloud-schaal NoSQL database door Apache Software Foundation op cassandra.apache.org; gehouden Cassandra is geschreven in Java en daarom worden uitgevoerd op beide op Windows, evenals Linux platforms.

De focus verplaatsen van dit artikel is Cassandra implementatie om op te geven Ubuntu als één en meerdere Datacenter cluster gebruikmaken van Microsoft Azure virtuele Machines en virtuele netwerken. De implementatie cluster voor productie geoptimaliseerd werkbelasting ligt buiten het bereik van dit artikel als er meerdere schijf knooppuntconfiguratie, topologie van juiste ringen en gegevens modelleren ter ondersteuning van de benodigde herhaling, de consistentie van de gegevens, de gegevensdoorvoer en de beschikbaarheid vereisten moeten.

In dit artikel wordt een fundamentele manier om weer te geven wat is betrokken bij het samenstellen van het cluster Cassandra vergeleken Docker, Chef of Puppet waarin de infrastructuur-implementatie veel eenvoudiger kunt aanbrengen.  

## <a name="the-deployment-models"></a>De implementatiemodellen
Microsoft Azure-netwerken kan de implementatie van geïsoleerd privé kolomgroepen de toegang van die beperkt worden kan tot de korrelig netwerkbeveiliging bereiken.  Omdat in dit artikel over het weergeven van de implementatie Cassandra op het niveau van een fundamentele, gaan we in niet op het niveau van de consistentie en het opslagontwerp optimale worden verwerkt. Dit zijn de vereisten voor onze hypothetische cluster netwerk:

- Externe systemen geen toegang tot de database van de Cassandra van binnen of buiten Azure
- Cassandra cluster moet zich achter een taakverdeling voor thrift verkeer
- Cassandra knooppunten in twee groepen in elke Datacenter voor de clusterbeschikbaarheid van een uitgebreide implementeren
- Vergrendelen het cluster dus die alleen serverfarm toepassing toegang tot de database rechtstreeks heeft
- Geen openbare netwerken eindpunten dan SSH
- Elke knooppunt Cassandra moet een vaste interne IP-adres

Cassandra kan worden geïmplementeerd op één Azure regio of meerdere regio's op basis van de gedistribueerde aard van de werklast. Meerdere landen/regio-implementatiemodel kan worden gebruikt om te dienen eindgebruikers dichter naar een bepaalde geografische via de dezelfde Cassandra-infrastructuur. Cassandra van ingebouwde knooppunt herhaling zorgt voor de synchronisatie van meerdere basispagina schrijft die afkomstig zijn van meerdere datacenters en consistente overzicht van de gegevens naar toepassingen geeft. Meerdere landen/regio-implementatie kunt ook met de risicobeperking van de bredere Azure-service bijvoorbeeld. Instelbare consistentie van Cassandra en replicatietopologie wordt bij het oplossen van behoeften diverse vrijgegeven Productieorder van toepassingen.

### <a name="single-region-deployment"></a>Één regio-implementatie
We beginnen met een bepaalde regio-implementatie en oogst van de geleerde lessen bij het maken van een model meerdere landen/regio. Azure virtuele netwerken worden gebruikt om te geïsoleerd subnetten maken zodat de hierboven genoemde netwerk beveiligingsvereisten die zijn kunnen worden gehaald.  Het proces beschreven in de implementatie van één regio maken gebruikt Ubuntu 14.04 LTS en Cassandra 2.08; het proces kunt echter gemakkelijk worden vastgesteld naar de andere Linux-varianten. Hier volgen enkele van de systematische kenmerken van de implementatie van één regio.  

**Beschikbaarheid:** De Cassandra knooppunten weergegeven in de afbeelding 1 zijn geïmplementeerd in twee sets van beschikbaarheid, zodat de knooppunten worden verdeeld tussen meerdere foutenstructuuranalyse domeinen voor maximale beschikbaarheid. VMs waarbij elke set beschikbaarheid van aantekeningen voorzien is toegewezen aan 2 foutenstructuuranalyse domeinen.  Microsoft Azure Hiermee gebruikt u het concept van het domein foutenstructuuranalyse voor het beheren van ongeplande tijd (bijvoorbeeld hardware en software fouten) terwijl het concept van de upgrade domein (bijvoorbeeld host of Gast patches/upgrades voor het besturingssysteem, toepassingsupgrades) wordt gebruikt voor het beheren van omlaag tijd is gepland. Zie [problemen oplossen en beschikbaarheid van Azure-toepassingen](http://msdn.microsoft.com/library/dn251004.aspx) voor de rol van foutenstructuuranalyse en domeinen in de beschikbaarheid bereiken upgraden.

![Één regio-implementatie](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux1.png)

Afbeelding 1: Één regio-implementatie

Houd er rekening mee dat op het moment van deze schrijven, Azure niet is toegestaan het expliciete toewijzing van een groep VMs naar een specifieke foutenstructuuranalyse-domein. dus ook niet met de implementatiemodel Zie afbeelding 1, is het statistisch waarschijnlijk dat de virtuele machines kan worden toegewezen aan twee domeinen voor foutenstructuuranalyse in plaats van vier.

**Taakverdeling Thrift verkeer:** Thrift clientbibliotheken binnen de webserver verbinden met het cluster via een interne taakverdeling. Hiervoor is het proces van het toevoegen van de interne taakverdeling aan het subnet "gegevens" vereist (Zie afbeelding 1) in de context van de cloudservice het cluster Cassandra hostingprovider. Zodra de interne taakverdeling is opgegeven, is het eindpunt van taakverdeling moet worden toegevoegd met de aantekeningen van een set taakverdeling met de naam van de verdeling van eerder gedefinieerde laden in elk knooppunt vereist. Zie [Azure interne taakverdeling ](../load-balancer/load-balancer-internal-overview.md)voor meer informatie.

**Cluster zaden:** Het is belangrijk te selecteren van de meeste ten zeerste beschikbare knooppunten voor zaaizaad wanneer het nieuwe knooppunten met zaad knooppunten communiceert te vinden van de topologie van het cluster. Een van de knooppunten in elke set beschikbaarheid aangewezen als zaad knooppunten om te voorkomen dat potentieel risico.

**Herhaling Factor en consistentie niveau:** Cassandra van ingebouwde hoge beschikbaarheid en gegevens levensduur wordt gekenmerkt door de Factor herhaling (RF - aantal exemplaren van elke rij die is opgeslagen op het cluster) en consistentie niveau (aantal replica's om te worden gelezen/geschreven voordat u het resultaat terugkeren naar de beller). Replicatie factor is opgegeven tijdens het maken van KEYSPACE (vergelijkbaar met een relationele database), terwijl het niveau van de consistentie is opgegeven tijdens het verzenden van de query CRUD. Zie Cassandra documentatie op [configureren voor consistentie](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) voor meer informatie de consistentie en de formule voor het berekenen van quorum.

Cassandra ondersteunt twee soorten integriteit gegevensmodellen – consistentie en eventuele consistentie; de herhaling Factor en consistentie niveau wordt samen bepalen of de gegevens consistent zodra u een bewerking is voltooid of het wordt uiteindelijk consistent zijn. Bijvoorbeeld QUORUM geven, zoals het niveau van de consistentie altijd wordt zorgt ervoor dat de gegevens consistentie terwijl u elk gewenst niveau consistentie, onder het aantal replica's worden geschreven naar wens te bereiken QUORUM (bijvoorbeeld een) resultaten in wordt uiteindelijk consistente gegevens.

De cluster 8 knooppunten hierboven, met een factor herhaling van 3 en QUORUM (2 knooppunten zijn lezen of schrijven voor consistentie) alleen-lezen/schrijven consistentie niveau, de theoretische verlies van het meeste 1 knooppunt per herhaling groep vóór de start van de toepassing desgewenst de fout kan worden bewaard. Hiermee wordt ervan uitgegaan dat alle de belangrijkste spaties hebt u ook verdeeld alleen-lezen/schrijven aanvragen.  Hier volgen de parameters die we voor het geïmplementeerd cluster gebruiken:

Configuratie van één regio Cassandra cluster:

| Cluster Parameter | Waarde | Opmerkingen |
| ----------------- | ----- | ------- |
| Aantal knooppunten (N) | 8   | Totaal aantal knooppunten in het cluster |
| Replicatie Factor (RF) | 3 | Aantal kopieën van een bepaalde rij |
| Consistentie niveau (schrijven) | QUORUM[(RF/2) +1) = 2] het resultaat van de formule naar beneden afgerond | Het meeste 2 replica's schrijft voordat het antwoord wordt verzonden naar de beller; 3e replica is geschreven in een uiteindelijk consistente wijze. |
| Consistentie niveau (lezen) | QUORUM [(RF/2) + 1 = 2] het resultaat van de formule naar beneden afgerond | Leest 2 replica's moet worden verzonden naar de beller. |
| Replicatiestrategie | NetworkTopologyStrategy Zie [Repliceren](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) in Cassandra documentatie voor meer informatie | De implementatietopologie begrijpt en replica's op knooppunten geplaatst, zodat alle replica's niet op hetzelfde rek uiteindelijk |
| Snitch | GossipingPropertyFileSnitch Zie [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) in Cassandra documentatie voor meer informatie | Een concept van snitch NetworkTopologyStrategy gebruikt voor meer informatie over de topologie. GossipingPropertyFileSnitch biedt betere controle in elk knooppunt aan datacenter en rek toewijzen. Het cluster worden vervolgens roddels gebruikt deze informatie doorgeven. Dit is veel eenvoudiger in dynamische IP-instelling ten opzichte van PropertyFileSnitch |


**Azure overwegingen voor Cassandra Cluster:** De mogelijkheid van Microsoft Azure virtuele Machines Azure-blobopslag voor Schijfopruiming permanente; wordt gebruikt Azure opslag wordt 3 kopieën van elke schijf voor hoge levensduur opgeslagen. Dat betekent dat elke rij met gegevens in een tabel Cassandra ingevoegd is al opgeslagen in 3 replica's en dus gegevensconsistentie is al afgehandeld zelfs als de herhaling Factor (RF) 1 is. Het belangrijkste probleem met herhaling Factor 1 is dat de toepassing downtime ervaart zelfs als één Cassandra knooppunt mislukt. Als een knooppunt is niet beschikbaar voor de problemen (bijvoorbeeld hardware, software systeemfouten) herkend door Azure stof Controller, wordt deze echter een nieuw knooppunt in plaats daarvan de dezelfde opslagstations met inrichten. Een nieuw knooppunt ter vervanging van de oude inrichting kan een paar minuten duren.  Op dezelfde manier gepland onderhoudsactiviteiten zoals Gast OS wijzigingen, Cassandra upgrades en voor wijzigingen in de toepassing Azure stof Controller voert schuivend upgrades van de knooppunten in het cluster.  Schuivend upgrades ook duurt omlaag enkele knooppunten tegelijk en dus het cluster waarnemen kort downtime voor een paar partities. De gegevens worden echter niet meer vanwege de ingebouwde Azure Storage redundantie verloren.  

Voor systemen die zijn geïmplementeerd in Azure waarvoor beschikbaarheid (bijvoorbeeld rond 99,9 die is gelijk aan 8.76 uur-jaar; [Beschikbaarheid](http://en.wikipedia.org/wiki/High_availability) Zie voor meer informatie) niet wordt u mogelijk om uit te voeren met RF = 1 en niveau van de consistentie = een.  Voor toepassingen met hoge beschikbaarheid vereisten, RF = 3 en consistentie niveau = QUORUM geplaatst en de tijd van een van de knooppunten van een van de replica's. RF = 1 in traditionele implementaties (bijvoorbeeld on-premises) vanwege de mogelijk gegevensverlies ten gevolge van problemen, zoals schijfproblemen kunnen niet worden gebruikt.   

## <a name="multi-region-deployment"></a>Meerdere landen/regio-implementatie
De Cassandra center-gegevensdetectie replicatie en consistentie model hierboven beschreven helpt bij de implementatie van meerdere landen/regio uit het vak zonder dat u een externe tooling. Dit is heel anders uit de traditionele relationele databases waarbij de instellingen voor het spiegelen van de database voor meerdere basispagina schrijft er ingewikkeld kan zijn. Cassandra in een meerdere landen/regio instellen kan helpen bij het gebruik-scenario's, waaronder de volgende:

**Implementatie op basis van nabijheid:** Meerdere tenant-toepassingen, met de toewijzing van tenant gebruikers wissen-naar-regio kunt resultaat door lage vertragingstijden van het cluster meerdere landen/regio worden geleid. Bijvoorbeeld een management learning systemen voor onderwijsinstellingen een verdeelde cluster in de regio's moet fungeren van de desbetreffende campussen voor Oost-Amerikaanse en West Amerikaans kunnen implementeren transacties en analyses. De gegevens kunt eenduidig lokaal aan het tijd lezen en schrijven en kunt uiteindelijk hele eenduidig beide de regio's. Er zijn andere voorbeelden zoals media verdeling, e-commerce en en alles dat geografische toegespitst gebruiker basistoewijzing fungeert is een goede use-case voor dit implementatiemodel.

**Beschikbaarheid:** Redundantie speelt een belangrijke rol bereiken van betere beschikbaarheid van software en hardware; Zie Building betrouwbare Cloud systemen op Microsoft Azure voor meer informatie. Op Microsoft Azure is het alleen betrouwbare manier zijn waar redundantie om door te implementeren van een cluster meerdere landen/regio. Toepassingen kunnen worden geïmplementeerd in de modus van een actieve of actieve-passieve en als een van de regio's niet actief is, Azure verkeer Manager verkeer kunt omleiden naar de actieve regio.  Met de implementatie van één regio, als de beschikbaarheid 99,9, is een twee regio-implementatie kan bereiken een beschikbaarheid van 99.9999 berekend door de formule: (1-(1-0.999) *(1-0,999))*100); Zie het bovenstaande papier voor meer informatie.

**Herstel:** Meerdere landen/regio Cassandra cluster, kunt als het goed is ontworpen, fatale data center bijvoorbeeld berekend. Als één regio niet actief is, kan de toepassing die zijn geïmplementeerd in andere regio's kunt starten voor eindgebruikers. Zoals elke andere bedrijven bedrijfscontinuïteit implementaties mag de toepassing worden fouttolerante voor gegevensverlies die resulteert uit de gegevens in de asynchrone pijplijn. Cassandra is echter het herstelproces is veel sneller dan de tijd die u hebt gemaakt door het herstelproces traditionele database. Afbeelding 2 ziet u de meestgebruikte voor meerdere landen/regio-model met acht knooppunten in elke regio. Beide regio's zijn spiegelbeeld elkaar voor dezelfde hoogte van het; echte wereld ontwerpen, hangt af van het type werkbelasting (bijvoorbeeld transacties of analytical), vrijgegeven Productieorder, RTO, consistentie van de gegevens en beschikbaarheid vereisten.

![Meerdere regio-implementatie](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux2.png)

Afbeelding 2: Meerdere landen/regio Cassandra implementatie

### <a name="network-integration"></a>Integratie van netwerken
Sets van virtuele machines geïmplementeerd in particuliere netwerken die zich op twee regio's communiceert met elkaar met behulp van een VPN-tunnel. De VPN-tunnel verbindt twee software gateways deze is ingericht tijdens de implementatie netwerk. Beide regio's hebt soortgelijke netwerkarchitectuur met "web" en "gegevens" subnetten; Azure netwerken het maken van zoveel subnetten naar wens en ACL's toepassen door de beveiliging van het netwerk. Terwijl de topologie cluster onder ontwerpen data center communicatie latentie en de economic gevolgen van het netwerkverkeer moet worden beschouwd.

### <a name="data-consistency-for-multi-data-center-deployment"></a>De consistentie van de gegevens voor meerdere Datacenter-implementatie
Houd rekening met het effect van de topologie cluster op doorvoer en betere beschikbaarheid implementaties nodig verdeeld. De RF en consistentie niveau moeten worden geselecteerd zodanig dat het quorum niet afhankelijk van de beschikbaarheid van alle datacenters.
Voor een systeem die nog moet worden hoge consistentie, zorgt een LOCAL_QUORUM voor consistentie niveau (voor lezen en schrijven) ervoor dat de lokale lezen en schrijven wordt voldaan uit de lokale knooppunten terwijl gegevens asynchroon worden gerepliceerd naar de externe datacenters.  Tabel 2 bevat een overzicht van de configuratiegegevens voor het meerdere landen/regio-cluster uit die later in het schrijven van beschreven.

**Configuratie van de twee regio Cassandra cluster**


| Cluster Parameter | Waarde | Opmerkingen |
| ----------------- | ----- | ------- |
| Aantal knooppunten (N) | 8 + 8 | Totaal aantal knooppunten in het cluster |
| Replicatie Factor (RF) | 3 | Aantal kopieën van een bepaalde rij |
| Consistentie niveau (schrijven) | LOCAL_QUORUM [(sum(RF)/2) +1) = 4] het resultaat van de formule naar beneden afgerond | 2 knooppunten worden geschreven Datacenter voor de eerste synchroon; de extra 2 knooppunten die u nodig hebt voor quorum geschreven asynchroon Datacenter voor de 2e. |
| Consistentie niveau (lezen) | LOCAL_QUORUM ((RF/2) + 1) = 2 het resultaat van de formule naar beneden afgerond | Alleen aanvragen is voldaan uit de tweede regio slechts één; 2 knooppunten worden opgelezen voordat het antwoord wordt verzonden naar de client. |
| Replicatiestrategie | NetworkTopologyStrategy Zie [Repliceren](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) in Cassandra documentatie voor meer informatie | De implementatietopologie begrijpt en replica's op knooppunten geplaatst, zodat alle replica's niet op hetzelfde rek uiteindelijk |
| Snitch | GossipingPropertyFileSnitch Zie [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) in Cassandra documentatie voor meer informatie | Een concept van snitch NetworkTopologyStrategy gebruikt voor meer informatie over de topologie. GossipingPropertyFileSnitch biedt betere controle in elk knooppunt aan datacenter en rek toewijzen. Het cluster worden vervolgens roddels gebruikt deze informatie doorgeven. Dit is veel eenvoudiger in dynamische IP-instelling ten opzichte van PropertyFileSnitch |


##<a name="the-software-configuration"></a>DE SOFTWARECONFIGURATIE
De volgende versies van de software worden gebruikt tijdens de implementatie:

<table>
<tr><th>Software</th><th>Bron</th><th>Versie</th></tr>
<tr><td>JRE </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu  </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Aangezien het downloaden van JRE nodig handmatig aanvaarding van Oracle-licentie heeft, de vereiste software om te vereenvoudigen de implementatie, downloaden naar het bureaublad voor later uploaden naar de Ubuntu Sjabloonafbeelding die we als een voordat de implementatie cluster zal maken.

Download de bovenstaande software in een downloadmap bekende (bijvoorbeeld %TEMP%/downloads in Windows of ~/Downloads op de meeste Linux onderzoeken of Mac) op de lokale computer.

### <a name="create-ubuntu-vm"></a>UBUNTU VM MAKEN
In deze stap van het proces wordt we Ubuntu afbeelding met de bibliotheekmachtigingen software maken, zodat de afbeelding opnieuw kan worden gebruikt voor het inrichten van verschillende Cassandra knooppunten.  
####<a name="step-1-generate-ssh-key-pair"></a>STAP 1: Een combinatie van SSH sleutel genereren
Azure moet een X509 openbare sleutel die PEM of DER-codering op het moment inrichten. Genereer een openbare/combinatie van persoonlijke sleutel volgens de instructies hoe u gebruik SSH met Linux op Azure zich bevindt. Als u van plan bent om te putty.exe gebruiken als een SSH-client op Windows- of Linux, moet u de PEM codering converteren RSA persoonlijke sleutel PPK cv opmaken met behulp van puttygen.exe; de instructies voor deze vindt u in de bovenstaande pagina met webonderdelen.

####<a name="step-2-create-ubuntu-template-vm"></a>STAP 2: Ubuntu sjabloon VM maken
Als u wilt maken van de sjabloon VM, meld u aan bij de portal van Azure klassieke en gebruiken van de volgende volgorde: klik op Nieuw, BEREKENINGSCLUSTER, VM, FROM GALERIE, UBUNTU, Ubuntu Server 14.04 LTS en klik op de pijl-rechts. Zie maken een Linux VM uitgevoerd voor een zelfstudie die wordt beschreven hoe u een VM Linux maken.

Voer de volgende gegevens op het Configuratiescherm "VM" #1:

<table>
<tr><th>VELDNAAM              </td><td>       VELDWAARDE               </td><td>         OPMERKINGEN                </td><tr>
<tr><td>RELEASEDATUM VERSIE    </td><td> Selecteer een datum van de drow omlaag</td><td></td><tr>
<tr><td>DE NAAM VAN DE VIRTUELE MACHINES    </td><td> Cass-sjabloon                </td><td> Dit is de hostnaam van VM </td><tr>
<tr><td>LAAG                     </td><td> STANDAARD                        </td><td> Laat de standaard              </td><tr>
<tr><td>GROOTTE                     </td><td> A1                              </td><td>Selecteer de VM op basis van de behoeften IO; Laat de standaardinstelling voor dit doel </td><tr>
<tr><td> NIEUWE GEBRUIKERSNAAM IN TE VOEREN           </td><td> localadmin                      </td><td> "beheerder" is een gereserveerde gebruikersnaam in te voeren in Ubuntu 12. xx en na</td><tr>
<tr><td> VERIFICATIE      </td><td> Klik op het selectievakje                 </td><td>Als u wilt beveiligen met een sleutel SSH controleren </td><tr>
<tr><td> CERTIFICAAT             </td><td> bestandsnaam van het certificaat met openbare sleutel </td><td> Gebruik van de openbare sleutel eerder gegenereerd</td><tr>
<tr><td> Een nieuw wachtwoord   </td><td> sterke wachtwoorden </td><td> </td><tr>
<tr><td> Bevestig het wachtwoord   </td><td> sterke wachtwoorden </td><td></td><tr>
</table>

Voer de volgende gegevens op het Configuratiescherm "VM" #2:

<table>
<tr><th>VELDNAAM             </th><th> VELDWAARDE                       </th><th> OPMERKINGEN                                 </th></tr>
<tr><td> CLOUDSERVICE  </td><td> Een nieuwe cloudservice maken    </td><td>Cloudservice is een resources voor het berekenen van container zoals virtuele machines</td></tr>
<tr><td> DE NAAM VAN DE DNS-SERVICE CLOUD </td><td>Ubuntu-template.cloudapp.net   </td><td>Geef een naam voor gelijkmatige verdeling van machine agnostische laden</td></tr>
<tr><td> LAND/AFFINITEIT GROEP/VIRTUAL NETWORK </td><td>    West VS </td><td> Selecteer een gebied waaruit de toegang tot het cluster Cassandra van uw webtoepassingen</td></tr>
<tr><td>OPSLAG-ACCOUNT </td><td>   Standaard gebruiken </td><td>Gebruik van het standaardaccount voor de opslag of een vooraf gemaakte opslag-account in een bepaald gebied</td></tr>
<tr><td>BESCHIKBAARHEID INSTELLEN </td><td>  Geen </td><td>  Laat het vak leeg</td></tr>
<tr><td>EINDPUNTEN   </td><td>Standaard gebruiken </td><td>  De standaard SSH configuratie uitvoeren </td></tr>
</table>

Klik op pijl-rechts, laat u de standaardwaarden in het scherm #3 en klik op de knop 'selectievakje' om te voltooien van de inrichting van proces VM. Na een paar minuten moet de VM met de naam 'ubuntu-sjabloon' in een status 'uitgevoerd'.

###<a name="install-the-necessary-software"></a>DE BENODIGDE SOFTWARE INSTALLEREN
####<a name="step-1-upload-tarballs"></a>STAP 1: Upload tarballs
Scp of pscp gebruikt, kopieert u de eerder gedownloade software naar ~/downloads-directory met de volgende opdracht opmaak:

#####<a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz

Herhaal de bovenstaande opdracht voor JRE ook als voor de bits Cassandra.

####<a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>STAP 2: De mapstructuur voorbereiden en extraheren van de archieven
Meld u aan bij de VM en maak de mapstructuur en software extraheren als supergebruiker met het onderstaande we vaker doen-script:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Als u dit script op vim venster plakt, controleert u of verwijderen van de Enter-teken ('\r ') met de volgende opdracht:

    tr -d '\r' <infile.sh >outfile.sh

####<a name="step-3-edit-etcprofile"></a>Stap 3: Enzovoort/profiel bewerken
Toevoegen aan het einde volgt te werk:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

####<a name="step-4-install-jna-for-production-systems"></a>Stap 4: Installatiefout JNA voor systemen
De volgorde van de volgende opdracht: de volgende opdracht jna-3.2.7.jar installeren en jna-platform-3.2.7.jar met /usr/share.java directory enz sudo get-libjna-java installeren

Symbolic koppelingen maken in de directory CASS_HOME/bibliotheek $ zodat Cassandra opstartscript deze potten kunt vinden:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

####<a name="step-5-configure-cassandrayaml"></a>Stap 5: Cassandra.yaml configureren
Cassandra.yaml op elke VM aan configuratie die nodig zijn voor alle de virtuele machines [we wordt dit aanpassen tijdens de werkelijke inrichting] bewerken:

<table>
<tr><th>Veldnaam   </th><th> Waarde  </th><th> Opmerkingen </th></tr>
<tr><td>clusternaam </td><td>  "CustomerService"   </td><td> Gebruik de naam die overeenkomt met uw implementatie</td></tr>
<tr><td>listen_address  </td><td>[laat deze leeg]   </td><td> "Localhost" verwijderen </td></tr>
<tr><td>rpc_addres   </td><td>[laat deze leeg]  </td><td> "Localhost" verwijderen </td></tr>
<tr><td>zaden   </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8" </td><td>Lijst met alle IP-adressen die zijn aangewezen als basis.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Hiermee wordt gebruikt door de NetworkTopologyStrateg voor afleiden van het datacenter en de rek van VM</td></tr>
</table>

####<a name="step-6-capture-the-vm-image"></a>Stap 6: De afbeelding VM vastleggen
Meld u aan bij de virtuele machine met de hostnaam (Hongkong-cas-template.cloudapp.net) en de SSH persoonlijke sleutel eerder hebt gemaakt. Zie hoe u gebruik SSH met Linux op Azure voor meer informatie over het aanmelden met de opdracht ssh of putty.exe.

Voer de volgende reeks acties om de afbeelding te halen uit:
#####<a name="1-deprovision"></a>1. identiteitsgegevens
Gebruik de opdracht ' sudo waagent – identiteitsgegevens + gebruiker "VM exemplaar specifieke gegevens verwijderen. Zie voor [een Linux virtuele Machine vastleggen](virtual-machines-linux-classic-capture-image.md) kunt gebruiken als een sjabloon meer details op proces voor het vastleggen van afbeelding.

#####<a name="2-shutdown-the-vm"></a>2: de VM afsluiten
Zorg ervoor dat de virtuele machine is geselecteerd en klik op de koppeling afsluiten van de opdracht onderste balk.

#####<a name="3-capture-the-image"></a>3: de afbeelding vastleggen
Zorg ervoor dat de virtuele machine is geselecteerd en klik op de koppeling VASTLEGGEN van de opdracht onderste balk. In het volgende scherm geven (bijvoorbeeld hk-cas-2-08-ub-14-04-2014071) op de naam van een afbeelding, passende beschrijving van de afbeelding en klik op het ' vinkje "om het proces VASTLEGGEN.

Dit kan een paar seconden duren en de afbeelding moet beschikbaar zijn in de sectie Mijn afbeeldingen van de van de afbeeldinggalerie. De bron VM, worden automatisch delated nadat de afbeelding met succes wordt vastgelegd.

##<a name="single-region-deployment-process"></a>Proces voor één regio-implementatie
**Stap 1: het virtuele netwerk maken** Meld u aan bij de portal van Azure klassieke en een virtueel netwerk maken met de kenmerken weergeven in de tabel. Zie [een Cloud-Only Virtual netwerk in de portal van Azure klassieke configureren](../virtual-network/virtual-networks-create-vnet-classic-portal.md) voor gedetailleerde stappen van het proces.      

<table>
<tr><th>VM kenmerknaam</th><th>Waarde</th><th>Opmerkingen</th></tr>
<tr><td>Naam</td><td>vnet-cass-west-opnemen</td><td></td></tr>
<tr><td>Regio</td><td>West VS</td><td></td></tr>
<tr><td>DNS-Servers </td><td>Geen</td><td>Dit negeren terwijl we niet werkt met een DNS-Server</td></tr>
<tr><td>Een VPN-verbinding voor de punt-naar-site configureren</td><td>Geen</td><td> Dit negeren</td></tr>
<tr><td>Een VPN-verbinding voor de site-naar-site configureren</td><td>Nnone</td><td> Dit negeren</td></tr>
<tr><td>-Adresruimte</td><td>10.1.0.0/16</td><td></td></tr>
<tr><td>IP starten</td><td>10.1.0.0</td><td></td></tr>
<tr><td>CIDR </td><td>/ 16 (65531)</td><td></td></tr>
</table>

De volgende subnetten toevoegen:

<table>
<tr><th>Naam</th><th>IP starten</th><th>CIDR</th><th>Opmerkingen</th></tr>
<tr><td>Web</td><td>10.1.1.0</td><td>/ 24 (251)</td><td>Subnet voor de webfarm</td></tr>
<tr><td>gegevens</td><td>10.1.2.0</td><td>/ 24 (251)</td><td>Subnet voor de databaseknooppunten</td></tr>
</table>

Gegevens en Web subnetten kunnen worden beveiligd via netwerk beveiligingsgroepen de licenties die buiten het bereik voor in dit artikel valt.  

**Stap 2: inrichten van virtuele Machines** Met de afbeelding die eerder hebt gemaakt, we de volgende virtuele machines maken in de cloud-server "Hongkong-c-svc-west" en koppelen aan de desbetreffende subnetten zoals hieronder wordt weergegeven:

<table>
<tr><th>De computernaam van de    </th><th>Subnet </th><th>IP-adres </th><th>Beschikbaarheid instellen</th><th>Domeincontroller/rek</th><th>Invullen?</th></tr>
<tr><td>Hongkong-c1-west-opnemen   </td><td>gegevens   </td><td>10.1.2.4   </td><td>Hongkong-c-uit-1    </td><td>domeincontroller = WESTUS rek = rack1 </td><td>Ja</td></tr>
<tr><td>Hongkong-c2-west-opnemen   </td><td>gegevens   </td><td>10.1.2.5   </td><td>Hongkong-c-uit-1    </td><td>domeincontroller = WESTUS rek = rack1 </td><td>Nee </td></tr>
<tr><td>Hongkong-c3-west-opnemen   </td><td>gegevens   </td><td>10.1.2.6   </td><td>Hongkong-c-uit-1    </td><td>domeincontroller = WESTUS rek = rack2 </td><td>Ja</td></tr>
<tr><td>Hongkong-c4-west-opnemen   </td><td>gegevens   </td><td>10.1.2.7   </td><td>Hongkong-c-uit-1    </td><td>domeincontroller = WESTUS rek = rack2 </td><td>Nee </td></tr>
<tr><td>Hongkong-c5-west-opnemen   </td><td>gegevens   </td><td>10.1.2.8   </td><td>Hongkong-c-uit-2    </td><td>domeincontroller = WESTUS rek = rack3 </td><td>Ja</td></tr>
<tr><td>Hongkong-c6-west-opnemen   </td><td>gegevens   </td><td>10.1.2.9   </td><td>Hongkong-c-uit-2    </td><td>domeincontroller = WESTUS rek = rack3 </td><td>Nee </td></tr>
<tr><td>Hongkong-c7-west-opnemen   </td><td>gegevens   </td><td>10.1.2.10  </td><td>Hongkong-c-uit-2    </td><td>domeincontroller = WESTUS rek = rack4 </td><td>Ja</td></tr>
<tr><td>Hongkong-c8-west-opnemen   </td><td>gegevens   </td><td>10.1.2.11  </td><td>Hongkong-c-uit-2    </td><td>domeincontroller = WESTUS rek = rack4 </td><td>Nee </td></tr>
<tr><td>Hongkong-w1-west-opnemen   </td><td>Web    </td><td>10.1.1.4   </td><td>Hongkong-w-uit-1    </td><td>                       </td><td>N/B</td></tr>
<tr><td>Hongkong-w2-west-opnemen   </td><td>Web    </td><td>10.1.1.5   </td><td>Hongkong-w-uit-1    </td><td>                       </td><td>N/B</td></tr>
</table>

Het maken van de bovenstaande lijst met VMs is vereist voor het volgende proces:

1.  Maak een lege cloudservice in een bepaald gebied
2.  Een VM uit de eerder vastgelegde afbeelding maken en toevoegen aan het virtuele netwerk gemaakt eerder hebt geregistreerd. Herhaal dit voor alle VMs
3.  Een interne taakverdeling toevoegen aan de cloudservice en toevoegen aan het subnet "gegevens"
4.  Toevoegen voor elke VM eerder hebt gemaakt, een eindpunt taakverdeling voor thrift verkeer via een set taakverdeling is verbonden met de eerder gemaakte interne taakverdeling

De bovenstaande procedure kan worden uitgevoerd met behulp van Azure klassieke portal; Gebruik een Windows-computer (gebruik per VM op Azure als u geen toegang tot een Windows-computer), gebruikt u het volgende PowerShell-script voor het inrichten van alle 8 VMs automatisch.

**Lijst 1: PowerShell-script voor het inrichten van virtuele machines**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Stap 3: Cassandra configureren op elke VM**

Meld u aan bij de VM en als volgt te werk:

* $CASS_HOME/conf/cassandra-rackdc.properties om op te geven van de data center en rek-eigenschappen bewerken:

       dc =EASTUS, rack =rack1

* Bewerk cassandra.yaml om te configureren zaad knooppunten zoals hieronder:

       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Stap 4: De VMs starten en testen van het cluster**

Meld u aan bij een van de knooppunten (bijvoorbeeld Hongkong-c1-west-opnemen) en voer de volgende opdracht uit om de status van het cluster weer te geven:

       nodetool –h 10.1.2.4 –p 7199 status

Hier ziet u de weergave die vergelijkbaar is met de hieronder voor een cluster met 8 knooppunten:

<table>
<tr><th>Status</th><th>Adres  </th><th>Laden   </th><th>Tokens </th><th>Eigenaar is van </th><th>Host-ID  </th><th>Rek</th></tr>
<tr><th>SCHAKEL  </td><td>10.1.2.4   </td><td>87.81 KB   </td><td>256    </td><td>38.0%  </td><td>GUID (verwijderd)</td><td>rack1</td></tr>
<tr><th>SCHAKEL  </td><td>10.1.2.5   </td><td>41.08 KB   </td><td>256    </td><td>68,9%  </td><td>GUID (verwijderd)</td><td>rack1</td></tr>
<tr><th>SCHAKEL  </td><td>10.1.2.6   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack2</td></tr>
<tr><th>SCHAKEL  </td><td>10.1.2.7   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack2</td></tr>
<tr><th>SCHAKEL  </td><td>10.1.2.8   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack3</td></tr>
<tr><th>SCHAKEL  </td><td>10.1.2.9   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack3</td></tr>
<tr><th>SCHAKEL  </td><td>10.1.2.10  </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack4</td></tr>
<tr><th>SCHAKEL  </td><td>10.1.2.11  </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (verwijderd)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Het Cluster één regio testen
Gebruik de volgende stappen om te testen van het cluster:

1.    Het IP-adres van de interne taakverdeling (bijvoorbeeld de Powershell-opdracht Get-AzureInternalLoadbalancer commandlet gebruikt, verkrijgen  10.1.2.101). De syntaxis van de opdracht hieronder wordt weergegeven: Get-AzureLoadbalancer – servicenaam "Hongkong-c-svc-west-ons" [ziet u de details van de verdeling van de interne belasting samen met het IP-adres]
2.  Meld u aan bij de webfarm VM (bijvoorbeeld Hongkong-w1-west-opnemen) met stopverf of ssh
3.  Uitvoeren van $CASS_HOME/bin/cqlsh 10.1.2.101 9160
4.  De volgende CQL-opdrachten gebruiken om te controleren of het cluster werkt:

        CREATE KEYSPACE customers_ks WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');

        SELECT * FROM Customers;

Hier ziet u een weergave zoals hieronder:

<table>
  <tr><th> customer_id </th><th> Voornaam </th><th> Achternaam </th></tr>
  <tr><td> 1 </td><td> John </td><td> Doe </td></tr>
  <tr><td> 2 </td><td> Jan </td><td> Doe </td></tr>
</table>

Houd er rekening mee dat het keyspace gemaakt in stap 4 SimpleStrategy met een replication_factor van 3 gebruikt. SimpleStrategy geschikt is voor één gegevens center implementaties dat NetworkTopologyStrategy voor meerdere gegevens centreren implementaties. Een replication_factor van 3 krijgt tolerantie voor knooppuntfouten.

##<a id="tworegion"> </a>Meerdere landen/regio-implementatie
Wordt gebruikmaken van de implementatie één regio is voltooid en Herhaal dezelfde stappen voor het installeren van de tweede regio. Het belangrijkste verschil tussen de implementatie van enkele of meerdere regio is de VPN tunnel instellen voor de communicatie tussen regio; We beginnen met de installatie via het netwerk, inrichten van de VMs en Cassandra configureren.

###<a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Stap 1: Het virtuele netwerk bij het 2e regio maken
Meld u aan bij de portal van Azure klassieke en een virtueel netwerk maken met de kenmerken weergeven in de tabel. Zie [een Cloud-Only Virtual netwerk in de portal van Azure klassieke configureren](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor gedetailleerde stappen van het proces.      

<table>
<tr><th>Kenmerknaam    </th><th>Waarde    </th><th>Opmerkingen</th></tr>
<tr><td>Naam    </td><td>vnet-cass-Oost-opnemen</td><td></td></tr>
<tr><td>Regio  </td><td>Oost-VS</td><td></td></tr>
<tr><td>DNS-Servers     </td><td></td><td>Dit negeren terwijl we niet werkt met een DNS-Server</td></tr>
<tr><td>Een VPN-verbinding voor de punt-naar-site configureren</td><td></td><td>     Dit negeren</td></tr>
<tr><td>Een VPN-verbinding voor de site-naar-site configureren</td><td></td><td>      Dit negeren</td></tr>
<tr><td>-Adresruimte   </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>IP starten </td><td>10.2.0.0   </td><td></td></tr>
<tr><td>CIDR    </td><td>/ 16 (65531)</td><td></td></tr>
</table>

De volgende subnetten toevoegen:
<table>
<tr><th>Naam    </th><th>IP starten    </th><th>CIDR   </th><th>Opmerkingen</th></tr>
<tr><td>Web </td><td>10.2.1.0   </td><td>/ 24 (251)  </td><td>Subnet voor de webfarm</td></tr>
<tr><td>gegevens    </td><td>10.2.2.0   </td><td>/ 24 (251)  </td><td>Subnet voor de databaseknooppunten</td></tr>
</table>


###<a name="step-2-create-local-networks"></a>Stap 2: Lokale netwerken maken
Een netwerk met een lokale in Azure virtuele netwerken is een proxy-adresruimte die is toegewezen aan een externe site, inclusief een privé cloud of een ander Azure regio. Deze proxy-adresruimte is gekoppeld aan een externe gateway voor routeren netwerk naar de juiste netwerken bestemmingen. Zie [een VNet naar VNet verbinding configureren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) voor de instructies voor het maken van verbinding VNET-naar-VNET.

Maak twee lokale netwerken per de volgende details letten:

| Netwerknaam | VPN Gateway-adres | -Adresruimte | Opmerkingen |
| ------------ | ------------------- | ------------- | ------- |
| HK-lnet-map-to-East-US | 23.1.1.1  | 10.2.0.0/16   | Geef een tijdelijke aanduiding voor gatewayadres bij het maken van het lokale netwerk. Adres van de reële gateway wordt doorgevoerd nadat de gateway is gemaakt. Zorg ervoor dat de adresruimte komt exact overeen met de desbetreffende externe VNET; in dit geval de VNET gemaakt in de regio Oost VS. |
| HK-lnet-map-to-West-US | 23.2.2.2  | 10.1.0.0/16   | Geef een tijdelijke aanduiding voor gatewayadres bij het maken van het lokale netwerk. Adres van de reële gateway wordt doorgevoerd nadat de gateway is gemaakt. Zorg ervoor dat de adresruimte komt exact overeen met de desbetreffende externe VNET; in dit geval de VNET gemaakt in de regio West VS. |


###<a name="step-3-map-local-network-to-the-respective-vnets"></a>Stap 3: "Lokale" netwerkverbinding naar de desbetreffende VNETs
In de klassieke Azure portal selecteert u elke vnet, klikt u op "Configureren", "Verbinding maken met het lokale netwerk" controleren en selecteer de lokale netwerken per de volgende details letten:


| Virtual Network | Lokale netwerk |
| --------------- | ------------- |
| Hongkong-vnet-west-opnemen | HK-lnet-map-to-East-US |
| Hongkong-vnet-Oost-opnemen | HK-lnet-map-to-West-US |


###<a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Stap 4: Gateways op VNET1 en VNET2 maken
Vanuit het dashboard van de virtuele netwerken, klikt u op GATEWAY maken waarin de inrichting van proces VPN-gateway wordt geactiveerd. Na een paar minuten moet het werkelijke gateway-adres op het dashboard van elke virtuele netwerk worden weergegeven.

###<a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Stap 5: Update "Lokale" netwerken met de adressen van de desbetreffende "Gateway"###
De lokale netwerken om het IP-adres van de tijdelijke aanduiding voor gateway vervangen door het reële IP-adres van de zojuist ingerichte gateways bewerken. De volgende toewijzing gebruiken:

<table>
<tr><th>Lokale netwerk    </th><th>Virtual Network Gateway</th></tr>
<tr><td>HK-lnet-map-to-East-US </td><td>Gateway van Hongkong-vnet-west-opnemen</td></tr>
<tr><td>HK-lnet-map-to-West-US </td><td>Gateway van Hongkong-vnet-Oost-opnemen</td></tr>
</table>

###<a name="step-6-update-the-shared-key"></a>Stap 6: De gedeelde sleutel bijwerken
Gebruik het volgende Powershell-script de sleutel IPSec van elke VPN gateway [Gebruik de sleutel verjaardagen voor beide gateways] bijgewerkt: Set-AzureVNetGatewayKey - VNetName Hongkong-vnet-Oost-ons - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName Hongkong-vnet-west-ons - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

###<a name="step-7-establish-the-vnet-to-vnet-connection"></a>Stap 7: De VNET-naar-VNET verbinding
Via het menu "DASHBOARD" van de virtuele netwerken gateway-naar-gateway-verbinding tot stand brengen van de Azure klassieke portal. Gebruik de menuonderdelen 'Koppelen' in de onderste werkbalk. Na een paar minuten moet het dashboard de verbindingsgegevens grafisch weergeven.

###<a name="step-8-create-the-virtual-machines-in-region-2"></a>Stap 8: De virtuele machines maken in de regio #2
De afbeelding Ubuntu maken zoals is beschreven in de regio #1-implementatie volgens dezelfde stappen of kopiëren het afbeeldingsbestand VHD aan de opslag van Azure-account in regio #2 bevinden en de afbeelding te maken. Gebruik deze afbeelding en de volgende lijst met virtuele machines maken naar een nieuwe cloudservice Hongkong-c-svc-Oost-opnemen:


| De computernaam van de | Subnet | IP-adres | Beschikbaarheid instellen | Domeincontroller/rek | Invullen? |
| ------------ | ------ | ---------- | ---------------- | ------- | ----- |
| Hongkong-c1-Oost-opnemen | gegevens  | 10.2.2.4   | Hongkong-c-uit-1      | domeincontroller = EASTUS rek = rack1 | Ja |
| Hongkong-c2-Oost-opnemen | gegevens  | 10.2.2.5   | Hongkong-c-uit-1      | domeincontroller = EASTUS rek = rack1 | Nee  |
| Hongkong-c3-Oost-opnemen | gegevens  | 10.2.2.6   | Hongkong-c-uit-1      | domeincontroller = EASTUS rek = rack2 | Ja |
| Hongkong-c5-Oost-opnemen | gegevens  | 10.2.2.8   | Hongkong-c-uit-2      | domeincontroller = EASTUS rek = rack3 | Ja |
| Hongkong-c6-Oost-opnemen | gegevens  | 10.2.2.9   | Hongkong-c-uit-2      | domeincontroller = EASTUS rek = rack3 | Nee  |
| Hongkong-c7-Oost-opnemen | gegevens  | 10.2.2.10  | Hongkong-c-uit-2      | domeincontroller = EASTUS rek = rack4 | Ja |
| Hongkong-c8-Oost-opnemen | gegevens  | 10.2.2.11  | Hongkong-c-uit-2      | domeincontroller = EASTUS rek = rack4 | Nee  |
| Hongkong-w1-Oost-opnemen | Web   | 10.2.1.4   | Hongkong-w-uit-1      | N/B                    | N/B |
| Hongkong-w2-Oost-opnemen | Web   | 10.2.1.5   | Hongkong-w-uit-1      | N/B                    | N/B |


Volg de instructies als regio #1 maar 10.2.xxx.xxx-adresruimte gebruiken.

###<a name="step-9-configure-cassandra-on-each-vm"></a>Stap 9: Cassandra op elke VM configureren
Meld u aan bij de VM en als volgt te werk:

1. $CASS_HOME/conf/cassandra-rackdc.properties als u wilt opgeven van het beheercentrum en rek-eigenschappen van gegevens in de indeling bewerken: domeincontroller = EASTUS rek = rack1
2. Cassandra.yaml om te configureren zaad knooppunten bewerken: zaden: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

###<a name="step-10-start-cassandra"></a>Stap 10: Cassandra starten
Meld u aan bij elke VM en Cassandra start op de achtergrond door de volgende opdracht uit te voeren: $CASS_HOME/bin/cassandra

## <a name="test-the-multi-region-cluster"></a>Test de Cluster meerdere landen/regio
Nu is Cassandra geïmplementeerd op 16 knooppunten met 8 knooppunten in elke Azure regio. Deze knooppunten zijn in hetzelfde cluster op grond van de naam van het algemene cluster en de configuratie van de knooppunten zaad. Gebruik het volgende proces het cluster testen:

###<a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Stap 1: De interne laden verdeling IP voor beide de regio's via PowerShell downloaden
- Get-AzureInternalLoadbalancer - servicenaam "Hongkong-c-svc-west-ons"
- Get-AzureInternalLoadbalancer - servicenaam "Hongkong-c-svc-Oost-ons"  

    Houd rekening met het IP-adressen (bijvoorbeeld west - 10.1.2.101, Oost - 10.2.2.101) weergegeven.

###<a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Stap 2: De volgende handelingen uit in de regio west uitvoeren nadat logboekregistratie in Hongkong-w1-west-opnemen
1.    Uitvoeren van $CASS_HOME/bin/cqlsh 10.1.2.101 9160
2.  De volgende CQL-opdrachten uitvoeren:

        CREATE KEYSPACE customers_ks
        WITH REPLICATION = { 'class' : 'NetworkToplogyStrategy', 'WESTUS' : 3, 'EASTUS' : 3};
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Hier ziet u een weergave zoals hieronder:

| customer_id | Voornaam | Achternaam |
| ----------- | --------- | -------- |
| 1           | John      | Doe      |
| 2           | Jan      | Doe      |


###<a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Stap 3: Voor het uitvoeren van de volgende handelingen uit in de regio Oost nadat de logboekregistratie in Hongkong-w1-Oost-opnemen:
1.    Uitvoeren van $CASS_HOME/bin/cqlsh 10.2.2.101 9160
2.  De volgende CQL-opdrachten uitvoeren:

        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

U ziet de dezelfde weergave zoals gezien voor de regio West het volgende:


| customer_id | Voornaam | Achternaam   |
|------------ | --------- | ---------- |
| 1           | John      | Doe        |
| 2           | Jan      | Doe        |


Uitvoeren van een paar meer wordt ingevoegd en Zie dat die naar west gerepliceerd-ons voor een deel van het cluster.

## <a name="test-cassandra-cluster-from-nodejs"></a>Test Cassandra Cluster met Node.js
Met een van de Linux VMs gewijzigd in het "web" trapsgewijs eerder, wordt een eenvoudig script Node.js als u wilt lezen van de eerder ingevoegde gegevens wordt uitgevoerd

**Stap 1: Node.js en Cassandra-Client installeren**

1. Installeer Node.js en npm
2. Knooppunt pakket 'cassandra-client"installeren npm gebruiken
3. Het volgende script bij de prompt shell waarin de json-tekenreeks van de opgehaalde gegevens uitvoeren:

        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };

        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }

        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }

        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);

           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }

        //update will also insert the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }

        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }

        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)


## <a name="conclusion"></a>Sluiten
Microsoft Azure is een flexibele platform waarmee het uitvoeren van zowel Microsoft als bron openen software, dankzij deze oefening. Maximaal beschikbare Cassandra clusters kunnen worden geïmplementeerd op één datacenter tot en met de verspreiding van knooppunten in meerdere foutenstructuuranalyse domeinen. Cassandra clusters kunnen ook worden geïmplementeerd tussen meerdere geografisch verre Azure regio's voor noodgevallen bewijs systems. Azure en Cassandra samen kunnen de bouw van zeer van schaalbaarheid en noodgevallen herstelbare cloudservices nodig door vandaag internet schaal services.  

##<a name="references"></a>Verwijzingen##
- [http://cassandra.apache.org](http://cassandra.apache.org)
- [http://www.datastax.com](http://www.datastax.com)
- [http://www.nodejs.org](http://www.nodejs.org)
