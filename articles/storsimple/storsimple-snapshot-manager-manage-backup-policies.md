<properties 
   pageTitle="Beleidsregels voor het back-up van StorSimple momentopname Manager | Microsoft Azure"
   description="Beschrijving van het gebruik van de module StorSimple momentopname Manager MMC maken en beheren van de back-up beleidsregels waarmee wordt bepaald geplande back-ups."
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
   ms.date="05/12/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Manager van gebruiken StorSimple momentopname maken en beheren van back-beleid

## <a name="overview"></a>Overzicht

Een back-beleid Hiermee maakt u een schema voor het back-ups van volumegegevens lokaal of in de cloud. Wanneer u een back-beleid maken, kunt u ook een bewaarbeleid. (U kunt maximaal 64 momentopnamen behouden). Zie voor meer informatie over het back-beleid [back-up typen](storsimple-what-is-snapshot-manager.md#backup-type) in [StorSimple 8000-serie: een hybride cloud-oplossing](storsimple-overview.md).

Deze zelfstudie wordt uitgelegd hoe u:

- Een back-beleid maken 
- Een back-beleid bewerken 
- Een back-beleid verwijderen 

## <a name="create-a-backup-policy"></a>Een back-beleid maken

Gebruik de volgende procedure een nieuw back-beleid maken.

#### <a name="to-create-a-backup-policy"></a>Een back-beleid maken

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** met de rechtermuisknop op het **Beleid voor het back-up**en op **Back-up-beleid maken**.

    ![Een back-beleid maken](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Het dialoogvenster **maken een beleid** wordt weergegeven. 

    ![Maak een beleid - tabblad Algemeen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Vul op het tabblad **Algemeen** in de volgende informatie:

   1. Typ in het tekstvak **naam** een naam voor het beleid.

   2. Typ de naam van de volume-groep die is gekoppeld aan het beleid in het tekstvak **Volume-groep** .

   3. Selecteer **lokale momentopname** of **Cloud momentopname**.

   4. Selecteer het aantal momentopnamen moeten worden bewaard. Als u **alle**selecteert, 64 momentopnamen nog niet behouden (de maximumwaarde). 

4. Klik op het tabblad **planning** .

    ![Beleid - tabblad planning maken](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Vul op het tabblad **planning** , in de volgende informatie: 

   1. Klik op het selectievakje **inschakelen** voor het plannen van de volgende back-up.

   2. Schakel onder **Instellingen** **eenmalig**, **dagelijks**, **Wekelijks**of **maandelijks**. 

   3. In het tekstvak **beginnen** , klikt u op het agendapictogram en selecteer een begindatum.

   4. U kunt optioneel herhaald schema en een einddatum instellen onder **Geavanceerde instellingen**.

   5. Klik op **OK**.

Nadat u een back-beleid maken, wordt de volgende gegevens weergegeven in het deelvenster **resultaten** :

- **Naam** : de naam van back-beleid.

- **Type** : lokale momentopname of een momentopname van de cloud.

- **Volume-groep** : de volume-groep die is gekoppeld aan het beleid.

- **Bewaarbeleid** – het aantal momentopnamen behouden; het maximale aantal is 64.

- **Gemaakt** : de datum waarop dit beleid is gemaakt.

- **Ingeschakeld** – of het beleid is momenteel in feite: **True** geeft aan dat dit van kracht; is **False** geeft aan dat dit niet in feite is. 

## <a name="edit-a-backup-policy"></a>Een back-beleid bewerken

Gebruik de volgende procedure om een bestaande back-beleid te bewerken.

#### <a name="to-edit-a-backup-policy"></a>Een back-beleid bewerken

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten. 

2. Klik op het knooppunt **Beleid van de back-up** in het deelvenster **bereik** . Het back-beleid worden weergegeven in het deelvenster **resultaten** . 

3. Met de rechtermuisknop op het beleid dat u wilt bewerken en klik vervolgens op **bewerken**. 

    ![Een back-beleid bewerken](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. Wanneer het venster **een beleid maken** wordt weergegeven, voert u uw wijzigingen en klik vervolgens op **OK**. 

## <a name="delete-a-backup-policy"></a>Een back-beleid verwijderen

Gebruik de volgende procedure een back-beleid verwijderen.

#### <a name="to-delete-a-backup-policy"></a>Een back-beleid verwijderen

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten. 

2. Klik op het knooppunt **Beleid van de back-up** in het deelvenster **bereik** . Het back-beleid worden weergegeven in het deelvenster **resultaten** . 

3. Met de rechtermuisknop op het back-beleid dat u wilt verwijderen en klik vervolgens op **verwijderen**.

4. Wanneer het bevestigingsbericht wordt weergegeven, klikt u op **Ja**.

    ![Back-beleid bevestiging verwijderen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [gebruik van StorSimple momentopname Manager kunt u uw oplossing StorSimple beheren](storsimple-snapshot-manager-admin.md).
- Informatie over het [gebruik van StorSimple momentopname Manager weergeven en beheren van back-taken](storsimple-snapshot-manager-manage-backup-jobs.md).
