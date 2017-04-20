<properties 
   pageTitle="Back-taken StorSimple momentopname Manager | Microsoft Azure"
   description="Beschrijving van het gebruik van de module StorSimple momentopname Manager MMC weergeven en beheren van geplande, voltooide en actieve back-taken."
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


# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>StorSimple momentopname Manager gebruiken om te bekijken en beheren van back-taken

## <a name="overview"></a>Overzicht

Het knooppunt **taken** in het deelvenster **bereik** ziet u de **geplande**, **afgelopen 24 uur**en **uitgevoerd** back-taken die u hebt gestart interactief of door een geconfigureerde beleid. 

Deze zelfstudie wordt uitgelegd hoe u het knooppunt **taken** kunt gebruiken om informatie over geplande, recente, en worden momenteel uitgevoerd back-taken weer te geven. (De lijst met taken en de bijbehorende informatie wordt weergegeven in het deelvenster met **resultaten** .) U kunt ook met de rechtermuisknop op een vermelden taak en een contextmenu met beschikbare acties ziet.

## <a name="view-scheduled-jobs"></a>Geplande taken weergeven

Gebruik de volgende procedure om weer te geven van de geplande back-taken.

#### <a name="to-view-scheduled-jobs"></a>Geplande taken weergeven

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten. 

2. Klik in het deelvenster **bereik** Vouw **taken** en **gepland**op. De volgende gegevens weergegeven in het deelvenster **resultaten** :

    - **Naam** : de naam van de geplande momentopname

    - **Volgende uitvoeren** : de datum en tijd van de volgende geplande momentopname

    - **Laatste uitvoeren** : de datum en tijd van de meest recente geplande momentopname

    >[AZURE.NOTE] Voor eenmalige alleen momentopnamen, worden de **Volgende** laatste wordt **Uitgevoerd** en niet hetzelfde zijn. 
 
    ![Geplande back-taken](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
 
3. Als u wilt aanvullende acties uitvoeren op een bepaalde taak, met de rechtermuisknop op de naam van de taak in het deelvenster **resultaten** en selecteer in de opties in het menu.

## <a name="view-recent-jobs"></a>Recente taken weergeven

Gebruik de volgende procedure om te bekijken van back-up en herstellen van taken die zijn voltooid in de afgelopen 24 uur.

#### <a name="to-view-recent-jobs"></a>Recente taken weergeven

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** Vouw **taken** en op **afgelopen 24 uur**. Het deelvenster **resultaten** bevat back-taken voor de afgelopen 24 uur (u kunt maximaal 64 taken). De volgende gegevens weergegeven in het deelvenster **resultaten** , afhankelijk van **de weergaveopties die u opgeeft** :

    - **Naam** : de naam van de geplande momentopname.
 
    - **Slag** – de datum en tijd waarop de momentopname begonnen.

    - **Gestopt** – de datum en tijd waarop de momentopname voltooid of is beëindigd.

    - Er treedt een time-out op in **verstreken** – de tijdsduur tussen de **gestart** en **gestopt** .

    - **Status** – de status van de onlangs voltooide taak. **Succes** wordt aangegeven dat de back-up is gemaakt. **Mislukt** wordt aangegeven dat de taak niet is gestart.

    - **Informatie** : de reden voor de fout.

    - **Bytes verwerkt (MB)** – de hoeveelheid gegevens uit de volume-groep die is verwerkt (in MB). 

    ![Taken die u hebt uitgevoerd in de afgelopen 24 uur](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 

3. Als u wilt aanvullende acties uitvoeren op een bepaalde taak, met de rechtermuisknop op de naam van de taak in het deelvenster **resultaten** en selecteer in de opties in het menu.

    ![Een taak verwijderen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png) 
     
## <a name="view-currently-running-jobs"></a>Actieve taken weergeven

Gebruik de volgende procedure om taken weergeven die momenteel worden uitgevoerd.

#### <a name="to-view-currently-running-jobs"></a>Actieve taken weergeven

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** Vouw het knooppunt **taken** uit en klikt u op **uitvoeren**. Afhankelijk van **de weergaveopties die u opgeeft,** wordt de volgende gegevens weergegeven in het deelvenster **resultaten** : 

    - **Naam** : de naam van de geplande momentopname.

    - **Slag** – de datum en tijd waarop de momentopname begonnen.

    - **Controlepunt** – de huidige actie van de back-up.

    - **Status** : het percentage voltooid werk.
    
    - **Verstreken** – de hoeveelheid tijd die is verstreken sinds de back-up van start is gegaan. 

    - **Gemiddelde doorvoer (MB)** -/ breedteverhouding van totaal aantal bytes van gegevens die worden verwerkt die van de totale tijd die u hebt gemaakt voor het verwerken van (MB) aan.

    - **Bytes verwerkt (MB)** – het aantal bytes gegevens die worden verwerkt (in MB).

    - **Bytes geschreven (MB)** – aantal bytes geschreven (in MB). De gegevens, evenals de metagegevens bevat en is daarom meestal groter is dan de Bytes verwerkt.

    ![Taken die al bezig](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)

3. Als u wilt aanvullende acties uitvoeren op een bepaalde taak, met de rechtermuisknop op de naam van de taak in het deelvenster **resultaten** en selecteer in de opties in het menu.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [gebruik van StorSimple momentopname Manager kunt u uw oplossing StorSimple beheren](storsimple-snapshot-manager-admin.md).
- Informatie over het [gebruik van StorSimple momentopname Manager voor het beheren van de back-catalogus](storsimple-snapshot-manager-manage-backup-catalog.md).















            


 

