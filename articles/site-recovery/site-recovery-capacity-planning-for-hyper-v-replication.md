<properties
    pageTitle="Het hulpprogramma van Hyper-V capaciteit teamplanner uitvoeren voor de Site herstel | Microsoft Azure"
    description="In dit artikel bevat instructies voor het gebruik van de functie Hyper-V capaciteit planner voor herstel van Azure-Site"
    services="site-recovery"
    documentationCenter="na"
    authors="rayne-wiselman"
    manager="jwhit"
    editor="" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/12/2016"
    ms.author="raynew" />

# <a name="run-the-hyper-v-capacity-planner-tool-for-site-recovery"></a>Het hulpmiddel Hyper-V capaciteit teamplanner uitvoeren voor sites worden hersteld

Als onderdeel van uw implementatie van Azure Site herstel moet u uw herhaling en bandbreedtevereisten duidelijk. Het hulpmiddel Hyper-V capaciteit planner voor Site-herstel helpt u bij uw vereisten voor replicatie en bandbreedte voor Hyper-V VM replicatie duidelijk.


In dit artikel wordt beschreven hoe het hulpprogramma Hyper-V capaciteit teamplanner uitvoeren. Dit hulpmiddel moet worden gebruikt samen met de andere capaciteit planning hulpprogramma's en informatie die worden beschreven in de [capaciteit, planning voor sites worden hersteld](site-recovery-capacity-planner.md).


## <a name="before-you-start"></a>Voordat u begint

U uitvoert het hulpprogramma op een Hyper-V server of cluster knooppunt in uw primaire-site. Het hulpprogramma uitvoeren moet de Hyper-V host-servers:

- Besturingssysteem: Windows Server® 2012 of Windows Server® 2012 R2
- Geheugen: 20 MB (minimaal)
- Processor: 5 procent realiseren (minimaal)
- Schijfruimte: 5 MB (minimaal)

Voordat u het hulpprogramma uitvoeren moet u de primaire site voorbereiden. Als u bent repliceren tussen twee on-premises implementatie sites en u wilt controleren bandbreedte, moet u ook een replicaserver voorbereiden.


## <a name="step-1-prepare-the-primary-site"></a>Stap 1: De primaire site voorbereiden
1. Maak een lijst van alle de Hyper-V virtuele machines die u wilt repliceren en de Hyper-V hosts/clusters waarop deze zich bevinden op de primaire-site. Het hulpmiddel kunt uitvoeren telkens wanneer voor meerdere zelfstandige hosts of voor één cluster maar niet beide samen. Dit moet ook afzonderlijk uitvoeren voor elk besturingssysteem, zodat u moet verzamelen en houd rekening met de servers Hyper-V als volgt:

  - Windows Server® 2012 zelfstandige servers
  - Windows Server® 2012 clusters
  - Windows Server® 2012 R2 zelfstandige servers
  - Windows Server® 2012 R2 clusters

3. Externe toegang met WMI op alle Hyper-V hosts en clusters inschakelen. Deze opdracht uitvoeren op elk server/cluster om te controleren of firewallregels en gebruikersmachtigingen worden ingesteld:

        netsh firewall set service RemoteAdmin enable

5. Schakelen prestatiecontroles uit op servers en kolomgroepen als volgt:

  - Open het Windows Firewall met de module **Geavanceerde beveiliging** en schakel de volgende regels voor binnenkomende verbindingen: **COM + netwerktoegang (DCOM-IN)** en alle regels in de **groep extern gebeurtenislogboek beheer**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Stap 2: Een replicaserver (on-premises implementatie on-premises implementatie replicatie) voorbereiden

U hoeft te doen als u naar Azure repliceren bent.

Het is raadzaam om dat u één Hyper-V host als herstelserver zo instellen dat een pop VM kan worden gerepliceerd naar deze bandbreedte controleren.  U kunt dit overslaan, maar niet mogelijk bandbreedte meten, tenzij u dit doen.

1. Als u een clusterknooppunt gebruiken wilt, zoals de replica Hyper-V Replica makelaar configureren:

    - Open in **Serverbeheer** **Failoverclusterbeheer**.
    - Verbinding maken met het cluster, de naam van de cluster markeren en klik op **Acties** > **Rol configureren** om de wizard beschikbaarheid te openen.
    - Klik op **Hyper-V Replica makelaar**in **Rol selecteren** . Geef in de wizard een **NetBIOS-naam** en het **IP-adres** moet worden gebruikt als het verbindingspunt dat aan het cluster (ook wel een client-toegangspunt genoemd). De **Hyper-V Replica makelaar** worden geconfigureerd, resulteert in de punt naam van een klant toegang die Houd er rekening mee.
    - Controleer of de rol van Hyper-V Replica makelaar komt is online en kan worden uitgevoerd tussen alle knooppunten van de cluster. Klik met de rechtermuisknop op de rol hiervoor, wijs **verplaatsen**en klik vervolgens op **Knooppunt selecteren**. Selecteer een knooppunt > **OK**.
    - Als u certificaatverificatie gebruikt, controleert u of elk clusterknooppunt en de client toegangspunt hebben allemaal het certificaat dat is geïnstalleerd.
2.  Een replicaserver inschakelen:

    - Voor een cluster open mislukt Cluster beheren, verbinding maken met het cluster en klik op **rollen** > select rol > **Herhaling instelling**s > **dit cluster als replicaserver inschakelen**. Houd er rekening mee dat als u een cluster als de replica gebruikt moet u de rol Hyper-V Replica makelaar presenteren op de cluster in de primaire site ook hebt.
    - Open Hyper-V-beheer voor een zelfstandige server. Klik in het deelvenster **Acties** **Hyper-V-instellingen** op voor de server die u wilt inschakelen en klik op **deze computer als replicaserver inschakelen**in **Herhaling configuratie** .
3. Verificatie instellen:

    - Selecteer in de **poorten verificatie en** hoe u om de primaire server en de verificatie-poorten te verifiëren. Als u gebruikmaakt van certificaat klikt u op **Certificaat selecteren** om een te selecteren. Kerberos gebruiken als de primaire en herstelbestanden Hyper-V-hosts in hetzelfde domein, of vertrouwde domeinen. Certificaten voor verschillende domeinen of een werkgroep-implementatie gebruiken.
    - Toestaan dat **alle** geverifieerde (primair) server herhaling gegevens te verzenden naar deze replicaserver in sectie **autorisatie en opslag** . Klik op **OK** of **toe te passen**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

    - Voer **netsh http servicestate weergeven** om te controleren of de luisteraar ervan af wordt uitgevoerd voor de opgegeven protocol/poort:  
4. Firewalls instellen. Firewallregels worden gemaakt tijdens de installatie van Hyper-V als u wilt dat verkeer is toegestaan op standaard poorten (HTTPS op 443, Kerberos op 80). Deze regels als volgt inschakelen:

        - Certificate authentication on cluster (443): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}**
        - Kerberos authentication on cluster (80): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}**
        - Certificate authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"**
        - Kerberos authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"**

## <a name="step-3-run-the-capacity-planner-tool"></a>Stap 3: Het capaciteit teamplanner-hulpprogramma uitvoeren

Nadat u hebt uw primaire site voorbereid en instellen op een herstelserver kunt u het hulpmiddel kunt uitvoeren.

1. [Download](https://www.microsoft.com/download/details.aspx?id=39057) het hulpprogramma uit het Microsoft Download Center.
2. Het hulpprogramma uitvoeren vanaf een van de primaire servers (of een van de knooppunten uit het primaire cluster). Met de rechtermuisknop op het .exe-bestand en kies vervolgens **Als administrator uitvoeren**.
3. Geef voor het gewenste hoe lang voor het verzamelen van gegevens in **voordat u begint** . Het is raadzaam om dat u het hulpprogramma uitvoeren tijdens productietijd om ervoor te zorgen dat gegevens medewerker is. Als u alleen probeert voor het valideren van netwerkconnectiviteit, kunt u alleen een minuut verzamelen.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. In de **primaire site details** Geef de servernaam of de FQDN-naam voor de host van een zelfstandig product of voor een cluster Geef de FQDN van de client punt, de clusternaam van de of een van de knooppunten in het cluster accepteren en klik op **volgende**. Het hulpmiddel wordt automatisch gedetecteerd voor de naam van de server die wordt uitgevoerd. Het hulpmiddel hervat VMs die kunnen worden gecontroleerd voor de opgegeven servers.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. Selecteer **tests die betrekking hebben op personen replica site overslaan**in **Replica Site Details** als u naar Azure repliceren bent of als u met een secundaire datacenter repliceren bent en een replicaserver nog niet hebt ingesteld. Als u met een secundaire datacenter zijn repliceren en u een ReplicaType in de FQDN-naam van de server zelfstandig product of de client-toegangspunt voor het cluster in **de naam van de Server (of) Hyper-V Replica makelaar Initiaal**hebt ingesteld.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. **Uitgebreide Replica** details inschakelen **de tests die betrekking hebben op personen uitgebreide Replica site overslaan**. Ze worden niet ondersteund door sites worden hersteld.
7. In **Kies VMs naar repliceren** de hulpmiddelen voor verbinding maakt met de server of cluster en VMs weergegeven en schijven uitgevoerd op de primaire server, volgens de instellingen u hebt opgegeven op de pagina **Details van primaire Site** . Opmerking waarop wordt uitgevoerd VMs die al zijn ingeschakeld voor replicatie of die niet weergegeven. Selecteer de VMs waarvoor u wilt voor het verzamelen van de doelstellingen. De VHD's automatisch selecteren verzamelt gegevens voor de VMs te.
9. Als u een replicaserver of cluster hebt geconfigureerd, **netwerkgegevens** Geef in de niet-geheel exacte WAN-bandbreedte u denkt dat wordt gebruikt tussen de primaire en replica sites en selecteert u de certificaten als u verificatie via clientcertificaat hebt geconfigureerd.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10. In het **overzicht van** instellingen controleren en klik op **volgende** om te beginnen met het verzamelen van de doelstellingen. Voortgang van het hulpmiddel en de status wordt weergegeven op de pagina **Capaciteit berekenen** . Wanneer is de hulpmiddel is voltooid met klikt op **View-rapport** om de uitvoer eens. Logboeken en rapporten worden standaard opgeslagen in **%systemdrive%\Users\Public\Documents\Capacity teamplanner**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)


## <a name="step-4-interpret-the-results"></a>Stap 4: De resultaten interpreteren
Hier volgen de belangrijke cijfers. Aan de doelstellingen die hier niet worden weergegeven, kunt u negeren. Ze zijn niet relevant zijn voor sites worden hersteld.

### <a name="on-premises-to-on-premises-replication"></a>On-premises implementatie naar on-premises implementatie herhaling
  - Gevolgen van replicatie op van de primaire host berekeningscluster, geheugen
  - Gevolgen van replicatie op de primaire, herstel hosts van opslag schijfruimte, IO's / s
  - Totale bandbreedte nodig is voor delta replicatie (Mbps)
  - Waargenomen netwerkbandbreedte tussen de primaire host en de host herstel (Mbps)
  - Suggestie voor het ideale aantal actieve parallelle overdrachten tussen de twee hosts/clusters

### <a name="on-premises-to-azure-replication"></a>On-premises implementatie Azure replicatie
  - Gevolgen van replicatie op van de primaire host berekeningscluster, geheugen
  - Gevolgen van herhaling van de primaire host opslag schijfruimte, IO's / s
  - Totale bandbreedte nodig is voor delta replicatie (Mbps)

## <a name="more-resources"></a>Meer informatiebronnen

- Lees het document bij het downloaden van het hulpmiddel voor gedetailleerde informatie over het hulpmiddel.
- Bekijk de stapsgewijze instructies voor het hulpmiddel in Keith Mayer van [TechNet-blog](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
- [Resultaten](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) van onze prestaties testen on-premises naar on-premises implementatie Hyper-V herhaling



## <a name="next-steps"></a>Volgende stappen

U kunt starten implementeert sites worden hersteld nadat u klaar bent met het plannen van de capaciteit:

- [Hyper-V VMs in VMM wolken repliceren naar Azure](site-recovery-vmm-to-azure.md)
- [Hyper-V VMs (zonder VMM) repliceren naar Azure](site-recovery-hyper-v-site-to-azure.md)
- [Hyper-V VMs repliceren tussen VMM-sites](site-recovery-vmm-to-vmm.md)
- [Hyper-V VMs repliceren tussen VMM-sites met SAN](site-recovery-vmm-san.md)
- [Repliceren hyper-V VMs op enkele VMM-server](site-recovery-single-vmm.md)