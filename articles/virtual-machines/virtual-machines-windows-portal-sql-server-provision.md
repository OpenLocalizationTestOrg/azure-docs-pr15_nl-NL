<properties
    pageTitle="Een SQL Server-VM inrichten | Microsoft Azure"
    description="Maak en verbinding maken met een SQL Server-VM in Azure wordt aangegeven met behulp van de Portal. Deze zelfstudie gebruikt de Resource Manager-modus."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Een SQL Server virtuele machine in de Portal Azure inrichten

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

Deze zelfstudie end-to-end ziet u hoe u de Azure-Portal gebruiken voor het inrichten van een virtuele machine met SQL Server.

De galerie met Azure virtuele machine (VM) bevat een aantal afbeeldingen met Microsoft SQL Server. U kunt met een paar muisklikken, selecteer een van de SQL VM afbeeldingen in de galerie en deze in uw omgeving Azure inrichten.

In deze zelfstudie zal u het volgende doen:

- [Selecteer de afbeelding van een SQL VM in de galerie](#select-a-sql-vm-image-from-the-gallery)
- [Configureren en de VM maken](#configure-the-vm)
- [Open de VM met extern bureaublad](#open-the-vm-with-remote-desktop)
- [Verbinding maken met SQL Server op afstand](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Selecteer de afbeelding van een SQL VM in de galerie

1. Meld u aan bij de [portal van Azure](https://portal.azure.com) met uw account.

    >[AZURE.NOTE] Als u niet een Azure-account hebt, bezoekt u de [gratis proefabonnement van Azure](https://azure.microsoft.com/pricing/free-trial/).

1. Klik op de Azure-portal op **Nieuw**. De portal wordt het **Nieuw** blad geopend. De SQL Server VM resources zijn in de groep **virtuele Machines** van de Marketplace.

1. Klik in het blad **Nieuw** op **virtuele Machines**.

1. Als u wilt zien van alle beschikbare afbeeldingen, klikt u op **alle zien** in het blad **virtuele Machines** .

    ![Azure virtuele Machines Blade](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. Klik onder **Database-servers**op **SQL Server**. Mogelijk moet u omlaag schuiven om te zoeken **Database-servers**. Bekijk de beschikbare sjablonen in de SQL Server.

    ![Galerie met virtuele Machine SQL-afbeeldingen](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Elke sjabloon kunt u een SQL Server-versie en een besturingssysteem identificeren. Selecteer een van deze afbeeldingen in de lijst. Bekijk het blad details vindt u een beschrijving van de afbeelding VM.

    >[AZURE.NOTE] VM SQL-afbeeldingen voor het opnemen van de licentievoorwaarden kosten voor SQL Server in de per minuut prijzen van de VM die u maakt. Er is een andere optie naar voren-uw-eigenaar bent van-licentie (BYOL) en betaald wordt alleen voor de VM. Namen van de afbeelding worden voorafgegaan door {BYOL}. Zie [aan de slag met SQL Server op Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over deze opties.

1. Klik onder **Selecteer een implementatiemodel**, Controleer of **Resourcemanager** is geselecteerd. Resourcemanager is het aanbevolen implementatiemodel voor nieuwe virtuele machines. Klik op **maken**.

    ![SQL-VM met resourcemanager maken](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>De VM configureren
Er zijn vijf bladen voor het configureren van een SQL Server-VM.

| Stap               | Beschrijving                          |
|---------------------|-------------------------------|
| **Basisbeginselen**              | [Basisinstellingen configureren](#1-configure-basic-settings)      |
| **Grootte**                | [VM tekengrootte kiezen](#2-choose-virtual-machine-size)   |
| **Instellingen**            | [Optionele functies configureren](#3-configure-optional-features)   |
| **SQL Server-instellingen** | [SQL server-instellingen configureren](#4-configure-sql-server-settings) |
| **Overzicht**             | [Bekijk het overzicht](#5-review-the-summary)            |

## <a name="1-configure-basic-settings"></a>1. basisinstellingen configureren
Klik op het blad **Basisbeginselen** , moet u de onderstaande informatie opgeven:

* Voer een unieke **naam**voor virtuele machine.
* Geef een **gebruikersnaam in te voeren** voor het lokale beheerdersaccount op de VM. Dit account wordt ook toegevoegd aan de rol van SQL Server **systeembeheerder** vaste server.
* Geef een sterk **wachtwoord**.
* Als u meerdere abonnementen hebt, controleert u of het abonnement klopt voor de nieuwe VM.
* Typ een naam voor een nieuwe resourcegroep in het vak **resourcegroep** . U kunt ook als u wilt gebruiken van een bestaande bron Klik op groep **Selecteer bestaande**. Een resourcegroep is een verzameling verwante resources in Azure wordt aangegeven (virtuele machines, opslag-accounts, virtuele netwerken, enzovoort).

    >[AZURE.NOTE] Een nieuwe resourcegroep is nuttig als u alleen testen of het raadplegen van SQL Server-implementaties in Azure wordt aangegeven. Nadat u klaar met de toets bent, verwijder de resourcegroep om automatisch de VM en alle resources die zijn gekoppeld aan die resourcegroep verwijderen. Zie voor meer informatie over resourcegroepen, [Azure resourcemanager overzicht](../azure-resource-manager/resource-group-overview.md).

* Selecteer een **locatie** voor deze installatie.
* Klik op **OK** als de instellingen wilt opslaan.

    ![SQL basisbeginselen Blade](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. VM tekengrootte kiezen
Kies in de stap koppeling de **grootte** , de grootte van een virtuele machine in het blad **Kies een grootte** . Het blad instantie aanbevolen machine grootte op basis van de sjabloon die u hebt geselecteerd. Deze ook maakt een schatting van de maandelijkse kosten voor de VM uitvoeren.

![Opties voor SQL-VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Voor productie-werkbelastingen, is het raadzaam te selecteren van de grootte van een virtuele machine die ondersteuning biedt voor [Premium opslag](../storage/storage-premium-storage.md). Als u geen dat niveau van prestaties hoeft, gebruikt u de knop **Alles weergeven** , waarin alle machine grootte opties. U kunt bijvoorbeeld een kleinere machine gebruiken voor een ontwikkeling of testomgeving.

>[AZURE.NOTE] Zie voor meer informatie over VM formaten, [grootte voor virtuele machines](virtual-machines-windows-sizes.md). Zie [prestaties aanbevolen procedures voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-performance.md)voor overwegingen over SQL Server VM grootte.

Kies de grootte van uw computer en klik vervolgens op **selecteren**.

## <a name="3-configure-optional-features"></a>3. optioneel functies configureren
Klik op het blad **Instellingen** configureren Azure opslag, toegang en het monitoren, voor de virtuele machine.

- Geef onder **opslag**, op een **schijf Typ** standaard of Premium (SSD). Premium opslag geschikt is voor productie werkbelasting.

>[AZURE.NOTE] Als u Premium (SSD) voor de grootte van een computer die geen ondersteuning biedt voor Premium opslag selecteert, wordt de grootte van uw computer automatisch gewijzigd.  

- Onder **opslag-account**, kunt u de automatisch ingerichte opslagaccountnaam accepteren. U kunt ook klikken op **opslag-account** dat u wilt een bestaand account kiezen en configureren van het accounttype opslag. Azure maakt standaard een nieuw account voor de opslag met lokaal overtollige opslagmedia. Zie voor meer informatie over opslagopties [Azure Storage herhaling](../storage/storage-redundancy.md).

- Klik onder **netwerk**, kunt u de automatisch ingevulde waarden accepteren. U kunt ook klikken op elke functie handmatig configureren voor het **virtuele netwerk**, **Subnet**, **openbare IP-adres**en **Netwerk-beveiligingsgroep**. Houd de standaardwaarden voor de toepassing van deze zelfstudie wordt.

- Azure kunt **Monitoring** al dan niet standaard met hetzelfde opslag-account voor de VM aangewezen. U kunt hier deze instellingen wijzigen.

- Geef onder **beschikbaarheid instellen**, een set beschikbaarheid. U kunt **geen**selecteren voor de toepassing van deze zelfstudie wordt. Als u van plan bent om in te stellen SQL AlwaysOn beschikbaarheidsgroepen, configureert u de beschikbaarheid om te voorkomen dat de virtuele machine opnieuw te maken.  Zie [de beschikbaarheid van virtuele Machines beheren](virtual-machines-windows-manage-availability.md)voor meer informatie.

Wanneer u klaar bent met het configureren van deze instellingen, klik op **OK**.

## <a name="4-configure-sql-server-settings"></a>4. SQL server-instellingen configureren
Klik op het blad **SQL Server-instellingen** , specifieke instellingen en optimalisaties voor SQL Server te configureren. De instellingen die u voor SQL Server configureren kunt onder andere de volgende.

| Instelling               |
|---------------------|
| [Connectiviteit](#connectivity)              |
| [Verificatie](#authentication)                |
| [Opslagconfiguratie](#storage-configuration)            |
| [Automatische herstellen](#automated-patching) |
| [Automatische back-up](#automated-backup)             |
| [Azure belangrijke kluis-integratie](#azure-key-vault-integration)             |
| [R-Services](#r-services) |

### <a name="connectivity"></a>Connectiviteit
Geef onder **SQL-connectiviteit**, het type toegang dat u de SQL Server-instantie op deze VM wilt. Selecteer voor de toepassing van deze zelfstudie, **openbare (internet)** toe te staan dat verbindingen met SQL Server uit machines en services op internet. Met deze optie selecteert, Azure automatisch geconfigureerd in de firewall uit en de netwerk-beveiligingsgroep toe te staan dat verkeer op poort 1433.  

![Opties voor SQL-connectiviteit](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Als u wilt verbinden met SQL Server via internet, moet u ook SQL Server-verificatie, waarin wordt beschreven in het volgende gedeelte inschakelen.

>[AZURE.NOTE] Het is mogelijk meer beperkingen voor de netwerkcommunicatie toevoegen aan uw SQL Server-VM. U kunt dit doen door het bewerken van de beveiligingsgroep van netwerk nadat de VM is gemaakt. Zie voor meer informatie [Wat is een netwerk beveiliging groep (NSG)?](../virtual-network/virtual-networks-nsg.md)

Als u liever niet inschakelen verbindingen met de Database-Engine via internet, kiest u een van de volgende opties:

- **Lokale (in een VM)** toe te staan dat verbindingen met SQL Server alleen vanuit de VM.
- **Privé (binnen het netwerk is Virtual)** verbindingen met SQL Server van machines of services in hetzelfde virtuele netwerk toestaan.

>[AZURE.NOTE] De afbeelding VM voor SQL Server Express edition wordt niet automatisch ingeschakeld voor het TCP/IP-protocol. Dit geldt ook voor de verbinding voor openbare en persoonlijke opties. Voor Express edition, moet u SQL Server Configuration Manager [handmatig](#configure-sql-server-to-listen-on-the-tcp-protocol) inschakelen het TCP/IP-protocol na het maken van de VM.

Beveiliging in het algemeen verbeteren door te kiezen de meeste beperkingen connectiviteit waarmee uw scenario. Maar alle opties zijn beveiligbare tot en met regels voor netwerk-beveiligingsgroep en SQL/Windows-verificatie.

**Poort** standaard 1433. U kunt een ander poortnummer opgeven.
Zie voor meer informatie [verbinding maken naar een SQL Server-VM (resourcemanager) | Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Verificatie
Als u SQL Server-verificatie vereist, klikt u op **inschakelen** onder **SQL-verificatie**.

![SQL Server-verificatie](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Als u van plan bent voor toegang tot SQL Server via internet (dat wil zeggen de optie openbare connectivity), moet u hier SQL-verificatie inschakelen. Openbare toegang tot de SQL-Server is het gebruik van de SQL-verificatie vereist.

Als u SQL Server-verificatie inschakelt, Geef een **aanmeldingsnaam** en **wachtwoord**. De naam van deze gebruiker is geconfigureerd als een SQL Server-verificatie login en lid zijn van de server vaste **systeembeheerder** . Zie [kiezen een verificatiemodus](http://msdn.microsoft.com/library/ms144284.aspx) voor meer informatie over verificatiemodi.

Als u SQL Server-verificatie niet inschakelt, kunt klikt u vervolgens u het lokale beheerdersaccount op de VM verbinding maken met de SQL Server-instantie.

### <a name="storage-configuration"></a>Opslagconfiguratie
Klik op **opslagconfiguratie** als u wilt de opslagvereisten opgeven.

![Configuratie van de SQL-opslag](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Als u standaard opslag selecteert, is deze optie niet beschikbaar. Automatische opslag optimalisatie is alleen beschikbaar voor Premium opslag.

U kunt vereisten opgeven als invoer/uitvoer-bewerkingen per seconde het IO's / (s), doorvoersnelheid in MB/s, en de grootte van de totale opslagcapaciteit. Hiermee configureert u deze waarden met behulp van de schaal beweegbare. Het aantal schijven op basis van deze vereisten is voldaan wordt automatisch berekend door de portal.

Azure wordt standaard de opslag geoptimaliseerd voor 5000 IO's / s, 200 MB en 1 TB opslagruimte. U kunt deze Opslaginstellingen op basis van de werkbelasting kunt wijzigen. Selecteer onder **opslag geoptimaliseerd voor**, een van de volgende opties:

- **Algemeen** is de standaardinstelling en de meeste werkbelasting ondersteunt.
- **Afhandeling** verwerking wordt geoptimaliseerd voor de opslag van traditionele database OLTP werkbelasting voor.
- De opslag van analytische en rapportage werkbelasting voor **gegevensopslag** worden geoptimaliseerd.

>[AZURE.NOTE] De bovenste beperkingen voor de schuifregelaars varieert afhankelijk van de grootte van uw geselecteerde VM.

### <a name="automated-patching"></a>Automatische herstellen
**Automatische patches** is standaard ingeschakeld. Geautomatiseerde patches kunt Azure automatisch patch SQL Server en het besturingssysteem. Geef een dag van de week, tijd en duur van een onderhoudsvenster. Azure kunt uitvoeren in dit onderhoudsvenster herstellen. De planning van het venster Onderhoud de landinstelling VM voor tijd wordt gebruikt. Als u niet dat Azure automatisch patch SQL Server en het besturingssysteem wilt, klikt u op **uitschakelen**.  

![SQL wordt automatisch herstellen](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Zie [Voor SQL Server in Azure virtuele Machines wordt automatisch herstellen](virtual-machines-windows-sql-automated-patching.md)voor meer informatie.

### <a name="automated-backup"></a>Automatische back-up
Schakel automatische database back-ups voor alle databases onder **Automatische back-up**. Automatische back-up is standaard uitgeschakeld.

Wanneer u automatische back-up van SQL inschakelt, kunt u het volgende:

- Bewaarperiode (dagen) back-ups
- Opslag-account om toegang te gebruiken voor back-ups
- Optie codering en het wachtwoord voor back-ups

Als u wilt versleutelen de back-up, klikt u op **inschakelen**. Geef het **wachtwoord**. Azure maakt van een certificaat voor het coderen van de back-ups en gebruikt het opgegeven wachtwoord te beveiligen dat certificaat.

![SQL automatische back-up maken](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Zie [Automatische back-up maken voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-automated-backup.md)voor meer informatie.

### <a name="azure-key-vault-integration"></a>Azure-toets kluis-integratie
**Integratie van Azure belangrijke kluis** om op te slaan beveiliging geheimen in Azure wordt aangegeven voor versleuteling, en klik op **inschakelen**.

![Integratie van SQL Azure belangrijke kluis](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

De volgende tabel bevat de vereiste voor het configureren van Azure-toets kluis integratie parameters.

|PARAMETER|BESCHRIJVING|VOORBEELD|
|----------|----------|-------|
|**Belangrijke kluis URL** |De locatie van de belangrijkste kluis.|https://contosokeyvault.Vault.Azure.NET/ |
|**UPN** |Azure Active Directory-service UPN. Deze naam wordt ook wel de Client-ID.  |fde2b411 33d - 5-4e11-af04eb07b669ccf2|
| **Principal geheim**|Azure Active Directory-service principal geheim. In dit geheim wordt ook de geheim Client genoemd. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =|
|**De naam van de referentie**|**De naam van de referentie**: AKV integratie Hiermee maakt u een referentie in SQL-Server, zodat de VM toegang heeft tot de belangrijkste kluis. Kies een naam voor deze referentie.| mycred1|

Zie [Azure toets kluis integratie configureren voor SQL Server op Azure VMs](virtual-machines-windows-ps-sql-keyvault.md)voor meer informatie.

Wanneer u klaar bent met het configureren van SQL Server-instellingen, klik op **OK**.

### <a name="r-services"></a>R-services
Voor SQL Server-2016 Enterprise edition hebt u de optie voor het inschakelen van [SQL Server-R-Services](https://msdn.microsoft.com/library/mt604845.aspx). Hiermee kunt u het gebruik van geavanceerde analyses met SQL Server-2016. Klik op **inschakelen** op het blad **SQL Server-instellingen** .

![SQL Server R Services inschakelen](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

>[AZURE.NOTE] Voor SQL Server-afbeeldingen die niet 2016 Enterprise edition zijn, is de optie om weeknummers R Services uitgeschakeld.

## <a name="5-review-the-summary"></a>5. Bekijk het overzicht
Bekijk het overzicht in het **Overzicht** blad, en klik op **OK** om te maken van SQL Server, resourcegroep en resources die zijn opgegeven voor dit VM weer te geven.

U kunt de implementatie van de azure portal controleren. De knop **meldingen** boven aan het scherm ziet u eenvoudige status van de implementatie.

>[AZURE.NOTE] Waarmee u een idee van implementatietijden, geïmplementeerd ik een VM SQL in de regio Oost Amerikaans met standaardinstellingen. De implementatie van deze toets heeft een totaal van 26 minuten duren. Maar u kunt ondervinden een sneller of langzamer afspelen implementatietijd op basis van uw regio en instellingen geselecteerd.

## <a name="open-the-vm-with-remote-desktop"></a>Open de VM met extern bureaublad

Gebruik de volgende stappen als u verbinding maakt met de virtuele machine met extern bureaublad:

1. Nadat de VM Azure is gebouwd, wordt het pictogram voor de VM weergegeven op uw Azure dashboard. U kunt dit ook vinden door te bladeren van uw bestaande virtuele machines. Klik op uw nieuwe SQL-VM. Een **virtuele machine** blade weergegeven de details van uw VM.
1. Aan de bovenkant van het blad **VM** , klikt u op **verbinding maken**.
1. Een RDP-bestand in de browser gedownload voor VM. Open het RDP-bestand.
    ![Extern bureaublad voor SQL-VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. De verbinding met extern bureaublad krijgt u dat de uitgever van deze verbinding met extern niet kunnen worden geïdentificeerd. Klik op **verbinding maken** om door te gaan.
1. Klik op **een ander account gebruiken**in het dialoogvenster **Windows-beveiliging** .
1. Voor **gebruikersnaam in te voeren** type ** \<gebruikersnaam >**, waarbij <user name> is de naam van de gebruiker die u hebt opgegeven wanneer u de VM hebt geconfigureerd. U moet een eerste backslash voordat u de naam toevoegen.
1. Typ het **wachtwoord** dat u eerder hebt geconfigureerd voor deze VM en klik vervolgens op **OK** om verbinding te maken.
1. Als u nog een **Verbinding met extern bureaublad** dialoogvenster wordt gevraagd of u wilt koppelen, klikt u op **Ja**.

Nadat u verbinding met de SQL Server virtuele machine maakt, kunt u SQL Server Management Studio starten en verbinding maken met Windows-verificatie met de referenties van uw lokale beheerder. Als u SQL Server-verificatie hebt ingeschakeld, kunt u ook verbinding maken met SQL-verificatie met SQL-aanmelding en hetzelfde wachtwoord dat u tijdens het inrichten hebt geconfigureerd.

Toegang tot de computer, kunt u rechtstreeks machine en op basis van uw vereisten voor SQL Server-instellingen wijzigen. U kunt bijvoorbeeld de firewallinstellingen configureren of SQL Server-configuratie-instellingen wijzigen.

## <a name="connect-to-sql-server-remotely"></a>Verbinding maken met SQL Server op afstand

In deze zelfstudie geselecteerd we **openbare** toegang voor de virtuele machine en **SQL Server-verificatie**. Deze instellingen is automatisch geconfigureerd voor de virtuele machine voor SQL Server-verbindingen toestaan van een client via internet (ervan uitgaande dat ze de juiste SQL-login hebben).

>[AZURE.NOTE] Als u geen openbare tijdens het inrichten hebt geselecteerd, zijn extra stappen vereist voor toegang tot uw SQL Server-instantie via internet. Zie [verbinding maken naar een SQL Server-VM](virtual-machines-windows-sql-connect.md)voor meer informatie.

De volgende secties wordt aangegeven hoe verbinding maken met uw SQL Server-instantie op uw VM vanaf een andere computer via internet.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van SQL Server in Azure wordt aangegeven, [SQL Server op Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md) en de [Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md).

Voor een video-overzicht van SQL Server op Azure virtuele Machines, Bekijk [Azure VM is de beste platform voor SQL Server-2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Verkennen leerpad](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor SQL Server op Azure virtuele machines.
