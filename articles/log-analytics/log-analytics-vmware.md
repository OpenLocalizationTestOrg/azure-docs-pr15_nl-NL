<properties
    pageTitle="Oplossing VMware cmdlets voor controle in Log Analytics | Microsoft Azure"
    description="Meer informatie over de manier waarop de oplossing VMware Monitoring kunt logboeken beheren en controleren van ESXi hosts."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="banders"/>

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Oplossing in Log Analytics VMware Monitoring (Preview)

De oplossing VMware cmdlets voor controle in Log Analytics is een oplossing die helpt bij het maken van een gecentraliseerde logboekregistratie en controle methode voor grote VMware Logboeken. In dit artikel wordt beschreven hoe u kunt oplossen, vastleggen en beheren van de ESXi-hosts op één locatie met de oplossing. Met de oplossing ziet u gedetailleerde gegevens voor uw ESXi-hosts op één locatie. U kunt zien bovenste gebeurtenis telt, status en trends VM en ESXi hosts via de logboeken aan de host ESXi. U kunt oplossen door te bekijken en gecentraliseerde ESXi host logboeken zoeken. En, kunt u de waarschuwingen op basis van log zoekquery's maken.

De oplossing systeemeigen syslog functionaliteit van de host ESXi met push-gegevens naar een doelsite VM, waarvoor OMS Agent gebruikt. Echter schrijven niet de oplossing bestanden naar syslog binnen het doel VM. De OMS-agent poort 1514 wordt geopend en luistert als volgt. Nadat u deze ontvangt de gegevens, worden de OMS-agent de gegevens in OMS.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing

Gebruik de volgende informatie voor het installeren en configureren van de oplossing.

- De oplossing VMware cmdlets voor controle toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>Ondersteunde VMware ESXi hosts
vSphere ESXi Host 5.5 en 6.0

#### <a name="prepare-a-linux-server"></a>Een server Linux voorbereiden
Maak een Linux-besturingssysteem VM syslog zorgen dat alle gegevens ontvangen van de ESXi-hosts. De [OMS Linux-Agent](log-analytics-linux-agents.md) is het siteverzameling voor alle ESXi host syslog gegevens. U kunt meerdere ESXi hosts gebruiken om door te sturen logboeken met één Linux-server, zoals in het volgende voorbeeld.  

   ![Syslog-mailstroom](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Syslog siteverzameling configureren

1. Stel syslog doorsturen voor VSphere in. Zie voor gedetailleerde informatie over syslog doorsturen instellen, [syslog configureren op ESXi 5.x en 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Ga naar **ESXi hostconfiguratie** > **Software** > **Geavanceerde instellingen** > **Syslog**.
  ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  

2. Voeg uw Linux-server en het nummer van de poort *1514*in het veld *Syslog.global.logHost* . Bijvoorbeeld `tcp://hostname:1514` of`tcp://123.456.789.101:1514`

3. Open de firewall ESXi host voor syslog. **Hostconfiguratie ESXi** > **Software** > **Beveiligingsprofiel** > **Firewall** uit en open **Eigenschappen**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  

4. Controleer de vSphere Console om te bevestigen dat deze syslog correct is ingesteld. Controleer op de host ESXI die poort **1514** is geconfigureerd.

5. De connectiviteit tussen de Linux-server en de host ESXi testen met behulp van de `nc` opdracht op de ESXi-Host. Bijvoorbeeld:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

6. Download en installeer de OMS-Agent voor Linux op de server Linux. Zie de [documentatie van OMS Agent voor Linux](https://github.com/Microsoft/OMS-Agent-for-Linux)voor meer informatie.

7. Nadat de OMS-Agent voor Linux is geïnstalleerd, gaat u naar de map /etc/opt/microsoft/omsagent/sysconf/omsagent.d en kopieert u het bestand vmware_esxi.conf op de map /etc/opt/microsoft/omsagent/conf/omsagent.d en de wijziging van de eigenaar/groep en de machtigingen van het bestand. Bijvoorbeeld:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```

8.  De OMS-Agent voor Linux opnieuw starten door te voeren `sudo /opt/microsoft/omsagent/bin/service_control restart`.

9. Klik in de Portal OMS doorzoeken log voor `Type=VMware_CL`. OMS verzamelt de syslog-gegevens, de syslog opmaak behouden. Klik in de portal worden sommige bepaalde velden vastgelegd, zoals *Hostname* en *Procesnaam*.  

    ![type](./media/log-analytics-vmware/type.png)  

    Als uw zoekresultaten voor weergave log vergelijkbaar met de bovenstaande afbeelding zijn, bent u instellen voor het gebruik van de oplossing OMS VMware Monitoring dashboard.  

## <a name="vmware-data-collection-details"></a>Detailgegevens voor siteverzameling VMware

De oplossing VMware Monitoring verzamelt verschillende maatstaven en log prestatiegegevens van ESXi hosts met behulp van de agenten OMS voor Linux die u hebt ingeschakeld.

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld.

| platform | OMS Agent voor Linux | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Linux|![Ja](./media/log-analytics-vmware/oms-bullet-green.png)|![Nee](./media/log-analytics-vmware/oms-bullet-red.png)|![Nee](./media/log-analytics-vmware/oms-bullet-red.png)|            ![Nee](./media/log-analytics-containers/oms-bullet-red.png)|![Nee](./media/log-analytics-vmware/oms-bullet-red.png)| elke 3 minuten|


De volgende tabel ziet u voorbeelden van gegevensvelden die worden verzameld door de oplossing VMware controleren:

| Veldnaam | Beschrijving |
| --- | --- |
| Device_s| VMware opslagapparaten |
| ESXIFailure_s | Fout bij typen |
| EventTime_t | tijd waarop de gebeurtenis heeft plaatsgevonden |
| HostName_s | Hostnaam ESXi |
| Operation_s | VM maken of verwijderen van VM |
| ProcessName_s | de gebeurtenisnaam van de |
| ResourceId_s | naam van de host VMware |
| ResourceLocation_s | VMware |
| ResourceName_s | VMware |
| ResourceType_s | Hyper-V |
| SCSIStatus_s | VMware SCSI-status |
| SyslogMessage_s | Syslog gegevens |
| UserName_s | gebruiker die gemaakt of verwijderd VM |
| VMName_s | VM naam |
| Computer | hostcomputer |
| TimeGenerated | de gegevens is gegenereerd tijd |
| DataCenter_s | VMware datacenter |
| StorageLatency_s | opslag latentie ([ms) |

## <a name="vmware-monitoring-solution-overview"></a>Overzicht van oplossing VMware bewaken

De tegel VMware wordt weergegeven in de portal OMS. Dit wordt een globaal overzicht van eventuele fouten. Wanneer u op de tegel hebt geklikt, gaat u naar de dashboardweergave van een.

![tegel](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>De dashboardweergave navigeren

In de weergave van de dashboard **VMware** bladen geordend op:

- Fout bij Status tellen
- Telt de bovenste Host door gebeurtenis
- Bovenste gebeurtenis telt
- VM activiteiten
- ESXi Host schijf gebeurtenissen


![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Klik op een blade als u wilt openen, deelvenster Direct zoeken Log Analytics die specifiek zijn voor het blad ziet u gedetailleerde informatie.

Van hieruit kunt u de zoekopdracht om dit te wijzigen voor een bepaald bewerken. Voor een zelfstudie over de basisbeginselen van OMS zoeken, raadpleegt u de [OMS log zoeken zelfstudie.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>ESXi host gebeurtenissen zoeken

Één ESXi host genereert meerdere logboeken, op basis van hun processen. De oplossing VMware Monitoring zijn ze gecentraliseerd en bevat een overzicht van de gebeurtenis-tellingen komen. Deze gecentraliseerde weergave kunt u meer informatie over welke host ESXi heeft een groot aantal gebeurtenissen en welke gebeurtenissen meest in uw omgeving.

![gebeurtenis](./media/log-analytics-vmware/events.png)

U kunt inzoomen verdere door een ESXi-host of een gebeurtenistype te klikken.

Wanneer u op de naam van een ESXi-host, kunt u informatie uit die host ESXi weergeven. Als u wilt en resultaten met het gebeurtenistype te beperken, voegt u `“ProcessName_s=EVENT TYPE”` in uw query. U kunt **Procesnaam** selecteren in het zoekfilter. Die de gegevens voor u wordt beperkt.

![meer details](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Hoge VM activiteiten zoeken

Een virtuele machine kan worden gemaakt en op een willekeurige host ESXi verwijderd. Het is nuttig als beheerder aan te geven hoeveel VMs Hiermee maakt u een ESXi-host. Die in omslaan, kunt u meer informatie over prestaties en capaciteit plannen. Het bijhouden van VM activiteitsgebeurtenissen is essentieel bij het beheren van uw omgeving.

![meer details](./media/log-analytics-vmware/vmactivities1.png)

Als u zien van extra ESXi host VM maken van gegevens wilt, klikt u op de naam van een ESXi-host.

![meer details](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Algemene zoekquery 's

De oplossing bevat andere handige query's die u kunt uw hosts ESXi, zoals hoog opslagruimte, opslagruimte latentie en pad mislukt beheren.

![query 's](./media/log-analytics-vmware/queries.png)

#### <a name="save-queries"></a>Query's opslaan

Zoekopdrachten op te slaan is een standaard-functie in OMS en kunt u query's die u hebt gevonden handig behouden. Nadat u een query die u nuttig zijn gemaakt, kunt u dit door te klikken op de **Favorieten**opslaan. Een opgeslagen query kunt u eenvoudig deze later opnieuw gebruiken van de pagina [Mijn Dashboard](log-analytics-dashboards.md) waarop u uw eigen aangepaste dashboards kunt maken.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Waarschuwingen maken van query 's

Nadat u uw query's hebt gemaakt, wilt u mogelijk de query's gebruiken om u te waarschuwen bij bepaalde gebeurtenissen. Zie [waarschuwingen in Log Analytics](log-analytics-alerts.md) voor informatie over het maken van waarschuwingen. Zie de [Monitor VMware door middel van OMS Log analyses](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) blogbericht voor voorbeelden van waarschuwingen van query's en andere voorbeelden van query's.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Wat moet ik doen op de ESXi hosten instelling? Welke invloed heeft het op mijn huidige omgeving?
De oplossing maakt gebruik van de systeemeigen ESXi Host Syslog om doorsturen. U kunt geen extra Microsoft-software op de ESXi Host om vast te leggen de logboeken niet nodig hebt. Deze moet een lage gevolgen voor uw bestaande omgeving hebben. U hoeft echter syslog doorsturen, dat wil ESXI functionaliteit zeggen instellen.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Heb ik nodig om de host van mijn ESXi opnieuw te starten?
Nee. Dit proces is geen een herstart vereist. Soms kan werkt vSphere niet goed het syslog. Meld u aan bij de host ESXi in dat geval en de syslog laden. U moet opnieuw, niet opnieuw opstarten de host, zodat dit proces is niet storend voor uw omgeving.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Kan ik vergroten of verkleinen van het volume van logboekgegevens verzonden naar OMS?
U kunt Ja. U kunt de instellingen ESXi Host logboekniveau gebruiken in vSphere. Log siteverzameling is gebaseerd op het niveau van de *info* . Ja, als u controleren VM maken of verwijderen wilt, moet u het niveau van de *info* op Hostd behouden. Zie de [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658)voor meer informatie.

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Waarom Hostd verleent geen gegevens naar OMS? Mijn log is ingesteld op info.
Er is een fout ESXi host voor de syslog tijdstempel. Zie de [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202)voor meer informatie. Nadat u de tijdelijke oplossing hebt toegepast, moeten Hostd normaal functioneren.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Kan ik meerdere ESXi hosts syslog gegevens worden doorgestuurd naar een enkel VM met omsagent hebben?
Ja. U kunt meerdere ESXi hosts doorsturen naar een enkel VM met omsagent hebben.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Waarom zie ik niet gegevens die doorloopt in OMS?

Er zijn verschillende redenen:

- De host ESXi is niet juist zet nieuwe gegevens voor VM omsagent uitgevoerd. Als u wilt testen, moet u de volgende stappen uitvoeren:
    1. Meld u aan bij de ESXi host met ssh om te bevestigen, en voer de volgende opdracht:`nc -z ipaddressofVM 1514`

        Als dit niet lukt, vSphere-instellingen in de geavanceerde configuratie hebben waarschijnlijk niet worden gecorrigeerd. Zie [configureren syslog siteverzameling](#configure-syslog-collection) voor informatie over het instellen van de host ESXi voor syslog doorsturen.

    2. Als syslog poort connectiviteit geslaagd is, maar u nog steeds geen gegevens niet ziet, klikt u vervolgens opnieuw laden de syslog op de host ESXi met ssh de volgende opdracht uit te voeren:` esxcli system syslog reload`

- De VM met OMS-Agent is niet juist ingesteld. U kunt dit testen, moet u de volgende stappen uitvoeren:
    1. OMS luistert naar de poort 1514 en worden gegevens naar OMS. Om te bevestigen dat dit geopend is, voert u de volgende opdracht uit:`netstat -a | grep 1514`
    2. U ziet nu poort `1514/tcp` openen. Als u niet het geval is, controleert u of de omsagent juist is geïnstalleerd. Als u de poortgegevens niet ziet, klikt u vervolgens de syslog poort is niet geopend op de VM.
        1. Controleer of de OMS-Agent is uitgevoerd met behulp van `ps -ef | grep oms`. Als deze niet wordt uitgevoerd, start u het proces door de opdracht uit te voeren` sudo /opt/microsoft/omsagent/bin/service_control start`
        2. Open de `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` bestand.

            Controleer of de juiste gebruiker en de instelling voor groep geldig, vergelijkbaar met:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

            Als het bestand niet bestaat of als de gebruiker en de instelling voor groep is gegaan, moet u corrigerende actie door het [voorbereiden van een Linux-server](#prepare-a-linux-server).

## <a name="next-steps"></a>Volgende stappen

- Gebruik [Zoekopdrachten in het logboek](log-analytics-log-searches.md) in Log Analytics gedetailleerde VMware hostgegevens moeten worden weergegeven.
- [Uw eigen dashboards maken](log-analytics-dashboards.md) met VMware hostgegevens.
- [Waarschuwingen maken](log-analytics-alerts.md) wanneer specifieke VMware host gebeurtenissen plaatsvinden.
