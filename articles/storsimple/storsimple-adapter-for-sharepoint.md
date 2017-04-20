<properties 
   pageTitle="StorSimple Adapter voor SharePoint | Microsoft Azure"
   description="Wordt beschreven hoe installeren en configureren of verwijderen van de StorSimple-Adapter voor SharePoint in een SharePoint server-farm."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/11/2016"
   ms.author="v-sharos" />

# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>Installeren en configureren van de StorSimple-Adapter voor SharePoint

## <a name="overview"></a>Overzicht

De Adapter StorSimple voor SharePoint is een onderdeel waarmee u Microsoft Azure StorSimple flexibele opslag en gegevensbescherming naar SharePoint server-farms opgeven. U kunt de adapter binaire grote Object (BLOB) inhoud van de SQL Server-inhoudsdatabases gaan naar het apparaat met Microsoft Azure StorSimple hybride cloud-opslag.

De Adapter StorSimple voor SharePoint fungeert als een externe BLOB Storage (RBS)-provider en de functie externe SQL Server-blobopslag is gebruikt voor de opslag van ongestructureerde SharePoint-inhoud (in de vorm van BLOB's) op een bestandsserver die wordt ondersteund door een StorSimple-apparaat.

>[AZURE.NOTE] De StorSimple-Adapter voor SharePoint ondersteunt SharePoint Server 2010 Remote BLOB Storage (RBS). SharePoint Server 2010 extern BLOB Storage (EBS) worden niet ondersteund.

- Als u wilt downloaden de StorSimple-Adapter voor SharePoint, gaat u naar [StorSimple Adapter voor SharePoint] [ 1] in het Microsoft Download Center.

- Voor informatie over planning voor Resourcestructuur en Resourcestructuur beperkingen, gaat u naar [Gebruik Resourcestructuur in SharePoint 2013 gebruiken] [ 2] of [plannen voor Resourcestructuur (SharePoint Server 2010)][3].

De rest van dit overzicht een korte beschrijving van de rol van de StorSimple-Adapter voor SharePoint en de SharePoint-capaciteit en prestaties limieten waarmee u houden moet rekening voordat u installeren en configureren van de adapter. Nadat u deze gegevens bekijken, gaat u naar [StorSimple Adapter voor SharePoint-installatie](#storsimple-adapter-for-sharepoint-installation) om te beginnen bij het instellen van de adapter.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple Adapter voor voordelen van SharePoint

Inhoud wordt opgeslagen in een SharePoint-site als ongestructureerde BLOB-gegevens in een of meer inhoudsdatabases. Standaard worden deze databases worden gehost op computers waarop SQL Server worden uitgevoerd en zich bevinden in de SharePoint server-farm. BLOB's toenemen snel grootte, door andere grote hoeveelheden on-premises implementatie opslag. Daarom mogelijk u wilt zoeken naar een andere, minder opslagruimte oplossing. SQL Server biedt een technologie met de naam van de externe Blob Storage (RBS) waarmee u BLOB-inhoud opslaan in het bestandssysteem, buiten de SQL Server-database. Met Resourcestructuur, BLOB's kunnen zich bevinden in het bestandssysteem op de computer waarop SQL Server wordt uitgevoerd, of ze kunnen worden opgeslagen in het bestandssysteem op een andere servercomputer.

Resourcestructuur is vereist dat u een provider voor Resourcestructuur, zoals de StorSimple-Adapter voor SharePoint, gebruiken om in te schakelen Resourcestructuur in SharePoint. De Adapter StorSimple voor SharePoint werkt met Resourcestructuur, zodat u BLOB's verplaatsen naar een back-up gemaakt door het systeem Microsoft Azure StorSimple server. Microsoft Azure StorSimple vervolgens de gegevens zijn opgeslagen BLOB lokaal of in de cloud, op basis van gebruik. BLOB's die zeer actieve zijn (meestal laag 1 of genoemd warm gegevens) staan lokaal. Minder actieve gegevens en archivering gegevens zich bevinden in de cloud. Nadat u op een inhoudsdatabase Resourcestructuur hebt ingeschakeld, wordt een nieuwe BLOB-inhoud die is gemaakt in SharePoint op het apparaat StorSimple en niet in de inhoudsdatabase opgeslagen.

De toepassing Microsoft Azure StorSimple van Resourcestructuur biedt de volgende voordelen:

- Zwevend BLOB-inhoud op een afzonderlijke server, kunt u het laden van een query op SQL Server, waar reactiesnelheid met SQL Server kunt beperken. 

- Azure StorSimple maakt gebruik van deduplication en compressie om te verkleinen van de gegevens.

- Azure StorSimple biedt gegevensbescherming in de vorm van lokale en cloud momentopnamen. Ook als u de database zelf op het apparaat StorSimple plaatst, kunt u back-up van de inhoudsdatabase en BLOB's samen op een vastlopen consistente manier. (De inhoudsdatabase verplaatsen naar het apparaat wordt alleen ondersteund voor het apparaat van de reeks StorSimple 8000. Deze functie wordt niet ondersteund voor de reeks 5000 of 7000.)

- Azure StorSimple bevat noodgevallen herstelfuncties waaronder failover, bestands- en volume herstel (inclusief herstelprocedure) en snelle herstel van gegevens.

- U kunt gegevens herstelsoftware, zoals Kroll Ontrack PowerControls, gebruiken met StorSimple momentopnamen van BLOB-gegevens itemniveau herstellen van SharePoint-inhoud. (Deze gegevens herstelsoftware is een afzonderlijke inkooporders.)

- De StorSimple-Adapter voor SharePoint wordt in de portal Centraal beheer van SharePoint, zodat u kunt uw hele SharePoint-oplossing beheren vanaf een centrale locatie.

BLOB inhoud verplaatsen naar het bestandssysteem kan worden verstrekt andere kosten besparen en voordelen. Bijvoorbeeld gebruiken Resourcestructuur kunt verkleinen dat u een dure laag 1-opslag en omdat deze de inhoudsdatabase kleiner, Resourcestructuur het aantal databases die zijn vereist in de SharePoint server-farm kunt verminderen. Kan zijn dat de andere factoren, zoals de maximale grootte database en het aantal niet-Resourcestructuur inhoud, ook opslagvereisten beïnvloeden. Zie voor meer informatie over de kosten en voordelen van het gebruik van Resourcestructuur, [plannen voor Resourcestructuur (SharePoint Foundation 2010)] [ 4] en [Gebruik Resourcestructuur in SharePoint 2013 gebruiken][5].

### <a name="capacity-and-performance-limits"></a>Limieten voor capaciteit en prestaties

Voordat u rekening houden met het gebruik van Resourcestructuur in uw SharePoint-oplossing, moet u rekening houden met het geteste prestaties en Capaciteitslimieten van SharePoint Server 2010 en SharePoint Server 2013 en hoe deze limieten zich verhouden tot acceptabel zijn. Zie [Softwaregrenzen en -limieten voor SharePoint 2013](https://technet.microsoft.com/library/cc262787.aspx)voor meer informatie.

Voordat u Resourcestructuur configureren, raadpleegt u de volgende handelingen uit:

- Zorg ervoor dat de totale grootte van de inhoud (de grootte van een inhoudsdatabase) plus de grootte van een gekoppeld externalized BLOB's niet groter is dan de bestandslimiet van de Resourcestructuur wordt ondersteund door SharePoint. Deze limiet is 200 GB. 

    **In de inhoudsdatabase maateenheid en de grootte van de BLOB**

     1. Deze query uitvoeren op het centrale beheer WFE. Start de SharePoint-beheershell en voer de volgende Windows PowerShell-opdracht om de grootte van de inhoudsdatabases:

        `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`

         Deze stap wordt de grootte van de database met inhoud op de schijf.

     2. Voer een van de volgende SQL-query's in SQL Management Studio op het vak van de SQL server op elke inhoudsdatabase en het resultaat toevoegen aan het getal verkregen in stap 1.

        Voer op de SharePoint 2013 inhoudsdatabases:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`

        Voer op de SharePoint 2010-inhoudsdatabases:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`

        Deze stap wordt de grootte van de BLOB's die u hebt is externalized.

- Het is raadzaam dat u alle BLOB en database-inhoud lokaal opslaan op het apparaat StorSimple. Het apparaat StorSimple is een cluster met twee knooppunten voor maximale beschikbaarheid. De inhoudsdatabases en BLOB's plaatsen op het apparaat StorSimple bevat beschikbaarheid.

    Gebruik traditionele SQL Server-migratie aanbevolen procedures de inhoudsdatabase verplaatsen naar het StorSimple-apparaat. Verplaats de database nadat alle BLOB inhoud van de database is verplaatst naar het bestand delen via de Resourcestructuur. Als u de database met inhoud verplaatsen naar het apparaat StorSimple kiest, wordt u aangeraden om de opslag inhoudsdatabase te op het apparaat als een primaire volume configureren.

- In Microsoft Azure-StorSimple, als via doorverbonden volume, is er geen te garanderen dat inhoud lokaal opgeslagen op het apparaat wordt niet worden doorverbonden naar Microsoft Azure-cloudopslag StorSimple. Dus is het raadzaam StorSimple lokaal vastgemaakt volumes gebruiken in combinatie met SharePoint RBS. Dit zorgt ervoor dat alle BLOB inhoud blijft lokaal op het apparaat StorSimple, en niet wordt verplaatst naar Microsoft Azure.

- Als u niet de inhoudsdatabases bij het apparaat StorSimple bewaren, gebruikt u traditionele SQL Server beschikbaarheid aanbevolen procedures die ondersteuning bieden voor Resourcestructuur. Resourcestructuur cluster van SQL Server ondersteunt, terwijl de SQL Server spiegelen betekent niet. 

>[AZURE.WARNING] Als u niet Resourcestructuur hebt ingeschakeld, wordt niet aanbevolen de inhoudsdatabase te verplaatsen naar de StorSimple-apparaat. Dit is een niet-geteste configuratie.
 
## <a name="storsimple-adapter-for-sharepoint-installation"></a>StorSimple Adapter voor SharePoint-installatie

Voordat u de StorSimple-Adapter voor SharePoint installeren kunt, moet u het apparaat StorSimple configureren en zorg ervoor dat de SharePoint server-farm en SQL Server Activeringsformulier voldoet aan alle vereisten voldoet. Deze zelfstudie wordt beschreven configuratievereisten, evenals de procedures voor het installeren en upgraden van de StorSimple-Adapter voor SharePoint. 

## <a name="configure-prerequisites"></a>Vereisten voor configureren

Controleer voordat u de StorSimple-Adapter voor SharePoint installeren kunt, of dat de StorSimple apparaat, SharePoint server-farm en SQL Server Activeringsformulier voldoet aan de volgende vereisten.

### <a name="system-requirements"></a>Systeemvereisten

De Adapter StorSimple voor SharePoint werkt met de volgende hardware en software:

- Ondersteund besturingssysteem – Windows Server 2008 R2 SP1, Windows Server 2012 of Windows Server 2012 R2 

- Ondersteunde SharePoint-versies – SharePoint Server 2010 of SharePoint Server 2013

- Ondersteunde SQL Server-versie: SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition of SQL Server 2012 Enterprise Edition

- Ondersteunde apparaten StorSimple – StorSimple 8000 reeks, StorSimple 7000 reeks of StorSimple 5000 reeks.

### <a name="storsimple-device-configuration-prerequisites"></a>Vereisten voor de configuratie van de StorSimple apparaat

Het apparaat StorSimple is een apparaat blokkeren en als zodanig is vereist een bestandsserver waarop de gegevens kunnen worden gehost. Het is raadzaam dat u een afzonderlijke server in plaats van een bestaande server uit de SharePoint-farm gebruiken. Deze bestandsserver moet zich op de hetzelfde LAN (LAN) als de SQL Server-computer waarop de inhoudsdatabases. 

>[AZURE.TIP] 
>
>- Als u uw SharePoint-farm beschikbaarheid configureren, moet u ook de bestandsserver beschikbaarheid distribueren.
>
>- Als u niet de inhoudsdatabase bij het apparaat StorSimple bewaren, gebruikt u traditionele beschikbaarheid aanbevolen procedures die ondersteuning bieden voor Resourcestructuur. Resourcestructuur cluster van SQL Server ondersteunt, terwijl de SQL Server spiegelen betekent niet. 

Zorg ervoor dat uw apparaat StorSimple correct is geconfigureerd en dat de juiste hoeveelheden die moeten de ondersteuning van uw SharePoint-implementatie zijn geconfigureerd en toegankelijk zijn vanuit uw SQL Server-computer. Ga naar [uw on-premises implementatie StorSimple apparaat Deploy](storsimple-deployment-walkthrough.md) als u nog niet geïmplementeerd en uw apparaat StorSimple geconfigureerd. Houd rekening met het IP-adres van het apparaat StorSimple; moet u deze tijdens StorSimple Adapter voor SharePoint-installatie. 

Bovendien controleren of het volume moet worden gebruikt voor BLOB externalization voldoet aan de volgende vereisten:

- Het volume moet worden opgemaakt als een 64 KB-toewijzing eenheidsgrootte.

- Uw web-front-end (WFE) en de toepassingsservers moet kunnen toegang tot het volume via een pad (Universal Naming Convention). 

- De SharePoint server-farm moet worden geconfigureerd om het volume te schrijven.

>[AZURE.NOTE] Als u installeren en configureren van de adapter, alle BLOB externalization moet gaan via het apparaat StorSimple (het apparaat wordt de hoeveelheden presenteren aan SQL Server en de lagen opslag beheren). U kunt andere doelen niet gebruiken voor BLOB externalization.
 
Als u van plan bent StorSimple momentopname Manager om momentopnamen van de gegevens BLOB en een database te gebruiken, moet u StorSimple momentopname Manager op de databaseserver installeren, zodat deze SQL Writer Service gebruiken kan voor de uitvoering van de Windows Volume schaduw Copy Service (VSS). 

>[AZURE.IMPORTANT] StorSimple momentopname Manager biedt geen ondersteuning voor de SharePoint-VSS schrijver en toepassing consistente momentopnamen kan geen van de SharePoint-gegevens. In een scenario voor SharePoint biedt StorSimple momentopname Manager alleen vastlopen consistente back-ups. 
 
## <a name="sharepoint-farm-configuration-prerequisites"></a>Vereisten voor de configuratie van de SharePoint-farm

Zorg ervoor dat uw SharePoint-serverfarm correct is geconfigureerd, als volgt:

- Controleer of uw SharePoint-serverfarm in orde zijn, en controleer het volgende: 

- Alle SharePoint WFE en geregistreerd in de farm toepassingsservers worden uitgevoerd en kunnen worden gepingd van de server waarop u de StorSimple-Adapter voor SharePoint installeren wilt.

- De SharePoint-Timer-service (SPTimerV3 of SPTimerV4) wordt uitgevoerd op elke WFE-server en toepassingsserver.

- De SharePoint-Timer-service en de IIS-toepassingen waarmee de Centraal beheer van SharePoint-site wordt uitgevoerd beheerdersmachtigingen. 

- Zorg ervoor dat Internet Explorer Enhanced beveiligingscontext (IE ESC) is uitgeschakeld. Volg deze stappen om de verbeterde beveiliging uitschakelen:

    1. Sluit alle exemplaren van Internet Explorer.

    2. Start de Server Manager.

    3. Klik in het linkerdeelvenster op **Lokale Server**.

    4. Klik in het rechterdeelvenster, naast **Verbeterde beveiliging van IE**, **op**.

    5. **Beheerders**, klik op **uitschakelen**.

    6. Klik op **OK**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Vereisten voor externe BLOB Storage (RBS)

Zorg ervoor dat u een ondersteunde versie van SQL Server worden gebruikt. Alleen de volgende versies zijn ondersteunde en gebruikmaken van Resourcestructuur:

- SQL Server 2008 Enterprise Edition

- SQL Server 2008 R2 Enterprise Edition

- SQL Server 2012 Enterprise Edition

BLOB's kunnen worden externalized voor alleen volumes die het apparaat StorSimple met SQL Server presenteert. Geen andere doelen voor BLOB externalization worden ondersteund.

Nadat u alle vereiste configuratiestappen hebt voltooid, gaat u naar [de Adapter StorSimple voor SharePoint installeren](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>Installeer de StorSimple-Adapter voor SharePoint

Gebruik de volgende stappen uit de StorSimple-Adapter voor SharePoint te installeren. Als u de software opnieuw installeert, raadpleegt u [upgraden of opnieuw installeren van de StorSimple-Adapter voor SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). De tijd die nodig is voor de installatie, is afhankelijk van het totale aantal SharePoint-databases in de SharePoint server-farm.

[AZURE.INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]


## <a name="configure-rbs"></a>Resourcestructuur configureren

Nadat u de StorSimple-Adapter voor SharePoint hebt geïnstalleerd, configureert u Resourcestructuur zoals wordt beschreven in de volgende procedure.

>[AZURE.TIP] De Adapter StorSimple voor SharePoint kunt aansluiten op de pagina Centraal beheer van SharePoint toestaan Resourcestructuur worden ingeschakeld of uitgeschakeld voor elke inhoudsdatabase in de SharePoint-farm. In- of Resourcestructuur uitschakelen op de inhoudsdatabase veroorzaakt echter een IIS opnieuw instellen, die, afhankelijk van uw serverfarm-configuratie tijdelijk de beschikbaarheid van de SharePoint-web-front-end (WFE verstoort kunt). (Factoren zoals het gebruik van een front-end taakverdeling, de werklast van de huidige server, enzovoort, kunnen beperken of geen deze verstoringen.) Als u wilt voorkomen dat gebruikers een onderbreking, raden dat u in- of uitschakelen van Resourcestructuur alleen tijdens een venster gepland onderhoud.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]
 

## <a name="configure-garbage-collection"></a>Opschonen configureren

Wanneer u objecten uit een SharePoint-site worden verwijderd, worden ze niet automatisch verwijderd uit het volume van de store Resourcestructuur. In plaats daarvan een asynchroon, achtergrond onderhoud wordt verwijderd zwevende BLOB's uit de bestandsopslag. Systeembeheerders kunnen deze procedure om uit te voeren regelmatig plannen of ze deze indien nodig kunnen starten.

Dit onderhoudsprogramma (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) wordt automatisch geïnstalleerd op alle SharePoint WFE servers en toepassingsservers wanneer u Resourcestructuur inschakelt. Het programma is geïnstalleerd op de volgende locatie: *opstartstation*: \Program Files\Microsoft externe SQL-blobopslag 10.50\Maintainer\

Voor informatie over de configuratie en met het onderhoudsprogramma, raadpleegt u [Voor het behoud van Resourcestructuur in SharePoint Server 2013][8].

>[AZURE.IMPORTANT] Resourcestructuur maintainer dit programma is veel bronnen. U moet deze alleen tijdens perioden van light activiteit uitvoeren op de SharePoint-farm plannen.

### <a name="delete-orphaned-blobs-immediately"></a>Zwevende BLOB's onmiddellijk verwijderen

Als u dat geen bovenliggend BLOB's onmiddellijk verwijderen wilt, kunt u de volgende instructies gebruiken. Houd er rekening mee dat deze instructies zijn bedoeld een voorbeeld van hoe u dit in een SharePoint 2013-omgeving met de volgende onderdelen doen kunt:

- De naam van de inhoudsdatabase is WSS_Content.
- De naam van de SQL Server is SHRPT13-SQL12\SHRPT13.
- Naam van de webtoepassing is SharePoint – 80.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]


## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>Upgraden of opnieuw installeren van de StorSimple-Adapter voor SharePoint

Gebruik de volgende procedure upgrade van SharePoint server en installeer Office opnieuw StorSimple Adapter voor SharePoint of voor gewoon upgraden of de adapter in een bestaande SharePoint-serverfarm opnieuw te installeren. 

>[AZURE.IMPORTANT] Lees de volgende informatie voordat u probeert te upgraden van uw SharePoint-software en/of de upgrade of opnieuw installeren van de StorSimple-Adapter voor SharePoint:
>
>- Alle bestanden die eerder zijn verplaatst naar externe opslag via de Resourcestructuur zijn pas beschikbaar nadat de installatie is voltooid en de Resourcestructuur-functie opnieuw is ingeschakeld. U beperkt de gevolgen voor de gebruiker door uitvoeren een upgrade of nieuwe installatie tijdens een venster gepland onderhoud.
>
>- De tijd die nodig is voor de upgrade/installatie kan variëren, afhankelijk van het totale aantal SharePoint-databases in de SharePoint server-farm.
>
>- Nadat de upgrade/installatie voltooid is, moet u Resourcestructuur inschakelen voor de inhoudsdatabases. Zie [Resourcestructuur configureren](#configure-rbs) voor meer informatie.
>
>- Als u Resourcestructuur configureert voor een SharePoint-farm met een groot aantal databases (groter dan 200), kan de **Centraal beheer van SharePoint** -pagina time-out. Als dat plaatsvindt, vernieuwt u de pagina. Dit geldt niet voor het configuratieproces.

[AZURE.INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]
 
## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple Adapter voor SharePoint verwijderen

De volgende procedures wordt uitgelegd hoe u de BLOB's terug verplaatsen naar de SQL Server-inhoudsdatabases en verwijder de StorSimple-Adapter voor SharePoint. 

>[AZURE.IMPORTANT] U moet de BLOB's terug verplaatsen naar de inhoudsdatabases voordat u de adaptersoftware verwijderen. 

### <a name="before-you-begin"></a>Voordat u begint 

De volgende informatie verzamelen voordat u de gegevens terug naar de SQL Server-inhoudsdatabases verplaatsen en het proces voor het verwijderen van adapter start:

- De namen van alle databases waarvoor Resourcestructuur is ingeschakeld
- Het UNC-pad van de geconfigureerde BLOB store

### <a name="move-the-blobs-back-to-the-content-databases"></a>De BLOB's terug verplaatsen naar de inhoudsdatabases

Als u de Adapter StorSimple voor SharePoint-software wilt verwijderen, moet u alle de BLOB's die zijn externalized terug naar de SQL Server-inhoudsdatabases migreren. Als u probeert te verwijderen van de StorSimple-Adapter voor SharePoint voordat u alle BLOB's terug naar de inhoudsdatabases verplaatsen, ziet u de volgende waarschuwing weergegeven.

![Waarschuwingsbericht](./media/storsimple-adapter-for-sharepoint/sasp1.png)
 
####<a name="to-move-the-blobs-back-to-the-content-databases"></a>De BLOB's terug verplaatsen naar de inhoudsdatabases 

1. Download elk van de externalized objecten.

2. Open de pagina **Centraal beheer van SharePoint** en blader naar **Systeeminstellingen**. 

3. Klik onder **Azure StorSimple**, klikt u op **StorSimple Adapter configureren**.

4. Klik op de pagina **StorSimple Adapter configureren** op de knop **uitschakelen** onder elk van de inhoudsdatabases die u wilt verwijderen uit de externe-blobopslag. 

5. Verwijder de objecten uit SharePoint en vervolgens opnieuw uploaden.

U kunt ook kunt u de Microsoft` RBS Migrate()` PowerShell-cmdlet die bij SharePoint worden geleverd. Zie [inhoud migreren in- of Resourcestructuur](https://technet.microsoft.com/library/ff628255.aspx)voor meer informatie.

Nadat u de BLOB's naar de inhoudsdatabase verplaatsen, gaat u naar de volgende stap: [de adapter verwijderen](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>De adapter verwijderen

Nadat u de BLOB's terug verplaatsen naar de SQL Server-inhoudsdatabases, een van de volgende opties gebruiken om de StorSimple-Adapter voor SharePoint verwijderen.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Gebruik het installatieprogramma van de adapter verwijderen 

1. Gebruik een account met beheerdersbevoegdheden aanmelden bij de webserver voor de front (WFE).
2. Dubbelklik op de StorSimple-Adapter voor SharePoint installer. De Wizard Setup wordt gestart.

    ![Installatiewizard](./media/storsimple-adapter-for-sharepoint/sasp2.png)

3. Klik op **volgende**. De volgende pagina wordt weergegeven.

    ![Pagina voor het opzetten wizard verwijderen](./media/storsimple-adapter-for-sharepoint/sasp3.png)

4. Klik op **verwijderen** om de verwijderingsproces hebt geselecteerd. De volgende pagina wordt weergegeven.

    ![Pagina voor het opzetten wizard bevestiging](./media/storsimple-adapter-for-sharepoint/sasp4.png)

5. Klik op **verwijderen** om het verwijderen te bevestigen. De volgende pagina van de voortgang wordt weergegeven.

    ![Pagina voor het opzetten wizard voortgang](./media/storsimple-adapter-for-sharepoint/sasp5.png)

6. Wanneer het verwijderen voltooid is, wordt de pagina finish weergegeven. Klik op **Voltooien** om de Wizard Setup te sluiten.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Gebruik van het Configuratiescherm de adapter verwijderen 

1. Open het Configuratiescherm en klik vervolgens op **programma's en onderdelen**.

2. Selecteer **StorSimple Adapter voor SharePoint**en klik vervolgens op **verwijderen**. 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx
