<properties
   pageTitle="Het configureren van toepassing afhankelijkheid Monitor (ADM) in bewerkingen Management Suite (OMS) | Microsoft Azure"
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

# <a name="configuring-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Toepassing afhankelijkheid Monitor oplossing in bewerkingen Management Suite Kantoorbeheersysteem configureren
![Waarschuwingspictogram Management](media/operations-management-suite-application-dependency-monitor/icon.png) Toepassing afhankelijkheid Monitor (ADM) automatisch detecteert toepassingsonderdelen op Windows en Linux-systemen en toewijzingen van de communicatie tussen services. U kunt uw servers weergeven terwijl u deze persoon zien als overlappende systemen waarop kritieke services geven.  Toepassing afhankelijkheid Monitor ziet u verbindingen tussen servers, processen, en poorten over eventuele architectuur TCP-verbonden met geen configuratie vereist andere dan de installatie van een agent.

In dit artikel worden de details van het configureren van toepassing afhankelijkheid Monitor en onboarding agenten.  Zie voor informatie over het gebruik van ADM [gebruiken toepassing afhankelijkheid Monitor oplossing in bewerkingen Management Suite Kantoorbeheersysteem](operations-management-suite-application-dependency-monitor.md)

>[AZURE.NOTE]Toepassing afhankelijkheid Monitor is momenteel in de proefversie van privé.  U kunt toegang tot de ADM privé voorbeeldversie bij [https://aka.ms/getadm](https://aka.ms/getadm)aanvragen.
>
>In privé voorbeeldweergave, alle OMS-accounts zijn onbeperkte toegang tot ADM.  ADM knooppunten zijn gratis, maar Log Analytics-gegevens voor de typen AdmComputer_CL en AdmProcess_CL wordt als een andere oplossing worden gemeten.
>
>Nadat ADM openbare preview invoert, is dit is alleen beschikbaar voor gratis en betaalde klanten van inzicht & Analytics in het Kantoorbeheersysteem prijzen Plan.  Gratis laag accounts worden beperkt tot 5 ADM knooppunten.  Als u aan de privé preview deelneemt en niet worden geregistreerd in het Kantoorbeheersysteem prijzen Plan wanneer ADM openbare preview invoert, wordt op dat moment ADM uitgeschakeld. 



## <a name="connected-sources"></a>Verbonden bronnen
De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door de oplossing ADM.

| Verbonden bron | Ondersteund | Beschrijving |
|:--|:--|:--|
| [Windows-agenten](../log-analytics/log-analytics-windows-agents.md) | Ja | ADM analyseert en verzamelt gegevens van Windows-agentcomputers.  <br><br>Houd er rekening mee dat de Windows-agenten naast de OMS-agent, Microsoft afhankelijkheid Agent vereist.  Raadpleeg de [besturingssystemen worden ondersteund](#supported-operating-systems) voor een volledige lijst van besturingssystemen. |
| [Linux agenten](../log-analytics/log-analytics-linux-agents.md) | Ja | ADM analyseert en verzamelt gegevens van Linux agentcomputers.  <br><br>Houd er rekening mee dat de Linux agenten naast de OMS-agent, Microsoft afhankelijkheid Agent vereist.  Raadpleeg de [besturingssystemen worden ondersteund](#supported-operating-systems) voor een volledige lijst van besturingssystemen. |
| [SCOM management groep](../log-analytics/log-analytics-om-agents.md) | Ja | ADM analyseert en verzamelt gegevens van Windows en Linux agenten in een verbonden SCOM management-groep. <br><br>Een directe verbinding van de computer van de agent SCOM met OMS is vereist. Gegevens worden verzonden rechtstreeks vanuit uit de groep management wordt doorgestuurd naar de bibliotheek OMS.|
| [Azure opslag-account](../log-analytics/log-analytics-azure-storage.md) | Nee | ADM verzamelt gegevens van agentcomputers, dus is er geen gegevens te verzamelen van Azure opslag. |

Houd er rekening mee dat ADM biedt alleen ondersteuning voor 64-bits platforms.

In Windows, Microsoft Monitoring Agent (MMA) wordt gebruikt door SCOM en OMS verzamelen en verzenden-gegevens bewaken.  (Deze agent heet het SCOM Agent, OMS Agent, MMA of directe Agent, afhankelijk van de context.)  SCOM en OMS bieden verschillende afmelden bij de versies van het vak van MMA, maar deze versies kunnen elk rapport naar SCOM op OMS, of beide.  Klik op Linux, de OMS-Agent voor Linux worden verzameld en verzonden naar OMS-gegevens bewaken.  U kunt ADM op servers met OMS directe agenten of op de servers die zijn gekoppeld aan OMS via SCOM Management groepen.  Voor deze documentatie wordt we verwijzen naar alle van deze agenten – op Linux of Windows, of verbinding hebben met een MG SCOM of rechtstreeks naar OMS – als de "OMS Agent', tenzij de implementatie van specifieke naam van de agent voor context nodig is.

De ADM-agent stuurt geen gegevens zelf en hoeft niet alle wijzigingen in firewalls of poorten.  ADM van gegevens is altijd verzonden door de OMS-Agent OMS, rechtstreeks of via de Gateway OMS.

![ADM agenten](media/operations-management-suite-application-dependency-monitor/agents.png)

Als u een klant SCOM met een groep Management is verbonden met OMS:

- Als uw agenten SCOM toegang internet verbinding maken met OMS tot hebben, is geen extra configuratie vereist.  
- Als uw agenten SCOM geen OMS via internet openen, moet u voor het configureren van de Gateway OMS voor gebruik met SCOM.
  
Als u de directe OMS-Agent gebruikt, moet u de OMS Agent zelf verbinding maken met OMS of tot de Gateway OMS configureren.  De Gateway OMS kan worden gedownload van [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666)


### <a name="avoiding-duplicate-data"></a>Voorkomen van dubbele gegevens

Als u een SCOM-klant bent, moet u niet uw agenten SCOM om te communiceren rechtstreeks naar OMS configureren of gegevens tweemaal worden vermeld.  In ADM resulteert dit in computers tweemaal in de lijst Machine voorkomen.

Configuratie van OMS moet gebeuren in slechts een van de volgende locaties:

- Het deelvenster SCOM Console bewerkingen Management Suite voor beheerde Computers
- Configuratie van de Azure operationele inzicht krijgen in de eigenschappen van het MMA

Gebruik van beide configuraties met dezelfde werkruimte in elk treedt dubbele gegevens. Gebruik van beide met verschillende werkruimten kan resulteren in conflicterende configuratie (optie ADM oplossing is ingeschakeld en de andere zonder) die kan verhinderen dat gegevens die doorloopt naar ADM volledig.  

Houd er rekening mee dat zelfs als de computer zelf niet wordt opgegeven in de SCOM-Console OMS configuratie wordt als een groep exemplaar zoals "Windows Server exemplaren groep" actief is, dit nog steeds leiden de machine ontvangst OMS configuratie via SCOM tot kan.



## <a name="management-packs"></a>Management packs
Als ADM in een werkruimte OMS is geactiveerd, een 300KB Management Pack is verzonden naar alle de Microsoft Monitoring kunt vinden in die werkruimte.  Als u SCOM kunt vinden in een [verbonden management groep](../log-analytics/log-analytics-om-agents.md)gebruikt, wordt het ADM Management Pack geïmplementeerd vanuit SCOM.  Als de agenten rechtstreeks verbinding hebben, worden de MP door OMS afgeleverd.

Het Beheerderspaneel heet Microsoft.IntelligencePacks.ApplicationDependencyMonitor*.  Dit is geschreven naar *%Programfiles%\Microsoft Monitoring Agent\Agent\Health State\Management servicepacks\*.  De gegevensbron die wordt gebruikt door het management pack is *% Program files%\Microsoft controle Agent\Agent\Health Service State\Resources\<AutoGeneratedID > \Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.



## <a name="configuration"></a>Configuratie
Naast Windows en Linux computers hebben een agent geïnstalleerd en verbonden met OMS, het installatieprogramma van afhankelijkheid Agent moet worden gedownload van de oplossing ADM en klik vervolgens op elke beheerde server hebt geïnstalleerd als hoofdsite of beheerder.  Wanneer de ADM-agent is geïnstalleerd op een server die rapporteren aan OMS, wordt ADM afhankelijkheid kaarten in 10 minuten weergegeven.  Als u problemen hebt, neemt u via e-mail verzenden [oms-adm-support@microsoft.com](mailto:oms-adm-support@microsoft.com).


### <a name="migrating-from-bluestripe-factfinder"></a>Migreren van BlueStripe FactFinder
Toepassing afhankelijkheid Monitor leveren BlueStripe technologie in OMS in fasen. FactFinder nog steeds wordt ondersteund voor bestaande klanten, maar is niet langer beschikbaar voor afzonderlijke aanschaffen.  Deze preview-versie van de afhankelijkheid-Agent kan alleen communiceren met OMS.  Als u een bestaande FactFinder-klant bent, Identificeer u een reeks test servers voor ADM die niet door FactFinder worden beheerd. 

### <a name="download-the-dependency-agent"></a>Download de afhankelijkheid-Agent
Naast de Microsoft Management Agent MMA () en OMS Linux Agent die de verbinding tussen de computer en OMS bieden, moeten alle computers geanalyseerd door toepassing afhankelijkheid Monitor de afhankelijkheid-Agent is geïnstalleerd.  Op Linux, moet de OMS-Agent voor Linux voordat u de afhankelijkheid-Agent zijn geïnstalleerd. 

![Monitor met een afhankelijkheid tussen de tegel van toepassing](media/operations-management-suite-application-dependency-monitor/tile.png)

Om te kunnen de afhankelijkheid-Agent downloaden, klikt u op de **Oplossing configureren** in de tegel **Toepassing afhankelijkheid Monitor** openen van het blad **Afhankelijkheid Agent** .  Het blad afhankelijkheid Agent bevat koppelingen voor de Windows- en de agenten Linux. Klik op de link voor het downloaden van elke agent. Zie de volgende secties voor meer informatie over het installeren van de agent op andere systemen voor.

### <a name="install-the-dependency-agent"></a>De afhankelijkheid-Agent installeren

#### <a name="microsoft-windows"></a>Microsoft Windows
Installeren of verwijderen van de agent zijn beheerdersbevoegdheden vereist.

De afhankelijkheid-Agent is geïnstalleerd op Windows-computers met ADM-Agent-Windows.exe. Als u dit uitvoerbare bestand zonder opties uitvoert, wordt deze een wizard die u volgen kunt om de installatie interactief uitvoeren starten.  

Gebruik de volgende stappen voor het installeren van de afhankelijkheid-Agent op elke Windows-computer.

1.  Zorg ervoor dat de OMS-agent is geïnstalleerd volgens de instructies op verbinding maken met computers rechtstreeks naar OMS.
2.  Download de Windows-agent en met de volgende opdracht worden uitgevoerd.<br>*ADM-Agent-Windows.exe*
3.  Voer de wizard om de agent installeren.
4.  Als de afhankelijkheid-Agent kan niet worden gestart, schakelt u de logboeken voor gedetailleerde foutgegevens. De logboekmap zich Windows agenten, *C:\Program Files\BlueStripe\Collector\logs*. 

De Agent voor Windows, afhankelijkheid kan worden verwijderd door een beheerder via het Configuratiescherm.


#### <a name="linux"></a>Linux
Toegang tot de hoofdmap is vereist voor het installeren of de agent configureren.

De afhankelijkheid-Agent is geïnstalleerd op Linux computers met ADM-Agent-Linux64.bin, een shellscript met een automatisch wordt uitgepakt binair getal. U kunt het bestand met v of toevoegen machtigingen om het bestand zelf uitvoeren.
 
Gebruik de volgende stappen voor het installeren van de afhankelijkheid-Agent op elke Linux-computer.

1.  Controleer of de OMS-agent is geïnstalleerd volgens de instructies op [verzamelen en beheren van gegevens van Linux computers.  Dit moet zijn geïnstalleerd voordat u de Linux afhankelijkheid-Agent](https://technet.microsoft.com/library/mt622052.aspx).
2.  Installeer de Linux afhankelijkheid-agent als de hoofdmap van de volgende opdracht.<br>*v ADM-Agent-Linux64.bin*.
3.  Als de afhankelijkheid-Agent kan niet worden gestart, schakelt u de logboeken voor gedetailleerde foutgegevens. De logboekmap zich Linux agenten, */var/opt/microsoft/dependency-agent/log*.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>De afhankelijkheid-Agent op Linux verwijderen
Als u wilt verwijderen volledig de afhankelijkheid-Agent van Linux, moet u de agent zelf en de proxy dat automatisch wordt geïnstalleerd met de agent verwijderen.  U kunt beide met de volgende opdracht uit één verwijderen.

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>Installeren vanaf een opdrachtprompt
De vorige sectie bevat richtlijnen voor het installeren van de afhankelijkheid Monitor-agent met standaardopties.  De onderstaande secties bevatten instructies voor het installeren van de agent vanaf de opdrachtregel met aangepaste opties.

#### <a name="windows"></a>Windows
Met de opties in de onderstaande tabel kunt u de installatie uitvoeren vanaf de opdrachtregel. Voor een overzicht van de installatie vlaggen voert u het installatieprogramma met de /? markeren als volgt.

    ADM-Agent-Windows.exe /?

| Vlag | Beschrijving |
|:--|:--|
| /S | Een stille installatie zonder vragen gebruiker uitvoeren. |

Bestanden voor de Windows-Agent afhankelijkheid worden in *C:\Program Files\BlueStripe\Collector* standaard geplaatst.


#### <a name="linux"></a>Linux
Met de opties in de onderstaande tabel kunt u de installatie uitvoert. Voor een overzicht van de installatie vlaggen uitvoeren de installatie-toepassingen met de - help vlag als volgt.

    ADM-Agent-Linux64.bin -help

| Beschrijving van de vlag
|:--|:--|
| -s | Een stille installatie zonder vragen gebruiker uitvoeren. |
| --controleren | Hiermee wordt gecontroleerd machtigingen en besturingssysteem gebruikt, maar de agent kan niet worden geïnstalleerd. |

Bestanden voor de Agent afhankelijkheid worden in de volgende mappen geplaatst.

| Bestanden | Locatie |
|:--|:--|
| Core-bestanden | /usr/lib/bluestripe-collector |
| Logboekbestanden | /var/opt/Microsoft/Dependency-agent/log |
| Bestanden van de zoekconfiguratie | /etc/opt/Microsoft/Dependency-agent/config |
| Service uitvoerbare bestanden | /sbin/bluestripe-collector<br>/sbin/bluestripe-collector-Manager |
| Binaire opslagbestanden | /var/opt/Microsoft/Dependency-agent/Storage |



## <a name="troubleshooting"></a>Problemen oplossen
Als u problemen met toepassing afhankelijkheid beeldscherm ondervindt, kunt u informatie over probleemoplossing uit meerdere onderdelen met behulp van de volgende informatie verzamelen.

### <a name="windows-agents"></a>Windows-agenten

#### <a name="microsoft-dependency-agent"></a>Afhankelijkheid van Microsoft Agent
Analytische gegevens van de afhankelijkheid-Agent genereren, open een opdrachtpromptvenster als beheerder en het gebruik van de volgende opdracht CollectBluestripeData.vbs-script uitvoeren.  U kunt toevoegen de--markeren help voor het weergeven van extra opties.

    cd C:\Program Files\Bluestripe\Collector\scripts
    cscript CollectDependencyData.vbs

Het pakket van de gegevens ondersteuning is opgeslagen in de map % USERPROFILE % voor de huidige gebruiker.  U kunt het--bestand <filename> optie opslaan naar een andere locatie.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>Afhankelijkheid van Microsoft Agent Management Pack voor MMA
De afhankelijkheid Agent Management Pack wordt uitgevoerd in Microsoft Management Agent.  Deze gegevens worden ontvangen van de afhankelijkheid-Agent en stuurt het naar de ADM-cloudservice.
  
Controleer of het management pack is gedownload door de volgende stappen uit te voeren.

1.  Zoek naar een bestand met de naam van Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp in C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management servicepacks.  
2.  Als het bestand niet aanwezig is en de-agent is verbonden met een SCOM management-groep, controleert u of dat deze zijn geïmporteerd in SCOM door te schakelen Management Packs in de werkruimte voor beheer van de bewerkingen-Console.

Het Beheerderspaneel ADM schrijft gebeurtenissen naar het gebeurtenissenlogboek van Windows voor Operations Manager.  Het logboek kan worden [gezocht in OMS](../log-analytics/log-analytics-log-searches.md) via de system log-oplossing, waar u welke logboekbestanden uploaden kunt configureren.  Als gebeurtenissen voor foutopsporing zijn ingeschakeld, worden ze naar het gebeurtenissenlogboek van toepassing, met de gebeurtenisbron *AdmProxy*geschreven.

#### <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
Voor het traceren van diagnostische informatie verzamelen, open een opdrachtpromptvenster als beheerder en voer de volgende opdrachten: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

Traces worden naar c:\Windows\Logs\OpsMgrTrace geschreven.  U kunt de tracering met StopTracing.cmd stoppen.


### <a name="linux-agents"></a>Linux agenten

#### <a name="microsoft-dependency-agent"></a>Afhankelijkheid van Microsoft Agent
Analytische gegevens van de afhankelijkheid-Agent, meld u aan met een account met bevoegdheden voor sudo of hoofdmap genereren en voer de volgende opdracht.  U kunt toevoegen de--markeren help voor het weergeven van extra opties.

    /usr/lib/bluestripe-collector/scripts/collect-dependency-agent-data.sh

Het pakket van de gegevens ondersteuning is opgeslagen op /var/opt/microsoft/dependency-agent/log (als hoofdsite) onder van de Agent-installatiemap of naar de basismap van de gebruiker het script uitvoeren (als niet-hoofdsite).  U kunt het--bestand <filename> optie opslaan naar een andere locatie.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>Afhankelijkheid van Microsoft Agent Fluentd-invoegtoepassing voor Linux
De invoegtoepassing afhankelijkheid Agent Fluentd wordt uitgevoerd binnen de OMS Linux-Agent.  Deze gegevens worden ontvangen van de afhankelijkheid-Agent en stuurt het naar de ADM-cloudservice.  

Logboeken naar de volgende twee bestanden geschreven.

- /var/opt/Microsoft/omsagent/log/omsagent.log
- /var/log/messages

#### <a name="oms-agent-for-linux"></a>OMS Agent voor Linux
Een resource voor probleemoplossing voor Linux-servers verbinden met OMS vindt u hier: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md) 

De logboeken voor de OMS-Agent voor Linux zich bevinden in */var/opt/microsoft/omsagent/log/*.  

De logboeken voor omsconfig (agentconfiguratie) zich bevinden in */var/opt/microsoft/omsconfig/log/*.
 
Het logboek voor de componenten OMI en SCX die aan de doelstellingen prestatiegegevens bieden zich bevinden in */var/opt/omi/log/* en */var/opt/microsoft/scx/log*.


## <a name="data-collection"></a>Gegevens verzamelen
U kunt elke agent voor het verzenden van ongeveer 25MB per dag, afhankelijk van hoe complexe zijn op uw systeem afhankelijkheden verwachten.  ADM afhankelijkheid gegevens is verzonden door elke agent elke 15 seconden.  

De ADM-Agent wordt meestal gebruikt 0,1 systeem geheugen en 0,1% van CPU-systeem.


## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
De volgende secties worden de ondersteunde besturingssystemen voor de afhankelijkheid-Agent vermeld.   32-bits architecturen worden niet ondersteund voor een besturingssysteem.

### <a name="windows-server"></a>Windows Server
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows-bureaublad
- Opmerking: Windows 10 is nog niet ondersteund
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Rode rol Enterprise Linux-, CentOS Linux- of Oracle-Linux (met RHEL Kernel)
- Alleen standaard en SMP Linux kernel releases worden ondersteund.
- Niet-standaard kernel loslaat, zoals PAE en Xen, worden niet ondersteund voor een Linux-verdeling. Bijvoorbeeld: een systeem met de tekenreeks release van "2.6.16.21-0.8-xen" wordt niet ondersteund.
- Aangepaste kernels, inclusief compilaties van standaard kernels, worden niet ondersteund.
- Centos Plus kernel wordt niet ondersteund.
- Oracle Unbreakable Kernel (UEK) vindt u in een andere sectie hieronder.


#### <a name="red-hat-linux-7"></a>Rode rol Linux 7
| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |

#### <a name="red-hat-linux-6"></a>Rode rol Linux 6
| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6,7 | 2.6.32-573 |
| 6,8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Rode rol Linux 5
| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 5,8 | 2.6.18-308 |
| 5,9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411 |

#### <a name="oracle-enterprise-linux-w-unbreakable-kernel-uek"></a>Oracle Enterprise Linux met Unbreakable Kernel (UEK)

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 5,8 | Oracle 2.6.32-300 (UEK R1) |
| 5,9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Diagnostic en het gebruik van gegevens
Microsoft verzamelt automatisch gegevens van het gebruik en prestaties tot en met het gebruik van de toepassing afhankelijkheid Monitor-service. Microsoft gebruikt deze gegevens om te bieden en de kwaliteit, de beveiliging en de integriteit van de toepassing afhankelijkheid Monitor-service te verbeteren. Gegevens bevat informatie over de configuratie van uw software zoals besturingssysteem en versie en bevat ook IP-adres, de naam van de DNS- en Werkstationnaam kan alleen worden aangeboden nauwkeurig en efficiënt probleemoplossing. Verzamelt namen, adressen of andere contactgegevens.

Voor meer informatie over het verzamelen van gegevens en het gebruik, raadpleegt u de [Privacyverklaring van Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Volgende stappen
- Leer hoe eenmaal [toepassing afhankelijkheid beeldscherm gebruiken](operations-management-suite-application-dependency-monitor.md) is geïmplementeerd en geconfigureerd.
