<properties 
   pageTitle="Beheer van de Manager StorSimple virtuele matrix | Microsoft Azure"
   description="Informatie over het beheren van uw StorSimple on-premises implementatie virtuele matrix met behulp van de StorSimple Manager-service in de portal van Azure klassieke."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-virtual-array"></a>Gebruik van de StorSimple Manager-service voor het beheren van uw virtuele StorSimple-matrix

![Processtroom](./media/storsimple-ova-manager-service-administration/manage4.png)

## <a name="overview"></a>Overzicht

Dit artikel worden de StorSimple Manager-interface, waaronder over het verbinding maken met deze en de verschillende opties beschikbaar, en koppelingen naar de specifieke werkstromen die kunnen worden uitgevoerd via deze gebruikersinterface. 

Lees dit artikel en weet u hoe u:

- Verbinding maken met de StorSimple Manager-service
- De gebruikersinterface van de Manager StorSimple navigeren
- Uw StorSimple virtuele matrix via de StorSimple Manager-service beheren

> [AZURE.NOTE] Als u wilt bekijken van de opties beschikbaar voor het apparaat van de reeks StorSimple 8000, gaat u naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md).

## <a name="connect-to-the-storsimple-manager-service"></a>Verbinding maken met de StorSimple Manager-service

De StorSimple Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple virtuele matrices. U kunt een centrale Microsoft Azure klassieke portal wordt uitgevoerd in een browser gebruiken om deze apparaten te beheren. Ga als volgt te werk als u wilt verbinden met de service StorSimple Manager.

#### <a name="to-connect-to-the-service"></a>Verbinding maken met de service

1. Ga naar [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

2. De referenties van uw Microsoft-account gebruikt, meld u aan bij de Microsoft Azure klassieke-portal (te vinden op de rechtsboven in het deelvenster).

3. Schuif omlaag in het linkernavigatiedeelvenster voor toegang tot de StorSimple Manager-service.

    ![Schuif naar de service](./media/storsimple-ova-manager-service-administration/admin-scroll.png)

## <a name="navigate-the-storsimple-manager-service-ui"></a>De StorSimple Manager-service UI navigeren

De navigatie hiërarchie voor de service StorSimple Manager UI wordt weergegeven in de volgende tabel.

- De pagina van de aantekening **StorSimple Manager** gaat u naar de pagina service level UI van toepassing op alle virtuele matrices binnen een service's.

- De pagina **apparaten** gaat u naar de toepassing op een specifieke virtuele matrix apparaatniveau – UI's.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple Manager service navigatie hiërarchie

|Dialoogvenster aantekening toevoegen|Pagina's op service-niveau|Apparaatniveau pagina 's|
|---|---|---|
|StorSimple Manager-service|Dashboard (service)|Dashboard (apparaat)|
||Apparaten →|Monitor|
||Back-catalogus|Waarden voor aandelen (bestandsserver) of </br>Volumes (iSCSI-server)|
||(Service) configureren|(Apparaat) configureren|
||Taken|Onderhoud|
||Waarschuwingen|

## <a name="use-the-storsimple-manager-service-to-perform-management-tasks"></a>Gebruik van de service StorSimple Manager beheertaken uitvoeren

De volgende tabel bevat een overzicht van alle veelvoorkomende beheertaken en complexe werkstromen die kunnen worden uitgevoerd binnen de gebruikersinterface van de StorSimple Manager-service. Deze taken zijn gerangschikt op basis van de UI-pagina's waarop ze zijn gestart.

Voor meer informatie over elke werkstroom, klikt u op de juiste procedure in de tabel.

#### <a name="storsimple-manager-workflows"></a>StorSimple Manager werkstromen

|Als u dit wilt doen...|Ga naar deze pagina UI...|Gebruik deze procedure|
|---|---|---|
|Een service maken</br>Een service verwijderen</br>De sleutel voor de registratie ophalen</br>De sleutel voor de registratie genereren|StorSimple Manager-service|[De StorSimple Manager-service implementeren](storsimple-ova-manage-service.md)|
|De service gegevens versleutelingssleutel wijzigen</br>De logboeken bewerkingen weergeven|StorSimple Manager service → Dashboard|[Gebruik het dashboard van de service StorSimple](storsimple-ova-service-dashboard.md)|
|Een virtuele matrix deactiveren</br>Een virtuele matrix verwijderen|StorSimple Manager service → apparaten|[Deactiveren of verwijderen van een virtuele matrix](storsimple-ova-deactivate-and-delete-device.md)|
|Noodgevallen herstel en apparaat failover</br>Vereisten voor failover</br>Failover naar een virtueel apparaat</br>Zakelijke bedrijfscontinuïteit herstel (BCDR)</br>Fouten tijdens herstel|StorSimple Manager service → apparaten|[Noodgevallen herstel en apparaat failover voor uw virtuele StorSimple-matrix](storsimple-ova-failover-dr.md)|
|Een back-up waarden voor aandelen en volumes</br>Een handmatige back-up maken</br>De back-planning wijzigen</br>Bestaande back-ups weergeven|StorSimple Manager service → back-up-catalogus|[Back-up van uw virtuele StorSimple-matrix](storsimple-ova-backup.md)|
|Waarden voor aandelen terugzetten uit een back-ups instellen</br>Volumes terugzetten uit een back-ups instellen</br>Itemniveau herstel (alleen bestandsserver)|StorSimple Manager service → back-up-catalogus|[Terugzetten uit een back-up van uw virtuele StorSimple-matrix](storsimple-ova-restore.md)|
|Over accounts opslag</br>Een opslag-account toevoegen</br>Een account opslag bewerken</br>Een account opslagruimte verwijderen|StorSimple Manager service → configureren|[Opslag accounts voor de StorSimple virtuele matrix beheren](storsimple-ova-manage-storage-accounts.md)|
|Over access besturingselement records</br>Toevoegen of wijzigen van een record voor een access-besturingselement </br>Een access-besturingselement record verwijderen|StorSimple Manager service → configureren|[Access besturingselement records beheren voor de virtuele StorSimple-matrix](storsimple-ova-manage-acrs.md)|
|Details van de taak weergeven|StorSimple Manager service → taken| [Virtuele matrix StorSimple taken beheren](storsimple-ova-manage-jobs.md)|
|Instellingen voor waarschuwingen configureren</br>Meldingen ontvangen</br>Waarschuwingen beheren</br>Waarschuwingen bekijken|StorSimple Manager service → waarschuwingen|[Bekijken en waarschuwingen beheren voor de virtuele StorSimple-matrix](storsimple-ova-manage-alerts.md)|
|Het apparaat administrator-wachtwoord wijzigen|StorSimple Manager service → apparaten → configureren|[Het virtuele matrix StorSimple apparaat beheerderswachtwoord wijzigen](storsimple-ova-change-device-admin-password.md)|
|Software-updates installeren|StorSimple Manager service → apparaten → onderhoud|[Bijwerken van uw virtuele matrix](storsimple-ova-install-update-01.md)|

>[AZURE.NOTE] U moet de [lokale web UI](storsimple-ova-web-ui-admin.md) gebruiken voor de volgende taken uitvoeren:
>
>- [De service gegevens versleutelingssleutel ophalen](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
>- [Een ondersteuningspakket maken](storsimple-ova-web-ui-admin.md#generate-a-log-package)
>- [Stoppen en opnieuw starten van een virtuele matrix](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)

##<a name="next-steps"></a>Volgende stappen
Voor informatie over het web UI en hoe u dit gebruikt, gaat u naar [het web StorSimple gebruikersinterface voor het beheren van uw StorSimple virtuele matrix gebruiken](storsimple-ova-web-ui-admin.md).
