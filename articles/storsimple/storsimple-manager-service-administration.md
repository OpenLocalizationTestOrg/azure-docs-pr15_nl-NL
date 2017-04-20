<properties 
   pageTitle="-StorSimple servicebeheer | Microsoft Azure"
   description="Informatie over het beheren van uw apparaat StorSimple met behulp van de StorSimple Manager-service in de portal van Azure klassieke."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>Gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple

## <a name="overview"></a>Overzicht

In dit artikel worden de StorSimple Manager-interface, waaronder over het verbinding maken met deze, de verschillende beschikbare opties en koppelingen uit naar de specifieke werkstromen die kunnen worden uitgevoerd via deze gebruikersinterface. Deze informatie is van toepassing op beide; de fysieke StorSimple en het virtuele apparaat.

Lees dit artikel en leert u hoe u kunt:

- Verbinding maken met StorSimple Manager-service
- De gebruikersinterface van de Manager StorSimple navigeren
- Uw apparaat StorSimple via de StorSimple Manager-service beheren


## <a name="connect-to-storsimple-manager-service"></a>Verbinding maken met StorSimple Manager-service

De StorSimple Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple-apparaten. U kunt een centrale Microsoft Azure klassieke portal wordt uitgevoerd in een browser gebruiken om deze apparaten te beheren. Ga als volgt te werk als u wilt verbinden met de service StorSimple Manager.

#### <a name="to-connect-to-the-service"></a>Verbinding maken met de service

1. Navigeer naar [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

1. De referenties van uw Microsoft-account gebruikt, meld u aan bij de Microsoft Azure klassieke-portal (te vinden op de rechtsboven in het deelvenster).

1. Schuif omlaag in het linkernavigatiedeelvenster voor toegang tot de StorSimple Manager-service.


## <a name="navigate-storsimple-manager-service-ui"></a>StorSimple Manager-service UI navigeren

De navigatie hiërarchie voor de service StorSimple Manager UI wordt weergegeven in de volgende tabel.

- Openingspagina van **StorSimple Manager** gaat u naar de pagina service level UI van toepassing op alle apparaten binnen een service's.

- Pagina **apparaten** gaat u naar de toepassing op een specifiek apparaat apparaatniveau – UI's.

- **Volume Containers** pagina gaat u naar de pagina van het volume waarop u alle de hoeveelheden die is gekoppeld aan een apparaat.


#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple Manager service navigatie hiërarchie

|Dialoogvenster aantekening toevoegen|Pagina's op service-niveau|Apparaatniveau pagina 's|Apparaatniveau pagina 's|
|---|---|---|---|
|StorSimple Manager-service|Service-dashboard|Apparaat-dashboard||
||Apparaten →|Monitor|
||Back-catalogus|Volume containers→|Volumes|
||(Service) configureren|Beleid voor het back-up||
||Taken|(Apparaat) configureren|
||Waarschuwingen|Onderhoud|

![Video beschikbaar](./media/storsimple-manager-service-administration/Video_icon.png) **Video beschikbaar**

Klik op een video die u u bij de gebruikersinterface van StorSimple Manager service begeleidt bekijken [hier](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>StorSimple apparaat met StorSimple Manager-service beheren

De volgende tabel bevat een overzicht van alle veelvoorkomende beheertaken en complexe werkstromen die kunnen worden uitgevoerd binnen de gebruikersinterface van de StorSimple Manager-service. Deze taken zijn gerangschikt op basis van de UI-pagina's waarop ze zijn gestart.

Voor meer informatie over elke werkstroom, klikt u op de juiste procedure in de tabel.

#### <a name="storsimple-manager-workflows"></a>StorSimple Manager werkstromen

|Als u dit wilt doen...|Ga naar deze pagina UI...|Gebruik deze procedure.|
|---|---|---|
|Een service maken</br>Een service verwijderen</br>Service registratiegegevens sleutel ophalen</br>Service registratiegegevens sleutel genereren|StorSimple Manager-service|[Een service StorSimple Manager implementeren](storsimple-manage-service.md)
|De service gegevens versleutelingssleutel wijzigen</br>De logboeken aan de bewerking weergeven|StorSimple Manager service → Dashboard|[Gebruik het servicedashboard StorSimple Manager](storsimple-service-dashboard.md)|
|Een apparaat deactiveren</br>Een apparaat verwijderen|StorSimple Manager service → apparaten|[Deactiveren of verwijderen van een apparaat](storsimple-deactivate-and-delete-device.md)|
|Meer informatie over noodgevallen herstel en apparaat failover</br>Failover naar een fysiek apparaat</br>Failover naar een virtueel apparaat</br>Zakelijke bedrijfscontinuïteit herstel (BCDR)|StorSimple Manager service → apparaten|[Failover en noodgevallen herstel voor uw apparaat StorSimple](storsimple-device-failover-disaster-recovery.md)|
|Lijst met back-ups voor een volume</br>Selecteer een back-ups instellen</br>Een back-reeks verwijderen|StorSimple Manager service → back-up-catalogus|[Back-ups beheren](storsimple-manage-backup-catalog.md)|
|Een volume klonen|StorSimple Manager service → back-up-catalogus|[Een volume klonen](storsimple-clone-volume.md)|
|Een back-up set terugzetten|StorSimple Manager service → back-up-catalogus|[Een back-up set terugzetten](storsimple-restore-from-backup-set.md)|
|Over accounts opslag</br>Een opslag-account toevoegen</br>Een account opslag bewerken</br>Een account opslagruimte verwijderen</br>Belangrijke draaiing van opslag-accounts|StorSimple Manager service → configureren|[Opslag accounts beheren](storsimple-manage-storage-accounts.md)|
|Over bandbreedte-sjablonen</br>Een sjabloon bandbreedte toevoegen</br>Sjabloon voor een bandbreedte bewerken</br>Een sjabloon bandbreedte verwijderen</br>Een standaardsjabloon bandbreedte gebruiken</br>Een hele dag duurt bandbreedte sjabloon vanaf een bepaalde tijd maken|StorSimple Manager service → configureren|[Bandbreedte distributiegroepen beheren](storsimple-manage-bandwidth-templates.md)|
|Over access besturingselement records</br>Een access-besturingselement-record maken</br>Een access-besturingselement record bewerken</br>Een access-besturingselement record verwijderen|StorSimple Manager service → configureren|[Access besturingselement records beheren](storsimple-manage-acrs.md)|
|Details van de taak weergeven</br>Een taak annuleren|StorSimple Manager service → taken|[Taken beheren](storsimple-manage-jobs.md)
|Meldingen ontvangen</br>Waarschuwingen beheren</br>Waarschuwingen bekijken|StorSimple Manager service → waarschuwingen|[Bekijken en StorSimple waarschuwingen beheren](storsimple-manage-alerts.md)
|Verbonden initiators weergeven</br>Het seriële getal van apparaat zoeken</br>Het doel IQN zoeken|StorSimple Manager service → apparaten → Dashboard|[Gebruik het dashboard StorSimple-apparaat](storsimple-device-dashboard.md)|
|Controle-diagrammen maken|StorSimple Manager service → apparaten → controleren|[Uw apparaat StorSimple controleren](storsimple-monitor-device.md)|
|Een container volume toevoegen</br>Een container volume wijzigen</br>Een container volume verwijderen|StorSimple Manager service → apparaten → Volume Containers|[Volume-containers beheren](storsimple-manage-volume-containers.md)|
|Een volume toevoegen</br>Een volume wijzigen</br>Een volume offline te zetten</br>Een volume verwijderen</br>Monitor met een volume|StorSimple Manager service → apparaten → Volume Containers → Volumes|[Volumes beheren](storsimple-manage-volumes.md)|
|Apparaatinstellingen wijzigen</br>Tijdinstellingen wijzigen</br>DNS.md-instellingen wijzigen</br>Netwerkinterfaces configureren|StorSimple Manager service → apparaten → configureren|[Apparaatconfiguratie voor uw apparaat StorSimple wijzigen](storsimple-modify-device-config.md)|
|Weergave web proxy-instellingen|StorSimple Manager service → apparaten → configureren|[Webproxy voor uw apparaat configureren](storsimple-configure-web-proxy.md)|
|Beheerderswachtwoord apparaat wijzigen</br>StorSimple momentopname Manager-wachtwoord wijzigen|StorSimple Manager service → apparaten → configureren|[StorSimple wachtwoorden wijzigen](storsimple-change-passwords.md)|
|Extern beheer configureren|StorSimple Manager service → apparaten → configureren|[Extern verbinding maken met uw apparaat StorSimple](storsimple-remote-connect.md)|
|Instellingen voor waarschuwingen configureren|StorSimple Manager service → apparaten → configureren|[Bekijken en StorSimple waarschuwingen beheren](storsimple-manage-alerts.md)|
|CHAP configureren voor uw apparaat StorSimple|StorSimple Manager service → apparaten → configureren|[CHAP configureren voor uw apparaat StorSimple](storsimple-configure-chap.md)|
|Een back-beleid toevoegen</br>Toevoegen of wijzigen van een planning</br>Een back-beleid verwijderen</br>Een handmatige back-up maken</br>Een aangepast back-beleid maken met meerdere volumes en planningen|StorSimple Manager service → apparaten → back-up-beleid|[Back-beleid beheren](storsimple-manage-backup-policies.md)|
|Apparaatcontrollers stoppen</br>Start opnieuw apparaatcontrollers</br>Apparaatcontrollers afsluiten</br>Uw apparaat fabrieksinstellingen herstellen</br>(Hierboven zijn voor on-premises implementatie apparaat alleen)|StorSimple Manager service → apparaten → onderhoud|[StorSimple apparaat controller beheren](storsimple-manage-device-controller.md)|
|Meer informatie over StorSimple hardwareonderdelen</br>Monitor hardwarestatus</br>(Hierboven zijn voor on-premises implementatie apparaat alleen)|StorSimple Manager service → apparaten → onderhoud|[Monitor hardwareonderdelen](storsimple-monitor-hardware-status.md)|
|Een ondersteuningspakket maken|StorSimple Manager service → apparaten → onderhoud|[Maken en beheren van een ondersteuningspakket](storsimple-create-manage-support-package.md)|
|Software-updates installeren|StorSimple Manager service → apparaten → onderhoud|[Bijwerken van uw apparaat](storsimple-update-device.md)|


##<a name="next-steps"></a>Volgende stappen
Als u problemen met de dagelijkse werking van uw apparaat StorSimple of met een van de hardwareonderdelen optreden, gebruikt u verwijzen naar:

- [Problemen met een operationele-apparaat](storsimple-troubleshoot-operational-device.md)
- [StorSimple monitoring indicator LED's gebruiken](storsimple-monitoring-indicators.md)

Als u niet kunt de problemen oplossen en moet u een serviceaanvraag maken, raadpleegt u [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).
