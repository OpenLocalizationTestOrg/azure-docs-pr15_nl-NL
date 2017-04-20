<properties
    pageTitle="SQL Server Business Intelligence | Microsoft Azure"
    description="In dit onderwerp resources die zijn gemaakt met het implementatiemodel klassieke gebruikt en wordt beschreven hoe de functies voor Business Intelligence (BI) die beschikbaar zijn voor SQL Server wordt uitgevoerd op Microsoft Azure virtuele Machines (VMs)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>SQL Server Business Intelligence in Azure virtuele Machines

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

De galerie met Microsoft Azure Virtual Machine bevat afbeeldingen met SQL Server-installaties. De edities van SQL Server worden ondersteund in de galerijafbeeldingen zijn de dezelfde installatiebestanden die u naar de on-premises implementatie computers en virtuele machines kunt installeren. In dit onderwerp bevat een overzicht van de SQL Server Business Intelligence (BI)-functies die zijn geïnstalleerd op de afbeeldingen en configuratiestappen vereist nadat een virtuele machine is ingericht. In dit onderwerp worden ook de van de ondersteunde implementatietopologieën voor BI-functies en aanbevolen procedures.

## <a name="license-considerations"></a>Aandachtspunten voor de licentie

Er zijn twee manieren om te licentie SQL Server in Microsoft Azure virtuele Machines:

1. Licentie mobiliteit voordelen die deel uitmaken van Software Assurance. Zie [Licentie mobiliteit via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/)voor meer informatie.

1. Betalen per uurtarief van Azure virtuele Machines met SQL Server is geïnstalleerd. Zie het gedeelte "SQL Server" in de [Virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Zie voor meer informatie over licenties en huidige tarieven, [Virtuele Machines Licensing Veelgestelde vragen](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server afbeeldingen beschikbaar zijn in de galerie met Azure virtuele Machine

De galerie met Microsoft Azure Virtual Machine bevat een aantal afbeeldingen met Microsoft SQL Server. De software op de afbeeldingen VM afhankelijk van de versie van het besturingssysteem en de versie van SQL Server. De lijst met afbeeldingen die beschikbaar zijn in de galerie met Azure virtuele machines vaak verandert.

![Afbeelding van de SQL in de galerie met azure VM](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Het volgende PowerShell-script resulteert in een lijst van Azure afbeeldingen die "SQL-Server" in de ImageName bevatten:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "settings" menu in Azure classic portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Zie de volgende onderwerpen voor meer informatie over edities en functies die worden ondersteund door SQL Server:

- [Edities van SQL Server](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)

- [Functies die worden ondersteund door de edities van SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>BI-functies op de galerie met virtuele Machine afbeeldingen van SQL Server is geïnstalleerd

De volgende tabel bevat een overzicht van de Business Intelligence-functies die is geïnstalleerd op de algemene Microsoft Azure Virtual Machine galerijafbeeldingen voor SQL Server"

- SQL Server 2016 RC3

- SQL Server 2014 SP1 Enterprise

- SQL Server 2014 SP1 Standard

- SQL Server 2012 SP2 Enterprise

- SQL Server 2012 SP2 Standard

|SQL Server BI-functie|Geïnstalleerd op de afbeelding|Notities|
|---|---|---|
|**Reporting Services Native modus**|Ja|Geïnstalleerd maar moet worden geconfigureerd, inclusief de URL van de manager rapport. Zie het gedeelte [Reporting Services configureren](#configure-reporting-services).|
|**SharePoint-modus van Reporting Services**|Nee|De galerijafbeelding met Microsoft Azure Virtual Machine is niet inbegrepen bij SharePoint- of SharePoint installatiebestanden. <sup>1</sup>|
|**Analysis Services multidimensionale en Data mining OLAP-kubus)**|Ja|Geïnstalleerd en geconfigureerd als standaardexemplaar Analysis Services|
|**Analyseservices in tabelvorm**|Nee|Ondersteund in SQL Server 2012, wordt 2014 en 2016 afbeeldingen, maar deze niet standaard geïnstalleerd. Installeer een ander exemplaar van Analysis Services. Zie de sectie andere SQL Server-Services en functies in dit onderwerp installeren.|
|**Analysis Services PowerPivot voor SharePoint**|Nee|De galerijafbeelding met Microsoft Azure Virtual Machine is niet inbegrepen bij SharePoint- of SharePoint installatiebestanden. <sup>1</sup>|

<sup>1</sup> voor meer informatie over SharePoint en Azure virtuele machines, raadpleegt u [Microsoft Azure architecturen voor SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) en [SharePoint-implementatie op Microsoft Azure virtuele Machines](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Voer de volgende PowerShell-opdracht om een lijst met geïnstalleerde services met 'SQL' in de servicenaam van de.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Algemene aanbevelingen en aanbevolen procedures

- De aanbevolen minimumgrootte voor een virtuele machine is **A3** wanneer u SQL Server Enterprise Edition gebruikt. De grootte van de VM **A4** geschikt is voor SQL Server BI-implementaties van Analysis Services en Reporting Services.

    Zie voor informatie over de huidige VM-grootte, [VM grootten voor Azure](virtual-machines-linux-sizes.md).

- Een goede gewoonte om Schijfopruiming te beheren is voor de opslag van gegevens, het logboek en back-upbestanden op stations dan **C**: en **D**:. Bijvoorbeeld maken gegevensschijven **E**: en **F**:.

    - Het station caching van beleid voor de standaard-station **C**: is niet optimaal voor het werken met gegevens.

    - De **D**: station is een tijdelijke station dat hoofdzakelijk voor het paginabestand wordt gebruikt. De **D**: station blijft niet behouden en wordt niet opgeslagen in blobopslag. Beheertaken, zoals een wijziging in de virtuele machine grootte opnieuw instellen van de **D**: station. Het wordt aanbevolen **geen** gebruik van de **D**: station voor databasebestanden, inclusief tempdb.

    Zie voor meer informatie over het maken en toevoegen van schijven, [het toevoegen van een schijf gegevens aan een virtuele Machine](virtual-machines-windows-classic-attach-disk.md).

- Stoppen of verwijderen van de services die u niet wilt gebruiken. Voor voorbeeld als u de virtuele machine wordt alleen gebruikt voor Reporting Services, stoppen of verwijderen van Analysis Services en SQL Server Integration Services. De volgende afbeelding is een voorbeeld van de services die standaard worden gestart.

    ![SQL Server-services](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)

    >[AZURE.NOTE] De SQL Server-database-engine is vereist in de ondersteunde BI-scenario's. In één server VM topologie, de database-engine moet worden uitgevoerd op de dezelfde VM.

    Voor meer informatie raadpleegt u de volgende handelingen uit: [Reporting Services verwijderen](https://msdn.microsoft.com/library/hh479745.aspx) en [verwijderen van een exemplaar van Analysis Services](https://msdn.microsoft.com/library/ms143687.aspx).

- **Windows Update** controleren op nieuwe 'belangrijke updates'. De afbeeldingen van Microsoft Azure Virtual Machine worden vaak vernieuwd; echter belangrijke updates kunnen worden beschikbaar via **Windows Update** nadat de afbeelding VM voor het laatst is bijgewerkt.

## <a name="example-deployment-topologies"></a>Voorbeeld implementatietopologieën

Hier volgen voorbeeld-implementaties met Microsoft Azure virtuele Machines. De topologieën in deze diagrammen zijn slechts enkele van de mogelijke topologieën die u met SQL Server BI-functies en Microsoft Azure virtuele Machines gebruiken kunt.

### <a name="single-virtual-machine"></a>Eén VM

Analysis Services, Reporting Services, SQL Server-Database Engine, en gegevensbronnen op een enkele virtuele machine.

![BI Standards scenario met 1 VM](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Twee virtuele Machines

- Analyseservices, Reporting Services en de SQL Server-Database Engine op een enkele virtuele machine. Deze installatie bevat de server-databases van het rapport.

- Gegevensbronnen op een tweede VM. De tweede VM bevat SQL Server-Database Engine als gegevensbron.

![BI iaas scenario met 2 virtuele machines](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Gemengde Azure-gegevens op Azure SQL-database

- Analyseservices, Reporting Services en de SQL Server-Database Engine op een enkele virtuele machine. Deze installatie bevat de server-databases van het rapport.

- Gegevensbron is Azure SQL-database.

![BI iaas scenario's vm en AzureSQL als gegevensbron](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hybride – lokale gegevens

- In dit voorbeeld-implementatie Analysis Services, Reporting Services en de SQL Server-Database Engine worden uitgevoerd op een enkele virtuele machine. De virtuele machine host de server-databases van het rapport. De virtuele machine is gekoppeld aan een on-premises domein via Azure virtuele netwerkproblemen of enkele andere VPN tunneling oplossing.

- Gegevensbron is on-premises implementatie.

![BI iaas scenario's vm en klik op de lokale gegevensbronnen](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Configuratie van Reporting Services-Native modus

De afbeelding van de galerie VM voor SQL Server bevat Reporting Services systeemeigen modus hebt geïnstalleerd, maar de rapportserver niet is geconfigureerd. De stappen in deze sectie configureren de server Reporting Services-rapport. Zie voor meer informatie over het configureren van Reporting Services Native modus [Installeren Reporting Services systeemeigen modus rapport Server (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

>[AZURE.NOTE] Zie voor soortgelijke inhoud met Windows PowerShell-scripts voor het configureren van de rapportserver, [PowerShell gebruiken om u te maken van een Azure VM met een systeemeigen modus rapportserver](virtual-machines-windows-classic-ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Verbinding maken met de virtuele Machine en de Reporting Services Configuration Manager starten

Er zijn twee veelvoorkomende werkstromen om verbinding te maken naar een Azure virtuele machines:

- Verbinding maken in het op de naam van de virtuele machine en klik vervolgens op **verbinding maken**. Een verbinding met extern bureaublad wordt geopend en de naam van de computer automatisch wordt gevuld.

    ![verbinding maken met azure virtuele machines](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)

- Verbinding maken met de virtuele machine Windows verbinding met extern bureaublad. In de gebruikersinterface van het externe bureaublad:

    1. Typ de **naam van de cloud-service** als de naam van de computer.

    1. Typ dubbele punt (:) en de openbare poortnummer in dat is geconfigureerd voor het eindpunt TCP externe bureaublad.

        Myservice.cloudapp.NET:63133

        Zie voor meer informatie [Wat is een cloudservice?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).

**Begin Reporting Services Configuration Manager.**

1. In **Windows Server 2012**:

1. Typ vanuit **het startscherm van** **Reporting Services** om een lijst met Apps.

1. Met de rechtermuisknop op **Reporting Services Configuration Manager** en klik op **als Administrator uitvoeren**.

1. In **Windows Server 2008 R2**:

1. Klik op **Start**en klik vervolgens op **Alle programma's**.

1. Klik op **Microsoft SQL Server 2016**.

1. Klik op **configuratiehulpprogramma**.

1. Met de rechtermuisknop op **Reporting Services Configuration Manager** en klik op **als Administrator uitvoeren**.

Of

1. Klik op **Start**.

1. Typ **reporting services**in het dialoogvenster **Zoeken in programma's en bestanden** . Als de VM wordt uitgevoerd van Windows Server 2012, typt u de **reporting services** op het scherm Start van Windows Server 2012.

1. Met de rechtermuisknop op **Reporting Services Configuration Manager** en klik op **als Administrator uitvoeren**.

    ![zoeken naar ssrs configuratiemanager](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Reporting Services configureren

**Service-account en webservice-URL:**

1. Controleer of dat de **Servernaam** is de naam van de lokale server en klik op **verbinding maken**.

1. Noteer de lege **Rapportnaam Server-Database**. De database wordt gemaakt wanneer de configuratie is voltooid.

1. Controleer of dat de **Status van de Server rapport** is **gestart**. Als u wilt controleren of de service in Windows Server Manager, is de service de **SQL Server Reporting Services** -Windows-Service.

1. Klik op **Service-Account** en het account wijzigen dat naar wens. Als de virtuele machine wordt gebruikt in een niet-gekoppelde domeinomgeving, is de ingebouwde **ReportServer** account voldoende. Zie voor meer informatie over het serviceaccount, [Service-Account](https://msdn.microsoft.com/library/ms189964.aspx).

1. Klik op **Service-Web-URL** in het linkerdeelvenster.

1. Klik op **toepassen** om te configureren van de standaardwaarden.

1. Houd rekening met het **rapport Server webservice-URL's**. Opmerking de standaardpoort naar TCP 80 en maakt deel uit van de URL. In een latere stap maakt u een Microsoft Azure Virtual Machine-eindpunt voor de poort.

1. Controleer of de acties die is voltooid in het deelvenster met **resultaten** .

**Databasefunctie:**

1. Klik op **Database** in het linkerdeelvenster.

1. Klik op **Database wijzigen**.

1. Controleer of **dat een nieuw rapport server-database maken** is geselecteerd en klik op volgende.

1. Controleer of **De naam van de Server** en klik op **Verbinding testen**.

1. Als het resultaat **verbindingstest is geslaagd is**, klikt u op **OK** en klik vervolgens op **volgende**.

1. Houd rekening met de naam van de database is **ReportServer** en de **rapportserver modus** is **systeemeigen** Klik op **volgende**.

1. Klik op **volgende** op de pagina **referenties** .

1. Klik op **volgende** op de pagina **Summary** .

1. Klik op **volgende** op de pagina **voortgang en voltooien** .

**Web Portal URL of Report Manager-URL voor 2012 en 2014:**

1. Klik op **Web Portal URL**of **Report Manager-URL** voor 2014 en 2012, in het linkerdeelvenster.

1. Klik op **toepassen**.

1. Controleer of de acties die is voltooid in het deelvenster met **resultaten** .

1. Klik op **Afsluiten**.

Zie voor informatie over het rapport servermachtigingen [Toekennen van machtigingen voor een systeemeigen modus rapportserver](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Blader naar de lokale Report Manager

Als u wilt controleren of de configuratie, blader naar Rapportbeheer op de VM.

1. Klik op de VM, start u Internet Explorer met beheerdersbevoegdheden.

1. Blader naar http://localhost/reports op de VM.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Klik op verbinding maken met externe webportal of Rapportbeheer voor 2014 en 2012

Als u wilt verbinding maken met de webportal of Rapportbeheer voor 2014 en 2012, op de virtuele machine vanaf een externe computer, maakt u een nieuwe virtuele machine TCP-eindpunt. De rapportserver luistert standaard voor HTTP-aanvragen op **poort 80**. Als u de rapport-URL's om te gebruiken van een andere poort configureren, moet u dat poortnummer in de volgende instructies.

1. Een eindpunt maken voor de virtuele Machine van TCP-poort 80. Zie voor meer informatie de sectie [VM eindpunten en firewallpoorten](#virtual-machine-endpoints-and-firewall-ports) in dit document.

1. Open poort 80 in de virtuele machine firewall.

1. Blader naar de webportal of Rapportbeheer, Azure virtuele machines **DNS-naam** gebruiken als de naam van de server in de URL. Bijvoorbeeld:

    **Rapportserver**: http://uebi.cloudapp.net/reportserver  **webportal**: http://uebi.cloudapp.net/reports

    [Een Firewall configureren voor toegang tot de Server van rapporten](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Maken en publiceren van rapporten op de Azure virtuele Machine

De volgende tabel worden enkele van de opties die beschikbaar zijn voor het publiceren van bestaande rapporten vanaf een lokale computer op de rapportserver die worden gehost op Microsoft Azure Virtual Machine:

- **Report Builder**: de virtuele machine bevat de Klik-eenmaal versie van Microsoft SQL Server Report Builder voor SQL-2014 en 2012. Rapport opbouwfunctie voor de eerste keer start op de virtuele machine met SQL-2016:

    1. Start uw browser met beheerdersbevoegdheden.

    1. Blader naar de webportal op de virtuele machine, en selecteer het pictogram **downloaden** in de rechterbovenhoek.
    
    1. Selecteer **Report Builder**.

    Zie voor meer informatie, [Report Builder starten](https://msdn.microsoft.com/library/ms159221.aspx).

- **SQL Server Data Tools**: VM: SQL Server Data Tools is geïnstalleerd op de virtuele machine en kunnen worden gebruikt om het **Rapport serverprojecten** en rapporten maken op de virtuele machine. SQL Server Data Tools kunt u de rapporten publiceren naar de rapportserver op de virtuele machine.

- **SQL Server Data Tools: externe**: op uw lokale computer, kunt u een Reporting Services-project maakt in SQL Server Data Tools die Reporting Services-rapporten bevat. Het project verbinding maken met de URL van de web-service configureren.

    ![de Projecteigenschappen SSDT voor SQL Server Reporting Services-project](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)

- Maak een. Harde schijf VHD die rapporten bevat en vervolgens uploaden en het station wilt toevoegen.

    1. Maak een. VHD harde schijf op uw lokale computer die uw rapporten bevat.

    1. Maak en een management-certificaat installeren.

    1. Het VHD-bestand uploaden naar Azure met de cmdlet toevoegen-AzureVHD [maken en uploaden een Windows Server VHD naar Azure](virtual-machines-windows-classic-createupload-vhd.md).

    1. De schijf toevoegen aan de virtuele machine.

## <a name="install-other-sql-server-services-and-features"></a>Installeer andere SQL Server-Services en functies

Als wilt installeren extra SQL Server-services, zoals Analysis Services in tabelvorm modus, voert u de installatiewizard van SQL server. De installatiebestanden zijn op de VM lokale schijf.

1. Klik op **Start** en klik vervolgens op **Alle programma's**.

1. Klik op **Microsoft SQL Server-2016**, **Microsoft SQL Server-2014** of **Microsoft SQL Server 2012** en klik op **Configuratiehulpprogramma**.

1. Klik op **SQL Server-installatie Center**.

Of C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe of C:\SQLServer_11.0_full\setup.exe uitvoeren

>[AZURE.NOTE] De eerste keer dat u de installatie van SQL Server uitvoert meer setup-bestanden kunnen worden gedownload en vereisen opnieuw opstarten van de virtuele machine en een opnieuw starten van de installatie van SQL Server.
>
>Als u meerdere keren pas de afbeelding uit Microsoft Azure Virtual Machine geselecteerd moet, kunt u uw eigen SQL Server-afbeelding maken. Analysis Services-SysPrep functionaliteit is met SQL Server 2012 SP1 CU2 ingeschakeld. Zie [Aandachtspunten voor de installatie van SQL Server werken met SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) en [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)voor meer informatie.

### <a name="to-install-analysis-services-tabular-mode"></a>Analysis Services in tabelvorm modus installeren

De stappen in deze sectie **samenvatten** de installatie van Analysis Services in tabelvorm modus. Zie de volgende onderwerpen voor meer informatie:

- [Analyseservices in tabelvorm modus installeren](https://msdn.microsoft.com/library/hh231722.aspx)

- [Tabellaire modellen (Adventure Works-zelfstudie)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Analysis Services in tabelvorm modus installeren:**

1. In de installatiewizard van SQL Server, klik op **installatie** in het linkerdeelvenster en klik vervolgens op **zelfstandige installatie van nieuwe SQL server of functies toevoegen aan een bestaande installatie**.

    - Als u de **Map**wordt weergegeven, blader naar c:\SQLServer_13.0_full, c:\SQLServer_12.0_full of c:\SQLServer_11.0_full en klik vervolgens op **Ok**.

1. Klik op **volgende** op de pagina product updates.

1. Klik op de pagina **Installatietype** Voer **een nieuwe installatie van SQL Server** en klik op **volgende**.

1. Klik op de pagina **Functie van de instelling** op **Installatie van SQL Server-functies**.

1. Klik op de pagina **Onderdelen selecteren** op **Analysis Services**.

1. Klik op de pagina **Configuratie exemplaar** Typ een beschrijvende naam, zoals **tabelvorm** in tekstvakken **Met de naam exemplaar** en **Exemplaar-Id** .

1. Selecteer op de pagina **Configuratie van Analysis Services** **In tabelvorm modus**. De huidige gebruiker toevoegen aan de lijst met beheerdersmachtigingen.

1. Voltooien en de installatiewizard van SQL Server te sluiten.

## <a name="analysis-services-configuration"></a>Configuratie van Analysis Services

### <a name="remote-access-to-analysis-services-server"></a>Externe toegang tot Analysis Services-Server

Analysis Services-server biedt alleen ondersteuning voor windows-verificatie. Toegang tot Analysis Services op afstand van clienttoepassingen zoals SQL Server Management Studio of SQL Server Data Tools, moet de virtuele machine aan uw lokale domein, met behulp van Azure virtuele toegang worden toegevoegd. Zie voor meer informatie, [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

Een **standaardexemplaar** van Analysis Services luistert op TCP-poorten **2383**. Open de poort in de virtuele machines-firewall. Een gegroepeerd benoemd exemplaar van Analysis Services luistert ook op poort **2383**.

Voor **benoemde exemplaar** van Analysis Services, is de SQL Server-Browser-service vereist voor het poort toegang beheren. De configuratie van de standaard SQL Server-Browser is poort **2382**.

In de firewall virtuele machines poort **2382** openen en een statische Analysis Services met de naam exemplaar poort maken.

1. Als u wilt controleren of poorten die al worden gebruikt op de VM en welke proces gebruikmaakt van de poorten, voert u de volgende opdracht uit met beheerdersbevoegdheden:

        netstat /ao

1. Gebruik SQL Server Management Studio om te maken van een statische Analysis Services naamwaarde exemplaar poort door 'Poort' bij te werken in tabelvorm onder exemplaar van de algemene eigenschappen. Zie de 'een vaste poort gebruiken voor een standaard of het benoemd exemplaar"in [configureren Windows Firewall Analysis Services-toegang toestaan](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed)voor meer informatie.

1. De tabelvormige exemplaar van de Analysis Services-service opnieuw starten.

Zie voor meer informatie de sectie **VM eindpunten en firewallpoorten** in dit document.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>VM eindpunten en firewallpoorten

In deze sectie bevat een overzicht van Microsoft Azure Virtual Machine eindpunten maken en poorten moeten worden geopend in de virtuele machine firewalls. De volgende tabel worden de **TCP** -poorten eindpunten voor maken en de poorten openen in de virtuele machines-firewall.

- Als u een enkel VM gebruikt en de volgende twee vereisten voldaan is, hoeft u niet VM eindpunten maken en u hoeft niet te open de poorten in de firewall op de VM.

    - U volgt extern geen verbinding maken met de SQL Server-functies op de VM. Een verbinding met extern bureaublad voor VM tot stand te brengen en toegang tot de functies van SQL Server lokaal op de VM wordt niet beschouwd als een externe verbinding met de SQL Server-functies.

    - U u niet deelnemen aan de VM naar een on-premises domein via Azure virtuele toegang of een andere VPN tunneling oplossing.

- Als de virtuele machine niet van een domein uitmaakt, maar u extern wilt verbinden met de SQL Server-functies op VM:

    - Open de poorten in de firewall op de VM.

    - Maak de eindpunten VM voor de vermelde poorten (*).

- Als de virtuele machine deel van een domein met een VPN-tunnel zoals Azure virtuele netwerkproblemen uitmaakt, klikt u vervolgens zijn de eindpunten niet vereist. Open de poorten echter in de firewall op de VM.

  	|Poort|Type|Beschrijving|
|---|---|---|
|**80**|TCP|Rapportserver RAS (*).|
|**1433**|TCP|SQL Server Management Studio (*).|
|**1434**|UDP|SQL Server-Browser. Dit is wanneer de VM in die is gekoppeld aan een domein nodig.|
|**2382**|TCP|SQL Server-Browser.|
|**2383**|TCP|SQL Server Analysis Services-standaardexemplaar en gegroepeerd benoemde exemplaren.|
|**Door de gebruiker gedefinieerde**|TCP|Maak een statische Analysis Services benoemde exemplaar poort voor een poortnummer die u kiest en vervolgens de blokkering opheffen het poortnummer in de firewall.|

Zie de volgende onderwerpen voor meer informatie over het maken van de eindpunten:

- Maak de eindpunten:[het instellen van de eindpunten naar een virtuele Machine](virtual-machines-windows-classic-setup-endpoints.md).

- SQL Server: Zie de sectie "Volledige configuratie stappen verbinding maken met de virtuele machine werken met SQL Server Management Studio" van de [inrichting van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md).

In het volgende diagram ziet u de poorten openen in de firewall VM om externe toegang te krijgen tot functies en -onderdelen op de VM.

![poorten voor bi toepassingen in Azure VMs opent](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Resources

- Bekijk het ondersteuningsbeleid voor Microsoft server-software die in de omgeving Azure virtuele machines worden gebruikt. Het volgende onderwerp bevat een overzicht van ondersteuning voor functies zoals BitLocker, Failoverclustering en netwerk van taakverdeling. [Serversoftware van Microsoft ondersteuning voor Azure virtuele Machines](http://support.microsoft.com/kb/2721672).

- [SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md)

- [Virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)

- [Een SQL Server virtuele Machine op Azure inrichting](virtual-machines-windows-portal-sql-server-provision.md)

- [Hoe u een gegevensschijf koppelt aan een virtuele Machine](virtual-machines-windows-classic-attach-disk.md)

- [Een Database migreren naar SQL Server op een Azure VM](virtual-machines-windows-migrate-sql.md)

- [Bepalen van de servermodus van een exemplaar van Analysis Services](https://msdn.microsoft.com/library/gg471594.aspx)

- [Multidimensionale modellen (Adventure Works-zelfstudie)](https://technet.microsoft.com/library/ms170208.aspx)

- [Azure documentatie Center](https://azure.microsoft.com/documentation/)

- [Power BI gebruiken in een hybride omgeving](https://msdn.microsoft.com/library/dn798994.aspx)

>[AZURE.NOTE] [Dien feedback- en contactgegevens tot en met Microsoft SQL Server verbinding maken](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Community-inhoud

- [Beheer van de Azure SQL-databases met PowerShell](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)
