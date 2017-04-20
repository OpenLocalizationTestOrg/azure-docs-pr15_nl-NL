<properties 
   pageTitle="StorSimple momentopname Manager volume groepen | Microsoft Azure"
   description="Wordt beschreven hoe u met de module StorSimple momentopname Manager MMC volume-groepen maken en beheren."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>Gebruik StorSimple momentopname Manager naar volume-groepen maken en beheren

## <a name="overview"></a>Overzicht

U kunt het knooppunt **Volume groepen** in het deelvenster **bereik** volumes toewijzen aan groepen volume, informatie weergeven over een volume-groep Back-ups plannen en volume-groepen bewerken. 

Volume-groepen zijn groepen van gerelateerde hoeveelheden gebruikt om ervoor te zorgen dat de back-ups van toepassing-consistent zijn. Zie [hoeveelheden en volume-groepen](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) en [integratie met Windows Volume schaduw Copy-Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service)voor meer informatie.

>[AZURE.IMPORTANT] 
>
> * Enkel volume van een groep volume moeten afkomstig zijn uit een serviceprovider één cloud.
> 
> * Als u het volume van groepen configureert, niet-gedeelde volumes (CSVs) en niet-CSVs in dezelfde groep volume mengen. StorSimple momentopname Manager biedt geen ondersteuning voor een combinatie van CSVs en niet-CSVs in de dezelfde momentopname.
 
![Volume groepen knooppunt](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Afbeelding 1: StorSimple momentopname Manager Volume groepen knooppunt** 

Deze zelfstudie wordt uitgelegd hoe u kunt StorSimple momentopname Manager om te wijzigen:

- Informatie over uw volume-groepen weergeven 
- Een volume-groep maken
- Back-up van een groep volume
- Een volume-groep bewerken
- Een volume-groep verwijderen

Al deze acties zijn ook beschikbaar in het deelvenster **Acties** .
 
## <a name="view-volume-groups"></a>Volume-groepen weergeven

Als u op het knooppunt **Volume groepen** , ziet u **het deelvenster met** de volgende informatie over elke groep volume, afhankelijk van de selecties van de kolom die u maakt. (De kolommen in het deelvenster **resultaten** worden geconfigureerd. Met de rechtermuisknop op het knooppunt **Volumes** , selecteert u **weergave**en selecteer vervolgens **Toevoegen/verwijderen kolommen**.)

Resultatenkolom | Beschrijving 
:--------------|:------------ 
Naam           | De kolom **naam** bevat de naam van de volume-groep.
Toepassing    | De kolom **toepassingen** bevat het nummer van VSS-schrijvers momenteel is geïnstalleerd en uit te voeren op de Windows-host.
Geselecteerd       | De **geselecteerde** kolom ziet u het aantal volume die zijn opgenomen in de groep volume. Een nul (0) wordt aangegeven dat er geen toepassing gekoppeld aan in de groep het volume van het volume is.
Geïmporteerd       | De **ingevoerde** kolom ziet u het aantal ingevoerde hoeveelheden. Als de waarde **waar**is, met deze kolom geeft aan dat een volume-groep is geïmporteerd uit de Azure klassieke portal en niet in StorSimple momentopname Manager gemaakt is.
 
>[AZURE.NOTE] StorSimple momentopname Manager volume groepen worden ook weergegeven op het tabblad **Beleid voor het back-up maken** in de portal van Azure klassieke.
 
## <a name="create-a-volume-group"></a>Een volume-groep maken

Gebruik de volgende procedure om een volume-groep te maken.

#### <a name="to-create-a-volume-group"></a>Een volume-groep maken

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten. 

2. Klik in het deelvenster **bereik** met de rechtermuisknop op **Het Volume van groepen**en klik vervolgens op **Volume-groep maken**. 

    ![Volume-groep maken](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
 
    Het dialoogvenster **een volume-groep maken** wordt weergegeven. 

    ![Dialoogvenster groep volume maken](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png) 

3.  Voer de volgende gegevens: 

    1. Typ in het vak **naam** een unieke naam voor de nieuwe volume-groep. 

    2. Selecteer in het vak **toepassingen** toepassingen die is gekoppeld aan de delen die u aan de volume-groep toevoegen wilt. 

        Het vak **toepassingen** bevat alleen die toepassingen die gebruikmaken van StorSimple volumes en VSS-schrijvers hebt ingeschakeld voor deze. Een VSS-schrijver is alleen beschikbaar als de hoeveelheden die de schrijver is een bekend StorSimple volumes zijn. Als het vak toepassingen leeg is, worden klikt u vervolgens geen toepassingen die gebruikmaken van Azure StorSimple volumes en VSS-schrijvers hebt ondersteund geïnstalleerd. (Momenteel Azure StorSimple ondersteunt Microsoft Exchange en SQL Server.) Zie voor meer informatie over VSS-schrijvers, [integratie met Windows Volume schaduw Copy-Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

        Als u een toepassing selecteert, worden alle volumes gekoppeld worden automatisch geselecteerd. Omgekeerd, als u volumes die is gekoppeld aan een specifieke toepassing selecteert, wordt de toepassing wordt automatisch in het vak **toepassingen** geselecteerd. 

    3. Selecteer in het vak **Volumes** StorSimple hoeveelheden die moeten toevoegen aan de volume-groep. 

      - U kunt volumes met enkelvoudige of meervoudige partities opnemen. (Meerdere partition volumes steekt nogal dynamische schijven of eenvoudige schijven met meerdere partities.) Een volume met meerdere partities wordt behandeld als een eenheid. Dus als u slechts één van de partities aan een groep volume toevoegt, worden alle andere partities automatisch toegevoegd aan die groep volume op hetzelfde moment. Nadat u een meerdere partition volume aan een groep volume toevoegt, blijft het volume van het meerdere partition worden behandeld als een eenheid.

      - U kunt het volume van lege groepen maken door niet alle volumes toewijzen aan deze. 

      - Meng niet-gedeelde volumes (CSVs) en niet-CSVs in dezelfde groep volume. StorSimple momentopname Manager biedt geen ondersteuning voor een combinatie van CSV-hoeveelheden en niet-CSV-volumes in de dezelfde momentopname. 

4. Klik op **OK** om op te slaan de volume-groep.

## <a name="back-up-a-volume-group"></a>Back-up van een groep volume

Gebruik de volgende procedure back-up een volume-groep.

#### <a name="to-back-up-a-volume-group"></a>Back-up een volume-groep

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** het knooppunt **Volume groepen** uitvouwen, met de rechtermuisknop op de naam van een volume-groep en klik vervolgens op **Back-up uitvoeren**. 

    ![Direct een back-up volume-groep](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)

3. Klik in het dialoogvenster **Back-up uitvoeren** Selecteer **Lokale momentopname** of **Cloud-opname**en klik vervolgens op **maken**. 

    ![Maken van back-dialoogvenster](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png) 

4. Vouw het knooppunt **taken** uit om te bevestigen dat de back-up wordt uitgevoerd, en klik op **uitvoeren**. De back-up moet worden vermeld.

5. Een momentopname van de voltooide bekijken, vouw **Back-up-catalogus** , de naam van de volume-groep uitvouwen en klik vervolgens op **Lokale momentopname** of een **Momentopname van de Cloud**. De back-up worden, vermeld als deze is voltooid. 

## <a name="edit-a-volume-group"></a>Een volume-groep bewerken

Gebruik de volgende procedure als u een volume-groep bewerken.

#### <a name="to-edit-a-volume-group"></a>Een volume-groep bewerken

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** het knooppunt **Volume groepen** uitvouwen, met de rechtermuisknop op de naam van een volume-groep en klik vervolgens op **bewerken**. 

3. Het dialoogvenster **een volume-groep maken **wordt weergegeven. U kunt de **naam**, - **toepassingen**en **Volumes** items wijzigen. 

4. Klik op **OK** om uw wijzigingen op te slaan.

## <a name="delete-a-volume-group"></a>Een volume-groep verwijderen

Gebruik de volgende procedure om een volume-groep te verwijderen. 

>[AZURE.WARNING] Hiermee verwijdert u ook alle de back-ups die is gekoppeld aan de volume-groep.

#### <a name="to-delete-a-volume-group"></a>Een volume-groep verwijderen

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten. 

2. Klik in het deelvenster **bereik** het knooppunt **Volume groepen** uitvouwen, met de rechtermuisknop op de naam van een volume-groep en klik vervolgens op **verwijderen**. 

3. Het dialoogvenster **Volume-groep verwijderen** wordt weergegeven. Typ **bevestigen** in het tekstvak en klik vervolgens op **OK**. 

    De groep verwijderde volume verdwijnt uit de lijst in het deelvenster met **resultaten** en alle back-ups die zijn gekoppeld aan die groep volume worden verwijderd uit de back-catalogus.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [gebruik van StorSimple momentopname Manager kunt u uw oplossing StorSimple beheren](storsimple-snapshot-manager-admin.md).
- Informatie over het [gebruik van StorSimple momentopname Manager maken en beheren van back-beleid](storsimple-snapshot-manager-manage-backup-policies.md).
