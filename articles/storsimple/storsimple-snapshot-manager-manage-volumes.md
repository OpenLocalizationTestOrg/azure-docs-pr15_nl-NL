<properties 
   pageTitle="StorSimple momentopname Manager en volumes | Microsoft Azure"
   description="Beschrijving van het gebruik van de module StorSimple momentopname Manager MMC weergeven en beheren van volumes en back-ups configureren."
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

# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>StorSimple momentopname Manager gebruiken om te bekijken en volumes beheren

## <a name="overview"></a>Overzicht

U kunt het knooppunt StorSimple momentopname Manager **Volumes** (in het deelvenster **bereik** ) volumes selecteren en informatie over deze te bekijken. De hoeveelheden worden als stations die overeenkomen met de volume gekoppeld door de host weergegeven. Het knooppunt **Volumes** ziet lokale hoeveelheden en volume-gegevenstypen die worden ondersteund door StorSimple, inclusief volumes ontdekt door middel van iSCSI en een apparaat. 

Voor meer informatie over ondersteunde volumes, gaat u naar [ondersteuning voor meerdere volumetypen](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Volumelijst in het deelvenster Zoekresultaten](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

Het knooppunt **Volumes** kunt u opnieuw controleren of de volumes verwijderen nadat StorSimple momentopname Manager ze ontdekt. 

Deze zelfstudie wordt uitgelegd hoe u kunt koppelen geïnitialiseerd, en formatteren en gebruik vervolgens StorSimple momentopname Manager om te wijzigen:

- Informatie over volumes weergeven 
- Volumes verwijderen
- Volumes scannen 
- Een standaardvolume configureren en back-up
- Een dynamisch gespiegeld volume configureren en back-up

>[AZURE.NOTE] Alle **Volume** knooppunt acties zijn ook beschikbaar in het deelvenster **Acties** .
 
## <a name="mount-volumes"></a>Volumes koppelen

Gebruik de volgende procedure om te koppelen, geïnitialiseerd en formatteren StorSimple. Deze procedure wordt Schijfopruiming Management, een systeemhulpprogramma voor het voor het beheren van vaste schijven en de bijbehorende volumes of partities gebruikt. Ga naar [Schijf Management](https://technet.microsoft.com/library/cc770943.aspx) op de Microsoft TechNet-website voor meer informatie over het beheer van de schijf.

#### <a name="to-mount-volumes"></a>Naar het volumes koppelen

1. Start het beginpunt van Microsoft iSCSI op uw hostcomputer.

2. Geef een van de interface IP-adressen als de doelportal of discovery IP-adres en verbinding maken met het apparaat. Zodra het apparaat is aangesloten, worden de hoeveelheden toegankelijk zijn voor uw Windows-bestandssysteem. Voor meer informatie over het gebruik van het beginpunt van Microsoft iSCSI, gaat u naar de sectie 'Verbinding maken met een iSCSI-doel-apparaat' in [installeren en configureren van Microsoft iSCSI Initiator][1].

3. Gebruik een van de volgende opties Schijfbeheer starten:

    - Typ Diskmgmt.msc in het vak **uitvoeren** .

    - Begindatum Serverbeheer, vouw het knooppunt **opslag** uit en selecteer **Schijfopruiming beheren**.

    - **Beheerprogramma's**starten, vouw het knooppunt **Beheer van de Computer** en selecteer **Schijfopruiming beheren**. 

    >[AZURE.NOTE] U moet beheerdersbevoegdheden gebruiken om uit te voeren schijf Management.
 
4. Het volume online uitvoeren:

   1. Beheer van schijf met de rechtermuisknop op een volume **Offline**gemarkeerd.

   2. Klik op **opnieuw activeren**. De schijf moet de status **Online** krijgen.

5. Het volume geïnitialiseerd:

   1. Met de rechtermuisknop op de ontdekt hoeveelheden.

   2. Klik in het menu Selecteer **Geïnitialiseerd schijf**.

   3. Selecteer de schijven die u wilt geïnitialiseerd in het dialoogvenster **Schijfopruiming geïnitialiseerd** en klik vervolgens op **OK**.

6. Formatteren eenvoudige:

   1. Met de rechtermuisknop op een volume dat u wilt opmaken.

   2. Selecteer in het menu **Nieuw eenvoudig Volume**.

   3. De wizard Nieuw eenvoudig Volume gebruiken om het volume:

      - Geef de grootte van het volume.
      - Geef een letter.
      - Selecteer het NTFS-bestandssysteem.
      - Geef een 64 KB-toewijzing eenheidsgrootte.
      - Een snelle indeling uitvoeren.

7. Meerdere partition formatteren. Ga naar de sectie 'partities en ' in volume [Schijf Management implementeren](https://msdn.microsoft.com/library/dd163556.aspx)voor instructies.

## <a name="view-information-about-your-volumes"></a>Informatie over begint weergeven

Gebruik de volgende procedure om informatie over lokaal en Azure StorSimple volumes te bekijken.

#### <a name="to-view-volume-information"></a>Volume-informatie kunnen bekijken

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten. 

2. Klik op het knooppunt **Volumes** in het deelvenster **bereik** . Er verschijnt een lijst van lokale en gekoppelde hoeveelheden, inclusief alle Azure StorSimple volumes, in het deelvenster **resultaten** . De kolommen in het deelvenster **resultaten** worden geconfigureerd. (Met de rechtermuisknop op het knooppunt **Volumes** , **weergave**selecteren en selecteer vervolgens **Toevoegen/verwijderen kolommen**).

    ![De kolommen configureren](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)

    Resultatenkolom | Beschrijving 
    :--------------|:-------------
    Naam           | De kolom **naam van** bevat de letter van elk ontdekt volume.
    Apparaat         | De kolom **apparaat** bevat het IP-adres van het apparaat is aangesloten op de hostcomputer.
    De naam van de Volume apparaat | De kolom **Naam van de Volume apparaat** bevat de naam van het volume van het apparaat die het geselecteerde volume hoort. Dit is de naam van het volume gedefinieerd in de portal van Azure klassieke voor dat specifieke volume.
    Access paden   | De kolom **Access paden** weergegeven het access-pad naar het volume. Dit is het station letter of een koppeling waarop het volume toegankelijk is op de host is.
 
## <a name="delete-a-volume"></a>Een volume verwijderen

Gebruik de volgende procedure uit StorSimple momentopname Manager van een volume verwijderen.

>[AZURE.NOTE] U kunt een volume niet verwijderen, als deze deel uitmaakt van een volume-groep. (De verwijderoptie is niet beschikbaar voor volumes die deel uitmaken van een groep volume.) U kunt de volledige volume-groep als u wilt verwijderen van het volume moet verwijderen.


#### <a name="to-delete-a-volume"></a>Een volume verwijderen

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik op het knooppunt **Volumes** in het deelvenster **bereik** . 

3. Klik in het deelvenster **resultaten** met de rechtermuisknop op het volume dat u wilt verwijderen.

4. Klik in het menu, klikt u op **verwijderen**. 

    ![Een volume verwijderen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 

5. Het dialoogvenster **Volume verwijderen** wordt weergegeven. Typ **bevestigen** in het tekstvak en klik vervolgens op **OK**.

6. Standaard StorSimple momentopname Manager back-ups van een volume voordat deze wordt verwijderd. Deze voorzorgsmaatregelen kunt beschermen tegen verlies van gegevens als de verwijdering onbedoeld is. StorSimple momentopname Manager, wordt er een bericht van de voortgang **Automatische momentopname** weergegeven terwijl deze back-ups van het volume. 

    ![Automatische momentopname bericht](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Volumes scannen

Gebruik de volgende procedure opnieuw controleren van de hoeveelheden verbonden aan StorSimple momentopname Manager.

#### <a name="to-rescan-the-volumes"></a>Opnieuw controleren van de hoeveelheden

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** met de rechtermuisknop op **Volumes**en klik op **opnieuw controleren volumes**.

    ![Volumes scannen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
 
    Deze procedure synchroniseert de volumelijst met StorSimple momentopname Manager. Wijzigingen, zoals nieuwe volumes of verwijderd volume, worden doorgevoerd in de zoekresultaten.

## <a name="configure-and-back-up-a-basic-volume"></a>Configureren en back-up van een standaardvolume

Gebruik de volgende procedure om te configureren van een back-up van een standaardvolume, en een back-up direct gestart of een beleid voor geplande back-ups maken.

### <a name="prerequisites"></a>Vereisten voor

Voordat u begint:

- Zorg ervoor dat de computer van een apparaat en host StorSimple correct zijn geconfigureerd. Ga naar [Deploy uw on-premises implementatie StorSimple apparaat](storsimple-deployment-walkthrough-u2.md)voor meer informatie.

- Installeren en configureren van StorSimple momentopname Manager. Ga naar [Implementeren StorSimple momentopname Manager](storsimple-snapshot-manager-deployment.md)voor meer informatie.

#### <a name="to-configure-backup-of-a-basic-volume"></a>Back-up van een standaardvolume configureren

1. Maak een standaardvolume op het apparaat StorSimple.

2. Koppelen, geïnitialiseerd en het volume opmaken zoals is beschreven in [volumes koppelen](#mount-volumes). 

3. Klik op het pictogram StorSimple momentopname Manager op uw bureaublad. Het venster StorSimple momentopname Manager wordt weergegeven. 

4. Klik in het deelvenster **bereik** met de rechtermuisknop op het knooppunt **Volumes** en selecteer vervolgens **opnieuw scannen volumes**. Wanneer de gescande afbeelding is voltooid, moet een lijst van hoeveelheden in het deelvenster **resultaten** weergegeven. 

5. Klik in het deelvenster **resultaten** met de rechtermuisknop op het volume en selecteer vervolgens **Volume-groep maken**. 

    ![Volume-groep maken](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 

6. Typ een naam voor de volume-groep in het dialoogvenster **Volume-groep maken** , volumes aan toewijzen en klik vervolgens op **OK**.

7. Vouw in het deelvenster **bereik** het **Volume groepen** uit. De groep nieuw volume wordt weergegeven onder het knooppunt **Volume-groepen** . 

8. Met de rechtermuisknop op de naam van de volume-groep.

    - Als u wilt een back-uptaak van interactieve (op aanvraag), klikt u op **Back-up uitvoeren**. 

    - Klik op **Back-beleid maken**voor het plannen van een automatische back-up. Selecteer een groep volume in de lijst op de pagina **Algemeen** . Voer de details van planning op de pagina **planning** . Wanneer u klaar bent, klikt u op **OK**. 

9. Vouw het knooppunt **taken** in het deelvenster **bereik** om te bevestigen dat u de back-uptaak heeft gestart, en klik vervolgens op het knooppunt **uitgevoerd** . De lijst met actieve taken wordt weergegeven in het deelvenster **resultaten** . 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Configureren en back-up van een dynamisch gespiegeld volume

Voer de volgende stappen uit om te configureren back-up van een dynamisch gespiegeld volume:

- Stap 1: Gebruik Schijfopruiming Management om een dynamische gespiegeld volume te maken. 

- Stap 2: StorSimple momentopname Manager gebruiken voor het configureren van back-up.

### <a name="prerequisites"></a>Vereisten voor

Voordat u begint:

- Zorg ervoor dat de computer van een apparaat en host StorSimple correct zijn geconfigureerd. Ga naar [Deploy uw on-premises implementatie StorSimple apparaat](storsimple-deployment-walkthrough-u2.md)voor meer informatie.

- Installeren en configureren van StorSimple momentopname Manager. Ga naar [Implementeren StorSimple momentopname Manager](storsimple-snapshot-manager-deployment.md)voor meer informatie.

- Twee volumes op het apparaat StorSimple configureren. (In de voorbeelden wordt de beschikbare hoeveelheden zijn **schijf 1** en **2 van de schijf**.) 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Stap 1: Gebruik Schijfopruiming Management een dynamische gespiegeld volume maken

Schijfbeheer van de is een systeemhulpprogramma voor het beheren van vaste schijven en de volumes of partities die ze bevatten. Ga naar [Schijf Management](https://technet.microsoft.com/library/cc770943.aspx) op de Microsoft TechNet-website voor meer informatie over het beheer van de schijf.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Een dynamische gespiegeld volume maken

1. Gebruik een van de volgende opties Schijfbeheer starten: 

   - Open het vak **uitvoeren** , typ **Diskmgmt.msc**en druk op Enter.

   - Begindatum Serverbeheer, vouw het knooppunt **opslag** uit en selecteer **Schijfopruiming beheren**. 

   - **Beheerprogramma's**starten, vouw het knooppunt **Beheer van de Computer** en selecteer **Schijfopruiming beheren**. 

2. Zorg ervoor dat er twee volumes beschikbaar op het apparaat StorSimple. (In het voorbeeld wordt de beschikbare hoeveelheden zijn **schijf 1** en **2 van de schijf**.) 

3. Klik in het venster Schijfopruiming Management in de rechterkolom van het onderste deelvenster met de rechtermuisknop op de **schijf 1** en selecteer **NieuwVolume gespiegeld weergegeven**. 

    ![Nieuwe gespiegeld Volume](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 

4. Klik op **volgende**op de pagina van de wizard **NieuwVolume gespiegeld weergegeven** .

5. Klik op de pagina **Schijven selecteren** **schijf 2** selecteert in het deelvenster **geselecteerde** , klikt u op **toevoegen**en klik vervolgens op **volgende**. 

6. Accepteer de standaardinstellingen op de pagina **station toewijzen of het pad** en klik vervolgens op **volgende**. 

7. Selecteer op de pagina **Het Volume van de indeling** in het vak **Grootte van de toewijzing** **64 kB**. Schakel het selectievakje **snel formatteren** en klik vervolgens op **volgende**. 

8. Klik op de pagina **voltooiing van het nieuwe gespiegeld weergegeven Volume** uw instellingen controleren en klik vervolgens op **Voltooien**. 

9. Er verschijnt een bericht om aan te geven dat de eenvoudige schijf worden geconverteerd naar een dynamische schijf. Klik op **Ja**.

    ![Dynamische schijf conversie van bericht](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 

10. Controleer of dat de schijf 1 en 2 van de schijf worden weergegeven als dynamische mirrorvolumes beheer van Schijfopruiming. (**Dynamische** moet worden weergegeven in de statuskolom en de kleur van de capaciteit balk moet wijzigen in rood, die aangeeft dat u een gespiegeld volume). 

    ![Schijf Management gespiegeld weergegeven dynamische schijven](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 
 
### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Stap 2: Gebruik StorSimple momentopname Manager back-up configureren

Gebruik de volgende procedure naar een dynamisch gespiegeld volume, configureren en een back-up direct gestart of een beleid voor geplande back-ups maken.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Back-up van een dynamisch gespiegeld volume configureren

1. Klik op het pictogram StorSimple momentopname Manager op uw bureaublad. Het venster StorSimple momentopname Manager wordt weergegeven. 

2. Klik in het deelvenster **bereik** met de rechtermuisknop op het knooppunt **Volumes** en selecteer **opnieuw scannen volumes**. Wanneer de gescande afbeelding is voltooid, moet een lijst van hoeveelheden in het deelvenster **resultaten** weergegeven. Het volume van het dynamische gespiegeld is vermeld als één volume. 

3. Met de rechtermuisknop op het dynamische gespiegeld volume in het deelvenster met **resultaten** en klik vervolgens op **Volume-groep maken**. 

4. Typ een naam voor de volume-groep in het dialoogvenster **Volume-groep maken** , het volume van het dynamische gespiegeld toewijzen aan deze groep en klik vervolgens op **OK**. 

5. Vouw in het deelvenster **bereik** het **Volume groepen** uit. De groep nieuw volume wordt weergegeven onder het knooppunt **Volume-groepen** . 

6. Met de rechtermuisknop op de naam van de volume-groep. 

    - Als u wilt een back-uptaak van interactieve (op aanvraag), klikt u op **Back-up uitvoeren**. 

    - Klik op **Back-beleid maken**voor het plannen van een automatische back-up. Selecteer de volume-groep in de lijst op de pagina **Algemeen** . Voer de details van planning op de pagina **planning** . Wanneer u klaar bent, klikt u op **OK**. 

7. Als dit wordt uitgevoerd, kunt u de back-uptaak controleren. Klik in het deelvenster **bereik** Vouw **taken** , en klik op **actief**, de taakgegevens worden weergegeven in het deelvenster met **resultaten** . Wanneer de back-uptaak is voltooid, worden de details overgebracht naar de takenlijst van de **afgelopen 24** uur. 

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [gebruik van StorSimple momentopname Manager kunt u uw oplossing StorSimple beheren](storsimple-snapshot-manager-admin.md).
- Informatie over het [gebruik van StorSimple momentopname Manager naar volume-groepen maken en beheren](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
