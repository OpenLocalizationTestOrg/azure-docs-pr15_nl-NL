<properties
   pageTitle="Gebruik het dashboard StorSimple Manager-apparaat | Microsoft Azure"
   description="Beschrijving van het dashboard van StorSimple Manager service apparaat en hoe u deze gebruiken om metrische opslaggegevens en verbonden initiators weergeven en het seriële getal en IQN zoeken."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-device-dashboard"></a>Gebruik het dashboard StorSimple Manager-apparaat

## <a name="overview"></a>Overzicht

Het dashboard van het apparaat StorSimple Manager biedt u een overzicht van gegevens voor een specifieke StorSimple-apparaat, in tegenstelling tot de servicedashboard, waarmee u informatie over alle apparaten opgenomen in uw Microsoft Azure StorSimple-oplossing.

![Apparaat dashboard-pagina](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Het dashboard bevat de volgende informatie:

- **Grafiekgebied** ziet u de relevante opslageenheden in het grafiekgebied boven aan het dashboard. U kunt aan de doelstellingen voor de totale primaire opslag (de hoeveelheid gegevens die zijn geschreven door hosts naar uw apparaat) en de totale cloudopslag gedurende een periode worden gebruikt door uw apparaat kunt weergeven in deze grafiek.

     In deze context *primaire opslag* verwijst naar de totale hoeveelheid gegevens die zijn geschreven door de host en kunnen worden opgedeeld door volumetype: *primaire gelaagde opslag* beide lokaal opgeslagen gegevens bevat en dat gegevens worden doorverbonden naar de cloud; *primaire lokaal vastgemaakt opslag* bevat alleen door de gegevens die lokaal zijn opgeslagen. *Cloudopslag*, is echter, een meting van de totale hoeveelheid gegevens die zijn opgeslagen in de cloud. Dit geldt ook voor trapsgewijze gegevens en back-ups. Houd er rekening mee dat de gegevens die zijn opgeslagen in de cloud is deduplicated en gecomprimeerd, terwijl de primaire opslag geeft aan de hoeveelheid opslagruimte gebruikt voordat de gegevens is deduplicated en gecomprimeerd. (U kunt deze twee getallen als u een idee van het tarief weer dat compressie vergelijken.) Voor beide primair en cloudopslag in de, de bedragen, worden gebaseerd op de bijhouden frequentie die u wilt configureren. Bijvoorbeeld als u een frequentie één week hebt gekozen, worden klikt u vervolgens de grafiek gegevens weergegeven voor elke dag in de vorige week.

     U kunt het diagram als volgt configureren:

     - Als u wilt zien van de hoeveelheid cloudopslag verbruikt na verloop van tijd, de optie **CLOUD opslag gebruikt** . Als u wilt zien van de totale opslagcapaciteit die is geschreven door de host, selecteert u de opties voor **Primaire DOORVERBONDEN opslag gebruikt** en **Primaire lokaal VASTGEMAAKT opslag gebruikt** . In de afbeelding, worden beide opties zijn geselecteerd. de grafiek bevat daarom opslag bedragen voor cloud zowel primaire opslag. Houd er rekening mee dat elke primaire opslag gebruikt vóór het installeren van de Update 2 wordt voorgesteld door de regel **Primaire DOORVERBONDEN opslag gebruikt** .
     - Gebruik het vervolgkeuzemenu in de rechterbovenhoek van de grafiek naar een 1 week, 1 maand, 3-maand of 1 jaar tijdsperiode opgeven. Houd er rekening mee dat de grafiek op het hoogste niveau vernieuwd slechts één keer per dag wordt, en daarom worden doorgevoerd van de vorige dag totalen.

     Zie [de StorSimple Manager-service te controleren van uw apparaat StorSimple gebruiken](storsimple-monitor-device.md)voor meer informatie.

- **Gebruik overzicht** – In het gebied **Gebruik overview** , ziet u de hoeveelheid primaire opslagruimte gebruikt, de hoeveelheid opslagruimte ingerichte en de maximale opslagcapaciteit voor uw apparaat. Door het vergelijken van deze getallen gebruik aan de maximale hoeveelheid opslagruimte die beschikbaar is, kunt u in één oogopslag zien als u moet extra opslagruimte verkrijgen. Houd er rekening mee dat dit overzicht elke 15 minuten wordt bijgewerkt en, vanwege het verschil in updatefrequentie, verschillende getallen dan die wordt weergegeven in het grafiekgebied bovenstaande vertonen mogelijk, dagelijks wordt bijgewerkt. Zie [de StorSimple Manager-service te controleren van uw apparaat StorSimple gebruiken](storsimple-monitor-device.md)voor meer informatie.


- **Waarschuwingen** – het gebied **waarschuwingen** bevat een overzicht van de meldingen voor uw apparaat. Waarschuwingen zijn gegroepeerd op ernst en een telling van het aantal waarschuwingen voor elk niveau ernst is opgegeven. Een bereik weergave van het tabblad waarschuwingen om aan te geven alleen de waarschuwingen van deze prioriteitsniveau voor dit apparaat te klikken op de waarschuwing ernst worden geopend.

- **Taken** – het gebied **taken** ziet u het resultaat van recente taak activiteit. Hiermee kunt u weet dan zeker dat het systeem werkt zoals verwacht, of u weet kunt dat u moet corrigerende maatregelen nemen. Als u meer informatie over onlangs voltooide taken, klikt u op **taken is voltooid in de afgelopen 24 uur**.

- Het gebied **snel aan te duiden** aan de rechterkant van het dashboard bevat nuttige informatie zoals Apparaatmodel, geeft het seriële getal, status, beschrijving en aantal volume.

U kunt ook failover configureren en weergeven van verbonden initiators vanuit het dashboard apparaat.

Algemene taken die kunnen worden uitgevoerd op deze pagina zijn:

- Verbonden initiators weergeven

- Het seriële getal van apparaat zoeken

- Het apparaat doel IQN zoeken

## <a name="view-connected-initiators"></a>Verbonden initiators weergeven

U kunt de iSCSI-initiators die zijn gekoppeld aan uw apparaat door te klikken op de koppeling **weergave verbonden initiators** in het gebied **snel aan te duiden** van uw apparaat dashboard weergeven. Deze pagina bevat een tabelvormige vermelding van de initiators die verbinding met uw apparaat hebt gemaakt. Voor elke initiator, kunt u zien:

- De iSCSI Qualified Name (IQN) van het verbonden beginpunt.

- De naam van de access besturingselement record (ACR) waarmee deze verbonden initiator.

- Het IP-adres van het verbonden beginpunt.

- De netwerkinterfaces die het beginpunt is gekoppeld aan op uw opslagapparaat. Deze tussen gegevens 0 tot en met 5 van gegevens.

- Alle de hoeveelheden dat het beginpunt verbonden is toegestaan voor toegang tot op basis van de huidige ACR-configuratie.

Als u onverwachte initiators in deze lijst te zien of de verwachte resultaten niet wordt weergegeven, raadpleegt u uw configuratie ACR. Maximaal 512 initiators kunt aansluiten op uw apparaat.

## <a name="find-the-device-serial-number"></a>Het seriële getal van apparaat zoeken

Mogelijk moet u het seriële getal van apparaat wanneer u Microsoft paden i/o-(MPIO) op het apparaat configureren. De volgende stappen om te zoeken het seriële getal van apparaat uitvoeren.

#### <a name="to-find-the-device-serial-number"></a>Het seriële getal van apparaat zoeken

1. Navigeer naar **apparaten** > **Dashboard**.

2. Ga naar het gedeelte **snel aan te duiden** in het rechterdeelvenster van het dashboard.

3. Schuif omlaag en zoek naar het seriële getal.

## <a name="find-the-device-target-iqn"></a>Het apparaat doel IQN zoeken

Mogelijk moet u het apparaat doel IQN wanneer u de vraag CHAP Handshake Authentication Protocol () op uw apparaat StorSimple configureren. De volgende stappen om te zoeken van het apparaat doel IQN uitvoeren.

### <a name="to-find-the-device-target-iqn"></a>Het apparaat doel IQN zoeken

1. Navigeer naar **apparaten** > **Dashboard**.

1. Ga naar het gedeelte **snel aan te duiden** in het rechterdeelvenster van het dashboard.

1. Schuif omlaag en zoek naar het doel IQN.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [servicedashboard van StorSimple Manager](storsimple-service-dashboard.md).
- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
