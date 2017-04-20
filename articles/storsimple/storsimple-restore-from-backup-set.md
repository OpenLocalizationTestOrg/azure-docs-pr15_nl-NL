<properties 
   pageTitle="Het volume van een StorSimple herstellen uit back-up | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u een volume StorSimple terugzetten uit een back-ups instellen met de cataloguspagina StorSimple Manager service back."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Het volume van een StorSimple terugzetten uit een back-ups instellen

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Overzicht

De **Catalogus met back-up** -pagina wordt weergegeven de back-sets die worden gemaakt wanneer handmatige of automatische back-ups zijn die u hebt gemaakt. U kunt deze pagina alle de back-ups weergeven voor een back-beleid of een volume, selecteren of verwijderen van back-ups gebruikt of een back-up gebruiken om te zetten of een volume klonen.

 ![Back-up maken cataloguspagina](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Deze zelfstudie wordt uitgelegd hoe u een volume op uw apparaat terugzetten uit een back-ups instellen met de **Back-up** cataloguspagina.

## <a name="how-to-use-the-backup-catalog"></a>Het gebruik van de back-catalogus 

De pagina **Back-catalogus** vindt u een query die u helpt bij het beperken van de back-up selectie instellen. U kunt de back-sets die zijn opgehaald op basis van de volgende parameters filteren:

- **Apparaat** – het apparaat waarop u de back-set is gemaakt.
- **Back-up-beleid** of het **volume** – de back-beleid of het volume dat is gekoppeld aan dit back-ups instellen.
- **Van** en **naar** – het bereik datum en tijd als de set back-up is gemaakt.

De gefilterde back-sets worden vervolgens de tabelindeling op basis van de volgende kenmerken:

- **Naam** : de naam van de back-beleid of het volume dat is gekoppeld aan de back-ups instellen.
- **Grootte** – de werkelijke grootte van de back-ups instellen.
- **Gemaakt op** : de datum en tijd waarop de back-ups zijn gemaakt. 
- **Type** , back-up sets kunnen worden lokale momentopnamen of momentopnamen cloud. Een lokale momentopname is een back-up van al uw volumegegevens lokaal opgeslagen op het apparaat, dat een momentopname cloud verwijst naar de back-up van het van volumegegevens in de cloud. Lokale momentopnamen bieden snellere toegang tot dat cloud momentopnamen voor tolerantie gegevens zijn geselecteerd.
- **Gestart door** – de back-ups automatisch kunnen worden gestart volgens een planning of handmatig door een gebruiker. (U kunt een back-beleid back-ups plannen. U kunt ook kunt u de optie **back-up maken** naar een interactieve back-up maken.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Hoe het volume van de StorSimple terugzet uit een back-up

U kunt de **Back-up** cataloguspagina herstellen van het volume van de StorSimple van een specifieke back-up. 

> [AZURE.WARNING] Terugzetten vanuit een back-up wordt vervangen door de bestaande hoeveelheden uit de back-up. Hierdoor kunnen de verlies van gegevens die is geschreven nadat de back-up is gemaakt.

Voordat u een herstellen op een volume starten, moet u ervoor zorgen dat het volume offline is. U moet het volume offline op de host eerste uitvoeren en klik vervolgens op het apparaat. Volg de stappen in [een volume offline nemen](storsimple-manage-volumes.md#take-a-volume-offline). Voer de volgende stappen uit als u wilt herstellen van een volume uit een back-set uit.

### <a name="to-restore-from-a-backup-set"></a>Terugzetten op basis van een back-ups instellen

1. Klik op het tabblad **back-up-catalogus** op de pagina van de service StorSimple Manager.

    ![Back-catalogus](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. Selecteer een back-up instellen als volgt:
  1. Selecteer het gewenste apparaat.
  2. Kies het volume of back-up-beleid voor de back-up die u wilt selecteren in de vervolgkeuzelijst.
  3. Geef het tijdsbereik.
  4. Klik op het pictogram controleren ![pictogram controleren](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) naar het uitvoeren van deze query.
 
    De back-ups die is gekoppeld aan het geselecteerde volume of back-beleid moet worden weergegeven in de lijst met back-sets.

3. Vouw de back-up instellen om weer te geven van de bijbehorende hoeveelheden. Deze volumes moeten worden verbroken op de host en apparaat voordat u ze kunt herstellen. Volg de stappen in [een volume offline nemen](storsimple-manage-volumes.md#take-a-volume-offline).

    >  [AZURE.IMPORTANT] Zorg ervoor dat u hebt gemaakt de hoeveelheden offline op de host eerst voordat u de hoeveelheden offline op het apparaat. Als u de hoeveelheden offline niet op de host uitvoeren volgt, kan dit mogelijk tegen beschadiging leiden.

4. Selecteer een back-ups instellen. Klik op **herstellen** onder aan de pagina.

6. U wordt gevraagd om bevestiging. 

    ![Bevestigingspagina](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. Controleer de gegevens herstellen en klik op het pictogram van het selectievakje ![Controleer pictogram](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Hiermee wordt een terugzettaak die u bekijken kunt via de pagina **taken** starten. 

8. Nadat het herstellen voltooid is, kunt u controleren of dat de inhoud van uw hoeveelheden worden vervangen door uit de back-up hoeveelheden.

![Video beschikbaar](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video beschikbaar**

Klik op een video die laat zien hoe u kunt de klonen gebruiken en herstellen van functies in StorSimple herstellen van verwijderde bestanden bekijken [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Volgende stappen

- Leer hoe u volume [StorSimple beheren](storsimple-manage-volumes.md).

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
