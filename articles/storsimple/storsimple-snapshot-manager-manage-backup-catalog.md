<properties 
   pageTitle="Back-catalogus StorSimple momentopname Manager | Microsoft Azure"
   description="Beschrijving van het gebruik van de module StorSimple momentopname Manager MMC weergeven en beheren van de back-catalogus."
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
   ms.date="04/26/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Gebruik StorSimple momentopname Manager voor het beheren van de back-catalogus

## <a name="overview"></a>Overzicht

De primaire functie van StorSimple momentopname Manager is dat u toepassing consistente back-ups van StorSimple hoeveelheden in de vorm van momentopnamen maken. Momentopnamen worden vermeld in een XML-bestand met de naam van een *back-catalogus*. De catalogus met back-up organiseert momentopnamen op volume-groep en vervolgens op de lokale momentopname of een momentopname van de cloud. 

Deze zelfstudie wordt beschreven hoe u kunt het knooppunt van de **Catalogus met back-up** is de volgende taken uitvoeren:

- Een volume herstellen 
- Een volume of het volume groep klonen 
- Een back-up verwijderen 
- Een bestand herstellen
- De Storsimple momentopname Manager-database herstellen

U kunt de back-catalogus weergeven door het knooppunt **Back-catalogus** in het deelvenster **bereik** uitvouwen, en vervolgens de volume-groep uit te vouwen.

- Als u op de naam van de volume-groep, ziet u het deelvenster **resultaten** het aantal lokale momentopnamen en cloud momentopnamen beschikbaar voor de volume-groep. 

- Als u op de **Lokale momentopname** of een **Momentopname van de Cloud**, ziet u **het deelvenster met** de volgende informatie over elke back-momentopname (afhankelijk van de **weergave** -instellingen): 

    - **Naam** : de tijd die de momentopname is gemaakt. 

    - **Type** , of dit een lokale momentopname of een momentopname van de cloud is. 

    - **Eigenaar** – eigenaar van de inhoud. 

    - **Beschikbare** – of de momentopname momenteel beschikbaar is. **Waar** wordt aangegeven dat de momentopname beschikbaar is en kan worden hersteld; **False** geeft aan dat de momentopname niet langer beschikbaar is. 

    - **Geïmporteerd** – of de back-up is geïmporteerd. **Waar** wordt aangegeven dat de back-up van de service StorSimple Manager is geïmporteerd op het moment dat het apparaat is geconfigureerd in StorSimple momentopname Manager. **False** geeft aan dat dit niet is geïmporteerd, maar dat is gemaakt door StorSimple momentopname Manager. (U kunt eenvoudig een geïmporteerde volume groep identificeren omdat achtervoegsel wordt toegevoegd die aangeeft dat het apparaat waaruit de volume-groep is geïmporteerd.)

    ![Back-catalogus](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)

- Als u **Lokale momentopname** of een **Momentopname van de Cloud uitvouwen**, en klik vervolgens op de naam van een afzonderlijke momentopname, ziet u **het deelvenster met** de volgende informatie over de momentopname die u hebt geselecteerd:

    - **Naam** : het volume die wordt aangeduid met stationsletter. 

    - **De naam van de lokale** : de lokale naam van het station (indien beschikbaar). 

    - **Apparaat** – de naam van het apparaat op het volume zich bevindt. 

    - **Beschikbare** – of de momentopname momenteel beschikbaar is. **Waar** wordt aangegeven dat de momentopname beschikbaar is en kan worden hersteld; **False** geeft aan dat de momentopname niet langer beschikbaar is. 


## <a name="restore-a-volume"></a>Een volume herstellen

Gebruik de volgende procedure als u een volume herstellen uit back-up.

#### <a name="prerequisites"></a>Vereisten voor

Als u dit nog niet hebt gedaan, een volume en volume-groep maken en verwijder vervolgens het volume. Standaard StorSimple momentopname Manager back-ups van een volume voordat deze worden verwijderd. Deze voorzorgsmaatregelen kunt verlies van gegevens voorkomen als het volume per ongeluk wordt verwijderd of dat de gegevens moet worden hersteld voor welke reden dan ook. 

StorSimple momentopname Manager wordt het volgende bericht weergegeven terwijl u dat de voorzorgsmaatregelen back-up wordt gemaakt.

![Automatische momentopname bericht](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

>[AZURE.IMPORTANT]U kunt een volume die deel uitmaakt van een groep volume niet verwijderen. De verwijderoptie is niet beschikbaar. <br>

#### <a name="to-restore-a-volume"></a>Een volume herstellen

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten. 

2. Klik in het deelvenster **bereik** Vouw **Back-up-catalogus** , een volume-groep uitvouwen en klik op **Lokale momentopnamen** of **Cloud momentopnamen**. Er verschijnt een lijst met back-up momentopnamen in het deelvenster **resultaten** . 

3. Zoeken naar de back-up die u terugzetten wilt, klik met de rechtermuisknop, en klik vervolgens op **herstellen**. 

    ![Back-catalogus terugzetten](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 

4. Klik op de bevestigingspagina Controleer de details, typt u **bevestigen**en klik vervolgens op **OK**. StorSimple momentopname Manager wordt de back-up om het volume herstellen. 

    ![Bevestigingsbericht herstellen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 

5. Als dit wordt uitgevoerd, kunt u de bewerking controleren. Vouw het knooppunt **taken** uit in het deelvenster **bereik** en klik vervolgens op **uitvoeren**. De taakgegevens worden weergegeven in het deelvenster **resultaten** . Wanneer de terugzettaak is voltooid, worden details van de overgebracht naar de lijst **afgelopen 24 uur** .

## <a name="clone-a-volume-or-volume-group"></a>Een volume of het volume groep klonen

Gebruik de volgende procedure een kopie (klonen) van een volume of het volume-groep te maken.

#### <a name="to-clone-a-volume-or-volume-group"></a>Een volume of het volume groep kopiëren

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** Vouw **Back-up-catalogus** , een volume-groep uitvouwen en klik op **Cloud momentopnamen**. Er verschijnt een lijst met back-ups in het deelvenster **resultaten** .

3. Zoek het volume of het volume-groep die u wilt klonen, met de rechtermuisknop op het volume of de naam van de volume-groep en klik op **klonen**. Het dialoogvenster **Klonen Cloud momentopname** wordt weergegeven.

    ![Een momentopname cloud klonen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Voer in het dialoogvenster **Klonen Cloud momentopname** als volgt: 

    1. Typ in het tekstvak **naam** een naam voor het gekloonde volume. Deze naam wordt weergegeven in het knooppunt **Volumes** . 

    2. (Optioneel) Selecteer **station**en selecteer vervolgens een letter in de vervolgkeuzelijst. 

    3. (Optioneel) Selecteer **Map (NTFS)**, en typ een mappad of klik op Bladeren en selecteer een locatie voor de map. 

    4. Klik op **maken**.

5. Wanneer het klonen proces is voltooid, moet u het volume van het gekloonde geïnitialiseerd. Serverbeheer vervolgens starten en schijf Management. Zie [volumes koppelen](storsimple-snapshot-manager-manage-volumes.md#mount-volumes)voor gedetailleerde instructies. Nadat deze is geïnitialiseerd, kan het volume, worden vermeld onder het knooppunt **Volumes** in het deelvenster **bereik** . Als u het volume vermeld niet ziet, vernieuwt u de lijst van hoeveelheden (met de rechtermuisknop op het knooppunt **Volumes** en klik vervolgens op **vernieuwen**).

## <a name="delete-a-backup"></a>Een back-up verwijderen

Gebruik de volgende procedure een momentopname verwijderen uit de back-catalogus. 

>[AZURE.NOTE]Een momentopname verwijdert, wordt de back-ups van gegevens die zijn gekoppeld aan de momentopname. Het proces van het opschonen van gegevens uit de cloud kan echter enige tijd duren.<br>
 
#### <a name="to-delete-a-backup"></a>Een back-up verwijderen

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** Vouw **Back-up-catalogus** , een volume-groep uitvouwen en klik op **Lokale momentopnamen** of **Cloud momentopnamen**. Er verschijnt een lijst met momentopnamen in het deelvenster **resultaten** . 

3. Met de rechtermuisknop op de momentopname die u wilt verwijderen en klik vervolgens op **verwijderen**.

4. Als het bevestigingsbericht wordt weergegeven, klikt u op **OK**. 

## <a name="recover-a-file"></a>Een bestand herstellen

Als een bestand per ongeluk wordt verwijderd uit een volume, kunt u het bestand een momentopname die vooraf datums van de verwijdering ophalen, met de momentopname te maken van het volume en vervolgens het bestand uit het gekloonde volume te kopiëren naar de oorspronkelijke volume terugzetten.

#### <a name="prerequisites"></a>Vereisten voor

Voordat u begint, zorg dat er een huidige back-up van de volume-groep. Vervolgens een bestand dat is opgeslagen op een van de hoeveelheden in die groep volume verwijderen. Gebruik de volgende stappen ten slotte het verwijderde bestand herstellen uit uw back-up. 

#### <a name="to-recover-a-deleted-file"></a>Een verwijderd bestand herstellen

1. Klik op het pictogram StorSimple momentopname Manager op uw bureaublad. Het consolevenster StorSimple momentopname Manager wordt weergegeven. 

2. Klik in het deelvenster **bereik** Vouw **Back-up-catalogus** en bladert u naar een momentopname die het verwijderde bestand bevat. Meestal, moet u een momentopname die is gemaakt vlak voor het verwijderen. 

3. Zoek het volume dat u klonen wilt, klik met de rechtermuisknop, en klik op **klonen**. Het dialoogvenster **Klonen Cloud momentopname** wordt weergegeven.

    ![Een momentopname cloud klonen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Voer in het dialoogvenster **Klonen Cloud momentopname** als volgt: 

   1. Typ in het tekstvak **naam** een naam voor het gekloonde volume. Deze naam wordt weergegeven in het knooppunt **Volumes** . 

   2. (Optioneel) Selecteer **station**en selecteer vervolgens een letter in de vervolgkeuzelijst. 

   3. (Optioneel) Selecteert u **Map (NTFS)**, en typ een mappad of klik op **Bladeren** en selecteer een locatie voor de map. 

   4. Klik op **maken**. 

5. Wanneer het klonen proces is voltooid, moet u het volume van het gekloonde geïnitialiseerd. Serverbeheer vervolgens starten en schijf Management. Zie [volumes koppelen](storsimple-snapshot-manager-manage-volumes.md#mount-volumes)voor gedetailleerde instructies. Nadat deze is geïnitialiseerd, kan het volume, worden vermeld onder het knooppunt **Volumes** in het deelvenster **bereik** . 

    Als u het volume vermeld niet ziet, vernieuwt u de lijst van hoeveelheden (met de rechtermuisknop op het knooppunt **Volumes** en klik vervolgens op **vernieuwen**).

6. Open de NTFS-map met het gekloonde volume, vouw het knooppunt **Volumes** en open vervolgens het gekloonde volume. Zoek het bestand dat u wilt herstellen, en kopieer deze naar de primaire volume.

7. Nadat u het bestand herstellen, kunt u de NTFS-map met het gekloonde volume verwijderen.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>De StorSimple momentopname Manager-database herstellen

U moet regelmatig back-up van de StorSimple momentopname Manager-database op de host. Als een noodgevallen optreedt of als de computer om welke reden dan ook mislukt, kunt u deze herstellen uit de back-up. De database back-up maakt, wordt een handmatig proces.

#### <a name="to-back-up-and-restore-the-database"></a>Een back-up en herstellen van de database

1. De Microsoft StorSimple Management-Service stoppen:

    1. Start Server Manager.

    2. Selecteer op het dashboard Serverbeheer, klikt u op het menu **Extra** **Services**.

    3. Selecteer de **Microsoft StorSimple Management-Service**in het venster **Services** .

    4. Klik in het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, op **de service stoppen**.

2. Op de hostcomputer, bladert u naar C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData is een verborgen map.
 
3. De catalogus XML-bestand hebt gevonden, kopieert u het bestand en de kopie opslaan in een veilige locatie of in de cloud. Als de host mislukt, kunt u dit back-upbestand te herstellen van het beleid voor het back-ups die u hebt gemaakt in StorSimple momentopname Manager.

    ![Azure StorSimple catalogus met back-bestand](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)

4. De Microsoft StorSimple Management-Service opnieuw: 

    1. Selecteer op het dashboard Serverbeheer, klikt u op het menu **Extra** **Services**.
    
    2. Selecteer de **Microsoft StorSimple Management-Service**in het venster **Services** .

    3. Klik in het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, op **de service opnieuw starten**.

5. Op de hostcomputer, bladert u naar C:\ProgramData\Microsoft\StorSimple\BACatalog. 

6. De catalogus XML-bestand verwijderen en vervangen door de back-up die u hebt gemaakt. 

7. Klik op het bureaublad StorSimple momentopname Manager-pictogram om StorSimple momentopname Manager te starten. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van StorSimple momentopname Manager kunt u uw oplossing StorSimple beheren](storsimple-snapshot-manager-admin.md).
- Meer informatie over [StorSimple momentopname Manager taken en werkstromen](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).
