<properties
   pageTitle="Toepassing afhankelijkheid Monitor (ADM) in bewerkingen Management-Suite (OMS) | Microsoft Azure"
   description="Toepassing afhankelijkheid Monitor (ADM) is een bewerkingen Management Suite Kantoorbeheersysteem-oplossing die automatisch wordt gevonden toepassingsonderdelen op Windows en Linux-systemen en toewijzingen van de communicatie tussen services.  In dit artikel bevat informatie voor ADM implementatie in uw omgeving en het gebruik van verschillende scenario's."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="using-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Gebruik van toepassing afhankelijkheid Monitor oplossing in bewerkingen Management Suite Kantoorbeheersysteem
![Waarschuwingspictogram Management](media/operations-management-suite-application-dependency-monitor/icon.png) Toepassing afhankelijkheid Monitor (ADM) automatisch detecteert toepassingsonderdelen op Windows en Linux-systemen en toewijzingen van de communicatie tussen services. U kunt uw servers weergeven terwijl u deze persoon zien als overlappende systemen waarop kritieke services geven.  Toepassing afhankelijkheid Monitor ziet u verbindingen tussen servers, processen, en poorten over eventuele architectuur TCP-verbonden met geen configuratie vereist andere dan de installatie van een agent.

In dit artikel worden de details van het gebruik van de toepassing afhankelijkheid Monitor.  Zie voor informatie over het configureren van ADM en onboarding agenten [configureren toepassing afhankelijkheid Monitor oplossing in bewerkingen Management Suite Kantoorbeheersysteem](operations-management-suite-application-dependency-monitor-configure.md)

>[AZURE.NOTE]Toepassing afhankelijkheid Monitor is momenteel in de proefversie van privé.  U kunt toegang tot de ADM privé voorbeeldversie bij [https://aka.ms/getadm](https://aka.ms/getadm)aanvragen.
>
>In privé voorbeeldweergave, alle OMS-accounts zijn onbeperkte toegang tot ADM.  ADM knooppunten zijn gratis, maar Log Analytics-gegevens voor de typen AdmComputer_CL en AdmProcess_CL wordt als een andere oplossing worden gemeten.
>
>Nadat ADM openbare preview invoert, is dit is alleen beschikbaar voor gratis en betaalde klanten van inzicht & Analytics in het Kantoorbeheersysteem prijzen Plan.  Gratis laag accounts worden beperkt tot 5 ADM knooppunten.  Als u aan de privé preview deelneemt en niet worden geregistreerd in het Kantoorbeheersysteem prijzen Plan wanneer ADM openbare preview invoert, wordt op dat moment ADM uitgeschakeld. 


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Gebruik zaken: Controleer uw IT afhankelijkheid hoogte verwerkt

### <a name="discovery"></a>Discovery
ADM genereert automatisch een algemene verwijzing kaart van afhankelijkheden in uw servers, processen en 3e partijen services.  Dit ontdekt en alle TCP-afhankelijkheden, identificeren verrassing verbindingen, externe kaarten 3e partijen systemen u, hangt af van en afhankelijkheden naar traditionele donkere gebieden van uw netwerk zoals DNS en AD.  ADM ontdekt mislukte netwerkverbindingen dat uw beheerde systemen maken wilt, zodat u mogelijke server onjuiste configuratie, bijvoorbeeld van service en netwerkproblemen identificeren.

### <a name="incident-management"></a>Oproepbeheer
ADM helpt de ervoor probleem isolatieniveau voorkomen door waarin hoe systemen zijn verbonden en dat dit gevolgen heeft elkaar.  Naast het mislukte verbindingen te informatie over verbonden clients zijn onjuist geconfigureerd netwerktaakverdelers, verrassend of overtollige belasting op kritieke services, identificeren en rogue clients zoals ontwikkelaars machines systemen spreekt.  Geïntegreerde werkstromen met OMS wijzigen bijhouden u kunt ook zien of een wijzigingsgebeurtenis op de computer van een back-enddatabase of service wordt uitgelegd dat de oorzaak van een incident.

### <a name="migration-assurance"></a>Migratie Assurance
ADM kunt u effectief plannen, versnellen en Azure migraties valideren te controleren of er niets resteert achter en er geen bijvoorbeeld verrassing zijn.  U kunt alle onderling afhankelijke systemen die moeten samenwerken migreren, Beoordeel systeemconfiguratie en capaciteit en geef aan of een actief systeem gebruikers nog steeds fungeert of een candidate voor buiten bedrijf stellen in plaats van de migratie is kan detecteren.  Wanneer de verplaatsing klaar is, kunt u controleren op de client laden en -identiteit om te bevestigen dat testsystemen en klanten verbinding maakt.  Als uw subnet plannen en firewall definities problemen ondervindt, wijs mislukte verbindingen ADM kaarten u naar de systemen die nodig connectivity hebt.

### <a name="business-continuity"></a>Bedrijfscontinuïteit
Als u Azure Site herstel gebruikt en moet Help bij het definiëren van de reeks herstel voor uw toepassingsomgeving, ADM automatisch ziet u hoe systemen zijn afhankelijk van elkaar om ervoor te zorgen dat is uw abonnement herstel betrouwbaar.  Door een kritieke server kiezen en weergeven van de clients, kunt u de front-mailsystemen die moeten worden hersteld nadat die kritieke server herstelde en beschikbaar is bepalen.  Door te kijken op een kritieke server back-enddatabase afhankelijkheden, kunt u daarentegen systemen die moeten worden hersteld voordat uw systeem focus wordt teruggezet identificeren.

### <a name="patch-management"></a>Patch Management
Het gebruik van OMS systeem Update Assessment verbetert ADM door weer die andere teams en -servers, hangt af van uw service, zodat u deze vooraf waarschuwen kunt voordat u uw systemen omlaag voor patches te.  ADM ook patch management in OMS wordt verbeterd door te tonen of uw services zijn alleen beschikbaar en goed verbonden nadat ze zijn geïnstalleerd en opnieuw gestart. 


## <a name="mapping-overview"></a>Overzicht van de toewijzing
ADM agenten Verzamel informatie over alle TCP-verbonden processen op de server waar deze zijn geïnstalleerd, evenals meer informatie over de binnenkomende en uitgaande verbindingen voor elk proces.  De lijst Machine aan de linkerkant van de oplossing ADM gebruikt, kunnen machines met ADM agenten kunt visualiseren bijbehorende afhankelijkheden via een geselecteerde tijdsbereik worden geselecteerd.  Machine afhankelijkheid focus op een specifieke machine kaarten en alle computers die directe TCP-clients en servers van deze computer zijn weergeven.

![ADM-overzicht](media/operations-management-suite-application-dependency-monitor/adm-overview.png)

Computers kunnen worden uitgevouwen in de kaart om weer te geven van de actieve processen met actieve netwerkverbindingen tijdens het geselecteerde tijdsbereik.  Wanneer een externe computer met een agent ADM wordt uitgevouwen Procesdetails wilt weergeven, worden alleen deze processen communiceren met de focus computer weergegeven.  Het aantal agentless front machines verbinding in de focus machine wordt aangegeven aan de linkerkant van de processen die ze verbinding met maken.  Als de focus machine is een verbinding met een back-enddatabase machine zonder een agent, waardoor u dat back-enddatabase wordt aangeduid met een knooppunt in de map die de bijbehorende IPv4-adres en het knooppunt kan worden uitgevouwen als afzonderlijke poorten en -services die de focus machine met communiceert wilt weergeven.

Standaard weergeven ADM kaarten de laatste 10 minuten van objectafhankelijkheidsinformatie.  Met de besturingselementen tijd in de linkerbovenhoek, kunnen kaarten worden doorzocht voor historische tijd bereiken, afgerond op één uur wide, om weer te geven hoe afhankelijkheden gezocht in het verleden, bijvoorbeeld tijdens een incident of voordat een wijziging is opgetreden.    ADM gegevens worden opgeslagen voor 30 dagen in betaalde werkruimten en zeven dagen in gratis werkruimten.

![Machine kaart met geselecteerde machine eigenschappen](media/operations-management-suite-application-dependency-monitor/machine-map.png)

## <a name="failed-connections"></a>Mislukte verbindingen
Mislukte verbindingen worden weergegeven in ADM-kaarten om processen en computers, met een rode stippellijn wordt weergegeven als een clientsysteem is verbroken om een proces of poort te gaan.  Mislukte verbindingen wordt gerapporteerd vanaf elk systeem met een uitgevouwen ADM-agent als dat systeem is de sectie die u probeert de verbinding is mislukt.  ADM meet dit door de naleving van TCP-sockets waarvan een verbinding tot stand is mislukt.  Dit kan zijn vanwege een firewall, een onjuiste configuratie van de client of server of een externe service tijdelijk niet beschikbaar zijn. 

![Mislukte verbindingen](media/operations-management-suite-application-dependency-monitor/failed-connections.png)

Informatie over mislukte verbindingen kunnen helpen bij het oplossen van problemen migratie validatie, beveiligingsanalyse en algemene architectuur lidmaatschap.  Soms is mislukt verbindingen worden, maar ze vaak wijst u rechtstreeks naar een probleem, zoals een failover-omgeving ineens dat u niet bereikbaar,.. of twee lagen zijn niet na de migratie van een wolk communiceren.  In de bovenstaande afbeelding IIS en WebSphere beide uitvoert, maar ze geen verbinding kunt maken. 

## <a name="computer-and-process-properties"></a>Proceseigenschappen van het en computer
Wanneer u navigeert door een toewijzing ADM, kunt u machines en processen om het nut van extra context bieden over hun eigenschappen te selecteren.  Machines bevatten informatie over DNS-naam, IPv4-adressen van processor en geheugen capaciteit, VM Type, besturingssysteemversie, laatste start opnieuw op tijd en de id's OMS en ADM agenten.

Procesdetails van het worden verzameld uit besturingssysteem metagegevens over het uitvoeren van processen, inclusief de procesnaam van het, Procesbeschrijving, de gebruikersnaam van de en domein (in Windows), bedrijfsnaam, productnaam, productversie, werkmap, opdrachtregel en Begintijd proces.

![Proceseigenschappen van het](media/operations-management-suite-application-dependency-monitor/process-properties.png)

Het deelvenster Overzicht van het proces biedt extra informatie over van dat proces connectivity, waaronder de afhankelijke poorten, binnenkomende en uitgaande verbindingen, maar is mislukt verbindingen. 

![Overzicht van het proces](media/operations-management-suite-application-dependency-monitor/process-summary.png)

## <a name="oms-change-tracking-integration"></a>OMS wijzigingen bijhouden integratie
De ADM-integratie met wijzigingen bijhouden is automatische als beide oplossingen zijn ingeschakeld en geconfigureerd in uw werkruimte OMS.

Het deelvenster van Machine overzicht wordt aangegeven of gebeurtenissen wijzigingen bijhouden tijdens het geselecteerde tijdsbereik op de geselecteerde computer zijn opgetreden.

![Deelvenster Overzicht van machine](media/operations-management-suite-application-dependency-monitor/machine-summary.png)

Het deelvenster Machine wijzigen bijhouden bevat een overzicht van alle wijzigingen, klikt u met de meest recente eerste, samen met een koppeling naar Inzoomen op Log zoeken voor meer informatie.
![Machine wijzigen bijhouden Configuratiescherm](media/operations-management-suite-application-dependency-monitor/machine-change-tracking.png)

Hier volgt een minder details weergave van de gebeurtenis configuratie wijzigen nadat u **weergeven in Log Analytics**hebt geselecteerd.
![Configuratie wijzigingsgebeurtenis](media/operations-management-suite-application-dependency-monitor/configuration-change-event.png)


## <a name="log-analytics-records"></a>Logboekrecords Analytics
ADM van computer en proces voorraadgegevens is beschikbaar voor [Zoeken](../log-analytics/log-analytics-log-searches.md) in Log Analytics.  Hiermee kan worden toegepast op scenario's, inclusief migratieplanning, capaciteitsanalyse, discovery and oplossen van ad-hoc prestatieproblemen gaat. 

Één record gegenereerd per uur voor elke unieke computer en proces naast records wanneer dat proces of de computer wordt gestart of op-aangehouden ADM.  Deze records hebt de eigenschappen in de volgende tabellen. 

Er zijn intern gegenereerde eigenschappen die u gebruiken kunt om unieke processen en computers te bepalen:

- PersistentKey_s uniek is gedefinieerd door de Procesconfiguratie, bijvoorbeeld opdrachtregel en gebruikers-id  Deze uniek is voor een bepaalde computer, maar kan worden herhaald al computers.
- ProcessId_s en ComputerId_s zijn globaal uniek in het model ADM.



### <a name="admcomputercl-records"></a>AdmComputer_CL records
Records met een soort **AdmComputer_CL** hebben voorraadgegevens voor servers met ADM agenten.  Deze records hebt de eigenschappen in de volgende tabel.  

| Eigenschap | Beschrijving |
|:--|:--|
| Type | *AdmComputer_CL* |
| SourceSystem | *OpsManager* |
| ComputerName_s | Naam van Windows of Linux-computer |
| CPUSpeed_d | CPU-snelheid in MHz |
| DnsNames_s | Lijst met alle DNS-namen voor deze computer |
| IPv4s_s | Lijst met alle IPv4-adressen door deze computer wordt gebruikt |
| IPv6s_s | Lijst met alle IPv6-adressen door deze computer gebruikt.  (ADM aangeeft van IPv6-adressen, maar wordt niet u IPv6 afhankelijkheden.) |
| Is64Bit_b | waar of onwaar op basis van het type besturingssysteem |
| MachineId_s | Een interne GUID, uniek zijn voor een werkruimte OMS  |
| OperatingSystemFamily_s | Windows- of Linux |
| OperatingSystemVersion_s | Lange OS versietekenreeks |
| TimeGenerated | Datum en tijd waarop de record is gemaakt. |
| TotalCPUs_d | Aantal CPU kernen |
| TotalPhysicalMemory_d | Geheugencapaciteit in MB |
| VirtualMachine_b | waar of ONWAAR, afhankelijk van of OS VM Gast |
| VirtualMachineID_g | Hyper-V VM-ID |
| VirtualMachineName_g | De naam van de Hyper-V VM |
| VirtualMachineType_s | Hyperv, Vmware, Xen, Kvm, Ldom, Lpar, Virtualpc |


### <a name="admprocesscl-type-records"></a>AdmProcess_CL Type records 
Records met een soort **AdmProcess_CL** hebben voorraadgegevens voor processen TCP-verbinding op servers met ADM agenten.  Deze records hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Type | *AdmProcess_CL* |
| SourceSystem | *OpsManager* |
| CommandLine_s | Volledige opdrachtregel van het proces |
| CompanyName_s | Bedrijfsnaam (vanuit Windows PE of Linux RPM) |
| Description_s | Beschrijving van de vergt wat werk (vanuit Windows PE of Linux RPM) |
| FileVersion_s | Uitvoerbaar bestandsversie (vanuit Windows PE, alleen Windows) |
| FirstPid_d | OS proces-ID |
| InternalName_s | Uitvoerbaar bestand interne naam (vanuit Windows PE, alleen Windows) |
| MachineId_s | Interne GUID uniek zijn voor een werkruimte OMS  |
| Name_s | De naam van het proces uitvoerbaar bestand |
| Path_s | Systeem bestandspad van het proces uitvoerbare bestand |
| PersistentKey_s | Interne GUID uniek zijn binnen deze computer |
| PoolId_d | Interne ID voor het verzamelen van processen op basis van een vergelijkbare opdracht lijnen. |
| ProcessId_s | Interne GUID uniek zijn voor een werkruimte OMS  |
| ProductName_s | Naam product (vanuit Windows PE of Linux RPM) |
| ProductVersion_s | Product versietekenreeks (vanuit Windows PE of Linux RPM) |
| StartTime_t | De begintijd proces op de lokale computerklok |
| TimeGenerated | Datum en tijd waarop de record is gemaakt. |
| UserDomain_s | Domein van proceseigenaar (alleen Windows) |
| UserName_s | Naam van proceseigenaar (alleen Windows) |
| WorkingDirectory_s | Proces-werkmap |


## <a name="sample-log-searches"></a>Voorbeeld log zoekopdrachten

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Lijst met de capaciteit fysiek geheugen van alle beheerde computers. 
Typ = AdmComputer_CL | Selecteer TotalPhysicalMemory_d, ComputerName_s | Dedup ComputerName_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-01.png)

### <a name="list-computer-name-dns-ip-and-os-version"></a>De naam van de lijst-computer, DNS, IP- en OS versie.
Typ = AdmComputer_CL | Selecteer ComputerName_s, OperatingSystemVersion_s, DnsNames_s, IPv4s_s | dedup ComputerName_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-02.png)

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Alle processen met 'sql' zoeken in de opdrachtregel
Typ = AdmProcess_CL CommandLine_s = \*sql\* | dedup ProcessId_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-03.png)

### <a name="after-viewing-event-data-for-given-process-use-its-machine-id-to-retrieve-the-computers-name"></a>Na het weergeven van gebeurtenisgegevens voor gegeven proces, de ID machine gebruiken om op te halen, de naam van de computer
Typ = AdmComputer_CL "m!m-9bb187fa-e522-5f73-66d2-211164dc4e2b" | DISTINCT ComputerName_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-04.png)

### <a name="list-all-computers-running-sql"></a>Lijst van alle computers met SQL
Type AdmComputer_CL MachineId_s IN = {Type = AdmProcess_CL \*sql\* | DISTINCT MachineId_s} | DISTINCT ComputerName_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-05.png)

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>Lijst met alle unieke productversies van krul in mijn datacenter
Typ = AdmProcess_CL Name_s = krul | DISTINCT ProductVersion_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-06.png)

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Een groep van de Computer van alle computers met CentOS maken

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-07.png)



## <a name="diagnostic-and-usage-data"></a>Diagnostic en het gebruik van gegevens
Microsoft verzamelt automatisch gegevens van het gebruik en prestaties tot en met het gebruik van de toepassing afhankelijkheid Monitor-service. Microsoft gebruikt deze gegevens om te bieden en de kwaliteit, de beveiliging en de integriteit van de toepassing afhankelijkheid Monitor-service te verbeteren. Gegevens bevat informatie over de configuratie van uw software zoals besturingssysteem en versie en bevat ook IP-adres, de naam van de DNS- en Werkstationnaam kan alleen worden aangeboden nauwkeurig en efficiënt probleemoplossing. Verzamelt namen, adressen of andere contactgegevens.

Voor meer informatie over het verzamelen van gegevens en het gebruik, raadpleegt u de [Privacyverklaring van Microsoft Online Services](hhttps://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [aanmelden zoekopdrachten](../log-analytics/log-analytics-log-searches.md] in Log Analytics to retrieve data collected by Application Dependency Monitor.)
