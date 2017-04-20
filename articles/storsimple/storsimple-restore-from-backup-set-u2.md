<properties 
   pageTitle="Het volume van een StorSimple herstellen uit back-up | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u een volume StorSimple terugzetten uit een back-ups instellen met de cataloguspagina StorSimple Manager service back."
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

# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Het volume van een StorSimple terugzetten uit een back-ups instellen (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Overzicht

De **Catalogus met back-up** -pagina wordt weergegeven de back-sets die worden gemaakt wanneer handmatige of automatische back-ups zijn die u hebt gemaakt. U kunt deze pagina alle de back-ups weergeven voor een back-beleid of een volume, selecteren of verwijderen van back-ups gebruikt of een back-up gebruiken om te zetten of een volume klonen.

 ![Back-up maken cataloguspagina](./media/storsimple-restore-from-backup-set-u2/restore.png)

Deze zelfstudie wordt uitgelegd hoe u de **Back-up** cataloguspagina herstellen van uw apparaat van een back-ups instellen.

U kunt een volume uit een lokale of cloud back-up herstellen. In beide gevallen kunt de bewerking voor terugzetten u het volume online direct terwijl de gegevens op de achtergrond wordt gedownload. 

Voordat u terugzet, start u rekening moet houden van de volgende handelingen uit:

- Start het terugzetten **dat moet u het volume offline nemen** – het volume offline nemen zowel de host als het apparaat voordat u op. Hoewel de bewerking herstellen om het volume online automatisch op het apparaat te brengen, moet u handmatig doen om het apparaat online op de host. U kunt het volume online overbrengen op de host zodra het volume online op het apparaat is. (U hoeft niet te wachten tot de bewerking herstellen is voltooid.) Voor procedures uit, gaat u naar [een volume offline nemen](storsimple-manage-volumes-u2.md#take-a-volume-offline).

- **Type volume na terugzetten** , verwijderde volumes worden hersteld op basis van het type in de momentopname; dat wil zeggen volumes die lokaal zijn vastgemaakt worden hersteld als lokaal vastgemaakte volumes en volumes die zijn doorverbonden als doorverbonden volumes worden hersteld.

    Voor een bestaand volume vervangt het volume van het huidige gebruik type het type dat is opgeslagen in de momentopname. Bijvoorbeeld als u een volume terugzet uit een momentopname die is uitgevoerd bij het volumetype is doorverbonden en dat volumetype wordt nu lokaal vastgemaakt (vanwege een conversiebewerking die is uitgevoerd), klikt u vervolgens het volume teruggezet als een lokaal vastgemaakte volume. Op dezelfde manier als een bestaand lokaal vastgemaakte volume is uitgevouwen en vervolgens worden teruggezet uit een oudere momentopname die u hebt gemaakt toen het volume nog kleinere, behoudt het volume van de herstelde de huidige uitgevouwen grootte.

    U kunt een volume niet converteren vanuit het volume van een doorverbonden naar een lokaal vastgemaakte volume of een lokaal vastgemaakte volume naar een doorverbonden volume, terwijl het volume wordt hersteld. Wacht totdat de bewerking herstellen is voltooid en klik vervolgens u het volume naar een ander type converteren kunt. Ga naar [het volumetype wijzigen](storsimple-manage-volumes-u2.md#change-the-volume-type)voor informatie over het converteren van een volume. 

- **Grootte van het volume worden doorgevoerd in het herstelde volume** – dit is een belangrijke overweging als u een lokaal vastgemaakte volume die is verwijderd (omdat lokaal vastgemaakte volumes volledig is ingericht) terugzet. Zorg ervoor dat er voldoende ruimte is voordat u probeert te herstellen van een lokaal vastgemaakte volume die eerder is verwijderd. 

- **Dat niet kunt u een volume terwijl deze wordt teruggezet uitvouwen** – wachten totdat de bewerking voor terugzetten voordat u probeert om uit te vouwen van het volume is voltooid. Voor informatie over het uitvouwen van een volume, gaat u naar [een volume aanpassen](storsimple-manage-volumes-u2.md#modify-a-volume).

- **U kunt een back-up terwijl u een lokaal volume herstelt uitvoeren** – voor procedures gaat u naar [gebruiken de StorSimple Manager-service voor het beheren van back-beleid](storsimple-manage-backup-policies.md).

- **Dat u kunt een bewerking annuleren** : als u de terugzettaak, wordt het volume opzegt wordt worden teruggezet naar de status waarin deze verkeerde voordat u de bewerking voor terugzetten gestart. Voor procedures uit, gaat u naar het [annuleren van een taak](storsimple-manage-jobs-u2.md#cancel-a-job).

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

U kunt de **Back-up** cataloguspagina herstellen van het volume van de StorSimple van een specifieke back-up. Houd er rekening mee echter, het volume naar de status toen de back-up is die u hebt gemaakt die u door het herstellen van een volume wordt hersteld. Alle gegevens die is toegevoegd na de back-up niet verloren.

> [AZURE.WARNING] Terugzetten vanuit een back-up wordt vervangen door de bestaande hoeveelheden uit de back-up. Hierdoor kunnen de verlies van gegevens die is geschreven nadat de back-up is gemaakt.

### <a name="to-restore-your-volume"></a>Het volume van de herstellen

1. Klik op het tabblad **back-up-catalogus** op de pagina van de service StorSimple Manager.

    ![Back-catalogus](./media/storsimple-restore-from-backup-set-u2/restore.png)

2. Selecteer een back-up instellen als volgt:
  1. Selecteer het gewenste apparaat.
  2. Kies het volume of back-up-beleid voor de back-up die u wilt selecteren in de vervolgkeuzelijst.
  3. Geef het tijdsbereik.
  4. Klik op het pictogram controleren ![pictogram controleren](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) naar het uitvoeren van deze query.
 
    De back-ups die is gekoppeld aan het geselecteerde volume of back-beleid moet worden weergegeven in de lijst met back-sets.

3. Vouw de back-up instellen om weer te geven van de bijbehorende hoeveelheden. Deze volumes moeten worden verbroken op de host en apparaat voordat u ze kunt herstellen. Toegang tot de hoeveelheden op de pagina **Volume Containers** en volg de stappen in [een volume offline nemen](storsimple-manage-volumes-u2.md#take-a-volume-offline) naar een ze offline te zetten.

    > [AZURE.IMPORTANT] Zorg ervoor dat u hebt gemaakt de hoeveelheden offline op de host eerst voordat u de hoeveelheden offline op het apparaat. Als u de hoeveelheden offline niet op de host uitvoeren volgt, kan dit mogelijk tegen beschadiging leiden.

4. Ga terug naar het tabblad **Back-catalogus** en selecteer een back-ups instellen.

5. Klik op **herstellen** onder aan de pagina.

6. U wordt gevraagd om bevestiging. Controleer de gegevens herstellen en selecteer vervolgens het selectievakje ter bevestiging.

    ![Bevestigingspagina](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)

7. Klik op het pictogram van het selectievakje ![Controleer pictogram](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Hiermee wordt een terugzettaak die u bekijken kunt via de pagina **taken** starten. 

8. Nadat het herstellen voltooid is, kunt u controleren of dat de inhoud van uw hoeveelheden worden vervangen door uit de back-up hoeveelheden.

![Video beschikbaar](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video beschikbaar**

Klik op een video die laat zien hoe u kunt de klonen gebruiken en herstellen van functies in StorSimple herstellen van verwijderde bestanden bekijken [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Als de terugzetten mislukt

U ontvangt een melding in als de bewerking voor terugzetten om welke reden dan ook is mislukt. Als dit gebeurt, vernieuwt u de back-lijst om te bevestigen dat de back-up nog geldig is. Als de back-up geldig is en u vanuit de cloud terugzetten wilt, klikt u vervolgens veroorzaakt verbindingsproblemen het probleem. 

U voltooit de bewerking voor terugzetten, maakt u het volume offline op de host en probeer het opnieuw herstellen. Houd er rekening mee dat eventuele wijzigingen aan de volumegegevens die zijn uitgevoerd tijdens het herstelproces verloren gaan.

## <a name="next-steps"></a>Volgende stappen

- Leer hoe u volume [StorSimple beheren](storsimple-manage-volumes-u2.md).

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
