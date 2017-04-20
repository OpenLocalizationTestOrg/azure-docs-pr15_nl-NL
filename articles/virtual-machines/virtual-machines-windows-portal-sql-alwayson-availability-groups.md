<properties
    pageTitle="Groep altijd op beschikbaarheid configureren voor de Resource Manager in Azure VM automatisch:"
    description="Een altijd op beschikbaarheid-groep maken met Azure virtuele machines in resourcemanager Azure-modus. De gebruikersinterface van deze zelfstudie hoofdzakelijk gebruikt om automatisch te maken de volledige oplossing."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-always-on-availability-group-in-azure-vm-automatically---resource-manager"></a>Groep altijd op beschikbaarheid configureren voor de Resource Manager in Azure VM automatisch:

> [AZURE.SELECTOR]
- [Resourcemanager: sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Resourcemanager: handmatige](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassieke: UI](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassieke: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

Deze zelfstudie end-to-end ziet u hoe u een groep van de beschikbaarheid van SQL Server maken met resourcemanager Azure virtuele machines. De zelfstudie wordt Azure bladen gebruikt voor het configureren van een sjabloon. U wordt de standaardinstellingen te controleren, typ de gewenste instellingen en bijwerken van de bladen in de portal als u deze zelfstudie doorloop.

Aan het einde van de zelfstudie, wordt uw SQL Server-oplossing beschikbaarheid van de groep in Azure bestaan uit de volgende elementen:

- Een virtueel netwerk met meerdere subnetten, inclusief een front-end en een back-enddatabase subnet

- Twee domeincontroller met een domein AD (Active Directory)

- Twee SQL Server VMs geïmplementeerd in het back-enddatabase subnet en toegevoegd aan het AD-domein

- Een 3-knooppunt WSFC cluster met het knooppunt meeste quorum-model

- Een groep met twee synchroon doorvoeren replica's van de beschikbaarheid van een database beschikbaarheid

De onderstaande afbeelding is een grafische weergave van de oplossing.

![Test testomgeving architectuur voor AG in Azure wordt aangegeven](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Alle bronnen in deze oplossing deel uitmaakt van een één resourcegroep.

Deze zelfstudie wordt ervan uitgegaan:

- U hebt al een Azure-account. Als u niet, [registreren voor een proefaccount hebt](http://azure.microsoft.com/pricing/free-trial/).

- U weet hoe voor het inrichten van een SQL Server-VM vanuit de galerie VM met behulp van de grafische gebruikersinterface. Zie voor meer informatie, [inrichting van SQL Server op Azure virtuele machines](virtual-machines-windows-portal-sql-server-provision.md)

- U hebt al een effen begrip van de van de beschikbaarheidsgroepen. Zie [altijd op beschikbaarheid-groepen (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx)voor meer informatie.

>[AZURE.NOTE] Als u geïnteresseerd beschikbaarheidsgroepen gebruiken met SharePoint bent, raadpleegt u ook [configureren SQL Server 2012 altijd op beschikbaarheidsgroepen voor SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).

In deze zelfstudie gebruikt u de Azure-portal naar:

- Selecteer de de sjabloon altijd op in de portal

- De sjablooninstellingen bekijken en bijwerken van een paar configuratie-instellingen voor uw omgeving

- Monitor Azure zoals deze wordt gemaakt van de gehele omgeving

- Verbinding maken met een van de domein en klik vervolgens op een van de SQL-Servers

[AZURE.INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]


## <a name="provision-the-cluster-from-the-gallery"></a>Het cluster in de galerie inrichten

Azure vindt u een afbeelding voor de volledige oplossing. Om te kunnen Zoek de sjabloon:

1.  Meld u aan bij de Azure-portal met uw account.
1.  Bij het Azure portal klikken **+ Nieuw.** De portal wordt het nieuwe blad geopend.
1.  Klik op het nieuwe blade zoeken naar **AlwaysOn**.
![AlwaysOn sjabloon zoeken](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
1.  Zoek de **SQL Server AlwaysOn Cluster**in de lijst met zoekresultaten.
![AlwaysOn-sjabloon](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
1.  Kies op het **selecteren van een implementatiemodel** **Resourcemanager**.

### <a name="basics"></a>Basisbeginselen

Klik op de **Basisbeginselen** en configureer het volgende:

- **Gebruikersnaam beheerder** is een gebruikersaccount met beheerdersmachtigingen en een lid van de rol van SQL Server systeembeheerder vaste server op beide exemplaren van SQL Server. Voor deze zelfstudie gebruikt u **DomainAdmin**.

- **Wachtwoord** is het wachtwoord voor het domein-beheerdersaccount. Gebruik een complexe wachtwoord. Bevestig het wachtwoord.

- **Abonnement** is het abonnement waaraan Azure wordt factureren aan het uitvoeren van alle bronnen voor de van de beschikbaarheidsgroep wordt geïmplementeerd. Als uw account heeft in meerdere abonnementen, kunt u een ander abonnement opgeven.

- **Resourcegroep** is de naam voor de groep die alle Azure resources die zijn gemaakt door deze zelfstudie moet behoren. Voor deze zelfstudie gebruikt u **SQL-HA-RG**. Zie voor meer informatie (Azure resourcemanager overzicht) [resource-groep-overview.md / #-resourcegroepen].

- **Locatie** is de Azure regio waar de bronnen voor deze zelfstudie wordt gemaakt. Selecteer een Azure gebied voor het hosten van de infrastructuur.

Hieronder ziet u hoe het blad **Basisbeginselen** eruitziet:

![Basisbeginselen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

- Klik op **OK**.

### <a name="domain-and-network-settings"></a>Domein en netwerkinstellingen

Deze sjabloon voor een galerie met Azure maakt een nieuw domein met nieuwe domeincontrollers. Ook wordt een nieuw netwerk en twee subnetten gemaakt. De sjabloon wordt niet ingeschakeld voor het maken van de servers in een bestaand domein of een virtueel netwerk. De volgende stap is het domein en netwerk-instellingen configureren.

Controleer de vooraf ingestelde waarden voor het domein en netwerk-instellingen op **domein en netwerkinstellingen** blade:

- **Bos hoofdsite domeinnaam** is de domeinnaam die wordt gebruikt voor het AD-domein dat wordt gehost in het cluster. Gebruik voor de zelfstudie **contoso.com**.

- **Virtuele netwerknaam** is de naam van het Azure virtuele netwerk. Voor deze zelfstudie gebruikt u **autohaVNET**.

- **De naam van de domeincontroller-subnet** is de naam van een gedeelte van het virtuele netwerk waarop de domeincontroller. Voor deze zelfstudie **subnet-1**te gebruiken. Dit subnet wordt gebruikt in adres voorvoegsel **10.0.0.0/24**.

- **De naam van de SQL Server-subnet** is de naam van een gedeelte van het virtuele netwerk dat hosts de SQL-Servers en het bestand getuige delen. Voor deze zelfstudie gebruikt u **subnet-2**. Dit subnet wordt gebruikt in adres voorvoegsel **10.0.1.0/26**.

Meer informatie over virtuele netwerken in [Azure virtuele netwerk overzicht](../virtual-network/virtual-networks-overview.md).  

De **Instellingen voor het domein en netwerk** ziet er als volgt:

![Domein en netwerkinstellingen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Zo nodig kunt u deze waarden kunt wijzigen. We gebruiken de vooraf gedefinieerde waarden voor deze zelfstudie.

- Controleer de instellingen en klik op **OK**.

###<a name="availability-group-settings"></a>beschikbaarheid van de groepsinstellingen

Controleer op **Groepsinstellingen beschikbaarheid van** de vooraf ingestelde waarden voor de beschikbaarheidsgroep en de luisteraar ervan af.

- **De naam van de groep beschikbaarheid** is de naam van de gegroepeerde resource voor de van de beschikbaarheidsgroep. Voor deze zelfstudie gebruikt u de **Contoso-ag**.

- **de naam van de luisteraar ervan af groep beschikbaarheid** wordt gebruikt door het cluster en de interne taakverdeling. Clients verbinding maken met SQL Server kunnen u deze naam gebruiken verbinding maken met de juiste replica van de database. Voor deze zelfstudie gebruikt u de **Contoso-luisteraar ervan af**.

-  **beschikbaarheid van de groep luisteraar ervan af poort** Hiermee geeft u dat de TCP-poort de SQL Server luisteraar ervan af wordt gebruikt. Voor deze zelfstudie gebruikt u de standaardpoort, **1433**.

Zo nodig kunt u deze waarden kunt wijzigen. Voor deze zelfstudie gebruikt u de vooraf ingestelde waarden.  

![beschikbaarheid van de groepsinstellingen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

- Klik op **OK**.

###<a name="vm-size-storage-settings"></a>VM grootte, opslaginstellingen

Klik op **VM grootte, Opslaginstellingen** kiest u een SQL Server VM grootte en de andere instellingen te controleren.

- **SQL Server VM grootte** is de grootte van de Azure virtuele machines voor zowel SQL Server. Kies een grootte VM geschikt te maken voor uw werkzaamheden. Als u samenstelt gebruikt deze omgeving voor de zelfstudie **DS2**. Voor productie werkbelasting kiest u de grootte van een virtuele machine die ondersteuning voor de werklast bieden. Veel productie werkbelasting moeten worden **DS4** of groter. De sjabloon wordt twee virtuele machines van deze grootte bouwen en SQL Server installeren op elkaar. Zie [grootten voor virtuele machines](virtual-machines-linux-sizes.md)voor meer informatie.

>[AZURE.NOTE]Azure installeert Enterprise Edition van SQL Server. De kosten, is afhankelijk van de versie en de grootte van de virtuele machine. Zie [virtuele machines prijzen](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql)voor gedetailleerde informatie over de huidige kosten.

- **VM hierbij** is de grootte van de virtuele machine voor het domein besturing voor. Voor deze zelfstudie gebruikt u **D2**.

- **Bestand delen getuige VM grootte** is de grootte van de virtuele machine voor de bestandssharewitness. Voor deze zelfstudie gebruikt u **A1**.

- **Opslag van de SQL-account** is de naam van het account opslag de SQL Server-gegevens en besturingssysteem schijven. Voor deze zelfstudie gebruikt u **alwaysonsql01**.

- **Opslag van de domeincontroller account** is de naam van het account opslag voor de besturing voor domein. Voor deze zelfstudie gebruikt u **alwaysondc01**.

- **SQL Server-gegevens schijf grootte** TB is de grootte van de SQL Server-gegevens-schijf in TB. Geef een getal van 1 tot en met 4. Dit is de grootte van de gegevensschijf die wordt gekoppeld aan elke SQL-Server. Gebruik voor deze zelfstudie **1**.

- **Opslag optimalisatie** Hiermee stelt u specifieke opslag configuratie-instellingen voor de SQL Server virtuele machines op basis van het type werkbelasting. Alle SQL Server in dit scenario gebruiken premium opslagruimte met Azure schijf host cache is ingesteld op alleen-lezen. Bovendien kunt u SQL Server-instellingen voor de werklast optimaliseren door een van deze drie instellingen te kiezen:

    - **Algemene werkbelasting** Hiermee stelt u geen specifieke configuratie-instellingen

    - **Verwerking van afhandeling** stelt doelcellen vlag 1117 en 1118

    - **Gegevensopslag** stelt doelcellen vlag 1117 en 610

Voor deze zelfstudie gebruikt u **algemene werkbelasting**.

![VM grootte opslaginstellingen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

- Controleer de instellingen en klik op **OK**.

####<a name="a-note-about-storage"></a>Een opmerking over de opslag

Aanvullende optimalisaties toe, hangt af van de grootte van de SQL Server-gegevensschijven. Azure toegevoegd voor elke terabyte van gegevensschijf, een extra opslagruimte van 1 TB premium (SSD). Wanneer een server 2 TB of meer vereist, wordt een resourcegroep die opslag in de sjabloon gemaakt op elke SQL-Server. Een opslaggroep is een formulier van opslag virtualization waar meerdere schijven zijn geconfigureerd hoger capaciteit, tolerantie en prestaties op te geven.  De sjabloon wordt gemaakt van een opslagruimte op de opslag van toepassingen vervolgens en dit als een eenmalige gegevens aan het besturingssysteem presenteert. De sjabloon wordt deze schijf als de gegevensschijf voor SQL Server. De sjabloon verbetert de de opslag van toepassingen prestatie voor SQL Server met de volgende instellingen:

- Streepgrootte is de instelling interleave voor de virtuele schijf. Voor transacties werkbelasting is dit ingesteld op 64 KB. De instelling is voor gegevensopslag werkbelasting 256 KB.

- Tolerantie is eenvoudig (geen tolerantie).

>[AZURE.NOTE] Azure premium opslag is lokaal overbodig en drie kopieën van de gegevens in één gebied, blijft zodat aanvullende tolerantie bij de opslag van toepassingen niet vereist is.

- Aantal kolommen is gelijk aan het aantal schijven in de opslaggroep.

Zie voor meer informatie over opslag spaties en opslag van toepassingen:

- [Opslag spaties-overzicht](http://technet.microsoft.com/library/hh831739.aspx).

- [Windows Server back-up- en opslag van toepassingen](http://technet.microsoft.com/library/dn390929.aspx)

Zie voor meer informatie over aanbevolen werkwijzen voor SQL Server configuration [prestaties aanbevolen procedures voor SQL Server in Azure virtuele machines](virtual-machines-windows-sql-performance.md)


###<a name="sql-server-settings"></a>SQL Server-instellingen

Controleer op **de instellingen van SQL Server** en voorvoegsel van de SQL Server VM, SQL Server-versie, SQL Server-service-account en wachtwoord en het onderhoudsplanning patches SQL-automatisch wijzigen.

- **SQL Server voorvoegsel** wordt gebruikt om een naam voor elke SQL-Server te maken. Voor deze zelfstudie gebruikt u de **Contoso-ag**. De SQL Server-namen worden *Contoso-ag-0* en *Contoso-ag-1*.

- **SQL Server-versie** is de versie van SQL Server. Voor deze zelfstudie gebruikt u **SQL Server-2014**. U kunt ook **SQL Server 2012** of **SQL Server-2016**.

- **SQL Server-service account-gebruikersnaam** is de naam van het domein-account voor de SQL Server-service. Voor deze zelfstudie gebruikt u **sqlservice**.

- **Wachtwoord** is het wachtwoord voor het account van SQL Server-service.  Gebruik een complexe wachtwoord. Bevestig het wachtwoord.

- **SQL-automatisch patches onderhoudsplanning** geeft aan wat het weekdag Azure wordt automatisch patch voor de SQL-Servers. Typ voor deze zelfstudie **zondag**.

- **SQL automatisch patches onderhoud starten uur** is de tijd voor de regio Azure wanneer automatische patches wordt gestart.

>[AZURE.NOTE]Het venster patch voor elke VM is het diamodel één uur. Slechts één virtuele machine gerepareerd tegelijk om te voorkomen van onderbrekingen services.

![SQL Server-instellingen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Controleer de instellingen en klik op **OK**.

###<a name="summary"></a>Overzicht

Azure is gevalideerd met de instellingen op de pagina Accountoverzicht. U kunt ook de sjabloon downloaden. Bekijk het overzicht. Klik op **OK**.

###<a name="buy"></a>Kopen

Deze definitief blade bevat de **Gebruiksvoorwaarden**en **privacybeleid**. Deze gegevens bekijken. Wanneer u klaar voor Azure om te beginnen met het maken van de virtuele machines en alle andere vereiste bronnen voor de van de beschikbaarheidsgroep bent, klikt u op **maken**.

De portal van Azure maakt de resourcegroep en alle bronnen.

##<a name="monitor-deployment"></a>Monitor-implementatie

De voortgang van de implementatie van de Azure portal volgen. Een pictogram voor de implementatie wordt automatisch vastgemaakt aan de Azure portal dashboard.

![Azure Dashboard](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

##<a name="connect-to-sql-server"></a>Verbinding maken met SQL Server

De nieuwe exemplaren van SQL Server worden uitgevoerd op virtuele machines die geen verbindingen met internet hebt. De domeincontrollers hebben echter de verbinding van een internetverbinding. De verbinding met de SQL-servers met extern bureaublad, eerste RDP naar een van de domein. Open vanuit de domeincontroller een tweede RDP met de SQL Server.

Naar RDP aan de primaire domeincontroller, als volgt te werk:

1.  Vanuit het Azure helemaal dat de implementatie is geslaagd portal dashboard.

1.  Klik op **Resources**.

1.  Klik in het blad **Resources** op **ad-primair-domeincontroller** de naam van de computer van de virtuele machine voor de primaire domeincontroller.

1.  Klik op het blad voor **ad-primair-domeincontroller** op **verbinding maken**. Uw browser wordt gevraagd of u wilt openen of opslaan van de verbinding met extern object. Klik op **openen**.
![Verbinding maken met domeincontroller](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/13-ad-primary-dc-connect.png)
1.  **Verbinding met extern bureaublad** mogelijk u waarschuwen dat de uitgever van deze verbinding met extern niet kunnen worden geïdentificeerd. Klik op **verbinding maken**.

1.  Windows-beveiliging vraagt u uw referenties verbinding maken met het IP-adres van de primaire domeincontroller moet invoeren. Klik op **een ander account gebruiken**. Typ **contoso\DomainAdmin**voor **gebruikersnaam in te voeren** . Dit is het account dat u hebt gekozen voor gebruikersnaam beheerder. Gebruik het complexe wachtwoord die u hebt gekozen wanneer u de sjabloon hebt geconfigureerd.

1.  **Extern bureaublad** mogelijk u waarschuwen dat de externe computer kan niet worden geverifieerd veroorzaakt door problemen met het beveiligingscertificaat. Deze ziet u de naam van de beveiliging. Als u de zelfstudie gevolgd zijn de naam van de **ad-primair-dc.contoso.com**. Klik op **Ja**.

U hebt nu een verbinding maken met de primaire domeincontroller. Naar RDP met de SQL Server, als volgt te werk:

1.  Open op de domeincontroller, **Verbinding met extern bureaublad**.

1.  Voor de **Computer**, typt u de naam van een van de SQL-Servers. Voor deze zelfstudie, typt u **SQL Server-0**.

1.  Gebruik de dezelfde gebruikersaccount en het wachtwoord dat u voor RDP naar de domeincontroller gebruikt.

U hebt nu een verbinding maken met RDP met de SQL Server. U kunt openen van SQL Server management studio, verbinding maken met het standaardexemplaar van SQL Server en controleer of dat de groep availabilty is geconfigureerd.


