<properties
    pageTitle="Containers oplossing in Log Analytics | Microsoft Azure"
    description="De oplossing Containers in Log Analytics helpt u bij het weergeven en beheren van uw Docker container hosts op één locatie."
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
    ms.date="10/10/2016"
    ms.author="banders"/>



# <a name="containers-preview-solution-log-analytics"></a>Containers (Preview) oplossing Log Analytics

In dit artikel wordt beschreven hoe instellen en gebruiken van de oplossing Containers in Log analyse, waarmee u kunt weergeven en beheren van uw Docker container hosts op één locatie. Docker is een software virtualization-systeem gebruikt om te maken van containers die software-implementatie om de IT-infrastructuur te automatiseren.

Met de oplossing, kunt u zien welke containers op de container-hosts worden uitgevoerd en welke afbeeldingen in de containers worden uitgevoerd. U kunt gedetailleerde controlegegevens met opdrachten gebruikt met containers weergeven. En, kunt u containers oplossen door te bekijken en gecentraliseerde logboeken zoeken zonder te hoeven Docker hosts op afstand weergeven. Containers die mogelijk veel ruis en in beslag nemen overtollige bronnen op een host, kunt u vinden. En u kunt gecentraliseerde CPU, geheugen, opslag, en gebruik en de prestaties netwerkgegevens voor containers weergeven.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing

Gebruik de volgende informatie voor het installeren en configureren van de oplossing.

De oplossing Containers toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).

Er zijn twee manieren installeren en gebruiken van Docker met OMS:

- Op Linux ondersteunde besturingssystemen, installeren en uitvoeren van Docker en vervolgens installeert en configureert OMS Agent voor Linux
- Op CoreOS, installeren en uitvoeren van Docker en configureer de OMSAgent om uit te voeren in een container

Lees de ondersteunde Docker en Linux besturingssystemen voor uw host container op [GitHub](https://github.com/Microsoft/OMS-docker).

>[AZURE.IMPORTANT] Docker moet worden uitgevoerd **voordat** u de [OMS Agent voor Linux](log-analytics-linux-agents.md) installeren op uw hosts container. Als u de agent voordat u installeert Docker al hebt geïnstalleerd, moet u de OMS-Agent voor Linux opnieuw te installeren. Zie voor meer informatie over Docker, de [Docker website](https://www.docker.com).

Moet u eerst de volgende instellingen op de container hosts geconfigureerd voordat u containers kunt controleren.

## <a name="configure-settings-for-the-linux-container-host"></a>Instellingen voor de host van de container Linux configureren

Nadat u Docker hebt geïnstalleerd, gebruikt u de volgende instellingen voor uw container host de agent voor gebruik met Docker configureren. CoreOS biedt geen ondersteuning van deze configuratiemethode.

### <a name="to-configure-settings-for-the-container-host---systemd-suse-opensuse-centos-7x-rhel-7x-and-ubuntu-15x-and-higher"></a>Instellingen configureren voor de container host - systemd (SUSE, openSUSE, CentOS 7.x, RHEL 7.x en Ubuntu 15.x en hoger)

1. Bewerk docker.service om toe te voegen het volgende:

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. $DOCKER toevoegen\_KIEST &quot;ExecStart = / usr/bin/docker daemon&quot; in uw bestand docker.service. Gebruik het volgende voorbeeld.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Start de Docker-service. Bijvoorbeeld:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-ubuntu-14x"></a>Instellingen configureren voor de container-host - Upstart (Ubuntu 14.x)

1. /Etc/default/docker bewerken en voer de volgende:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Sla het bestand en start de Docker en OMS-services.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>Instellingen voor de container host - Amazon Linux configureren

1. /Etc/sysconfig/docker bewerken en voer de volgende:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Sla het bestand en vervolgens de Docker-service opnieuw te starten.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>Instellingen voor CoreOS containers configureren

Nadat u Docker hebt geïnstalleerd, gebruikt u de volgende instellingen voor CoreOS Docker uitvoeren en maken van een container. U kunt een ondersteunde versie van Linux, met inbegrip van CoreOS, met deze configuratiemethode. U moet uw [OMS werkruimte-ID en van toetsen](log-analytics-linux-agents.md).

### <a name="to-use-oms-for-all-containers-with-coreos"></a>OMS gebruiken voor alle containers met CoreOS

- Start de container OMS die u wilt controleren. Wijzigen en gebruikt u het volgende voorbeeld.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Overstappen van het gebruik van een geïnstalleerde agent aan een in een container

Als u eerder hebt gebruikt de agent direct-geïnstalleerd en u wilt gebruiken in plaats daarvan een agent wordt uitgevoerd in een container, moet u eerst OMSAgent verwijderen. Zie de [stappen voor het installeren van de OMS-Agent voor Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md).

## <a name="containers-data-collection-details"></a>Containers siteverzameling detailgegevens

De oplossing Containers verzamelt verschillende maatstaven en log prestatiegegevens van container hosts en containers met behulp van de agenten OMS voor Linux die u hebt ingeschakeld en van OMSAgent uitgevoerd in containers.

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor Containers.

| platform | OMS Agent voor Linux | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Linux|![Ja](./media/log-analytics-containers/oms-bullet-green.png)|![Nee](./media/log-analytics-containers/oms-bullet-red.png)|![Nee](./media/log-analytics-containers/oms-bullet-red.png)|            ![Nee](./media/log-analytics-containers/oms-bullet-red.png)|![Nee](./media/log-analytics-containers/oms-bullet-red.png)| elke 3 minuten|


De volgende tabel ziet u voorbeelden van gegevenstypen die worden verzameld door de oplossing Containers:

| Gegevenstype | Velden |
| --- | --- |
| Prestaties voor hosts en containers | Computer, objectnaam, CounterName & #40; % processortijd, schijf leest MB, schijf schrijft MB, geheugen gebruik MB, netwerk Bytes ontvangen, netwerk verzenden Bytes Processor gebruik sec, netwerk & #41; tegenwaarde, TimeGenerated, itempad, SourceSystem |
| Container voorraad | TimeGenerated, de Computer, de containernaam, ContainerHostname, afbeelding, ImageTag, ContinerState, ExitCode, EnvironmentVar, opdracht, CreatedTime, StartedTime, FinishedTime, SourceSystem, IdContainer, ImageID |
| Container afbeelding voorraad | TimeGenerated, Computer, afbeelding, ImageTag, ImageSize, VirtualSize, actief is, is onderbroken, gestopt, is mislukt, SourceSystem, ImageID, TotalContainer |
| Container log | TimeGenerated, Computer, afbeeldings-ID, containernaam, LogEntrySource, LogEntry, SourceSystem, IdContainer |
| Logboek van de container-service | TimeGenerated, Computer, TimeOfCommand, afbeelding, opdracht, SourceSystem, IdContainer |

## <a name="monitor-containers"></a>Monitor containers

Nadat u de oplossing in de portal OMS is ingeschakeld hebt, ziet u de **Containers** tegel met samenvatting van de gegevens over uw container-hosts en de containers uitgevoerd in hosts.

![Tegel voor containers](./media/log-analytics-containers/containers-title.png)

De tegel ziet u een overzicht van hoeveel containers die u hebt in de omgeving en of ze bent is mislukt, uitgevoerd, of is gestopt.

### <a name="using-the-containers-dashboard"></a>Gebruik van het dashboard Containers

Klik op de tegel **Containers** . Hier ziet u weergaven die zijn gerangschikt op:

- Container gebeurtenissen
- Fouten
- Containers Status
- Container afbeelding voorraad
- Prestaties van processor en geheugen

Elke deelvenster in het dashboard is een visuele weergave van een zoekopdracht die wordt uitgevoerd op verzamelde gegevens.

![Dashboard voor containers](./media/log-analytics-containers/containers-dash01.png)

![Dashboard voor containers](./media/log-analytics-containers/containers-dash02.png)

Klik in het blad **Container Status** , klikt u op bovengedeelte, zoals hieronder wordt weergegeven.

![Containers status](./media/log-analytics-containers/containers-status.png)

Log zoeken met de informatie over de hosts en containers uitgevoerd in deze wordt geopend.

![Meld u aan zoeken voor containers](./media/log-analytics-containers/containers-log-search.png)

Van hieruit kunt u de query als u wilt wijzigen om de specifieke gegevens te zoeken waarin u bent geïnteresseerd bewerken. Zie voor meer informatie over Log zoekopdrachten, [Log zoekopdrachten in Log Analytics](log-analytics-log-searches.md).

U kunt bijvoorbeeld de query wijzigen zodat de gestopt containers in plaats van de actieve containers worden weergegeven door te **voeren** op **gestopt** in de query wijzigen.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Problemen met door te zoeken naar een container is mislukt

Als deze is afgesloten met een niet-nul afsluitcode, OMS een container gemarkeerd als **mislukt** . U kunt een overzicht van de fouten en fouten in de omgeving in het blad **Containers is mislukt** .

### <a name="to-find-failed-containers"></a>Is mislukt containers om te zoeken.

1. Klik op het blad **Container gebeurtenissen** .  
  ![containers gebeurtenissen](./media/log-analytics-containers/containers-events.png)
2. Log zoeken wordt geopend, met de status van containers, ongeveer als volgt uit.  
  ![containers staat](./media/log-analytics-containers/containers-container-state.png)
3. Klik vervolgens op de mislukte waarde om aanvullende informatie, zoals de grootte van afbeeldingen en het aantal gestopt en mislukte afbeeldingen weer te geven. Vouw **meer weergeven** om weer te geven van de afbeelding-ID.  
  ![mislukte containers](./media/log-analytics-containers/containers-state-failed.png)
4. Zoek vervolgens de container die wordt uitgevoerd in deze afbeelding. Typ de volgende handelingen uit in de query.
  `Type=ContainerInventory <ImageID>`Hiermee worden de logboeken weergegeven. U kunt schuiven om te zien van de container is mislukt.  
  ![mislukte containers](./media/log-analytics-containers/containers-failed04.png)


## <a name="search-logs-for-container-data"></a>Zoeken in Logboeken voor container gegevens

Als u een specifieke fout wilt oplossen bent, kan het handiger om te zien waar deze zich voordoet in uw omgeving. De volgende typen voor log kunt u query's om terug te keren de informatie die dat u wilt maken.

- **ContainerInventory** : Gebruik dit type als u informatie over de locatie van de container wilt, wat hun namen zijn en wat afbeeldingen ze uitvoert.
- **ContainerImageInventory** : Gebruik dit type wanneer u probeert om informatie te vinden ingedeeld op afbeelding en afbeelding informatie zoals afbeeldings-id's of tekengrootten kunnen bekijken.
- **ContainerLog** : Gebruik dit type als u wilt zoeken naar specifieke fout logboekgegevens en vermeldingen.
- **ContainerServiceLog** : Gebruik dit type wanneer u probeert om controle Audittrailinformatie voor de daemon Docker, zoals starten, stoppen, verwijderen of halen opdrachten te vinden.

### <a name="to-search-logs-for-container-data"></a>Logboeken voor container gegevens zoeken

- Kies een afbeelding waarvan u weet dat onlangs is mislukt en de foutenlogboeken voor het zoeken. Begin met de containernaam van een met die afbeelding met een zoekopdracht **ContainerInventory** zoeken. Bijvoorbeeld zoeken`Type=ContainerInventory ubuntu Failed`  
    ![Zoeken naar Ubuntu containers](./media/log-analytics-containers/search-ubuntu.png)

  Noteer de naam van de container naast **de naam**en zoek naar deze logboeken. In dit voorbeeld is het `Type=ContainerLog adoring_meitner`.

**Informatie over prestaties weergeven**

Wanneer u begint met het maken van query's, kan het handiger om te zien wat is het mogelijk eerst. Bijvoorbeeld alle prestatiegegevens, bekijkt u een brede query door te typen van de volgende query.

```
Type=Perf
```

![containers prestaties](./media/log-analytics-containers/containers-perf01.png)



U kunt dit zien in een meer grafische formulier wanneer u klikt op het woord **aan de doelstellingen** in de zoekresultaten.

![containers prestaties](./media/log-analytics-containers/containers-perf02.png)



U kunt het bereik van de prestatiegegevens die u aan een specifieke container ziet door de naam ervan aan de rechterkant van de query te typen.

```
Type=Perf <containerName>
```

Waarin de lijst met prestatiestatistieken ophalen die zijn verzameld voor een individuele container wordt weergegeven.

![containers prestaties](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Voorbeeld log zoekquery 's

Vaak is het handig om te maken van query's beginnen met een voorbeeld of twee en deze wilt aanpassen aan uw omgeving vervolgens te bewerken. Als uitgangspunt, kunt u experimenteren met het blad **Aantal aanzienlijke query's** om meer geavanceerde query's samen te stellen.

![Containers query 's](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Log zoekquery's opslaan

Query's op te slaan, is een standaard functie in Log Analytics. Door deze te slaan, hebt u items die u hebt gevonden handig handige voor toekomstig gebruik.

Nadat u een query die u nuttig zijn gemaakt, kunt u dit door te klikken op **Favorieten** boven aan de pagina Log opslaan. Vervolgens u kunt eenvoudig toegang tot deze later vanaf de pagina **Mijn Dashboard** .

## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) om gedetailleerde container gegevensrecords te bekijken.
