<properties 
   pageTitle="Bewaak uw StorSimple-apparaat | Microsoft Azure"
   description="Wordt beschreven hoe de service StorSimple Manager gebruiken om de i/o-prestaties, gebruik van capaciteit netwerkdoorvoer en Apparaatprestaties van het te houden."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>De service StorSimple Manager gebruiken om te controleren van uw apparaat StorSimple 

## <a name="overview"></a>Overzicht

U kunt de service StorSimple Manager gebruiken om de specifieke apparaten binnen uw oplossing StorSimple te houden. U kunt aangepaste grafieken op basis van de i/o-prestaties, gebruik van capaciteit netwerkdoorvoer en prestatiegegevens apparaat maken. 

Als u wilt de controlegegevens voor een specifiek apparaat, in de portal van Azure klassieke bekijkt, selecteert u de StorSimple Manager-service. Klik op het tabblad **Beeldscherm** en selecteer vervolgens in de lijst met apparaten. De pagina **Monitor** bevat de volgende informatie.

## <a name="io-performance"></a>O prestaties 

**O prestaties** nummers maatstaven gerelateerd aan het aantal lezen en schrijven bewerkingen tussen hetzij de iSCSI initiator-interfaces op de hostserver en het apparaat of het apparaat en de cloud. Deze prestaties kunt voor een specifiek volume, een container specifieke volume of alle volume containers worden gemeten.

In onderstaande tabel ziet u de I/O voor het beginpunt aan uw apparaat gebruiken voor de hoeveelheden voor een productie-apparaat. De aan de doelstellingen uitgezet worden opgelezen en bytes per seconde schrijven, lezen en schrijven IO bewerkingen per seconde, en lezen en schrijven vertragingstijden.

![IO prestaties van initiator naar apparaat](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Voor hetzelfde apparaat, worden de i/o-bewerkingen voor de gegevens van het apparaat naar de cloud voor alle volume-containers uitgezet. Klik op dit apparaat, de gegevens alleen in de lineaire laag is en niets heeft zich verspreid in de cloud. Er zijn geen alleen-lezen-bewerkingen optreedt van apparaat in de cloud. Daarom zijn de pieken in de grafiek op een interval van 5 minuten die overeenkomt met de frequentie waarop de heartbeat tussen het apparaat en de service is ingeschakeld. 

![IO prestaties van apparaat naar cloud](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)


Voor hetzelfde apparaat, een momentopname cloud is die u hebt gemaakt voor het van volumegegevens, beginnend bij 2:00 pm. Waardoor gegevens die van het apparaat in de cloud. Leest schrijft zijn served in de cloud in deze duur. De grafiek IO ziet u een piek in de diverse cijfers die overeenkomt met de tijd waarop de momentopname is gemaakt. 

![IO prestaties voor apparaat naar cloud na cloud momentopname](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)


## <a name="capacity-utilization"></a>Gebruik van de capaciteit 

**Gebruik van capaciteit** wordt bijgehouden gegevens met betrekking tot de hoeveelheid opslagruimte die wordt gebruikt door de volumes, het volume containers of het apparaat. U kunt rapporten op basis van het gebruik van de capaciteit van uw primaire opslag, uw cloudopslag of de opslag van uw apparaat maken. Gebruik van de capaciteit kan worden gemeten op een specifiek volume, een container specifieke volume of alle volume containers.


De primaire, cloud, en de opslagcapaciteit apparaat kunt als volgt worden beschreven:

###<a name="primary-storage-capacity-utilization"></a>Gebruik van de capaciteit primaire opslag
 
Deze grafieken weergeven de hoeveelheid gegevens die zijn geschreven StorSimple volume voordat de gegevens worden deduplicated en gecomprimeerd. U kunt het gebruik van de primaire opslagruimte door alle volumes of voor één volume bekijken.

Wanneer u de primaire opslag volume capaciteit gebruik grafieken voor alle volumes versus elk volume met het afzonderlijke bekijken en de primaire gegevens in beide gevallen deze totaliseren, er zijn mogelijk niet overeen tussen de twee getallen. De totale primaire gegevens op alle volumes mogelijk niet tot de totale som van de primaire gegevens van de afzonderlijke hoeveelheden toevoegen. Dit kan worden veroorzaakt door een van de volgende opties:

- **Momentopnamegegevens voor alle volumes**: dit probleem is zichtbaar voor alleen als u versie eerder dan Update 3 uitvoert. De primaire gegevens weergegeven voor alle de hoeveelheden is de som van de primaire gegevens voor elk volume en de momentopnamegegevens. De primaire gegevens weergegeven voor een bepaald volume komt overeen met alleen de hoeveelheid gegevens die zijn toegewezen op het volume (en de bijbehorende gegevens van de volume-momentopname bevat geen).

    Dit kan ook worden uitgelegd door de volgende vergelijking:

    *Primaire gegevens (alle volumes) = som (primaire gegevens (volume i) + grootte van momentopnamegegevens (volume i))*
    
    *waar, primaire gegevens (volume i) grootte van primaire gegevens die zijn toegewezen aan volume i =*
 
    Als de momentopnamen worden verwijderd via de service, wordt de verwijdering asynchroon op de achtergrond uitgevoerd. Het duurt enige tijd voor de grootte van het volume gegevens moeten worden bijgewerkt na de verwijdering momentopname. 

    Als met Update 3 of hoger, klikt u vervolgens de momentopnamegegevens niet opgenomen in de volumegegevens. En het gebruik van de primaire wordt als volgt berekend:

    * Primaire gegevens (alle volumes) = som (primaire gegevens (volume i)
    
    *waar, primaire gegevens (volume i) grootte van primaire gegevens die zijn toegewezen aan volume i =*
 
- **Met het bewaken uitgeschakeld deel uitmaakt in alle hoeveelheden**: hebt u volumes op uw apparaat waarvoor monitoring is uitgeschakeld, de controlegegevens voor deze afzonderlijke volumes is alleen beschikbaar in de grafieken. De gegevens voor enkel volume van de grafiek bevat echter de hoeveelheden waarvoor monitoring is uitgeschakeld. 
 
- **Verwijderde volumes met opgenomen voor alle volumes live gekoppeld back-ups**: als delen met momentopnamegegevens worden verwijderd, maar er zijn nog steeds de bijbehorende momentopnamen aanwezig en u niet overeen ziet mogelijk.

- **Voor alle volumes deel van uitmaakt met het verwijderde**: In sommige gevallen, oude volumes bestaat mogelijk ondanks dat deze zijn verwijderd. Het effect van verwijdering niet te zien en het apparaat lagere beschikbare capaciteit kan weergeven. Moet u contact op met Microsoft Support als u wilt deze volumes verwijderen.

De volgende grafieken weergeven in het gebruik van de capaciteit primaire opslag van een apparaat StorSimple voordat en nadat een momentopname cloud is gemaakt. Als dit uitsluitend volumegegevens is, moet de primaire opslag niet wijzigen door een momentopname van de cloud. Zoals u ziet, ziet u de grafiek geen verschil tussen het gebruik van de primaire capaciteit die het resultaat is een momentopname van de cloud. De cloud-momentopname starten at oplossing 2:00 pm op dat apparaat.

![Gebruik van de primaire capaciteit voordat cloud momentopname](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Gebruik van de primaire capaciteit na cloud momentopname](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Als u werkt met bijwerken van 2 of hoger, u kunt Splits op het gebruik van de capaciteit primaire opslag door een afzonderlijke volume, alle volumes, alle doorverbonden volumes en alle lokaal vastgemaakte volumes zoals hieronder wordt weergegeven. Door alle lokaal vastgemaakte volumes splitsen, kunt u snel nagaan hoe veel van de lokale laag omhoog wordt gebruikt.

![Gebruik van de primaire capaciteit voor alle lokaal vastgemaakte volumes](./media/storsimple-monitor-device/localvolumes.png)


###<a name="cloud-storage-capacity-utilization"></a>Gebruik van de capaciteit cloud opslag

Deze grafieken weergeven de hoeveelheid cloudopslag gebruikt. Deze gegevens is deduplicated en gecomprimeerd. Dit bedrag bevat cloud momentopnamen die mogelijk gegevens bevatten die niet wordt doorgevoerd in alle primaire volume en bijgehouden voor oude of vereiste bewaarbeleid doeleinden. U kunt de primaire vergelijken en cloud opslag verbruik van afbeeldingen als u een idee van de snelheid van de gegevens wilt verkleinen, hoewel het getal niet exacte. De volgende grafieken weergeven de cloud capaciteit gebruik van de opslagruimte van een apparaat StorSimple voordat en nadat een momentopname cloud is gemaakt. De cloud-momentopname de slag op oplossing 2:00 pm op dat apparaat en ziet u het gebruik van de capaciteit cloud opgenomen omhoog op hetzelfde moment, versie 4.04 GB met groter wordende van 5.73 MB.

![Gebruik van de capaciteit cloud voordat cloud momentopname](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Gebruik van de capaciteit cloud na cloud momentopname](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)


###<a name="device-storage-capacity-utilization"></a>Gebruik van de capaciteit apparaat opslag

Deze grafieken weergeven de totale gebruik voor het apparaat, die meer dan primaire opslag omdat deze de lineaire laag SSD bevat. In dit niveau bevat een hoeveelheid gegevens die ook aanwezig op het apparaat dat de andere niveaus. De capaciteit in de SSD lineaire laag volgens zodat wanneer nieuwe gegevens binnenkomt, de oude gegevens wordt verplaatst naar de harde schijf laag (waarop deze is deduplicated en gecomprimeerd) en vervolgens naar de cloud.

Na verloop van tijd verhoogt gebruik van primaire capaciteit en het gebruik van de capaciteit apparaat waarschijnlijk samen totdat de gegevens naar worden doorverbonden naar de cloud begint. Op dat moment wordt het gebruik van de capaciteit apparaat waarschijnlijk gaat bereikt, maar het gebruik van de primaire capaciteit, neemt zoals meer gegevens worden geschreven.

De volgende grafieken weergeven in het gebruik van de capaciteit primaire opslag van een apparaat StorSimple voordat en nadat een momentopname cloud is gemaakt. De cloud-momentopname begonnen at 2:00 pm en het gebruik van de capaciteit apparaat aflopende volgorde op dat moment. Het gebruik van de capaciteit van de apparaat opslagruimte gekomen omlaag van 11.58 GB 7.48 GB. Hiermee wordt aangeduid dat waarschijnlijk de niet-gecomprimeerde gegevens in de lineaire SSD laag is deduplicated, gecomprimeerd en naar de harde schijf laag verplaatst. Houd er rekening mee dat als het apparaat al een grote hoeveelheid gegevens in de SSD en de harde schijf lagen heeft, u niet deze verkleinen ziet mogelijk. In dit voorbeeld heeft het apparaat een kleine hoeveelheid gegevens.

![Gebruik van de capaciteit apparaat voordat cloud momentopname](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Gebruik van de capaciteit apparaat na cloud momentopname](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)


## <a name="network-throughput"></a>Netwerkdoorvoer

**Netwerkdoorvoer** wordt bijgehouden gegevens met betrekking tot de hoeveelheid gegevens die worden overgebracht van de iSCSI initiator netwerk-interfaces op de hostserver en het apparaat en tussen het apparaat en de cloud. U kunt deze metrisch voor elk van de iSCSI-netwerk-interfaces controleren op uw apparaat.

De volgende grafieken weergeven de netwerkdoorvoer voor de gegevensvelden 0 en de gegevens 4, beide 1 GbE netwerk-interfaces op uw apparaat. In dit exemplaar, is gegevens 0 cloud ingeschakelde terwijl gegevens 4 iSCSI is ingeschakeld was. U ziet het inkomende en uitgaand verkeer voor uw apparaat StorSimple. De platte lijn in de grafiek starten van 3:24 uur is ten gevolge van het feit dat we alleen elke 5 minuten voor gegevens verzamelen en moet worden genegeerd. 

![Netwerkdoorvoer voor Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Netwerkdoorvoer voor Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)


## <a name="device-performance"></a>Apparaatprestaties 

**Prestaties van het apparaat** wordt bijgehouden gegevens met betrekking tot de prestaties van uw apparaat. Het volgende diagram ziet u de Stat CPU-gebruik voor een apparaat in productie.

![CPU-gebruik voor apparaat](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [gebruik van het dashboard van StorSimple Manager service apparaat](storsimple-device-dashboard.md).

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
