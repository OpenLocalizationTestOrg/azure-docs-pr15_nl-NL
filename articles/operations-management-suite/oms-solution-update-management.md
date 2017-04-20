<properties
    pageTitle="Bijwerken van beheeroplossing in OMS | Microsoft Azure"
    description="In dit artikel is bedoeld om u te helpen u meer informatie over het gebruik van deze oplossing voor het beheren van updates voor uw computers met Windows en Linux."
    services="operations-management-suite"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="operations-management-suite"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="magoedte"/>

# <a name="update-management-solution-in-omsmediaoms-solution-update-managementupdate-management-solution-iconpng-update-management-solution-in-oms"></a>![Beheeroplossing in OMS bijwerken](./media/oms-solution-update-management/update-management-solution-icon.png) Beheeroplossing in OMS bijwerken

Het Update-beheeroplossing in OMS kunt u updates beheren voor uw computers met Windows en Linux.  U kunt snel de status van de beschikbare updates op alle agentcomputers beoordelen en het proces voor het installeren van de vereiste updates voor servers initiëren. 

## <a name="prerequisites"></a>Vereisten voor

-   Windows-agenten moeten worden geconfigureerd om te communiceren met een Windows Server Update Services (WSUS)-server of toegang hebt tot de Microsoft Update.  

    >[AZURE.NOTE] De Windows-agent worden niet door System Center Configuration Manager gelijktijdig beheerd.  
  
-   Linux agenten hebben toegang tot een bibliotheek bijwerken.  De OMS-Agent voor Linux kan worden gedownload van [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Configuratie

De volgende stappen om de Update beheeroplossing toevoegen aan uw werkruimte OMS toe te voegen Linux agenten uitvoeren.  Windows-agenten worden automatisch toegevoegd met geen aanvullende configuratie.

1.  Het Update-beheeroplossing toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [oplossingen OMS toevoegen](../log-analytics/log-analytics-add-solutions.md) vanuit de galerie met oplossingen.  
2.  Selecteer **Instellingen** en klik vervolgens op **Verbonden bronnen**in de portal OMS.  Houd rekening met de **werkruimte-ID** en de **primaire sleutel** of **secundaire sleutel**.
3.  De volgende stappen uitvoeren voor elke Linux-computer.

    een.  Installeer de meest recente versie van de OMS-Agent voor Linux door de volgende opdrachten uit te voeren.  Vervang <Workspace ID> met de werkruimte-ID en <Key> met de toets primaire of secundaire.

        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

     b. Als u wilt verwijderen de agent, moet u de volgende opdracht uitvoeren.

        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Management packs

Als uw systeem Center Operations Manager management groep is verbonden met uw werkruimte OMS, wordt klikt u vervolgens de volgende management packs geïnstalleerd in Operations Manager wanneer u deze oplossing toevoegt. Er is geen configuratie of onderhoud van deze management packs vereist. 

-   Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
-   Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
-   Implementatie MP bijwerken

Zie voor meer informatie over hoe de oplossing management packs worden bijgewerkt, [Verbinding Operations Manager naar Log Analytics](../log-analytics/log-analytics-om-agents.md).

## <a name="data-collection"></a>Gegevens verzamelen

### <a name="supported-agents"></a>Ondersteunde agenten

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing.

Verbonden bron | Ondersteund | Beschrijving|
----------|----------|----------|
Windows-agenten | Ja | De oplossing verzamelt gegevens over systeemupdates voor van Windows agenten en start de installatie van de vereiste updates.|
Linux agenten | Ja | De oplossing verzamelt gegevens over systeemupdates voor van Linux agenten.|
Operations Manager management groep | Ja | De oplossing verzamelt gegevens over systeemupdates voor van agenten in een groep verbonden management.<br>Een directe koppeling van de Operations Manager-agent naar Log Analytics is niet vereist. Gegevens uit de groep management doorgestuurd naar de bibliotheek OMS.|
Azure opslag-account | Nee | Azure opslagruimte is niet inbegrepen voor informatie over systeemupdates voor.|  

### <a name="collection-frequency"></a>Frequentie van de siteverzameling

Een scan wordt uitgevoerd voor elke beheerde Windows-computer, twee keer per dag.  Wanneer een update is geïnstalleerd, is de gegevens ook bijgewerkt binnen 15 minuten.  

Voor elke beheerde Linux-computer, wordt een gescande afbeelding om 3 uur uitgevoerd.  

## <a name="using-the-solution"></a>Gebruik van de oplossing

Wanneer u de Update beheeroplossing aan uw werkruimte OMS toevoegt, wordt de tegel **Update Management** worden toegevoegd aan uw dashboard OMS. Deze tegel wordt een aantal en grafische weergave van het aantal computers weergegeven in uw omgeving momenteel die moet worden systeem bijgewerkt.<br><br>
![Samenvatting tegel Management bijwerken](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Weergave Update beoordelingen

Klik op de tegel **Update Management** het **Update** -Beheerdashboard openen. Het dashboard bevat de kolommen in de volgende tabel. Elke kolom bevat maximaal tien items die overeenkomen met de die kolom criteria voor het opgegeven bereik en tijdsbereik. U kunt een zoekfunctie waarmee alle records geretourneerd door te klikken op **alle** onderaan in de kolom of door te klikken op de kolomkop waarop u kunt uitvoeren.

Kolom | Beschrijving|
----------|----------|
**Computers ontbrekende Updates** ||
Kritieke of beveiligingsupdates | Lijsten de top tien computers waarop ontbreken bijgewerkt gesorteerd op het aantal updates zijn de ontbrekende. Klik op de naam van een computer om uit te voeren een logboek zoekopdracht retourneren dat alle records voor de computer bijwerken.|
Kritiek of beveiligingsupdates die ouder zijn dan 30 dagen| Geeft het aantal computers die zijn ontbreekt kritieke of beveiligingsupdates die zijn gegroepeerd op de tijdsduur sinds de update is gepubliceerd. Klik op een van de items naar een logboek zoekopdracht retourneren alle ontbrekende en kritieke updates worden uitgevoerd.|
**Ontbrekende Updates vereist**||
Kritieke of beveiligingsupdates | Classificaties van updates dat computers ontbreken gesorteerd op het aantal computers ontbrekende updates in de categorie bevat. Klik op een classificatie voor het uitvoeren van een logboek zoekopdracht retourneren dat alle records voor die classificatie bijwerkt.|
**Updates installeren**||
Updates installeren | Het aantal geplande updates installeren en de duur tot de volgende geplande start.  Klik op de tegel naar weergave planningen, moment actief zijn en voltooide updates of naar een nieuwe implementatie plannen.|  
<br>  
![Samenvatting Beheerdashboard bijwerken](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Weergave van beheer Dashboard Computer bijwerken](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Weergave van beheer Dashboard pakket bijwerken](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Updates worden geïnstalleerd

Nadat de updates zijn voor alle van de Windows-computers in uw omgeving beoordeeld, kunt hebt u-updates hebt geïnstalleerd door te maken van een *Update implementatie*vereist.  Een Update-implementatie is een geplande installatie van de vereiste updates voor een of meer computers van Windows.  U de datum en tijd voor de implementatie naast een computer of een groep computers die opgenomen worden moeten.  

Updates worden geïnstalleerd door runbooks in Azure automatisering.  U kunt deze runbooks momenteel niet weergeven en ze elke configuratie niet vereisen.  Wanneer een bijwerken-implementatie is gemaakt, wordt een planning in die begint een basispagina update runbook op het opgegeven tijdstip voor de opgenomen computers.  Deze basispagina runbook begint een onderliggende runbook op elke Windows-agent waarmee u de installatie van de vereiste updates.  

### <a name="viewing-update-deployments"></a>Weergave updates installeren

Klik op de tegel **Update implementatie** om weer te geven van de lijst met bestaande updates installeren.  Ze zijn gegroepeerd op status – **gepland**, **uitgevoerd**en **voltooid**.<br><br> ![Pagina van de planning implementaties bijwerken](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

De eigenschappen weergegeven voor elke Update-implementatie worden beschreven in de volgende tabel.

Eigenschap | Beschrijving|
----------|----------|
Naam | Naam van de Update-implementatie.|
Planning | Type planning.  *OneTime* is momenteel de enige mogelijke waarde.|
Begintijd|Datum en tijd waarop de Update-implementatie volgens de planning moet beginnen.|
Duur | Het aantal minuten die de Update-implementatie is toegestaan om uit te voeren.  Als alle updates worden niet geïnstalleerd binnen deze duur, moeten klikt u vervolgens de resterende updates wachten tot de volgende Update-implementatie.|
Servers | Het aantal computers beïnvloed door de Update-implementatie.|
Status | Huidige status van de Update-implementatie.<br><br>Mogelijke waarden zijn:<br>-Niet gestart<br>-Uitgevoerd<br>-Voltooid|  

Klik op de implementatie van een Update naar de detail-scherm, waaronder de kolommen in de volgende tabel.  Deze kolommen wordt niet gevuld als de Update-implementatie is nog niet gestart.<br>

Kolom | Beschrijving|
----------|----------|
**Resultaten van de computer**||
Voltooid | Toont het aantal computers in de Update-implementatie op status.  Klik op een status om uit te voeren een logboek zoekopdracht retourneren dat alle records bijwerken met de status voor de implementatie bijwerken.|
Installatiestatus van de computer| Dit zijn de computers verbindt waarop het Update-implementatie- en het percentage van de updates die is geïnstalleerd. Klik op een van de items naar een logboek zoekopdracht retourneren alle ontbrekende en kritieke updates worden uitgevoerd.|
**Exemplaar resultaten bijwerken**||
Installatiestatus van exemplaar | Classificaties van updates dat computers ontbreken gesorteerd op het aantal computers ontbrekende updates in de categorie bevat. Klik op een computer als u wilt uitvoeren van een logboek zoekopdracht retourneren dat alle records voor de computer bijwerken.|  
<br><br> ![Overzicht van de resultaten van de Update-implementatie](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Maken van een Update-implementatie

Maak een nieuwe Update-implementatie door te klikken op de knop **toevoegen** aan de bovenkant van het scherm om de **Implementatie van de nieuwe** pagina te openen.  U moet waarden opgeven voor de eigenschappen in de volgende tabel.

Eigenschap | Beschrijving|
----------|----------|
Naam | Unieke naam voor de update-implementatie.|
Tijdzone | De tijdzone voor de begintijd wilt gebruiken.|
Begintijd | Datum en tijd aan het starten van de update-implementatie.|
Duur | Het aantal minuten die de Update-implementatie is toegestaan om uit te voeren.  Als alle updates worden niet geïnstalleerd binnen deze duur, moeten klikt u vervolgens de resterende updates wachten tot de volgende Update-implementatie.|
Computers | Namen van computers of computergroepen om op te nemen in de Update-implementatie.  Selecteer een of meer vermeldingen in de vervolgkeuzelijst.|
<br><br> ![Nieuwe pagina van de Update implementatie](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Tijdsbereik

Standaard is het bereik van de gegevens analyseren in de beheeroplossing voor bijwerken van alle verbonden management groepen gegenereerd binnen de laatste dag van de 1. 

Als het tijdsbereik van de gegevens, selecteert u de **gegevens op basis van** aan de bovenkant van het dashboard. U kunt de records die zijn gemaakt of bijgewerkt in de laatste 7 dagen, 1 dag of 6 uur selecteren. Of u kunt **aangepaste** selecteren en geef een aangepast datumbereik.<br><br> ![De optie aangepaste tijdsbereik](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Logboekrecords Analytics

Het Update-beheeroplossing Hiermee maakt u twee soorten records in de bibliotheek OMS.

### <a name="update-records"></a>Records bijwerken

Een record met een soort **bijwerken** wordt gemaakt voor elke update die wordt geïnstalleerd of op elke computer nodig. Update records hebt de eigenschappen in de volgende tabel.

Eigenschap | Beschrijving|
----------|----------|
Type | *Update*|
SourceSystem | De bron die installatie van de update goedgekeurd.<br>Mogelijke waarden zijn:<br>-Microsoft Update<br>-Windows Update<br>-SCCM<br>-Linux-Servers (van pakket Managers opgehaald)|
Goedgekeurd | Hiermee geeft u of de update voor de installatie is goedgekeurd.<br> Voor Linux-servers dit is momenteel optioneel als patches niet wordt beheerd door OMS.|
Indeling voor Windows | Classificatie van de update.<br>Mogelijke waarden zijn:<br>-Toepassingen<br>-Essentiële Updates<br>-Definition-Updates<br>-Packs functie<br>-Beveiligingsupdates<br>-Service Packs<br>-Samentellingen bijwerken<br>-Updates|
Indeling voor Linux | Cassification van de update.<br>Mogelijke waarden zijn:<br>-Essentiële Updates<br>-Beveiligingsupdates<br>-Andere Updates|
Computer | De naam van de computer.|
InstallTimeAvailable | Geeft aan of de installatietijd beschikbaar van andere agenten die de dezelfde update heb geïnstalleerd.|
InstallTimePredictionSeconds | Geschatte installatietijd in seconden op basis van andere agenten die de dezelfde update heb geïnstalleerd.|
KBID | ID van het KB-artikel waarmee de update wordt beschreven.|
ManagementGroupName | De naam van de management group voor SCOM agenten.  Voor andere gemachtigden, is dit de AOI -<workspace ID>.|
MSRCBulletinID | ID van het Microsoft-beveiligingsbulletin met een beschrijving van de update.|
MSRCSeverity | Ernst van het Microsoft-beveiligingsbulletin.<br>Mogelijke waarden zijn:<br>-Kritieke<br>-Belangrijke<br>-Optreden als moderator|
Optionele | Geeft aan of de update optioneel.|
Product | Naam van het product van de update is bedoeld voor.  Klik op **weergave** om te openen van het artikel in een browser.|
PackageSeverity | De ernst van het probleem opgelost in deze update, volgens de Linux distro leveranciers. | 
PublishDate | Datum en tijd waarop de update is geïnstalleerd.|
RebootBehavior | Hiermee geeft u als de update opnieuw opstarten afgedwongen.<br>Mogelijke waarden zijn:<br>-canrequestreboot<br>-neverreboots|
RevisionNumber | Het revisienummer van de update.|
SourceComputerId | GUID ter identificatie van de computer.|
TimeGenerated | Datum en tijd waarop de record voor het laatst werd bijgewerkt.|
Titel | De titel van de update.|
Update-id | GUID ter identificatie van de update.|
UpdateState | Hiermee geeft u of de update op deze computer is geïnstalleerd.<br>Mogelijke waarden zijn:<br>-Geïnstalleerd - is de update op deze computer geïnstalleerd.<br>-Nodig - de update niet is geïnstalleerd en op deze computer nodig is.|  

<br>
U kunt de weergave van de **Updates** die verschijnt een overzicht van de updates die het resultaat van de zoekopdracht samenvatten tegels selecteren wanneer u een logboek zoeken die resulteert in records met een soort **Update** uitvoert. U kunt klikken op de items in de **ontbrekende en toegepaste updates** en **vereiste en optionele updates** tegels om het bereik van de weergave die reeks updates. Selecteer de weergave **lijst** of **tabel** om de afzonderlijke records te geven.<br> 

![Weergave voor de Update van log zoeken met de Record van het Type Update](./media/oms-solution-update-management/update-la-view-updates.png)  

In **de tabelweergave,** kunt u klikken op de **KBID** voor elke record een browser geopend met het KB-artikel. Hiermee kunt u snel Lees meer over de details van de bepaalde update.<br> 

![Weergave voor de tabel van log zoeken met tegels recordtype Updates](./media/oms-solution-update-management/update-la-view-table.png)

Klik in **de lijstweergave** op de koppeling voor de **weergave** naast het KBID het KB-artikel openen.<br>

![Weergave voor de lijst van log zoeken met tegels recordtype Updates](./media/oms-solution-update-management/update-la-view-list.png)


###<a name="updatesummary-records"></a>UpdateSummary records

Een record met een soort **UpdateSummary** is voor elke computer met Windows-agent gemaakt. Deze record wordt bijgewerkt wanneer die de computer wordt gescand op updates. **UpdateSummary** records hebt de eigenschappen in de volgende tabel.

Eigenschap | Beschrijving|
----------|----------|
Type | UpdateSummary|
SourceSystem | OpsManager |
Computer | De naam van de computer.|
CriticalUpdatesMissing | Het aantal essentiële updates ontbreekt op de computer.|
ManagementGroupName | De naam van de management group voor SCOM agenten. Voor andere gemachtigden, is dit de AOI -<workspace ID>.|
NETRuntimeVersion | Versie van de .NET runtime op de computer is geïnstalleerd.|
OldestMissingSecurityUpdateBucket | Emmertje voor het categoriseren van de tijd na de oudste ontbrekende beveiliging op deze computer is gepubliceerd.<br>Mogelijke waarden zijn:<br>-Oudere<br>-180 dagen geleden<br>-150 dagen geleden<br>-120 dagen geleden<br>-90 dagen geleden<br>-60 dagen geleden<br>-gaat u 30 dagen<br>-Recente|
OldestMissingSecurityUpdateInDays | Het aantal dagen na de oudste ontbrekende beveiliging op deze computer is gepubliceerd.|
Versie_besturing | Versie van het besturingssysteem op de computer is geïnstalleerd.|
OtherUpdatesMissing | Het aantal andere updates ontbreekt op de computer.|
SecurityUpdatesMissing | Het aantal beveiligingsupdates ontbreken op de computer.|
SourceComputerId | GUID ter identificatie van de computer.|
TimeGenerated | Datum en tijd waarop de record voor het laatst werd bijgewerkt.|
TotalUpdatesMissing |Totaal aantal updates ontbreekt op de computer.|
WindowsUpdateAgentVersion | Het versienummer van de Windows Update-agent op de computer.|
WindowsUpdateSetting | Instelling voor hoe de computer belangrijke updates automatisch geïnstalleerd.<br>Mogelijke waarden zijn:<br>-Uitgeschakeld<br>-Een melding voor de installatie<br>-Geplande installatie|
WSUSServer | URL van WSUS-server als de computer is geconfigureerd voor gebruik een.|  

## <a name="sample-log-searches"></a>Voorbeeld log zoekopdrachten

De volgende tabel vindt steekproef log wordt gezocht naar update records die zijn verzameld met deze oplossing. 

Query | Beschrijving|
----------|----------|
Alle computers met ontbrekende updates | Type Update UpdateState = = optioneel nodig = false & #124; Selecteer de Computer, titel, KBID, indeling, UpdateSeverity en PublishedDate|
Ontbrekende updates voor computer "COMPUTER01.contoso.com" (vervangen door de naam van uw eigen computer) | Type Update UpdateState = = optioneel nodig = false Computer = "COMPUTER01.contoso.com" & #124; Selecteer de Computer, titel, KBID, Product, UpdateSeverity, PublishedDate|
Alle computers met ontbrekende kritieke of beveiligingsupdates | Type Update UpdateState = = optioneel nodig = false (classificatie = 'Beveiligingsupdates' of classificatie = "Essentiële Updates")|
Kritiek of beveiligingsgroepen updates die nodig zijn voor machines waar handmatig updates zijn toegepast | Type Update UpdateState = = optioneel nodig = false (classificatie = 'Beveiligingsupdates' of classificatie = "Essentiële Updates") Computer IN {Type = UpdateSummary WindowsUpdateSetting = handmatig & #124; DISTINCT Computer} & #124; DISTINCT KBID|
Foutgebeurtenissen voor machines met ontbrekende kritieke of beveiliging vereist updates | Type gebeurtenis EventLevelName = fout Computer IN = {Type Update = (classificatie = 'Beveiligingsupdates' of classificatie = "Essentiële Updates") UpdateState = optioneel nodig = false & #124; DISTINCT Computer}|
Alle computers met ontbrekende samentellingen bijwerken | Type Update optioneel = = false classificatie = 'Regel' UpdateState = nodig & #124; Selecteer de Computer, titel, KBID, indeling, UpdateSeverity en PublishedDate|
DISTINCT ontbrekende updates op alle computers | Type Update UpdateState = = optioneel nodig = false & #124; Unieke titel|
Lidmaatschap van de computer WSUS | Typ = UpdateSummary & #124; count() door WSUSServer meten|
Configuratie voor automatische updates | Typ = UpdateSummary & #124; count() door WindowsUpdateSetting meten|
Computers met automatische updates die worden uitgeschakeld | Typ = UpdateSummary WindowsUpdateSetting = handmatige|  
Lijst met alle Linux-computers die met een pakket update beschikbaar | Typ = bijwerken en waardereeksen = Linux en UpdateState! = "Niet nodig" & #124; eenheid count() door Computer|
Lijst met alle Linux-computers die met een pakket update beschikbaar welke Kritiek of beveiligingsgroepen beveiligingsprobleem adressen | Type = bijwerken en waardereeksen = Linux en UpdateState! "Niet nodig" = en (classificatie = 'Essentiële Updates' of classificatie = "Beveiligingsupdates") & #124; eenheid count() door Computer|
Lijst met alle pakketten met een update beschikbaar | Typ = bijwerken en waardereeksen = Linux en UpdateState! = 'Niet nodig'|
Lijst met alle pakketten met een update beschikbaar welke Kritiek of beveiligingsgroepen beveiligingsprobleem adressen | Type = bijwerken en waardereeksen = Linux en UpdateState! "Niet nodig" = en (classificatie = 'Essentiële Updates' of classificatie = "Beveiligingsupdates")|
Lijst met alle 'Ubuntu' computers met een update beschikbaar | Typ = bijwerken en waardereeksen = Linux en OSName = Ubuntu & #124; eenheid count() door Computer|

## <a name="next-steps"></a>Volgende stappen

- Gebruik zoekopdrachten in het logboek in [Log Analytics](../log-analytics/log-analytics-log-searches.md) gedetailleerde updategegevens moeten worden weergegeven.

- [Uw eigen dashboards maken](../log-analytics/log-analytics-dashboards.md) waarin update naleving voor uw beheerde computers.

- [Waarschuwingen maken](../log-analytics/log-analytics-alerts.md) wanneer u essentiële updates zijn gevonden als ontbreken in computers of een computer heeft automatische updates die worden uitgeschakeld.  




