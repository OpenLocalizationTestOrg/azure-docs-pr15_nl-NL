<properties
    pageTitle="Groep altijd op beschikbaarheid configureren in Azure VM - klassiek"
    description="Maak een permanente beschikbaarheid van de groep met Azure virtuele Machines. In deze zelfstudie wordt hoofdzakelijk de gebruikersinterface en de hulpmiddelen in plaats van uitvoeren van scripts."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm---classic"></a>Groep altijd op beschikbaarheid configureren in Azure VM - klassiek

> [AZURE.SELECTOR]
- [Resourcemanager: sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Resourcemanager: handmatige](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassieke: UI](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassieke: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Deze zelfstudie end-to-end ziet u hoe u de beschikbaarheid van de groepen met SQL Server altijd op Azure virtuele machines waarop implementeren.

Aan het einde van de zelfstudie, wordt uw oplossing SQL Server altijd op in Azure bestaan uit de volgende elementen:

- Een virtueel netwerk met meerdere subnetten, inclusief een front-end en een back-enddatabase subnet

- Een domeincontroller met een domein AD (Active Directory)

- Twee SQL Server VMs geïmplementeerd in het back-enddatabase subnet en toegevoegd aan het AD-domein

- Een 3-knooppunt WSFC cluster met het knooppunt meeste quorum-model

- Een groep met twee synchroon doorvoeren replica's van de beschikbaarheid van een database beschikbaarheid

De onderstaande afbeelding is een grafische weergave van de oplossing.

![Test testomgeving architectuur voor AG in Azure wordt aangegeven](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Houd er rekening mee dat dit een mogelijke configuratie is. U kunt bijvoorbeeld het aantal VMs voor een beschikbaarheidsgroep van de twee kopieën minimaliseren om op te slaan op een berekeningscluster uur in Azure wordt aangegeven met behulp van de domeincontroller als de quorum bestandssharewitness in een 2-knooppunt WSFC cluster. Deze methode beperkt het aantal VM door een van de bovenstaande configuratie.

Deze zelfstudie wordt ervan uitgegaan:

- U hebt al een Azure-account.

- U weet hoe voor het inrichten van een klassieke SQL Server-VM vanuit de galerie VM met behulp van de grafische gebruikersinterface.

- U hebt al een goed begrip van altijd op beschikbaarheid van de groepen. Zie [Altijd op beschikbaarheidsgroepen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx)voor meer informatie.

>[AZURE.NOTE] Als u geïnteresseerd altijd bent op beschikbaarheidsgroepen gebruiken met SharePoint, raadpleegt u ook [Configureren SQL Server 2012 altijd op beschikbaarheid groepen voor SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Het virtuele netwerk en Domain controllerserver maken

U begint met een nieuw account voor Azure proefabonnement. Zodra u klaar bent met uw account instellen, moet u zich in het beginscherm van de Azure klassieke portal.

1. Klik op de knop **Nieuw** in de linkerbenedenhoek van de pagina, zoals hieronder wordt weergegeven.

    ![Klik op Nieuw in de portal](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)

1. Klik op **Netwerkservices**, en vervolgens op **Virtueel netwerk,** en klik op **Aangepaste maken**, zoals hieronder wordt weergegeven.

    ![Virtuele netwerk maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)

1. Klik in het dialoogvenster **VIRTUEEL netwerk maken** door een nieuwe virtuele netwerk door door de pagina's met de volgende instellingen te maken. 

  	|Pagina|Instellingen|
|---|---|
|Virtual Network Details|**NAAM = ContosoNET**<br/>**REGIO = West US**|
|DNS-Servers en VPN-verbinding|Geen|
|Virtual Network adres spaties|Instellingen worden weergegeven in de onderstaande schermafbeelding: ![Virtuele netwerk maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png)|

1. U maakt vervolgens de VM die u als de domeincontroller (domeincontroller) wilt gebruiken. Op **Nieuw** opnieuw, en vervolgens **berekenen**, en vervolgens **VM**, en klik vervolgens **Uit galerie**, zoals hieronder wordt weergegeven.

    ![Een VM maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)

1. In het dialoogvenster **A VM maken** door een nieuwe VM door door de pagina's met de onderstaande instellingen te configureren 

  	|Pagina|Instellingen|
|---|---|
|Selecteer het besturingssysteem VM|Windows Server 2012 R2 Datacenter|
|VM configuratie|**RELEASEDATUM versie** = (nieuwste)<br/>**De naam van de VM** = ContosoDC<br/>**Laag** = STANDARD<br/>**Grootte** = A2 (2 cores)<br/>**Nieuwe gebruikersnaam in te voeren** = AzureAdmin<br/>**Een nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000|
|VM configuratie|**CLOUDSERVICE** = maken een nieuwe cloudservice<br/>**De naam van de DNS-SERVICE CLOUD** = de naam van een unieke cloud-service<br/>**DNS-naam** een unieke naam = (ex: ContosoDC123)<br/>**Land/AFFINITEIT groep/virtuele netwerk** = ContosoNET<br/>**Virtuele NETWERKSUBNETTEN** = Back(10.10.2.0/24)<br/>**Opslag ACCOUNT** = gebruiken een account automatisch gegenereerde opslag<br/>**Beschikbaarheid instellen** = (geen)|
|VM-opties|Standaardwaarden gebruiken|

Wanneer u klaar bent met het configureren van de nieuwe VM, wacht u totdat de VM moeten provsioned. Dit proces duurt enige tijd om te voltooien en als u op het tabblad **VM** in de portal van Azure klassieke, ziet u ContosoDC waarbij Staten uit **starten (Provisioning)** **gestopt**, **starten**, **voorlopig (Provisioning)**en ten slotte **actief**.

De domeincontroller-server is nu is ingericht. Configureer het Active Directory-domein wordt u vervolgens op deze domeincontroller-server.

## <a name="configure-the-domain-controller"></a>De domeincontroller configureren

In de volgende stappen, kunt u de computer ContosoDC configureren als een domeincontroller voor corp.contoso.com.

1. Klik in de portal door de machine **ContosoDC** te selecteren. Klik op het tabblad **Dashboard** op **verbinding maken** om een RDP-bestand voor toegang tot extern bureaublad te openen.

    ![Verbinding maken met Vritual computer](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)

1. Meld u aan met uw geconfigureerde beheerdersaccount (**\AzureAdmin**) en wachtwoord (**Contoso! 000**).

1. Standaard moet het dashboard **Serverbeheer** worden weergegeven.

1. Klik op de koppeling **toevoegen rollen en functies** op het dashboard.

    ![Server Explorer rollen toevoegen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)

1. Selecteer in de **volgende** totdat u toegang hebt tot de sectie **Serverrollen** .

1. Selecteer de rollen **Active Directory Domain Services** en **DNS-Server** . Wanneer u wordt gevraagd, voegt u aanvullende functies vereist door deze rollen.

    >[AZURE.NOTE] Hier krijgt u een waarschuwing dat er geen statische IP-adres is gegevensvalidatie. Als u test de configuratie, klik op continue. Voor productie scenario's [PowerShell om in te stellen van het statische IP-adres van de computer van de domeincontroller gebruiken](./virtual-network/virtual-networks-reserved-private-ip.md).

    ![Dialoogvenster rollen toevoegen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)

1. Klik op **volgende** totdat u de sectie **bevestiging** hebt bereikt. Schakel het selectievakje **Start opnieuw op de doelserver automatisch als vereist** .

1. Klik op **installeren**.

1. Nadat de functies geïnstalleerd hebt, kunt u terug naar het dashboard **Serverbeheer** .

1. Selecteer de nieuwe **AD DS** -optie in het linkerdeelvenster.

1. Klik op de koppeling **meer** op de gele waarschuwing-balk.

    ![Dialoogvenster van de AD DS op DNS-Server VM](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)

1. In de kolom **actie** van het dialoogvenster **Taakgegevens voor alle Server** , klikt u op **niveau verhogen deze server met een domeincontroller**.

1. Gebruik de volgende waarden in de **Active Directory Domain Services configuratiewizard**:

  	|Pagina|Instelling|
|---|---|
|, Implementatieconfiguratie|**Een nieuw bos toevoegen** geselecteerde =<br/>**Hoofddomein** corp.contoso.com =|
|Domeincontroller-opties|**Wachtwoord** = Contoso! 000<br/>**Bevestig het wachtwoord** = Contoso! 000|

1. Klik op **volgende** om te gaan tot en met de andere pagina's in de wizard. Controleer of u het volgende bericht worden weergegeven op de pagina **Controle van vereisten** : **alle vereiste controles gevolg is voltooid**. Houd er rekening mee dat u moet een toepasselijke waarschuwingsberichten bekijken, maar het is mogelijk om door te gaan met de installatie.

1. Klik op **installeren**. De **ContosoDC** virtuele machine zal automatisch opnieuw opstarten.

## <a name="configure-domain-accounts"></a>Domeinaccounts configureren

De volgende stappen uit configureren de AD (Active Directory)-accounts voor later gebruik.

1. Meld u weer aan de computer **ContosoDC** .

1. Selecteer **Extra** en klik op **Beheerderscentrum voor Active Directory**in **Serverbeheer** .

    ![Active Directory-Beheerderscentrum](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)

1. Klik in het **Beheerderscentrum voor Active Directory** select **corp (lokaal)** in het linkerdeelvenster.

1. In het rechterdeelvenster van **taken** , selecteert u daarna **Nieuw** en klik op **gebruiker**. Gebruik de volgende instellingen:

  	|Instelling|Waarde|
|---|---|
|**Voornaam**|Installeren|
|**Gebruiker SamAccountName**|Installeren|
|**Wachtwoord**|Contoso! 000|
|**Bevestig het wachtwoord**|Contoso! 000|
|**Andere wachtwoordopties**|Geselecteerd|
|**Wachtwoord verloopt nooit**|Ingeschakeld|

1. Klik op **OK** als u wilt maken van de gebruiker **installeren** . Dit account wordt gebruikt voor het configureren van het failovercluster en de van de beschikbaarheidsgroep.

1. Maak twee extra gebruikers met dezelfde stappen uit: **CORP\SQLSvc1** en **CORP\SQLSvc2**. Deze accounts worden gebruikt voor de SQL Server-exemplaren. Vervolgens moet u **CORP\Install** geven de benodigde machtigingen voor het configureren van Windows Service Failover cluster (WSFC).

1. Selecteer in het **Beheerderscentrum voor Active Directory**, **corp (lokaal)** in het linkerdeelvenster. Klik in het rechterdeelvenster van **taken** , klikt u op **Eigenschappen**.

    ![CORP gebruikerseigenschappen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)

1. Selecteer **extensies**en klik vervolgens op de knop **Geavanceerd** op het tabblad **beveiliging** .

1. Klik in het dialoogvenster **Geavanceerde beveiligingsinstellingen voor corp** . Klik op **toevoegen**.

1. Klik op **een principal selecteren**. Zoek naar **CORP\Install**. Klik op **OK**.

1. Selecteer de machtigingen voor **alle eigenschappen lezen** en **objecten van de Computer maken** .

    ![Gebruikersmachtigingen Corp](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)

1. Klik op **OK**en klik vervolgens nogmaals op **OK** . Sluit het eigenschappenvenster voor corp.

Nu dat u klaar bent met het configureren van Active Directory en de gebruikersobjecten, u maakt u drie SQL Server VMs en voeg ze toe aan dit domein.

## <a name="create-the-sql-server-vms"></a>De SQL Server-VMs maken

Maak vervolgens drie VMs, inclusief WSFC clusterknooppunt en twee SQL Server VMs. Elk van de VMs maken, gaat u terug naar de portal van Azure klassieke, klikt u op **Nieuw**, **berekenen**, **virtuele Machine**en vervolgens **Vanuit de galerie**. Gebruik vervolgens de sjablonen in de volgende tabel om te maken van de VMs.

|Pagina|VM1|VM2|VM3|
|---|---|---|---|
|Selecteer het besturingssysteem VM|**Windows Server 2012 R2 Datacenter**|**SQL Server 2014 RTM Enterprise**|**SQL Server 2014 RTM Enterprise**|
|VM configuratie|**RELEASEDATUM versie** = (nieuwste)<br/>**De naam van de VM** = ContosoWSFCNode<br/>**Laag** = STANDARD<br/>**Grootte** = A2 (2 cores)<br/>**Nieuwe gebruikersnaam in te voeren** = AzureAdmin<br/>**Een nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000|**RELEASEDATUM versie** = (nieuwste)<br/>**De naam van de VM** = ContosoSQL1<br/>**Laag** = STANDARD<br/>**Grootte** = A3 (4 cores)<br/>**Nieuwe gebruikersnaam in te voeren** = AzureAdmin<br/>**Een nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000|**RELEASEDATUM versie** = (nieuwste)<br/>**De naam van de VM** = ContosoSQL2<br/>**Laag** = STANDARD<br/>**Grootte** = A3 (4 cores)<br/>**Nieuwe gebruikersnaam in te voeren** = AzureAdmin<br/>**Een nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000|
|VM configuratie|**CLOUDSERVICE** = eerder gemaakte unieke Cloud Service DNS-naam (ex: ContosoDC123)<br/>**Land/AFFINITEIT groep/virtuele netwerk** = ContosoNET<br/>**Virtuele NETWERKSUBNETTEN** = Back(10.10.2.0/24)<br/>**Opslag ACCOUNT** = gebruiken een account automatisch gegenereerde opslag<br/>**Beschikbaarheid instellen** = maken een beschikbaarheid instellen<br/>**Beschikbaarheid van de naam instellen** = SQLHADR|**CLOUDSERVICE** = eerder gemaakte unieke Cloud Service DNS-naam (ex: ContosoDC123)<br/>**Land/AFFINITEIT groep/virtuele netwerk** = ContosoNET<br/>**Virtuele NETWERKSUBNETTEN** = Back(10.10.2.0/24)<br/>**Opslag ACCOUNT** = gebruiken een account automatisch gegenereerde opslag<br/>**Beschikbaarheid instellen** = SQLHADR (u kunt ook de beschikbaarheid instellen nadat u de computer hebt gemaakt. Alle drie computers moeten worden toegewezen aan de set van de beschikbaarheid van SQLHADR.)|**CLOUDSERVICE** = eerder gemaakte unieke Cloud Service DNS-naam (ex: ContosoDC123)<br/>**Land/AFFINITEIT groep/virtuele netwerk** = ContosoNET<br/>**Virtuele NETWERKSUBNETTEN** = Back(10.10.2.0/24)<br/>**Opslag ACCOUNT** = gebruiken een account automatisch gegenereerde opslag<br/>**Beschikbaarheid instellen** = SQLHADR (u kunt ook de beschikbaarheid instellen nadat u de computer hebt gemaakt. Alle drie computers moeten worden toegewezen aan de set van de beschikbaarheid van SQLHADR.)|
|VM-opties|Standaardwaarden gebruiken|Standaardwaarden gebruiken|Standaardwaarden gebruiken|

<br/>

>[AZURE.NOTE] De configuratie van de vorige stappen worden voorgesteld standaard laag virtuele machines, omdat eenvoudige laag machines taakverdeling eindpunten vereist later maken van een groep beschikbaarheid listeners niet ondersteunen. Ook, de grootte van de computer die hier wordt getoond zijn bedoeld voor het testen van de beschikbaarheid van groepen in Azure VMs. Zie de aanbevelingen voor SQL Server machine grootten en configuratie in [prestaties aanbevolen procedures voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-performance.md)voor de beste prestaties op productie werkbelasting.

Nadat u de drie VMs volledig is ingericht, moet u deze toevoegen aan het **corp.contoso.com** -domein en CORP\Install beheerdersrechten verlenen aan de machines. Gebruik hiervoor de volgende stappen voor elk van de drie VMs.

1. Wijzig eerst het adres van de voorkeur DNS-server. Begin met het downloaden van elke VM extern bureaublad (RDP) bestand naar uw lokale map door te klikken en te klikken op de knop **verbinding maken met** de VM selecteren in de lijst. Als u wilt een VM hebt geselecteerd, klikt u ergens maar de eerste cel in de rij, zoals hieronder wordt weergegeven.

    ![Download het RDP-bestand](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)

1. Start het RDP-bestand dat u hebt gedownload en meld u aan bij het gebruik van uw geconfigureerde VM beheerdersaccount (**BUILTIN\AzureAdmin**) en wachtwoord (**Contoso! 000**).

1. Nadat u bent aangemeld, ziet u het dashboard **Serverbeheer** . Klik in het linkerdeelvenster op **Lokale Server** .

1. Selecteer de koppeling **IPv4-adres toegewezen door DHCP, IPv6 ingeschakeld** .

1. Selecteer het netwerkpictogram in het venster **Netwerkverbindingen** .

    ![VM voorkeur DNS-servers wijzigen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)

1. Klik op de balk met opdrachten op **de instellingen van deze verbinding wijzigen** (afhankelijk van de grootte van het venster, u moet mogelijk tot klikt u op de dubbele pijl naar rechts om te zien van deze opdracht).

1. Selecteer **Internet Protocol versie 4 (TCP/IPv4)** en klik op Eigenschappen.

1. Selecteer gebruiken de volgende DNS-server adressen en **10.10.2.4** in **voorkeur DNS-server**opgeven.

1. De adres- **10.10.2.4** is het adres dat is toegewezen aan een VM in het subnet 10.10.2.0/24 in een Azure virtuele netwerk en die VM **ContosoDC**is. Om te verifiëren **ContosoDC**van IP-adres, gebruikt u de **nslookup contosodc** in de opdrachtprompt, zoals hieronder wordt weergegeven.

    ![Gebruik NSLOOKUP IP-adres voor domeincontroller vinden](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)

1. Klik op O**K** en vervolgens op **sluiten** als de wijzigingen wilt doorvoeren. U bent nu kunnen deelnemen aan de VM naar **corp.contoso.com**.

1. Klik op de koppeling **werkgroep** terug in het venster **Server voor lokale** .

1. Klik op **wijzigen**in de sectie **De naam van de Computer** .

1. Schakel het selectievakje **domein** en **corp.contoso.com** Typ in het tekstvak. Klik op **OK**.

1. Geef de referenties voor de standaard domein-beheerdersaccount (**CORP\AzureAdmin**) en het wachtwoord in het dialoogvenster **Windows-beveiliging** pop (**Contoso! 000**).

1. Wanneer u het bericht "Welkom bij het domein corp.contoso.com" ziet, klikt u op **OK**.

1. Klik op **sluiten**en klik vervolgens op **Nu opnieuw opstarten** in het dialoogvenster pop.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-vm"></a>De gebruiker Corp\Install als een beheerder op elke VM toevoegen:

1. Wacht totdat de VM opnieuw is opgestart, en vervolgens het RDP-bestand opnieuw aan te melden bij de VM met behulp van **BUILTIN\AzureAdmin** account starten.

1. In **Serverbeheer** Selecteer **Extra**en klik vervolgens op **Beheer van de Computer**.

    ![Computerbeheer van de](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)

1. Klik in het venster **Computer Management** Vouw **lokale gebruikers en groepen**en selecteer **groepen**.

1. Dubbelklik op de groep **Administrators** .

1. Klik in het dialoogvenster **Eigenschappen van beheerders** op de knop **toevoegen** .

1. Voer de gebruiker **CORP\Install**en klik vervolgens op **OK**. Wanneer u hierom wordt gevraagd om referenties, gebruikt u het account **AzureAdmin** met de **Contoso! 000** wachtwoord.

1. Klik op **OK** om te sluiten van het dialoogvenster **Eigenschappen van de beheerder** .

### <a name="add-the-failover-clustering-feature-to-each-vm"></a>De functie **Failoverclustering** toevoegen aan elke VM.

1. Klik in het dashboard **Serverbeheer** op **functies toevoegen en functies**.

1. Klik in de **rollen toevoegen en functies Wizard**op **volgende** totdat u toegang hebt tot de pagina **onderdelen** .

1. Selecteer **Failoverclustering**. Wanneer u wordt gevraagd, voegt u andere afhankelijke functies toe.

    ![Functie voor failoverclusters voor VM toevoegen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)

1. Klik op **volgende**en klik vervolgens op **de bevestigingspagina** op **installeren** .

1. Wanneer de functie **Failoverclustering** installatie is voltooid, klikt u op **sluiten**.

1. Meld u af bij de VM.

1. Herhaal de stappen in deze sectie voor alle drie servers-- **ContosoWSFCNode**, **ContosoSQL1**en **ContosoSQL2**.

De SQL Server-VMs nu is ingericht en uitgevoerd, maar ze worden geïnstalleerd met SQL Server met standaardopties.

## <a name="create-the-wsfc-cluster"></a>Het Cluster WSFC maken

In dit gedeelte maakt u het WSFC cluster die als host fungeert de van de beschikbaarheidsgroep die u later wilt maken. Nu moet u de volgende handelingen uit op elk van de drie VMs die u in het cluster WSFC gebruiken zult hebben gedaan:

- Volledig deze is ingericht in Azure wordt aangegeven

- Gekoppelde VM op het domein

- Toegevoegde **CORP\Install** naar de lokale beheerdersgroep

- De functie Failoverclustering toegevoegd

Alles dit zijn de vereisten op elke VM voordat u deze aan het cluster WSFC deelnemen kunt.

Houd er ook rekening mee dat het Azure virtuele netwerk niet meer op dezelfde manier als een on-premises netwerk werkt. U moet het cluster maken in de volgende volgorde:

1. Maak een cluster met één knooppunt op een van de knooppunten (**ContosoSQL1**).

1. Wijzig het IP-adres van cluster naar een niet-gebruikte IP-adres (**10.10.2.101**).

1. Geef de naam van het cluster online.

1. Voeg de andere knooppunten (**ContosoSQL2** en **ContosoWSFCNode**).

Volg de onderstaande stappen voor het uitvoeren van deze taken die volledig configureert het cluster.

1. Start het RDP-bestand voor **ContosoSQL1** en meld u aan met het domeinaccount **CORP\Install**.

1. In het dashboard **Serverbeheer** Selecteer **Extra**en klik vervolgens op **Failoverclusterbeheer**.

1. Klik in het linkerdeelvenster met de rechtermuisknop op **Failoverclusterbeheer**en klik vervolgens op **een Cluster maken**, zoals hieronder wordt weergegeven.

    ![Cluster maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)

1. Klik in de Wizard Cluster maken maken in een cluster met één knooppunt doorlopen van de pagina's met de volgende instellingen:

  	|Pagina|Instellingen|
|---|---|
|Voordat u begint|Standaardwaarden gebruiken|
|Selecteer de optie Servers|Typ **ContosoSQL1** in **de naam van de Enter-server** en klik op **toevoegen**|
|Waarschuwing van gegevensvalidatie|Selecteer **Nee. ik hoeven niet ondersteuning van Microsoft voor dit cluster en kunnen daarom niet wilt uitvoeren van de tests gegevensvalidatie. Wanneer ik op volgende hebt geklikt, doorgaan met het maken van het cluster**.|
|Toegangspunt voor het beheer van het Cluster|Type **Cluster1** in **clusternaam**|
|Bevestiging|Gebruik standaardwaarden tenzij u opslag spaties gebruikt. Zie de opmerking deze tabel.|

    >[AZURE.WARNING] Als u [Opslag spaties](https://technet.microsoft.com/library/hh831739), die meerdere schijven van in opslagpools groepen, moet u het selectievakje **alle in aanmerking komend opslagruimte aan het cluster toevoegen** op **de bevestigingspagina** uitschakelen. Als u deze optie niet uitschakelt doen, wordt de virtuele schijven tijdens het clustering worden losgekoppeld. Hierdoor worden ze ook niet in schijf Manager of Explorer totdat de opslagruimte spaties zijn verwijderd uit cluster en vervolgens opnieuw via PowerShell.

1. In het linkerdeelvenster, **Failoverclusterbeheer**uitvouwen en klik vervolgens op **Cluster1.corp.contoso.com**.

1. In het middelste deelvenster, schuif omlaag naar de sectie **Cluster Core Resources** en vouwt u uit de **naam: Clutser1** details. U ziet zowel de **naam** en het **IP-adres** resources in de stand **mislukt** . De bron van het IP-adres worden niet online gebracht omdat het cluster is toegewezen van hetzelfde IP-adres als die van de computer zelf, namelijk een dubbel adres.

1. Met de rechtermuisknop op de mislukte bronnen **IP-adres** en klik vervolgens op **Eigenschappen**.

    ![Eigenschappen van cluster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)

1. Selecteer **Statisch IP-adres** en **10.10.2.101** opgeven in het vak adres. Klik vervolgens op **OK**.

1. Klik in de sectie **Cluster Core Resources** met de rechtermuisknop op **naam: Cluster1** en klik op **Online brengen**. Klik, wacht u totdat beide resources online zijn. Wanneer de cluster naam resource afkomstig online is, de domeincontroller-server wordt bijgewerkt met een nieuw account voor AD-computer. Dit account AD wordt later uit te voeren de beschikbaarheid van de groep gegroepeerde-service gebruikt.

1. Tot slot dat u de resterende knooppunten toevoegen aan het cluster. Klik in de browser met de rechtermuisknop op **Cluster1.corp.contoso.com** en klikt u op **Knooppunt toevoegen**, zoals hieronder wordt weergegeven.

    ![Knooppunt toevoegen aan de Cluster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)

1. Klik in de **Wizard knooppunt toevoegen**, klikt u op **volgende**. In de pagina **Selecteer Servers** **ContosoSQL2** en **ContosoWSFCNode** aan de lijst toevoegen door te typen van de servernaam in **de naam van de Enter-server** en klik vervolgens op **toevoegen**te klikken. Wanneer u klaar bent, klikt u op **volgende**.

1. Op de pagina **Validatiewaarschuwing** , klikt u op **Nee** (in een productieschema als u de validatietests uitvoert). Klik vervolgens op **volgende**.

1. Klik op **volgende** als u wilt toevoegen van de knooppunten in **het bevestigingsvenster** .

    >[AZURE.WARNING] Als u [Opslag spaties](https://technet.microsoft.com/library/hh831739), die meerdere schijven van in opslagpools groepen, moet u het selectievakje **alle in aanmerking komend opslagruimte aan het cluster toevoegen** uitschakelen. Als u deze optie niet uitschakelt doen, wordt de virtuele schijven tijdens het clustering worden losgekoppeld. Hierdoor worden ze ook niet in schijf Manager of Explorer totdat de opslagruimte spaties zijn verwijderd uit cluster en vervolgens opnieuw via PowerShell.

1. Wanneer de knooppunten zijn toegevoegd aan het cluster, klikt u op **Voltooien**. Failoverclusterbeheer wordt nu weergegeven dat uw cluster heeft drie knooppunten en somt u deze in de container **knooppunten** .

1. Meld u af bij de sessie voor extern bureaublad.

## <a name="prepare-the-sql-server-instances-for-availability-group"></a>Groep beschikbaarheid van de exemplaren van SQL Server voorbereiden

In deze sectie, wordt u de volgende bewerkingen uit op zowel **ContosoSQL1** als **contosoSQL2**doen:

- Een aanmelding voor **Systeemaccount** met een vereiste machtigingen instellen in het standaard SQL Server-exemplaar toevoegen

- **CORP\Install** als een rol systeembeheerder toevoegen aan de standaard SQL Server-instantie

- Open de firewall voor externe toegang van SQL Server

- De functie altijd op beschikbaarheidsgroepen inschakelen

- Voor het wijzigen van de SQL Server-account in **CORP\SQLSvc1** en **CORP\SQLSvc2**, is het respectievelijk

Deze acties kunnen worden uitgevoerd in een willekeurige volgorde. Echter doorlopen de onderstaande stappen die ze in volgorde. Volg de stappen voor zowel **ContosoSQL1** als **ContosoSQL2**:

1. Als u niet afmelden bij de sessie voor extern bureaublad voor VM hebt aangemeld, moet u dat nu doen.

1. Start de RDP-bestanden voor **ContosoSQL1** en **ContosoSQL2** en meld u aan als **BUILTIN\AzureAdmin**.

1. **Systeemaccount** eerst toevoegen aan de SQL Server-aanmeldingen en bij de benodigde machtigingen. **SQL Server Management Studio**starten.

1. Klik op **verbinding maken** met de verbinding maken met de standaard SQL Server-instantie.

1. Vouw **beveiliging**in **Object Explorer**en vouwt u **aanmeldingen**.

1. Met de rechtermuisknop op de **Systeemaccount** login en klik op **Eigenschappen**.

1. Op de pagina **Securables** voor de lokale server, selecteer **verlenen** voor de volgende machtigingen en klik op **OK**.

    - Een beschikbaarheidsgroep wijzigen

    - Verbinding maken met SQL

    - Status weergeven

1. **CORP\Install** als een rol **systeembeheerder** vervolgens toevoegen aan de standaard SQL Server-instantie. **Object Explorer**, met de rechtermuisknop op **aanmeldingen** en klik op **Nieuwe aanmelding**.

1. Typ **CORP\Install** in **aanmeldingsnaam**.

1. Selecteer in de pagina **Serverrollen** **systeembeheerder**. Klik vervolgens op **OK**. Nadat de aanmelding is gemaakt, kunt u deze kunt weergeven door **aanmeldingen** in **Object Explorer**uitvouwen.

1. U kunt vervolgens een firewallregel maken voor SQL Server. Starten vanuit **het startscherm van** **Windows Firewall met geavanceerde beveiliging**.

1. Selecteer de **Regels voor binnenkomende verbindingen**in het linkerdeelvenster. Klik in het rechterdeelvenster op **Nieuwe regel**.

1. **Type regel** op de pagina Selecteer **programma**en klik op **volgende**.

1. Selecteer **Dit programmapad** **programma** op de pagina en typ **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** in het tekstvak (als u deze aanwijzingen volgen, maar het gebruik van SQL Server 2012, de SQL Server-adreslijst **MSSQL11 is. MSSQLSERVER**). Klik vervolgens op **volgende**.

1. **Actie** op de pagina behouden **de verbinding toestaan** geselecteerd en klik op **volgende**.

1. Accepteer de standaardinstellingen in **de profielpagina,** en klik op **volgende**.

1. **Naam** op de pagina Geef de regelnaam van een, zoals **SQL Server (programmaregel)** in het tekstvak **naam** , klik vervolgens op **Voltooien**.

1. Vervolgens kunt u de functie **Altijd op beschikbaarheidsgroepen** inschakelen. Starten vanuit **het startscherm van** **SQL Server Configuration Manager**.

1. Klik in de browser op **SQL Server-Services**, en vervolgens met de rechtermuisknop op de service **SQL Server (MSSQLSERVER)** en klikt u op **Eigenschappen**.

1. Klik op het tabblad **Altijd op beschikbaarheid** en selecteer **Inschakelen altijd op beschikbaarheidsgroepen**, zoals hieronder wordt weergegeven, en klik vervolgens op **toepassen**. Klik op **OK** in het pop-upvenster en sluit het eigenschappenvenster niet nog. Nadat u de serviceaccount hebt gewijzigd, wordt u de SQL Server-service opnieuw starten.

    ![Altijd inschakelen op van beschikbaarheidsgroepen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)

1. Vervolgens kunt u de SQL Server-account wijzigen. Klik op het tabblad **Aanmelden** en vervolgens **CORP\SQLSvc1** (voor **ContosoSQL1**) of **CORP\SQLSvc2** (voor **ContosoSQL2**) Typ in het vak **Naam van het Account**en vervolgens invullen en Bevestig het wachtwoord en klik vervolgens op **OK**.

1. Klik in het pop-upvenster op **Ja** om de SQL Server-service opnieuw te starten. Nadat de SQL Server-service opnieuw is opgestart, zijn de aangebrachte wijzigingen in het eigenschappenvenster effectieve.

1. Meld u af bij de VMs.

## <a name="create-the-availability-group"></a>De beschikbaarheid van de groep hebt gemaakt

U bent nu klaar voor het configureren van een beschikbaarheidsgroep. Hieronder ziet u een overzicht van wat u doet:

- Maak een nieuwe database (**MyDB1**) op **ContosoSQL1**

- Een volledige back-up zowel een transactie back-up van de database maken

- De volledige herstellen en meld u back-ups bij **ContosoSQL2** met de optie **NORECOVERY**

- De van de beschikbaarheidsgroep (**AG1**) met synchroon doorvoeren, automatische failover en leesbare secundaire replica's maken

### <a name="create-the-mydb1-database-on-contososql1"></a>De database MyDB1 op ContosoSQL1 wilt maken:

1. Als u hebt niet al afgemeld bij de extern bureaublad-sessies voor **ContosoSQL1** en **ContosoSQL2**, moet u dit nu doen.

1. Start het RDP-bestand voor **ContosoSQL1** en meld u aan als **CORP\Install**.

1. Klik in **Verkenner**onder * *C:\**, maken van een map met de naam * *back-up**. U kunt deze directory gebruiken als u wilt een back-up en herstellen van uw database wordt gebruikt.

1. Met de rechtermuisknop op de nieuwe map, wijs **delen aan**en klik vervolgens op **specifieke personen**, zoals hieronder wordt weergegeven.

    ![Een back-upmap maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)

1. **CORP\SQLSvc1** toevoegen en hieraan de machtiging **Alleen-lezen/schrijven** en vervolgens **CORP\SQLSvc2** toevoegen en hieraan de machtiging **lezen** , zoals hieronder wordt weergegeven en klik vervolgens op **delen**. Als u het proces met gedeelde bestanden hebt voltooid, klikt u op **Gereed**.

    ![Machtigingen verlenen voor back-upmap](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)

1. Vervolgens kunt u de database maken. Klik in het menu **Start** **SQL Server Management Studio**starten en klik **verbinding maken** met de verbinding maken met de standaard SQL Server-instantie.

1. De **Object Explorer**met de rechtermuisknop op **Databases** en klik op **Nieuwe Database**.

1. Typ **MyDB1**in de **naam van de Database**en klik op **OK**.

### <a name="take-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Een volledige back-up van MyDB1 maken en terugzetten op ContosoSQL2:

1. Vervolgens neemt u een volledige back-up van de database. In het **Object Explorer**uitvouwen **Databases**, en vervolgens met de rechtermuisknop op **MyDB1**, en vervolgens wijs **taken**aan en klik vervolgens op **Back-Up**.

1. Houd in de sectie **bron** **back-up type** ingesteld op **volledige**. Klik in de sectie **bestemming** , op **verwijderen** als u wilt verwijderen van de standaard-bestandspad voor het back-upbestand.

1. In de sectie **bestemming** , klikt u op **toevoegen**.

1. Typ in het tekstvak **bestandsnaam** ** \\ContosoSQL1\backup\MyDB1.bak**. Vervolgens klikt u op **OK**en klik vervolgens op **OK** om te back-up maken van de database. Wanneer de back-up is voltooid, klikt u op **OK** om te sluit het dialoogvenster.

1. Vervolgens neemt u een transactielogboek back-up van de database. In het **Object Explorer**uitvouwen **Databases**, en vervolgens met de rechtermuisknop op **MyDB1**, en vervolgens wijs **taken**aan en klik vervolgens op **Back-Up**.

1. Selecteer in de **back-up** type **Transactielogboek**. Houd het bestandspad **bestemming** is ingesteld op de sectie die u eerder opgegeven en klik op **OK**. Zodra de back-up is voltooid, klikt u nogmaals op **OK** .

1. Vervolgens kunt u de volledige en transactie log back-ups **ContosoSQL2**herstellen. Start het RDP-bestand voor **ContosoSQL2** en meld u aan als **CORP\Install**. Laat de sessie voor extern bureaublad voor **ContosoSQL1** openen.

1. Klik in het menu **Start** **SQL Server Management Studio**starten en klik **verbinding maken** met de verbinding maken met de standaard SQL Server-instantie.

1. De **Object Explorer**met de rechtermuisknop op **Databases** en klik op **Database terugzetten**.

1. Klik in de sectie **bron** Selecteer **apparaat**en klikt u op de **...** knop.

1. In de **back-apparaten selecteren**, klikt u op **toevoegen**.

1. Typ in de back-up bestandslocatie, \\ContosoSQL1\backup, klik vervolgens op vernieuwen, en vervolgens MyDB1.bak, selecteert en vervolgens klikt u op OK en klik vervolgens nogmaals op OK. U ziet nu de volledige back-up en de back-up in de back-up wordt ingesteld als het deelvenster herstellen.

1. Ga naar de pagina met opties wellicht herstellen Selecteer herstel status, en klik vervolgens op OK om te zetten van de database. Zodra de bewerking voor terugzetten is voltooid, klikt u op OK.

### <a name="create-the-availability-group"></a>De van de beschikbaarheidsgroep hebt gemaakt:

1. Ga terug naar de externe bureaubladsessie voor **ContosoSQL1**. Klik in het **Object Explorer** in SSMS met de rechtermuisknop op **Altijd op beschikbaarheid** en klik op **Wizard van de groep nieuw beschikbaarheid**, zoals hieronder wordt weergegeven.

    ![Wizard Nieuwe groep beschikbaarheid starten](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)

1. Klik op **volgende**op de pagina **Inleiding** . **Beschikbaarheid van de groepsnaam opgeven** op de pagina **AG1** typt in **naam van de beschikbaarheid van de groep**en klik op **volgende** opnieuw.

    ![Wizard Nieuw AG AG naam opgeven](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)

1. Selecteer **MyDB1** **Databases selecteren** op de pagina en klik op **volgende**. De database voldoet aan de vereisten voor een beschikbaarheidsgroep, omdat u ten minste één volledige back-up hebt gemaakt met de beoogde primaire replica.

    ![Wizard Nieuwe AG, selecteert u Databases](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)

1. Klik op de pagina **Opgeven replica's** op **Replica toevoegen**.

    ![Wizard Nieuw AG replica's opgeven](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)

1. Het dialoogvenster **verbinding maken met de Server** verschijnt. Typ **ContosoSQL2** in **de naam van de Server**en klik op **verbinding maken**.

    ![Wizard Nieuw AG verbinding maken met Server](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)

1. Weer aan de pagina **Opgeven replica's** wordt nu **ContosoSQL2** vermeld in **Beschikbaar replica's**worden weergegeven. Configureer de replica's zoals hieronder wordt weergegeven. Wanneer u klaar bent, klikt u op **volgende**.

    ![Wizard Nieuw AG opgeven replica's (voltooid)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)

1. **Selecteer eerste synchronisatie voor gegevens** op de pagina Selecteer **alleen deelnemen** en klik op **volgende**. U hebt al uitgevoerd gegevenssynchronisatie handmatig wanneer u de volledige en transactie back-ups op **ContosoSQL1** genomen en ze op **ContosoSQL2**hersteld. U kunt in plaats daarvan niet naar de back-up en herstellen bewerkingen op uw database en selecteer **volledige** laat de beschikbaarheid van de Wizard Nieuwe groep synchronisatie van gegevens voor u uitvoeren. Dit is echter niet aanbevolen voor zeer grote databases die zijn gevonden in sommige ondernemingen.

    ![Wizard Nieuwe AG, selecteert u de synchronisatie van de oorspronkelijke gegevens](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)

1. Klik op **volgende**op de pagina **validatie** . Deze pagina moet er ongeveer hieronder uit. Er is een waarschuwing voor de configuratie luisteraar ervan af omdat u niet een luisteraar ervan af groep beschikbaarheid hebt geconfigureerd. U kunt deze waarschuwing, negeren, omdat deze zelfstudie wordt niet geconfigureerd voor een luisteraar ervan af. Zie voor meer informatie over het configureren van de luisteraar ervan af na het voltooien van deze zelfstudie [configureren een ILB luisteraar ervan af voor altijd op beschikbaarheid van groepen in Azure wordt aangegeven](virtual-machines-windows-classic-ps-sql-int-listener.md).

    ![Wizard Nieuwe AG, gegevensvalidatie](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)

1. De pagina **Summary** Klik op **Voltooien**en wacht tot de wizard configureert de van de beschikbaarheidsgroep. Op de pagina **voortgang** kunt u **meer informatie** als u wilt de gedetailleerde voortgang bekijken. Wanneer de wizard voltooid is, controleren u de pagina met **zoekresultaten** om te bevestigen dat de van de beschikbaarheidsgroep is gemaakt, zoals hieronder wordt weergegeven en klik op **sluiten** om de wizard te sluiten.

    ![Wizard Nieuw AG resultaten](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)

1. In het **Object Explorer** **Altijd op beschikbaarheid**uitvouwen en **Beschikbaarheid van de groepen**uitvouwen. U ziet nu de nieuwe beschikbaarheidsgroep in deze container. Met de rechtermuisknop op **AG1 (primair)** en klik op **Dashboard weergeven**.

    ![AG Dashboard weergeven](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)

1. Uw **Altijd op Dashboard** moet uitzien zoals hieronder wordt weergegeven. U kunt de replica's, de failover-modus van elke replica en de synchronisatiestatus zien.

    ![AG Dashboard](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)

1. Ga terug naar **Serverbeheer**, selecteer **Extra**en vervolgens starten **Failoverclusterbeheer**.

1. Vouw **Cluster1.corp.contoso.com**en vouwt u **Services en toepassingen**. Selecteer **rollen** en houd er rekening mee dat de **AG1** beschikbaarheid van de rol is gemaakt. Houd er rekening mee dat AG1 geen elk IP-adres door welke database clients verbinding met de beschikbaarheidsgroep maken kunnen omdat u niet een luisteraar ervan af hebt geconfigureerd. U kunt verbinding maken rechtstreeks naar de primaire voor lezen / schrijven bewerkingen en de secundaire knooppunten voor alleen-lezenquery's.

    ![AG in Failoverclusterbeheer](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

>[AZURE.WARNING] Probeer niet over de van de beschikbaarheidsgroep uit de Failoverclusterbeheer is mislukt. Alle failover-bewerkingen moeten worden uitgevoerd binnen **Altijd op Dashboard** in SSMS. Zie [beperkingen op gebruik van de WSFC Failoverclusterbeheer met beschikbaarheidsgroepen](https://msdn.microsoft.com/library/ff929171.aspx)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
U hebt nu geïmplementeerd SQL Server altijd op door te maken van een beschikbaarheidsgroep in Azure wordt aangegeven. Zie een luisteraar ervan af voor deze beschikbaarheidsgroep configureren [configureren een ILB luisteraar ervan af voor altijd op beschikbaarheid van groepen in Azure wordt aangegeven](virtual-machines-windows-classic-ps-sql-int-listener.md).

Zie voor meer informatie over het gebruik van SQL Server in Azure wordt aangegeven, [SQL Server op Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).
